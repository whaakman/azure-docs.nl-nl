---
title: Gebruik Resource Manager-sjablonen in Data Factory | Microsoft Docs
description: Informatie over het maken en gebruiken van Azure Resource Manager-sjablonen voor het maken van Data Factory-entiteiten.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.assetid: 37724021-f55f-4e85-9206-6d4a48bda3d8
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: ca8b3930b9d9f708d83dc760be3ee89737b074dc
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57541584"
---
# <a name="use-templates-to-create-azure-data-factory-entities"></a>Gebruik sjablonen om aan te maken van Azure Data Factory-entiteiten
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. 

## <a name="overview"></a>Overzicht
Tijdens het gebruik van Azure Data Factory voor uw Gegevensbehoeften integratie, u zult zien dat hetzelfde patroon voor verschillende omgevingen of dezelfde taak binnen dezelfde oplossing herhaaldelijk implementeren. Met sjablonen kunt u implementeren en beheren van deze scenario's op een eenvoudige manier. Sjablonen in Azure Data Factory zijn ideaal voor scenario's voor hergebruik en herhaling.

Houd rekening met de situatie waarbij een organisatie 10 fabrieken over de hele wereld heeft. De logboeken van elke plant worden opgeslagen in een afzonderlijke on-premises SQL Server-database. Het bedrijf wil een enkel datawarehouse in de cloud voor ad-hoc analytics opzetten. Het wil ook hebben de dezelfde logica maar verschillende configuraties voor ontwikkeling, testen en productie-omgevingen.

In dit geval moet een taak worden herhaald in dezelfde omgeving, maar met verschillende waarden voor de 10 data factory's voor elke fabriek. In feite **herhaling** aanwezig is. Templating kan de abstractie van deze algemene stroom (dat wil zeggen, pijplijnen die dezelfde activiteiten in elke data factory), maar maakt gebruik van een afzonderlijke parameterbestand voor elke fabriek.

Bovendien, als de organisatie wil deze 10 data factory's meerdere malen implementeren in verschillende omgevingen, sjablonen kunnen gebruiken dit **herbruikbaarheid** door het gebruik van afzonderlijke parameterbestanden voor ontwikkeling, testen, en productie-omgevingen.

## <a name="templating-with-azure-resource-manager"></a>Templating met Azure Resource Manager
[Azure Resource Manager-sjablonen](../../azure-resource-manager/resource-group-overview.md#template-deployment) zijn een uitstekende manier om te realiseren templating in Azure Data Factory. Resource Manager-sjablonen definieert de infrastructuur en configuratie van uw Azure-oplossing via een JSON-bestand. Omdat Azure Resource Manager-sjablonen met alle/de meeste Azure-services werkt, kunt het algemeen worden gebruikt voor alle resources van uw Azure-assets eenvoudig te beheren. Zie [Authoring Azure Resource Manager-sjablonen](../../azure-resource-manager/resource-group-authoring-templates.md) voor meer informatie over de Resource Manager-sjablonen in het algemeen.

## <a name="tutorials"></a>Zelfstudies
Zie de volgende zelfstudies voor stapsgewijze instructies voor het maken van Data Factory-entiteiten met behulp van Resource Manager-sjablonen:

* [Zelfstudie: Een pijplijn om gegevens te kopiëren met behulp van Azure Resource Manager-sjabloon maken](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [Zelfstudie: Een pijplijn om gegevens te verwerken met behulp van Azure Resource Manager-sjabloon maken](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a>Data Factory-sjablonen op GitHub
Raadpleeg de volgende Azure quick start-sjablonen op GitHub:

* [Een Data factory om gegevens te kopiëren van Azure Blob Storage naar Azure SQL Database maken](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
* [Een Data factory maken met Hive-activiteit op Azure HDInsight-cluster](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
* [Maak een Data factory om gegevens te kopiëren uit Salesforce aan Azure-Blobs](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)
* [Maak een gegevensfactory dat is gekoppeld activiteiten: kopieert gegevens van een FTP-server naar Azure-Blobs, roept een hive-script op een HDInsight-cluster op aanvraag om de gegevens te transformeren en het resultaat wordt gekopieerd naar Azure SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-data-factory-ftp-hive-blob)

U kunt voor het delen van uw Azure Data Factory-sjablonen op [Azure Quick start](https://azure.microsoft.com/documentation/templates/). Raadpleeg de [handleiding voor bijdragen](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) tijdens het ontwikkelen van sjablonen die kunnen worden gedeeld via deze opslagplaats.

De volgende secties vindt u informatie over het definiëren van Data Factory-resources in een Resource Manager-sjabloon.

## <a name="defining-data-factory-resources-in-templates"></a>Het definiëren van Data Factory-resources in sjablonen
De sjabloon op het hoogste niveau voor het definiëren van een data factory is:

```JSON
"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
"contentVersion": "1.0.0.0",
"parameters": { ...
},
"variables": { ...
},
"resources": [
{
    "name": "[parameters('dataFactoryName')]",
    "apiVersion": "[variables('apiVersion')]",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "westus",
    "resources": [
    { "type": "linkedservices",
        ...
    },
    {"type": "datasets",
        ...
    },
    {"type": "dataPipelines",
        ...
    }
}
```

### <a name="define-data-factory"></a>Een gegevensfactory definiëren
U definieert een gegevensfactory in de Resource Manager-sjabloon zoals in het volgende voorbeeld wordt weergegeven:

```JSON
"resources": [
{
    "name": "[variables('<mydataFactoryName>')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "East US"
}
```
De variabele dataFactoryName wordt gedefinieerd in 'variabelen' als:

```JSON
"dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",
```

### <a name="define-linked-services"></a>Gekoppelde services definiëren

```JSON
"type": "linkedservices",
"name": "[variables('<LinkedServiceName>')]",
"apiVersion": "2015-10-01",
"dependsOn": [ "[variables('<dataFactoryName>')]" ],
"properties": {
    ...
}
```

Zie [gekoppelde Storage-Service](data-factory-azure-blob-connector.md#azure-storage-linked-service) of [gekoppelde Services berekenen](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie over de JSON-eigenschappen voor de specifieke gekoppelde service die u wilt implementeren. De parameter 'dependsOn' geeft de naam van de bijbehorende data factory. Een voorbeeld van het definiëren van een gekoppelde service voor Azure Storage wordt weergegeven in de volgende JSON-definitie:

### <a name="define-datasets"></a>Gegevenssets definiëren

```JSON
"type": "datasets",
"name": "[variables('<myDatasetName>')]",
"dependsOn": [
    "[variables('<dataFactoryName>')]",
    "[variables('<myDatasetLinkedServiceName>')]"
],
"apiVersion": "2015-10-01",
"properties": {
    ...
}
```
Raadpleeg [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) voor meer informatie over de JSON-eigenschappen voor het gegevenssettype specifieke die u wilt implementeren. Houd er rekening mee dat met de parameter "dependsOn" Hiermee geeft u de naam van de bijbehorende data factory en de opslag gekoppelde service. Een voorbeeld van het definiëren van gegevenssettype van Azure blob-opslag wordt weergegeven in de volgende JSON-definitie:

```JSON
"type": "datasets",
"name": "[variables('storageDataset')]",
"dependsOn": [
    "[variables('dataFactoryName')]",
    "[variables('storageLinkedServiceName')]"
],
"apiVersion": "2015-10-01",
"properties": {
"type": "AzureBlob",
"linkedServiceName": "[variables('storageLinkedServiceName')]",
"typeProperties": {
    "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
    "fileName": "[parameters('sourceBlobName')]",
    "format": {
        "type": "TextFormat"
    }
},
"availability": {
    "frequency": "Hour",
    "interval": 1
}
```

### <a name="define-pipelines"></a>Pijplijnen definiëren

```JSON
"type": "dataPipelines",
"name": "[variables('<mypipelineName>')]",
"dependsOn": [
    "[variables('<dataFactoryName>')]",
    "[variables('<inputDatasetLinkedServiceName>')]",
    "[variables('<outputDatasetLinkedServiceName>')]",
    "[variables('<inputDataset>')]",
    "[variables('<outputDataset>')]"
],
"apiVersion": "2015-10-01",
"properties": {
    activities: {
        ...
    }
}
```

Raadpleeg [pijplijnen definiëren](data-factory-create-pipelines.md#pipeline-json) voor meer informatie over de JSON-eigenschappen voor het definiëren van de specifieke pijplijn en de activiteiten die u wilt implementeren. Houd er rekening mee met de parameter "dependsOn" Hiermee geeft u de naam van de data factory en de bijbehorende gekoppelde services of gegevenssets. Een voorbeeld van een pijplijn waarmee gegevens worden gekopieerd van Azure Blob Storage naar Azure SQL Database wordt weergegeven in de volgende JSON-fragment:

```JSON
"type": "datapipelines",
"name": "[variables('pipelineName')]",
"dependsOn": [
    "[variables('dataFactoryName')]",
    "[variables('azureStorageLinkedServiceName')]",
    "[variables('azureSqlLinkedServiceName')]",
    "[variables('blobInputDatasetName')]",
    "[variables('sqlOutputDatasetName')]"
],
"apiVersion": "2015-10-01",
"properties": {
    "activities": [
    {
        "name": "CopyFromAzureBlobToAzureSQL",
        "description": "Copy data frm Azure blob to Azure SQL",
        "type": "Copy",
        "inputs": [
            {
                "name": "[variables('blobInputDatasetName')]"
            }
        ],
        "outputs": [
            {
                "name": "[variables('sqlOutputDatasetName')]"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "BlobSource"
            },
            "sink": {
                "type": "SqlSink",
                "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
            },
            "translator": {
                "type": "TabularTranslator",
                "columnMappings": "Column0:FirstName,Column1:LastName"
            }
        },
        "Policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 3,
            "timeout": "01:00:00"
        }
    }
    ],
    "start": "2016-10-03T00:00:00Z",
    "end": "2016-10-04T00:00:00Z"
}
```
## <a name="parameterizing-data-factory-template"></a>Sjabloon voor Data Factory parametriseren
Zie voor aanbevolen procedures op parametriseren [aanbevolen procedures voor het maken van Azure Resource Manager-sjablonen](../../azure-resource-manager/resource-manager-template-best-practices.md). In het algemeen moet gebruik van parameters worden geminimaliseerd, met name als variabelen in plaats daarvan kunnen worden gebruikt. Geef alleen parameters op in de volgende scenario's:

* Instellingen variëren per omgeving (voorbeeld: ontwikkeling, testen en productie)
* Geheimen (zoals wachtwoorden)

Als u nodig hebt voor het ophalen van geheimen van [Azure Key Vault](../../key-vault/key-vault-overview.md) bij het implementeren van Azure Data Factory-entiteiten met behulp van sjablonen, geef de **sleutelkluis** en **geheime naam** zoals wordt weergegeven in de bijvoorbeeld:

```JSON
"parameters": {
    "storageAccountKey": {
        "reference": {
            "keyVault": {
                "id":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.KeyVault/vaults/<keyVaultName>",
             },
            "secretName": "<secretName>"
           },
       },
       ...
}
```

> [!NOTE]
> Hoewel het exporteren van sjablonen voor bestaande data factory's wordt momenteel nog niet ondersteund, is het in de werkt.
>
>

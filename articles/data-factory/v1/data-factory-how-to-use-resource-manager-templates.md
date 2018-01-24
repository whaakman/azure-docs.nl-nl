---
title: Gebruik Resource Manager-sjablonen in de Data Factory | Microsoft Docs
description: Informatie over het maken en gebruiken van Azure Resource Manager-sjablonen Data Factory-entiteiten maken.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: 
ms.assetid: 37724021-f55f-4e85-9206-6d4a48bda3d8
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: e6dbac2f88df3b7c2e4d7f982fc9a573e3d6e62f
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="use-templates-to-create-azure-data-factory-entities"></a>Sjablonen gebruiken voor het maken van Azure Data Factory-entiteiten
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. 

## <a name="overview"></a>Overzicht
Tijdens het gebruik van Azure Data Factory voor uw behoeften van de integratie gegevens u zult zien hergebruiken van hetzelfde patroon tussen verschillende omgevingen of dezelfde taak binnen dezelfde oplossing herhaaldelijk implementeren. Sjablonen te implementeren en beheren van deze scenario's in een eenvoudige manier. Sjablonen in Azure Data Factory zijn ideaal voor scenario's voor hergebruik en herhaling.

Houd rekening met de situatie waarin een organisatie 10 fabrieken over de hele wereld heeft. De logboeken van elke plant worden opgeslagen in een afzonderlijke on-premises SQL Server-database. Het bedrijf wil voor het bouwen van een enkele datawarehouse in de cloud voor ad-hoc analyses. Dit wil ook de dezelfde logica maar verschillende configuraties voor ontwikkeling, testen en productie-omgevingen.

In dit geval wordt moet een taak worden herhaald in dezelfde omgeving maar met verschillende waarden voor de 10 gegevensfactory's voor elke fabriek. In feite **herhaling** aanwezig is. Templating kunnen de abstrahering van deze algemene stroom (dat wil zeggen, pijplijnen met dezelfde activiteiten in elke gegevensfactory), maar een afzonderlijke parameter-bestand wordt gebruikt voor elke fabriek.

Bovendien, als de organisatie wil deze 10 gegevensfactory meerdere keren implementeren in verschillende omgevingen, sjablonen kunnen deze gebruiken **hergebruik** door het gebruik van afzonderlijke parameterbestanden voor ontwikkeling, testen en productie-omgevingen.

## <a name="templating-with-azure-resource-manager"></a>Templating met Azure Resource Manager
[Azure Resource Manager-sjablonen](../../azure-resource-manager/resource-group-overview.md#template-deployment) vormen een uitstekende manier om te bereiken templating in Azure Data Factory. Resource Manager-sjablonen worden de infrastructuur en configuratie van uw Azure-oplossing via een JSON-bestand definieert. Omdat Azure Resource Manager-sjablonen met alle/meeste Azure-services werkt, kan het veel voor het beheren van eenvoudig alle resources van uw Azure activa gebruikt. Zie [Azure Resource Manager-sjablonen samenstellen](../../azure-resource-manager/resource-group-authoring-templates.md) voor meer informatie over de Resource Manager-sjablonen in het algemeen.

## <a name="tutorials"></a>Zelfstudies
Zie de volgende zelfstudies voor stapsgewijze instructies voor het Data Factory-entiteiten maken met behulp van Resource Manager-sjablonen:

* [Zelfstudie: Een pijplijn om gegevens te kopiëren met behulp van Azure Resource Manager-sjabloon maken](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [Zelfstudie: Een pijplijn om gegevens te verwerken met behulp van Azure Resource Manager-sjabloon maken](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a>Data Factory-sjablonen op GitHub
Bekijk de volgende Azure snel starten-sjablonen op GitHub:

* [Maak een gegevensfactory om gegevens te kopiëren uit Azure Blob Storage met Azure SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
* [Maak een gegevensfactory met Hive-activiteit op Azure HDInsight-cluster](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
* [Maak een gegevensfactory om gegevens te kopiëren van Salesforce op Azure Blobs](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)
* [Maak een gegevensfactory dat gekoppeld is activiteiten: kopieert gegevens van een FTP-server naar Azure Blobs, roept u een hive-script op een HDInsight-cluster op aanvraag om de gegevens te transformeren en resultaat kopieert naar Azure SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-data-factory-ftp-hive-blob)

Voor het delen van uw Azure Data Factory-sjablonen op gerust [Azure snel starten](https://azure.microsoft.com/documentation/templates/). Raadpleeg de [bijdrage handleiding](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) tijdens de ontwikkeling van sjablonen die kunnen worden gedeeld via deze opslagplaats.

De volgende secties bevatten meer informatie over het definiëren van Data Factory-resources in een Resource Manager-sjabloon.

## <a name="defining-data-factory-resources-in-templates"></a>Het definiëren van Data Factory-resources in sjablonen
De sjabloon op het hoogste niveau voor het definiëren van een gegevensfactory is:

```JSON
"$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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
De dataFactoryName is in 'variabelen' gedefinieerd als:

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

Zie [Storage Linked Service](data-factory-azure-blob-connector.md#azure-storage-linked-service) of [gekoppelde Services berekenen](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie over de JSON-eigenschappen voor de specifieke gekoppelde service die u wilt implementeren. De parameter 'dependsOn' geeft de naam van de bijbehorende gegevensfactory. Een voorbeeld van het definiëren van een gekoppelde service voor Azure Storage wordt weergegeven in de volgende JSON-definitie:

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
Raadpleeg [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) voor meer informatie over de JSON-eigenschappen voor het type specifieke gegevensset die u wilt implementeren. Opmerking dat de parameter 'dependsOn' geeft de naam van de bijbehorende gegevensfactory en de opslag gekoppelde service. Een voorbeeld van het type van de dataset van Azure blob storage definiëren wordt weergegeven in de volgende JSON-definitie:

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

Raadpleeg [pijplijnen definiëren](data-factory-create-pipelines.md#pipeline-json) voor meer informatie over de JSON-eigenschappen voor het definiëren van de specifieke pijplijn en activiteiten die u wilt implementeren. Noteer de parameter 'dependsOn' geeft de naam van de gegevensfactory en eventuele bijbehorende gekoppelde services of gegevenssets. Een voorbeeld van een pijplijn waarmee gegevens worden gekopieerd uit Azure Blob Storage met Azure SQL Database wordt weergegeven in de volgende JSON-fragment:

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
## <a name="parameterizing-data-factory-template"></a>Data Factory-sjabloon parameters voorzien
Zie voor aanbevolen procedures op parameters voorzien [aanbevolen procedures voor het maken van Azure Resource Manager-sjablonen](../../azure-resource-manager/resource-manager-template-best-practices.md). In het algemeen moet gebruik van parameters worden geminimaliseerd, vooral als variabelen in plaats daarvan kunnen worden gebruikt. Geef alleen parameters in de volgende scenario's:

* Instellingen variëren door omgeving (voorbeeld: ontwikkeling, testen en productie)
* Geheimen (zoals wachtwoorden)

Als u nodig hebt voor het ophalen van geheimen van [Azure Key Vault](../../key-vault/key-vault-get-started.md) bij het implementeren van Azure Data Factory-entiteiten met behulp van sjablonen, geef de **sleutelkluis** en **geheime naam** zoals weergegeven in het volgende voorbeeld:

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
> Tijdens het exporteren van sjablonen voor bestaande data Factory wordt momenteel nog niet ondersteund, is het in de werkt.
>
>

---
title: Versleutelen van de referenties in Azure Data Factory | Microsoft Docs
description: Informatie over het versleutelen en referenties voor uw on-premises gegevensopslagexemplaren opslaan op een machine met host zichzelf integratie runtime.
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: abnarain
ms.openlocfilehash: 0f42d971fcf21d0f719468a8c10ff637f2e523f9
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="encrypt-credentials-for-on-premises-data-stores-in-azure-data-factory"></a>Versleutelen van referenties voor on-premises gegevensopslagexemplaren in Azure Data Factory
U kunt versleutelen en opslaan van referenties voor uw on-premises gegevensopslagexemplaren (gekoppelde services met gevoelige informatie) op een computer met de host zichzelf integratie runtime. 

U geeft een JSON-definitie-bestand met de referenties voor de <br/>[**Nieuwe AzureRmDataFactoryV2LinkedServiceEncryptedCredential** ](https://docs.microsoft.com/powershell/module/azurerm.datafactoryv2/New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential?view=azurermps-4.4.0) cmdlet voor het produceren van een JSON-definitiebestand voor uitvoer met de versleutelde referenties. Vervolgens gebruikt u de bijgewerkte JSON-definitie voor de gekoppelde services maken.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt die algemeen beschikbaar is (GA), raadpleegt u [Documentatie van versie 1 van Data Factory](v1/data-factory-introduction.md).

## <a name="author-sql-server-linked-service"></a>De auteur van SQL Server gekoppelde service
Maak een JSON-bestand met de naam **SqlServerLinkedService.json** in een andere map met de volgende inhoud:  

Vervang `<servername>`, `<databasename>`, `<username>`, en `<password>` met waarden voor uw SQL-Server voordat u het bestand opslaat. En vervang `<integration runtime name>` met de naam van uw integratie-runtime. 

```json
{
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=<servername>;Database=<databasename>;User ID=<username>;Password=<password>;Timeout=60"
            }
        },
        "connectVia": {
            "type": "integrationRuntimeReference",
            "referenceName": "<integration runtime name>"
        },
        "name": "SqlServerLinkedService"
    }
}
```

## <a name="encrypt-credentials"></a>Referenties versleutelen
Voor het versleutelen van gevoelige gegevens uit de JSON-nettolading op een lokale host zichzelf integratie runtime uitvoeren **nieuw AzureRmDataFactoryV2LinkedServiceEncryptedCredential**, en geef op de JSON-nettolading. Deze cmdlet zorgt ervoor dat de referenties zijn versleuteld met behulp van DPAPI en op de host zichzelf integratie runtime-knooppunt lokaal opgeslagen. De nettolading van de uitvoer kan worden omgeleid naar een andere JSON-bestand (in dit geval 'encryptedLinkedService.json'), waarin versleutelde referenties.

```powershell
New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "SqlServerLinkedService" -DefinitionFile ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
```

## <a name="use-the-json-with-encrypted-credentials"></a>De JSON met versleutelde referenties gebruiken
De JSON-bestand voor uitvoer van de vorige opdracht met de versleutelde referentie nu gebruiken voor het instellen van de **SqlServerLinkedService**.

```powershell
Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -DefinitionFile ".\encryptedSqlServerLinkedService.json" 
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de beveiligingsoverwegingen voor gegevensverplaatsing [Data movement beveiligingsoverwegingen](data-movement-security-considerations.md).


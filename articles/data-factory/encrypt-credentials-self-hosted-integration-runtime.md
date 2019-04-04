---
title: Versleutelen van de referenties in Azure Data Factory | Microsoft Docs
description: Informatie over het versleutelen en referenties voor uw on-premises gegevensopslagexemplaren opslaan op een computer met de zelf-hostende integratieruntime.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: abnarain
ms.openlocfilehash: 8e705a4430f6ccee847dc7d41ef80456a6dc4ea5
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58903791"
---
# <a name="encrypt-credentials-for-on-premises-data-stores-in-azure-data-factory"></a>Referenties voor on-premises gegevensarchieven in Azure Data Factory coderen
U kunt versleutelen en opslaan van referenties voor uw on-premises gegevensopslagexemplaren (gekoppelde services met gevoelige informatie) op een computer met de zelf-hostende integratieruntime. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Doorgeven van een JSON-definitie-bestand met de referenties voor de <br/>[**Nieuwe AzDataFactoryV2LinkedServiceEncryptedCredential** ](/powershell/module/az.datafactory/New-AzDataFactoryV2LinkedServiceEncryptedCredential) cmdlet voor het produceren van een uitvoer JSON-definitie-bestand met de versleutelde referenties. Vervolgens gebruikt u de bijgewerkte JSON-definitie de gekoppelde services te maken.

## <a name="author-sql-server-linked-service"></a>De auteur van gekoppelde SQL Server-service
Maak een JSON-bestand met de naam **SqlServerLinkedService.json** in een map met de volgende inhoud:  

Vervang `<servername>`, `<databasename>`, `<username>`, en `<password>` met waarden voor uw SQL-Server voordat u het bestand opslaat. En vervang `<integration runtime name>` met de naam van uw integratieruntime. 

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
Voer voor het versleutelen van de gevoelige gegevens uit de JSON-nettolading in een on-premises zelf-hostende integratieruntime **New-AzDataFactoryV2LinkedServiceEncryptedCredential**, en de JSON-nettolading doorgeven. Deze cmdlet zorgt ervoor dat de referenties zijn versleuteld met behulp van DPAPI en op het zelf-hostende integration runtime-knooppunt lokaal opgeslagen. De nettolading van de uitvoer met de versleutelde verwijzing naar de referentie op die kan worden omgeleid naar een ander JSON-bestand (in dit geval 'encryptedLinkedService.json').

```powershell
New-AzDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "SqlServerLinkedService" -DefinitionFile ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
```

## <a name="use-the-json-with-encrypted-credentials"></a>De JSON met versleutelde referenties gebruiken
Gebruik nu het JSON-bestand voor uitvoer van de vorige opdracht met de versleutelde referentie voor het instellen van de **SqlServerLinkedService**.

```powershell
Set-AzDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -DefinitionFile ".\encryptedSqlServerLinkedService.json" 
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de beveiligingsoverwegingen voor het verplaatsen van gegevens [beveiligingsoverwegingen voor Data movement](data-movement-security-considerations.md).


---
title: Azure Data Factory-service-identiteit | Microsoft Docs
description: Meer informatie over data factory-service-identiteit in Azure Data Factory.
services: data-factory
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: jingwang
ms.openlocfilehash: b0cb72b8a9c1710b2b22d987c2818a490a2ea553
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55753356"
---
# <a name="azure-data-factory-service-identity"></a>Azure Data Factory-service-identiteit

In dit artikel helpt u begrijpen wat een data factory-service-identiteit is en hoe het werkt.

## <a name="overview"></a>Overzicht

Bij het maken van een data factory, kan een service-identiteit worden gemaakt, samen met factory's worden gemaakt. De service-identiteit is een beheerde toepassing geregistreerd bij Azure Active Directory en vertegenwoordigt deze specifieke data factory.

Data factory-service-identiteit voordelen biedt voor de volgende functies:

- [Referentie in Azure Key Vault Store](store-credentials-in-key-vault.md), in welk geval data factory-service-identiteit wordt gebruikt voor verificatie van Azure Key Vault.
- Connectors zoals [Azure Blob-opslag](connector-azure-blob-storage.md), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md), [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md), [Azure SQL Database](connector-azure-sql-database.md), en [Azure SQL datawarehouse](connector-azure-sql-data-warehouse.md).
- [Activiteit web](control-flow-web-activity.md).

## <a name="generate-service-identity"></a>Service-identiteit genereren

Data factory-service-identiteit wordt als volgt gegenereerd:

- Bij het maken van data factory via **Azure portal of PowerShell**, service-identiteit altijd automatisch worden gemaakt.
- Bij het maken van data factory via **SDK**, alleen als u service-identiteit wordt gemaakt ' id = nieuwe FactoryIdentity() ' in het factoryobject voor het maken van. Zie het voorbeeld in [.NET-snelstart: een data factory maken](quickstart-create-data-factory-dot-net.md#create-a-data-factory).
- Bij het maken van data factory via **REST-API**, service-identiteit wordt alleen als u de sectie 'id' in de hoofdtekst van de aanvraag opgeven worden gemaakt. Zie het voorbeeld in [REST-quickstart - data factory maken](quickstart-create-data-factory-rest-api.md#create-a-data-factory).

Als u uw data factory beschikt niet over een service-identiteit die is gekoppeld na vinden [ophalen van de service-identiteit](#retrieve-service-identity) vorm van instructie, kunt u expliciet genereren door de data factory programmatisch met de identiteit initiator bijwerken:

- [Genereren van service-identiteit met behulp van PowerShell](#generate-service-identity-using-powershell)
- [Genereren met behulp van REST-API voor service-identiteit](#generate-service-identity-using-rest-api)
- [Genereren van service-identiteit met een Azure Resource Manager-sjabloon](#generate-service-identity-using-azure-resource-manager-template)
- [Service-identiteit met behulp van SDK genereren](#generate-service-identity-using-sdk)

>[!NOTE]
>- Service-identiteit kan niet worden gewijzigd. Bijwerken van een data factory waarvoor al een service-identiteit geen dit van invloed is, de service-identiteit blijft ongewijzigd.
>- Als u een data factory gebruikt die al een service-identiteit zonder de parameter 'id' in het factoryobject op te geven of zonder op te geven de sectie 'id' in de hoofdtekst van de REST-aanvraag hebt bijgewerkt, wordt er een fout.
>- Wanneer u een data factory verwijdert, worden de bijbehorende service-identiteit langs verwijderd.

### <a name="generate-service-identity-using-powershell"></a>Genereren van service-identiteit met behulp van PowerShell

Bel **Set-AzureRmDataFactoryV2** opdracht nogmaals, ziet u 'Id' velden pas wordt gegenereerd:

```powershell
PS C:\WINDOWS\system32> Set-AzureRmDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName> -Location <region>

DataFactoryName   : ADFV2DemoFactory
DataFactoryId     : /subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/ADFV2DemoFactory
ResourceGroupName : <resourceGroupName>
Location          : East US
Tags              : {}
Identity          : Microsoft.Azure.Management.DataFactory.Models.FactoryIdentity
ProvisioningState : Succeeded
```

### <a name="generate-service-identity-using-rest-api"></a>Genereren met behulp van REST-API voor service-identiteit

Onder de API aanroepen met de sectie 'id' in de aanvraagtekst:

```
PATCH https://management.azure.com/subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/<data factory name>?api-version=2017-09-01-preview
```

**Aanvraagtekst**: "identiteit" toevoegen: {"type": "SystemAssigned"}.

```json
{
    "name": "<dataFactoryName>",
    "location": "<region>",
    "properties": {},
    "identity": {
        "type": "SystemAssigned"
    }
}
```

**Antwoord**: service-identiteit wordt automatisch gemaakt en de sectie 'id' dienovereenkomstig wordt gevuld.

```json
{
    "name": "<dataFactoryName>",
    "tags": {},
    "properties": {
        "provisioningState": "Succeeded",
        "loggingStorageAccountKey": "**********",
        "createTime": "2017-09-26T04:10:01.1135678Z",
        "version": "2017-09-01-preview"
    },
    "identity": {
        "type": "SystemAssigned",
        "principalId": "765ad4ab-XXXX-XXXX-XXXX-51ed985819dc",
        "tenantId": "72f988bf-XXXX-XXXX-XXXX-2d7cd011db47"
    },
    "id": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/ADFV2DemoFactory",
    "type": "Microsoft.DataFactory/factories",
    "location": "<region>"
}
```

### <a name="generate-service-identity-using-an-azure-resource-manager-template"></a>Genereren van service-identiteit met een Azure Resource Manager-sjabloon

**Sjabloon**: "identiteit" toevoegen: {"type": "SystemAssigned"}.

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "resources": [{
        "name": "<dataFactoryName>",
        "apiVersion": "2018-06-01",
        "type": "Microsoft.DataFactory/factories",
        "location": "<region>",
        "identity": {
            "type": "SystemAssigned"
        }
    }]
}
```

### <a name="generate-service-identity-using-sdk"></a>Service-identiteit met behulp van SDK genereren

Roep de functie data factory create_or_update met id = nieuwe FactoryIdentity(). Met behulp van .NET-voorbeeldcode:

```csharp
Factory dataFactory = new Factory
{
    Location = <region>,
    Identity = new FactoryIdentity()
};
client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, dataFactory);
```

## <a name="retrieve-service-identity"></a>Service-identiteit ophalen

U kunt de service-identiteit in Azure portal of via een programma ophalen. De volgende secties ziet enkele voorbeelden.

>[!TIP]
> Als u de service-identiteit niet ziet [genereren van service-identiteit](#generate-service-identity) door het bijwerken van uw gegevensfactory.

### <a name="retrieve-service-identity-using-azure-portal"></a>Service-identiteit met Azure portal ophalen

U vindt de identiteitsgegevens van de service van Azure portal -> uw data factory -> Instellingen -> eigenschappen:

- ID VAN DE SERVICE-IDENTITEIT
- TENANT VAN SERVICE-IDENTITEIT
- **TOEPASSINGS-ID van SERVICE-identiteit** > deze waarde kopiëren

![Service-identiteit ophalen](media/data-factory-service-identity/retrieve-service-identity-portal.png)

### <a name="retrieve-service-identity-using-powershell"></a>Service-identiteit met behulp van PowerShell ophalen

De service-identiteit principal-ID en tenant-ID wordt geretourneerd wanneer u een specifieke data factory als volgt:

```powershell
PS C:\WINDOWS\system32> (Get-AzureRmDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName>).Identity

PrincipalId                          TenantId
-----------                          --------
765ad4ab-XXXX-XXXX-XXXX-51ed985819dc 72f988bf-XXXX-XXXX-XXXX-2d7cd011db47
```

De principal-ID kopiëren en vervolgens uitvoeren onder Azure Active Directory-opdracht met de principal-ID als parameter aan de **ApplicationId**, die u gebruiken om toegang te verlenen:

```powershell
PS C:\WINDOWS\system32> Get-AzureRmADServicePrincipal -ObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc

ServicePrincipalNames : {76f668b3-XXXX-XXXX-XXXX-1b3348c75e02, https://identity.azure.net/P86P8g6nt1QxfPJx22om8MOooMf/Ag0Qf/nnREppHkU=}
ApplicationId         : 76f668b3-XXXX-XXXX-XXXX-1b3348c75e02
DisplayName           : ADFV2DemoFactory
Id                    : 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
Type                  : ServicePrincipal
```

## <a name="next-steps"></a>Volgende stappen
Zie de volgende onderwerpen die introduceren wanneer en hoe u data factory-service-identiteit gebruiken:

- [Store-referentie in Azure Key Vault](store-credentials-in-key-vault.md)
- [Gegevens kopiëren van/naar Azure Data Lake Store met behulp van beheerde identiteiten voor verificatie van de Azure-resources](connector-azure-data-lake-store.md)

Zie [beheerde identiteiten voor een overzicht van Azure-Resources](/azure/active-directory/managed-identities-azure-resources/overview) voor meer achtergrondinformatie over beheerde identiteiten voor Azure-resources die data factory-service-identiteit is gebaseerd op. 

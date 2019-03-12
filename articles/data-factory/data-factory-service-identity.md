---
title: Beheerde identiteit voor Data Factory | Microsoft Docs
description: Meer informatie over beheerde identiteit voor Azure Data Factory.
services: data-factory
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: jingwang
ms.openlocfilehash: 3663526dc32b0a607c9fca3d7c76496bfb5566f4
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57549138"
---
# <a name="managed-identity-for-data-factory"></a>Beheerde identiteit voor Data Factory

Dit artikel helpt u inzicht in wat er beheerde identiteit voor de Data Factory (voorheen bekend als Managed Service Identity/MSI) en hoe het werkt.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Overzicht

Bij het maken van een data factory, kan een beheerde identiteit worden gemaakt, samen met factory's worden gemaakt. De beheerde identiteit is een beheerde toepassing geregistreerd bij Azure Active Directory en vertegenwoordigt deze specifieke data factory.

Beheerde identiteit voor Data Factory voordelen biedt voor de volgende functies:

- [Referentie in Azure Key Vault Store](store-credentials-in-key-vault.md), in welk geval data factory beheerde identiteit wordt gebruikt voor verificatie van Azure Key Vault.
- Connectors zoals [Azure Blob-opslag](connector-azure-blob-storage.md), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md), [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md), [Azure SQL Database](connector-azure-sql-database.md), en [Azure SQL datawarehouse](connector-azure-sql-data-warehouse.md).
- [Activiteit web](control-flow-web-activity.md).

## <a name="generate-managed-identity"></a>Genereren van beheerde identiteit

Beheerde identiteit voor Data Factory wordt als volgt gegenereerd:

- Bij het maken van data factory via **Azure portal of PowerShell**, beheerde identiteit wordt altijd automatisch worden gemaakt.
- Bij het maken van data factory via **SDK**, beheerde identiteit wordt gemaakt alleen als u ' identiteit nieuwe FactoryIdentity() = "in het factoryobject voor het maken van. Zie het voorbeeld in [.NET-snelstart: een data factory maken](quickstart-create-data-factory-dot-net.md#create-a-data-factory).
- Bij het maken van data factory via **REST-API**, beheerde identiteit wordt alleen als u de sectie 'id' in de hoofdtekst van de aanvraag opgeven worden gemaakt. Zie het voorbeeld in [REST-quickstart - data factory maken](quickstart-create-data-factory-rest-api.md#create-a-data-factory).

Als u uw data factory beschikt niet over een beheerde identiteit die is gekoppeld na vinden [ophalen van beheerde identiteit](#retrieve-managed-identity) vorm van instructie, kunt u expliciet genereren door de data factory programmatisch met de identiteit initiator bijwerken:

- [Genereren van beheerde identiteit met behulp van PowerShell](#generate-managed-identity-using-powershell)
- [Genereren van beheerde identiteit met behulp van REST-API](#generate-managed-identity-using-rest-api)
- Genereren van beheerde identiteit met een Azure Resource Manager-sjabloon
- [Beheerde identiteit met behulp van SDK genereren](#generate-managed-identity-using-sdk)

>[!NOTE]
>- Beheerde identiteit kan niet worden gewijzigd. Bijwerken van een data factory gebruikt die al een beheerde identiteit hebben geen dit van invloed is, de beheerde identiteit blijft ongewijzigd.
>- Als u een data factory gebruikt die al een beheerde identiteit zonder de parameter 'id' in het factoryobject op te geven of zonder op te geven de sectie 'id' in de hoofdtekst van de REST-aanvraag hebt bijgewerkt, wordt er een fout.
>- Wanneer u een data factory verwijdert, worden de bijbehorende beheerde identiteit langs verwijderd.

### <a name="generate-managed-identity-using-powershell"></a>Genereren van beheerde identiteit met behulp van PowerShell

Bel **Set AzDataFactoryV2** opdracht nogmaals, ziet u 'Id' velden pas wordt gegenereerd:

```powershell
PS C:\WINDOWS\system32> Set-AzDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName> -Location <region>

DataFactoryName   : ADFV2DemoFactory
DataFactoryId     : /subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/ADFV2DemoFactory
ResourceGroupName : <resourceGroupName>
Location          : East US
Tags              : {}
Identity          : Microsoft.Azure.Management.DataFactory.Models.FactoryIdentity
ProvisioningState : Succeeded
```

### <a name="generate-managed-identity-using-rest-api"></a>Genereren van beheerde identiteit met behulp van REST-API

Onder de API aanroepen met de sectie 'id' in de aanvraagtekst:

```
PATCH https://management.azure.com/subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/<data factory name>?api-version=2018-06-01
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

**Antwoord**: beheerde identiteit wordt automatisch gemaakt en de sectie 'id' dienovereenkomstig wordt gevuld.

```json
{
    "name": "<dataFactoryName>",
    "tags": {},
    "properties": {
        "provisioningState": "Succeeded",
        "loggingStorageAccountKey": "**********",
        "createTime": "2017-09-26T04:10:01.1135678Z",
        "version": "2018-06-01"
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

### <a name="generate-managed-identity-using-an-azure-resource-manager-template"></a>Genereren van beheerde identiteit met een Azure Resource Manager-sjabloon

**Sjabloon**: "identiteit" toevoegen: {"type": "SystemAssigned"}.

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

### <a name="generate-managed-identity-using-sdk"></a>Beheerde identiteit met behulp van SDK genereren

Roep de functie data factory create_or_update met id = nieuwe FactoryIdentity(). Met behulp van .NET-voorbeeldcode:

```csharp
Factory dataFactory = new Factory
{
    Location = <region>,
    Identity = new FactoryIdentity()
};
client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, dataFactory);
```

## <a name="retrieve-managed-identity"></a>Ophalen van beheerde identiteit

U kunt de beheerde identiteit in Azure portal of via een programma ophalen. De volgende secties ziet enkele voorbeelden.

>[!TIP]
> Als u de beheerde identiteit niet ziet [genereren beheerde identiteit](#generate-managed-identity) door het bijwerken van uw gegevensfactory.

### <a name="retrieve-managed-identity-using-azure-portal"></a>Beheerde identiteit met Azure portal ophalen

U vindt de beheerde identiteitsgegevens uit Azure portal -> uw data factory -> Instellingen -> eigenschappen:

- ID VAN DE SERVICE-IDENTITEIT
- TENANT VAN SERVICE-IDENTITEIT
- **TOEPASSINGS-ID van SERVICE-identiteit** > deze waarde kopiëren

![Ophalen van beheerde identiteit](media/data-factory-service-identity/retrieve-service-identity-portal.png)

### <a name="retrieve-managed-identity-using-powershell"></a>Ophalen van beheerde identiteit met behulp van PowerShell

De beheerde identiteit principal-ID en tenant-ID wordt geretourneerd wanneer u een specifieke data factory als volgt:

```powershell
PS C:\WINDOWS\system32> (Get-AzDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName>).Identity

PrincipalId                          TenantId
-----------                          --------
765ad4ab-XXXX-XXXX-XXXX-51ed985819dc 72f988bf-XXXX-XXXX-XXXX-2d7cd011db47
```

De principal-ID kopiëren en vervolgens uitvoeren onder Azure Active Directory-opdracht met de principal-ID als parameter aan de **ApplicationId**, die u gebruiken om toegang te verlenen:

```powershell
PS C:\WINDOWS\system32> Get-AzADServicePrincipal -ObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc

ServicePrincipalNames : {76f668b3-XXXX-XXXX-XXXX-1b3348c75e02, https://identity.azure.net/P86P8g6nt1QxfPJx22om8MOooMf/Ag0Qf/nnREppHkU=}
ApplicationId         : 76f668b3-XXXX-XXXX-XXXX-1b3348c75e02
DisplayName           : ADFV2DemoFactory
Id                    : 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
Type                  : ServicePrincipal
```

## <a name="next-steps"></a>Volgende stappen
Zie de volgende onderwerpen die wanneer introduceren en hoe u data factory gebruiken beheerde identiteit:

- [Store-referentie in Azure Key Vault](store-credentials-in-key-vault.md)
- [Gegevens kopiëren van/naar Azure Data Lake Store met behulp van beheerde identiteiten voor verificatie van de Azure-resources](connector-azure-data-lake-store.md)

Zie [beheerde identiteiten voor een overzicht van Azure-Resources](/azure/active-directory/managed-identities-azure-resources/overview) voor meer achtergrondinformatie over beheerde identiteiten voor een Azure-resources, welke gegevensfactory identiteit beheerde is gebaseerd op. 

---
title: Azure Data Factory-service-identiteit | Microsoft Docs
description: Meer informatie over data factory-service-identiteit in Azure Data Factory.
services: data-factory
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: jingwang
ms.openlocfilehash: aad93abd6e7bdf75e6f3b4fcd02b433a1d301ebc
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="azure-data-factory-service-identity"></a>Azure Data Factory-service-identiteit

In dit artikel helpt u begrijpen wat data factory-service-identiteit is en hoe het werkt.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [documentatie voor Data Factory version1 gedefinieerd](v1/data-factory-introduction.md).

## <a name="overview"></a>Overzicht

Bij het maken van een gegevensfactory kan een service-identiteit samen met het maken van de fabriek worden gemaakt. De service-identiteit is geregistreerd bij Azure activiteit Directory beheerde toepassingen en deze specifieke gegevensfactory vertegenwoordigt.

Data factory-service-identiteit voordelen biedt voor de volgende twee functies:

- [Opslaan van referenties in Azure Key Vault](store-credentials-in-key-vault.md), in welk geval data factory-service-identiteit voor Azure Sleutelkluis-verificatie wordt gebruikt.
- [Gegevens kopiëren van/naar Azure Data Lake Store](connector-azure-data-lake-store.md), in welk geval data factory-service-identiteit kan worden gebruikt als een van de ondersteunde typen voor Data Lake Store-verificatie.

## <a name="generate-service-identity"></a>Service-identiteit genereren

Data factory-service-identiteit gegenereerd als volgt:

- Bij het maken van de gegevensfactory via **Azure-portal of PowerShell**, service-identiteit altijd automatisch worden gemaakt omdat de openbare preview ADF V2.
- Bij het maken van de gegevensfactory via **SDK**, service-identiteit maakt alleen als u ' identiteit nieuwe FactoryIdentity() = ' in het factory-object voor het maken van. Zie het voorbeeld in [.NET Quick Start - gegevensfactory maken](quickstart-create-data-factory-dot-net.md#create-a-data-factory).
- Bij het maken van de gegevensfactory via **REST-API**, service-identiteit maakt alleen als u ' identiteitsgedeelte ' in de aanvraagtekst opgeeft. Zie het voorbeeld in [REST-Quick Start - gegevensfactory maken](quickstart-create-data-factory-rest-api.md#create-a-data-factory).

Als u uw gegevensfactory beschikt niet over een service-identiteit die zijn gekoppeld na vinden [ophalen van de service-identiteit](#retrieve-service-identity) instructie, kunt u expliciet genereren door de gegevensfactory programmatisch met identiteit initiator bijwerken:

- [Genereren met behulp van PowerShell-service-identiteit](#generate-service-identity-using-powershell)
- [Genereren met behulp van REST-API-service-identiteit](#generate-service-identity-using-rest-api)
- [Genereren met behulp van SDK-service-identiteit](#generate-service-identity-using-sdk)

>[!NOTE]
>- Service-identiteit kan niet worden gewijzigd. Bijwerken van een data factory die al een service-identiteit hebben geen invloed, de service-identiteit blijft ongewijzigd.
>- Als u een gegevensfactory die u hebt al een service-identiteit zonder op te geven van de parameter 'identity' in het factory-object of zonder op te geven ' identiteitsgedeelte ' in de aanvraagtekst REST bijwerkt, krijgt u een fout opgetreden.
>- Wanneer u een gegevensfactory verwijdert, worden de gekoppelde service-identiteit langs verwijderd.

### <a name="generate-service-identity-using-powershell"></a>Genereren met behulp van PowerShell-service-identiteit

Roep **Set AzureRmDataFactoryV2** opdracht opnieuw, en u ziet 'Id' velden zojuist wordt gegenereerd:

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

### <a name="generate-service-identity-using-rest-api"></a>Genereren met behulp van REST-API-service-identiteit

Hieronder API aanroepen met ' identiteitsgedeelte ' in de aanvraagtekst:

```
PATCH https://management.azure.com/subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/<data factory name>?api-version=2017-09-01-preview
```

**Aanvraagtekst**: "identiteit" toevoegen: {'type': 'SystemAssigned'}.

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

**Antwoord**: identiteit van de service wordt automatisch gemaakt en ' identiteitsgedeelte ' dienovereenkomstig wordt gevuld.

```json
{
    "name": "ADFV2DemoFactory",
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
    "location": "EastUS"
}
```

### <a name="generate-service-identity-using-sdk"></a>Genereren met behulp van SDK-service-identiteit

Roep de functie data factory create_or_update met id = nieuwe FactoryIdentity(). Voorbeeldcode met .NET:

```csharp
Factory dataFactory = new Factory
{
    Location = <region>,
    Identity = new FactoryIdentity()
};
client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, dataFactory);
```

## <a name="retrieve-service-identity"></a>Service-identiteit ophalen

U kunt de service-identiteit van de Azure-portal of programmatisch kunt ophalen. De volgende secties ziet enkele voorbeelden.

>[!TIP]
> Als er geen service-identiteit [genereren van de service-identiteit](#generate-service-identity) door het bijwerken van uw gegevensfactory.

### <a name="retrieve-service-identity-using-azure-portal"></a>Ophalen van de service-identiteit met Azure portal

U vindt de identiteitsgegevens van de service van Azure-portal -> uw data factory -> Instellingen -> eigenschappen:

- IDENTITY-SERVICE-ID
- SERVICE-IDENTITEIT TENANT
- **TOEPASSINGS-ID voor SERVICE-identiteit** > deze waarde kopiëren

![Service-identiteit ophalen](media/data-factory-service-identity/retrieve-service-identity-portal.png)

### <a name="retrieve-service-identity-using-powershell"></a>Service-identiteit met behulp van PowerShell ophalen

De identiteit van de service principal-ID en tenant-ID wordt geretourneerd als u een specifieke gegevensfactory als volgt:

```powershell
PS C:\WINDOWS\system32> (Get-AzureRmDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName>).Identity

PrincipalId                          TenantId
-----------                          --------
765ad4ab-XXXX-XXXX-XXXX-51ed985819dc 72f988bf-XXXX-XXXX-XXXX-2d7cd011db47
```

Kopieer de principal-ID en voer vervolgens onder Azure Active Directory-opdracht met de principal-ID als parameter voor het ophalen van de **ApplicationId**, dat u gebruiken om toegang te verlenen:

```powershell
PS C:\WINDOWS\system32> Get-AzureRmADServicePrincipal -ObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc

ServicePrincipalNames : {76f668b3-XXXX-XXXX-XXXX-1b3348c75e02, https://identity.azure.net/P86P8g6nt1QxfPJx22om8MOooMf/Ag0Qf/nnREppHkU=}
ApplicationId         : 76f668b3-XXXX-XXXX-XXXX-1b3348c75e02
DisplayName           : ADFV2DemoFactory
Id                    : 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
Type                  : ServicePrincipal
```

## <a name="next-steps"></a>Volgende stappen
Zie de volgende onderwerpen die introduceren wanneer en hoe u data factory-service-identiteit:

- [Opslaan van referenties in Azure Sleutelkluis](store-credentials-in-key-vault.md)
- [Gegevens kopiëren van/naar Azure Data Lake Store met behulp van verificatie van de beheerde service-identiteit](connector-azure-data-lake-store.md)

Zie [MSI overzicht](~/articles/active-directory/msi-overview.md) voor meer achtergrondinformatie over Service-identiteit beheerd welke data factory-service-identiteit is gebaseerd op. 
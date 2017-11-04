---
title: Referenties opslaan in Azure Sleutelkluis | Microsoft Docs
description: Informatie over het opslaan van referenties voor gegevensopslag gebruikt in een Azure sleutelkluis die Azure Data Factory automatisch tijdens runtime ophalen kunnen.
services: data-factory
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: jingwang
ms.openlocfilehash: 193d7c77f01384106b3e0d932d02ba6cdff9e750
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="store-credential-in-azure-key-vault"></a>Opslaan van referenties in Azure Sleutelkluis

Slaat u referenties voor gegevensopslag in een [Azure Key Vault](../key-vault/key-vault-whatis.md). Azure Data Factory haalt de referenties bij het uitvoeren van een activiteit die gebruikmaakt van het gegevensarchief. Op dit moment alleen [Dynamics connector](connector-dynamics-crm-office-365.md) deze functie ondersteunen.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [documenttion voor Data Factory version1 gedefinieerd](v1/data-factory-introduction.md).

## <a name="steps"></a>Stappen

Bij het maken van een gegevensfactory kan een service-identiteit samen met het maken van de fabriek worden gemaakt. De service-identiteit is geregistreerd bij Azure activiteit Directory beheerde toepassingen en deze specifieke gegevensfactory vertegenwoordigt.

- Bij het maken van de gegevensfactory via **Azure-portal of PowerShell**, service-identiteit altijd automatisch worden gemaakt omdat de openbare preview.
- Bij het maken van de gegevensfactory via **SDK**, service-identiteit maakt alleen als u ' identiteit nieuwe FactoryIdentity() = ' in het factory-object voor het maken van. Zie het voorbeeld van [.NET Quick Start - gegevensfactory maken](quickstart-create-data-factory-dot-net.md#create-a-data-factory).
- Bij het maken van de gegevensfactory via **REST-API**, service-identiteit maakt alleen als u ' identiteitsgedeelte ' in de aanvraagtekst opgeeft. Zie het voorbeeld van [REST-Quick Start - gegevensfactory maken](quickstart-create-data-factory-rest-api.md#create-a-data-factory).

Om te verwijzen naar een referentie die zijn opgeslagen in Azure Sleutelkluis, moet u:

1. Kopieer de "SERVICE identiteit TOEPASSINGS-ID gegenereerd samen met uw gegevensfactory. Raadpleeg [ophalen van de service-identiteit](#retrieve-service-identity).
2. De service-identiteit toegang verlenen aan uw Azure Sleutelkluis. In de sleutelkluis -> Access control -> toevoegen -> deze service identiteit toepassings-ID om toe te voegen lezer machtiging zoeken. Hierdoor kan de aangewezen fabriek voor toegang tot geheim in de sleutelkluis.
3. Maak een gekoppelde service verwijst naar uw Azure Sleutelkluis. Raadpleeg [gekoppelde service van Azure Sleutelkluis](#azure-key-vault-linked-service).
4. Gegevens store gekoppelde service, in welke verwijzing naar de bijbehorende opgeslagen in geheime sleutel-kluis maken. Raadpleeg [verwijzen naar referenties opgeslagen in de sleutelkluis](#reference-credential-stored-in-key-vault).

## <a name="retrieve-service-identity"></a>Service-identiteit ophalen

U kunt de service-identiteit van de Azure-portal of programmatisch kunt ophalen. De volgende secties ziet enkele voorbeelden.

>[!TIP]
> Als er geen service-identiteit [genereren van de service-identiteit](#generate-service-identity) door het bijwerken van uw gegevensfactory.

### <a name="using-azure-portal"></a>Azure Portal gebruiken

U vindt de identiteitsgegevens van de service van Azure-portal -> uw data factory -> Instellingen -> eigenschappen:

- IDENTITY-SERVICE-ID
- SERVICE-IDENTITEIT TENANT
- **TOEPASSINGS-ID voor SERVICE-identiteit** > deze waarde voor het verlenen van toegang in de Sleutelkluis kopiëren

![Service-identiteit ophalen](media/store-credentials-in-key-vault/retrieve-service-identity-portal.png)

### <a name="using-powershell"></a>PowerShell gebruiken

De identiteit van de service principal-ID en tenant-ID wordt geretourneerd als u een specifieke gegevensfactory als volgt:

```powershell
PS C:\WINDOWS\system32> (Get-AzureRmDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName>).Identity

PrincipalId                          TenantId
-----------                          --------
765ad4ab-XXXX-XXXX-XXXX-51ed985819dc 72f988bf-XXXX-XXXX-XXXX-2d7cd011db47
```

Kopieer de principal-ID en voer vervolgens onder Azure Active Directory-opdracht met de principal-ID als parameter voor het ophalen van de **ApplicationId**, waarmee u toegang in de Sleutelkluis verlenen:

```powershell
PS C:\WINDOWS\system32> Get-AzureRmADServicePrincipal -ObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc

ServicePrincipalNames : {76f668b3-XXXX-XXXX-XXXX-1b3348c75e02, https://identity.azure.net/P86P8g6nt1QxfPJx22om8MOooMf/Ag0Qf/nnREppHkU=}
ApplicationId         : 76f668b3-XXXX-XXXX-XXXX-1b3348c75e02
DisplayName           : ADFV2DemoFactory
Id                    : 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
Type                  : ServicePrincipal
```

## <a name="azure-key-vault-linked-service"></a>Azure Sleutelkluis gekoppelde service

De volgende eigenschappen worden ondersteund voor Azure Sleutelkluis gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **AzureKeyVault**. | Ja |
| BaseUrl | Geef de URL van Azure Sleutelkluis. | Ja |

**Voorbeeld:**

```json
{
    "name": "AzureKeyVaultLinkedService",
    "properties": {
        "type": "AzureKeyVault",
        "typeProperties": {
        "baseUrl": "https://<azureKeyVaultName>.vault.azure.net"
        }
    }
}
```

## <a name="reference-credential-stored-in-key-vault"></a>Verwijzing naar referenties opgeslagen in de sleutelkluis

De volgende eigenschappen worden ondersteund wanneer u een veld in de gekoppelde service verwijst naar een geheim sleutelkluis configureren:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van het veld moet worden ingesteld op: **AzureKeyVaultSecret**. | Ja |
| secretName | De naam van het geheim in azure sleutelkluis. | Ja |
| secretVersion | De versie van het geheim in azure sleutelkluis.<br/>Als niet wordt opgegeven, gebruikt deze altijd de nieuwste versie van het geheim.<br/>Indien opgegeven, klikt u vervolgens blijven deze hangen naar de opgegeven versie.| Nee |
| opslaan | Verwijst naar een Azure Key Vault gekoppelde service die u gebruikt voor het opslaan van de referentie. | Ja |

**Voorbeeld: (Zie de sectie 'password')**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "typeProperties": {
            "deploymentType": "<>",
            "organizationName": "<>",
            "authenticationType": "<>",
            "username": "<>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "mySecret",
                "store":{
                    "linkedServiceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
}
```

## <a name="generate-service-identity"></a>Service-identiteit genereren

Als u uw gegevensfactory beschikt niet over een service-identiteit die zijn gekoppeld na vinden [ophalen van de service-identiteit](#retrieve-service-identity) instructie, u door het bijwerken van de gegevensfactory met de identiteit initiator programmatisch kunt genereren.

> [!NOTE]
> - **Service-identiteit kan niet worden gewijzigd**. Bijwerken van een data factory die al een service-identiteit hebben geen invloed, de service-identiteit ongewijzigd moet worden bewaard.
> - **Service-identiteit kan niet worden verwijderd**. Als u een gegevensfactory die u hebt al een service-identiteit zonder op te geven van de parameter 'identity' in het factory-object of zonder op te geven ' identiteitsgedeelte ' in de aanvraagtekst REST bijwerkt, krijgt u een fout opgetreden.

De volgende secties ziet enkele voorbeelden op service-identiteit voor uw factory genereren als het bestand niet bestaat.

### <a name="using-powershell"></a>PowerShell gebruiken

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

### <a name="using-rest-api"></a>REST API gebruiken

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

### <a name="using-sdk"></a>Met behulp van SDK

Roep de functie data factory create_or_update met id = nieuwe FactoryIdentity(). Voorbeeldcode met .NET:

```csharp
Factory dataFactory = new Factory
{
    Location = <region>,
    Identity = new FactoryIdentity()
};
client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, dataFactory);
```

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
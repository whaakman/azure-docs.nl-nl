---
title: Beheer van de levenscyclus van de Azure-opslag
description: Informatie over het maken lifecycle beleidsregels overgang ouderdom gegevens van warm naar koud en archief lagen.
services: storage
author: yzheng-msft
ms.service: storage
ms.topic: article
ms.date: 11/04/2018
ms.author: yzheng
ms.subservice: common
ms.openlocfilehash: 3013365f96fa6841e4db826f9010c6ea994eba2a
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56879591"
---
# <a name="managing-the-azure-blob-storage-lifecycle-preview"></a>Beheren van de Azure Blob-opslag Lifecycle (Preview)

Gegevenssets hebben unieke Lifecycle-beleid. Vroeg in de levenscyclus, mensen toegang tot bepaalde gegevens vaak. Maar hebt u aanzienlijk als de gegevens van de leeftijd door de noodzaak voor toegang wordt geweigerd. Sommige gegevens in de cloud blijft niet-actieve en zelden worden gebruikt wanneer deze zijn opgeslagen. Sommige gegevens verlopen dagen of maanden na het maken, terwijl andere gegevenssets actief worden gelezen en gewijzigd gedurende hun levensduur. Azure Blob storage-levenscyclusbeheer (Preview) biedt een uitgebreide, op regels gebaseerde beleid voor GPv2- en Blob storage-accounts. Gebruik het beleid aan uw gegevens naar de juiste laag overgang of verloopt aan het einde van de levenscyclus van de gegevens.

De levenscyclus van management-beleid kunt u:

- Overgang van blobs en een minder dynamische opslaglaag (' hot ' naar ' Cool ', ' hot ' als u wilt archiveren of koud naar archief) om te optimaliseren voor prestaties en kosten
- Verwijderen van blobs aan het einde van de levenscycli hiervan
- Definieer regels moet één keer per dag worden uitgevoerd op het niveau van de storage-account
- Regels toepassen op containers of een subset van blobs (met behulp van voorvoegsels als filters)

Houd rekening met het scenario waar een gegevensset opgehaald regelmatig toegang tijdens de eerste fasen van de levenscyclus van, maar vervolgens alleen af en toe na twee weken. Na de eerste maand, wordt de gegevensset zelden worden gebruikt. In dit scenario wordt er tijdens de eerste fasen hot storage aanbevolen. Koude opslag is het meest geschikt is voor incidentele toegang en archiefopslag is de beste optie laag nadat de gegevens van de leeftijd meer dan een maand. Door het aanpassen van opslaglagen met betrekking tot de leeftijd van de gegevens, kunt u de minst dure opslagopties ontwerpen voor uw behoeften. Voor het bereiken van deze overgang, zijn beheerbeleidsregels lifecycle veroudering om gegevens te verplaatsen naar een minder dynamische laag beschikbaar.

## <a name="storage-account-support"></a>Ondersteuning voor Storage-account

De levenscyclus van management-beleid is beschikbaar bij zowel algemeen gebruik v2 (GPv2) accounts en Blob storage-accounts. U kunt een bestaande account voor algemeen gebruik (GPv1) upgraden naar een GPv2-account via een eenvoudig éénkliksproces in Azure portal. Zie [Overzicht van Azure-opslagaccount](../common/storage-account-overview.md) voor meer informatie over opslagaccounts.  

## <a name="pricing"></a>Prijzen 

De levenscyclus van management-functie is gratis in preview. Klanten betalen voor de bewerkingskosten van de normale voor de [Blobs weergeven](https://docs.microsoft.com/rest/api/storageservices/list-blobs) en [Blob-laag instellen](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) API-aanroepen. Zie voor meer informatie over prijzen [prijzen voor blok-Blob](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="register-for-preview"></a>Registreren voor de Preview-versie 
Om in te schrijven in openbare preview, moet u een aanvraag indient bij het registreren van deze functie kunt u uw abonnement. Aanvragen worden meestal binnen 72 uur goedgekeurd. In afwachting van goedkeuring, alle bestaande en nieuwe GPv2- of Blob storage-accounts in de volgende regio's zijn onder andere de functie: VS-West 2, West-Centraal VS, VS-Oost 2 en West-Europa. Preview-versie ondersteunt alleen blok-blob. Net als bij de meeste Preview-versies, mag niet u deze functie voor werkbelastingen voor productie gebruiken totdat het bereikt die algemene beschikbaarheid.

Als u wilt een aanvraag indient, voer de volgende PowerShell of CLI-opdrachten.

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Een aanvraag indienen:

```powershell
Register-AzProviderFeature -FeatureName DLM -ProviderNamespace Microsoft.Storage 
```
U kunt de status van de goedkeuring registratie met de volgende opdracht controleren:
```powershell
Get-AzProviderFeature -FeatureName DLM -ProviderNamespace Microsoft.Storage
```
Bij goedkeuring en correct is geregistreerd, ontvangt u de *geregistreerde* status wanneer u de vorige aanvragen indienen.

### <a name="azure-cli"></a>Azure-CLI

Een aanvraag indienen: 
```cli
az feature register --namespace Microsoft.Storage --name DLM
```
U kunt de status van de goedkeuring registratie met de volgende opdracht controleren:
```cli
az feature show --namespace Microsoft.Storage --name DLM
```
Bij goedkeuring en correct is geregistreerd, ontvangt u de *geregistreerde* status wanneer u de vorige aanvragen indienen.


## <a name="add-or-remove-a-policy"></a>Toevoegen of verwijderen van een beleid 

U kunt toevoegen, bewerken of verwijderen van een beleid met behulp van Azure portal, [PowerShell](https://www.powershellgallery.com/packages/Az.Storage), [Azure CLI](https://docs.microsoft.com/cli/azure/ext/storage-preview/storage/account/management-policy?view=azure-cli-latest#ext-storage-preview-az-storage-account-management-policy-create), [REST-API's](https://docs.microsoft.com/rest/api/storagerp/managementpolicies/createorupdate), of clienthulpprogramma's in de volgende talen: [.NET ](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/8.0.0-preview), [Python](https://pypi.org/project/azure-mgmt-storage/2.0.0rc3/), [Node.js]( https://www.npmjs.com/package/azure-arm-storage/v/5.0.0), [Ruby](https://rubygems.org/gems/azure_mgmt_storage/versions/0.16.2). 

### <a name="azure-portal"></a>Azure Portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer **alle Resources** en selecteer vervolgens uw opslagaccount.

3. Selecteer **levenscyclusbeheer (preview)** gegroepeerd op Blob-Service weergeven of wijzigen van uw beleid.

### <a name="powershell"></a>PowerShell

```powershell
$rules = '{ ... }' 

Set-AzStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName] -Policy $rules 

Get-AzStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName]
```

### <a name="azure-cli"></a>Azure-CLI

```
az account set --subscription "[subscriptionName]”
az extension add --name storage-preview
az storage account management-policy show --resource-group [resourceGroupName] --account-name [accountName]
```

> [!NOTE]
Als u firewallregels voor uw opslagaccount inschakelt, worden lifecycle management-aanvragen geblokkeerd. U kunt deze aanvragen blokkering opheffen door op te geven van uitzonderingen. Zie voor meer informatie de sectie uitzonderingen in [firewalls en virtuele netwerken configureren](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).

## <a name="policy"></a>Beleid

Een lifecycle management-beleid is een verzameling van regels in een JSON-document:

```json
{
  "version": "0.5",
  "rules": [
    {
      "name": "rule1",
      "type": "Lifecycle",
      "definition": {...}
    },
    {
      "name": "rule2",
      "type": "Lifecycle",
      "definition": {...}
    }
  ]
}
```


Een beleid vereist twee parameters:

| Parameternaam | Parametertype | Opmerkingen |
|----------------|----------------|-------|
| versie        | Een tekenreeks die is uitgedrukt als `x.x` | Het nummer van de preview-versie is 0,5. |
| regels          | Een matrix met regelobjecten | Ten minste één regel is in elke beleidsregel vereist. Tijdens de preview, kunt u maximaal 4 regels per beleid opgeven. |

Elke regel in het beleid vereist drie parameters:

| Parameternaam | Parametertype | Opmerkingen |
|----------------|----------------|-------|
| Name           | String | De naam van een regel kan elke combinatie van alfanumerieke tekens bevatten. De naam van regel is hoofdlettergevoelig. Deze moet uniek zijn binnen een beleid. |
| type           | Een enum-waarde | De geldige waarde voor de Preview-versie is `Lifecycle`. |
| definitie     | Een object dat de levenscyclus van regel definieert | Elke definitie bestaat uit een filter en een actie. |

## <a name="rules"></a>Regels

De definitie van elke regel bevat een filterset en een actie-set. De [filteren set](#rule-filters) beperkt regelacties op een bepaalde set van objecten in een container of namen van objecten. De [actie set](#rule-actions) van toepassing is de laag of acties aan de gefilterde set objecten verwijderen.

### <a name="sample-rule"></a>Van voorbeeldregel
Het volgende voorbeeldregel filtert de account voor uitvoering van de acties alleen op `container1/foo`. Voer de volgende acties voor alle objecten die bestaan binnen `container1` **en** begint met `foo`: 

- Laag-blob naar de koude laag 30 dagen na de laatste wijziging
- Laag-blob naar de laag archief 90 dagen na de laatste wijziging
- Het verwijderen van de blob 2,555 dagen (zeven jaar) na de laatste wijziging
- 90 dagen na het maken van momentopnamen voor blob-momentopnamen verwijderen

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "ruleFoo", 
      "type": "Lifecycle", 
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "container1/foo" ]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
            "delete": { "daysAfterModificationGreaterThan": 2555 }
          },
          "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}

```

### <a name="rule-filters"></a>Regel filters

Filters beperken regelacties tot een subset van de blobs in de storage-account. Als meer dan één filter wordt gedefinieerd, een logische `AND` wordt uitgevoerd op alle filters.

Tijdens de preview, geldige filters zijn onder andere:

| Naam van het filter | Filtertype | Opmerkingen | Is vereist |
|-------------|-------------|-------|-------------|
| blobTypes   | Een matrix met vooraf gedefinieerde enum-waarden. | De evaluatieversie uitsluitend ondersteunt `blockBlob`. | Ja |
| prefixMatch | Een matrix met tekenreeksen voor voorvoegsels worden aan. Een voorvoegseltekenreeks moet beginnen met een containernaam. Bijvoorbeeld, als u wilt om alle blobs onder 'https://myaccount.blob.core.windows.net/container1/foo/... ' voor een regel, is het de prefixMatch `container1/foo`. | Als u geen prefixMatch definieert, zijn de regels van toepassing op alle blobs in het account. | Nee |

### <a name="rule-actions"></a>Regelacties

Acties worden uitgevoerd voor de gefilterde blobs wanneer de uitvoering van voorwaarde wordt voldaan.

Preview-versie biedt ondersteuning voor levenscyclusbeheer in lagen en verwijderen van blobs en verwijderen van blob-momentopnamen. Ten minste één actie voor elke regel op blobs of blobschermopnamen definiëren.

| Bewerking        | Basis-Blob                                   | Momentopname      |
|---------------|---------------------------------------------|---------------|
| tierToCool    | Ondersteuning voor blobs dat zich momenteel in de warme laag         | Niet ondersteund |
| tierToArchive | Ondersteuning voor blobs dat zich momenteel in de opslaglaag hot of cool | Niet ondersteund |
| delete        | Ondersteund                                   | Ondersteund     |

>[!NOTE] 
Als u meer dan één actie op dezelfde blob definieert, is levensduurbeheer van de minst dure actie van toepassing op de blob. Bijvoorbeeld: actie `delete` is goedkoper dan actie `tierToArchive`. Actie `tierToArchive` is goedkoper dan actie `tierToCool`.

Preview-versie zijn de voorwaarden van de uitvoering van actie op basis van leeftijd. Basis-blobs gebruikt u de laatst gewijzigd om bij te houden van de leeftijd en blob-momentopnamen gebruiken de aanmaaktijd van de momentopname voor het bijhouden van leeftijd.

| Actie tot uitvoering van voorwaarde | Voorwaarde-waarde | Description |
|----------------------------|-----------------|-------------|
| daysAfterModificationGreaterThan | Geheel getal-waarde die aangeeft van de leeftijd in dagen | Geldige voorwaarde voor base blob-acties |
| daysAfterCreationGreaterThan     | Geheel getal-waarde die aangeeft van de leeftijd in dagen | Geldige voorwaarde voor acties van blob-momentopname | 

## <a name="examples"></a>Voorbeelden
De volgende voorbeelden laten zien hoe u algemene scenario's met de levenscyclus van beleidsregels.

### <a name="move-aging-data-to-a-cooler-tier"></a>Ouderdom gegevens verplaatsen naar een minder dynamische laag

Dit voorbeeld laat zien hoe u voor de overgang van blok-blobs voorafgegaan door `container1/foo` of `container2/bar`. Het beleid overgangen blobs die nog niet zijn gewijzigd in meer dan 30 dagen op koude opslag en -blobs niet worden gewijzigd in 90 dagen voor de archive-laag:

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "agingRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ],
            "prefixMatch": [ "container1/foo", "container2/bar" ]
          },
          "actions": {
            "baseBlob": {
              "tierToCool": { "daysAfterModificationGreaterThan": 30 },
              "tierToArchive": { "daysAfterModificationGreaterThan": 90 }
            }
          }
        }      
    }
  ]
}
```

### <a name="archive-data-at-ingest"></a>Archiveer gegevens in de opname 

Sommige gegevens in de cloud blijft niet-actieve en zelden, als tot nooit geopend eenmaal opgeslagen. Deze gegevens archiveren onmiddellijk één keer worden opgenomen. De volgende lifecycle-beleid is geconfigureerd voor het archiveren van data-at-opname. In dit voorbeeld overgangen blok-blobs in de storage-account in container `archivecontainer` onmiddellijk in een archive-laag. De onmiddellijke overgang wordt bereikt door het uitvoeren van blobs 0 dagen na het tijdstip laatst gewijzigd:

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "archiveRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ],
            "prefixMatch": [ "archivecontainer" ]
          },
          "actions": {
            "baseBlob": { 
                "tierToArchive": { "daysAfterModificationGreaterThan": 0 }
            }
          }
        }      
    }
  ]
}

```

### <a name="expire-data-based-on-age"></a>Gegevens op basis van leeftijd laten verlopen

Sommige gegevens wordt verlopen dagen of maanden na het maken van kosten verlagen of te voldoen aan de overheidsvereisten verwacht. U kunt een levenscyclus van management-beleid om te verlopen gegevens door verwijderen op basis van gegevens leeftijd configureren. Het volgende voorbeeld ziet een beleid dat Hiermee verwijdert u alle blok-blobs (met geen voorvoegsel dat is opgegeven) ouder is dan 365 dagen.

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "expirationRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ]
          },
          "actions": {
            "baseBlob": {
              "delete": { "daysAfterModificationGreaterThan": 365 }
            }
          }
        }      
    }
  ]
}
```

### <a name="delete-old-snapshots"></a>Oude momentopnamen verwijderen

Voor gegevens die is gewijzigd en gedurende hun levensduur regelmatig geopend, worden momentopnamen vaak gebruikt voor het bijhouden van oudere versies van de gegevens. U kunt een beleid dat Hiermee verwijdert u de oude momentopnamen op basis van de leeftijd momentopname maken. De leeftijd van de momentopname wordt bepaald door het evalueren van de aanmaaktijd van de momentopname. Deze beleidsregel verwijderen blokkeren blob-momentopnamen binnen de container `activedata` die zijn 90 dagen of ouder nadat de momentopname is gemaakt.

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "snapshotRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ],
            "prefixMatch": [ "activedata" ]
          },
          "actions": {            
            "snapshot": {
              "delete": { "daysAfterCreationGreaterThan": 90 }
            }
          }
        }      
    }
  ]
}
```
## <a name="faq---i-created-a-new-policy-why-are-the-actions-not-run-immediately"></a>Veelgestelde vragen - ik heb gemaakt voor een nieuw beleid, waarom worden de acties die niet direct uitvoeren? 

Het platform wordt één keer per dag uitgevoerd de lifecycle-beleid. Nadat u een nieuw beleid hebt ingesteld, duurt het tot 24 uur voor sommige acties (zoals blobniveau en verwijdering) start en uitvoert.  

## <a name="next-steps"></a>Volgende stappen

Informatie over het herstellen van gegevens na het per ongeluk verwijderen:

- [Voorlopig verwijderen voor Azure Storage-blobs](../blobs/storage-blob-soft-delete.md)

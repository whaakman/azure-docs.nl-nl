---
title: Beheer van de levenscyclus van de Azure-opslag
description: Informatie over het maken lifecycle beleidsregels overgang againg gegevens van hot naar cool en archive-lagen.
services: storage
author: yzheng-msft
ms.service: storage
ms.topic: article
ms.date: 04/30/2018
ms.author: yzheng
ms.component: common
ms.openlocfilehash: edc0cf7c8700e1ab728109bc9cbfda8135a59ee9
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45574728"
---
# <a name="managing-the-azure-blob-storage-lifecycle-preview"></a>Beheer van de levenscyclus van Azure Blob Storage (Preview)

Gegevenssets hebben unieke Lifecycle-beleid. Sommige gegevens worden vaak vroeg geopend in de levenscyclus van, maar de noodzaak voor toegang wordt geweigerd hebt u aanzienlijk als de gegevens van de leeftijd. Sommige gegevens in de cloud inactief en zelden worden gebruikt wanneer deze zijn opgeslagen. Sommige gegevens verlopen dagen of maanden na het maken, terwijl andere gegevenssets actief worden gelezen en gewijzigd gedurende hun levensduur. Azure Blob-opslag levenscyclusbeheer (Preview) biedt een uitgebreide, op basis van regels beleid die u gebruiken kunt voor de overgang van uw gegevens naar de beste toegangslaag en verloopt gegevens aan het einde van de levenscyclus.

Levenscyclusbeheerbeleid kunt u:

- Overgang van blobs naar een minder dynamische opslaglaag (warm naar koud, warm naar archief of koud naar archief) om te optimaliseren voor prestaties en kosten
- Verwijderen van blobs aan het einde van de levenscycli hiervan
- Definieer regels moet één keer per dag worden uitgevoerd op het niveau van de storage-account (het ondersteunt zowel GPv2- en Blob storage-accounts)
- Regels toepassen op containers of een subset van blobs (met behulp van voorvoegsels als filters)

Houd rekening met een set gegevens die vaak tijdens de eerste fase van de levenscyclus is geopend, is alleen nodig af en toe na twee weken en zelden worden gebruikt na één maand en daarbuiten. In dit scenario hot storage is het beste tijdens de eerste fasen koude opslag is het meest geschikt is voor incidentele toegang en archiefopslag is de beste optie laag nadat de gegevens van de leeftijd meer dan een maand. Door het aanpassen van opslaglagen met betrekking tot de leeftijd van de gegevens, kunt u de minst dure opslagopties ontwerpen voor uw behoeften. Voor het bereiken van deze overgang zijn lifecycle management-beleid veroudering om gegevens te verplaatsen naar een minder dynamische laag beschikbaar.

## <a name="storage-account-support"></a>Ondersteuning voor Storage-account

Levenscyclusbeheerbeleid is beschikbaar bij zowel algemeen gebruik v2 (GPv2)-account en Blob Storage-account. U kunt een bestaande account voor algemeen gebruik (GPv1) converteren naar een GPv2-account via een eenvoudig éénkliksproces in Azure portal. Zie [opties voor Azure Storage-account](../common/storage-account-options.md) voor meer informatie.  

## <a name="pricing"></a>Prijzen 

Functie voor het beheer van levenscyclus is gratis in preview. Klanten betalen voor de bewerkingskosten van de normale voor de [Blobs weergeven](https://docs.microsoft.com/rest/api/storageservices/list-blobs) en [Blob-laag instellen](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) API-aanroepen. Zie [prijzen voor blok-Blob](https://azure.microsoft.com/pricing/details/storage/blobs/) voor meer informatie over prijzen.

## <a name="register-for-preview"></a>Registreren voor de Preview-versie 
Als u wilt registreren in openbare preview-versie, moet u een aanvraag indient bij het registreren van deze functie kunt u uw abonnement. Nadat uw aanvraag is goedgekeurd (binnen een paar dagen), is alle bestaande en nieuwe GPv2 of Blob Storage-account in VS-West 2, West-Centraal VS en West-Europa, heeft de functie is ingeschakeld. Tijdens de preview, wordt alleen blok-blob ondersteund. Net als bij de meeste Preview-versies, mag deze functie niet worden gebruikt voor werkbelastingen voor productie totdat het bereikt die algemene beschikbaarheid.

Als u wilt een aanvraag indient, voer de volgende PowerShell of CLI-opdrachten.

### <a name="powershell"></a>PowerShell

Een aanvraag indienen:

```powershell
Register-AzureRmProviderFeature -FeatureName DLM -ProviderNamespace Microsoft.Storage 
```
U kunt de status van de goedkeuring registratie met de volgende opdracht controleren:
```powershell
Get-AzureRmProviderFeature -FeatureName DLM -ProviderNamespace Microsoft.Storage
```
Als de functie is goedgekeurd en juist is geregistreerd, kunt u de status 'Registered' moeten ontvangen.

### <a name="cli-20"></a>CLI 2.0

Een aanvraag indienen: 
```cli
az feature register --namespace Microsoft.Storage --name DLM
```
U kunt de status van de goedkeuring registratie met de volgende opdracht controleren:
```cli
az feature show --namespace Microsoft.Storage --name DLM
```
Als de functie is goedgekeurd en juist is geregistreerd, kunt u de status 'Registered' moeten ontvangen. 


## <a name="add-or-remove-policies"></a>Beleidsregels toevoegen of verwijderen 

U kunt toevoegen, bewerken of verwijderen van een beleid met behulp van Azure portal, [PowerShell](https://www.powershellgallery.com/packages/AzureRM.Storage/5.0.3-preview), [REST-API's](https://docs.microsoft.com/rest/api/storagerp/storageaccounts/createorupdatemanagementpolicies), of clienthulpprogramma's in de volgende talen: [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/8.0.0-preview), [Python](https://pypi.org/project/azure-mgmt-storage/2.0.0rc3/), [Node.js]( https://www.npmjs.com/package/azure-arm-storage/v/5.0.0), [Ruby](   https://rubygems.org/gems/azure_mgmt_storage/versions/0.16.2). 

### <a name="azure-portal"></a>Azure Portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Ga naar uw opslagaccount, selecteer Alle resources en selecteer vervolgens uw opslagaccount.

3. Klik in de blade instellingen op **levenscyclusbeheer** gegroepeerd op Blob-Service om te bekijken en/of beleidsregels wijzigen.

### <a name="powershell"></a>PowerShell

```powershell
$rules = '{ ... }' 

Set-AzureRmStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName] -Policy $rules 

Get-AzureRmStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName]
```

> [!NOTE]
Als u firewallregels voor uw opslagaccount inschakelt, worden lifecycle management-aanvragen geblokkeerd. U kunt deze blokkering opheffen door op te geven van uitzonderingen. Zie voor meer informatie de sectie uitzonderingen op [firewalls en virtuele netwerken configureren](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).

## <a name="policies"></a>Beleidsregels

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


Binnen een beleid worden twee parameters nodig:

| Parameternaam | Parametertype | Opmerkingen |
|----------------|----------------|-------|
| versie        | Een tekenreeks die is uitgedrukt als `x.x` | Het nummer van de preview-versie is 0,5 |
| regels          | Een matrix met regelobjecten | Ten minste één regel is in elke beleidsregel vereist. Tijdens de preview, kunt u maximaal 4 regels per beleid opgeven. |

In een regel vereiste parameters zijn:

| Parameternaam | Parametertype | Opmerkingen |
|----------------|----------------|-------|
| Naam           | Reeks | De naam van een regel kan elke combinatie van alfanumerieke tekens bevatten. De naam van regel is hoofdlettergevoelig. Deze moet uniek zijn binnen een beleid. |
| type           | Een enum-waarde | De geldige waarde voor de Preview-versie is `Lifecycle` |
| definitie     | Een object dat de levenscyclus van regel definieert | Elke definitie is opgebouwd met een filter en een actie. |

## <a name="rules"></a>Regels

De definitie van elke regel bevat een filterset en een actie-set. Het volgende voorbeeldregel wijzigt de laag voor base blok-blobs met het voorvoegsel `container1/foo`. In het beleid, worden deze regels zijn gedefinieerd als:

- Laag-blob naar ' Cool ' opslag van 30 dagen na de laatste wijziging
- Laag-blob naar Archive storage 90 dagen na de laatste wijziging
- Het verwijderen van de blob 2,555 dagen (7 jaar) na de laatste wijziging
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

Filters beperken regelacties tot een subset van de blobs in de storage-account. Als meerdere filters zijn gedefinieerd, een logische `AND` wordt uitgevoerd op alle filters.

Tijdens de preview, geldige filters zijn onder andere:

| Naam van het filter | Filtertype | Opmerkingen | Is vereist |
|-------------|-------------|-------|-------------|
| blobTypes   | Een matrix met vooraf gedefinieerde enum-waarden. | Voor de preview-versie, alleen `blockBlob` wordt ondersteund. | Ja |
| prefixMatch | Een matrix met tekenreeksen voor voorvoegsels worden aan. Een voorvoegseltekenreeks moet beginnen met een containernaam. Bijvoorbeeld, als alle blobs onder 'https://myaccount.blob.core.windows.net/mycontainer/mydir/... ' moeten worden gekoppeld voor een regel, is het voorvoegsel ' mycontainer/mijnmap'. | Als dit niet is gedefinieerd, wordt deze regel geldt voor alle blobs in het account. | Nee |

### <a name="rule-actions"></a>Regelacties

Acties worden uitgevoerd voor de gefilterde blobs wanneer de uitvoering van voorwaarde wordt voldaan.

Preview-versie biedt ondersteuning voor levenscyclusbeheer blobniveau en verwijdering van blob en het verwijderen van blobmomentopnamen. Elke regel moet ten minste één actie gedefinieerd voor blobs of blobschermopnamen hebben.

| Bewerking        | Basis-Blob                                   | Momentopname      |
|---------------|---------------------------------------------|---------------|
| tierToCool    | Ondersteuning voor blobs dat zich momenteel in de warme laag         | Niet ondersteund |
| tierToArchive | Ondersteuning voor blobs dat zich momenteel in de warme of koude laag | Niet ondersteund |
| delete        | Ondersteund                                   | Ondersteund     |

>[!NOTE] 
Als meer dan één actie is gedefinieerd in dezelfde blob, geldt levensduurbeheer van de minst dure actie naar de blob. (bijv, actie `delete` is goedkoper dan actie `tierToArchive`. Actie `tierToArchive` is goedkoper dan actie `tierToCool`.)

Preview-versie zijn de voorwaarden van de uitvoering van actie op basis van leeftijd. Maakt gebruik van basis blob tijd bij te houden leeftijd en blob-momentopnamen maakt gebruik van momentopname Aanmaaktijd om bij te houden leeftijd het laatst is gewijzigd.

| Actie tot uitvoering van voorwaarde | Voorwaarde-waarde | Beschrijving |
|----------------------------|-----------------|-------------|
| daysAfterModificationGreaterThan | Geheel getal-waarde die aangeeft van de leeftijd in dagen | Geldige voorwaarde voor base blob-acties |
| daysAfterCreationGreaterThan     | Geheel getal-waarde die aangeeft van de leeftijd in dagen | Geldige voorwaarde voor acties van blob-momentopname | 

## <a name="examples"></a>Voorbeelden
De volgende voorbeelden laten zien hoe u algemene scenario's met de levenscyclus van beleidsregels.

### <a name="move-aging-data-to-a-cooler-tier"></a>Ouderdom gegevens verplaatsen naar een minder dynamische laag

Het volgende voorbeeld ziet u hoe u voor de overgang van blok-blobs voorafgegaan door `container1/foo` of `container2/bar`. Het beleid overgangen blobs die nog niet zijn gewijzigd in meer dan 30 dagen op koude opslag en -blobs niet worden gewijzigd in 90 dagen voor de archive-laag:

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

Weer andere gegevens in de cloud zijn inactief en worden, als ze eenmaal zijn opgeslagen, zelden tot nooit geopend. Deze gegevens is het beste worden gearchiveerd onmiddellijk nadat deze is opgenomen. De volgende lifecycle-beleid is geconfigureerd voor het archiveren van data-at-opname. In dit voorbeeld overgangen blok-blobs in de storage-account in container `archivecontainer` onmiddellijk in een archive-laag. De onmiddellijke overgang wordt bereikt door het uitvoeren van blobs 0 dagen na het tijdstip laatst gewijzigd:

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

Sommige gegevens wordt verlopen dagen of maanden na het maken van kosten verlagen of te voldoen aan wettelijke voorschriften verwacht. Een lifecycle management-beleid kan worden ingesteld op gegevens laten verlopen door verwijderen op basis van gegevens leeftijd. Het volgende voorbeeld ziet een beleid dat Hiermee verwijdert u alle blok-blobs (met geen voorvoegsel dat is opgegeven) ouder is dan 365 dagen.

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

## <a name="next-steps"></a>Volgende stappen

Informatie over het herstellen van gegevens na het per ongeluk verwijderen:

- [Voorlopig verwijderen voor Azure Storage-blobs ](../blobs/storage-blob-soft-delete.md)

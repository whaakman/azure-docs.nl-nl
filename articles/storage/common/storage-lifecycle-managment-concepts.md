---
title: Beheren van de levenscyclus van de Azure-opslag
description: Informatie over het lifecycle om beleidsregels te maken met overgang againg gegevens van hot naar cool en te archiveren segment.
services: storage
author: yzheng-msft
manager: jwillis
ms.service: storage
ms.workload: storage
ms.topic: article
ms.date: 04/30/2018
ms.author: yzheng
ms.openlocfilehash: 9721935f005bbd9a5dc261fe801ecc14744b004f
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36752789"
---
# <a name="managing-the-azure-blob-storage-lifecycle-preview"></a>Het beheren van de levenscyclus van de Azure Blob Storage (Preview)

Gegevenssets hebben unieke levenscycli. Sommige gegevens vaak vroeg in de levenscyclus wordt geopend, maar de noodzaak om toegang te krijgen drastisch wordt neergezet als de leeftijd van de gegevens. Sommige gegevens inactief kan zijn in de cloud en één keer opgeslagen zelden wordt geopend. Sommige gegevens verloopt dagen of maanden na het maken, terwijl andere gegevenssets actief worden gelezen en gewijzigd gedurende hun levensduur. Levenscyclusbeheer Azure Blob Storage (Preview) biedt een rijke, regel gebaseerd beleid dat u gebruiken kunt voor de overgang van uw gegevens om de beste toegangslaag en verloopt gegevens aan het einde van de levenscyclus.

De levenscyclus van management-beleid helpt u:

- Overgang blobs in een koelervoorbeeld storage-laag (Hot naar Cool, Hot te archiveren, of naar archief Cool) om te optimaliseren voor prestaties en kosten
- Blobs aan het einde van de levenscycli verwijderen
- Definieer regels voor één keer per dag worden uitgevoerd op het niveau van de storage-account (het ondersteunt zowel GPv2 als Blob storage-accounts)
- Regels toepassen op containers of een subset van BLOB's (met voorvoegsels als filters)

U kunt een set gegevens die tijdens de vroeg stadium van de levenscyclus vaak wordt geopend, is alleen nodig af en toe na twee weken en wordt zelden geopend na een maand en hoger. In dit scenario hot storage is het beste tijdens de eerste fasen cool storage is het meest geschikt voor incidenteel toegang en archiefopslag is de beste optie laag na de leeftijd van de gegevens gedurende een maand. Door het aanpassen van opslaglagen met betrekking tot de leeftijd van de gegevens, kunt u de minst dure opties voor opslag ontwerpen voor uw behoeften. Zodat deze overgang zijn lifecycle beheerbeleid veroudering om gegevens te verplaatsen naar koelervoorbeeld segment beschikbaar.

## <a name="storage-account-support"></a>Ondersteuning voor Storage-account

Levenscyclus management-beleid is beschikbaar met zowel algemeen v2 (GPv2)-account en Blob Storage-account. U kunt een bestaand account voor algemene doeleinden (GPv1) converteren naar een GPv2-account via een eenvoudig proces met één klik in de Azure portal. Zie [opties voor Azure Storage-account](../common/storage-account-options.md) voor meer informatie.  

## <a name="pricing"></a>Prijzen 

Onderdeel voor het beheer van levenscyclus is gratis Preview-versie. Klanten worden in rekening gebracht de bewerkingskosten van het normale voor de [lijst Blobs](https://docs.microsoft.com/rest/api/storageservices/list-blobs) en [Blob laag ingesteld](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) API-aanroepen. Zie [blok-Blob prijzen](https://azure.microsoft.com/pricing/details/storage/blobs/) voor meer informatie over prijzen.

## <a name="register-for-preview"></a>Registreren voor preview 
Als u wilt registreren in de openbare preview, moet u een aanvraag voor het registreren van deze functie kunt u uw abonnement. Nadat uw aanvraag is goedgekeurd (binnen een paar dagen), moeten alle bestaande en nieuwe GPv2 of Blob Storage-account in VS-West 2 en West-Centraal VS de functie is ingeschakeld. Tijdens de preview, wordt alleen blok-blob ondersteund. Net als bij de meeste previews mag deze functie niet worden gebruikt voor productieworkloads totdat het bereikt algemene beschikbaarheid.

Hiervoor een aanvraag indienen, voer de volgende PowerShell of CLI-opdrachten.

### <a name="powershell"></a>PowerShell

Een aanvraag indienen:

```powershell
Register-AzureRmProviderFeature -FeatureName DLM -ProviderNamespace Microsoft.Storage 
```
U kunt controleren dat de goedkeuringsstatus registratie met de volgende opdracht:
```powershell
Get-AzureRmProviderFeature -FeatureName DLM -ProviderNamespace Microsoft.Storage
```
Als de functie is goedgekeurd en juist is geregistreerd, kunt u de status 'Geregistreerde' ontvangt.

### <a name="cli-20"></a>CLI 2.0

Een aanvraag indienen: 
```cli
az feature register –-namespace Microsoft.Storage –-name DLM
```
U kunt controleren dat de goedkeuringsstatus registratie met de volgende opdracht:
```cli
-az feature show –-namespace Microsoft.Storage –-name DLM
```
Als de functie is goedgekeurd en juist is geregistreerd, kunt u de status 'Geregistreerde' ontvangt. 


## <a name="add-or-remove-policies"></a>Beleidsregels toevoegen of verwijderen 

U kunt toevoegen, bewerken of verwijderen van een beleid met Azure portal [PowerShell](https://www.powershellgallery.com/packages/AzureRM.Storage/5.0.3-preview), REST-API's of clienthulpprogramma's in de volgende talen: [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/8.0.0-preview), [Python](https://pypi.org/project/azure-mgmt-storage/2.0.0rc3/), [ Node.js]( https://www.npmjs.com/package/azure-arm-storage/v/5.0.0), [Ruby]( https://rubygems.org/gems/azure_mgmt_storage/versions/0.16.2). 

### <a name="azure-portal"></a>Azure Portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Ga naar uw opslagaccount, selecteer Alle resources en selecteer vervolgens uw opslagaccount.

3. Klik in de blade instellingen op **levenscyclusbeheer** gegroepeerd volgens de Blob-Service om te bekijken en/of beleidsregels wijzigen.

### <a name="powershell"></a>PowerShell

```powershell
$rules = '{ ... }' 

Set-AzureRmStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName] -Policy $rules 

Get-AzureRmStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName]
```

> [!NOTE]
Als u firewallregels voor uw opslagaccount inschakelt, worden de levenscyclus van management-aanvragen geblokkeerd. U kunt deze blokkering opheffen door te geven van uitzonderingen. Zie voor meer informatie de sectie uitzonderingen op [configureren van firewalls en virtuele netwerken](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).

## <a name="policies"></a>Beleid

Een levenscyclus management-beleid is een verzameling van regels in een JSON-document:

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


Twee parameters zijn vereist in een beleid:

| Parameternaam | Parametertype | Opmerkingen |
|----------------|----------------|-------|
| versie        | Een tekenreeks die is uitgedrukt als `x.x` | Het nummer van de preview-versie is 0,5 |
| regels          | Een matrix met regelobjecten | Ten minste één regel is in elke beleidsregel vereist. Tijdens de preview, kunt u maximaal 4 regels per beleid. |

Binnen een regel vereiste parameters zijn:

| Parameternaam | Parametertype | Opmerkingen |
|----------------|----------------|-------|
| Naam           | Reeks | De naam van een regel kan een combinatie van alfanumerieke tekens bevatten. De naam is hoofdlettergevoelig. Het moet uniek zijn binnen een beleid. |
| type           | Een enum-waarde | De geldige waarde voor het voorbeeld is `Lifecycle` |
| definitie     | Een object dat de levenscyclus van regel bepaalt | Elke definitie is opgebouwd met een filterset en een set actie. |

## <a name="rules"></a>Regels

De definitie van elke regel bevat een filterset en een set actie. Het volgende voorbeeldregel wijzigt de laag voor base blok-blobs met voorvoegsel `foo`. Deze regels zijn in het beleid gedefinieerd als:

- Laag-blob naar de cool storage 30 dagen na de laatste wijziging
- Laag blob naar archiefopslag 90 dagen na de laatste wijziging
- Het verwijderen van de blob 2,555 dagen (7 jaar) na de laatste wijziging
- Verwijder momentopnamen van de blob 90 dagen na het maken van momentopnamen

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
          "prefixMatch": [ "foo" ]
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

### <a name="rule-filters"></a>Regel-filters

Filters regelacties op een subset van de blobs in de storage-account worden beperkt. Als meerdere filters zijn gedefinieerd, een logische `AND` wordt uitgevoerd op alle filters.

Tijdens de preview omvatten geldige filters:

| Naam van het filter | Filtertype | Opmerkingen | Is vereist |
|-------------|-------------|-------|-------------|
| blobTypes   | Een matrix met vooraf gedefinieerde enum-waarden. | Voor de preview-versie, `blockBlob` wordt ondersteund. | Ja |
| prefixMatch | Een matrix met tekenreeksen voor voorvoegsels worden aan. | Als deze niet is gedefinieerd, wordt deze regel geldt voor alle blobs in het account. | Nee |

### <a name="rule-actions"></a>Regelacties

Acties worden toegepast op de gefilterde blobs wanneer de uitvoering voorwaarde wordt voldaan.

In de preview ondersteunt levenscyclusbeheer in lagen en verwijderen van blob en het verwijderen van blob-momentopnamen. Elke regel moet ten minste één actie gedefinieerd op blobs of blob momentopnamen hebben.

| Actie        | Base Blob                                   | Momentopname      |
|---------------|---------------------------------------------|---------------|
| tierToCool    | Ondersteuning voor blobs dat zich momenteel in de Hot-laag         | Niet ondersteund |
| tierToArchive | Ondersteuning voor blobs dat zich momenteel in de Hot of Cool laag | Niet ondersteund |
| verwijderen        | Ondersteund                                   | Ondersteund     |

>[!NOTE] 
Als meer dan één actie op de dezelfde blob is gedefinieerd, geldt levenscyclusbeheer van de minst dure actie naar de blob. (bijvoorbeeld actie `delete` is goedkoper dan actie `tierToArchive`. Actie `tierToArchive` is goedkoper dan actie `tierToCool`.)

Preview-versie, zijn de actie uitvoeren voorwaarden gebaseerd op leeftijd. Maakt gebruik van Base blob tijd voor het bijhouden van de leeftijd en momentopnamen gebruikt momentopname Aanmaaktijd bijhouden leeftijd blob laatst is gewijzigd.

| Actie uitvoering voorwaarde | Waarde van de voorwaarde | Beschrijving |
|----------------------------|-----------------|-------------|
| daysAfterModificationGreaterThan | Geheel getal-waarde die aangeeft van de leeftijd in dagen | Geldige voorwaarde voor base blob-acties |
| daysAfterCreationGreaterThan     | Geheel getal-waarde die aangeeft van de leeftijd in dagen | Geldige voorwaarde voor blob momentopname acties | 

## <a name="examples"></a>Voorbeelden
De volgende voorbeelden laten zien hoe u voor het oplossen van veelvoorkomende scenario's met de levenscyclus van beleidsregels.

### <a name="move-aging-data-to-a-cooler-tier"></a>Ouderdom gegevens verplaatsen naar een koelervoorbeeld laag

Het volgende voorbeeld toont hoe voor de overgang van blok-blobs voorafgegaan door `foo` of `bar`. Het beleid overgangen blobs die nog niet zijn gewijzigd in meer dan 30 dagen voor de cool storage en blobs in 90 dagen aan de laag archief niet gewijzigd:

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
            "prefixMatch": [ "foo", "bar" ]
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

### <a name="archive-data-at-ingest"></a>Gegevens archiveren op opnemen 

Weer andere gegevens in de cloud zijn inactief en worden, als ze eenmaal zijn opgeslagen, zelden tot nooit geopend. Deze gegevens is het beste worden gearchiveerd direct zodra deze wordt ingenomen. Het volgende lifecycle-beleid is geconfigureerd voor het archiveren van gegevens op opnemen. In dit voorbeeld overgangen blok-blobs in de storage-account met het voorvoegsel van `archive` direct in een archief-laag. De onmiddellijke overgang wordt gerealiseerd door fungeert op blobs 0 dagen na het tijdstip laatst gewijzigd:

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
            "prefixMatch": [ "archive" ]
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

### <a name="expire-data-based-on-age"></a>Gegevens op basis van de leeftijd verlopen

Sommige gegevens wordt verloopt dagen of maanden na het maken van een kosten te verlagen of voldoen aan wettelijke voorschriften verwacht. Een levenscyclus management-beleid kan verloopt gegevens worden ingesteld door verwijdering op basis van de leeftijd van gegevens. Het volgende voorbeeld ziet een beleid dat Hiermee verwijdert u alle blok-blobs (met geen voorvoegsel opgegeven) ouder zijn dan 365 dagen.

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

### <a name="delete-old-snapshots"></a>Verwijderen van oude momentopnamen

Voor gegevens die is gewijzigd en regelmatig gedurende hun levensduur worden geopend, worden momentopnamen vaak gebruikt voor het bijhouden van oudere versies van de gegevens. U kunt een beleid maken dat oude momentopnamen op basis van de leeftijd van de momentopname verwijdert. De leeftijd van de momentopname wordt bepaald door het evalueren van de tijd voor het maken van momentopnamen. Deze beleidsregel verwijderen blokkeren blob momentopnamen met voorvoegsel `activeData` die zijn 90 dagen of ouder nadat de momentopname is gemaakt.

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
            "prefixMatch": [ "activeData" ]
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

Meer informatie over het herstellen van gegevens na het per ongeluk verwijderen:

- [Soft voor Azure Storage-blobs verwijderen ](../blobs/storage-blob-soft-delete.md)

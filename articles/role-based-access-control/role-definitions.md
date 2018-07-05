---
title: Informatie over roldefinities in Azure RBAC | Microsoft Docs
description: Meer informatie over roldefinities in op rollen gebaseerd toegangsbeheer (RBAC) voor fijnmazig toegangsbeheer van bronnen in Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/18/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: 1d594b91b85a1bad3bbaa69bc27e62a4829a5661
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37438015"
---
# <a name="understand-role-definitions"></a>Roldefinities begrijpen

Als u probeert om te begrijpen hoe een rol werkt of als u uw eigen [aangepaste rol](custom-roles.md), is het handig om te begrijpen hoe rollen worden gedefinieerd. In dit artikel worden de details van roldefinities beschreven en vindt u enkele voorbeelden.

## <a name="role-definition-structure"></a>Structuur van beleidsdefinities rol

Een *roldefinitie* is een verzameling van machtigingen. Dit heet soms maar een *rol*. Een roldefinitie vindt u de bewerkingen die kunnen worden uitgevoerd, zoals lezen, schrijven en verwijderen. Het kan ook een lijst de bewerkingen die kunnen niet worden uitgevoerd of de bewerkingen die betrekking hebben op de onderliggende gegevens. Een roldefinitie heeft de volgende structuur:

```
assignableScopes []
description
id
name
permissions []
  actions []
  dataActions []
  notActions []
  notDataActions []
roleName
roleType
type
```

Bewerkingen zijn opgegeven met tekenreeksen die de volgende notatie hebben:

- `{Company}.{ProviderName}/{resourceType}/{action}`

De `{action}` gedeelte van een tekenreeks bewerking geeft het type van de bewerkingen die u op een resourcetype uitvoeren kunt. Bijvoorbeeld, ziet u de volgende subtekenreeksen in `{action}`:

| Actie subtekenreeks    | Beschrijving         |
| ------------------- | ------------------- |
| `*` | Het jokerteken verleent toegang tot alle bewerkingen die overeenkomen met de tekenreeks. |
| `read` | Hiermee leesbewerkingen (GET). |
| `write` | Hiermee schrijfbewerkingen (PUT, POST en PATCH). |
| `delete` | Hiermee-verwijderbewerkingen (verwijderen). |

Hier volgt de [Inzender](built-in-roles.md#contributor) roldefinitie in JSON-indeling. Het jokerteken (`*`) uitvoering bij `actions` geeft aan dat de principal die is toegewezen aan deze rol kan alle acties uitvoeren, of met andere woorden, dit alles kunt beheren. Dit omvat de acties die zijn gedefinieerd in de toekomst, zoals Azure nieuwe resourcetypen voegt. De bewerkingen onder `notActions` worden afgetrokken van `actions`. In het geval van de [Inzender](built-in-roles.md#contributor) rol `notActions` wordt verwijderd van deze rol kunnen toegang tot resources beheren en ook toegang tot resources toewijzen.

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you manage everything except access to resources.",
    "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "permissions": [
      {
        "actions": [
          "*"
        ],
        "additionalProperties": {},
        "dataActions": [],
        "notActions": [
          "Microsoft.Authorization/*/Delete",
          "Microsoft.Authorization/*/Write",
          "Microsoft.Authorization/elevateAccess/Action"
        ],
        "notDataActions": []
      }
    ],
    "roleName": "Contributor",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

## <a name="management-and-data-operations-preview"></a>Beheer en bewerkingen (Preview)

Op rollen gebaseerd toegangsbeheer voor beheerbewerkingen is opgegeven in de `actions` en `notActions` eigenschappen van een roldefinitie. Hier volgen enkele voorbeelden van bewerkingen in Azure:

- Toegang tot een storage-account beheren
- Maken, bijwerken of verwijderen van een blob-container
- Een resourcegroep en alle bijbehorende resources verwijderen

Toegang tot de is niet overgenomen met uw gegevens. Dankzij deze scheiding wordt voorkomen dat functies met jokertekens (`*`) van hebben onbeperkte toegang tot uw gegevens. Bijvoorbeeld, als een gebruiker heeft een [lezer](built-in-roles.md#reader) rol op een abonnement, klikt u vervolgens kunnen ze bekijken de storage-account, maar standaard zij de onderliggende gegevens niet weergeven.

Op rollen gebaseerd toegangsbeheer is eerder, niet gebruikt voor bewerkingen voor gegevens. Autorisatie voor gegevensbewerkingen verschillend voor resourceproviders. Het dezelfde op rollen gebaseerd autorisatie model voor toegangsbeheer gebruikt voor beheerbewerkingen is uitgebreid tot bewerkingen (momenteel in Preview-versie).

Ter ondersteuning van gegevensbewerkingen, zijn gegevenseigenschappen van nieuwe toegevoegd aan de structuur van de definitie rol. Gegevensbewerkingen zijn opgegeven in de `dataActions` en `notDataActions` eigenschappen. Door de eigenschappen van deze gegevens toe te voegen wordt de scheiding tussen het beheer en de gegevens gehandhaafd. Dit voorkomt dat huidige roltoewijzingen met jokertekens (`*`) plotseling hebben toegang tot gegevens. Hier volgen enkele bewerkingen die kunnen worden opgegeven in `dataActions` en `notDataActions`:

- Een lijst met blobs in een container lezen
- Een opslag-blob schrijven in een container
- Een bericht in een wachtrij verwijderen

Hier volgt de [gegevenslezer voor Opslagblob (Preview)](built-in-roles.md#storage-blob-data-reader-preview) roldefinitie, waaronder operations in zowel de `actions` en `dataActions` eigenschappen. Deze rol kunt u de blob-container en ook de onderliggende blob-gegevens worden gelezen.

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Allows for read access to Azure Storage blob containers and data.",
    "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
    "name": "2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
    "permissions": [
      {
        "actions": [
          "Microsoft.Storage/storageAccounts/blobServices/containers/read"
        ],
        "additionalProperties": {},
        "dataActions": [
          "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read"
        ],
        "notActions": [],
        "notDataActions": []
      }
    ],
    "roleName": "Storage Blob Data Reader (Preview)",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

Alleen de bewerkingen van de gegevens kunnen worden toegevoegd aan de `dataActions` en `notDataActions` eigenschappen. Resourceproviders identificeren welke bewerkingen zijn gegevensbewerkingen, door in te stellen de `isDataAction` eigenschap `true`. Voor een overzicht van de bewerkingen waarbij `isDataAction` is `true`, Zie [resourceproviderbewerkingen](resource-provider-operations.md). Functies die nog geen gegevensbewerkingen hoeven niet te hebben `dataActions` en `notDataActions` eigenschappen in de roldefinitie van de.

Autorisatie voor alle management bewerking API-aanroepen wordt verwerkt door Azure Resource Manager. Autorisatie voor API-aanroepen van data-bewerking wordt verwerkt door een resourceprovider of een Azure Resource Manager.

### <a name="data-operations-example"></a>Voorbeeld van de gegevens-bewerkingen

Voor meer informatie over de werking van beheer en bewerkingen, laten we eens een specifiek voorbeeld. Els is toegewezen de [eigenaar](built-in-roles.md#owner) rol op het abonnementsbereik. Bob is toegewezen de [Gegevensbijdrager voor Blob (Preview)](built-in-roles.md#storage-blob-data-contributor-preview) rol bij een storage-accountbereik. Het volgende diagram toont het volgende voorbeeld.

![Op rollen gebaseerd toegangsbeheer is uitgebreid ter ondersteuning van beheer- en bewerkingen](./media/role-definitions/rbac-management-data.png)

De [eigenaar](built-in-roles.md#owner) rol voor Els en de [Gegevensbijdrager voor Blob (Preview)](built-in-roles.md#storage-blob-data-contributor-preview) rol voor Berend hebben de volgende acties:

Eigenaar

&nbsp;&nbsp;&nbsp;&nbsp;Acties<br>
&nbsp;&nbsp;&nbsp;&nbsp;`*`

Gegevensbijdrager voor opslagblob (preview-versie)

&nbsp;&nbsp;&nbsp;&nbsp;Acties<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/delete`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/read`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/write`<br>
&nbsp;&nbsp;&nbsp;&nbsp;DataActions<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write`

Aangezien Alice een jokerteken heeft (`*`) actie op het bereik van een abonnement, haar machtigingen overnemen omlaag zodat zij alle beheeracties uitvoeren. Els kan echter niet gegevensbewerkingen uitvoeren. Bijvoorbeeld, standaard, Els de blobs in een container kan niet worden gelezen, maar ze kan lezen, schrijven en verwijderen van containers.

Berend machtigingen zijn beperkt tot alleen de `actions` en `dataActions` opgegeven in de [Gegevensbijdrager voor Blob (Preview)](built-in-roles.md#storage-blob-data-contributor-preview) rol. Op basis van de rol, kunt Bob uitvoeren beheer- en bewerkingen voor gegevens. Bijvoorbeeld, Bob kan lezen, schrijven en verwijderen van containers in het opgegeven opslagaccount en hij kan ook lezen, schrijven en verwijderen van de blobs.

### <a name="what-tools-support-using-rbac-for-data-operations"></a>Welke hulpprogramma's ondersteunen met het RBAC voor gegevensbewerkingen?

Als u wilt weergeven en werken met gegevensbewerkingen, moet u de juiste versie van de hulpprogramma's of de SDK's hebben:

| Hulpprogramma  | Versie  |
|---------|---------|
| [Azure PowerShell](/powershell/azure/install-azurerm-ps) | 5.6.0 of hoger |
| [Azure-CLI](/cli/azure/install-azure-cli) | 2.0.30 of hoger |
| [Azure voor .NET](/dotnet/azure/) | 2.8.0-Preview of hoger |
| [Azure SDK voor Go](/go/azure/azure-sdk-go-install) | 15.0.0 of hoger |
| [Azure voor Java](/java/azure/) | 1.9.0 of hoger |
| [Azure voor Python](/python/azure) | 0.40.0 of hoger |
| [Azure-SDK voor Ruby](https://rubygems.org/gems/azure_sdk) | 0.17.1 of hoger |

## <a name="actions"></a>acties

De `actions` machtiging Hiermee geeft u de beheerbewerkingen die de rol kan worden uitgevoerd. Er is een verzameling van bewerking tekenreeksen die beveiligbare bewerkingen van Azure-resource-providers identificeren. Hier volgen enkele voorbeelden van bewerkingen die kunnen worden gebruikt in `actions`.

| Bewerking tekenreeks    | Beschrijving         |
| ------------------- | ------------------- |
| `*/read` | Verleent toegang tot de leesbewerkingen voor alle resourcetypen van alle Azure-resource-providers.|
| `Microsoft.Compute/*` | Verleent toegang tot alle bewerkingen voor alle resourcetypen in de Microsoft.Compute-resourceprovider.|
| `Microsoft.Network/*/read` | Verleent toegang tot de leesbewerkingen voor alle resourcetypen in de Microsoft.Network-resourceprovider.|
| `Microsoft.Compute/virtualMachines/*` | Verleent toegang tot alle bewerkingen van virtuele machines en het onderliggende resourcetypen.|
| `microsoft.web/sites/restart/Action` | Verleent toegang tot een WebApp opnieuw te starten.|

## <a name="notactions"></a>notActions

De `notActions` machtiging Hiermee geeft u de beheerbewerkingen die zijn uitgesloten van het toegestane aantal `actions`. Gebruik de `notActions` machtiging als de reeks bewerkingen die u wilt toestaan dat eenvoudiger door uit te sluiten van beperkte bewerkingen is gedefinieerd. De toegang wordt verleend door een rol (effectieve machtigingen) wordt berekend door af te trekken van de `notActions` bewerkingen uit de `actions` bewerkingen.

> [!NOTE]
> Als een gebruiker een rol die niet van toepassing op een bewerking in toegewezen `notActions`, en een tweede functie die toegang tot dezelfde bewerking verleent, de gebruiker is toegestaan voor deze bewerking is toegewezen. `notActions` is niet een weigeren regel – het is gewoon een handige manier om u te maken van een set toegestane bewerkingen wanneer specifieke bewerkingen die moeten worden uitgesloten.
>

## <a name="dataactions-preview"></a>dataActions (Preview)

De `dataActions` machtiging Hiermee geeft u de bewerkingen die de rol kan worden uitgevoerd met uw gegevens binnen dat object. Bijvoorbeeld, als een gebruiker toegang tot blob-gegevens in een storage-account lezen heeft, kan deze lezen de blobs in het storage-account. Hier volgen enkele voorbeelden van bewerkingen voor gegevens die kunnen worden gebruikt in `dataActions`.

| Bewerking tekenreeks    | Beschrijving         |
| ------------------- | ------------------- |
| `Microsoft.Storage/storageAccounts/ blobServices/containers/blobs/read` | Retourneert een blob of een lijst met blobs. |
| `Microsoft.Storage/storageAccounts/ blobServices/containers/blobs/write` | Retourneert het resultaat van het schrijven van een blob. |
| `Microsoft.Storage/storageAccounts/ queueServices/queues/messages/read` | Retourneert het bericht. |
| `Microsoft.Storage/storageAccounts/ queueServices/queues/messages/*` | Retourneert een bericht of het resultaat van schrijven of verwijderen van een bericht. |

## <a name="notdataactions-preview"></a>notDataActions (Preview)

De `notDataActions` machtiging Hiermee geeft u de bewerkingen voor gegevens die zijn uitgesloten van het toegestane aantal `dataActions`. De toegang wordt verleend door een rol (effectieve machtigingen) wordt berekend door af te trekken van de `notDataActions` bewerkingen uit de `dataActions` bewerkingen. Elke resourceprovider biedt een bijbehorende set API's om te voldoen aan gegevensbewerkingen.

> [!NOTE]
> Als een gebruiker een rol die niet van toepassing op een gegevensbewerking in toegewezen `notDataActions`, en een tweede functie die toegang tot de dezelfde gegevensbewerking is uitgevoerd verleent, de gebruiker is toegestaan om uit te voeren die voor deze gegevensbewerking is toegewezen. `notDataActions` is niet een weigeren regel – het is gewoon een handige manier om u te maken van een set toegestane gegevensbewerkingen wanneer specifieke gegevensbewerkingen moeten worden uitgesloten.
>

## <a name="assignablescopes"></a>assignableScopes

De `assignableScopes` -eigenschap geeft de scopes (beheergroepen (die momenteel in preview), abonnementen, resourcegroepen of resources) dat de functie beschikbaar voor toewijzing is. U kunt de rol beschikbaar maken voor toewijzing in alleen de abonnementen of resourcegroepen die nodig, en niet de gebruiker overbodige items hebben ervaring voor de rest van de abonnementen of resourcegroepen. Moet u ten minste één management groep, abonnement, resourcegroep of resource-ID.

Ingebouwde rollen hebben een `assignableScopes` ingesteld op het bereik van de hoofdmap (`"/"`). Het bereik van de hoofdmap geeft aan dat de functie beschikbaar voor toewijzing in alle bereiken is. Voorbeelden van geldige toewijsbare bereiken zijn:

| Scenario | Voorbeeld |
|----------|---------|
| Functie is beschikbaar voor toewijzing in één abonnement | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"` |
| Functie is beschikbaar voor toewijzing in twee abonnementen | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"` |
| Functie is beschikbaar voor toewijzing alleen in de netwerk-resourcegroep | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network"` |
| Functie is beschikbaar voor toewijzing in alle bereiken | `"/"` |

Voor informatie over `assignableScopes` voor aangepaste rollen, Zie [aangepaste rollen](custom-roles.md).

## <a name="next-steps"></a>Volgende stappen

* [Ingebouwde rollen](built-in-roles.md)
* [Aangepaste rollen](custom-roles.md)
* [Azure Resource Manager-resourceproviderbewerkingen](resource-provider-operations.md)

---
title: Roldefinities in Azure RBAC begrijpen | Microsoft Docs
description: Meer informatie over roldefinities in op rollen gebaseerde toegangsbeheer (RBAC) en het definiëren van aangepaste rollen voor Geavanceerd toegangsbeheer van bronnen in Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/18/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.custom: ''
ms.openlocfilehash: cb6bb5ed80f08941b872e6fabeb8bb150a21dede
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34640384"
---
# <a name="understand-role-definitions"></a>Roldefinities begrijpen

Als u probeert te begrijpen hoe een rol werkt of als u uw eigen [aangepaste rol](custom-roles.md), is het handig om te begrijpen hoe de functies zijn gedefinieerd. Dit artikel worden de details van roldefinities beschreven en vindt u enkele voorbeelden.

## <a name="role-definition-structure"></a>Rol definitie structuur

Een *roldefinitie* is een verzameling van machtigingen. Heet soms een *rol*. Een roldefinitie vindt u de bewerkingen die kunnen worden uitgevoerd, zoals lezen, schrijven en verwijderen. Het kan ook de bewerkingen die kunnen niet worden uitgevoerd of de bewerkingen die betrekking hebben op de onderliggende gegevens weergeven. Een roldefinitie heeft de volgende structuur:

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

- `Microsoft.{ProviderName}/{ChildResourceType}/{action}`

De `{action}` deel van een tekenreeks bewerking geeft het type kunt u bewerkingen op een resourcetype uitvoeren. Bijvoorbeeld, ziet u de volgende subtekenreeksen in `{action}`:

| De subtekenreeks actie    | Beschrijving         |
| ------------------- | ------------------- |
| `*` | Het jokerteken verleent toegang tot alle bewerkingen die overeenkomen met de tekenreeks. |
| `read` | Hiermee leesbewerkingen (GET). |
| `write` | Hiermee schrijfbewerkingen (PUT, POST en PATCH). |
| `delete` | Hiermee verwijderbewerkingen (verwijderen). |

Hier volgt de [Inzender](built-in-roles.md#contributor) roldefinitie in JSON-indeling. Het jokerteken (`*`) bewerking onder `actions` geeft aan dat de principal die is toegewezen aan deze rol kan alle acties uitvoeren, of met andere woorden, het kan alles beheren. Dit omvat de acties die zijn gedefinieerd in de toekomst, zoals Azure nieuwe brontypen voegt. De bewerkingen onder `notActions` worden afgetrokken van `actions`. In het geval van de [Inzender](built-in-roles.md#contributor) rol, `notActions` Hiermee verwijdert u deze rol kunnen toegang tot resources beheren en wijs ook toegang tot bronnen.

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

## <a name="management-and-data-operations-preview"></a>Bewerkingen voor beheer en (Preview)

Op rollen gebaseerde toegangsbeheer voor beheertaken uit te voeren is opgegeven in de `actions` en `notActions` secties van een roldefinitie. Hier volgen enkele voorbeelden van beheerbewerkingen in Azure:

- Toegang tot een opslagaccount beheren
- Maken, bijwerken of verwijderen van een blob-container
- Een resourcegroep en alle bijbehorende resources verwijderen

Beheertoegang wordt niet overgenomen tot uw gegevens. Deze scheiding wordt voorkomen dat functies met jokertekens (`*`) van hebben onbeperkte toegang tot uw gegevens. Als een gebruiker heeft bijvoorbeeld een [lezer](built-in-roles.md#reader) rol op een abonnement, vervolgens kunnen ze bekijken het storage-account, maar standaard zij de onderliggende gegevens niet weergeven.

Toegangsbeheer op basis van rollen is eerder niet gebruikt voor gegevensbewerkingen. Autorisatie voor gegevensbewerkingen verspreid over resourceproviders. De dezelfde op rollen gebaseerde autorisatie model voor toegangsbeheer gebruikt voor beheerbewerkingen uitgebreid naar gegevensbewerkingen (momenteel in preview).

Ter ondersteuning van gegevensbewerkingen zijn nieuwe gegevenssecties toegevoegd aan de rol definitie-structuur. Gegevensbewerkingen zijn opgegeven in de `dataActions` en `notDataActions` secties. Door deze gegevenssecties toe te voegen wordt de scheiding tussen het beheer en de gegevens gehandhaafd. Dit voorkomt dat huidige roltoewijzingen met jokertekens (`*`) van het plotseling hebben toegang tot gegevens. Hier volgen enkele gegevensbewerkingen die kunnen worden opgegeven in `dataActions` en `notDataActions`:

- Een lijst met blobs in een container lezen
- Schrijven van een blob storage in een container
- Een bericht in een wachtrij verwijderen

Hier volgt de [gegevenslezer Storage-Blob (Preview)](built-in-roles.md#storage-blob-data-reader-preview) roldefinitie, waaronder operations in zowel de `actions` en `dataActions` secties. Deze functie kunt u de blob-container en ook de onderliggende blobgegevens worden gelezen.

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

Alleen de bewerkingen van de gegevens kunnen worden toegevoegd aan de `dataActions` en `notDataActions` secties. Resourceproviders identificeren welke bewerkingen zijn bewerkingen, door in te stellen de `isDataAction` eigenschap `true`. Voor een overzicht van de bewerkingen waarbij `isDataAction` is `true`, Zie [Resource provider operations](resource-provider-operations.md). Functies die u geen gegevensbewerkingen hebt hoeven niet te hebben `dataActions` en `notDataActions` secties binnen de functiedefinitie.

Autorisatie voor alle management bewerking API-aanroepen wordt verwerkt door Azure Resource Manager. Autorisatie voor API-aanroepen voor gegevens-bewerking wordt uitgevoerd door een resourceprovider of een Azure Resource Manager.

### <a name="data-operations-example"></a>Voorbeeld van de bewerkingen gegevens

Voor meer informatie over de werking van beheer- en bewerkingen, laten we eens een voorbeeld van een specifieke. Els is toegewezen de [eigenaar](built-in-roles.md#owner) rol bij het abonnementsbereik. Bob is toegewezen de [Storage Blob Inzender (Preview)](built-in-roles.md#storage-blob-data-contributor-preview) rol op een scope storage-account. Het volgende diagram toont het volgende voorbeeld.

![Toegangsbeheer op basis van rollen ter ondersteuning van zowel management en gegevensbewerkingen uitgebreid](./media/role-definitions/rbac-management-data.png)

De [eigenaar](built-in-roles.md#owner) rol voor Els en de [Storage Blob Inzender (Preview)](built-in-roles.md#storage-blob-data-contributor-preview) voor Berend beschikt u over de volgende acties:

Eigenaar

&nbsp;&nbsp;&nbsp;&nbsp;acties<br>
&nbsp;&nbsp;&nbsp;&nbsp;`*`

Gegevensbijdrager voor opslagblob (preview-versie)

&nbsp;&nbsp;&nbsp;&nbsp;acties<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/delete`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/read`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/write`<br>
&nbsp;&nbsp;&nbsp;&nbsp;DataActions<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write`

Aangezien Els een jokerteken heeft (`*`) actie op een scope abonnement haar machtigingen overnemen omlaag zodat ze alle acties uitvoeren. Els kan echter niet gegevensbewerkingen uitvoeren. Bijvoorbeeld: standaard Els de blobs in een container kan niet worden gelezen, maar ze kan lezen, schrijven en verwijderen van containers.

Berend machtigingen zijn beperkt tot alleen de `actions` en `dataActions` opgegeven in de [Storage Blob Inzender (Preview)](built-in-roles.md#storage-blob-data-contributor-preview) rol. Op basis van de rol, kunt Bob uitvoeren van beheer en gegevensbewerkingen. Bijvoorbeeld: Bob kunt lezen, schrijven en verwijderen van containers in het opgegeven opslagaccount en hij kan ook lezen, schrijven en verwijderen van de blobs.

### <a name="what-tools-support-using-rbac-for-data-operations"></a>Welke hulpprogramma's voor ondersteuning voor het gebruik van RBAC voor gegevensbewerkingen?

Als u wilt weergeven en werken met gegevensbewerkingen, hebt u de juiste versies van de hulpprogramma's of de SDK's:

| Hulpprogramma  | Versie  |
|---------|---------|
| [Azure PowerShell](/powershell/azure/install-azurerm-ps) | 5.6.0 of hoger |
| [Azure-CLI](/cli/azure/install-azure-cli) | 2.0.30 of hoger |
| [Azure voor .NET](/dotnet/azure/) | 2.8.0-Preview of hoger |
| [Azure SDK voor Ga](/go/azure/azure-sdk-go-install) | 15.0.0 of hoger |
| [Azure voor Java](/java/azure/) | 1.9.0 of hoger |
| [Azure voor Python](/python/azure) | 0.40.0 of hoger |
| [Azure-SDK voor Ruby](https://rubygems.org/gems/azure_sdk) | 0.17.1 of hoger |

## <a name="actions"></a>acties

De `actions` toestemming geeft de beheerbewerkingen die de rol toegang verleent. Er is een verzameling van bewerking tekenreeksen waarmee beveiligbare bewerkingen van de Azure-resourceproviders. Hier volgen enkele voorbeelden van de beheerbewerkingen die kunnen worden gebruikt in `actions`.

| Bewerking tekenreeks    | Beschrijving         |
| ------------------- | ------------------- |
| `*/read` | verleent toegang tot leesbewerkingen voor alle resourcetypen van alle Azure-resourceproviders.|
| `Microsoft.Compute/*` | verleent toegang tot alle bewerkingen voor alle resourcetypen in de Microsoft.Compute-resourceprovider.|
| `Microsoft.Network/*/read` | Verleent toegang tot leesbewerkingen voor alle resourcetypen in de Microsoft.Network-resourceprovider.|
| `Microsoft.Compute/virtualMachines/*` | verleent toegang tot alle bewerkingen van virtuele machines en het onderliggende resourcetypen.|
| `microsoft.web/sites/restart/Action` | Verleent toegang tot het starten van een web-app.|

## <a name="notactions"></a>NotActions

De `notActions` toestemming geeft de beheerbewerkingen die zijn uitgesloten van de toegestane `actions`. Gebruik de `notActions` machtiging als een reeks bewerkingen die u wilt toestaan dat eenvoudiger wordt gedefinieerd door het met uitzondering van beperkte bewerkingen. De toegang wordt verleend door een rol (effectieve machtigingen) wordt berekend door af te trekken de `notActions` bewerkingen van de `actions` bewerkingen.

> [!NOTE]
> Als een gebruiker een rol heeft die worden uitgesloten van een bewerking in toegewezen `notActions`, en een tweede rol die toegang tot dezelfde bewerking verleent, de gebruiker is toegestaan voor deze bewerking is toegewezen. `notActions` is geen weigeren regel – het is gewoon een handige manier voor het maken van een set toegestane bewerkingen wanneer specifieke bewerkingen moeten worden uitgesloten.
>

## <a name="dataactions-preview"></a>dataActions (Preview)

De `dataActions` machtiging Hiermee worden de gegevensbewerkingen waarvoor de functie toegang tot uw gegevens binnen dit object verleent. Bijvoorbeeld, als een gebruiker toegang tot blob-gegevens in een opslagaccount leesmachtigingen heeft, vervolgens kan worden gelezen de blobs binnen dit opslagaccount. Hier volgen enkele voorbeelden van gegevensbewerkingen die kunnen worden gebruikt in `dataActions`.

| Bewerking tekenreeks    | Beschrijving         |
| ------------------- | ------------------- |
| `Microsoft.Storage/storageAccounts/ blobServices/containers/blobs/read` | Retourneert een blob of een lijst met blobs. |
| `Microsoft.Storage/storageAccounts/ blobServices/containers/blobs/write` | Retourneert het resultaat van het schrijven van een blob. |
| `Microsoft.Storage/storageAccounts/ queueServices/queues/messages/read` | Retourneert een bericht. |
| `Microsoft.Storage/storageAccounts/ queueServices/queues/messages/*` | Retourneert een bericht of het resultaat van schrijven of verwijderen van een bericht. |

## <a name="notdataactions-preview"></a>notDataActions (Preview)

De `notDataActions` machtiging Hiermee worden de gegevensbewerkingen die zijn uitgesloten van de toegestane `dataActions`. De toegang wordt verleend door een rol (effectieve machtigingen) wordt berekend door af te trekken de `notDataActions` bewerkingen van de `dataActions` bewerkingen. Elke resourceprovider biedt hun respectieve reeks API's om te voldoen aan gegevensbewerkingen.

> [!NOTE]
> Als een gebruiker een rol heeft die worden uitgesloten van een gegevensbewerking in toegewezen `notDataActions`, en een tweede rol die toegang tot de gegevensbewerking met dezelfde verleent, de gebruiker is toegestaan om uit te voeren die voor deze gegevensbewerking is toegewezen. `notDataActions` is geen weigeren regel – het is gewoon een handige manier voor het maken van een set toegestane gegevensbewerkingen wanneer specifieke gegevensbewerkingen moeten worden uitgesloten.
>

## <a name="assignablescopes"></a>AssignableScopes

De `assignableScopes` sectie geeft de scopes (abonnementen beheergroepen (momenteel in preview) resourcegroepen of resources) dat de rol beschikbaar voor toewijzing is. U kunt de functie beschikbaar voor toewijzing in alleen de abonnementen of resourcegroepen waarvoor en niet de gebruiker vol ervaring voor de rest van de abonnementen of resourcegroepen. Moet u minstens één groep, abonnement, resourcegroep of resource-ID.

Ingebouwde rollen `assignableScopes` ingesteld op het root-bereik (`"/"`). Het bereik van de hoofdmap wordt aangegeven dat de rol beschikbaar voor toewijzing in alle bereiken. U kunt het bereik van de hoofdmap niet gebruiken in uw eigen aangepaste rollen. Als u probeert, ontvangt u een Autorisatiefout.

Voorbeelden van geldige toewijsbare bereiken zijn:

| Scenario | Voorbeeld |
|----------|---------|
| Functie is beschikbaar voor toewijzing in één abonnement | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"` |
| Functie is beschikbaar voor toewijzing in twee abonnementen | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"` |
| Functie is beschikbaar voor toewijzing alleen in de resourcegroep netwerk | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network"` |
| Functie is beschikbaar voor toewijzing in alle scopes | `"/"` |

## <a name="assignablescopes-and-custom-roles"></a>assignableScopes en aangepaste rollen

De `assignableScopes` sectie voor een aangepaste beveiligingsrol ook besturingselementen die kunt maken, verwijderen, wijzigen of de aangepaste rol weergeven.

| Taak | Bewerking | Beschrijving |
| --- | --- | --- |
| Een aangepaste rol maken/verwijderen | `Microsoft.Authorization/ roleDefinition/write` | Gebruikers die deze bewerking op alle krijgen de `assignableScopes` van de aangepaste rol kunt maken (of verwijderen) aangepaste rollen voor gebruik in deze bereiken. Bijvoorbeeld: [eigenaars](built-in-roles.md#owner) en [beheerders van de gebruiker toegang](built-in-roles.md#user-access-administrator) van abonnementen, resourcegroepen en resources. |
| Een aangepaste rol wijzigen | `Microsoft.Authorization/ roleDefinition/write` | Gebruikers die deze bewerking op alle krijgen de `assignableScopes` van de aangepaste rol aangepaste rollen in deze bereiken kunt wijzigen. Bijvoorbeeld: [eigenaars](built-in-roles.md#owner) en [beheerders van de gebruiker toegang](built-in-roles.md#user-access-administrator) van abonnementen, resourcegroepen en resources. |
| Een aangepaste beveiligingsrol weergeven | `Microsoft.Authorization/ roleDefinition/read` | Gebruikers die deze bewerking op een scope worden verleend, kunnen de aangepaste rollen die beschikbaar voor toewijzing op dat bereik zijn bekijken. Alle ingebouwde rollen toestaan aangepaste rollen beschikbaar voor toewijzing. |

## <a name="role-definition-examples"></a>Voorbeelden van functie-definitie

Het volgende voorbeeld wordt de [lezer](built-in-roles.md#reader) roldefinitie zoals weergegeven met de Azure CLI:

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you view everything, but not make any changes.",
    "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "name": "acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "permissions": [
      {
        "actions": [
          "*/read"
        ],
        "additionalProperties": {},
        "dataActions": [],
        "notActions": [],
        "notDataActions": []
      }
    ],
    "roleName": "Reader",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

Het volgende voorbeeld ziet u een aangepaste rol voor het controleren en opnieuw starten van virtuele machines met Azure PowerShell weergegeven:

```json
{
  "Name":  "Virtual Machine Operator",
  "Id":  "88888888-8888-8888-8888-888888888888",
  "IsCustom":  true,
  "Description":  "Can monitor and restart virtual machines.",
  "Actions":  [
                  "Microsoft.Storage/*/read",
                  "Microsoft.Network/*/read",
                  "Microsoft.Compute/*/read",
                  "Microsoft.Compute/virtualMachines/start/action",
                  "Microsoft.Compute/virtualMachines/restart/action",
                  "Microsoft.Authorization/*/read",
                  "Microsoft.Resources/subscriptions/resourceGroups/read",
                  "Microsoft.Insights/alertRules/*",
                  "Microsoft.Insights/diagnosticSettings/*",
                  "Microsoft.Support/*"
  ],
  "NotActions":  [

                 ],
  "DataActions":  [

                  ],
  "NotDataActions":  [

                     ],
  "AssignableScopes":  [
                           "/subscriptions/{subscriptionId1}",
                           "/subscriptions/{subscriptionId2}",
                           "/subscriptions/{subscriptionId3}"
                       ]
}
```

## <a name="see-also"></a>Zie ook

* [Ingebouwde rollen](built-in-roles.md)
* [Aangepaste rollen](custom-roles.md)
* [Azure Resource Manager resource provider-bewerkingen](resource-provider-operations.md)

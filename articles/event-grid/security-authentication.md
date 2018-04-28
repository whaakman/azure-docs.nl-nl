---
title: Azure Event raster beveiligings- en -verificatie
description: Beschrijving van Azure Event raster en de concepten.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 03/15/2018
ms.author: babanisa
ms.openlocfilehash: 4b9ab8aaef091573d204b8de58115cc03707aa01
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="event-grid-security-and-authentication"></a>Gebeurtenis raster beveiligings- en -verificatie 

Azure Event raster heeft drie soorten verificatie:

* Gebeurtenisabonnementen
* Gebeurtenis publiceren
* WebHook gebeurtenis levering

## <a name="webhook-event-delivery"></a>WebHook gebeurtenis levering

Webhooks zijn veel manieren voor het ontvangen van gebeurtenissen uit Azure Event raster. Wanneer een nieuwe gebeurtenis klaar is, wordt in de gebeurtenis raster Webhook een HTTP-aanvraag verzendt naar de geconfigureerde HTTP-eindpunt met de gebeurtenis in de hoofdtekst.

Wanneer u uw eigen WebHook-eindpunt met raster gebeurtenis registreert, stuurt u een POST-aanvraag met een eenvoudige validatiecode om te bewijzen eindpunt eigendom. Uw app moet reageren door echo terug code voor de validatie. Gebeurtenis raster leveren niet gebeurtenissen aan WebHook-eindpunten die nog niet gevalideerd.

### <a name="validation-details"></a>Validatie-informatie

* Op het moment van gebeurtenis abonnement maken of bij te werken boekt raster gebeurtenis een gebeurtenis 'SubscriptionValidationEvent' naar het doel-eindpunt.
* De gebeurtenis bevat een waarde voor header 'Aeg gebeurtenistype: SubscriptionValidation'.
* De hoofdtekst van de gebeurtenis heeft hetzelfde schema als andere gebeurtenissen gebeurtenis raster.
* Gegevens van de gebeurtenis bevat de eigenschap 'validationCode' met een willekeurige tekenreeks. Bijvoorbeeld ' validationCode: acb13... '.
* De matrix bevat alleen de validatiegebeurtenis. Andere gebeurtenissen worden verzonden in een afzonderlijke aanvraag nadat u echo teruggestuurd code voor de validatie.

Een voorbeeld SubscriptionValidationEvent wordt weergegeven in het volgende voorbeeld:

```json
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2018-01-25T22:12:19.4556811Z",
  "metadataVersion": "1",
  "dataVersion": "1"
}]
```

Om te bewijzen dat eindpunt eigendom, echo teruggestuurd de validatiecode in de eigenschap validationResponse, zoals wordt weergegeven in het volgende voorbeeld:

```json
{
  "validationResponse": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
}
```
### <a name="event-delivery-security"></a>Gebeurtenis levering van beveiliging

U kunt uw eindpunt webhook beveiligen door queryparameters toevoegen aan de webhook-URL bij het maken van een gebeurtenisabonnement. Stel één van deze queryparameters moeten een geheim, zoals een [toegangstoken](https://en.wikipedia.org/wiki/Access_token) dat de webhook kunt gebruiken voor het herkennen van de gebeurtenis afkomstig is van de gebeurtenis raster met geldige machtigingen. Gebeurtenis raster neemt deze queryparameters in elke gebeurtenis levering aan de webhook.

Bij het bewerken van het abonnement van de gebeurtenis, de queryparameters wordt niet weergegeven of geretourneerd, tenzij de [--opnemen-volledige-eindpunt-url](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_show) parameter wordt gebruikt in Azure [CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest).

Ten slotte is het belangrijk te weten dat Azure gebeurtenis raster biedt alleen ondersteuning voor HTTPS-webhook-eindpunten.

## <a name="event-subscription"></a>Voor een gebeurtenisabonnement

Abonneren op een gebeurtenis, hebt u de **Microsoft.EventGrid/EventSubscriptions/Write** toegang tot de vereiste bron. U moet deze machtiging omdat u bij het schrijven van een nieuw abonnement op het bereik van de resource. De vereiste bron verschilt op basis van of u op een systeemonderwerp of aangepaste onderwerp abonneren. Beide typen worden in deze sectie beschreven.

### <a name="system-topics-azure-service-publishers"></a>Systeemonderwerpen (uitgevers Azure-service)

Voor systeemonderwerpen moet u machtigingen voor schrijven van een nieuw gebeurtenisabonnement bij het bereik van de bron voor het publiceren van de gebeurtenis. De indeling van de resource is: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

Bijvoorbeeld, om u te abonneren op een gebeurtenis op een opslagaccount met de naam **MIJNACCT**, moet u de machtiging Microsoft.EventGrid/EventSubscriptions/Write op: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`

### <a name="custom-topics"></a>Aangepaste-onderwerpen

Voor aangepaste onderwerpen moet u machtigingen voor schrijven van een nieuw gebeurtenisabonnement bij het bereik van het onderwerp raster. De indeling van de resource is: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

Bijvoorbeeld, om u te abonneren op een eigen onderwerp met de naam **mytopic**, moet u de machtiging Microsoft.EventGrid/EventSubscriptions/Write op: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`

## <a name="topic-publishing"></a>Onderwerp publiceren

Onderwerpen over Shared Access Signature (SAS) of verificatie met sleutel gebruiken. SAS wordt aangeraden, maar sleutelverificatie biedt eenvoudige programmering en is compatibel met veel bestaande webhook uitgevers. 

De verificatie-waarde in de HTTP-header te nemen. Gebruik voor SAS, **aeg-sas-token** voor de headerwaarde. Gebruik voor sleutelverificatie **aeg-sas-sleutel** voor de headerwaarde.

### <a name="key-authentication"></a>Verificatie met sleutel

Verificatie met een sleutel is de eenvoudigste vorm van verificatie. Gebruik de volgende indeling: `aeg-sas-key: <your key>`

Bijvoorbeeld het doorgeven van een sleutel met:

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>SAS-tokens

SAS-tokens voor gebeurtenis raster zijn de resource een verlooptijd en een handtekening. De indeling van het SAS-token is: `r={resource}&e={expiration}&s={signature}`.

De resource is het pad voor het raster onderwerp waarnaar u gebeurtenissen verzendt. Een geldige resource-pad is bijvoorbeeld: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`

U de handtekening van een sleutel gegenereerd.

Bijvoorbeeld, een geldige **aeg-sas-token** waarde is:

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
```

Het volgende voorbeeld wordt een SAS-token voor gebruik met gebeurtenis raster:

```cs
static string BuildSharedAccessSignature(string resource, DateTime expirationUtc, string key)
{
    const char Resource = 'r';
    const char Expiration = 'e';
    const char Signature = 's';

    string encodedResource = HttpUtility.UrlEncode(resource);
    var culture = CultureInfo.CreateSpecificCulture("en-US");
    var encodedExpirationUtc = HttpUtility.UrlEncode(expirationUtc.ToString(culture));

    string unsignedSas = $"{Resource}={encodedResource}&{Expiration}={encodedExpirationUtc}";
    using (var hmac = new HMACSHA256(Convert.FromBase64String(key)))
    {
        string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(unsignedSas)));
        string encodedSignature = HttpUtility.UrlEncode(signature);
        string signedSas = $"{unsignedSas}&{Signature}={encodedSignature}";

        return signedSas;
    }
}
```

## <a name="management-access-control"></a>Toegangsbeheer voor beheer

Azure Event raster kunt u bepalen het niveau van toegang krijgen tot andere gebruikers kunnen verschillende bewerkingen zoals de lijst met abonnementen doen, maakt u nieuwe en genereren van sleutels. Gebeurtenis raster maakt gebruik van Azure toegang controleren RBAC (Role Based).

### <a name="operation-types"></a>Bewerkingstypen

Azure event raster ondersteunt de volgende acties:

* Microsoft.EventGrid/*/read
* Microsoft.EventGrid/*/write
* Microsoft.EventGrid/*/delete
* Microsoft.EventGrid/eventSubscriptions/getFullUrl/action
* Microsoft.EventGrid/topics/listKeys/action
* Microsoft.EventGrid/topics/regenerateKey/action

De laatste drie bewerkingen retourneren mogelijk geheime informatie opgehaald gefilterd buiten het normale leesbewerkingen. Het is best practice voor u toegang tot deze bewerkingen te beperken. Aangepaste rollen kunnen worden gemaakt met [Azure PowerShell](../role-based-access-control/role-assignments-powershell.md), [Azure-opdrachtregelinterface (CLI)](../role-based-access-control/role-assignments-cli.md), en de [REST-API](../role-based-access-control/role-assignments-rest.md).

### <a name="enforcing-role-based-access-check-rbac"></a>Afdwingen van de rol op basis van de toegangscontrole (RBAC)

Gebruik de volgende stappen uit om af te dwingen RBAC voor verschillende gebruikers:

#### <a name="create-a-custom-role-definition-file-json"></a>Maken van een aangepaste rol-definitiebestand (.json)

Hieronder vindt u voorbeeld gebeurtenis raster roldefinities waarmee gebruikers op verschillende sets van acties uitvoeren.

**EventGridReadOnlyRole.json**: alleen alleen-lezen bewerkingen toe te staan.

```json
{
  "Name": "Event grid read only role",
  "Id": "7C0B6B59-A278-4B62-BA19-411B70753856",
  "IsCustom": true,
  "Description": "Event grid read only role",
  "Actions": [
    "Microsoft.EventGrid/*/read"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription Id>"
  ]
}
```

**EventGridNoDeleteListKeysRole.json**: toestaan beperkte post acties maar verwijderingsacties weigeren.

```json
{
  "Name": "Event grid No Delete Listkeys role",
  "Id": "B9170838-5F9D-4103-A1DE-60496F7C9174",
  "IsCustom": true,
  "Description": "Event grid No Delete Listkeys role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action"
  ],
  "NotActions": [
    "Microsoft.EventGrid/*/delete"
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

**EventGridContributorRole.json**: kunnen alle gebeurtenis raster acties.

```json
{
  "Name": "Event grid contributor role",
  "Id": "4BA6FB33-2955-491B-A74F-53C9126C9514",
  "IsCustom": true,
  "Description": "Event grid contributor role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/*/delete",
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

#### <a name="create-and-assign-custom-role-with-azure-cli"></a>Maken en toewijzen van aangepaste rol met Azure CLI

Gebruik het volgende voor het maken van een aangepaste rol:

```azurecli
az role definition create --role-definition @<file path>
```

Als u wilt de rol toewijst aan een gebruiker, gebruiken:

```azurecli
az role assignment create --assignee <user name> --role "<name of role>"
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot gebeurtenis raster, [over gebeurtenis raster](overview.md)

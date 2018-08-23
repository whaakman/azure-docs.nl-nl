---
title: Azure Event Grid resource group-gebeurtenisschema
description: Beschrijft de eigenschappen die beschikbaar zijn voor de resource-group-gebeurtenissen met Azure Event Grid
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: reference
ms.date: 08/17/2018
ms.author: tomfitz
ms.openlocfilehash: 22629ba553cc58435f99ed0fed97be252b24b409
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "42058727"
---
# <a name="azure-event-grid-event-schema-for-resource-groups"></a>Azure Event Grid-gebeurtenisschema voor resourcegroepen

Dit artikel bevat de eigenschappen en het schema voor de resource-group-gebeurtenissen. Zie voor een inleiding tot gebeurtenisschema's, [Azure Event Grid-gebeurtenisschema](event-schema.md).

Azure-abonnementen en resourcegroepen uitzenden van hetzelfde type als de gebeurtenis. De typen gebeurtenissen zijn die betrekking hebben op wijzigingen in de resources. Het belangrijkste verschil is dat resourcegroepen-gebeurtenissen voor resources binnen de resourcegroep verzenden en Azure-abonnementen verzenden van gebeurtenissen voor resources in het abonnement.

Resource-gebeurtenissen worden gemaakt voor PUT, PATCH, en verwijderen van bewerkingen die worden verzonden naar `management.azure.com`. Maak geen gebeurtenissen POST als GET-bewerkingen. Bewerkingen die worden verzonden naar het vlak van gegevens (zoals `myaccount.blob.core.windows.net`) Maak geen gebeurtenissen.

Wanneer u zich op gebeurtenissen voor een resourcegroep abonneert, ontvangt uw eindpunt alle gebeurtenissen voor die resourcegroep. De gebeurtenissen kunnen gebeurtenis die u wilt zien, zoals het bijwerken van een virtuele machine, maar ook gebeurtenissen die misschien niet belangrijk voor u, zoals het schrijven van een nieuwe vermelding in de geschiedenis van de implementatie bevatten. U kunt alle gebeurtenissen ontvangen op uw eindpunt en schrijven van code die verwerkt de gebeurtenissen die u wilt verwerken, of kunt u een filter instellen bij het maken van het gebeurtenisabonnement.

Via een programma om gebeurtenissen te verwerken, kunt u gebeurtenissen sorteren door te kijken de `operationName` waarde. Bijvoorbeeld, het eindpunt van de gebeurtenis kan alleen gebeurtenissen worden verwerkt voor bewerkingen die gelijk zijn aan `Microsoft.Compute/virtualMachines/write` of `Microsoft.Storage/storageAccounts/write`.

Het onderwerp van de gebeurtenis is de resource-ID van de resource die het doel van de bewerking. Als u wilt filteren van gebeurtenissen voor een resource, bieden die resource-ID bij het maken van het gebeurtenisabonnement.  Als u wilt filteren op een resourcetype, gebruik een waarde in de volgende indeling: `/subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.Compute/virtualMachines`

Zie voor een lijst met zelfstudies en voorbeelden van scripts, [Resource group-gebeurtenisbron](event-sources.md#resource-groups).

## <a name="available-event-types"></a>Typen van de gebeurtenis berichten beschikbaar

Resourcegroepen introduceren management gebeurtenissen van Azure Resource Manager, zoals wanneer een virtuele machine wordt gemaakt of een storage-account wordt verwijderd.

| Gebeurtenistype | Beschrijving |
| ---------- | ----------- |
| Microsoft.Resources.ResourceWriteSuccess | Deze gebeurtenis treedt op slaagt wanneer een resource maken of bijwerken van de bewerking. |
| Microsoft.Resources.ResourceWriteFailure | Treedt op wanneer een resource maken of update-bewerking is mislukt. |
| Microsoft.Resources.ResourceWriteCancel | Deze gebeurtenis treedt op is wanneer een resource maken of bijwerken van de bewerking geannuleerd. |
| Microsoft.Resources.ResourceDeleteSuccess | Treedt op wanneer een resource delete-bewerking is geslaagd. |
| Microsoft.Resources.ResourceDeleteFailure | Treedt op wanneer een resource delete-bewerking is mislukt. |
| Microsoft.Resources.ResourceDeleteCancel | Treedt op wanneer een resource delete-bewerking is geannuleerd. Deze gebeurtenis treedt op wanneer de sjabloonimplementatie van een is geannuleerd. |

## <a name="example-event"></a>Voorbeeld van de gebeurtenis

Het volgende voorbeeld ziet u het schema voor een **ResourceWriteSuccess** gebeurtenis. Hetzelfde schema wordt gebruikt voor **ResourceWriteFailure** en **ResourceWriteCancel** gebeurtenissen met verschillende waarden voor `eventType`.

```json
[{
  "subject": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
  "eventType": "Microsoft.Resources.ResourceWriteSuccess",
  "eventTime": "2018-07-19T18:38:04.6117357Z",
  "id": "4db48cba-50a2-455a-93b4-de41a3b5b7f6",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
      "action": "Microsoft.Storage/storageAccounts/write",
      "evidence": {
        "role": "Subscription Admin"
      }
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "_claim_names": "{\"groups\":\"src1\"}",
      "_claim_sources": "{\"src1\":{\"endpoint\":\"{URI}\"}}",
      "http://schemas.microsoft.com/claims/authnclassreference": "1",
      "aio": "{token}",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "{ID}",
      "e_exp": "{expiration}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "{last-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "{first-name}",
      "ipaddr": "{IP-address}",
      "name": "{full-name}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "onprem_sid": "{ID}",
      "puid": "{ID}",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",
      "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "{user-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "{user-name}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "resourceProvider": "Microsoft.Storage",
    "resourceUri": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
    "operationName": "Microsoft.Storage/storageAccounts/write",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}"
}]
```

Het volgende voorbeeld ziet u het schema voor een **ResourceDeleteSuccess** gebeurtenis. Hetzelfde schema wordt gebruikt voor **ResourceDeleteFailure** en **ResourceDeleteCancel** gebeurtenissen met verschillende waarden voor `eventType`.

```json
[{
  "subject": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
  "eventType": "Microsoft.Resources.ResourceDeleteSuccess",
  "eventTime": "2018-07-19T19:24:12.763881Z",
  "id": "19a69642-1aad-4a96-a5ab-8d05494513ce",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
      "action": "Microsoft.Storage/storageAccounts/delete",
      "evidence": {
        "role": "Subscription Admin"
      }
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "_claim_names": "{\"groups\":\"src1\"}",
      "_claim_sources": "{\"src1\":{\"endpoint\":\"{URI}\"}}",
      "http://schemas.microsoft.com/claims/authnclassreference": "1",
      "aio": "{token}",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "{ID}",
      "e_exp": "262800",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "{last-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "{first-name}",
      "ipaddr": "{IP-address}",
      "name": "{full-name}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "onprem_sid": "{ID}",
      "puid": "{ID}",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",
      "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "{user-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "{user-name}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "httpRequest": {
      "clientRequestId": "{ID}",
      "clientIpAddress": "{IP-address}",
      "method": "DELETE",
      "url": "https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2018-02-01"
    },
    "resourceProvider": "Microsoft.Storage",
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
    "operationName": "Microsoft.Storage/storageAccounts/delete",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}"
}]
```

## <a name="event-properties"></a>Eigenschappen van gebeurtenis

Een gebeurtenis heeft de volgende gegevens op het hoogste niveau:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| onderwerp | tekenreeks | Volledige resource-pad naar de bron van de gebeurtenis. Dit veld is niet beschrijfbaar. Event Grid biedt deze waarde. |
| Onderwerp | tekenreeks | Uitgever gedefinieerde pad naar het onderwerp van de gebeurtenis. |
| type gebeurtenis | tekenreeks | Een van de geregistreerde gebeurtenis-typen voor de bron van deze gebeurtenis. |
| eventTime | tekenreeks | Het moment waarop dat de gebeurtenis is gegenereerd, is afhankelijk van de UTC-tijd van de provider. |
| id | tekenreeks | De unieke id voor de gebeurtenis. |
| gegevens | object | Gebeurtenisgegevens van de resource-groep. |
| dataVersion | tekenreeks | De schemaversie van het gegevensobject. De uitgever definieert de schemaversie. |
| metadataVersion | tekenreeks | De schemaversie van de metagegevens van de gebeurtenis. Event Grid definieert het schema van de eigenschappen op het hoogste niveau. Event Grid biedt deze waarde. |

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| Autorisatie | object | De aangevraagde autorisatie voor de bewerking. |
| claims | object | De eigenschappen van de claims. Zie voor meer informatie, [JWT-specificatie](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html). |
| correlationId | tekenreeks | Een bewerking-ID voor het oplossen van problemen. |
| httpRequest | object | De details van de bewerking. Dit object is alleen opgenomen wanneer u een bestaande resource bijwerkt of een bron te verwijderen. |
| resourceProvider | tekenreeks | De resourceprovider wanneer de bewerking wordt uitgevoerd. |
| resourceUri | tekenreeks | De URI van de resource in de bewerking. |
| operationName | tekenreeks | De bewerking die werd uitgevoerd. |
| status | tekenreeks | De status van de bewerking. |
| subscriptionId | tekenreeks | De abonnements-ID van de resource. |
| tenant-id | tekenreeks | De tenant-ID van de resource. |

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot Azure Event Grid, [wat is Event Grid?](overview.md)
* Zie voor meer informatie over het maken van een Azure Event Grid-abonnement [Event Grid-abonnementsschema](subscription-creation-schema.md).

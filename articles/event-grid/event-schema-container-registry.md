---
title: Azure Container Registry voor Event Grid-gebeurtenisschema
description: Beschrijft de eigenschappen die beschikbaar zijn voor Container Reigstry gebeurtenissen met Azure Event Grid
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 07/20/2018
ms.author: tomfitz
ms.openlocfilehash: 9ed918a7402abcbe79e302421f3b2ac725857464
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39188783"
---
# <a name="azure-event-grid-event-schema-for-container-registry"></a>Azure Event Grid-gebeurtenisschema voor Container Registry

Dit artikel bevat de eigenschappen en het schema voor Container Registry-gebeurtenissen. Zie voor een inleiding tot gebeurtenisschema's, [Azure Event Grid-gebeurtenisschema](event-schema.md).

## <a name="available-event-types"></a>Typen van de gebeurtenis berichten beschikbaar

BLOB-opslag, verzendt de volgende typen gebeurtenissen:

| Gebeurtenistype | Beschrijving |
| ---------- | ----------- |
| Microsoft.ContainerRegistry.ImagePushed | Treedt op wanneer een installatiekopie wordt gepusht. |
| Microsoft.ContainerRegistry.ImageDeleted | Treedt op wanneer een installatiekopie wordt gewist. |

## <a name="example-event"></a>Voorbeeld van de gebeurtenis

Het volgende voorbeeld ziet u het schema van een installatiekopie van een gebeurtenis gepusht: 

```json
[{
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "topic": "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.ContainerRegistry/registries/<name>",
  "subject": "aci-helloworld:v1",
  "eventType": "ImagePushed",
  "eventTime": "2018-04-25T21:39:47.6549614Z",
  "data": {
    "id": "31c51664-e5bd-416a-a5df-e5206bc47ed0",
    "timestamp": "2018-04-25T21:39:47.276585742Z",
    "action": "push",
    "target": {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "size": 3023,
      "digest": "sha256:213bbc182920ab41e18edc2001e06abcca6735d87782d9cef68abd83941cf0e5",
      "length": 3023,
      "repository": "aci-helloworld",
      "tag": "v1"
    },
    "request": {
      "id": "7c66f28b-de19-40a4-821c-6f5f6c0003a4",
      "host": "demo.azurecr.io",
      "method": "PUT",
      "useragent": "docker/18.03.0-ce go/go1.9.4 git-commit/0520e24 os/windows arch/amd64 UpstreamClient(Docker-Client/18.03.0-ce \\\\(windows\\\\))"
    }
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

Het schema voor een afbeelding verwijderd gebeurtenis lijkt:

```json
[{
  "id": "f06e3921-301f-42ec-b368-212f7d5354bd",
  "topic": "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.ContainerRegistry/registries/<name>",
  "subject": "aci-helloworld",
  "eventType": "ImageDeleted",
  "eventTime": "2018-04-26T17:56:01.8211268Z",
  "data": {
    "id": "f06e3921-301f-42ec-b368-212f7d5354bd",
    "timestamp": "2018-04-26T17:56:00.996603117Z",
    "action": "delete",
    "target": {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "digest": "sha256:213bbc182920ab41e18edc2001e06abcca6735d87782d9cef68abd83941cf0e5",
      "repository": "aci-helloworld"
    },
    "request": {
      "id": "aeda5b99-4197-409f-b8a8-ff539edb7de2",
      "host": "demo.azurecr.io",
      "method": "DELETE",
      "useragent": "python-requests/2.18.4"
    }
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
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
| gegevens | object | Gebeurtenisgegevens voor BLOB-opslag. |
| dataVersion | tekenreeks | De schemaversie van het gegevensobject. De uitgever definieert de schemaversie. |
| metadataVersion | tekenreeks | De schemaversie van de metagegevens van de gebeurtenis. Event Grid definieert het schema van de eigenschappen op het hoogste niveau. Event Grid biedt deze waarde. |

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| id | tekenreeks | De gebeurtenis-ID. |
| tijdstempel | tekenreeks | De tijd waarop de gebeurtenis heeft plaatsgevonden. |
| actie | tekenreeks | De actie die de opgegeven gebeurtenis omvat. |
| doel | object | Het doel van de gebeurtenis. |
| aanvraag | object | De aanvraag die de gebeurtenis is gegenereerd. |
| actor | object | De agent die de gebeurtenis heeft gestart. Deze waarde kan zijn voor de meeste situaties uit de autorisatiecontext van de aanvraag. |
| source | object | De registerknooppunt dat de gebeurtenis is gegenereerd. De bron plaats anders, terwijl de actor de gebeurtenis initieert, genereert het. |

Het doelobject heeft de volgende eigenschappen:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| mediaType | tekenreeks | Het MIME-type van het object waarnaar wordt verwezen. |
| grootte | geheel getal | Het aantal bytes van de inhoud. Hetzelfde als het veld lengte. |
| Verificatiesamenvatting | tekenreeks | De samenvatting van de inhoud, zoals gedefinieerd door het register V2 HTTP API-specificatie. |
| Lengte | geheel getal | Het aantal bytes van de inhoud. Hetzelfde als het veld grootte. |
| Opslagplaats | tekenreeks | De naam van de opslagplaats. |
| url | tekenreeks | De rechtstreekse URL naar de inhoud. |
| tag | tekenreeks | De naam van de tag. |

Het request-object heeft de volgende eigenschappen:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| id | tekenreeks | De ID van de aanvraag die de gebeurtenis heeft gestart. |
| adres van doel | tekenreeks | Het IP of hostnaam en mogelijk poort van de clientverbinding die de gebeurtenis heeft gestart. Deze waarde is de RemoteAddr van de standaard-http-aanvraag. |
| host | tekenreeks | De extern toegankelijke hostnaam van de registry-exemplaar, zoals opgegeven door de http host-header op binnenkomende aanvragen. |
| method | tekenreeks | De aanvraagmethode waarmee de gebeurtenis is gegenereerd. |
| userAgent | tekenreeks | De gebruiker agent-kop van de aanvraag. |

De actorobject heeft de volgende eigenschappen:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| naam | tekenreeks | Het onderwerp of de gebruikersnaam die is gekoppeld aan de context van de aanvraag die de gebeurtenis is gegenereerd. |

Het bronobject heeft de volgende eigenschappen:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| adres van doel | tekenreeks | Het IP-adres of hostnaam en de poort van de registerknooppunt dat de gebeurtenis is gegenereerd. Over het algemeen wordt deze waarde worden omgezet door besturingssysteem. Hostname() samen met de actieve. |
| instanceID | tekenreeks | Het actieve exemplaar van een toepassing. Wijzigingen na elke opnieuw opstarten. |

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot Azure Event Grid, [wat is Event Grid?](overview.md)
* Zie voor meer informatie over het maken van een Azure Event Grid-abonnement [Event Grid-abonnementsschema](subscription-creation-schema.md).

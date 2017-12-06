---
title: Azure Container register webhook-schemaverwijzing
description: Webhook aanvraag nettolading naslaginformatie voor JSON voor Azure Container register.
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 12/02/2017
ms.author: marsma
ms.openlocfilehash: 84f0277a7b1a5bd7dfe2178f78f34140b1dd2642
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="azure-container-registry-webhook-reference"></a>Naslaginformatie over Azure-Container register webhook

U kunt [configureren van webhooks](container-registry-webhook.md) voor de container-registersleutel die genereren van gebeurtenissen bij bepaalde acties worden uitgevoerd. U kunt bijvoorbeeld de webhooks die worden geactiveerd op de installatiekopie van de container inschakelen `push` en `delete` bewerkingen. Wanneer een webhook wordt geactiveerd, geeft Azure Container register een HTTP of HTTPS-aanvraag met informatie over de gebeurtenis naar een eindpunt dat u opgeeft. Uw eindpunt kunt verwerken van de webhook en dienovereenkomstig reageren.

De volgende secties bevatten informatie over het schema van de webhook-aanvragen die door ondersteunde gebeurtenissen gegenereerd. De gebeurtenis secties bevatten de nettolading schema voor het gebeurtenistype, de aanvraaglading van een voorbeeld en een of meer Voorbeeldopdrachten die de webhook wilt activeren.

Zie voor meer informatie over het configureren van webhooks voor uw Azure-container register [met behulp van Azure Container register webhooks](container-registry-webhook.md).

## <a name="webhook-requests"></a>Webhook-aanvragen

### <a name="http-request"></a>HTTP-aanvraag

Een geactiveerde webhook maakt een HTTP `POST` aanvragen naar de URL-eindpunt u hebt opgegeven bij het instellen van de webhook.

### <a name="http-headers"></a>HTTP-headers

Webhook-aanvragen bevatten een `Content-Type` van `application/json` als u niet hebt opgegeven een `Content-Type` aangepaste header voor uw webhook.

Geen andere kopteksten worden toegevoegd aan de aanvraag dan deze aangepaste headers die u mogelijk hebt opgegeven voor de webhook.

## <a name="push-event"></a>Push-gebeurtenis

Webhook geactiveerd wanneer een installatiekopie van een container wordt doorgeschoven, is naar een opslagplaats.

### <a name="push-event-payload"></a>Push-nettolading

|Element|Type|Beschrijving|
|-------------|----------|-----------|
|`id`|Reeks|De ID van de webhook-gebeurtenis.|
|`timestamp`|Datum en tijd|De tijd waarop de gebeurtenis webhook is geactiveerd.|
|`action`|Reeks|De actie die de webhook-gebeurtenis geactiveerd.|
|[doel](#target)|Complex Type|Het doel van de gebeurtenis waarvoor de webhook-gebeurtenis.|
|[aanvraag](#request)|Complex Type|De aanvraag die de webhook-gebeurtenis heeft gegenereerd.|

### <a name="target"></a>doel

|Element|Type|Beschrijving|
|------------------|----------|-----------|
|`mediaType`|Reeks|Het MIME-type van het object waarnaar wordt verwezen.|
|`size`|Int32|Het aantal bytes van de inhoud. Hetzelfde als het veld lengte.|
|`digest`|Reeks|De samenvatting van de inhoud, zoals gedefinieerd door het register V2 HTTP-API-specificatie.|
|`length`|Int32|Het aantal bytes van de inhoud. Hetzelfde als het veld grootte.|
|`repository`|Reeks|De naam van de opslagplaats.|
|`tag`|Reeks|De naam van de installatiekopie-tag.|

### <a name="request"></a>Aanvraag

|Element|Type|Beschrijving|
|------------------|----------|-----------|
|`id`|Reeks|De ID van de aanvraag die de gebeurtenis heeft gestart.|
|`host`|Reeks|De hostnaam van de extern toegankelijke van het register-exemplaar, zoals opgegeven door de HTTP host-header op binnenkomende aanvragen.|
|`method`|Reeks|De aanvraagmethode die de gebeurtenis heeft gegenereerd.|
|`useragent`|Reeks|De kop van de agent gebruiker van de aanvraag.|

### <a name="payload-example-push-event"></a>Voorbeeld van de nettolading: push-gebeurtenis

```JSON
{
  "id": "cb8c3971-9adc-488b-bdd8-43cbb4974ff5",
  "timestamp": "2017-11-17T16:52:01.343145347Z",
  "action": "push",
  "target": {
    "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
    "size": 524,
    "digest": "sha256:80f0d5c8786bb9e621a45ece0db56d11cdc624ad20da9fe62e9d25490f331d7d",
    "length": 524,
    "repository": "hello-world",
    "tag": "v1"
  },
  "request": {
    "id": "3cbb6949-7549-4fa1-86cd-a6d5451dffc7",
    "host": "myregistry.azurecr.io",
    "method": "PUT",
    "useragent": "docker/17.09.0-ce go/go1.8.3 git-commit/afdb6d4 kernel/4.10.0-27-generic os/linux arch/amd64 UpstreamClient(Docker-Client/17.09.0-ce \\(linux\\))"
  }
}
```

Voorbeeld [Docker CLI](https://docs.docker.com/engine/reference/commandline/cli/) opdracht die u activeert de **push** gebeurtenis webhook:

```bash
docker push myregistry.azurecr.io/hello-world:v1
```

## <a name="delete-event"></a>Gebeurtenis verwijderen

Webhook geactiveerd wanneer een opslagplaats of manifest wordt verwijderd. Niet geactiveerd wanneer een label wordt verwijderd.

### <a name="delete-event-payload"></a>Verwijderen van de nettolading

|Element|Type|Beschrijving|
|-------------|----------|-----------|
|`id`|Reeks|De ID van de webhook-gebeurtenis.|
|`timestamp`|Datum en tijd|De tijd waarop de gebeurtenis webhook is geactiveerd.|
|`action`|Reeks|De actie die de webhook-gebeurtenis geactiveerd.|
|[doel](#delete_target)|Complex Type|Het doel van de gebeurtenis waarvoor de webhook-gebeurtenis.|
|[aanvraag](#delete_request)|Complex Type|De aanvraag die de webhook-gebeurtenis heeft gegenereerd.|

### <a name="delete_target"></a>doel

|Element|Type|Beschrijving|
|------------------|----------|-----------|
|`mediaType`|Reeks|Het MIME-type van het object waarnaar wordt verwezen.|
|`digest`|Reeks|De samenvatting van de inhoud, zoals gedefinieerd door het register V2 HTTP-API-specificatie.|
|`repository`|Reeks|De naam van de opslagplaats.|

### <a name="delete_request"></a>aanvraag

|Element|Type|Beschrijving|
|------------------|----------|-----------|
|`id`|Reeks|De ID van de aanvraag die de gebeurtenis heeft gestart.|
|`host`|Reeks|De hostnaam van de extern toegankelijke van het register-exemplaar, zoals opgegeven door de HTTP host-header op binnenkomende aanvragen.|
|`method`|Reeks|De aanvraagmethode die de gebeurtenis heeft gegenereerd.|
|`useragent`|Reeks|De kop van de agent gebruiker van de aanvraag.|

### <a name="payload-example-delete-event"></a>Voorbeeld van de nettolading: verwijderen van gebeurtenis

```JSON
{
    "id": "afc359ce-df7f-4e32-bdde-1ff8aa80927b",
    "timestamp": "2017-11-17T16:54:53.657764628Z",
    "action": "delete",
    "target": {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "digest": "sha256:80f0d5c8786bb9e621a45ece0db56d11cdc624ad20da9fe62e9d25490f331d7d",
      "repository": "hello-world"
    },
    "request": {
      "id": "3d78b540-ab61-4f75-807f-7ca9ecf559b3",
      "host": "myregistry.azurecr.io",
      "method": "DELETE",
      "useragent": "python-requests/2.18.4"
    }
  }
```

Voorbeeld [Azure CLI 2.0](/cli/azure/acr) opdrachten die een **verwijderen** webhook gebeurtenis:

```azurecli
# Delete repository
az acr repository delete -n MyRegistry --repository MyRepository

# Delete manifest
az acr repository delete -n MyRegistry --repository MyRepository --tag MyTag --manifest
```

## <a name="next-steps"></a>Volgende stappen

[Met behulp van Azure Container register webhooks.](container-registry-webhook.md)
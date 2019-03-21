---
title: Azure Container Registry webhook-schemaverwijzing
description: Webhook-aanvraag JSON-nettolading verwijzing voor Azure Container Registry.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 03/05/2019
ms.author: danlep
ms.openlocfilehash: 4c0845b9cf5194ecbd0ab813997e17e070840f44
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58099896"
---
# <a name="azure-container-registry-webhook-reference"></a>Naslaginformatie over Azure Container Registry webhook

U kunt [configureren van webhooks](container-registry-webhook.md) voor uw containerregister die gebeurtenissen wanneer bepaalde acties worden uitgevoerd op basis van het genereren. Bijvoorbeeld, kunt u webhooks die worden geactiveerd wanneer een containerinstallatiekopie Helm-diagram naar een register gepusht of is verwijderd. Wanneer een webhook wordt geactiveerd, problemen met Azure Container Registry een HTTP of HTTPS-aanvraag met informatie over de gebeurtenis naar een eindpunt dat u opgeeft. Het eindpunt vervolgens verwerken van de webhook en dienovereenkomstig kan reageren.

De volgende secties bevatten informatie over het schema van de webhook-aanvragen die worden gegenereerd door ondersteunde gebeurtenissen. De gebeurtenis secties bevatten de payload-schema voor het gebeurtenistype, een voorbeeld van de aangevraagde nettolading en een of meer Voorbeeldopdrachten die de webhook wordt geactiveerd.

Zie voor meer informatie over het configureren van webhooks voor het Azure container registry [met behulp van Azure Container Registry-webhooks](container-registry-webhook.md).

## <a name="webhook-requests"></a>Webhook-aanvragen

### <a name="http-request"></a>HTTP-aanvraag

Een webhook geactiveerde maakt een HTTP `POST` aanvragen naar de URL-eindpunt u hebt opgegeven bij het configureren van de webhook.

### <a name="http-headers"></a>HTTP-headers

Webhook-aanvragen bevatten een `Content-Type` van `application/json` als u niet hebt opgegeven een `Content-Type` aangepaste header voor de webhook.

Er zijn geen andere headers zijn toegevoegd aan de aanvraag dan deze aangepaste kopteksten die u mogelijk hebt opgegeven voor de webhook.

## <a name="push-event"></a>Push-gebeurtenis

Webhook wordt geactiveerd wanneer een containerinstallatiekopie is gepusht naar een opslagplaats.

### <a name="push-event-payload"></a>Push-nettolading

|Element|Type|Description|
|-------------|----------|-----------|
|`id`|String|De ID van de webhookgebeurtenis.|
|`timestamp`|DateTime|De tijd waarop de webhookgebeurtenis is geactiveerd.|
|`action`|String|De actie die de webhookgebeurtenis geactiveerd.|
|[target](#target)|Complex Type|Het doel van de gebeurtenis die de webhookgebeurtenis geactiveerd.|
|[request](#request)|Complex Type|De aanvraag die de webhookgebeurtenis is gegenereerd.|

### <a name="target"></a>Doel

|Element|Type|Description|
|------------------|----------|-----------|
|`mediaType`|String|Het MIME-type van het object waarnaar wordt verwezen.|
|`size`|Int32|Het aantal bytes van de inhoud. Hetzelfde als het veld lengte.|
|`digest`|String|De samenvatting van de inhoud, zoals gedefinieerd door het register V2 HTTP API-specificatie.|
|`length`|Int32|Het aantal bytes van de inhoud. Hetzelfde als het veld grootte.|
|`repository`|String|De naam van de opslagplaats.|
|`tag`|String|De naam van de installatiekopie-tag.|

### <a name="request"></a>Aanvraag

|Element|Type|Description|
|------------------|----------|-----------|
|`id`|String|De ID van de aanvraag die de gebeurtenis heeft gestart.|
|`host`|String|De extern toegankelijke hostnaam van de registry-exemplaar, zoals opgegeven door de HTTP host-header op binnenkomende aanvragen.|
|`method`|String|De aanvraagmethode waarmee de gebeurtenis is gegenereerd.|
|`useragent`|String|De gebruiker agent-kop van de aanvraag.|

### <a name="payload-example-image-push-event"></a>Voorbeeld van de nettolading: afbeelding push-gebeurtenis

```JSON
{
  "id": "cb8c3971-9adc-488b-xxxx-43cbb4974ff5",
  "timestamp": "2017-11-17T16:52:01.343145347Z",
  "action": "push",
  "target": {
    "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
    "size": 524,
    "digest": "sha256:xxxxd5c8786bb9e621a45ece0dbxxxx1cdc624ad20da9fe62e9d25490f33xxxx",
    "length": 524,
    "repository": "hello-world",
    "tag": "v1"
  },
  "request": {
    "id": "3cbb6949-7549-4fa1-xxxx-a6d5451dffc7",
    "host": "myregistry.azurecr.io",
    "method": "PUT",
    "useragent": "docker/17.09.0-ce go/go1.8.3 git-commit/afdb6d4 kernel/4.10.0-27-generic os/linux arch/amd64 UpstreamClient(Docker-Client/17.09.0-ce \\(linux\\))"
  }
}
```

Voorbeeld [Docker-Opdrachtregelinterface](https://docs.docker.com/engine/reference/commandline/cli/) opdracht die de installatiekopie wordt geactiveerd **push** event webhook:

```bash
docker push myregistry.azurecr.io/hello-world:v1
```

## <a name="chart-push-event"></a>Grafiek push-gebeurtenis

Webhook wordt geactiveerd wanneer een Helm-grafiek wordt doorgestuurd naar een opslagplaats.

### <a name="chart-push-event-payload"></a>Grafiek push-nettolading

|Element|Type|Description|
|-------------|----------|-----------|
|`id`|String|De ID van de webhookgebeurtenis.|
|`timestamp`|DateTime|De tijd waarop de webhookgebeurtenis is geactiveerd.|
|`action`|String|De actie die de webhookgebeurtenis geactiveerd.|
|[target](#helm_target)|Complex Type|Het doel van de gebeurtenis die de webhookgebeurtenis geactiveerd.|

### <a name="helm_target"></a>Doel

|Element|Type|Description|
|------------------|----------|-----------|
|`mediaType`|String|Het MIME-type van het object waarnaar wordt verwezen.|
|`size`|Int32|Het aantal bytes van de inhoud.|
|`digest`|String|De samenvatting van de inhoud, zoals gedefinieerd door het register V2 HTTP API-specificatie.|
|`repository`|String|De naam van de opslagplaats.|
|`tag`|String|De naam van de grafiek-tag.|
|`name`|String|De naam van de grafiek.|
|`version`|String|De versie van de grafiek.|

### <a name="payload-example-chart-push-event"></a>Voorbeeld van de nettolading: grafiek push-gebeurtenis

```JSON
{
  "id": "6356e9e0-627f-4fed-xxxx-d9059b5143ac",
  "timestamp": "2019-03-05T23:45:31.2614267Z",
  "action": "chart_push",
  "target": {
    "mediaType": "application/vnd.acr.helm.chart",
    "size": 25265,
    "digest": "sha256:xxxx8075264b5ba7c14c23672xxxx52ae6a3ebac1c47916e4efe19cd624dxxxx",
    "repository": "repo",
    "tag": "wordpress-5.4.0.tgz",
    "name": "wordpress",
    "version": "5.4.0.tgz"
  }
}
```

Voorbeeld [Azure CLI](/cli/azure/acr) opdracht die wordt geactiveerd de **chart_push** event webhook:

```azurecli
az acr helm push wordpress-5.4.0.tgz --name MyRegistry
```

## <a name="delete-event"></a>Gebeurtenis verwijderen

Webhook wordt geactiveerd wanneer een opslagplaats voor installatiekopieën of het manifest is verwijderd. Niet geactiveerd wanneer een label wordt verwijderd.

### <a name="delete-event-payload"></a>Nettolading verwijderen

|Element|Type|Description|
|-------------|----------|-----------|
|`id`|String|De ID van de webhookgebeurtenis.|
|`timestamp`|DateTime|De tijd waarop de webhookgebeurtenis is geactiveerd.|
|`action`|String|De actie die de webhookgebeurtenis geactiveerd.|
|[target](#delete_target)|Complex Type|Het doel van de gebeurtenis die de webhookgebeurtenis geactiveerd.|
|[request](#delete_request)|Complex Type|De aanvraag die de webhookgebeurtenis is gegenereerd.|

### <a name="delete_target"></a> Doel

|Element|Type|Description|
|------------------|----------|-----------|
|`mediaType`|String|Het MIME-type van het object waarnaar wordt verwezen.|
|`digest`|String|De samenvatting van de inhoud, zoals gedefinieerd door het register V2 HTTP API-specificatie.|
|`repository`|String|De naam van de opslagplaats.|

### <a name="delete_request"></a> Aanvraag

|Element|Type|Description|
|------------------|----------|-----------|
|`id`|String|De ID van de aanvraag die de gebeurtenis heeft gestart.|
|`host`|String|De extern toegankelijke hostnaam van de registry-exemplaar, zoals opgegeven door de HTTP host-header op binnenkomende aanvragen.|
|`method`|String|De aanvraagmethode waarmee de gebeurtenis is gegenereerd.|
|`useragent`|String|De gebruiker agent-kop van de aanvraag.|

### <a name="payload-example-image-delete-event"></a>Voorbeeld van de nettolading: gebeurtenis van de installatiekopie verwijderen

```JSON
{
    "id": "afc359ce-df7f-4e32-xxxx-1ff8aa80927b",
    "timestamp": "2017-11-17T16:54:53.657764628Z",
    "action": "delete",
    "target": {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "digest": "sha256:xxxxd5c8786bb9e621a45ece0dbxxxx1cdc624ad20da9fe62e9d25490f33xxxx",
      "repository": "hello-world"
    },
    "request": {
      "id": "3d78b540-ab61-4f75-xxxx-7ca9ecf559b3",
      "host": "myregistry.azurecr.io",
      "method": "DELETE",
      "useragent": "python-requests/2.18.4"
    }
  }
```

Voorbeeld [Azure CLI](/cli/azure/acr) opdrachten die een **verwijderen** event webhook:

```azurecli
# Delete repository
az acr repository delete --name MyRegistry --repository MyRepository

# Delete image
az acr repository delete --name MyRegistry --image MyRepository:MyTag
```

## <a name="chart-delete-event"></a>Gebeurtenis van grafiek verwijderen

Webhook wordt geactiveerd wanneer een Helm-diagram of de opslagplaats is verwijderd. 

### <a name="chart-delete-event-payload"></a>Grafiek verwijderen-nettolading

|Element|Type|Description|
|-------------|----------|-----------|
|`id`|String|De ID van de webhookgebeurtenis.|
|`timestamp`|DateTime|De tijd waarop de webhookgebeurtenis is geactiveerd.|
|`action`|String|De actie die de webhookgebeurtenis geactiveerd.|
|[target](#chart_delete_target)|Complex Type|Het doel van de gebeurtenis die de webhookgebeurtenis geactiveerd.|

### <a name="chart_delete_target"></a> Doel

|Element|Type|Description|
|------------------|----------|-----------|
|`mediaType`|String|Het MIME-type van het object waarnaar wordt verwezen.|
|`size`|Int32|Het aantal bytes van de inhoud.|
|`digest`|String|De samenvatting van de inhoud, zoals gedefinieerd door het register V2 HTTP API-specificatie.|
|`repository`|String|De naam van de opslagplaats.|
|`tag`|String|De naam van de grafiek-tag.|
|`name`|String|De naam van de grafiek.|
|`version`|String|De versie van de grafiek.|

### <a name="payload-example-chart-delete-event"></a>Voorbeeld van de nettolading: gebeurtenis van grafiek verwijderen

```JSON
{
  "id": "338a3ef7-ad68-4128-xxxx-fdd3af8e8f67",
  "timestamp": "2019-03-06T00:10:48.1270754Z",
  "action": "chart_delete",
  "target": {
    "mediaType": "application/vnd.acr.helm.chart",
    "size": 25265,
    "digest": "sha256:xxxx8075264b5ba7c14c23672xxxx52ae6a3ebac1c47916e4efe19cd624dxxxx",
    "repository": "repo",
    "tag": "wordpress-5.4.0.tgz",
    "name": "wordpress",
    "version": "5.4.0.tgz"
  }
}
```

Voorbeeld [Azure CLI](/cli/azure/acr) opdracht die wordt geactiveerd de **chart_delete** event webhook:

```azurecli
az acr helm delete wordpress --version 5.4.0 --name MyRegistry
```

## <a name="next-steps"></a>Volgende stappen

[Met behulp van Azure Container Registry-webhooks](container-registry-webhook.md)
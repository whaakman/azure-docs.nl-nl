---
title: Spraak containers installeren
titleSuffix: Azure Cognitive Services
description: Details over de configuratieopties van de Text to Speech helm-grafiek.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: e6c7dcd3015b0b8ab5b3c719ebd2397bc814b81a
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711492"
---
### <a name="text-to-speech-sub-chart-chartstexttospeech"></a>Text to Speech (onderliggende grafiek: grafieken/textToSpeech)

Als u wilt overschrijven van de grafiek "overkoepelende", voegt u het voorvoegsel `textToSpeech.` op elke parameter aanbrengen specifiekere. Bijvoorbeeld, overschrijft het met de bijbehorende parameter bijvoorbeeld `textToSpeech.numberOfConcurrentRequest` overschrijft `numberOfConcurrentRequest`.

|Parameter|Description|Standaard|
| -- | -- | -- |
| `enabled` | Of de **tekst naar spraak** -service is ingeschakeld. | `false` |
| `numberOfConcurrentRequest` | Het aantal gelijktijdige aanvragen voor de **tekst naar spraak** service. In deze grafiek wordt automatisch berekend CPU en geheugen resources, op basis van deze waarde. | `2` |
| `optimizeForTurboMode`| Of de service moet worden geoptimaliseerd voor tekstinvoer via tekstbestanden. Als `true`, deze grafiek wordt meer CPU-bronnen naar service toewijzen. | `false` |
| `image.registry`| De **tekst naar spraak** docker-installatiekopieregisters. | `containerpreview.azurecr.io` |
| `image.repository` | De **tekst naar spraak** opslagplaats voor docker-installatiekopieën. | `microsoft/cognitive-services-text-to-speech` |
| `image.tag` | De **tekst naar spraak** docker afbeeldingscode. | `latest` |
| `image.pullSecrets` | De geheimen van de afbeelding voor het ophalen van de **tekst naar spraak** docker-installatiekopie. | |
| `image.pullByHash`| Of de docker-installatiekopie is opgehaald door hash. Als `true`, `image.hash` is vereist. | `false` |
| `image.hash`| De **tekst naar spraak** docker-kopie-hash. Alleen gebruikt wanneer `image.pullByHash: true`.  | |
| `image.args.eula` (vereist) | Geeft aan dat u de licentievoorwaarden hebt geaccepteerd. De enige geldige waarde is `accept` | |
| `image.args.billing` (vereist) | Het eindpunt van de facturering URI-waarde is beschikbaar op de pagina van de spraak-overzicht van de Azure portal. | |
| `image.args.apikey` (vereist) | Gebruikt voor het bijhouden van informatie over facturering. ||
| `service.type` | De Kubernetes-service type van de **tekst naar spraak** service. Zie de [Kubernetes-service van het type instructies](https://kubernetes.io/docs/concepts/services-networking/service/) voor meer informatie en controleer of de provider-cloudondersteuning. | `LoadBalancer` |
| `service.port`|  De poort van de **tekst naar spraak** service. | `80` |
| `service.autoScaler.enabled` | Of de [horizontale Pod-automatisch schalen](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) is ingeschakeld. Als `true`, wordt de `text-to-speech-autoscaler` wordt geïmplementeerd in het Kubernetes-cluster. | `true` |
| `service.podDisruption.enabled` | Of de [Pod onderbreking Budget](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) is ingeschakeld. Als `true`, wordt de `text-to-speech-poddisruptionbudget` wordt geïmplementeerd in het Kubernetes-cluster. | `true` |
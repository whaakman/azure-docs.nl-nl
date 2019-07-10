---
title: Spraak containers installeren
titleSuffix: Azure Cognitive Services
description: Details over de configuratieopties voor spraak-naar-tekst helm-grafiek.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 1b46c58d3f3c804052e637f7bde2e1a456764dba
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711503"
---
### <a name="speech-to-text-sub-chart-chartsspeechtotext"></a>Spraak-naar-tekst (onderliggende grafiek: grafieken/speechToText)

Als u wilt overschrijven van de grafiek "overkoepelende", voegt u het voorvoegsel `speechToText.` op elke parameter aanbrengen specifiekere. Bijvoorbeeld, overschrijft het met de bijbehorende parameter bijvoorbeeld `speechToText.numberOfConcurrentRequest` overschrijft `numberOfConcurrentRequest`.

|Parameter|Description|Standaard|
| -- | -- | -- |
| `enabled` | Of de **spraak-naar-tekst** -service is ingeschakeld. | `false` |
| `numberOfConcurrentRequest` | Het aantal gelijktijdige aanvragen voor de **spraak-naar-tekst** service. In deze grafiek wordt automatisch berekend CPU en geheugen resources, op basis van deze waarde. | `2` |
| `optimizeForAudioFile`| Of de service moet worden geoptimaliseerd voor audio-invoer via audio-bestanden. Als `true`, deze grafiek wordt meer CPU-bronnen naar service toewijzen. | `false` |
| `image.registry`| De **spraak-naar-tekst** docker-installatiekopieregisters. | `containerpreview.azurecr.io` |
| `image.repository` | De **spraak-naar-tekst** opslagplaats voor docker-installatiekopieën. | `microsoft/cognitive-services-speech-to-text` |
| `image.tag` | De **spraak-naar-tekst** docker afbeeldingscode. | `latest` |
| `image.pullSecrets` | De geheimen van de afbeelding voor het ophalen van de **spraak-naar-tekst** docker-installatiekopie. | |
| `image.pullByHash`| Of de docker-installatiekopie is opgehaald door hash. Als `true`, `image.hash` is vereist. | `false` |
| `image.hash`| De **spraak-naar-tekst** docker-kopie-hash. Alleen gebruikt wanneer `image.pullByHash: true`.  | |
| `image.args.eula` (vereist) | Geeft aan dat u de licentievoorwaarden hebt geaccepteerd. De enige geldige waarde is `accept` | |
| `image.args.billing` (vereist) | Het eindpunt van de facturering URI-waarde is beschikbaar op de pagina van de spraak-overzicht van de Azure portal. | |
| `image.args.apikey` (vereist) | Gebruikt voor het bijhouden van informatie over facturering. ||
| `service.type` | De Kubernetes-service type van de **spraak-naar-tekst** service. Zie de [Kubernetes-service van het type instructies](https://kubernetes.io/docs/concepts/services-networking/service/) voor meer informatie en controleer of de provider-cloudondersteuning. | `LoadBalancer` |
| `service.port`|  De poort van de **spraak-naar-tekst** service. | `80` |
| `service.autoScaler.enabled` | Of de [horizontale Pod-automatisch schalen](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) is ingeschakeld. Als `true`, wordt de `speech-to-text-autoscaler` wordt geïmplementeerd in het Kubernetes-cluster. | `true` |
| `service.podDisruption.enabled` | Of de [Pod onderbreking Budget](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) is ingeschakeld. Als `true`, wordt de `speech-to-text-poddisruptionbudget` wordt geïmplementeerd in het Kubernetes-cluster. | `true` |
---
title: bestand opnemen
description: bestand opnemen
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 08/30/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: fe1227d91c0d039a94e5b9a3046b879f4f17355a
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56246817"
---
| Resource | Basic | Standard | Premium |
|---|---|---|---|---|
| Storage<sup>1</sup> | 10 GiB | 100 GiB| 500 GiB |
| Maximumgrootte van installatiekopie-laag | 20 GiB | 20 GiB | 50 giB |
| ReadOps per minuut<sup>2, 3</sup> | 1000 | 3.000 | 10.000 |
| WriteOps per minuut<sup>2, 4</sup> | 100 | 500 | 2,000 |
| Downloaden van bandbreedte MBps<sup>2</sup> | 30 | 60 | 100 |
| Uploaden van bandbreedte MBps<sup>2</sup> | 10 | 20 | 50 |
| Webhooks | 2 | 10 | 100 |
| Geo-replicatie | N/A | N/A | [Ondersteund][geo-replication] |
| Inhoud vertrouwen (Preview-versie) | N/A | N/A | [Ondersteund][content-trust] |

<sup>1</sup> de opgegeven opslaglimieten zijn het bedrag van *opgenomen* opslag voor elke laag. U betaalt een extra dagelijks tarief dat per GB voor afbeeldingopslag hierboven deze limieten. Zie voor meer informatie het tarief, [prijzen voor Container Registry][pricing].

<sup>2</sup> *ReadOps*, *WriteOps*, en *bandbreedte* minimale schattingen zijn. ACR streeft ernaar om prestaties te verbeteren als gebruik vereist.

<sup>3</sup> [docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) wordt omgezet in meerdere leesbewerkingen op basis van het aantal lagen in de afbeelding, plus het manifest ophalen.

<sup>4</sup> [docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) wordt omgezet in meerdere schrijfbewerkingen, op basis van het aantal lagen die moet worden gepusht. Een `docker push` bevat *ReadOps* om op te halen van een manifest voor een bestaande installatiekopie.

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/

<!-- LINKS - Internal -->
[geo-replication]: ../articles/container-registry/container-registry-geo-replication.md
[content-trust]: ../articles/container-registry/container-registry-content-trust.md

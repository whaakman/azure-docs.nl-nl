---
title: bestand opnemen
description: bestand opnemen
services: container-registry
author: mmacy
ms.service: container-registry
ms.topic: include
ms.date: 08/30/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: bddb17a5163333a5aeb86b296a21867f0611d12f
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43302520"
---
| Resource | Basic | Standard | Premium |
|---|---|---|---|---|
| Opslag<sup>1</sup> | 10 GiB | 100 GiB| 500 GiB |
| Maximumgrootte van installatiekopie-laag | 20 giB | 20 giB | 50 giB |
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
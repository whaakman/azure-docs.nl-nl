---
title: bestand opnemen
description: bestand opnemen
services: container-registry
author: mmacy
ms.service: container-registry
ms.topic: include
ms.date: 05/29/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: 92a5d162e7a0b2c752a2f8e9c5941edf43e539e3
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991072"
---
| Resource | Basic | Standard | Premium |
|---|---|---|---|---|
| Storage | 10 GiB | 100 GiB| 500 GiB |
| Maximumgrootte van installatiekopie-laag | 20 giB | 20 giB | 50 giB |
| ReadOps per minuut<sup>1, 2</sup> | 1000 | 3.000 | 10.000 |
| WriteOps per minuut<sup>1, 3</sup> | 100 | 500 | 2,000 |
| Downloaden van bandbreedte MBps<sup>1</sup> | 30 | 60 | 100 |
| Uploaden van bandbreedte MBps<sup>1</sup> | 10 | 20 | 50 |
| Webhooks | 2 | 10 | 100 |
| Geo-replicatie | N/A | N/A | [Ondersteund](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication) |

<sup>1</sup> *ReadOps*, *WriteOps*, en *bandbreedte* minimale schattingen zijn. ACR streeft ernaar om prestaties te verbeteren als gebruik vereist.

<sup>2</sup> [docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) wordt omgezet in meerdere leesbewerkingen op basis van het aantal lagen in de afbeelding, plus het manifest ophalen.

<sup>3</sup> [docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) wordt omgezet in meerdere schrijfbewerkingen, op basis van het aantal lagen die moet worden gepusht. Een `docker push` bevat *ReadOps* om op te halen van een manifest voor een bestaande installatiekopie.

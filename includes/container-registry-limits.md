---
title: bestand opnemen
description: bestand opnemen
services: container-registry
author: mmacy
ms.service: container-registry
ms.topic: include
ms.date: 03/23/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: 575483192954f4e05db50e701e223829e041cffc
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
| Resource | Basic | Standard | Premium |
|---|---|---|---|---|
| Storage | 10 GiB | 100 GiB| 500 GiB |
| ReadOps per minuut<sup>1, 2</sup> | 1000 | 3000 | 10.000 |
| WriteOps per minuut<sup>1, 3</sup> | 100 | 500 | 2000 |
| Download MBps bandbreedte<sup>1</sup> | 30 | 60 | 100 |
| Uploaden van bandbreedte MBps<sup>1</sup> | 10 | 20 | 50 |
| Webhooks | 2 | 10 | 100 |
| Geo-replicatie | N/A | N/A | [Ondersteund *(preview)*](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication) |

<sup>1</sup> *ReadOps*, *WriteOps*, en *bandbreedte* zijn minimale schattingen. ACR wil de prestaties verbeteren als gebruik vereist.

<sup>2</sup> [docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) kan aan meerdere leesbewerkingen op basis van het aantal lagen in de afbeelding, plus het manifest ophalen.

<sup>3</sup> [docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) kan aan meerdere schrijfbewerkingen, op basis van het aantal lagen die moet worden geactiveerd. Een `docker push` bevat *ReadOps* voor het ophalen van een manifest voor een bestaande installatiekopie.
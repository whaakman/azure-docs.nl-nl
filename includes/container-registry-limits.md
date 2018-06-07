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
ms.openlocfilehash: 942b9bdf0201acaefe3333bcf928772899b9bdc2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34665049"
---
| Resource | Basic | Standard | Premium |
|---|---|---|---|---|
| Storage | 10 GiB | 100 GiB| 500 GiB |
| Maximumgrootte van de installatiekopie van laag | 20 giB | 20 giB | 50 giB |
| ReadOps per minuut<sup>1, 2</sup> | 1000 | 3000 | 10.000 |
| WriteOps per minuut<sup>1, 3</sup> | 100 | 500 | 2000 |
| Download MBps bandbreedte<sup>1</sup> | 30 | 60 | 100 |
| Uploaden van bandbreedte MBps<sup>1</sup> | 10 | 20 | 50 |
| Webhooks | 2 | 10 | 100 |
| Geo-replicatie | N/A | N/A | [Ondersteund](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication) |

<sup>1</sup> *ReadOps*, *WriteOps*, en *bandbreedte* zijn minimale schattingen. ACR wil de prestaties verbeteren als gebruik vereist.

<sup>2</sup> [docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) kan aan meerdere leesbewerkingen op basis van het aantal lagen in de afbeelding, plus het manifest ophalen.

<sup>3</sup> [docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) kan aan meerdere schrijfbewerkingen, op basis van het aantal lagen die moet worden geactiveerd. Een `docker push` bevat *ReadOps* voor het ophalen van een manifest voor een bestaande installatiekopie.
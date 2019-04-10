---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 99314538f90404d7c2b72da0dd2da2d8ac60a08a
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59291746"
---
| Resource | Standard-bestandsshares | Premium-bestandsshares (preview) |
|----------|---------------|------------------------------------------|
| Minimale grootte van een bestandsshare | Geen minimum; betalen naar gebruik | 100 giB; ingericht |
| Maximale grootte van een bestandsshare | 5 TiB | 5 TiB (openbare preview), 100 TiB (beperkte openbare preview-versie) |
| Maximale grootte van een bestand in een bestandsshare | 1 TiB | 1 TiB |
| Maximum aantal bestanden in een bestandsshare | Geen limiet | Geen limiet |
| Maximale IOPS per share | 1000 IOPS | 5.120 basis IOPS met 15,360 burst limiet (openbare preview), 100.000 IOPS (beperkte openbare preview-versie)|
| Maximum aantal opgeslagen toegangsbeleid per bestand delen | 5 | 5 |
| Doeldoorvoer voor het delen van één bestand | Maximaal 60 MiB/sec | Premium-bestand delen inkomend en uitgaand verkeer waarden|
| Maximum aantal uitgaande voor het delen van één bestand | Zie doeldoorvoer van standaard-bestand delen | Tot 368 MiB/s (openbare preview), tot 6,204 MiB/s (beperkte openbare preview-versie) |
| Maximale ingangslimiet voor het delen van één bestand | Zie doeldoorvoer van standaard-bestand delen | Tot 245 MiB/s (openbare preview), tot 4,136 MiB/s (beperkte openbare preview-versie) |
| Maximum aantal open ingangen per bestand | 2000 open ingangen | 2000 open ingangen |
| Maximum aantal momentopnamen van shares | 200 momentopnamen | 200 momentopnamen |
| Lengte van maximaal object (mappen en bestanden) | 2.048 tekens | 2.048 tekens |
| Maximale pathname onderdeel (in het pad \A\B\C\D, elke letter is een onderdeel) | 255 tekens | 255 tekens |
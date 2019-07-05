---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 05/06/2019
ms.author: rogarana
ms.openlocfilehash: 9f259c3e403e933c847ac56000b1db2cd594caf5
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449946"
---
| Resource | Standard-bestandsshares | Premium-bestandsshares |
|----------|---------------|------------------------------------------|
| Minimale grootte van een bestandsshare | Geen minimum; betalen naar gebruik | 100 giB; ingericht |
| Maximale grootte van een bestandsshare | 5 TiB (GA), 100 TiB (preview) | 100 TiB |
| Maximale grootte van een bestand in een bestandsshare | 1 TiB | 1 TiB |
| Maximum aantal bestanden in een bestandsshare | Geen limiet | Geen limiet |
| Maximale IOPS per share | 1000 IOPS (GA), 10.000 IOPS (preview) | 100.000 IOPS |
| Maximum aantal opgeslagen toegangsbeleid per bestand delen | 5 | 5 |
| Doeldoorvoer voor het delen van één bestand | Maximaal 60 MiB per seconde (GA), maximaal 300 MiB per seconde (preview) | Premium-bestand delen inkomend en uitgaand verkeer waarden|
| Maximum aantal uitgaande voor het delen van één bestand | Zie doeldoorvoer van standaard-bestand delen | Tot 6,204 MiB/s |
| Maximale ingangslimiet voor het delen van één bestand | Zie doeldoorvoer van standaard-bestand delen | Tot 4,136 MiB/s |
| Maximum aantal open ingangen per bestand | 2000 open ingangen | 2000 open ingangen |
| Maximum aantal momentopnamen van shares | 200 momentopnamen | 200 momentopnamen |
| Lengte van maximaal object (mappen en bestanden) | 2\.048 tekens | 2\.048 tekens |
| Maximale pathname onderdeel (in het pad \A\B\C\D, elke letter is een onderdeel) | 255 tekens | 255 tekens |
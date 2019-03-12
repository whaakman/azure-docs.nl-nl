---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: ec75cf35bb503e49885664a6682e3be319cc45f6
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553640"
---
| Resource | Standard-bestandsshares | Premium-bestandsshares (preview) |
|----------|---------------|------------------------------------------|
| Minimale grootte van een bestandsshare | Geen minimum; betalen naar gebruik | 100 GiB |
| Maximale grootte van een bestandsshare | 5 TiB | 5 TiB |
| Maximale grootte van een bestand in een bestandsshare | 1 TiB | 1 TiB |
| Maximum aantal bestanden in een bestandsshare | Geen limiet | Geen limiet |
| Maximale IOPS per share | 1000 IOPS | 5.120 IOPS-basislijn<br />15,360 IOPS met een ' burst ' |
| Maximum aantal opgeslagen toegangsbeleid per bestand delen | 5 | 5 |
| Doeldoorvoer voor het delen van één bestand | Maximaal 60 MiB/sec | Maximaal 612 MiB per seconde (ingericht) |
| Maximum aantal open ingangen per bestand | 2000 open ingangen | 2000 open ingangen |
| Maximum aantal momentopnamen van shares | 200 momentopnamen | 200 momentopnamen |
| Lengte van maximaal object (mappen en bestanden) | 2.048 tekens | 2.048 tekens |
| Maximale pathname onderdeel (in het pad \A\B\C\D, elke letter is een onderdeel) | 255 tekens | 255 tekens |
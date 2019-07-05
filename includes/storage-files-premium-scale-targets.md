---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 06/07/2019
ms.author: rogarana
ms.openlocfilehash: 368f08272173b019873dfe20e1164d6baf72ff5e
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542671"
---
#### <a name="additional-premium-file-share-level-limits"></a>Aanvullende premium-bestand delen niveau limieten

|Onderwerp  |Doel  |
|---------|---------|
|Minimale grootte vergroten/verkleinen    |1 GiB      |
|IOPS-basislijn    |1 IOP's per GiB maximaal 100.000|
|IOPS bursting    |3 x IOPS per GB, maximaal 100.000|
|Snelheid van uitgangsgebeurtenissen         |60 MiB/s + 0,06 * ingericht GiB        |
|Gegevens| 40 MiB/s + 0,04 * ingericht GiB |

#### <a name="file-level-limits"></a>Beperkingen van niveau

|Onderwerp  |Premium-bestand  |Standaard-bestand |
|---------|---------|---------|
|Size                  |1 TiB         |1 TiB|
|Maximale IOPS per bestand     |5,000         |1000|
|Gelijktijdige verwerkt    |2,000         |2,000|
|Inkomend verkeer  |300 MiB/sec|      Waarden voor de doorvoer standaard-bestand|
|Uitgaand verkeer   |200 Mib/sec| Waarden voor de doorvoer standaard-bestand|
|Doorvoer| Waarden voor de premium-bestand Inkomend/uitgaand verkeer| Maximaal 60 MiB/sec|
---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/20/2019
ms.author: tamram
ms.openlocfilehash: 2319a7620b70547d186a4ef5cb96f5ca6684c62c
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67305275"
---
| Resource | Doel |
|----------|---------------|
| Maximale grootte van één tabel | 500 TiB |
| Maximale grootte van een tabel-entiteit | 1 MiB |
| Maximum aantal eigenschappen in een tabel-entiteit | 255, waaronder drie Systeemeigenschappen: PartitionKey en RowKey Timestamp |
| Maximale totale grootte van de waarden van de eigenschap in een entiteit | 1 MiB |
| Maximale totale grootte van een afzonderlijke eigenschap in een entiteit | Is afhankelijk van de eigenschap van het type. Zie voor meer informatie, **eigenschaptypen** in [inzicht in het Table Service Data Model](/rest/api/storageservices/understanding-the-table-service-data-model). |
| Maximum aantal opgeslagen toegangsbeleid per tabel | 5 |
| Maximale aantal aanvragen per opslagaccount | 20.000 transacties per seconde, waarbij ervan wordt uitgegaan van een grootte van 1-KiB entiteit |
| Doeldoorvoer van een met één tabelpartitie (1 KiB-entiteiten) | Maximaal 2000 entiteiten per seconde |

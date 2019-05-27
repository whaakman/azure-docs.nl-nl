---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 04/02/2019
ms.author: wesmc
ms.openlocfilehash: 498a7ee28b9404d0733e4615f4df635a8c904b51
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66132818"
---
### <a name="cacheskuname"></a>cacheSKUName

De prijscategorie van de nieuwe Azure-Cache voor Redis.

```json
    "cacheSKUName": {
      "type": "string",
      "allowedValues": [
        "Basic",
        "Standard",
        "Premium"
      ],
      "defaultValue": "Basic",
      "metadata": {
        "description": "The pricing tier of the new Azure Cache for Redis."
      }
    },
```

De sjabloon definieert u de waarden die zijn toegestaan voor deze parameter (Basic, Standard of Premium) en een standaardwaarde (basis) toegewezen, als er geen waarde is opgegeven. Basic biedt een enkel knooppunt met meerdere grootten beschikbaar die maximaal beschikbaar tot 53 GB. Standard biedt twee knooppunten primair/Replica met verschillende formaten maximaal beschikbaar tot 53 GB en 99,9% SLA.

### <a name="cacheskufamily"></a>cacheSKUFamily

De familie voor de sku.

```json
    "cacheSKUFamily": {
      "type": "string",
      "allowedValue/s": [
        "C",
        "P"
      ],
      "defaultValue": "C",
      "metadata": {
        "description": "The family for the sku."
      }
    },
```

### <a name="cacheskucapacity"></a>cacheSKUCapacity

De grootte van de nieuwe Azure-Cache voor Redis-exemplaar.

Voor de Basic en Standard-families:

```json
    "cacheSKUCapacity": {
      "type": "int",
      "allowedValues": [
        0,
        1,
        2,
        3,
        4,
        5,
        6
      ],
      "defaultValue": 0,
      "metadata": {
        "description": "The size of the new Azure Cache for Redis instance. "
      }
    }
```

De Premium-capaciteit waarde cache is gedefinieerd hetzelfde, met uitzondering van de toegestane waarden van 1 tot en met 5 in plaats van van 0 tot en met 6 worden uitgevoerd.

De sjabloon definieert de waarden voor geheel getal dat is toegestaan voor deze parameter (0 tot en met 6 voor de Basic en Standard-families; 1 t/m 5 voor de Premium-familie). Als geen waarde opgeeft, wordt de standaardwaarde 0 in de sjabloon toegewezen voor basis en standaard, 1 voor Premium.

De waarden komen overeen met de volgende cachegrootte:

| Value | Basic en Standard<br>Cachegrootte | Premium<br>Cachegrootte |
| :---: | :------------------------------: | :-------------------: |
| 0     | 250 MB (standaard)                 | N.v.t.                   |
| 1     | 1 GB                             | 6 GB (standaard)        |
| 2     | 2,5 GB                           | 13 GB                 |
| 3     | 6 GB                             | 26 GB                 |
| 4     | 13 GB                            | 53 GB                 |
| 5     | 26 GB                            | 120 GB                |
| 6     | 53 GB                            | N.v.t.                   |

---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 11/21/2018
ms.author: wesmc
ms.openlocfilehash: 3252a6454bf3f70250d2d792ca1f36a819ab22bf
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53019768"
---
### <a name="cacheskuname"></a>cacheSKUName
De prijscategorie van de nieuwe Azure-Cache van Azure voor Redis.

    "cacheSKUName": {
      "type": "string",
      "allowedValues": [
        "Basic",
        "Standard"
      ],
      "defaultValue": "Basic",
      "metadata": {
        "description": "The pricing tier of the new Azure Azure Cache for Redis."
      }
    },

De sjabloon definieert u de waarden die zijn toegestaan voor deze parameter (Basic of Standard) en een standaardwaarde (basis) toegewezen, als er geen waarde is opgegeven. Basic biedt een enkel knooppunt met meerdere grootten beschikbaar die maximaal beschikbaar tot 53 GB.
Standard biedt twee knooppunten primair/Replica met verschillende formaten maximaal beschikbaar tot 53 GB en 99,9% SLA.

### <a name="cacheskufamily"></a>cacheSKUFamily
De familie voor de sku.

    "cacheSKUFamily": {
      "type": "string",
      "allowedValues": [
        "C"
      ],
      "defaultValue": "C",
      "metadata": {
        "description": "The family for the sku."
      }
    },


### <a name="cacheskucapacity"></a>cacheSKUCapacity
De grootte van de nieuwe Azure-Cache van Azure voor Redis-exemplaar. 

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
        "description": "The size of the new Azure Azure Cache for Redis instance. "
      }
    }


De sjabloon definieert u de waarden die zijn toegestaan voor deze parameter (0, 1, 2, 3, 4, 5 of 6), en wijst een standaardwaarde (0) als er geen waarde is opgegeven. Deze getallen komen overeen met de volgende cachegrootte: 0 = 250 MB, 1 = 1 GB, 2 = 2,5 GB, 3 = 6 GB, 4 = 13 GB, 5 = 26 GB, 6 = 53 GB


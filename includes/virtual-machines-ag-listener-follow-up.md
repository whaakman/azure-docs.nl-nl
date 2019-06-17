---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 8ae22ec7f75b9cd0d7958977dfd97169706c9389
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66165426"
---
Nadat u de beschikbaarheidsgroep-listener maakt, kan het nodig zijn om aan te passen van de parameters van de cluster RegisterAllProvidersIP en HostRecordTTL voor de listener-resource zijn. Deze parameters kunnen opnieuw verbinden tijd na een failover, waardoor verbindingstime-outs mogelijk beperken. Zie voor meer informatie over deze parameters, evenals voorbeeldcode [maken of configureren van een beschikbaarheidsgroep-listener](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover).


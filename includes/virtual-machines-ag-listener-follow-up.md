---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 8ae22ec7f75b9cd0d7958977dfd97169706c9389
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50227042"
---
Nadat u de beschikbaarheidsgroep-listener maakt, kan het nodig zijn om aan te passen van de parameters van de cluster RegisterAllProvidersIP en HostRecordTTL voor de listener-resource zijn. Deze parameters kunnen opnieuw verbinden tijd na een failover, waardoor verbindingstime-outs mogelijk beperken. Zie voor meer informatie over deze parameters, evenals voorbeeldcode [maken of configureren van een beschikbaarheidsgroep-listener](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover).


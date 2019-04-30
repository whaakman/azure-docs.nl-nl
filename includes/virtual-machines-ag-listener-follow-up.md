---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: 8ae22ec7f75b9cd0d7958977dfd97169706c9389
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62097650"
---
Nadat u de beschikbaarheidsgroep-listener maakt, kan het nodig zijn om aan te passen van de parameters van de cluster RegisterAllProvidersIP en HostRecordTTL voor de listener-resource zijn. Deze parameters kunnen opnieuw verbinden tijd na een failover, waardoor verbindingstime-outs mogelijk beperken. Zie voor meer informatie over deze parameters, evenals voorbeeldcode [maken of configureren van een beschikbaarheidsgroep-listener](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover).

<!-- Update_Description: update meta properties -->
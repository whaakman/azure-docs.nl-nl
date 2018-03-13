---
title: Failover naar Azure fouten oplossen | Microsoft Docs
description: In dit artikel wordt beschreven hoe het oplossen van veelvoorkomende fouten in Azure worden niet via
services: site-recovery
documentationcenter: 
author: ponatara
manager: abhemraj
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 03/09/2018
ms.author: ponatara
ms.openlocfilehash: 5c94e26c4639284f7e4c53d924f16040118d996c
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2018
---
# <a name="troubleshoot-errors-when-failing-over-a-virtual-machine-to-azure"></a>Fouten bij failover van een virtuele machine in Azure oplossen
U wordt een van de volgende fouten tijdens de failover van een virtuele machine in Azure. Voor het oplossen, kunt u de stappen beschreven voor elke fout.


## <a name="failover-failed-with-error-id-28031"></a>Failover is mislukt met fout-ID 28031

Site Recovery is niet een mislukte maken via de virtuele machine in Azure. Dit kan gebeuren vanwege een van de volgende redenen:

* Is er niet voldoende quotum beschikbaar voor het maken van de virtuele machine: U kunt de beschikbare quota controleren door te gaan naar de abonnement -> Gebruik + quota's. U kunt openen een [nieuw ondersteuningsverzoek](http://aka.ms/getazuresupport) het quotum te verhogen.

* U probeert te failover virtuele machines van verschillende grootte families in dezelfde beschikbaarheidsset. Zorg ervoor dat u dezelfde familie van de grootte voor alle virtuele machines in dezelfde beschikbaarheidsset kiezen. Grootte wijzigen door te gaan naar berekenings- en instellingen van de virtuele machine en probeer vervolgens de failover.

* Er is een beleid voor het abonnement dat voorkomt het maken van een virtuele machine. Wijzig het beleid voor het toestaan van het maken van een virtuele machine en probeer de failover.

## <a name="failover-failed-with-error-id-28092"></a>Failover is mislukt met fout-ID 28092

Site Recovery is niet een netwerkinterface voor de mislukte maken via de virtuele machine. Zorg ervoor dat er voldoende quotum beschikbaar voor het maken van netwerkinterfaces in het abonnement. U kunt de beschikbare quota controleren door te gaan naar de abonnement -> Gebruik + quota's. U kunt openen een [nieuw ondersteuningsverzoek](http://aka.ms/getazuresupport) het quotum te verhogen. Als er voldoende quotum, wordt dit wordt mogelijk een onregelmatige uitgeven, probeer het opnieuw. Als het probleem zich blijft zelfs na verschillende pogingen voordoen, laat u een opmerking aan het einde van dit document.  

## <a name="failover-failed-with-error-id-70038"></a>Failover is mislukt met fout-ID 70038

Site Recovery is niet een mislukte maken via de klassieke virtuele machine in Azure. Deze fout kan optreden omdat:

* Een van de resources, zoals een virtueel netwerk dat is vereist voor de virtuele machine moet worden gemaakt bestaat niet. Maken van het virtuele netwerk, zoals bepaald in de berekenings- en instellingen van de virtuele machine of wijzig de instelling voor een virtueel netwerk dat al bestaat en voer vervolgens de failover opnieuw uit.


## <a name="next-steps"></a>Volgende stappen

Als u meer hulp nodig hebt, moet u vervolgens uw query boeken op [Site Recovery-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr) of laat een opmerking aan het einde van dit document. We hebben een actieve community dat moet kunnen om u te helpen.

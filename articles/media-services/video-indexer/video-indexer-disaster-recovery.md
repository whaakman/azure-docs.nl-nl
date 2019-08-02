---
title: Video Indexer bedrijfs continuïteit en herstel na nood gevallen (BCDR)-Azure | Microsoft Docs
description: Meer informatie over hoe u een failover naar een secundaire Video Indexer account kunt doen als er sprake is van een storing in een regionaal Data Center.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 07/29/2019
ms.author: juliako
ms.openlocfilehash: ce788b4640f0a6c6f25b3280ce4f52fd018d1699
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68668304"
---
# <a name="handle-video-indexer-business-continuity-and-disaster-recovery"></a>Video Indexer bedrijfs continuïteit en herstel na nood gevallen afhandelen

Azure Media Services Video Indexer biedt geen onmiddellijke failover van de service als er sprake is van een storing in het lokale Data Center of een storing. In dit artikel wordt uitgelegd hoe u uw omgeving kunt configureren voor een failover om optimale Beschik baarheid van toepassingen en een geminimaliseerd herstel tijdstip te garanderen als er sprake is van een nood geval.

We raden u aan om bedrijfs continuïteit nood herstel (BCDR) over regionale paren te configureren om te profiteren van het isolatie-en beschikbaarheids beleid van Azure. Zie [gekoppelde Azure-regio's](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)voor meer informatie.

## <a name="prerequisites"></a>Vereisten 

Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, meldt u zich aan voor een [gratis proef versie van Azure](https://azure.microsoft.com/free/).

## <a name="failover-to-a-secondary-account"></a>Failover naar een secundair account

Voor het implementeren van BCDR moet u twee Video Indexer accounts hebben om redundantie te kunnen afhandelen.

1. Maak twee Video Indexer accounts die zijn verbonden met Azure (Zie [accounts maken](connect-to-azure.md)). Een voor de primaire regio en de andere voor de gekoppelde Azure-regio. 
1. Als er een fout optreedt in de primaire regio, schakelt u over naar indexeren met het secundaire account.

> [!TIP]
> U kunt BCDR automatiseren door waarschuwingen voor activiteiten Logboeken in te stellen voor service status meldingen per [activiteit logboek waarschuwingen maken voor service meldingen](../../service-health/alerts-activity-log-service-notifications.md).

Zie [meerdere tenants beheren](manage-multiple-tenants.md)voor meer informatie over het gebruik van meerdere tenants. Als u BCDR wilt implementeren, kiest u een van de volgende twee opties: [Video indexer account per Tenant](manage-multiple-tenants.md#video-indexer-account-per-tenant) of [Azure-abonnement per Tenant](manage-multiple-tenants.md#azure-subscription-per-tenant).

## <a name="next-steps"></a>Volgende stappen

[Een video indexer account beheren dat is verbonden met Azure](manage-account-connected-to-azure.md).

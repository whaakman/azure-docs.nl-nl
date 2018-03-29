---
title: Azure Advisor kosten aanbevelingen | Microsoft Docs
description: Gebruik Azure Advisor om te optimaliseren van de kosten van uw Azure-implementaties.
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 7a8807a580f1a7f1fe67e026a8fbd4cc0e96c41c
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="advisor-cost-recommendations"></a>Advisor kosten aanbevelingen

Advisor helpt u te optimaliseren en reduceren uw algehele Azure hoeven te besteden aan met het vaststellen van niet-actieve- en onderbenutte resources. U kunt ophalen kosten aanbevelingen van de **kosten** op het dashboard Advisor.

## <a name="optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances"></a>Optimaliseren te besteden aan de virtuele machine door afsluiten onderbenutte exemplaren of vergroten of verkleinen 
Hoewel bepaalde toepassingsscenario's leiden minder gebruik standaard tot kunnen, kunt u vaak geld besparen door het beheer van de grootte en het nummer van uw virtuele machines. Advisor bewaakt het gebruik van uw virtuele machine 14 dagen en identificeert dan laag gebruik virtuele machines. Virtuele machines waarvan CPU-gebruik 5 is % of minder en netwerkgebruik is 7 MB of minder voor vier of meer dagen worden beschouwd als laag gebruik virtuele machines.

Advisor ziet u de geschatte kosten van u wilt doorgaan met het uitvoeren van uw virtuele machine, zodat u kunt deze afsluiten of het formaat.

Als u worden agressievere wilt op onderbenutte virtuele machines te identificeren, kunt u het gemiddelde CPU-gebruik regel op basis van per abonnement kunt aanpassen.

## <a name="reduce-costs-by-eliminating-unprovisioned-expressroute-circuits"></a>Doordat de inrichting van ExpressRoute-circuits kosten te verlagen
Advisor identificeert ExpressRoute-circuits die zijn aangebracht in de providerstatus van *niet ingericht* voor meer dan één maand en raadt het circuit verwijderen als u van plan zijn niet het circuit inrichten met de verbinding in orde provider.

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a>Toegang tot de aanbevelingen in Azure Advisor kosten

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open vervolgens [Advisor](https://aka.ms/azureadvisordashboard).

2.  Klik op het dashboard Advisor de **kosten** tabblad.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Advisor aanbevelingen:
* [Inleiding tot Advisor](advisor-overview.md)
* [Aan de slag](advisor-get-started.md)
* [Aanbevelingen van Advisor-prestaties](advisor-cost-recommendations.md)
* [Hoge beschikbaarheid aanbevelingen Advisor te ontvangen](advisor-cost-recommendations.md)
* [Aanbevelingen voor beveiliging van Advisor](advisor-cost-recommendations.md)

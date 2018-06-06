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
ms.openlocfilehash: ade6ef996c00c0c06d5b8e44815520e6e4ab7e9f
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34735864"
---
# <a name="advisor-cost-recommendations"></a>Advisor kosten aanbevelingen

Advisor helpt u te optimaliseren en reduceren uw algehele Azure hoeven te besteden aan met het vaststellen van niet-actieve- en onderbenutte resources. U kunt ophalen kosten aanbevelingen van de **kosten** op het dashboard Advisor.

## <a name="optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances"></a>Optimaliseren te besteden aan de virtuele machine door afsluiten onderbenutte exemplaren of vergroten of verkleinen 
Hoewel bepaalde toepassingsscenario's leiden minder gebruik standaard tot kunnen, kunt u vaak geld besparen door het beheer van de grootte en het nummer van uw virtuele machines. Advisor bewaakt het gebruik van uw virtuele machine 14 dagen en identificeert dan laag gebruik virtuele machines. Virtuele machines waarvan CPU-gebruik 5 is % of minder en netwerkgebruik is 7 MB of minder voor vier of meer dagen worden beschouwd als laag gebruik virtuele machines.

Advisor ziet u de geschatte kosten van u wilt doorgaan met het uitvoeren van uw virtuele machine, zodat u kunt deze afsluiten of het formaat.

Als u worden agressievere wilt op onderbenutte virtuele machines te identificeren, kunt u het gemiddelde CPU-gebruik regel op basis van per abonnement kunt aanpassen.

## <a name="reduce-costs-by-eliminating-unprovisioned-expressroute-circuits"></a>Doordat de inrichting van ExpressRoute-circuits kosten te verlagen
Advisor identificeert ExpressRoute-circuits die zijn aangebracht in de providerstatus van *niet ingericht* voor meer dan één maand en raadt het circuit verwijderen als u van plan zijn niet het circuit inrichten met de verbinding in orde provider.

## <a name="buy-virtual-machine-reserved-instances-to-save-money-over-pay-as-you-go-costs"></a>Gereserveerde instanties van virtuele machines kopen om te besparen ten opzichte van betaling per gebruik
Advisor wordt uw gebruik van virtuele machines controleren in de afgelopen 30 dagen en Bepaal als u geld besparen kan door aan te schaffen gereserveerde exemplaren. Advisor ziet u de regio's en grootten waar u mogelijk hebt de meeste besparingen en ziet u de geschatte besparingen van het aanschaffen van een gereserveerde exemplaren. 

U kunt vooraf de basiskosten voor uw virtuele machines met gereserveerde exemplaren aanschaffen. Kortingen automatisch van toepassing op nieuwe of bestaande virtuele machines die de grootte en de regio als uw gereserveerde exemplaren hebben. [Meer informatie over Azure gereserveerde VM-exemplaren.](https://azure.microsoft.com/pricing/reserved-vm-instances/)

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

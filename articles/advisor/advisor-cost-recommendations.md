---
title: Service verlagen met behulp van Azure Advisor | Microsoft Docs
description: Met Azure Advisor kunt verlagen de kosten van uw Azure-implementaties.
services: advisor
documentationcenter: NA
author: kasparks
ms.service: advisor
ms.topic: article
ms.date: 01/29/2019
ms.author: kasparks
ms.openlocfilehash: cc5c4310d5f9886518bee6612113fba174130bec
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56648713"
---
# <a name="reduce-service-costs-using-azure-advisor"></a>Service verlagen met behulp van Azure Advisor

Advisor helpt u te optimaliseren en uw totale Azure verminderen besteden door niet-actieve en onderbenutte resources te identificeren. U kunt u kosten aanbevelingen van de **kosten** tabblad op de Advisor-dashboard.

## <a name="optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances"></a>Optimaliseer virtuele machine uitgaven per vergroten of verkleinen of weinig gebruikte exemplaren afsluiten 

Hoewel bepaalde toepassingsscenario's in laag gebruik standaard resulteren kunnen, kunt u vaak geld besparen door het beheer van de grootte en het nummer van uw virtuele machines. Advisor bewaakt het gebruik van uw virtuele machine 7 dagen en identificeert het vervolgens laag gebruik virtuele machines. Virtuele machines worden beschouwd als laag-gebruik als het CPU-gebruik 5 is % of minder en hun netwerkgebruik is minder dan 2% of als de huidige werkbelasting kan worden aangepast door een kleinere virtuele machine.

Advisor geeft u de geschatte kosten van u verdergaat met het uitvoeren van uw virtuele machine, zodat u kunt deze wordt afgesloten of wijzig de grootte ervan.

Als u wilt agressiever ten grondslag ligt weinig gebruikte virtuele machines moet worden, kunt u de gemiddelde CPU-gebruik regel op basis van een abonnement aanpassen.

## <a name="reduce-costs-by-eliminating-unprovisioned-expressroute-circuits"></a>Kosten verlagen door het elimineren van niet-ingerichte ExpressRoute-circuits

Advisor identificeert ExpressRoute-circuits die in de status van de provider van zijn *niet ingericht* voor meer dan één maand en de gebruiker wordt aanbevolen het circuit verwijderen als u niet van plan bent voor het inrichten van het circuit met de verbinding de provider.

## <a name="reduce-costs-by-deleting-or-reconfiguring-idle-virtual-network-gateways"></a>Kosten verlagen door te verwijderen of opnieuw configureren van niet-actieve virtuele netwerkgateways

Advisor identificeert virtuele netwerk-poorten die meer dan 90 dagen inactief zijn geweest. Omdat deze gateways worden per uur gefactureerd, moet u rekening houden met opnieuw configureren of te verwijderen als u niet wilt dat ze niet meer gebruiken. 

## <a name="buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs"></a>Gereserveerde virtuele machine-instanties om geld te besparen ten opzichte van betalen per gebruik kopen

Advisor wordt uw gebruik van virtuele machines in de afgelopen 30 dagen bekijken en te bepalen als u geld besparen kunt door het aanschaffen van een Azure-reservering. Advisor ziet u de regio's en -grootten waar u mogelijk hebt de meeste besparingen en ziet u de geschatte besparing bij de aanschaf van reserveringen. 

Met Azure reserveringen, kunt u de basiskosten voor het vooraf kunt kopen voor uw virtuele machines. Kortingen wordt automatisch toegepast op nieuwe of bestaande virtuele machines met dezelfde grootte en regio als uw reserveringen. [Meer informatie over Azure Reserved VM Instances.](https://azure.microsoft.com/pricing/reserved-vm-instances/)

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a>Toegang tot kosten aanbevelingen in Azure Advisor

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open vervolgens [Advisor](https://aka.ms/azureadvisordashboard).

2.  Klik op de Advisor-dashboard op de **kosten** tabblad.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over aanbevelingen van Advisor:
* [Inleiding tot Advisor](advisor-overview.md)
* [Aan de slag](advisor-get-started.md)
* [Advisor-aanbevelingen voor prestaties](advisor-cost-recommendations.md)
* [Advisor-aanbevelingen voor hoge beschikbaarheid](advisor-cost-recommendations.md)
* [Advisor-aanbevelingen voor beveiliging](advisor-cost-recommendations.md)

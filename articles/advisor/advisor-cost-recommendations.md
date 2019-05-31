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
ms.openlocfilehash: fef7b82e6969de16d1815250d2373c99021b0e86
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/28/2019
ms.locfileid: "66254723"
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

Advisor wordt uw gebruik van virtuele machines in de afgelopen 30 dagen bekijken en te bepalen als u geld besparen kunt door het aanschaffen van een Azure-reservering. Advisor ziet u de regio's en -grootten waar u mogelijk hebt de meeste besparingen en ziet u de geschatte besparing bij de aanschaf van reserveringen. Met Azure reserveringen, kunt u de basiskosten voor het vooraf kunt kopen voor uw virtuele machines. Kortingen wordt automatisch toegepast op nieuwe of bestaande virtuele machines met dezelfde grootte en regio als uw reserveringen. [Meer informatie over Azure Reserved VM Instances.](https://azure.microsoft.com/pricing/reserved-vm-instances/)

Advisor wordt ook een melding van gereserveerde instanties die u hebt die in de komende 30 dagen verloopt. Het wordt aangeraden dat u nieuwe gereserveerde instanties koopt om te voorkomen dat de prijzen voor betalen per gebruik betalen.

## <a name="delete-unassociated-public-ip-addresses-to-save-money"></a>Verwijderen van niet-gekoppelde openbare IP-adressen om geld te besparen

Advisor identificeert openbare IP-adressen die niet momenteel gekoppeld aan Azure-resources zoals Load Balancers of virtuele machines zijn. Deze openbare IP adressen worden geleverd met een nominaal bedrag in rekening gebracht. Als u niet van plan bent om ze te gebruiken, kan deze worden verwijderd leiden tot kostenbesparingen.

## <a name="delete-azure-data-factory-pipelines-that-are-failing"></a>Azure Data Factory-pijplijnen die niet verwijderen

Azure Advisor detecteert Azure Data Factory-pijplijnen die herhaaldelijk mislukken en raadzaam dat u de problemen oplossen of verwijderen van de pijplijnen mislukken als ze niet meer nodig zijn. U wordt gefactureerd voor deze pijplijnen, zelfs als hoewel ze niet u van dienst zijn terwijl ze mislukken. 

## <a name="use-standard-snapshots-for-managed-disks"></a>Standard-momentopnamen voor beheerde schijven gebruiken
Om op te slaan 60% van de kosten, wordt u aangeraden de momentopnamen opslaan in de Standard-opslag, ongeacht het opslagtype van de bovenliggende schijf. Dit is de standaardoptie voor momentopnamen van Managed Disks. Azure Advisor identificeert momentopnamen die zijn opgeslagen Premium-opslag en aanbevolen migreren van de momentopname van Premium naar Standard-opslag. [Meer informatie over de prijzen voor Managed Disk](https://aka.ms/aa_manageddisksnapshot_learnmore)

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

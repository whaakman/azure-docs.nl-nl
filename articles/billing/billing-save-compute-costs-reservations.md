---
title: Geld besparen door virtuele machines in Azure vooraf betalen - Azure | Microsoft Docs
description: Meer informatie over Azure gereserveerde virtuele Machine-exemplaar op te slaan op de kosten van uw virtuele machines.
services: billing
documentationcenter: 
author: vikramdesai01
manager: vikramdesai01
editor: 
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2017
ms.author: vikdesai
ms.openlocfilehash: 96e9cf2fed0b22fd7aa7b9ffeab0e94738ce510d
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="save-money-on-virtual-machines-with-reserved-virtual-machine-instances"></a>Geld besparen op virtuele machines met een gereserveerde virtuele Machine-exemplaren 
Gereserveerde virtuele Machine-exemplaren kunt u vooraf betalen voor één jaar of drie jaar rekencapaciteit korting te krijgen op de virtuele machines die u gebruikt. De kosten van de virtuele machine op de aanzienlijk wordt gereduceerd — tot 72 procent op betalen naar gebruik prijzen – met vooraf inzet van één jaar of drie jaar. Gereserveerde virtuele Machine-exemplaren is een korting van facturering en heeft geen invloed op de runtimestatus van virtuele machines.

U kunt een exemplaar van de gereserveerde virtuele Machine kopen de [Azure-portal](https://aka.ms/reservations). Zie voor meer informatie [Vooruitbetalen voor virtuele machines en geld besparen met gereserveerde virtuele Machine-exemplaren](https://go.microsoft.com/fwlink/?linkid=861721).

## <a name="why-should-i-buy-a-reserved-virtual-machine-instance"></a>Waarom zou ik aanschaffen van een exemplaar van de gereserveerde virtuele Machine?
Als u virtuele machines die worden uitgevoerd voor langere tijd hebt, kunt aanschaffen van een exemplaar van de gereserveerde virtuele Machine u de beste effectieve prijs. Bijvoorbeeld, als u vier exemplaren van standaard D2 continu in de regio VS-West uitvoeren, zonder een reservering u worden in rekening gebracht volgens de tarieven voor betalen-als-Ga-you. Als u een exemplaar van de gereserveerde virtuele Machine voor de vier VMs koopt, u de virtuele machines onmiddellijk de facturering profiteren. Ze zijn niet meer in rekening gebracht volgens de pay-as-you Ga tarieven. 

## <a name="what-charges-does-a-reserved-virtual-machine-instance-cover"></a>Welke kosten heeft betrekking op een exemplaar van de gereserveerde virtuele Machine?
Een reservering bevat alleen informatie over de kosten van de virtuele machine infrastructuur voor uw virtuele machines van Windows of Linux. Een reservering omvatten geen extra kosten voor software-, netwerk- of opslag. Voor Windows virtuele machines, kunt u de licentiekosten met Windows dekt [Azure hybride voordeel](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reserved-virtual-machine-instance"></a>Wie is in aanmerking voor een exemplaar van de gereserveerde virtuele Machine?
Azure-klanten met deze typen abonnementen kunnen aanschaffen van een exemplaar van de gereserveerde virtuele Machine:
-   Enterprise agreement type abonnementsaanbod (MS-AZR - 0017P).
-   [Betalen naar gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/) type abonnementsaanbod (MS-AZR - 003 P).
U moet zich in de 'Eigenaar'-rol op het abonnement op een gereserveerde exemplaar kopen. Op de aanschaf van reserveringen in een enterprise-inschrijving moet de ondernemingsbeheerder reservering aankopen in de portal EA inschakelen standaard die de instelling is ingeschakeld.

## <a name="how-is-a-reserved-virtual-machine-instances-purchase-billed"></a>Hoe wordt de aankoop van een gereserveerde virtuele Machine-exemplaren in rekening gebracht?
De reservering aankoop wordt verrekend met de betalingsmethode die is gekoppeld aan het abonnement. Als u een Enterprise-abonnement hebt, wordt de reservering kosten van het saldo van uw bedrag afgetrokken. Als uw saldo bedrag dekt de kosten van de reservering niet, bent u gefactureerd de overschrijding.
Als u een abonnement met betalen naar gebruik hebt, wordt de creditcard die u voor uw account hebt onmiddellijk gefactureerd. Als u wordt gefactureerd per factuur, ziet u de kosten op uw volgende factuur.

## <a name="how-is-the-purchased-reserved-virtual-machine-instance-discount-applied"></a>Hoe wordt de gekochte exemplaar van de gereserveerde virtuele Machine korting toegepast?
De gereserveerde virtuele Machine exemplaar korting geldt voor de virtuele machines die overeenkomen met de kenmerken die u selecteert bij de aankoop van de reservering. De kenmerken omvatten het bereik waarin de overeenkomende virtuele machines worden uitgevoerd. Bijvoorbeeld, als u een korting gereserveerde VM-instantie voor vier standaard D2 virtuele machines in de regio VS-West wilt, selecteer het abonnement waar de virtuele machines worden uitgevoerd. Als de virtuele machines worden uitgevoerd in verschillende abonnementen in uw registratie-/ account, selecteert u het bereik als gedeeld. Gedeeld bereik kan de reservering korting moet worden toegepast voor abonnementen.
Nadat u een gereserveerde VM-instantie hebt gekocht, kunt u het bereik wijzigen. Zie documentatie over het beheren van reserveringen wijzigen van het bereik.

De reservering korting geldt alleen voor virtuele machines in abonnementen met enterprise- of typen van de aanbieding betalen naar gebruik. Virtuele machines die worden uitgevoerd in een abonnement met andere aanbiedingstypen ontvangen niet de reservering korting. Enterprise ontwikkelen en testen abonnementen zijn niet voor enterprise-inschrijvingen in aanmerking komen voor de gereserveerde exemplaar voordelen.

Hoe reservering is van invloed op de virtuele machine facturering wordt uitgelegd in [inzicht in de toepassing van de reservering facturering voordeel](https://go.microsoft.com/fwlink/?linkid=863405).

## <a name="what-happens-when-the-reservation-term-expires"></a>Wat gebeurt er wanneer de term reservering is verlopen?
Aan het einde van de termijn reservering, de facturering korting verlopen en de infrastructuur van de virtuele machine wordt in rekening gebracht tegen de pay-as-you Ga prijs. Reserveringen verlengd niet automatisch. Als u wilt doorgaan met het ophalen van de facturering korting, moet u een nieuw exemplaar van gereserveerde virtuele Machine aanschaffen. 

## <a name="sizes-and-regional-availability"></a>Grootten en regionale beschikbaarheid
Reserveringen zijn beschikbaar voor de meeste VM-groottes met enkele uitzonderingen:
- Preview VM-grootten: de grootte die in de preview zijn niet beschikbaar voor de aankoop van de instantie van de gereserveerde virtuele Machine.
- Clouds – gereserveerde virtuele Machine-exemplaren zijn niet beschikbaar voor de aankoop van het Azure US Government, Duitsland of China regio's. 
- Onvoldoende quota: Er is een gereserveerde virtuele machine exemplaar dat is afgestemd op één abonnement moet vCPU quotum beschikbaar in het abonnement voor de nieuwe k hebben. Bijvoorbeeld, als het doelabonnement een quotumlimiet van 10 vcpu's voor de D-reeks familie heeft, kan geen u koop een gereserveerde VM-instantie voor 11 Standard_D1 exemplaren. De controle van de quota voor reserveringen bevat de virtuele machines die al zijn geïmplementeerd in het abonnement. Bijvoorbeeld als het abonnement heeft een quotum van 10 vcpu's voor de D-reeks familie. Als dit abonnement twee standard_D1 instanties zijn geïmplementeerd heeft, kunt u een exemplaar van de gereserveerde virtuele machine voor 10 standard_D1 exemplaren in dit abonnement kopen. 
- Capaciteitsbeperkingen – In zeldzame gevallen, Azure limieten aanschaf van nieuwe reserveringen voor een subset van VM-grootte en als gevolg van onvoldoende capaciteit in een regio.

## <a name="next-steps"></a>Volgende stappen
Start opslaan op uw virtuele machines door het aanschaffen van een [exemplaar van de gereserveerde virtuele Machine](https://go.microsoft.com/fwlink/?linkid=861721). 

Als u hulp nodig hebt, moet [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.

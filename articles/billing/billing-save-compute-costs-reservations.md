---
title: Wat zijn gereserveerd Azure-exemplaren? | Microsoft Docs
description: Meer informatie over Azure gereserveerde VM-instanties en prijzen om op de kosten van uw virtuele machines opslaan en krijgt de beste effectieve prijs VM.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: yashar
ms.openlocfilehash: 12fc88596b4283b6f809575328ab801704cc1c8d
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063864"
---
# <a name="what-are-azure-reserved-vm-instances"></a>Wat zijn gereserveerd VM-exemplaren van Azure?
[Azure VM-instanties van gereserveerde](https://azure.microsoft.com/pricing/reserved-vm-instances) helpt u geld besparen door vooraf betalen voor één jaar of drie jaar rekencapaciteit zodat u korting te krijgen op de virtuele machines die u gebruikt. Azure gereserveerde-exemplaren kunnen uw virtuele machine aanzienlijk verlagen — tot 72 procent op betalen naar gebruik prijzen – met vooraf inzet van één jaar of drie jaar. Gereserveerde exemplaren facturering korting geven en hebben geen invloed op de runtimestatus van uw virtuele machines.

U kunt een gereserveerde exemplaar (k) kopen de [Azure-portal](https://aka.ms/reservations). Zie voor meer informatie [Vooruitbetalen voor virtuele machines en geld met gereserveerde exemplaren besparen](https://go.microsoft.com/fwlink/?linkid=861721).

## <a name="why-should-i-buy-a-reserved-instance"></a>Waarom zou ik een gereserveerde exemplaar aanschaffen?
Als u virtuele machines die worden uitgevoerd voor langere tijd hebt, biedt aanschaffen van een gereserveerde exemplaar u de meest rendabele optie. Bijvoorbeeld, als u vier exemplaren van een standaard D2 VM continu in de regio VS-West uitvoeren, zonder een gereserveerde exemplaar u worden in rekening gebracht volgens de tarieven voor betalen naar gebruik. Als u een gereserveerde exemplaar voor de vier VMs koopt, u de virtuele machines onmiddellijk de facturering profiteren. Ze zijn niet meer in rekening gebracht volgens de tarieven voor betalen naar gebruik. 

## <a name="what-charges-does-a-reserved-instance-cover"></a>Een gereserveerde exemplaar behandeld welke kosten worden gebruikt?
Een gereserveerde exemplaar bevat alleen informatie over de kosten van de virtuele machine infrastructuur voor uw virtuele machines van Windows of Linux. Een gereserveerde exemplaar omvatten geen extra kosten voor software-, netwerk- of opslag. Voor Windows virtuele machines, kunt u de licentiekosten met Windows dekt [Azure hybride voordeel](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reserved-instance"></a>Wie is in aanmerking voor een gereserveerde exemplaar?
Met deze typen abonnementen Azure-klanten kunnen een gereserveerde exemplaar aanschaffen:
-   Enterprise agreement type abonnementsaanbod (MS-AZR - 0017P).
-   [Betalen naar gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/) type abonnementsaanbod (MS-AZR - 003 P). U moet zich in de 'Eigenaar'-rol op het abonnement op een gereserveerde exemplaar kopen. Op de aanschaf van gereserveerde instanties in een enterprise-inschrijving moet de ondernemingsbeheerder aankopen gereserveerde exemplaar in de portal EA inschakelen. Deze instelling is standaard ingeschakeld.
-   Cloud Solution Provider (CSP) partners Azure Portal kunnen gebruiken of [Partnercentrum](https://docs.microsoft.com/partner-center/azure-reservations) om aan te schaffen gereserveerde exemplaren.

## <a name="how-is-a-reserved-instance-purchase-billed"></a>Hoe wordt de aanschaf van een gereserveerde exemplaar gefactureerd?
De gereserveerde exemplaar aankoop wordt verrekend met de betalingsmethode die is gekoppeld aan het abonnement. Als u een Enterprise-abonnement hebt, wordt de kosten gereserveerde exemplaar van het saldo van uw bedrag afgetrokken. Als uw saldo bedrag niet de kosten van het gereserveerde exemplaar dekt, bent u gefactureerd de overschrijding.
Als u een abonnement met betalen naar gebruik hebt, wordt de creditcard die u voor uw account hebt onmiddellijk gefactureerd. Als u wordt gefactureerd per factuur, ziet u de kosten op uw volgende factuur.

## <a name="how-is-the-purchased-reserved-instance-discount-applied"></a>Hoe wordt de gekochte gereserveerde exemplaar korting toegepast?
De gereserveerde exemplaar korting geldt voor de virtuele machines die overeenkomen met de kenmerken die u selecteert bij de aankoop van het gereserveerde exemplaar. De kenmerken omvatten het bereik waarin de overeenkomende virtuele machines worden uitgevoerd. Bijvoorbeeld, als u een korting gereserveerde exemplaar voor vier standaard D2 virtuele machines in de regio VS-West wilt, selecteer het abonnement waar de virtuele machines worden uitgevoerd. Als de virtuele machines worden uitgevoerd in verschillende abonnementen in uw registratie-/ account, selecteert u het bereik als gedeeld. Gedeeld bereik kan de gereserveerde exemplaar korting moet worden toegepast voor abonnementen. Nadat u een gereserveerde exemplaar hebt gekocht, kunt u het bereik wijzigen. Zie het wijzigen van het bereik [gereserveerde exemplaren in Azure beheren](billing-manage-reserved-vm-instance.md).

De gereserveerde exemplaar korting geldt alleen voor virtuele machines die zijn gekoppeld aan enterprise of betalen naar gebruik abonnement typen. Virtuele machines die worden uitgevoerd in een abonnement met andere aanbiedingstypen ontvangen niet de gereserveerde exemplaar korting. Enterprise ontwikkelen en testen abonnementen zijn niet voor enterprise-inschrijvingen in aanmerking komen voor de gereserveerde exemplaar voordelen.

Om beter te begrijpen hoe gereserveerde exemplaar is van invloed op de facturering voor uw virtuele machine, Zie [begrijpen hoe de gereserveerde exemplaar korting wordt toegepast](billing-understand-vm-reservation-charges.md).

## <a name="what-happens-when-the-reserved-instance-term-expires"></a>Wat gebeurt er wanneer de term gereserveerde exemplaar is verlopen?
Aan het einde van de termijn gereserveerde exemplaar, de facturering korting verlopen en de infrastructuur van de virtuele machine wordt in rekening gebracht tegen de pay-as-you Ga prijs. Azure-gereserveerde exemplaren verlengd niet automatisch. Als u wilt doorgaan met het ophalen van de facturering korting, moet u een nieuw exemplaar van de gereserveerde aanschaffen. 

## <a name="sizes-and-regional-availability"></a>Grootten en regionale beschikbaarheid
Gereserveerde exemplaren zijn beschikbaar voor de meeste VM-groottes met enkele uitzonderingen:
- Virtuele machines in Preview: een VM-serie of de grootte die in de preview zijn niet gereserveerd exemplaar aanschaffen.
- Clouds – gereserveerde instanties zijn niet beschikbaar voor de aankoop van het Azure US Government, Duitsland of China regio's. 
- Onvoldoende quota – een gereserveerde-exemplaar dat is afgestemd op één abonnement moet vCPU quotum beschikbaar in het abonnement voor de nieuwe k hebben. Bijvoorbeeld, als het doelabonnement een quotumlimiet van 10 vcpu's voor D-reeks heeft, kan geen u koop een gereserveerde exemplaar voor 11 Standard_D1 exemplaren. De controle van de quota voor gereserveerde exemplaren bevat de virtuele machines die al zijn geïmplementeerd in het abonnement. Bijvoorbeeld, als het abonnement een quotum van 10 vcpu's voor D-reeks heeft en twee standard_D1 instanties zijn geïmplementeerd heeft, kunt u koop een gereserveerde exemplaar voor 10 standard_D1 exemplaren in dit abonnement. 
- Capaciteit beperkingen – In zeldzame gevallen, Azure limieten voor de aanschaf van nieuwe gereserveerde exemplaren voor een subset van VM-groottes, vanwege onvoldoende capaciteit in een regio.

## <a name="next-steps"></a>Volgende stappen
Start opslaan op uw virtuele machines door het aanschaffen van een [gereserveerde exemplaar van Azure](https://go.microsoft.com/fwlink/?linkid=861721). 

Zie voor meer informatie over gereserveerde exemplaren, de volgende artikelen:

- [Wat zijn gereserveerd VM-exemplaren van Azure?](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Gereserveerde exemplaren in Azure beheren](billing-manage-reserved-vm-instance.md)
- [Begrijpen hoe de gereserveerde exemplaar korting wordt toegepast](billing-understand-vm-reservation-charges.md)
- [Gebruik van de gereserveerde exemplaar voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
- [Gereserveerde exemplaar gebruiksgegevens voor uw Enterprise enrollment begrijpen](billing-understand-reserved-instance-usage-ea.md)
- [Kosten voor Windows-software niet zijn opgenomen in de gereserveerde exemplaren](billing-reserved-instance-windows-software-costs.md)
- [Gereserveerde exemplaren in programma Partner Center Cloud Solution Provider (CSP)](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog steeds meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.

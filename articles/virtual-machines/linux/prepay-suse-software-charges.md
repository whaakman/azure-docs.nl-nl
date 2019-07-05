---
title: Betaal vooruit voor software - reserveringen voor Azure-abonnement
description: Meer informatie over hoe u kunt Vooruitbetalen voor software-abonnement om geld te besparen ten opzichte van uw betalen per gebruik.
author: bandersmsft
manager: yashesvi
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: c8e9d07bf01536d7865bd79f667a937037d96837
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67489887"
---
# <a name="prepay-for-azure-software-plans"></a>Vooruitbetalen voor Azure-softwareabonnementen

Wanneer u voor uw gebruik van de software SUSE- en Red Hat in Azure vooruitbetaalt, kunt u geld besparen ten opzichte van uw betalen per gebruik. De kortingen die gelden alleen voor SUSE- en Red Hat meters en niet op het gebruik van de virtuele machine. U kunt de reserveringen voor virtuele machines afzonderlijk voor nog meer ruimtebesparing kopen.

U kunt kopen SUSE- en Red Hat softwareabonnementen in Azure portal. Een abonnement aanschaffen:

- U moet de rol van eigenaar voor ten minste één Enterprise of afzonderlijk abonnement met betalen naar gebruik hebben.
- Voor Enterprise-abonnementen, de **gereserveerde instanties toevoegen** optie moet zijn ingeschakeld in de [EA-portal](https://ea.azure.com/). Als de instelling is uitgeschakeld, moet u een EA-beheerder voor het abonnement zijn.
- Voor het programma Cloud Solution Provider (CSP) kopen de beheerder agents of sales-agents de software-abonnement.

## <a name="buy-a-software-plan"></a>Een softwareabonnement kopen

1. Meld u aan bij Azure portal en Ga naar [reserveringen](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).
2. Klik op **toevoegen** en selecteer vervolgens het plan van de software die u wilt kopen.
Vul de vereiste velden in. Alle SUSE Linux-VM of RedHat VM die overeenkomt met de kenmerken van wat u koopt krijgen korting. Het werkelijke aantal implementaties die aan de korting is afhankelijk van het bereik en de hoeveelheid die is geselecteerd.
3. Selecteer een abonnement. Het wordt gebruikt om te betalen voor het abonnement.
De betalingswijze abonnement, wordt in rekening gebracht de kosten vooraf voor de reservering. Het abonnementstype moet een Enterprise Agreement (getallen bieden: MS-AZR-0017P of MS-AZR - 0148 P) of afzonderlijke overeenkomst met het betalen naar gebruik (getallen bieden: MS-AZR-0003P of MS-AZR-0023P).
    - Voor een Enterprise-abonnement worden de kosten in mindering gebracht op het toezeggingsbedrag of ze worden in rekening gebracht als overschrijding.
    - Voor een afzonderlijk abonnement met betalen per gebruik, de kosten worden in rekening gebracht op van het abonnement of creditcard of factuur betalingswijze wordt gebruikt.
4. Selecteer een bereik. Het bereik kan betrekking hebben op één abonnement of meerdere abonnementen (gedeeld bereik).
    - Enkel abonnement - korting van het abonnement wordt toegepast op de overeenkomende gebruik in het abonnement.
    - Gedeeld - wordt-korting van het plan toegepast op de overeenkomende-exemplaren in een abonnement in de context van de facturering. Voor zakelijke klanten, de context van de facturering van de inschrijving en bevat alle abonnementen in de registratie. De context van de facturering is voor afzonderlijke abonnement met betalen per gebruik prijzen klanten alle afzonderlijke abonnementen met betalen per gebruik prijzen abonnementen die zijn gemaakt door de accountbeheerder.
5. Selecteer een product om de VM-grootte en het afbeeldingstype te kiezen. De korting geldt voor de geselecteerde VM-grootte alleen.
6. Selecteer een termijn van één jaar of drie jaar.
7. Kies een volume, is het aantal vooruitbetaalde VM-exemplaren die de korting voor facturering krijgt.
8. Het product toevoegen aan de winkelwagen, beoordeling en inkoop.

De reserveringskorting wordt automatisch toegepast op de meter voor software die u vooraf betaalt. VM-rekenkosten in rekening gebracht niet zijn gekoppeld aan het abonnement. U kunt de VM-reserveringen afzonderlijk kopen.

## <a name="discount-applies-to-different-suse-vm-sizes"></a>Korting is van toepassing op andere SUSE VM-grootten

Plannen voor SUSE Linux bieden, zoals gereserveerde VM-instanties, instantieflexibiliteit grootte. Uw korting geldt zelfs wanneer u implementeert een virtuele machine die een andere grootte van de SUSE-planning die u hebt gekocht. Zie voor meer informatie, [te begrijpen hoe de korting voor software-plan wordt toegepast](../../billing/billing-understand-suse-reservation-charges.md).

## <a name="redhat-plan-discount"></a>Korting voor Red Hat-abonnement

Abonnementen zijn alleen beschikbaar voor Red Hat Enterprise Linux virtuele machines. De korting geldt niet voor virtuele machines van Red Hat Enterprise Linux SAP HANA of Red Hat Enterprise Linux SAP Business Apps-VM's.

Red Hat plan kortingen gelden alleen voor de VM-grootte die u hebt geselecteerd op het moment van aankoop. RHEL-abonnementen kunnen niet worden terugbetaald of die na de aankoop worden uitgewisseld.


## <a name="cancellation-and-exchanges-not-allowed"></a>Annulering en uitwisselingen niet toegestaan

U kunt geen annuleren of exchange een SUSE of Red Hat-abonnement dat u hebt gekocht. Controleer uw gebruik om er zeker van te zijn dat u het juiste abonnement koopt. Zie voor meer informatie om te bepalen wat u wilt kopen, [te begrijpen hoe de korting voor software-plan wordt toegepast](../../billing/billing-understand-suse-reservation-charges.md).

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het beheren van een reservering, [beheren Azure reserveringen](../../billing/billing-manage-reserved-vm-instance.md).

Zie de volgende artikelen voor meer informatie:

- [Wat zijn Azure-reserveringen?](../../billing/billing-save-compute-costs-reservations.md)
- [Reserveringen in Azure beheren](../../billing/billing-manage-reserved-vm-instance.md)
- [Begrijpen hoe de reserveringskorting SUSE wordt toegepast](../../billing/billing-understand-suse-reservation-charges.md)
- [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](../../billing/billing-understand-reserved-instance-usage.md)
- [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](../../billing/billing-understand-reserved-instance-usage-ea.md)

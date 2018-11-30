---
title: Plannen voor SUSE Linux - Azure-reserveringen kopen | Microsoft Docs
description: Ontdek hoe u vooruitbetaalt voor uw gebruik van SUSE en bespaart geld ten opzichte van de kosten voor betalen per gebruik.
services: virtual-machines-linux
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/29/2018
ms.author: yashar
ms.openlocfilehash: 44d23cf38eb1dca9ade94c5a8fd0ae495300be44
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52582187"
---
# <a name="prepay-for-suse-software-plans-from-azure-reservations"></a>Betaal vooruit voor SUSE-software plannen uit de Azure-reserveringen

Betaal vooruit voor uw gebruik van SUSE en bespaart geld ten opzichte van de kosten voor betalen per gebruik. De kortingen zijn alleen van toepassing op SUSE meters en niet op het gebruik van de virtuele machine. U kunt de reserveringen voor virtuele machines om nog meer besparen afzonderlijk te kopen.

U kunt plannen voor SUSE-software in Azure portal kunt kopen. Een abonnement aanschaffen:

- U moet zich in een rol van eigenaar voor ten minste één Enterprise of abonnement op gebruiksbasis.
- Voor Enterprise-abonnementen, aankopen in de reservering moeten zijn ingeschakeld in de [EA-portal](https://ea.azure.com).
- Voor het programma Cloud Solution Provider (CSP) kopen de beheerder agents of sales-agents de SUSE-abonnementen.

## <a name="buy-a-suse-software-plan"></a>Koop een abonnement SUSE-software

1. Ga naar [reserveringen](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) in Azure portal.
1. Selecteer **toevoegen** en selecteer SUSE Linux.
1. Vul de vereiste velden in. Alle SUSE Linux VM die overeenkomt met de kenmerken van wat u koopt de korting opgehaald. Het werkelijke aantal implementaties die aan de korting is afhankelijk van het bereik en de hoeveelheid die is geselecteerd.

    | Veld      | Beschrijving|
    |:------------|:--------------|
    |Naam        |De naam van deze aankoop.|
    |Abonnement|Het abonnement dat wordt gebruikt om te betalen voor dit plan. De betalingswijze voor het abonnement wordt in rekening gebracht de kosten vooraf voor de reservering. Het abonnement moet een Enterprise-overeenkomst (aanbieding: MS-AZR-0017P) of een Betalen-per-gebruik-abonnement (aanbieding: MS-AZR-0003P) zijn. Voor een Enterprise-abonnement worden de kosten in mindering gebracht op het toezeggingsbedrag of ze worden in rekening gebracht als overschrijding. Voor een Betalen per gebruik-abonnement worden de kosten in rekening gebracht op de creditcard of de factuurbetalingswijze van het abonnement.|
    |Bereik       |Het bereik kan betrekking hebben op één abonnement of meerdere abonnementen (gedeeld bereik). Als u selecteert: <ul><li>Eén abonnement - korting van het abonnement wordt toegepast op SUSE Linux-gebruik in dit abonnement. </li><li>Gedeeld - wordt korting van het abonnement toegepast op SUSE Linux-gebruik in alle abonnementen in de context van de facturering. Voor zakelijke klanten, gedeeld bereik is van de inschrijving en bevat alle abonnementen (met uitzondering van dev/test-abonnementen) binnen de inschrijving. Voor klanten van betalen per gebruik is het gedeelde bereik alle betalen per gebruik-abonnementen die zijn gemaakt door de accountbeheerder.</li></ul>|
    |Softwareabonnement     |Selecteer de SUSE Linux-plan. Zie [Begrijpen hoe de korting voor reservering van SUSE Linux Enterprise-software wordt toegepast](../../billing/billing-understand-suse-reservation-charges.md) voor meer informatie.|
    |VM-grootte     |Prijsinformatie voor SUSE Linux, is afhankelijk van het aantal vcpu's op de virtuele machine. Selecteer de optie die het aantal vcpu's op uw SUSE Linux VM's vertegenwoordigt.|
    |Termijn        |Één of drie jaar.|
    |Hoeveelheid    |Het aantal virtuele machines die u wilt dit plan SUSE Linux voor kopen. Het aantal is het aantal actieve instanties van SUSE Linux die de korting voor facturering krijgt.|
1. Selecteer **Aankoop**.
1. Selecteer **deze reservering weergeven** om te zien van de status van uw aankoop.

De reserveringskorting wordt automatisch toegepast op alle actieve SUSE virtuele machines die overeenkomt met de reservering. De korting geldt alleen voor de SUSE-meter. De VM-rekenkosten in rekening gebracht vallen niet onder dit plan.

## <a name="discount-applies-to-different-vm-sizes-with-instance-size-flexibility"></a>Korting geldt voor verschillende VM-grootten met een grootte-instantieflexibiliteit

Plannen voor SUSE Linux bieden, zoals gereserveerde VM-instanties, instantieflexibiliteit grootte. Dit betekent dat uw korting geldt zelfs wanneer het implementeren van een virtuele machine die een andere grootte van de SUSE-planning die u hebt gekocht. Zie voor meer informatie, [te begrijpen hoe de reserveringskorting van SUSE Linux Enterprise software wordt toegepast](../../billing/billing-understand-suse-reservation-charges.md).

## <a name="cancellation-and-exchanges-not-allowed"></a>Annulering en uitwisselingen niet toegestaan

Het is niet mogelijk om een SUSE-abonnement dat u hebt gekocht te annuleren of uit te wisselen. Controleer uw gebruik om er zeker van te zijn dat u het juiste abonnement koopt. Zie [Begrijpen hoe de korting voor reservering van SUSE Linux Enterprise-software wordt toegepast](../../billing/billing-understand-suse-reservation-charges.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het beheren van een reservering, [beheren Azure reserveringen](../../billing/billing-manage-reserved-vm-instance.md).

Zie de volgende artikelen voor meer informatie:

- [Wat zijn Azure-reserveringen?](../../billing/billing-save-compute-costs-reservations.md)
- [Reserveringen in Azure beheren](../../billing/billing-manage-reserved-vm-instance.md)
- [Begrijpen hoe de reserveringskorting SUSE wordt toegepast](../../billing/billing-understand-suse-reservation-charges.md)
- [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](../../billing/billing-understand-reserved-instance-usage.md)
- [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](../../billing/billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
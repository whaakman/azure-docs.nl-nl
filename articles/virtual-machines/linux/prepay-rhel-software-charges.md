---
title: Plannen voor Red Hat Enterprise Linux - Azure-reserveringen kopen | Microsoft Docs
description: Leer hoe u vooruitbetaalt voor uw gebruik van Red Hat en bespaart geld ten opzichte van de kosten voor betalen per gebruik.
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
ms.date: 01/18/2019
ms.author: yashar
ms.openlocfilehash: e90241d9da140ca33d1aae8ffc5636f8eb2fbc74
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58652953"
---
# <a name="prepay-for-red-hat-enterprise-linux-plans-from-azure-reservations"></a>Betaal vooruit voor Red Hat Enterprise Linux-serviceplannen van Azure-reserveringen

Betaal vooruit voor het gebruik van uw Red Hat en bespaart geld ten opzichte van de kosten voor betalen per gebruik. De kortingen zijn alleen van toepassing op Red Hat meters en niet op het gebruik van de virtuele machine. U kunt de reserveringen voor virtuele machines om nog meer besparen afzonderlijk te kopen.

U kunt plannen voor Red Hat-software in Azure portal kunt kopen. Een abonnement aanschaffen:

- U moet zich in een rol van eigenaar voor ten minste één Enterprise of abonnement op gebruiksbasis.
- Voor Enterprise-abonnementen, **gereserveerde instanties toevoegen** moet zijn ingeschakeld in de [EA-portal](https://ea.azure.com). Of, als deze instelling is uitgeschakeld, moet u een EA-beheerder voor het abonnement zijn.
- Voor het programma Cloud Solution Provider (CSP) kunnen de beheerder agents of sales-agents de Red Hat-abonnementen kopen.

## <a name="buy-a-red-hat-software-plan"></a>Koop een abonnement van Red Hat-software

1. Ga naar [reserveringen](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) in Azure portal.
1. Selecteer **toevoegen** en Red Hat Linux selecteert.
1. Vul de vereiste velden in. Elke Red Hat Linux virtuele machine die overeenkomt met de kenmerken van wat u kopen met deze eigenschap wordt de korting. Het werkelijke aantal implementaties die aan de korting is afhankelijk van het bereik en de hoeveelheid die is geselecteerd.

    | Veld      | Description|
    |:------------|:--------------|
    |Name        |De naam van deze aankoop.|
    |Abonnement|Het abonnement dat wordt gebruikt om te betalen voor dit plan. De betalingswijze voor het abonnement wordt in rekening gebracht de kosten vooraf voor de reservering. Het abonnementstype moet een enterprise agreement (getallen bieden: MS-AZR-0017P of MS-AZR - 0148 P) en betalen per gebruik (getallen bieden: MS-AZR-0003P of MS-AZR - 0023 P). Voor een Enterprise-abonnement worden de kosten in mindering gebracht op het toezeggingsbedrag of ze worden in rekening gebracht als overschrijding. Voor een Betalen per gebruik-abonnement worden de kosten in rekening gebracht op de creditcard of de factuurbetalingswijze van het abonnement.|
    |Bereik       |Het bereik kan betrekking hebben op één abonnement of meerdere abonnementen (gedeeld bereik). Als u selecteert: <ul><li>Eén abonnement - korting van het abonnement wordt toegepast op Red Hat Linux-gebruik in dit abonnement. </li><li>Gedeeld - wordt korting van het abonnement toegepast op Red Hat Linux-gebruik in alle abonnementen in de context van de facturering. Voor zakelijke klanten, gedeeld bereik is van de inschrijving en bevat alle abonnementen binnen de inschrijving. Voor klanten van betalen per gebruik is het gedeelde bereik alle betalen per gebruik-abonnementen die zijn gemaakt door de accountbeheerder.</li></ul>|
    |Plannen     |Selecteer het Red Hat Linux-plan. Voor hulp bij het identificeren van wat u wilt kopen, raadpleegt u begrijpen hoe de reserveringskorting van Red Hat Linux Enterprise software wordt toegepast.|
    |VM-grootte     |Red Hat Linux prijs, is afhankelijk van het aantal vcpu's op de virtuele machine. Selecteer de optie die het aantal vcpu's voor uw Red Hat Linux-VM's vertegenwoordigt.|
    |Termijn        |Één of drie jaar.|
    |Hoeveelheid    |Het aantal virtuele machines dat u dit plan Red Hat Linux voor koopt. Het aantal is het aantal actieve instanties van Red Hat Linux die de korting voor facturering krijgt.|
1. Selecteer **Aankoop**.
1. Selecteer **deze reservering weergeven** om te zien van de status van uw aankoop.

De reserveringskorting wordt automatisch toegepast op elke actieve virtuele machines van Red Hat die overeenkomt met de reservering. De korting geldt alleen voor de Red Hat-meter. De VM-rekenkosten in rekening gebracht vallen niet onder dit plan.

## <a name="discount-applies-to-different-vm-sizes"></a>Korting is van toepassing op andere VM-grootten

Zoals gereserveerde VM-instanties bieden Red Hat Linux plannen instantieflexibiliteit grootte. Dit betekent dat uw korting geldt zelfs wanneer het implementeren van een virtuele machine die een andere grootte van het Red Hat-abonnement dat u hebt gekocht. Zie voor meer informatie, begrijpen hoe de reserveringskorting van Red Hat Linux Enterprise software wordt toegepast.

## <a name="cancellation-and-exchanges-not-allowed"></a>Annulering en uitwisselingen niet toegestaan

U niet annuleren of exchange een Red Hat-abonnement dat u hebt gekocht. Controleer uw gebruik om er zeker van te zijn dat u het juiste abonnement koopt. Voor hulp bij het identificeren van wat u wilt kopen, raadpleegt u begrijpen hoe de reserveringskorting van Red Hat Linux Enterprise software wordt toegepast.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het beheren van een reservering, [beheren Azure reserveringen](../../billing/billing-manage-reserved-vm-instance.md).

Zie de volgende artikelen voor meer informatie:

- [Wat zijn Azure-reserveringen?](../../billing/billing-save-compute-costs-reservations.md)
- [Reserveringen in Azure beheren](../../billing/billing-manage-reserved-vm-instance.md)
- Begrijpen hoe de Red Hat-reserveringskorting wordt toegepast
- [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](../../billing/billing-understand-reserved-instance-usage.md)
- [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](../../billing/billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.
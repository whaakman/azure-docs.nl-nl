---
title: Azure gereserveerde exemplaren korting - Azure-facturering begrijpen | Microsoft Docs
description: Meer informatie over hoe Azure gereserveerde VM-instantie korting wordt toegepast op virtuele machines worden uitgevoerd.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: yashar
ms.openlocfilehash: a0800bafc3d6b858387e28a3b75bc7b3a6bfe6e8
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="understand-how-the-reserved-instance-discount-is-applied"></a>Begrijpen hoe de gereserveerde exemplaar korting wordt toegepast
Nadat u een exemplaar van Azure gereserveerde virtuele machine hebt gekocht, wordt de korting gereserveerde exemplaar van Azure automatisch toegepast op virtuele machines die overeenkomt met de kenmerken en de hoeveelheid van het gereserveerde exemplaar. Een gereserveerde exemplaar bevat informatie over de kosten van de infrastructuur van uw virtuele machines. De volgende tabel ziet u de kosten voor uw virtuele machine na de aankoop van een gereserveerde exemplaar. In alle gevallen moet u in rekening worden gebracht voor opslag en netwerken op de normale tarieven.

| Type van de virtuele Machine  | Kosten met gereserveerde exemplaar |    
|-----------------------|--------------------------------------------|
|Virtuele Linux-machines zonder extra software | Het gereserveerde exemplaar bevat informatie over de kosten van uw VM-infrastructuur.|
|Virtuele Linux-machines met de software-kosten (bijvoorbeeld Red Hat) | Het gereserveerde exemplaar bevat informatie over de kosten van de infrastructuur. Worden in rekening gebracht voor andere software.|
|VM's van Windows zonder extra software |Het gereserveerde exemplaar bevat informatie over de kosten van de infrastructuur. Worden in rekening gebracht voor Windows-software.|
|Windows-VM's met aanvullende software (bijvoorbeeld SQL server) | Het gereserveerde exemplaar bevat informatie over de kosten van de infrastructuur. Er worden in rekening gebracht voor Windows-software en voor andere software.|
|Windows-VM's met [Azure hybride Benefit](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) | Het gereserveerde exemplaar bevat informatie over de kosten van de infrastructuur. De kosten voor de Windows-software, het voordeel van de hybride Azure vallen. Geen extra software wordt afzonderlijk in rekening gebracht.| 

## <a name="application-of-reserved-instance-discount-to-non-windows-vms"></a>Toepassing van gereserveerde exemplaar korting naar niet - Windows virtuele machines
 De gereserveerde exemplaar korting wordt toegepast op de VM-exemplaren op uurbasis uitgevoerd. De gereserveerde exemplaren die u hebt aangeschaft worden vergeleken met het gebruik dat is verzonden door de actieve virtuele machines toe te passen de korting gereserveerde exemplaar. Voor virtuele machines die mogelijk niet het volledige uur uitgevoerd, wordt het gereserveerde exemplaar ingevuld van andere VM's die niet met een gereserveerde exemplaar, met inbegrip van gelijktijdig uitgevoerde VM's. Aan het einde van het uur, is de gereserveerde exemplaar-toepassing voor virtuele machines in het uur vergrendeld. In het geval van een virtuele machine kan niet worden uitgevoerd voor een uur of gelijktijdige virtuele machines binnen het uur vult u het uur van de gereserveerde exemplaar niet, wordt het gereserveerde exemplaar onderbenutte voor dat uur. De volgende grafiek ziet u de toepassing van een gereserveerde exemplaar op factureerbare VM-gebruik. De afbeelding is gebaseerd op een gereserveerde exemplaar inkoop en twee overeenkomende VM-exemplaren.

![Schermopname van één exemplaar van toegepaste gereserveerd en twee overeenkomende VM-exemplaren](media/billing-reserved-vm-instance-application/billing-reserved-vm-instance-application.png)

1.  Enig gebruik die hoger is dan de regel gereserveerde exemplaar opgehaald in rekening gebracht volgens de tarieven van de reguliere betalen naar gebruik. Bent u niet in rekening gebracht voor enig gebruik onder de regel gereserveerde exemplaren omdat deze al is betaald als onderdeel van de gereserveerde exemplaar aankoop.
2.  1 uur, exemplaar 1 0,75 uur uitgevoerd en exemplaar 2 0,5 uur uitgevoerd. Totaal gebruik voor 1 uur is 1,25 uur. Er zijn de tarieven voor betalen naar gebruik in rekening gebracht voor de resterende 0,25 uur.
3.  Voor het uur 2 en 3 uur, worden beide exemplaren 1 uur uitgevoerd. Een exemplaar wordt gedekt door het gereserveerde exemplaar en de andere in rekening gebracht volgens de tarieven voor betalen naar gebruik.
4.  Gedurende 4 uur, exemplaar 1 0,5 uur uitgevoerd en exemplaar 2 uur uitgevoerd. Exemplaar 1 volledig wordt gedekt door het gereserveerde exemplaar en 0,5 uren van 2-exemplaar wordt behandeld. U bent de betalen naar gebruik snelheid in rekening gebracht voor de resterende 0,5 uur.

Om te begrijpen en de toepassing van de gereserveerde exemplaren weergeven in gebruiksrapporten facturering, Zie [gereserveerde exemplaar begrijpen gebruik](https://go.microsoft.com/fwlink/?linkid=862757).

## <a name="application-of-reserved-instance-discount-to-windows-vms"></a>Toepassing van gereserveerde exemplaar korting op Windows-VM 's
Wanneer u exemplaren van de virtuele machine van Windows uitvoert, wordt het gereserveerde exemplaar toegepast ten aanzien van de kosten van de infrastructuur. De toepassing van het gereserveerde exemplaar voor de kosten van de VM-infrastructuur voor VM's van Windows is hetzelfde als voor niet - Windows virtuele machines. U wordt afzonderlijk in rekening gebracht voor de per vCPU op basis van een Windows-software. Zie [kosten voor Windows-software met gereserveerde exemplaren](https://go.microsoft.com/fwlink/?linkid=862756). U kunt uw Windows-licentiekosten met [Azure hybride voordeel voor Windows Server] dekt (https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over gereserveerde exemplaren, de volgende artikelen:

- [Geld besparen op virtuele machines met gereserveerde exemplaren van Azure](billing-save-compute-costs-reservations.md)
- [Vooruitbetalen voor virtuele Machines met gereserveerde exemplaren](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Gereserveerde exemplaren beheren](billing-manage-reserved-vm-instance.md)
- [Begrijpen hoe de gereserveerde exemplaar korting wordt toegepast](billing-understand-vm-reservation-charges.md)
- [Gebruik van de gereserveerde exemplaar voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
- [Gereserveerde exemplaar gebruiksgegevens voor uw Enterprise enrollment begrijpen](billing-understand-reserved-instance-usage-ea.md)
- [Gebruik voor abonnementen van de CSP gereserveerde exemplaar begrijpen](https://docs.microsoft.com/partner-center/azure-reservations)
- [Kosten voor Windows-software niet zijn opgenomen in de gereserveerde exemplaren](billing-reserved-instance-windows-software-costs.md)


## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog steeds meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.

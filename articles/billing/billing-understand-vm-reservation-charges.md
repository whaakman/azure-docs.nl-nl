---
title: De gereserveerde virtuele Machine-exemplaren Azure korting toepassing begrijpen | Microsoft Docs
description: Meer informatie over hoe Azure gereserveerde VM-instantie korting wordt toegepast op de VM's worden uitgevoerd.
services: billing
documentationcenter: 
author: vikramdesai01
manager: vikdesai
editor: 
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/10/2017
ms.author: vikdesai
ms.openlocfilehash: d476380fa841617f7eb914167ebd7d5b8aa611c2
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="understand-how-the-reserved-virtual-machine-instance-discount-is-applied"></a>Begrijpen hoe de korting exemplaar van de gereserveerde virtuele Machine wordt toegepast
Nadat u een gereserveerde VM-instantie hebt gekocht, wordt de reservering korting automatisch toegepast op virtuele machines die overeenkomt met de kenmerken en de hoeveelheid van de reservering. Een reservering bevat informatie over de kosten van de infrastructuur van uw virtuele machines. De volgende tabel ziet u de kosten voor uw virtuele machine na de aankoop van een reservering. In alle gevallen moet u in rekening worden gebracht voor opslag en netwerken op de normale tarieven.

| Type van de virtuele Machine  | Kosten met-reservering |    
|-----------------------|--------------------------------------------| 
|Virtuele Linux-machines zonder extra software | Reservering bevat informatie over de kosten van uw VM-infrastructuur.|
|Virtuele Linux-machines met de software-kosten (bijvoorbeeld Red Hat) | Reservering bevat informatie over de kosten van de infrastructuur. Worden in rekening gebracht voor andere software.|
|VM's van Windows zonder extra software |Reservering bevat informatie over de kosten van de infrastructuur. Worden in rekening gebracht voor Windows-software.|
|Windows-VM's met aanvullende software (bijvoorbeeld SQL server) | Reservering bevat informatie over de kosten van de infrastructuur. Er worden in rekening gebracht voor Windows-software en voor andere software.|
|Windows-VM's met [Azure hybride Benefit](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) | Reservering bevat informatie over de kosten van de infrastructuur. De kosten voor de Windows-software, het voordeel van de hybride Azure vallen. Geen extra software wordt afzonderlijk in rekening gebracht.| 

## <a name="application-of-reservation-discount-to-non-windows-vms"></a>Toepassing van de reservering korting naar niet - Windows virtuele machines
 De reservering korting wordt toegepast op de VM-exemplaren op uurbasis uitgevoerd. De reserveringen die u hebt gekocht worden vergeleken met het gebruik door de actieve virtuele machines toe te passen de reservering korting verzonden. De volgende grafiek ziet u de toepassing van een reservering op factureerbare VM-gebruik. De afbeelding is gebaseerd op één reservering inkoop en twee overeenkomende VM-exemplaren.

![Gereserveerde VM-instantie toepassing](media/billing-reserved-vm-instance-application/billing-reserved-vm-instance-application.png)

1.  Enig gebruik die hoger is dan de regel gereserveerde VM-instantie opgehaald in rekening gebracht volgens de tarieven van de reguliere betalen naar gebruik. Bent u niet in rekening gebracht voor enig gebruik onder de regel gereserveerde VM-exemplaren omdat deze al is betaald als onderdeel van de reservering aankoop.
2.  1 uur, exemplaar 1 0,75 uur uitgevoerd en exemplaar 2 0,5 uur uitgevoerd. Totaal gebruik voor 1 uur is 1,25 uur. Er zijn de tarieven voor betalen naar gebruik in rekening gebracht voor de resterende 0,25 uur.
3.  Voor het uur 2 en 3 uur, worden beide exemplaren 1 uur uitgevoerd. Een exemplaar wordt gedekt door de reservering en de andere in rekening gebracht volgens de tarieven voor betalen naar gebruik.
4.  Gedurende 4 uur, exemplaar 1 0,5 uur uitgevoerd en exemplaar 2 uur uitgevoerd. Exemplaar 1 volledig wordt gedekt door de reservering en 0,5 uren van 2-exemplaar wordt behandeld. U bent de betalen naar gebruik snelheid in rekening gebracht voor de resterende 0,5 uur.

Om te begrijpen en de toepassing van uw reserveringen in gebruiksrapporten facturering, Zie [gereserveerde VM-instantie begrijpen gebruik](https://go.microsoft.com/fwlink/?linkid=862757).

## <a name="application-of-reservation-discount-to-windows-vms"></a>Toepassing van de reservering korting op Windows-VM 's
Wanneer u exemplaren van de virtuele machine van Windows uitvoert, wordt de reservering toegepast ten aanzien van de kosten van de infrastructuur. De toepassing van de reservering voor de kosten van de VM-infrastructuur voor VM's van Windows is hetzelfde als voor niet - Windows virtuele machines. U wordt afzonderlijk in rekening gebracht voor de per vCPU op basis van een Windows-software. Zie [kosten voor Windows-software met reserveringen](https://go.microsoft.com/fwlink/?linkid=862756). U kunt uw Windows-licentiekosten met [Azure hybride voordeel voor Windows Server] (https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) uitgelegd.

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u hulp nodig hebt, moet [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.

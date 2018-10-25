---
title: Korting op Azure Reserved VM Instances begrijpen | Microsoft Docs
description: Meer informatie over hoe de korting voor Azure gereserveerde VM-instantie wordt toegepast op virtuele machines worden uitgevoerd.
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
ms.date: 08/08/2018
ms.author: cwatson
ms.openlocfilehash: e43188459bf495748a054b0f24936c9141bd218c
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50025702"
---
# <a name="understand-how-the-azure-reservation-discount-is-applied-to-virtual-machines"></a>Begrijpen hoe de Azure reserveringskorting wordt toegepast op virtuele machines

Nadat u een virtuele Machine voor Azure gereserveerde instantie koopt, wordt de reserveringskorting wordt automatisch toegepast op virtuele machines die overeenkomen met de kenmerken en de hoeveelheid van de reservering. Een reservering dekt de kosten voor rekenuren van uw virtuele machines.

Zie voor SQL-Database gereserveerde capaciteit, [korting op Azure Reserved Instances begrijpen](billing-understand-reservation-charges.md).

De volgende tabel ziet u de kosten voor uw virtuele machine na de aankoop van gereserveerde VM-instantie. In alle gevallen moet u betalen voor opslag en netwerken op basis van de normale tarieven.

| Type virtuele Machine  | Kosten in rekening gebracht met gereserveerde VM-exemplaar |
|-----------------------|--------------------------------------------|
|Virtuele Linux-machines zonder aanvullende software | De reservering dekt de kosten van uw VM-infrastructuur.|
|Virtuele Linux-machines met de kosten voor software (bijvoorbeeld, Red Hat) | De reservering heeft betrekking op de infrastructuurkosten. U betaalt voor extra software.|
|Windows-machines zonder aanvullende software |De reservering heeft betrekking op de infrastructuurkosten. U betaalt voor Windows-software.|
|Windows-VM's met aanvullende software (bijvoorbeeld SQL server) | De reservering heeft betrekking op de infrastructuurkosten. U betaalt voor Windows-software en voor andere software.|
|Windows-VM's met [Azure Hybrid Benefit](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) | De reservering heeft betrekking op de infrastructuurkosten. De kosten voor de Windows-software zijn inbegrepen bij de Azure Hybrid Benefit. Aanvullende software wordt afzonderlijk in rekening gebracht.| 

## <a name="application-of-reservation-discount-to-non-windows-vms"></a>Toepassing van de reserveringskorting op niet - Windows-VM 's

 De Azure reserveringskorting wordt toegepast op het uitvoeren van VM-exemplaren op uurbasis. De reserveringen die u hebt aangeschaft, zijn afgestemd op het gebruik dat wordt verzonden door de actieve virtuele machines naar past de reserveringskorting toe. Voor virtuele machines die mogelijk niet volledig uur worden uitgevoerd, wordt de reservering van andere virtuele machines niet met behulp van een reservering, inclusief het gelijktijdig uitvoeren van virtuele machines worden gevuld. Aan het einde van het uur, is de reservering-toepassing voor virtuele machines in het uur vergrendeld. In het geval van een virtuele machine wordt niet uitgevoerd voor een uur of gelijktijdige VM's binnen het uur vult u het uur van de reservering niet, wordt de reservering gebruikt voor dat uur. In het volgende diagram ziet u de toepassing van een reservering factureerbare VM-gebruik. De afbeelding is gebaseerd op één reserveringsaankoop en twee overeenkomende VM-exemplaren.

![Schermafbeelding van een toegepaste reservering en twee gegevensbronnen die overeenkomt met de VM-exemplaren](media/billing-reserved-vm-instance-application/billing-reserved-vm-instance-application.png)

1. Gebruik dat hoger is dan de reserveringsregel wordt in rekening gebracht op de normale betalen per gebruik-tarieven betalen. U kunt geen kosten in rekening gebracht voor het gebruik onder de lijn reserveringen omdat deze al is betaald als onderdeel van het kopen van reserveringen.
2. Uur 1, 1 exemplaar 0,75 uur wordt uitgevoerd en exemplaar 2 0,5 uur wordt uitgevoerd. Totaal gebruik voor 1 uur is 1,25 uur. U bent de betalen naar gebruik-tarieven in rekening gebracht voor de resterende 0,25 uur.
3. Voor het uur 2 en 3 uur, die beide exemplaren worden uitgevoerd voor één uur. Één exemplaar wordt gedekt door de reservering en de andere wordt in rekening gebracht tegen betalen per gebruik-tarieven.
4. 4 uur, 1 exemplaar 0,5 uur wordt uitgevoerd en exemplaar 2 voor 1 uur wordt uitgevoerd. Exemplaar 1 volledig wordt gedekt door de reservering en 0,5 uur van 2-exemplaar wordt behandeld. U bent de betalen naar gebruik-tarief in rekening gebracht voor de resterende 0,5 uur.

Om te begrijpen en te bekijken van de toepassing van uw Azure-reserveringen in gebruiksrapporten facturering, Zie [begrijpen reservering gebruik](https://go.microsoft.com/fwlink/?linkid=862757).

## <a name="application-of-reservation-discount-to-windows-vms"></a>Toepassing van de reserveringskorting op Windows-VM 's

Wanneer u Windows-VM-exemplaren uitvoert, wordt de reservering wordt toegepast aan zodat de infrastructuurkosten. De toepassing van de reservering van de kosten voor de VM-infrastructuur voor Windows-VM's is hetzelfde als voor niet - Windows-VM's. U bent afzonderlijk in rekening gebracht voor Windows-software op basis van per vCPU. Zie [Windows softwarekosten met reserveringen](https://go.microsoft.com/fwlink/?linkid=862756). U kunt uw Windows licentiekosten met dekt [Azure Hybrid Benefit voor Windows Server](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure-reserveringen, de volgende artikelen:

- [Wat zijn Azure-reserveringen?](billing-save-compute-costs-reservations.md)
- [Vooruitbetalen voor Virtual Machines met Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Vooruitbetalen voor compute-resources van SQL Database met gereserveerde capaciteit voor Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)
- [Azure-reserveringen beheren](billing-manage-reserved-vm-instance.md)
- [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
- [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](billing-understand-reserved-instance-usage-ea.md)
- [Informatie over het gebruik van de reservering voor CSP-abonnementen](https://docs.microsoft.com/partner-center/azure-reservations)
- [Kosten van de Windows-software is niet opgenomen in de reserveringen](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.

---
title: Korting op Azure Reserved VM Instances begrijpen | Microsoft Docs
description: Meer informatie over hoe de korting voor Azure gereserveerde VM-instantie wordt toegepast op virtuele machines worden uitgevoerd.
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/13/2019
ms.author: banders
ms.openlocfilehash: b112dd881d4b2e87e617111d00bc82c6151d7750
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60370068"
---
# <a name="how-the-azure-reservation-discount-is-applied-to-virtual-machines"></a>Hoe de Azure reserveringskorting wordt toegepast op virtuele machines

Nadat u een virtuele Machine voor Azure gereserveerde instantie koopt, wordt de reserveringskorting wordt automatisch toegepast op virtuele machines die overeenkomen met de kenmerken en de hoeveelheid van de reservering. Een reservering dekt de kosten voor rekenuren van uw virtuele machines.

Een reserveringskorting is van toepassing op de basis-VM's die u bij de Azure Marketplace aanschaft.

Zie voor SQL-Database gereserveerde capaciteit, [korting op Azure Reserved Instances begrijpen](billing-understand-reservation-charges.md).

De volgende tabel ziet u de kosten voor uw virtuele machine na de aankoop van gereserveerde VM-instantie. In alle gevallen moet u betalen voor opslag en netwerken op basis van de normale tarieven.

| Type virtuele Machine  | Kosten in rekening gebracht met gereserveerde VM-exemplaar |
|-----------------------|--------------------------------------------|
|Virtuele Linux-machines zonder aanvullende software | De reservering dekt de kosten van uw VM-infrastructuur.|
|Virtuele Linux-machines met de kosten voor software (bijvoorbeeld, Red Hat) | De reservering heeft betrekking op de infrastructuurkosten. U betaalt voor extra software.|
|Windows-machines zonder aanvullende software |De reservering heeft betrekking op de infrastructuurkosten. U betaalt voor Windows-software.|
|Windows-VM's met aanvullende software (bijvoorbeeld SQL server) | De reservering heeft betrekking op de infrastructuurkosten. U betaalt voor Windows-software en voor andere software.|
|Windows-VM's met [Azure Hybrid Benefit](../virtual-machines/windows/hybrid-use-benefit-licensing.md) | De reservering heeft betrekking op de infrastructuurkosten. De kosten voor de Windows-software zijn inbegrepen bij de Azure Hybrid Benefit. Aanvullende software wordt afzonderlijk in rekening gebracht.|

## <a name="how-reservation-discount-is-applied"></a>Hoe reserveringskorting wordt toegepast

Een reserveringskorting is '*gebruik-it-of-verliezen-it*'. Dus als u geen overeenkomende resources voor elk uur, verliest klikt u een reserveringshoeveelheid voor dat uur. U niet uitvoeren voor het doorsturen van niet-gebruikte gereserveerde uur.

Als u een resource wordt afgesloten, past de reserveringskorting automatisch naar een andere overeenkomende resource in het opgegeven bereik. Als er geen overeenkomende bronnen u in het opgegeven bereik vindt, wordt de gereserveerde uren *verloren*.

## <a name="reservation-discount-for-non-windows-vms"></a>De reserveringskorting voor niet - Windows-VM 's

 De Azure reserveringskorting wordt toegepast op het uitvoeren van VM-exemplaren op uurbasis. De reserveringen die u hebt aangeschaft, zijn afgestemd op het gebruik dat wordt verzonden door de actieve virtuele machines naar past de reserveringskorting toe. Voor virtuele machines die mogelijk niet volledig uur worden uitgevoerd, wordt de reservering van andere virtuele machines niet met behulp van een reservering, inclusief het gelijktijdig uitvoeren van virtuele machines worden gevuld. Aan het einde van het uur, is de reservering-toepassing voor virtuele machines in het uur vergrendeld. In het geval van een virtuele machine wordt niet uitgevoerd voor een uur of gelijktijdige VM's binnen het uur vult u het uur van de reservering niet, wordt de reservering gebruikt voor dat uur. In het volgende diagram ziet u de toepassing van een reservering factureerbare VM-gebruik. De afbeelding is gebaseerd op één reserveringsaankoop en twee overeenkomende VM-exemplaren.

![Schermafbeelding van een toegepaste reservering en twee gegevensbronnen die overeenkomt met de VM-exemplaren](./media/billing-reserved-vm-instance-application/billing-reserved-vm-instance-application.png)

1. Gebruik dat hoger is dan de reserveringsregel wordt in rekening gebracht op de normale betalen per gebruik-tarieven betalen. U kunt geen kosten in rekening gebracht voor het gebruik onder de lijn reserveringen omdat deze al is betaald als onderdeel van het kopen van reserveringen.
2. Uur 1, 1 exemplaar 0,75 uur wordt uitgevoerd en exemplaar 2 0,5 uur wordt uitgevoerd. Totaal gebruik voor 1 uur is 1,25 uur. U bent de betalen naar gebruik-tarieven in rekening gebracht voor de resterende 0,25 uur.
3. Voor het uur 2 en 3 uur, die beide exemplaren worden uitgevoerd voor één uur. Één exemplaar wordt gedekt door de reservering en de andere wordt in rekening gebracht tegen betalen per gebruik-tarieven.
4. 4 uur, 1 exemplaar 0,5 uur wordt uitgevoerd en exemplaar 2 voor 1 uur wordt uitgevoerd. Exemplaar 1 volledig wordt gedekt door de reservering en 0,5 uur van 2-exemplaar wordt behandeld. U bent de betalen naar gebruik-tarief in rekening gebracht voor de resterende 0,5 uur.

Om te begrijpen en te bekijken van de toepassing van uw Azure-reserveringen in gebruiksrapporten facturering, Zie [begrijpen reservering gebruik](billing-understand-reserved-instance-usage-ea.md).

## <a name="reservation-discount-for-windows-vms"></a>De reserveringskorting voor Windows-VM 's

Wanneer u Windows-VM-exemplaren uitvoert, wordt de reservering wordt toegepast aan zodat de infrastructuurkosten. De toepassing van de reservering van de kosten voor de VM-infrastructuur voor Windows-VM's is hetzelfde als voor niet - Windows-VM's. U bent afzonderlijk in rekening gebracht voor Windows-software op basis van per vCPU. Zie [Windows softwarekosten met reserveringen](billing-reserved-Instance-windows-software-costs.md). U kunt uw Windows licentiekosten met dekt [Azure Hybrid Benefit voor Windows Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

## <a name="discount-can-apply-to-different-sizes"></a>Korting kunt toepassen op verschillende grootten

Wanneer u een gereserveerde VM-instantie, kopen als u selecteert **geoptimaliseerd voor**: **exemplaar grootte flexibiliteit**, de dekking van de korting is afhankelijk van de VM-grootte die u selecteert. De reservering kunt toepassen op de grootten van virtuele machines (VM's) in de groep voor reeks van dezelfde grootte. Zie voor meer informatie, [VM-grootte flexibiliteit in combinatie met gereserveerde VM-instanties](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).

## <a name="discount-applies-to-matching-servicetype-only"></a>Korting op de overeenkomende ServiceType alleen van toepassing is

Een reserveringskorting geldt alleen voor het gebruik van de virtuele machine waar de `ServiceType` waarde in de `AdditionalInfo` komt overeen met de reservering die aangeschaft. Toepassing van korting op reservering negeert de meter gebruikt voor virtuele machines en alleen evalueert `ServiceType`. Weet welke service typt u de virtuele machine voor hebt aangeschaft. U kunt een niet-premium storage VM reservering voor een premium storage-reservering, of in de tegengestelde manier kan uitwisselen.

## <a name="classic-vms-and-cloud-services"></a>Klassieke virtuele machines en cloudservices

Gereserveerde VM-exemplaren automatisch van toepassing op zowel klassieke virtuele machines en cloudservices als grootte-instantieflexibiliteit is ingeschakeld. Voor cloudservices, wordt de reserveringskorting geldt alleen voor de compute-kosten. Wanneer de reserveringskorting wordt toegepast op cloudservices, de gebruikskosten worden onderverdeeld in rekenkosten in rekening gebracht (Linux meter) en cloud services-kosten in rekening gebracht (cloud services management meter). Zie voor meer informatie, [hoe de reserveringskorting is van toepassing op Cloud Services](billing-reserved-instance-windows-software-costs.md#cloud-services-software-meters-not-included-in-reservation-cost).

## <a name="need-help-contact-us"></a>Hulp nodig? Contact opnemen

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure-reserveringen, de volgende artikelen:

- [Wat zijn de reserveringen voor Azure?](billing-save-compute-costs-reservations.md)
- [Vooruitbetalen voor Virtual Machines met Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Vooruitbetalen voor compute-resources van SQL Database met gereserveerde capaciteit voor Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)
- [Reserveringen voor Azure beheren](billing-manage-reserved-vm-instance.md)
- [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
- [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](billing-understand-reserved-instance-usage-ea.md)
- [Informatie over het gebruik van de reservering voor CSP-abonnementen](/partner-center/azure-reservations)
- [Kosten van de Windows-software is niet opgenomen in de reserveringen](billing-reserved-instance-windows-software-costs.md)

---
title: Korting op Azure reserveringen begrijpen | Microsoft Docs
description: Meer informatie over hoe een reserveringskorting wordt toegepast op het uitvoeren van SQL-Databases.
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
ms.openlocfilehash: 176e282a53c19e303fd06629a0045a79fd200dea
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52580368"
---
# <a name="understand-how-an-azure-reservation-discount-is-applied-to-sql-databases"></a>Begrijpen hoe een Azure reserveringskorting wordt toegepast op de SQL-Databases

Nadat u een Azure SQL Database gereserveerde capaciteit koopt, wordt de reserveringskorting wordt automatisch toegepast op SQL-Databases die overeenkomen met de kenmerken en de hoeveelheid van de reservering. Een reservering bevat informatie over de compute-kosten van uw SQL-Database. U betaalt voor de software, opslag en netwerken op basis van de normale tarieven. U kunt de licentiekosten voor SQL-Databases met dekt [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/).

Zie voor Reserved Virtual Machine Instances, [inzicht in Azure Reserved VM Instances korting](billing-understand-vm-reservation-charges.md).

## <a name="reservation-discount-applied-to-sql-databases"></a>De reserveringskorting wordt toegepast op de SQL-Databases

 De SQL-Database gereserveerde capaciteit korting wordt toegepast op het uitvoeren van SQL-Databases op uurbasis. De reservering die u koopt, wordt vergeleken met de compute-gebruik verzonden door de actieve SQL-Databases. Voor SQL Databases die geen vol uur worden uitgevoerd, wordt de reservering automatisch toegepast op andere SQL Databases die aan de reserveringskenmerken voldoen. De korting kunt toepassen op de SQL-Databases die gelijktijdig worden uitgevoerd. Als u geen SQL-Databases die voor een volledig uur worden uitgevoerd die overeenkomen met de kenmerken van de reservering hebt, krijgt u niet het volledige voordeel van de reserveringskorting voor dat uur.

De volgende voorbeelden ziet u hoe de SQL-Database gereserveerde capaciteit korting van toepassing is afhankelijk van het aantal kernen die u hebt gekocht, en wanneer ze worden uitgevoerd.

- Scenario 1: Koopt u een SQL-Database gereserveerde capaciteit voor een 8-core SQL-Database. U uitvoeren een 16-core SQL-Database die overeenkomen met de rest van de kenmerken van de reservering. U betaalt het betalen per gebruik betalen prijs voor 8 kernen van compute-gebruik van SQL-Database. U krijgt de reserveringskorting voor één uur van 8 kernen compute-gebruik van SQL-Database.

Voor de rest van deze voorbeelden wordt ervan uitgegaan dat de SQL-Database gereserveerde capaciteit die u kopen voor een 16-core SQL-Database en de rest van de kenmerken van de reservering overeenkomen met de actieve SQL-Databases.

- Scenario 2: U hebt twee SQL-Databases met 8 kernen uitvoeren voor een uur. De reserveringskorting 16 kernen wordt toegepast om de compute-gebruik voor zowel de 8 cores SQL-Databases.
- Scenario 3: Uitvoeren van een 16-core, SQL-Database van 1 uur aan 13:30 uur. U hebt uitgevoerd van een andere 16 kernen SQL-Database van 1:30 tot 2 uur. Beide zijn inbegrepen bij de reserveringskorting.
- Scenario 4: Uitvoeren van een 16-core, SQL-Database van 1 uur aan 13:45 uur. U hebt uitgevoerd van een andere 16 kernen SQL-Database van 1:30 tot 2 uur. U betaalt het betalen per gebruik betalen prijs voor de overlapping van 15 minuten. De reserveringskorting is van toepassing op de compute-gebruik voor de rest van de tijd.

Om te begrijpen en te bekijken van de toepassing van uw Azure-reserveringen in gebruiksrapporten facturering, Zie [over Azure-reservering gebruik](https://go.microsoft.com/fwlink/?linkid=862757).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure-reserveringen, de volgende artikelen:

- [Wat zijn Azure-reserveringen?](billing-save-compute-costs-reservations.md)
- [Vooruitbetalen voor Virtual Machines met Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Vooruitbetalen voor compute-resources van SQL Database met gereserveerde capaciteit voor Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)
- [Azure-reserveringen beheren](billing-manage-reserved-vm-instance.md)
- [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
- [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](billing-understand-reserved-instance-usage-ea.md)
- [Informatie over het gebruik van de reservering voor CSP-abonnementen](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

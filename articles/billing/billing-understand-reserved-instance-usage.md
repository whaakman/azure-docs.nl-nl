---
title: Gebruik Azure reserveringen voor betalen per gebruik-abonnement begrijpen | Microsoft Docs
description: Informatie over het lezen van uw gebruik voor meer informatie over hoe de Azure-reservering voor uw betalen per gebruik-abonnement wordt toegepast.
services: billing
documentationcenter: ''
author: manish-shukla01
manager: manshuk
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/28/2018
ms.author: cwatson
ms.openlocfilehash: 81ee681fac4496ad2d5360f01926634373c23075
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52584482"
---
# <a name="understand-azure-reservation-usage-for-your-pay-as-you-go-subscription"></a>Gebruik Azure-reservering voor uw abonnement op gebruiksbasis begrijpen

Gebruik de ReservationId van [reservering pagina](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) en het gebruiksbestand van de [Accounts in Azure portal](https://account.azure.com) om te evalueren van het gebruik van uw reservering.

Als u een klant met een Enterprise Agreement, Zie [begrijpen reservering gebruik voor uw Enterprise-inschrijving.](billing-understand-reserved-instance-usage-ea.md).

In dit artikel wordt ervan uitgegaan dat de reservering wordt toegepast op één abonnement. Als de reservering wordt toegepast op meer dan één abonnement, kan uw reserveringsvoordeel meerdere gebruik van CSV-bestanden omvatten.

## <a name="usage-for-reserved-virtual-machine-instances"></a>Gebruik voor gereserveerde VM-instanties

Voor de volgende secties wordt ervan uitgegaan dat u een Standard_DS1_v2 Windows virtuele machine worden uitgevoerd in de regio VS-Oost en uw gereserveerde VM-exemplaar informatie lijkt erop dat de volgende tabel:

| Veld | Waarde |
|---| :---: |
|ReservationId |8117adfb-1d94-4675-be2b-f3c1bca808b6|
|Hoeveelheid |1|
|SKU | Standard_DS1_v2|
|Regio | eastus |

De hardware-gedeelte van de virtuele machine wordt behandeld, omdat de geïmplementeerde virtuele machine komt overeen met de kenmerken van de reservering. Als u wilt zien welke Windows-software wordt niet gedekt door de gereserveerde VM-instantie, Zie [kosten voor Azure gereserveerde VM-exemplaren Windows-software](billing-reserved-instance-windows-software-costs.md)

### <a name="statement-section-of-csv-file-for-vms"></a>De sectie overzicht van CSV-bestand voor virtuele machines

Deze sectie van uw CSV-bestand toont het totale gebruik voor de reservering. Het filter toepassen op de **subcategorie van de Meter** veld met **'Reservering-'**. U ziet er ongeveer als de volgende schermafbeelding:

![Schermafbeelding van gefilterde reservering informatie over het gebruik en kosten](./media/billing-understand-reserved-instance-usage/billing-payg-reserved-instance-csv-statements.png)

De **reservering-basis-VM** regel bevat het totale aantal uren die worden gedekt door de reservering. Deze regel is $0,00 omdat de reservering bedekt. De **reservering-Windows-Server (1 Kerngeheugen)** regel dekt de kosten van Windows-software.

### <a name="daily-usage-section-of-csv-file"></a>De sectie dagelijks gebruik van CSV-bestand

Filteren op **aanvullende informatie** en typt u in uw **Reserverings-ID**. De volgende schermafbeelding ziet u de velden met betrekking tot de reservering.

![Schermafbeelding van informatie over het dagelijkse gebruik en kosten](./media/billing-understand-reserved-instance-usage/billing-payg-reserved-instance-csv-details.png)

1. **ReservationId** in de **aanvullende informatie** veld is de reservering die wordt toegepast op de virtuele machine.
2. **ConsumptionMeter** is de meter-ID voor de virtuele machine.
3. De **reservering-basis-VM** **subcategorie van de Meter** regel vertegenwoordigt de kosten van $0 in de sectie overzicht. De kosten van het uitvoeren van deze virtuele machine is al betaald door de reservering.
4. **ID meten** is de meter-ID voor de reservering. De kosten van deze meter is $0. Deze meter-id wordt weergegeven voor elke virtuele machine die in aanmerking voor de reserveringskorting komt.
5. Standard_DS1_v2 is één vCPU VM en de virtuele machine wordt geïmplementeerd zonder Azure Hybrid Benefit. Ja, deze meter bevat informatie over de extra kosten in rekening gebracht van de Windows-software. De meter die overeenkomt met de D-serie 1-core VM Zie [softwarekosten voor Azure gereserveerde VM-exemplaren Windows](billing-reserved-instance-windows-software-costs.md). Hebt u Azure Hybrid Benefit, worden deze extra kosten wordt niet toegepast.

## <a name="usage-for-sql-database--cosmos-db-reserved-capacity-reservations"></a>Gebruik voor SQL-Database en Cosmos DB gereserveerde capaciteitsreserveringen

De volgende secties voor het gebruik van Azure SQL Database als voorbeeld voor het beschrijven van het gebruiksrapport. U kunt dezelfde stappen voor het ophalen van gebruik van Azure Cosmos DB is het ook gebruiken. 

Wordt ervan uitgegaan dat u een SQL-Database Gen 4 worden uitgevoerd in de regio VS-Oost en uw reservering informatie er ongeveer zo uitziet als de volgende tabel:

| Veld | Waarde |
|---| --- |
|ReservationId |446ec809-423D-467c-8c5c-bbd5d22906b1|
|Hoeveelheid |2|
|Product| SQL Database Gen 4 (2 Kerngeheugens)|
|Regio | eastus |

### <a name="statement-section-of-csv-file"></a>De sectie overzicht van CSV-bestand

Filteren op **gebruik van gereserveerde instanties** naam meten en kiest u de vereiste **Metercategorie** -Azure SQL database of Azure Cosmos DB. U ziet er ongeveer als de volgende schermafbeelding:

![CSV-bestand voor SQL-Database gereserveerde capaciteit](./media/billing-understand-reserved-instance-usage/billing-payg-sql-db-reserved-capacity-csv-statements.png)

De **gebruik van gereserveerde instanties** regel bevat het totaal aantal core-uren wordt gedekt door de reservering. Het tarief is $0 voor deze regel als de kosten voor de reservering wordt gedekt.

### <a name="detail-section-of-csv-file"></a>De detailsectie van CSV-bestand

Filteren op **aanvullende informatie** en typt u in uw **Reserverings-ID**. De volgende schermafbeelding ziet u de velden met betrekking tot de SQL-Database gereserveerde capaciteitsreservering.

![CSV-bestand voor SQL-Database gereserveerde capaciteit](./media/billing-understand-reserved-instance-usage/billing-payg-sql-db-reserved-capacity-csv-details.png)

1. **ReservationId** in de **aanvullende informatie** veld is de SQL-Database gereserveerde capaciteitsreservering dat wordt toegepast op de SQL database-resource.
2. **ConsumptionMeter** is de meter-ID voor de SQL-Database-resource.
3. De **Meter-Id** is de meter reservering. De kosten van deze meter is $0. Een SQL Database-resources die in aanmerking komen voor de reserveringskorting deze meter-ID in het CSV-bestand bevat.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure-reserveringen, de volgende artikelen:

- [Wat zijn Azure-reserveringen?](billing-save-compute-costs-reservations.md)
- [Vooruitbetalen voor Virtual Machines met Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Vooruitbetalen voor compute-resources van SQL Database met gereserveerde capaciteit voor Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)
- [Azure-reserveringen beheren](billing-manage-reserved-vm-instance.md)
- [Begrijpen hoe de reserveringskorting wordt toegepast](billing-understand-vm-reservation-charges.md)
- [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](billing-understand-reserved-instance-usage-ea.md)
- [Kosten van de Windows-software is niet opgenomen in de reserveringen](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).



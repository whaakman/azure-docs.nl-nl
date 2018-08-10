---
title: Informatie over het gebruik van de Azure-reservering voor Enterprise | Microsoft Docs
description: Informatie over het lezen van uw gebruik voor meer informatie over hoe de Azure-reservering voor uw Enterprise-inschrijving wordt toegepast.
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
ms.date: 08/08/2018
ms.author: manshuk
ms.openlocfilehash: 5ce0103315f297996ed3f3bd88b5e53558e22e14
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39628229"
---
# <a name="understand-azure-reservation-usage-for-your-enterprise-enrollment"></a>Informatie over Azure-reservering gebruik voor uw Enterprise-inschrijving

Gebruik de **ReservationId** van [op de pagina reserveringen](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) en het gebruiksbestand van de [EA-portal](https://ea.azure.com) om te evalueren van het gebruik van uw reservering. U ziet ook het gebruik van de reservering in de samenvatting van gebruik van [EA-portal](https://ea.azure.com).

Als u de reservering in een context van de betalen per gebruik facturering hebt gekocht, Zie [begrijpen reservering gebruik voor uw abonnement op gebruiksbasis.](billing-understand-reserved-instance-usage.md)

## <a name="usage-for-reserved-virtual-machines-instances"></a>Gebruik voor gereserveerde VM-instanties

Voor de volgende secties wordt ervan uitgegaan dat u een Windows Standard_D1_v2 VM worden uitgevoerd in de regio VS-Oost en uw reservering informatie er ongeveer zo uitziet als de volgende tabel:

| Veld | Waarde |
|---| --- |
|ReservationId |8f82d880-d33e-4e0d-bcb5-6bcb5de0c719|
|Hoeveelheid |1|
|SKU | Standard_D1|
|Regio | eastus |

De hardware-gedeelte van de virtuele machine wordt behandeld, omdat de geïmplementeerde virtuele machine komt overeen met de kenmerken van de reservering. Als u wilt zien welke Windows-software wordt niet gedekt door de reservering, Zie [softwarekosten voor Azure gereserveerde VM-exemplaren Windows](billing-reserved-instance-windows-software-costs.md).

### <a name="usage-in-csv-file-for-reserved-vm-instances"></a>Gebruik in CSV-bestand voor gereserveerde VM-instanties

U kunt het zakelijke gebruik van CSV-bestand downloaden van de Enterprise portal. In het CSV-bestand filteren op **aanvullende informatie** en typt u in uw **ReservationID**. De volgende schermafbeelding ziet u de velden met betrekking tot de reservering:

![Enterprise Agreement (EA)-csv voor Azure-reservering](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-csv.png)

1. **ReservationId** in **aanvullende informatie** veld vertegenwoordigt de reservering die wordt toegepast op de virtuele machine.
2. **ConsumptionMeter** is de meter-ID voor de virtuele machine.
3. **ID meten** is de meter reservering met de kosten van $0. De kosten van de actieve virtuele machine wordt betaald door de gereserveerde VM-instantie.
4. Standard_D1 is één vCPU VM en de virtuele machine wordt geïmplementeerd zonder Azure Hybrid Benefit. Zodat deze meter bevat informatie over de extra kosten in rekening gebracht van de Windows-software. De meter die overeenkomt met de D-serie 1-core VM Zie [softwarekosten voor Azure gereserveerde VM-exemplaren Windows](billing-reserved-instance-windows-software-costs.md).  Hebt u Azure Hybrid Benefit, worden deze extra kosten wordt niet toegepast.

## <a name="usage-for-sql-database-reserved-capacity-reservations"></a>Gebruik voor SQL-Database gereserveerde capaciteitsreserveringen

Voor de volgende secties wordt ervan uitgegaan dat u een SQL-Database Gen 4 worden uitgevoerd in de regio VS-Oost en uw reservering informatie er ongeveer zo uitziet als de volgende tabel:

| Veld | Waarde |
|---| --- |
|ReservationId |8244e673-83e9-45ad-b54b-3f5295d37cae|
|Hoeveelheid |2|
|Product| SQL Database Gen 4 (2 Kerngeheugens)|
|Regio | eastus |

### <a name="usage-in-csv-file-for-sql-database-reserved-capacity"></a>Gebruik in CSV-bestand voor SQL-Database gereserveerde capaciteit

Filteren op **aanvullende informatie** en typt u in uw **Reserverings-ID**. De volgende schermafbeelding ziet u de velden met betrekking tot de reservering.

![Enterprise Agreement (EA)-csv voor SQL-Database gereserveerde capaciteit](./media/billing-understand-reserved-instance-usage-ea/billing-ea-sql-db-reserved-capacity-csv.png)

1. **ReservationId** in de **aanvullende informatie** veld is de reservering die wordt toegepast op de SQL-Database-resource.
2. **ConsumptionMeter** is de meter-ID voor de SQL-Database-resource.
3. **ID meten** is de meter reservering met de kosten van $0. Een SQL-Database-resource die in aanmerking voor de reservering komt bevat deze meter-ID in het CSV-bestand.

## <a name="usage-summary-page-in-enterprise-portal"></a>De overzichtspagina voor gebruik in Enterprise portal

Het gebruik van uw Azure-reservering wordt ook weergegeven in de samenvatting van gebruik van Enterprise portal: ![samenvatting van gebruik van Enterprise Agreement (EA)](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-usagesummary.png)

1. Niet in rekening gebracht voor de hardware-onderdeel van de virtuele machine als deze wordt omspannen door reservering. Voor de reservering van een SQL-Database, ziet u een regel met **servicenaam** gereserveerde capaciteitsgebruik als Azure SQL-Database.
2. In dit voorbeeld hebt u geen Azure Hybrid Benefit, zodat u kosten in voor de Windows-software die is gebruikt in combinatie met de virtuele machine gebracht rekening.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure reserveringen, de volgende artikelen:

- [Wat zijn Azure reserveringen?](billing-save-compute-costs-reservations.md)
- [Vooruitbetalen voor virtuele Machines met Azure gereserveerde VM-instanties](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Betaal vooruit voor SQL Database-compute-resources met Azure SQL Database gereserveerde capaciteit](../sql-database/sql-database-reserved-capacity.md) 
- [Azure-reserveringen beheren](billing-manage-reserved-vm-instance.md)
- [Begrijpen hoe de reserveringskorting wordt toegepast](billing-understand-vm-reservation-charges.md)
- [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
- [Kosten van de Windows-software is niet opgenomen in de reserveringen](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.
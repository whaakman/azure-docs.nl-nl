---
title: Gebruik van de gereserveerde exemplaar van Azure voor Enterprise - Azure-facturering begrijpen | Microsoft Docs
description: Informatie over het lezen van uw gebruik om te begrijpen hoe de Azure gereserveerde VM-instantie voor uw Enterprise enrollment wordt toegepast.
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
ms.date: 05/09/2018
ms.author: manshuk
ms.openlocfilehash: a92fce33b194c5cb7b763930e7fd11135f9fbd4f
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="understand-azure-reserved-instance-usage-for-your-enterprise-enrollment"></a>Gebruik Azure gereserveerde exemplaar voor uw Enterprise enrollment begrijpen
Gebruik van gereserveerde exemplaar begrijpen met behulp van de **ReservationId** van [op de pagina reserveringen](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) en het bestand informatie over het gebruik van de [EA portal](https://ea.azure.com). U ziet ook het gebruik van de gereserveerde exemplaar in de samenvatting van gebruik van [EA portal](https://ea.azure.com).

>[!NOTE]
>Als u het gereserveerde exemplaar in een context die betalen naar gebruik facturering hebt gekocht, Zie [gereserveerde exemplaar begrijpen gebruiksgegevens voor uw abonnement met betalen naar gebruik.](billing-understand-reserved-instance-usage.md)

Voor de volgende sectie wordt ervan uitgegaan dat u een Windows Standard_D1_v2 VM in de regio VS-Oost en uw gereserveerde exemplaar informatie ziet als in de volgende tabel eruit worden uitgevoerd:

| Veld | Waarde |
|---| --- |
|ReservationId |8f82d880-d33e-4e0d-bcb5-6bcb5de0c719|
|Hoeveelheid |1|
|SKU | Standard_D1|
|Regio | eastus |

## <a name="reserved-instance-application"></a>Gereserveerde exemplaar toepassing

Het gedeelte hardware van de virtuele machine wordt besproken, omdat de geïmplementeerde virtuele machine overeenkomt met de gereserveerde exemplaar kenmerken. Als u wilt zien welke Windows-software wordt niet gedekt door het gereserveerde exemplaar, gaat u naar Azure gereserveerde VM-instanties softwarekosten, gaat u naar [kosten voor software van Windows Azure reserveren VM-exemplaren.](billing-reserved-instance-windows-software-costs.md)


### <a name="reserved-instance-usage-in-csv"></a>Gebruik van de gereserveerde exemplaar in csv
U kunt de informatie over het gebruik EA csv downloaden van EA-portal. In het gedownloade csv-bestand filteren op aanvullende informatie en typt u in uw **ReservationID**. De volgende schermafbeelding ziet de velden met betrekking tot het gereserveerde exemplaar:

![Enterprise Agreement (EA) csv voor gereserveerde exemplaar van Azure](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-csv.png)

1. **ReservationId** in aanvullende informatie veld vertegenwoordigt het gereserveerde exemplaar dat is gebruikt voor het voordeel van toepassing op de virtuele machine.
2. ConsumptionMeter is de MeterId voor de virtuele machine.
3. Dit is de reservering Meter met $0 kosten omdat de kosten van het uitvoeren van de virtuele machine al is betaald door de gereserveerde exemplaar. 
4. Standard_D1 is één vCPU virtuele machine en de virtuele machine zonder Azure hybride voordeel is geïmplementeerd. Daarom deze meter bevat informatie over de extra kosten van Windows-software. Zie [kosten voor software van Windows Azure reserveren VM-exemplaren.](billing-reserved-instance-windows-software-costs.md) de meter die overeenkomt met de D-reeks 1 core VM vinden. Als Azure hybride voordeel wordt gebruikt, wordt deze extra kosten niet toegepast.

### <a name="reserved-instance-usage-in-usage-summary-page-in-ea-portal"></a>Gebruik van de gereserveerde exemplaar in de overzichtspagina voor gebruik in EA-portal

Gebruik ook in de samenvatting van gebruik van de portal EA weergegeven exemplaar gereserveerd: ![Enterprise Agreement (EA) samenvatting van gebruik](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-usagesummary.png)

1. Er zijn niet in rekening gebracht voor hardware-onderdeel van de virtuele machine als deze wordt omspannen door gereserveerde exemplaar. 
2. Er worden in rekening gebracht voor Windows-software zoals Azure hybride voordeel wordt niet gebruikt. 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over gereserveerde exemplaren van Azure, de volgende artikelen:

- [Geld besparen op virtuele machines met gereserveerde exemplaren van Azure](billing-save-compute-costs-reservations.md)
- [Vooruitbetalen voor virtuele Machines met gereserveerde exemplaren](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Gereserveerde exemplaren beheren](billing-manage-reserved-vm-instance.md)
- [Begrijpen hoe de gereserveerde exemplaar korting wordt toegepast](billing-understand-vm-reservation-charges.md)
- [Gebruik van de gereserveerde exemplaar voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
- [Kosten voor Windows-software niet zijn opgenomen in de gereserveerde exemplaren](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog steeds meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.
---
title: Gebruik Azure gereserveerde exemplaar voor uw abonnement op gebruiksbasis - Azure-facturering begrijpen | Microsoft Docs
description: Informatie over het lezen van uw gebruik om te begrijpen hoe de Azure gereserveerde VM-instantie voor uw abonnement op gebruiksbasis wordt toegepast.
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
ms.openlocfilehash: 7bf4aea86d4d430c15d60a8d73365705ace18b5a
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="understand-reserved-instance-usage-for-your-pay-as-you-go-subscription"></a>Gebruik van de gereserveerde exemplaar voor uw abonnement op gebruiksbasis begrijpen

Het gebruik van een exemplaar van Azure gereserveerde VM begrijpen met behulp van de ReservationId van [reservering pagina](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) en het bestand informatie over het gebruik van de [Accounts in Azure portal](https://account.azure.com).


>[!NOTE]
>In dit artikel geldt niet voor EA klanten. Als u een klant EA, Zie [gebruiksgegevens voor uw Enterprise enrollment gereserveerde exemplaar begrijpen.](billing-understand-reserved-instance-usage-ea.md) In dit artikel wordt ervan uitgegaan dat gereserveerde exemplaar wordt toegepast op één abonnement. Als het gereserveerde exemplaar wordt toegepast op meer dan één abonnement, kan gereserveerde exemplaar voordeel meerdere informatie over het gebruik van csv-bestanden omvatten. 

Voor de volgende sectie wordt ervan uitgegaan dat u een Standard_DS1_v2 Windows VM in de regio VS-Oost en uw gereserveerde exemplaar informatie ziet als in de volgende tabel eruit worden uitgevoerd:

| Veld | Waarde |
|---| :---: |
|ReservationId |8117adfb-1d94-4675-be2b-f3c1bca808b6|
|Hoeveelheid |1|
|SKU | Standard_DS1_v2|
|Regio | eastus |

## <a name="reserved-instance-application"></a>Gereserveerde exemplaar toepassing

Het gedeelte hardware van de virtuele machine wordt besproken, omdat de geïmplementeerde virtuele machine overeenkomt met de gereserveerde exemplaar kenmerken. Als u wilt zien welke Windows-software wordt niet gedekt door het gereserveerde exemplaar, gaat u naar [kosten voor software van Windows Azure reserveren VM-exemplaren.](billing-reserved-instance-windows-software-costs.md)

### <a name="statement-section-of-csv"></a>Sectie van de csv-instructie
Deze sectie van uw csv toont het totale gebruik voor uw gereserveerde exemplaar. Het filter toepassen op Meter subcategorie veld waarin 'Reservering-' en uw gegevens lijkt erop in de volgende schermafbeelding: ![schermafbeelding van gefilterde gereserveerde exemplaar gebruiksgegevens en kosten](./media/billing-understand-reserved-instance-usage/billing-payg-reserved-instance-csv-statements.png)

Reservering basis-VM regel bevat het totale aantal uren dat het gereserveerde exemplaar vallen. Deze regel is $0,00 omdat het exemplaar van de gereserveerde bedekt. Reservering Windows Svr (1 Kerngeheugen) regel bevat informatie over de kosten van het Windows-software.

### <a name="daily-usage-section-of-csv"></a>De sectie dagelijks gebruik van csv
Filteren op aanvullende informatie en typt u in uw **Reserverings-ID**. De volgende schermafbeelding ziet de velden met betrekking tot het gereserveerde exemplaar. 

![Schermopname van het dagelijkse gebruik en -kosten](./media/billing-understand-reserved-instance-usage/billing-payg-reserved-instance-csv-details.png)

1. **ReservationId** in aanvullende informatie is veld de gereserveerde exemplaar dat is gebruikt voor het voordeel van toepassing op de virtuele machine.
2. ConsumptionMeter is de Meter-Id voor de virtuele machine.
3. Reservering Base VM Meter subcategorie regel staat voor de regel van de kosten $0 in instructie sectie. Kosten van het uitvoeren van deze virtuele machine is al betaald door de gereserveerde exemplaar.
4. Dit is de Id van de Meter voor gereserveerde exemplaar. De kosten van deze meter is $0. Een virtuele machine die in aanmerking voor gereserveerde exemplaar komt heeft deze MeterId in de csv voor het account voor de kosten. 
5. Standard_DS1_v2 is één vCPU virtuele machine en de virtuele machine zonder Azure hybride voordeel is geïmplementeerd. Daarom deze meter bevat informatie over de extra kosten van Windows-software. Zie [kosten voor software van Windows Azure reserveren VM-exemplaren.](billing-reserved-instance-windows-software-costs.md) de meter die overeenkomt met de D-reeks 1 core VM vinden. Als Azure hybride voordeel wordt gebruikt, wordt deze extra kosten niet toegepast. 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over gereserveerde exemplaren, de volgende artikelen:

- [Geld besparen op virtuele machines met gereserveerde exemplaren van Azure](billing-save-compute-costs-reservations.md)
- [Vooruitbetalen voor virtuele Machines met gereserveerde exemplaren](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Gereserveerde exemplaren beheren](billing-manage-reserved-vm-instance.md)
- [Begrijpen hoe de gereserveerde exemplaar korting wordt toegepast](billing-understand-vm-reservation-charges.md)
- [Gereserveerde exemplaar gebruiksgegevens voor uw Enterprise enrollment begrijpen](billing-understand-reserved-instance-usage-ea.md)
- [Kosten voor Windows-software niet zijn opgenomen in de gereserveerde exemplaren](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog steeds meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.

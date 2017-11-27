---
title: Gebruik Azure gereserveerde exemplaar voor uw abonnement op gebruiksbasis begrijpen | Microsoft Docs
description: Meer informatie over het gebruik van uw toepassing begrijpen van gereserveerde exemplaar voor betalen per gebruik abonnement lezen
services: billing
documentationcenter: 
author: manish-shukla01
manager: manshuk
editor: 
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/03/2017
ms.author: manshuk
ms.openlocfilehash: 6982d6177ef5c94436a28cd68beb9feb5cc343e5
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="understand-reserved-instance-usage-for-your-pay-as-you-go-subscription"></a>Gebruik van de gereserveerde exemplaar voor uw abonnement op gebruiksbasis begrijpen

Gebruik van gereserveerde exemplaar begrijpen met behulp van de ReservationId van [reservering pagina](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade ) en het bestand informatie over het gebruik van [Accounts in Azure portal.](https://account.azure.com)


>[!NOTE]
>In dit artikel geldt niet voor EA klanten. Als u een klant EA, Zie [gebruiksgegevens voor uw Enterprise enrollment gereserveerde exemplaar begrijpen.](billing-understand-reserved-instance-usage-ea.md) In dit artikel wordt ervan uitgegaan dat de reservering wordt toegepast op één abonnement. Als de reservering wordt toegepast op meer dan één abonnement, mogelijk reservering voordeel meerdere informatie over het gebruik van csv-bestanden omvatten. 

Voor de volgende sectie wordt ervan uitgegaan dat u een virtuele machine Standard_DS1_v2 van Windows worden uitgevoerd in de regio VS-Oost en de reservering informatie ziet er als uit de volgende tabel:

| Veld | Waarde |
|---| :---: |
|ReservationId |8117adfb-1d94-4675-be2b-f3c1bca808b6|
|Hoeveelheid |1|
|SKU | Standard_DS1_v2|
|Regio | eastus |

## <a name="reservation-application"></a>Reservering toepassing

Het gedeelte hardware van de virtuele machine wordt besproken, omdat de geïmplementeerde virtuele machine overeenkomt met de reservering kenmerken. Als u wilt zien welke Windows-software wordt niet gedekt door het gereserveerde exemplaar, gaat u naar [kosten voor software van Windows Azure reserveren VM-exemplaren.](billing-reserved-instance-windows-software-costs.md)

### <a name="statement-section-of-csv"></a>Sectie van de csv-instructie
Deze sectie van uw csv toont het totale gebruik voor de reservering. Het filter toepassen op de subcategorie Meter veld waarin 'Reservering-' en uw gegevens lijkt erop in de volgende schermafbeelding: ![directe instructie reservering](./media/billing-understand-reserved-instance-usage/billing-payg-reserved-instance-csv-statements.png)

Reservering basis-VM regel bevat het totale aantal uren dat de reservering vallen. Deze regel is $0,00 omdat het exemplaar van de gereserveerde bedekt. Reservering Windows Svr (1 Kerngeheugen) regel bevat informatie over de kosten van het Windows-software.

### <a name="daily-usage-section-of-csv"></a>De sectie dagelijks gebruik van csv
Filteren op aanvullende informatie en typt u in uw reservering-ID. De volgende schermafbeelding ziet de velden die betrekking hebben op de reservering. 

![Dagelijkse gebruikskosten](./media/billing-understand-reserved-instance-usage/billing-payg-reserved-instance-csv-details.png)

1. ReservationId in het veld voor aanvullende informatie is de reservering dat is gebruikt voor het voordeel van toepassing op de virtuele machine.
2. ConsumptionMeter is de Meter-Id voor de virtuele machine.
3. Reservering Base VM Meter subcategorie regel staat voor de regel van de kosten $0 in instructie sectie. Kosten van het uitvoeren van deze virtuele machine is al door de reservering betaald.
4. Dit is de Id van de Meter voor reservering. De kosten van deze meter is $0. Een virtuele machine die in aanmerking voor gereserveerde exemplaar komt heeft deze MeterId in de csv voor het account voor de kosten. 
5. Standard_DS1_v2 is één vCPU virtuele machine en de virtuele machine zonder Azure hybride voordeel is geïmplementeerd. Daarom deze meter bevat informatie over de extra kosten van Windows-software. Zie [kosten voor software van Windows Azure reserveren VM-exemplaren.](billing-reserved-instance-windows-software-costs.md) de meter die overeenkomt met de D-reeks 1 core VM vinden. Als Azure hybride voordeel wordt gebruikt, wordt deze extra kosten niet toegepast. 

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.

Als u nog hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.
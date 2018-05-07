---
title: Gereserveerde exemplaar van Azure-gebruik voor ondernemingen begrijpen | Microsoft Docs
description: Informatie over het gebruik van uw toepassing van gereserveerde exemplaar voor uw Enterprise enrollment inzicht lezen.
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
ms.date: 11/03/2017
ms.author: manshuk
ms.openlocfilehash: cf79926e6497c50156f2a0191997ca06bc605c16
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="understand--reserved-instance-usage-for-your-enterprise-enrollment"></a>Gereserveerde exemplaar gebruiksgegevens voor uw Enterprise enrollment begrijpen
Gebruik van gereserveerde exemplaar begrijpen met behulp van de ReservationId van [reservering pagina](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade ) en het bestand informatie over het gebruik van [EA-portal.](https://ea.azure.com) U ziet ook het gebruik van de reservering in de samenvatting van gebruik van [EA-portal.](https://ea.azure.com)

>[!NOTE]
>Als u de reservering in de context van een betalen naar gebruik facturering hebt gekocht, Zie [gereserveerde exemplaar begrijpen gebruiksgegevens voor uw abonnement met betalen naar gebruik.](billing-understand-reserved-instance-usage.md)

Voor de volgende sectie wordt ervan uitgegaan dat u een virtuele machine Standard_D1_v2 van Windows worden uitgevoerd in de regio VS-Oost en de reservering informatie ziet er als uit de volgende tabel:

| Veld | Waarde |
|---| --- |
|ReservationId |8f82d880-d33e-4e0d-bcb5-6bcb5de0c719|
|Hoeveelheid |1|
|SKU | Standard_D1|
|Regio | eastus |

## <a name="reservation-application"></a>Reservering toepassing

Het gedeelte hardware van de virtuele machine wordt besproken, omdat de geïmplementeerde virtuele machine overeenkomt met de reservering kenmerken. Als u wilt zien welke Windows-software wordt niet gedekt door het gereserveerde exemplaar, gaat u naar Azure gereserveerde VM-instanties softwarekosten, gaat u naar [kosten voor software van Windows Azure reserveren VM-exemplaren.](billing-reserved-instance-windows-software-costs.md)


### <a name="reservation-usage-in-csv"></a>Gebruik van de reservering in csv
U kunt de informatie over het gebruik EA csv downloaden van EA-portal. In het gedownloade csv-bestand filteren op aanvullende informatie en typt u in uw reservering-ID. De volgende schermafbeelding ziet de velden die betrekking hebben op de reservering:

![EA csv voor gereserveerde exemplaar](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-csv.png)

1. ReservationId in het veld voor aanvullende informatie vertegenwoordigt de reservering dat is gebruikt voor het voordeel van toepassing op de virtuele machine.
2. ConsumptionMeter is de MeterId voor de virtuele machine.
3. Dit is de ReservationMeter met $0 kosten aangezien de kosten van het uitvoeren van de VM is al zijn betaald door de reservering. 
4. Standard_D1 is één vCPU virtuele machine en de virtuele machine zonder Azure hybride voordeel is geïmplementeerd. Daarom deze meter bevat informatie over de extra kosten van Windows-software. Zie [kosten voor software van Windows Azure reserveren VM-exemplaren.](billing-reserved-instance-windows-software-costs.md) de meter die overeenkomt met de D-reeks 1 core VM vinden. Als Azure hybride voordeel wordt gebruikt, wordt deze extra kosten niet toegepast.

### <a name="reservation-usage-in-usage-summary-page-in-ea-portal"></a>Gebruik van de reservering in de overzichtspagina voor gebruik in EA-portal

Gebruik ook in de samenvatting van gebruik van de portal EA weergegeven exemplaar gereserveerd: ![EA samenvatting van gebruik](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-usagesummary.png)

1. Er zijn niet in rekening gebracht voor hardware-onderdeel van de virtuele machine als deze wordt omspannen door gereserveerde exemplaar. 
2. Er worden in rekening gebracht voor Windows-software zoals Azure hybride voordeel wordt niet gebruikt. 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over gereserveerde virtuele Machine-exemplaren.

- [Vooruitbetalen voor virtuele Machines met een gereserveerde VM-exemplaren](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Exemplaren van de gereserveerde virtuele Machine beheren](billing-manage-reserved-vm-instance.md)
- [Geld besparen op virtuele machines met een gereserveerde virtuele Machine-exemplaren](billing-save-compute-costs-reservations.md)
- [Begrijpen hoe de korting exemplaar van de gereserveerde virtuele Machine wordt toegepast](billing-understand-vm-reservation-charges.md)
- [Gebruik van de gereserveerde exemplaar voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
- [Kosten voor Windows-software niet zijn opgenomen in de gereserveerde exemplaren](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.

Als u nog hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.
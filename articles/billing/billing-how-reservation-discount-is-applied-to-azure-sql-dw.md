---
title: Hoe reservering kortingen van toepassing op Azure SQL Data Warehouse | Microsoft Docs
description: Meer informatie over hoe reservering kortingen van toepassing op Azure SQL Data Warehouse kunt u geld besparen.
services: billing
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 04/13/2019
ms.author: banders
ms.openlocfilehash: 10e19377d31489cd19465fe6171ffb530bd58c28
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "60013740"
---
# <a name="how-reservation-discounts-apply-to-azure-sql-data-warehouse"></a>Hoe reservering kortingen van toepassing op Azure SQL Data Warehouse

Nadat u Azure SQL Data Warehouse gereserveerde capaciteit koopt, wordt de reserveringskorting wordt automatisch toegepast op datawarehouses die aanwezig zijn in deze regio. De reserveringskorting is van toepassing op het gebruik dat wordt verzonden door de SQL Data Warehouse cDWU-meter. Opslag en netwerken worden in rekening gebracht tegen betalen per gebruik-tarieven.

## <a name="reservation-discount-application"></a>Reservering-korting van toepassing

De korting voor SQL Data Warehouse gereserveerde capaciteit wordt toegepast op het uitvoeren van datawarehouses op uurbasis. Als u geen een datawarehouse voor een uur is geïmplementeerd, wordt de gereserveerde capaciteit verspild voor dat uur. Het wordt niet meegenomen.

Na de aankoop, wordt de reservering die u koopt vergeleken met SQL Data Warehouse gebruik die zijn verzonden door het uitvoeren van datawarehouses op elk gewenst moment in-time. Als u sommige magazijnen afsluit, toepassen reservering kortingen automatisch op andere overeenkomende datawarehouses.

Voor datawarehouses die niet worden uitgevoerd voor een volledig uur, wordt de reservering automatisch toegepast op andere bijbehorende exemplaren in dat uur.

## <a name="discount-examples"></a>Korting voor voorbeelden

De volgende voorbeelden ziet hoe de SQL Data Warehouse gereserveerde capaciteit korting van toepassing is, afhankelijk van de implementaties.

- **Voorbeeld 1**: U kopen 5 eenheden van 100 cDWU gereserveerde capaciteit. U hebt een DW1500c SQL Data Warehouse-exemplaar voor een uur wordt uitgevoerd. In dit geval wordt gebruik verzonden voor 15 eenheden van 100 cDWU-gebruik. De reserveringskorting is van toepassing op de 5-eenheden die u hebt gebruikt. De kosten worden berekend met behulp van betalen per gebruik-tarieven voor de resterende 10 eenheden van 100 cDWU gebruik die u hebt gebruikt.

- **Voorbeeld 2**: U kopen 5 eenheden van 100 cDWU gereserveerde capaciteit. U hebt twee DW100c SQL Data Warehouse-instanties gedurende een uur worden uitgevoerd. In dit geval worden er twee gebruiksgebeurtenissen verzonden voor 1 eenheid van 100 cDWU-gebruik. Beide gebruiksgebeurtenissen krijgen gereserveerde capaciteit kortingen. De resterende 3 eenheden van 100 cDWU gereserveerde capaciteit worden verspild en niet meegenomen voor toekomstig gebruik.

- **Voorbeeld 3**: U kopen 1 eenheid van 100 cDWU gereserveerde capaciteit. U hebt twee DW100c SQL Data Warehouse-exemplaren worden uitgevoerd. Elke 30 minuten uitgevoerd. In dit geval u beide gebruiksgebeurtenissen gereserveerde capaciteit kortingen. Er is geen gebruik wordt verrekend met behulp van betalen per gebruik-tarieven.

## <a name="need-help-contact-us"></a>Hulp nodig? Contact opnemen

- Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure-reserveringen, de volgende artikelen:

- [Wat zijn Azure-reserveringen?](billing-save-compute-costs-reservations.md)
- [Transacties van de reservering weergeven](billing-view-reservations.md)
- [Reservering-transacties en via API-gebruik](billing-reservation-apis.md)
- [Reserveringen beheren](billing-manage-reserved-vm-instance.md)

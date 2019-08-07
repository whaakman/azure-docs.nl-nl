---
title: Kosten voor vooruitbetalen voor SQL Data Warehouse met gereserveerde capaciteit van Azure
description: Meer informatie over hoe u kunt betalen voor SQL Data Warehouse kosten met gereserveerde capaciteit om geld te besparen.
services: billing
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 08/02/2019
ms.author: banders
ms.openlocfilehash: 6ee7a661434bb756c6cf196937229db19d06b373
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/05/2019
ms.locfileid: "68779984"
---
# <a name="prepay-for-sql-data-warehouse-charges-with-reserved-capacity"></a>Kosten voor vooruitbetalen voor SQL Data Warehouse met gereserveerde capaciteit

U kunt geld besparen met Azure SQL Data Warehouse door gedurende een periode van één of drie jaar voor uw cDWU-gebruik te betalen. Als u SQL Data Warehouse gereserveerde capaciteit wilt kopen, moet u de Azure-regio en de periode kiezen. Voeg vervolgens de SQL Data Warehouse SKU toe aan uw winkel wagen en kies het aantal cDWU-eenheden dat u wilt kopen.

Wanneer u een reserve ring koopt, wordt het SQL Data Warehouse gebruik dat overeenkomt met de reserverings kenmerken niet meer in rekening gebracht tegen de betalen naar gebruik-tarieven.

Een reserve ring dekt geen opslag-of netwerk kosten die zijn gekoppeld aan het SQL Data Warehouse gebruik.

Wanneer de gereserveerde capaciteit verloopt, worden SQL Data Warehouse exemplaren blijven worden uitgevoerd, maar worden er kosten in rekening gebracht op basis van het tarief voor betalen naar gebruik. Reserve ringen worden niet automatisch verlengd.

Voor prijs informatie raadpleegt u de [SQL Data Warehouse gereserveerde capaciteits aanbieding](https://azure.microsoft.com/pricing/details/sql-data-warehouse/gen2/).

U kunt Azure SQL Data Warehouse gereserveerde capaciteit kopen in de [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade). Gereserveerde capaciteit kopen:

- U moet de rol van eigenaar hebben voor ten minste één Enter prise-of betalen per gebruik-abonnement.
- Voor ondernemings abonnementen moet de optie **gereserveerde instanties toevoegen** zijn ingeschakeld in de [EA-Portal](https://ea.azure.com/). Als de instelling is uitgeschakeld, moet u een EA-beheerder zijn.
- Voor het programma Cloud Solution Provider (CSP) kunnen alleen beheerders of verkoop medewerkers SQL Data Warehouse gereserveerde capaciteit kopen.

Zie het [gebruik van Azure-reserve ringen voor uw Enter prise-inschrijving](billing-understand-reserved-instance-usage-ea.md) en [inzicht krijgen in het gebruik van Azure-reserve ringen voor uw bedrijf voor meer informatie over de manier waarop zakelijke klanten en betalen per gebruik-klanten in rekening worden gebracht voor reserverings aankopen. Betalen per gebruik-abonnement](billing-understand-reserved-instance-usage.md).

## <a name="choose-the-right-size-before-purchase"></a>Het juiste formaat kiezen voordat u het aankoopt

De SQL Data Warehouse reserverings grootte moet worden gebaseerd op de totale reken Data Warehouse-eenheden (cDWU) die u verbruikt. Aankopen worden gedaan in 100 cDWU-stappen.

Stel bijvoorbeeld dat uw totale verbruik van SQL Data Warehouse DW3000c is. U wilt gereserveerde capaciteit voor alles aanschaffen. Daarom moet u 30 eenheden van cDWU gereserveerde capaciteit kopen.

## <a name="buy-sql-data-warehouse-reserved-capacity"></a>SQL Data Warehouse gereserveerde capaciteit kopen

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer **alle services** > -**reserve ringen**.
3. Selecteer een abonnement. Gebruik de lijst met abonnementen om het abonnement te kiezen dat wordt gebruikt om te betalen voor de gereserveerde capaciteit. Voor de betalings wijze van het abonnement worden de kosten vooraf in rekening gebracht voor de gereserveerde capaciteit. Het abonnements type moet een Enter prise Agreement zijn (nummers van aanbiedingen: MS-AZR-0017P of MS-AZR-0148P) of betalen naar gebruik (aantal aanbiedingen: MS-AZR-0003P of MS-AZR-0023P).
   - Voor een Enterprise-abonnement worden de kosten in mindering gebracht op het toezeggingsbedrag of ze worden in rekening gebracht als overschrijding.
   - Voor een Betalen per gebruik-abonnement worden de kosten in rekening gebracht op de creditcard of de factuurbetalingswijze van het abonnement.
4. Selecteer een bereik. Gebruik de scope lijst om een abonnements bereik te kiezen.
   - **Bereik van één resource groep** : past de reserverings korting alleen toe op de overeenkomende resources in de geselecteerde resource groep.
   - **Bereik van één abonnement** : past de reserverings korting toe op de overeenkomende resources in het geselecteerde abonnement.
   - **Gedeeld bereik** : past de reserverings korting toe op overeenkomende resources in in aanmerking komende abonnementen in de facturerings context. De facturerings context is voor Enterprise Agreement klanten de inschrijving. Voor afzonderlijke abonnementen met betalen per gebruik-tarieven geldt het facturerings bereik uit alle in aanmerking komende abonnementen die zijn gemaakt door de account beheerder.
   - Voor zakelijke klanten is de facturerings context de EA-inschrijving.
   - Voor betalen per gebruik-klanten is de gedeelde Scope alle abonnementen met betalen per gebruik, gemaakt door de account beheerder.
5. Selecteer een regio om een Azure-regio te kiezen die wordt gedekt door de gereserveerde capaciteit.
6. Kies een hoeveelheid. Voer het aantal 100 data warehouse-eenheden (cDWU) in dat u wilt kopen.    
   Zo geeft een hoeveelheid van 30 bijvoorbeeld elk uur 3.000 cDWU met gereserveerde capaciteit.
7. Bekijk de SQL Data Warehouse gereserveerde capaciteits kosten van de reserve ring in het gedeelte **kosten** .
8. Selecteer **Aankoop**.
9. Selecteer **deze reserve ring weer geven** om de status van uw aankoop te bekijken.

## <a name="cancel-exchange-or-refund-reservations"></a>Reserve ringen annuleren, omruilen of terugbetalen

U kunt reserve ringen annuleren, vervangen of terugbetalen met bepaalde beperkingen. Zie [self-service-uitwisseling en terugbetalingen voor Azure Reservations](billing-azure-reservations-self-service-exchange-and-refund.md)voor meer informatie.

Er wordt automatisch een reserverings korting toegepast op het aantal SQL Data Warehouse exemplaren dat overeenkomt met het bereik en de regio van de gereserveerde capaciteit van SQL Data Warehouse. U kunt het bereik van de SQL Data Warehouse gereserveerde capaciteit bijwerken met de [Azure Portal](https://portal.azure.com/), Power shell, CLI of via de API.

## <a name="need-help-contact-us"></a>Hulp nodig? Contact opnemen

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/).

## <a name="next-steps"></a>Volgende stappen

- Zie hoe reserverings kortingen van [toepassing zijn op Azure SQL Data Warehouse](billing-prepay-sql-data-warehouse-charges-with-reserved-capacity.md)voor meer informatie over hoe reserverings kortingen van toepassing zijn op Azure SQL Data Warehouse.

- Raadpleeg de volgende artikelen voor meer informatie over Azure Reservations:
  - [Wat zijn Azure Reservations?](billing-save-compute-costs-reservations.md)
  - [Azure-reserveringen beheren](billing-manage-reserved-vm-instance.md)
  - [Azure Reservations korting begrijpen](billing-understand-reservation-charges.md)
  - [Het gebruik van de reserve ring begrijpen voor uw abonnement voor betalen naar gebruik](billing-understand-reserved-instance-usage.md)
  - [Het gebruik van de reserve ring begrijpen voor uw Enter prise-inschrijving](billing-understand-reserved-instance-usage-ea.md)

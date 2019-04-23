---
title: Betaal vooruit voor SQL Data Warehouse-kosten met Azure gereserveerde capaciteit | Microsoft Docs
description: Meer informatie over hoe u kunt vooraf betaald voor kosten voor SQL Data Warehouse met gereserveerde capaciteit om geld te besparen.
services: billing
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 04/13/2019
ms.author: banders
ms.openlocfilehash: 3d56ee99f8056139a234477b6eed93d9fcbe2bbb
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "60013539"
---
# <a name="prepay-for-sql-data-warehouse-charges-with-reserved-capacity"></a>Betaal vooruit voor SQL Data Warehouse kosten in rekening gebracht met gereserveerde capaciteit

U kunt geld besparen met Azure SQL Data Warehouse door vooruitbetaalt voor uw gebruik cDWU gedurende een periode van één of drie jaar. Om aan te schaffen SQL Data Warehouse gereserveerde capaciteit, moet u kiezen voor de Azure-regio en termijn. Vervolgens de SKU van SQL Data Warehouse aan uw winkelwagen toevoegen en kiest u het aantal cDWU-eenheden die u wilt kopen.

Wanneer u een reservering koopt, gaat u de SQL Data Warehouse gebruik die overeenkomt met de kenmerken van de reservering niet meer wordt verrekend tegen het betaalt u tarieven.

Een reservering dekt niet opslag of netwerken kosten die zijn gekoppeld aan het gebruik van SQL Data Warehouse.

Wanneer de gereserveerde capaciteit is verlopen, SQL Data Warehouse-exemplaren worden uitgevoerd, maar worden in rekening gebracht het tarief voor betalen waarbij u gaat. Reserveringen verlengd niet automatisch.

Zie voor informatie over de prijzen, de [SQL Data Warehouse gereserveerde capaciteit aanbieding](https://azure.microsoft.com/pricing/details/sql-data-warehouse/gen2/).

U kunt Azure SQL Data Warehouse gereserveerde capaciteit kopen de [Azure-portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade). Gereserveerde capaciteit kopen:

- U moet de rol van eigenaar voor ten minste één enterprise of betalen per gebruik-abonnement hebben.
- Voor Enterprise-abonnementen, de **gereserveerde instanties toevoegen** optie moet zijn ingeschakeld in de [EA-portal](https://ea.azure.com/). Als de instelling is uitgeschakeld, moet u een EA-beheerder.
- Voor het programma Cloud Solution Provider (CSP), kunnen alleen de agents beheerder of de verkoop agents SQL Data Warehouse gereserveerde capaciteit kopen.

Zie voor meer informatie over hoe betalen per gebruik en enterprise-klanten kosten in rekening voor reservering aankopen gebracht worden [inzicht in Azure-reservering gebruik voor uw Enterprise-inschrijving](billing-understand-reserved-instance-usage-ea.md) en [inzicht in Azure gebruik van de reservering voor uw abonnement op gebruiksbasis](billing-understand-reserved-instance-usage.md).

## <a name="choose-the-right-size-before-purchase"></a>Kies de juiste grootte voor aankoop

De SQL Data Warehouse reserveringsgrootte moet worden gebaseerd op de totale compute datawarehouse units (cDWU) die u verbruikt. Aankopen worden in stappen van 100 cDWU gedaan.

Stel dat uw totale gebruik van SQL Data Warehouse is DW3000c. U wilt kopen van gereserveerde capaciteit voor alle ervan. U moet dus 30 eenheden van cDWU gereserveerde capaciteit kopen.

## <a name="buy-sql-data-warehouse-reserved-capacity"></a>SQL Data Warehouse gereserveerde capaciteit kopen

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/).
2. Selecteer **alle services** > **reserveringen**.
3. Selecteer een abonnement. Gebruik de lijst met abonnementen om het abonnement dat wordt gebruikt om te betalen voor de gereserveerde capaciteit. De betalingswijze van het abonnement wordt in rekening gebracht de kosten vooraf voor de gereserveerde capaciteit. Het abonnementstype moet een enterprise agreement (getallen bieden: MS-AZR-0017P of MS-AZR - 0148 P) en betalen per gebruik (getallen bieden: MS-AZR-0003P of MS-AZR - 0023 P).
  - Voor een Enterprise-abonnement worden de kosten in mindering gebracht op het toezeggingsbedrag of ze worden in rekening gebracht als overschrijding.
  - Voor een Betalen per gebruik-abonnement worden de kosten in rekening gebracht op de creditcard of de factuurbetalingswijze van het abonnement.
4. Selecteer een bereik. Gebruik de lijst Scope om te kiezen van een bereik van het abonnement.
  - Met de **één** optie, de reserveringskorting wordt toegepast op SQL Data Warehouse is geïmplementeerd in het geselecteerde abonnement.
  - Met de **gedeelde** optie, de reserveringskorting wordt toegepast op de exemplaren die worden uitgevoerd in alle abonnementen binnen de context van de facturering.
    - Voor zakelijke klanten is de context van de facturering de EA-inschrijving.
    - Voor klanten van betalen per gebruik is het gedeelde bereik alle betalen per gebruik-abonnementen die zijn gemaakt door de accountbeheerder.
5. Selecteer een regio te kiezen van een Azure-regio die wordt gedekt door de gereserveerde capaciteit.
6. Kies een volume. Geef het aantal 100 Data Warehouse units (cDWU) die u wilt kopen.    
  Bijvoorbeeld, een aantal van 30, krijgt u 3.000 cDWU van gereserveerde capaciteit per uur.
7. Controleer de SQL Data Warehouse gereserveerde capaciteit reserveringskosten bij de **kosten** sectie.
8. Selecteer **Aankoop**.
9. Selecteer **deze reservering weergeven** om te zien van de status van uw aankoop.

## <a name="cancellations-and-exchanges"></a>Annuleringen en uitwisselingen

Als u nodig hebt om te annuleren van uw SQL Data Warehouse gereserveerde capaciteit, wordt er mogelijk een kosten voor vroegtijdige beëindiging van 12%. Restituties worden gebaseerd op de laagste prijs, zijnde de aankoopprijs of de huidige prijs van de reservering, welke het laagste is. Restituties zijn beperkt tot $50,000.00 per jaar. U ontvangt restitutie is het resterende bedrag naar rato minus de kosten voor vroegtijdige beëindiging van 12%. Om aan te vragen bij een annulering, gaat u naar de reservering in de Azure portal en selecteer **gerestitueerd** een ondersteuningsaanvraag wilt maken.

Als u wijzigen van de capaciteit van uw SQL Data Warehouse is gereserveerd op een andere regio of term wilt, kunt u het uitwisselen voor een andere reservering van gelijke of grotere waarde. De begindatum van de periode voor de nieuwe reservering wordt niet meegenomen naar de uitgewisselde reservering. Een of drie jaar term wordt gestart bij het maken van de nieuwe reservering. Om aan te vragen bij een uitwisseling, opent u de reservering in de Azure-portal en selecteer **Exchange** een ondersteuningsaanvraag wilt maken.

Zie voor meer informatie over het exchange- of restitutie reserveringen [reservering worden uitgewisseld en terugbetalingen](billing-azure-reservations-self-service-exchange-and-refund.md).

De reserveringskorting wordt automatisch toegepast op het aantal exemplaren van SQL Data Warehouse die overeenkomen met de SQL Data Warehouse gereserveerde capaciteit bereik en de regio. U kunt het bereik van de capaciteit van de SQL Data Warehouse gereserveerd met bijwerken de [Azure-portal](https://portal.azure.com/), PowerShell, CLI of via de API.

## <a name="need-help-contact-us"></a>Hulp nodig? Contact opnemen

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/).

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over hoe reservering kortingen van toepassing op Azure SQL Data Warehouse, [hoe reservering kortingen van toepassing op Azure SQL Data Warehouse](billing-prepay-sql-data-warehouse-charges-with-reserved-capacity.md).

- Zie voor meer informatie over Azure-reserveringen, de volgende artikelen:
  - [Wat zijn Azure-reserveringen?](billing-save-compute-costs-reservations.md)
  - [Azure-reserveringen beheren](billing-manage-reserved-vm-instance.md)
  - [Korting op Azure reserveringen begrijpen](billing-understand-reservation-charges.md)
  - [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
  - [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](billing-understand-reserved-instance-usage-ea.md)

---
title: Betaal vooruit voor Azure SQL Database vCores om geld te besparen | Microsoft Docs
description: Meer informatie over het aanschaffen van Azure SQL Database gereserveerde capaciteit om op te slaan op uw rekenkosten.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: e9edbc2a3bbc878f7a3cf99a3ca6efb4e69797ad
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394707"
---
# <a name="prepay-for-sql-database-compute-resources-with-azure-sql-database-reserved-capacity"></a>Betaal vooruit voor SQL Database-compute-resources met Azure SQL Database gereserveerde capaciteit

Bespaar geld met Azure SQL Database door vooruitbetaalt voor Azure SQL Database-rekenresources in vergelijking met betalen per gebruik. Met Azure SQL Database gereserveerde capaciteit maken u een toezegging op SQL-Database voor een periode van één of drie jaar om op te halen van een aanzienlijke korting op de rekenkosten. Om aan te schaffen SQL-Database gereserveerde capaciteit, moet u de Azure-regio, het implementatietype, de prestatielaag en de term opgeven. 

U hoeft niet de reservering toewijzen aan SQL Database-exemplaren. Die overeenkomen met SQL Database-exemplaren die al worden uitgevoerd of die onlangs zijn geïmplementeerd, krijgen automatisch het voordeel. Door een reservering koopt, betaalt u vooraf voor de compute-kosten voor de SQL Database-exemplaren voor een periode van één of drie jaar. Als u een reservering koopt, gaat u de SQL-Database rekenkosten in rekening gebracht die overeenkomen met de kenmerken van de reservering niet meer in rekening gebracht tegen de betalen waarbij u tarieven. Kosten voor software, netwerk of opslag is gekoppeld aan de SQL-Database-exemplaar heeft geen betrekking op een reservering. De facturering benefit verloopt aan het einde van de reserveringstermijn, en de SQL-Databases in rekening gebracht volgens de betaalt u Ga-prijs. Reserveringen kunnen niet automatisch verlengen. Zie voor informatie over de prijzen, de [SQL-Database gereserveerde capaciteit aanbieding](https://azure.microsoft.com/pricing/details/sql-database/managed/).

U kunt Azure SQL Database gereserveerde capaciteit kopen de [Azure-portal](https://portal.azure.com). Gereserveerde capaciteit voor het kopen van SQL Database:
- U moet zich in de rol van eigenaar voor ten minste één Enterprise of abonnement op gebruiksbasis.
- Voor Enterprise-abonnementen, aankopen in de Azure-reservering moeten zijn ingeschakeld in de [EA-portal](https://ea.azure.com).
-  Programma Cloud Solution Provider (CSP), kunnen alleen de agents beheerder of de verkoop agents SQL-Database gereserveerde capaciteit kopen.

Zie de informatie over hoe betalen per gebruik en enterprise-klanten worden kosten in rekening gebracht voor reservering, aankopen [over Azure-reservering gebruik voor uw Enterprise-inschrijving](../billing/billing-understand-reserved-instance-usage-ea.md) en [inzicht in Azure-reservering Gebruik voor uw abonnement op gebruiksbasis](../billing/billing-understand-reserved-instance-usage.md).

## <a name="determine-the-right-sql-size-before-purchase"></a>Bepaal de juiste SQL-grootte voor aankoop

De grootte van de reservering moet worden gebaseerd op de totale hoeveelheid rekenkracht die wordt gebruikt door de bestaande of snel-naar--geïmplementeerd SQL enkele databases en/of elastische groepen binnen een bepaalde regio en het gebruik van dezelfde prestaties laag en hardware generatie. 

Bijvoorbeeld: Stel dat u een algemene, Gen5 – 16 vCore elastische pool en twee bedrijfskritiek, Gen5 – 4 vCore individuele databases uitvoert. Bovendien mag gaan we dat u van plan bent om te implementeren in de volgende maand een extra algemene, Gen5 – 16 vCore elastische pool, en één business kritiek, Gen5 – 32 vCore elastische pool. Ook Stel dat u weet dat u deze resources voor ten minste 1 jaar moet. In dit geval moet u een 32 kopen (2 x 16) vCores, 1 jaar reservering voor SQL Database Single/elastische Pool voor algemeen gebruik - Compute Gen5 en een 40 (32 x 4 + 2) 1 jaar-reservering voor SQL Database Single/elastische Pool bedrijfskritiek - Compute Gen5 vCore.

## <a name="buy-sql-database-reserved-capacity"></a>SQL-Database gereserveerde capaciteit kopen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle services** > **reserveringen**.
3. Selecteer **toevoegen** en selecteer vervolgens in het deelvenster met de Product-Type selecteren **SQL-Database** naar een nieuwe reservering kopen voor SQL-Database.
4. Vul de vereiste velden in. Bestaande of nieuwe individuele databases of elastische pools die overeenkomen met de kenmerken die u in aanmerking komen voor het ophalen van de korting gereserveerde capaciteit. Het werkelijke nummer van uw SQL Database-exemplaren die aan de korting is afhankelijk van het bereik en de hoeveelheid die is geselecteerd.

   ![Schermafbeelding voor het indienen van de SQL-Database gereserveerde capaciteit kopen](./media/sql-database-reserved-vcores/sql-reserved-vcores-purchase.png)

    | Veld      | Beschrijving|
    |:------------|:--------------|
    |Naam        |De naam van deze reservering.| 
    |Abonnement|Het abonnement dat wordt gebruikt om te betalen voor de reservering van SQL-Database gereserveerde capaciteit. De betalingswijze voor het abonnement wordt in rekening gebracht de kosten vooraf voor de reservering van SQL-Database gereserveerde capaciteit. Het abonnement moet een Enterprise-overeenkomst (aanbieding: MS-AZR-0017P) of een Betalen-per-gebruik-abonnement (aanbieding: MS-AZR-0003P) zijn. Voor een Enterprise-abonnement worden de kosten in mindering gebracht op het toezeggingsbedrag of ze worden in rekening gebracht als overschrijding. Voor een Betalen per gebruik-abonnement worden de kosten in rekening gebracht op de creditcard of de factuurbetalingswijze van het abonnement.|    
    |Bereik       |Bereik van de vCore-reservering kan betrekking hebben op één abonnement of meerdere abonnementen (gedeeld bereik). Als u selecteert: <ul><li>Eén abonnement - de reserveringskorting vCore wordt toegepast op SQL Database-exemplaren in dit abonnement. </li><li>Gedeeld - wordt de reserveringskorting vCore toegepast op SQL Database-exemplaren die worden uitgevoerd in alle abonnementen binnen de context van de facturering. Voor zakelijke klanten, gedeeld bereik is van de inschrijving en bevat alle abonnementen (met uitzondering van dev/test-abonnementen) binnen de inschrijving. Voor klanten van betalen per gebruik is het gedeelde bereik alle betalen per gebruik-abonnementen die zijn gemaakt door de accountbeheerder.</li></ul>|
    |Regio      |De Azure-regio die wordt gedekt door de SQL-Database gereserveerde capaciteit.|    
    |Implementatietype|Het type van de SQL-resource die u wilt kopen, de reservering voor.|
    |Prestatielaag|De servicelaag voor de SQL Database-exemplaren.
    |Termijn        |Één of drie jaar.|
    |Hoeveelheid    |Het aantal exemplaren dat u hebt gekocht in de SQL-Database gereserveerde capaciteit. Het aantal is het aantal actieve SQL Database-exemplaren die de korting voor facturering krijgt. Bijvoorbeeld, als u 10 SQL Database-exemplaren in de VS-Oost, geeft vervolgens u hoeveelheid 10 te optimaliseren voor alle machines die worden uitgevoerd. |

5. Controleer de kosten van de SQL-Database gereserveerde capaciteitsreservering in de **kosten** sectie.
6. Selecteer **Aankoop**.
7. Selecteer **deze reservering weergeven** om te zien van de status van uw aankoop.

## <a name="cancellations-and-exchanges"></a>Annuleringen en uitwisselingen

Als u nodig hebt om te annuleren van uw SQL-Database gereserveerde capaciteit, kunnen er een kosten voor vroegtijdige beëindiging van 12%. Restituties zijn gebaseerd op de laagste prijzen van uw aankoopprijs of de huidige prijs van de reservering. Restituties zijn beperkt tot 50.000 per jaar. U ontvangt restitutie is het resterende bedrag naar rato minus de kosten voor vroegtijdige beëindiging van 12%. Om aan te vragen bij een annulering, gaat u naar de reservering in de Azure portal en selecteer **gerestitueerd** een ondersteuningsaanvraag wilt maken.

Als u wijzigen van de SQL-Database gereserveerde capaciteitsreservering naar een andere regio, implementatietype, prestatielaag of term wilt, kunt u het uitwisselen voor een andere reservering van gelijke of grotere waarde. De begindatum van de termijn voor de nieuwe reservering niet meegenomen in de reservering uitgewisseld. De 1 of 3 jaar term wordt gestart wanneer u de nieuwe reservering maakt. Om aan te vragen bij een uitwisseling, gaat u naar de reservering in Azure portal en selecteer **Exchange** een ondersteuningsaanvraag wilt maken.

## <a name="vcore-size-flexibility"></a>vCore grootte flexibiliteit

vCore grootte flexibiliteit kunt u omhoog of omlaag schalen binnen een prestatielaag en de regio, zonder verlies van het voordeel voor gereserveerde capaciteit. SQL-Database gereserveerde capaciteit biedt u ook de flexibiliteit om tijdelijk uw hot-databases verplaatsen tussen pools en individuele databases als onderdeel van uw normale bewerkingen (in dezelfde regio en de prestaties laag) zonder verlies van de gereserveerde capaciteit profiteren. Doordat een buffer niet toegepast op uw reservering kunt u de prestaties van pieken effectief beheren zonder uw budget overschrijdt.

## <a name="next-steps"></a>Volgende stappen

Het vCore-reserveringskorting wordt automatisch toegepast op het aantal exemplaren van SQL-Database die overeenkomen met de SQL-Database gereserveerde capaciteit reserveringsbereik en kenmerken. U kunt het bereik van de reservering van de SQL-Database gereserveerde capaciteit via bijwerken [Azure-portal](https://portal.azure.com), PowerShell, CLI of via de API. 

Voor informatie over het beheren van de SQL-Database gereserveerde capaciteitsreservering, Zie [SQL Database beheren gereserveerde capaciteit](../billing/billing-manage-reserved-vm-instance.md).

Zie voor meer informatie over Azure-reserveringen, de volgende artikelen:

- [Wat zijn Azure-reserveringen?](../billing/billing-save-compute-costs-reservations.md)
- [Azure-reserveringen beheren](../billing/billing-manage-reserved-vm-instance.md)
- [Korting op Azure reserveringen begrijpen](../billing/billing-understand-reservation-charges.md)
- [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](../billing/billing-understand-reserved-instance-usage.md)
- [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](../billing/billing-understand-reserved-instance-usage-ea.md)
- [Azure-reserveringen in programma Partner Center Cloud Solution Provider (CSP)](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.


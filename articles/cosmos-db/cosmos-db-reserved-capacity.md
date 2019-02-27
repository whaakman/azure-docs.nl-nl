---
title: Kosten van Azure Cosmos DB-resources optimaliseren met gereserveerde capaciteit
description: Meer informatie over het aanschaffen van Azure Cosmos DB gereserveerde capaciteit om op te slaan op uw rekenkosten.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 01/18/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: e7a5ed6274a0429726f0ed19c0765cfdc3378daa
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56888522"
---
# <a name="optimize-cost-with-reserved-capacity-in-azure-cosmos-db"></a>Kosten optimaliseren met gereserveerde capaciteit in Azure Cosmos DB

Azure Cosmos DB gereserveerde capaciteit kunt die u geld besparen door vooraf betalen voor Azure Cosmos DB-resources voor één jaar of drie jaar. Met Azure Cosmos DB gereserveerde capaciteit krijgt u een korting op de ingerichte doorvoer voor Cosmos DB-resources. Voorbeelden van resources zijn databases en containers (tabellen, verzamelingen en grafieken).

Azure Cosmos DB gereserveerde capaciteit uw Cosmos DB-kosten aanzienlijk kunt verlagen&mdash;tot 65 procent op reguliere prijzen met een toezegging van één jaar of drie jaar. Gereserveerde capaciteit biedt een korting van facturering en heeft geen invloed op de runtimestatus van uw Azure Cosmos DB-resources.

Azure Cosmos DB gereserveerde capaciteit bevat informatie over de doorvoer die is ingericht voor uw resources. Dekt niet kosten voor de opslag en netwerken. Als u een reservering koopt, gaat u de doorvoer kosten in rekening gebracht die overeenkomen met de reservering kenmerken niet meer in tegen de betalen waarbij u gebracht rekening tarieven. Zie voor meer informatie over reserveringen, de [Azure reserveringen](../billing/billing-save-compute-costs-reservations.md) artikel. 

U kunt Azure Cosmos DB gereserveerde capaciteit uit kopen de [Azure-portal](https://portal.azure.com). Gereserveerde capaciteit kopen:

* U moet zich in de rol van eigenaar voor ten minste één Enterprise of abonnement op gebruiksbasis.  
* Voor Enterprise-abonnementen, **gereserveerde instanties toevoegen** moet zijn ingeschakeld in de [EA-portal](https://ea.azure.com). Of, als deze instelling is uitgeschakeld, moet u een EA-beheerder voor het abonnement zijn.
* Voor het programma Cloud Solution Provider (CSP), kunnen alleen admin-agenten of verkoop agents Azure Cosmos DB gereserveerde capaciteit aanschaffen.

## <a name="determine-the-required-throughput-before-purchase"></a>De vereiste doorvoer voor aankoop bepalen

De grootte van de reservering moet worden gebaseerd op de totale hoeveelheid doorvoer die de bestaande of snel-naar--geïmplementeerd Azure Cosmos DB-resources wilt gebruiken. U kunt de vereiste doorvoer bepalen in de volgende manieren:

* De historische gegevens ophalen voor de totale ingerichte doorvoer in uw Azure Cosmos DB-accounts, databases en verzamelingen in alle regio's. Bijvoorbeeld, kunt u de dagelijkse gemiddelde ingerichte doorvoer evalueren door te downloaden van uw dagelijkse gebruik overzicht van `https://account.azure.com`.

* Als u een Enterprise Agreement (EA)-klant bent, kunt u het gebruiksbestand de informatie van de doorvoer van Azure Cosmos DB kunt downloaden. Raadpleeg de **servicetype** waarde in de **aanvullende informatie** sectie van het gebruiksbestand.

* U kunt de gemiddelde doorvoer voor alle werkbelastingen optellen van uw Azure Cosmos DB-accounts die u verwacht om uit te voeren voor de volgende één of drie jaar. U kunt deze hoeveelheid vervolgens gebruiken voor de reservering.

## <a name="buy-azure-cosmos-db-reserved-capacity"></a>Azure Cosmos DB gereserveerde capaciteit kopen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).  

2. Selecteer **alle services** > **reserveringen** > **toevoegen**.  

3. Uit de **Product-Type selecteren** deelvenster kiezen **Azure Cosmos DB** > **Selecteer** een nieuwe reservering kopen.  

4. Vul de vereiste velden in zoals beschreven in de volgende tabel:

   ![Vul in het formulier gereserveerde capaciteit](./media/cosmos-db-reserved-capacity/fill_reserved_capacity_form.png) 

   |Veld  |Description  |
   |---------|---------|
   |Name   |    De naam van de reservering. Dit veld wordt automatisch gevuld met `CosmosDB_Reservation_<timeStamp>`. U kunt een andere naam opgeven tijdens het maken van de reservering. Of u kunt de naam wijzigen nadat de reservering is gemaakt.      |
   |Abonnement  |   Abonnement dat wordt gebruikt om te betalen voor de Azure Cosmos DB gereserveerde capaciteit. De betalingswijze voor het geselecteerde abonnement wordt gebruikt in de kosten vooraf in rekening gebracht. Het abonnementstype moet een van de volgende: <br/><br/>  Enterprise Agreement (getallen bieden: MS-AZR-0017P of MS-AZR - 0148 P): De kosten zijn voor een Enterprise-abonnement in mindering gebracht op de monetaire toezeggingsbedrag van de inschrijving of kosten in rekening gebracht als overschrijding. <br/><br/> Betalen per gebruik (getallen bieden: MS-AZR-0003P or MS-AZR-0023P): Voor een betalen per gebruik-abonnement, de kosten worden in rekening gebracht op de betalingsmethode creditcard of per factuur voor het abonnement.    |
   |Bereik   |   Deze optie bepaalt het aantal abonnementen dat is gekoppeld aan de reservering facturering voordeel kunnen gebruiken. Ook wordt bepaald hoe de reservering wordt toegepast op bepaalde abonnementen.   <br/><br/>  Als u selecteert **enkel abonnement**, de reserveringskorting wordt toegepast op Azure Cosmos DB-exemplaren in het geselecteerde abonnement. <br/><br/>  Als u selecteert **gedeelde**, de reserveringskorting wordt toegepast op Azure Cosmos DB-exemplaren die worden uitgevoerd in alle abonnementen in de context van de facturering. De context van de facturering is gebaseerd op hoe u zich hebt geregistreerd voor Azure. Voor zakelijke klanten, gedeeld bereik is van de inschrijving en bevat alle abonnementen binnen de inschrijving. Voor klanten van betalen per gebruik is het gedeelde bereik alle betalen per gebruik-abonnementen die zijn gemaakt door de accountbeheerder.  <br/><br/> U kunt het reserveringsbereik wijzigen nadat u de gereserveerde capaciteit kopen.  |
   |Type gereserveerde capaciteit   |  Doorvoer die is ingericht als basis van aanvraageenheden.|
   |Eenheden gereserveerde capaciteit  |      Hoeveelheid doorvoer die u wilt reserveren. U kunt deze waarde berekenen met het bepalen van de doorvoer die nodig zijn voor al uw Cosmos DB-resources (bijvoorbeeld, databases of containers) per regio. U vervolgens vermenigvuldigd met het aantal regio's die u aan uw Cosmos DB-database koppelen moet.  <br/><br/> Bijvoorbeeld: Als u vijf regio's met 1 miljoen RU/sec. in elke regio hebt, selecteert u 5 miljoen RU/sec voor het kopen van reserveringen capaciteit.    |
   |Termijn  |   Één of drie jaar.   |

5. Bekijk de korting en de prijs van de reservering in de **kosten** sectie. Deze reservering prijs is van toepassing op Azure Cosmos DB-resources met doorvoer die is ingericht in alle regio's.  

6. Selecteer **Aankoop**. Wanneer de aankoop voltooid is, ziet de volgende pagina: 

   ![Vul in het formulier gereserveerde capaciteit](./media/cosmos-db-reserved-capacity/reserved_capacity_successful.png) 

Nadat u een reservering koopt, wordt deze onmiddellijk toegepast op bestaande Azure Cosmos DB-resources die overeenkomen met de voorwaarden van de reservering. Als u geen bestaande Azure Cosmos DB-resources, wordt de reservering wordt toegepast wanneer u een nieuwe Cosmos DB-exemplaar dat overeenkomt met de voorwaarden van de reservering implementeert. In beide gevallen wordt de periode van de reservering direct na een succesvolle aankoop gestart. 

Wanneer uw reservering verloopt, wordt uw Azure Cosmos DB-exemplaren worden uitgevoerd en worden gefactureerd tegen de normale betalen per gebruik-tarieven betalen.

## <a name="cancellation-and-exchanges"></a>Annulering en uitwisselingen

Zie voor hulp bij het identificeren van de juiste gereserveerde capaciteit [te begrijpen hoe de reserveringskorting wordt toegepast op Azure Cosmos DB](../billing/billing-understand-cosmosdb-reservation-charges.md). In het geval, u wilt annuleren of uitwisselen van een Azure Cosmos DB-reservering, neem contact op met het team van Azure Cosmos DB via `askCosmosDB@microsoft.com`.

## <a name="next-steps"></a>Volgende stappen

De reserveringskorting wordt automatisch toegepast op de Azure Cosmos DB-resources die overeenkomen met het reserveringsbereik en kenmerken. U kunt het bereik van de reservering via Azure portal, PowerShell, Azure CLI of de API bijwerken.

*  Zie voor meer informatie hoe gereserveerde capaciteit kortingen zijn toegepast op Azure Cosmos DB, [inzicht in de Azure reserveringskorting](../billing/billing-understand-cosmosdb-reservation-charges.md).

* Zie voor meer informatie over Azure reserveringen, de volgende artikelen:

   * [Wat zijn Azure reserveringen?](../billing/billing-save-compute-costs-reservations.md)  
   * [Azure-reserveringen beheren](../billing/billing-manage-reserved-vm-instance.md)  
   * [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](../billing/billing-understand-reserved-instance-usage-ea.md)  
   * [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](../billing/billing-understand-reserved-instance-usage.md)
   * [Azure-reserveringen in het Partner Center CSP-programma](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).


---
title: Wat zijn Azure-reserveringen?
description: Meer informatie over Azure-reserveringen en prijzen op te slaan op uw virtuele machines, SQL-databases, Azure Cosmos DB en andere resourcekosten.
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 07/03/2019
ms.author: banders
ms.openlocfilehash: cd0a70aa0fb5096c5b0157ae078c961da03109bc
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565320"
---
# <a name="what-are-azure-reservations"></a>Wat zijn Azure-reserveringen?

Azure reserveringen kunnen u geld besparen door vooraf betalen voor een eenjarige of driejarige van virtuele machines, SQL-Database compute-capaciteit, doorvoer van Azure Cosmos DB of andere Azure-resources. Vooraf betaalt, kunt u krijg een korting op de resources die u gebruikt. Uw virtuele machine, compute-SQL-database, Azure Cosmos DB kunnen aanzienlijk verkorten door reserveringen of andere bron kosten tot 72% bij betalen per gebruik. Reserveringen bieden een korting van facturering en hebben geen invloed op de runtimestatus van uw resources.

U kunt een reservering kopen de [Azure-portal](https://ms.portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).

## <a name="why-buy-a-reservation"></a>Waarom zou ik een reservering kopen?

Hebt u virtuele machines, Azure Cosmos DB of SQL-databases die langdurig worden uitgevoerd, biedt een reservering koopt u de meest voordelige optie zijn. Bijvoorbeeld, wanneer u doorlopend vier exemplaren van een service zonder een reservering uitvoert, u bent in rekening gebracht tegen betalen per gebruik-tarieven. Als u een reservering voor deze resources koopt, krijgt u onmiddellijk de reserveringskorting. De resources zijn niet meer in rekening gebracht tegen de betalen naar gebruik-tarieven.

## <a name="charges-covered-by-reservation"></a>Kosten voor reservering vallen

Service-plannen:

- **Gereserveerde VM-instantie** -een reservering heeft alleen betrekking op de kosten voor rekenuren van de virtuele machine. Het bedekt geen extra kosten voor software, netwerken en opslag.
- **Azure Cosmos DB gereserveerde capaciteit** -een reservering bevat informatie over de doorvoer die is ingericht voor uw resources. Dekt niet kosten voor de opslag en netwerken.
- **SQL Database vCore gereserveerd** - alleen de berekeningskosten opgenomen in een reservering zijn. De licentie wordt apart in rekening gebracht.

Voor virtuele machines van Windows en SQL Database, kunt u de licentiekosten met dekt [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reservation"></a>Wie komt in aanmerking voor een reservering kopen?

Als u wilt een abonnement kopen, moet u een abonnement de eigenaar van de rol in een onderneming (MS-AZR - 0017P of MS-AZR - 0148P) of een abonnement voor betalen per gebruik (MS-AZR - 003P of MS-AZR - 0023P) hebben. Cloud solution providers Azure portal kunnen gebruiken of [Partner Center](/partner-center/azure-reservations) Azure reserveringen kopen.

EA-klanten aankopen kunnen beheerders van de EA kunnen beperken door het uitschakelen van de **gereserveerde instanties toevoegen** optie in de EA-Portal. EA-beheerders moet de eigenaar van een abonnement voor ten minste één EA-abonnement naar een reservering koopt. De optie is handig voor ondernemingen die u wilt dat een centraal team kopen van reserveringen voor andere kostenplaatsen. Na de aankoop, kunnen gecentraliseerd teams kosten center eigenaren toevoegen aan de reserveringen. Eigenaren kunnen vervolgens het bereik van de reservering naar hun abonnementen. Het centrale team hoeft niet te abonnement eigenaar toegang hebben wanneer de reservering wordt aangeschaft.

Een reserveringskorting is alleen van toepassing op resources die zijn gekoppeld aan abonnementen die zijn aangeschaft via het Enterprise-, CSP en afzonderlijke plannen met betalen per gebruik-tarieven.

## <a name="scope-reservations"></a>Bereik reserveringen

U kunt het bereik van een reservering voor een abonnement of resource-groepen. Instellen van het bereik voor een reservering selecteren waar de besparingen reservering toepassen. Wanneer u het bereik van de reservering naar een resourcegroep, reservering kortingen gelden alleen voor de resourcegroep, niet het hele abonnement.

### <a name="reservation-scoping-options"></a>Reservering scope-opties

Met resource hebt groep scoping u drie opties als bereik voor een reservering, afhankelijk van uw behoeften:

- **Enkele resource groepsbereik** : de reserveringskorting is van toepassing op de overeenkomende resources in alleen de geselecteerde resourcegroep.
- **Eén abonnementsbereik** : de reserveringskorting is van toepassing op de overeenkomende resources in het geselecteerde abonnement.
- **Gedeeld bereik** : de reserveringskorting is van toepassing op het zoeken naar resources in die in aanmerking komen abonnementen die zich in de context van de facturering. Voor Enterprise Agreement-klanten is de context van de facturering van de inschrijving. De facturering scope is voor afzonderlijke abonnementen met betalen per gebruik-tarieven, alle in aanmerking komende abonnementen die zijn gemaakt door de accountbeheerder.

Azure verwerkt tijdens het toepassen van de reservering kortingen op uw gebruik, de reservering in de volgende volgorde:

1. Reserveringen die zijn gericht op een resourcegroep
2. Één scope reserveringen
3. Gedeeld bereik reserveringen

Een enkele resourcegroep kunt reservering kortingen ophalen uit meerdere reserveringen, afhankelijk van hoe u het bereik van de reserveringen.

### <a name="scope-a-reservation-to-a-resource-group"></a>Het bereik van een reservering voor een resourcegroep

U kunt het bereik van de reservering naar een resourcegroep wanneer u de reservering kopen, of u het bereik na de aankoop instellen. U moet de eigenaar van een abonnement om het bereik van de reservering naar een resourcegroep te zijn.

Om het bereik, gaat u naar de [reservering kopen](https://ms.portal.azure.com/#blade/Microsoft\_Azure\_Reservations/CreateBlade/referrer/Browse\_AddCommand) pagina in de Azure portal. Selecteer vervolgens het reserveringstype die u wilt kopen. Op de **selecteert u het product die u wilt kopen** selectie formulier wijzigen de **bereik** waarde die moet worden **één resourcegroep** en selecteer een resourcegroep.

![Voorbeeld van VM-reservering kopen selectie](./media/billing-save-compute-costs-reservations/select-product-to-purchase.png)

Aanschafaanbevelingen voor de resourcegroep in de reservering van virtuele machines worden weergegeven. Aanbevelingen worden berekend door de analyse van uw gebruik in de afgelopen 30 dagen. Een aanbeveling aankoop wordt uitgevoerd wanneer de kosten van het uitvoeren van resources met gereserveerde instanties goedkoper dan de kosten is van het uitvoeren van resources met betalen per gebruik-tarieven. Zie voor meer informatie over aanbevelingen voor de reservering kopen, de [aanschafaanbevelingen gereserveerde instantie ophalen op basis van het gebruikspatroon](https://azure.microsoft.com/blog/get-usage-based-reserved-instance-recommendations) blogbericht.

U kunt het bereik altijd bijwerken nadat u een reservering kopen. Om dit te doen, gaat u naar de reservering, klikt u op **configuratie** en rescope van de reservering. Een reservering rescoping is niet een commerciële transactie. De reserveringstermijn van de is niet gewijzigd. Zie voor meer informatie over het bijwerken van het bereik [het bereik bijwerken nadat u een reservering koopt](billing-manage-reserved-vm-instance.md#change-the-reservation-scope).

![Voorbeeld van een reserveringsbereik wijzigen](./media/billing-save-compute-costs-reservations/rescope-reservation-resource-group.png)

### <a name="monitor-and-optimize-reservation-usage"></a>Bewaken en optimaliseren van de reservering gebruik

U kunt het gebruik van uw reservering op meerdere manieren – via Azure portal, via API's of via gebruiksgegevens bewaken. Als u wilt zien welke de reserveringen die u hebt toegang tot, gaat u naar **reserveringen** in Azure portal. Het raster reserveringen toont de laatste opgenomen gebruikspercentage voor de reservering. Klik op de reservering om te zien van de lange termijn gebruik van de reservering.

U kunt ook ophalen voor gebruik met behulp van reservering [API's](billing-reservation-apis.md#see-reservation-usage) en naar uw [gebruiksgegevens](billing-understand-reserved-instance-usage-ea.md#common-cost-and-usage-tasks) als u een enterprise overeenkomst-klant bent.

Als u merkt dat het gebruik van uw resourcegroep binnen het bereik reservering is laag, en vervolgens kunt u het reserveringsbereik enkel abonnement of deze in de context van de facturering delen bijwerken. U kunt ook de reservering splitsen en de resulterende reserveringen toepassen op verschillende resourcegroepen bevinden.

### <a name="other-considerations"></a>Andere overwegingen

Als u geen die overeenkomt met de resources in een resourcegroep, wordt de reservering worden gebruikt. De reservering niet automatisch worden toegepast op een andere resourcegroep of abonnement waarbij er laag gebruik.

Een reserveringsbereik niet automatisch bijgewerkt als u de resourcegroep van één abonnement naar een andere verplaatst. U moet de reservering rescope. Anders wordt de reservering wordt volledig worden gebruikt.

## <a name="discounted-subscription-and-offer-types"></a>Gereduceerde abonnement en biedt deze typen

Reservering-kortingen van toepassing op de volgende in aanmerking komende abonnementen en typen bieden.

- Enterprise agreement (getallen bieden: MS-AZR-0017P of MS-AZR - 0148 P)
- Afzonderlijke abonnementen met betalen per gebruik-tarieven (getallen bieden: MS-AZR-0003P or MS-AZR-0023P)
- CSP-abonnementen

Resources die worden uitgevoerd in een abonnement met andere typen ontvangen niet de reserveringskorting.

## <a name="how-is-a-reservation-billed"></a>Hoe wordt een reservering gefactureerd?

De reservering wordt verrekend met de betalingswijze die is gekoppeld aan het abonnement. Als u een Enterprise-abonnement hebt, worden de reserveringskosten voor wordt afgetrokken van uw monetaire toezeggingsbedrag. Als uw monetaire toezeggingsbedrag dekt de kosten van de reservering niet, u wordt gefactureerd voor de overschrijding. Als u een abonnement op een afzonderlijk abonnement met betalen per gebruik-tarieven hebt, wordt de creditcard die u voor uw account hebt wordt onmiddellijk gefactureerd. Als u wordt gefactureerd per factuur, ziet u de kosten in rekening gebracht op uw volgende factuur.

## <a name="how-reservation-discount-is-applied"></a>Hoe reserveringskorting wordt toegepast

De reserveringskorting is van toepassing op het gebruik van bronnen die overeenkomt met de kenmerken die u selecteert wanneer u de reservering kopen. De kenmerken omvatten het bereik waarin de overeenkomende virtuele machines, SQL databases, Azure Cosmos DB of andere bronnen wordt uitgevoerd. Bijvoorbeeld, als u een reserveringskorting voor vier standaard D2 virtuele machines in de regio VS-West wilt, selecteer het abonnement waarin de virtuele machines worden uitgevoerd.

Een reserveringskorting is '*gebruik-it-of-verliezen-it*'. Als u geen bijbehorende resources voor elk uur, klikt verliest u een reserveringshoeveelheid voor dat uur. U niet uitvoeren voor het doorsturen van niet-gebruikte gereserveerde uur.

Als u een resource wordt afgesloten, past de reserveringskorting automatisch naar een andere overeenkomende resource in het opgegeven bereik. Als er geen overeenkomende bronnen u in het opgegeven bereik vindt, wordt de gereserveerde uren *verloren*.

Bijvoorbeeld, mogelijk u later een resource maken en hebt een overeenkomende reservering die wordt gebruikt. In dit voorbeeld de reserveringskorting automatisch toegepast op de nieuwe overeenkomende resource.

Als de virtuele machines worden uitgevoerd in verschillende abonnementen binnen uw inschrijvingsaccount, selecteert u het bereik als een gedeeld. Gedeeld bereik kunt de reserveringskorting moet worden toegepast voor abonnementen. U kunt het bereik wijzigen nadat u een reservering kopen. Zie voor meer informatie, [Azure-reserveringen beheren](billing-manage-reserved-vm-instance.md).

Een reserveringskorting is alleen van toepassing op resources die zijn gekoppeld aan Enterprise, CSP, of abonnementen met betaalt u Ga-tarieven. Resources die worden uitgevoerd in een abonnement met andere typen ontvangen niet de reserveringskorting.

## <a name="when-the-reservation-term-expires"></a>Wanneer de reserveringstermijn verloopt

De facturering korting verloopt aan het einde van de reserveringstermijn, en de virtuele machine, SQL database, Azure Cosmos DB of andere resource wordt gefactureerd voor de betaalt u Ga-prijs. Azure-reserveringen verlengen niet automatisch. Als u wilt doorgaan met het ophalen van de korting voor facturering, moet u een nieuwe reservering voor in aanmerking komende services en software kopen.

## <a name="discount-applies-to-different-sizes"></a>Korting geldt voor verschillende grootten

Wanneer u een reservering koopt, wordt de korting kunt toepassen op andere exemplaren met kenmerken die zich binnen de groep met dezelfde grootte. Deze functie wordt de grootte-instantieflexibiliteit genoemd. De flexibiliteit van de dekking van de korting is afhankelijk van het type reservering en de kenmerken die u kiest wanneer u de reservering kopen.

Service-plannen:

- Gereserveerde VM-exemplaren: Wanneer u de reservering kopen en selecteer **geoptimaliseerd voor**: **exemplaar grootte flexibiliteit**, de dekking van de korting is afhankelijk van de VM-grootte die u selecteert. De reservering kunt toepassen op de grootten van virtuele machines (VM's) in de groep voor reeks van dezelfde grootte. Zie voor meer informatie, [VM-grootte flexibiliteit in combinatie met gereserveerde VM-instanties](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).
- SQL-Database gereserveerde capaciteit: De dekking van de korting is afhankelijk van de prestatielaag die u kiest. Zie voor meer informatie, [te begrijpen hoe een Azure reserveringskorting wordt toegepast](billing-understand-reservation-charges.md).
- Azure Cosmos DB gereserveerde capaciteit: De dekking van de korting is afhankelijk van de ingerichte doorvoer. Zie voor meer informatie, [te begrijpen hoe een Azure Cosmos DB-reserveringskorting wordt toegepast](billing-understand-cosmosdb-reservation-charges.md).

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over Azure-reserveringen met de volgende artikelen:
    - [Azure-reserveringen beheren](billing-manage-reserved-vm-instance.md)
    - [Informatie over het gebruik van de reservering voor uw abonnement met betalen per gebruik-tarieven](billing-understand-reserved-instance-usage.md)
    - [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](billing-understand-reserved-instance-usage-ea.md)
    - [Kosten van de Windows-software is niet opgenomen in de reserveringen](billing-reserved-instance-windows-software-costs.md)
    - [Azure-reserveringen in programma Partner Center Cloud Solution Provider (CSP)](/partner-center/azure-reservations)

- Meer informatie over reserveringen voor service-plannen:
    - [Virtuele Machines met Azure gereserveerde VM-instanties](../virtual-machines/windows/prepay-reserved-vm-instances.md)
    - [Azure Cosmos DB-resources met Azure Cosmos DB gereserveerde capaciteit](../cosmos-db/cosmos-db-reserved-capacity.md)
    - [SQL-Database compute-resources met Azure SQL Database gereserveerde capaciteit](../sql-database/sql-database-reserved-capacity.md) meer informatie over reserveringen voor software-abonnement:
    - [Red Hat software plannen uit de Azure-reserveringen](../virtual-machines/linux/prepay-rhel-software-charges.md)
    - [Plannen voor SUSE-software uit de Azure-reserveringen](../virtual-machines/linux/prepay-suse-software-charges.md)

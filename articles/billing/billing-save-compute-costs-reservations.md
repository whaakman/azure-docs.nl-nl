---
title: Wat zijn Azure-reserveringen? | Microsoft Docs
description: Meer informatie over Azure-reserveringen en prijzen op te slaan op uw virtuele machines, SQL-databases, Azure Cosmos DB en andere resourcekosten.
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 04/13/2019
ms.author: banders
ms.openlocfilehash: b20983c45cd62b9812cdb52de32a6e29da459efe
ms.sourcegitcommit: c884e2b3746d4d5f0c5c1090e51d2056456a1317
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "60149194"
---
# <a name="what-are-azure-reservations"></a>Wat zijn Azure-reserveringen?

Azure reserveringen kunnen u geld besparen door vooraf betalen voor een eenjarige of driejarige van virtuele machines, SQL-Database compute-capaciteit, doorvoer van Azure Cosmos DB of andere Azure-resources. Vooraf betaalt, kunt u krijg een korting op de resources die u gebruikt. Uw virtuele machine, compute-SQL-database, Azure Cosmos DB kunnen aanzienlijk verkorten door reserveringen of andere bron kosten tot 72% bij betalen per gebruik. Reserveringen bieden een korting van facturering en hebben geen invloed op de runtimestatus van uw resources.

U kunt een reservering kopen de [Azure-portal](https://aka.ms/reservations).

## <a name="why-buy-a-reservation"></a>Waarom zou ik een reservering kopen?

Hebt u virtuele machines, Azure Cosmos DB of SQL-databases die langdurig worden uitgevoerd, biedt een reservering koopt u de meest voordelige optie zijn. Bijvoorbeeld, wanneer u doorlopend vier exemplaren van een service zonder een reservering uitvoert, u bent in rekening gebracht tegen betalen per gebruik-tarieven. Als u een reservering voor deze resources koopt, krijgt u onmiddellijk de reserveringskorting. De resources zijn niet meer in rekening gebracht tegen de betalen naar gebruik-tarieven.

## <a name="charges-covered-by-reservation"></a>Kosten voor reservering vallen

Service-plannen:

- Gereserveerde VM-instantie: Een reservering heeft alleen betrekking op de kosten voor rekenuren van de virtuele machine. Het bedekt geen extra kosten voor software, netwerken en opslag.
- Azure Cosmos DB gereserveerde capaciteit: Een reservering bevat informatie over de doorvoer die is ingericht voor uw resources. Dekt niet kosten voor de opslag en netwerken.
- SQL Database vCore gereserveerd: Alleen de kosten voor de rekenuren zijn opgenomen in een reservering. De licentie wordt apart in rekening gebracht.

Voor virtuele machines van Windows en SQL Database, kunt u de licentiekosten met dekt [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reservation"></a>Wie komt in aanmerking voor een reservering kopen?

Als u wilt een abonnement kopen, moet u een abonnement de eigenaar van de rol in een onderneming (MS-AZR - 0017P of MS-AZR - 0148P) of een abonnement voor betalen per gebruik (MS-AZR - 003P of MS-AZR - 0023P) hebben. Cloud solution providers Azure portal kunnen gebruiken of [Partner Center](/partner-center/azure-reservations) Azure reserveringen kopen.

EA-klanten aankopen kunnen beheerders van de EA kunnen beperken door het uitschakelen van de **gereserveerde instanties toevoegen** optie in de Portal. EA-beheerders moet de eigenaar van een abonnement voor ten minste één EA-abonnement naar een reservering koopt. De optie is handig voor ondernemingen die u wilt dat een centraal team kopen van reserveringen voor andere kostenplaatsen. Na de aankoop, kunnen gecentraliseerd teams kosten center eigenaren toevoegen aan de reserveringen. Eigenaren kunnen vervolgens het bereik van de reservering naar hun abonnementen. Het centrale team hoeft niet te abonnement eigenaar toegang hebben wanneer de reservering wordt aangeschaft.

Een reserveringskorting is alleen van toepassing op resources die zijn gekoppeld aan Enterprise, betalen per gebruik of CSP-abonnementstypen.

## <a name="reservation-scope"></a>Reserveringsbereik

Een reserveringsbereik bepaalt de resources waarop de reserveringskorting van toepassing is. Een reserveringsbereik kunt beschikken over de volgende waarden:

**Gedeeld bereik** -de reserveringskorting wordt toegepast op de overeenkomende resources in een in aanmerking komende abonnementen binnen de context van de facturering.

- Voor Enterprise Agreement-klanten is de context van de facturering van de inschrijving.
 Voor klanten van betalen per gebruik is de facturering scope alle in aanmerking komende abonnementen die zijn gemaakt door de accountbeheerder.

**Enkel abonnement** -de reserveringskorting wordt toegepast op de overeenkomende resources in het geselecteerde abonnement.

U kunt [het bereik bijwerken nadat u een reservering koopt](billing-manage-reserved-vm-instance.md#change-the-reservation-scope).

## <a name="discounted-subscription-and-offer-types"></a>Gereduceerde abonnement en biedt deze typen

Reservering-kortingen van toepassing op de volgende in aanmerking komende abonnementen en typen bieden.

- Enterprise agreement (getallen bieden: MS-AZR-0017P of MS-AZR - 0148 P)
- Betalen per gebruik (getallen bieden: MS-AZR-0003P or MS-AZR-0023P)
- CSP-abonnementen

Resources die worden uitgevoerd in een abonnement met andere typen ontvangen niet de reserveringskorting.

## <a name="how-is-a-reservation-billed"></a>Hoe wordt een reservering gefactureerd?

De reservering wordt verrekend met de betalingswijze die is gekoppeld aan het abonnement. Als u een Enterprise-abonnement hebt, worden de reserveringskosten voor wordt afgetrokken van uw monetaire toezeggingsbedrag. Als uw monetaire toezeggingsbedrag dekt de kosten van de reservering niet, u wordt gefactureerd voor de overschrijding. Als u een betalen per gebruik-abonnement hebt, wordt de creditcard die u voor uw account hebt wordt onmiddellijk gefactureerd. Als u wordt gefactureerd per factuur, ziet u de kosten in rekening gebracht op uw volgende factuur.

## <a name="how-reservation-discount-is-applied"></a>Hoe reserveringskorting wordt toegepast

De reserveringskorting is van toepassing op het gebruik van bronnen die overeenkomt met de kenmerken die u selecteert wanneer u de reservering kopen. De kenmerken omvatten het bereik waarin de overeenkomende virtuele machines, SQL databases, Azure Cosmos DB of andere bronnen wordt uitgevoerd. Bijvoorbeeld, als u een reserveringskorting voor vier standaard D2 virtuele machines in de regio VS-West wilt, selecteer het abonnement waarin de virtuele machines worden uitgevoerd.

Een reserveringskorting is '*gebruik-it-of-verliezen-it*'. Als u geen bijbehorende resources voor elk uur, klikt verliest u een reserveringshoeveelheid voor dat uur. U niet uitvoeren voor het doorsturen van niet-gebruikte gereserveerde uur.

Als u een resource wordt afgesloten, past de reserveringskorting automatisch naar een andere overeenkomende resource in het opgegeven bereik. Als er geen overeenkomende bronnen u in het opgegeven bereik vindt, wordt de gereserveerde uren *verloren*.

Bijvoorbeeld, mogelijk u later een resource maken en hebt een overeenkomende reservering die wordt gebruikt. In dit voorbeeld de reserveringskorting automatisch toegepast op de nieuwe overeenkomende resource.

Als de virtuele machines worden uitgevoerd in verschillende abonnementen binnen uw inschrijvingsaccount, selecteert u het bereik als een gedeeld. Gedeeld bereik kunt de reserveringskorting moet worden toegepast voor abonnementen. U kunt het bereik wijzigen nadat u een reservering kopen. Zie voor meer informatie, [Azure-reserveringen beheren](billing-manage-reserved-vm-instance.md).

Een reserveringskorting is alleen van toepassing op resources die zijn gekoppeld aan Enterprise, betalen per gebruik of CSP-abonnementstypen. Resources die worden uitgevoerd in een abonnement met andere typen ontvangen niet de reserveringskorting.

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
    - [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
    - [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](billing-understand-reserved-instance-usage-ea.md)
    - [Kosten van de Windows-software is niet opgenomen in de reserveringen](billing-reserved-instance-windows-software-costs.md)
    - [Azure-reserveringen in programma Partner Center Cloud Solution Provider (CSP)](/partner-center/azure-reservations)

- Meer informatie over reserveringen voor service-plannen:
    - [Virtuele Machines met Azure gereserveerde VM-instanties](../virtual-machines/windows/prepay-reserved-vm-instances.md)
    - [Azure Cosmos DB-resources met Azure Cosmos DB gereserveerde capaciteit](../cosmos-db/cosmos-db-reserved-capacity.md)
    - [SQL-Database compute-resources met Azure SQL Database gereserveerde capaciteit](../sql-database/sql-database-reserved-capacity.md) meer informatie over reserveringen voor software-abonnement:
    - [Red Hat software plannen uit de Azure-reserveringen](../virtual-machines/linux/prepay-rhel-software-charges.md)
    - [Plannen voor SUSE-software uit de Azure-reserveringen](../virtual-machines/linux/prepay-suse-software-charges.md)

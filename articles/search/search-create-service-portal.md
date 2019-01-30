---
title: Een Azure Search-service maken in de portal - Azure Search
description: Een Azure Search-resource inrichten in de Microsoft Azure-portal-portal. Resourcegroepen, regio's en de SKU of prijscategorie kiezen.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: quickstart
ms.date: 01/17/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 6d71ad9bdc7744898480fb2cc6743e59131ec588
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54423439"
---
# <a name="create-an-azure-search-service-in-the-portal"></a>Een Azure Search-service maken in de portal

Azure Search is een zelfstandige resource die wordt gebruikt voor het toevoegen van een zoekervaring aan aangepaste apps. Hoewel Azure Search eenvoudig kan worden geïntegreerd met veel andere Azure-services, kunt u het ook zelfstandig gebruiken, met apps op netwerkservers of met software die wordt uitgevoerd op andere cloudplatforms. 

Dit artikel bevat meer informatie over hoe u een Azure Search-resource in [Azure Portal](https://portal.azure.com/) kunt maken. 

[ ![GIF-animatie](./media/search-create-service-portal/AnimatedGif-AzureSearch-small.gif) ](./media/search-create-service-portal/AnimatedGif-AzureSearch.gif#lightbox)

Hebt u liever PowerShell? Gebruik de [servicesjabloon](https://azure.microsoft.com/resources/templates/101-azure-search-create/) van Azure Resource Manager. Zie [Azure Search beheren met PowerShell](search-manage-powershell.md) voor hulp om aan de slag te gaan.

## <a name="subscribe-free-or-paid"></a>Abonneren (gratis of betaald)

[Maak een gratis Azure-account](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) en gebruik gratis tegoed om betaalde Azure-services uit te proberen. Nadat u het tegoed hebt opgemaakt, kunt u het account houden en de gratis Azure-services, zoals Websites, blijven gebruiken. Er wordt nooit geld van uw creditcard afgeschreven, tenzij u uw instellingen zelf wijzigt en expliciet aangeeft dat u wilt betalen.

U kunt ook [voordelen voor MSDN-abonnees activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). Als u een MSDN-abonnement hebt, ontvangt u elke maand tegoeden die u voor betaalde Azure-services kunt gebruiken. 

## <a name="find-azure-search"></a>Azure Search vinden
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik op het plusteken (+ Resource maken) in de linkerbovenhoek.
3. Gebruik de zoekbalk om te zoeken naar 'Azure Search' of ga naar de resource via **Web** > **Azure Search**.

![Naar een Azure Search-resource navigeren](./media/search-create-service-portal/find-search3.png "Navigatiepad naar Azure Search")

## <a name="name-the-service-and-url-endpoint"></a>De service en het URL-eindpunt een naam geven

Een servicenaam is onderdeel van het URL-eindpunt waaraan API-aanroepen worden geadresseerd: `https://your-service-name.search.windows.net`. Voer de naam van uw service in het **URL**-veld in.

Als u bijvoorbeeld wilt dat het eindpunt `https://my-app-name-01.search.windows.net` wordt, voert u `my-app-name-01` in.

Vereisten voor servicenaam:
   * De naam moet uniek zijn binnen de naamruimte search.windows.net
   * tussen de 2 en 60 tekens lang zijn
   * Gebruik kleine letters, cijfers of streepjes (-)
   * Vermijd streepjes (-) bij de eerste 2 tekens of als het allerlaatste teken
   * Zorg ervoor dat er nergens twee opeenvolgende streepjes (--) staan

## <a name="select-a-subscription"></a>Een abonnement selecteren
Als u meer dan één abonnement hebt, moet u er één kiezen dat ook services voor gegevensopslag of File Storage-services biedt. Azure Search kan automatisch Azure Table- en Blob-opslag, SQL-Database en Azure Cosmos DB detecteren voor indexering via [*indexeerfuncties*](search-indexer-overview.md), maar alleen voor services in hetzelfde abonnement.

## <a name="select-a-resource-group"></a>Een resourcegroep selecteren
Een resourcegroep is een verzameling Azure-services en -resources die samen worden gebruikt. Als u bijvoorbeeld Azure Search gebruikt om een SQL-database te indexeren, dan moeten beide services deel uitmaken van dezelfde resourcegroep.

Als u resources niet in één groep combineert of als bestaande resourcegroepen resources bevatten die worden gebruikt in oplossingen waartussen geen verband bestaat, maakt u een nieuwe resourcegroep voor uw Azure Search-resource.

> [!TIP]
> Als u een resourcegroep verwijdert, worden de services binnen die resourcegroep ook verwijderd. Bij prototypeprojecten die gebruikmaken van meerdere services, wordt het opschonen na afloop van het project gemakkelijker als die services zich allemaal in dezelfde resourcegroep bevinden. 

## <a name="select-a-hosting-location"></a>Een hostinglocatie selecteren 
Azure Search kan als Azure-service worden gehost in datacenters over de hele wereld. [Prijzen kunnen verschillen](https://azure.microsoft.com/pricing/details/search/) per regio.

Als u van plan bent Cognitive Search te gebruiken, kiest u een [regio waarin deze functie beschikbaar is](cognitive-search-quickstart-blob.md#supported-regions).

## <a name="select-a-pricing-tier-sku"></a>Een prijscategorie selecteren (SKU)
[Azure Search wordt momenteel aangeboden in verschillende prijscategorieën](https://azure.microsoft.com/pricing/details/search/): Gratis, Basic of Standard. Elke categorie heeft eigen [capaciteiten en limieten](search-limits-quotas-capacity.md). Raadpleeg [Choose a pricing tier or SKU](search-sku-tier.md) (Een prijscategorie of SKU kiezen) voor hulp.

Standard wordt meestal gekozen voor productieworkloads, maar de meeste klanten beginnen met de gratis service.

Een prijscategorie kan niet worden gewijzigd nadat de service is gemaakt. Als u een hogere of lagere categorie nodig hebt, moet u de service opnieuw maken.

## <a name="create-your-service"></a>Uw service maken

Vergeet niet om uw service aan het dashboard vast te maken voor eenvoudige toegang wanneer u zich aanmeldt.

![Vastmaken aan het dashboard](./media/search-create-service-portal/new-service3.png "De resource vastmaken aan uw dashboard voor eenvoudige toegang")

## <a name="get-a-key-and-url-endpoint"></a>Een sleutel en het URL-eindpunt ophalen

Op enkele uitzonderingen na vereist uw nieuwe service dat u het URL-eindpunt en een API-sleutel voor autorisatie opgeeft. Quickstarts, zelfstudies, zoals [REST API's voor Azure Search verkennen (Postman)](search-fiddler.md) en [Weten hoe u Azure Search gebruikt via een .NET-toepassing](search-howto-dotnet-sdk.md), voorbeelden en aangepaste code vereisen alle een eindpunt en sleutel die moeten worden uitgevoerd in uw specifieke resource.

1. Zoek en kopieer op de overzichtspagina van de service het URL-eindpunt aan de linkerkant van de pagina. 

   ![Overzichtspagina met URL-eindpunt](./media/search-create-service-portal/url-endpoint.png "URL-eindpunt en andere details van de service")

2. Selecteer **Sleutels** in het navigatiedeelvenster links en kopieer een van de beheersleutels (deze zijn equivalent). API-beheersleutels voor beheerders zijn vereist voor het maken, bijwerken en verwijderen van objecten in uw service.

   ![Pagina met sleutels waarop primaire en secundaire sleutels worden weergegeven](./media/search-create-service-portal/admin-api-keys.png "API-sleutels voor beheerders voor autorisatie")

Een eindpunt en sleutel zijn niet nodig voor op de portal gebaseerde taken. De portal is al gekoppeld aan uw Azure Search-resource met beheerdersrechten. Begin voor een zelfstudie voor de portal met [Zelfstudie: Importeren, indexeren en query's uitvoeren in Azure Search](search-get-started-portal.md).

## <a name="scale-your-service"></a>Uw service schalen
Het kan een paar minuten duren om een service te maken (15 minuten of meer, afhankelijk van de categorie). Nadat uw service is geconfigureerd, kunt u deze schalen, zodat de service aan uw behoeften voldoet. Aangezien u voor uw Azure Search-service hebt gekozen voor de categorie Standard, kunt u uw service in twee dimensies schalen: replica's en partities. Als u had gekozen voor de categorie Basic, had u alleen replica's kunnen toevoegen. Als u had gekozen voor de gratis service, had u niet kunnen schalen.

***Partities*** zorgen ervoor dat uw service meer documenten bewaart en meer documenten doorzoekt.

***Replica's*** zorgen ervoor dat uw services meer zoekquery's kunnen verwerken.

Als u resources toevoegt, wordt uw maandfactuur hoger. Met de [prijscalculator](https://azure.microsoft.com/pricing/calculator/) krijgt u inzicht in het effect van het toevoegen van resources op uw facturering. Onthoud dat u resources kunt aanpassen aan de benodigde capaciteit. Zo kunt u eerst resources toevoegen voor het maken van een volledige eerste index en later het aantal resources weer verlagen tot een niveau dat geschikt is voor incrementele indexering.

> [!Important]
> Een service moet [twee replica's hebben voor een alleen-lezen-SLA en drie replica's voor een lezen/schrijven-SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

1. Ga naar de pagina van uw zoekservice in Azure Portal.
2. Selecteer in het navigatiedeelvenster aan de linkerkant **Instellingen** > **Schalen**.
3. Gebruik de schuifbalk om resources van een bepaald type toe te voegen.

![Capaciteit toevoegen](./media/search-create-service-portal/settings-scale.png "Capaciteit toevoegen via replica's en partities")

> [!Note] 
> Elke categorie heeft verschillende [limieten](search-limits-quotas-capacity.md) op het aantal zoekeenheden dat is toegestaan in een enkele service (replica's * partities = totaalaantal zoekeenheden).

## <a name="when-to-add-a-second-service"></a>Wanneer u een tweede service moet toevoegen

De meeste klanten gebruiken maar één service die geconfigureerd is voor de categorie die de [juiste balans voor resources](search-sku-tier.md) biedt. Eén service kan meerdere indexen hosten, afhankelijk van de [maximale limieten van de door u geselecteerde categorie](search-capacity-planning.md), en alle indexen zijn van elkaar geïsoleerd. In Azure Search kunnen aanvragen alleen worden gericht aan één index, waardoor de kans dat er per ongeluk of opzettelijk gegevens worden opgehaald uit andere indexen in dezelfde service, wordt geminimaliseerd.

Hoewel de meeste klanten slechts één service gebruiken, kan serviceredundantie nodig zijn als de volgende operationele vereisten gelden:

+ Herstel na noodgevallen (onderbreking datacentrum). Azure Search biedt geen onmiddellijke failover in geval van een storing. Raadpleeg voor aanbevelingen en richtlijnen [Servicebeheer](search-manage.md).
+ Uit uw onderzoek naar multitenancymodellering blijkt dat een ontwerp met meerdere services optimaal is. Raadpleeg voor meer informatie [Ontwerp voor multitenancy](search-modeling-multitenant-saas-applications.md).
+ Voor globaal geïmplementeerde toepassingen kan het nodig zijn dat u een exemplaar van Azure Search in meerdere regio's plaatst om de latentie van het internationale verkeer in uw toepassing te minimaliseren.

> [!NOTE]
> In Azure Search kunt u indexering en querywerkbelastingen niet scheiden; u kunt dus nooit meerdere services voor gescheiden werkbelastingen maken. Een query wordt altijd uitgevoerd op de service waarin de index is gemaakt (u kunt niet een index in een service maken en vervolgens naar een andere service kopiëren).
>

Een tweede service is niet vereist voor hoge beschikbaarheid. Hoge beschikbaarheid voor query's wordt bereikt wanneer u twee of meer replica's in dezelfde service gebruikt. Replica-updates zijn sequentieel, wat betekent dat er ten minste één operationeel is wanneer er een service-update wordt geïmplementeerd. Voor meer informatie over uptime raadpleegt u [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

## <a name="next-steps"></a>Volgende stappen
Nadat u een Azure Search-service hebt ingericht, kunt u in de portal verdergaan met het maken van uw eerste index.

> [!div class="nextstepaction"]
> [Zelfstudie: Gegevens en index importeren, en query's uitvoeren in de portal](search-get-started-portal.md)

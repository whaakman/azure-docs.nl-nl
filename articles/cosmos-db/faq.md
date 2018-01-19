---
title: Veelgestelde vragen over Azure Cosmos DB | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over Azure Cosmos DB, een globaal gedistribueerd en modellen database-service. Meer informatie over de capaciteit, prestatieniveaus en schalen.
keywords: Databasevragen, veelgestelde vragen, documentdb, azure, Microsoft azure
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: b68d1831-35f9-443d-a0ac-dad0c89f245b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2018
ms.author: mimig
ms.openlocfilehash: 0f7998ca4000a4ccfd77b173cb3dd9756b4777ae
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-cosmos-db-faq"></a>Azure Cosmos DB FAQ
## <a name="azure-cosmos-db-fundamentals"></a>Grondbeginselen van Azure DB Cosmos
### <a name="what-is-azure-cosmos-db"></a>Wat is Azure Cosmos DB?
Azure Cosmos-database is een wereldwijd gerepliceerde en modellen database-service waarmee die biedt uitgebreide query over gegevens zonder schema configureerbare en betrouwbare prestaties leveren en maakt snelle ontwikkeling. Dit wordt bereikt door middel van een beheerd platform dat wordt ondersteund door de kracht en het bereik van Microsoft Azure. 

Azure Cosmos-database is de juiste oplossing voor web-, gaming- en IoT-toepassingen wanneer voorspelbare doorvoer, hoge beschikbaarheid, lage latentie en een gegevensmodel zonder schema belangrijke vereisten zijn. Levert schemaflexibiliteit en geavanceerde indexeren en transactionele ondersteuning voor meerdere documenten met geïntegreerde JavaScript bevat. 

Zie voor meer databasevragen, antwoorden en instructies voor het implementeren en gebruiken van deze service, de [documentatiepagina voor Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/).

### <a name="what-happened-to-the-documentdb-api"></a>Wat is er gebeurd met de API DocumentDB?

De DocumentDB-API van Azure Cosmos-database of SQL (DocumentDB) API is nu bekend als Azure Cosmos DB SQL-API. U hoeft niet te wijzigen wat als u wilt doorgaan met het uitvoeren van uw apps gebouwd met DocumentDB-API. De functionaliteit blijft hetzelfde.

Als u een DocumentDB-API-account hebt, hebt u nu een SQL-API-account en uw facturering niet gewijzigd. 

### <a name="what-happened-to-azure-documentdb-as-a-service"></a>Wat is er gebeurd met Azure DocumentDB als een service?

De Azure DocumentDB-service maakt nu deel uit van de service Azure Cosmos DB en doet zich in de vorm van de SQL-API. Toepassingen die zijn gebouwd met Azure DocumentDB wordt waartegen Azure Cosmos DB SQL-API uitgevoerd zonder deze te wijzigen. Daarnaast ondersteunt Azure Cosmos DB de Graph API, tabel-API, MongoDB-API en Cassandra API (Preview).

### <a name="what-are-the-typical-use-cases-for-azure-cosmos-db"></a>Wat zijn typische gebruiksvoorbeelden voor Azure Cosmos DB?
Azure Cosmos-database is een goede keuze voor nieuwe web, mobiel, games, en IoT-toepassingen waarbij automatisch schalen, voorspelbare prestaties, volgorde van milliseconde responstijden en de mogelijkheid te query snel gegevens zonder schema is belangrijk. Azure Cosmos DB gepaard met snelle ontwikkeling en de continue herhaling van toepassingsgegevensmodellen ondersteunen. Toepassingen die door gebruikers gegenereerde inhoud en gegevens beheren zijn [algemene gebruiksvoorbeelden voor Azure Cosmos DB](use-cases.md). 

### <a name="how-does-azure-cosmos-db-offer-predictable-performance"></a>Hoe biedt Azure Cosmos DB voorspelbare prestaties?
Een [aanvraag eenheid](request-units.md) (RU) is de mate van doorvoer in Azure Cosmos DB. Een doorvoer 1 RU komt overeen met de doorvoer van een document van 1 KB op te HALEN. Elke bewerking in Azure Cosmos DB, met inbegrip van leesbewerkingen, schrijfbewerkingen, SQL-query's en opgeslagen procedure-uitvoeringen, heeft een deterministische RU-waarde die gebaseerd op de doorvoer die is vereist om de bewerking te voltooien. In plaats van het denken over CPU, IO, en geheugen en hoe deze elke doorvoer van uw toepassing beïnvloeden, kunt u denken in termen van een enkele RU-meting.

U kunt elke Azure DB die Cosmos-container met ingerichte doorvoer in termen van RUs aan doorvoer per seconde reserveren. Voor toepassingen van elke schaal, kunt u afzonderlijke aanvragen voor het meten van de waarden ervan RU benchmark en inrichten van een container voor het afhandelen van het totale aantal aanvraageenheden voor alle aanvragen. U kunt ook opschalen of terugschroeven doorvoer van uw container naarmate de behoeften van uw toepassing veranderen. Voor meer informatie over aanvraageenheden en hulp bij het bepalen van de container moet, Zie [schatten doorvoerbehoeften](request-units.md#estimating-throughput-needs) en probeer de [doorvoer Rekenmachine](https://www.documentdb.com/capacityplanner). De term *container* verwijst hier naar verwijst naar een verzameling van de SQL-API, Graph API grafiek, MongoDB-API-verzameling en tabel API-tabel. 

### <a name="how-does-azure-cosmos-db-support-various-data-models-such-as-keyvalue-columnar-document-and-graph"></a>Hoe ondersteunt Azure Cosmos DB verschillende gegevensmodellen zoals sleutel/waarde, kolommen, document en grafiek?

Sleutel/waarde van (tabel), kolommen, documenteren en grafiekgegevens modellen zijn alle standaard ondersteund vanwege de ARS (atomen, registreert en reeksen) die Cosmos Azure DB ontwerp is gebaseerd op. Atomen, records en reeksen kunnen eenvoudig worden toegewezen en toegewezen aan verschillende gegevensmodellen. De API's voor een subset van modellen beschikbaar rechts nu (SQL, MongoDB, tabel en Graph API's) en anderen die specifiek zijn voor modellen van aanvullende gegevens in de toekomst beschikbaar zullen zijn.

Azure Cosmos-database heeft een schema agnostisch indexing-engine geschikt voor automatisch indexeren van alle gegevens die deze opgenomen zonder dat een schema of secundaire indexen van de ontwikkelaar. De engine wordt gebruikgemaakt van een set van logische index-indelingen (omgekeerde, kolommen, structuur) die in geval van de opslagindeling van de index en queryverwerking subsystemen ontkoppelt. Cosmos DB heeft ook de mogelijkheid te ondersteunen van een set API's en kabel protocollen op een uitbreidbare manier en efficiënt te vertalen naar het gegevensmodel core (1) en de logische index-indelingen (2) zodat u een unieke kunnen meerdere gegevensmodellen systeemeigen ondersteunen.

### <a name="is-azure-cosmos-db-hipaa-compliant"></a>Is Azure Cosmos DB HIPAA compatibel?
Ja, Azure Cosmos DB HIPAA-compatibel is. HIPAA stelt vereisten vast voor het gebruik, de openbaarmaking en de beveiliging van individueel identificeerbare gezondheidsinformatie. Zie [Microsoft Trust Center](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA) voor meer informatie.

### <a name="what-are-the-storage-limits-of-azure-cosmos-db"></a>Wat zijn de opslaglimieten van Azure DB die Cosmos?
Er is geen limiet voor de totale hoeveelheid gegevens die een container kunt opslaan in Azure Cosmos DB.

### <a name="what-are-the-throughput-limits-of-azure-cosmos-db"></a>Wat zijn de doorvoerlimieten van Azure DB die Cosmos?
Er is geen limiet voor de totale hoeveelheid doorvoer die een container in Azure Cosmos DB kan ondersteunen. Het belangrijkste idee is dat uw werkbelasting verdelen ongeveer een voldoende aantal partitiesleutels.

### <a name="how-much-does-azure-cosmos-db-cost"></a>Wat kost Azure Cosmos DB?
Raadpleeg voor meer informatie, de [Azure Cosmos DB prijsinformatie](https://azure.microsoft.com/pricing/details/cosmos-db/) pagina. Azure DB Cosmos gebruikskosten worden bepaald door het aantal ingerichte containers, het aantal uren de containers online zijn en de ingerichte doorvoer voor elke container. De term *containers* verwijst hier naar de verzameling van de SQL-API, Graph API grafiek, MongoDB-API-verzameling en tabel API tabellen. 

### <a name="is-a-free-account-available"></a>Is een gratis account beschikbaar?
Ja, kunt u aanmelden voor een account met beperkte gratis met geen toezegging. Als u wilt aanmelden, gaat u naar [probeert Azure Cosmos DB gratis](https://azure.microsoft.com/try/cosmosdb/) of meer informatie in de [probeer Cosmos DB Veelgestelde vragen over Azure](#try-cosmos-db).

Als u niet bekend met Azure bent, kunt u zich aanmelden voor een [gratis Azure-account](https://azure.microsoft.com/free/), u hebt dan 30 dagen en en een creditcard om te proberen alle Azure-services. Als u een Visual Studio-abonnement hebt, bent u ook in aanmerking komen voor [gratis Azure-tegoed](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) moet worden gebruikt voor een Azure-service. 

U kunt ook de [Azure Cosmos DB Emulator](local-emulator.md) te ontwikkelen en testen van de toepassing lokaal voor gratis, zonder te maken van een Azure-abonnement. Wanneer u tevreden bent over hoe uw toepassing in de Azure-Emulator Cosmos DB werkt, kunt u overschakelen naar het met een Azure DB die Cosmos-account in de cloud.

### <a name="how-can-i-get-additional-help-with-azure-cosmos-db"></a>Hoe kan ik aanvullende ondersteuning voor Azure Cosmos DB krijgen?

Als u een technische vraag vragen, kunt u posten naar een van deze twee vraag en antwoord forums:
* [MSDN-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB)
* [Stack-overloop](http://stackoverflow.com/questions/tagged/azure-cosmosdb). Stack-overloop wordt aanbevolen voor het programmeren van vragen. Controleer of uw vraag is [op onderwerp](https://stackoverflow.com/help/on-topic) en [vindt u informatie van zo veel mogelijk, waardoor de vraag duidelijke en kan worden beantwoord](https://stackoverflow.com/help/how-to-ask). 

Om aan te vragen van nieuwe functies, kunt u een nieuwe aanvraag maken op [Uservoice](https://feedback.azure.com/forums/263030-azure-cosmos-db).

U herstelt een probleem met uw account door het bestand een [ondersteuningsaanvraag](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) in de Azure portal.

Andere vragen kunnen worden verstuurd naar het team via [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com); maar dit niet een alias voor technische ondersteuning is. 

<a id="try-cosmos-db"></a>
## <a name="try-azure-cosmos-db-subscriptions"></a>Probeer Azure DB die Cosmos-abonnementen

U kunt nu profiteren van een tijdelijke Azure DB die Cosmos-ervaring zonder een abonnement, gratis en verplichtingen. Als u wilt aanmelden voor een abonnement probeert Azure Cosmos DB, gaat u naar [gratis Azure Cosmos DB probeer](https://azure.microsoft.com/try/cosmosdb/). Dit abonnement is gescheiden van de [gratis proefversie van Azure](https://azure.microsoft.com/free/), en kan worden gebruikt naast een gratis proefversie van Azure of een Azure betaald abonnement. 

Probeer Azure Cosmos DB abonnementen weergegeven in de Azure portal naast andere abonnementen die zijn gekoppeld aan uw gebruikers-ID. 

De volgende voorwaarden van toepassing op Azure Cosmos-DB probeer abonnementen:

* Een container per abonnement voor SQL, Gremlin (Graph API) en tabel accounts.
* Maximaal 3 verzamelingen per abonnement voor MongoDB-accounts.
* 10 GB opslagcapaciteit.
* Globale replicatie is beschikbaar in de volgende [Azure-regio's](https://azure.microsoft.com/regions/): Centraal VS, Noord-Europa en Azië en Stille Oceaan Zuidoost
* Maximale doorvoer van 5 K RU/s.
* Abonnementen vervalt na 24 uur, en kunnen worden uitgebreid tot maximaal 48 uur totale.
* Ondersteuning van Azure-tickets kunnen niet worden gemaakt voor accounts probeert Azure Cosmos DB; ondersteuning is echter beschikbaar voor abonnees met bestaande ondersteuningsplannen. 

## <a name="set-up-azure-cosmos-db"></a>Instellen van Azure DB die Cosmos
### <a name="how-do-i-sign-up-for-azure-cosmos-db"></a>Hoe meld ik me voor Azure Cosmos DB?
Azure Cosmos DB is beschikbaar in de Azure-portal. Eerst zich registreren voor een Azure-abonnement. Nadat u zich hebt aangemeld, kunt u een SQL-API, Graph API, tabel-API, MongoDB-API of Cassandra API-account toevoegen aan uw Azure-abonnement.

### <a name="what-is-a-master-key"></a>Wat is een hoofdsleutel?
Een hoofdsleutel is een beveiligingstoken voor toegang tot alle resources voor een account. Personen met de sleutel hebt gelezen en schrijftoegang tot alle resources in het account van de database. Wees voorzichtig met het distribueren van hoofdsleutels. De hoofdsleutel van de primaire en secundaire hoofdsleutel zijn beschikbaar op de **sleutels** blade van de [Azure-portal][azure-portal]. Zie voor meer informatie over sleutels [weergeven, kopiëren en opnieuw genereren toegangstoetsen](manage-account.md#keys).

### <a name="what-are-the-regions-that-preferredlocations-can-be-set-to"></a>Wat zijn de gebieden die PreferredLocations kan worden ingesteld op? 
De waarde PreferredLocations kan worden ingesteld met een van de Azure-regio's waarin Cosmos DB beschikbaar is. Zie voor een lijst met beschikbare regio's, [Azure-regio's](https://azure.microsoft.com/regions/).

### <a name="is-there-anything-i-should-be-aware-of-when-distributing-data-across-the-world-via-the-azure-datacenters"></a>Is er iets die ik houden moet rekening bij het distribueren van gegevens over de hele wereld via de Azure-datacenters? 
Azure Cosmos DB aanwezig is in alle Azure-regio's, zoals opgegeven op de [Azure-regio's](https://azure.microsoft.com/regions/) pagina. Omdat het de Kernservice, heeft elke nieuwe datacenter de aanwezigheid van een Azure Cosmos DB. 

Vergeet niet dat Azure Cosmos DB soevereine en government clouds respecteert bij het instellen van een regio. Dat wil zeggen, als u een account in een soevereine regio maken, niet te repliceren buiten die soevereine regio. U kunt replicatie naar andere locaties soevereine van een externe account op deze manier niet inschakelen. 

## <a name="develop-against-the-sql-api"></a>Op basis van de SQL-API ontwikkelen

### <a name="how-do-i-start-developing-against-the-sql-api"></a>Hoe start ontwikkelen van toepassingen met de SQL-API?
Eerst moet u aanmelden voor een Azure-abonnement. Zodra u zich voor een Azure-abonnement aanmelden, kunt u een SQL-API-container toevoegen aan uw Azure-abonnement. Zie voor instructies over het toevoegen van een account voor Azure Cosmos DB [Azure DB die Cosmos-databaseaccount maken](create-sql-api-dotnet.md#create-account). 

[SDK's](sql-api-sdk-dotnet.md) zijn beschikbaar voor .NET, Python, Node.js, JavaScript en Java. Ontwikkelaars kunnen ook gebruikmaken van de [RESTful HTTP API's](/rest/api/documentdb/) om te communiceren met Azure Cosmos DB resources uit verschillende platforms en talen.

### <a name="can-i-access-some-ready-made-samples-to-get-a-head-start"></a>Ik toegang tot de vooraf gedefinieerde voorbeelden voor een goed uitgangspunt?
Voorbeelden voor de SQL-API [.NET](sql-api-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](sql-api-nodejs-samples.md), en [Python](sql-api-python-samples.md) SDK's zijn beschikbaar op GitHub.


### <a name="does-the-sql-api-database-support-schema-free-data"></a>De database van SQL-API biedt ondersteuning voor gegevens zonder schema?
Ja, de SQL-API kan voor toepassingen om op te slaan zonder schemadefinities of hints willekeurige JSON-documenten. Gegevens zijn onmiddellijk beschikbaar voor de query via de interface Azure Cosmos DB SQL-query.  

### <a name="does-the-sql-api-support-acid-transactions"></a>De SQL-API biedt ondersteuning voor ACID-transactions?
Ja, de SQL-API biedt ondersteuning voor transacties van tussen meerder documenten uitgedrukt in JavaScript opgeslagen procedures en triggers. Transacties worden binnen het bereik van één partitie binnen elke verzameling en uitgevoerd met ACID-semantiek als 'Alles of niets,' geïsoleerd van andere code en gebruikersaanvragen aanvragen voor het gelijktijdig worden uitgevoerd. Als er uitzonderingen worden veroorzaakt door de server-side '-uitvoering van JavaScript-toepassingscode, de hele transactie teruggedraaid. Zie voor meer informatie over transacties [Database programmatransacties](programming.md#database-program-transactions).

### <a name="what-is-a-collection"></a>Wat is een verzameling?
Een verzameling is een groep van documenten en hun bijbehorende JavaScript-toepassingslogica. Een verzameling is een factureerbare entiteit waar de [kosten](performance-levels.md) wordt bepaald door de doorvoer en opslag gebruikt. Verzamelingen kunnen een of meer partities of servers omvatten en kunnen worden geschaald voor vrijwel onbeperkte hoeveelheid opslag of doorvoer.

Verzamelingen zijn ook de factureringsentiteiten voor Azure Cosmos DB. Elke verzameling wordt per uur gefactureerd op basis van de ingerichte doorvoer en opslagruimte gebruikt. Zie voor meer informatie [prijzen van Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/). 

### <a name="how-do-i-create-a-database"></a>Hoe maak ik een database?
U kunt databases maken met behulp van de [Azure-portal](https://portal.azure.com), zoals beschreven in [een verzameling toevoegen](create-sql-api-dotnet.md#create-collection), wordt er één van de [Azure Cosmos DB SDK's](sql-api-sdk-dotnet.md), of de [REST-API's](/rest/api/documentdb/). 

### <a name="how-do-i-set-up-users-and-permissions"></a>Hoe stel ik gebruikers en machtigingen in?
U kunt gebruikers en machtigingen maken met een van de [Cosmos DB API SDK's](sql-api-sdk-dotnet.md) of de [REST-API's](/rest/api/documentdb/).  

### <a name="does-the-sql-api-support-sql"></a>De SQL-API biedt ondersteuning voor SQL?
De SQL-querytaal ondersteund door SQL-API-accounts is een uitgebreide subset van de queryfunctionaliteit die wordt ondersteund door SQL Server. De Azure Cosmos DB SQL-querytaal biedt geavanceerde hiërarchische en relationele operators en uitbreidingsmogelijkheden via op basis van JavaScript, door gebruiker gedefinieerde functies (UDF's). JSON-grammatica biedt de mogelijkheid voor het JSON-documenten te modelleren als structuren met gelabelde knooppunten, die worden gebruikt door zowel de Azure DB die Cosmos automatische indexering technieken en de SQL-querydialect van Azure Cosmos-database. Zie voor meer informatie over het gebruik van SQL-grammatica de [SQL-Query] [ query] artikel.

### <a name="does-the-sql-api-support-sql-aggregation-functions"></a>De SQL-API biedt ondersteuning voor SQL-aggregatiefuncties?
De SQL-API ondersteunt lage latentie aggregatie op elke schaal via statistische functies `COUNT`, `MIN`, `MAX`, `AVG`, en `SUM` via de SQL-grammatica. Zie voor meer informatie [statistische functies](sql-api-sql-query.md#Aggregates).

### <a name="how-does-the-sql-api-provide-concurrency"></a>Hoe biedt de SQL-API gelijktijdigheid?
De SQL-API biedt ondersteuning voor Optimistisch gelijktijdigheidbeheer (OCC) via HTTP-entity-tags of ETags. Elke SQL-API-resource heeft een ETag en de ETag is ingesteld op de server telkens wanneer een document wordt bijgewerkt. De ETag-header en de huidige waarde worden opgenomen in alle antwoordberichten. ETags kan worden gebruikt met de header If-Match waardoor de server om te bepalen of een resource moet worden bijgewerkt. De If-Match-waarde is de ETag-waarde gecontroleerd. Als de ETag-waarde overeenkomt met de ETag-waarde van de server, wordt de bron bijgewerkt. Als de ETag niet meer actueel is, de server weigert het opnieuw met een ' HTTP 412 Precondition failure ' antwoordcode. De client haalt vervolgens opnieuw de resource te verkrijgen van de huidige ETag-waarde voor de resource. ETags kan bovendien met de header If-None-Match worden gebruikt om te bepalen of een opnieuw ophalen van een bron die nodig is.

Optimistische gelijktijdigheid in .NET, gebruikt de [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) klasse. Zie voor een voorbeeld .NET [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) in de steekproef DocumentManagement op GitHub.

### <a name="how-do-i-perform-transactions-in-the-sql-api"></a>Hoe voer ik transacties uit in de SQL-API?
De SQL-API biedt ondersteuning voor taalgeïntegreerde transacties via in JavaScript opgeslagen procedures en triggers. Alle databasebewerkingen in scripts worden uitgevoerd onder snapshot-isolatie. Als dit een verzameling van één partitie is, wordt de uitvoering is afgestemd op de verzameling. Als de verzameling is gepartitioneerd, wordt de uitvoering is afgestemd op documenten met dezelfde partitiesleutel waarde binnen de verzameling. Er wordt aan het begin van de transactie een schermopname van de documentversies (ETags) gemaakt en de transactie wordt alleen uitgevoerd als het script kan worden uitgevoerd. Als JavaScript een fout genereert, wordt de transactie teruggedraaid. Zie voor meer informatie [serverzijde JavaScript programmeren voor Azure Cosmos DB](programming.md).

### <a name="how-can-i-bulk-insert-documents-into-cosmos-db"></a>Hoe kan ik bulksgewijs invoegen documenten naar Cosmos DB?
U kunt bulksgewijs invoegen documenten naar Azure Cosmos DB op twee manieren:

* Het hulpprogramma voor migratie, zoals beschreven in [hulpprogramma voor migratie van Database voor Azure Cosmos DB](import-data.md).
* Opgeslagen procedures, zoals beschreven in [serverzijde JavaScript programmeren voor Azure Cosmos DB](programming.md).

### <a name="does-the-sql-api-support-resource-link-caching"></a>Ondersteunt de SQL-API ondersteuning resourcekoppelingen?
Ja, omdat Azure Cosmos DB een RESTful-service, resourcekoppelingen onveranderbaar zijn en kunnen in de cache worden opgeslagen. SQL-API-clients kunnen een header 'If-None-Match' opgeven voor leesbewerkingen voor een resource-achtige document of een verzameling en vervolgens hun lokale kopieën werken nadat de serverversie is gewijzigd.

### <a name="is-a-local-instance-of-sql-api-available"></a>Is een lokaal exemplaar van SQL-API beschikbaar?
Ja. De [Azure Cosmos DB Emulator](local-emulator.md) biedt een hoogwaardige emulatie van de Cosmos-DB-service. Het ondersteunt functionaliteit die identiek is aan Azure Cosmos DB, inclusief ondersteuning voor het maken en uitvoeren van query's JSON-documenten, inrichting en schalen van verzamelingen en uitvoering van opgeslagen procedures en triggers. U kunt ontwikkelen en testen van toepassingen met behulp van de Azure-Emulator Cosmos DB en deze implementeren in Azure op een wereldwijde schaal door het maken van een configuratie voor één eindpunt van de verbinding voor Azure Cosmos DB wijzigen.

## <a name="develop-against-the-api-for-mongodb"></a>Ontwikkelen met de API voor MongoDB
### <a name="what-is-the-azure-cosmos-db-api-for-mongodb"></a>Wat is de Azure DB-API voor MongoDB Cosmos?
De Azure DB-API voor MongoDB Cosmos is een compatibiliteitslaag waarmee toepassingen kunnen eenvoudig en transparant communiceren met de systeemeigen database-engine van Azure DB die Cosmos via bestaande, community ondersteund Apache MongoDB APIs en stuurprogramma's. Ontwikkelaars kunnen nu bestaande MongoDB hulpprogramma ketens en vaardigheden gebruiken om toepassingen die van Azure DB die Cosmos gebruikmaken te bouwen. Ontwikkelaars profiteren van de unieke mogelijkheden van Azure Cosmos DB, waaronder onderhoud voor automatisch indexeren, back-up en back-financieel service level agreements (Sla).

### <a name="how-do-i-connect-to-my-api-for-mongodb-database"></a>Hoe kan ik mijn API voor MongoDB-database verbinden?
De snelste manier verbinding maken met de Azure Cosmos DB-API voor MongoDB naar head via wordt de [Azure-portal](https://portal.azure.com). Ga naar uw account en klik vervolgens op het menu linkernavigatievenster **Quick Start**. Snel starten, is de beste manier om op te halen codefragmenten verbinding maken met uw database. 

Azure Cosmos DB zorgt er strenge beveiligingsvereisten en standaarden. Azure DB Cosmos-accounts verificatie en veilige communicatie via SSL vereisen, dus zorg ervoor dat u TLSv1.2.

Zie voor meer informatie [verbinding maken met uw API voor MongoDB-database](connect-mongodb-account.md).

### <a name="are-there-additional-error-codes-for-an-api-for-mongodb-database"></a>Zijn er extra foutcodes voor een API voor MongoDB-database?
Naast de algemene foutcodes voor MongoDB heeft de MongoDB-API een eigen specifieke foutcodes:


| Fout               | Code  | Beschrijving  | Oplossing  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | Het totale aantal aanvraageenheden verbruikt is groter dan de frequentie van ingerichte aanvraag-eenheid voor de verzameling en is beperkt. | Overweeg het schalen van de doorvoer van de verzameling van de Azure-portal of u opnieuw probeert te. |
| ExceededMemoryLimit | 16501 | Als een multitenant-service heeft de bewerking van de client geheugen aandeel overschreden. | Verminder het bereik van de bewerking via de meest beperkende querycriteria of neem contact op met ondersteuning van de [Azure-portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). <br><br>Example: *&nbsp;&nbsp;&nbsp;&nbsp;db.getCollection('users').aggregate([<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{$match: {name: "Andy"}}, <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{$sort: {age: -1}}<br>&nbsp;&nbsp;&nbsp;&nbsp;])*) |

## <a name="develop-with-the-table-api"></a>Met de tabel API ontwikkelen

### <a name="how-can-i-use-the-table-api-offering"></a>Hoe kan ik het aanbod van de tabel-API gebruiken? 
De Azure-API voor tabel Cosmos DB is beschikbaar in de [Azure-portal][azure-portal]. Eerst moet u aanmelden voor een Azure-abonnement. Nadat u zich hebt aangemeld, kunt u een tabel-API van Azure Cosmos DB-account toevoegen aan uw Azure-abonnement en tabellen vervolgens toe te voegen aan uw account. 

U vindt de ondersteunde talen en de bijbehorende quick start in de [Inleiding tot Azure Cosmos DB tabel API](table-introduction.md).

### <a name="do-i-need-a-new-sdk-to-use-the-table-api"></a>Moet ik een nieuwe SDK in de tabel-API gebruiken? 
Nee, bestaande opslag-SDK's moeten nog steeds werken. Maar het wordt aangeraden dat een altijd de nieuwste SDK's voor de beste ondersteuning en in veel gevallen hogere prestaties haalt. Zie de lijst met beschikbare talen in de [Inleiding tot Azure Cosmos DB tabel API](table-introduction.md).

### <a name="where-is-table-api-not-identical-with-azure-table-storage-behavior"></a>Waar wordt tabel API niet identiek zijn met Azure Table storage gedrag?
Er zijn enkele gedrag van die afkomstig zijn van Azure Table storage gebruikers willen om tabellen te maken met de Azure-API voor tabel Cosmos DB rekening houden met moet verschillen:

* Azure Cosmos DB tabel-API maakt gebruik van een gereserveerde capaciteit om ervoor te zorgen gegarandeerde prestaties, maar dit betekent dat een voor de capaciteit betaalt als de tabel is gemaakt, zelfs als de capaciteit niet wordt gebruikt. Met Azure Table storage betaalt één alleen voor capaciteit die daadwerkelijk wordt gebruikt. Dit helpt waarin wordt uitgelegd waarom tabel API kunt aanbieden aan dat een 10 ms lezen en 15 ms schrijven, SLA op het 99th percentiel terwijl Azure Table storage een 10 tweede SLA biedt. Maar als gevolg hiervan met tabel-API-tabellen, zelfs lege tabellen zonder verzoeken kosten geld om ervoor te zorgen dat de capaciteit is beschikbaar voor het verwerken van aanvragen voor deze aan de SLA die worden aangeboden door Azure Cosmos DB.
* De resultaten van de query is geretourneerd door de tabel-API worden niet gesorteerd partitie sleutel/rij sleutel omdat ze zich in Azure Table storage.
* Rij sleutels mag uit maximaal 255 bytes
* Batches mogen slechts tot 2 MB
* CORS wordt momenteel niet ondersteund.
* Namen van tabellen in de Azure Table storage zijn niet hoofdlettergevoelig, maar ze zijn in Azure Cosmos DB tabel-API
* Sommige Azure Cosmos DB interne indelingen voor codering informatie, zoals binaire velden zijn momenteel niet zo efficiënt als een mogelijk nodig hebt. Daarom kan dit onverwachte beperkingen op gegevensgrootte veroorzaken. Bijvoorbeeld: momenteel een kan niet gebruiken de volledige 1 Meg van een Tabelentiteit voor het opslaan van binaire gegevens omdat de codering van de gegevens groter wordt.

Er zijn een aantal eindpunten/query-opties die niet worden ondersteund door Azure Cosmos DB tabel API in termen van de REST-API:
| Rest-methode(n) | Rest-eindpunt/Query-optie | Doc-URL 's | Uitleg |
| ------------| ------------- | ---------- | ----------- |
| GET, PUT | /?restype=service@comp=properties| [Tabel-Service-eigenschappen instellen](https://docs.microsoft.com/rest/api/storageservices/set-table-service-properties) en [tabel Service-eigenschappen ophalen](https://docs.microsoft.com/rest/api/storageservices/get-table-service-properties) | Dit eindpunt wordt gebruikt voor het CORS-regels, configuratie van storage analytics en instellingen voor logboekregistratie instellen. CORS wordt momenteel niet ondersteund en analytics en logboekregistratie anders in Azure Cosmos DB dan Azure Storage-tabellen worden verwerkt |
| OPTIES | /<table-resource-name> | [Vooraf vlucht CORS tabel aanvraag](https://docs.microsoft.com/rest/api/storageservices/preflight-table-request) | Dit is onderdeel van CORS die Azure Cosmos DB momenteel niet ondersteund. |
| GET | /? restype =service@comp= statistieken | [Statistieken voor tabel-Service ophalen](https://docs.microsoft.com/rest/api/storageservices/get-table-service-stats) | Bevat informatie hoe snel van gegevens tussen de primaire en secundaire replica's repliceren. Dit is niet nodig in Cosmos-database omdat de replicatie deel uitmaakt van schrijfbewerkingen. |
| GET, PUT | /mytable?comp=acl | [Tabel ACL verkrijgen](https://docs.microsoft.com/rest/api/storageservices/get-table-acl) en [tabel ACL instellen](https://docs.microsoft.com/rest/api/storageservices/set-table-acl) | Dit opgehaald en ingesteld van de opgeslagen toegangsbeleid dat is gebruikt voor het beheren van Shared Access Signatures (SAS). Hoewel SAS wordt ondersteund, worden ze ingesteld en anders beheerd. |

Daarnaast ondersteunt Azure Cosmos DB tabel API alleen de JSON-indeling niet ATOM.

Biedt ondersteuning voor Azure Cosmos DB zijn Shared Access Signatures (SAS) er bepaalde beleidsregels wordt niet ondersteund, speciaal die gerelateerd zijn aan beheertaken uit te voeren zoals het recht om nieuwe tabellen te maken.

Voor de .NET SDK in het bijzonder, zijn er bepaalde klassen en methoden die Azure Cosmos DB momenteel niet ondersteund.

| Klasse | Niet-ondersteunde methode |
|-------|-------- |
| CloudTableClient | \*ServiceProperties* |
|                  | \*ServiceStats* |
| CloudTable | SetPermissions * |
|            | GetPermissions * |
| TableServiceContext | * (is deze klasse daadwerkelijk afgeschaft) |
| TableServiceEntity | " " |
| TableServiceExtensions | " " |
| TableServiceQuery | " " |

Als een van deze verschillen een probleem voor uw project contact op met [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com) en laat ons weten.

### <a name="how-do-i-provide-feedback-about-the-sdk-or-bugs"></a>Hoe ik feedback geven over de SDK of fouten?
U kunt uw feedback delen in een van de volgende manieren:

* [Uservoice](https://feedback.azure.com/forums/599062-azure-cosmos-db-table-api)
* [MSDN-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB)
* [Stack-overloop](http://stackoverflow.com/questions/tagged/azure-cosmosdb). Stack-overloop wordt aanbevolen voor het programmeren van vragen. Controleer of uw vraag is [op onderwerp](https://stackoverflow.com/help/on-topic) en [vindt u informatie van zo veel mogelijk, waardoor de vraag duidelijke en kan worden beantwoord](https://stackoverflow.com/help/how-to-ask).

### <a name="what-is-the-connection-string-that-i-need-to-use-to-connect-to-the-table-api"></a>Wat is de verbindingsreeks die ik moet verbinding maken met de tabel-API gebruiken?
De verbindingsreeks is:
```
DefaultEndpointsProtocol=https;AccountName=<AccountNamefromCosmos DB;AccountKey=<FromKeysPaneofCosmosDB>;TableEndpoint=https://<AccountName>.table.cosmosdb.azure.com
```
U kunt de verbindingsreeks ophalen via de pagina verbindingsreeks in de Azure-portal. 

### <a name="how-do-i-override-the-config-settings-for-the-request-options-in-the-net-sdk-for-the-table-api"></a>Hoe ik de configuratie-instellingen voor de aanvraag-opties in de .NET SDK voor de tabel-API overschrijven?
Zie voor informatie over configuratie-instellingen, [mogelijkheden van Azure DB die Cosmos](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). Sommige instellingen worden verwerkt op de methode CreateCloudTableClient en andere via het app.config in de sectie appSettings in de clienttoepassing.

### <a name="are-there-any-changes-for-customers-who-are-using-the-existing-azure-table-storage-sdks"></a>Zijn er wijzigingen voor klanten die de bestaande Azure Table-opslag-SDK's gebruiken?
Geen. Er zijn geen wijzigingen voor bestaande of nieuwe klanten die de bestaande Azure Table-opslag-SDK's. 

### <a name="how-do-i-view-table-data-that-is-stored-in-azure-cosmos-db-for-use-with-the-table-api"></a>Hoe kan ik gegevens in een tabel die is opgeslagen in Azure Cosmos DB voor gebruik met de tabel-API bekijken? 
De Azure-portal kunt u de gegevens bladeren. U kunt ook de tabel-API-code of de hulpprogramma's in het volgende antwoord genoemd. 

### <a name="which-tools-work-with-the-table-api"></a>Welke hulpprogramma's voor gebruik met de tabel-API? 
U kunt de [Azure Opslagverkenner](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).

Hulpprogramma's voor de flexibiliteit om op te nemen van een verbindingsreeks in de opgegeven indeling biedt eerder ondersteuning voor de nieuwe tabel-API. Een lijst met hulpprogramma's voor tabel vindt u op de [hulpprogramma's voor Azure Storage Client](../storage/common/storage-explorers.md) pagina. 

### <a name="do-powershell-or-azure-cli-work-with-the-table-api"></a>PowerShell of Azure CLI werken met de tabel-API?
Er is ondersteuning voor [PowerShell](table-powershell.md). Azure CLI-ondersteuning is momenteel niet beschikbaar.

### <a name="is-the-concurrency-on-operations-controlled"></a>Is de concurrency van bewerkingen die worden beheerd?
Ja, optimistische gelijktijdigheid van taken wordt opgegeven via het gebruik van het mechanisme voor ETag. 

### <a name="is-the-odata-query-model-supported-for-entities"></a>Wordt het OData-query-model voor entiteiten ondersteund? 
Ja, de tabel-API ondersteunt de OData-query en LINQ-query. 

### <a name="can-i-connect-to-azure-table-storage-and-azure-cosmos-db-table-api-side-by-side-in-the-same-application"></a>Kan ik verbinding maken met Azure Table Storage en Azure Cosmos DB tabel API gelijktijdige in dezelfde toepassing? 
Ja, u kunt verbinding maken met het maken van twee afzonderlijke exemplaren van de CloudTableClient, die elk een eigen URI via de verbindingsreeks te wijzen.

### <a name="how-do-i-migrate-an-existing-azure-table-storage-application-to-this-offering"></a>Hoe Migreer ik een bestaande Azure Table storage-toepassing op deze aanbieding
[AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy) en de [Azure Cosmos DB hulpprogramma voor gegevensmigratie](import-data.md) worden beide ondersteund.

### <a name="how-is-expansion-of-the-storage-size-done-for-this-service-if-for-example-i-start-with-n-gb-of-data-and-my-data-will-grow-to-1-tb-over-time"></a>Wat is het uitbreiden van de grootte van de opslagruimte voor deze service wordt uitgevoerd als er bijvoorbeeld wordt gestart met  *n*  GB aan gegevens en Mijn gegevens na verloop van tijd tot 1 TB zal toenemen? 
Azure Cosmos DB is zodanig ontworpen dat onbeperkte opslag via het gebruik van horizontaal schalen. De service kunt bewaken en effectief meer opslagruimte. 

### <a name="how-do-i-monitor-the-table-api-offering"></a>Hoe bewaak ik het aanbod van de tabel-API
U kunt de tabel-API gebruiken **metrische gegevens** deelvenster om aanvragen en opslaggebruik te bewaken. 

### <a name="how-do-i-calculate-the-throughput-i-require"></a>Hoe bereken ik de doorvoer die ik nodig?
U kunt de capaciteit estimator gebruiken voor het berekenen van de TableThroughput die zijn voor de bewerkingen vereist. Zie voor meer informatie [schatting Aanvraageenheden en gegevensopslag](https://www.documentdb.com/capacityplanner). In het algemeen kunt u uw entiteit als JSON vertegenwoordigen en de getallen opgeven voor uw bewerkingen. 

### <a name="can-i-use-the-table-api-sdk-locally-with-the-emulator"></a>Kan ik gebruiken de tabel API SDK lokaal met de emulator?
Momenteel niet.

### <a name="can-my-existing-application-work-with-the-table-api"></a>Kan mijn bestaande toepassing werken met de tabel-API? 
Ja, de dezelfde API wordt ondersteund.

### <a name="do-i-need-to-migrate-my-existing-azure-table-storage-applications-to-the-sdk-if-i-do-not-want-to-use-the-table-api-features"></a>Moet ik mijn bestaande Azure Table storage toepassingen met de SDK gemigreerd als ik niet wil gebruiken van de tabel-API-functies?
Nee, kunt u maken en gebruiken van bestaande Azure Table storage activa zonder onderbreking van elk type. Als u niet de tabel-API gebruikt, kunt u kan geen echter voor profiteren van de automatische index, extra consistentie optie of distributielijsten. 

### <a name="how-do-i-add-replication-of-the-data-in-the-table-api-across-multiple-regions-of-azure"></a>Hoe voeg ik replicatie van de gegevens in de tabel-API over meerdere regio's van Azure?
U kunt de Azure DB die Cosmos-portal [globale replicatie-instellingen](tutorial-global-distribution-sql-api.md#portal) regio's die geschikt voor uw toepassing zijn toevoegen. Voor het ontwikkelen van een globaal gedistribueerde toepassing, moet u ook uw toepassing toevoegen met de informatie PreferredLocation is ingesteld op de lokale regio voor het ontwikkelen van de lage latentie voor lezen. 

### <a name="how-do-i-change-the-primary-write-region-for-the-account-in-the-table-api"></a>Hoe wijzig ik de regio primaire schrijven voor het account in de tabel-API
Het Azure Cosmos DB globale replicatie portal deelvenster kunt u een regio toevoegen en vervolgens een failover uitvoeren naar de vereiste regio. Zie voor instructies [ontwikkelen met meerdere landen/regio Azure Cosmos DB accounts](regional-failover.md). 

### <a name="how-do-i-configure-my-preferred-read-regions-for-low-latency-when-i-distribute-my-data"></a>Hoe kan ik mijn voorkeur lezen regio's voor de lage latentie configureren wanneer ik mijn gegevens distribueert? 
Om te lezen van de lokale locatie, gebruikt u de sleutel PreferredLocation in het bestand app.config. Voor bestaande toepassingen wordt in de API van de tabel een fout genereert als LocationMode is ingesteld. Deze code niet verwijderen omdat de tabel-API neemt over deze informatie in het bestand app.config. Zie voor meer informatie [mogelijkheden van Azure DB die Cosmos](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="how-should-i-think-about-consistency-levels-in-the-table-api"></a>Hoe moet ik denken dat zij consistentieniveaus in de tabel-API? 
Azure Cosmos DB biedt goed gemotiveerd verschillen tussen de consistentie, beschikbaarheid en latentie. Azure Cosmos DB biedt vijf consistentieniveaus voor tabel-API-ontwikkelaars, zodat u kunt kiezen voor het model van de juiste consistentiecontrole op het tabelniveau van de en afzonderlijke aanvragen maken tijdens het doorzoeken van de gegevens. Wanneer een client verbinding maakt, kunt het niveau van een consistentiecontrole opgeven. U kunt het niveau via het argument consistencyLevel van CreateCloudTableClient wijzigen. 

De tabel-API biedt lage latentie leest met 'lezen uw eigen schrijfbewerkingen' met de consistentiecontrole dat wordt begrensd veroudering als standaardwaarde. Zie voor meer informatie [consistentieniveaus](consistency-levels.md). 

Azure Table storage biedt standaard sterke consistentie binnen een regio en Eventual consistentie in de secundaire locaties. 

### <a name="does-azure-cosmos-db-table-api-offer-more-consistency-levels-than-azure-table-storage"></a>Biedt Azure Cosmos DB tabel API meer consistentieniveaus dan Azure Table storage?
Ja, voor informatie over hoe u kunt profiteren van de gedistribueerde aard van Azure Cosmos DB, Zie [consistentieniveaus](consistency-levels.md). Omdat de garanties zijn opgegeven voor de consistentieniveaus, kunt u ze kunt gebruiken met vertrouwen. Zie voor meer informatie [mogelijkheden van Azure DB die Cosmos](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="when-global-distribution-is-enabled-how-long-does-it-take-to-replicate-the-data"></a>Wanneer de algemene distributie is ingeschakeld, hoe lang duurt om de gegevens te repliceren?
Azure Cosmos DB voert de gegevens in de lokale regio blijvend en duwt de gegevens naar andere regio's binnen een paar milliseconden onmiddellijk. Deze replicatie is afhankelijk van de round trip-tijd (RTT) van het datacenter. Zie voor meer informatie over de algemene distributie-functionaliteit van Azure Cosmos DB, [Azure Cosmos DB: een wereldwijd gedistribueerde database-service op Azure](distribute-data-globally.md).

### <a name="can-the-read-request-consistency-level-be-changed"></a>Kan het niveau van de consistentie leesaanvraag worden gewijzigd?
Met Azure Cosmos DB, kunt u het consistentieniveau van de instellen op het niveau van de container (op de tabel). U kunt het niveau wijzigen door de waarde voor de sleutel in het bestand app.config TableConsistencyLevel met behulp van de .NET SDK. De mogelijke waarden zijn: sterk, gebonden veroudering sessie, consistente voorvoegsel en Eventual. Zie voor meer informatie [gegevens instelbare consistentieniveaus in Azure Cosmos DB](consistency-levels.md). Het belangrijkste idee is dat u niet de consistentie van de aanvraag niveau op meer dan de instelling voor de tabel instellen. U kunt het consistentieniveau van de voor de tabel bijvoorbeeld niet instellen op Eventual en het niveau aanvraag consistentie van sterke. 

### <a name="how-does-the-table-api-handle-failover-if-a-region-goes-down"></a>Hoe wordt de tabel-API verwerkt failover als een regio uitgeschakeld wordt? 
De tabel-API maakt gebruik van het globaal gedistribueerde Azure DB die Cosmos-platform. Inschakelen om ervoor te zorgen dat uw toepassing datacenter uitvaltijd kan tolereren, ten minste één meer regio voor het account in de portal voor Azure Cosmos DB [ontwikkelen met meerdere landen/regio Azure Cosmos DB accounts](regional-failover.md). U kunt de prioriteit van de regio instellen met behulp van de portal [ontwikkelen met meerdere landen/regio Azure Cosmos DB accounts](regional-failover.md). 

U kunt zoveel regio als u wilt gebruiken voor het account en waar deze een failover bepalen naar door te geven van een failover-prioriteit toevoegen. Voor het gebruik van de database, moet u natuurlijk er een toepassing te bieden. Wanneer u doet dit, krijgen uw klanten geen uitvaltijd. De [nieuwste .NET client SDK](table-sdk-dotnet.md) automatisch homing is, maar de andere SDK's zijn niet. Dat wil zeggen, kunnen de regio die niet actief is en automatisch een failover naar de nieuwe regio worden gedetecteerd.

### <a name="is-the-table-api-enabled-for-backups"></a>Is de tabel-API voor back-ups ingeschakeld?
Ja, de tabel-API maakt gebruik van het platform van Azure Cosmos-database voor back-ups. Back-ups worden automatisch gemaakt. Zie voor meer informatie [Online back-up en herstel met Azure Cosmos DB](online-backup-and-restore.md).

 
### <a name="does-the-table-api-index-all-attributes-of-an-entity-by-default"></a>De API van de tabel alle kenmerken van een entiteit index standaard
Ja, worden alle kenmerken van een entiteit geïndexeerd standaard. Zie voor meer informatie [Azure Cosmos DB: beleid indexeren](indexing-policies.md). 

### <a name="does-this-mean-i-do-not-have-to-create-multiple-indexes-to-satisfy-the-queries"></a>Betekent dit dat ik heb geen meerdere indexen om te voldoen aan de query's maken? 
Ja, biedt Azure Cosmos DB tabel API automatische indexeren van alle kenmerken zonder eventuele schemadefinitie. Deze automatisering wordt vrijgemaakt ontwikkelaars zich te concentreren op de toepassing in plaats van op index maken en beheren. Zie voor meer informatie [Azure Cosmos DB: beleid indexeren](indexing-policies.md).

### <a name="can-i-change-the-indexing-policy"></a>Kan ik het indexeringsbeleid wijzigen?
Ja, kunt u het indexeringsbeleid doordat de indexdefinitie. Zie voor meer informatie [mogelijkheden van Azure DB die Cosmos](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). U moet goed coderen en escape voor de instellingen. 

voor de niet - .NET SDK's het indexeringsbeleid kan alleen worden ingesteld in de portal op **Data Explorer**, gaat u naar de specifieke tabel die u wilt wijzigen en Ga naar de **schaal & instellingen**indexeren beleid -> Breng de gewenste wijziging en vervolgens **opslaan**.

Van de .NET SDK kan deze worden verstuurd in het bestand app.config:
```
{
  "indexingMode": "consistent",
  "automatic": true,
  "includedPaths": [
    {
      "path": "/somepath",
      "indexes": [
        {
          "kind": "Range",
          "dataType": "Number",
          "precision": -1
        },
        {
          "kind": "Range",
          "dataType": "String",
          "precision": -1
        } 
      ]
    }
  ],
  "excludedPaths": 
[
 {
      "path": "/anotherpath"
 }
]
}
```

### <a name="azure-cosmos-db-as-a-platform-seems-to-have-lot-of-capabilities-such-as-sorting-aggregates-hierarchy-and-other-functionality-will-you-be-adding-these-capabilities-to-the-table-api"></a>Azure Cosmos-database als een platform waarschijnlijk veel mogelijkheden, zoals sorteren, statistische functies, hiërarchie en andere functionaliteit. Gaat u deze mogelijkheden toevoegen aan de tabel-API? 
De tabel-API biedt dezelfde queryfunctionaliteit als Azure Table storage. Azure Cosmos DB biedt ook ondersteuning voor sorteren, statistische functies, georuimtelijke query hiërarchie en een groot aantal ingebouwde functies. We bieden aanvullende functionaliteit in de tabel-API in een toekomstige service-update. Zie voor meer informatie [SQL-query's](sql-api-sql-query.md).
 
### <a name="when-should-i-change-tablethroughput-for-the-table-api"></a>Wanneer moet ik TableThroughput wijzigen voor de tabel-API?
Wanneer een van de volgende voorwaarden van toepassing is, moet u TableThroughput wijzigen:
* U voert een uitpakken, transformeren en laden (ETL) van gegevens of u wilt uploaden van een grote hoeveelheid gegevens in korte tijd. 
* U moet meer doorvoer van de container op de back-end. Bijvoorbeeld, ziet u dat de gebruikte doorvoer meer dan de ingerichte doorvoer is en u zijn ophalen beperkt. Zie voor meer informatie [Set doorvoer voor Azure Cosmos DB containers](set-throughput.md).

### <a name="can-i-scale-up-or-scale-down-the-throughput-of-my-table-api-table"></a>Kan ik opschalen of terugschroeven de doorvoer van de tabel tabel API? 
Ja, kunt u de Azure DB die Cosmos-portal scale deelvenster schalen van de doorvoer. Zie voor meer informatie [Set doorvoer](set-throughput.md).

### <a name="is-a-default-tablethroughput-set-for-newly-provisioned-tables"></a>Is een standaard die tablethroughput voor nieuw ingerichte tabellen ingesteld?
Ja, als u de TableThroughput via app.config niet overschrijven en gebruik niet een vooraf gemaakte container in Azure Cosmos DB, maakt de service een tabel met doorvoersnelheid van 400.
 
### <a name="is-there-any-change-of-pricing-for-existing-customers-of-the-azure-table-storage-service"></a>Is er een wijziging van de prijzen voor bestaande klanten van de service Azure Table storage?
Geen. Er is geen wijziging in de prijs voor bestaande klanten voor Azure Table-opslag. 

### <a name="how-is-the-price-calculated-for-the-table-api"></a>Hoe wordt de prijs berekend voor de tabel-API? 
De prijs is afhankelijk van de toegewezen TableThroughput. 

### <a name="how-do-i-handle-any-throttling-on-the-tables-in-table-api-offering"></a>Hoe u verwerkt beperking op de tabellen in de tabel API aanbieding? 
Als het percentage aanvragen groter is dan de capaciteit van de ingerichte doorvoer voor de onderliggende container, u krijgt een fout en de SDK de aanroep pogingen door het toepassen van het beleid voor opnieuw proberen.

### <a name="why-do-i-need-to-choose-a-throughput-apart-from-partitionkey-and-rowkey-to-take-advantage-of-the-table-api-offering-of-azure-cosmos-db"></a>Waarom moet ik een doorvoer naast PartitionKey en RowKey om te profiteren van de tabel-API-aanbod van Azure DB die Cosmos kiezen?
Als u niet dat een in het bestand app.config of via de portal opgeven, Azure Cosmos-DB een standaard-doorvoer ingesteld voor de container. 

Azure Cosmos DB biedt garanties voor prestaties en de latentie met bovengrenzen van bewerking. Deze garantie is mogelijk wanneer de engine voor het beheer van de tenant-bewerkingen kunt afdwingen. TableThroughput instelling zorgt ervoor dat u de gegarandeerde doorvoer en latentie, omdat het platform deze capaciteit gereserveerd en wordt gegarandeerd dat operationele geslaagd. 

Met behulp van de specificatie doorvoer, kunt u deze om te profiteren van de seizoensgebonden van uw toepassing, voldoen aan de doorvoerbehoeften en kosten besparen elastisch wijzigen.

### <a name="azure-table-storage-has-been-very-inexpensive-for-me-because-i-pay-only-to-store-the-data-and-i-rarely-query-the-azure-cosmos-db-table-api-offering-seems-to-be-charging-me-even-though-i-have-not-performed-a-single-transaction-or-stored-anything-can-you-please-explain"></a>Azure Table storage is zeer goedkope voor mij, omdat ik betaalt alleen voor het opslaan van de gegevens, en ik zelden query. De aanbieding voor Azure Cosmos DB tabel API lijkt te worden in rekening gebracht mij ondanks dat ik niet heb één transactie uitgevoerd of iets opgeslagen. Kunt u Leg?

Azure Cosmos DB is ontworpen om te worden van een systeem globaal gedistribueerd, op basis van een SLA met garanties met betrekking tot beschikbaarheid, latentie en doorvoer. Bij het reserveren van doorvoer in Azure DB die Cosmos is gegarandeerd, in tegenstelling tot de doorvoer van andere systemen. Azure Cosmos DB biedt aanvullende functies die klanten hebt aangevraagd, zoals secundaire indexen en distributielijsten.  

### <a name="i-never-get-a-quota-full-notification-indicating-that-a-partition-is-full-when-i-ingest-data-into-azure-table-storage-with-the-table-api-i-do-get-this-message-is-this-offering-limiting-me-and-forcing-me-to-change-my-existing-application"></a>Krijg ik nooit een ' volledige ' quotamelding (waarmee wordt aangegeven dat een partitie vol is) wanneer ik opnemen van gegevens in Azure Table storage. Met de API tabel krijg ik dit bericht. Dit biedt mij beperken en dwingen mij mijn bestaande toepassing aan te passen?

Azure Cosmos-database is een SLA-systeem waarmee onbeperkte scale garanties met betrekking tot latentie, doorvoer, beschikbaarheid en consistentie. Om ervoor te zorgen gegarandeerde premium prestaties, zorg ervoor dat uw gegevensgrootte en een index beheerbare en schaalbare. De limiet van 10 GB op het aantal entiteiten of items per partitiesleutel is om ervoor te zorgen dat wij goede zoekopdracht en query-prestaties. Om ervoor te zorgen dat uw toepassing, zelfs voor Azure Storage schaalt, raden we u *niet* een hot partitie maken door alle informatie in een partitie op te slaan en het uitvoeren van query's. 

### <a name="so-partitionkey-and-rowkey-are-still-required-with-the-table-api"></a>Dus PartitionKey en RowKey zijn nog steeds vereist met de tabel-API? 
Ja. Omdat het serveroppervlaktegebied van de tabel-API vergelijkbaar met die van de SDK van Azure Table storage is, biedt de partitiesleutel een efficiënte manier voor het distribueren van de gegevens. De rijsleutel is uniek zijn binnen deze partitie. De rijsleutel moet aanwezig zijn en mag niet null zijn als in de standaard SDK. De lengte van RowKey is 255 bytes en de lengte van PartitionKey is 1 KB. 

### <a name="what-are-the-error-messages-for-the-table-api"></a>Wat zijn de foutberichten voor de tabel-API?
Azure Table storage en Azure Cosmos DB tabel API gebruiken de dezelfde SDK's zodat de meeste van de fouten hetzelfde zijn.

### <a name="why-do-i-get-throttled-when-i-try-to-create-lot-of-tables-one-after-another-in-the-table-api"></a>Waarom ik ophalen beperkte bandbreedte wanneer ik probeer te veel tabellen achter elkaar maken in de tabel-API?
Azure Cosmos-database is een SLA-systeem waarmee latentie, doorvoer, beschikbaarheid en consistentie wordt gegarandeerd. Omdat het een ingerichte systeem, reserveren het van bronnen om te garanderen van deze vereisten. De snelle frequentie van het maken van tabellen is gedetecteerd en beperkt. U wordt aangeraden dat u kijken naar de frequentie van het maken van tabellen en het verlagen naar minder dan 5 per minuut. Houd er rekening mee dat de tabel-API een ingerichte systeem is. Het moment dat u inricht, begint u betalen. 

## <a name="develop-against-the-graph-api"></a>Op basis van de grafiek API ontwikkelen
### <a name="how-can-i-apply-the-functionality-of-graph-api-to-azure-cosmos-db"></a>Hoe kan ik de functionaliteit van Graph API toepassen op Azure Cosmos DB?
Toepassen van de functionaliteit van Graph API kunt u een extensiebibliotheek. Deze bibliotheek Microsoft Azure-grafieken wordt genoemd, en is beschikbaar in NuGet. 

### <a name="it-looks-like-you-support-the-gremlin-graph-traversal-language-do-you-plan-to-add-more-forms-of-query"></a>Het lijkt erop dat u de taal Gremlin grafiek traversal ondersteunt. Wilt u meer formulieren van de query toevoegen?
Ja, we willen andere mechanismen voor query in de toekomst toevoegen. 

### <a name="how-can-i-use-the-new-graph-api-offering"></a>Hoe kan ik de nieuwe Graph API aanbieding gebruiken? 
Om te beginnen, voltooi de [Graph API](../cosmos-db/create-graph-dotnet.md) snel starten-artikel.

<a id="cassandra"></a> 
## <a name="develop-with-the-apache-cassandra-api-preview"></a>Ontwikkelen met de Apache Cassandra API (preview)

### <a name="what-is-the-protocol-version-supported-in-the-private-preview-is-there-a-plan-to-support-other-protocols"></a>Wat is de versie van het protocol private Preview-versie ondersteund? Is er een plan voor de ondersteuning van andere protocollen?
Apache Cassandra API voor Azure Cosmos DB ondersteunt vandaag CQL versie 4. Als u feedback hebt over ondersteuning van andere protocollen, laat ons weten [feedback op uservoice](https://feedback.azure.com/forums/263030-azure-cosmos-db) of stuur een e-mail naar [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com). 

### <a name="why-is-choosing-a-throughput-for-a-table-a-requirement"></a>Waarom is een doorvoer voor een tabel te kiezen een vereiste?
Azure Cosmos DB standaard doorvoer ingesteld voor de container op basis van waar het maken van de tabel van - portal of CQL. Azure Cosmos DB biedt garanties voor prestaties en de latentie met bovengrenzen van bewerking. Deze garantie is mogelijk wanneer de engine voor het beheer van de tenant-bewerkingen kunt afdwingen. Doorvoer van de instelling zorgt ervoor dat u de gegarandeerde doorvoer en latentie, omdat het platform deze capaciteit gereserveerd en wordt gegarandeerd dat de geslaagde bewerking. U kunt elastisch doorvoer om te profiteren van de seizoensgebonden van uw toepassing en kosten besparen wijzigen.

Het concept doorvoer wordt uitgelegd in de [Aanvraageenheden in Azure Cosmos DB](request-units.md) artikel. De doorvoer voor een tabel is evenredig verdeeld over de onderliggende fysieke partities.  

### <a name="what-is-the-default-rus-of-table-when-created-through-cql-what-if-i-need-to-change-it"></a>Wat is de standaardinstelling RU/s van tabel wanneer via CQL gemaakt? Wat gebeurt er als ik nodig om deze te wijzigen?
Azure Cosmos-database maakt gebruik van aanvraageenheden per seconde (RU/s) als valuta voor het ontwikkelen van doorvoer. Tabellen die zijn gemaakt via CQL hebben 400 RU. U kunt de RU wijzigen via de portal. 

CQL
```
CREATE TABLE keyspaceName.tablename (user_id int PRIMARY KEY, lastname text) WITH cosmosdb_provisioned_throughput=1200
```

.NET
```csharp
int provisionedThroughput = 400;
var simpleStatement = new SimpleStatement($"CREATE TABLE {keyspaceName}.{tableName} (user_id int PRIMARY KEY, lastname text)");
var outgoingPayload = new Dictionary<string, byte[]>();
outgoingPayload["cosmosdb_provisioned_throughput"] = Encoding.UTF8.GetBytes(provisionedThroughput.ToString());
simpleStatement.SetOutgoingPayload(outgoingPayload); 
``` 

### <a name="what-happens-when-throughput-is-exceeded"></a>Wat gebeurt er wanneer de doorvoer wordt overschreden? 
Azure Cosmos DB biedt garanties voor prestaties en de latentie met bovengrenzen van bewerking. Deze garantie is mogelijk wanneer de engine voor het beheer van de tenant-bewerkingen kunt afdwingen. Dit is mogelijk gebaseerd op het instellen van de doorvoer, wat zorgt ervoor dat u de gegarandeerde doorvoer en latentie, omdat het platform deze capaciteit gereserveerd en wordt gegarandeerd dat de geslaagde bewerking. Als u deze capaciteit overschrijdt krijgt u de overbelaste foutbericht dat uw capaciteit is overschreden. 0x1001 overbelast: de aanvraag kan niet worden verwerkt omdat 'Aanvraagsnelheid hoog is'. In dit stadium is het essentieel om te zien welke bewerkingen en het volume wordt dit probleem. U krijgt een idee van verbruikte capaciteit van meer dan de ingerichte capaciteit met metrische gegevens op de portal. Vervolgens moet u ervoor zorgen capaciteit is verbruikt, bijna even voor alle onderliggende partities. Als u ziet in de meeste van de doorvoer door één partitie wordt gebruikt, hebt u scheeftrekken van werkbelasting. 

Metrische gegevens beschikbaar zijn die ziet u hoe doorvoer wordt gebruikt via uren, dagen, en per zeven dagen meerdere partities of geaggregeerd. Zie voor meer informatie [bewaking en foutopsporing met metrische gegevens in Azure Cosmos DB](use-metrics.md).

Diagnostische logboeken worden beschreven de [Diagnostische logboekregistratie van Azure DB die Cosmos](logging.md) artikel.

### <a name="does-the-primary-key-map-to-the-partition-key-concept-of-azure-cosmos-db"></a>Ondersteunt de primaire sleutel toewijzen aan de partitie sleutel concept van Azure DB die Cosmos?
Ja, de partitiesleutel wordt gebruikt voor de entiteit in de juiste locatie plaatsen. In Azure Cosmos DB gebruikt dit recht logische partitie die is opgeslagen op een fysieke partitie vinden. Het concept partitionering wordt ook uitgelegd in de [partitie en schalen in Azure Cosmos DB](partition-data.md) artikel. De essentiële nemen is afwezig hier dat een logische partitie mag niet langer zijn dan de limiet van 10 GB vandaag. 

### <a name="what-happens-when-i-get-a-quota-full-notification-indicating-that-a-partition-is-full"></a>Wat gebeurt er wanneer ik een ' volledige ' quotamelding die aangeeft dat een partitie ophalen vol is?
Azure Cosmos-database is een SLA-systeem waarmee onbeperkte scale garanties met betrekking tot latentie, doorvoer, beschikbaarheid en consistentie. De Cassandra API kunt te onbeperkte opslag van gegevens. Deze onbeperkte opslag is gebaseerd op de horizontale scaleout van gegevens met behulp van partitioneren van de belangrijkste concept. Het concept partitionering wordt ook uitgelegd in de [partitie en schalen in Azure Cosmos DB](partition-data.md) artikel.

De limiet van 10 GB op het aantal entiteiten of items per logische partitie die moet u voldoen aan. Om ervoor te zorgen dat uw toepassing goed schaalbaar, raden we u *niet* een hot partitie maken door alle informatie in een partitie op te slaan en het uitvoeren van query's. Deze fout kan alleen afkomstig zijn als u gegevens is vervormd - die is dat u grote hoeveelheden gegevens voor een partitiesleutel - dat wil zeggen, zijn meer dan 10 GB. U vindt de distributie van gegevens met behulp van de portal van de opslag. Manier om deze fout te herstellen is het recrete in de tabel en kies een gedetailleerde primaire (partitiesleutel), waardoor betere verdeling van gegevens.

### <a name="is-it-possible-to-use-cassandra-api-as-key-value-store-with-millions-or-billions-of-individual-partition-keys"></a>Is het mogelijk om Cassandra API als sleutelwaarde archief met van miljoenen of miljarden afzonderlijke partitiesleutels gebruiken?
Azure Cosmos DB kunt onbeperkte gegevens opslaan door het uitbreiden van de opslag. Dit is onafhankelijk van de doorvoer. U kunt de Cassandra API Ja altijd alleen gebruiken voor het opslaan en ophalen van sleutel en-waarden door te geven van de juiste primaire/partitiesleutel. Deze afzonderlijke sleutels krijgen hun eigen logische partitie en zich bevinden op fysieke partitie zonder problemen. 

### <a name="is-it-possible-to-create-multiple-tables-with-apache-cassandra-api-of-azure-cosmos-db"></a>Is het mogelijk te maken van meerdere tabellen met Apache Cassandra API van Azure Cosmos DB?
Ja, is het mogelijk te maken meerdere tabellen met Apache Cassandra API. Elk van deze tabellen wordt beschouwd als eenheid voor doorvoer en opslag. 

### <a name="is-it-possible-to-create-multiple-tables-in-succession"></a>Is het mogelijk te maken van meerdere tabellen achter elkaar?
Azure Cosmos DB is resource bepaald systeem voor zowel gegevens als besturingselement vlak-activiteiten. Containers zoals verzamelingen, tabellen zijn runtime-entiteiten die zijn ingericht voor gegeven doorvoercapaciteit. Het maken van deze containers snel achter elkaar is niet de verwachte activiteit en beperkt. Probeer om ze als u tests die neerzetten/tabellen onmiddellijk maken - hebt.

### <a name="what-is-maximum-number-of-tables-which-can-be-created"></a>Wat is de maximum aantal tabellen die kan worden gemaakt?
Er is geen fysieke limiet voor aantal tabellen, stuurt u een e-mailbericht op [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com) als u een zeer groot aantal tabellen (waarbij de totale grootte van de steeds groter wordt dan 10 TB aan gegevens) die moeten worden gemaakt van gebruikelijke per 10 of 100s hebt. 

### <a name="what-is-the-maximum--of-keyspace-which-we-can-create"></a>Wat is het maximum aantal keyspace die we kunt maken? 
Er is geen fysieke limiet voor aantal keyspaces omdat ze metagegevens containers, stuurt u een e-mailbericht op [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com) als u een zeer groot aantal keyspaces voor een bepaalde reden hebt. 

### <a name="is-it-possible-to-bring-in-lot-of-data-after-starting-from-normal-table"></a>Is het mogelijk op te zetten in grote hoeveelheden gegevens na het starten van de normale tabel? 
De capaciteit automatisch wordt beheerd en neemt toe naarmate u meer gegevens pushen. U kunt dus zorgeloos zo veel mogelijk gegevens zonder beheren en inrichting van knooppunten, enzovoort importeren. 

### <a name="is-it-possible-to-supply-yaml-file-settings-to-configure-apache-casssandra-api-of-azure-cosmos-db-behavior"></a>Is het mogelijk om op te geven yaml-bestandsinstellingen om Apache Casssandra API van Azure Cosmos DB gedrag te configureren?
Apache Cassandra API van Azure Cosmos DB is een platformservice. Het biedt protocol niveau compatibiliteit voor het uitvoeren van bewerkingen. Deze wordt opgeslagen de complexiteit van het beheer, bewaking en configuratie verborgen. Als een ontwikkelaar/gebruiker hoeft u geen zorgen te hoeven maken over de beschikbaarheid, tombstones, key cache, rij-cache, waas filter en groot aantal andere instellingen. Azure Cosmos-DB Apache Cassandra API richt zich op het bieden van lees- en schrijfprestaties die u zonder de overhead van de configuratie en beheer moet.

### <a name="will-apache-cassandra-api-for-azure-cosmos-db-support-node-additioncluster-statusnode-status-commands"></a>Biedt ondersteuning voor Apache Cassandra API voor Azure Cosmos DB knooppunt toevoeging/cluster node Componentstatus/status opdrachten?
Apache Cassandra API is een platformservice waardoor capaciteitsplanning reageert op de vraag elasticiteit voor doorvoer en opslag zeer eenvoudig. U inrichten doorvoer u moet met Azure Cosmos DB. Vervolgens kunt u deze omhoog en omlaag vaak via de dag schalen zonder dat u knooppunten toevoegen/verwijderen of beheren. Dit betekent dat u niet wilt dat het knooppunt, cluster management-hulpprogramma te gebruiken. 

### <a name="what-happens-with-respect-to-various-config-settings-for-keyspace-creation-like-simplenetwork"></a>Wat gebeurt er met betrekking tot verschillende configuratie-instellingen voor het maken van keyspace zoals eenvoudige/netwerk?
Azure Cosmos DB biedt wereldwijde distributie buiten het voor de beschikbaarheid en lage latentie redenen. U hoeft niet te setup replica's enz. Alle schrijfbewerkingen zijn altijd blijvend quorum in een elke regio waarin u schrijven waarbij prestaties garanties doorgevoerd.  

### <a name="what-happens-with-respect-to-various-settings-for-table-metadata-like-bloom-filter-caching-read-repair-change-gcgrace-compression-memtableflushperiod-etc"></a>Wat gebeurt er met betrekking tot verschillende instellingen voor tabelmetagegevens, zoals waas filter, cache lezen reparatie wijziging, gc_grace, compressie memtable_flush_period enzovoort?
Azure Cosmos DB biedt prestaties voor leest/schrijft en doorvoer zonder behoefte aan een van de configuratie-instellingen en per ongeluk te bewerken.  

### <a name="is-time-to-live-ttl-supported-for-cassandra-tables"></a>Wordt time to live (TTL) voor Cassandra tabellen ondersteund? 
Ja, de TTL wordt ondersteund. 

### <a name="is-it-possible-to-monitor-node-status-replica-status-gc-and-os-parameters-earlier-with-various-tools-what-needs-to-be-monitored-now"></a>Is het mogelijk om de monitor knooppunt status, status van replica gc en OS-parameters eerder met verschillende hulpprogramma's? Wat er moet nu worden bewaakt?
Azure Cosmos-database is een platformservice waarmee u de productiviteit verhogen en u geen zorgen over het beheren en controleren van de infrastructuur. U moet alleen zorgt voor doorvoer die beschikbaar in de portal metrische gegevens om te zoeken is als zijn ophalen beperkt en het vergroten of verkleinen waarop doorvoer. Monitor [serviceovereenkomsten](monitor-accounts.md).
Gebruik [metrische gegevens](use-metrics.md) gebruik [diagnostische logboeken](logging.md).

### <a name="which-client-sdks-can-work-with-apache-cassandra-api-of-azure-cosmos-db"></a>Welke client-SDK's kunnen werken met Apache Cassandra API van Azure Cosmos DB?
Stuurprogramma's die gebruik CQLv3 zijn persoonlijke preview Apache Cassandra van de SDK-client voor clientprogramma's gebruikt. Als u beschikt over andere stuurprogramma's die u gebruikt of als u problemen worden geconfronteerd verzend een e-mail naar [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com). 

### <a name="is-composite-partition-key-supported"></a>Wordt samengesteld partitiesleutel ondersteund?
Ja, kunt u de syntaxis van de reguliere samengestelde partitiesleutel maken. 

### <a name="can-i-use-sstable-loader-for-data-loading"></a>Kan ik sstable loader gebruiken om gegevens te laden?
Nee, tijdens de preview sstable loader wordt niet ondersteund. 

### <a name="can-an-on-premises-cassandra-cluster-be-paired-with-azure-cosmos-dbs-apache-cassandra-api"></a>Kan een on-premises cassandra-cluster worden gekoppeld aan Azure Cosmos DB Apache Cassandra API?
Bij aanwezig Azure Cosmos DB heeft een geoptimaliseerde ervaring voor cloudomgeving zonder de overhead van bewerkingen. Als u koppelen wilt, stuurt u een e-mail naar [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com) met een beschrijving van uw scenario.

### <a name="does-cassandra-api-provide-full-backups"></a>Biedt Cassandra API volledige back-ups 
Azure Cosmos DB biedt twee gratis volledige back-ups die op basis van vier uur interval vandaag tussen alle API's. Hierdoor hoeft u niet voor het instellen van een back-upschema enzovoort. Als u wijzigen bewaring en frequentie wilt, stuurt u een e-mail naar [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com) of een ondersteuningsaanvraag te verhogen. Informatie over back-upfunctionaliteit vindt u in de [automatische online back-up en herstel met Azure Cosmos DB](online-backup-and-restore.md) artikel. 

### <a name="how-does-the-cassandra-api-account-handle-failover-if-a-region-goes-down"></a>Hoe wordt het account Cassandra API verwerkt failover als een regio uitgeschakeld wordt? 
De Azure-API voor Cassandra Cosmos DB voortbouwt op het globaal gedistribueerde Azure DB die Cosmos-platform. Inschakelen om ervoor te zorgen dat uw toepassing datacenter uitvaltijd kan tolereren, ten minste één meer regio voor het account in de portal voor Azure Cosmos DB [ontwikkelen met meerdere landen/regio Azure Cosmos DB accounts](regional-failover.md). U kunt de prioriteit van de regio instellen met behulp van de portal [ontwikkelen met meerdere landen/regio Azure Cosmos DB accounts](regional-failover.md). 

U kunt zoveel regio als u wilt gebruiken voor het account en waar deze een failover bepalen naar door te geven van een failover-prioriteit toevoegen. Voor het gebruik van de database, moet u er een toepassing te bieden. Wanneer u doet dit, krijgen uw klanten geen uitvaltijd. 

### <a name="does-the-apache-cassandra-api-index-all-attributes-of-an-entity-by-default"></a>De Apache Cassandra API alle kenmerken van een entiteit index standaard
Ja, worden alle kenmerken van een entiteit geïndexeerd standaard door Azure Cosmos DB. Zie voor meer informatie [Azure Cosmos DB: beleid indexeren](indexing-policies.md). Voordelen van gegarandeerde prestaties met consistente indexeren en duurzame quorum doorgevoerd altijd schrijft. 

### <a name="does-this-mean-i-do-not-have-to-create-multiple-indexes-to-satisfy-the-queries"></a>Betekent dit dat ik heb geen meerdere indexen om te voldoen aan de query's maken? 
Ja, biedt Azure Cosmos DB automatische indexeren van alle kenmerken zonder eventuele schemadefinitie. Deze automatisering wordt vrijgemaakt ontwikkelaars zich te concentreren op de toepassing in plaats van op index maken en beheren. Zie voor meer informatie [Azure Cosmos DB: beleid indexeren](indexing-policies.md).

### <a name="can-i-use-the-new-cassandra-api-sdk-locally-with-the-emulator"></a>Kan ik gebruiken de nieuwe Cassandra API SDK lokaal met de emulator?
We willen ondersteuning van deze functie in de toekomst. 

### <a name="azure-cosmos-db-as-a-platform-seems-to-have-lot-of-capabilities-such-as-changefeed-and-other-functionality-will-these-capabilities-be-added-to-the-cassandra-api"></a>Azure Cosmos-database als een platform waarschijnlijk veel mogelijkheden, zoals changefeed en andere functionaliteit. Deze mogelijkheden worden toegevoegd aan de API Cassandra? 
De Apache Cassandra-API biedt dezelfde functionaliteit als Apache Cassandra CQL. We bent van plan te onderzoeken haalbaarheid van verschillende mogelijkheden in de toekomst te ondersteunen.

### <a name="feature-x-of-regular-cassandra-api-is-not-working-as-today-where-can-the-feedback-be-provided"></a>Functie x van reguliere Cassandra API werkt niet als vandaag de dag waarop kan de feedback worden opgegeven?
Feedback geven via [feedback op uservoice](https://feedback.azure.com/forums/263030-azure-cosmos-db).

[azure-portal]: https://portal.azure.com
[query]: sql-api-sql-query.md

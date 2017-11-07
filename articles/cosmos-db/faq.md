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
ms.date: 11/02/2017
ms.author: mimig
ms.openlocfilehash: 091446fd45b09913dee70dbb4c7e5ebbca02819b
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="azure-cosmos-db-faq"></a>Veelgestelde vragen over Azure Cosmos DB
## <a name="azure-cosmos-db-fundamentals"></a>Grondbeginselen van Azure DB Cosmos
### <a name="what-is-azure-cosmos-db"></a>Wat is Azure Cosmos DB?
Azure Cosmos-database is een wereldwijd gerepliceerde en modellen database-service waarmee die biedt uitgebreide query over gegevens zonder schema configureerbare en betrouwbare prestaties leveren en maakt snelle ontwikkeling. Dit wordt bereikt door middel van een beheerd platform dat wordt ondersteund door de kracht en het bereik van Microsoft Azure. 

Azure Cosmos-database is de juiste oplossing voor web-, gaming- en IoT-toepassingen wanneer voorspelbare doorvoer, hoge beschikbaarheid, lage latentie en een gegevensmodel zonder schema belangrijke vereisten zijn. Levert schemaflexibiliteit en geavanceerde indexeren en transactionele ondersteuning voor meerdere documenten met geïntegreerde JavaScript bevat. 

Zie voor meer databasevragen, antwoorden en instructies voor het implementeren en gebruiken van deze service, de [documentatiepagina voor Azure Cosmos DB](https://azure.microsoft.com/documentation/services/cosmos-db/).

### <a name="what-happened-to-documentdb"></a>Wat is er gebeurd met DocumentDB?
De DocumentDB-API is een van de ondersteunde API's en gegevensmodellen voor Azure Cosmos DB. Bovendien ondersteunt Azure Cosmos DB u met Graph API (Preview), tabel-API (Preview) en MongoDB-API. Zie voor meer informatie [vragen van klanten DocumentDB](#moving-to-cosmos-db).

### <a name="how-do-i-get-to-my-documentdb-account-in-the-azure-portal"></a>Hoe krijg ik mijn DocumentDB-account in de Azure portal
Klik op het pictogram Azure Cosmos DB in het linkerdeelvenster in de Azure portal. Als u een DocumentDB-account voordat had, hebt u nu een Azure DB die Cosmos-account en uw facturering niet gewijzigd.

### <a name="what-are-the-typical-use-cases-for-azure-cosmos-db"></a>Wat zijn typische gebruiksvoorbeelden voor Azure Cosmos DB?
Azure Cosmos-database is een goede keuze voor nieuwe web, mobiel, games, en IoT-toepassingen waarbij automatisch schalen, voorspelbare prestaties, volgorde van milliseconde responstijden en de mogelijkheid te query snel gegevens zonder schema is belangrijk. Azure Cosmos DB gepaard met snelle ontwikkeling en de continue herhaling van toepassingsgegevensmodellen ondersteunen. Toepassingen die door gebruikers gegenereerde inhoud en gegevens beheren zijn [algemene gebruiksvoorbeelden voor Azure Cosmos DB](use-cases.md). 

### <a name="how-does-azure-cosmos-db-offer-predictable-performance"></a>Hoe biedt Azure Cosmos DB voorspelbare prestaties?
Een [aanvraag eenheid](request-units.md) (RU) is de mate van doorvoer in Azure Cosmos DB. Een doorvoer 1 RU komt overeen met de doorvoer van een document van 1 KB op te HALEN. Elke bewerking in Azure Cosmos DB, met inbegrip van leesbewerkingen, schrijfbewerkingen, SQL-query's en opgeslagen procedure-uitvoeringen, heeft een deterministische RU-waarde die gebaseerd op de doorvoer die is vereist om de bewerking te voltooien. In plaats van het denken over CPU, IO, en geheugen en hoe deze elke doorvoer van uw toepassing beïnvloeden, kunt u denken in termen van een enkele RU-meting.

U kunt elke Azure DB die Cosmos-container met ingerichte doorvoer in termen van RUs aan doorvoer per seconde reserveren. Voor toepassingen van elke schaal, kunt u afzonderlijke aanvragen voor het meten van de waarden ervan RU benchmark en inrichten van een container voor het afhandelen van het totale aantal aanvraageenheden voor alle aanvragen. U kunt ook opschalen of terugschroeven doorvoer van uw container naarmate de behoeften van uw toepassing veranderen. Voor meer informatie over aanvraageenheden en hulp bij het bepalen van de container moet, Zie [schatten doorvoerbehoeften](request-units.md#estimating-throughput-needs) en probeer de [doorvoer Rekenmachine](https://www.documentdb.com/capacityplanner). De term *container* verwijst hier naar verwijst naar een verzameling in DocumentDB-API, Graph API grafiek, MongoDB-API-verzameling en tabel API-tabel. 

### <a name="how-does-azure-cosmos-db-support-various-data-models-such-as-keyvalue-columnar-document-and-graph"></a>Hoe ondersteunt Azure Cosmos DB verschillende gegevensmodellen zoals sleutel/waarde, kolommen, document en grafiek?

Sleutel/waarde van (tabel), kolommen, documenteren en grafiekgegevens modellen zijn alle standaard ondersteund vanwege de ARS (atomen, registreert en reeksen) die Cosmos Azure DB ontwerp is gebaseerd op. Atomen, records en reeksen kunnen eenvoudig worden toegewezen en toegewezen aan verschillende gegevensmodellen. De API's voor een subset van modellen beschikbaar rechts nu (DocumentDB, MongoDB, tabel en Graph API's) en anderen die specifiek zijn voor modellen van aanvullende gegevens in de toekomst beschikbaar zullen zijn.

Azure Cosmos-database heeft een schema agnostisch indexing-engine geschikt voor automatisch indexeren van alle gegevens die deze opgenomen zonder dat een schema of secundaire indexen van de ontwikkelaar. De engine wordt gebruikgemaakt van een set van logische index-indelingen (omgekeerde, kolommen, structuur) die in geval van de opslagindeling van de index en queryverwerking subsystemen ontkoppelt. Cosmos DB heeft ook de mogelijkheid te ondersteunen van een set API's en kabel protocollen op een uitbreidbare manier en efficiënt te vertalen naar het gegevensmodel core (1) en de logische index-indelingen (2) zodat u een unieke kunnen meerdere gegevensmodellen systeemeigen ondersteunen.

### <a name="is-azure-cosmos-db-hipaa-compliant"></a>Is Azure Cosmos DB HIPAA compatibel?
Ja, Azure Cosmos DB HIPAA-compatibel is. HIPAA stelt vereisten vast voor het gebruik, de openbaarmaking en de beveiliging van individueel identificeerbare gezondheidsinformatie. Zie [Microsoft Trust Center](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA) voor meer informatie.

### <a name="what-are-the-storage-limits-of-azure-cosmos-db"></a>Wat zijn de opslaglimieten van Azure DB die Cosmos?
Er is geen limiet voor de totale hoeveelheid gegevens die een container kunt opslaan in Azure Cosmos DB.

### <a name="what-are-the-throughput-limits-of-azure-cosmos-db"></a>Wat zijn de doorvoerlimieten van Azure DB die Cosmos?
Er is geen limiet voor de totale hoeveelheid doorvoer die een container in Azure Cosmos DB kan ondersteunen. Het belangrijkste idee is dat uw werkbelasting verdelen ongeveer een voldoende aantal partitiesleutels.

### <a name="how-much-does-azure-cosmos-db-cost"></a>Wat kost Azure Cosmos DB?
Raadpleeg voor meer informatie, de [Azure Cosmos DB prijsinformatie](https://azure.microsoft.com/pricing/details/cosmos-db/) pagina. Azure DB Cosmos gebruikskosten worden bepaald door het aantal ingerichte containers, het aantal uren de containers online zijn en de ingerichte doorvoer voor elke container. De term *containers* verwijst hier naar de API van de DocumentDB-verzameling, Graph API grafiek, MongoDB-API-verzameling en tabel API tabellen. 

### <a name="is-a-free-account-available"></a>Is een gratis account beschikbaar?
Ja, kunt u aanmelden voor een account met beperkte gratis met geen toezegging. Als u wilt aanmelden, gaat u naar [probeert Azure Cosmos DB gratis](https://azure.microsoft.com/try/cosmosdb/) of meer informatie in de [probeer Cosmos DB Veelgestelde vragen over Azure](#try-cosmos-db).

Als u niet bekend met Azure bent, kunt u zich aanmelden voor een [gratis Azure-account](https://azure.microsoft.com/free/), u hebt dan 30 dagen en en een creditcard om te proberen alle Azure-services. Als u een Visual Studio-abonnement hebt, bent u ook in aanmerking komen voor [gratis Azure-tegoed](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) moet worden gebruikt voor een Azure-service. 

U kunt ook de [Azure Cosmos DB Emulator](local-emulator.md) te ontwikkelen en testen van de toepassing lokaal voor gratis, zonder te maken van een Azure-abonnement. Wanneer u tevreden bent over hoe uw toepassing in de Azure-Emulator Cosmos DB werkt, kunt u overschakelen naar het met een Azure DB die Cosmos-account in de cloud.

### <a name="how-can-i-get-additional-help-with-azure-cosmos-db"></a>Hoe kan ik aanvullende ondersteuning voor Azure Cosmos DB krijgen?
Als u hulp nodig hebt, bereiken ons op [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-cosmosdb) of de [MSDN-forum](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=AzureDocumentDB), of een gesprek voert chatten met het technische team van Azure DB die Cosmos plannen door het verzenden van e-mail naar [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com). 

<a id="try-cosmos-db"></a>
## <a name="try-azure-cosmos-db-subscriptions"></a>Probeer Azure DB die Cosmos-abonnementen

U kunt nu profiteren van een tijdelijke Azure DB die Cosmos-ervaring zonder een abonnement, gratis en verplichtingen. Als u wilt aanmelden voor een abonnement probeert Azure Cosmos DB, gaat u naar [gratis Azure Cosmos DB probeer](https://azure.microsoft.com/try/cosmosdb/). Dit abonnement is gescheiden van de [gratis proefversie van Azure](https://azure.microsoft.com/free/), en kan worden gebruikt naast een gratis proefversie van Azure of een Azure betaald abonnement. 

Probeer Azure Cosmos DB abonnementen weergegeven in de Azure portal naast andere abonnementen die zijn gekoppeld aan uw gebruikers-ID. 

De volgende voorwaarden van toepassing op Azure Cosmos-DB probeer abonnementen:

* Een container per abonnement voor SQL (DocumentDB-API), Gremlin (Graph API) en accounts van de tabel-API.
* Maximaal 3 verzamelingen per abonnement voor MongoDB-accounts.
* 10 GB opslagcapaciteit.
* Globale replicatie is beschikbaar in de volgende [Azure-regio's](https://azure.microsoft.com/regions/): Centraal VS, Noord-Europa en Azië en Stille Oceaan Zuidoost
* Maximale doorvoer van 5 K RU/s.
* Abonnementen vervalt na 24 uur, en kunnen worden uitgebreid tot maximaal 48 uur totale.
* Ondersteuning van Azure-tickets kunnen niet worden gemaakt voor accounts probeert Azure Cosmos DB; ondersteuning is echter beschikbaar voor abonnees met bestaande ondersteuningsplannen. 

## <a name="set-up-azure-cosmos-db"></a>Instellen van Azure DB die Cosmos
### <a name="how-do-i-sign-up-for-azure-cosmos-db"></a>Hoe meld ik me voor Azure Cosmos DB?
Azure Cosmos DB is beschikbaar in de Azure-portal. Eerst zich registreren voor een Azure-abonnement. Nadat u zich hebt aangemeld, kunt u een DocumentDB-API, Graph-API (Preview), tabel-API (Preview) of MongoDB-API-account toevoegen aan uw Azure-abonnement.

### <a name="what-is-a-master-key"></a>Wat is een hoofdsleutel?
Een hoofdsleutel is een beveiligingstoken voor toegang tot alle resources voor een account. Personen met de sleutel hebt gelezen en schrijftoegang tot alle resources in het account van de database. Wees voorzichtig met het distribueren van hoofdsleutels. De hoofdsleutel van de primaire en secundaire hoofdsleutel zijn beschikbaar op de **sleutels** blade van de [Azure-portal][azure-portal]. Zie voor meer informatie over sleutels [weergeven, kopiëren en opnieuw genereren toegangstoetsen](manage-account.md#keys).

### <a name="what-are-the-regions-that-preferredlocations-can-be-set-to"></a>Wat zijn de gebieden die PreferredLocations kan worden ingesteld op? 
De waarde PreferredLocations kan worden ingesteld met een van de Azure-regio's waarin Cosmos DB beschikbaar is. Zie voor een lijst met beschikbare regio's, [Azure-regio's](https://azure.microsoft.com/regions/).

### <a name="is-there-anything-i-should-be-aware-of-when-distributing-data-across-the-world-via-the-azure-datacenters"></a>Is er iets die ik houden moet rekening bij het distribueren van gegevens over de hele wereld via de Azure-datacenters? 
Azure Cosmos DB aanwezig is in alle Azure-regio's, zoals opgegeven op de [Azure-regio's](https://azure.microsoft.com/regions/) pagina. Omdat het de Kernservice, heeft elke nieuwe datacenter de aanwezigheid van een Azure Cosmos DB. 

Vergeet niet dat Azure Cosmos DB soevereine en government clouds respecteert bij het instellen van een regio. Dat wil zeggen, als u een account in een soevereine regio maken, niet te repliceren buiten die soevereine regio. U kunt replicatie naar andere locaties soevereine van een externe account op deze manier niet inschakelen. 

## <a name="develop-against-the-documentdb-api"></a>Ontwikkelen met de DocumentDB-API

### <a name="how-do-i-start-developing-against-the-documentdb-api"></a>Hoe start ontwikkelen van toepassingen met de API DocumentDB?
DocumentDB-API van Microsoft is beschikbaar in de [Azure-portal][azure-portal]. Eerst moet u aanmelden voor een Azure-abonnement. Zodra u zich voor een Azure-abonnement aanmelden, kunt u DocumentDB-API-container toevoegen aan uw Azure-abonnement. Zie voor instructies over het toevoegen van een account voor Azure Cosmos DB [Azure DB die Cosmos-databaseaccount maken](create-documentdb-dotnet.md#create-account). Als u een DocumentDB-account in het verleden waren, hebt u nu een Cosmos-DB Azure-account. 

Er zijn [SDK's](documentdb-sdk-dotnet.md) beschikbaar voor .NET, Python, Node.js, JavaScript en Java. Ontwikkelaars kunnen ook gebruikmaken van de [RESTful HTTP API's](/rest/api/documentdb/) om te communiceren met Azure Cosmos DB resources uit verschillende platforms en talen.

### <a name="can-i-access-some-ready-made-samples-to-get-a-head-start"></a>Ik toegang tot de vooraf gedefinieerde voorbeelden voor een goed uitgangspunt?
Voorbeelden voor de API DocumentDB [.NET](documentdb-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md), en [Python](documentdb-python-samples.md) SDK's zijn beschikbaar op GitHub.


### <a name="does-the-documentdb-api-database-support-schema-free-data"></a>De database van de DocumentDB-API biedt ondersteuning voor gegevens zonder schema?
Ja, de DocumentDB-API kan voor toepassingen om op te slaan zonder schemadefinities of hints willekeurige JSON-documenten. Gegevens zijn onmiddellijk beschikbaar voor de query via de interface Azure Cosmos DB SQL-query.  

### <a name="does-the-documentdb-api-support-acid-transactions"></a>De DocumentDB-API biedt ondersteuning voor ACID-transactions?
Ja, de DocumentDB-API biedt ondersteuning voor transacties van tussen meerder documenten uitgedrukt in JavaScript opgeslagen procedures en triggers. Transacties worden binnen het bereik van één partitie binnen elke verzameling en uitgevoerd met ACID-semantiek als 'Alles of niets,' geïsoleerd van andere code en gebruikersaanvragen aanvragen voor het gelijktijdig worden uitgevoerd. Als er uitzonderingen worden veroorzaakt door de server-side '-uitvoering van JavaScript-toepassingscode, de hele transactie teruggedraaid. Zie voor meer informatie over transacties [Database programmatransacties](programming.md#database-program-transactions).

### <a name="what-is-a-collection"></a>Wat is een verzameling?
Een verzameling is een groep van documenten en hun bijbehorende JavaScript-toepassingslogica. Een verzameling is een factureerbare entiteit waar de [kosten](performance-levels.md) wordt bepaald door de doorvoer en opslag gebruikt. Verzamelingen kunnen een of meer partities of servers omvatten en kunnen worden geschaald voor vrijwel onbeperkte hoeveelheid opslag of doorvoer.

Verzamelingen zijn ook de factureringsentiteiten voor Azure Cosmos DB. Elke verzameling wordt per uur gefactureerd op basis van de ingerichte doorvoer en opslagruimte gebruikt. Zie voor meer informatie [prijzen van Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/). 

### <a name="how-do-i-create-a-database"></a>Hoe maak ik een database?
U kunt databases maken met behulp van de [Azure-portal](https://portal.azure.com), zoals beschreven in [een verzameling toevoegen](create-documentdb-dotnet.md#create-collection), wordt er één van de [Azure Cosmos DB SDK's](documentdb-sdk-dotnet.md), of de [REST-API's](/rest/api/documentdb/). 

### <a name="how-do-i-set-up-users-and-permissions"></a>Hoe stel ik gebruikers en machtigingen in?
U kunt gebruikers en machtigingen maken met een van de [Cosmos DB API SDK's](documentdb-sdk-dotnet.md) of de [REST-API's](/rest/api/documentdb/).  

### <a name="does-the-documentdb-api-support-sql"></a>De DocumentDB-API biedt ondersteuning voor SQL?
De SQL-querytaal is een uitgebreide subset van de queryfunctionaliteit die wordt ondersteund door SQL. De Azure Cosmos DB SQL-querytaal biedt geavanceerde hiërarchische en relationele operators en uitbreidingsmogelijkheden via op basis van JavaScript, door gebruiker gedefinieerde functies (UDF's). JSON-grammatica biedt de mogelijkheid voor het JSON-documenten te modelleren als structuren met gelabelde knooppunten, die worden gebruikt door zowel de Azure DB die Cosmos automatische indexering technieken en de SQL-querydialect van Azure Cosmos-database. Zie voor meer informatie over het gebruik van SQL-grammatica de [QueryDocumentDB] [ query] artikel.

### <a name="does-the-documentdb-api-support-sql-aggregation-functions"></a>De DocumentDB-API biedt ondersteuning voor SQL-aggregatiefuncties?
De DocumentDB-API ondersteunt lage latentie aggregatie op elke schaal via statistische functies `COUNT`, `MIN`, `MAX`, `AVG`, en `SUM` via de SQL-grammatica. Zie voor meer informatie [statistische functies](documentdb-sql-query.md#Aggregates).

### <a name="how-does-the-documentdb-api-provide-concurrency"></a>Hoe biedt de API DocumentDB gelijktijdigheid?
De DocumentDB-API biedt ondersteuning voor Optimistisch gelijktijdigheidbeheer (OCC) via HTTP-entity-tags of ETags. Elke DocumentDB-API-resource heeft een ETag en de ETag is ingesteld op de server telkens wanneer een document wordt bijgewerkt. De ETag-header en de huidige waarde worden opgenomen in alle antwoordberichten. ETags kan worden gebruikt met de header If-Match waardoor de server om te bepalen of een resource moet worden bijgewerkt. De If-Match-waarde is de ETag-waarde gecontroleerd. Als de ETag-waarde overeenkomt met de ETag-waarde van de server, wordt de bron bijgewerkt. Als de ETag niet meer actueel is, de server weigert het opnieuw met een ' HTTP 412 Precondition failure ' antwoordcode. De client haalt vervolgens opnieuw de resource te verkrijgen van de huidige ETag-waarde voor de resource. ETags kan bovendien met de header If-None-Match worden gebruikt om te bepalen of een opnieuw ophalen van een bron die nodig is.

Optimistische gelijktijdigheid in .NET, gebruikt de [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) klasse. Zie voor een voorbeeld .NET [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) in de steekproef DocumentManagement op GitHub.

### <a name="how-do-i-perform-transactions-in-the-documentdb-api"></a>Hoe voer ik transacties in de API DocumentDB uit?
De DocumentDB-API biedt ondersteuning voor taalgeïntegreerde transacties via in JavaScript opgeslagen procedures en triggers. Alle databasebewerkingen in scripts worden uitgevoerd onder snapshot-isolatie. Als dit een verzameling van één partitie is, wordt de uitvoering is afgestemd op de verzameling. Als de verzameling is gepartitioneerd, wordt de uitvoering is afgestemd op documenten met dezelfde partitiesleutel waarde binnen de verzameling. Er wordt aan het begin van de transactie een schermopname van de documentversies (ETags) gemaakt en de transactie wordt alleen uitgevoerd als het script kan worden uitgevoerd. Als JavaScript een fout genereert, wordt de transactie teruggedraaid. Zie voor meer informatie [serverzijde JavaScript programmeren voor Azure Cosmos DB](programming.md).

### <a name="how-can-i-bulk-insert-documents-into-cosmos-db"></a>Hoe kan ik bulksgewijs invoegen documenten naar Cosmos DB?
U kunt bulksgewijs invoegen documenten naar Azure Cosmos DB op twee manieren:

* Het hulpprogramma voor migratie, zoals beschreven in [hulpprogramma voor migratie van Database voor Azure Cosmos DB](import-data.md).
* Opgeslagen procedures, zoals beschreven in [serverzijde JavaScript programmeren voor Azure Cosmos DB](programming.md).

### <a name="does-the-documentdb-api-support-resource-link-caching"></a>Ondersteunt de DocumentDB-API ondersteuning resourcekoppelingen?
Ja, omdat Azure Cosmos DB een RESTful-service, resourcekoppelingen onveranderbaar zijn en kunnen in de cache worden opgeslagen. DocumentDB-API-clients kunnen een header 'If-None-Match' opgeven voor leesbewerkingen voor een resource-achtige document of een verzameling en vervolgens hun lokale kopieën werken nadat de serverversie is gewijzigd.

### <a name="is-a-local-instance-of-documentdb-api-available"></a>Is een lokaal exemplaar van DocumentDB API beschikbaar?
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
| ExceededMemoryLimit | 16501 | Als een multitenant-service heeft de bewerking van de client geheugen aandeel overschreden. | Verminder het bereik van de bewerking via de meest beperkende querycriteria of neem contact op met ondersteuning van de [Azure-portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). <br><br>Voorbeeld:  *&nbsp; &nbsp; &nbsp; &nbsp;db.getCollection('users').aggregate ([<br> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;{$match: {naam: 'Andy'}}, <br> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;{$sort: {leeftijd: -1} }<br>&nbsp;&nbsp;&nbsp;&nbsp;])*) |

## <a name="develop-with-the-table-api-preview"></a>Ontwikkelen met de tabel-API (Preview)

### <a name="terms"></a>Voorwaarden 
De API van Azure Cosmos DB tabel (Preview) verwijst naar de premium Azure Cosmos DB aanbieding voor een tabel-gegevensmodel aangekondigd op Build 2017. 

### <a name="how-can-i-use-the-new-table-api-preview-offering"></a>Hoe kan ik de nieuwe tabel-API (Preview) aanbieding gebruiken? 
De Azure-API voor tabel Cosmos DB is beschikbaar in de [Azure-portal][azure-portal]. Eerst moet u aanmelden voor een Azure-abonnement. Nadat u zich hebt aangemeld, kunt u een tabel-API van Azure Cosmos DB-account toevoegen aan uw Azure-abonnement en tabellen vervolgens toe te voegen aan uw account. 

Tijdens de evaluatieperiode wanneer [SDK's](../cosmos-db/table-sdk-dotnet.md) zijn beschikbaar voor .NET, u kunt aan de slag met het voltooien van de [tabel API](../cosmos-db/create-table-dotnet.md) snel starten-artikel.

### <a name="do-i-need-a-new-sdk-to-use-the-table-api-preview"></a>Moet ik een nieuwe SDK gebruik van de tabel-API (Preview)? 
Ja, de [Windows Azure-opslag Premium Table (Preview) SDK](https://www.nuget.org/packages/WindowsAzure.Storage-PremiumTable) beschikbaar is in NuGet en is vereist voor het gebruik van de Azure-API voor tabel Cosmos DB. Aanvullende informatie is beschikbaar op de [Azure Cosmos DB tabel .NET API: downloaden en opmerkingen bij de release](https://github.com/Microsoft/azure-docs-pr/cosmos-db/table-sdk-dotnet.md) pagina. 

### <a name="how-do-i-provide-feedback-about-the-sdk-or-bugs"></a>Hoe ik feedback geven over de SDK of fouten?
U kunt uw feedback delen in een van de volgende manieren:

* [Uservoice](https://feedback.azure.com/forums/599062-azure-cosmos-db-table-api)
* [MSDN-forum](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=AzureDocumentDB)
* [StackOverflow](http://stackoverflow.com/questions/tagged/azure-cosmosdb)

### <a name="what-is-the-connection-string-that-i-need-to-use-to-connect-to-the-table-api-preview"></a>Wat is de verbindingsreeks die ik gebruiken moet voor verbinding met de tabel-API (Preview)?
De verbindingsreeks is:
```
DefaultEndpointsProtocol=https;AccountName=<AccountNamefromCosmos DB;AccountKey=<FromKeysPaneofCosmosDB>;TableEndpoint=https://<AccountNameFromDocumentDB>.documents.azure.com
```
U kunt de verbindingsreeks ophalen via de pagina sleutels in de Azure-portal. 

### <a name="how-do-i-override-the-config-settings-for-the-request-options-in-the-new-table-api-preview"></a>Hoe ik de configuratie-instellingen voor de aanvraag-opties in de nieuwe tabel-API (Preview) overschrijven?
Zie voor informatie over configuratie-instellingen, [mogelijkheden van Azure DB die Cosmos](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). U kunt de instellingen wijzigen door ze toe te voegen aan app.config in de sectie appSettings in de clienttoepassing.

    <appSettings>
        <add key="TableConsistencyLevel" value="Eventual|Strong|Session|BoundedStaleness|ConsistentPrefix"/>
        <add key="TableThroughput" value="<PositiveIntegerValue"/>
        <add key="TableIndexingPolicy" value="<jsonindexdefn>"/>
        <add key="TableUseGatewayMode" value="True|False"/>
        <add key="TablePreferredLocations" value="Location1|Location2|Location3|Location4>"/>....
    </appSettings>


### <a name="are-there-any-changes-for-customers-who-are-using-the-existing-azure-table-storage-sdk"></a>Zijn er wijzigingen voor klanten die de bestaande Azure Table-opslag-SDK gebruiken?
Geen. Er zijn geen wijzigingen voor bestaande of nieuwe klanten die de bestaande Azure Table-opslag-SDK's. 

### <a name="how-do-i-view-table-data-that-is-stored-in-azure-cosmos-db-for-use-with-the-table-api-review"></a>Hoe kan ik gegevens in een tabel die is opgeslagen in Azure Cosmos DB voor gebruik met de tabel-API (revisie) bekijken? 
De Azure-portal kunt u de gegevens bladeren. U kunt ook de code van de tabel-API (Preview) of de hulpprogramma's in het volgende antwoord genoemd. 

### <a name="which-tools-work-with-the-table-api-preview"></a>Welke hulpprogramma's voor gebruik met de tabel-API (Preview)? 
U kunt de oudere versie van Azure Explorer (0.8.9) gebruiken.

Hulpprogramma's voor de flexibiliteit om op te nemen van een verbindingsreeks in de opgegeven indeling kunnen eerder de nieuwe tabel-API (Preview) ondersteunen. Een lijst met hulpprogramma's voor tabel vindt u op de [hulpprogramma's voor Azure Storage Client](../storage/common/storage-explorers.md) pagina. 

### <a name="do-powershell-or-azure-cli-work-with-the-new-table-api-preview"></a>PowerShell of Azure CLI werken met de nieuwe tabel-API (Preview)?
We zullen toevoegen van ondersteuning voor PowerShell en Azure CLI voor tabel-API (Preview). 

### <a name="is-the-concurrency-on-operations-controlled"></a>Is de concurrency van bewerkingen die worden beheerd?
Ja, optimistische gelijktijdigheid van taken wordt opgegeven via het gebruik van het mechanisme voor ETag. 

### <a name="is-the-odata-query-model-supported-for-entities"></a>Wordt het OData-query-model voor entiteiten ondersteund? 
Ja, de tabel-API (Preview) ondersteunt de OData-query en LINQ-query. 

### <a name="can-i-connect-to-the-azure-table-storage-and-the-azure-cosmos-db-table-api-preview-side-by-side-in-the-same-application"></a>Kan ik de Azure Table storage en de Azure Cosmos DB tabel-API (Preview) naast elkaar in dezelfde toepassing verbinden? 
Ja, u kunt verbinding maken met het maken van twee afzonderlijke exemplaren van de CloudTableClient, die elk een eigen URI via de verbindingsreeks te wijzen.

### <a name="how-do-i-migrate-an-existing-azure-table-storage-application-to-this-new-offering"></a>Hoe Migreer ik een bestaande Azure Table storage-toepassing op deze nieuwe aanbieding
Om te profiteren van de nieuwe Azure Cosmos DB tabel API-aanbieding voor uw bestaande gegevens van de tabel-opslag, neem contact op met [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com). 

### <a name="what-is-the-roadmap-for-this-service-and-when-will-you-offer-other-standard-table-api-functionality"></a>Wat de roadmap voor deze service is en wanneer bieden u andere standaard tabel API-functies?
Ondersteuning voor de SAS-tokens, ServiceContext, statistieken, Client-side versleuteling, Analytics en andere functies toegevoegd als we gaan naar de algemene beschikbaarheid. U kunt feedback op [Uservoice](https://feedback.azure.com/forums/599062-azure-cosmos-db-table-api). 

### <a name="how-is-expansion-of-the-storage-size-done-for-this-service-if-for-example-i-start-with-n-gb-of-data-and-my-data-will-grow-to-1-tb-over-time"></a>Wat is het uitbreiden van de grootte van de opslagruimte voor deze service wordt uitgevoerd als er bijvoorbeeld wordt gestart met  *n*  GB aan gegevens en Mijn gegevens na verloop van tijd tot 1 TB zal toenemen? 
Azure Cosmos DB is zodanig ontworpen dat onbeperkte opslag via het gebruik van horizontaal schalen. De service kunt bewaken en effectief meer opslagruimte. 

### <a name="how-do-i-monitor-the-table-api-preview-offering"></a>Hoe bewaak ik het aanbod van de tabel-API (Preview)
U kunt de tabel-API (Preview) **metrische gegevens** deelvenster in de Azure portal om aanvragen en opslaggebruik te bewaken. 

### <a name="how-do-i-calculate-the-throughput-i-require"></a>Hoe bereken ik de doorvoer die ik nodig?
U kunt de capaciteit estimator gebruiken voor het berekenen van de TableThroughput die zijn voor de bewerkingen vereist. Zie voor meer informatie [schatting Aanvraageenheden en gegevensopslag](https://www.documentdb.com/capacityplanner). In het algemeen kunt u uw entiteit als JSON vertegenwoordigen en de getallen opgeven voor uw bewerkingen. 

### <a name="can-i-use-the-new-table-api-preview-sdk-locally-with-the-emulator"></a>Kan ik de nieuwe tabel-API (Preview) SDK lokaal in de emulator gebruiken?
Ja, kunt u de tabel-API (Preview) met de lokale emulator wanneer u de nieuwe SDK gebruiken. Nieuwe emulator downloaden, gaat u naar [de Emulator Azure Cosmos DB gebruiken voor lokale ontwikkeling en tests](local-emulator.md). De waarde StorageConnectionString in app.config moet:

```
DefaultEndpointsProtocol=https;AccountName=localhost;AccountKey=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==;TableEndpoint=https://localhost:8081`. 
```

### <a name="can-my-existing-azure-table-storage-application-work-with-the-table-api-preview"></a>Kan mijn bestaande Azure Table storage-toepassing werken met de tabel-API (Preview)? 
Het oppervlak van de nieuwe tabel-API (Preview) is compatibel met de bestaande Azure Table-opslag SDK over het maken, verwijderen, bijwerken en querybewerkingen in de .NET API. Zorg ervoor dat u een rijsleutel, omdat de tabel-API (Preview) is zowel een partitiesleutel als een rijsleutel vereist. We willen ook meer SDK ondersteuning, zoals we gaan naar GA van deze serviceaanbiedingen toevoegen.

### <a name="do-i-need-to-migrate-my-existing-azure-table-storage-applications-to-the-new-sdk-if-i-do-not-want-to-use-the-table-api-preview-features"></a>Moet ik mijn bestaande Azure Table storage toepassingen naar de nieuwe SDK gemigreerd als ik wil niet de functies van de tabel-API (Preview) te gebruiken?
Nee, kunt u maken en gebruiken van bestaande Azure Table storage activa zonder onderbreking van elk type. Als u niet de nieuwe tabel-API (Preview) gebruikt, kunt u kan geen echter voor profiteren van de automatische index, extra consistentie optie of distributielijsten. 

### <a name="how-do-i-add-replication-of-the-data-in-the-table-api-preview-across-multiple-regions-of-azure"></a>Hoe voeg ik replicatie van de gegevens in de tabel-API (Preview) over meerdere regio's van Azure?
U kunt de Azure DB die Cosmos-portal [globale replicatie-instellingen](tutorial-global-distribution-documentdb.md#portal) regio's die geschikt voor uw toepassing zijn toevoegen. Voor het ontwikkelen van een globaal gedistribueerde toepassing, moet u ook uw toepassing toevoegen met de informatie PreferredLocation is ingesteld op de lokale regio voor het ontwikkelen van de lage latentie voor lezen. 

### <a name="how-do-i-change-the-primary-write-region-for-the-account-in-the-table-api-preview"></a>Hoe wijzig ik de regio primaire schrijven voor het account in de tabel-API (Preview)
Het Azure Cosmos DB globale replicatie portal deelvenster kunt u een regio toevoegen en vervolgens een failover uitvoeren naar de vereiste regio. Zie voor instructies [ontwikkelen met meerdere landen/regio Azure Cosmos DB accounts](regional-failover.md). 

### <a name="how-do-i-configure-my-preferred-read-regions-for-low-latency-when-i-distribute-my-data"></a>Hoe kan ik mijn voorkeur lezen regio's voor de lage latentie configureren wanneer ik mijn gegevens distribueert? 
Om te lezen van de lokale locatie, gebruikt u de sleutel PreferredLocation in het bestand app.config. Voor bestaande toepassingen wordt in de tabel-API (Preview) een fout genereert als LocationMode is ingesteld. Deze code niet verwijderen omdat de premium tabel-API (Preview) neemt over deze informatie in het bestand app.config. Zie voor meer informatie [mogelijkheden van Azure DB die Cosmos](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="how-should-i-think-about-consistency-levels-in-the-table-api-preview"></a>Hoe moet ik denken dat zij consistentieniveaus in de tabel-API (Preview)? 
Azure Cosmos DB biedt goed gemotiveerd verschillen tussen de consistentie, beschikbaarheid en latentie. Azure Cosmos DB biedt vijf consistentieniveaus voor tabel-API (Preview) ontwikkelaars, zodat u kunt kiezen voor het model van de juiste consistentiecontrole op het tabelniveau van de en afzonderlijke aanvragen maken tijdens het doorzoeken van de gegevens. Wanneer een client verbinding maakt, kunt het niveau van een consistentiecontrole opgeven. U kunt het niveau via het app.config-instelling voor de waarde van de sleutel TableConsistencyLevel wijzigen. 

De tabel-API (Preview) biedt lage latentie leest met 'lezen uw eigen schrijfbewerkingen' met sessieconsistentie als standaardwaarde. Zie voor meer informatie [consistentieniveaus](consistency-levels.md). 

Azure Table storage biedt standaard sterke consistentie binnen een regio en Eventual consistentie in de secundaire locaties. 

### <a name="does-the-azure-cosmos-db-table-api-offer-more-consistency-levels-than-azure-table-storage"></a>Biedt de Azure-API voor tabel Cosmos DB meer consistentieniveaus dan Azure Table storage?
Ja, voor informatie over hoe u kunt profiteren van de gedistribueerde aard van Azure Cosmos DB, Zie [consistentieniveaus](consistency-levels.md). Omdat de garanties zijn opgegeven voor de consistentieniveaus, kunt u ze kunt gebruiken met vertrouwen. Zie voor meer informatie [mogelijkheden van Azure DB die Cosmos](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="when-global-distribution-is-enabled-how-long-does-it-take-to-replicate-the-data"></a>Wanneer de algemene distributie is ingeschakeld, hoe lang duurt om de gegevens te repliceren?
Azure Cosmos DB voert de gegevens in de lokale regio blijvend en duwt de gegevens naar andere regio's binnen een paar milliseconden onmiddellijk. Deze replicatie is afhankelijk van de round trip-tijd (RTT) van het datacenter. Zie voor meer informatie over de algemene distributie-functionaliteit van Azure Cosmos DB, [Azure Cosmos DB: een wereldwijd gedistribueerde database-service op Azure](distribute-data-globally.md).

### <a name="can-the-read-request-consistency-level-be-changed"></a>Kan het niveau van de consistentie leesaanvraag worden gewijzigd?
Met Azure Cosmos DB, kunt u het consistentieniveau van de instellen op het niveau van de container (op de tabel). U kunt het niveau wijzigen door de waarde voor de sleutel in het bestand app.config TableConsistencyLevel met behulp van de SDK. De mogelijke waarden zijn: sterk, gebonden veroudering sessie, consistente voorvoegsel en Eventual. Zie voor meer informatie [gegevens instelbare consistentieniveaus in Azure Cosmos DB](consistency-levels.md). Het belangrijkste idee is dat u niet de consistentie van de aanvraag niveau op meer dan de instelling voor de tabel instellen. U kunt het consistentieniveau van de voor de tabel bijvoorbeeld niet instellen op Eventual en het niveau aanvraag consistentie van sterke. 

### <a name="how-does-the-table-api-preview-account-handle-failover-if-a-region-goes-down"></a>Hoe wordt het account van de tabel-API (Preview) verwerkt failover als een regio uitgeschakeld wordt? 
De API van Azure Cosmos DB tabel (Preview) voortbouwt op het globaal gedistribueerde Azure DB die Cosmos-platform. Inschakelen om ervoor te zorgen dat uw toepassing datacenter uitvaltijd kan tolereren, ten minste één meer regio voor het account in de portal voor Azure Cosmos DB [ontwikkelen met meerdere landen/regio Azure Cosmos DB accounts](regional-failover.md). U kunt de prioriteit van de regio instellen met behulp van de portal [ontwikkelen met meerdere landen/regio Azure Cosmos DB accounts](regional-failover.md). 

U kunt zoveel regio als u wilt gebruiken voor het account en waar deze een failover bepalen naar door te geven van een failover-prioriteit toevoegen. Voor het gebruik van de database, moet u natuurlijk er een toepassing te bieden. Wanneer u doet dit, krijgen uw klanten geen uitvaltijd. De client SDK homing auto. Dat wil zeggen, kunnen de regio die niet actief is en automatisch een failover naar de nieuwe regio worden gedetecteerd.

### <a name="is-the-table-api-preview-enabled-for-backups"></a>De tabel-API (Preview) is ingeschakeld voor back-ups
Ja, de Azure Cosmos DB tabel-API (Preview) voortbouwt op het platform van Azure Cosmos-database voor back-ups. Back-ups worden automatisch gemaakt. Zie voor meer informatie [Online back-up en herstel met Azure Cosmos DB](online-backup-and-restore.md).

 
### <a name="does-the-table-api-preview-index-all-attributes-of-an-entity-by-default"></a>De tabel-API (Preview) alle kenmerken van een entiteit index standaard
Ja, worden alle kenmerken van een entiteit geïndexeerd standaard door Azure Cosmos DB. Zie voor meer informatie [Azure Cosmos DB: beleid indexeren](indexing-policies.md). 

### <a name="does-this-mean-i-do-not-have-to-create-multiple-indexes-to-satisfy-the-queries"></a>Betekent dit dat ik heb geen meerdere indexen om te voldoen aan de query's maken? 
Ja, biedt Azure Cosmos DB automatische indexeren van alle kenmerken zonder eventuele schemadefinitie. Deze automatisering wordt vrijgemaakt ontwikkelaars zich te concentreren op de toepassing in plaats van op index maken en beheren. Zie voor meer informatie [Azure Cosmos DB: beleid indexeren](indexing-policies.md).

### <a name="can-i-change-the-indexing-policy"></a>Kan ik het indexeringsbeleid wijzigen?
Ja, kunt u het indexeringsbeleid doordat de indexdefinitie. Zie voor meer informatie [mogelijkheden van Azure DB die Cosmos](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). U moet goed coderen en escape voor de instellingen. 

Tekenreeks json-indeling in het bestand app.config:
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
Preview-versie, worden in de tabel-API dezelfde queryfunctionaliteit als Azure Table storage biedt. Azure Cosmos DB biedt ook ondersteuning voor sorteren, statistische functies, georuimtelijke query hiërarchie en een groot aantal ingebouwde functies. We bieden aanvullende functionaliteit in de tabel-API in een toekomstige service-update. Zie voor meer informatie [SQL-query's voor Azure Cosmos DB DocumentDB API](../documentdb/documentdb-sql-query.md).
 
### <a name="when-should-i-change-tablethroughput-for-the-table-api-preview"></a>Wanneer moet ik TableThroughput wijzigen voor de tabel-API (Preview)?
Wanneer een van de volgende voorwaarden van toepassing is, moet u TableThroughput wijzigen:
* U voert een uitpakken, transformeren en laden (ETL) van gegevens of u wilt uploaden van een grote hoeveelheid gegevens in korte tijd. 
* U moet meer doorvoer van de container op de back-end. Bijvoorbeeld, ziet u dat de gebruikte doorvoer meer dan de ingerichte doorvoer is en u zijn ophalen beperkt. Zie voor meer informatie [Set doorvoer voor Azure Cosmos DB containers](set-throughput.md).

### <a name="can-i-scale-up-or-scale-down-the-throughput-of-my-table-api-preview-table"></a>Kan ik opschalen of terugschroeven de doorvoer van de tabel tabel-API (Preview)? 
Ja, kunt u de Azure DB die Cosmos-portal scale deelvenster schalen van de doorvoer. Zie voor meer informatie [Set doorvoer](set-throughput.md).

### <a name="is-a-default-tablethroughput-set-for-newly-provisioned-tables"></a>Is een standaard die tablethroughput voor nieuw ingerichte tabellen ingesteld?
Ja, als u de TableThroughput via app.config niet overschrijven en gebruik niet een vooraf gemaakte container in Azure Cosmos DB, maakt de service een tabel met doorvoersnelheid van 400.
 
### <a name="is-there-any-change-of-pricing-for-existing-customers-of-azure-table-storage"></a>Is er een wijziging van de prijzen voor bestaande klanten van Azure Table storage?
Geen. Er is geen wijziging in de prijs voor bestaande klanten voor Azure Table-opslag. 

### <a name="how-is-the-price-calculated-for-the-table-api-preview"></a>Hoe wordt de prijs berekend voor de tabel-API (Preview)? 
De prijs van de API van Azure Cosmos DB tabel (Preview) is afhankelijk van de toegewezen TableThroughput. 

### <a name="how-do-i-handle-any-throttling-in-table-api-preview-offering"></a>Hoe u verwerkt beperking in de aanbieding tabel-API (Preview)? 
Als het percentage aanvragen groter is dan de capaciteit van de ingerichte doorvoer voor de onderliggende container, ontvangt u een fout opgetreden en de SDK wordt opnieuw geprobeerd het gesprek door toe te passen van het beleid voor opnieuw proberen.

### <a name="why-do-i-need-to-choose-a-throughput-apart-from-partitionkey-and-rowkey-to-take-advantage-of-the-azure-cosmos-db-table-api-preview"></a>Waarom moet ik kiezen een doorvoer naast PartitionKey en RowKey om te profiteren van de API van Azure Cosmos DB tabel (Preview)?
Als u een in het bestand app.config niet opgeeft, Azure Cosmos-DB een doorvoer standaard ingesteld voor de container. 

Azure Cosmos DB biedt garanties voor prestaties en de latentie met bovengrenzen van bewerking. Deze garantie is mogelijk wanneer de engine voor het beheer van de tenant-bewerkingen kunt afdwingen. TableThroughput instelling zorgt ervoor dat u de gegarandeerde doorvoer en latentie, omdat het platform deze capaciteit gereserveerd en wordt gegarandeerd dat operationele geslaagd. 

Met behulp van de specificatie doorvoer, kunt u deze om te profiteren van de seizoensgebonden van uw toepassing, voldoen aan de doorvoerbehoeften en kosten besparen elastisch wijzigen.

### <a name="azure-storage-sdk-has-been-very-inexpensive-for-me-because-i-pay-only-to-store-the-data-and-i-rarely-query-the-new-azure-cosmos-db-offering-seems-to-be-charging-me-even-though-i-have-not-performed-a-single-transaction-or-stored-anything-can-you-please-explain"></a>Azure-opslag-SDK is zeer goedkope voor mij, omdat ik betaalt alleen voor het opslaan van de gegevens, en ik zelden query. De nieuwe Azure Cosmos DB aanbieding lijkt te worden in rekening gebracht mij ondanks dat ik niet heb één transactie uitgevoerd of iets opgeslagen. Kunt u Leg?

Azure Cosmos DB is ontworpen om te worden van een systeem globaal gedistribueerd, op basis van een SLA met garanties met betrekking tot beschikbaarheid, latentie en doorvoer. Bij het reserveren van doorvoer in Azure DB die Cosmos is gegarandeerd, in tegenstelling tot de doorvoer van andere systemen. Azure Cosmos DB biedt aanvullende functies die klanten hebt aangevraagd, zoals secundaire indexen en distributielijsten.  

### <a name="i-never-get-a-quota-full-notification-indicating-that-a-partition-is-full-when-i-ingest-data-into-azure-table-storage-with-the-table-api-preview-i-do-get-this-message-is-this-offering-limiting-me-and-forcing-me-to-change-my-existing-application"></a>Krijg ik nooit een ' volledige ' quotamelding (waarmee wordt aangegeven dat een partitie vol is) wanneer ik opnemen van gegevens in Azure Table storage. Ik ontvangt dit bericht met de tabel-API (Preview). Dit biedt mij beperken en dwingen mij mijn bestaande toepassing aan te passen?

Azure Cosmos-database is een SLA-systeem waarmee onbeperkte scale garanties met betrekking tot latentie, doorvoer, beschikbaarheid en consistentie. Om ervoor te zorgen gegarandeerde premium prestaties, zorg ervoor dat uw gegevensgrootte en een index beheerbare en schaalbare. De limiet van 10 GB op het aantal entiteiten of items per partitiesleutel is om ervoor te zorgen dat wij goede zoekopdracht en query-prestaties. Om ervoor te zorgen dat uw toepassing geschaald ook goed voor Azure Storage, raden we u *niet* een hot partitie maken door alle informatie in een partitie op te slaan en het uitvoeren van query's. 

### <a name="so-partitionkey-and-rowkey-are-still-required-with-the-new-table-api-preview"></a>Dus PartitionKey en RowKey zijn nog steeds vereist met de nieuwe tabel-API (Preview)? 
Ja. Omdat het serveroppervlaktegebied van de tabel-API (Preview) vergelijkbaar met die van de SDK van Azure Table storage is, biedt de partitiesleutel een efficiënte manier voor het distribueren van de gegevens. De rijsleutel is uniek zijn binnen deze partitie. De rijsleutel moet aanwezig zijn en mag niet null zijn als in de standaard SDK. De lengte van RowKey is 255 bytes en de lengte van PartitionKey is 100 bytes (snel kan worden verhoogd tot 1 KB). 

### <a name="what-are-the-error-messages-for-the-table-api-preview"></a>Wat zijn de foutberichten voor de tabel-API (Preview)?
Omdat dit voorbeeld compatibel met Azure Table storage is, de meeste van de fouten zal worden toegewezen aan de fouten van de standaard tabel. 

### <a name="why-do-i-get-throttled-when-i-try-to-create-lot-of-tables-one-after-another-in-the-table-api-preview"></a>Waarom ik ophalen beperkte bandbreedte wanneer ik probeer te veel tabellen achter elkaar maken in de tabel-API (Preview)?
Azure Cosmos-database is een SLA-systeem waarmee latentie, doorvoer, beschikbaarheid en consistentie wordt gegarandeerd. Omdat het een ingerichte systeem, reserveren het van bronnen om te garanderen van deze vereisten. De snelle frequentie van het maken van tabellen is gedetecteerd en beperkt. U wordt aangeraden dat u kijken naar de frequentie van het maken van tabellen en het verlagen naar minder dan 5 per minuut. Houd er rekening mee dat de tabel-API (Preview) een ingerichte systeem is. Het moment dat u inricht, begint u betalen. 

## <a name="develop-against-the-graph-api-preview"></a>Ontwikkelen op basis van de grafiek API (Preview)
### <a name="how-can-i-apply-the-functionality-of-graph-api-preview-to-azure-cosmos-db"></a>Hoe kan ik de functionaliteit van Graph API (Preview) bij Azure Cosmos DB toepassen?
Toepassen van de functionaliteit van Graph API (Preview) kunt u een extensiebibliotheek. Deze bibliotheek Microsoft Azure-grafieken wordt genoemd, en is beschikbaar in NuGet. 

### <a name="it-looks-like-you-support-the-gremlin-graph-traversal-language-do-you-plan-to-add-more-forms-of-query"></a>Het lijkt erop dat u de taal Gremlin grafiek traversal ondersteunt. Wilt u meer formulieren van de query toevoegen?
Ja, we willen andere mechanismen voor query in de toekomst toevoegen. 

### <a name="how-can-i-use-the-new-graph-api-preview-offering"></a>Hoe kan ik de nieuwe Graph-API (Preview) aanbieding gebruiken? 
Om te beginnen, voltooi de [Graph API](../cosmos-db/create-graph-dotnet.md) snel starten-artikel.

<a id="moving-to-cosmos-db"></a>
## <a name="questions-from-documentdb-customers"></a>Vragen van klanten DocumentDB
### <a name="why-are-you-moving-to-azure-cosmos-db"></a>Waarom verplaatst u bij Azure Cosmos DB? 

Azure Cosmos-database is de volgende grote leap globaal gedistribueerde geschaald cloud-databases. Als een DocumentDB-klant hebt u nu toegang tot de innovatieve systeem en de mogelijkheden die worden aangeboden door Azure Cosmos DB.

Azure Cosmos DB gestart als 'Project Florence' in 2010 voor het oplossen van de knelpunten waarmee ontwikkelaars bij het bouwen van grootschalige toepassingen binnen Microsoft. De uitdagingen bij het bouwen van globaal gedistribueerde apps zijn niet uniek is voor Microsoft, zodat we de eerste generatie van deze technologie die beschikbaar zijn gesteld in 2015 Azure ontwikkelaars in de vorm van Azure DocumentDB. 

Sinds die tijd hebt we nieuwe functies toegevoegd en belangrijke nieuwe functies geïntroduceerd. Azure Cosmos-database is het resultaat. Als onderdeel van deze release, DocumentDB klanten met hun gegevens automatisch en naadloos worden Azure DB die Cosmos-klanten. Deze mogelijkheden zijn op het gebied van de database-engine core, evenals globale distributie, elastische schaalbaarheid en toonaangevende, uitgebreide Sla's. In het bijzonder hebben we de database-engine van Azure DB die Cosmos efficiënt alle populaire gegevensmodellen, type-systemen en API's worden toegewezen aan het onderliggende gegevensmodel van Azure DB die Cosmos ontwikkeld. 

De huidige ontwikkelaars gerichte manifest van deze taak is de nieuwe ondersteuning voor [Gremlin](../cosmos-db/graph-introduction.md) en [tabel-API's](../cosmos-db/table-introduction.md). En dit is slechts een begin. We willen andere populaire API's en nieuwere gegevensmodellen gedurende een periode met meer ontwikkelingen in prestaties en de opslag op globale schaal toevoegen. 

Het is belangrijk te weten dat de DocumentDB [SQL-dialect](../documentdb/documentdb-sql-query.md) is slechts een van de vele-API's die ondersteuning voor de onderliggende Azure Cosmos-database biedt. Voor ontwikkelaars die gebruikmaken van een volledig beheerde service zoals Azure Cosmos DB, is de enige interface met de service de API's die beschikbaar worden gesteld door de service. Niets echt voor bestaande DocumentDB-klanten wordt gewijzigd. In Azure Cosmos DB krijgt u precies de dezelfde SQL-API die DocumentDB biedt. En nu (en in de toekomst), u toegang hebt tot andere mogelijkheden van eerder niet toegankelijk 

Een ander manifest van onze blijvende werk is de uitgebreide basis voor globale en elastische schaalbaarheid van de doorvoer en opslag. Er zijn verschillende fundamentele verbeteringen aangebracht in de algemene distributie-subsysteem. Een van de vele dergelijke ontwikkelaars gerichte voorzieningen is het model consistentie Consistent voorvoegsel, waardoor een totale vijf goed gedefinieerde consistentie-modellen. We brengen veel meer interessante mogelijkheden hun. 

### <a name="what-do-i-need-to-do-to-ensure-that-my-documentdb-resources-continue-to-run-on-azure-cosmos-db"></a>Wat moet ik doen om ervoor te zorgen dat mijn DocumentDB-resources worden uitgevoerd op Azure Cosmos DB?

U moet helemaal geen wijzigingen aanbrengt. Uw DocumentDB-resources zijn nu Azure Cosmos DB bronnen en er is niets van een onderbreking in de service wanneer deze verplaatsen opgetreden.

### <a name="what-changes-do-i-need-to-make-for-my-app-to-work-with-azure-cosmos-db"></a>Welke wijzigingen moet ik voor mijn app werkt met Azure Cosmos DB aan te brengen?

Er zijn geen wijzigingen aanbrengen. Klassen, naamruimten en NuGet-pakketnamen zijn niet gewijzigd. Zoals altijd wordt aangeraden dat u de SDK's up-to-date te houden om te profiteren van de meest recente functies en verbeteringen. 

### <a name="whats-changed-in-the-azure-portal"></a>Wat er gewijzigd in de Azure portal?

DocumentDB wordt niet meer in de portal weergegeven als een Azure-service. In plaats daarvan wordt een nieuw Azure DB die Cosmos-pictogram zoals weergegeven in de volgende afbeelding. Alle collecties zijn beschikbaar, omdat deze zich vóór en u nog steeds doorvoer, consistentieniveaus wijzigen en monitor Sla's schalen kunt. De mogelijkheden van Data Explorer (Preview) zijn verbeterd. U kunt nu weergeven en bewerken van documenten, maken en query's uitvoeren en werken met opgeslagen procedures, triggers en UDF van één pagina, zoals wordt weergegeven in de volgende afbeelding: 

![De blade Azure Cosmos DB verzamelingen](./media/faq/cosmos-db-data-explorer.png)

### <a name="are-there-changes-to-pricing"></a>Zijn er wijzigingen in de prijzen?

Nee, de kosten van uw app uitgevoerd op Azure Cosmos DB hetzelfde is als deze voordat was.

### <a name="are-there-changes-to-the-slas"></a>Zijn er wijzigingen aan de serviceovereenkomsten?

Nee, de serviceovereenkomsten voor beschikbaarheid, consistentie, latentie en doorvoer blijven ongewijzigd en nog steeds worden weergegeven in de portal. Zie voor meer informatie [SLA voor Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/).
   
![To-do-app met voorbeeldgegevens](./media/faq/azure-cosmosdb-portal-metrics-slas.png)


[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md

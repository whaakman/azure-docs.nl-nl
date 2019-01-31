---
title: Optimaliseren van doorvoer kosten bij Azure Cosmos DB
description: In dit artikel wordt uitgelegd over het optimaliseren van doorvoer kosten voor de gegevens die zijn opgeslagen in Azure Cosmos DB.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: rimman
ms.openlocfilehash: 443bf5694515720b1b865c310e70ca9c45add262
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55465585"
---
# <a name="optimize-provisioned-throughput-cost-in-azure-cosmos-db"></a>Kosten van de ingerichte doorvoer in Azure Cosmos DB optimaliseren

Model van de ingerichte doorvoer biedt en biedt Azure Cosmos DB voorspelbare prestaties op elke schaal. Elimineert het 'noisy neighbor-effect' op de prestaties van uw reservering of doorvoer vooraf inrichten. U geeft de exacte hoeveelheid doorvoer die u nodig en Azure Cosmos DB garandeert voor de geconfigureerde doorvoer, ondersteund door een SLA.

U kunt beginnen met een minimale doorvoercapaciteit van 400 RU/sec. en schaal tientallen miljoenen aanvragen per seconde of zelfs meer. Elke aanvraag die u uitgeven op basis van uw Azure Cosmos-container of de database, zoals een leesaanvraag, schrijfaanvraag, queryaanvraag, opgeslagen procedures hebben een bijbehorende kosten die worden afgetrokken van de ingerichte doorvoer. Als u inrichten van 400 RU/s en een query die 40 ru's kosten, kunt u zich om uit te geven van 10 dergelijke query's per seconde. Elk verzoek verder krijgt beperkt in de snelheid en moet u de aanvraag opnieuw proberen. Als u van-clientstuurprogramma's gebruikmaakt, ondersteunen ze de logica voor automatische nieuwe pogingen.

U kunt de doorvoer voor databases inrichten of containers en elke strategie kunt u besparen op de kosten afhankelijk van het scenario.

## <a name="optimize-by-provisioning-throughput-at-different-levels"></a>Optimaliseren door in te richten doorvoer op verschillende niveaus

* Als u de doorvoer voor een database, de containers inrichten kunt bijvoorbeeld verzamelingen/tabellen/grafieken in die database delen de doorvoer op basis van de belasting. Op het databaseniveau van de gereserveerde doorvoer wordt ongelijkmatig, gedeeld, afhankelijk van de werkbelasting op een specifieke set met containers.

* Als u de doorvoer voor een container inricht, wordt de doorvoer gegarandeerde voor die container, ondersteund door de SLA. De keuze van de sleutel van een logische partitie is van cruciaal belang voor evenredige verdeling van belasting over alle logische partities van een container. Zie [partitioneren](partitioning-overview.md) en [horizontaal schalen](partition-data.md) artikelen voor meer informatie.

Hier volgen enkele richtlijnen om te bepalen wat een strategie voor ingerichte doorvoer:

**Houd rekening met het inrichten van de doorvoer voor een Azure Cosmos DB-database (met een set met containers) als**:

1. U hebt enkele tientallen Azure Cosmos-containers en wilt dat het delen van doorvoer op sommige of alle mappen. 

2. U migreert vanaf een één tenant-database die is ontworpen om te worden uitgevoerd op virtuele machines van IaaS gehost of on-premises, bijvoorbeeld NoSQL of relationele databases met Azure Cosmos DB. Basis van de bewerkingen hebt u veel verzamelingen/tabellen/grafieken en u wilt geen wijzigingen aanbrengen in uw gegevensmodel. Opmerking: mogelijk moet u enkele van de voordelen die worden aangeboden door Azure Cosmos DB als u uw gegevensmodel niet worden bijgewerkt wanneer u migreert van een on-premises database in gevaar brengen. Het raadzaam dat u altijd toegang uw gegevensmodel om alles wat betreft prestaties en kosten optimaliseren. 

3. U wilt opnemen van niet-geplande pieken in de werkbelasting omdat gegroepeerde doorvoer op databaseniveau onderworpen aan onverwachte piek in de workload. 

4. In plaats van specifieke doorvoer instellen voor afzonderlijke containers zorgt u over het ophalen van de cumulatieve doorvoer van een set van containers in de database.

**Houd rekening met doorvoer voor een afzonderlijke container ingericht als:**

1. Hebt u een paar Azure Cosmos-containers. Omdat Azure Cosmos DB is de schema-agnostische, kan een container objecten bevatten die heterogene schema's en vereist geen klanten om meerdere containertypen, één voor elke entiteit te maken. Het is altijd een optie om te overwegen als afzonderlijke groepering 10-20 containers in een enkele container zinvol is. Met een 400 ru's minimum voor containers, alle 10-20 containers groeperen in één kan meer rendabel zijn. 

2. U wilt beheren van de doorvoer voor een specifieke container en ophalen van de gegarandeerde doorvoer voor een bepaalde container gesteund door een SLA.

**Houd rekening met een hybride van de bovenstaande twee strategieën:**

1. Zoals eerder vermeld, kunt Azure Cosmos DB u combineren en matchen van de bovenstaande twee strategieën, zodat u kunt nu enkele containers in Azure Cosmos-database, die de doorvoer die is ingericht op de database als sommige containers binnen dezelfde database kan worden gedeeld. , die hoeveelheden ingerichte doorvoer hebt toegewezen. 

2. U kunt de bovenstaande strategieën te kunnen komen met een hybride configuratie, waarin u werkt met beide database niveau ingerichte doorvoer met enkele containers speciale doorvoer toepassen.

Zoals wordt weergegeven in de volgende tabel, afhankelijk van de keuze van de API, kunt u de doorvoer bij verschillende granulaties inrichten.

|API|Voor **gedeelde** doorvoer en configureren |Voor **toegewezen** doorvoer en configureren |
|----|----|----|
|SQL-API|Database|Container|
|Azure Cosmos DB-API voor MongoDB|Database|Verzameling|
|Cassandra-API|Keyspace|Tabel|
|Gremlin-API|Databaseaccount|Graph|
|Tabel-API|Databaseaccount|Tabel|

Door inrichting doorvoer op verschillende niveaus, kunt u uw kosten op basis van de kenmerken van uw workload kunt optimaliseren. Zoals eerder vermeld, u kunt via een programma, en een toename van de tijd of verkleinen van de ingerichte doorvoer voor een afzonderlijke container (s) of gezamenlijk over een verzameling van containers. Met elastisch schalen doorvoer als uw workload wordt gewijzigd, betaalt u alleen voor de doorvoer die u hebt geconfigureerd. Als uw container of een set van containers wordt verdeeld over meerdere regio's, de doorvoer u configureert u vervolgens op de container of een set van containers wordt gegarandeerd in alle regio's beschikbaar worden gesteld.

## <a name="optimize-with-rate-limiting-your-requests"></a>Optimaliseren met frequentiebeperkende uw aanvragen

Voor workloads die niet gevoelig voor latentie zijn, kunt u minder doorvoer inrichten en zodat de toepassing verwerken gelden enkele beperkingen wanneer de werkelijke doorvoer groter is dan de ingerichte doorvoer. De server te beëindigen van de aanvraag met RequestRateTooLarge (HTTP-statuscode 429) en retourneren de `x-ms-retry-after-ms` header die aangeeft van de hoeveelheid tijd in milliseconden, die de gebruiker wachten moet voordat u de aanvraag opnieuw uitvoert. 

```html
HTTP Status 429, 
 Status Line: RequestRateTooLarge 
 x-ms-retry-after-ms :100
```

### <a name="retry-logic-in-sdks"></a>Pogingslogica in SDK 's 

De systeemeigen SDK's (.NET/.NET Core, Java, Node.js en Python) impliciet catch dit antwoord, aansluiten bij de server opgegeven opnieuw proberen na de header en probeer de aanvraag. Als uw account wordt gelijktijdig worden gebruikt door meerdere clients, wordt de volgende poging slaagt.

Hebt u meer dan één client cumulatief werken consistent boven de snelheid van aanvragen, het standaardaantal nieuwe pogingen op dit moment die momenteel is ingesteld op 9 mogelijk niet voldoende. In dit geval is, de client genereert een `DocumentClientException` met de status code 429 naar de toepassing. Het standaardaantal-nieuwe pogingen kan worden gewijzigd door in te stellen de `RetryOptions` op de ConnectionPolicy-instantie. Standaard wordt de DocumentClientException met de statuscode 429 geretourneerd na een cumulatieve wachttijd van 30 seconden als de aanvraag worden voortgezet boven de snelheid van aanvragen. Dit gebeurt zelfs als het huidige aantal nieuwe pogingen is kleiner dan het maximale aantal, worden deze de standaardwaarde van 9 of een door de gebruiker gedefinieerde waarde. 

[MaxRetryAttemptsOnThrottledRequests](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.retryoptions.maxretryattemptsonthrottledrequests?view=azure-dotnet#Microsoft_Azure_Documents_Client_RetryOptions_MaxRetryAtte) is ingesteld op 3, dus in dit geval als een aanvraagbewerking frequentielimieten zijn van de gereserveerde doorvoer voor de verzameling van meer dan de aanvraagbewerking pogingen drie keer voordat genereren de uitzondering voor de toepassing.  [MaxRetryWaitTimeInSeconds](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.retryoptions.maxretrywaittimeinseconds?view=azure-dotnet#Microsoft_Azure_Documents_Client_RetryOptions_MaxRetryWaitTimeInSeconds) is ingesteld op 60, dus in dit geval als de cumulatieve nieuwe poging wacht tijd in seconden sinds de eerste aanvraag groter is dan 60 seconden, de uitzondering is opgetreden.

```csharp
ConnectionPolicy connectionPolicy = new ConnectionPolicy(); 

connectionPolicy.RetryOptions.MaxRetryAttemptsOnThrottledRequests = 3; 

connectionPolicy.RetryOptions.MaxRetryWaitTimeInSeconds = 60;
```

## <a name="partitioning-strategy-and-provisioned-throughput-costs"></a>Strategie voor partitioneren en de kosten van de ingerichte doorvoer

Goede strategie voor partitioneren is het belangrijk om te optimaliseren, kosten in Azure Cosmos DB. Zorg ervoor dat er geen scheeftrekken van partities die worden weergegeven via metrische opslaggegevens. Zorg ervoor dat er geen scheeftrekken van doorvoer voor een partitie, die wordt weergegeven met metrische gegevens over doorvoer. Zorg ervoor dat er geen scheeftrekken naar bepaalde partitiesleutels. Dominante sleutels in de opslag worden weergegeven via metrische gegevens, maar de sleutel zijn afhankelijk van uw toepassing toegangspatroon. Het is raadzaam om na te denken over de juiste logische partitie-sleutel. Een goede partitiesleutels wordt verwacht dat de volgende kenmerken:

* Kies een partitiesleutel waarmee werkbelasting gelijkmatig over alle partities en gelijkmatig gedurende een periode verspreidt. U mag niet met andere woorden, sommige sleutels hebt met de meerderheid van de gegevens en sommige sleutels met kleiner dan of er geen gegevens. 

* Kies een partitiesleutel die kunnen toegang hebben tot patronen om te worden gelijkmatig verdeeld over logische partities. De werkbelasting is redelijk zelfs voor alle sleutels. Met andere woorden, mag niet het merendeel van de werkbelasting worden gericht op een paar specifieke sleutels. 

* Kies een partitiesleutel waarmee een groot aantal waarden heeft. 

Het uitgangspunt is op de gegevens en de activiteit in de container verdeeld over de set met logische partities, zodat de resources voor gegevensopslag en doorvoer kunnen worden verdeeld over de logische partities. Kandidaten voor partitiesleutels, omvat mogelijk de eigenschappen die vaak als een filter in uw query's worden weergegeven. Query's kunnen door de partitiesleutel te nemen in het filterpredicaat efficiënt worden gerouteerd. Met dergelijke een strategie voor partitionering is het optimaliseren van de ingerichte doorvoer veel eenvoudiger. 

### <a name="design-smaller-items-for-higher-throughput"></a>Ontwerp kleinere items voor een hogere doorvoer 

De aanvraag kosten in rekening gebracht of de kosten voor het verwerken van aanvraag van een bepaalde bewerking is direct gecorreleerd met de grootte van het item. Bewerkingen voor grote objecten duurder dan bewerkingen op kleinere items. 

## <a name="data-access-patterns"></a>Patronen voor databasetoegang gegevens 

Het is altijd een goede gewoonte om uw gegevens logisch te scheiden in logische categorieën op basis van hoe vaak u toegang de gegevens tot. U kunt de verbruikte opslag en de vereiste doorvoer verfijnen door deze te categoriseren als hot, Gemiddeld of koude gegevens. U kunt, afhankelijk van de frequentie van toegang, plaatst u de gegevens in afzonderlijke containers (bijvoorbeeld tabellen, grafieken en verzamelingen) en afstemmen van de ingerichte doorvoer op deze om aan de behoeften van dat segment van de gegevens mogelijk te maken. 

Als u Azure Cosmos DB en u weet u niet wilt zoeken op bepaalde waarden of wordt zelden toegang, moet u bovendien de gecomprimeerde waarden van deze kenmerken opslaan. Met deze methode die u kunt opslaan op opslagruimte, index-ruimte en ingerichte doorvoer en leiden tot lagere kosten.

## <a name="optimize-by-changing-indexing-policy"></a>Optimaliseren door het veranderen van indexeringsbeleid 

Azure Cosmos DB indexeert automatisch elke eigenschap van elke record standaard. Dit is bedoeld voor ontwikkeling vereenvoudigen en zorgen voor uitstekende prestaties voor veel verschillende typen ad-hocquery's. Als u grote records met duizenden eigenschappen hebben, betalen van de kosten voor doorvoer voor indexering van elke eigenschap mogelijk niet handig, met name als u alleen query op basis van 10 en 20 van deze eigenschappen. Als u dichter bij het ophalen van een ingang op uw specifieke werkbelasting, onze richtlijnen zijn bedoeld om af te stemmen uw index-beleid. Alle informatie over Azure Cosmos DB indexeringsbeleid vindt [hier](indexing-policies.md). 

## <a name="monitoring-provisioned-and-consumed-throughput"></a>Bewaking ingericht en doorvoer die worden gebruikt 

U kunt het totale aantal ingerichte ru's, het aantal aanvragen beperkt in de snelheid, evenals het aantal ru's die u hebt verbruikt in Azure portal controleren. De volgende afbeelding toont een voorbeeld van de metrische gegevens:

![Aanvraageenheden in Azure portal controleren](./media/optimize-cost-throughput/monitoring.png)

U kunt ook meldingen om te controleren als het aantal aanvragen beperkt in de snelheid van een bepaalde drempelwaarde overschrijdt instellen. Zie [over het bewaken van Azure Cosmos DB](use-metrics.md) artikel voor meer informatie. Deze waarschuwingen kunnen een e-mailbericht verzenden naar de accountbeheerders of bel een aangepaste HTTP-Webhook of een Azure-functie voor een betere automatisch ingerichte doorvoer. 

## <a name="scale-your-throughput-elastically-and-on-demand"></a>Uw doorvoer elastisch schalen en on-demand 

Omdat u kosten in rekening voor de ingerichte doorvoer gebracht, kan die overeenkomt met de ingerichte doorvoer aan uw behoeften u helpen te voorkomen dat de kosten voor de niet-gebruikte doorvoer. U kunt uw ingerichte doorvoer omhoog of omlaag schalen elk gewenst moment, indien nodig.  

* Bewaking van het verbruik van uw ru's en de verhouding van de tarief-limited aanvragen waarschijnlijk dat u niet wilt houden ingerichte in de gehele constante gedurende de dag of de week. Ontvangt u mogelijk minder verkeer 's nachts of tijdens het weekend. Met behulp van Azure portal of Azure Cosmos DB native SDK's of REST-API, kunt u uw ingerichte doorvoer kunt schalen op elk gewenst moment. Azure Cosmos DB van REST-API biedt eindpunten om bij te werken via een programma het prestatieniveau van uw containers waardoor het eenvoudig om aan te passen van de doorvoer van uw code afhankelijk van de tijd van de dag of de dag van de week. De bewerking wordt uitgevoerd zonder uitvaltijd en normaal gesproken wordt van kracht in minder dan een minuut. 

* Een van de gebieden die moeten worden geschaald doorvoer is wanneer u gegevens in Azure Cosmos DB, bijvoorbeeld tijdens de gegevensmigratie opnemen. Nadat u de migratie hebt voltooid, kunt u de ingerichte doorvoer omlaag schalen om af te handelen van de oplossing stabiele status.  

* Vergeet niet dat de facturering is met de granulariteit van één uur, zodat u geld niet wordt opgeslagen als u uw ingerichte doorvoer vaker dan één uur op een tijdstip wijzigen.

## <a name="determine-the-throughput-needed-for-a-new-workload"></a>De doorvoer die nodig zijn voor een nieuwe werkbelasting bepalen 

Om te bepalen van de ingerichte doorvoer voor een nieuwe werkbelasting, kunt u de volgende stappen uit: 

1. Een initiële, ruwe evaluatie met behulp van de Capaciteitsplanner uitvoeren en aanpassen van uw schattingen met behulp van Azure Cosmos Explorer in Azure portal. 

2. Het is raadzaam om te maken van de containers met hogere doorvoer dan verwacht en vervolgens omlaag schalen naar behoefte. 

3. Het is raadzaam een van de systeemeigen Azure Cosmos DB SDK's gebruiken om de voordelen van automatische nieuwe pogingen wanneer aanvragen beperkt in de snelheid. Als u werkt op een platform dat wordt niet ondersteund en de REST-API van Cosmos DB gebruiken, implementeert u uw eigen nieuwe pogingen beleid met de `x-ms-retry-after-ms` header. 

4. Zorg ervoor dat de code van uw toepassing zonder problemen ondersteuning biedt voor het geval wanneer alle nieuwe pogingen mislukken. 

5. U kunt waarschuwingen van de Azure-portal om meldingen voor frequentielimiet te kunnen configureren. U kunt beginnen met conservatieve limieten, zoals 10 rate-limited aanvragen gedurende de laatste 15 minuten en schakel over naar meer enthousiaste regels zodra u het werkelijke verbruik te achterhalen. Incidentele frequentielimieten zijn prima, dat worden ze weergegeven dat u bent spelen met de limieten die u hebt ingesteld en dat is precies wat u wilt doen. 

6. Bewaking gebruiken om te begrijpen van het patroon van uw verkeer, zodat u kunt overwegen de noodzaak dynamisch aan te passen de doorvoer-inrichting op de dag of week. 

7. Controleer uw ingerichte versus verbruikte doorvoerverhouding regelmatig om er zeker van zijn dat u niet meer dan het vereiste aantal containers en -databases hebt ingericht. Met een beetje over ingerichte doorvoer is een goede beveiliging uit.  

### <a name="best-practices-to-optimize-provisioned-throughput"></a>Aanbevolen procedures voor het optimaliseren van de ingerichte doorvoer 

De volgende stappen helpen u uw oplossingen zeer schaalbare en rendabele maken bij het gebruik van Azure Cosmos DB.  

1. Hebt u aanzienlijk via ingerichte doorvoer in containers en -databases, moet u ru's ingerichte Vs verbruikte ru's controleren en afstemmen van de werkbelastingen.  

2. Een methode voor het schatten van de hoeveelheid gereserveerde doorvoer die is vereist voor de toepassing, is het vastleggen van de aanvraag eenheid RU kosten die zijn gekoppeld aan met het normale bewerkingen uitvoeren in een representatieve Azure Cosmos-container of een database die wordt gebruikt door uw toepassing en vervolgens schatting maken van het aantal bewerkingen die u verwacht dat per seconde uitvoeren. Zorg dat voor het meten en zijn typische query's en ook hun gebruik. Voor meer informatie over hoe u kosten wilt ramen RU van query's via een programma of met behulp van portal Zie [de kosten van query's optimaliseren](online-backup-and-restore.md). 

3. Er is een andere manier om hun kosten en bewerkingen in ru's door in te schakelen van Log Analytics, waarmee u de verdeling van de bewerking/duur en de kosten van de aanvraag. Azure Cosmos DB biedt aanvraag kosten in rekening gebracht voor elke bewerking, zodat elke bewerking kosten in rekening gebracht kan worden opgeslagen van het antwoord en vervolgens worden gebruikt voor analyse. 

4. U kunt flexibel schalen omhoog en omlaag de ingerichte doorvoer, als u nodig hebt om aan de behoeften van uw werkbelasting te. 

5. U kunt toevoegen en verwijderen van regio's die zijn gekoppeld aan uw Azure Cosmos-account als u nodig hebt en kosten te beheren. 

6. Zorg ervoor dat u gelijkmatige verdeling van gegevens en workloads voor logische partities van uw containers hebt. Hebt u een partitie ongelijke distributie, kan dit ertoe leiden dat voor het inrichten van hogere hoeveelheid doorvoer dan de waarde die nodig is. Als u dat u een ongelijke verdeling hebt hebt geïdentificeerd, wij raden aan de belasting gelijkmatig verdelen over de partities of partitioneer de gegevens. 

7. Als u veel containers hebt en deze containers geen Sla's vereisen, kunt u de aanbieding op basis van een database voor gevallen waarin de per containerdoorvoer Sla's zijn niet van toepassing. U moet bepalen welke van de Azure Cosmos-containers die u wilt migreren naar de database-level doorvoer bieden en migreer ze dan met behulp van een oplossing wijzigen op basis van een feed. 

8. Overweeg het gebruik van de 'Cosmos DB gratis laag' (gratis gedurende één jaar), probeer het Cosmos DB (maximaal drie regio's) of downloadbare Cosmos DB-emulator gebruikt voor scenario's voor ontwikkelen en testen. Met behulp van deze opties voor test-ontwikkelen, kunt u uw kosten aanzienlijk verlagen.  

9. U kunt verder werklastspecifiek waar kosten geoptimaliseerd kunnen – bijvoorbeeld uitvoeren verhoging van de batch-grootte, de load-balancing leesbewerkingen in meerdere regio's en ongedaan maken het dupliceren van gegevens, indien van toepassing.

10. Met Azure Cosmos DB gereserveerde capaciteit, kunt u aanzienlijke kortingen krijgen voor maximaal 65% voor drie jaar. Azure Cosmos DB gereserveerde Capaciteitsmodel is een toezegging van aanvraageenheden die nodig zijn na verloop van tijd. De kortingen die zich gelaagd zodat de meer aanvraageenheden u over een langere periode, hoe beter de korting is. Deze kortingen worden onmiddellijk toegepast. Alle ru's boven uw ingerichte waarden worden in rekening gebracht op basis van de capaciteitskosten voor niet-gereserveerde. Zie [Cosmos DB gereserveerde capaciteit](cosmos-db-reserved-capacity.md)) voor meer informatie. Overweeg het kopen van gereserveerde capaciteit voor de kosten voor de ingerichte doorvoer nog verder verlagen.  

## <a name="next-steps"></a>Volgende stappen

Vervolgens kunt u doorgaan naar meer informatie over kostenoptimalisatie in Azure Cosmos DB met de volgende artikelen:

* Meer informatie over [optimaliseren voor de ontwikkeling en testen](optimize-dev-test.md)
* Meer informatie over [informatie over uw factuur voor Azure Cosmos DB](understand-your-bill.md)
* Meer informatie over [opslagkosten te optimaliseren](optimize-cost-storage.md)
* Meer informatie over [optimaliseren van de kosten van lees- en schrijfbewerkingen](optimize-cost-reads-writes.md)
* Meer informatie over [de kosten van query's optimaliseren](optimize-cost-queries.md)
* Meer informatie over [optimaliseren van de kosten van Azure Cosmos-accounts voor meerdere regio's](optimize-cost-regions.md)


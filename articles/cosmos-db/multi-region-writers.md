---
title: Meerdere masters op wereldwijde schaal met Azure Cosmos DB | Microsoft Docs
description: ''
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: rimman
ms.openlocfilehash: 4911a302bf9055948827a72f2e631663b8be741e
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39265594"
---
# <a name="multi-master-at-global-scale-with-azure-cosmos-db"></a>Meerdere masters op wereldwijde schaal met Azure Cosmos DB 
 
Ontwikkeling van wereldwijd gedistribueerde toepassingen die met een lokale latentie reageren tijdens het onderhouden van consistente weergaven van gegevens over de hele wereld is een uitdaging zijn om probleem. Klanten gebruiken de wereldwijd gedistribueerde databases, omdat ze nodig hebt voor de data access-latentie verbeteren, bereiken van hoge beschikbaarheid van gegevens, zorg ervoor dat gegarandeerde noodherstel en om te voldoen aan hun zakelijke vereisten. Meerdere masters in Azure Cosmos DB biedt een hoge mate van beschikbaarheid (99,999%), slechts enkele milliseconden latentie om gegevens en schaalbaarheid met ondersteuning voor de ingebouwde geavanceerde en flexibele conflict schermresolutie te schrijven. Deze functies aanzienlijk eenvoudiger van wereldwijd gedistribueerde toepassingen. Voor wereldwijd gedistribueerde toepassingen is ondersteuning voor meerdere masters van cruciaal belang. 

![Meerdere masters-architectuur](./media/multi-region-writers/multi-master-architecture.png)

U kunt met Azure Cosmos DB ondersteuning voor meerdere masters, schrijfbewerkingen uitvoeren op containers van gegevens (bijvoorbeeld, verzamelingen, grafieken, tabellen) gedistribueerd overal ter wereld. U kunt gegevens in de regio die is gekoppeld aan uw databaseaccount bijwerken. Deze Gegevensupdates kunnen asynchroon worden doorgegeven. Naast het bieden van snelle toegang en schrijflatentie tot uw gegevens, biedt meerdere masters ook een praktische oplossing voor failover en taakverdeling problemen. Kortom, met Azure Cosmos DB profiteert u van schrijflatentie van < 10 ms in het 99e percentiel overal in de hele wereld, voor 99,999% schrijven en leesbeschikbaarheid overal ter wereld en de mogelijkheid voor het schalen van zowel doorvoer overal over de hele wereld lezen en schrijven.   

> [!IMPORTANT]
> Ondersteuning voor meerdere masters private Preview-versie, gebruik van de preview-versie is [aanmelden](#sign-up-for-multi-master-support) nu.

## <a name="sign-up-for-multi-master-support"></a>Aanmelden voor de ondersteuning voor meerdere masters

Als u al een Azure-abonnement hebt, kunt u zich aanmelden voor deelname aan het previewprogramma meerdere masters in Azure portal. Als u een nieuwe klant bent van Azure, registreert u zich voor een [gratis proefversie](https://azure.microsoft.com/free), waarna u 12 maanden gratis toegang krijgt tot Azure Cosmos DB. De volgende stappen voor het aanvragen van toegang tot het meerdere masters preview-programma.

1. Klik in [Azure Portal](https://portal.azure.com) op **Resource maken** > **Databases** > **Azure Cosmos DB**.  

2. Geef een naam op voor uw Azure Cosmos DB-account, kiest u de API, abonnement, resourcegroep en locatie op de pagina Nieuw Account.  

3. Selecteert u vervolgens **registreren voor Previews vandaag** onder het veld meerdere Master Preview.  

   ![Aanmelden voor meerdere masters preview](./media/multi-region-writers/sign-up-for-multi-master-preview.png)

4. In de **registreren voor Previews vandaag** deelvenster, klikt u op **OK**. Nadat u de aanvraag hebt ingediend, wordt de status gewijzigd in **in afwachting van goedkeuring** in de blade-account maken.  

Nadat u de aanvraag hebt ingediend, ontvangt u een e-mailbericht dat uw aanvraag is goedgekeurd. Vanwege het grote aantal aanvragen kan het maximaal een week duren voordat u een melding ontvangt. U hoeft geen ondersteuningsticket te maken om de aanvraag te voltooien. Aanvragen worden beoordeeld in de volgorde waarin ze zijn ontvangen.

## <a name="a-simple-multi-master-example--content-publishing"></a>Een eenvoudig meerdere masters voorbeeld-inhoud publiceren  

We bekijken een echte scenario waarin wordt beschreven hoe u ondersteuning voor meerdere masters met Azure Cosmos DB. Houd rekening met een content publishing dat is gebouwd met Azure Cosmos DB. Hier volgen enkele vereisten die dit platform moet voldoen aan voor een optimale ervaring voor zowel uitgevers en consumenten. 

* Zowel auteurs en abonnees worden verdeeld over de hele wereld.  

* Auteurs moeten (schrijven) artikelen publiceren naar hun eigen lokale regio (dichtstbijzijnde).  

* Auteurs hebben lezers /-abonnees hun artikelen die zijn verdeeld over de hele wereld.  

* Abonnees moeten een melding ontvangen wanneer nieuwe artikelen worden gepubliceerd.  

* Abonnees moeten in staat om artikelen te lezen uit hun eigen lokale regio zijn. Ze moeten ook worden beoordelingen toevoegen aan deze artikelen.  

* Iedereen met inbegrip van de auteur van de artikelen moet kunnen weergeven, alle beoordelingen die zijn gekoppeld aan artikelen van een lokale regio's.  

Ervan uitgaande dat miljoenen consumenten en uitgevers met miljarden artikelen, snel hebben we pakt u de problemen van schaal, samen met de locatie van de toegang te garanderen. Een gebruiksvoorbeeld is een perfecte keuze voor Azure Cosmos DB meerdere masters. 

## <a name="benefits-of-having-multi-master-support"></a>Voordelen van ondersteuning voor meerdere masters 

Ondersteuning voor meerdere masters is essentieel voor wereldwijd gedistribueerde toepassingen. Meerdere masters is samengesteld uit [meerdere hoofd-regio's](distribute-data-globally.md) die evenredig deel uitmaken van een schrijven overal model (actief / actief-patroon) en deze wordt gebruikt om ervoor te zorgen dat gegevens beschikbaar is op elk gewenst moment waar u ze nodig hebt. Updates aangebracht in een afzonderlijke regio wordt asynchroon doorgegeven aan de andere regio's (die op zijn beurt zijn master regio's in hun eigen). Azure Cosmos DB-regio's die als hoofd regio's in een configuratie met meerdere masters automatisch werken als u wilt de gegevens van alle replica's worden geconvergeerd en zorg ervoor dat [globale consistentie en gegevensintegriteit](consistency-levels.md). De volgende afbeelding ziet u de replicatie voor lezen/schrijven voor een single-master en meerdere masters.

![Single-master en meerdere masters](./media/multi-region-writers/single-vs-multi-master.png)

Implementatie meerdere masters zelf wordt last toegevoegd voor ontwikkelaars. Grootschalige klanten die zich voor het implementeren van meerdere masters op hun eigen spendeert honderden uur configureren en testen van een configuratie met meerdere masters over de hele wereld en veel hebben een toegewezen set met technici die enige taak is om te controleren en onderhouden van meerdere masters replicatie. Het maken en beheren van meerdere masters instellen op uw eigen duurt, resources weg van de toepassing zelf innoveren en veel hogere kosten leidt. Azure Cosmos DB biedt ondersteuning voor meerdere masters "out-of-the-box" en verwijdert u deze overhead van ontwikkelaars.  

Kortom biedt meerdere masters de volgende voordelen:

* **Herstel na noodgevallen voor een betere, beschikbaarheid en failover schrijven**-meerdere masters kan worden gebruikt om de hoge beschikbaarheid van een essentiële database naar een grotere mate behouden. Een database met meerdere masters kan bijvoorbeeld gegevens van de ene regio naar een failoverregio repliceren wanneer de primaire regio niet beschikbaar vanwege een storing of een regionaal noodgeval. Een failoverregio fungeert als een volledig functionele master regio ter ondersteuning van de toepassing. Meerdere masters biedt betere "overlevingsvermogen" bescherming met betrekking tot natuurrampen, stroomstoringen of sabotage, omdat de resterende regio's kan gebruikmaken van geografisch verschillende met meerdere modellen met een gegarandeerde schrijven beschikbaarheid > 99,999%. 

* **Schrijflatentie verbeterd voor eindgebruikers** - de dichter bij uw gegevens (die u fungeren) is voor de eindgebruikers, hoe beter de ervaring is. Bijvoorbeeld, als u gebruikers in Europa hebt, maar de database zich in de Verenigde Staten of Australië, de extra latentie is ongeveer 140 ms en 300 ms voor de desbetreffende regio's. Vertragingen zijn niet toegestaan beginnen met voor veel populaire games, bankieren vereisten of interactieve toepassingen (web- of mobile). Latentie van een grote rol speelt bij van de klant perceptie van een hoogwaardige ervaring en is gebleken dat het gedrag van gebruikers tot op zekere hoogte merkbare invloed uitoefenen op. Verbetert de technologie en met name met de komst van AR, VR en MR, moeten vereisen nog meer diepgaande en levensechte ervaringen, ontwikkelaars nu voor het produceren van softwaresystemen met strikte latentievereisten. Daarom is het belangrijker lokaal beschikbare toepassingen en gegevens (inhoud voor de apps) in te stellen. Met meerdere masters in Azure Cosmos DB is prestaties zo snel mogelijk regelmatig lokale leest en schrijft en verbeterde wereldwijd door geo-distributie.  

* **Verbeterde schaalbaarheid schrijven en doorvoer van schrijfbewerkingen** : meerdere masters krijgt hogere doorvoer en meer capaciteit terwijl meerdere consistentiemodellen met juistheid garandeert en back-up gemaakt door Sla's. 

  ![Schaal de doorvoer van schrijfbewerkingen met meerdere masters](./media/multi-region-writers/scale-write-throughput.png)

* **Betere ondersteuning voor niet-verbonden omgevingen (bijvoorbeeld edge-apparaten)** -meerdere masters kan gebruikers alle repliceren of een subset van gegevens uit een edge-apparaat naar een regio die het dichtst in een omgeving zonder verbinding. In dit scenario is Typerend voor verkopers automation-systemen, waar een subset van gegevens met betrekking tot de afzonderlijke verkoper in laptop van een persoon (een niet-verbonden apparaat) worden opgeslagen. Master regio's in de cloud die zich overal ter wereld kunnen worden uitgevoerd als het doel van het exemplaar van de externe edge-apparaten.  

* **Taakverdeling** -met meerdere masters, de belasting van de toepassing kan worden uitgevoerd door gebruikers/workloads kunt verplaatsen van een zwaar belaste regio naar regio's waar belasting gelijkmatig wordt verdeeld. Schrijven capaciteit kan eenvoudig worden uitgebreid door een nieuwe regio toevoegen en vervolgens enkele schrijfbewerkingen overschakelen naar de nieuwe regio. 

* **Beter gebruik van de ingerichte capaciteit** - met meerdere masters, voor workloads met schrijfintensief en gemengde, kunt u de ingerichte capaciteit verzadigen in meerdere regio's...  In sommige gevallen die u opnieuw kunt distribueren lees- en schrijfbewerkingen meer evenredig, zodat er minder doorvoer moet worden ingericht en leidt tot meer kostenbesparingen voor klanten.  

* **Eenvoudiger en beter bestand is tegen app-architecturen** -toepassingen verplaatsen naar de configuratie van meerdere masters gegarandeerde tolerantie voor gegevens.  Met Azure Cosmos DB verbergen alle complexiteit, kan deze het toepassingsontwerp en de architectuur aanzienlijk vereenvoudigen. 

* **Risico's gratis testen van failover** -Failover testen geen een verslechtering van de op de doorvoer van schrijfbewerkingen. Alle andere regio's zijn volledige-modellen, met meerdere masters, zodat de failover is niet veel invloed op de doorvoer van schrijfbewerkingen.  

* **Lagere totale kosten van Ownership(TCO) en DevOps** -aan de wensen van schaalbaarheid, prestaties, wereldwijde distributie, dat de beoogde hersteltijden zijn vaak kostbaar vanwege dure invoegtoepassingen of onderhouden van een back-up maken van infrastructuur die pas na noodgevallen at-rest ramp. Met Azure Cosmos DB meerdere masters ondersteund door marktleidende, toonaangevende Sla's, ontwikkelaars niet meer nodig te bouwen en onderhouden van de 'back-end lijm logica' zelf en ophalen van een zorgen hoeft te maken met hun essentiële workloads. 

## <a name="use-cases-where-multi-master-support-is-needed"></a>Use cases, waarbij meerdere masters ondersteuning nodig is

Er zijn talrijke gebruiksvoorbeelden voor meerdere masters in Azure Cosmos DB: 

* **IoT** -meerdere masters van Azure Cosmos DB kan voor vereenvoudigde gedistribueerde gegevensverwerking IoT-implementatie. Geografisch gedistribueerde edge-implementaties die gebruikmaken van CRDT conflict gratis gerepliceerde gegevens typen vaak nodig hebt om bij te houden van time series-gegevens van meerdere locaties. Elk apparaat kan worden toegewezen aan een van de dichtstbijzijnde regio's, en een apparaat kan worden verzonden (bijvoorbeeld een auto) en kan dynamisch opnieuw om te schrijven naar een andere regio worden ondergebracht.  

* **E-Commerce** -om de goede gebruikerservaring in e-commerce-scenario's moet hoge beschikbaarheid en herstelmogelijkheden bij foutscenario's. In het geval een regio uitvalt, desgewenst gebruikerssessies, winkelwagens, actieve winkelen lijsten moeten naadloos worden opgepikt door een andere regio zonder verlies van status. In de tussentijd updates die door de gebruiker op de juiste wijze moeten worden verwerkt (bijvoorbeeld wordt toegevoegd en verwijderd uit de winkelwagen moeten overdragen via). Met meerdere masters, Azure Cosmos DB kunnen worden verwerkt dergelijke scenario's zonder problemen, met een soepele overgang tussen actieve regio's tijdens het onderhoud van een consistente weergave van de gebruiker vanuit het oogpunt van. 

* **Fraude/Anomaliedetectie** -vaak toepassingen die gebruikersactiviteit of accountactiviteit controleren wereldwijd worden gedistribueerd en moet bijhouden van meerdere gebeurtenissen tegelijkertijd. Tijdens het maken en onderhouden van scores voor een gebruiker, moeten acties op basis van verschillende geografische regio's tegelijkertijd scores om te voorkomen dat de metrische gegevens risico inline bijwerken. Azure Cosmos DB kunt zorgen dat ontwikkelaars zich niet hoeven te conflict scenario's op het toepassingsniveau van de. 

* **Samenwerking** : voor toepassingen die positie gebaseerd op populariteit van artikelen, zoals goederen over verkoop of media om te worden verbruikt enzovoort. Populariteit bijhouden in geografische regio's krijgen ingewikkeld, met name wanneer royalty's nodig hebt om te worden betaald of echte reclame beslissingen worden genomen. Rangschikking, sorteren en rapportage in veel regio's wereldwijd in realtime met Azure Cosmos DB kan ontwikkelaars functies zonder veel moeite en optimale latenties leveren. 

* **Softwarelicentiecontrole** - telling en het regelen van gebruik (zoals API-aanroepen, transacties per seconde, minuten gebruikt) wereldwijd kan worden geïmplementeerd met eenvoudig te houden met behulp van Azure Cosmos DB meerdere masters. Ingebouwde conflictoplossing zorgt ervoor dat beide nauwkeurigheid van de aantallen en regelgeving in realtime. 

* **Persoonlijke instellingen** - onderscheidingen of u geografisch verspreide tellers die de acties zoals loyaliteit activeren onderhoudt verwijst of implementeren van aangepaste gebruikerssessie bekijkt, de hoge beschikbaarheid en vereenvoudigd geografisch gedistribueerde telling geleverd door Azure Cosmos DB, kunt toepassingen bieden hoge prestaties met eenvoudig te houden. 

## <a name="conflict-resolution-with-multi-master"></a>Conflictoplossing met meerdere masters 

Met meerdere masters, worden de uitdaging vaak is dat twee (of meer) replica's van dezelfde record door verschillende schrijvers in twee of meer verschillende regio's tegelijkertijd kunnen worden bijgewerkt. Gelijktijdige schrijfbewerkingen kunnen leiden tot twee verschillende versies van dezelfde record en zonder ingebouwde conflictoplossing en de toepassing zelf conflictoplossing op te lossen dit inconsistentie moet uitvoeren.  

**Voorbeeld** -gaan we ervan uit dat u van Azure Cosmos DB gebruikmaakt als het archief met persistentie voor een toepassing voor winkelen en deze toepassing is geïmplementeerd in twee regio's: VS-Oost en VS-West.  Als u ongeveer op hetzelfde moment, een gebruiker in San Francisco een item toegevoegd aan de winkelwagen (bijvoorbeeld een boek) tijdens een proces voor het beheren van inventaris in de VS-Oost, wordt een ander winkelwagen winkelwagen item (bijvoorbeeld een nieuwe telefoon) ongeldig voor dezelfde gebruiker in reactie op een s upplier melding dat de releasedatum had vertraagd. Op tijdstip T1 verschillen de winkelwagen winkelwagen records in de twee regio's. De database wordt de replicatie en het mechanisme voor conflictoplossing gebruiken om op te lossen dit inconsistentie en uiteindelijk een van de twee versies van de winkelwagen wordt geselecteerd. Met behulp van het conflict resolutie methodiek meestal toegepast door meerdere masters databases (bijvoorbeeld laatste schrijven wins), is het niet mogelijk voor de gebruiker of toepassing om te voorspellen welke versie wordt geselecteerd. In beide gevallen moet gegevens verloren zijn gegaan of onverwacht gedrag kan zich voordoen. Als de versie van de regio Oost is geselecteerd, klikt u vervolgens de selectie van de gebruiker van een nieuwe inkooporder-item (dat wil zeggen, een boek) gaat verloren en als de regio West is geselecteerd, klikt u vervolgens het eerder geselecteerde item (dat wil zeggen, telefoon) is nog steeds in de winkelwagen. In beide gevallen informatie gaat verloren. Ten slotte een ander proces inspecteren van de winkelen winkelwagen tussen tijdstippen T1 en T2 gaat ook niet-deterministisch gedrag. Bijvoorbeeld, een achtergrondproces waarmee het magazijn vervulling selecteert en werkt de winkelwagen verzendkosten zou leiden tot resultaten die met de uiteindelijke inhoud van de winkelwagen conflicteren. Als het proces wordt uitgevoerd in de regio West en alternatieve 1 reality wordt, zou deze de verzendkosten voor twee items compute, zelfs als de winkelwagen beschikt binnenkort over slechts één item, het rapport. 

Azure Cosmos DB implementeert de logica voor het verwerken van conflicterende schrijfbewerkingen in de database-engine zelf. Azure Cosmos DB biedt **ondersteuning voor de uitgebreide en flexibele conflict schermresolutie** door het aanbieden van verschillende conflict resolutie modellen, met inbegrip van automatische (gegevenstypen CRDT conflict gratis gerepliceerd), laatste schrijven Wins (LWW) en aangepaste ( Opgeslagen Procedure) voor automatische conflictoplossing. De modellen van de oplossing conflict juistheid en consistentie garanties bieden voor de en verwijderen van de werkbelasting van ontwikkelaars hebben om na te denken over consistentie, beschikbaarheid, prestaties, replicatielatentie en complexe combinaties van-gebeurtenissen onder geo-failover en regio-overschrijdende kunt oplossen.  

  ![Mult-master conflictoplossing](./media/multi-region-writers/multi-master-conflict-resolution-blade.png)

Hebt u 3 soorten of conflict resolutie modellen die worden aangeboden door Azure Cosmos DB. De semantiek van het conflict resolutie-modellen zijn als volgt: 

**Automatische** -dit is het standaardbeleid voor de oplossing van conflicten. Selecteren van dit beleid zorgt ervoor dat Azure Cosmos DB de conflicterende updates op de server automatisch moeten worden opgelost en sterke-eindconsistentie garandeert. Azure Cosmos DB implementeert intern, automatische conflictoplossing door gebruik te maken van Conflict-Free-gerepliceerd-gegevenstypen (CRDTs) in de database-engine.  

**Laatste schrijven Wins (LWW)** - eigenschap timestamp kiezen dit beleid kunt u oplossen conflicten op basis van het systeem gedefinieerde gesynchroniseerd of een aangepaste eigenschap gedefinieerd voor de conflicterende versie van de records. De conflictoplossing vindt plaats op de server en de versie met de meest recente tijdstempel is geselecteerd als de winnaar.  

**Aangepaste** -u kunt een gedefinieerd conflict resolutie toepassingslogica registreren met het registreren van een opgeslagen procedure. De opgeslagen procedure wordt ophalen aangeroepen tijdens de detectie van Bijwerkconflicten onder de verantwoordelijkheid van de databasetransactie van een, klikt u op de server. Als u de optie selecteert, maar niet registreren van een opgeslagen procedure (of als de opgeslagen procedure een uitzondering tijdens runtime genereert), kunt u toegang tot alle van de conflicterende versie via de conflicten Feed en deze afzonderlijk kunt oplossen.  

## <a name="next-steps"></a>Volgende stappen  

In dit artikel vernomen u dat wereldwijd gedistribueerde meerdere masters gebruiken met Azure Cosmos DB. Haal vervolgens een overzicht van de volgende bronnen: 

* [Meer informatie over hoe Azure Cosmos DB biedt ondersteuning voor wereldwijde distributie](distribute-data-globally.md)  

* [Meer informatie over automatische failover in Azure Cosmos DB](regional-failover.md)  

* [Meer informatie over globale consistentie met Azure Cosmos DB](consistency-levels.md)  

* Ontwikkelen met meerdere regio's met behulp van de Azure Cosmos DB - [SQL API](tutorial-global-distribution-sql-api.md), [MongoDB-API](tutorial-global-distribution-mongodb.md), of [Table-API](tutorial-global-distribution-table.md)  

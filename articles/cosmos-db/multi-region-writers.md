---
title: Meerdere masters op globale schaal met Azure Cosmos DB | Microsoft Docs
description: ''
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: rimman
ms.openlocfilehash: cc66b2f506d81a7ba10b26c3b24287472e890682
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34724904"
---
# <a name="multi-master-at-global-scale-with-azure-cosmos-db"></a>Meerdere master op globale schaal met Azure Cosmos-DB 
 
Globaal ontwikkelen gedistribueerde toepassingen die met lokale latentie reageren terwijl consistente weergaven van gegevens over de hele wereld onderhouden een probleem lastig is. Klanten gebruiken globaal gedistribueerde databases, omdat er data access-latentie verbeteren, hoge beschikbaarheid van gegevens te bereiken, ervoor zorgen dat gegarandeerde noodherstel en om te voldoen aan hun zakelijke behoeften. Meerdere master in Azure Cosmos DB biedt een hoge mate van beschikbaarheid (99,999%), één cijfer milliseconde latentie schrijven van gegevens en schaalbaarheid met ondersteuning voor ingebouwde uitgebreide en flexibele conflict oplossen. Deze functies vereenvoudigen aanzienlijk globaal gedistribueerde toepassingen te ontwikkelen. Voor globaal gedistribueerde toepassingen is met meerdere masters ondersteuning van cruciaal belang. 

![Meerdere masters architectuur](./media/multi-region-writers/multi-master-architecture.png)

U kunt met ondersteuning voor Azure Cosmos DB meerdere masters schrijfbewerkingen uitvoeren voor containers van gegevens (bijvoorbeeld verzamelingen, grafieken en tabellen) gedistribueerd overal ter wereld. U kunt gegevens in elke regio die is gekoppeld aan uw databaseaccount bijwerken. Deze Gegevensupdates kunnen asynchroon doorgeven. Naast het voorzien in snel toegang en schrijflatentie van uw gegevens, bevat meerdere master ook een praktische oplossing voor failover en taakverdeling problemen. Kortom, met Azure Cosmos DB beschikt u over schrijflatentie van < 10 ms op het 99th percentiel overal in de wereld, 99,999% schrijven en lezen beschikbaarheid overal ter wereld en de mogelijkheid om te schalen beide doorvoer overal overal ter wereld lezen en schrijven.   

> [!IMPORTANT]
> Ondersteuning voor meerdere masters is private Preview-versie, de preview-versie te gebruiken [aanmelden](#sign-up-for-multi-master-support) nu.

## <a name="sign-up-for-multi-master-support"></a>Aanmelden voor meerdere masters ondersteuning

Als u al een Azure-abonnement hebt, kunt u zich kunt aanmelden bij het meerdere masters preview-programma in de Azure portal. Als u geen ervaring met Azure, zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/free) waar u 12 maanden gratis toegang tot Azure Cosmos DB krijgen. De volgende stappen om aan te vragen toegang tot het programma meerdere masters preview.

1. In de [Azure-portal](https://portal.azure.com), klikt u op **maken van een resource** > **Databases** > **Azure Cosmos DB**.  

2. Geef een naam voor uw account voor Azure Cosmos DB, kiest u de API, abonnement, resourcegroep en locatie op de pagina nieuwe Account.  

3. Selecteert u vervolgens **zich registreren voor de preview vandaag** onder het veld Multi Mater Preview.  

   ![Aanmelden voor meerdere masters preview](./media/multi-region-writers/sign-up-for-multi-master-preview.png)

4. In de **zich registreren voor de preview vandaag** deelvenster, klikt u op **OK**. Nadat u de aanvraag verzendt, wordt de status gewijzigd in **in afwachting van goedkeuring** in de blade-account maken.  

Nadat u de aanvraag indient, ontvangt u een e-mailbericht dat uw aanvraag is goedgekeurd. Vanwege het grote aantal aanvragen ontvangt u meldingen binnen een week. U hoeft niet te maken van een ondersteuningsticket voor het voltooien van de aanvraag. Aanvragen wordt gecontroleerd in de volgorde waarin ze werden ontvangen.

## <a name="a-simple-multi-master-example--content-publishing"></a>Een eenvoudig meerdere masters voorbeeld – inhoud publiceren  

We bekijken een echte-scenario waarin wordt beschreven hoe u ondersteuning voor meerdere masters gebruiken met Azure Cosmos DB. U kunt een content publishing platform gebouwd op Azure Cosmos DB. Hier volgen enkele vereisten waaraan dit platform voor consumenten en uitgevers voor een goede gebruikerservaring moet voldoen. 

* Zowel auteurs en abonnees worden verdeeld over de hele wereld.  

* Auteurs moeten artikelen (schrijven) publiceren op hun lokale regio (dichtst).  

* Auteurs hebben lezers/abonnees van hun artikelen die zijn verdeeld over de hele wereld.  

* Abonnees moeten een melding krijgen wanneer nieuwe artikelen worden gepubliceerd.  

* Abonnees moet leesrechten artikelen hun lokale regio. Ze moeten ook worden kunnen beoordelingen toevoegen aan deze artikelen.  

* Iedereen met inbegrip van de auteur van de artikelen moet kunnen weergeven, alle beoordelingen zijn verbonden met artikelen vanaf een lokale regio.  

Miljoenen consumenten en uitgevers miljarden artikelen, ervan uitgaande dat we hebben snel pakt u de problemen van de schaal samen met garanderen plaats van toegang. Een gebruiksvoorbeeld is een ideale keuze voor meerdere Azure DB die Cosmos-master. 

## <a name="benefits-of-having-multi-master-support"></a>Voordelen van meerdere masters ondersteuning 

Ondersteuning voor meerdere masters is essentieel voor globaal gedistribueerde toepassingen. Meerdere master is samengesteld uit [meerdere master gebieden](distribute-data-globally.md) die gelijkmatig deelnemen aan een schrijven overal model (actief / actief-patroon) en deze wordt gebruikt om ervoor te zorgen dat gegevens beschikbaar is op elk gewenst moment wanneer u deze nodig. Wijzigingen in een afzonderlijke regio zijn asynchroon doorgegeven aan alle andere regio's (die op zijn beurt master gebieden in hun eigen). Azure DB Cosmos-regio's als master regio's in een configuratie met meerdere masters automatisch besturingssysteem werken om convergeren de gegevens van alle replica's en zorg ervoor dat [globale consistentie en gegevensintegriteit](consistency-levels.md). De volgende afbeelding toont de lezen/schrijven-replicatie voor een single-master en mult-master.

![Single-master en meerdere masters](./media/multi-region-writers/single-vs-multi-master.png)

Geïmplementeerd met meerdere master op uw eigen voegt last voor ontwikkelaars. Grootschalige klanten die zich willen implementeren meerdere master op hun eigen spendeert mogelijk honderden uren configureren en testen van een configuratie met meerdere masters overal ter wereld en veel hebben een speciale reeks engineers waarvan enige taak is om te controleren en onderhouden van de meerdere master replicatie. Maken en beheren van meerdere masters setup op uw eigen duurt, resources weg van de toepassing zelf innoveren en resulteert in veel hogere kosten. Azure Cosmos DB biedt ondersteuning voor meerdere masters 'out-of-the-box' en verwijdert u deze overhead van ontwikkelaars.  

Kortom biedt met meerdere master de volgende voordelen:

* **Herstel na noodgevallen voor een betere, beschikbaarheid en failover schrijven**-meerdere master kan worden gebruikt voor het behouden van de hoge beschikbaarheid van een essentiële database naar een grotere mate. Bijvoorbeeld kan meerdere hoofddatabase gegevens repliceren van één regio in een regio failover wanneer de primaire regio niet beschikbaar vanwege een storing of een regionale noodgeval. Dergelijk gebied failover fungeert als een volledig functionele master regio ter ondersteuning van toepassing. Meerdere master biedt betere 'overlevingsvermogen' bescherming met betrekking tot een natuurramp, stroomuitval of sabotage of beide omdat resterende regio's kan zich in een geografisch verschillende meerdere masters met een gegarandeerde schrijven beschikbaarheid > 99,999%. 

* **Schrijflatentie verbeterd voor eindgebruikers** - dichter uw gegevens (die u wilt verzorgen) is voor eindgebruikers, hoe nauwkeuriger de ervaring worden. Bijvoorbeeld, als er gebruikers zijn in Europa, maar uw database zich in de Verenigde Staten of Australië, extra latentie is ongeveer 140 ms en 300 ms voor de desbetreffende regio's. Vertragingen zijn onaanvaardbaar beginnen met voor veel populaire games, bankieren vereisten of interactieve toepassingen (web of mobile). Latentie een enorme rol speelt bij perceptie van de klant van een ervaring van hoge kwaliteit en is gebleken dat het gedrag van gebruikers tot op zekere hoogte merkbare invloed. Technologie verbetert en met name met de komst van AR, VR en MR moeten vereisen nog meer ervaringen van indrukwekkende en natuurlijke, ontwikkelaars nu voor het produceren van softwaresystemen met strenge latentievereisten. Daarom is het belangrijker lokaal beschikbare toepassingen en gegevens (inhoud voor de apps). Met meerdere master in Azure Cosmos DB is prestaties net zo snel als reguliere lokale leest en schrijft en verbeterde globaal door geo-distributie.  

* **Verbeterde schaalbaarheid schrijven en de doorvoer van schrijfbewerkingen** - meerdere master krijgt hogere doorvoer en groter gebruik en biedt tegelijkertijd meerdere modellen voor consistentie met juistheid garandeert en een back-up door Sla's. 

  ![Schalen schrijfbewerkingen met meerdere master](./media/multi-region-writers/scale-write-throughput.png)

* **Betere ondersteuning voor niet-verbonden omgevingen (bijvoorbeeld randapparaten)** -meerdere master kan gebruikers repliceren alle of een subset van gegevens uit een edge-apparaat in een regio die het dichtst in een omgeving zonder verbinding. Dit scenario wordt gebruikt voor de verkopers automation-systemen, waar een subset van gegevens met betrekking tot de afzonderlijke verkoper worden opgeslagen in een afzonderlijk laptop (een niet-verbonden apparaat). Master gebieden in de cloud overal ter wereld bevinden kunnen als het doel van de kopie van de externe randapparaten werken.  

* **Taakverdeling** -met meerdere master de belasting in de hele toepassing kan worden uitgevoerd door gebruikers/werkbelastingen verplaatsen van een sterk geladen regio naar regio's waar load gelijkmatig wordt verdeeld. Schrijven capaciteit kan gemakkelijk worden uitgebreid door een nieuw gebied toevoegen en vervolgens een aantal schrijfbewerkingen overschakelt naar de nieuwe regio. 

* **Beter gebruik van ingerichte capaciteit** - met meerdere master voor schrijven zware en gemengde werkbelastingen, kunt u de ingerichte capaciteit verzadigen over meerdere regio's...  In sommige gevallen die u opnieuw kunt distribueren lees- en schrijfbewerkingen meer evenredig, zodat er minder doorvoer moeten worden ingericht en leidt tot meer kostenbesparingen voor klanten.  

* **Eenvoudiger en is toleranter app-architecturen** -toepassingen verplaatsen naar meerdere masters configuratie ophalen herstelmogelijkheden van gegevens gegarandeerd.  Azure Cosmos DB verbergen van de complexiteit, kan deze het toepassingsontwerp en de architectuur aanzienlijk vereenvoudigen. 

* **Risico gratis failover testen** -Failover testen geen een verslechtering van op de doorvoer van schrijfbewerkingen. Alle andere regio's zijn volledige-masters met meerdere master, dus failover niet zoveel invloed op de doorvoer van schrijfbewerkingen hebben wordt.  

* **Lagere totale kosten van Ownership(TCO) en DevOps** -schaalbaarheid, prestaties, wereldwijde distributie vergadering hersteltijddoelen zijn vaak kostbaar vanwege dure invoegtoepassingen of onderhouden van een back-up-infrastructuur die in rust pas na noodgevallen biedt. Met Azure Cosmos DB meerdere master back-up gemaakt door de toonaangevende Sla's, niet langer ontwikkelaars maken en onderhouden van de 'back-end lijm logica' zelf vereisen en ophalen van een gemoedsrust waarop hun bedrijfskritieke werkbelastingen worden uitgevoerd. 

## <a name="use-cases-where-multi-master-support-is-needed"></a>Gebruiksvoorbeelden wanneer meerdere masters ondersteuning nodig is

Er zijn talrijke gebruiksvoorbeelden voor meerdere master in Azure Cosmos DB: 

* **IoT** -vereenvoudigde gedistribueerde implementatie van de IoT-gegevensverwerking kunt u meerdere Azure DB die Cosmos-master. Geografisch verspreide edge-implementaties die gebruikmaken van CRDT conflict gratis gerepliceerde gegevens typen vaak moeten voor het bijhouden van reeksgegevens tijd vanaf meerdere locaties. Elk apparaat kan worden homed op een van de dichtstbijzijnde regio's en een apparaat kan worden verzonden (bijvoorbeeld een auto) en kan dynamisch opnieuw om te schrijven naar een andere regio worden ondergebracht.  

* **E-Commerce** -zodat de gebruikerservaring in scenario's voor e-commerce zeker moet hoge beschikbaarheid en herstelmogelijkheden bij scenario's is mislukt. Als een regio is mislukt, willen gebruikerssessies, on, actieve winkelen lijsten moeten naadloos worden opgenomen door een andere regio zonder verlies van status. In de tussentijd updates die door de gebruiker op de juiste wijze moeten worden verwerkt (bijvoorbeeld wordt toegevoegd en wordt verwijderd uit de winkelwagen moeten overdragen via). Met meerdere master kan Azure Cosmos DB verwerken dergelijke scenario's zonder problemen, met een overgang tussen actieve regio's en tegelijkertijd een consistente weergave van de gebruiker verwerkt. 

* **Fraude/Afwijkingsdetectie** -vaak toepassingen die controleren van de gebruiker of accountactiviteit worden globaal gedistribueerd en moet bijhouden meerdere gebeurtenissen tegelijkertijd. Tijdens het maken en onderhouden van scores voor een gebruiker, moeten acties uit verschillende geografische regio's scores zodat het risico metrieken inline tegelijkertijd bijwerken. Azure Cosmos DB kunt zorgen ontwikkelaars niet hebben voor het afhandelen van conflict scenario's op het toepassingsniveau van de. 

* **Samenwerking** - voor toepassingen die positie op basis van de populariteit van artikelen zoals goederen bij de verkoop of media om te worden verbruikt enzovoort. Populariteit bijhouden over geografische regio's ophalen ingewikkeld, met name wanneer royalties worden betaald of echte reclame beslissingen moeten kunnen worden uitgevoerd. Rangschikking, sorteren en rapportage over veel regio's wereldwijd in realtime met Azure Cosmos DB kan ontwikkelaars functies met weinig moeite en zonder gevaar op latenties te leveren. 

* **Softwarelicentiecontrole** - tellen en gebruik reguleren (zoals API-aanroepen transacties per seconde minuten gebruikt) globaal met eenvoud met behulp van Azure DB die Cosmos meerdere master kan worden geïmplementeerd. Ingebouwde conflictoplossing zorgt ervoor dat beide nauwkeurigheid van tellingen en regelgeving in realtime. 

* **Persoonlijke instellingen** - prijzen of u geografisch verspreide prestatiemeteritems die acties zoals loyaliteit activeren onderhoudt verwijst of persoonlijke gebruikerssessie implementeren bekijkt, hoge beschikbaarheid en vereenvoudigd geo-distributie telling verstrekt door Azure Cosmos DB, kunt hoge prestaties voor toepassingen leveren met eenvoud. 

## <a name="conflict-resolution-with-multi-master"></a>Conflictoplossing met meerdere master 

Met meerdere master is de uitdaging vaak dat twee (of meer) replica's van dezelfde record tegelijkertijd kunnen worden bijgewerkt door verschillende schrijvers in twee of meer verschillende regio's. Gelijktijdige schrijfbewerkingen kunnen leiden tot twee verschillende versies van dezelfde record en zonder ingebouwde conflictoplossing en de toepassing zelf conflictoplossing om op te lossen deze inconsistentie moet uitvoeren.  

**Voorbeeld** -gaan we ervan uit dat u van Azure DB die Cosmos gebruikmaakt als het persistentie-archief voor de toepassing van een webwinkel winkelwagen en deze toepassing wordt geïmplementeerd in twee gebieden: VS-Oost en VS-West.  Als er ongeveer op hetzelfde moment, een gebruiker in San Francisco een item toegevoegd aan de winkelwagen (bijvoorbeeld een boek) tijdens het proces van een inventaris in de VS-Oost, wordt een ander winkelwagen winkelwagen item (bijvoorbeeld een nieuw telefoonnummer) ongeldig voor die dezelfde gebruiker in reactie op een s upplier melding dat de releasedatum had vertraagd. Op tijdstip T1 maakt zijn de winkelwagen winkelwagen records in de twee regio's verschillend. De database wordt de replicatie en het mechanisme voor conflictoplossing gebruiken voor het omzetten van deze inconsistentie en uiteindelijk een van de twee versies van de winkelwagen worden geselecteerd. Met behulp van de conflict resolutie methodiek vaakst door meerdere masters databases (bijvoorbeeld laatste schrijven wins) is toegepast, is het onmogelijk voor de gebruiker of toepassing te voorspellen, welke versie wordt geselecteerd. In beide gevallen gegevens niet verloren of onverwachte problemen optreden. Als de versie van de regio Oost is geselecteerd, klikt u vervolgens de selectie van de gebruiker van een nieuwe aankoop item (dat wil zeggen, een boek) gaat verloren en of de regio West is geselecteerd, klikt u vervolgens het eerder gekozen item (dat wil zeggen, phone) is nog steeds in de winkelwagen. In beide gevallen informatie gaat verloren. Ten slotte winkelwagen een ander proces de winkelwagen te bekijken tussen de tijdstippen T1 en T2 zullen ook niet-deterministisch gedrag. Bijvoorbeeld een achtergrondproces dat het magazijn vervulling selecteert en de kosten van de back-upfunctie winkelwagen-updates zou leiden tot resultaten die met de uiteindelijke inhoud van de winkelwagen conflicteren. Als het proces wordt uitgevoerd in de regio West en alternatieve 1 realiteit wordt, zou deze de kosten van de back-upfunctie voor twee items compute, hoewel de winkelwagen binnenkort slechts één item, het rapport hebben mogelijk. 

Azure Cosmos DB implementeert de logica voor het verwerken van conflicterende schrijfbewerkingen in de database-engine zelf. Biedt Azure Cosmos DB **omzettingsondersteuning uitgebreide en flexibele conflict** door het aanbieden van verschillende conflict resolutie modellen, met inbegrip van automatische (het CRDT conflict gratis gerepliceerd gegevenstypen), laatste schrijven Wins (LWW) en aangepaste ( Opgeslagen Procedure) voor automatische conflictoplossing. De resolutie conflict modellen juist is en consistentie garanties bieden en de last van ontwikkelaars aan te denken consistentie, beschikbaarheid, prestaties, replicatielatentie en complexe combinaties van-gebeurtenissen onder geo-failovers verwijderen en regio-overschrijdende schrijven conflicten.  

  ![Mult-master conflictoplossing](./media/multi-region-writers/multi-master-conflict-resolution-blade.png)

Hebt u 3 soorten conflict resolutie modellen die worden aangeboden door Azure Cosmos DB. De semantiek van de conflict resolutie modellen zijn als volgt: 

**Automatische** -dit is het standaardbeleid voor de oplossing van conflicten. Dit beleid zorgt ervoor dat Azure Cosmos DB automatisch oplossen de conflicterende updates aan de serverzijde en sterke-uiteindelijke-consistentie wordt gegarandeerd. Azure Cosmos DB implementeert intern automatische conflictoplossing door gebruik van Conflict-vrij-gerepliceerd-gegevenstypen (CRDTs) in de database-engine.  

**Laatste schrijven Wins (LWW)** - tijdstempeleigenschap kiezen dit beleid kunt u bij het oplossen van conflicten op basis van het systeem gedefinieerde gesynchroniseerd of een aangepaste eigenschap is gedefinieerd voor de conflicterende versie van de records. De conflictoplossing gebeurt op de server en de versie met de meest recente tijdstempel voeren is geselecteerd.  

**Aangepaste** -u kunt een gedefinieerd conflict resolutie toepassingslogica registreren met het registreren van een opgeslagen procedure. De opgeslagen procedure wordt ophalen aangeroepen na detectie van update veroorzaakt een conflict in het kader van een databasetransactie op de server. Als u de optie selecteert, maar niet registreren van een opgeslagen procedure (of als de opgeslagen procedure er een uitzondering gegenereerd tijdens runtime), kunt u toegang tot alle van de conflicterende versies via de conflicten Feed en deze afzonderlijk kunt oplossen.  

## <a name="next-steps"></a>Volgende stappen  

In dit artikel vernomen u globaal gedistribueerd over meerdere master gebruiken met Azure Cosmos DB. Haal vervolgens een overzicht van de volgende bronnen: 

* [Meer informatie over hoe Azure Cosmos DB globale distributie ondersteunt](distribute-data-globally.md)  

* [Meer informatie over automatische failovers in Azure Cosmos-DB](regional-failover.md)  

* [Meer informatie over globale consistentie met Azure Cosmos-DB](consistency-levels.md)  

* Ontwikkelen met meerdere regio's met behulp van de Cosmos Azure DB - [SQL-API](tutorial-global-distribution-sql-api.md), [MongoDB API](tutorial-global-distribution-mongodb.md), of [tabel-API](tutorial-global-distribution-table.md)  

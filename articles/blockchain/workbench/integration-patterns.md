---
title: Slimme Contract integratiepatronen in Azure Blockchain Workbench
description: Overzicht van slimme contract integratiepatronen in Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/1/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 1e1bc16d32167d62d5f66f64bb383fcceeb79eb5
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54267144"
---
# <a name="smart-contract-integration-patterns"></a>Slimme contract integratiepatronen

Slimme contracten vertegenwoordigt vaak zakelijke werkstromen die moet worden geïntegreerd met externe systemen en apparaten.

De vereisten van deze werkstromen zijn nodig om transacties op een gedistribueerd grootboek met gegevens uit een extern systeem, service of een apparaat te starten. Ze moeten ook externe systemen die reageren op gebeurtenissen die afkomstig zijn van slimme contracten op een gedistribueerd grootboek hebben.

De REST-API en de messaging-integratie bieden de mogelijkheid voor zowel transacties in externe systemen verzenden naar slimme contracten opgenomen in een Azure Blockchain Workbench-toepassing, evenals het verzenden van meldingen van gebeurtenissen met externe systemen op basis van wijzigingen die plaats in een toepassing.

Voor scenario's voor gegevensintegratie bevat Azure Blockchain Workbench een set met databaseweergaven die een combinatie van transactionele gegevens van de blockchain en metagegevens over de toepassingen en slimme contracten samenvoegen.

Bovendien sommige scenario's, zoals die zijn gerelateerd aan Geef keten of media, moet mogelijk de integratie van documenten. Terwijl Azure Blockchain Workbench geen dat API-aanroepen biedt voor de verwerking van documenten rechtstreeks, kunnen documenten worden opgenomen in een Azure Blockchain-toepassing. Deze sectie bevat ook een patroon.

Deze sectie bevat de patronen geïdentificeerd voor de implementatie van elk van deze typen integraties in uw end-to-end-oplossingen.

## <a name="rest-api-based-integration"></a>REST API-integratie

Mogelijkheden in de web-App voor Azure Blockchain Workbench gegenereerd worden beschikbaar gesteld via de REST-API. Mogelijkheden zijn Azure Blockchain Workbench uploaden, configuratie en beheer van toepassingen, transacties te verzenden naar een gedistribueerd grootboek en het opvragen van metagegevens en grootboek toepassingsgegevens.

De REST-API wordt voornamelijk gebruikt voor interactieve clients, zoals web, mobiel, en bot-toepassingen.

Deze sectie kijken we patronen die zijn gericht op de aspecten van de REST-API die transacties verzenden naar een gedistribueerd grootboek en bronnen die gegevens over transacties via Azure Blockchain Workbench op te vragen *uitgeschakeld keten* SQL-database.

### <a name="sending-transactions-to-a-distributed-ledger-from-an-external-system"></a>Transacties van een extern systeem wilt verzenden naar een gedistribueerd grootboek

De REST-API van Azure Blockchain Workbench biedt de mogelijkheid om geverifieerde aanvragen voor het uitvoeren van transacties op een gedistribueerd grootboek te verzenden.

![Transacties wilt verzenden naar een gedistribueerd grootboek](./media/integration-patterns/send-transactions-ledger.png)

Dit gebeurt met behulp van het proces dat wordt weergegeven, waarbij:

-   De externe toepassing wordt geverifieerd met de Azure Active Directory ingericht als onderdeel van de Azure Blockchain Workbench-implementatie.
-   Geautoriseerde gebruikers ontvangen een bearer-token dat naar de API met aanvragen kan worden verzonden.
-   Externe toepassingen aanroepen naar de REST-API met behulp van het bearer-token.
-   De REST-API pakketten van de aanvraag als een bericht en verzendt ze naar de Service Bus. Hier wordt worden opgehaald, ondertekend en verzonden naar het juiste gedistribueerd grootboek.
-   De REST-API maakt een aanvraag voor Azure Blockchain Workbench SQL DB op te nemen van de aanvraag en de huidige status van inrichting tot stand brengen.
-   De SQL-database wordt de status van de inrichting en de API-aanroep retourneert de ID naar de externe toepassing die wordt aangeroepen.

### <a name="querying-blockchain-workbench-metadata-and-distributed-ledger-transactions"></a>Een query uitvoeren op Blockchain Workbench metagegevens en gedistribueerde transacties

De REST-API van Azure Blockchain Workbench biedt de mogelijkheid voor het verzenden van geverifieerde aanvragen query details met betrekking tot slimme contract kan worden uitgevoerd op een gedistribueerd grootboek.

![Een query uitvoeren op metagegevens](./media/integration-patterns/querying-metadata.png)

Dit gebeurt met behulp van het proces dat wordt weergegeven, waarbij:

1. De externe toepassing wordt geverifieerd met de Azure Active Directory ingericht als onderdeel van de Azure Blockchain Workbench-implementatie.
2. Geautoriseerde gebruikers ontvangen een bearer-token dat naar de API met aanvragen kan worden verzonden.
3. Externe toepassingen aanroepen naar de REST-API met behulp van het bearer-token.
4. De REST-API query uitgevoerd op de gegevens voor de aanvraag van de SQL-database en stuurt deze naar de client.

## <a name="messaging-integration"></a>Messaging-integratie

Integratie van berichten vereenvoudigt het uitvoeren van interactie met systemen, services en apparaten waarop een interactieve aanmelding niet mogelijk of wenselijk is is. Integratie van berichten is gericht op twee soorten berichten, die aanvragen dat transacties worden uitgevoerd op een gedistribueerd grootboek en gebeurtenissen die door beschikbaar worden gesteld dat grootboek wanneer transacties hebben plaatsgevonden.

Integratie van berichten is gericht op de uitvoering en bewaking van transacties met betrekking tot het maken van een gebruikersaccount, contract maken en uitvoeren van transacties van contracten en wordt voornamelijk gebruikt door *headless* back-endsystemen.

Deze sectie kijken we patronen die zijn gericht op de aspecten van de berichten gebaseerde API die transacties verzenden naar een gedistribueerd grootboek en die berichten voor gebeurtenissen die worden weergegeven door het onderliggende gedistribueerd grootboek vertegenwoordigen.

### <a name="one-way-event-delivery-from-a-smart-contract-to-an-event-consumer"></a>Eenzijdige gebeurtenisverzending uit een slimme contract aan een gebeurtenisconsumer 

In dit scenario wordt gebeurtenis een binnen een slimme contract, bijvoorbeeld een wijziging in de status of de uitvoering van een specifiek type transactie. Deze gebeurtenis wordt verzonden via een Event Grid downstream consumenten en consumenten vervolgens gepaste actie ondernemen.

Een voorbeeld van dit scenario is dat wanneer een transactie wordt uitgevoerd, een consument wilt worden gewaarschuwd en actie ondernemen kan, zoals de gegevens op te nemen in een SQL-database of in de Common Data Service. Dit is hetzelfde patroon dat Workbench volgt om in te vullen de *uitgeschakeld keten* SQL-database.

Een andere zou zijn als een slimme contract in een bepaalde status, bijvoorbeeld verandert wanneer een contract wordt weergegeven in een *OutOfCompliance*. Als deze statuswijziging gebeurt, kan er een waarschuwing moet worden verzonden naar de beheerder van een mobiele telefoon activeren.

![Eenzijdige gebeurtenisverzending](./media/integration-patterns/one-way-event-delivery.png)

Dit gebeurt met behulp van het proces dat wordt weergegeven, waarbij:

-   De intelligente contract verandert in een nieuwe status en stuurt een gebeurtenis naar het grootboek.
-   Het grootboek ontvangt en de gebeurtenis levert aan Azure Blockchain Workbench.
-   Azure Blockchain Workbench is geabonneerd op gebeurtenissen uit het grootboek en ontvangt de gebeurtenis.
-   Azure Blockchain Workbench publiceert de gebeurtenis voor de Event Grid-abonnees.
-   Externe systemen zijn geabonneerd op de Event Grid, het bericht in beslag nemen en nemen de juiste actie (s).

## <a name="one-way-event-delivery-of-a-message-from-an-external-system-to-a-smart-contract"></a>Eenzijdige bezorging van gebeurtenissen van een bericht van een extern systeem op een slimme contract

Er is ook een scenario dat van de tegengestelde richting stromen. In dit geval een gebeurtenis wordt gegenereerd door een sensor of een extern systeem en de gegevens van die gebeurtenis moet worden verzonden naar een slimme contract.

Een veelvoorkomend voorbeeld is de levering van gegevens van financiële markten, bijvoorbeeld: prijzen van grondstoffen, voorraad of obligaties, op een slimme contract.

### <a name="direct-delivery-of-an-azure-blockchain-workbench-in-the-expected-format"></a>Directe levering van een Azure Blockchain Workbench de verwachte indeling

Sommige toepassingen zijn gebouwd om te integreren met Azure Blockchain Workbench en rechtstreeks worden gegenereerd en het verzenden van berichten in de verwachte indelingen.

![Directe levering](./media/integration-patterns/direct-delivery.png)

Dit gebeurt met behulp van het proces dat wordt weergegeven, waarbij:

-   Er treedt een gebeurtenis op in een extern systeem waarmee het maken van een bericht voor Azure Blockchain Workbench wordt geactiveerd.
-   Het externe systeem heeft van code geschreven naar het maken van dit bericht in een bekende indeling en verzendt dit rechtstreeks naar de Service Bus.
-   Azure Blockchain Workbench is geabonneerd op gebeurtenissen van de Service Bus en haalt het bericht.
-   Azure Blockchain Workbench initieert een aanroep naar de posten, verzenden van gegevens van het externe systeem naar een specifieke opdracht.
-   Na ontvangst van het bericht het contract status verandert in een nieuwe.

### <a name="delivery-of-a-message-in-a-format-unknown-to-azure-blockchain-workbench"></a>Een bericht in een onbekende indeling te leveren aan Azure Blockchain Workbench

Sommige systemen worden niet gewijzigd om berichten te leveren in de standard-indelingen die worden gebruikt door Azure Blockchain Workbench. In deze gevallen, de al aanwezige mechanismen en het bericht kunnen vaak indelingen uit deze systemen worden gebruikt. Met name kunnen de systeemeigen berichttypen van deze systemen omgezet met behulp van Logic Apps, Azure Functions of andere aangepaste code om toe te wijzen aan een van de standard messaging indelingen verwacht.

![Onbekende indeling](./media/integration-patterns/unknown-message-format.png)

Dit gebeurt met behulp van het proces dat wordt weergegeven, waarbij:

-   Er treedt een gebeurtenis op in een extern systeem waarmee het maken van een bericht wordt geactiveerd.
-   Een logische App of aangepaste code wordt gebruikt voor het ontvangen bericht en transformeren naar een standaard Azure Blockchain Workbench opgemaakte bericht.
-   De logische App verzendt het getransformeerde bericht rechtstreeks naar de Service Bus.
-   Azure Blockchain Workbench is geabonneerd op gebeurtenissen van de Service Bus en haalt het bericht.
-   Azure Blockchain Workbench initieert een aanroep naar de posten, verzenden van gegevens van het externe systeem naar een specifieke functie in het contract.
-   De functie wordt uitgevoerd en meestal wijzigt de status. De wijziging van de status wordt verplaatst naar voren de bedrijfswerkstroom uit weerspiegeld in de slimme contract, het inschakelen van andere functies moet nu worden uitgevoerd waar nodig.

### <a name="transitioning-control-to-an-external-process-and-await-completion"></a>Overstappen van besturingselement in op een externe verwerken en te wachten op voltooiing

Er zijn scenario's waarbij een slimme contract interne uitvoering en lever aan een extern proces stoppen moet. Verzend een bericht naar de intelligente contract en de uitvoering, dat is dat vervolgens extern proces voltooien zou, zou vervolgens blijven binnen de slimme contract.

#### <a name="transition-to-the-external-process"></a>Overgang naar het externe proces

Dit patroon wordt gewoonlijk geïmplementeerd met behulp van de volgende benadering:

-   De intelligente contract overgangen naar een specifieke status. In deze status kunnen beide geen of een beperkt aantal functies kan worden uitgevoerd totdat een extern systeem een gewenste actie onderneemt.
-   De wijziging van de status is opgehaald als een gebeurtenis aan een downstream consument.
-   De downstream consument ontvangt van de gebeurtenis en wordt de uitvoering van externe code.

![Overgang besturingselement Extern proces](./media/integration-patterns/transition-external-process.png)

#### <a name="return-of-control-from-the-smart-contract"></a>Opbrengst van besturingselement van het smart contract

Afhankelijk van de mogelijkheid tot het aanpassen van het externe systeem mogelijk of misschien niet mogelijk om berichten te bezorgen in een van de standard-indelingen die Azure Blockchain Workbench verwacht. Op basis van de mogelijkheid van externe systemen voor het genereren van een van deze berichten, bepaalt welke van de volgende twee geretourneerde paden moeten worden ondernomen.

##### <a name="direct-delivery-of-an-azure-blockchain-workbench-in-the-expected-format"></a>Directe levering van een Azure Blockchain Workbench de verwachte indeling

![](./media/integration-patterns/direct-delivery.png)

In dit model, de communicatie met het contract en de wijziging in de volgende status na de bovenstaande treedt op wanneer - verwerken

-   Wanneer u hebt voltooid of een specifieke mijlpaal in de externe code worden uitgevoerd, wordt een gebeurtenis verzonden naar Service Bus is verbonden met Azure Blockchain Workbench.

-   Voor systemen die rechtstreeks kunnen niet worden aangepast voor het schrijven van een bericht dat voldoet aan de verwachtingen van de API, wordt deze omgezet.

-   De inhoud van het bericht is verpakt en verzonden naar een specifieke functie op de slimme contract. Dit wordt gedaan namens de gebruiker die is gekoppeld aan het externe systeem.

-   De functie wordt uitgevoerd en doorgaans de status wordt gewijzigd. De wijziging van de status wordt verplaatst naar voren de bedrijfswerkstroom uit weerspiegeld in de slimme contract, het inschakelen van andere functies moet nu worden uitgevoerd waar nodig.

### 

### <a name="delivery-of-a-message-in-a-format-unknown-to-azure-blockchain-workbench"></a>Een bericht in een onbekende indeling te leveren aan Azure Blockchain Workbench

![Onbekende indeling](./media/integration-patterns/unknown-message-format.png)

In dit model waarbij een bericht in een standaardindeling kan niet worden verzonden rechtstreeks, de communicatie met het contract en volgende statuswijziging optreedt nadat de bovenstaande waar verwerken:

1.  Wanneer u hebt voltooid of een specifieke mijlpaal in de externe code worden uitgevoerd, wordt een gebeurtenis verzonden naar Service Bus is verbonden met Azure Blockchain Workbench.
2.  Een logische App of aangepaste code wordt gebruikt voor het ontvangen bericht en transformeren naar een standaard Azure Blockchain Workbench opgemaakte bericht.
3.  De logische App verzendt het getransformeerde bericht rechtstreeks naar de Service Bus.
4.  Azure Blockchain Workbench is geabonneerd op gebeurtenissen van de Service Bus en haalt het bericht.
5.  Azure Blockchain Workbench initieert een aanroep naar de posten, verzenden van gegevens van het externe systeem naar een specifieke opdracht.
6. De inhoud van het bericht is verpakt en verzonden naar een specifieke functie op de slimme contract. Dit wordt gedaan namens de gebruiker die is gekoppeld aan het externe systeem.
7.  De functie wordt uitgevoerd en doorgaans de status wordt gewijzigd. De wijziging van de status wordt verplaatst naar voren de bedrijfswerkstroom uit weerspiegeld in de slimme contract, het inschakelen van andere functies moet nu worden uitgevoerd waar nodig.

## <a name="iot-integration"></a>IoT-integratie

Een veelvoorkomend integratiescenario is het opnemen van telemetrische gegevens van sensoren in een slimme contract opgehaald. Op basis van gegevens die worden geleverd door sensors, kunnen slimme contracten maatregelen nemen op de hoogte en de status van de opdracht alter.

Bijvoorbeeld, als een vrachtwagen leveren diergeneeskunde had de temperatuur Zweef 110 graden, deze mogelijk van invloed op de effectiviteit van de arts kan ertoe leiden dat een probleem met de openbare veiligheid als dat niet gedetecteerd en verwijderd uit de toeleveringsketen. Als een stuurprogramma zijn auto tot 100 mijl per uur versnelde, kan de resulterende sensorinformatie bij een annulering van de verzekering door zijn verzekering provider activeren. Als de auto een auto verhuur is, kan de GPS-gegevens aangeven wanneer het stuurprogramma is verkeerd buiten een geografisch gebied zijn verhuur overeenkomst vallen en de kosten in rekening gebracht een.

De uitdaging is dat dit soort sensoren kunnen gegevens worden leveren op basis van een constante en het is niet geschikt is voor al deze gegevens verzenden naar een slimme contract. Een gebruikelijke aanpak is het beperken van het aantal berichten verzonden naar de blockchain tijdens het bezorgen van alle berichten naar een secundaire opslag. Bijvoorbeeld berichten worden verzonden op basis van alleen vaste interval, bijvoorbeeld één keer per uur, en wanneer een ingesloten waarde valt buiten een overeengekomen ontvangen van buiten het bereik van een slimme contract. Controleren van waarden die buiten tolerantie vallen, zorgt ervoor dat de gegevens die relevant zijn voor de bedrijfslogica contracten ontvangen en uitgevoerd. Controle van de waarde op basis van het interval wordt bevestigd dat de sensor nog steeds rapporteert. Alle gegevens worden verzonden naar een secundaire reporting store om in te schakelen bredere rapportage, analyses en machine learning. Bij het ophalen van sensorwaarden, serverlogs, voor GPS kan niet worden vereist per minuut voor een slimme contract, kunnen ze bijvoorbeeld interessante gegevens om te worden gebruikt in rapporten of toewijzing routes leveren.

Op de Azure-platform, wordt integratie met apparaten meestal gedaan met IoT Hub. IoT Hub biedt de mogelijkheid om te routeren van berichten op basis van inhoud, en kunt het type van de functionaliteit die hierboven worden beschreven.

![IoT-berichten](./media/integration-patterns/iot.png)

De bovenstaande procedure ziet u een patroon voor dit wordt geïmplementeerd:

-   Een apparaat communiceert rechtstreeks of via een veldgateway met IoT Hub.
-   IoT Hub ontvangt de berichten en evalueert de berichten op basis van routes tot stand gebracht dat de inhoud van het bericht, bijvoorbeeld controleren. *Rapporteert de sensor een temperatuur van meer dan 50 graden?*
-   De IoT-Hub verzendt-berichten die voldoen aan de criteria voor een opgegeven Service Bus voor de route.
-   Een logische App of een andere code worden voor de route naar de Service Bus, waardoor IoT-Hub tot stand heeft gebracht luistert.
-   De logische App of een andere code worden opgehaald en transformatie van het bericht naar een bekende indeling.
-   De getransformeerde bericht, wordt nu in een standaardindeling verzonden naar Service Bus voor Azure Blockchain Workbench.
-   Azure Blockchain Workbench is geabonneerd op gebeurtenissen van de Service Bus en haalt het bericht.
-   Azure Blockchain Workbench initieert een aanroep naar de posten, verzenden van gegevens van het externe systeem naar een specifieke opdracht.
-   Na ontvangst van het bericht het contract wordt geëvalueerd als de gegevens en kan invloed op de status op basis van het resultaat van deze evaluatie, bijvoorbeeld voor een hoge temperatuur, de status om te wijzigen *van naleving*.

## <a name="data-integration"></a>Gegevensintegratie

Naast REST en berichten gebaseerde API biedt Azure Blockchain Workbench ook toegang tot een SQL-database gevuld met de toepassing en contract-meta-gegevens, evenals transactionele gegevens van gedistribueerde grootboeken.

![Gegevensintegratie](./media/integration-patterns/data-integration.png)

De integratie van gegevens is erg bekend is:

-   Azure Blockchain Workbench slaat metagegevens op over toepassingen, werkstromen, contracten en transacties als onderdeel van het normale gedrag kan worden uitgevoerd.
-   Externe systemen of -hulpprogramma's bieden een of meer dialoogvensters voor het verzamelen van informatie over de database, zoals de database-servernaam, databasenaam, type verificatie, aanmeldingsreferenties en welke database weergaven gebruiken in het kader.
-   Query's zijn ontwikkeld op basis van SQL database-weergaven om downstream verbruik mogelijk te maken met externe systemen, services, rapportage, ontwikkelhulpprogramma's en hulpprogramma's voor productiviteit enterprise.

## <a name="storage-integration"></a>Opslagintegratie van

Veel scenario's mogelijk de noodzaak om op te nemen attestable bestanden. Om meerdere redenen, kan deze worden niet geschikt is voor de bestanden op een blockchain plaatst. In plaats daarvan is een veelgebruikte manier voor het uitvoeren van een cryptografische hash (bijvoorbeeld, SHA-256) op basis van een bestand en delen die hash van een gedistribueerde grootboek. De hash opnieuw op een later tijdstip uitvoeren, moet hetzelfde resultaat retourneren. Als het bestand is gewijzigd, zelfs als slechts één pixel in een installatiekopie wordt gewijzigd, wordt de hash een andere waarde geretourneerd.

![Opslagintegratie van](./media/integration-patterns/storage-integration.png)

Het patroon kan worden geïmplementeerd waarbij:

-   Een extern systeem zich blijft voordoen met een bestand in een opslagmechanisme voor, zoals Azure Storage.
-   Een hash wordt gegenereerd met het bestand of het bestand en bijbehorende metagegevens, zoals een id voor de eigenaar van de URL waar het bestand zich bevindt, enzovoort.
-   De hash en alle metagegevens wordt verzonden naar een functie op een slimme contract, zoals *FileAdded*
-   In de toekomst kunnen het bestand en de metagegevens worden hashing opnieuw en vergeleken met de waarden die zijn opgeslagen op het grootboek.

## <a name="prerequisites-for-implementing-integration-patterns-using-the-rest-and-message-apis"></a>Vereisten voor het implementeren van de van integratiepatronen met behulp van de REST en het bericht API 's

Om te vergemakkelijken de mogelijkheid voor een extern systeem of een apparaat om te communiceren met de slimme overeenkomst met de REST- of bericht API, moet het volgende gebeuren:

1. In de Azure Active Directory voor het consortium, een account gemaakt die overeenkomt met een extern systeem of het apparaat.
2. Contract/de juiste slimme contracten voor uw Azure Blockchain Workbench-toepassing hebt functies die zijn gedefinieerd voor het accepteren van gebeurtenissen uit uw extern systeem of het apparaat.
3. Het configuratiebestand van de toepassing voor uw slimme contract bevat de functie, die het systeem of het apparaat wordt toegewezen.
4. Het configuratiebestand van de toepassing voor uw slimme contract aangeeft in welke statussen dat deze functie kan worden aangeroepen in de gedefinieerde rol.
5. Het configuratiebestand van de toepassing en de slimme contracten worden geüpload naar Azure Blockchain Workbench.

Zodra de toepassing is geüpload, wordt de Azure Active Directory-account voor het externe systeem is toegewezen aan het contract en de bijbehorende functieservices.

## <a name="testing-external-system-integration-flows-prior-to-writing-integration-code"></a>Extern systeem Integratieflows voorafgaand aan het schrijven van integratiecode testen 

Biedt de mogelijkheid om te integreren met externe systemen is een basisvereiste van veel scenario's. Is het wenselijk om te worden gevalideerd slimme contract ontwerp voorgaande of parallel uitvoeren voor de ontwikkeling van de code voor integratie met externe systemen.

Het gebruik van Azure Active Directory (Azure AD) kan aanzienlijk versnellen productiviteit van ontwikkelaars en het tijdstip op waarde. Met name een essentieel hoeveelheid tijd kan duren voordat de code-integratie met een extern systeem. Met behulp van Azure AD en de automatische generatie van UX door Azure Blockchain Workbench, kan dit kunnen ontwikkelaars Meld u aan bij Workbench als dat externe systeem en statuswaarden verwacht van dat externe systeem via het UX. vullen Hiermee wordt voor het snel ontwikkelen en valideren van ideeën in een conceptontwerpomgeving ofwel voorafgaand aan of parallel met van integratiecode voor de externe systemen worden geschreven.

---
title: Contract integratie patronen in Azure Blockchain Workbench van smartcard
description: Overzicht van de smartcard contract integratie patronen in Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/1/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: a6a44e30fe58617b43c5491a72fc882015bc9591
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="smart-contract-integration-patterns"></a>Smart contract integratie patronen

Smart contracten vertegenwoordigt vaak een zakelijke werkstroom die nodig zijn voor de integratie met externe systemen en apparaten.

De vereisten van deze werkstromen bevatten een nodig om op een gedistribueerde grootboek transacties die gegevens uit een extern systeem, de service of het apparaat bevatten te starten. Ze moeten ook naar het externe systemen reageren op gebeurtenissen die afkomstig zijn van een gedistribueerde grootboek smart contracten hebben.

De REST-API en de messaging-integratie kunnen u zowel transacties in externe systemen verzenden naar smart contracten die zijn opgenomen in een toepassing Azure Blockchain Workbench, evenals gebeurtenismeldingen verzenden naar externe systemen op basis van wijzigingen die worden uitgevoerd plaats in een toepassing.

Voor gegevens integratiescenario's bevat Azure Blockchain Workbench een aantal weergaven die een combinatie van transactionele gegevens van de blockchain en metagegevens over de toepassingen en slimme contracten samenvoegen.

Daarnaast kunnen sommige scenario's, zoals die betrekking hebben op te geven keten of media, ook de integratie van documenten vereisen. Terwijl Azure Blockchain Workbench niet in dat API-aanroepen voorziet voor het verwerken van documenten rechtstreeks kunnen documenten worden opgenomen in een Azure-Blockchain-toepassing. Deze sectie bevat ook een patroon.

Deze sectie bevat de patronen die zijn geïdentificeerd voor de implementatie van elk van deze typen integraties in uw end-to-end-oplossingen.

## <a name="rest-api-based-integration"></a>REST API gebaseerde integratie

Mogelijkheden van de webtoepassing Azure Blockchain Workbench gegenereerd beschikbaar worden gesteld via de REST-API. Dit omvat Azure Blockchain Workbench uploaden, configuratie en beheer van toepassingen, transacties wilt verzenden naar een gedistribueerde grootboek en het opvragen van metagegevens en grootboek toepassingsgegevens.

De REST-API wordt voornamelijk gebruikt voor interactieve-clients, zoals webtoepassingen, mobiele en bot-toepassingen.

Deze sectie wordt gekeken naar patronen die zijn gericht op de aspecten van de REST-API die transacties verzenden naar een gedistribueerde grootboek die en een query over gegevens over transacties vanuit Azure Blockchain Workbench *uitgeschakeld keten* SQL-database.

### <a name="sending-transactions-to-a-distributed-ledger-from-an-external-system"></a>Transacties wilt verzenden naar een gedistribueerde grootboek uit een extern systeem

De REST-API van Azure Blockchain Workbench biedt de mogelijkheid voor het verzenden van geverifieerde aanvragen voor transacties op een gedistribueerde grootboek uitvoeren.

![Transacties wilt verzenden naar een gedistribueerde grootboek](media/blockchain-workbench-integration-patterns/send-transactions-ledger.png)

Dit gebeurt met behulp van het proces afgebeeld hierboven, waarbij:

-   De externe toepassing geverifieerd op de Azure Active Directory ingericht als onderdeel van de Azure Blockchain Workbench-implementatie.
-   Geautoriseerde gebruikers ontvangen een bearer-token met aanvragen kan worden verzonden naar de API.
-   Externe toepassingen maken aanroepen naar de REST-API met behulp van de bearer-token.
-   De REST-API pakketten van de aanvraag als een bericht en verzendt het naar de Service Bus. Hier worden het opgehaald, ondertekend, en verzonden naar het juiste gedistribueerde grootboek.
-   De REST-API doet een aanvraag bij Azure Blockchain Workbench SQL DB vastleggen van de aanvraag en tot stand brengen van de huidige status van de inrichting.
-   De SQL-database wordt de status van de inrichting en de API-aanroep retourneert de ID voor de externe toepassing die wordt aangeroepen.

### <a name="querying-blockchain-workbench-metadata-and-distributed-ledger-transactions"></a>Uitvoeren van query's Blockchain Workbench metagegevens en gedistribueerde grootboektransacties

De REST-API van Azure Blockchain Workbench biedt de mogelijkheid geverifieerde aanvragen verzenden naar Querydetails van de gerelateerd aan smart contract uitvoering in een gedistribueerde grootboek.

![Opvragen van metagegevens](media/blockchain-workbench-integration-patterns/querying-metadata.png)

Dit gebeurt met behulp van het proces afgebeeld hierboven, waarbij:

1. De externe toepassing geverifieerd op de Azure Active Directory ingericht als onderdeel van de Azure Blockchain Workbench-implementatie.
2. Geautoriseerde gebruikers ontvangen een bearer-token met aanvragen kan worden verzonden naar de API.
3. Externe toepassingen maken aanroepen naar de REST-API met behulp van de bearer-token.
4. De REST-API bevraagt de gegevens voor de aanvraag van de SQL-database en stuurt deze naar de client.

## <a name="messaging-integration"></a>Messaging-integratie

Messaging-integratie vereenvoudigt interactie met systemen, services en waarop een interactieve aanmelding niet mogelijk of wenselijk is apparaten. Messaging-integratie is gericht op twee soorten berichten die aanvragen dat transacties worden uitgevoerd op een gedistribueerde grootboek en gebeurtenissen die door beschikbaar worden gesteld die grootboek wanneer transacties hebben plaatsgevonden.

Messaging-integratie is gericht op de uitvoering en bewaking van transacties betrekking hebben op het maken van een gebruikersaccount, contract maken en de uitvoering van transacties op contracten en wordt voornamelijk gebruikt door *headless* back-end-systemen.

Deze sectie wordt bekeken patronen die zijn gericht op de aspecten van de API op basis van berichten die transacties verzenden naar een gedistribueerde grootboek die en event-berichten die worden weergegeven door het onderliggende gedistribueerde grootboek vertegenwoordigen.

### <a name="one-way-event-delivery-from-a-smart-contract-to-an-event-consumer"></a>Eenzijdige gebeurtenis levering van een smartcard contract aan een gebeurtenisconsumer 

In dit scenario wordt een gebeurtenis plaatsvindt binnen een intelligente contract, bijvoorbeeld een statuswijziging of de uitvoering van een specifiek type transactie. Deze gebeurtenis wordt verzonden via een raster gebeurtenis downstream-gebruikers en consumenten en passende maatregelen.

Een voorbeeld van dit scenario is dat wanneer een transactie wordt uitgevoerd, een consumer wilt worden gewaarschuwd en een actie uitvoert kan, zoals het vastleggen van de informatie in een SQL-database of de algemene Data-Service. Dit is hetzelfde patroon die Workbench volgt om te voorzien van de *uitgeschakeld keten* SQL-database.

Een andere zou zijn als een smartcard contract naar een bepaalde status, bijvoorbeeld overgezet wanneer een contract wordt weergegeven in een *OutOfCompliance*. Als deze wijziging in de status gebeurt, kan het activeren van een waarschuwing moet worden verzonden naar een beheerder mobiele telefoon.

![Eenzijdige gebeurtenis levering](media/blockchain-workbench-integration-patterns/one-way-event-delivery.png)

Dit gebeurt met behulp van het proces afgebeeld hierboven, waarbij:

-   De smartcard contract overgezet naar een nieuwe status en verzendt een gebeurtenis naar het grootboek.
-   De grootboek ontvangt en levert de gebeurtenis voor Azure Blockchain Workbench.
-   Azure Blockchain Workbench is geabonneerd op gebeurtenissen uit het grootboek en de gebeurtenis ontvangt.
-   De gebeurtenis publiceert Azure Blockchain Workbench naar abonnees op het raster gebeurtenis.
-   Externe systemen bent geabonneerd op het raster gebeurtenis, gebruiken het bericht en de juiste acties ondernemen.

## <a name="one-way-event-delivery-of-a-message-from-an-external-system-to-a-smart-contract"></a>Eenzijdige gebeurtenis levering van een bericht uit een extern systeem naar een smartcard contract

Er is ook een scenario overlopen in de tegengestelde richting. In dit geval wordt een gebeurtenis wordt gegenereerd door een sensor of een extern systeem en de gegevens van die gebeurtenis moet worden verzonden naar een smartcard contract.

Een veelvoorkomend voorbeeld is de levering van gegevens van financiële markten, bijvoorbeeld: prijzen van grondstoffen, stock of obligaties, naar een smartcard contract.

### <a name="direct-delivery-of-an-azure-blockchain-workbench-in-the-expected-format"></a>Directe levering van een Azure Blockchain Workbench de verwachte indeling

Sommige toepassingen om te integreren met Azure Blockchain Workbench zijn ingebouwd en rechtstreeks genereert en verzenden van berichten in de verwachte indeling.

![Directe levering](media/blockchain-workbench-integration-patterns/direct-delivery.png)

Dit gebeurt met behulp van het proces afgebeeld hierboven, waarbij:

-   Er treedt een gebeurtenis op in externe systemen die het maken van een bericht voor Azure Blockchain Workbench activeert.
-   Het externe systeem code die is geschreven voor het maken van dit bericht in een bekende indeling heeft en dit rechtstreeks naar de Service Bus verzendt.
-   Azure Blockchain Workbench is geabonneerd op gebeurtenissen van de Service Bus en haalt het bericht.
-   Azure Blockchain Workbench initieert een aanroep naar de posten, verzenden van gegevens van het externe systeem naar een specifieke contract.
-   Bij ontvangst van het bericht het contract status verandert in een nieuwe.

### <a name="delivery-of-a-message-in-a-format-unknown-to-azure-blockchain-workbench"></a>Levering van berichten in een onbekende indeling van een aan Azure Blockchain Workbench

Sommige systemen kunnen niet worden gewijzigd voor het leveren van berichten in de standard-indelingen die worden gebruikt door Azure Blockchain Workbench. In deze gevallen bestaande mechanismen en bericht worden indelingen van deze systemen vaak gebruikt. De systeemeigen berichttypen van deze systemen kunnen in het bijzonder worden omgezet met behulp van Logic Apps, Azure Functions of andere aangepaste code toewijzen aan een van de standaard messaging indelingen verwacht.

![Onbekend bericht-indeling](media/blockchain-workbench-integration-patterns/unknown-message-format.png)

Dit gebeurt met behulp van het proces afgebeeld hierboven, waarbij:

-   Er treedt een gebeurtenis op in een extern systeem waarmee het maken van een bericht wordt geactiveerd.
-   Een logische App of een aangepaste code wordt gebruikt voor het ontvangen bericht en transformeren naar een standaardbericht voor Azure Blockchain Workbench opgemaakt.
-   De logische App verzendt het getransformeerde bericht rechtstreeks naar de Service Bus.
-   Azure Blockchain Workbench is geabonneerd op gebeurtenissen van de Service Bus en haalt het bericht.
-   Azure Blockchain Workbench initieert een aanroep naar de posten, verzenden van gegevens van het externe systeem naar een specifieke functie in het contract.
-   De functie wordt uitgevoerd en meestal wijzigt de status. De wijziging van de status wordt voorwaarts verplaatst de workflow binnen het bedrijf worden weergegeven in het smart contract, het inschakelen van andere functies kunnen nu worden uitgevoerd naar gelang van toepassing.

### <a name="transitioning-control-to-an-external-process-and-await-completion"></a>Besturingselement op een externe overgang verwerken en wachten op voltooiing

Er zijn scenario's waarbij een intelligente contract interne uitvoering en lever aan een extern proces stoppen moet. Dat vervolgens extern proces voltooien zou, een bericht op de smartcard contract en de uitvoering verzenden zou gaat u verder binnen de slimme contract.

#### <a name="transition-to-the-external-process"></a>Overgang naar het externe proces

Dit patroon wordt gewoonlijk geïmplementeerd met behulp van de volgende benadering:

-   De smartcard contract overgangen naar een specifieke status. In deze toestand is, geen of een beperkt aantal functies kan worden uitgevoerd totdat een extern systeem een gewenste actie neemt.
-   De wijziging van de status is opgehaald als een gebeurtenis met een consumer downstream.
-   De consument downstream ontvangt van de gebeurtenis en wordt de uitvoering van de externe code.

![Besturingselement van de overgang naar een extern proces](media/blockchain-workbench-integration-patterns/transition-external-process.png)

#### <a name="return-of-control-from-the-smart-contract"></a>Retourneren van besturingselement van het contract van de smartcard

Afhankelijk van de mogelijkheden voor het aanpassen van het externe systeem mogelijk of misschien niet mogelijk om berichten te bezorgen in een van de standaard-indelingen die Azure Blockchain Workbench verwacht. Op basis van de externe systemen mogelijkheid voor het genereren van een van deze berichten, bepalen welke van de volgende twee return paden moeten worden ondernomen.

##### <a name="direct-delivery-of-an-azure-blockchain-workbench-in-the-expected-format"></a>Directe levering van een Azure Blockchain Workbench de verwachte indeling

![](media/blockchain-workbench-integration-patterns/direct-delivery.png)

In dit model, de communicatie met het contract en de wijziging in de volgende status deze gebeurtenis treedt op na het bovenstaande where - verwerken

-   Bij het bereiken van de voltooiing of een specifieke mijlpaal in de externe code worden uitgevoerd, wordt een gebeurtenis verzonden naar de Service Bus is verbonden met Azure Blockchain Workbench.

-   Voor systemen die rechtstreeks kunnen niet worden aangepast voor het schrijven van een bericht dat voldoet aan de verwachtingen van de API, wordt deze omgezet.

-   De inhoud van het bericht wordt verpakt en verzonden naar een specifieke functie voor de smartcard contract. Dit wordt gedaan namens de gebruiker die is gekoppeld aan het externe systeem.

-   De functie wordt uitgevoerd en doorgaans de status wordt gewijzigd. De wijziging van de status wordt voorwaarts verplaatst de workflow binnen het bedrijf worden weergegeven in het smart contract, het inschakelen van andere functies kunnen nu worden uitgevoerd naar gelang van toepassing.

### 

### <a name="delivery-of-a-message-in-a-format-unknown-to-azure-blockchain-workbench"></a>Levering van berichten in een onbekende indeling van een aan Azure Blockchain Workbench

![Onbekend bericht-indeling](media/blockchain-workbench-integration-patterns/unknown-message-format.png)

In dit model waarbij een bericht in een standaardindeling kan niet worden verzonden rechtstreeks de communicatie met het contract en latere statuswijziging optreedt nadat de bovenstaande verwerken waar:

1.  Bij het bereiken van de voltooiing of een specifieke mijlpaal in de externe code worden uitgevoerd, wordt een gebeurtenis verzonden naar de Service Bus is verbonden met Azure Blockchain Workbench.
2.  Een logische App of een aangepaste code wordt gebruikt voor het ontvangen bericht en transformeren naar een standaardbericht voor Azure Blockchain Workbench opgemaakt.
3.  De logische App verzendt het getransformeerde bericht rechtstreeks naar de Service Bus.
4.  Azure Blockchain Workbench is geabonneerd op gebeurtenissen van de Service Bus en haalt het bericht.
5.  Azure Blockchain Workbench initieert een aanroep naar de posten, verzenden van gegevens van het externe systeem naar een specifieke contract.
6. De inhoud van het bericht wordt verpakt en verzonden naar een specifieke functie voor de smartcard contract. Dit wordt gedaan namens de gebruiker die is gekoppeld aan het externe systeem.
7.  De functie wordt uitgevoerd en doorgaans de status wordt gewijzigd. De wijziging van de status wordt voorwaarts verplaatst de workflow binnen het bedrijf worden weergegeven in het smart contract, het inschakelen van andere functies kunnen nu worden uitgevoerd naar gelang van toepassing.

## <a name="iot-integration"></a>IoT-integratie

Een veelvoorkomend integratiescenario is het opnemen van telemetriegegevens die zijn opgehaald van sensoren die een overeenkomst met een smartcard. Gebaseerd op gegevens die worden geleverd door sensoren, kunnen smart contracten gefundeerde acties kunnen uitvoeren en de status van de opdracht alter.

Bijvoorbeeld, als een vrachtwagen leveren arts had de temperatuur Zweef en 110 graden, deze mogelijk van invloed op de effectiviteit van de arts kan ertoe leiden dat een probleem met de openbare veiligheid als dat niet gedetecteerd en verwijderd uit de toeleveringsketen. Als een stuurprogramma zijn auto tot 100 mijl per uur versnelde, kan de resulterende sensorinformatie een annulering van verzekering door zijn verzekering provider activeert. De auto is een verhuur auto, kan GPS-gegevens aangeven wanneer het stuurprogramma is gegaan buiten een Geografie zijn overeenkomst verhuur vallen als een kosten in rekening gebracht.

De uitdaging is dat deze sensoren kunnen worden consistente gegevens op basis van constante en niet geschikt is is voor al deze gegevens verzenden naar een smartcard contract. Een typische aanpak is te beperken het aantal berichten dat is verzonden naar de blockchain tijdens het bezorgen van alle berichten naar een secundaire opslag. Berichten ontvangen met alleen vast interval, bijvoorbeeld één keer per uur, en wanneer een ingesloten waarde valt buiten een overeengekomen bijvoorbeeld leveren bij bereik voor een smartcard contract. Waarden die buiten toleranties vallen controleren, zorgt u ervoor dat de gegevens die relevant zijn voor de bedrijfslogica contracten is ontvangen en uitgevoerd. Controleren of de waarde met het interval wordt bevestigd dat de sensor nog steeds rapporteert. Alle gegevens worden verzonden naar een secundaire reporting store om in te schakelen breder rapportage, analytics en machine learning. Tijdens het ophalen van sensormetingen voor GPS niet mogelijk vereist elke minuut voor een smartcard contract, kunnen deze bijvoorbeeld interessante gegevens worden gebruikt in rapporten of toewijzing routes opgeven.

Op het Azure-platform-integratie met apparaten gewoonlijk wordt uitgevoerd met IoT Hub. IoT Hub biedt de mogelijkheid te routeren van berichten op basis van inhoud en het type van de hierboven beschreven functionaliteit maakt.

![IoT-berichten](media/blockchain-workbench-integration-patterns/iot.png)

De bovenstaande procedure ziet u een patroon voor dit is geïmplementeerd:

-   Een apparaat communiceert rechtstreeks of via een veldgateway met IoT Hub.
-   IoT Hub berichten ontvangt en de berichten op basis van routes tot stand gebracht evalueert die bijvoorbeeld Controleer de inhoud van het bericht. *Rapporteert de sensor een groter is dan 50 graden temperatuur?*
-   De IoT Hub verzendt berichten die voldoen aan de criteria voor een gedefinieerde Service Bus voor de route.
-   Een logische App of andere code luistert naar de Service Bus die IoT Hub heeft gemaakt voor de route.
-   De logische App of andere code haalt en transformatie van het bericht naar een bekende indeling.
-   Het bericht de getransformeerde nu in een standaardindeling verzonden naar de Service Bus voor Azure Blockchain Workbench.
-   Azure Blockchain Workbench is geabonneerd op gebeurtenissen van de Service Bus en haalt het bericht.
-   Azure Blockchain Workbench initieert een aanroep naar de posten, verzenden van gegevens van het externe systeem naar een specifieke contract.
-   Na ontvangst van het bericht het contract evalueert de gegevens en mogelijk invloed op de status op basis van het resultaat van deze evaluatie, bijvoorbeeld voor een hoge temperatuur, de status te wijzigen *van naleving*.

## <a name="data-integration"></a>Gegevensintegratie

Naast de REST en berichten gebaseerde API biedt Azure Blockchain Workbench ook toegang tot een SQL-database gevuld met de toepassing en contract meta-data, evenals transactionele gegevens van gedistribueerde posten.

![Gegevensintegratie](media/blockchain-workbench-integration-patterns/data-integration.png)

De gegevensintegratie is bekend:

-   Azure Blockchain Workbench slaat metagegevens over toepassingen, werkstromen, contracten en transacties als onderdeel van de normale operationele gedrag.
-   Externe systemen of -hulpprogramma's bieden een of meer dialoogvensters vergemakkelijkt het verzamelen van informatie over de database, zoals een database-servernaam, databasenaam, type verificatie, aanmeldingsreferenties en welke database weergaven te kunnen gebruiken.
-   Query's worden geschreven met SQL database-weergaven ter bevordering van de downstream-verbruik door externe systemen, services, rapportage, hulpprogramma's voor ontwikkelaars en enterprise productiviteitsprogramma's.

## <a name="storage-integration"></a>Opslagintegratie

Veel scenario's mogelijk de noodzaak attestable bestanden opnemen. Om meerdere redenen zal zijn niet geschikt is voor de bestanden op een blockchain plaatsen. In plaats daarvan is een cryptografische hash (bijvoorbeeld, SHA-256) op basis van een bestand uitvoeren en die hash van een gedistribueerde grootboek delen een gemeenschappelijke aanpak. De hash op een later tijdstip opnieuw uitvoeren, moet hetzelfde resultaat retourneren. Als het bestand is gewijzigd, zelfs als er slechts één pixel in een installatiekopie hebt gewijzigd, wordt de hash voor een andere waarde geretourneerd.

![Opslagintegratie](media/blockchain-workbench-integration-patterns/storage-integration.png)

Het patroon kan worden geïmplementeerd waarbij:

-   Een extern systeem persistente een bestand in een opslagmechanisme met, zoals Azure Storage.
-   Een hash wordt gegenereerd met het bestand of het bestand en bijbehorende metagegevens zoals een id voor de eigenaar van de URL waar het bestand zich bevindt, enzovoort.
-   De hash en metagegevens is verzonden naar een functie op een smartcard contract, zoals *FileAdded*
-   In de toekomst, worden het bestand en de metagegevens opgedeeld opnieuw en vergeleken met de waarden die zijn opgeslagen op het grootboek.

## <a name="prerequisites-for-implementing-integration-patterns-using-the-rest-and-message-apis"></a>Vereisten voor de integratie van patronen met behulp van de REST en het bericht API's implementeren

Om te vergemakkelijken de mogelijkheid voor een extern systeem of een apparaat om te communiceren met de smartcard overeenkomst met de REST- of API-bericht, moet het volgende gebeuren-

1. In de Azure Active Directory voor het consortium, een account is gemaakt met de externe systeem of het apparaat.
2. De juiste smart contract(en) voor uw toepassing Azure Blockchain Workbench hebben functies die zijn gedefinieerd voor het accepteren van de gebeurtenissen van uw extern systeem of het apparaat.
3. Het configuratiebestand voor uw smartcard contract bevat de functie, die het systeem of het apparaat worden toegewezen.
4. Het configuratiebestand voor uw smartcard contract identificeert in welke statussen van dat deze functie kan worden aangeroepen door de gedefinieerde functie.
5. Het configuratiebestand van de toepassing en de smartcard contracten worden geüpload naar Azure Blockchain Workbench.

Zodra de toepassing is geüpload, wordt de Azure Active Directory-account voor het externe systeem is toegewezen aan het contract en de bijbehorende functieservices.

## <a name="testing-external-system-integration-flows-prior-to-writing-integration-code"></a>Extern systeem integratie stromen vóór het schrijven van Code integratie testen 

De mogelijkheid om te integreren met externe systemen is een belangrijke vereiste veel scenario's. Het is het wenselijk is om te kunnen valideren smart contract ontwerp voorgaande of parallel aan de ontwikkeling van code voor de integratie met externe systemen.

Het gebruik van Azure Active Directory (Azure AD) kan aanzienlijk versnellen productiviteit van ontwikkelaars en tijd op waarde. In het bijzonder een niet-triviale hoeveelheid tijd kan duren voordat de code-integratie met een extern systeem. Met behulp van Azure AD en het automatisch genereren van UX door Azure Blockchain Workbench, deze manier kunnen ontwikkelaars kunnen aanmelden bij de Workbench als dat externe systeem en statuswaarden verwacht van dat externe systeem via de UX vullen Hierdoor kan voor het snel ontwikkelen en valideren van ideeën in een bewijs van concept omgeving ofwel voorafgaand aan of parallel integratie code voor de externe systemen worden geschreven.

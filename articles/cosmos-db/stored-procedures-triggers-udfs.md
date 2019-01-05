---
title: Werken met opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies in Azure Cosmos DB
description: In dit artikel bevat de concepten, zoals opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies in Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/11/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 14cb821152e6a380de1be4fddef76aa2edec4a8f
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54042254"
---
# <a name="working-with-azure-cosmos-db-stored-procedures-triggers-and-user-defined-functions"></a>Werken met Azure Cosmos DB opgeslagen procedures, triggers en gebruikersgedefinieerde functies

Azure Cosmos DB biedt language integrated, transactionele uitvoering van JavaScript. Wanneer u de SQL-API in Azure Cosmos DB, kunt u **opgeslagen procedures**, **triggers**, en **gebruikersgedefinieerde functies (UDF's)** in de JavaScript-taal. U kunt uw logica schrijven in JavaScript die werd uitgevoerd binnen de database-engine. U kunt maken en uitvoeren van triggers en opgeslagen procedures, UDF's met behulp van [Azure-portal](https://portal.azure.com/), wordt de [JavaScript-taal geïntegreerd query-API in Azure Cosmos DB](javascript-query-api.md) of de [Cosmos DB SQL API-client SDK's](how-to-use-stored-procedures-triggers-udfs.md).

## <a name="benefits-of-using-server-side-programming"></a>Voordelen van het gebruik van programmeren op de server

Opgeslagen procedures, triggers en gebruikersgedefinieerde functies (UDF's) in JavaScript schrijven, kunt u uitgebreide toepassingen bouwen en ze hebben de volgende voordelen:

* **Procedurele logica:** JavaScript als een op hoog niveau programmeertaal die wordt gebruikt om rijke en vertrouwde interface naar snelle bedrijfslogica. U kunt een reeks complexe bewerkingen op de gegevens uitvoeren.

* **Atomische transacties:** Azure Cosmos DB garandeert dat de databasebewerkingen die worden uitgevoerd binnen een opgeslagen procedure met één of een trigger atomisch zijn. Deze atomic functionaliteit kunt een toepassing gerelateerde bewerkingen in één batch, combineren, zodat alle bewerkingen slagen of geen van beide slagen.

- **Prestaties:** De JSON-gegevens is intrinsiek toegewezen aan de JavaScript-typesysteem taal. Deze toewijzing kunt u een aantal optimalisaties, zoals vertraagde materialisatie van JSON-documenten in de buffergroep en u ze on-demand beschikbaar voor het uitvoeren van code. Er zijn andere prestatievoordelen die zijn gekoppeld aan het verzenden van bedrijfslogica op de database, waaronder:

   * *Batchverwerking:* U kunt bewerkingen zoals toevoegingen groep en legt deze in één bulkbewerking. De netwerkkosten voor de latentie van verkeer en de store-overhead voor het maken van afzonderlijke transacties zijn aanzienlijk verminderd.

   * *Vooraf compileren:* Opgeslagen procedures, triggers en UDF's zijn impliciet vooraf gecompileerde naar de byte-codeopmaak om te voorkomen dat compilatie kosten op het moment van elke aanroepen van scripts. Vanwege het vooraf compileren, het aanroepen van opgeslagen procedures is snel en een lage footprint is.

   * *Sequentiëren:* Soms bewerkingen moeten een activerende mechanisme waarmee een of meer updates voor de gegevens. Naast atomisch zijn er ook prestatievoordelen bij het uitvoeren van op de server.

- **Inkapseling:** Opgeslagen procedures kunnen worden gebruikt voor het groeperen van logica op één plek. Inkapseling wordt toegevoegd een abstractielaag bovenop de gegevens, waarmee u uw toepassingen onafhankelijk van de gegevens veranderen. Deze laag van abstractie is handig wanneer de gegevens zonder schema en u hoeft te beheren met het toevoegen van aanvullende logica rechtstreeks in uw toepassing. De abstrahering kunt uw houden die de gegevens beveiligen door de toegang van de scripts te stroomlijnen.

> [!TIP]
> Opgeslagen procedures zijn het meest geschikt voor bewerkingen die zijn zware schrijven. Wanneer u bepaalt waar u het gebruik van opgeslagen procedures, optimaliseren om de maximale hoeveelheid schrijfbewerkingen mogelijk encapsulating. Opgeslagen procedures zijn algemeen, niet de meest efficiënte manier om grote aantallen leesbewerkingen te doen, zodat het gewenste voordeel niet opgeslagen procedures om grote aantallen leesbewerkingen batch te gebruiken om terug te keren naar de client tot leidt.

## <a name="transactions"></a>Transacties

Transactie in een typische database kan worden gedefinieerd als een reeks bewerkingen die worden uitgevoerd als één logische eenheid van het werk. Elke transactie biedt **eigenschap ACID-garanties**. ACID is een bekende afkorting die staat voor: **Een**tomicity, **C**onsistency, **ik**solation, en **D**urability. 

* Atomisch zorgt ervoor dat alle bewerkingen uitgevoerd binnen een transactie worden behandeld als één eenheid en van alle hiervan doorgevoerd zijn of geen van beide zijn. 

* Consistentie zorgt ervoor dat de gegevens altijd in een geldige status voor transacties is. 

* Isolatie zorgt ervoor dat er zijn geen twee transacties leiden tot met elkaar problemen – veel commerciële systemen bieden verschillende isolatieniveaus die kunnen worden gebruikt op basis van de vereisten voor de toepassing. 

* Duurzaamheid zorgt ervoor dat eventuele wijzigingen dat is vastgelegd in een database altijd aanwezig zijn.

JavaScript-runtime wordt in Azure Cosmos DB worden gehost in de database-engine. Daarom kan aanvragen in de opgeslagen procedures en triggers worden uitgevoerd in hetzelfde bereik als de databasesessie. Deze functie kunt Azure Cosmos DB om te waarborgen van ACID-eigenschappen voor alle bewerkingen die deel van een opgeslagen procedure of een trigger uitmaken. Zie voor voorbeelden van [het implementeren van transacties](how-to-write-stored-procedures-triggers-udfs.md#transactions) artikel.

### <a name="scope-of-a-transaction"></a>Bereik van een transactie

Als een opgeslagen procedure gekoppeld aan een Azure Cosmos-container is, wordt de opgeslagen procedure uitgevoerd binnen het transactiebereik van een logische partitie-sleutel. Elke uitvoering van opgeslagen procedure moet een logische waarde voor de partitiesleutel die overeenkomt met het bereik van de transactie omvatten. Zie voor meer informatie, [partitioneren van Azure Cosmos DB](partition-data.md) artikel.

### <a name="commit-and-rollback"></a>Doorvoeren en ongedaan maken

Transacties worden geïntegreerd in het Azure Cosmos DB JavaScript-programmeermodel. Binnen een JavaScript-functie, worden automatisch alle bewerkingen samengevoegd in één transactie. Als de JavaScript-logica in een opgeslagen procedure is voltooid zonder eventuele uitzonderingen, zijn alle bewerkingen binnen de transactie zijn vastgelegd in de database. Instructies zoals `BEGIN TRANSACTION` en `COMMIT TRANSACTION` (bekend is bij relationele databases) zijn impliciet in Azure Cosmos DB. Als er geen uitzonderingen van het script zijn, wordt de Azure Cosmos DB JavaScript-runtime Draai de hele transactie terug. Als zodanig, die een uitzondering veroorzaakt is effectief gelijk is aan een `ROLLBACK TRANSACTION` in Azure Cosmos DB.

### <a name="data-consistency"></a>Gegevensconsistentie

Opgeslagen procedures en triggers worden altijd uitgevoerd op de primaire replica van een Azure Cosmos-container. Deze functie zorgt ervoor dat wordt gelezen uit de aanbieding van opgeslagen procedures [sterke consistentie](consistency-levels-tradeoffs.md). Query's met behulp van de gebruiker gedefinieerde functies kunnen worden uitgevoerd op de primaire of op elke secundaire replica. Opgeslagen procedures en triggers zijn bedoeld ter ondersteuning van transactionele schrijfbewerkingen – ondertussen alleen-lezen-logica beste wordt geïmplementeerd als logica op de toepassing en query's met de [SDK's voor Azure Cosmos DB SQL API](sql-api-dotnet-samples.md), ziet u een volledig in beslag nemen de de doorvoer van de database. 

## <a name="bounded-execution"></a>Gebonden uitvoering

Alle Azure Cosmos DB-bewerkingen moeten voltooid binnen de duur van de opgegeven time-out. Deze beperking geldt voor JavaScript - functies in opgeslagen procedures, triggers en gebruikersgedefinieerde functies. Als een bewerking niet binnen de tijdslimiet wordt voltooid, wordt de transactie teruggedraaid.

U kunt ofwel ervoor zorgen dat uw JavaScript-functies binnen de tijdslimiet is voltooid of implementeren van een model op basis van een voortzetting van batch/hervatten kan worden uitgevoerd. Vereenvoudigt de ontwikkeling van opgeslagen procedures en triggers om af te handelen tijdslimieten, die alle functies in de Azure Cosmos-container (bijvoorbeeld maken, lezen, bijwerken en verwijderen van items) retourneren een Booleaanse waarde die aangeeft of deze bewerking wordt voltooid. Als deze waarde ingesteld op false is, is het een indicatie dat de procedure van kan worden uitgevoerd inpakken moet omdat het script is verbruikt meer tijd of ingerichte doorvoer dan de geconfigureerde waarde. Bewerkingen in de wachtrij vóór de eerste bewerking in niet-geaccepteerde store om uit te voeren als de opgeslagen procedure is voltooid in de tijd en geen aanvullende aanvragen niet in de wachtrij worden gegarandeerd. Bewerkingen moet dus in de wachtrij een op een tijdstip met behulp van JavaScript callback overeenkomst voor het beheren van de Controlestroom van het script. Omdat de scripts worden uitgevoerd in een server-side-omgeving, worden ze strikt geregeld. Scripts die herhaaldelijk strijdig zijn met de grenzen van de uitvoering kunnen worden gemarkeerd als inactief en kunnen niet worden uitgevoerd, en ze opnieuw moeten worden gemaakt om te voldoen aan de grenzen van de uitvoering.

JavaScript-functies zijn ook onderhevig aan [ingericht doorvoercapaciteit](request-units.md). JavaScript-functies mogelijk met behulp van een groot aantal aanvraageenheden binnen een korte periode kunnen beëindigen en mogelijk rate-beperkt als de capaciteitslimiet van de ingerichte doorvoer is bereikt. Het is belangrijk te weten dat scripts als u meer doorvoer naast de doorvoer is besteed aan het uitvoerende databasebewerkingen gebruiken, hoewel deze databasebewerkingen voor de iets minder duur zijn dan het uitvoeren van bewerkingen van de client.

## <a name="triggers"></a>Triggers

Deze sectie beschrijft de twee soorten triggers:

### <a name="pre-triggers"></a>Vooraf triggers

Azure Cosmos DB biedt triggers die kunnen worden aangeroepen door het uitvoeren van een bewerking op een Azure Cosmos DB-item. U kunt bijvoorbeeld een pre-trigger opgeven wanneer u een item maakt. De vooraf trigger wordt in dit geval worden uitgevoerd voordat het item wordt gemaakt. Pre-triggers kunnen geen invoerparameters hebben. Indien nodig, kan de Aanvraagobject kan worden gebruikt om bij te werken van de hoofdtekst van het document in de oorspronkelijke aanvraag. Als triggers zijn geregistreerd, kunnen gebruikers de bewerkingen die het kan worden uitgevoerd met opgeven. Als een trigger is gemaakt met `TriggerOperation.Create`, betekent dit dat met behulp van de trigger in een vervangingsbewerking niet worden toegestaan. Zie voor voorbeelden van [over het schrijven van triggers](how-to-write-stored-procedures-triggers-udfs.md#triggers) artikel.

### <a name="post-triggers"></a>Na triggers

Net als bij vooraf triggers, na triggers zijn ook gekoppeld aan een bewerking op een Azure Cosmos DB-item en ze vereisen invoerparameters die zijn opgegeven. Ze worden uitgevoerd *nadat* de bewerking is voltooid en toegang hebben tot het antwoordbericht dat wordt verzonden naar de client. Zie voor voorbeelden van [over het schrijven van triggers](how-to-write-stored-procedures-triggers-udfs.md#triggers) artikel.

## <a id="udfs"></a>De gebruiker gedefinieerde functies

Gebruiker gedefinieerde functies (UDF's) worden gebruikt voor het uitbreiden van de syntaxis van de SQL-API-query en eenvoudig aangepaste bedrijfslogica implementeren. Ze kunnen alleen in query's worden aangeroepen. UDF's hebben geen toegang tot het context-object en zijn bedoeld om te worden gebruikt als compute alleen JavaScript. Daarom kunnen UDF's worden uitgevoerd op secundaire replica's. Zie voor voorbeelden van [over het schrijven van de gebruiker gedefinieerde functies](how-to-write-stored-procedures-triggers-udfs.md#udfs) artikel.

## <a id="jsqueryapi"></a>JavaScript language integrated query API

Naast het uitgeven van query's met behulp van SQL-API-query-syntaxis, de [server-side SDK](https://azure.github.io/azure-cosmosdb-js-server) kunt u query's uitvoeren met behulp van een JavaScript-interface zonder enige kennis van SQL. De JavaScript-query API kunt u query's via een programma kunt maken met het predicaat functies in volgorde van de aanroepen van de functie doorgegeven. Query's worden geparseerd door het JavaScript-runtime en efficiënt worden uitgevoerd in Azure Cosmos DB. Zie voor meer informatie over de ondersteuning van de JavaScript-API-query, [werken met JavaScript-taal geïntegreerd query-API](javascript-query-api.md) artikel. Zie voor voorbeelden van [over het schrijven van opgeslagen procedures en triggers die met behulp van Javascript-Query API](how-to-write-javascript-query-api.md) artikel.

## <a name="next-steps"></a>Volgende stappen

Informatie over het schrijven en opgeslagen procedures, triggers en gebruikersgedefinieerde functies in Azure Cosmos DB gebruiken met de volgende artikelen:

* [Over het schrijven van opgeslagen procedures, triggers en gebruikersgedefinieerde functies](how-to-write-stored-procedures-triggers-udfs.md)

* [Het gebruik van opgeslagen procedures, triggers en gebruikersgedefinieerde functies](how-to-use-stored-procedures-triggers-udfs.md)

* [Werken met JavaScript-taal geïntegreerd query-API](javascript-query-api.md)

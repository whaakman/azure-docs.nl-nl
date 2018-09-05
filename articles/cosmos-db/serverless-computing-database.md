---
title: Serverless database computing - Azure Functions en Azure Cosmos DB | Microsoft Docs
description: Meer informatie over hoe Azure Cosmos DB en Azure Functions kunnen samen worden gebruikt om gebeurtenisgestuurde serverloze computing apps te maken.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: sngun
ms.openlocfilehash: b54a302c022a8af84c1964c13ffadc52b45c4ca4
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43696816"
---
# <a name="azure-cosmos-db-serverless-database-computing-using-azure-functions"></a>Azure Cosmos DB: Serverless database computing met Azure Functions

Alles over de mogelijkheid om zich te richten op afzonderlijke onderdelen van de logica die herhaalbare en stateless is serverloze computing. Deze onderdelen vereisen geen infrastructuurbeheer en ze resources verbruiken alleen voor de seconden of milliseconden, ze worden uitgevoerd voor. De spil van de serverloze computing verplaatsing zijn functies, die in de Azure-ecosysteem door beschikbaar worden gesteld [Azure Functions](https://azure.microsoft.com/services/functions).

Met de ingebouwde integratie tussen [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db) en Azure Functions, kunt u databasetriggers, invoerbindingen en uitvoerbindingen rechtstreeks vanuit uw Azure Cosmos DB-account. Met behulp van Azure Functions en Azure Cosmos DB, kunt u maken en implementeren van gebeurtenisgestuurde serverloze apps met lage latentie toegang tot uitgebreide gegevens voor een globale gebruikersgroep.

## <a name="overview"></a>Overzicht

Azure Cosmos DB en Azure Functions kunt u uw databases en serverloze apps integreren in de volgende manieren:

* Maak een gebeurtenisgestuurde **Azure Cosmos DB-trigger** in een Azure-functie. Afhankelijk van deze trigger [wijzigingenfeed](change-feed.md) streams voor het bewaken van uw Azure Cosmos DB-container voor wijzigingen. Wanneer wijzigingen worden aangebracht in een container, wordt de stream-wijzigingenfeed verzonden naar de trigger, die de Azure Function aanroept.
* U kunt ook een Azure-functie binden aan een Azure Cosmos DB-container met een **Invoerbinding**. Wanneer een functie wordt uitgevoerd, invoerbindingen gegevens lezen uit een container.
* Binding van een functie met een Azure Cosmos DB-container met een **Uitvoerbinding**. Wanneer een functie is voltooid, uitvoerbindingen gegevens schrijven naar een container.

> [!NOTE]
> Op dit moment werken de Azure Cosmos DB-trigger, invoerbindingen en uitvoerbindingen met SQL-API en Gremlin-API-accounts.

Het volgende diagram illustreert elk van deze drie integraties: 

![Hoe u Azure Cosmos DB en Azure Functions integreren](./media/serverless-computing-database/cosmos-db-azure-functions-integration.png)

De Azure Cosmos DB-trigger, Invoerbinding en Uitvoerbinding kunnen worden gebruikt in de volgende combinaties:
* Een Azure Cosmos DB-trigger kan worden gebruikt met een Uitvoerbinding naar een andere Azure Cosmos DB-container. Nadat een functie een actie op een item in de wijzigingenfeed uitvoert kunt u het schrijven naar een andere container (schrijven naar dezelfde container die is afkomstig van een recursieve lus effectief maakt). Of u kunt een Azure Cosmos DB-trigger gebruiken om effectief alle gewijzigde items uit een container te migreren naar een andere container, door gebruik te maken van een Uitvoerbinding.
* Invoerbindingen en uitvoerbindingen voor Azure Cosmos DB kunnen worden gebruikt in dezelfde Azure-functie. Dit werkt goed in gevallen wanneer u wilt dat bepaalde gegevens vinden met de Invoerbinding, wijzig dit in de Azure-functie en sla deze op dezelfde container of een andere container na de wijziging.
* Geen Invoerbinding met een Azure Cosmos DB-container kan worden gebruikt in dezelfde functie als een Azure Cosmos DB-trigger en kan worden gebruikt met of zonder een Uitvoerbinding ook. U kunt deze combinatie gebruiken om toe te passen up-to-date valuta exchange-gegevens (opgehaald met een Invoerbinding naar een exchange-container) de wijzigingenfeed van nieuwe orders in uw winkelwagen winkelwagen-service. Het bijgewerkte totaal voor winkelen winkelwagen, kan met de huidige valutaconversie toegepast, worden geschreven naar een derde container met behulp van een Uitvoerbinding.

## <a name="use-cases"></a>Gebruiksvoorbeelden

De volgende gevallen gebruik laten zien hoe u een paar kunt u alles uit uw Azure Cosmos DB-gegevens - door verbinding te maken van uw gegevens op basis van gebeurtenissen van Azure Functions.

### <a name="iot-use-case---azure-cosmos-db-trigger-and-output-binding"></a>IoT-use-case - Azure Cosmos DB-trigger en Storage-Uitvoerbinding

In IoT-implementaties, kunt u een functie aanroepen wanneer het selectievakje engine licht wordt weergegeven in een aangesloten auto.

**Implementatie:** gebruik van een Azure Cosmos DB-trigger en een Uitvoerbinding

1. Een **Azure Cosmos DB-trigger** wordt gebruikt voor het activeren van gebeurtenissen met betrekking tot waarschuwingen van auto's, zoals het controle-engine licht afkomstig is uit in een aangesloten auto.
2. Wanneer het selectievakje engine licht afkomstig is, wordt de sensorgegevens verzonden naar Azure Cosmos DB.
3. Azure Cosmos DB wordt gemaakt of bijgewerkt nieuwe sensor data documenten en deze wijzigingen worden gestreamd naar de Azure Cosmos DB-trigger.
4. De trigger wordt aangeroepen op alle gegevens-wijzigingen die op de sensor gegevens te verzamelen, zoals alle wijzigingen worden gestreamd via de feed wijzigen.
5. De voorwaarde van een drempelwaarde wordt gebruikt in de functie voor het verzenden van de sensorgegevens op de garantieafdeling.
6. Als de temperatuur hoger ook over een bepaalde waarde is, wordt een waarschuwing ook verzonden naar de eigenaar.
7. De **Uitvoerbinding** op de functie-updates de auto-record in een andere Azure Cosmos DB-container voor het opslaan van informatie over de controle-engine-gebeurtenis.

De volgende afbeelding toont de code die is geschreven in de Azure-portal voor deze trigger.

![Een Azure Cosmos DB-trigger maken in Azure portal](./media/serverless-computing-database/cosmos-db-trigger-portal.png)

### <a name="financial-use-case---timer-trigger-and-input-binding"></a>Financiële use-case - timertrigger en Invoerbinding

In implementaties van financiële, kunt u een functie aanroepen wanneer het saldo voor een bankrekening onder een bepaalde hoeveelheid valt.

**Implementatie:** een timertrigger met een Azure Cosmos DB-Invoerbinding

1. Met behulp van een [timertrigger](../azure-functions/functions-bindings-timer.md), vindt u het saldo bankgegevens die zijn opgeslagen in een Azure Cosmos DB-container met bepaalde tussenpozen met behulp van een **Invoerbinding**.
2. Als het saldo lager dan de drempelwaarde saldo bijna op is ingesteld door de gebruiker is, klikt u vervolgens volgen met de actie van de Azure-functie.
3. De Uitvoerbinding mag een [SendGrid integratie](../azure-functions/functions-bindings-sendgrid.md) die een e-mailbericht verzendt van een service-account naar het e-mailadressen geïdentificeerd voor elk van de laag saldo-accounts.

De volgende afbeeldingen ziet de code in de Azure-portal voor dit scenario.

![Index.js-bestand voor een timertrigger voor een financiële scenario](./media/serverless-computing-database/cosmos-db-functions-financial-trigger.png)

![Het bestand Run.csx voor een timertrigger voor een financiële scenario](./media/serverless-computing-database/azure-function-cosmos-db-trigger-run.png)

### <a name="gaming-use-case---azure-cosmos-db-trigger-and-output-binding"></a>Gaming use case: Azure Cosmos DB-trigger en Storage-Uitvoerbinding

In games, wanneer een nieuwe gebruiker wordt gemaakt. u kunt zoeken naar andere gebruikers die kent u ze met behulp van mogelijk de [Gremlin-API van Azure Cosmos DB](graph-introduction.md). U kunt de resultaten vervolgens schrijven naar een [Azure Cosmos DB SQL-database] voor eenvoudig op te halen.

**Implementatie:** gebruik van een Azure Cosmos DB-trigger en een Uitvoerbinding

1. Met behulp van een Azure Cosmos DB [grafiekdatabase](graph-introduction.md) voor het opslaan van alle gebruikers, kunt u een nieuwe functie maken met een Azure Cosmos DB-trigger. 
2. Wanneer een nieuwe gebruiker wordt ingevoegd, de functie is aangeroepen en vervolgens het resultaat wordt opgeslagen met behulp van een **Uitvoerbinding**.
3. De functie vraagt de graph-database om te zoeken naar alle gebruikers die direct zijn gerelateerd aan de nieuwe gebruiker en deze gegevensset aan de functie retourneert.
4. Deze gegevens worden vervolgens opgeslagen in een Azure Cosmos DB, die vervolgens eenvoudig kan worden opgehaald door een front-end-toepassing waarin de nieuwe gebruiker verbonden vrienden.

### <a name="retail-use-case---multiple-functions"></a>Detailhandel use-case - meerdere functies

In implementaties van een retailanalyse, wanneer een gebruiker een item toegevoegd aan het mandje hebt u nu de flexibiliteit voor het maken en functies voor het optionele business pijplijn-onderdelen aanroepen.

**Implementatie:** meerdere Azure Cosmos DB-triggers te hebben geluisterd naar één container

1. U kunt meerdere Azure Functions maken door toe te voegen met Azure Cosmos DB-triggers voor elk - die naar dezelfde luisteren wijzigingenfeed van winkelen winkelwagen gegevens. Houd er rekening mee dat wanneer meerdere functies luisteren naar dezelfde wijzigingenfeed, een nieuwe leaseverzameling is vereist voor elke functie. Zie voor meer informatie over verzamelingen lease [inzicht in de Change Feed Processor-bibliotheek](change-feed.md#understand-cf).
2. Wanneer een nieuw item wordt toegevoegd aan een winkelwagen gebruikers, wordt elke functie onafhankelijk aangeroepen door de wijzigingenfeed vanuit de shopping winkelwagen-container.
    * Een functie kan de inhoud van het huidige mandje gebruiken om te wijzigen van de weergave van andere items die de gebruiker mogelijk geïnteresseerd in.
    * Een andere functie kan inventarisatie totalen worden bijgewerkt.
    * Een andere functie kan verzenden klantgegevens voor bepaalde producten naar de marketingafdeling, die ze een speciale e-mailprogramma verzendt. 

    Elke afdeling kan een Azure Cosmos DB-trigger maken door te luisteren naar de wijzigingenfeed en zorg ervoor dat ze kritieke volgorde verwerking van gebeurtenissen in het proces wordt niet uitstellen.

In al deze gevallen gebruikt, omdat de functie is losgekoppeld van de app zelf, hoeft u om te activeren voortdurend nieuwe app-exemplaren. Azure Functions is in plaats daarvan draaien van afzonderlijke functies voor het voltooien van afzonderlijke processen, zoals die nodig zijn.

## <a name="tooling"></a>Hulpprogramma 's

Systeemeigen integratie tussen Azure Cosmos DB en Azure Functions is beschikbaar in Azure portal en in Visual Studio 2017.
* U kunt een Azure Cosmos DB-trigger maken in de Azure Functions-portal. Zie voor instructies snelstartgids [een Azure Cosmos DB-trigger maken in Azure portal](https://aka.ms/cosmosdbtriggerportalfunc) ![een Azure Cosmos DB-trigger maken in de Azure Functions-portal](./media/serverless-computing-database/azure-function-cosmos-db-trigger.png) 
* In de Azure Functions-portal, kunt u ook Azure Cosmos DB-invoerbindingen en uitvoerbindingen toevoegen voor andere soorten triggers. Zie voor instructies snelstartgids [Store niet-gestructureerde gegevens met behulp van Azure Functions en Cosmos DB](../azure-functions/functions-integrate-store-unstructured-data-cosmosdb.md).
    ![Een Azure Cosmos DB-trigger maken in de Azure Functions-portal](./media/serverless-computing-database/function-portal-input-binding.png)
*   In de portal voor Azure Cosmos DB, kunt u een Azure Cosmos DB-trigger toevoegen aan een bestaande Azure-functie-app in dezelfde resourcegroep bevinden.
    ![Een Azure Cosmos DB-trigger maken in de Azure Functions-portal](./media/serverless-computing-database/cosmos-db-portal.png)
* U kunt een Azure Cosmos DB-trigger met de geïntegreerde sjabloon maken in Visual Studio 2017:

    >[!VIDEO https://www.youtube.com/embed/iprndNsUeeg]


## <a name="why-choose-azure-functions-integration-for-serverless-computing"></a>Waarom kiezen voor Azure Functions-integratie voor serverloze computing?

Azure Functions biedt de mogelijkheid om te maken van schaalbare eenheden van werk- of beknopte stukjes logica die kunnen worden uitgevoerd op aanvraag, zonder inrichten of beheren van infrastructuur. U hoeft te maken van een volwaardige app om te reageren op wijzigingen in uw Azure Cosmos DB-database, kunt u kleine herbruikbare functies voor specifieke taken maken met behulp van Azure Functions. Bovendien u ook Azure Cosmos DB-gegevens kunt gebruiken als invoer of uitvoer naar een Azure-functie in reactie op een gebeurtenis, zoals een HTTP-aanvragen of een getimede-trigger.

Azure Cosmos DB is de aanbevolen database voor uw serverloze computing-architectuur voor de volgende redenen:

* **Directe toegang tot al uw gegevens**: hebt u nauwkeurige toegang tot elke waarde die is opgeslagen, omdat Azure Cosmos DB [automatisch geïndexeerd](indexing-policies.md) alle gegevens standaard, en maakt deze indexen onmiddellijk beschikbaar. Dit betekent dat u zich kunt voortdurend query, bijwerken, en nieuwe items toevoegen aan uw database en directe toegang via Azure Functions hebben.

* **Schemaloos**. Azure Cosmos DB is schemaloos - dus is het een unieke overweg kan met elke gegevensuitvoer van een Azure-functie. Deze aanpak 'aankunnen,' maakt het eenvoudig om een verscheidenheid aan functies maken die alle uitvoer naar Azure Cosmos DB.

* **Schaalbare doorvoer**. Doorvoer kan worden verhoogd of verlaagd direct in Azure Cosmos DB. Als u honderden of duizenden functies uitvoeren van query's en het schrijven naar dezelfde container hebt, kunt u omhoog schalen uw [RU/s](request-units.md) om de belasting te verwerken. Alle functies kunnen werken parallel met behulp van de toegewezen RU/s en uw gegevens is gegarandeerd [consistente](consistency-levels.md).

* **Globale replicatie**. U kunt Azure Cosmos DB-gegevens repliceren [over de hele wereld](distribute-data-globally.md) latentie, geolocatie uw om gegevens te reduceren die het dichtst bij uw gebruikers. Als met alle Azure Cosmos DB-query's, worden gegevens van gebeurtenisgestuurde triggers gelezen gegevens uit de Azure Cosmos DB, die het dichtst bij de gebruiker.

Als u wilt integreren met Azure Functions voor het opslaan van gegevens en niet grondige te indexeren hoeft of als u nodig hebt voor het opslaan van bijlagen en media-bestanden, de [trigger voor Azure Blob Storage](../azure-functions/functions-bindings-storage-blob.md) mogelijk een betere optie.

Voordelen van Azure Functions: 

* **Gebeurtenisgestuurde**. Azure Functions worden op basis van gebeurtenissen en kunnen luisteren naar een van de Azure Cosmos DB-wijzigingenfeed. Dit betekent dat u hoeft te luisteren naar logische maken, u alleen gaten houden uit om de wijzigingen die u naar luisteren wilt. 

* **Er zijn geen limieten**. Functies uitvoeren in parallelle instructies en de service draaien om zo veel als u nodig hebt. U instellen de parameters.

* **Geschikt voor snelle taken**. Nieuwe exemplaren van functies die de service automatisch draaien wanneer een gebeurtenis wordt geactiveerd en wordt deze gesloten zodra de functie is voltooid. U betaalt alleen voor de tijd dat uw functies worden uitgevoerd.

Als u niet zeker weet of Flow, Logic Apps, Azure Functions of WebJobs vooral geschikt voor uw implementatie zijn, Zie [kiezen tussen Flow, Logic Apps, Functions en WebJobs](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md).

## <a name="next-steps"></a>Volgende stappen

Nu gaan we verbinding maken met Azure Cosmos DB en Azure Functions echt: 

* [Een Azure Cosmos DB-trigger maken in Azure portal](https://aka.ms/cosmosdbtriggerportalfunc)
* [Een Azure Functions HTTP-trigger maken met een Azure Cosmos DB-Invoerbinding](https://aka.ms/cosmosdbinputbind)
* [Niet-gestructureerde gegevens met behulp van Azure Functions en Cosmos DB Store](../azure-functions/functions-integrate-store-unstructured-data-cosmosdb.md)
* [Azure Cosmos DB-bindingen en triggers](../azure-functions/functions-bindings-cosmosdb.md)


 




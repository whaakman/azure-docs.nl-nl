---
title: Zonder Server-database computing - functies van Azure en Azure Cosmos DB | Microsoft Docs
description: Meer informatie over hoe Azure Cosmos DB en Azure Functions kunnen tegelijk worden gebruikt om gebeurtenisafhankelijke zonder server computing apps te maken.
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: mimig
ms.openlocfilehash: aeef39294bbf3ad4192fe116c6972e52bfa1c816
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="azure-cosmos-db-serverless-database-computing-using-azure-functions"></a>Azure Cosmos DB: Zonder Server database computing met behulp van Azure Functions

Zonder server computing gaat over de mogelijkheid om zich te richten op afzonderlijke onderdelen van logica die herhaalbare en staatloze zijn. Deze onderdelen vereisen geen infrastructuurbeheer en ze alleen voor de seconden of milliseconden ze worden uitgevoerd voor gebruik van bronnen. De kern van het zonder server computing verkeer worden functies die beschikbaar is in het Azure-ecosysteem door [Azure Functions](https://azure.microsoft.com/services/functions).

Met de systeemeigen integratie tussen [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db) Azure Functions, kunt u databasetriggers, bindingen invoer en uitvoer bindingen rechtstreeks vanuit uw Azure DB die Cosmos-account. Azure Functions en Azure Cosmos DB gebruikt, kunt u apps maken en implementeren gebeurtenisafhankelijke zonder server met lage latentie toegang met uitgebreide gegevens voor een globale gebruikersgroep.

## <a name="overview"></a>Overzicht

Azure Cosmos DB en Azure Functions, kunt u voor het integreren van uw databases en zonder server apps in de volgende manieren:

* Maak een gebeurtenisafhankelijke **Azure Cosmos DB trigger** in een Azure-functie. Deze trigger is afhankelijk van [wijzigen van de feed](change-feed.md) stromen voor het bewaken van uw Azure DB die Cosmos-container voor wijzigingen. Wanneer wijzigingen zijn aangebracht in een container, wordt de wijziging feed stroom verzonden naar de trigger die de Azure-functie aanroept.
* Een Azure-functie ook koppelen aan een Azure Cosmos DB verzameling met een **invoer binding**. Wanneer een functie wordt uitgevoerd, invoer bindingen gegevens lezen uit een container.
* Een functie binden aan een Azure Cosmos DB verzameling met een **uitvoer binding**. Wanneer een functie is voltooid, uitvoer bindingen gegevens schrijven naar een container.

> [!NOTE]
> Op dit moment worden de Azure DB die Cosmos trigger bindingen invoer en uitvoer bindingen met SQL-API en Graph API-accounts werken.

Het volgende diagram illustreert elk van deze drie integraties: 

![De manier waarop Azure Cosmos DB en Azure Functions integreert](./media/serverless-computing-database/cosmos-db-azure-functions-integration.png)

De Azure DB die Cosmos trigger, binding invoer en uitvoer binding kunnen worden gebruikt in de volgende combinaties van:
* Een Azure DB die Cosmos-trigger kan worden gebruikt met een uitvoer-binding met een andere Azure DB die Cosmos-container. Nadat een functie een actie uitgevoerd op een item in de feed wijzigen kunt u het schrijven naar een andere container (schrijven naar de container die is afkomstig van een recursieve lus effectief te maken). Of u kunt een Azure DB die Cosmos-trigger effectief alle gewijzigde items uit een container te migreren naar een andere container, met het gebruik van een uitvoer-binding.
* Bindingen voor invoer en uitvoer bindingen voor Azure Cosmos DB kunnen worden gebruikt in dezelfde Azure-functie. Dit werkt goed in gevallen wanneer u wilt zoeken van bepaalde gegevens met de invoer binding, wijzigen in de Azure-functie en vervolgens opslaan in dezelfde container of een andere container na de wijziging.
* Een invoer-binding aan een Azure DB die Cosmos-container kan worden gebruikt in dezelfde functie uit als een Azure DB die Cosmos-trigger en kan worden gebruikt met of zonder binding ook uitvoer. U kunt deze combinatie up-to-date valuta exchange-gegevens (opgehaald met een invoer-binding aan een exchange-container) toepassen op de feed wijziging van de nieuwe orders in uw winkelwagen winkelwagen-service. De winkelwagen winkelwagen Totaal bijgewerkt, kan met de huidige valutaconversie toegepast, worden geschreven naar een derde container met een uitvoer-binding.

## <a name="use-cases"></a>Gebruiksvoorbeelden

De volgende gevallen demonstreren een aantal manieren kunt u de meeste van de gegevens van uw Azure DB die Cosmos - door verbinding te maken van uw gegevens naar Azure Functions gebeurtenisafhankelijke.

### <a name="iot-use-case---azure-cosmos-db-trigger-and-output-binding"></a>IoT gebruiksvoorbeeld - trigger Azure Cosmos DB en uitvoer van de binding

U kunt een functie in IoT-implementaties aanroepen wanneer het selectievakje engine lichte wordt weergegeven in een verbonden auto.

**Implementatie:** een trigger Azure Cosmos DB en een uitvoer-binding gebruiken

1. Een **Azure Cosmos DB trigger** wordt gebruikt voor het activeren van gebeurtenissen met betrekking tot auto waarschuwingen, zoals het controle-engine lichte binnenkort op in een verbonden auto.
2. Wanneer het selectievakje engine lichte wordt geleverd, wordt de sensorgegevens verzonden naar Azure Cosmos DB.
3. Azure Cosmos DB maken of bijwerken van de nieuwe sensor gegevensdocumenten vervolgens deze wijzigingen worden gestreamd naar de Azure DB die Cosmos-trigger.
4. De trigger wordt aangeroepen op elke gegevenswijziging aan de verzameling van sensor gegevens, zoals alle wijzigingen worden gestreamd via de wijziging feed.
5. Een voorwaarde drempelwaarde wordt gebruikt in de functie voor de sensorgegevens verzenden naar de afdeling Garantie.
6. Als de temperatuur ook over een bepaalde waarde, wordt een waarschuwing ook verzonden naar de eigenaar.
7. De **uitvoer binding** op de functie-updates de auto-record in een andere Azure Cosmos DB verzameling voor het opslaan van informatie over de controle-engine-gebeurtenis.

De volgende afbeelding toont de code die is geschreven in de Azure portal voor deze trigger.

![Een Azure DB die Cosmos-trigger maken in de Azure-portal](./media/serverless-computing-database/cosmos-db-trigger-portal.png)

### <a name="financial-use-case---timer-trigger-and-input-binding"></a>Financiële gebruiksvoorbeeld - timertrigger en invoer van de binding

U kunt een functie in financiële implementaties aanroepen wanneer een saldo onder een bepaalde hoeveelheid valt.

**Implementatie:** een timertrigger met een Cosmos Azure DB invoer binding

1. Met behulp van een [timertrigger](../azure-functions/functions-bindings-timer.md), kunt u ophalen van de bank saldo accountgegevens opgeslagen in een Azure DB die Cosmos-container met bepaalde tussenpozen met behulp van een **invoer binding**.
2. Als het saldo minder dan de drempelwaarde laag saldo is ingesteld door de gebruiker is, volgt u met een actie van de Azure-functie.
3. De uitvoer-binding kan niet een [SendGrid-integratie](../azure-functions/functions-bindings-sendgrid.md) die een e-mailbericht van een serviceaccount verzendt naar de e-mailadressen geïdentificeerd voor elk van de laag saldo-accounts.

De volgende afbeeldingen tonen de code in de Azure-portal voor dit scenario.

![Index.js-bestand voor een timertrigger voor een financiële scenario](./media/serverless-computing-database/cosmos-db-functions-financial-trigger.png)

![Run.csx-bestand voor een timertrigger voor een financiële scenario](./media/serverless-computing-database/azure-function-cosmos-db-trigger-run.png)

### <a name="gaming-use-case---azure-cosmos-db-trigger-and-output-binding"></a>Games gebruiksvoorbeeld - trigger Azure Cosmos DB en uitvoer van de binding

In games, wanneer een nieuwe gebruiker is gemaakt kunt u zoeken voor andere gebruikers die weten met behulp van mogelijk de [Azure Cosmos DB Graph API](graph-introduction.md). U kunt de resultaten vervolgens schrijven naar een [Azure Cosmos DB SQL-database] gemakkelijk kunt ophalen.

**Implementatie:** een trigger Azure Cosmos DB en een uitvoer-binding gebruiken

1. Met behulp van een Cosmos Azure DB [grafiek database](graph-introduction.md) voor het opslaan van alle gebruikers, kunt u een nieuwe functie maken met een Azure DB die Cosmos-trigger. 
2. Wanneer een nieuwe gebruiker wordt geplaatst, de functie is aangeroepen en vervolgens het resultaat opgeslagen met behulp van een **uitvoer binding**.
3. De functie uit de database van de grafiek om te zoeken naar alle gebruikers die direct gerelateerd zijn aan de nieuwe gebruiker en deze gegevensset retourneert aan de functie opvraagt.
4. Deze gegevens worden vervolgens opgeslagen in een Cosmos Azure DB, die vervolgens gemakkelijk kunnen worden opgehaald met een front-toepassing die wordt weergegeven van de nieuwe gebruiker verbonden vrienden.

### <a name="retail-use-case---multiple-functions"></a>Gebruiksvoorbeeld Retail - meerdere functies

In retail-implementaties wanneer een gebruiker een item wordt toegevoegd aan hun mandje hebt u nu de flexibiliteit om te maken en functies voor optionele business pipeline-onderdelen aanroepen.

**Implementatie:** meerdere Azure Cosmos DB triggers luisteren naar een verzameling

1. U kunt meerdere Azure Functions maken door toe te voegen Azure DB die Cosmos-triggers aan elk - die allemaal op hetzelfde luisteren feed van winkelen winkelwagen gegevens wijzigen. Houd er rekening mee dat wanneer meerdere functies luisteren naar dezelfde feed wijzigen, een nieuwe verzameling van de lease is vereist voor elke functie.
2. Wanneer een nieuw item wordt toegevoegd aan een winkelwagen gebruikers, wordt elke functie onafhankelijk aangeroepen door een wijziging feed uit de winkelwagen winkelwagen-container.
    * Een functie kan de inhoud van het huidige mandje gebruiken om de weergave van andere items die de gebruiker mogelijk geïnteresseerd zijn in wijzigen.
    * Een andere functie kan bijwerken inventarisatie totalen.
    * Een andere functie mogelijk klantgegevens voor bepaalde producten verzenden naar de marketingafdeling die ze een e-mailprogramma aanbiedingen verzendt. 

    Elke afdeling een trigger Azure Cosmos DB maken door te luisteren naar de feed wijzigen en zorg ervoor dat ze won't kritieke volgorde verwerking van gebeurtenissen in het proces uit te stellen.

In al deze gevallen gebruiken, omdat de functie heeft de app zelf ontkoppeld, u hoeft niet te draaien van nieuwe exemplaren van de app de tijd. Azure Functions draait in plaats daarvan afzonderlijke functies discrete processen uitvoeren indien nodig.

## <a name="tooling"></a>Tooling

Systeemeigen integratie tussen Azure Cosmos DB en Azure Functions is beschikbaar in de Azure-portal.
* U kunt een Azure DB die Cosmos-trigger maken in de Azure Functions-portal. Zie voor instructies Quick Start [een trigger Azure Cosmos DB maken in de Azure-portal](https://aka.ms/cosmosdbtriggerportalfunc) ![een trigger Azure Cosmos DB maken in de Azure Functions-portal](./media/serverless-computing-database/azure-function-cosmos-db-trigger.png) 
* In de Azure Functions-portal kunt u ook invoer bindingen van Azure DB die Cosmos- en uitvoer bindingen voor andere typen triggers toevoegen. Zie voor instructies Quick Start [opslaan van ongestructureerde gegevens met behulp van Azure Functions en Cosmos DB](../azure-functions/functions-integrate-store-unstructured-data-cosmosdb.md).
    ![Een Azure DB die Cosmos-trigger maken in de Azure Functions-portal](./media/serverless-computing-database/function-portal-input-binding.png)
*   U kunt in de Azure DB die Cosmos-portal een Azure DB die Cosmos-trigger toevoegen aan een bestaande Azure-functie-app in dezelfde resourcegroep.
    ![Een Azure DB die Cosmos-trigger maken in de Azure Functions-portal](./media/serverless-computing-database/cosmos-db-portal.png)

## <a name="why-choose-azure-functions-integration-for-serverless-computing"></a>Waarom Azure Functions-integratie voor zonder server computing kiezen?

Azure Functions biedt de mogelijkheid voor het maken van schaalbare eenheden van werk of beknopte stukjes logica die op aanvraag kan worden uitgevoerd zonder het inrichten of infrastructuurbeheer. U hoeft te maken van een volledige app om te reageren op wijzigingen in uw Azure DB die Cosmos-database met behulp van Azure Functions, kunt u kleine herbruikbare functies voor specifieke taken maken. Daarnaast kun je Azure Cosmos DB gegevens als invoer of uitvoer naar een Azure-functie in reactie op een gebeurtenis, zoals een HTTP-aanvragen of een trigger is een time-out opgetreden.

Azure Cosmos-database is de aanbevolen database voor uw zonder server computing architectuur voor de volgende redenen:

* **Direct toegang tot al uw gegevens**: U hebt gedetailleerde toegang tot elke waarde die is opgeslagen, omdat Azure Cosmos DB [indexeert automatisch](indexing-policies.md) alle gegevens standaard en maakt deze indexen onmiddellijk beschikbaar. Dit betekent dat u zich kunt voortdurend query, bijwerken, en nieuwe items toevoegen aan uw database en onmiddellijk toegang via Azure Functions hebben.

* **Schemaloos**. Azure Cosmos DB is schemaloos - dus wordt er een unieke voor het afhandelen van elke gegevensuitvoer van een Azure-functie. Deze aanpak 'Alles verwerken' maakt het eenvoudig te maken van een breed scala aan functies die alle uitvoer naar Azure Cosmos DB.

* **Schaalbare doorvoer**. Doorvoer kan worden uitgebreid omhoog en omlaag direct in Azure Cosmos DB. Als er honderden of duizenden functies zoeken en schrijven naar dezelfde verzameling, u kunt opschalen uw [RU/s](request-units.md) om de belasting te verwerken. Alle functies kunnen werken in combinatie met uw toegewezen RU/s en uw gegevens is gegarandeerd [consistente](consistency-levels.md).

* **Globale replicatie**. U kunt Azure Cosmos DB gegevens repliceren [overal ter wereld](distribute-data-globally.md) latentie, geo-zoeken naar uw om gegevens te reduceren die het dichtst bij waar uw gebruikers zijn. Als aan alle Azure Cosmos DB-query's gegevens uit het triggers gebeurtenisafhankelijke gelezen gegevens van de Cosmos Azure DB die het dichtst bij de gebruiker.

Als u op zoek bent om te integreren met Azure Functions voor het opslaan van gegevens en niet grondige te indexeren hoeft of als u voor het opslaan van bijlagen en media-bestanden, moet de [Azure Blob Storage-trigger](../azure-functions/functions-bindings-storage-blob.md) mogelijk een betere optie.

Voordelen van Azure Functions: 

* **Gebeurtenisafhankelijke**. Azure Functions zijn gebeurtenisafhankelijke en een wijziging in de feed vanuit Azure Cosmos DB kunnen volgen. Dit betekent dat u hoeft niet te luisteren logica maken, u alleen Let om de wijzigingen die u voor luistert. 

* **Er zijn geen limieten**. Functies uitvoeren in parallelle instructies en de service draaien up zoveel als u nodig hebt. U instellen de parameters.

* **Geschikt is voor het snel taken**. De service draait nieuwe exemplaren van functies die telkens wanneer een gebeurtenis wordt geactiveerd en deze sluit zodra de functie is voltooid. U betaalt alleen voor de tijd die uw functies worden uitgevoerd.

Als u niet zeker weet of stroom, Logic Apps, Azure Functions of WebJobs het meest geschikt voor uw implementatie zijn, Zie [Kies tussen stroom, Logic Apps, functies en WebJobs](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md).

## <a name="next-steps"></a>Volgende stappen

Nu gaan we Azure Cosmos DB en Azure Functions echt verbinden: 

* [Een Azure DB die Cosmos-trigger maken in de Azure-portal](https://aka.ms/cosmosdbtriggerportalfunc)
* [De trigger van een HTTP-functies van Azure maken met een binding van Azure DB die Cosmos-invoer](https://aka.ms/cosmosdbinputbind)
* [Opslaan van ongestructureerde gegevens met behulp van Azure Functions en Cosmos-DB](../azure-functions/functions-integrate-store-unstructured-data-cosmosdb.md)
* [Azure DB Cosmos-bindingen en wordt geactiveerd](../azure-functions/functions-bindings-cosmosdb.md)


 




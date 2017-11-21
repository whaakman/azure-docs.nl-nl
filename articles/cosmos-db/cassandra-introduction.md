---
title: Inleiding tot de Azure Cosmos DB Cassandra API | Microsoft Docs
description: Ontdek hoe u Azure Cosmos-database gebruiken 'lift-en-shift' bestaande toepassingen en nieuwe toepassingen bouwen met behulp van de Cassandra stuurprogramma's en CQL die u al bekend met bent Cassandra-API.
services: cosmos-db
author: govindk
manager: jhubbard
documentationcenter: 
ms.assetid: 73839abf-5af5-4ae0-a852-0f4159bc00a0
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/20/2017
ms.author: govindk
ms.openlocfilehash: fe911b69278021262f1d90f2157bb78ca48db0db
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2017
---
# <a name="introduction-to-apache-cassandra-api-for-azure-cosmos-db"></a>Inleiding tot Apache Cassandra API voor Azure Cosmos DB

Azure Cosmos DB biedt de Cassandra-API (preview) voor toepassingen die zijn geschreven voor Apache Cassandra dat moeten premium mogelijkheden, zoals:

* [Schaalbare opslaggrootte en doorvoer](partition-data.md).
* [Directe globale distributie](distribute-data-globally.md)
* Latentie van slechts enkele milliseconden op het 99e percentiel.
* [Vijf goed gedefinieerde consistentieniveaus](consistency-levels.md)
* [Automatische indexering van gegevens](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) zonder te moeten omgaan met schema- en -beheer. 
* Hoge beschikbaarheid, alle back-ups door gegarandeerd [toonaangevende Sla's](https://azure.microsoft.com/support/legal/sla/cosmos-db/)

## <a name="what-is-the-azure-cosmos-db-apache-cassandra-api"></a>Wat is Azure Cosmos DB Apache Cassandra API?

Azure Cosmos-database kan worden gebruikt als gegevensopslag voor apps die zijn geschreven voor [Apache Cassandra](https://cassandra.apache.org/), met behulp van de Apache Cassandra-API. Dit betekent dat met behulp van de bestaande [Apache in licentie gegeven stuurprogramma's die compatibel zijn met CQLv4](https://cassandra.apache.org/doc/latest/getting_started/drivers.html?highlight=driver), uw toepassing voor Cassandra voortaan met de Azure-API voor Cassandra Cosmos-database communiceren kunnen geschreven. In veel gevallen kunt u overschakelen van het gebruik van Apache Cassandra voor het gebruik van Azure Cosmos DB Apache Cassandra API, door eenvoudigweg een verbindingsreeks. Met deze functionaliteit kunt u gemakkelijk maken en Cassandra API databasetoepassingen uitvoeren in de Azure-cloud met Azure Cosmos DB globale verdeling en [uitgebreide toonaangevende Sla's](https://azure.microsoft.com/support/legal/sla/cosmos-db), maar blijft bekend gebruiken vaardigheden en hulpprogramma's voor Cassandra API.

![Azure Cosmos DB Cassandra-API](./media/cassandra-introduction/cosmosdb-cassandra.png)

De API Cassandra kunt u communiceren met gegevens die zijn opgeslagen in Azure Cosmos-database met de querytaal Cassandra op basis van hulpprogramma's (zoals CQLSH) en Cassandra clientstuurprogramma's u al bekend bent met. Meer informatie over het in deze video Microsoft Mechanics met Principal Engineering Manager Kirill Gavrylyuk.

> [!VIDEO https://www.youtube.com/embed/1Sf4McGN1AQ]
>

## <a name="what-is-the-benefit-of-using-apache-cassandra-api-for-azure-cosmos-db"></a>Wat is het voordeel van het gebruik van Apache Cassandra API voor Azure Cosmos DB?

**Er is geen operations management**: als een echte wordt volledig service beheerde, Azure Cosmos DB zorgt ervoor dat beheerders Cassandra API's niet te hoeven maken over het beheren en controleren van een allerlei instellingen voor OS, JVM en yaml-bestanden en hun wisselwerking. Azure Cosmos DB biedt bewaking van doorvoer, latentie, opslag en beschikbaarheid en waarschuwingen kunnen worden geconfigureerd. 

**Prestatiebeheer**: Azure Cosmos DB biedt SLA back lage latentie leest en schrijft voor het 99th percentiel. Gebruikers hoeven niet te veel van de operationele overhead bieden goede lezen en schrijven van SLA's. Deze omvatten doorgaans compressie, het beheren van tombstones, waas filters instellen en replica lag planning. Azure DB Cosmos vergt weg de zorgen van het beheer van deze problemen en kunt u zich richten op de resultaten van de toepassing.

**Automatische indexering**: Azure Cosmos DB indexeert automatisch alle kolommen van de tabel in Cassandra API-database. Azure Cosmos-database is niet vereist voor het maken van secundaire indexen om query's te versnellen. Dit zorgt voor lage latentie lezen en schrijven tijdens de automatische consistent te indexeren. 

**Mogelijkheid om te gebruiken van bestaande code en hulpprogramma's**: Azure Cosmos DB biedt kabel protocol niveau compatibiliteit met bestaande SDK's en hulpprogramma's. Deze compatibiliteit zorgt ervoor dat u kunt uw bestaande codebase met Cassandra API van Azure Cosmos DB met trivial wijzigingen.

**Doorvoer en opslag elasticiteit**: Azure Cosmos-platform biedt elasticiteit van gegarandeerde doorvoer tussen regio's via eenvoudige portal, PowerShell of CLI-bewerkingen. U kunt schalen Azure Cosmos DB tabellen met voorspelbare prestaties naadloos wanneer uw toepassing groeit. Azure Cosmos DB ondersteunt Cassandra API-tabellen die kunnen worden geschaald naar grootten vrijwel onbeperkte opslag. 

**Globale distributie en de beschikbaarheid**: Azure Cosmos DB biedt de mogelijkheid om gegevens in de gehele Azure-regio's zodat gebruikers met een lage latentie ervaring terwijl de beschikbaarheid te distribueren. Azure Cosmos DB biedt 99,99% beschikbaarheid binnen een regio en 99,999% beschikbaarheid lezen over de regio's met de overhead van geen bewerkingen. Azure Cosmos DB is beschikbaar in 30 + [Azure-gebieden](https://azure.microsoft.com/regions/services/). Meer informatie [Distribueer gegevens globaal](distribute-data-globally.md). 

**Keuze van de consistentie**: Azure Cosmos DB biedt de keuze van vijf goed gedefinieerde consistentieniveaus voor een optimale balans tussen de consistentie en prestaties. Deze consistentieniveaus zijn sterk, gebonden-verouderd, sessie, consistente voorvoegsel en uiteindelijk. Deze gedetailleerde, goed gedefinieerde consistentieniveaus kunt ontwikkelaar afweging geluid tussen de consistentie, beschikbaarheid en latentie. Zie [Consistentieniveaus gebruiken om de beschikbaarheid en prestaties te maximaliseren](consistency-levels.md) voor meer informatie. 

**Enterprise hoogwaardige**: beveiligd en compatibel zijn standaard: Azure cosmos DB biedt [naleving certificeringen](https://www.microsoft.com/trustcenter) zodat gebruikers kunnen het platform gebruiken zonder dat u problemen met naleving. Azure Cosmos DB biedt ook versleuteling in rust en bij beweging, firewall voor IP- en auditlogboeken voor vlak controleactiviteiten.  

<a id="sign-up-now"></a>
## <a name="sign-up-now"></a>Nu aanmelden 

Als u al een Azure-abonnement hebt, kunt u zich aanmeldt bij het programma Cassandra API (preview) in de [Azure-portal](https://aka.ms/cosmosdb-cassandra-signup).  Als u geen ervaring met Azure, zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/free) waar u 12 maanden gratis toegang tot Azure Cosmos DB krijgen. De volgende stappen om aan te vragen toegang tot het programma Cassandra API (preview).

1. In de [Azure-portal](https://portal.azure.com), klikt u op **nieuw** > **Databases** > **Azure Cosmos DB**. 

2. Selecteer op de pagina nieuwe Account **Cassandra** in het vak API. 

3. In de **abonnement** Selecteer de Azure-abonnement dat u wilt gebruiken voor dit account.

4. Klik op **zich registreren voor de preview vandaag**.

    ![Azure Cosmos DB Cassandra-API](./media/cassandra-introduction/cassandra-sign-up.png)

3. Klik in de aanmelding voor vandaag voorbeeldvenster op **OK**. 

    Als u de aanvraag indient, de status gewijzigd in **in afwachting van goedkeuring** in het deelvenster met de nieuwe account. 

Nadat u uw aanvraag indient, wacht u totdat de e-mailmelding dat uw aanvraag is goedgekeurd. Vanwege het grote aantal aanvragen ontvangt u meldingen binnen een week. U hoeft niet te maken van een ondersteuningsticket voor het voltooien van de aanvraag. Aanvragen wordt gecontroleerd in de volgorde waarin ze werden ontvangen. 

## <a name="how-to-get-started"></a>Hoe gaat u aan de slag
Eenmaal u het preview-programma hebt toegevoegd, voert u de snelstartgidsen Cassandra API voor het maken van een app met behulp van de API Cassandra:

* [Snelstartgids: Een Cassandra bouwen met Node.js en Azure DB die Cosmos-web-app](create-cassandra-nodejs.md)
* [Snelstartgids: Een Cassandra bouwen met Java en Azure DB die Cosmos-web-app](create-cassandra-java.md)
* [Snelstartgids: Een Cassandra bouwen web-app met .NET- en Azure Cosmos-DB](create-cassandra-dotnet.md)
* [Snelstartgids: Een Cassandra bouwen web-app met Python en Azure Cosmos-DB](create-cassandra-python.md)

## <a name="next-steps"></a>Volgende stappen

Informatie over de API van Azure Cosmos DB Cassandra is geïntegreerd in de algehele Azure DB die Cosmos-documentatie, maar hier zijn enkele verwijzingen naar u op weg:

* Ga als volgt de [snelstartgidsen](create-cassandra-nodejs.md) een account en een nieuwe app maken met behulp van een Git-voorbeeld
* Ga als volgt de [zelfstudie](tutorial-develop-cassandra-java.md) programmatisch maken van een nieuwe app.
* Ga als volgt de [Cassandra importeren gegevens zelfstudie](cassandra-import-data.md) uw bestaande gegevens wilt importeren in Azure Cosmos DB.
* Lees de [Veelgestelde vragen over](faq.md#cassandra).

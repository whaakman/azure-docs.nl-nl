---
title: Inleiding tot Stream Analytics | Microsoft Docs
description: Lees over Stream Analytics, een beheerde service waarmee u streaminggegevens van het Internet of Things (IoT) in realtime kun analyseren.
keywords: analyse als een service, beheerde services, verwerking van streams, analyse van streams, wat is analyse van streams
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 613c9b01-d103-46e0-b0ca-0839fee94ca8
ms.service: stream-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/16/2017
ms.author: jeffstok
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 421bdfb3132bc8c9f193bcca8d55c9cf9eba1c3b
ms.contentlocale: nl-nl
ms.lasthandoff: 07/04/2017


---

<a id="what-is-stream-analytics" class="xliff"></a>

# Wat is een Stream Analytics?

Azure Stream Analytics is een volledig beheerde verwerkingsengine voor gebeurtenissen, waarmee u in real time analytische berekeningen kunt uitvoeren op gegevensstromen. De gegevens kunnen afkomstig zijn van apparaten, sensoren, websites, socialemediafeeds, toepassingen, infrastructuursystemen, enzovoort. 

<a id="what-can-i-use-stream-analytics-for" class="xliff"></a>

## Waarvoor kan ik Stream Analytics gebruiken?

Met Stream Analytics kunt u hoge volumes aan gegevensstromen van apparaten of processen onderzoeken, informatie onttrekken aan de gegevensstroom en zoeken naar patronen, trends en relaties. Op basis van de bevindingen kunt u vervolgens toepassingstaken uitvoeren. U kunt bijvoorbeeld waarschuwingen genereren, automatiseringswerkstromen starten, informatie verzenden naar een rapportageprogramma zoals Power BI of gegevens opslaan voor later onderzoek. 

Voorbeelden van scenario’s voor Stream Analytics zijn:

* Persoonlijke realtimeanalyses van aandelenkoersen en meldingen van financiële dienstverleners.
* Fraudedetectie in real time op basis van transactiegegevens. 
* Services ter beveiliging van gegevens en identiteit.
* Analyse van gegevens die zijn gegenereerd door sensoren en actuators in fysieke objecten (het Internet of Things, IoT).
* Clickstream-analyses op internet.
* CRM-toepassingen (Customer Relationship Management) voor bijvoorbeeld het genereren van waarschuwingen wanneer de klantervaring verslechtert gedurende een bepaalde periode.

<a id="how-does-stream-analytics-work" class="xliff"></a>

## Hoe werkt Stream Analytics?

In het volgende diagram toont de pijplijn van Stream Analytics en laat zien hoe de gegevens worden opgenomen, geanalyseerd en worden verzonden voor presentatie of actie. 

![Stream Analytics-pijplijn](./media/stream-analytics-introduction/stream_analytics_intro_pipeline.png)

Stream Analytics begint met een gegevensstroom als bron. De gegevens kunnen worden opgenomen in Azure vanaf een apparaat met een Azure Event Hub of Azure IoT Hub. De gegevens kunnen ook worden opgehaald uit een gegevensarchief zoals Azure Blob Storage. 

Voor het onderzoeken van de stroom maakt u een Stream Analytics-*taak* die aangeeft waar de gegevens vandaan komen. De taak bevat ook een specificatie van een *transformatie*&mdash;hoe er wordt gezocht naar gegevens, patronen of relaties. Voor deze taak ondersteunt Stream Analytics een SQL-achtige querytaal, waarmee u gegevens kunt filteren, sorteren en combineren en gegevensstromen kunt samenvoegen gedurende een bepaalde periode.

Als laatste bevat de taak een specificatie van waar de getransformeerde gegevens naartoe worden verzonden. Hiermee kunt u instellen wat er moet gebeuren als reactie op de informatie die u hebt geanalyseerd. Als reactie op een analyse kunt u bijvoorbeeld het volgende doen:

* Een opdracht verzenden om de instellingen van een apparaat te wijzigen. 
* Gegevens verzenden naar een wachtrij die wordt bewaakt door een proces dat actie onderneemt op basis van de bevindingen. 
* Gegevens verzenden naar een Power BI-dashboard voor rapportage.
* Gegevens verzenden naar een opslaglocatie zoals Data Lake Store, een SQL Server-database, Azure Blob Storage of Azure Table Storage.

Terwijl de taak wordt uitgevoerd, kunt u die bewaken en instellen hoeveel gebeurtenissen per seconde er moeten worden verwerkt. U kunt ook diagnostische logboeken laten genereren voor het oplossen van problemen.

<a id="key-capabilities-and-benefits" class="xliff"></a>

## Belangrijkste mogelijkheden en voordelen

Stream Analytics is gebruiksvriendelijk, flexibel, schaalbaar tot elke taakgrootte en voordelig.

<a id="connectivity-to-many-inputs-and-outputs" class="xliff"></a>

### Verbindingen met vele soorten invoer en uitvoer

Stream Analytics maakt rechtstreeks verbinding met [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) en [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/) voor opname van streams, en met de [Azure Blob Storage-service](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage-accounts) voor opname van historische gegevens. Als u gegevens ontvangt van gebeurtenishubs, kunt u Stream Analytics combineren met andere gegevensbronnen en verwerkingsengines.

De invoer van taken kan ook referentiegegevens (statische of langzaam veranderende gegevens) omvatten. U kunt gegevensstromen samenvoegen met deze referentiegegevens voor het uitvoeren van zoekbewerkingen, net zoals u dat zou doen met databasequery’s.

De uitvoer van een Stream Analytics-taak kan op vele manieren worden gerouteerd. De uitvoer kan worden geschreven naar opslag, zoals Azure Storage-blobs of -tabellen, Azure SQL DB, Azure Data Lake Stores of Azure Cosmos DB. Vanaf daar kunnen de gegevens bijvoorbeeld worden verzonden voor batchanalyse via Azure HDInsight. U kunt de uitvoer ook verzenden voor gebruik door andere serviceprocessen, zoals gebeurtenishubs, Azure Service Bus-onderwerpen of wachtrijen. U kunt de uitvoer ook verzenden voor visualisatie in Power BI.

<a id="ease-of-use" class="xliff"></a>

### Gebruiksgemak

Voor het definiëren van transformaties gebruikt u een eenvoudige declaratieve [Stream Analytics-querytaal](https://msdn.microsoft.com/library/azure/dn834998.aspx), waarmee u geavanceerde analyses kunt maken zonder te hoeven programmeren. De querytaal gebruikt gegevensstromen als invoer. Vervolgens kunt u de gegevens filteren en sorteren, waarden combineren, berekeningen uitvoeren, gegevens samenvoegen (binnen een stroom of naar referentiegegevens) en georuimtelijke functies gebruiken. U kunt query’s in de portal bewerken met behulp van IntelliSense en syntaxiscontrole, en u kunt query’s testen met voorbeeldgegevens afkomstig uit de live gegevensstroom.

<a id="extensible-query-language" class="xliff"></a>

### Uitbreidbare querytaal

U kunt de mogelijkheden van de querytaal uitbreiden door extra functie te definiëren en aan te roepen. U kunt functieaanroepen definiëren in de Azure Machine Learning-service en gebruikmaken van Azure Machine Learning-oplossingen. Daarnaast kunt u door de gebruiker gedefinieerde functies (UDF’s) van JavaScript integreren voor het uitvoeren van complexe berekeningen als deel van de Stream Analytics-query.

<a id="scalability" class="xliff"></a>

### Schaalbaarheid

Stream Analytics kan maximaal 1 GB aan inkomende gegevens per seconde verwerken. Dankzij integratie met [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) en [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/) kunnen taken miljoenen gebeurtenissen per seconde opnemen, afkomstig van verbonden apparaten, clickstreams, logboekbestanden, enzovoort. Met de partitiefunctie van Event Hubs kunt u berekeningen partitioneren in logische stappen, die elk verder kunnen worden gepartitioneerd voor meer schaalbaarheid.

<a id="low-cost" class="xliff"></a>

### Lage kosten

Stream Analytics is een cloudservice die is geoptimaliseerd met het oog op lage kosten. U betaalt voor gebruik op basis van het aantal gebruikte streamingeenheden en de hoeveelheid gegevens die door het systeem wordt verwerkt. Gebruik wordt berekend op basis van het aantal gebeurtenissen dat wordt verwerkt en de hoeveelheid computercapaciteit binnen het cluster die nodig is om de Stream Analytics-taken af te handelen.

<a id="reliability-quick-recovery-and-repeatability" class="xliff"></a>

### Betrouwbaarheid, snel herstel en herhaalbaarheid

Als beheerde cloudservice draagt Stream Analytics bij aan het voorkomen van gegevensverlies en zorgt de service voor bedrijfscontinuïteit. De service beschikt over ingebouwde herstelmogelijkheden voor het geval dat er fouten optreden. Dankzij het vermogen om de interne toestand te handhaven, biedt de service herhaalbare resultaten zodat gebeurtenissen kunnen worden gearchiveerd en verwerking later opnieuw kan worden toegepast, en altijd dezelfde resultaten worden verkregen. Hierdoor kunt u teruggaan in de tijd en berekeningen onderzoeken terwijl de hoofdoorzaak wordt onderzocht, wat-als-analyses worden uitgevoerd, enzovoort.

<a id="next-steps" class="xliff"></a>

## Volgende stappen

* Aan de slag met het [experimenteren met invoer en query’s van IoT-apparaten](stream-analytics-get-started-with-azure-stream-analytics-to-process-data-from-iot-devices.md).
* Bouw een [end-to-end Stream Analytics-oplossing](stream-analytics-real-time-fraud-detection.md) voor het onderzoeken van telefoonmetagegevens op frauduleuze oproepen.
* Meer informatie over de SQL-achtige querytaal voor Stream Analytics en over unieke concepten zoals [vensterfuncties](stream-analytics-window-functions.md).
* Meer informatie over het [schalen van Stream Analytics-taken](stream-analytics-scale-jobs.md). 
* Meer informatie over het [integreren van Stream Analytics en Azure Machine Learning](stream-analytics-machine-learning-integration-tutorial.md).
* U vindt antwoorden op uw vragen over Stream Analytics in het [Azure Stream Analytics-internetforum](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics).



---
title: Taak schaling mogelijk met Azure Stream Analytics & AzureML functies | Microsoft Docs
description: Meer informatie over het correct schalen Stream Analytics-taken (partitioneren, SU hoeveelheid en meer) bij gebruik van Azure Machine Learning-functies.
keywords: 
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 47ce7c5e-1de1-41ca-9a26-b5ecce814743
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 1e1c3724462a4d2a67eab3ef42867d2aeb5d3fa1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="scale-your-stream-analytics-job-with-azure-machine-learning-functions"></a>Schalen van uw Stream Analytics-taak met Azure Machine Learning-functies
Vaak is het eenvoudig instellen van een Stream Analytics-taak en voorbeeldgegevens doorlopen. Wat moeten we doen als we moet dezelfde taak uitvoeren met hogere gegevensvolume? Is vereist om te begrijpen hoe de Stream Analytics-taak zo configureren dat het schalen. In dit document richten we op de speciale aspecten van de Stream Analytics-taken met Machine Learning functions schalen. Zie het artikel voor meer informatie over het schalen van Stream Analytics-taken in het algemeen [taken schalen](stream-analytics-scale-jobs.md).

## <a name="what-is-an-azure-machine-learning-function-in-stream-analytics"></a>Wat is een Azure Machine Learning-functie in Stream Analytics?
Een Machine Learning-functie in Stream Analytics kan worden gebruikt als een reguliere functieaanroep in de Stream Analytics query language. De functieaanroepen zijn echter achter de scene daadwerkelijk Azure Machine Learning-webservice-aanvragen. Machine Learning-webservices ondersteuning voor 'batchverwerking' meerdere rijen die Mini batch in de dezelfde web service API-aanroep voor het verbeteren van de totale doorvoer wordt genoemd. Zie de volgende artikelen voor meer informatie; [Azure Machine Learning-functies in Stream Analytics](https://blogs.technet.microsoft.com/machinelearning/2015/12/10/azure-ml-now-available-as-a-function-in-azure-stream-analytics/) en [Azure Machine Learning-webservices](../machine-learning/studio/consume-web-services.md).

## <a name="configure-a-stream-analytics-job-with-machine-learning-functions"></a>Configureren van een Stream Analytics-taak met Machine Learning-functies
Bij het configureren van een Machine Learning-functie voor Stream Analytics-taak, zijn er twee parameters rekening moet houden, de batchgrootte van de Machine Learning-functieaanroepen en de streaming-eenheden (SUs) ingericht voor de Stream Analytics-taak. Om te bepalen de juiste waarden voor deze, moet eerst een beslissing worden gemaakt tussen latentie en doorvoer, dat wil zeggen, de latentie van de Stream Analytics-taak en de doorvoer van elke SU. SUs kunnen altijd worden toegevoegd aan een taak voor het verbeteren van doorvoer van een goed gepartitioneerde Stream Analytics query, hoewel aanvullende SUs de kosten verhoogt van het uitvoeren van de taak.

Daarom is het belangrijk om te bepalen de *tolerantie* van latentie in een Stream Analytics-taak wordt uitgevoerd. Extra latentie Azure Machine Learning-serviceaanvragen worden uitgevoerd verhoogd met de batchgrootte, die de latentie van de Stream Analytics-taak verbindingen natuurlijk. Aan de andere kant vergroten met de batch kan de Stream Analytics-taak verwerken * meer gebeurtenissen met de *hetzelfde nummer* van Machine Learning web serviceaanvragen. De toename van Machine Learning web service latentie is vaak de verhoging van batchgrootte sublineaire dus is het belangrijk rekening houden met de meest voordelige batchgrootte voor een Machine Learning-webservice in een situatie met opgegeven. De standaardgrootte van de batch voor de webservice-aanvragen is 1000 en kunnen worden gewijzigd met behulp van de [Stream Analytics REST-API](https://msdn.microsoft.com/library/mt653706.aspx "Stream Analytics REST-API") of de [PowerShell-client voor stroom Analytics](stream-analytics-monitor-and-manage-jobs-use-powershell.md "PowerShell-client voor Stream Analytics").

Zodra een batchgrootte is bepaald, het aantal streaming-eenheden (SUs) kunnen worden bepaald, gebaseerd op het aantal gebeurtenissen dat de functie moet verwerken per seconde. Zie voor meer informatie over het streaming-eenheden, [Stream Analytics taken schalen](stream-analytics-scale-jobs.md).

Er zijn in het algemeen 20 gelijktijdige verbindingen met de Machine Learning-webservice voor elke 6 SUs, behalve dat 1 SU jobs en taken van 3 SU 20 gelijktijdige verbindingen ook.  Als de invoergegevens snelheid 200.000 gebeurtenissen per seconde en de batchgrootte op de standaardwaarde van 1000 blijft is de latentie van de resulterende service met 1000 gebeurtenissen Mini batch 200 ms. Dit betekent dat elke verbinding vijf aanvragen kunt maken met de Machine Learning-webservice gedurende een seconde. Met 20 verbindingen kan de Stream Analytics-taak 20.000 gebeurtenissen in 200 ms en daarom 100.000 gebeurtenissen verwerken in een tweede. Voor het verwerken van 200.000 gebeurtenissen per seconde, dus moet de Stream Analytics-taak 40 gelijktijdige verbindingen die afkomstig uit 12 SUS zijn. Het volgende diagram illustreert de aanvragen van de Stream Analytics-taak met de Machine Learning web service-eindpunt: elke 6 SUs heeft 20 gelijktijdige verbindingen met Machine Learning-webservice op max.

![Stream Analytics schalen met Machine Learning functies twee taak voorbeeld](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-00.png "Scale Stream Analytics met Machine Learning functies twee taak voorbeeld")

In het algemeen ***B*** voor batchgrootte, ***L*** voor de web service latentieperiode batchgrootte B in milliseconden, de doorvoer van een Stream Analytics taak met ***N*** SUs is:

![Stream Analytics met Machine Learning functies formule schalen](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-02.png "Stream Analytics met Machine Learning functies formule schalen")

Een extra aandacht mogelijk 'maximumaantal gelijktijdige aanroepen van de' aan de kant van Machine Learning web service, verdient het aanbeveling om in te stellen dit aan de maximumwaarde (momenteel 200).

Raadpleeg voor meer informatie over deze instelling de [voor Machine Learning-webservices schalen artikel](../machine-learning/studio/scaling-webservice.md).

## <a name="example--sentiment-analysis"></a>Voorbeeld: gevoel analyse
Het volgende voorbeeld bevat een Stream Analytics-taak met de analyse gevoel Machine Learning-functie, zoals beschreven in de [Stream Analytics, Machine Learning-integratie zelfstudie](stream-analytics-machine-learning-integration-tutorial.md).

De query is een eenvoudige gepartitioneerd volledig query gevolgd door de **gevoel** werkt als volgt weergegeven:

    WITH subquery AS (
        SELECT text, sentiment(text) as result from input
    )

    Select text, result.[Score]
    Into output
    From subquery

Neem het volgende scenario; met een doorvoersnelheid van 10.000 tweets per seconde moet een Stream Analytics-taak worden gemaakt voor het gevoel over analyse van de tweets (gebeurtenissen) uitvoeren. 1 SU gebruikt, kan deze Stream Analytics-taak zijn het verkeer dan verwerken? Met behulp van de standaardgrootte van batch 1000 moet de taak kunnen blijven van de invoer. Verder moet de toegevoegde Machine Learning-functie genereren niet meer dan een seconde van latentie, dit de algemene is standaardwaarde latentie van de analyse gevoel Machine Learning-webservice (met een standaardbatchgrootte 1000). De Stream Analytics-taak **algehele** of latentie end-to-end doorgaans een paar seconden. Meer gedetailleerde verdiepen in deze Stream Analytics-taak *vooral* de Machine Learning-functieaanroepen. De batchgrootte 1000 hebben, een doorvoer van 10.000 gebeurtenissen netwerkverzoeken ongeveer 10 webservice. Er zijn onvoldoende gelijktijdige verbindingen voor deze invoer verkeer zelfs met 1 SU.

Wat gebeurt er als verhoogt de snelheid van de invoer gebeurtenissen door 100 x maar nu de Stream Analytics-taak moet 1.000.000 tweets per seconde verwerken? Er zijn twee opties:

1. Verhoog de batchgrootte, of
2. De invoerstroom verwerkt de gebeurtenissen parallel partitie

Met de eerste optie, de taak **latentie** verhoogt.

Met de tweede optie moet meer SUs worden ingericht en daarom genereren meer gelijktijdige aanvragen van Machine Learning web service. Dit betekent dat de taak **kosten** verhoogt.

Stel de latentie van de analyse gevoel Machine Learning-webservice 200 ms voor batches van 1000 gebeurtenissen of hieronder 250 ms voor 5.000 gebeurtenis batches, 300 ms voor batches van 10.000 gebeurtenissen of 500 ms voor batches 25.000-gebeurtenis.

1. Met de eerste optie (**niet** meer SUs inrichting), de batchgrootte van de kan worden verhoogd tot **25.000**. Op zijn beurt Hierdoor kan de taak 1.000.000 om gebeurtenissen te verwerken met 20 gelijktijdige verbindingen met de Machine Learning-webservice (met een latentie van 500 ms per aanroep). Zo zou de extra latentie van de Stream Analytics-taak als gevolg van de aanvragen van de functie gevoel tegen de Machine Learning web service-aanvragen worden verhoogd van **200 ms** naar **500 ms**. Echter, batchgrootte **kan niet** worden verhoogd oneindig de Machine Learning-webservices moet de grootte van de nettolading van een aanvraag 4 MB of kleiner webservice aanvragen time-out na 100 seconden van bewerking.
2. Met behulp van de tweede optie, de batchgrootte van de is links op 1000, met een latentie van 200 ms web service, elke 20 gelijktijdige verbindingen met de webservice is in staat zijn om gebeurtenissen te verwerken 1000 * 20 * 5 = 100.000 per seconde. Als u wilt verwerken 1.000.000 gebeurtenissen per seconde, moet de taak zodat 60 SUs. Vergeleken met de eerste optie, zouden Stream Analytics-taak meer batch webserviceaanvragen, op zijn beurt genereren van een extra kosten.

Hieronder vindt u een tabel voor de doorvoer van de Stream Analytics-taak voor verschillende SUs en batch grootte (in het aantal gebeurtenissen per seconde).

| Batchgrootte (ML latentie) | 500 (200 ms) | 1000 (200 ms) | 5.000 (250 ms) | 10.000 (300 ms) | 25.000 (500 ms) |
| --- | --- | --- | --- | --- | --- |
| **1 SU** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **SUs 3** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **6 SUs** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **SUs 12** |5,000 |10.000 |40,000 |60,000 |100,000 |
| **18 SUs** |7,500 |15,000 |60,000 |90,000 |150,000 |
| **24 SUs** |10.000 |20,000 |80,000 |120,000 |200.000 |
| **…** |… |… |… |… |… |
| **60 SUs** |25,000 |50,000 |200.000 |300,000 |500,000 |

Nu moet u al een goed begrip van de werking van Machine Learning-functies in Stream Analytics hebben. U waarschijnlijk begrijpen ook dat gegevens van gegevensbronnen Stream Analytics-taken 'pull' en elke 'pull' retourneert een reeks gebeurtenissen voor de Stream Analytics-taak verwerken. Hoe deze pull-model gevolgen de Machine Learning web serviceaanvragen?

De batchgrootte die we ingesteld voor Machine Learning-functies niet normaal gesproken exact worden gedeeld door het aantal gebeurtenissen dat is geretourneerd door elke Stream Analytics-taak 'pull'. Dit gebeurt wanneer dat de Machine Learning-webservice wordt aangeroepen met 'gedeeltelijke' batches. Dit wordt gedaan om te worden extra taak latentie overhead in samenvoegen gebeurtenissen pull pull.

## <a name="new-function-related-monitoring-metrics"></a>Nieuwe functie-gerelateerde bewaking metrische gegevens
In het gebied van de Monitor van een Stream Analytics-taak, zijn drie extra functie-gerelateerde statistieken toegevoegd. Dit zijn de aanvragen van functie, gebeurtenissen voor de functie en MISLUKTE functie aanvragen, zoals wordt weergegeven in de onderstaande afbeelding.

![Stream Analytics met Machine Learning functies metrische gegevens schalen](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-01.png "schalen Stream Analytics met Machine Learning functies metrische gegevens")

De zijn als volgt gedefinieerd:

**AANVRAGEN van functie**: het aantal aanvragen van functie.

**GEBEURTENISSEN voor de functie**: het aantal gebeurtenissen in de functie-aanvragen.

**MISLUKTE aanvragen voor de functie**: het aantal mislukte functie aanvragen.

## <a name="key-takeaways"></a>Sleutel Takeaways
Als u wilt geven een overzicht van de belangrijkste punten om te schalen van een Stream Analytics-taak met Machine Learning-functies, moeten de volgende items worden beschouwd:

1. De snelheid van de invoer-gebeurtenissen
2. De verdragen latentie voor de actieve Stream Analytics-taak (en dus de batchgrootte van de Machine Learning web service-aanvragen)
3. De ingerichte Stream Analytics SUs en het aantal aanvragen van Machine Learning web service (de extra functie-gerelateerde kosten)

Een voorbeeld is een volledig gepartitioneerde Stream Analytics query gebruikt. Als een complexere query nodig is de [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) is een zeer waardevolle resource voor aanvullende hulp van de Stream Analytics-team.

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over Stream Analytics, Zie:

* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

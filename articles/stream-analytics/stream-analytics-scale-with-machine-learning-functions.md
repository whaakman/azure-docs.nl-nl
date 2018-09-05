---
title: Schalen van Machine Learning-functies in Azure Stream Analytics
description: In dit artikel wordt beschreven hoe u Stream Analytics-taken die gebruikmaken van Machine Learning-functies, door het configureren van partitionering en stream eenheden schalen.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: 115273086eeb88064c4b179f67d2d400d9f84692
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43696095"
---
# <a name="scale-your-stream-analytics-job-with-azure-machine-learning-functions"></a>Schalen van uw Stream Analytics-taak met Azure Machine Learning-functies
Het is vrij eenvoudig instellen van een Stream Analytics-taak en enkele voorbeeldgegevens doorlopen. Wat moeten we doen als we moeten de dezelfde taak uitvoeren met hogere gegevensvolume? Hiervoor inzicht in hoe u de Stream Analytics-taak configureren zodat deze kan worden geschaald. In dit document, we ons richten op de speciale aspecten van het schalen van Stream Analytics-taken met Machine Learning-functies. Zie het artikel voor meer informatie over het schalen van Stream Analytics-taken in het algemeen [taken schalen](stream-analytics-scale-jobs.md).

## <a name="what-is-an-azure-machine-learning-function-in-stream-analytics"></a>Wat is een Azure Machine Learning-functie in Stream Analytics?
Een Machine Learning-functie in Stream Analytics kan worden gebruikt als een reguliere functieaanroep in de Stream Analytics-querytaal. De functieaanroepen zijn echter achter de scène daadwerkelijk Azure Machine Learning-webservice-aanvragen. Machine Learning-webservices ondersteuning "batchverwerking" meerdere rijen die Mini batch in de dezelfde web service API-aanroep voor het verbeteren van de totale doorvoer wordt genoemd. Zie voor meer informatie, [Azure Machine Learning-functies in Stream Analytics](https://blogs.technet.microsoft.com/machinelearning/2015/12/10/azure-ml-now-available-as-a-function-in-azure-stream-analytics/) en [Azure Machine Learning-webservices](../machine-learning/studio/consume-web-services.md).

## <a name="configure-a-stream-analytics-job-with-machine-learning-functions"></a>Configureren van een Stream Analytics-taak met Machine Learning-functies
Wanneer u een Machine Learning-functie voor Stream Analytics-taak configureert, zijn er twee parameters om te overwegen, de batchgrootte van de Machine Learning functieaanroepen en de streaming-eenheden (su's) voor de Stream Analytics-taak is ingericht. Om te bepalen de juiste waarden voor deze, moet eerst een beslissing worden gemaakt tussen latentie en doorvoer, dat wil zeggen, latentie van de Stream Analytics-taak en de doorvoer van elke SU. SUs kunnen altijd worden toegevoegd aan een taak voor het verhogen van de doorvoer van een goed gepartitioneerde Stream Analytics-query, maar aanvullende SUs de kosten verhoogt van het uitvoeren van de taak.

Daarom is het belangrijk om te bepalen de *tolerantie* van latentie bij het uitvoeren van een Stream Analytics-taak. Aanvullende neemt de latentie aanvragen voor Azure Machine Learning-service wordt uitgevoerd op een natuurlijke manier toe met de batchgrootte, die verbindingen van de latentie van de Stream Analytics-taak. Aan de andere kant de Stream Analytics-taak voor het verwerken van batch vergroten kunt * meer gebeurtenissen met de *hetzelfde aantal* van Machine Learning web serviceaanvragen. De toename van Machine Learning web service latentie is vaak sublineaire op de toename van de batchgrootte van het is dus belangrijk rekening houden met de meest betaalbare batchgrootte voor een Machine Learning-webservice in een bepaalde situatie. De standaardgrootte van de batch voor de webservice-aanvragen is 1000 en kan worden gewijzigd met behulp van de [REST-API voor Stream Analytics](https://msdn.microsoft.com/library/mt653706.aspx "REST-API voor Stream Analytics") of de [PowerShell-client voor Stream Analytics](stream-analytics-monitor-and-manage-jobs-use-powershell.md "PowerShell-client voor Stream Analytics").

Als een batchgrootte is bepaald, het aantal streaming-eenheden (su's) kunnen worden bepaald, afhankelijk van het aantal gebeurtenissen dat de functie moet verwerken per seconde. Zie voor meer informatie over het streaming-eenheden, [schalen van Stream Analytics-taken](stream-analytics-scale-jobs.md).

Er zijn in het algemeen is 20 gelijktijdige verbindingen met de Machine Learning-webservice voor elke 6 SUs, behalve dat 1 SU-taken en taken van 3 SU 20 gelijktijdige verbindingen ook.  Bijvoorbeeld, als het percentage invoergegevens 200.000 gebeurtenissen per seconde is en de batchgrootte is links op de standaardwaarde van 1000 is de latentie van de resulterende web service met 1000 gebeurtenissen Mini batch 200 ms. Dit betekent dat elke verbinding vijf aanvragen kan versturen naar de Machine Learning-webservice in een tweede. Met 20 verbindingen kan de Stream Analytics-taak 20.000 gebeurtenissen in 200 ms en daarom 100.000 gebeurtenissen verwerken in een tweede. Voor het verwerken van 200.000 gebeurtenissen per seconde, dus moet de Stream Analytics-taak 40 gelijktijdige verbindingen, die afkomstig uit 12 SUS zijn. Het volgende diagram illustreert de aanvragen van de Stream Analytics-taak voor het Machine Learning web service-eindpunt – elke 6 SUs is 20 gelijktijdige verbindingen met Machine Learning-webservice op max.

![Schalen van Stream Analytics met Machine Learning-functies twee taak voorbeeld](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-00.png "schaal Stream Analytics met Machine Learning-functies twee taak voorbeeld")

In het algemeen ***B*** voor batchgrootte, ***L*** voor de web service latentieperiode batchgrootte B in milliseconden, de doorvoer van een Stream Analytics job met ***N*** SUs is:

![Schalen van Stream Analytics met Machine Learning-functies formule](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-02.png "schalen van Stream Analytics met Machine Learning-functies formule")

Een extra aandacht mogelijk 'max gelijktijdige aanroepen van de' aan de kant van Machine Learning web service, het is raadzaam om in te stellen deze aan de maximumwaarde (momenteel 200).

Raadpleeg voor meer informatie over deze instelling de [artikel schalen voor Machine Learning-webservices](../machine-learning/studio/scaling-webservice.md).

## <a name="example--sentiment-analysis"></a>Voorbeeld: Sentimentanalyse
Het volgende voorbeeld bevat een Stream Analytics-taak met de sentimentanalyse Machine Learning-functie, zoals beschreven in de [zelfstudie voor integratie van Stream Analytics, Machine Learning](stream-analytics-machine-learning-integration-tutorial.md).

De query is een eenvoudige gepartitioneerd volledig query gevolgd door de **sentiment** functioneren, als volgt weergegeven:

    WITH subquery AS (
        SELECT text, sentiment(text) as result from input
    )

    Select text, result.[Score]
    Into output
    From subquery

Houd rekening met het volgende scenario; met een doorvoer van 10.000 tweets per seconde moet een Stream Analytics-taak worden gemaakt voor het uitvoeren van sentimentanalyse voor tweets (gebeurtenissen). 1 SU gebruikt, kan deze Stream Analytics-taak zijn kan het verkeer verwerken? Met behulp van de standaardgrootte van de batch van 1000 de taak zou het mogelijk om te blijven met de invoer. Verder de toegevoegde Machine Learning-functie moet worden gegenereerd niet meer dan een seconde latentie, dit de algemene is standaardwaarde latentie van de sentimentanalyse Machine Learning-webservice (met een standaard-batchgrootte van 1000). De Stream Analytics-taak **algehele** of end-to-end latentie meestal een paar seconden. In deze Stream Analytics-taak, een meer gedetailleerde Kijk *vooral* de Machine Learning-functieaanroepen. Wanneer u de batchgrootte als 1000, een doorvoer van 10.000 gebeurtenissen ongeveer 10 aanvragen gaat naar de webservice. Zelfs met 1 SU zijn er onvoldoende gelijktijdige verbindingen voor deze invoer verkeer.

Als de snelheid van invoer met 100 x wordt verhoogd, klikt u vervolgens moet de Stream Analytics-taak 1.000.000 tweets per seconde verwerken. Er zijn twee opties voor het uitvoeren van het toegenomen schaal:

1. Verhoog de batchgrootte, of
2. Partitie de invoerstroom om de gebeurtenissen parallel te verwerken

Met de eerste optie, de taak **latentie** wordt verhoogd.

Met de tweede optie moet meer su's worden ingericht en daarom genereren meer gelijktijdige aanvragen voor Machine Learning web service. Dit betekent dat de taak **kosten** wordt verhoogd.

Wordt ervan uitgegaan dat de latentie van de sentimentanalyse Machine Learning-webservice is 200 ms voor batches van 1000 gebeurtenissen of onder, 250 ms voor batches van 5000-gebeurtenissen, 300 ms voor batches van 10.000 gebeurtenissen of 500 ms voor 25.000-gebeurtenis batches.

1. Met behulp van de eerste optie (**niet** inrichting meer su's). De batchgrootte kan worden verhoogd tot **25.000**. Op zijn beurt Hierdoor kan de taak voor het verwerken van 1.000.000 gebeurtenissen met 20 gelijktijdige verbindingen met de Machine Learning-webservice (met een latentie van 500 ms per aanroep). Zo zou de extra latentie van de Stream Analytics-taak vanwege de sentiment-functie-aanvragen op basis van de Machine Learning web service-aanvragen worden verhoogd van **200 ms** naar **500 ms**. Echter, batchgrootte **kan niet** oneindig voor de Machine Learning-webservices moet de grootte van de nettolading van een aanvraag worden 4 MB of kleiner worden verhoogd webservice aanvragen time-out na 100 seconden van de bewerking.
2. Met behulp van de tweede optie, de batchgrootte aan 1000, met 200 ms latentie voor web-service is geplaatst, elke 20 gelijktijdige verbindingen met de webservice zou kunnen procesgebeurtenissen 1000 * 20 * 5 = 100.000 per seconde. De taak zou dus voor het verwerken van 1.000.000 gebeurtenissen per seconde moet 60 su's. Vergeleken met de eerste optie, zouden Stream Analytics-taak meer batch webserviceaanvragen, op zijn beurt genereren van een extra kosten.

Hieronder vindt u een tabel voor de doorvoer van de Stream Analytics-taak voor verschillende su's en -batch-grootten (in het aantal gebeurtenissen per seconde).

| Batchgrootte (ML latentie) | 500 (200 ms) | 1000 (200 ms) | 5000 (250 ms) | 10.000 (300 ms) | 25.000 (500 ms) |
| --- | --- | --- | --- | --- | --- |
| **1 SU** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **3 su 's** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **6 su 's** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **12 su 's** |5.000 |10.000 |40,000 |60,000 |100.000 |
| **18 su 's** |7.500 |15,000 |60,000 |90,000 |150,000 |
| **24 su 's** |10.000 |20,000 |80,000 |120,000 |200.000 |
| **…** |… |… |… |… |… |
| **60 su 's** |25,000 |50,000 |200.000 |300,000 |500,000 |

Nu moet u al een goed begrip van de werking van Machine Learning-functies in Stream Analytics hebt. U waarschijnlijk weten ook dat Stream Analytics-taken 'pull' gegevens van gegevensbronnen en elke 'pull' retourneert een reeks gebeurtenissen voor de Stream Analytics-taak te verwerken. Hoe deze pull-model impact de Machine Learning web-aanvragen van een service?

Normaal gesproken de grootte van de partij die we voor Machine Learning-functies instellen precies niet deelbaar is door het aantal gebeurtenissen dat is geretourneerd door elke Stream Analytics-taak 'pull'. Dit gebeurt wanneer dat de Machine Learning-webservice wordt aangeroepen met "gedeeltelijke" batches. Dit wordt gedaan om te worden er extra taakgegevens latentie overhead in samenvoegen gebeurtenissen pull pull.

## <a name="new-function-related-monitoring-metrics"></a>Nieuwe functie met betrekking tot bewaking metrische gegevens
In het gebied van de Monitor van een Stream Analytics-taak, zijn drie extra functie gerelateerde metrische gegevens toegevoegd. Dit zijn de FUNCTIEAANVRAGEN, gebeurtenissen voor de functie en MISLUKTE FUNCTIEAANVRAGEN, zoals wordt weergegeven in de onderstaande afbeelding.

![Schalen van Stream Analytics met Machine Learning-functies metrische gegevens over](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-01.png "schalen van Stream Analytics met Machine Learning-functies metrische gegevens")

De zijn als volgt gedefinieerd:

**FUNCTIEAANVRAGEN**: het aantal Functions-verzoeken.

**GEBEURTENISSEN voor de functie**: het aantal gebeurtenissen in de functieaanvragen.

**MISLUKTE FUNCTIEAANVRAGEN**: het aantal mislukte functieaanvragen.

## <a name="key-takeaways"></a>Belangrijkste Takeaways
Om samen te vatten de belangrijkste punten om te schalen van een Stream Analytics-taak met Machine Learning-functies, moeten de volgende items worden beschouwd:

1. De snelheid van invoer
2. De verdragen latentie voor de actieve Stream Analytics-taak (en dus de batchgrootte van de Machine Learning web service-aanvragen)
3. De ingerichte SUs van Stream Analytics en het aantal aanvragen van Machine Learning web service (de extra functie met betrekking tot kosten)

Er is een volledig gepartitioneerde Stream Analytics-query gebruikt als voorbeeld. Als er een complexere query is vereist de [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) is een fantastische bron voor extra hulp krijgen van de Stream Analytics-team.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Stream Analytics:

* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

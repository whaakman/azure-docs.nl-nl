---
title: Schalen van Machine Learning-functies in Azure Stream Analytics
description: In dit artikel wordt beschreven hoe u Stream Analytics-taken die gebruikmaken van Machine Learning-functies, door het configureren van partitionering en stream eenheden schalen.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: 3d478c2421066c8347622f9064c479bb8255b112
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67621754"
---
# <a name="scale-your-stream-analytics-job-with-azure-machine-learning-studio-functions"></a>Schalen van uw Stream Analytics-taak met Azure Machine Learning Studio-functies

In dit artikel wordt beschreven hoe u efficiënt schalen van Azure Stream Analytics-taken die gebruikmaken van Azure Machine Learning-functies. Zie het artikel voor meer informatie over het schalen van Stream Analytics-taken in het algemeen [taken schalen](stream-analytics-scale-jobs.md).

## <a name="what-is-an-azure-machine-learning-function-in-stream-analytics"></a>Wat is een Azure Machine Learning-functie in Stream Analytics?

Een Machine Learning-functie in Stream Analytics kan worden gebruikt als een reguliere functieaanroep in de Stream Analytics-querytaal. Achter de schermen worden deze functieaanroepen echter daadwerkelijk Azure Machine Learning-webservice-aanvragen.

U kunt de doorvoer van aanvragen voor Machine Learning web service verbeteren door meerdere rijen samen in de dezelfde web service API-aanroep "batchverwerking'. Deze groepering is een mini batch genoemd. Zie voor meer informatie, [Azure Machine Learning Studio-webservices](../machine-learning/studio/consume-web-services.md). Ondersteuning voor Azure Machine Learning Studio in Stream Analytics is in preview.

## <a name="configure-a-stream-analytics-job-with-machine-learning-functions"></a>Configureren van een Stream Analytics-taak met Machine Learning-functies

Er zijn twee parameters voor het configureren van de Machine Learning-functie die wordt gebruikt door uw Stream Analytics-taak:

* Batchgrootte van de Machine Learning-functie is aangeroepen.
* Het aantal Streaming-eenheden (su's) voor de Stream Analytics-taak is ingericht.

Om te bepalen de juiste waarden voor SUs, Bepaal of u dat wilt om latentie van de Stream Analytics-taak of de doorvoer van elke SU te optimaliseren. SUs kunnen altijd worden toegevoegd aan een taak om de doorvoer van een goed gepartitioneerde Stream Analytics-query te vergroten. Aanvullende SUs verhoogt de kosten van het uitvoeren van de taak.

De latentie bepalen *tolerantie* voor uw Stream Analytics-taak. Vergroten van de batch wordt vergroot de latentie van de aanvragen voor Azure Machine Learning-service en de latentie van de Stream Analytics-taak.

De Stream Analytics-taak voor het verwerken van het batch-vergroten kunt **meer gebeurtenissen** met de **hetzelfde aantal** van Machine Learning web serviceaanvragen. De toename van Machine Learning web service latentie is normaal gesproken sublineaire op de toename van de batchgrootte. 

Het is belangrijk rekening houden met de meest betaalbare batchgrootte voor een Machine Learning-webservice in een bepaalde situatie. De standaardgrootte van de batch voor webserviceaanvragen is 1000. Kunt u deze standaard grootte met de [REST-API voor Stream Analytics](https://docs.microsoft.com/previous-versions/azure/mt653706(v=azure.100) "REST-API voor Stream Analytics") of de [PowerShell-client voor Stream Analytics](stream-analytics-monitor-and-manage-jobs-use-powershell.md).

Als u een batchgrootte hebt bepaald, kunt u het aantal streaming-eenheden (su's), op basis van het aantal gebeurtenissen dat de functie moet instellen per seconde verwerken. Zie voor meer informatie over het streaming-eenheden, [schalen van Stream Analytics-taken](stream-analytics-scale-jobs.md).

Elke 6 SUs ophalen 20 gelijktijdige verbindingen met de Machine Learning-webservice. 1 SU-taak en 3 SU taken krijgen echter 20 gelijktijdige verbindingen.  

Als uw toepassing 200.000 gebeurtenissen per seconde genereert en de batchgrootte van de 1000 is, is de resulterende latentie van web service 200 ms. Dit tarief betekent dat elke verbinding kunt vijf aanvragen versturen naar de Machine Learning-webservice per seconde. Met 20 verbindingen kan de Stream Analytics-taak 20.000 gebeurtenissen in 200 ms en 100.000 gebeurtenissen verwerken in een tweede.

Voor het verwerken van 200.000 gebeurtenissen per seconde, moet de Stream Analytics-taak 40 gelijktijdige verbindingen, die afkomstig uit 12 SUS zijn. Het volgende diagram illustreert de aanvragen van de Stream Analytics-taak voor het Machine Learning web service-eindpunt – elke 6 SUs is 20 gelijktijdige verbindingen met Machine Learning-webservice op max.

![Schalen van Stream Analytics met Machine Learning-functies twee taak voorbeeld](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-00.png "schaal Stream Analytics met Machine Learning-functies twee taak voorbeeld")

In het algemeen ***B*** voor batchgrootte, ***L*** voor de web service latentieperiode batchgrootte B in milliseconden, de doorvoer van een Stream Analytics job met ***N*** SUs is:

![Schalen van Stream Analytics met Machine Learning-functies formule](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-02.png "schalen van Stream Analytics met Machine Learning-functies formule")

U kunt ook de 'max gelijktijdige aanroepen' configureren op de Machine Learning-webservice. Het is raadzaam om in te stellen van deze parameter aan de maximumwaarde (momenteel 200).

Raadpleeg voor meer informatie over deze instelling de [artikel schalen voor Machine Learning-webservices](../machine-learning/studio/scaling-webservice.md).

## <a name="example--sentiment-analysis"></a>Voorbeeld: Sentimentanalyse
Het volgende voorbeeld bevat een Stream Analytics-taak met de sentimentanalyse Machine Learning-functie, zoals beschreven in de [zelfstudie voor integratie van Stream Analytics, Machine Learning](stream-analytics-machine-learning-integration-tutorial.md).

De query is een eenvoudige gepartitioneerd volledig query gevolgd door de **sentiment** functioneren, zoals wordt weergegeven in het volgende voorbeeld:

```SQL
    WITH subquery AS (
        SELECT text, sentiment(text) as result from input
    )

    Select text, result.[Score]
    Into output
    From subquery
```

We gaan de configuratie die nodig zijn om te maken van een Stream Analytics-taak, welke sentimentanalyse kiest, wordt er van tweets met een snelheid van 10.000 tweets per seconde.

1 SU gebruikt, kan deze Stream Analytics-taak verwerkt het verkeer? De taak kunt houden met de invoer met de standaardgrootte van de batch van 1000. De standaard-latentie van de sentimentanalyse Machine Learning-webservice (met een standaard-batchgrootte van 1000) wordt niet meer dan een seconde latentie gemaakt.

De Stream Analytics-taak **algehele** of end-to-end latentie meestal een paar seconden. In deze Stream Analytics-taak, een meer gedetailleerde Kijk *vooral* de Machine Learning-functieaanroepen. Met de batchgrootte van een van 1000 gaat een doorvoer van 10.000 gebeurtenissen ongeveer 10 aanvragen naar de webservice. Zelfs met één SU zijn er onvoldoende gelijktijdige verbindingen voor deze invoer verkeer.

Als de snelheid van invoer met 100 x wordt verhoogd, klikt u vervolgens moet de Stream Analytics-taak 1.000.000 tweets per seconde verwerken. Er zijn twee opties voor het uitvoeren van het toegenomen schaal:

1. Verhoog de batchgrootte.
2. Partitie de invoerstroom om de gebeurtenissen parallel te verwerken.

Met de eerste optie, de taak **latentie** wordt verhoogd.

Met de tweede optie moet u meer su's als u meer gelijktijdige aanvragen voor Machine Learning web service wilt inrichten. Deze groter aantal SUs, verhoogt u de taak **kosten**.

We bekijken de schalen met behulp van de volgende replicatielatentie voor de batchgrootte van elke:

| Latentie | Batchgrootte |
| --- | --- |
| 200 ms | batches van 1000 gebeurtenissen of lager |
| 250 ms | batches van 5000-gebeurtenissen |
| 300 ms | batches van 10.000 gebeurtenissen |
| 500 ms | 25.000-gebeurtenis batches |

1. Met behulp van de eerste optie (**niet** inrichting meer su's). De batchgrootte kan worden verhoogd tot **25.000**. Het vergroten van batch op deze manier kunt de taak voor het verwerken van 1.000.000 gebeurtenissen met 20 gelijktijdige verbindingen met de Machine Learning-webservice (met een latentie van 500 ms per aanroep). Zo zou de extra latentie van de Stream Analytics-taak vanwege de sentiment-functie-aanvragen op basis van de Machine Learning web service-aanvragen worden verhoogd van **200 ms** naar **500 ms**. Echter, batchgrootte **kan geen** oneindig voor de Machine Learning-webservices moet de grootte van de nettolading van een aanvraag worden 4 MB of kleiner worden verhoogd en op het web service aanvragen time-out na 100 seconden van de bewerking.
1. Met behulp van de tweede optie, de batchgrootte aan 1000, met 200 ms latentie voor web-service is geplaatst, elke 20 gelijktijdige verbindingen met de webservice zou kunnen procesgebeurtenissen 1000 * 20 * 5 = 100.000 per seconde. De taak zou dus voor het verwerken van 1.000.000 gebeurtenissen per seconde moet 60 su's. Vergeleken met de eerste optie, zouden Stream Analytics-taak meer batch webserviceaanvragen, op zijn beurt genereren van een extra kosten.

Hieronder vindt u een tabel voor de doorvoer van de Stream Analytics-taak voor verschillende su's en -batch-grootten (in het aantal gebeurtenissen per seconde).

| Batchgrootte (ML latentie) | 500 (200 ms) | 1000 (200 ms) | 5000 (250 ms) | 10.000 (300 ms) | 25.000 (500 ms) |
| --- | --- | --- | --- | --- | --- |
| **1 SU** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **3 su 's** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **6 su 's** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **12 su 's** |5,000 |10.000 |40,000 |60,000 |100,000 |
| **18 su 's** |7,500 |15,000 |60,000 |90,000 |150,000 |
| **24 su 's** |10.000 |20,000 |80,000 |120,000 |200.000 |
| **…** |… |… |… |… |… |
| **60 su 's** |25,000 |50,000 |200.000 |300,000 |500,000 |

Nu moet u al een goed begrip van de werking van Machine Learning-functies in Stream Analytics hebt. U waarschijnlijk weten ook dat Stream Analytics-taken 'pull' gegevens van gegevensbronnen en elke 'pull' retourneert een reeks gebeurtenissen voor de Stream Analytics-taak te verwerken. Hoe deze pull-model impact de Machine Learning web-aanvragen van een service?

Normaal gesproken de grootte van de partij die we voor Machine Learning-functies instellen precies niet deelbaar is door het aantal gebeurtenissen dat is geretourneerd door elke Stream Analytics-taak 'pull'. Wanneer dit het geval is, wordt de Machine Learning-webservice wordt aangeroepen met "gedeeltelijke" batches. Met behulp van gedeeltelijke batches voorkomt waarbij extra kosten voor extra taakgegevens latentie overhead in samenvoegen gebeurtenissen pull pull.

## <a name="new-function-related-monitoring-metrics"></a>Nieuwe functie met betrekking tot bewaking metrische gegevens
In het gebied van de Monitor van een Stream Analytics-taak, zijn drie extra functie gerelateerde metrische gegevens toegevoegd. Ze zijn **FUNCTIEAANVRAGEN**, **gebeurtenissen voor de functie** en **MISLUKTE FUNCTIEAANVRAGEN**, zoals wordt weergegeven in de onderstaande afbeelding.

![Schalen van Stream Analytics met Machine Learning-functies metrische gegevens over](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-01.png "schalen van Stream Analytics met Machine Learning-functies metrische gegevens")

De zijn als volgt gedefinieerd:

**FUNCTIEAANVRAGEN**: Het aantal Functions-verzoeken.

**GEBEURTENISSEN VOOR DE FUNCTIE**: Het aantal gebeurtenissen in de functieaanvragen.

**MISLUKTE FUNCTIEAANVRAGEN**: Het aantal mislukte functieaanvragen.

## <a name="key-takeaways"></a>Belangrijkste Takeaways

Als u wilt schalen een Stream Analytics-taak met Machine Learning-functies, houd rekening met de volgende factoren:

1. De snelheid van de invoer.
2. De verdragen latentie voor de actieve Stream Analytics-taak (en dus de batchgrootte van de Machine Learning web service-aanvragen).
3. De ingerichte SUs van Stream Analytics en het aantal aanvragen van Machine Learning web service (de extra functie met betrekking tot kosten).

Er is een volledig gepartitioneerde Stream Analytics-query gebruikt als voorbeeld. Als een complexere query is vereist, de [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) is een fantastische bron voor extra hulp krijgen van de Stream Analytics-team.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Stream Analytics:

* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

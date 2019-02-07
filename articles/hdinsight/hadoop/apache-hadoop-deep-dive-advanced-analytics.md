---
title: Deep dive - advanced analytics - Azure HDInsight
description: Informatie over hoe u met geavanceerde analyse worden algoritmen gebruikt voor het verwerken van big data.
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/14/2017
ms.author: ashishth
ms.openlocfilehash: 0e17fa877bc2e1e4f72891315d940546a11e674d
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55812492"
---
# <a name="deep-dive---advanced-analytics"></a>Deep dive - geavanceerde analyses

## <a name="what-is-advanced-analytics-for-hdinsight"></a>Wat is analytics geavanceerde voor HDInsight?

HDInsight biedt de mogelijkheid om waardevolle inzichten van grote hoeveelheden gestructureerde, ongestructureerde en ontdekt gegevens te verkrijgen. Geavanceerde analyses, is het gebruik van zeer schaalbare architecturen, statistische en machine learning-modellen en intelligente dashboards om te voorzien van bruikbare inzichten. Machine learning, of *Voorspellend*, gebruikt algoritmen die identificeren en leer van relaties in uw gegevens voor voorspellingen en uw beslissingen voor de handleiding.

## <a name="advanced-analytics-process"></a>Geavanceerde analyse-proces

![Verwerken](./media/apache-hadoop-deep-dive-advanced-analytics/process.png)

Nadat u het zakelijke probleem hebt geïdentificeerd en zijn begonnen met het verzamelen en verwerken van uw gegevens, die u wilt maken van een model dat staat voor de vraag die u wilt voorspellen. Uw model gebruikt een of meer machine learning-algoritmen voor het maken van het type voorspelling die het beste past bij de behoeften van uw bedrijf.  Het merendeel van uw gegevens moet worden gebruikt om uw model te trainen met de rest gebruikt om te testen of evalueren ervan. 

Nadat u maken, laden, testen en evalueren van uw model, wordt de volgende stap is het uw model implementeert zodat deze begint met het leveren van antwoorden op uw vragen. De laatste stap is om te controleren van de prestaties van uw model en af te stemmen indien nodig. 

## <a name="common-types-of-algorithms"></a>Algemene typen algoritmen

Geavanceerde analyse-oplossingen bieden een set van machine learning-algoritmen. Hier volgt een overzicht van de categorieën van algoritmen en de bijbehorende algemene zakelijke gebruiksvoorbeelden.

![Machine Learning-Use-Cases](./media/apache-hadoop-deep-dive-advanced-analytics/ml-use-cases.png)

Samen met de best passende verifiëringsalgoritme (s) selecteren, moet u overwegen of u moet gegevens opgeven voor training. Machine learning-algoritmen zijn als volgt onderverdeeld:

* Onder supervisie - moet algoritme worden getraind op een set met gelabelde gegevens voordat deze resultaten kan opgeven
* Semi onder supervisie - algoritme kan worden uitgebreid door extra doelen via interactieve query door een trainer, die niet beschikbaar tijdens de eerste fase van de training waren
* Niet onder supervisie - is algoritme niet vereist Traininggegevens 
* Leren met bekrachtigingen beschikt - algoritme maakt gebruik van softwareagents om te bepalen het gedrag van de ideale binnen een specifieke context (vaak gebruikt in robotiek)


| Algoritme voor categorie| Gebruiken | Learning-Type | Algoritmen |
| --- | --- | --- | -- |
| Classificatie | Personen of dingen classificeren in groepen | Onder supervisie | Beslissingsstructuren, logistieke regressie, neurale netwerken |
| Clustering | Delen van een reeks voorbeelden in homogene groepen | Niet onder supervisie | K-means clustering |
| Detectie van het patroon | Frequente koppelingen in de gegevens identificeren | Niet onder supervisie | Koppelingsregels voor |
| Regressie | Numerieke resultaten voorspellen | Onder supervisie | Lineaire regressie, neurale netwerken |
| Leren met bekrachtigingen beschikt | Optimale gedrag voor robots bepalen | Leren met bekrachtigingen beschikt | Monte Carlo-simulaties, DeepMind |

## <a name="machine-learning-on-hdinsight"></a>Machine learning in HDInsight

HDInsight heeft meerdere machine learning-opties voor een geavanceerde analysewerkstroom:

* Machine Learning en Apache Spark
* R- en ML-Services
* Azure Machine Learning en Apache Hive
* Apache Spark- en Deep learning

### <a name="machine-learning-and-apache-spark"></a>Machine Learning en Apache Spark


[HDInsight Spark](../spark/apache-spark-overview.md) is een aanbieding wordt gehost op Azure van [Apache Spark](https://spark.apache.org/), een uniforme, open source, een framework voor parallelle gegevensverwerking die gebruikmaakt van in-memory verwerking om analyse van Big Data te verbeteren. De Spark-verwerkingsengine is gebouwd voor snelheid, gebruiksgemak, en geavanceerde analyses. Gedistribueerde berekening in-memory-mogelijkheden van Spark kunnen u een goede keuze voor de zich herhalende algoritmen in machine learning- en grafiekberekeningen gebruikt. 


Er zijn drie schaalbare machine learning-bibliotheken die algoritmische modellering naar deze gedistribueerde omgeving meenemen:

* [**MLlib** ](https://spark.apache.org/docs/latest/ml-guide.html) -MLlib de oorspronkelijke API gebouwd boven op Spark rdd's bevat.
* [**SparkML** ](https://spark.apache.org/docs/1.2.2/ml-guide.html) -SparkML is een nieuwere pakket dat een hoger niveau API gebouwd boven op Spark DataFrames biedt voor het maken van ML-pijplijnen.
* [**MMLSpark** ](https://github.com/Azure/mmlspark) - Microsoft Machine Learning-bibliotheek voor Apache Spark (MMLSpark) is zodanig ontworpen dat gegevenswetenschappers productiever op Spark, te verhogen de snelheid van experimenten en gebruikmaken van geavanceerde machine learning technieken, met inbegrip van deep learning, op zeer grote gegevenssets. De MMLSpark-bibliotheek vereenvoudigt algemene modellering van taken voor het ontwikkelen van modellen in PySpark. 

### <a name="r-and-ml-services"></a>R- en ML-Services

Als onderdeel van HDInsight, kunt u een HDInsight-cluster met [ML-Services](../r-server/r-server-overview.md) klaar om te worden gebruikt met grote gegevenssets en -modellen. Deze nieuwe mogelijkheid biedt gegevenswetenschappers en statistici met een bekend R-interface die kunnen worden geschaald op aanvraag via HDInsight, zonder de overhead van het cluster instellen en beheren.

### <a name="azure-machine-learning-and-apache-hive"></a>Azure Machine Learning en Apache Hive

[Azure Machine Learning Studio](https://studio.azureml.net/) biedt hulpprogramma's waarmee Voorspellend model, evenals een volledig beheerde service die u kunt gebruiken voor het implementeren van uw voorspellende modellen als webservices gereed om te gebruiken. Azure Machine Learning bevat hulpprogramma's voor het maken van volledige predictive analytics-oplossingen in de cloud snel maken, testen, operationeel maken en beheren van voorspellende modellen. Selecteer in een grote bibliotheek met algoritmen, een webgebaseerde studio gebruiken voor het ontwikkelen van modellen en eenvoudig uw modellen vervolgens als een webservice implementeren.

### <a name="apache-spark-and-deep-learning"></a>Apache Spark- en Deep learning

[Deep learning](https://www.microsoft.com/research/group/dltc/) wordt er een vertakking van machine learning die gebruikmaakt van *deep neural networks* (dnn's), laat u inspireren door de biologische processen van het menselijk brein moeite heeft. Deep learning zien veel onderzoekers als een veelbelovende benadering voor kunstmatige intelligentie. Enkele voorbeelden van diepteleren zijn gesproken taal vertalers, image recognition systemen en machine redenering. Ga een eigen werk in deep learning, zodat Microsoft de gratis, eenvoudig te gebruiken, open-source heeft ontwikkeld [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/). De toolkit wordt intensief gebruikt door een groot aantal Microsoft-producten, bedrijven over de hele wereld met een hoeft te implementeren van deep learning op schaal, en door studenten die geïnteresseerd zijn in de meest recente algoritmen en technieken. 

## <a name="scenario---score-images-to-identify-patterns-in-urban-development"></a>Scenario - Score installatiekopieën om patronen in stedelijke ontwikkeling te identificeren

Laten we bekijken een voorbeeld van een geavanceerde analyse-machine learning-pijplijn met HDInsight.

In dit scenario ziet u hoe dnn's gemaakt in een deep learning-frameworks, Microsoft Cognitive Toolkit (CNTK), van kunt operationaliseren voor het scoren van grote afbeelding verzamelingen die zijn opgeslagen in een Azure Blob Storage-account met behulp van de PySpark op een HDInsight Spark-cluster. Deze methode wordt toegepast op een algemene DNN use case, afbeeldingsclassificatie, en kan worden gebruikt om recente patronen in stedelijke ontwikkeling te identificeren.  U gebruikt een classificeringsmodel vooraf getrainde afbeeldingen. Het model is vooraf getrainde op de [CIFAR-10-gegevensset](https://www.cs.toronto.edu/~kriz/cifar.html) en tot 10.000 ingehouden installatiekopieën is toegepast.

Er zijn drie belangrijke taken in dit scenario voor geavanceerde analyse:

1. Maak een Azure HDInsight Hadoop-cluster met een Apache Spark 2.1.0-distributie. 
2. Een aangepast script voor het installeren van Microsoft Cognitive Toolkit op alle knooppunten van een Azure HDInsight Spark-cluster worden uitgevoerd. 
3. Een vooraf gemaakte Jupyter-notebook uploaden naar uw HDInsight Spark-cluster om toe te passen een getrainde Microsoft Cognitive Toolkit deep learning-model tot bestanden in een Azure Blob Storage-Account met behulp van de Spark-Python-API (PySpark). 

Dit voorbeeld wordt de installatiekopie van het CIFAR-10 gecompileerde en gedistribueerd door Alex Krizhevsky, Vinod Nair en Geoffrey Hinton instellen. De CIFAR-10-gegevensset bevat 60.000 32 x 32 kleur installatiekopieën die behoren tot 10 sluiten elkaar wederzijds uit klassen:

![Installatiekopieën](./media/apache-hadoop-deep-dive-advanced-analytics/ml-images.png)

Zie voor meer informatie over de gegevensset van Alex Krizhevsky [Learning meerdere lagen van de functies van kleine afbeeldingen](https://www.cs.toronto.edu/~kriz/learning-features-2009-TR.pdf).

De gegevensset is in een trainingsset van 50.000 installatiekopieën en een test reeks 10.000 afbeeldingen gepartitioneerd. De eerste set is gebruikt voor het trainen van een 20-laag-deep convolutional resterende netwerk (ResNet)-model met behulp van Microsoft Cognitive Toolkit door [in deze zelfstudie](https://github.com/Microsoft/CNTK/tree/master/Examples/Image/Classification/ResNet) van de Cognitive Toolkit-GitHub-opslagplaats. De resterende 10.000 afbeeldingen zijn gebruikt voor het testen van de nauwkeurigheid van het model. Dit is waar gedistribueerde gegevensverwerking geleverd aan de orde: de taak van het vooraf verwerken en de afbeeldingen scoren is maximaal worden opgestart. Met de opgeslagen getrainde model in voorraad, we hebben gebruikt:

* PySpark om de afbeeldingen en het getrainde model op de worker-knooppunten van het cluster te distribueren.
* Python voor het vooraf verwerken van de afbeeldingen op elk knooppunt van het HDInsight Spark-cluster.
* Cognitive Toolkit laden van het model en score de vooraf verwerkte afbeeldingen op elk knooppunt.
* Jupyter-Notebooks om uit te voeren van de PySpark-script, samenvoegen van de resultaten en gebruik [Matplotlib](https://matplotlib.org/) voor het visualiseren van de modelprestaties.

Voor het hele voorverwerking/scoren van 10.000 afbeeldingen, duurt minder dan één minuut op een cluster met 4 worker-knooppunten. Het model voorspelt nauwkeurig de labels van installatiekopieën van ~ 9,100 (91%). Een verwarringsmatrix ziet u de meest voorkomende classificatiefouten. De matrix ziet u bijvoorbeeld dat honden als katten en vice versa etikettering optreedt meer vaak dan voor andere paren met een label.

![Resultaten](./media/apache-hadoop-deep-dive-advanced-analytics/ml-results.png)

### <a name="try-it-out"></a>Probeer het nu!

Ga als volgt [in deze zelfstudie](../spark/apache-spark-microsoft-cognitive-toolkit.md) voor het implementeren van deze oplossing voor end-to-end: een HDInsight Spark-cluster instellen, Cognitive Toolkit installeren en uitvoeren van de Jupyter-Notebook die 10.000 CIFAR afbeeldingen beoordeelt.

## <a name="next-steps"></a>Volgende stappen

Apache Hive en Azure Machine Learning

* [Apache Hive en Azure Machine Learning end-to-end](../../machine-learning/team-data-science-process/hive-walkthrough.md)
* [Met behulp van een Azure HDInsight Hadoop-Cluster op een gegevensset van 1 TB](../../machine-learning/team-data-science-process/hive-criteo-walkthrough.md)

Apache Spark and MLLib

* [Machine learning met Apache Spark in HDInsight](../../machine-learning/team-data-science-process/spark-overview.md)
* [Apache Spark met Machine Learning: Apache Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](../spark/apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark met Machine Learning: Apache Spark in HDInsight gebruiken voor de resultaten van voedingsinspectie voorspellen](../spark/apache-spark-machine-learning-mllib-ipython.md)

Deep Learning, Cognitive Toolkit en anderen

* [Perfect parallel afbeeldingsclassificatie, met behulp van Cognitive Toolkit en TensorFlow op Azure HDInsight Spark](https://blogs.technet.microsoft.com/machinelearning/2017/04/12/embarrassingly-parallel-image-classification-using-cognitive-toolkit-tensorflow-on-azure-hdinsight-spark/)
* [Data Science virtuele Machine van Azure](../../machine-learning/data-science-virtual-machine/overview.md)
* [Maak kennis met H2O.ai op Azure HDInsight](https://azure.microsoft.com/blog/introducing-h2o-ai-with-on-azure-hdinsight-to-bring-the-most-robust-ai-platform-for-enterprises/)

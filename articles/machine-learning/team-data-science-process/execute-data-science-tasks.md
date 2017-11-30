---
title: Uitvoeren van de gegevens wetenschappelijke taken - Azure Machine Learning | Microsoft Docs
description: Hoe een wetenschappelijk gegevens een wetenschappelijke gegevensproject kunt uitvoeren in een herleidbare versie beheerd en gezamenlijke manier.
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: bradsev;
ms.openlocfilehash: 1ad4e8c117f93f2f085c01fae2a5ab38cdd10d2f
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2017
---
# <a name="execute-data-science-tasks-exploration-modeling-and-deployment"></a>Gegevens wetenschappelijke taken uitvoeren: exploratie modelleren en implementatie

Typische gegevens wetenschappelijke taken omvatten gegevensverkenning, modellering en implementatie. Dit artikel laat zien hoe u de **interactieve Gegevensverkenning, analyse en rapportage (IDEAR)** en **geautomatiseerde modelleren en rapportage (AMAR)** hulpprogramma's voor verschillende algemene gegevens wetenschappelijke taken voltooien zoals interactieve gegevensverkenning, data-analyse, rapportage en maken van het model. Dit wordt ook uitgelegd welke opties voor het implementeren van een model in een productieomgeving verschillende platformen, toolkits en gegevens, zoals het volgende:

- [Azure Machine Learning](../preview/index.yml)
- [SQL-Server met ML-services](https://docs.microsoft.com/sql/advanced-analytics/r/r-services#in-database-analytics-with-sql-server)
- [Microsoft Machine Learning-Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server)


## 1. <a name='DataQualityReportUtility-1'></a>Exploratie 

Een wetenschappelijk gegevens verkennen en rapportage in tal van manieren uitvoeren: met behulp van bibliotheken en pakketten beschikbaar voor Python (matplotlib bijvoorbeeld) of met R (ggplot of andere bijvoorbeeld). Gegevenswetenschappers kunnen deze code aanpassen aan de behoeften van gegevensverkenning voor specifieke scenario's aanpassen. De behoeften voor het omgaan met gestructureerde gegevens zijn verschillend voor ongestructureerde gegevens, zoals tekst of afbeeldingen. 

Producten zoals Azure Machine Learning Workbench bieden ook [gegevens voorbereiden geavanceerde](../preview/tutorial-bikeshare-dataprep.md) voor gegevens worsteling en verkennen, met inbegrip van het maken van de functie. De gebruiker, beslissen over de hulpprogramma's, bibliotheken en pakketten die het meest suite hun behoeften. 

Het product aan het einde van deze fase wordt een rapport van gegevens te verkennen. Het rapport leveren een redelijk uitgebreid overzicht van de gegevens moet worden gebruikt voor het modelleren en een beoordeling van of de gegevens zijn geschikt om door te gaan met de stap modellering. Het Team gegevens wetenschap proces (TDSP) hulpprogramma's beschreven in de volgende secties voor semi geautomatiseerde exploratie modelleren en rapportage bieden ook gestandaardiseerde gegevensverkenning en rapporten te modelleren. 

### <a name="interactive-data-exploration-analysis-and-reporting-using-the-idear-utility"></a>Interactieve gegevensverkenning, analyse en rapportage met het hulpprogramma IDEAR

Deze R op basis van markdown of Python-notebook gebaseerde hulpprogramma voorziet in een flexibele en interactieve hulpprogramma om te evalueren en gegevenssets te verkennen. Gebruikers kunnen snel rapporten genereren van de gegevensset met minimale coderen. Gebruikers kunnen klikken op knoppen waarmee u kunt de exploratie resultaten in het hulpprogramma voor interactieve exporteren naar een definitieve rapport die kan worden geleverd aan clients of gebruikt voor het nemen van beslissingen over welke variabelen moeten worden opgenomen in de volgende modellering stap.

Op dit moment werkt het hulpprogramma alleen op gegevensframes in het geheugen. Een YAML-bestand is nodig om op te geven van de parameters van de gegevensset worden onderzocht. Zie voor meer informatie [IDEAR in hulpprogramma's TDSP gegevens wetenschappelijke](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/DataReport-Utils).


## 2. <a name='ModelingUtility-2'></a>Modeling

Er zijn talrijke toolkits en pakketten voor training modellen in verschillende talen. Gegevenswetenschappers moeten gerust gebruik die uitgevoerde die ze vertrouwd met, bent zolang de prestatie-overwegingen met betrekking tot de nauwkeurigheid en latentie is voldaan voor het relevante bedrijf use cases en productiescenario's.

De volgende sectie wordt beschreven hoe een TDSP op basis van het R-hulpprogramma gebruiken voor semi geautomatiseerde modelleren. Dit hulpprogramma AMAR kan basislijn modellen snel genereren evenals de parameters die worden afgestemd moeten om een beter uitvoeren model worden gebruikt.
De volgende sectie van de model-management laat zien hoe een systeem voor het registreren en beheren van meerdere modellen.


### <a name="model-training-modeling-and-reporting-using-the-amar-utility"></a>Model voor training: modelleren en rapportage met het hulpprogramma AMAR

De [geautomatiseerde modelleren en rapportage (AMAR) hulpprogramma](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/Modeling) voorziet in een aanpasbare, semi geautomatiseerde hulpprogramma model maken met hyper-parameter sweeping en de nauwkeurigheid van deze modellen vergelijken. 

Het model maken van het hulpprogramma is een R Markdown-bestand dat kan worden uitgevoerd om ingesloten HTML-uitvoer met een tabel met inhoud voor navigatie door de verschillende secties produceren. Drie algoritmen worden uitgevoerd wanneer de Markdown-bestand (knit) wordt uitgevoerd: overgegaan regressie met behulp van de glmnet inpakken, willekeurige forest met behulp van het pakket randomForest en versterking van structuren met het pakket xgboost). Elk van deze algoritmen produceert een getraind model. De nauwkeurigheid van deze modellen wordt vervolgens vergeleken en de functie voor relatieve belang waarnemingspunten worden gerapporteerd. Er zijn momenteel twee hulpprogramma's: een voor een classificatietaak binaire en één voor een taak regressie. De belangrijkste verschillen tussen deze twee parameters voor de manier is en nauwkeurigheid metrische gegevens voor deze taken learning zijn opgegeven. 

Een YAML-bestand wordt gebruikt om op te geven:

- de gegevensinvoer (een SQL-bron of een R-bestand) 
- welk gedeelte van de gegevens wordt gebruikt voor training en welk gedeelte voor het testen
- welke algoritmen om uit te voeren 
- de keuze van besturingsparameters voor optimalisatie model:
    - kruisvalidatie 
    - uitvoeren van de bootstrap
    - vouwen van kruisvalidatie
- de hyper-parameter wordt ingesteld voor elk algoritme. 

Het nummer van algoritmen, het aantal vouwen voor optimalisatie, hyper-parameters en het aantal hyper-parametersets getrokken via kunnen ook worden gewijzigd in het bestand Yaml worden de modellen snel uitgevoerd. Ze kunnen bijvoorbeeld worden uitgevoerd met een kleiner aantal vouwen dat k/l, een kleiner aantal parametersets. Als gerechtvaardigd is, kunnen ze ook worden uitgevoerd meer uitvoerig met een hoger aantal vouwen dat k/l of een groter aantal parametersets.

Zie voor meer informatie [geautomatiseerde modelleren hulpprogramma's en rapportage in hulpprogramma's TDSP gegevens wetenschappelijke](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/Modeling).

### <a name="model-management"></a>Modelbeheer
Nadat u meerdere modellen zijn gebouwd, moet u meestal een systeem voor het registreren en beheren van de modellen. Normaal gesproken moet u een combinatie van scripts of API's en een back-end-database of versiebeheer-systeem. Er zijn een aantal opties die u voor deze beheertaken overwegen kunt:

1. [Azure Machine Learning - model-management-service](../preview/index.yml)
2. [ModelDB van MIT](https://mitdbg.github.io/modeldb/) 
3. [SQL-seerver als een model-management-systeem](https://blogs.technet.microsoft.com/dataplatforminsider/2016/10/17/sql-server-as-a-machine-learning-model-management-system/)
4. [Microsoft Machine Learning-Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)

## 3. <a name='Deployment-3'></a>Implementatie

Productie-implementatie kunt een model voor een actieve rol spelen in een onderneming. Voorspellingen op basis van een geïmplementeerde model kunnen worden gebruikt voor zakelijke beslissingen te nemen.

### <a name="production-platforms"></a>Productieplatforms
Er zijn verschillende benaderingen en platforms modellen in productie te plaatsen. Hier volgen een aantal opties:


- [Implementatie van het model in Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/preview/model-management-overview)
- [Implementatie van een model in SQL-server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Microsoft Machine Learning-Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)

>
>
>Opmerking: Vóór de implementatie een heeft om ervoor te zorgen dat de latentie van het model score berekenen is laag genoeg is voor gebruik in productie.
>

Meer voorbeelden zijn beschikbaar in scenario's die de stappen in het proces voor het demonstreren **specifieke scenario's**. Ze worden weergegeven en gekoppeld aan de miniatuur beschrijvingen in de [voorbeeld-scenario's](walkthroughs.md) artikel. Ze te laten zien hoe cloud, 's voor on-premises en -services combineren in een werkstroom of pijplijn een intelligente toepassing maken.

Opmerking: Voor de implementatie met behulp van Azure Machine Learning Studio, Zie [Azure Machine Learning-webservice implementeren](../studio/publish-a-machine-learning-web-service.md).

### <a name="ab-testing"></a>A / B-tests
Wanneer meerdere modellen in productie, dit kan nuttig zijn om uit te voeren [A / B-tests](https://en.wikipedia.org/wiki/A/B_testing) vergelijken de prestaties van de modellen. 

 
## <a name="next-steps"></a>Volgende stappen

[Voortgang van de gegevens wetenschappelijke projecten volgen](track-progress.md) ziet u hoe een wetenschappelijk gegevens de voortgang van een wetenschappelijke gegevensproject kunt bijhouden.
 



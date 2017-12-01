---
title: Interpreteren model resulteert in een Machine Learning | Microsoft Docs
description: Het kiezen van de optimale parameter ingesteld voor het gebruik van een algoritme en visualiseren score-model levert.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 6230e5ab-a5c0-4c21-a061-47675ba3342c
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: bradsev;garye
ms.openlocfilehash: d6563d411e9f159399f9863a5b572365dc2b05cc
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="interpret-model-results-in-azure-machine-learning"></a>Model resulteert in een Azure Machine Learning interpreteren
In dit onderwerp wordt uitgelegd hoe visualiseren en interpreteren voorspelling resulteert in een Azure Machine Learning Studio. Nadat u hebt een model wordt getraind en gedaan voorspellingen toe ('berekend het model'), moet u om te begrijpen en het resultaat van de voorspelling te interpreteren.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

Er zijn vier belangrijke typen van machine learning-modellen in Azure Machine Learning:

* Classificatie
* Clustering
* Regressie
* Recommender systemen

De modules die wordt gebruikt voor de prognose boven op deze modellen zijn:

* [Score Model] [ score-model] -module voor de classificatie en regressie
* [Toewijzen aan Clusters] [ assign-to-clusters] -module voor clustering
* [Score Matchbox Recommender] [ score-matchbox-recommender] voor systemen die aanbeveling

Dit document wordt uitgelegd hoe resultaten moet interpreteren voorspelling voor elk van deze modules. Zie voor een overzicht van deze modules [het kiezen van de parameters voor het optimaliseren van uw algoritmen in Azure Machine Learning](algorithm-parameters-optimize.md).

Dit onderwerp bevat voorspelling interpretatie maar niet de evaluatie van het model. Zie voor meer informatie over het evalueren van uw model [het evalueren van de prestaties van het model in Azure Machine Learning](evaluate-model-performance.md).

Als u niet bekend met Azure Machine Learning bent en hulp bij het maken van een eenvoudig experiment aan de slag, raadpleeg dan [een eenvoudig experiment maken in Azure Machine Learning Studio](create-experiment.md) in Azure Machine Learning Studio.

## <a name="classification"></a>Classificatie
Er zijn twee subcategorieën van classificatie problemen:

* Problemen met slechts twee klassen (tweeklasse of binaire classificatie)
* Problemen met meer dan twee klassen (meerdere klasse-indeling)

Azure Machine Learning heeft verschillende modules omgaan met elk van deze typen van classificatie, maar de methoden voor het interpreteren van de resultaten van hun voorspelling vergelijkbaar zijn.

### <a name="two-class-classification"></a>Classificatie van de twee-klasse
**Voorbeeld experiment**

Een voorbeeld van een tweeklasse klassificatieprobleem is de classificatie van iris bloemen. De taak is iris bloemen op basis van hun functies classificeren. De gegevensset Iris is opgegeven in Azure Machine Learning is een subset van de populaire [Iris gegevensset](http://en.wikipedia.org/wiki/Iris_flower_data_set) exemplaren van die alleen de twee bloem soorten (klassen 0 en 1). Er zijn vier functies voor elke bloem (sepal breedte sepal lengte Bloemblad lengte en Bloemblad breedte).

![Schermopname van iris experiment](./media/interpret-model-results/1.png)

Afbeelding 1. IRIS tweeklasse classificatie probleem experiment

Een experiment is uitgevoerd om dit probleem te verhelpen zoals in afbeelding 1. Een tweeklasse gestimuleerd besluit structuur model is getraind en berekend. Nu kunt u de resultaten van de voorspelling van visualiseren de [Score Model] [ score-model] module door te klikken op de uitvoerpoort van de [Score Model] [ score-model] module en vervolgens te klikken op **Visualize**.

![De module score model](./media/interpret-model-results/1_1.png)

Hiermee wordt de resultaten van score berekenen zoals in afbeelding 2.

![Resultaten van iris tweeklasse classificatie experiment](./media/interpret-model-results/2.png)

Afbeelding 2. Een resultaat score-model in twee klasse classificatie visualiseren

**Resultaat interpretatie**

Er zijn zes kolommen in de tabel met resultaten. Vier kolommen aan de linkerkant vindt u de vier functies. Rechts twee kolommen Scored Labels en kansen berekend, zijn de resultaten van de voorspelling. De kolom berekend kansen toont de kans dat een bloem behoort tot het positieve class (klasse 1). Bijvoorbeeld, is het eerste nummer in de kolom (0.028571) betekent dat er 0.028571 kans dat de eerste bloem behoort tot klasse 1. De kolom Scored Labels bevat de voorspelde klasse voor elke bloem. Dit is gebaseerd op de kolom berekend kansen. Als de kans op een bloem scored groter dan 0,5 is, wordt het voorspeld als klasse 1. Anders wordt deze als klasse 0 voorspeld.

**Publicatie van de Web service**

Nadat de resultaten van de voorspelling zijn begrepen en geluid beoordeeld, kan het experiment worden gepubliceerd als een webservice zodat u kunt deze in verschillende toepassingen implementeren en deze voor voorspellingen op alle nieuwe iris bloem klasse aanroepen. Zie voor meer informatie over het wijzigen van een trainingsexperiment in een score experiment en publiceren als een webservice, [publiceren van de webservice Azure Machine Learning](walkthrough-5-publish-web-service.md). Deze procedure biedt u een experiment scoreprofiel zoals in afbeelding 3.

![Schermopname van het experiment score berekenen](./media/interpret-model-results/3.png)

Afbeelding 3. Score berekenen voor het experiment iris tweeklasse classificatie probleem

Nu moet u voor de invoer en uitvoer voor de webservice. De invoer is de rechteruitvoerpoort van [Score Model][score-model], dit is de bloem Iris invoer functies. De keuze van de uitvoer is afhankelijk van of u geïnteresseerd in de voorspelde klasse (scored label), de kans op scored of beide bent. In dit voorbeeld wordt ervan uitgegaan dat u geïnteresseerd in beide bent. U selecteert de gewenste uitvoerkolommen met een [kolommen in gegevensset selecteren] [ select-columns] module. Klik op [kolommen in gegevensset selecteren][select-columns], klikt u op **Launch column selector**, en selecteer **Scored Labels** en **berekend kansen**. Na het instellen van de uitvoerpoort van [kolommen in gegevensset selecteren] [ select-columns] en opnieuw wordt uitgevoerd, moet u het experiment scoreprofiel publiceren als een webservice door te klikken op **WEBSERVICE PUBLICEERT**. Het laatste experiment ziet er afbeelding 4.

![Het iris tweeklasse classificatie experiment](./media/interpret-model-results/4.png)

Afbeelding 4. Laatste scoreprofiel experiment van een tweeklasse classificatie iris probleem

Nadat u de webservice wordt uitgevoerd en voer de waarden van bepaalde functie van een exemplaar van de test, retourneert het resultaat twee getallen. Het eerste getal is de scored label en de tweede is de kans op scored. Deze bloem wordt voorspeld als klasse 1 met 0.9655 kans.

![Test interpreteren score-model](./media/interpret-model-results/4_1.png)

![Score berekenen testresultaten](./media/interpret-model-results/5.png)

Afbeelding 5. Resultaat van de Web service van iris tweeklasse classificatie

### <a name="multi-class-classification"></a>Meerdere klasse-classificatie
**Voorbeeld experiment**

In dit experiment voert u een taak letter erkenning als voorbeeld voor multiklassen classificatie. De classificatie probeert te voorspellen van een bepaalde letter (klasse) op basis van enkele hand geschreven kenmerkwaarden opgehaald uit de installatiekopieën van de hand geschreven.

![Voorbeeld van de letter erkenning](./media/interpret-model-results/5_1.png)

Er zijn 16 functies die zijn opgehaald uit de letter hand geschreven afbeeldingen in de trainingsgegevens. Het letters 26 vormen onze 26 klassen. Afbeelding 6 ziet u een experiment die een model multiklassen classificatie voor letter opname trainen en te voorspellen op dezelfde functie ingesteld op een test-gegevensset.

![Letter erkenning multiklassen classificatie experiment](./media/interpret-model-results/6.png)

Afbeelding 6. Letter erkenning multiklassen classificatie probleem experiment

Visualiseren van de resultaten van de [Score Model] [ score-model] module door te klikken op de uitvoerpoort van [Score Model] [ score-model] module en vervolgens te klikken op **Visualize**, ziet u inhoud zoals in afbeelding 7.

![Resultaten van score-model](./media/interpret-model-results/7.png)

Afbeelding 7. Scoreresultaten visualiseren model in een klasse met meerdere categorie

**Resultaat interpretatie**

16 kolommen aan de linkerkant vertegenwoordigen de waarden van de functie van de test is ingesteld. De kolommen met de namen zijn berekend kansen voor de klasse 'XX' net zoals de kolom berekend kansen in het geval van twee-klasse. Ze geven de kans weer die de bijbehorende vermelding in een bepaalde klasse valt. Bijvoorbeeld: voor de eerste vermelding bestaat 0.003571 waarschijnlijkheid aan dat dit een 'A,' 0.000451 waarschijnlijkheid aan dat het een "B", enzovoort. De laatste kolom (Scored Labels) is hetzelfde als Scored Labels in het geval van twee-klasse. De klasse met de grootste kans op scored wordt deze als de verwachte klasse van het bijbehorende item geselecteerd. Bijvoorbeeld: voor het eerste item het scored label is 'F' omdat het de grootste kans om te worden van een 'F' (0.916995) heeft.

**Publicatie van de Web service**

U kunt ook het scored label ophalen voor elk item en de kans dat het scored label. De basislogica is de grootste kans tussen de scored waarschijnlijkheid vinden. Om dit te doen, moet u gebruiken de [R-Script uitvoeren] [ execute-r-script] module. De R-code wordt weergegeven in afbeelding 8 en het resultaat van het experiment wordt weergegeven in afbeelding 9.

![R-codevoorbeeld](./media/interpret-model-results/8.png)

Afbeelding 8. R-code voor het uitpakken van Scored Labels en de bijbehorende waarschijnlijkheid van de labels

![Experiment resultaat](./media/interpret-model-results/9.png)

Afbeelding 9. Laatste scoreprofiel experiment van de letter erkenning multiklassen classificatie probleem

Nadat u publiceren en de webservice wordt uitgevoerd en voer de waarden van bepaalde functie gebruikersinvoer, de geretourneerde resultaat lijkt erop afbeelding 10. Deze brief hand geschreven met de uitgepakte 16 functies, wordt moet een "T' met 0.9715 waarschijnlijkheid voorspeld.

![Testmodule interpreteren score](./media/interpret-model-results/9_1.png)

![Testresultaat](./media/interpret-model-results/10.png)

Afbeelding 10. Resultaat van de Web service van multiklassen classificatie

## <a name="regression"></a>Regressie
Regressie problemen worden verschillende problemen met de classificatie. In een probleem classificatie waarmee u wilt voorspellen discrete klassen, zoals welke klasse een bloem iris behoort. Maar zoals u in het volgende voorbeeld van een probleem regressie ziet, probeert u een continue variabele zijn, zoals de prijs van een auto's voorspellen.

**Voorbeeld experiment**

Auto's voorspellen gebruiken als uw voorbeeld voor regressie. U probeert te voorspellen van de prijs van een auto op basis van de functies, zoals maken, brandstoftype, hoofdtekst en station wheel. Het experiment wordt weergegeven in afbeelding 11.

![Auto prijs regressie-experiment](./media/interpret-model-results/11.png)

Afbeelding 11. Auto prijs regressie probleem experiment

Visualiseren de [Score Model] [ score-model] -module, het resultaat ziet eruit als afbeelding 12.

![Resultaten van score berekenen voor auto prijs voorspelling probleem](./media/interpret-model-results/12.png)

Afbeelding 12. Score berekenen voor resultaat voor de voorspelling auto prijs probleem

**Resultaat interpretatie**

Scored Labels is de kolom met resultaten in het resultaat van deze score. De getallen zijn de voorspelde prijs voor elke auto.

**Publicatie van de Web service**

U kunt de regressie-experiment in een webservice publiceren en deze op dezelfde manier als in het geval van twee klasse classificatie gebruiken voor auto's voorspellen aanroepen.

![Score berekenen voor experiment voor auto prijs regressie probleem](./media/interpret-model-results/13.png)

Afbeelding 13. Score berekenen experiment van een auto prijs regressie probleem

De webservice wordt uitgevoerd, het geretourneerde resultaat ziet eruit als afbeelding 14. De voorspelde prijs voor deze auto is $15,085.52.

![Interpreteren scoremodule testen](./media/interpret-model-results/13_1.png)

![Resultaten van de module score berekenen](./media/interpret-model-results/14.png)

Afbeelding 14. Web service resultaat van een auto prijs regressie probleem

## <a name="clustering"></a>Clustering
**Voorbeeld experiment**

Laten we de gegevensset Iris opnieuw gebruiken voor het bouwen van een experiment clustering. Hier kunt u filteren uit de klasse labels in de gegevensset zodat deze alleen functies en kan worden gebruikt voor clustering. In deze iris gebruiksvoorbeeld, geef het aantal clusters worden twee tijdens de training, wat betekent dat u de bloemen zou clusteren in de twee klassen. Het experiment wordt weergegeven in afbeelding 15.

![Clustering probleem iris experimenteren](./media/interpret-model-results/15.png)

Afbeelding 15. Clustering probleem iris experimenteren

Clustering verschilt van de classificatie in dat de set trainingsgegevens geen compleet waarheid labels op zichzelf. Clustering groepen de exemplaren van de gegevensset training in verschillende clusters. Het model als de vermeldingen tijdens de training label door te leren van de verschillen tussen hun functies. Daarna kan het getrainde model worden gebruikt voor het classificeren van verdere toekomstige vermeldingen. Er zijn twee onderdelen van het resultaat dat we geïnteresseerd in het binnen een clustering probleem bent. Het eerste deel is de set trainingsgegevens labels en de tweede is een nieuwe gegevensset met het getrainde model classificeren.

Het eerste deel van het resultaat kan worden weergegeven door te klikken op de uitvoerpoort links van [Clustering Train-Model] [ train-clustering-model] en vervolgens te klikken op **Visualize**. De visualisatie wordt weergegeven in afbeelding 16.

![Clustering resultaat](./media/interpret-model-results/16.png)

Afbeelding 16. Resultaat van de set trainingsgegevens clustering visualiseren

Het resultaat van het tweede deel clustering nieuwe vermeldingen met het getrainde model van de clustering wordt weergegeven in afbeelding 17.

![Clustering resultaat visualiseren](./media/interpret-model-results/17.png)

Afbeelding 17. Resultaat van een nieuwe gegevensset clustering visualiseren

**Resultaat interpretatie**

Hoewel de resultaten van de twee delen uit verschillende experiment fasen voortvloeien, deze er hetzelfde uitzien en op dezelfde manier worden geïnterpreteerd. De eerste vier kolommen zijn functies. De laatste kolom, toewijzingen, is het resultaat van de voorspelling. De vermeldingen die hetzelfde nummer toegewezen worden zich in hetzelfde cluster, dat wil zeggen, dat ze delen overeenkomsten op een bepaalde manier (dit experiment maakt gebruik van de standaard Euclidean afstand metriek) voorspeld. Omdat u het aantal clusters 2 hebt opgegeven, worden de vermeldingen in toewijzingen label 0 of 1.

**Publicatie van de Web service**

U kunt het clustering experiment in een webservice publiceert en deze aanroepen voor dezelfde manier als in de classificatie tweeklasse gebruiksvoorbeeld voorspellingen clustering.

![Score berekenen voor experiment voor iris clustering probleem](./media/interpret-model-results/18.png)

Afbeelding 18. Score berekenen experiment van een iris clustering probleem

Nadat u hebt de webservice wordt uitgevoerd, is het resultaat van de geretourneerde ziet eruit als afbeelding 19. Deze bloem wordt moeten in een cluster 0 voorspeld.

![Test scoremodule interpreteren](./media/interpret-model-results/18_1.png)

![Resultaat van de module score berekenen](./media/interpret-model-results/19.png)

Afbeelding 19. Resultaat van de Web service van iris tweeklasse classificatie

## <a name="recommender-system"></a>Recommender systeem
**Voorbeeld experiment**

Voor recommender systemen, kunt u het probleem van de aanbeveling restaurant gebruiken als een voorbeeld: u kunt restaurant aanbevelen voor klanten op basis van de geschiedenis van hun classificatie. De ingevoerde gegevens bestaat uit drie delen:

* Beoordelingen van klanten restaurant
* Functie klantgegevens
* Gegevens van de functie restaurant

Er zijn verschillende dingen die we met doen kunt de [Train Matchbox Recommender] [ train-matchbox-recommender] -module in Azure Machine Learning:

* De classificaties voor een bepaalde gebruiker en het item te voorspellen
* Het beste van items aan een opgegeven gebruiker
* Gebruikers die zijn gerelateerd aan een bepaalde gebruiker zoeken
* Items die betrekking hebben op een bepaald item zoeken

U kunt kiezen wat u wilt doen door het selecteren van de vier opties in de **Recommender voorspelling soort** menu. Hier kunt u alle vier scenario's doorlopen.

![Matchbox recommender](./media/interpret-model-results/19_1.png)

Een typische Azure Machine Learning-experiment voor een systeem recommender eruit als afbeelding 20. Zie voor meer informatie over het gebruik van deze system aanbevelingsmodules [Train matchbox recommender] [ train-matchbox-recommender] en [Score matchbox recommender][score-matchbox-recommender].

![Recommender system experiment](./media/interpret-model-results/20.png)

Afbeelding 20. Recommender system experiment

**Resultaat interpretatie**

**De classificaties voor een bepaalde gebruiker en het item te voorspellen**

Door het selecteren van **classificatie voorspelling** onder **Recommender voorspelling soort**, vraagt u het systeem recommender te voorspellen van de classificatie voor een bepaalde gebruiker en het item. De visualisatie van de [Score Matchbox Recommender] [ score-matchbox-recommender] uitvoer ziet eruit als afbeelding 21.

![Resultaat van het systeem recommender--classificatie voorspelling beoordelen](./media/interpret-model-results/21.png)

Afbeelding 21. Het resultaat van de score van het systeem recommender--classificatie voorspelling visualiseren

De eerste twee kolommen zijn de item voor gebruikersgegevens-paren die is geleverd door de invoergegevens. De derde kolom is de voorspelde classificatie van een gebruiker voor een bepaald onderdeel. In de eerste rij de klant U1048 bijvoorbeeld naar snelheid restaurant 135026 voorspelde door 2.

**Het beste van items aan een opgegeven gebruiker**

Door het selecteren van **aanbeveling Item** onder **Recommender voorspelling soort**, vraagt u het systeem recommender aan te bevelen items naar een bepaalde gebruiker. De laatste parameter te kiezen in dit scenario is *item selectie aanbevolen*. De optie **van beoordeeld-Items (voor de evaluatie van het model)** is voornamelijk bedoeld voor evaluatie tijdens de training model. Voor deze fase voorspelling kiest **van alle Items**. De visualisatie van de [Score Matchbox Recommender] [ score-matchbox-recommender] uitvoer ziet eruit als afbeelding 22.

![Resultaat van het systeem recommender--aanbeveling item beoordelen](./media/interpret-model-results/22.png)

Afbeelding 22. Resultaat van de score van het systeem recommender--aanbeveling item visualiseren

De eerste dag van de zes kolommen vertegenwoordigt de opgegeven gebruikers-id aan te bevelen items, zoals bepaald door de invoergegevens. De vijf kolommen vertegenwoordigen de items die zijn aanbevolen aan de gebruiker in aflopende volgorde van belang. Bijvoorbeeld, is het meest aanbevolen restaurant voor klant U1048 in de eerste rij 134986, gevolgd door 135018, 134975 135021 en 132862.

**Gebruikers die zijn gerelateerd aan een bepaalde gebruiker zoeken**

Door het selecteren van **gerelateerde gebruikers** onder **Recommender voorspelling soort**, vraagt u het systeem recommender om gerelateerde gebruikers aan een bepaalde gebruiker te zoeken. Verwante gebruikers zijn de gebruikers die vergelijkbare voorkeuren hebben. De laatste parameter te kiezen in dit scenario is *gebruikersselectie gerelateerde*. De optie **van gebruikers dat beoordeeld artikelen (voor het model evaluatie)** is voornamelijk bedoeld voor evaluatie tijdens de training model. Kies **van alle gebruikers** voor deze fase van de voorspelling. De visualisatie van de [Score Matchbox Recommender] [ score-matchbox-recommender] uitvoer ziet eruit als afbeelding 23.

![Resultaat van de score van recommender system--gerelateerde gebruikers](./media/interpret-model-results/23.png)

Afbeelding 23. Scoreresultaten visualiseren van het systeem recommender--gerelateerde gebruikers

De eerste dag van de zes kolommen ziet u de opgegeven gebruiker id's die nodig zijn om te vinden van gerelateerde gebruikers worden geleverd door invoergegevens. De voorspelde gerelateerde gebruikers van de gebruiker opslaan de vijf kolommen in aflopende volgorde van belang. In de eerste rij is de meest relevante klant voor klant U1048 bijvoorbeeld U1051, gevolgd door U1066, U1044 U1017 en U1072.

**Items die betrekking hebben op een bepaald item zoeken**

Door het selecteren van **verwante Items** onder **Recommender voorspelling soort**, vraagt u het systeem recommender verwante items voor een bepaald item gevonden. Verwante items zijn het meest waarschijnlijk worden bevallen door dezelfde gebruiker items. De laatste parameter te kiezen in dit scenario is *gerelateerd item selectie*. De optie **van beoordeeld-Items (voor de evaluatie van het model)** is voornamelijk bedoeld voor evaluatie tijdens de training model. We kiezen **van alle Items** voor deze fase van de voorspelling. De visualisatie van de [Score Matchbox Recommender] [ score-matchbox-recommender] uitvoer ziet eruit als afbeelding 24.

![Resultaat van de score van recommender system--verwante items](./media/interpret-model-results/24.png)

Afbeelding 24. Scoreresultaten visualiseren van het systeem recommender--verwante items

De eerste dag van de zes kolommen vertegenwoordigt het opgegeven item-id's die nodig zijn voor het zoeken van verwante items worden geleverd door de invoergegevens. De vijf kolommen opslaan de voorspelde verwante items van het item in aflopende volgorde in termen van belang. Bijvoorbeeld, is het meest relevant artikel voor artikel 135026 in de eerste rij 135074, gevolgd door 135035, 132875 135055 en 134992.

**Publicatie van de Web service**

Het proces voor het publiceren van deze experimenten als webservices ophalen voorspellingen lijkt voor elk van de vier scenario's. Hier nemen we het tweede scenario (aanbevolen items naar een bepaalde gebruiker) als voorbeeld. U kunt dezelfde procedure met de andere drie volgen.

Het systeem getraind recommender op te slaan als een getraind model en de invoergegevens aan een enkele gebruiker-ID-kolom filteren, zoals aangevraagd, kunt u het experiment zoals in afbeelding 25 aansluiten en publiceren als een webservice.

![Score berekenen experiment van het probleem van de aanbeveling restaurant](./media/interpret-model-results/25.png)

Afbeelding 25. Score berekenen experiment van het probleem van de aanbeveling restaurant

De webservice wordt uitgevoerd, het geretourneerde resultaat ziet eruit als afbeelding 26. De vijf aanbevolen restaurant voor gebruiker U1048 zijn 134986, 135018 134975, 135021 en 132862.

![Voorbeeld van recommender systeemservice](./media/interpret-model-results/25_1.png)

![Voorbeeldresultaten experiment](./media/interpret-model-results/26.png)

Afbeelding 26. Resultaat van de Web service restaurant aanbeveling probleem

<!-- Module References -->
[assign-to-clusters]: https://msdn.microsoft.com/library/azure/eed3ee76-e8aa-46e6-907c-9ca767f5c114/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-matchbox-recommender]: https://msdn.microsoft.com/library/azure/55544522-9a10-44bd-884f-9a91a9cec2cd/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-clustering-model]: https://msdn.microsoft.com/library/azure/bb43c744-f7fa-41d0-ae67-74ae75da3ffd/
[train-matchbox-recommender]: https://msdn.microsoft.com/library/azure/fa4aa69d-2f1c-4ba4-ad5f-90ea3a515b4c/

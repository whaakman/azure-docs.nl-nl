---
title: 'K-Means Clustering: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Informatie over het gebruik van de module K-Means clusters in de Azure Machine Learning-service clustering modellen kunt trainen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/06/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 7e9b7c8f2cf86245322679198b84b50d2c5edce8
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65464673"
---
# <a name="module-k-means-clustering"></a>Module: K-means-clustering

In dit artikel wordt beschreven hoe u de *K-Means Clustering* module in Azure Machine Learning Studio om een ongetrainde K-means clustering-model te maken. 
 
K-means is een van de eenvoudigste en meest bekende *zonder supervisie* learning-algoritmen. U kunt het algoritme gebruiken voor een verscheidenheid aan machine learning-taken, zoals: 

* [Detecteren van abnormaal gegevens](https://msdn.microsoft.com/magazine/jj891054.aspx).
* Clustering tekstdocumenten.
* Gegevenssets analyseren voordat u andere methoden classificatie- of regressiemodel. 

Clustering-model, maakt u:

* Deze module toevoegen aan uw experiment.
* Verbinding maken met een gegevensset.
* Stel parameters, zoals het aantal clusters die u verwacht, de metriek afstand te gebruiken bij het maken van de clusters, enzovoort. 
  
Nadat u de module hyperparameters hebt geconfigureerd, u verbinding maken met het ongetrainde model aan de [Clustering Train-Model](train-clustering-model.md). Omdat de K-means-algoritme een methode zonder supervisie learning is, is een labelkolom is optioneel. 

+ Als uw gegevens een label bevat, kunt u de labelwaarden te begeleiden selectie van de clusters en het model te optimaliseren. 

+ Als uw gegevens geen label heeft, maakt de algoritme die mogelijk categorieën, die uitsluitend zijn gebaseerd op de gegevens vertegenwoordigt die clusters.  

##  <a name="understand-k-means-clustering"></a>K-means clustering begrijpen
 
In het algemeen clustering maakt gebruik van herhalende technieken groep gevallen in een gegevensset in clusters met vergelijkbare kenmerken hebben. Deze groeperingen zijn handig voor gegevens, het identificeren van afwijkingen in de gegevens verkennen en uiteindelijk voor het maken van voorspellingen. Clustering modellen ook kunt u relaties in een gegevensset die u mogelijk niet logisch afgeleid door te bladeren door of anderszins waarneming identificeren. Daarom wordt clustering vaak gebruikt in de eerste fasen van machine learning-taken, de gegevens verkennen en detectie van onverwachte correlaties.  
  
 Wanneer u een clustering-model met behulp van de methode K-means configureert, moet u een doelgetal *k* die aangeeft dat het aantal *centroids* die in het model. De massamiddelpunt is een punt die representatief is voor elk cluster. Het algoritme K-means wordt elke binnenkomende gegevenspunt toegewezen aan een van de clusters door het minimaliseren van de som binnen het cluster van de kwadraten. 
 
Wanneer deze de trainingsgegevens verwerkt, wordt het algoritme K-means begint met een eerste set met willekeurig gekozen centroids. Centroids fungeren als startpunt voor de clusters en ze toepassen van Lloyd algoritme iteratief verfijnen hun locaties. Het algoritme K-means stopt met het bouwen en verfijnen van clusters als deze voldoet aan een of meer van deze voorwaarden:  
  
-   De centroids stabiliseren, wat betekent dat de cluster-toewijzingen voor afzonderlijke punten niet meer wijzigen en de algoritme is geconvergeerd aan een oplossing.  
  
-   De algoritme voltooid met het opgegeven aantal iteraties.  
  
 Nadat u de fase training hebt voltooid, gebruikt u de [gegevens toewijzen aan Clusters](assign-data-to-clusters.md) module nieuwe aanvragen toewijzen aan een van de clusters die u met behulp van de algoritme K-means gevonden. U uitvoeren de toewijzing van het cluster door de afstand tussen de nieuwe aanvraag en de massamiddelpunt van elk cluster computing. Elke nieuwe aanvraag is toegewezen aan het cluster met de dichtstbijzijnde massamiddelpunt.  

## <a name="configure-the-k-means-clustering-module"></a>De module K-Means Clustering configureren
  
1.  Voeg de **K-Means Clustering** module naar het experimentcanvas.  
  
2.  Selecteer om te bepalen hoe u wilt dat het model te trainen, de **trainer aanmaakmodus** optie.  
  
    -   **Eén Parameter**: Als u bekend bent met de exacte parameters die u wilt gebruiken in de clustering-model, kunt u een specifieke set waarden opgeven als argumenten.  
  
3.  Voor **nummer van Centroids**, typt u het aantal clusters die u wilt dat de algoritme om te beginnen met.  
  
     Het model wordt niet gegarandeerd voor het produceren van exact dit aantal clusters. Het algoritme begint met dit aantal gegevenspunten en gegevensbrontabellen loopt, om de optimale configuratie.  
  
4.  De eigenschappen **initialisatie** wordt gebruikt om op te geven van de algoritme die wordt gebruikt voor het definiëren van de eerste clusterconfiguratie.  
  
    -   **Eerste N**: Een eerste aantal gegevenspunten zijn gekozen uit de gegevensset en gebruikt als de eerste manier. 
    
         Deze methode wordt ook aangeroepen de *Forgy methode*.  
  
    -   **Willekeurige**: Het algoritme willekeurig plaatst een gegevenspunt in een cluster en vervolgens het eerste gemiddelde moet de massamiddelpunt van van het cluster willekeurig toegewezen punten worden berekend. 

         Deze methode wordt ook aangeroepen de *willekeurige partitie* methode.  
  
    -   **K-middelen ++**: Dit is de standaardmethode voor het initialiseren van clusters.  
  
         De **K-middelen ++** algoritme is voorgesteld in 2007 door David Arthur en Sergei Vassilvitskii om te voorkomen dat slecht clustering door de norm K-means-algoritme. **K-middelen ++** norm K-means verbeterd met behulp van een andere methode voor het kiezen van de eerste cluster datacenters.  
  
    
5.  Voor **willekeurig getal seed**, (optioneel) Typ een waarde moet worden gebruikt als basis voor de initialisatie van het cluster. Deze waarde kan een aanzienlijke invloed op de selectie van het cluster hebben.  
  
6.  Voor **Metric**, kiest u de functie te gebruiken voor het meten van de afstand tussen vectoren cluster of tussen nieuwe gegevenspunten en het willekeurig gekozen massamiddelpunt. Azure Machine Learning ondersteunt de volgende metrische gegevens voor cluster-afstand:  
  
    -   **Euclidean**: De Euclidean afstand wordt doorgaans gebruikt als een meting van cluster spreiding voor K-means clustering. Met deze metriek verdient de voorkeur omdat deze de gemiddelde afstand tussen punten en de centroids wordt geminimaliseerd.
  
7.  Voor **iteraties**, typt u het aantal keren dat het algoritme de trainingsgegevens herhalen moet voordat deze de selectie van centroids wordt.  
  
     U kunt deze parameter saldo nauwkeurigheid van de trainingstijd aanpassen.  
  
8.  Voor **toewijzen label modus**, kies een optie die hoe een labelkolom aangeeft als deze aanwezig in de gegevensset is moet worden verwerkt.  
  
     Labels zijn optioneel, omdat het K-means clustering een zonder supervisie machine learning-methode. Echter als uw gegevensset al een labelkolom heeft, kunt u deze waarden voor de selectie van de clusters, of u kunt opgeven dat de waarden worden genegeerd.  
  
    -   **Labelkolom negeren**: De waarden in de labelkolom worden genegeerd en worden niet gebruikt bij het bouwen van het model.
  
    -   **Vul de ontbrekende waarden**: De kolomwaarden label worden gebruikt als functies kunt opbouwen van de clusters. Als alle rijen een label ontbreekt, wordt de waarde toegerekende met behulp van andere functies.  
  
    -   **Overschrijven uit die het dichtst bij center**: De kolomwaarden label vervangen door labelwaarden van de voorspelde, met behulp van het label van het punt die zich het dichtst bij de huidige massamiddelpunt.  

8.  Selecteer de **normaliseren functies** optie als u wilt normaliseren functies voordat een training.
  
     Als u van toepassing is normalisering, training, de gegevenspunten zijn genormaliseerd voor `[0,1]` door MinMaxNormalizer.

10. Het model te trainen.  
  
    -   Als u instelt **trainer aanmaakmodus** naar **één Parameter**, een label gegevensset toe te voegen en het model te trainen met behulp van de [Clustering-Model trainen](train-clustering-model.md) module.  
  
### <a name="results"></a>Resultaten

Als u klaar bent met het configureren en het model te trainen, hebt u een model dat u gebruiken kunt voor het genereren van scores. Er zijn echter meerdere manieren om het model te trainen en meerdere manieren om te bekijken en gebruiken van de resultaten: 

#### <a name="capture-a-snapshot-of-the-model-in-your-workspace"></a>Vastleggen van een momentopname van het model in uw werkruimte

Als u gebruikt de [Clustering Train-Model](train-clustering-model.md) module:

1. Met de rechtermuisknop op de **Clustering Train-Model** module.

2. Selecteer **Trained model**, en selecteer vervolgens **opslaan als het getrainde Model**.

De opgeslagen model vertegenwoordigt de trainingsgegevens op het moment dat u het model hebt opgeslagen. Als u later de trainingsgegevens gebruikt in het experiment bijwerkt, bijwerken niet het het opgeslagen model. 

#### <a name="see-the-clustering-result-dataset"></a>Zie de clustering resultaat-gegevensset 

Als u gebruikt de [Clustering Train-Model](train-clustering-model.md) module:

1. Met de rechtermuisknop op de **Clustering Train-Model** module.

2. Selecteer **resultaten gegevensset**, en selecteer vervolgens **Visualize**.

### <a name="tips-for-generating-the-best-clustering-model"></a>Tips voor het genereren van het beste model clustering  

Het is bekend dat de *seeding* proces dat wordt gebruikt tijdens de clustering kan aanzienlijke invloed hebben op het model. Seeding betekent dat de eerste positie van punten in mogelijke centroids.
 
Als de gegevensset veel uitschieters bevat, en een uitschieter wordt gekozen bij de seeding van de clusters, geen andere gegevenspunten zouden passen goed met het cluster en het cluster wordt mogelijk een singleton. Dat wil zeggen er dat slechts één punt.  
  
U kunt dit probleem op een aantal manieren voorkomen:  
  
-   Het aantal centroids wijzigen en probeer het meerdere seed-waarden.  
  
-   Maak meerdere modellen, variëren de metrische gegevens of meer doorlopen.  
  
In het algemeen met clustering modellen, is het mogelijk dat een bepaalde configuratie in een lokaal geoptimaliseerde set van clusters resulteren zal. Met andere woorden, de set van clusters die wordt geretourneerd door het model past bij de huidige gegevenspunten en is niet generaliseerbaar met andere gegevens. Als u een andere initiële configuratie gebruikt, kan het K-means-methode een configuratie met verschillende en misschien superieure vinden. 

---
title: 'K-Means Clustering: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Leer hoe u de clustering K-means-module gebruiken in Azure Machine Learning-service clustering modellen kunt trainen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/06/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 29aeb460581655190b7c3f4256647059f4642d3e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029698"
---
# <a name="k-means-clustering"></a>K-Means Clustering

In dit artikel wordt beschreven hoe u de **K-Means Clustering** module in Azure Machine Learning Studio om een ongetrainde K-means clustering-model te maken. 
 
K-means is een van de eenvoudigste en meest bekende *zonder supervisie* learning-algoritmen en kan worden gebruikt voor een verscheidenheid aan machine learning-taken, zoals [detecteren van abnormaal gegevens](https://msdn.microsoft.com/magazine/jj891054.aspx), clustering van tekst documenten en analyse van een gegevensset voordat u andere methoden classificatie- of regressiemodel. Voor het maken van een clustering-model, deze module toevoegen aan uw experiment, verbinding maken met een gegevensset en instellen van de parameters op, zoals het aantal clusters die u verwacht, de metriek afstand te gebruiken bij het maken van de clusters, enzovoort. 
  
Nadat u de module hyperparameters hebt geconfigureerd, verbinding maken met het ongetrainde model aan de [Clustering Train-Model](train-clustering-model.md) omdat het K-means-algoritme een methode zonder supervisie learning is, een labelkolom is optioneel. 

+ Als uw gegevens een label bevat, kunt u de labelwaarden te begeleiden selectie van de clusters en het model te optimaliseren. 

+ Als uw gegevens geen label heeft, maakt de algoritme die mogelijk categorieën, die uitsluitend zijn gebaseerd op de gegevens vertegenwoordigt die clusters.  
  

  
##  <a name="understanding-k-means-clustering"></a>Understanding k-means clustering
 
In het algemeen clustering maakt gebruik van herhalende technieken groep gevallen in een gegevensset in clusters met vergelijkbare kenmerken. Deze groeperingen zijn handig voor gegevens, het identificeren van afwijkingen in de gegevens verkennen en uiteindelijk voor het maken van voorspellingen. Clustering modellen ook kunt u relaties in een gegevensset die u mogelijk niet logisch afgeleid door te bladeren door of anderszins waarneming identificeren. Daarom wordt clustering vaak gebruikt in de eerste fasen van machine learning-taken, de gegevens verkennen en detectie van onverwachte correlaties.  
  
 Wanneer u een clustering-model met behulp van de methode k-means configureert, moet u een doelgetal *k* die wijzen op het aantal *centroids* die in het model. De massamiddelpunt is een punt die representatief is voor elk cluster. Het algoritme K-means wordt elke binnenkomende gegevenspunt toegewezen aan een van de clusters door het minimaliseren van de som binnen het cluster van de kwadraten. 
 
Bij het verwerken van de trainingsgegevens, begint het K-means-algoritme met een eerste set met randomnly gekozen centroids, die dienen als startpunt voor elk cluster, en is van toepassing Lloyd's-algoritme de locaties van de centroids iteratief te verfijnen. Het algoritme K-means stopt met het bouwen en verfijnen van clusters als deze voldoet aan een of meer van deze voorwaarden:  
  
-   De centroids stabiliseren, wat betekent dat de cluster-toewijzingen voor afzonderlijke punten niet meer wijzigen en de algoritme is geconvergeerd aan een oplossing.  
  
-   De algoritme voltooid met het opgegeven aantal iteraties.  
  
 Na voltooiing de fase van de training, gebruikt u de [gegevens toewijzen aan Clusters](assign-data-to-clusters.md) module nieuwe aanvragen toewijzen aan een van de clusters die door de algoritme k-means is gevonden. Toewijzing van het cluster wordt uitgevoerd door de afstand tussen de nieuwe aanvraag en de massamiddelpunt van elk cluster computing. Elke nieuwe aanvraag is toegewezen aan het cluster met de dichtstbijzijnde massamiddelpunt.  

## <a name="how-to-configure-k-means-clustering"></a>K-Means Clustering configureren
  
1.  Voeg de **K-Means Clustering** module naar het experimentcanvas.  
  
2.  Geef op hoe u het model te trainen, door in te stellen de **trainer aanmaakmodus** optie.  
  
    -   **Eén Parameter**: Als u bekend bent met de exacte parameters die u wilt gebruiken in de clustering-model, kunt u een specifieke set waarden opgeven als argumenten.  
  
   
  
3.  Voor **nummer van Centroids**, typt u het aantal clusters die u wilt dat de algoritme om te beginnen met.  
  
     Het model is niet noodzakelijkerwijs produceren exact dit aantal clusters. De algorithn begint met dit aantal gegevenspunten en gegevensbrontabellen loopt, om de optimale configuratie.  
  
     
  
4.  De eigenschappen **initialisatie** wordt gebruikt om op te geven van de algoritme die wordt gebruikt voor het definiëren van de eerste clusterconfiguratie.  
  
    -   **Eerste N**: Een eerste aantal gegevenspunten worden gekozen in de gegevensset en gebruikt als de eerste manier.  
  
         Ook wel de *Forgy methode*.  
  
    -   **Willekeurige**: Het algoritme willekeurig plaatst een gegevenspunt in een cluster en vervolgens het eerste gemiddelde moet de massamiddelpunt van van het cluster willekeurig toegewezen punten worden berekend.  
  
         Ook wel de *willekeurige partitie* methode.  
  
    -   **K-middelen ++**: Dit is de standaardmethode voor het initialiseren van clusters.  
  
         De **K-means ++** algoritme is voorgesteld in 2007 door David Arthur en k-means Sergei Vassilvitskii om te voorkomen dat slecht clustering van het standaard algoritme. **K-means ++** norm K-means verbeterd met behulp van een andere methode voor het kiezen van de eerste cluster datacenters.  
  
    
5.  Voor **willekeurig getal seed**, (optioneel) Typ een waarde moet worden gebruikt als basis voor de initialisatie van het cluster. Deze waarde kan een aanzienlijke invloed op de selectie van het cluster hebben.  
  
    
  
6.  Voor **Metric**, kiest u de functie te gebruiken voor het meten van de afstand tussen vectoren cluster of tussen nieuwe gegevenspunten en het willekeurig gekozen massamiddelpunt. Azure Machine Learning ondersteunt de volgende metrische gegevens voor cluster-afstand:  
  
    -   **Euclidean**: De Euclidean afstand wordt doorgaans gebruikt als een meting van cluster spreiding voor K-means clustering. Met deze metriek verdient de voorkeur omdat deze de gemiddelde afstand tussen punten en de centroids wordt geminimaliseerd.
  

  
7.  Voor **iteraties**, typt u het aantal keren dat het algoritme voor het voltooien van de selectie van centroids de trainingsgegevens moet herhalen.  
  
     U kunt deze parameter saldo nauwkeurigheid versus trainingstijd aanpassen.  
  
8.  Voor **toewijzen label modus**, kies een optie die aangeeft hoe een labelkolom, indien aanwezig in de gegevensset moet worden verwerkt.  
  
     Labels zijn optioneel, omdat het K-means clustering een zonder supervisie machine learning-methode. Echter als uw gegevensset al een labelkolom heeft, kunt u deze waarden voor het selecteren van de clusters, of u kunt opgeven dat de waarden worden genegeerd.  
  
    -   **Labelkolom negeren**: De waarden in de labelkolom worden genegeerd en worden niet gebruikt bij het bouwen van het model.
  
    -   **Vul de ontbrekende waarden**: De kolomwaarden label worden gebruikt als functies kunt opbouwen van de clusters. Als alle rijen een label ontbreekt, wordt de waarde toegerekende met behulp van andere functies.  
  
    -   **Overschrijven uit die het dichtst bij center**: De kolomwaarden label vervangen door labelwaarden van de voorspelde, met behulp van het label van het punt die zich het dichtst bij de huidige massamiddelpunt.  

8.  Selecteer de optie **normaliseren functies**, als u wilt normaliseren functies voordat een training.
  
     Als u van toepassing is normalisering training gegevenspunten zijn genormaliseerd voor `[0,1]` door MinMaxNormalizer.

10. Het model te trainen.  
  
    -   Als u instelt **trainer aanmaakmodus** naar **één Parameter**, een label gegevensset toe te voegen en het model te trainen met behulp van de [Clustering-Model trainen](train-clustering-model.md) module.  
  

### <a name="results"></a>Resultaten

Als u klaar bent met het configureren en het model te trainen, hebt u een model dat u gebruiken kunt voor het genereren van scores. Er zijn echter meerdere manieren om het model te trainen en meerdere manieren om te bekijken en gebruiken van de resultaten: 

#### <a name="capture-a-snapshot-of-the-model-in-your-workspace"></a>Vastleggen van een momentopname van het model in uw werkruimte

+ Als u gebruikt de [Clustering Train-Model](train-clustering-model.md) module
    1. Met de rechtermuisknop op de [Clustering Train-Model](train-clustering-model.md) module.
    2. Selecteer **Trained model** en klik vervolgens op **opslaan als het getrainde Model**.


De opgeslagen model vertegenwoordigt de trainingsgegevens op het moment dat u het model hebt opgeslagen. Als u later de trainingsgegevens gebruikt in het experiment bijwerkt, wordt de opgeslagen model niet bijgewerkt. 



#### <a name="see-the-clustering-result-dataset"></a>Zie de clustering resultaat-gegevensset 

+ Als u gebruikt de [Clustering Train-Model](train-clustering-model.md) module
    1. Met de rechtermuisknop op de [Clustering Train-Model](train-clustering-model.md) module.
    2. Selecteer **resultaten gegevensset** en klik vervolgens op **Visualize**.


### <a name="tips-for-generating-the-best-clustering-model"></a>Tips voor het genereren van het beste model clustering  

Het is bekend dat de **seeding** proces dat wordt gebruikt tijdens de clustering kan aanzienlijke invloed hebben op het model. Seeding betekent dat de eerste positie van punten in potental centroids.
 
Bijvoorbeeld, als de gegevensset veel uitschieters bevat en een uitschieter wordt gekozen bij de seeding van de clusters, geen andere gegevenspunten zouden passen goed met het cluster en het cluster wordt mogelijk een singleton: dat wil zeggen, een cluster met slechts één punt.  
  
Er zijn verschillende manieren om dit probleem te voorkomen:  
  
-   Het aantal centroids wijzigen en probeer het meerdere seed-waarden.  
  
-   Maak meerdere modellen, variëren de metrische gegevens of meer doorlopen.  
  
  
In het algemeen met clustering modellen, is het mogelijk dat een bepaalde configuratie in een lokaal geoptimaliseerde set van clusters resulteren zal. Met andere woorden, de set van clusters die zijn geretourneerd door het model past bij de huidige gegevenspunten en is geen generaliseerbaar met andere gegevens. Als u een andere initiële configuratie gebruikt, kan het K-means-methode een configuratie met verschillende en misschien superieure vinden. 

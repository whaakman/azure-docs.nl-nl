---
title: 'Beslissingsforest met twee klassen: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Informatie over het gebruik van de module Beslissingsforest met twee klassen in de Azure Machine Learning-service te maken van een machine learning-model op basis van de beslissing forests algoritme.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 73b7822c56e2b07eeefdedce1bce6d410d110ebc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65411489"
---
# <a name="two-class-decision-forest-module"></a>Beslissingsforest met twee klassen module

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Deze module gebruiken voor het maken van een machine learning-model op basis van de beslissing forests algoritme.  

Besluit forests zijn snelle, onder supervisie ensembles modellen. Deze module is een goede keuze als u wilt een doel met een maximum van twee resultaten te voorspellen. 

## <a name="understanding-decision-forests"></a>Understanding besluit forests

Dit besluit forest-algoritme is een methode die bestemd zijn voor de taken voor bestandsclassificatie learning ensembles. Ensembles methoden zijn afhankelijk van het algemene principe dat in plaats van te vertrouwen op één model, kunt u betere resultaten en een meer algemene model krijgen door het maken van meerdere verwante modellen en deze op een bepaalde manier worden gecombineerd. Over het algemeen bieden ensembles modellen betere dekking en de nauwkeurigheid van dan één beslisbomen. 

Er zijn veel manieren om afzonderlijke modellen maken en deze combineren in een ensembles. Deze bepaalde implementatie van een forest besluit werkt volgens het bouwen van meerdere beslisbomen en vervolgens **stemmende** over de meest populaire uitvoer-klasse. Uw stem is een van de bekende fabrikanten methoden voor het genereren van resultaten in een model ensembles. 

+ Veel afzonderlijke classificatie structuren die zijn gemaakt, met behulp van de volledige gegevensset, maar andere (meestal ingedeeld) beginpunt. Dit wijkt af van de benadering met willekeurige forest waarin de afzonderlijke beslisbomen alleen een willekeurige gedeelte van de gegevens of functies kunnen gebruiken.
+ Elke boomstructuur van de beslissingsstructuur forest voert een niet-genormaliseerde frequentie histogram van labels. 
+ Het aggregatieproces voor van de som van deze histogrammen en Normaliseert het resultaat voor de 'kansen' voor elk label. 
+ De vertakkingen die erop vertrouwen dat hoge voorspelling hebben een hoger gewicht in de definitieve beslissing van de ensembles.

Beslisbomen hebben in het algemeen veel voordelen voor classificatie taken:
  
- Ze kunnen niet-lineaire besluit grenzen vastleggen.
- U kunt trainen en voorspellen op grote hoeveelheden gegevens, zoals ze efficiënt in de berekening-en geheugengebruik zijn.
- Functieselectie is geïntegreerd in de processen voor training en -classificatie.  
- Structuren aankan ruis gegevens en veel functies.  
- Ze zijn niet-parametrische modellen, wat betekent dat ze kunnen gegevens met verschillende distributies worden verwerkt. 

Eenvoudige beslisbomen kunnen overfit op gegevens, en zijn echter minder generaliseerbaar dan structuur ensembles.

Zie voor meer informatie, [besluit Forests](https://go.microsoft.com/fwlink/?LinkId=403677).  

## <a name="how-to-configure"></a>Configureren
  
1.  Voeg de **Beslissingsforest met twee klassen** module aan uw experiment in Azure Machine Learning, en open de **eigenschappen** deelvenster van de module. 

    U vindt de module onder **Machine Learning**. Vouw **initialiseren**, en vervolgens **classificatie**.  
  
2.  Voor **pixels methode**, kiest u de methode die wordt gebruikt voor het maken van de afzonderlijke structuren.  U kunt kiezen uit **gezakt** of **repliceren**.  
  
    -   **Gezakt**: Ook gezakt heet *bootstrap aggregeren*. Bij deze methode wordt elke structuur geworden van een nieuwe steekproef, die zijn gemaakt door de oorspronkelijke gegevensset met vervanging willekeurig steekproeven totdat u de grootte van de oorspronkelijke een gegevensset hebt.  
  
         De uitvoer van de modellen gecombineerd met *stemmende*, dit is een vorm van aggregatie. Elke boomstructuur van een classificatie beslissingsforest voert een niet-genormaliseerde frequentie histogram van labels. De aggregatie is het som van deze histogrammen en normaliseren om op te halen van de 'kansen' voor elk label. Op deze manier heeft de vertakkingen die erop vertrouwen dat hoge voorspelling een hoger gewicht in de definitieve beslissing van de ensembles.  
  
         Zie voor meer informatie de Wikipedia-vermelding voor het verzamelen van Bootstrap.  
  
    -   **Repliceren**: Bij replicatie wordt elke structuur getraind op exact dezelfde invoer gegevens. Het bepalen van welke splitsing predicaat wordt gebruikt voor elk knooppunt willekeurige blijft en de structuren worden diverse.   
  
3.  Geef op hoe u het model te trainen, door in te stellen de **trainer aanmaakmodus** optie.  
  
    -   **Eén Parameter**: Als u hoe u wilt configureren van het model weet, kunt u een specifieke set waarden opgeven als argumenten.
  
4.  Voor **aantal beslisbomen**, typt u het maximum aantal beslisbomen dat kan worden gemaakt in de ensembles. Als u meer beslisbomen maakt, krijgt u mogelijk betere dekking, maar trainingstijd toeneemt.  
  
    > [!NOTE]
    >  Deze waarde bepaalt ook het aantal structuren weergegeven bij het visualiseren van het getrainde model. Als u wilt bekijken of één tree afdrukken, kunt u de waarde ingesteld op 1. Slechts één structuur kan echter zijn geproduceerd (de structuur met de eerste set parameters) en er is geen verdere pogingen worden uitgevoerd.
  
5.  Voor **maximale diepte van de beslisbomen**, typ een getal in om te beperken van de maximale diepte van een beslissingsstructuur. De diepte van de structuur van de vergroot, mogelijk precisie, indien even training overfitting en meer.
  
6.  Voor **aantal willekeurige splitsingen per knooppunt**, typt u het nummer van splitsingen moet worden gebruikt bij het bouwen van elk knooppunt van de structuur. Een *splitsen* betekent dat de functies in elk niveau van de structuur (node) willekeurig worden verdeeld.
  
7.  Voor **minimumaantal steekproeven per leaf-knooppunt**, het minimum aantal aanvragen die zijn vereist voor het maken van een terminal (leaf) van de knooppunten in een boomstructuur.
  
     Door deze waarde, verhoogt u de drempelwaarde voor het maken van nieuwe regels. Bijvoorbeeld, met de standaardwaarde van 1, zelfs op een enkel geval kan leiden tot een nieuwe regel moet worden gemaakt. Als u de waarde 5 verhoogt, wordt de trainingsgegevens hoeft te bevatten ten minste vijf aanvragen die voldoen aan de dezelfde voorwaarden.  
  
8.  Selecteer de **onbekende waarden voor de categorische functies toestaan** optie voor het maken van een groep voor onbekende waarden in de training of validatie-sets. Het model is mogelijk minder nauwkeurig voor bekende waarden, maar dit kunt doen van betere voorspellingen voor nieuwe (onbekend) waarden bieden. 

     Als u deze optie uitschakelt, kan het model alleen de waarden die zijn opgenomen in de trainingsgegevens accepteren.
  
9. Koppelen van een gegevensset met gelabelde en een van de [trainingsmodules](module-reference.md):  
  
    -   Als u instelt **trainer aanmaakmodus** naar **één Parameter**, gebruikt u de [Train Model](./train-model.md) module.  
  
    
## <a name="results"></a>Resultaten

Na de training is voltooid:

+ Als u wilt zien van de structuur die is gemaakt op elke herhaling, met de rechtermuisknop op de uitvoer van de [Train Model](./train-model.md) -module en selecteer **Visualize**.
  
    Klik op elke structuur als u wilt inzoomen op de splitsingen en de regels voor elk knooppunt.

+ Om een momentopname van het model hebt opgeslagen, met de rechtermuisknop op de **getrainde Model** uitvoer, en selecteer **Model opslaan**. De opgeslagen model is niet bijgewerkt op opeenvolgende uitvoeringen van het experiment.

+ Voor het gebruik van het model voor het scoren, voeg de **Score Model** module aan een experiment.


## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 
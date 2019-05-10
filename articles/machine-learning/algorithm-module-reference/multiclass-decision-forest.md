---
title: 'Beslissingsforest met multiklasse: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Informatie over het gebruik van de module Beslissingsforest met Multiklasse in Azure Machine Learning-service te maken van een machine learning-model op basis van de *forest besluit* algoritme.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 10364d014431a500e7c38a02d47f432cd464feb6
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65411456"
---
# <a name="multiclass-decision-forest-module"></a>Beslissingsforest met multiklasse-module

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Deze module gebruiken voor het maken van een machine learning-model op basis van de *forest besluit* algoritme. Een beslissingsforest is een model ensembles die snel een reeks beslisbomen tijdens het leren van gecodeerde gegevens.

## <a name="more-about-decision-forests"></a>Meer informatie over de beslissing forests

De beslissing forest-algoritme is een ensembles learning-methode voor classificatie. Het algoritme werkt volgens het bouwen van meerdere beslisbomen en vervolgens *stemmende* over de meest populaire uitvoer-klasse. Uw stem is een vorm van aggregatie hebben, in welke elke boomstructuur van een beslissing classificatie forest een histogram van de niet-genormaliseerde frequentie van labels levert. Het aggregatieproces voor van de som van deze histogrammen en Normaliseert het resultaat voor de 'kansen' voor elk label. De vertakkingen die erop vertrouwen dat hoge voorspelling hebben een hoger gewicht in de definitieve beslissing van de ensembles.

Beslisbomen zijn in het algemeen niet parametrische modellen, wat betekent dat ze ondersteuning voor gegevens met verschillende distributies. In de structuur van wordt een reeks eenvoudige tests uitgevoerd voor elke klasse, de niveaus van een boomstructuur verhogen tot een leafknooppunt (besluit) is bereikt.

Beslisbomen hebben veel voordelen:

+ Ze kunnen niet-lineaire besluit grenzen vertegenwoordigen.
+ Ze zijn efficiënt in de berekening- en geheugengebruik tijdens trainen en voorspellen.
+ Ze uitvoeren geïntegreerde functieselectie en -classificatie.
+ Ze zijn flexibel met ruis functies.

De classificatie van de beslissing forest in Azure Machine Learning bestaat uit een ensembles van beslissingsstructuren. Over het algemeen bieden ensembles modellen betere dekking en de nauwkeurigheid van dan één beslisbomen. Zie voor meer informatie, [Decision trees](https://go.microsoft.com/fwlink/?LinkId=403677).

## <a name="how-to-configure-multiclass-decision-forest"></a>Beslissingsforest met Multiklasse configureren



1. Toevoegen de **Beslissingsforest met Multiklasse** module aan uw experiment in de interface. U vindt deze module onder **Machine Learning**, **Initialize Model**, en **classificatie**.

2. Dubbelklik op de module te openen de **eigenschappen** deelvenster.

3. Voor **pixels methode**, kiest u de methode die wordt gebruikt voor het maken van de afzonderlijke structuren.  U kunt kiezen uit gezakt of replicatie.

    + **Gezakt**: Ook gezakt heet *bootstrap aggregeren*. Bij deze methode wordt elke structuur geworden van een nieuwe steekproef, die zijn gemaakt door de oorspronkelijke gegevensset met vervanging willekeurig steekproeven totdat u de grootte van de oorspronkelijke een gegevensset hebt. De uitvoer van de modellen gecombineerd met *stemmende*, dit is een vorm van aggregatie. Zie voor meer informatie de Wikipedia-vermelding voor het verzamelen van Bootstrap.

    + **Repliceren**: Bij replicatie wordt elke structuur getraind op exact dezelfde invoer gegevens. Het bepalen van welke splitsing predicaat wordt gebruikt voor elk knooppunt blijft willekeurige, het maken van verschillende structuren.

   

4. Geef op hoe u het model te trainen, door in te stellen de **trainer aanmaakmodus** optie.

    + **Eén Parameter**: Selecteer deze optie als u hoe u wilt configureren van het model weet, en een set waarden als argumenten.


5. **Aantal beslisbomen**: Typ het maximum aantal beslisbomen dat kan worden gemaakt in de ensembles. Als u meer beslisbomen maakt, krijgt u mogelijk betere dekking, maar training moment verhogen.

    Deze waarde bepaalt ook het aantal structuren weergegeven in de resultaten bij het visualiseren van het getrainde model. Als u wilt bekijken of afdrukken van één tree, kunt u de waarde instelt op 1. Dit betekent echter dat slechts één structuur kan worden gemaakt (de structuur met de eerste set parameters), en er is geen verdere pogingen worden uitgevoerd.

6. **Maximale diepte van de beslisbomen**: Typ een getal in om te beperken van de maximale diepte van een beslissingsstructuur. De diepte van de structuur van de vergroot, mogelijk precisie, indien even training overfitting en meer.

7. **Het aantal willekeurige splitsingen per knooppunt**: Typ het aantal splitsingen moet worden gebruikt bij het bouwen van elk knooppunt van de structuur. Een *splitsen* betekent dat de functies in elk niveau van de structuur (node) willekeurig worden verdeeld.

8. **Minimum aantal voorbeelden per leaf-knooppunt**: Het minimumaantal cases die nodig zijn voor het maken van een terminal (leaf) van de knooppunten in een boomstructuur aangeven. Door deze waarde, verhoogt u de drempelwaarde voor het maken van nieuwe regels.

    Bijvoorbeeld, met de standaardwaarde van 1, zelfs op een enkel geval kan leiden tot een nieuwe regel moet worden gemaakt. Als u de waarde 5 verhoogt, wordt de trainingsgegevens hoeft te bevatten ten minste vijf aanvragen die voldoen aan de dezelfde voorwaarden.



10. Verbinding maken met een gegevensset met gelabelde en een van de trainingsmodules:

    + Als u instelt **trainer aanmaakmodus** naar **één Parameter**, gebruikt u de [Train Model](./train-model.md) module.

11. Voer het experiment uit.

## <a name="results"></a>Resultaten

Na de training is voltooid:

+ Als u wilt zien van de structuur die is gemaakt op elke herhaling, met de rechtermuisknop op de uitvoer van de [Train Model](./train-model.md) -module en selecteer **Visualize**.
+ Voor de regels voor elk knooppunt, klikt u op elke structuur Inzoomen op de splitsingen.


## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 
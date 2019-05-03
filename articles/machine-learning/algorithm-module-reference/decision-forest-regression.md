---
title: 'Beslissingsforest met regressie: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Informatie over het gebruik van de module besluit Forest regressie in de Azure Machine Learning-service te maken van een regressiemodel dat op basis van een ensembles van beslissingsstructuren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 67016582149824c8deb83b54102190a57bd19383
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028798"
---
# <a name="decision-forest-regression-module"></a>Beslissingsforest met regressie-module

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module om een regressiemodel dat op basis van een ensembles van beslissingsstructuren maken.

Nadat u het model hebt geconfigureerd, moet u het model met behulp van een gegevensset met gelabelde trainen en het [Train Model](./train-model.md) module.  Het getrainde model kan vervolgens worden gebruikt om voorspellingen te doen. 

## <a name="how-it-works"></a>Hoe werkt het?

Beslisbomen zijn niet-parametrische modellen die een reeks eenvoudige tests voor elk exemplaar een binaire gegevens van de boomstructuur doorlopen uitvoeren totdat een leafknooppunt (besluit) is bereikt.

Beslisbomen hebben deze voordelen:

- Ze zijn efficiënt in de berekening- en geheugengebruik tijdens trainen en voorspellen.

- Ze kunnen niet-lineaire besluit grenzen vertegenwoordigen.

- Deze geïntegreerde functieselectie en classificatie uitvoeren en hiertegen bestand met ruis functies.

Dit regressiemodel bestaat uit een ensembles van beslissingsstructuren. Elke structuur in een forest van de beslissing regressie voert een distributie Gaussiaans als een voorspelling. Een aggregatie wordt uitgevoerd via de ensembles van structuren een Gaussiaans-distributie die het dichtst bij de gecombineerde verdeling van alle structuren vinden in het model.

Raadpleeg dit artikel voor meer informatie over het theoretische framework voor deze algoritme en de uitvoering ervan: [Besluit Forests: Een geünificeerd Framework voor classificatie, regressie, dichtheid schatting, inlaatspruitstuk Learning en leren met semi supervisie ](https://www.microsoft.com/en-us/research/publication/decision-forests-a-unified-framework-for-classification-regression-density-estimation-manifold-learning-and-semi-supervised-learning/?from=http%3A%2F%2Fresearch.microsoft.com%2Fapps%2Fpubs%2Fdefault.aspx%3Fid%3D158806#)

## <a name="how-to-configure-decision-forest-regression-model"></a>Besluit Forest regressiemodel configureren

1. Voeg de **besluit Forest regressie** module naar het experimentcanvas. U kunt de module niet vinden in de interface onder **Machine Learning**, **Initialize Model**, en **regressie**.

2. Open de module-eigenschappen, en voor **pixels methode**, kiest u de methode die wordt gebruikt voor het maken van de afzonderlijke structuren.  U kunt kiezen uit **gezakt** of **repliceren**.

    - **Gezakt**: Ook gezakt heet *bootstrap aggregeren*. Elke structuur in een forest van de beslissing regressie voert een distributie Gaussiaans beperkend voorspelling. De aggregatie is een Gaussiaans waarvan de eerste twee seconden overeenkomen met de momenten van de combinatie van Gaussians gegeven door een combinatie van alle Gaussians geretourneerd door afzonderlijke structuren vinden.

         Zie voor meer informatie de Wikipedia-vermelding voor [Bootstrap aggregeren](https://wikipedia.org/wiki/Bootstrap_aggregating).

    - **Repliceren**: Bij replicatie wordt elke structuur getraind op exact dezelfde invoer gegevens. Het bepalen van welke splitsing predicaat wordt gebruikt voor elk knooppunt willekeurige blijft en de structuren worden diverse.

         Voor meer informatie over het trainingsproces met de **repliceren** Zie [besluit Forests voor Computer Vision en medische analyse van de afbeelding. Criminisi en J. Shotton. Springer 2013.](http://research.microsoft.com/projects/decisionforests/).

3. Geef op hoe u het model te trainen, door in te stellen de **trainer aanmaakmodus** optie.

    - **Single Parameter**

      Als u hoe u wilt configureren van het model weet, kunt u een specifieke set waarden opgeven als argumenten. U kunt mogelijk deze waarden hebt geleerd door experimenten of deze ontvangen als richtlijn.



4. Voor **aantal beslisbomen**, geeft het totale aantal beslissingsstructuren om in de ensembles te maken. Als u meer beslisbomen maakt, krijgt u mogelijk betere dekking, maar trainingstijd wordt verhoogd.

    > [!TIP]
    > Deze waarde bepaalt ook het aantal structuren weergegeven bij het visualiseren van het getrainde model. Als u wilt bekijken of één tree afdrukken, kunt u de waarde ingesteld op 1. Dit betekent echter dat slechts één structuur worden geproduceerd (de structuur met de eerste set parameters) en er is geen verdere iteraties wordt uitgevoerd.

5. Voor **maximale diepte van de beslisbomen**, typ een getal in om te beperken van de maximale diepte van een beslissingsstructuur. De diepte van de structuur van de vergroot, mogelijk precisie, indien even training overfitting en meer.

6. Voor **aantal willekeurige splitsingen per knooppunt**, typt u het nummer van splitsingen moet worden gebruikt bij het bouwen van elk knooppunt van de structuur. Een *splitsen* betekent dat de functies in elk niveau van de structuur (node) willekeurig worden verdeeld.

7. Voor **minimumaantal steekproeven per leaf-knooppunt**, het minimum aantal aanvragen die zijn vereist voor het maken van een terminal (leaf) van de knooppunten in een boomstructuur.

     Door deze waarde, verhoogt u de drempelwaarde voor het maken van nieuwe regels. Bijvoorbeeld, met de standaardwaarde van 1, zelfs op een enkel geval kan leiden tot een nieuwe regel moet worden gemaakt. Als u de waarde 5 verhoogt, wordt de trainingsgegevens hoeft te bevatten ten minste vijf aanvragen die voldoen aan de dezelfde voorwaarden.


9. Verbinding maken met een gegevensset met gelabelde en verbinding maken met een enkelvoudig label-kolom met resultaten niet meer dan twee selecteren [Train Model](./train-model.md).

    - Als u instelt **trainer aanmaakmodus** optie naar **één Parameter**, het model te trainen met behulp van de [Train Model](./train-model.md) module.

   

10. Voer het experiment uit.

### <a name="results"></a>Resultaten

Na de training is voltooid:

+ Als u wilt zien van de structuur die is gemaakt op elke herhaling, met de rechtermuisknop op de uitvoer van de trainingsmodule en selecteer **Visualize**.

+ Als u de regels voor elk knooppunt, klikt u op elke structuur en inzoomen op de splitsingen.

+ Met de rechtermuisknop op de uitvoer van de trainingsmodule om een momentopname van het getrainde model hebt opgeslagen, en selecteer **opslaan als getrainde Model**. Deze kopie van het model is niet bijgewerkt op opeenvolgende uitvoeringen van het experiment. 

## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 
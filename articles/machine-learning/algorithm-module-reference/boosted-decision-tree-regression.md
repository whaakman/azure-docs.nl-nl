---
title: 'Boosted Decision Tree regressie: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Informatie over het gebruik van de module Boosted Decision Tree regressie in de Azure Machine Learning-service te maken van een ensembles van regressiestructuren met behulp van verhogen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 67e54f10074ee566ce974dbd27485904bfe0a653
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65411551"
---
# <a name="boosted-decision-tree-regression-module"></a>Boosted Decision Tree regressie-module

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Deze module gebruiken voor het maken van een ensembles van regressiestructuren met behulp van verhogen. *Versterking* betekent dat elke structuur afhankelijk van de voorafgaande structuren is. Het algoritme leert door de resterende van de vertakkingen die eraan vooraf gingen. Dus doorgaans in een decision tree ensembles versterking de nauwkeurigheid vergroten met kleine kans op minder dekking.  
  
Deze regressiemethode is een methode voor het leren met supervisie, en daarom vereist een *gegevensset met het label*. De labelkolom moet numerieke waarden bevatten.  

> [!NOTE]
> Gebruik deze module alleen met gegevenssets die gebruikmaken van numerieke variabelen.  

Nadat u het model hebt gedefinieerd, traint met behulp van de [Train Model](./train-model.md).

> [!TIP]
> Wilt u meer weten over de structuren die zijn gemaakt? Nadat het model is getraind, met de rechtermuisknop op de uitvoer van de [Train Model](./train-model.md) -module en selecteer **Visualize** om te zien van de structuur die is gemaakt op elke iteratie. U kunt inzoomen op de splitsingen voor elke structuur en bekijkt de regels voor elk knooppunt.  
  
## <a name="more-about-boosted-regression-trees"></a>Meer informatie over het boosted regressiestructuren  

Versterking is een van verschillende klassieke methoden voor het maken van modellen ensembles, samen met gezakt, willekeurige forests, enzovoort.  Boosted decision trees gebruiken in Azure Machine Learning, een efficiënte implementatie van de kleurovergang voor de DATAMART versterking algoritme. Gradient boosting is een machine learning-technieken voor regressie problemen. Deze bouwt elke boomstructuur regressie van een stap voor stap wijze, met een vooraf gedefinieerde verlies-functie voor het meten van de fout in elke stap en corrigeren voor het in de volgende. De voorspellingsmodel is dus eigenlijk een ensembles van zwakkere voorspellende modellen.  
  
Problemen met regressie, versterking bouwt een reeks structuren in een stap voor stap manier en selecteert vervolgens de optimale structuur met behulp van een willekeurige differentieerbare verlies-functie.  
  
Voor meer informatie, Zie de volgende artikelen:  
  
+ [https://wikipedia.org/wiki/Gradient_boosting#Gradient_tree_boosting](https://wikipedia.org/wiki/Gradient_boosting)

    Deze Wikipedia-artikel op gradient boosting wordt enige achtergrondinformatie gegeven op boosted structuren. 
  
-  [https://research.microsoft.com/apps/pubs/default.aspx?id=132652](https://research.microsoft.com/apps/pubs/default.aspx?id=132652)  

    Microsoft Research: Van RankNet naar LambdaRank naar LambdaMART: Een overzicht. Door J.C. Burges.

De kleurovergang voor de verhoging van de methode kan ook worden gebruikt voor classificatie problemen doordat ze naar de regressie bij een geschikte verlies-functie. Zie voor meer informatie over de implementatie boosted structuren voor classificatie taken [Two-Class Boosted beslissingsstructuur](./two-class-boosted-decision-tree.md).  

## <a name="how-to-configure-boosted-decision-tree-regression"></a>Boosted Decision Tree regressie configureren

1.  Voeg de **beslissingsstructuur heeft de Klantenopbrengst** module naar het experimentcanvas. U vindt deze module onder **Machine Learning**, **initialiseren**onder de **regressie** categorie. 
  
2.  Geef op hoe u het model te trainen, door in te stellen de **trainer aanmaakmodus** optie.  
  
    -   **Eén Parameter**: Selecteer deze optie als u hoe u wilt configureren van het model weet, en een specifieke set waarden als argumenten.  
   
  
3. **Maximum aantal Bladeren per structuur**: Het maximum aantal terminal knooppunten (bladeren) dat kan worden gemaakt in een boomstructuur aangeven.  

    Door deze waarde, u mogelijk Verhoog de grootte van de structuur en betere nauwkeurigheid, indien overfitting en meer tijd voor trainingen.  

4. **Minimum aantal voorbeelden per leaf-knooppunt**: Het minimumaantal cases die zijn vereist voor het maken van een terminal (leaf) van de knooppunten in een boomstructuur aangeven.

    Door deze waarde, verhoogt u de drempelwaarde voor het maken van nieuwe regels. Bijvoorbeeld, met de standaardwaarde van 1, zelfs op een enkel geval kan leiden tot een nieuwe regel moet worden gemaakt. Als u de waarde 5 verhoogt, wordt de trainingsgegevens hoeft te bevatten ten minste 5 aanvragen die voldoen aan de dezelfde voorwaarden.

5. **Learning-tarief**: Typ een getal tussen 0 en 1 die de Stapgrootte van de bij het leren definieert. Het leertempo bepaalt hoe snel of traag de cursist convergeert wel op de optimale oplossing ligt. Als de Stapgrootte van de is te groot is, kunt u de optimale oplossing overschrijding. Als de stap te klein is, duurt training langer om te convergeren op de beste oplossing.

6. **Aantal structuren gebouwd**: Geeft het totale aantal beslissingsstructuren om te maken in de ensembles. Als u meer beslisbomen maakt, krijgt u mogelijk betere dekking, maar trainingstijd toeneemt.

    Deze waarde bepaalt ook het aantal structuren weergegeven bij het visualiseren van het getrainde model. Als u wilt zien of een boomstructuur ingle afdrukken, kunt u de waarde ingesteld op 1. echter slechts één structuur wordt gemaakt (de structuur met de eerste set parameters) en er is geen verdere pogingen worden uitgevoerd.

7. **Willekeurig getal seed**: Typ een optionele niet-negatief geheel getal in om te gebruiken als de random seed-waarde. Een seed opgeeft, zorgt u ervoor reproduceerbaarheid voor uitvoert met dezelfde gegevens en parameters.

    De random seed is standaard ingesteld op 0, wat betekent dat de eerste seed-waarde wordt opgehaald uit de systeemklok.
  
8. **Onbekende categorische niveaus toestaan**: Selecteer deze optie voor het maken van een groep voor onbekende waarden in de sets trainings- en validatie. Als u deze optie uitschakelt, kan het model alleen de waarden die zijn opgenomen in de trainingsgegevens accepteren. Het model is mogelijk minder nauwkeurig voor bekende waarden, maar dit kunt doen van betere voorspellingen voor nieuwe (onbekend) waarden bieden.

9. Een gegevensset training en een van de trainingsmodules toevoegen:

    - Als u instelt **trainer aanmaakmodus** optie naar **één Parameter**, gebruikt u de [Train Model](train-model.md) module.  
  
    

10. Voer het experiment uit.  
  
### <a name="results"></a>Resultaten

Na de training is voltooid:

+ Als u wilt zien van de structuur die is gemaakt op elke herhaling, met de rechtermuisknop op de uitvoer van de [Train Model](train-model.md) -module en selecteer **Visualize**.
  
     Klik op elke structuur als u wilt inzoomen op de splitsingen en de regels voor elk knooppunt.  

+ Voor het gebruik van het model voor het scoren, verbindt deze met [Score Model](./score-model.md), om waarden voor nieuwe invoer voorbeelden te voorspellen.

+ Om een momentopname van het getrainde model hebt opgeslagen, met de rechtermuisknop op de **Trained model** uitvoer van de trainingsmodule en selecteer **OpslaanAls**. De kopie van het getrainde model dat u opslaat, is niet bijgewerkt op opeenvolgende uitvoeringen van het experiment.

## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 
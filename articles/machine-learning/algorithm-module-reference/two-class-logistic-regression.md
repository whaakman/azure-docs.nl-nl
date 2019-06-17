---
title: 'Two-Class Logistic Regression: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de module Two-Class Logistic Regression in Azure Machine Learning-service te maken van een model voor logistieke regressie die kan worden gebruikt om te voorspellen van uitkomsten twee (en slechts twee).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: aacaf6c64ef77d0e694f97e3675060eca33794ed
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65029248"
---
# <a name="two-class-logistic-regression-module"></a>Two-Class Logistic Regression-module

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Deze module gebruiken voor het maken van een model voor logistieke regressie die kan worden gebruikt om te voorspellen van uitkomsten twee (en slechts twee). 

Logistieke regressie is een bekende statistische techniek die wordt gebruikt voor het modelleren van vele soorten problemen. Deze algoritme is een *onder supervisie learning* methode.  Daarom moet u een gegevensset die al de resultaten bevat voor het model te trainen.  

### <a name="about-logistic-regression"></a>Over logistieke regressie  

Logistieke regressie is een bekende methode in statistieken die wordt gebruikt voor het voorspellen van de kans op een resultaat en met name populaire voor classificatie taken. Het algoritme voorspelt de kans op een gebeurtenis door gegevens naar een logistieke functie.
  
In deze module worden de classificatiealgoritme is geoptimaliseerd voor dichotomous of binaire variabelen. Als u nodig hebt voor het classificeren van meerdere resultaten, gebruikt u de [Multiklasse Logistic Regression](./multiclass-logistic-regression.md) module.

##  <a name="how-to-configure"></a>Configureren  

Als u wilt dit model te trainen, moet u een gegevensset die een label of klasse kolom bevat. Omdat deze module is bedoeld voor problemen met twee klassen, moet de kolom label of klasse precies twee waarden bevatten. 

Bijvoorbeeld, de labelkolom mogelijk [gestemd] met de mogelijke waarden van "Ja" of "Nee". Of [kredietrisico], kan het zijn met de mogelijke waarden voor 'Hoog' of 'Laag'. 
  
1.  Voeg de **Two-Class Logistic Regression** module naar het experimentcanvas.  
  
2.  Geef op hoe u het model te trainen, door in te stellen de **trainer aanmaakmodus** optie.  
  
    -   **Eén Parameter**: Als u hoe u wilt configureren van het model weet, kunt u een specifieke set waarden opgeven als argumenten.  
  
3.  Voor **optimalisatie tolerantie**, Geef een waarde voor drempel te gebruiken bij het optimaliseren van het model. Als de verbetering tussen iteraties onder de opgegeven drempelwaarde valt, het algoritme wordt beschouwd als aan een oplossing hebben geconvergeerde en training stopt.  
  
4.  Voor **L1 regularisatie gewicht** en **L2 regularisatie gewicht**, typt u een waarde moet worden gebruikt voor de parameters regularisatie L1 en L2. Een andere waarde dan nul wordt aanbevolen voor beide.  
  
     *Regularisatie* is een methode om te voorkomen door het penalizing modellen met extreme coëfficiënt waarden. Regularisatie werkt door de boete die is gekoppeld aan de fout van de hypothese coëfficiënt waarden toe te voegen. Zo zou een nauwkeurige model met extreme coëfficiënt waarden meer worden bestraft, maar een minder nauwkeurig model met meer conservatieve waarden minder zou worden bestraft.  
  
     L1 en L2 regularisatiegraad hebben verschillende effecten en gebruikt.  
  
    -   L1 kan worden toegepast op sparse modellen, dit is handig als u werkt met high-dimensionale gegevens.  
  
    -   L2-regularisatie is daarentegen beter voor gegevens die geen sparse.  
  
     Dit algoritme biedt ondersteuning voor een lineair kombinaci hodnot L1 en L2 regularisatie: dat wil zeggen, als <code>x = L1</code> en <code>y = L2</code>, klikt u vervolgens <code>ax + by = c</code> definieert het lineair bereik van de voorwaarden regularisatie.  
  
    > [!NOTE]
    >  Wilt u meer informatie over L1 en L2 regularisatie? Het volgende artikel vindt u een beschrijving van hoe L1 en L2 regularisatie verschillen en hoe ze van invloed op model aanpassing van labels, met codevoorbeelden voor logistieke regressie en modellen neurale netwerk:  [L1 en L2 Regularisatiegraad voor Machine Learning](https://msdn.microsoft.com/magazine/dn904675.aspx)  
    >
    > Verschillende lineair combinaties van L1 en L2 termen hebben is ontworpen voor logistieke regressiemodellen: bijvoorbeeld [elastische net regularisatie](https://wikipedia.org/wiki/Elastic_net_regularization). Het is raadzaam dat u verwijzen naar deze combinaties voor het definiëren van een combinatie van lineair die in uw model van kracht is.
      
5.  Voor **geheugengrootte voor L BFGS**, geef de hoeveelheid geheugen te gebruiken voor *L BFGS* optimalisatie.  
  
     L-BFGS staat voor 'beperkt geheugen Broyden Fletcher-Goldfarb Shanno'. Het is een optimalisatie-algoritme dat wordt gebruikt voor schatting van de parameter. Deze parameter geeft het aantal afgelopen posities en verlopen om op te slaan voor de berekening van de volgende stap.  
  
     Deze parameter optimalisatie beperkt de hoeveelheid geheugen die wordt gebruikt voor het berekenen van de volgende stap en de richting. Wanneer u minder geheugen opgeeft, wordt training is sneller, maar minder nauwkeurig.  
  
6.  Voor **willekeurig getal seed**, typt u een geheel getal. Definieert een seed-waarde is van belang als u de resultaten worden gereproduceerd via meerdere uitvoeringen van het hetzelfde experiment wilt.  
  
  
8. Een gegevensset met tags toevoegen aan het experiment en verbinding maken met een van de [trainingsmodules](module-reference.md).  
  
    -   Als u instelt **trainer aanmaakmodus** naar **één Parameter**, gebruikt u de [Train Model](./train-model.md) module.  
  
9. Voer het experiment uit.  
  
## <a name="results"></a>Resultaten

Na de training is voltooid:

+ U ziet een overzicht van de parameters van het model, samen met de functie gewichten geleerd van training, met de rechtermuisknop op de uitvoer van [Train Model](./train-model.md) en selecteer **Visualize**.   
  
+ Gebruiken om voorspellingen te maken van nieuwe gegevens, het getrainde model en de nieuwe gegevens als invoer voor de [Score Model](./score-model.md) module. 


## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 
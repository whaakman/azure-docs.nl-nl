---
title: 'Neural Network regressie: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Informatie over het gebruik van de module Neural Network regressie in Azure Machine Learning-service te maken van een regressiemodel een aanpasbare neural network-algoritme met...
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: bc6a7505ab09e929e5add61eea687f871aedf242
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029308"
---
# <a name="neural-network-regression-module"></a>Neurale netwerk regressie-module

*Hiermee maakt u een regressiemodel met behulp van een neural network-algoritme*  
  
 Categorie: Machine Learning / Model initialiseren / regressie
  
## <a name="module-overview"></a>Module-overzicht  

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Deze module gebruiken voor het maken van een regressiemodel met behulp van een aanpasbare neural network-algoritme.
  
 Hoewel neurale netwerken voor deep learning en modelleren van complexe problemen, zoals beeldherkenning alom bekend zijn, zijn eenvoudig worden aangepast aan de regressie problemen. Elke objectklasse statistische modellen kan worden aangeduid als een neural network als ze adaptieve gewicht en schatting van niet-lineaire functies van de invoer maken kunnen. Neurale netwerk regressie is dus geschikt voor problemen waar een meer traditionele regressiemodel een oplossing kan niet aanpassen.
  
 Regressie neurale netwerk is een methode voor het leren met supervisie, en daarom vereist een *gelabeld gegevensset*, waaronder een labelkolom. Omdat een regressiemodel een numerieke waarde voorspelt, is de labelkolom moet een numeriek gegevenstype.  
  
 U kunt het model te trainen door te geven van het model en de label-gegevensset als invoer voor [Train Model](./train-model.md). Het getrainde model kan vervolgens worden gebruikt om waarden voor de nieuwe invoer voorbeelden te voorspellen.  
  
## <a name="configure-neural-network-regression"></a>Neural Network regressie configureren 

Neurale netwerken kunnen uitgebreid worden aangepast. Deze sectie beschrijft het maken van een model met twee methoden:
  
+ [Maken van een neural network-model met behulp van de standaard-architectuur](#bkmk_DefaultArchitecture)  
  
    Als u akkoord gaat met de standaard neural network-architectuur, gebruikt u de **eigenschappen** deelvenster parameters waarmee het gedrag van het neurale netwerk, zoals het aantal knooppunten in de verborgen laag, leertempo en normalisering in te stellen.

    Begin hier als u niet bekend bent met neurale netwerken. De module ondersteuning biedt voor veel aanpassingen, evenals model afstemmen, zonder uitgebreide kennis van neurale netwerken. 

+ Een aangepaste architectuur voor een neuraal netwerk definiëren 

    Gebruik deze optie als u wilt toevoegen van extra verborgen lagen, of de netwerk-architectuur, de verbindingen en activering van de functies volledig kunt aanpassen.
    
    Deze optie wordt aanbevolen als u al enigszins bekend met neurale netwerken bent. U de Net #-taal gebruiken voor het definiëren van de netwerkarchitectuur.  

##  <a name="bkmk_DefaultArchitecture"></a> Maken van een neural network-model met behulp van de standaard-architectuur
  
1.  Toevoegen de **Neural Network regressie** module aan uw experiment in de interface. U vindt deze module onder **Machine Learning**, **initialiseren**, in de **regressie** categorie. 
  
2. Aangeven hoe u het model te trainen, door in te stellen de **trainer aanmaakmodus** optie.  
  
    -   **Eén Parameter**: Selecteer deze optie als u al weet hoe u wilt configureren van het model.  

3.  In **verborgen laag specificatie**, selecteer **volledig verbonden geval**. Deze optie maakt u een model met behulp van de standaard neural network-architectuur, waarbij voor een neuraal netwerk regressiemodel, is deze kenmerken:  
  
    + Het netwerk is exact één verborgen laag.
    + De uitvoer-laag is volledig wordt verbonden met de verborgen laag en de verborgen laag volledig wordt verbonden met de invoer-laag.
    + Het aantal knooppunten in de verborgen laag kan worden ingesteld door de gebruiker (de standaardwaarde is 100).  
  
    Omdat het aantal knooppunten in de invoer-laag wordt bepaald door het aantal functies in de trainingsgegevens, in een regressiemodel kunnen er slechts één knooppunt in de uitvoer-laag.  
  
4. Voor **aantal verborgen knooppunten**, typt u het nummer van verborgen knooppunten. De standaardwaarde is een verborgen laag met 100 knooppunten. (Deze optie is niet beschikbaar als u een aangepaste architectuur met Net # definieert.)
  
5.  Voor **leertempo**, typ een waarde die bepaalt de stap die bij elke herhaling, voordat u correctie worden genomen. Een grotere waarde op voor het leren van tarief kan leiden tot het model te convergeren sneller, maar het lokale minima kunt overschrijding.

6.  Voor **aantal iteraties learning**, geef het maximum aantal keren dat de algoritme de trainingscases verwerkt.

7.  Voor ** de initiële leerperiode gewicht diameter, typ een waarde die bepaalt het gewicht van het knooppunt aan het begin van het leerproces.

8.  Voor **het momentum**, typt u een waarde om toe te passen tijdens learning als een gewicht op knooppunten uit de vorige herhalingen.

10. Selecteer de optie **voorbeelden in willekeurige volgorde**, de volgorde van de gevallen tussen iteraties wijzigen. Als u deze optie uitschakelt, worden aanvragen verwerkt in dezelfde volgorde telkens wanneer die u het experiment uitvoert.
  
11. Voor **willekeurig getal seed**, kunt u desgewenst een waarde om te gebruiken als de seed typen. Een seed op te geven is waarde handig als u ervoor wilt zorgen herhaalbaarheid voor het hetzelfde experiment uitvoert.
  
13. Verbinding maken met een gegevensset training en een van de [trainingsmodules](module-reference.md): 
  
    -   Als u instelt **trainer aanmaakmodus** naar **één Parameter**, gebruikt u [Train Model](./train-model.md).  
  
   
14. Voer het experiment uit.  

## <a name="results"></a>Resultaten

Na de training is voltooid:

+ Voor een overzicht van de parameters van het model, samen met de functie gewichten geleerd van training en andere parameters van het neurale netwerk, met de rechtermuisknop op de uitvoer van [Train Model](./train-model.md), en selecteer **Visualize**.  

+ Om een momentopname van het getrainde model hebt opgeslagen, met de rechtermuisknop op de **Trained model** uitvoer en selecteer **opslaan als getrainde Model**. Dit model wordt niet bijgewerkt op het hetzelfde experiment opeenvolgende is uitgevoerd.


## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 
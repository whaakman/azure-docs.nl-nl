---
title: 'Two-Class Boosted-beslisboom: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Informatie over het gebruik van de module Two-Class Boosted beslissingsstructuur in Azure Machine Learning-service te maken van een machine learning-model dat is gebaseerd op de boosted decision trees-algoritme.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 09ea530cac5bdbd62208f134177e5ceaccb545e2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65027943"
---
# <a name="two-class-boosted-decision-tree-module"></a>Two-Class Boosted beslissingsstructuur module

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Deze module gebruiken voor het maken van een machine learning-model dat is gebaseerd op de boosted decision trees-algoritme. 

Een boosted-beslisboom is een methode waarbij de structuur van de tweede voor de fouten van de structuur van de eerste corrigeert learning ensembles de structuur van de derde corrigeert fouten van de eerste en tweede structuren, enzovoort.  Voorspellingen zijn gebaseerd op de hele ensembles structuren samen waarmee de voorspelling.
  
Bij het correct is geconfigureerd, zijn boosted decision trees over het algemeen de eenvoudigste methode waarmee om de beste prestaties op een groot aantal machine learning-taken. Maar ze zijn ook een van de cursisten meer geheugenintensieve en de huidige implementatie bevat alles wat in het geheugen. Daarom een boosted decision tree model mogelijk niet voor het verwerken van de grote gegevenssets die bepaalde lineaire cursisten kunnen verwerken.

## <a name="how-to-configure"></a>Configureren

Deze module wordt een ongetrainde classificeringsmodel gemaakt. Omdat de classificatie is een methode voor leren met supervisie, met het trainen van het model, moet u een *gelabeld gegevensset* die de kolom van een label met een waarde voor alle rijen bevat.

U kunt dit type van het gebruik van het model trainen [Train Model](././train-model.md). 

1.  In Azure Machine Learning, voegt de **beslissingsstructuur heeft de Klantenopbrengst** module naar het experimentcanvas.
  
2.  Geef op hoe u het model te trainen, door in te stellen de **trainer aanmaakmodus** optie.
  
    + **Eén Parameter**: Als u hoe u wilt configureren van het model weet, kunt u een specifieke set waarden opgeven als argumenten.
  
  
3.  Voor **Bladeren per structuur het maximale aantal**, het maximum aantal terminal knooppunten (bladeren) dat kan worden gemaakt in een boomstructuur geven.
  
     Door deze waarde, u mogelijk Verhoog de grootte van de structuur en betere nauwkeurigheid, indien overfitting en meer tijd voor trainingen.
  
4.  Voor **minimumaantal steekproeven per leaf-knooppunt**, geeft het aantal aanvragen dat is vereist voor het maken van een terminal (leaf) van de knooppunten in een boomstructuur.  
  
     Door deze waarde, verhoogt u de drempelwaarde voor het maken van nieuwe regels. Bijvoorbeeld, met de standaardwaarde van 1, zelfs op een enkel geval kan leiden tot een nieuwe regel moet worden gemaakt. Als u de waarde 5 verhoogt, wordt de trainingsgegevens hoeft te bevatten ten minste vijf aanvragen die voldoen aan de dezelfde voorwaarden.
  
5.  Voor **Learning tarief**, typ een getal tussen 0 en 1 die de Stapgrootte van de bij het leren definieert.  
  
     Het leertempo bepaalt hoe snel of traag de cursist convergeert wel op de optimale oplossing ligt. Als de Stapgrootte van de te groot is is, kunt u de optimale oplossing overschrijding. Als de stap te klein is, duurt training langer om te convergeren op de beste oplossing.
  
6.  Voor **aantal structuren gebouwd**, geeft het totale aantal beslissingsstructuren om in de ensembles te maken. Als u meer beslisbomen maakt, krijgt u mogelijk betere dekking, maar trainingstijd wordt verhoogd.
  
     Deze waarde bepaalt ook het aantal structuren weergegeven bij het visualiseren van het getrainde model. Als u wilt bekijken of één tree afdrukken, moet u de waarde ingesteld op 1. Echter, als u dit doet, slechts één structuur wordt gemaakt (de structuur met de eerste set parameters) en er is geen verdere pogingen worden uitgevoerd.
  
7.  Voor **willekeurig getal seed**, (optioneel) Typ een niet-negatief geheel getal te gebruiken als de random seed-waarde. Een seed opgeeft, zorgt u ervoor reproduceerbaarheid voor uitvoert met dezelfde gegevens en parameters.  
  
     De random seed is standaard ingesteld op 0, wat betekent dat de eerste seed-waarde wordt opgehaald uit de systeemklok.  Opeenvolgende wordt uitgevoerd met behulp van een willekeurige basis kunnen verschillende resultaten hebben.
  

9. Het model te trainen.
  
    + Als u instelt **trainer aanmaakmodus** naar **één Parameter**, verbinding maken met een gegevensset met tags en de [Train Model](./train-model.md) module.  
  
   
## <a name="results"></a>Resultaten

Nadat het trainen van het model is voltooid, met de rechtermuisknop op de uitvoer van [Train Model](./train-model.md) om de resultaten weer te geven:

+ De structuur die is gemaakt op elke herhaling Selecteer **Visualize**. 
+ Als u wilt inzoomen op de splitsingen en de regels voor elk knooppunt ziet, klikt u op elke structuur.


## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 
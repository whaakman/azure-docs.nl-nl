---
title: 'Train Model Clustering: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de module Train Model voor clusters in Azure Machine Learning-service clustering modellen kunt trainen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/06/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 41cdec1d7f1c3932b17da6f9b1de518071f3f542
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65028078"
---
# <a name="train-clustering-model"></a>Clustermodel trainen

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module om een clustering-model te trainen.

De module heeft een ongetrainde clustering-model dat u al hebt geconfigureerd met behulp van de [K-Means Clustering](k-means-clustering.md) -module, en traint het model met behulp van een set met het label of niet-gelabelde gegevens. De module maakt zowel een getraind model die u voor voorspelling en een set van de cluster-toewijzingen voor elke aanvraag in de trainingsgegevens gebruiken kunt.

> [!NOTE]
> Een clustering model grootteprofiel worden getraind met behulp van de [Train Model](train-model.md) module waarmee de algemene module voor het trainen van machine learning-modellen. Dat komt doordat [Train Model](train-model.md) werkt alleen met algoritmen voor leren met supervisie. K-means en andere clusters algoritmen kunnen zonder supervisie learning, wat betekent dat het algoritme van niet-gelabelde gegevens leren kunt.  
  
## <a name="how-to-use-train-clustering-model"></a>Het gebruik van de trein Clustering-Model  
  
1.  Voeg de **Clustering Train-Model** module aan uw experiment in Studio. U vindt de module onder **Machine Learning-Modules**, in de **Train** categorie.  
  
2. Voeg de [K-Means Clustering](k-means-clustering.md) module of een andere aangepaste module die u maakt een compatibel clustering model en stel de parameters van de clustering-model.  
    
3.  Een gegevensset training koppelen aan de rechter invoerpoort van **Clustering Train-Model**.
  
5.  In **kolomset**, selecteert u de kolommen uit de gegevensset te gebruiken bij het bouwen van clusters. Zorg ervoor dat u de kolommen selecteren die functies van een goede: bijvoorbeeld Vermijd het gebruik van id's of andere kolommen met unieke waarden of de kolommen die dezelfde waarden hebben.

    Als een label beschikbaar is, kunt u gebruiken als een functie of laat u deze uit.  
  
6. Selecteer de optie **controleren toevoegen of schakel het selectievakje voor alleen resultaat**, als u wilt uitvoeren van de trainingsgegevens samen met het nieuwe cluster label.

    Als u deze optie uitschakelt, worden alleen de toewijzingen van het cluster uitgevoerd. 

7. Voer het experiment uit en klikt u op de **Clustering Train-Model** -module en selecteer **geselecteerde uitvoeren**.  
  
### <a name="results"></a>Resultaten

Na de training is voltooid:


+  Als u wilt weergeven van de waarden in de gegevensset, met de rechtermuisknop op de module, selecteert u **leiden tot gegevenssets**, en klikt u op **Visualize**.

+ Als u wilt opslaan van het getrainde model later opnieuw kunt gebruiken, met de rechtermuisknop op de module, selecteert u **Trained model**, en klikt u op **opslaan als getrainde Model**.

+ Gebruiken voor het genereren van scores uit het model, [gegevens toewijzen aan Clusters](assign-data-to-clusters.md).



## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 
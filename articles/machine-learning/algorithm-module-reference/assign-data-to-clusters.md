---
title: 'Gegevens toewijzen aan Cluster: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de gegevens toewijzen aan de Cluster-module in Azure Machine Learning-service om clustering-model te beoordelen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/06/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 1c2d2a02ecfb617551dd9174b87f363d57b151a8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65467201"
---
# <a name="module-assign-data-to-clusters"></a>Module: Gegevens toewijzen aan Clusters

In dit artikel wordt beschreven hoe u de *gegevens toewijzen aan Clusters* module in de visuele interface van Azure Machine Learning. De module genereert voorspellingen via clustering-model dat is getraind met de *K-means clustering* algoritme.

De gegevens toewijzen aan Clusters module retourneert een gegevensset met de waarschijnlijke toewijzingen voor elke nieuwe gegevenspunt. 


## <a name="how-to-use-assign-data-to-clusters"></a>Het gebruik van gegevens met Clusters toewijzen
  
1. Zoek in de visuele interface van Azure Machine Learning, een eerder getrainde clustering-model. U kunt maken en trainen clustering-model met behulp van een van de volgende methoden:  
  
    - De K-means clustering-algoritme configureren met behulp van de [K-Means Clustering](k-means-clustering.md) module en trainen het model met behulp van een gegevensset en de module Train Clustering-Model (dit artikel).  
  
    - U kunt ook toevoegen met een bestaande getrainde clustering-model van de **opgeslagen modellen** groep in uw werkruimte.

2. Het getrainde model koppelen aan de linkerinvoerpoort van **gegevens toewijzen aan Clusters**.  

3. Koppel een nieuwe gegevensset als invoer. 

   In deze gegevensset zijn labels optioneel. Clustering is over het algemeen een methode zonder supervisie learning. U wordt niet aan de categorieën van tevoren weten. De ingevoerde kolommen moeten wel gelijk zijn aan de kolommen die werden gebruikt in training die de clustering-model of een fout optreedt.

    > [!TIP]
    > Als u wilt verkleinen het aantal kolommen die zijn geschreven met de interface van de voorspellingen van het cluster, gebruikt u [kolommen selecteren in de gegevensset](select-columns-in-dataset.md), en een subset van de kolommen selecteren. 
    
4. Laat de **controleren toevoegen of schakel het selectievakje voor alleen resultaat** selectievakje is ingeschakeld als u wilt dat de resultaten bevatten de volledige invoergegevensset, met inbegrip van een kolom waarin de resultaten (cluster-toewijzingen).
  
    Als u dit selectievakje uitschakelt, worden alleen de resultaten worden geretourneerd. Deze optie kan nuttig zijn bij het maken van voorspellingen als onderdeel van een webservice.
  
5.  Voer het experiment uit.  
  
### <a name="results"></a>Resultaten

+  Als u wilt weergeven van de waarden in de gegevensset, met de rechtermuisknop op de module, selecteert u **leiden tot gegevenssets**, en selecteer vervolgens **Visualize**.


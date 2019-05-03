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
ms.openlocfilehash: b370c6ef5be311ed9c8df2737fa1b01144d61011
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028768"
---
# <a name="assign-data-to-clusters"></a>Gegevens toewijzen aan Clusters

In dit artikel wordt beschreven hoe u de [gegevens toewijzen aan Clusters](assign-data-to-clusters.md) visuele interface, voor het genereren van voorspellingen met behulp van een clustering-model is getraind met behulp van de K-Means clustering-algoritme van de module.

De module retourneert een gegevensset met de waarschijnlijke toewijzingen voor elke nieuwe gegevenspunt. 


## <a name="how-to-use-assign-data-to-clusters"></a>Het gebruik van gegevens met Clusters toewijzen
  
1.  Zoek een eerder getrainde clustering-model in de visuele interface. U kunt maken en trainen clustering-model met behulp van een van deze methoden:  
  
    - Configureren het K-means algoritme met behulp van de [K-Means Clustering](k-means-clustering.md) -module en klikt u vervolgens trainen het model met behulp van een gegevensset en de [Clustering-Model trainen](train-clustering-model.md) module.  
  
  
    U kunt ook toevoegen met een bestaande getrainde clustering-model van de **opgeslagen modellen** groep in uw werkruimte.

2. Het getrainde model koppelen aan de linkerinvoerpoort van [gegevens toewijzen aan Clusters](assign-data-to-clusters.md).  

3. Koppel een nieuwe gegevensset als invoer. In deze gegevensset zijn labels optioneel. Over het algemeen is clustering een methode zonder supervisie learning zodat het is niet waarschijnlijk dat u categorieën vooraf weet.

    De ingevoerde kolommen moeten wel gelijk zijn aan de kolommen die werden gebruikt in training die de clustering-model of een fout optreedt.

    > [!TIP]
    > Als u wilt verkleinen het aantal kolommen uitvoer van voorspellingen van de cluster, gebruikt u [Select Columns in Dataset](select-columns-in-dataset.md), en een subset van de kolommen selecteren. 
    
4. Laat de optie **controleren toevoegen of schakel het selectievakje voor alleen resultaat** ingeschakeld als u wilt dat de resultaten bevatten de volledige invoergegevensset, samen met een kolom die de resultaten (cluster-toewijzingen) waarmee wordt aangegeven.
  
    Als u deze optie uitschakelt, krijgen je weer toegang alleen de resultaten. Dit kan nuttig zijn bij het maken van voorspellingen als onderdeel van een webservice.
  
5.  Voer het experiment uit.  
  
### <a name="results"></a>Resultaten

 
+  Als u wilt weergeven van de waarden in de gegevensset, met de rechtermuisknop op de module, selecteert u **leiden tot gegevenssets**, en klikt u op **Visualize**.


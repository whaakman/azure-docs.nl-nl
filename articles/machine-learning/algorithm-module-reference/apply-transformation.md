---
title: 'Transformatie toepassen: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Informatie over het gebruik van de module transformatie toepassen in Azure Machine Learning-service om te wijzigen van een invoergegevensset op basis van een eerder berekende transformatie.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 810f375642af49814049589cb83ad17fea578b13
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65028708"
---
# <a name="apply-transformation-module"></a>Module transformatie toepassen

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module om een invoergegevensset op basis van een eerder berekende transformatie.  
  
Bijvoorbeeld, als u z-scores gebruikt voor het normaliseren van uw trainingsgegevens met behulp van de **gegevens normaliseren** -module, u wilt de z-score-waarde die is berekend gebruiken voor training tijdens ook de scoring fase. In Azure Machine Learning, kunt u de methode normalisering opslaan als een transformatie, en klik vervolgens met behulp **transformatie toepassen** de z-score toepassen op de ingevoerde gegevens voordat de score.
  
Azure Machine Learning biedt ondersteuning voor het maken en vervolgens verschillende soorten aangepaste transformaties toe te passen. U wilt bijvoorbeeld, opslaan en opnieuw transformaties te gebruiken:  
  
- Verwijder of vervang de ontbrekende waarden, met behulp van **Clean Missing Data**
- Gegevens, met behulp van normaliseren **gegevens normaliseren**
  

## <a name="how-to-use-apply-transformation"></a>Het gebruik van de transformatie toepassen  
  
1. Voeg de **transformatie toepassen** module naar het experimentcanvas. U vindt deze module onder **Machine Learning**, in de **Score** categorie. 
  
2. Zoek naar een bestaande transformatie te gebruiken als invoer.  Eerder opgeslagen transformaties kunnen u vinden in de **transformeert** groep in het navigatiedeelvenster links.  
  
   
  
3. Verbinding maken met de gegevensset die u wilt transformeren. De gegevensset moet exact hetzelfde schema hebben (aantal kolommen, kolomnamen, gegevenstypen) als de gegevensset waarvoor de transformatie voor het eerst is ontworpen.  
  
4. Er zijn geen andere parameters moeten worden ingesteld omdat alle aanpassingen worden uitgevoerd bij het definiëren van de transformatie.  
  
5. Als u wilt een transformatie toepassen op de nieuwe gegevensset, moet u het experiment uitvoert.  

## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 
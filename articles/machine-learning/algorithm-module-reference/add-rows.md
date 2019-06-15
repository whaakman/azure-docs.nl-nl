---
title: 'Rijen toevoegen: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de module rijen toevoegen in Azure Machine Learning-service om twee gegevenssets samen te voegen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: ffd693ea3452ef48dc3e05e7bc4a6d3988a487b0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65028753"
---
# <a name="add-rows-module"></a>Rijen module toevoegen

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module om twee gegevenssets samen te voegen. In samengevoegde, worden de rijen van de tweede gegevensset toegevoegd aan het einde van de eerste gegevensset.  
  
Samengevoegde rijen is handig in scenario's zoals deze:  
  
+ Als u een reeks evaluatie statistieken hebt gegenereerd, en u wilt combineren in één tabel voor het melden van eenvoudiger.  
  
+ U hebt gewerkt met verschillende gegevenssets, en u wilt combineren de gegevenssets om een uiteindelijke gegevensset te maken.  

## <a name="how-to-use-add-rows"></a>Het gebruik van rijen toevoegen  

Als u wilt samenvoegen rijen van twee gegevenssets, moeten de rijen precies hetzelfde schema hebben. Dit betekent, hetzelfde aantal kolommen en hetzelfde type gegevens in de kolommen.

1.  Sleep de **rijen toevoegen** -module in uw experiment, kunt u deze vinden onder **gegevenstransformatie**, in de **bewerken** categorie.

2. De gegevenssets verbinden met de twee invoerpoorten. De gegevensset die u wilt toevoegen, moet zijn verbonden met de tweede (rechts)-poort. 
  
3.  Voer het experiment uit. Het aantal rijen in de uitvoergegevensset moet gelijk zijn aan de som van de rijen van beide invoergegevenssets.

    Als u dezelfde gegevensset aan zowel invoer van toevoegen de **rijen toevoegen** -module, de gegevensset wordt gedupliceerd. 

## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 
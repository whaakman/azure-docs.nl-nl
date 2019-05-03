---
title: 'Kolommen toevoegen: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de module kolommen toevoegen in Azure Machine Learning-service om twee gegevenssets samen te voegen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: f1e087e97007c6ba271651a9791c7c3b38a9b9b7
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029353"
---
# <a name="add-columns-module"></a>Kolommen module toevoegen

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module om twee gegevenssets samen te voegen. Combineert u alle kolommen uit de twee gegevenssets die u als invoer opgeeft voor het maken van een enkele gegevensset. Als u samenvoegen van meer dan twee gegevenssets wilt, gebruikt u meerdere exemplaren van **kolommen toevoegen**.



## <a name="how-to-configure-add-columns"></a>Het configureren van kolommen toevoegen
1. Toevoegen de **kolommen toevoegen** module naar het experimentcanvas.

2. Verbinding maken met de twee gegevenssets die u wilt samenvoegen. Als u combineren van meer dan twee gegevenssets wilt, u kunt keten van verschillende combinaties van **kolommen toevoegen**.

    - Het is mogelijk om twee kolommen met een ander aantal rijen te combineren. De uitvoergegevensset opgevuld met ontbrekende waarden voor elke rij in de bronkolom kleiner.

    - U kunt geen afzonderlijke kolommen toe te voegen. Alle kolommen uit elke gegevensset zijn samengevoegd wanneer u **kolommen toevoegen**. Als u toevoegen van slechts een subset van de kolommen wilt, Gebruik daarom Select Columns in Dataset om te maken van een gegevensset met de kolommen die u wilt.

3. Voer het experiment uit.

### <a name="results"></a>Resultaten
Nadat het experiment is uitgevoerd:

- Als u wilt zien van de eerste rijen van de nieuwe gegevensset, met de rechtermuisknop op de uitvoer van **kolommen toevoegen** en selecteer Visualize.

Het aantal kolommen in de nieuwe gegevensset is gelijk aan de som van de kolommen in beide invoergegevenssets.

Als er twee kolommen met dezelfde naam in de invoergegevenssets, wordt een numeriek achtervoegsel wordt toegevoegd aan de naam van de kolom. Bijvoorbeeld, als er twee exemplaren van een kolom met de naam TargetOutcome, in de linkerkolom zou worden hernoemd TargetOutcome_1 en in de rechterkolom bevindt TargetOutcome_2 gewijzigd.

## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 
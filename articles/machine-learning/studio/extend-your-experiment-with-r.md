---
title: Uw experiment uitbreiden met R | Microsoft Docs
description: Het uitbreiden van de functionaliteit van Azure Machine Learning Studio via de taal R met behulp van de module R-Script niet uitvoeren.
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 2c038a45-ba4d-42ea-9a88-e67391ef8c0a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: e9f11ec987a5dd71998f6b23399228554f1dcc11
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="extend-your-experiment-with-r"></a>Uw experiment uitbreiden met R
U kunt de functionaliteit van Azure Machine Learning Studio via de taal R uitbreiden met behulp van de [R-Script uitvoeren] [ execute-r-script] module.

Deze module accepteert meerdere invoergegevenssets en resulteert in een enkel gegevensset als uitvoer. Typ een R-script in de **R-Script** parameter van de [R-Script uitvoeren] [ execute-r-script] module.

U openen elke invoerpoort van de module met code ziet er als volgt:

    dataset1 <- maml.mapInputPort(1)

## <a name="listing-all-currently-installed-packages"></a>Aanbieding geïnstalleerde pakketten
De lijst met geïnstalleerde pakketten kunt wijzigen. Een lijst met geïnstalleerde pakketten vindt u in [R-pakketten wordt ondersteund door Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt741980.aspx).

U kunt de volledige, actuele lijst met geïnstalleerde pakketten ook opvragen door te voeren van de volgende code naar de [R-Script uitvoeren] [ execute-r-script] module:

    out <- data.frame(installed.packages(,,,fields="Description"))
    maml.mapOutputPort("out")

De lijst met pakketten nu verzonden naar de uitvoerpoort van de [R-Script uitvoeren] [ execute-r-script] module.
De pakketlijst wilt weergeven, zoals een module conversie verbinding [converteren naar CSV] [ convert-to-csv] aan de linkerkant uitvoer van de [R-Script uitvoeren] [ execute-r-script] module Voer het experiment uit, klik op de uitvoer van de conversie-module en selecteer **downloaden**. 

![Uitvoer van de module 'Niet converteren naar CSV' downloaden](./media/extend-your-experiment-with-r/download-package-list.png)


<!--
For convenience, here is the [current full list with version numbers in Excel format](http://az754797.vo.msecnd.net/docs/RPackages.xlsx).
-->

## <a name="importing-packages"></a>Importeren van pakketten
U kunt importeren pakketten die nog niet zijn geïnstalleerd met behulp van de volgende opdrachten in de [R-Script uitvoeren] [ execute-r-script] module:

    install.packages("src/my_favorite_package.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("my_favorite_package", lib.loc = ".", logical.return = TRUE, verbose = TRUE)

waar de `my_favorite_package.zip` -bestand bevat het pakket.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/

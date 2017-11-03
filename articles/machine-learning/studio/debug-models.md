---
title: Fouten opsporen in uw Model in Azure Machine Learning | Microsoft Docs
description: Fouten opsporen van fouten die wordt geproduceerd door Train Model en Score Model modules in Azure Machine Learning.
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 629dc45e-ac1e-4b7d-b120-08813dc448be
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bradsev;garye
ms.openlocfilehash: e6e9e1a3b30f84d634592581ea24fb308dcb478e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="debug-your-model-in-azure-machine-learning"></a>Fouten opsporen in uw model in Azure Machine Learning

Dit artikel wordt uitgelegd van de mogelijke redenen waarom een van de volgende twee storingen aangetroffen mogelijk bij het uitvoeren van een model:

* de [Train Model] [ train-model] module, treedt een fout 
* de [Score Model] [ score-model] module onjuiste resultaten oplevert 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="train-model-module-produces-an-error"></a>Module Train-Model, treedt een fout

![image1](./media/debug-models/train_model-1.png)

De [Train Model] [ train-model] Module verwacht twee invoeritems:

1. Het type van machine learning-model uit de verzameling van modellen die worden geleverd door het Azure Machine Learning.
2. De trainingsgegevens met een opgegeven labelkolom waarin de variabele te voorspellen (de andere kolommen wordt verondersteld dat functies).

Deze module kunt produceren van een fout in de volgende gevallen:

1. De kolom Label is onjuist opgegeven. Dit kan gebeuren als u meer dan één kolom is geselecteerd als het Label of een onjuiste kolomindex selecteert. Het tweede geval zou bijvoorbeeld van toepassing als een kolomindex 30 wordt gebruikt met een invoergegevensset die alleen 25 kolommen heeft.

2. De gegevensset bevat geen functie kolommen. Bijvoorbeeld, als de invoergegevensset slechts één kolom, die is gemarkeerd als de kolom Label heeft, zou er geen onderdelen waarmee het model bouwen. In dit geval de [Train Model] [ train-model] module, treedt een fout.

3. Oneindig bevat invoergegevensset (functies of Label) een waarde.

## <a name="score-model-module-produces-incorrect-results"></a>De Module score Model oplevert onjuiste resultaten

![image2](./media/debug-models/train_test-2.png)

In een typische experiment training/testen voor leren met supervisie de [Split Data] [ split] module verdeelt de oorspronkelijke gegevensset in twee delen: één onderdeel wordt gebruikt voor het model trainen en één onderdeel wordt gebruikt om te beoordelen hoe goed het getrainde model wordt uitgevoerd. Het getrainde model wordt vervolgens gebruikt voor de beoordeling van de testgegevens, waarna de resultaten worden geëvalueerd om te bepalen van de nauwkeurigheid van het model.

De [Score Model] [ score-model] module vereist twee invoeritems:

1. De uitvoer van een getraind model van de [Train Model] [ train-model] module.
2. Een score gegevensset die verschilt van de gegevensset die wordt gebruikt voor het model trainen.

Het is mogelijk dat zelfs als het experiment is geslaagd, de [Score Model] [ score-model] module onjuiste resultaten oplevert. Verschillende scenario's kunnen ertoe leiden dat dit mag gebeuren:

1. Als de opgegeven naam categorische is en een regressiemodel op de gegevens wordt getraind, onjuist uitvoer zou worden geproduceerd door de [Score Model] [ score-model] module. Dit is omdat regressie een continue antwoord-variabele vereist. In dit geval zou het geschikte gebruiken een classificatie-model zijn. 

2. Op dezelfde manier als een classificatie-model wordt getraind van een gegevensset met getallen met drijvende komma in de kolom Label, kan deze ongewenste resultaten opleveren. Dit is omdat classificatie vereist een discrete antwoord-variabele, waarmee alleen waarden die variëren ten opzichte van een set eindig en meestal erg klein van klassen.

3. Als de score gegevensset niet alle functies die worden gebruikt bevat voor het model trainen de [Score Model] [ score-model] , treedt een fout.

4. Als een rij in de score gegevensset een ontbrekende waarde of een oneindige waarde voor een van de functies van bevat de [Score Model] [ score-model] wordt geen uitvoer die overeenkomt met die rij wordt geproduceerd.

5. De [Score Model] [ score-model] identiek outputs voor alle rijen in de gegevensset scoreprofiel kan veroorzaken. Dit kan bijvoorbeeld gebeuren tijdens een poging de classificatie met behulp van de beslissing Forests als u het minimale aantal steekproeven per leaf-knooppunt moet meer zijn dan het aantal voorbeelden van training beschikbaar hebt gekozen.

<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/


---
title: Azure Batch AI-service - AI-training | Microsoft Docs
description: Meer informatie over het gebruik van de beheerde Azure Batch AI-service voor het trainen van kunstmatige intelligentie (AI) en andere Machine Learning-modellen op GPU- en CPU-clusters.
services: batch-ai
documentationcenter: ''
author: alexsuttonms
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 08/01/2018
ms.author: danlep
ROBOTS: NOINDEX
ms.openlocfilehash: 37f1bf5dc20d097f7f5f560e3bf1fdd25dc38045
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53408048"
---
# <a name="what-is-azure-batch-ai"></a>Wat is Azure Batch AI?

[!INCLUDE [batch-ai-retiring](../../includes/batch-ai-retiring.md)]

Azure Batch AI is een beheerde service waarmee gegevenswetenschappers en AI-onderzoekers machine learning en AI-modellen op schaal trainen en testen in Azure, zonder dat er een complexe infrastructuur hoeft te worden beheerd. U beschrijft de compute-resources, de taken die u wilt uitvoeren en waar de in- en uitvoer van het model moet worden opgeslagen, en Batch AI zorgt voor de rest.

U kunt Batch AI zelfstandig gebruiken voor het trainen van het model of Batch AI uitvoeren als onderdeel van een grotere ontwikkelingswerkstroom:

* Batch AI kan zelfstandig worden gebruikt voor het trainen, testen, en batch scoren van machine learning- en AI-modellen in geclusterde [GPU's](../virtual-machines/linux/sizes-gpu.md) of CPU's. 

* Batch AI kan ook worden uitgevoerd op een cluster in een werkstroom met [Azure Machine Learning](../machine-learning/service/overview-what-is-azure-ml.md) of andere [Azure AI Platform-hulpprogramma's](https://azure.microsoft.com/overview/ai-platform/). Azure ML bevat uitgebreide functies voor gegevensvoorbereiding, experimenten en taakgeschiedenis. Met Azure ML kunt u ook een getraind model verpakken in een container, en een model voor inferentie of naar IoT-apparaten implementeren.  

## <a name="train-machine-learning-and-ai-models"></a>Machine learning- en AI-modellen trainen

U kunt Batch AI gebruiken voor het trainen van machine learning-modellen, neurale netwerken (deep learning) en andere AI-methoden. Batch AI heeft ingebouwde ondersteuning voor populaire open-source AI-bibliotheken en frameworks, waaronder [TensorFlow](https://github.com/tensorflow/tensorflow), [PyTorch](https://github.com/pytorch/pytorch), [Chainer](https://github.com/chainer/chainer) en [Microsoft Cognitive Toolkit (CNTK)](https://github.com/Microsoft/CNTK).

Nadat u het probleemgebied hebt geïdentificeerd en uw gegevens hebt voorbereid, kunt u interactief werken met Batch AI om modelideeën te testen. Wanneer u klaar bent om op schaal te experimenteren, start u taken op meerdere GPU's met MPI of andere communicatiebibliotheken, en voert u meer experimenten parallel uit.

Batch AI biedt verschillende mogelijkheden om te helpen bij het trainen van modellen op schaal. Bijvoorbeeld: 

|  |  |
|---------|---------|
| **Training distribueren**<br/>![Gedistribueerde training](./media/overview/distributed-training.png)  | Training opschalen voor één taak op veel GPU's die verbonden zijn over netwerk, om grotere netwerken te trainen met grote hoeveelheden gegevens.|
| **Experiment**<br/>![Experimenten](./media/overview/experimentation.png) | Training uitschalen met meerdere taken. Parametersweeps uitvoeren om nieuwe ideeën te testen of hyperparameters afstemmen om de nauwkeurigheid en prestaties te optimaliseren. |
| **Parallel uitvoeren**![Parallelle uitvoering](./media/overview/parallel-execution.png) | Meerdere modellen tegelijk trainen of scoren via parallelle uitvoering op verschillende servers om de taken sneller te voltooien.|

Nadat u een model hebt getraind, kunt u Batch AI gebruiken om het model te testen, als u dat nog niet hebt gedaan via uw trainingsscript, of om batchscoring uit te voeren.

## <a name="how-it-works"></a>Hoe werkt het?

Gebruik Batch AI-SDK's, opdrachtregelscripts of de Azure-portal om rekenresources te beheren en AI-trainingstaken en testen te plannen: 

* **VM-clusters inrichten en schalen** - Kies het aantal knooppunten (VM's) en selecteer een VM met GPU of een andere VM-grootte in overeenstemming met uw trainingsbehoeften. Schaal het aantal knooppunten automatisch of handmatig omhoog of omlaag zodat u alleen resources gebruikt wanneer dat nodig is. 

* **Afhankelijkheden en containers beheren** - Standaard worden Batch AI-clusters uitgevoerd onder Linux VM-installatiekopieën met afhankelijkheden die vooraf zijn geïnstalleerd om op containers gebaseerde trainingsframeworks uit te voeren op GPU's of CPU's. Voeg aangepaste installatiekopieën toe of voer opstartscripts uit voor aanvullende configuratieopties.

* **Gegevens distribueren**: kies een of meer opslagopties voor het beheren van invoergegevens, scripts en taakuitvoer: Azure Files, Azure Blob Storage of een beheerde NFS-server. Batch AI biedt ook ondersteuning voor aangepaste opslagoplossingen, zoals krachtige parallelle bestandssystemen. Koppel opslagbestandssystemen aan de clusterknooppunten en taakcontainers met behulp van eenvoudige configuratiebestanden.

* **Taken plannen** - Plaats taken in wachtrijen op basis van prioriteit om clusterbronnen te delen en te profiteren van VM's met lage prioriteit en gereserveerde instanties.

* **Fouten afhandelen** - Bewaak de status van taken en start taken opnieuw als er problemen met VM's optreden tijdens langlopende taken.

* **Resultaten verzamelen** - Krijg eenvoudig toegang tot uitvoerlogboeken, Stdout, Stderr en getrainde modellen. Configureer uw Batch AI-taken zo dat de uitvoer rechtstreeks naar de gekoppelde opslag wordt gepusht.

Als Azure-service ondersteunt Batch AI algemene hulpprogramma's, zoals op rollen gebaseerd toegangsbeheer (RBAC) en virtuele netwerken van Azure voor beveiliging.  

## <a name="next-steps"></a>Volgende stappen

* Lees meer over de [Batch AI-resources](resource-concepts.md) voor het trainen van een machine learning- of AI-model.

* Ga aan de slag met het [trainen van een deep learning-voorbeeldmodel](quickstart-tensorflow-training-cli.md) met Batch AI.

* Bekijk voorbeelden van [trainingsrecepten](https://github.com/Azure/BatchAI/blob/master/recipes) voor populaire AI-frameworks.

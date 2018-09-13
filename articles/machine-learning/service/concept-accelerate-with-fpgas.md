---
title: Wat is een FPGA? – Project Brainwave – Azure Machine Learning
description: Informatie over het versnellen van modellen en deep neural networks met FPGA's.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 05/31/2018
ms.openlocfilehash: 5c8efcbb5f2e9014c9edabfc2aee37c8c29ea0b4
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35643649"
---
# <a name="what-is-fpga-and-project-brainwave"></a>Wat is FPGA en Project Brainwave?

Dit artikel bevat een inleiding tot veld-programmable gate arrays (FPGA) en hoe FPGA is geïntegreerd met Azure machine learning voor realtime AI.

## <a name="fpgas-vs-cpu-gpu-and-asic"></a>FPGA's vs. CPU, GPU en ASIC

FPGA's bevatten een matrix van programmeerbare logische blokken, en een hiërarchie van herconfigureerbare koppelt waarmee de blokken op verschillende manieren worden geconfigureerd nadat de productie.

FPGA's bieden een combinatie van programmeren en prestaties vergelijken met andere chips:

![Vergelijking van Azure Machine Learning FPGA](./media/concept-accelerate-with-fpgas/azure-machine-learning-fpga-comparison.png)

- **Central processing units (CPU)** voor algemeen gebruik-processors zijn. CPU-prestaties is niet ideaal voor grafische afbeeldingen en video verwerking.
- **Grafische verwerkingseenheden (GPU)** bieden een parallelle verwerking en zijn een populaire keuze voor AI-berekeningen. De parallelle verwerking met GPU's leiden tot snellere beeldweergave dan CPU's.
- **Toepassingsspecifieke geïntegreerde circuits (ASIC)**, zoals Google TensorFlow-Processor-eenheden, worden aangepaste circuits. Hoewel deze chips de hoogste efficiëntie bieden, zijn ASICs niet-flexibele.
- **FPGA's**, zoals die beschikbaar zijn op Azure, de prestaties dicht bij ASIC bieden, maar bieden de flexibiliteit om het later opnieuw worden geconfigureerd.

FPGA's zijn nu in elke nieuwe Azure-server. Microsoft maakt al gebruik van FPGA's voor Bing zoeken-volgorde, deep neural network (DNN)-evaluatie en software-netwerken (SDN)-versnelling gedefinieerde. Deze implementaties FPGA Verminder de latentie bij het vrijmaken van CPU's voor andere taken.

## <a name="project-brainwave-on-azure"></a>Project Brainwave op Azure

Project Brainwave is een hardwarearchitectuur die speciaal is ontworpen op basis van Intel FPGA apparaten en gebruikt voor het versnellen van realtime AI-berekeningen. Met deze architectuur betaalbare FPGA-functionaliteit kan zo snel mogelijk en met een lagere latentie een getrainde neurale netwerk worden uitgevoerd. Project Brainwave kunt vooraf getrainde dnn's parallel op FPGA's uit de service te schalen.

- Prestaties

    FPGA's maken het mogelijk is om te realiseren met lage latentie voor realtime inferentietaken aanvragen. Batchverwerking betekent dat een aanvraag in kleinere delen worden opgesplitst en voeren ze naar een processor voor het gebruik van hardware te verbeteren. Batchverwerking is niet efficiënt en kan leiden tot latentie. Brainwave vereist geen batchverwerking; Daarom is de latentie 10 keer minder ten opzichte van CPU en GPU.

- Flexibiliteit

    FPGA's kunnen worden geconfigureerd voor verschillende soorten machine learning-modellen. Deze flexibiliteit maakt het gemakkelijker om te versnellen van de toepassingen op basis van de meest optimale numerieke precisie en model van het geheugen wordt gebruikt.

    Regelmatig nieuwe machine learning-technieken worden ontwikkeld en ontwerp van de hardwareconfiguratie van Project Brainwave is ook zich snel ontwikkelende. Aangezien FPGA's herconfigureerbare, is het mogelijk te actueel blijven met de vereisten van de snel veranderende AI-algoritmen.

- Schalen

    Microsoft Azure is's werelds grootste investeringen in de cloud in FPGA's. U kunt Brainwave uitvoeren op de infrastructuur van Azure schaal.

Een voorbeeld van Project Brainwave geïntegreerd met Azure Machine Learning is momenteel beschikbaar. En een beperkte Preview-versie is ook beschikbaar voor het Project Brainwave brengen naar de rand, zodat u van die computers snelheid in uw eigen bedrijven en faciliteiten profiteren kunt.

In de huidige Preview-versie is de Brainwave beperkt tot de TensorFlow-implementatie en de ResNet50 gebaseerde neurale netwerken op Intel FPGA hardware voor classificatie van afbeeldingen en herkenning. Er zijn plannen voor de ondersteuning van meer modellen van de galerie en andere frameworks.

De volgende scenario's gebruiken FPGA op Project Brainwave architectuur:

- Geautomatiseerd systeem voor netwerkinspectie optische. Zie [Real-time AI: Microsoft kondigt de Preview-versie van het Project Brainwave](https://blogs.microsoft.com/ai/build-2018-project-brainwave/).
- Land cover-toewijzing. Zie [FPGA's voor Deep Learning Deductie gebruiken om uit te voeren Land Cover toewijzing op Terabytes aan Luchtfoto installatiekopieën](https://blogs.technet.microsoft.com/machinelearning/2018/05/29/how-to-use-fpgas-for-deep-learning-inference-to-perform-land-cover-mapping-on-terabytes-of-aerial-images/).

## <a name="how-to-deploy-a-web-service-to-an-fpga"></a>Hoe kunt u een webservice implementeert in een FPGA?

De stroom op hoog niveau voor het maken van een image recognition-service in Azure met behulp van ResNet50 als een featurizer is als volgt:

1. ResNet50 is in Brainwave al geïmplementeerd. U kunt andere grafieken (datum invoer, classificatie, enzovoort) met TensorFlow bouwen, en definieert een pijplijn (invoer -> parametriseer -> classificeren) met behulp van een json-bestand van de service-definitie. Comprimeer de definitie en grafieken in een zip-bestand en upload het zip-bestand naar Azure Blob-opslag.
2. Registreer het model met behulp van Azure ML-Model Management-API met het zip-bestand in de Blob-opslag.
3. De service implementeren met het geregistreerde model met behulp van Azure ML-Model Management-API.

Meer informatie over dit proces in het artikel [implementeert een model als een webservice op een FPGA met Azure Machine Learning](how-to-deploy-fpga-web-service.md).


## <a name="start-using-fpga"></a>FPGA gebruiken

Azure Machine Learning Hardware versnelde modellen kunt u getrainde DNN modellen implementeren op FPGA's in de Azure-cloud. Als u wilt beginnen, Zie:

- [Een model implementeren als een webservice op een FPGA](how-to-deploy-fpga-web-service.md)
- [Microsoft Azure Machine Learning-Hardware versnelde modellen die worden aangestuurd door Project Brainwave](https://github.com/azure/aml-real-time-ai).

## <a name="next-steps"></a>Volgende stappen

[Een model implementeren als een webservice op een FPGA](how-to-deploy-fpga-web-service.md)

[Informatie over het installeren van de FPGA SDK](reference-fpga-package-overview.md)

[Zeer grootschalige hardware: ML op schaal boven op Azure + FPGA: Build 2018 (video)](https://www.youtube.com/watch?v=BMgQAHIx2eY)

[In de Microsoft FPGA gebaseerde instelbare cloud (video)](https://channel9.msdn.com/Events/Build/2017/B8063)

[Microsoft onthult Project Brainwave voor realtime AI](https://www.microsoft.com/research/blog/microsoft-unveils-project-brainwave/)
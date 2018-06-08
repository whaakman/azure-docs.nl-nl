---
title: Wat is er een FPGA? – Project Brainwave – Azure Machine Learning
description: Informatie over het versnellen van modellen en diep neural netwerken met FPGAs.
services: machine-learning
ms.service: machine-learning
ms.component: service
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 05/31/2018
ms.openlocfilehash: 18b0ee143443ecf128bdfe57adb61cc22f95db87
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34832581"
---
# <a name="what-is-fpga-and-project-brainwave"></a>Wat is FPGA en Project Brainwave?

Dit artikel bevat een inleiding tot veld programmeerbare gate matrices (FPGA) en hoe FPGA is geïntegreerd met Azure machine learning voor het bieden van realtime AI.

## <a name="fpgas-vs-cpu-gpu-and-asic"></a>Vs FPGAs. CPU en GPU ASIC

FPGAs bevatten een matrix van programmeerbare logische blokken en een hiërarchie van configureerbare verbindingen waarmee de blokken op verschillende manieren worden geconfigureerd nadat de productie.

FPGAs bieden een combinatie van programmeerbaarheid en prestaties vergelijken met andere chips:

![Vergelijking van Azure Machine Learning FPGA](./media/concept-accelerate-with-fpgas/azure-machine-learning-fpga-comparison.png)

- **Central processing unit (CPU)** zijn algemeen processors. CPU-prestaties is niet ideaal voor afbeeldingen en video-verwerking.
- **Grafische verwerkingseenheden (GPU)** bieden parallelle verwerking en zijn een populaire keuze voor AI-berekeningen. De parallelle verwerking met GPU's leiden tot een snellere rendering van afbeeldingen dan CPU's.
- **Toepassingsspecifiek geïntegreerde circuits (ASIC)**, zoals Google TensorFlow Processor eenheden, worden aangepaste circuits. Deze chips bieden de hoogste efficiëntie, zijn ASICs niet-flexibele.
- **FPGAs**, zoals die beschikbaar zijn op Azure, de prestaties dicht bij ASIC bieden, maar bieden de flexibiliteit om het later opnieuw worden geconfigureerd.

FPGAs worden nu in elke nieuwe Azure-server. Microsoft maakt al gebruik van FPGAs voor Bing ranking diep neurale netwerk (DNN) evaluatieversie zoeken en software-netwerken (SDN) versnelling gedefinieerde. Deze implementaties FPGA de latentie te verminderen bij het vrijmaken van CPU's voor andere taken.

## <a name="project-brainwave-on-azure"></a>Project Brainwave op Azure

Project Brainwave is een hardwarearchitectuur die speciaal is ontworpen op basis van de Intel FPGA apparaten en gebruikt om te versnellen realtime AI-berekeningen. Met deze architectuur voordelige FPGA ingeschakeld kan zo snel mogelijk en met lagere latentie neurale netwerk wordt getraind worden uitgevoerd. Project Brainwave kunt vooraf getraind DNNs parallelize over FPGAs uit uw service te schalen.

- Prestaties

    FPGAs maken het mogelijk om het bereiken van de lage latentie voor realtime inferencing aanvragen. Batchverwerking betekent dat een aanvraag in kleinere delen splitsen en voeding ze aan een processor voor het gebruik van hardware te verbeteren. Batchverwerking is niet efficiënt en kan leiden tot latentie. Brainwave vereist geen batchverwerking; Daarom is de latentie 10 keer lagere vergeleken met de CPU en GPU.

- Flexibiliteit

    FPGAs kunnen worden geconfigureerd voor verschillende soorten machine learning-modellen. Deze flexibiliteit maakt het gemakkelijker te versnellen van de toepassingen op basis van de meest optimale numerieke precisie en model van het geheugen wordt gebruikt.

    Nieuwe machine learning-technieken regelmatig worden ontwikkeld en ontwerp van de hardwareconfiguratie van Project Brainwave is ook snel ontwikkeling. Aangezien FPGAs configureerbare, is het mogelijk om te blijven met de vereisten van de snel veranderende AI-algoritmen.

- Schalen

    Microsoft Azure is de grootste cloud investering in FPGAs. U kunt Brainwave uitvoeren op Azure scale-infrastructuur.

Een voorbeeld van Project Brainwave geïntegreerd met Azure Machine Learning is momenteel beschikbaar. En een beperkte preview is ook beschikbaar voor het Project Brainwave naar de rand plaatsen, zodat u van die computer snelheid in uw eigen bedrijven en faciliteiten profiteren kunt.

In de huidige preview is Brainwave beperkt tot de TensorFlow-implementatie en de ResNet50 gebaseerde neural netwerken op Intel FPGA hardware voor classificatie van de installatiekopie en herkenning. Er zijn plannen om meer galerij-modellen en andere frameworks te ondersteunen.

De volgende scenario's gebruiken FPGA op Project Brainwave architectuur:

- Systeem voor netwerkinspectie optische geautomatiseerd. Zie [Real-time AI: Microsoft introduceert evaluatieversie van Project Brainwave](https://blogs.microsoft.com/ai/build-2018-project-brainwave/).
- Toewijzing van vaste behandeld. Zie [FPGAs voor grondige Learning Deductie gebruiken om uit te voeren Land behandeld toewijzing op Terabytes aan lucht installatiekopieën](https://blogs.technet.microsoft.com/machinelearning/2018/05/29/how-to-use-fpgas-for-deep-learning-inference-to-perform-land-cover-mapping-on-terabytes-of-aerial-images/).

## <a name="how-to-create-an-fpga-service"></a>Het maken van een service FPGA?

De stroom op hoog niveau voor het maken van een image-service voor opname in Azure met behulp van ResNet50 als een featurizer is als volgt:

1. ResNet50 is in Brainwave al geïmplementeerd. Maken van andere grafieken met TensorFlow (invoer datum, classificatie, enzovoort) en een pijplijn definiëren (invoer -> featurize -> classificeren) met behulp van een service definitie json-bestand. De definitie en grafieken comprimeren in een zip-bestand en het zip-bestand uploaden naar Azure Blob-opslag.
2. Registreer het model met behulp van Azure ML-Model Management-API met het zip-bestand in Blob storage.
3. De service implementeren met het geregistreerde model met behulp van Azure ML-Model Management-API.

Zie voor een zelfstudie [Azure ML Hardware versnelde modellen Quickstart](https://github.com/Azure/aml-real-time-ai/blob/master/notebooks/resnet50/00_QuickStart.ipynb).

## <a name="start-using-fpga"></a>Aan de slag met FPGA

Azure Machine Learning Hardware versnelde modellen kunt u om te trained DNN modellen implementeren op FPGAs in de Azure-cloud. Zie het volgende om te beginnen:

- [Een model als een webservice op een FPGA implementeren](how-to-deploy-fpga-web-service.md)
- [Microsoft Azure Machine Learning-Hardware versnelde modellen aangedreven door Project Brainwave](https://github.com/azure/aml-real-time-ai).

## <a name="next-steps"></a>Volgende stappen

[Een model als een webservice op een FPGA implementeren](how-to-deploy-fpga-web-service.md)

[Informatie over het installeren van de SDK FPGA](reference-fpga-package-overview.md)

[Grootschalig hardware: ML op grote schaal boven op Azure + FPGA: Build 2018 (video)](https://www.youtube.com/watch?v=BMgQAHIx2eY)

[In de Microsoft FPGA gebaseerde configureerbare cloud (video)](https://channel9.msdn.com/Events/Build/2017/B8063)

[Microsoft onthult Project Brainwave voor realtime AI](https://www.microsoft.com/research/blog/microsoft-unveils-project-brainwave/)
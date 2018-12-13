---
title: Wat is een FPGA en Project Brainwave?
titleSuffix: Azure Machine Learning service
description: Informatie over het versnellen van modellen en deep neural networks met FPGA's op Azure. Dit artikel bevat een inleiding tot veld-programmable gate arrays (FPGA) en hoe Azure Machine Learning-Service biedt realtime kunstmatige intelligentie (AI) wanneer u uw model naar een Azure-FPGA implementeert.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: tedway
author: tedway
ms.reviewer: jmartens
ms.date: 10/24/2018
ms.custom: seodec18
ms.openlocfilehash: b55d641f9a72fc2f757a7687b1b6214a98c92507
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53093995"
---
# <a name="what-is-fpga-and-project-brainwave"></a>Wat is FPGA en Project Brainwave?

Dit artikel bevat een inleiding tot veld-programmable gate arrays (FPGA) en hoe Azure Machine Learning-service biedt realtime kunstmatige intelligentie (AI) wanneer u uw model naar een Azure-FPGA implementeert.

FPGA's bevatten een matrix van programmeerbare logische blokken en een hiërarchie van herconfigureerbare koppelt. De verbindingen toestaan deze blokken worden geconfigureerd in de verschillende manieren na productie. FPGA's bieden een combinatie van programmeren en prestaties in vergelijking met andere chips.

## <a name="fpgas-vs-cpu-gpu-and-asic"></a>FPGA's vs. CPU, GPU en ASIC

![FPGA vergelijking van Azure Machine Learning-service](./media/concept-accelerate-with-fpgas/azure-machine-learning-fpga-comparison.png)

|Processor||Description|
|---|:-------:|------|
|Toepassingsspecifieke geïntegreerde circuits|ASICs|Aangepaste circuits, zoals Google TensorFlow Processor eenheden (TPU), biedt het hoogste rendement. Ze kunnen niet opnieuw worden geconfigureerd wanneer uw behoeften veranderen.|
|Veld-programmable gate arrays|FPGA 's|FPGA's, zoals die beschikbaar zijn op Azure, levert prestaties dicht bij ASICs, maar zijn flexibel en herconfigureerbare na verloop van tijd om nieuwe logica te implementeren.|
|Grafische verwerkingseenheden|GPU 's|Een populaire optie voor AI-berekeningen bieden mogelijkheden voor de parallelle verwerking waardoor het sneller op beeldweergave dan CPU's.|
|Central processing units|CPU's|Voor algemene doeleinden-processors waarvan de prestaties niet ideaal voor grafische afbeeldingen en video verwerking is.|

## <a name="project-brainwave-on-azure"></a>Project Brainwave op Azure

[Project Brainwave](https://www.microsoft.com/research/project/project-brainwave/) is van Microsoft voordelige hardware-architectuur, op basis van Intel FPGA-apparaten, die data scientists en ontwikkelaars gebruiken om te versnellen realtime AI-berekeningen.  Deze architectuur FPGA-functionaliteit biedt **prestaties**, **flexibiliteit**, en **schaal** en is beschikbaar op Azure.

**FPGA's maken het mogelijk is om te realiseren met lage latentie voor realtime inferentietaken aanvragen.** Asynchrone aanvragen (batch) zijn niet nodig. Batchverwerking kan leiden tot latentie omdat meer gegevens moet worden verwerkt, maar deze doorvoer in sommige contexten kunt verbeteren. Project Brainwave implementaties van neurale verwerkingseenheden vereisen batchverwerking; de latentie mag daarom vaak lager dan voor CPU en GPU.

### <a name="reconfigurable-power"></a>Herconfigureerbare power
**FPGA's kunnen worden geconfigureerd voor verschillende soorten machine learning-modellen.** Deze flexibiliteit maakt het gemakkelijker om te versnellen van de toepassingen op basis van de meest optimale numerieke precisie en model van het geheugen wordt gebruikt.

Regelmatig nieuwe machine learning-technieken worden ontwikkeld en ontwerp van de hardwareconfiguratie van Project Brainwave is ook zich snel ontwikkelende. Aangezien FPGA's herconfigureerbare, is het mogelijk te actueel blijven met de vereisten van de snel veranderende AI-algoritmen.

### <a name="whats-supported-on-azure"></a>Wat wordt ondersteund op Azure
**Microsoft Azure is's werelds grootste investeringen in de cloud in FPGA's.** U kunt Project Brainwave uitvoeren op de infrastructuur van Azure schaal.

Nu een Project Brainwave ondersteunt:
+ Afbeelding classificatie en herkenning van scenario 's
+ TensorFlow-implementatie
+ Dnn's: ResNet 50, ResNet 152 VGG-16, SSD-VGG en DenseNet 121
+ Intel FPGA-hardware 

Met deze hardwarearchitectuur FPGA-functionaliteit, getrainde neurale netwerken uitvoeren snel en met een lagere latentie. Project Brainwave kunt vooraf getrainde (DNN) in de deep neural networks parallel op FPGA's uit de service te schalen. De dnn's kunnen worden vooraf getrainde, als een grondige featurizer voor overdracht leren of op uw wensen afgestemd met bijgewerkte waarden.

### <a name="scenarios-and-applications"></a>Scenario's en toepassingen

Project Brainwave is geïntegreerd met Azure Machine Learning. Gebruikt door Microsoft FPGA's voor DNN evaluatie, Bing-zoekresultaten en software gedefinieerde netwerken (SDN)-versnelling om te verminderen latentie bij het vrijmaken van CPU's voor andere taken.

De volgende scenario's gebruiken FPGA op Project Brainwave architectuur:
+ [Geautomatiseerd systeem voor netwerkinspectie optische](https://blogs.microsoft.com/ai/build-2018-project-brainwave/)

+ [Land cover toewijzing](https://blogs.technet.microsoft.com/machinelearning/2018/05/29/how-to-use-fpgas-for-deep-learning-inference-to-perform-land-cover-mapping-on-terabytes-of-aerial-images/)

## <a name="deploy-to-fpgas-on-azure"></a>Implementeren op FPGA's op Azure

Dit is de werkstroom voor het maken van een image recognition-service in Azure met behulp van ondersteunde dnn's als een featurizer voor implementatie op Azure FPGA's:

1. Gebruik de [Azure Machine Learning-SDK voor Python](https://aka.ms/aml-sdk) voor het maken van de servicedefinitie van een die wordt een bestand met een beschrijving van een pijplijn van grafieken (invoer, featurizer en classificatie) op basis van TensorFlow. De implementatieopdracht wordt automatisch de definitie en grafieken in een ZIP-bestand comprimeren en het ZIP-bestand uploaden naar Azure Blob-opslag.  De DNN is al geïmplementeerd op het Project Brainwave uit te voeren op FPGA.

1. Registreer het model met behulp van de SDK met het ZIP-bestand in Azure Blob-opslag.

1. De service implementeren met het geregistreerde model met behulp van SDK.

U kunt aan de slag implementeren getrainde DNN modellen op FPGA's in de Azure-cloud met dit artikel **"[implementeert een model als een webservice op een FPGA](how-to-deploy-fpga-web-service.md)"**.


## <a name="next-steps"></a>Volgende stappen

Bekijk deze video's en blogs:

+ [Zeer grootschalige hardware: ML op schaal boven op Azure + FPGA: Build 2018 (video)](https://www.youtube.com/watch?v=BMgQAHIx2eY)

+ [In de Microsoft FPGA gebaseerde instelbare cloud (video)](https://channel9.msdn.com/Events/Build/2017/B8063)

+ [Project Brainwave voor realtime AI: project-startpagina](https://www.microsoft.com/research/project/project-brainwave/)

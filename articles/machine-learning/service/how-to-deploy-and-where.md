---
title: Waar u wilt implementeren van modellen met Azure Machine Learning-service | Microsoft Docs
description: Meer informatie over de verschillende manieren waarop u uw modellen in productie door middel van de service Azure Machine Learning kunt implementeren.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 08/29/2018
ms.openlocfilehash: 606e8aed42bce69d5b3210b4e97f8cbfeaaf104c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46961005"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Implementeer modellen met de Azure Machine Learning-service

De service Azure Machine Learning biedt verschillende manieren die kunt u het getrainde model implementeren. In dit document leert u hoe u uw model als een webservice in de Azure-cloud, of op IoT edge-apparaten implementeren.

U kunt modellen implementeren op de volgende compute-doelen:

- Azure Container Instances (ACI)
- Azure Kubernetes Service (AKS)
- Azure IoT Edge
- Veld-programmable gate array (FPGA)

De rest van dit document praat over elk van deze opties in detail.

## <a name="azure-container-instances"></a>Azure Container Instances

Gebruik Azure Container Instances voor uw modellen implementeren als een REST-API-eindpunt als een of meer van de volgende voorwaarden is waar:
- U zoekt naar een snelle implementatie om te beoordelen en valideren van uw model. ACI-implementatie is meestal voltooid in minder dan vijf minuten.
- U bent voor het implementeren van uw model in een ontwikkel- of testomgeving. ACI kunt u groepen met 20 containers per abonnement kunt implementeren. Zie voor meer informatie de [quota en beschikbaarheid in regio's voor Azure Container Instances](https://docs.microsoft.com/azure/container-instances/container-instances-quotas) document.

Zie voor meer informatie de [een model implementeren in Azure Container Instances](how-to-deploy-to-aci.md) document.

## <a name="azure-kubernetes-service"></a>Azure Kubernetes Service

Voor grote schaal productiescenario's, kunt u uw model implementeren naar de Azure Kubernetes Service (AKS). U kunt een bestaand AKS-cluster gebruiken of een nieuwe maken met behulp van de SDK van Azure Machine Learning, CLI of Azure portal.

Het maken van AKS-cluster is een proces tijd voor uw werkruimte. Nadat u hebt gemaakt, kunt u dit cluster voor meerdere implementaties opnieuw gebruiken. Als u het cluster of de resourcegroep waarin het verwijdert, moet klikt u vervolgens u een nieuw cluster de volgende keer dat u wilt implementeren.

Implementeren naar AKS biedt automatisch schalen, logboekregistratie, gegevensverzameling van model en snelle responstijden voor uw web-services. 

Het proces voor het maken van een AKS-cluster duurt ongeveer 20 minuten.

Zie voor meer informatie de [een model implementeren in Azure Kubernetes Service](how-to-deploy-to-aks.md) document.

## <a name="azure-iot-edge"></a>Azure IoT Edge

Met IoT-apparaten is het sneller aan het uitvoeren van beoordelingen op het apparaat in plaats van die gegevens verzenden naar de cloud en er wordt gewacht op een cloud-gebaseerde model om gegevens te retourneren. U kunt uw model op edge-apparaten kunt hosten met Azure IoT Edge. Als u een of meer van de volgende mogelijkheden nodig hebt, kunt u uw model implementeert naar IoT Edge:
- Prioriteit taken lokaal, zelfs zonder een cloudverbinding verwerken
- Werken met gegenereerde gegevens die is te groot om op te halen snel vanuit de cloud
- Verwerking in realtime via intelligence in of in de buurt van lokale apparaten inschakelen
- Aan gegevens met betrekking tot privacy te voldoen 

Zie voor meer informatie de [implementeren in Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning) document.

Zie voor meer informatie over de IoT Edge-service, de [documentatie voor Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/).


## <a name="field-programmable-gate-arrays-fpga"></a>Veld-programmable gate arrays (FPGA)

Hardware versnelde modellen mogelijk gemaakt door Project Brainwave maken het mogelijk is om te voorzien in zeer lage latentie voor realtime inferentietaken aanvragen bereiken. Project Brainwave versnelt DNN deep neural networks () geïmplementeerd op veld-programmable gate arrays in de Azure-cloud. Gebruikte dnn's zijn beschikbaar als featurizers voor overdrachtsleren of aanpasbare met een gewicht bijvoorbeeld getraind met uw eigen gegevens.

Zie voor meer informatie de [implementeren in een FPGA](how-to-deploy-fpga-web-service.md) document.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende documenten voor meer informatie over hoe u een model implementeren naar een specifieke compute-doel:

* [Een model implementeren in Azure Container Instances](how-to-deploy-to-aci.md)
* [Een model implementeren in Azure Kubernetes Service](how-to-deploy-to-aks.md)
* [Een model implementeren naar Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning)
* [Een model implementeren op FPGA](how-to-deploy-fpga-web-service.md)

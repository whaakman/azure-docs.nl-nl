---
title: De Containerinstallatiekopie die wordt gebruikt voor het implementeren van Azure ML-modellen aanpassen | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een containerinstallatiekopie voor Azure Machine Learning-modellen aanpassen
services: machine-learning
author: tedway
ms.author: tedway
manager: mwinkle
ms.reviewer: mldocs, raymondl
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 03/26/2018
ROBOTS: NOINDEX
ms.openlocfilehash: e49743de817bc1fe92afcbc80764771f6df66c56
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53271263"
---
# <a name="customize-the-container-image-used-for-azure-ml-models"></a>De containerinstallatiekopie die wordt gebruikt voor Azure ML-modellen aanpassen

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



In dit artikel wordt beschreven hoe u een containerinstallatiekopie voor Azure Machine Learning-modellen aanpassen.  Azure ML Workbench maakt gebruik van containers voor het implementeren van machine learning-modellen. De modellen worden geïmplementeerd plus de bijbehorende afhankelijkheden, en Azure ML bouwt een installatiekopie van het model, de afhankelijkheden en de bijbehorende bestanden.

## <a name="how-to-customize-the-docker-image"></a>Over het aanpassen van de Docker-installatiekopie
De Docker-installatiekopie die Azure ML wordt geïmplementeerd met behulp van aanpassen:

1. Een `dependencies.yml` bestand: voor het beheren van afhankelijkheden die geïnstalleerd vanuit zijn [PyPi]( https://pypi.python.org/pypi), kunt u de `conda_dependencies.yml` bestand van het Workbench-project, of maak uw eigen. Dit is de aanbevolen methode voor het installeren van Python-afhankelijkheden die geïnstalleerd met pip zijn.

   Voorbeeld van de CLI-opdracht:
   ```azurecli
   az ml image create -n <my Image Name> --manifest-id <my Manifest ID> -c amlconfig\conda_dependencies.yml
   ```

   Voorbeeld van een conda_dependencies bestand: 
   ```yaml
   name: project_environment
   dependencies:
      - python=3.5.2
      - scikit-learn
      - ipykernel=4.6.1
      
      - pip:
        - azure-ml-api-sdk==0.1.0a11
        - matplotlib
   ```
        
2. Een Docker stappen bestand: met deze optie kunt u de geïmplementeerde installatiekopie aanpassen door de installatie van afhankelijkheden die van PyPi kunnen niet worden geïnstalleerd. 

   Het bestand moet de installatiestappen Docker, zoals een docker-bestand bevatten. De volgende opdrachten zijn toegestaan in het bestand: 

    UITVOERT, WEERGEVEN ENV, FUNC, LABEL

   Voorbeeld van de CLI-opdracht:
   ```azurecli
   az ml image create -n <my Image Name> --manifest-id <my Manifest ID> --docker-file <myDockerStepsFileName> 
   ```

   Afbeelding, Manifest en Service-opdrachten de vlag docker-bestand accepteren.

   Voorbeeldbestand Docker stappen:
   ```docker
   # Install tools required to build the project
   RUN apt-get update && apt-get install -y git libxml2-dev
   # Install library dependencies
   RUN dep ensure -vendor-only
   ```

> [!NOTE]
> De basisinstallatiekopie voor Azure ML-containers Ubuntu en kan niet worden gewijzigd. Als u verschillende installatiekopieën opgeeft, wordt dit genegeerd.

## <a name="next-steps"></a>Volgende stappen
Nu dat u uw containerinstallatiekopie hebt aangepast, kunt u deze kunt implementeren in een cluster voor grootschalige gebruik.  Zie voor meer informatie over het instellen van een cluster voor de implementatie van de webservice [Model Management Configuration](deployment-setup-configuration.md). 

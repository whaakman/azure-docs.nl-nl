---
title: De Container-installatiekopie gebruikt voor de implementatie van Azure ML-modellen aanpassen | Microsoft Docs
description: In dit artikel wordt beschreven hoe u de installatiekopie van een container voor Azure Machine Learning-modellen
services: machine-learning
author: tedway
ms.author: tedway, raymondl
manager: mwinkle
ms.reviewer: mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 3/26/2018
ms.openlocfilehash: 858c8933565aeeb22dc1b685082dab2c6481737b
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="customize-the-container-image-used-for-azure-ml-models"></a>De container-installatiekopie die wordt gebruikt voor Azure ML modellen aanpassen

In dit artikel wordt beschreven hoe de installatiekopie van een container voor Azure Machine Learning-modellen aanpassen.  Azure ML-Workbench gebruikt containers voor het implementeren van machine learning-modellen. De modellen samen met hun afhankelijkheden zijn geïmplementeerd en Azure ML bouwt een installatiekopie van het model, de afhankelijkheden en de bijbehorende bestanden.

## <a name="how-to-customize-the-docker-image"></a>Het aanpassen van de Docker-afbeelding
De Docker-installatiekopie die Azure ML implementeert met behulp van aanpassen:

1. Een bestand depenencies.yml: voor het beheren van de afhankelijkheden die installeerbare van zijn [PyPi]( https://pypi.python.org/pypi), u kunt het bestand conda_dependencies.yml uit het project Workbench gebruiken of zelf. Dit is de aanbevolen aanpak voor het installeren van Python-afhankelijkheden die geïnstalleerd met pip zijn.

   Voorbeeld van de CLI-opdracht:
   ```azurecli
   az ml image create -n <my Image Name> --manifest-id <my Manifest ID> -c amlconfig\conda_dependencies.yml
   ```

   Voorbeeld conda_dependencies bestand: 
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
        
2. Een Docker bestand stappen: deze optie gebruikt, u de geïmplementeerde installatiekopie aanpassen door de installatie van afhankelijkheden die van PyPi kunnen niet worden geïnstalleerd. 

   Het bestand moet de installatiestappen Docker zoals een DockerFile bevatten. De volgende opdrachten zijn toegestaan in het bestand: 

    RUN, ENV, ARG, LABEL, EXPOSE

   Voorbeeld van de CLI-opdracht:
   ```azurecli
   az ml image create -n <my Image Name> --manifest-id <my Manifest ID> --docker-file <myDockerStepsFileName> 
   ```

   Installatiekopie, het Manifest en Service-opdrachten de vlag docker-bestand accepteren.

   Voorbeeldbestand Docker stappen:
   ```docker
   # Install tools required to build the project
   RUN apt-get update && apt-get install -y git libxml2-dev
   # Install library dependencies
   RUN dep ensure -vendor-only
   ```

> [!NOTE]
> De basisinstallatiekopie voor Azure ML-containers Ubuntu en kan niet worden gewijzigd. Als u een andere basisinstallatiekopie opgeeft, worden genegeerd.

## <a name="next-steps"></a>Volgende stappen
Nu dat u uw installatiekopie container hebt aangepast, kunt u deze kunt implementeren naar een cluster voor grootschalige gebruik.  Zie voor meer informatie over het instellen van een cluster voor web service-implementatie [Model Management Configuration](deployment-setup-configuration.md). 

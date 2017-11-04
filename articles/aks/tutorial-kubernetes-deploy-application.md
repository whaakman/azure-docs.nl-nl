---
title: Kubernetes op zelfstudie voor Azure - toepassing implementeren | Microsoft Docs
description: AKS zelfstudie - toepassing implementeren
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: Docker, Containers, Micro-services, Kubernetes, DC/OS, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 73b943709fb3846058d4b1c09bc76b09460855ed
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="run-applications-in-azure-container-service-aks"></a>Toepassingen uitvoeren in Azure Container Service (AKS)

In deze zelfstudie maakt deel uit van vier van acht, een voorbeeld van een toepassing wordt geïmplementeerd in een cluster met Kubernetes. Stappen voltooid omvatten:

> [!div class="checklist"]
> * Kubernetes manifestbestanden bijwerken
> * Toepassing uitvoert in Kubernetes
> * De toepassing testen

In volgende zelfstudies deze toepassing is uitgebreid, bijgewerkt, en Operations Management Suite geconfigureerd voor het controleren van het cluster Kubernetes.

Deze zelfstudie wordt ervan uitgegaan dat een basiskennis van Kubernetes-concepten voor gedetailleerde informatie over Kubernetes Zie de [Kubernetes documentatie](https://kubernetes.io/docs/home/).

## <a name="before-you-begin"></a>Voordat u begint

In vorige zelfstudies, een toepassing in een installatiekopie van een container is verpakt, deze installatiekopie is geüpload naar Azure Container register en een Kubernetes-cluster is gemaakt. 

Voor deze zelfstudie hebt voltooid, moet u de vooraf gemaakte `azure-vote-all-in-one-redis.yml` Kubernetes manifestbestand. Dit bestand is gedownload met de broncode van de toepassing in een vorige zelfstudie. Controleer of dat u de opslagplaats hebt gekloond en dat u de mappen in de gekloonde opslagplaats hebt gewijzigd.

Als u deze stappen nog niet hebt gedaan en u wilt volgen, terug naar [zelfstudie 1 – installatiekopieën van de container maken](./tutorial-kubernetes-prepare-app.md). 

## <a name="update-manifest-file"></a>Manifestbestand bijwerken

In deze zelfstudie is Azure Container register (ACR) gebruikt voor het opslaan van een installatiekopie van een container. Voordat de toepassing wordt uitgevoerd, moet de servernaam van de ACR-aanmelding in het manifestbestand Kubernetes worden bijgewerkt.

Ophalen van de servernaam van ACR aanmelding met de [az acr lijst](/cli/azure/acr#list) opdracht.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Het manifestbestand is vooraf gemaakte met een aanmeldingsnaam voor de server van `microsoft`. Open het bestand met een teksteditor. In dit voorbeeld wordt het bestand is geopend met `vi`.

```console
vi azure-vote-all-in-one-redis.yml
```

Vervang `microsoft` met de naam van ACR aanmelding. Deze waarde is gevonden op regel **47** van het manifestbestand.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:redis-v1
```

Sla en sluit het bestand.

## <a name="deploy-application"></a>Toepassing implementeren

Gebruik de opdracht [kubectl create](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) om de toepassing uit te voeren. Deze opdracht het manifestbestand parseert en de gedefinieerde Kubernetes objecten maken.

```azurecli
kubectl create -f azure-vote-all-in-one-redis.yml
```

Uitvoer:

```
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-application"></a>Toepassing testen

Een [Kubernetes service](https://kubernetes.io/docs/concepts/services-networking/service/) die beschrijft de toepassing met het internet wordt gemaakt. Dit proces kan enkele minuten duren. 

Gebruik de opdracht [kubectl get service](https://kubernetes.io/docs/user-guide/kubectl/v1.7/#get) met het argument `--watch` om de voortgang te controleren.

```azurecli
kubectl get service azure-vote-front --watch
```

Eerst wordt het *externe IP-adres* voor de service *azure-vote-front* weergegeven als *in behandeling*.
  
```
azure-vote-front   10.0.34.242   <pending>     80:30676/TCP   7s
```

Eenmaal de *extern IP-* adres is gewijzigd van *in behandeling* naar een *IP-adres*, gebruik `CTRL-C` het proces van de controle kubectl te stoppen. 

```
azure-vote-front   10.0.34.242   52.179.23.131   80:30676/TCP   2m
```

Overzicht van de toepassing, blader naar het externe IP-adres.

![Afbeelding van Kubernetes-cluster in Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie is de toepassing Azure stem geïmplementeerd op een cluster Kubernetes in AKS. Taken zijn voltooid, zijn onder andere:  

> [!div class="checklist"]
> * Kubernetes manifest-bestanden downloaden
> * De toepassing uitvoert in Kubernetes
> * De toepassing testen

Ga naar de volgende zelfstudie voor meer informatie over het schalen van zowel een Kubernetes-toepassing en de onderliggende Kubernetes-infrastructuur. 

> [!div class="nextstepaction"]
> [Schaal Kubernetes toepassingen en infrastructuur](./tutorial-kubernetes-scale.md)

---
title: 'Zelfstudie voor Kubernetes in Azure: toepassing implementeren'
description: 'Zelfstudie voor AKS: toepassing implementeren'
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 33725eb64399f446ff540a36f702c80107958242
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/03/2018
---
# <a name="run-applications-in-azure-container-service-aks"></a>Toepassingen uitvoeren in Azure Container Service (AKS)

In deze zelfstudie, deel vier van acht, wordt een voorbeeldtoepassing geïmplementeerd in een Kubernetes-cluster. Dit zijn de uitgevoerde stappen:

> [!div class="checklist"]
> * Kubernetes-manifestbestanden bijwerken
> * Toepassing uitvoeren in Kubernetes
> * De toepassing testen

In de volgende zelfstudies wordt deze toepassing geschaald en bijgewerkt en wordt Operations Management Suite geconfigureerd om het Kubernetes-cluster te controleren.

In deze zelfstudie wordt ervan uitgegaan dat u over basiskennis van Kubernetes-concepten beschikt. Raadpleeg de [Kubernetes-documentatie][kubernetes-documentation] voor gedetailleerde informatie over Kubernetes.

## <a name="before-you-begin"></a>Voordat u begint

In de vorige zelfstudies is een toepassing verpakt in een containerinstallatiekopie, is deze installatiekopie geüpload naar Azure Container Registry en is een Kubernetes-cluster gemaakt. 

Om deze zelfstudie te voltooien hebt u het vooraf gemaakte Kubernetes-manifestbestand `azure-vote-all-in-one-redis.yaml` nodig. Dit bestand is met de broncode van de toepassing gedownload in een vorige zelfstudie. Controleer of u de opslagplaats hebt gekloond en of u naar de gekloonde opslagplaats bent gegaan.

Als u deze stappen niet hebt uitgevoerd en deze zelfstudie wilt volgen, gaat u terug naar [Zelfstudie 1: Containerinstallatiekopieën maken][aks-tutorial-prepare-app].

## <a name="update-manifest-file"></a>Manifestbestand bijwerken

In deze zelfstudie wordt Azure Container Registry (ACR) gebruikt om een containerinstallatiekopie op te slaan. Voordat u de toepassing uitvoert, moet de naam van de ACR-aanmeldingsserver in het Kubernetes-manifestbestand worden bijgewerkt.

Haal de naam van de ACR-aanmeldingsserver op met de opdracht [az acr list][az-acr-list].

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Het manifestbestand is vooraf gemaakt met `microsoft` als naam van de aanmeldingsserver. Open het bestand met een teksteditor. In dit voorbeeld wordt het bestand geopend met `vi`.

```console
vi azure-vote-all-in-one-redis.yaml
```

Vervang `microsoft` door de naam van de ACR-aanmeldingsserver. U vindt deze waarde op regel **47** van het manifestbestand.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:v1
```

Sla het bestand op en sluit het.

## <a name="deploy-application"></a>Toepassing implementeren

Gebruik de opdracht [kubectl create][kubectl-create] om de toepassing uit te voeren. Deze opdracht parseert het manifestbestand en maakt de gedefinieerde Kubernetes-objecten.

```azurecli
kubectl create -f azure-vote-all-in-one-redis.yaml
```

Uitvoer:

```
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-application"></a>Toepassing testen

Er wordt een [Kubernetes-service][kubernetes-service] gemaakt die de toepassing beschikbaar maakt op internet. Dit proces kan enkele minuten duren. 

Gebruik de opdracht [kubectl get service][kubectl-get] met het argument `--watch` om de voortgang te controleren.

```azurecli
kubectl get service azure-vote-front --watch
```

Eerst wordt het *externe IP-adres* voor de service *azure-vote-front* weergegeven als *in behandeling*.
  
```
azure-vote-front   10.0.34.242   <pending>     80:30676/TCP   7s
```

Zodra het *EXTERNE IP-adres* is gewijzigd van *in behandeling* in een *IP-adres*, gebruikt u `CTRL-C` om het controleproces van kubectl te stoppen. 

```
azure-vote-front   10.0.34.242   52.179.23.131   80:30676/TCP   2m
```

Als u de toepassing wilt zien, bladert u naar het externe IP-adres.

![Afbeelding van Kubernetes-cluster in Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie is een Azure Vote-toepassing geïmplementeerd in een Kubernetes-cluster in AKS. Dit zijn de uitgevoerde taken:  

> [!div class="checklist"]
> * Kubernetes-manifestbestanden downloaden
> * De toepassing uitvoeren in Kubernetes
> * De toepassing testen

Ga naar de volgende zelfstudie om te leren hoe u zowel een Kubernetes-toepassing als de onderliggende Kubernetes-infrastructuur schaalt. 

> [!div class="nextstepaction"]
> [Kubernetes-toepassing en -infrastructuur schalen][aks-tutorial-scale]

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-documentation]: https://kubernetes.io/docs/home/
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-scale]: ./tutorial-kubernetes-scale.md
[az-acr-list]: /cli/azure/acr#list

---
title: 'Zelfstudie voor Kubernetes in Azure: een toepassing implementeren'
description: In deze zelfstudie over Azure Kubernetes Service (AKS) gaat u een toepassing met meerdere containers in uw cluster implementeren met behulp van een aangepaste installatiekopie die is opgeslagen in Azure Container Registry.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 08/14/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: bf817f553250ead449ec0d5db3d33acc2eff23f3
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2018
ms.locfileid: "41918902"
---
# <a name="tutorial-run-applications-in-azure-kubernetes-service-aks"></a>Zelfstudie: Toepassingen uitvoeren in AKS (Azure Kubernetes Service)

Kubernetes biedt een gedistribueerd platform voor toepassingen in containers. U gaat uw eigen toepassingen en services implementeren in een Kubernetes-cluster, en u laat het cluster de beschikbaarheid en connectiviteit beheren. In deze zelfstudie, deel vier van zeven, wordt een voorbeeldtoepassing geïmplementeerd in een Kubernetes-cluster. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een Kubernetes-manifestbestand bijwerken
> * Een toepassing in Kubernetes uitvoeren
> * De toepassing testen

In opeenvolgende zelfstudies wordt deze toepassing geschaald en bijgewerkt.

In deze zelfstudie wordt ervan uitgegaan dat u over basiskennis van Kubernetes-concepten beschikt. Raadpleeg de [Kubernetes-documentatie][kubernetes-documentation] voor gedetailleerde informatie over Kubernetes.

## <a name="before-you-begin"></a>Voordat u begint

In de vorige zelfstudies is een toepassing verpakt in een containerinstallatiekopie, is deze installatiekopie geüpload naar Azure Container Registry en is een Kubernetes-cluster gemaakt.

Om deze zelfstudie te voltooien hebt u het vooraf gemaakte Kubernetes-manifestbestand `azure-vote-all-in-one-redis.yaml` nodig. Dit bestand is met de broncode van de toepassing gedownload in een vorige zelfstudie. Controleer of u de opslagplaats hebt gekloond en of u naar de gekloonde opslagplaats bent gegaan. Als u deze stappen niet hebt uitgevoerd en deze zelfstudie wilt volgen, gaat u terug naar [Zelfstudie 1: Containerinstallatiekopieën maken][aks-tutorial-prepare-app].

Voor deze zelfstudie moet u Azure CLI versie 2.0.44 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli-install].

## <a name="update-the-manifest-file"></a>Het manifestbestand bijwerken

In deze zelfstudies wordt met een instantie van Azure Container Registry (ACR) de containerinstallatiekopie voor de voorbeeldtoepassing opgeslagen. Om de toepassing te implementeren, moet u de naam van de installatiekopie in het Kubernetes-manifestbestand bijwerken zodat de naam van de ACR-aanmeldingsserver erin is opgenomen.

Haal de naam van de ACR-aanmeldingsserver als volgt op met de opdracht [az acr list][az-acr-list].

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Het voorbeeldmanifestbestand van de Git-opslagplaats dat in de eerste zelfstudie is gekloond, gebruikt *microsoft* als de naam van de aanmeldingsserver. Open dit manifestbestand openen met een teksteditor, zoals `vi`:

```console
vi azure-vote-all-in-one-redis.yaml
```

Vervang *microsoft* door de naam van de ACR-aanmeldingsserver. De naam van de installatiekopie vindt u op regel 47 van het manifestbestand. In het volgende voorbeeld ziet u de standaardnaam van de installatiekopie:

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:v1
```

Geef uw eigen naam van de ACR-aanmeldingsserver op zodat uw manifestbestand eruitziet zoals in het volgende voorbeeld:

```yaml
containers:
- name: azure-vote-front
  image: <acrName>.azurecr.io/azure-vote-front:v1
```

Sla het bestand op en sluit het.

## <a name="deploy-the-application"></a>De toepassing implementeren

Gebruik de opdracht [kubectl apply][kubectl-apply] om uw toepassing te implementeren. Deze opdracht parseert het manifestbestand en maakt de gedefinieerde Kubernetes-objecten. Geef het voorbeeldmanifestbestand op, zoals wordt weergegeven in het volgende voorbeeld:

```console
kubectl apply -f azure-vote-all-in-one-redis.yaml
```

De Kubernetes-objecten worden gemaakt binnen het cluster, zoals wordt weergegeven in het volgende voorbeeld:

```
$ kubectl apply -f azure-vote-all-in-one-redis.yaml

deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-the-application"></a>De toepassing testen

Er wordt een [Kubernetes-service][kubernetes-service] gemaakt die de toepassing beschikbaar maakt op internet. Dit proces kan enkele minuten duren. Gebruik de opdracht [kubectl get service][kubectl-get] met het argument `--watch` om de voortgang te controleren.

```console
kubectl get service azure-vote-front --watch
```

Het *EXTERNE IP-adres* voor de service *azure-vote-front* wordt aanvankelijk weergegeven als *in behandeling*, zoals het volgende voorbeeld laat zien:

```
azure-vote-front   10.0.34.242   <pending>     80:30676/TCP   7s
```

Zodra het *EXTERNE IP-adres* is gewijzigd van *in behandeling* in een echt openbaar IP-adres, gebruikt u `CTRL-C` om het controleproces van kubectl te stoppen. In het volgende voorbeeld ziet u dat er nu een openbaar IP-adres is toegewezen:

```
azure-vote-front   10.0.34.242   52.179.23.131   80:30676/TCP   2m
```

Open een webbrowser naar het externe IP-adres als u de toepassing in actie wilt zien.

![Afbeelding van Kubernetes-cluster in Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

Als de toepassing niet is geladen, kan dit zijn veroorzaakt door een autorisatieprobleem met het register van de installatiekopie. Als u de status van uw containers wilt bekijken, gebruikt u de opdracht `kubectl get pods`. Zie [Toegang tot Container Registry met een Kubernetes-geheim toestaan](https://docs.microsoft.com/azure/container-registry/container-registry-auth-aks#access-with-kubernetes-secret) als de containerinstallatiekopieën niet kunnen worden opgehaald.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie is een Azure Vote-toepassing geïmplementeerd in een Kubernetes-cluster in AKS. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een Kubernetes-manifestbestand bijwerken
> * Een toepassing in Kubernetes uitvoeren
> * De toepassing testen

Ga naar de volgende zelfstudie om te leren hoe u zowel een Kubernetes-toepassing als de onderliggende Kubernetes-infrastructuur schaalt.

> [!div class="nextstepaction"]
> [Kubernetes-toepassing en -infrastructuur schalen][aks-tutorial-scale]

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-documentation]: https://kubernetes.io/docs/home/
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-scale]: ./tutorial-kubernetes-scale.md
[az-acr-list]: /cli/azure/acr#list
[azure-cli-install]: /cli/azure/install-azure-cli

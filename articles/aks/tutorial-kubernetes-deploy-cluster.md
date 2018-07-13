---
title: 'Zelfstudie voor Kubernetes in Azure: Cluster implementeren'
description: 'Zelfstudie voor AKS: Cluster implementeren'
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: c8698f16138e9baeb9c9c1142a5d0c8937a69d1b
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37341396"
---
# <a name="tutorial-deploy-an-azure-kubernetes-service-aks-cluster"></a>Zelfstudie: Een AKS-cluster (Azure Kubernetes Service) implementeren

Kubernetes biedt een gedistribueerd platform voor toepassingen in containers. Met AKS kunt u snel een productieklaar Kubernetes-cluster inrichten. In deze zelfstudie, deel drie van de zeven, wordt een Kubernetes-cluster geïmplementeerd in AKS. Dit zijn de uitgevoerde stappen:

> [!div class="checklist"]
> * Een service-principal voor de resource-interacties maken
> * Een Kubernetes AKS-cluster implementeren
> * Installatie van de Kubernetes-CLI (kubectl)
> * Configuratie van kubectl

In opeenvolgende zelfstudies wordt de Azure Vote-toepassing geïmplementeerd in het cluster, vervolgens geschaald en bijgewerkt.

## <a name="before-you-begin"></a>Voordat u begint

In de vorige zelfstudies is een containerinstallatiekopie gemaakt en geüpload naar een Azure Container Registry-exemplaar. Als u deze stappen niet hebt uitgevoerd en deze zelfstudie wilt volgen, gaat u terug naar [Zelfstudie 1: Containerinstallatiekopieën maken][aks-tutorial-prepare-app].

## <a name="create-a-service-principal"></a>Een service-principal maken

Een AKS-cluster heeft een service-principal van Azure Active Directory nodig om met andere Azure-resources te kunnen communiceren. Deze service-principal kan automatisch worden gemaakt door de Azure CLI of via de portal, of u kunt er een vooraf maken en hieraan extra machtigingen toewijzen. In deze zelfstudie gaat u een service-principal maken, toegang verlenen tot de ACR-instantie (Azure Container Registry) die u in de vorige zelfstudie hebt gemaakt, en vervolgens een AKS-cluster maken.

Gebruik [az ad sp create-for-rbac][] om een service-principal te maken. De parameter `--skip-assignment` zorgt ervoor dat eventuele extra machtigingen beperkt kunnen worden toegewezen.

```azurecli
az ad sp create-for-rbac --skip-assignment
```

De uitvoer lijkt op die in het volgende voorbeeld:

```
{
  "appId": "e7596ae3-6864-4cb8-94fc-20164b1588a9",
  "displayName": "azure-cli-2018-06-29-19-14-37",
  "name": "http://azure-cli-2018-06-29-19-14-37",
  "password": "52c95f25-bd1e-4314-bd31-d8112b293521",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

Noteer de *appId* en *wachtwoord*. Deze waarden worden gebruikt in de volgende stappen.

## <a name="configure-acr-authentication"></a>ACR-verificatie configureren

Voor toegang tot installatiekopieën die zijn opgeslagen in ACR, moet u aan de AKS-service-principal de juiste rechten verlenen om installatiekopieën uit ACR op te kunnen halen.

Haal eerst de resource-ID van de ACR op met [az acr show][]. Werk de naam van het `<acrName>`-register bij met die van uw ACR-instantie en van de resourcegroep waar de ACR-instantie zich bevindt.

```azurecli
az acr show --name <acrName> --resource-group myResourceGroup --query "id" --output tsv
```

Als u het AKS de juiste toegangsrechten wilt verlenen om de installatiekopieën te gebruiken die zijn opgeslagen in de ACR, maakt u een roltoewijzing met [az role assignment create][]. Vervang `<appId`> en `<acrId>` door de waarden die zijn verzameld in de vorige twee stappen.

```azurecli
az role assignment create --assignee <appId> --role Reader --scope <acrId>
```

## <a name="create-kubernetes-cluster"></a>Een Kubernetes-cluster maken

Maak nu een AKS-cluster met [az aks create][]. In het volgende voorbeeld wordt een cluster met de naam *myAKSCluste* gemaakt in de resourcegroep met de naam *myResourceGroup*. Deze resourcegroep is gemaakt in de [vorige zelfstudie][aks-tutorial-prepare-acr]. Voer uw eigen `<appId>` en `<password>` in die u in de vorige stap hebt gemaakt toen u de service-principal maakte.

```azurecli
az aks create \
    --name myAKSCluster \
    --resource-group myResourceGroup \
    --node-count 1 \
    --generate-ssh-keys \
    --service-principal <appId> \
    --client-secret <password>
```

Na enkele minuten is de implementatie voltooid en retourneert JSON opgemaakte informatie over de AKS-implementatie.

## <a name="install-the-kubectl-cli"></a>De CLI kubectl installeren

Gebruik [kubectl][kubectl], de Kubernetes-opdrachtregelclient, als u vanaf uw clientcomputer verbinding wilt maken met het Kubernetes-cluster.

Als u Azure Cloud Shell gebruikt, is kubectl al geïnstalleerd. U kunt deze ook lokaal installeren met [az aks install-cli][]:

```azurecli
az aks install-cli
```

## <a name="connect-with-kubectl"></a>Verbinden met kubectl

Als u kubectl zo wilt configureren dat deze verbinding maakt met uw Kubernetes-cluster, gebruikt u [az aks get-credentials][]. In het volgende voorbeeld worden de referenties voor het AKS-cluster met de naam *myAKSCluster* in *myResourceGroup* opgehaald:

```azurecli
az aks get-credentials --name myAKSCluster --resource-group myResourceGroup
```

Als u de verbinding met uw cluster wilt controleren, voert u de opdracht [kubectl get nodes][kubectl-get] uit.

```azurecli
kubectl get nodes
```

Uitvoer:

```
NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-66427764-0   Ready     agent     9m        v1.9.6
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie is een cluster Kubernetes geïmplementeerd in AKS. De volgende stappen zijn voltooid:

> [!div class="checklist"]
> * Er is een service-principal voor de resource-interacties gemaakt
> * Er is een Kubernetes AKS-cluster geïmplementeerd
> * De Kubernetes-CLI (kubectl) is geïnstalleerd
> * Kubectl is geconfigureerd

Ga naar de volgende zelfstudie om te leren hoe u de toepassing uitvoert in het cluster.

> [!div class="nextstepaction"]
> [Toepassing implementeren in Kubernetes][aks-tutorial-deploy-app]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->
[aks-tutorial-deploy-app]: ./tutorial-kubernetes-deploy-application.md
[aks-tutorial-prepare-acr]: ./tutorial-kubernetes-prepare-acr.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[az ad sp create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az acr show]: /cli/azure/acr#az-acr-show
[az role assignment create]: /cli/azure/role/assignment#az-role-assignment-create
[az aks create]: /cli/azure/aks#az-aks-create
[az aks install-cli]: /cli/azure/aks#az-aks-install-cli
[az aks get-credentials]: /cli/azure/aks#az-aks-get-credentials

---
title: Beheren Azure Kubernetes-cluster met web-UI
description: Informatie over het gebruik van de ingebouwde Kubernetes web UI-dashboard met Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/09/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: af48af596e86e0eb09fe45deabe13beedef57cd2
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39307922"
---
# <a name="access-the-kubernetes-dashboard-with-azure-kubernetes-service-aks"></a>Toegang tot het Kubernetes-dashboard met Azure Kubernetes Service (AKS)

Kubernetes biedt een webdashboard dat kan worden gebruikt voor eenvoudige beheerbewerkingen. In dit artikel ziet u hoe u toegang tot het Kubernetes-dashboard met de Azure CLI en leidt u door enkele eenvoudige dashboard-bewerkingen. Zie voor meer informatie over het Kubernetes-dashboard [Kubernetes Web UI-Dashboard][kubernetes-dashboard].

## <a name="before-you-begin"></a>Voordat u begint

De stappen die in dit document wordt ervan uitgegaan dat u een AKS-cluster hebt gemaakt en hebben een `kubectl` verbinding met het cluster. Als u maken van een AKS-cluster wilt, raadpleegt u de [Quick Start voor AKS][aks-quickstart].

Ook moet de Azure CLI-versie 2.0.27 of later zijn geïnstalleerd en geconfigureerd. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][install-azure-cli].

## <a name="start-kubernetes-dashboard"></a>Kubernetes-dashboard starten

Gebruiken om het Kubernetes-dashboard starten, de [az aks Bladeren] [ az-aks-browse] opdracht. Het volgende voorbeeld wordt het dashboard voor het cluster met de naam *myAKSCluster* in de resourcegroep met de naam *myResourceGroup*:

```azurecli
az aks browse --resource-group myResourceGroup --name myAKSCluster
```

Met deze opdracht maakt een proxy tussen uw systeem voor de ontwikkeling en het Kubernetes-API en opent een webbrowser naar de Kubernetes-dashboard.

### <a name="for-rbac-enabled-clusters"></a>Voor clusters met RBAC-ingeschakeld

Als uw AKS-cluster maakt gebruik van RBAC, een *ClusterRoleBinding* moet worden gemaakt voordat u het dashboard correct kan openen. Standaard wordt met het Kubernetes-dashboard wordt geïmplementeerd met minimale leestoegang en RBAC toegangsfouten worden weergegeven. Het Kubernetes-dashboard biedt momenteel geen ondersteuning voor gebruiker opgegeven referenties om te bepalen het niveau van toegang, in plaats daarvan wordt de rollen die zijn verleend aan de serviceaccount. De Clusterbeheerder van een kunt aanvullende toegang verlenen tot de *kubernetes-dashboard* -serviceaccount, maar dit kan een vector voor uitbreiding van bevoegdheden zijn. U kunt ook Azure Active Directory-verificatie voor een meer gedetailleerd niveau van toegang integreren.

U kunt een binding maken met de [kubectl maken clusterrolebinding] [ kubectl-create-clusterrolebinding] opdracht zoals wordt weergegeven in het volgende voorbeeld. 

> [!WARNING]
> Deze voorbeeld-binding is niet van toepassing op alle onderdelen van de aanvullende verificatie en kan leiden tot onveilig gebruiken. Het Kubernetes-dashboard is geopend voor iedereen met toegang tot de URL. Het Kubernetes-dashboard niet openbaar beschikbaar.
>
> Zie voor meer informatie over het gebruik van de verschillende verificatiemethoden, de Kubernetes-dashboard-wiki op [besturingselementen voor toegang][dashboard-authentication].

```console
kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
```

U kunt nu toegang tot het Kubernetes-dashboard in uw cluster RBAC-functionaliteit. Gebruiken om het Kubernetes-dashboard starten, de [az aks Bladeren] [ az-aks-browse] opdracht zoals beschreven in de vorige stap.

## <a name="run-an-application"></a>Een toepassing uitvoeren

Klik in het Kubernetes-dashboard op de **maken** knop in het bovenste venster. Geef de naam op voor de implementatie `nginx` en voer `nginx:latest` voor de naam van de container-installatiekopie. Onder **Service**, selecteer **externe** en voer `80` voor de poort en de doelpoort.

Wanneer u klaar bent, klikt u op **implementeren** de implementatie te maken.

![Dialoogvenster maken Kubernetes-Service](./media/container-service-kubernetes-ui/create-deployment.png)

## <a name="view-the-application"></a>De toepassing weergeven

Status van de toepassing worden weergegeven op het Kubernetes-dashboard. Zodra de toepassing wordt uitgevoerd, heeft elk onderdeel van een groene selectievakje ernaast.

![Kubernetes-schillen](./media/container-service-kubernetes-ui/complete-deployment.png)

Als u meer informatie over de toepassingspods, klikt u op **schillen** in het menu links, en selecteer vervolgens de **NGINX** schil. Hier ziet u pod-specifieke informatie zoals het gebruik van resources.

![Kubernetes-Resources](./media/container-service-kubernetes-ui/running-pods.png)

Als u het IP-adres van de toepassing zoekt, klikt u op **Services** in het menu links, en selecteer vervolgens de **NGINX** service.

![nginx weergeven](./media/container-service-kubernetes-ui/nginx-service.png)

## <a name="edit-the-application"></a>De toepassing bewerken

Naast het maken en toepassingen voor het weergeven, kan het Kubernetes-dashboard worden gebruikt om te bewerken en bijwerken van implementaties van toepassingen.

Als u wilt een implementatie bewerken, klikt u op **implementaties** in het menu links, en selecteer vervolgens de **NGINX** implementatie. Selecteer ten slotte **bewerken** in de bovenste rechter navigatiebalk.

![Kubernetes bewerken](./media/container-service-kubernetes-ui/view-deployment.png)

Zoek de `spec.replica` -waarde moet 1, wijzigt u deze waarde in 3. Het aantal replica's van de NGINX-implementatie wordt in dat geval, verhoogd van 1 tot 3.

Selecteer **Update** als u klaar bent.

![Kubernetes bewerken](./media/container-service-kubernetes-ui/edit-deployment.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het Kubernetes-dashboard, de Kubernetes-documentatie.

> [!div class="nextstepaction"]
> [Kubernetes Web UI-Dashboard][kubernetes-dashboard]

<!-- LINKS - external -->
[kubernetes-dashboard]: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
[dashboard-authentication]: https://github.com/kubernetes/dashboard/wiki/Access-control
[kubectl-create-clusterrolebinding]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-clusterrolebinding-em-
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-aks-browse]: /cli/azure/aks#az-aks-browse

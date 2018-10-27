---
title: Een Azure Kubernetes Service-cluster met de webdashboard beheren
description: Informatie over het gebruik van de ingebouwde Kubernetes web UI-dashboard voor het beheren van een cluster Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 10/08/2018
ms.author: iainfou
ms.openlocfilehash: 127f6a5cd8f46b72a4cddcef09cb31b60edd0582
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50157187"
---
# <a name="access-the-kubernetes-web-dashboard-in-azure-kubernetes-service-aks"></a>Toegang tot het Kubernetes-web-dashboard in Azure Kubernetes Service (AKS)

Kubernetes biedt een webdashboard dat kan worden gebruikt voor eenvoudige beheerbewerkingen. Dit dashboard kunt u de status van de basisbewaking van statussen en metrische gegevens voor uw toepassingen weergeven, maken en implementeren van services en bewerken van bestaande toepassingen. In dit artikel ziet u hoe u toegang tot het Kubernetes-dashboard met de Azure CLI en leidt u door enkele eenvoudige dashboard-bewerkingen.

Zie voor meer informatie over het Kubernetes-dashboard [Kubernetes Web UI-Dashboard][kubernetes-dashboard].

## <a name="before-you-begin"></a>Voordat u begint

De stappen die in dit document wordt ervan uitgegaan dat u een AKS-cluster hebt gemaakt en hebben een `kubectl` verbinding met het cluster. Als u maken van een AKS-cluster wilt, raadpleegt u de [Quick Start voor AKS][aks-quickstart].

Ook moet de Azure CLI-versie 2.0.46 of later zijn geïnstalleerd en geconfigureerd. Voer  `az --version`  uit om de versie te bekijken. Als u wilt installeren of upgraden, Zie [Azure CLI installeren][install-azure-cli].

## <a name="start-the-kubernetes-dashboard"></a>Start het Kubernetes-dashboard

Gebruiken om het Kubernetes-dashboard starten, de [az aks Bladeren] [ az-aks-browse] opdracht. Het volgende voorbeeld wordt het dashboard voor het cluster met de naam *myAKSCluster* in de resourcegroep met de naam *myResourceGroup*:

```azurecli
az aks browse --resource-group myResourceGroup --name myAKSCluster
```

> [!TIP]
> Als u deze opdracht vanaf de Azure Cloud Shell uitvoert, voegt u toe de `--enable-cloud-console-aks-browse` parameter correct openen het dashboard.

Met deze opdracht maakt een proxy tussen uw systeem voor de ontwikkeling en het Kubernetes-API en opent een webbrowser naar de Kubernetes-dashboard. Als een webbrowser niet aan het Kubernetes-dashboard wordt geopend, kopieert en plakt u de URL-adres dat is vermeld in de Azure CLI, doorgaans *http://127.0.0.1:8001*.

![De overzichtspagina van het Kubernetes-web-dashboard](./media/kubernetes-dashboard/dashboard-overview.png)

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

## <a name="create-an-application"></a>Een app maken

Als u wilt zien hoe het Kubernetes-dashboard kan de complexiteit van beheertaken, laten we u een toepassing maken. U kunt een toepassing van het Kubernetes-dashboard maken door op te geven tekstinvoer, een YAML-bestand, of via een grafische wizard.

Voor het maken van een toepassing, voert u de volgende stappen uit:

1. Selecteer de **maken** knop in het bovenste venster.
1. Als u wilt de grafische wizard gebruikt, wilt **maken van een app**.
1. Geef een naam voor de implementatie, zoals *nginx*
1. Voer de naam in voor de container-installatiekopie moet worden gebruikt, zoals *nginx:1.15.5*
1. Als u poort 80 voor webverkeer beschikbaar, wilt maken u een Kubernetes-service. Onder **Service**, selecteer **externe**, voert u **80** voor de poort en de doelpoort.
1. Wanneer u klaar bent, selecteert u **implementeren** om de app te maken.

![Een app implementeren in het Kubernetes-web-dashboard](./media/kubernetes-dashboard/create-app.png)

Het duurt een minuut of twee voor een openbare externe IP-adres moet worden toegewezen aan de Kubernetes-service. Op de grootte links, onder **detectie en taakverdeling** Selecteer **Services**. De service van uw toepassing wordt weergegeven, met inbegrip van de *externe eindpunten*, zoals wordt weergegeven in het volgende voorbeeld:

![Lijst met services en eindpunten weergeven](./media/kubernetes-dashboard/view-services.png)

Selecteer het adres van het eindpunt opent een browservenster naar de standaard-NGINX-pagina:

![De standaard-NGINX-pagina van de geïmplementeerde toepassing weergeven](./media/kubernetes-dashboard/default-nginx.png)

## <a name="view-pod-information"></a>Pod-informatie weergeven

Het Kubernetes-dashboard kan bieden eenvoudige metrische gegevens controleren en het oplossen van problemen, zoals Logboeken.

Meer informatie over uw toepassing schillen Selecteer **schillen** in het linkermenu. De lijst met beschikbare schillen wordt weergegeven. Kies uw *nginx* schil om informatie, zoals het gebruik van resources weer te geven:

![Pod-informatie weergeven](./media/kubernetes-dashboard/view-pod-info.png)

## <a name="edit-the-application"></a>De toepassing bewerken

Naast het maken en toepassingen voor het weergeven, kan het Kubernetes-dashboard worden gebruikt om te bewerken en bijwerken van implementaties van toepassingen. Aanvullende om redundantie te bieden voor de toepassing, laten we u het aantal NGINX replica's vergroten.

Een implementatie bewerken:

1. Selecteer **implementaties** in het menu links en kies vervolgens uw *nginx* implementatie.
1. Selecteer **bewerken** in de bovenste rechter navigatiebalk.
1. Zoek de `spec.replica` waarde, op rond regel 20. Als u wilt het aantal replica's voor de toepassing te verhogen, wijzig deze waarde van *1* naar *3*.
1. Selecteer **Update** als u klaar bent.

![De implementatie voor het bijwerken van het aantal replica's bewerken](./media/kubernetes-dashboard/edit-deployment.png)

Het duurt een paar minuten voor de nieuwe pods die moet worden gemaakt binnen een replicaset. Kies in het menu links **replicasets**, en kies vervolgens uw *nginx* replicaset. De lijst van schillen bevat nu het aantal bijgewerkte replica's, zoals wordt weergegeven in de volgende voorbeelduitvoer:

![Informatie weergeven over de replicaset](./media/kubernetes-dashboard/view-replica-set.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het Kubernetes-dashboard, de [Kubernetes Web UI-Dashboard][kubernetes-dashboard].

<!-- LINKS - external -->
[kubernetes-dashboard]: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
[dashboard-authentication]: https://github.com/kubernetes/dashboard/wiki/Access-control
[kubectl-create-clusterrolebinding]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-clusterrolebinding-em-
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-aks-browse]: /cli/azure/aks#az-aks-browse

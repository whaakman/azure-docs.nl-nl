---
title: 'Voorbeeld: maken van een Windows Server-container op een cluster Azure Kubernetes Service (AKS)'
description: Leer hoe u snel een Kubernetes-cluster maken, het implementeren van een toepassing in een Windows Server-container in Azure Kubernetes Service (AKS) met de Azure CLI.
services: container-service
author: tylermsft
ms.service: container-service
ms.topic: article
ms.date: 06/06/2019
ms.author: twhitney
ms.openlocfilehash: cdcc1b985c570d1af4bbb33ac29a37e63b1dfa90
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2019
ms.locfileid: "66752389"
---
# <a name="preview---create-a-windows-server-container-on-an-azure-kubernetes-service-aks-cluster-using-the-azure-cli"></a>Voorbeeld: maken van een Windows Server-container op een Azure Kubernetes Service (AKS)-cluster met behulp van de Azure CLI

Azure Kubernetes Service (AKS) is een beheerde Kubernetes-service waarmee u snel clusters kunt implementeren en beheren. In dit artikel implementeert u een AKS-cluster met behulp van de Azure CLI. U kunt ook een ASP.NET-voorbeeld-toepassing in een Windows Server-container implementeren in het cluster.

Deze functie is momenteel beschikbaar als preview-product.

![Afbeelding van bladeren naar de voorbeeld-ASP.NET-toepassing](media/windows-container/asp-net-sample-app.png)

In dit artikel wordt ervan uitgegaan van basiskennis van Kubernetes-concepten beschikt. Zie [Kubernetes-kernconcepten voor Azure Kubernetes Service (AKS)][kubernetes-concepts] voor meer informatie.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om te installeren en de CLI lokaal gebruikt, in dit artikel is vereist dat u de Azure CLI versie 2.0.61 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli-install].

## <a name="before-you-begin"></a>Voordat u begint

Nadat u uw cluster met Windows Server-containers kunt maken, moet u een extra knooppunt van toepassingen toevoegen. Toevoegen van een extra knooppunt van toepassingen in een latere stap wordt behandeld, maar u moet eerst een paar preview-functies inschakelen.

> [!IMPORTANT]
> AKS-preview-functies zijn selfservice, aanmelden. Ze zijn bedoeld om het verzamelen van fouten en feedback van onze community. Preview-versie, worden deze functies zijn niet bedoeld voor gebruik in productieomgevingen. Functies in public preview vallen onder 'best effort'-ondersteuning. Hulp van de AKS-teams voor technische ondersteuning is beschikbaar tijdens kantooruren Pacific tijdzone (PST) alleen. Zie de volgende artikelen ondersteuning voor aanvullende informatie:
>
> * [Ondersteuningsbeleid voor AKS][aks-support-policies]
> * [Veelgestelde vragen over ondersteuning van Azure][aks-faq]

### <a name="install-aks-preview-cli-extension"></a>Aks-preview CLI-extensie installeren
    
De CLI-opdrachten voor het maken en beheren van meerdere groepen zijn beschikbaar in de *aks-preview* CLI-extensie. Installeer de *aks-preview* Azure CLI-extensie met de [az-extensie toevoegen] [ az-extension-add] opdracht, zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive
az extension add --name aks-preview
```

> [!NOTE]
> Als u eerder hebt geïnstalleerd het *aks-preview* extensie, installatie van de beschikbare updates met behulp van de `az extension update --name aks-preview` opdracht.

### <a name="register-windows-preview-feature"></a>Preview-functie van Windows registreren

Voor het maken van een AKS-cluster die kunt gebruiken van meerdere groepen en Windows Server-containers worden uitgevoerd, eerst inschakelen de *WindowsPreview* functie vlaggen voor uw abonnement. De *WindowsPreview* functie ook gebruikmaakt van clusters met meerdere knooppunten van toepassingen en virtuele-machineschaalset voor het beheren van de implementatie en configuratie van de Kubernetes-knooppunten. Registreert de *WindowsPreview* functie vlag met behulp van de [az functie registreren] [ az-feature-register] opdracht zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive
az feature register --name WindowsPreview --namespace Microsoft.ContainerService
```

> [!NOTE]
> Een AKS-cluster dat u maakt nadat u hebt geregistreerd de *WindowsPreview* functievlag gebruik van deze preview-cluster-ervaring. Als u wilt doorgaan met het maken van clusters van reguliere, volledig ondersteunde, geen preview-functies voor productieabonnementen inschakelen. Gebruik een afzonderlijke test- en Azure-abonnement voor het testen van de preview-functies.

Het duurt enkele minuten duren voordat de status om weer te geven *geregistreerde*. U kunt controleren op de registratie van status met behulp van de [az Functielijst] [ az-feature-list] opdracht:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/WindowsPreview')].{Name:name,State:properties.state}"
```

Wanneer u klaar bent, vernieuwt u de registratie van de *Microsoft.ContainerService* resourceprovider met behulp van de [az provider register] [ az-provider-register] opdracht:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

### <a name="limitations"></a>Beperkingen

De volgende beperkingen zijn van toepassing wanneer u maken en beheren van AKS-clusters die ondersteuning bieden voor meerdere groepen:

* Meerdere knooppuntgroepen zijn beschikbaar voor clusters die zijn gemaakt nadat u hebt geregistreerd de *WindowsPreview*. Meerdere knooppuntgroepen zijn ook beschikbaar als u zich registreert de *MultiAgentpoolPreview* en *VMSSPreview* functies voor uw abonnement. U kunt toevoegen of beheren van groepen met een bestaand AKS-cluster dat is gemaakt voordat deze functies zijn geregistreerd.
* U kunt het eerste knooppunt van toepassingen niet verwijderen.

Hoewel deze functie nog in preview, gelden de volgende aanvullende beperkingen:

* Het AKS-cluster kan maximaal acht knooppuntgroepen hebben.
* Het AKS-cluster kan maximaal 400 knooppunten hebt in die pools acht knooppunten.
* De pool-naam van het Windows Server-knooppunt heeft een limiet van 6 tekens.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een Azure-resourcegroep is een logische groep waarin Azure-resources worden geïmplementeerd en beheerd. Wanneer u een resourcegroep maakt, wordt u gevraagd een locatie op te geven. Deze locatie is waar de metagegevens van de resource is opgeslagen, het is ook waar uw resources in Azure uitvoeren als u een andere regio geen tijdens het maken van resources opgeeft. Maak een resourcegroep met de opdracht [az group create][az-group-create].

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS - oost*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

In de volgende voorbeelduitvoer ziet u dat de resourcegroep is gemaakt:

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup",
  "location": "eastus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": null
}
```

## <a name="create-aks-cluster"></a>AKS-cluster maken
Als u wilt uitvoeren van een AKS-cluster die ondersteuning biedt voor knooppuntgroepen voor Windows Server-containers, nodig voor het cluster te gebruiken een beleid voor netwerken die gebruikmaakt van [Azure CNI] [ azure-cni-about] (Geavanceerd) netwerk-invoegtoepassing. Zie voor meer informatie over het plannen van de vereiste subnet bereiken en de aandachtspunten voor netwerken, [configureren van netwerken van Azure CNI][use-advanced-networking]. Gebruik de [az aks maken] [ az-aks-create] opdracht voor het maken van een AKS-cluster met de naam *myAKSCluster*. Met deze opdracht wordt de vereiste netwerkresources maken als deze nog niet bestaan.
  * Het cluster is geconfigureerd met één knooppunt
  * De *windows beheerderswachtwoord* en *windows-beheerdersgebruikersnaam* -parameters de beheerdersreferenties ingesteld voor alle Windows Server-containers die zijn gemaakt op het cluster.

Geef uw eigen veilige *PASSWORD_WIN*.

```azurecli-interactive
PASSWORD_WIN="P@ssw0rd1234"

az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --enable-addons monitoring \
    --kubernetes-version 1.14.0 \
    --generate-ssh-keys \
    --windows-admin-password $PASSWORD_WIN \
    --windows-admin-username azureuser \
    --enable-vmss \
    --network-plugin azure
```

Na enkele minuten is de opdracht voltooid en retourneert deze informatie over het cluster in JSON-indeling.

## <a name="add-a-windows-server-node-pool"></a>De groep van een Windows Server-knooppunt toevoegen

Standaard wordt een AKS-cluster gemaakt met een knooppuntgroep die Linux-containers kan worden uitgevoerd. Gebruik `az aks nodepool add` opdracht voor het toevoegen van een extra knooppunt van toepassingen die Windows Server-containers kan worden uitgevoerd.

```azurecli
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --os-type Windows \
    --name npwin \
    --node-count 1 \
    --kubernetes-version 1.14.0
```

De bovenstaande opdracht maakt u een nieuwe knooppuntgroep van een met de naam *npwin* en deze toevoegt aan de *myAKSCluster*. Bij het maken van een knooppunt van toepassingen voor het uitvoeren van Windows Server-containers, de standaardwaarde voor *knooppunt-vm-grootte* is *Standard_D2s_v3*. Als u ervoor kiest om in te stellen de *knooppunt-vm-grootte* parameter, Controleer of de lijst met [beperkte VM-grootten][restricted-vm-sizes]. De minimaal aanbevolen grootte is *Standard_D2s_v3*. De bovenstaande opdracht gebruikt ook de standaard-subnet in de standaard-vnet gemaakt bij het uitvoeren van `az aks create`.

## <a name="connect-to-the-cluster"></a>Verbinding maken met het cluster

Als u een Kubernetes-cluster wilt beheren, gebruikt u [kubectl][kubectl], de Kubernetes-opdrachtregelclient. Als u Azure Cloud Shell gebruikt, is `kubectl` al geïnstalleerd. Als u `kubectl` lokaal wilt installeren, gebruikt u de opdracht [az aks install-cli][az-aks-install-cli]:

```azurecli
az aks install-cli
```

Gebruik de opdracht [az aks get-credentials][az-aks-get-credentials] om `kubectl` te configureren dat er verbinding wordt gemaakt met het Kubernetes-cluster. Bij deze opdracht worden referenties gedownload en wordt Kubernetes CLI geconfigureerd voor het gebruik van deze referenties.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Als u de verbinding met uw cluster wilt controleren, gebruikt u de opdracht [kubectl get][kubectl-get] om een lijst met clusterknooppunten te retourneren.

```azurecli-interactive
kubectl get nodes
```

In de volgende voorbeelduitvoer ziet u het enkele knooppunt dat is gemaakt in de vorige stappen. Zorg ervoor dat de status van het knooppunt *Ready* is:

```
NAME                                STATUS   ROLES   AGE    VERSION
aks-nodepool1-12345678-vmssfedcba   Ready    agent   13m    v1.14.0
aksnpwin987654                      Ready    agent   108s   v1.14.0
```

## <a name="run-the-application"></a>De toepassing uitvoeren

In een Kubernetes-manifestbestand wordt een gewenste status voor het cluster gedefinieerd, zoals welke containerinstallatiekopieën moeten worden uitgevoerd. In dit artikel wordt een manifest gebruikt om alle objecten die nodig zijn voor het uitvoeren van de ASP.NET-voorbeeld-App in een Windows Server-container te maken. Dit manifest bevat een [Kubernetes-implementatie] [ kubernetes-deployment] voor de ASP.NET-voorbeeld-App en een externe [Kubernetes-service] [ kubernetes-service] aan toegang tot de toepassing vanaf internet.

De ASP.NET-voorbeeld-App wordt geleverd als onderdeel van de [.NET Framework-voorbeelden] [ dotnet-samples] en wordt uitgevoerd in een Windows Server-container. AKS is vereist om te worden op basis van installatiekopieën van Windows Server-containers *Windows Server 2019* of hoger. De Kubernetes-manifestbestand moet ook definiëren een [knooppunt selector] [ node-selector] naar uw AKS-cluster om uit te voeren van uw ASP.NET-voorbeeldtoepassing schil op een knooppunt met Windows Server-containers kunt zien.

Maak een bestand met de naam `sample.yaml` en kopieer de volgende YAML-definitie naar het bestand. Als u Azure Cloud Shell gebruikt, kan dit bestand worden gemaakt met behulp van `vi` of `nano`, zoals bij een virtueel of fysiek systeem:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sample
  labels:
    app: sample
spec:
  replicas: 1
  template:
    metadata:
      name: sample
      labels:
        app: sample
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": windows
      containers:
      - name: sample
        image: mcr.microsoft.com/dotnet/framework/samples:aspnetapp
        resources:
          limits:
            cpu: 1
            memory: 800m
          requests:
            cpu: .1
            memory: 300m
        ports:
          - containerPort: 80
  selector:
    matchLabels:
      app: sample
---
apiVersion: v1
kind: Service
metadata:
  name: sample
spec:
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 80
  selector:
    app: sample
```

Implementeer de toepassing met de opdracht [kubectl apply][kubectl-apply] en geef de naam op van uw YAML-manifest:

```azurecli-interactive
kubectl apply -f sample.yaml
```

De volgende voorbeelduitvoer ziet u de implementatie en de Service is gemaakt:

```
deployment.apps/sample created
service/sample created
```

## <a name="test-the-application"></a>De toepassing testen

Wanneer de toepassing wordt uitgevoerd, maakt een Kubernetes-service de front-end van de toepassing beschikbaar op internet. Dit proces kan enkele minuten duren.

Gebruik de opdracht [kubectl get service][kubectl-get] met het argument `--watch` om de voortgang te controleren.

```azurecli-interactive
kubectl get service sample --watch
```

In eerste instantie de *externe IP-adres* voor de *voorbeeld* service wordt weergegeven als *in behandeling*.

```
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
sample             LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

Zodra het *EXTERNAL-IP*-adres is gewijzigd van *in behandeling* in een echt openbaar IP-adres, gebruikt u `CTRL-C` om het controleproces van `kubectl` te stoppen. In de volgende voorbeelduitvoer ziet u een geldig openbaar IP-adres dat aan de service is toegewezen:

```
sample  LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Als u wilt de voorbeeld-app in actie zien, open een webbrowser naar het externe IP-adres van uw service.

![Afbeelding van bladeren naar de voorbeeld-ASP.NET-toepassing](media/windows-container/asp-net-sample-app.png)

## <a name="delete-cluster"></a>Cluster verwijderen

Gebruik de opdracht [az group delete][az-group-delete] om de resourcegroep, de containerservice en alle gerelateerde resources te verwijderen wanneer u het cluster niet meer nodig hebt.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Wanneer u het cluster verwijdert, wordt de Azure Active Directory-service-principal die door het AKS-cluster wordt gebruikt niet verwijderd. Zie [Overwegingen voor en verwijdering van AKS service-principal][sp-delete] voor stappen voor het verwijderen van de service-principal.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een Kubernetes-cluster geïmplementeerd en een ASP.NET-voorbeeld-toepassing in een Windows Server-container geïmplementeerd. [Open het Kubernetes-webdashboard][kubernetes-dashboard] voor het cluster dat u zojuist hebt gemaakt.

Voor meer informatie over AKS en een volledig stapsgewijs voorbeeld van code tot implementatie gaat u naar de zelfstudie over Kubernetes-clusters.

> [!div class="nextstepaction"]
> [AKS-zelfstudie][aks-tutorial]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[node-selector]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[dotnet-samples]: https://hub.docker.com/_/microsoft-dotnet-framework-samples/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md

<!-- LINKS - internal -->
[kubernetes-concepts]: concepts-clusters-workloads.md
[aks-monitor]: https://aka.ms/coingfonboarding
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-browse]: /cli/azure/aks?view=azure-cli-latest#az-aks-browse
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[az-provider-register]: /cli/azure/provider#az-provider-register
[azure-cli-install]: /cli/azure/install-azure-cli
[azure-cni-about]: concepts-network.md#azure-cni-advanced-networking
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[azure-portal]: https://portal.azure.com
[kubernetes-deployment]: concepts-clusters-workloads.md#deployments-and-yaml-manifests
[kubernetes-service]: concepts-network.md#services
[kubernetes-dashboard]: kubernetes-dashboard.md
[restricted-vm-sizes]: quotas-skus-regions.md#restricted-vm-sizes
[use-advanced-networking]: configure-advanced-networking.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md

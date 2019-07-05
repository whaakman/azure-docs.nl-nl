---
title: Preview - gebruik van een standaard-SKU load balancer in Azure Kubernetes Service (AKS)
description: Meer informatie over het gebruik van een load balancer met een standaard-SKU om beschikbaar te stellen van uw services met Azure Kubernetes Service (AKS).
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 06/25/2019
ms.author: zarhoads
ms.openlocfilehash: a9cf3db3a15fab5a2f067a146950e02923a20379
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476814"
---
# <a name="preview---use-a-standard-sku-load-balancer-in-azure-kubernetes-service-aks"></a>Preview - gebruik van een standaard-SKU load balancer in Azure Kubernetes Service (AKS)

Om toegang te bieden aan uw toepassingen in Azure Kubernetes Service (AKS), kunt u maken en gebruiken van een Azure Load Balancer. Een load balancer die wordt uitgevoerd op AKS kan worden gebruikt als een interne of een externe load balancer. Een interne load balancer maakt een Kubernetes-service alleen toegankelijk is voor toepassingen die worden uitgevoerd in hetzelfde virtuele netwerk bevinden als het AKS-cluster. Een externe load balancer ontvangt van een of meer openbare IP-adressen voor inkomend verkeer en maakt een Kubernetes-service die toegankelijk is extern via het openbare IP-adressen.

Azure Load Balancer is beschikbaar in twee SKU's - *Basic* en *Standard*. Standaard de *Basic* SKU wordt gebruikt wanneer een servicemanifest wordt gebruikt voor het maken van een load balancer in AKS. Met behulp van een *Standard* SKU load balancer biedt aanvullende functies en functionaliteit, zoals groter voor back-end-pool en Beschikbaarheidszones. Het is belangrijk dat u bekend bent met de verschillen tussen *Standard* en *Basic* netwerktaakverdelers voordat u kiest voor het gebruik. Als u een AKS-cluster maakt, kunt u de load balancer SKU voor dat cluster niet wijzigen. Voor meer informatie over de *Basic* en *Standard* SKU's, Zie [vergelijking van Azure load balancer SKU][azure-lb-comparison].

Dit artikel leest u hoe het maken en gebruiken van een Azure Load Balancer met de *Standard* SKU met Azure Kubernetes Service (AKS).

In dit artikel wordt ervan uitgegaan van basiskennis van Kubernetes en Azure Load Balancer-concepten. Zie voor meer informatie, [Kubernetes core concepten voor Azure Kubernetes Service (AKS)][kubernetes-concepts] and [What is Azure Load Balancer?][azure-lb].

Deze functie is momenteel beschikbaar als preview-product.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om te installeren en de CLI lokaal gebruikt, in dit artikel is vereist dat u de Azure CLI versie 2.0.59 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren][install-azure-cli] als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="before-you-begin"></a>Voordat u begint

De AKS-clusterservice-principal moet de machtiging voor het beheren van netwerkbronnen als u een bestaand subnet of de resourcegroep. In het algemeen toewijzen de *Inzender voor netwerken* rol aan uw service-principal op de gedeelde resources. Zie voor meer informatie over machtigingen [AKS gemachtigde toegang tot andere Azure-resources][aks-sp].

U moet een AKS-cluster dat Hiermee stelt u de SKU voor de load balancer maken *Standard* in plaats van de standaard *Basic*. Het maken van een AKS-cluster in een latere stap wordt behandeld, maar u moet eerst een paar preview-functies inschakelen.

> [!IMPORTANT]
> AKS-preview-functies zijn selfservice, aanmelden. Ze zijn bedoeld om het verzamelen van fouten en feedback van onze community. Preview-versie, worden deze functies zijn niet bedoeld voor gebruik in productieomgevingen. Functies in public preview vallen onder 'best effort'-ondersteuning. Hulp van de AKS-teams voor technische ondersteuning is beschikbaar tijdens kantooruren Pacific tijdzone (PST) alleen. Zie de volgende artikelen ondersteuning voor aanvullende informatie:
>
> * [Ondersteuningsbeleid voor AKS][aks-support-policies]
> * [Veelgestelde vragen over ondersteuning van Azure][aks-faq]

### <a name="install-aks-preview-cli-extension"></a>Aks-preview CLI-extensie installeren

Gebruik de Azure load balancer standaard-SKU, moet u de *aks-preview* CLI versie van de extensie 0.4.1 of hoger. Installeer de *aks-preview* Azure CLI-extensie met de [az-extensie toevoegen][az-extension-add] command, then check for any available updates using the [az extension update][az-extension-update] opdracht::

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-aksazurestandardloadbalancer-preview-feature"></a>Preview-functie AKSAzureStandardLoadBalancer registreren

Het maken van een AKS-cluster met een load balancer met de *Standard* SKU, moet u inschakelen de *AKSAzureStandardLoadBalancer* functie vlag aan uw abonnement. De *AKSAzureStandardLoadBalancer* functie ook gebruikt *VMSSPreview* bij het maken van een cluster met behulp van schaalsets voor virtuele machines. Deze functie biedt de nieuwste set serviceverbeteringen bij het configureren van een cluster. Terwijl het niet vereist is, wordt het aanbevolen u inschakelt de *VMSSPreview* functie ook de vlag.

> [!CAUTION]
> Als u een functie op een abonnement registreert, kunt u niet op dit moment opheffen van de registratie die functie. Nadat u een preview-functies ingeschakeld, kunnen de standaardwaarden worden gebruikt voor alle AKS-clusters wordt gemaakt in het abonnement. Geen preview-functies voor productieabonnementen niet inschakelen. Gebruik een afzonderlijk abonnement voor het testen van de preview-functies en verzamelen van feedback.

Registreert de *VMSSPreview* en *AKSAzureStandardLoadBalancer* functie worden gemarkeerd met behulp van de [az functie registreren][az-feature-register] opdracht zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive
az feature register --namespace "Microsoft.ContainerService" --name "VMSSPreview"
az feature register --namespace "Microsoft.ContainerService" --name "AKSAzureStandardLoadBalancer"
```

> [!NOTE]
> Een AKS-cluster dat u maakt nadat u hebt geregistreerd de *VMSSPreview* of *AKSAzureStandardLoadBalancer* functie vlaggen gebruiken deze preview-cluster-ervaring. Als u wilt doorgaan met het maken van clusters van reguliere, volledig ondersteunde, geen preview-functies voor productieabonnementen inschakelen. Gebruik een afzonderlijke test- en Azure-abonnement voor het testen van de preview-functies.

Het duurt enkele minuten duren voordat de status om weer te geven *geregistreerde*. U kunt controleren op de registratie van status met behulp van de [az Functielijst][az-feature-list] opdracht:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/VMSSPreview')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKSAzureStandardLoadBalancer')].{Name:name,State:properties.state}"
```

Wanneer u klaar bent, vernieuwt u de registratie van de *Microsoft.ContainerService* resourceprovider met behulp van de [az provider register][az-provider-register] opdracht:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

### <a name="limitations"></a>Beperkingen

De volgende beperkingen zijn van toepassing wanneer u maken en beheren van AKS-clusters die ondersteuning bieden voor een load balancer met de *Standard* SKU:

* Wanneer u de *Standard* SKU voor een load balancer, die u moet openbare adressen toestaan en Vermijd het maken van een Azure-beleid dat het maken van IP-verboden. Het AKS-cluster maakt automatisch een *Standard* SKU en openbare IP in dezelfde resourcegroep gemaakt voor het AKS-cluster, die doorgaans met de naam met *MC_* aan het begin. AKS wijst het openbare IP-adres aan de *Standard* SKU load balancer. Het openbare IP-adres is vereist voor het toestaan van uitgaande verkeer vanuit het AKS-cluster. Dit openbare IP-adres is ook vereist voor het onderhouden van verbinding tussen de besturingselement vlak- en agentknooppunten evenals garantie voor compatibiliteit met eerdere versies van AKS.
* Wanneer u de *Standard* SKU voor een load balancer, moet u Kubernetes versie 1.13.5 of hoger.

Hoewel deze functie nog in preview, gelden de volgende aanvullende beperkingen:

* Wanneer u de *Standard* SKU voor een load balancer in AKS, u kunt uw eigen openbare IP-adres voor uitgaand verkeer niet instellen voor de load balancer. U moet het IP-adres dat AKS wordt toegewezen aan uw load balancer.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een Azure-resourcegroep is een logische groep waarin Azure-resources worden geïmplementeerd en beheerd. Wanneer u een resourcegroep maakt, wordt u gevraagd een locatie op te geven. Deze locatie is waar de metagegevens van de resource is opgeslagen, het is ook waar uw resources in Azure uitvoeren als u een andere regio geen tijdens het maken van resources opgeeft. Maak een resource-groep met de [az-groep maken][az-group-create] opdracht.

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
Als u wilt uitvoeren van een AKS-cluster die ondersteuning biedt voor een load balancer met de *Standard* SKU, uw cluster nodig heeft om in te stellen de *load-balancer-sku* parameter *standard*. Deze parameter wordt een load balancer met de *Standard* SKU wanneer uw cluster is gemaakt. Bij het uitvoeren van een *LoadBalancer* service in uw cluster, de configuratie van de *Standard* SK load balancer wordt bijgewerkt met de configuratie van de service. Gebruik de [az aks maken][az-aks-create] opdracht voor het maken van een AKS-cluster met de naam *myAKSCluster*.

> [!NOTE]
> De *load-balancer-sku* eigenschap kan alleen worden gebruikt wanneer uw cluster is gemaakt. U kunt de load balancer SKU niet wijzigen nadat een AKS-cluster is gemaakt. U kunt ook alleen een type load balancer SKU in één cluster gebruiken.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --enable-vmss \
    --node-count 1 \
    --kubernetes-version 1.14.0 \
    --load-balancer-sku standard \
    --generate-ssh-keys
```

Na enkele minuten is de opdracht voltooid en retourneert deze informatie over het cluster in JSON-indeling.

## <a name="connect-to-the-cluster"></a>Verbinding maken met het cluster

Voor het beheren van een Kubernetes-cluster, gebruikt u [kubectl][kubectl], de Kubernetes-opdrachtregelclient. Als u Azure Cloud Shell gebruikt, is `kubectl` al geïnstalleerd. Voor het installeren van `kubectl` lokaal, gebruikt u de [az aks install-cli][az-aks-install-cli] opdracht:

```azurecli
az aks install-cli
```

Gebruik de opdracht [az aks get-credentials][az-aks-get-credentials] om `kubectl` zodanig te configureren dat er verbinding wordt gemaakt met het Kubernetes-cluster. Bij deze opdracht worden referenties gedownload en wordt Kubernetes CLI geconfigureerd voor het gebruik van deze referenties.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Als u de verbinding met uw cluster wilt controleren, gebruikt u de opdracht [kubectl get][kubectl-get] om een lijst met clusterknooppunten te retourneren.

```azurecli-interactive
kubectl get nodes
```

In de volgende voorbeelduitvoer ziet u het enkele knooppunt dat is gemaakt in de vorige stappen. Zorg ervoor dat de status van het knooppunt *Ready* is:

```
NAME                       STATUS   ROLES   AGE     VERSION
aks-nodepool1-31718369-0   Ready    agent   6m44s   v1.14.0
```

## <a name="verify-your-cluster-uses-the-standard-sku"></a>Controleer of het cluster maakt gebruik van de *Standard* SKU

Gebruik de [az aks show][az-aks-show] om de configuratie van uw cluster weer te geven.

```console
$ az aks show --resource-group myResourceGroup --name myAKSCluster

{
  "aadProfile": null,
  "addonProfiles": null,
   ...
   "networkProfile": {
    "dnsServiceIp": "10.0.0.10",
    "dockerBridgeCidr": "172.17.0.1/16",
    "loadBalancerSku": "standard",
    ...
```

Controleer of de *loadBalancerSku* eigenschap wordt weergegeven als *standard*.

## <a name="use-the-load-balancer"></a>De load balancer gebruiken

Voor het gebruik van de load balancer in uw cluster, kunt u een servicemanifest maken met het servicetype *LoadBalancer*. Als u wilt weergeven van de load balancer werken, manifest voor een andere met een voorbeeldtoepassing uit te voeren op uw cluster te maken. Deze voorbeeldtoepassing wordt beschikbaar gemaakt via de load balancer en kan worden weergegeven via een browser.

Maken van een manifestbestand met de naam `sample.yaml` zoals wordt weergegeven in het volgende voorbeeld:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-back
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": linux
      containers:
      - name: azure-vote-back
        image: redis
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-front
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": linux
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:v1
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
```

Het manifest van de bovenstaande twee implementaties configureert: *azure-vote-front* en *azure-vote-back*. Het configureren van *azure-vote-front* implementatie beschikbaar wordt gemaakt met behulp van de load balancer, maak een manifestbestand met de naam `standard-lb.yaml` zoals wordt weergegeven in het volgende voorbeeld:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

De service *azure-vote-front* maakt gebruik van de *LoadBalancer* type voor het configureren van de load balancer op uw AKS-cluster verbinding maken met de *azure-vote-front* implementatie.

De voorbeeldtoepassing implementeren en load balancer met behulp van de [kubectl toepassen][kubectl-apply] en geef de naam van de manifesten YAML:

```console
kubectl apply -f sample.yaml
kubectl apply -f standard-lb.yaml
```

De *Standard* SKU load balancer is nu geconfigureerd om de voorbeeld-App weer te geven. Bekijk de servicedetails van *azure-vote-front* met behulp van [kubectl ophalen][kubectl-get] om te zien van het openbare IP-adres van de load balancer. Het openbare IP-adres van de load balancer wordt weergegeven in de *externe IP-adres* kolom. Het duurt een minuut of twee voor het IP-adres te wijzigen van *\<in behandeling\>* een werkelijke externe IP-adres, zoals weergegeven in het volgende voorbeeld:

```
$ kubectl get service azure-vote-front

NAME                TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE
azure-vote-front    LoadBalancer   10.0.227.198   52.179.23.131   80:31201/TCP   16s
```

Navigeer naar het openbare IP-adres in een browser en controleer of dat u ziet de voorbeeldtoepassing. In het bovenstaande voorbeeld wordt het openbare IP-adres is `52.179.23.131`.

![Afbeelding van browsen naar Azure Vote](media/container-service-kubernetes-walkthrough/azure-vote.png)

> [!NOTE]
> U kunt ook zo configureren dat de load balancer voor interne en een openbaar IP-adres niet beschikbaar. Voor het configureren van de load balancer als interne, toevoegen `service.beta.kubernetes.io/azure-load-balancer-internal: "true"` als een aantekening aan de *LoadBalancer* service. U ziet een voorbeeld-yaml manifest als ook meer informatie over een interne load balancer [hier][internal-lb-yaml].

## <a name="clean-up-the-standard-sku-load-balancer-configuration"></a>Opschonen van de standaard-SKU load balancer-configuratie

Voor het verwijderen van de voorbeeld-toepassing en de load balancer-configuratie, gebruikt u [kubectl verwijderen][kubectl-delete]:

```console
kubectl delete -f sample.yaml
kubectl delete -f standard-lb.yaml
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Kubernetes-services op de [documentatie voor Kubernetes services][kubernetes-services].

<!-- LINKS - External -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/
[aks-engine]: https://github.com/Azure/aks-engine

<!-- LINKS - Internal -->
[advanced-networking]: configure-azure-cni.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[aks-sp]: kubernetes-service-principal.md#delegate-access-to-other-azure-resources
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-group-create]: /cli/azure/group#az-group-create
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[azure-lb]: ../load-balancer/load-balancer-overview.md
[azure-lb-comparison]: ../load-balancer/load-balancer-overview.md#skus
[install-azure-cli]: /cli/azure/install-azure-cli
[internal-lb-yaml]: internal-lb.md#create-an-internal-load-balancer
[kubernetes-concepts]: concepts-clusters-workloads.md
[use-kubenet]: configure-kubenet.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update

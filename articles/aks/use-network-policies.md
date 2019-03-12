---
title: Beveiligde schillen met beleid voor netwerken in Azure Kubernetes Service (AKS)
description: Meer informatie over het beveiligen van verkeer dat in en uit schillen stromen met behulp van het netwerkbeleid Kubernetes in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 02/12/2019
ms.author: iainfou
ms.openlocfilehash: 81b45a25c8040916b835ab333c5ce80ab6c1a788
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57772310"
---
# <a name="secure-traffic-between-pods-by-using-network-policies-in-azure-kubernetes-service"></a>Beveiliging van verkeer tussen schillen met behulp van beleid voor netwerken in Azure Kubernetes Service

Wanneer u een moderne, op basis van microservices-toepassingen in Kubernetes uitvoeren, wilt u meestal om te bepalen welke onderdelen met elkaar kunnen communiceren. Het principe van minimale bevoegdheden moet worden toegepast op hoe verkeer tussen de schillen in een cluster Azure Kubernetes Service (AKS stromen kan). Stel dat u waarschijnlijk wilt blokkeren van verkeer rechtstreeks naar de back-end-toepassingen. De *netwerkbeleid* functie in Kubernetes kunt u regels definiëren voor inkomend en uitgaand verkeer tussen de schillen in een cluster.

Calico, een open-source-netwerk- en netwerk-beveiligingsoplossing die is opgericht door Tigera, biedt een netwerk groepsbeleid-engine die beleidsregels voor Kubernetes netwerk kunt implementeren. Dit artikel leest u hoe de beleidsengine Calico netwerk installeren en Kubernetes netwerk beleid om te bepalen van de verkeersstroom tussen de schillen in AKS.

> [!IMPORTANT]
> Deze functie is momenteel beschikbaar als preview-product. Preview-versies worden beschikbaar gesteld op voorwaarde dat u akkoord gaat met de [aanvullende gebruiksvoorwaarden][terms-of-use]. Sommige aspecten van deze functie kunnen worden gewijzigd voorafgaand aan de algemene beschikbaarheid (GA).

## <a name="before-you-begin"></a>Voordat u begint

U moet de Azure CLI versie 2.0.56 of later geïnstalleerd en geconfigureerd. Voer  `az --version` uit om de versie te bekijken. Als u de Azure CLI wilt installeren of upgraden, raadpleegt u  [Azure CLI installeren][install-azure-cli].

Voor het maken van een AKS-cluster die netwerkbeleid kunt gebruiken, moet u eerst een functievlag voor uw abonnement inschakelen. Om u te registreren de *EnableNetworkPolicy* vlag functie, gebruikt u de [az functie registreren] [ az-feature-register] opdracht zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive
az feature register --name EnableNetworkPolicy --namespace Microsoft.ContainerService
```

Het duurt enkele minuten duren voordat de status om weer te geven *geregistreerde*. U kunt de registratiestatus controleren met behulp van de [az Functielijst] [ az-feature-list] opdracht:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/EnableNetworkPolicy')].{Name:name,State:properties.state}"
```

Wanneer u klaar bent, vernieuwt u de registratie van de *Microsoft.ContainerService* resourceprovider met behulp van de [az provider register] [ az-provider-register] opdracht:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="overview-of-network-policy"></a>Overzicht van beleid voor netwerken

Alle schillen in een AKS-cluster kunnen verzenden en ontvangen van verkeer zonder beperkingen in de standaard. U kunt regels waarmee de verkeersstroom definiëren voor een betere beveiliging. Back-end-toepassingen zijn vaak alleen blootgesteld aan de vereiste front-end-services, bijvoorbeeld. Of databaseonderdelen zijn alleen toegankelijk is voor de toepassingslagen die verbinding met deze maken.

Beleid voor netwerken worden Kubernetes-resources die u kunnen de verkeersstroom tussen schillen beheren. U kunt toestaan of weigeren van verkeer op basis van instellingen, zoals toegewezen labels, naamruimte of verkeer poort. Beleid voor netwerken zijn gedefinieerd zoals YAML-manifesten. Deze beleidsregels kunnen worden opgenomen als onderdeel van een bredere manifest dat ook een implementatie of de service maakt zijn.

Beleid voor netwerken in actie, laten we zien maken en vouw vervolgens in een beleid dat verkeersstroom definieert:

* Al het verkeer naar pod weigeren.
* Toestaan van verkeer op basis van de schil labels.
* Toestaan van verkeer op basis van de naamruimte.

## <a name="create-an-aks-cluster-and-enable-network-policy"></a>Een AKS-cluster maken en inschakelen van beleid voor netwerken

Netwerkbeleid kan alleen worden ingeschakeld wanneer het cluster is gemaakt. U kunt beleid voor netwerken in een bestaand AKS-cluster niet inschakelen. 

Voor het gebruik van beleid voor netwerken met een AKS-cluster, moet u de [Azure CNI invoegtoepassing] [ azure-cni] en uw eigen virtuele netwerk en subnetten definiëren. Zie voor meer informatie over het plannen van de vereiste subnet bereiken, [geavanceerde netwerken configureren][use-advanced-networking].

Het volgende voorbeeldscript:

* Hiermee maakt u een virtueel netwerk en subnet.
* Maakt een Azure Active Directory (Azure AD) service-principal voor gebruik met het AKS-cluster.
* Toegewezen *Inzender* machtigingen voor de AKS-cluster service-principal in het virtuele netwerk.
* Hiermee maakt u een AKS-cluster in het opgegeven virtuele netwerk en kunt netwerkbeleid.

Geef uw eigen veilige *SP_PASSWORD*. U kunt vervangen de *RESOURCE_GROUP_NAME* en *clusternaam* variabelen:

```azurecli-interactive
SP_PASSWORD=mySecurePassword
RESOURCE_GROUP_NAME=myResourceGroup-NP
CLUSTER_NAME=myAKSCluster
LOCATION=canadaeast

# Create a resource group
az group create --name $RESOURCE_GROUP_NAME --location $LOCATION

# Create a virtual network and subnet
az network vnet create \
    --resource-group $RESOURCE_GROUP_NAME \
    --name myVnet \
    --address-prefixes 10.0.0.0/8 \
    --subnet-name myAKSSubnet \
    --subnet-prefix 10.240.0.0/16

# Create a service principal and read in the application ID
SP_ID=$(az ad sp create-for-rbac --password $SP_PASSWORD --skip-assignment --query [appId] -o tsv)

# Wait 15 seconds to make sure that service principal has propagated
echo "Waiting for service principal to propagate..."
sleep 15

# Get the virtual network resource ID
VNET_ID=$(az network vnet show --resource-group $RESOURCE_GROUP_NAME --name myVnet --query id -o tsv)

# Assign the service principal Contributor permissions to the virtual network resource
az role assignment create --assignee $SP_ID --scope $VNET_ID --role Contributor

# Get the virtual network subnet resource ID
SUBNET_ID=$(az network vnet subnet show --resource-group $RESOURCE_GROUP_NAME --vnet-name myVnet --name myAKSSubnet --query id -o tsv)

# Create the AKS cluster and specify the virtual network and service principal information
# Enable network policy by using the `--network-policy` parameter
az aks create \
    --resource-group $RESOURCE_GROUP_NAME \
    --name $CLUSTER_NAME \
    --node-count 1 \
    --kubernetes-version 1.12.6 \
    --generate-ssh-keys \
    --network-plugin azure \
    --service-cidr 10.0.0.0/16 \
    --dns-service-ip 10.0.0.10 \
    --docker-bridge-address 172.17.0.1/16 \
    --vnet-subnet-id $SUBNET_ID \
    --service-principal $SP_ID \
    --client-secret $SP_PASSWORD \
    --network-policy calico
```

Het duurt een paar minuten om het cluster te maken. Wanneer het cluster gereed is, configureert u `kubectl` verbinding maken met uw Kubernetes-cluster met behulp van de [az aks get-credentials] [ az-aks-get-credentials] opdracht. Met deze opdracht worden referenties gedownload en configureert u de Kubernetes-CLI voor het gebruik ervan:

```azurecli-interactive
az aks get-credentials --resource-group $RESOURCE_GROUP_NAME --name $CLUSTER_NAME
```

## <a name="deny-all-inbound-traffic-to-a-pod"></a>Alle binnenkomend verkeer naar een schil weigeren

Voordat u regels definieert voor het specifieke netwerkverkeer toestaan, moet u eerst een beleid voor het weigeren van al het verkeer voor netwerken maken. Dit beleid biedt u een startpunt om te beginnen aan lijst met geaccepteerde alleen het gewenste verkeer. U kunt ook een duidelijk zien dat verkeer wordt verwijderd wanneer het beleid voor netwerken wordt toegepast.

Voor de voorbeeld-toepassingsomgeving en regels voor netwerkverkeer, laten we eerst een naamruimte maken met de naam *ontwikkeling* om uit te voeren van de voorbeeld-schillen:

```console
kubectl create namespace development
kubectl label namespace/development purpose=development
```

Maak een voorbeeld van de back-end-schil waarop NGINX wordt uitgevoerd. Deze back-end-schil kan worden gebruikt voor het simuleren van een back-end web gebaseerde voorbeeldtoepassing. Maken van deze pod in de *ontwikkeling* naamruimte, en open poort *80* om webverkeer te genereren. De schil met een label *app = webapp, rol = back-end* zodat we kunnen het doel met een beleid voor netwerken in de volgende sectie:

```console
kubectl run backend --image=nginx --labels app=webapp,role=backend --namespace development --expose --port 80 --generator=run-pod/v1
```

Een andere pod maken en koppelen van een terminalsessie als u wilt testen dat u de standaard-NGINX-webpagina met succes kunt bereiken:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

Gebruik op de shell-prompt `wget` om te bevestigen dat u toegang hebt tot de standaard-NGINX-webpagina:

```console
wget -qO- http://backend
```

De volgende voorbeelduitvoer ziet u dat de standaard-NGINX-webpagina is geretourneerd:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

De gekoppelde terminalsessie afsluiten. De test-schil wordt automatisch verwijderd.

```console
exit
```

### <a name="create-and-apply-a-network-policy"></a>Maken en toepassen van een beleid voor netwerken

Nu dat u hebt vastgesteld dat kunt u de standaard NGINX-webpagina op de voorbeeld-back-end-schil, maakt u een beleid voor netwerken voor het weigeren van al het verkeer. Maak een bestand met de naam `backend-policy.yaml` en plak de volgende YAML-manifest. Maakt gebruik van deze manifest een *podSelector* koppelen van het beleid aan schillen waarvoor de *app:webapp, rol: back-end* label, zoals uw voorbeeld NGINX-schil. Er zijn geen regels gedefinieerd onder *inkomend*, zodat al het binnenkomende verkeer naar de schil wordt geweigerd:

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: backend-policy
  namespace: development
spec:
  podSelector:
    matchLabels:
      app: webapp
      role: backend
  ingress: []
```

Het netwerkbeleid toepassen met behulp van de [kubectl toepassen] [ kubectl-apply] opdracht en geeft u de naam van uw YAML-manifest:

```azurecli-interactive
kubectl apply -f backend-policy.yaml
```

### <a name="test-the-network-policy"></a>Testen van het beleid voor netwerken


Laten we zien als u kunt de NGINX-webpagina op de back-end-schil opnieuw gebruiken. Een andere test-schil maken en koppelen van een terminal-sessie:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

Gebruik op de shell-prompt `wget` om te zien of u toegang hebt tot de standaard-NGINX-webpagina. Dit moment een time-outwaarde ingesteld op *2* seconden. Het beleid voor netwerken nu blokkeert al het binnenkomende verkeer, zodat de pagina kan niet worden geladen, zoals wordt weergegeven in het volgende voorbeeld:

```console
$ wget -qO- --timeout=2 http://backend

wget: download timed out
```

De gekoppelde terminalsessie afsluiten. De test-schil wordt automatisch verwijderd.

```console
exit
```

## <a name="allow-inbound-traffic-based-on-a-pod-label"></a>Binnenkomend verkeer op basis van een label pod toestaan

In de vorige sectie, een back-end NGINX-schil is gepland en een netwerkbeleid is gemaakt voor het weigeren van al het verkeer. Laten we een front-end-schil maken en bijwerken van het beleid voor netwerken voor verkeer van de front-end-schillen.

Bijwerken van het beleid voor netwerken voor verkeer van schillen met de labels *app:webapp, rol: frontend* en in een naamruimte. Bewerk de vorige *back-end-policy.yaml* bestand en voeg toe *matchLabels* ingress regels zodat uw manifest ziet als in het volgende voorbeeld eruit:

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: backend-policy
  namespace: development
spec:
  podSelector:
    matchLabels:
      app: webapp
      role: backend
  ingress:
  - from:
    - namespaceSelector: {}
      podSelector:
        matchLabels:
          app: webapp
          role: frontend
```

> [!NOTE]
> Dit netwerkbeleid maakt gebruik van een *namespaceSelector* en een *podSelector* element voor de regel voor inkomend verkeer. De syntaxis van de YAML is belangrijk voor de ingress regels zijn additief. In dit voorbeeld moeten overeenkomen met beide elementen voor de regel voor inkomend verkeer moet worden toegepast. Kubernetes-versies voorafgaand aan *1.12* niet mogelijk deze elementen correct worden geïnterpreteerd en het netwerkverkeer beperken, zoals verwacht. Zie voor meer informatie over dit gedrag, [gedrag van en naar het selectoren][policy-rules].

De bijgewerkte netwerkbeleid toepassen met behulp van de [kubectl toepassen] [ kubectl-apply] opdracht en geeft u de naam van uw YAML-manifest:

```azurecli-interactive
kubectl apply -f backend-policy.yaml
```

Plannen van een schil die is gelabeld als *app = Web-App, rol = frontend* en het koppelen van een terminal-sessie:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace development --generator=run-pod/v1
```

Gebruik op de shell-prompt `wget` om te zien of u toegang hebt tot de standaard-NGINX-webpagina:

```console
wget -qO- http://backend
```

Omdat de regel voor inkomend verkeer met schillen waarvoor de labels kan *app: Web-App, rol: frontend*, het verkeer van de front-end-schil is toegestaan. De volgende voorbeelduitvoer ziet u de standaard-NGINX-webpagina geretourneerd:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

De gekoppelde terminalsessie afsluiten. De schil wordt automatisch verwijderd.

```console
exit
```

### <a name="test-a-pod-without-a-matching-label"></a>Testen van een schil zonder een bijbehorende label

Het netwerkbeleid kan verkeer van schillen met het label *app: Web-App, rol: frontend*, maar al het andere verkeer te weigeren. We gaan testen om te zien of een andere pod zonder deze labels toegang heeft tot de back-end NGINX-schil. Een andere test-schil maken en koppelen van een terminal-sessie:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

Gebruik op de shell-prompt `wget` om te zien of u toegang hebt tot de standaard-NGINX-webpagina. Het netwerkbeleid blokkeert het inkomende verkeer, zodat de pagina kan niet worden geladen, zoals wordt weergegeven in het volgende voorbeeld:

```console
$ wget -qO- --timeout=2 http://backend

wget: download timed out
```

De gekoppelde terminalsessie afsluiten. De test-schil wordt automatisch verwijderd.

```console
exit
```

## <a name="allow-traffic-only-from-within-a-defined-namespace"></a>Toestaan dat alleen verkeer van binnen een gedefinieerde naamruimte

In de vorige voorbeelden kunt u een netwerkbeleid gemaakt waarmee al het verkeer geweigerd en wordt het beleid voor het toestaan van verkeer van schillen met een specifiek label wordt bijgewerkt. Een andere algemene nodig is om te beperken van verkeer naar alleen binnen een bepaalde naamruimte. Als de vorige voorbeelden zijn voor verkeer in een *ontwikkeling* naamruimte, maken van een beleid voor netwerken waarmee wordt voorkomen verkeer van een andere naamruimte, zoals dat *productie*, de schillen bereikt.

Maak eerst een nieuwe naamruimte voor het simuleren van een productie-naamruimte:

```console
kubectl create namespace production
kubectl label namespace/production purpose=production
```

Plannen van een test-schil in de *productie* naamruimte die wordt aangeduid als *app = webapp, rol = frontend*. Koppel een terminal-sessie:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace production --generator=run-pod/v1
```

Gebruik op de shell-prompt `wget` om te bevestigen dat u toegang hebt tot de standaard-NGINX-webpagina:

```console
wget -qO- http://backend.development
```

Omdat de labels voor de schil overeenkomen met wat in het beleid voor netwerken op dit moment is toegestaan, wordt het verkeer is toegestaan. Het beleid voor netwerken kijken niet naar de naamruimten, alleen de schil labels. De volgende voorbeelduitvoer ziet u de standaard-NGINX-webpagina geretourneerd:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

De gekoppelde terminalsessie afsluiten. De test-schil wordt automatisch verwijderd.

```console
exit
```

### <a name="update-the-network-policy"></a>Bijwerken van het beleid voor netwerken

We werken de regel voor inkomend verkeer *namespaceSelector* sectie om toe te staan alleen verkeer vanuit het *ontwikkeling* naamruimte. Bewerk de *back-end-policy.yaml* manifestbestand zoals wordt weergegeven in het volgende voorbeeld:

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: backend-policy
  namespace: development
spec:
  podSelector:
    matchLabels:
      app: webapp
      role: backend
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          purpose: development
      podSelector:
        matchLabels:
          app: webapp
          role: frontend
```

In complexere voorbeelden, kunt u definiëren meerdere ingress regels, zoals een *namespaceSelector* en vervolgens een *podSelector*.

De bijgewerkte netwerkbeleid toepassen met behulp van de [kubectl toepassen] [ kubectl-apply] opdracht en geeft u de naam van uw YAML-manifest:

```azurecli-interactive
kubectl apply -f backend-policy.yaml
```

### <a name="test-the-updated-network-policy"></a>De bijgewerkte netwerkbeleid testen

Plannen van een andere pod in de *productie* naamruimte en het koppelen van een terminal-sessie:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace production --generator=run-pod/v1
```

Gebruik op de shell-prompt `wget` om te zien dat het beleid voor netwerken nu verkeer weigert:

```console
$ wget -qO- --timeout=2 http://backend.development

wget: download timed out
```

De test-schil afsluiten:

```console
exit
```

Met verkeer geweigerd vanuit de *productie* naamruimte, schema een test-schil back-ups maken de *ontwikkeling* naamruimte en het koppelen van een terminal-sessie:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace development --generator=run-pod/v1
```

Gebruik op de shell-prompt `wget` om te zien dat het netwerkbeleid het verkeer toestaat:

```console
wget -qO- http://backend
```

Verkeer is toegestaan omdat de schil is gepland in de naamruimte die overeenkomt met wat toegestaan in het beleid voor netwerken. De volgende voorbeelduitvoer ziet u de standaard-NGINX-webpagina geretourneerd:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

De gekoppelde terminalsessie afsluiten. De test-schil wordt automatisch verwijderd.

```console
exit
```

## <a name="clean-up-resources"></a>Resources opschonen

In dit artikel, we twee naamruimten die zijn gemaakt en een netwerkbeleid toegepast. Voor het opschonen van deze resources, gebruikt u de [kubectl verwijderen] [ kubectl-delete] opdracht en geeft u de namen van voorbeeldresources:

```console
kubectl delete namespace production
kubectl delete namespace development
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over netwerkbronnen, [concepten voor toepassingen in Azure Kubernetes Service (AKS) netwerk][concepts-network].

Zie voor meer informatie over het beleid, [Kubernetes netwerkbeleidsregels][kubernetes-network-policies].

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubernetes-network-policies]: https://kubernetes.io/docs/concepts/services-networking/network-policies/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[policy-rules]: https://kubernetes.io/docs/concepts/services-networking/network-policies/#behavior-of-to-and-from-selectors

<!-- LINKS - internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[use-advanced-networking]: configure-advanced-networking.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[concepts-network]: concepts-network.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register

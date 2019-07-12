---
title: Gebruik Beschikbaarheidszones in Azure Kubernetes Service (AKS)
description: Informatie over het maken van een cluster met knooppunten over beschikbaarheidszones in Azure Kubernetes Service (AKS verdeelt)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 06/24/2019
ms.author: iainfou
ms.openlocfilehash: 0f99386aa9eeb75a990507e383c32412fb39eceb
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840678"
---
# <a name="preview---create-an-azure-kubernetes-service-aks-cluster-that-uses-availability-zones"></a>Voorbeeld: een Azure Kubernetes Service (AKS)-cluster dat gebruik maakt van Beschikbaarheidszones maken

Een cluster Azure Kubernetes Service (AKS) verdeelt resources, zoals de knooppunten en meerdere logische secties van de onderliggende Azure-opslag van de rekeninfrastructuur. Dit implementatiemodel zorgt ervoor dat de knooppunten worden uitgevoerd in afzonderlijke domeinen voor de update- en foutdomeinen in een enkel Azure-datacenter. AKS-clusters die zijn geïmplementeerd met dit standaardgedrag bieden een hoge mate van beschikbaarheid ter bescherming tegen een hardware-uitval of gepland onderhoud.

Voor een hoger niveau van beschikbaarheid voor uw toepassingen, kunnen de AKS-clusters worden verdeeld in meerdere beschikbaarheidszones. Deze zones zijn fysiek gescheiden datacenters binnen een bepaalde regio. Als de clusteronderdelen zijn verdeeld over meerdere zones, is uw AKS-cluster kunnen tolereren van een storing in een van de zones. Uw toepassingen en beheerbewerkingen nog steeds beschikbaar, zelfs als er is een probleem met een hele datacenter.

Dit artikel ziet u hoe u een AKS-cluster maken en distribueren van de onderdelen van het knooppunt in meerdere beschikbaarheidszones. Deze functie is momenteel beschikbaar als preview-product.

> [!IMPORTANT]
> AKS-preview-functies zijn selfservice, aanmelden. Ze zijn bedoeld om het verzamelen van fouten en feedback van onze community. Preview-versie, worden deze functies zijn niet bedoeld voor gebruik in productieomgevingen. Functies in public preview vallen onder 'best effort'-ondersteuning. Hulp van de AKS-teams voor technische ondersteuning is beschikbaar tijdens kantooruren Pacific tijdzone (PST) alleen. Zie de volgende artikelen ondersteuning voor aanvullende informatie:
>
> * [Ondersteuningsbeleid voor AKS][aks-support-policies]
> * [Veelgestelde vragen over ondersteuning van Azure][aks-faq]

## <a name="before-you-begin"></a>Voordat u begint

U moet de Azure CLI versie 2.0.66 of later geïnstalleerd en geconfigureerd. Voer  `az --version` uit om de versie te bekijken. Als u wilt installeren of upgraden, Zie [Azure CLI installeren][install-azure-cli].

### <a name="install-aks-preview-cli-extension"></a>Aks-preview CLI-extensie installeren

AKS om clusters te maken die gebruikmaken van beschikbaarheidszones, moet u de *aks-preview* CLI versie van de extensie 0.4.1 of hoger. Installeer de *aks-preview* Azure CLI-extensie met de [az-extensie toevoegen][az-extension-add] command, then check for any available updates using the [az extension update][az-extension-update] opdracht::

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-feature-flags-for-your-subscription"></a>Functie vlaggen voor uw abonnement registreren

Vlaggen voor het maken van een AKS-cluster die beschikbaarheidszones, een functie voor het eerst inschakelt voor uw abonnement. Clusters gebruikt u een virtuele-machineschaalset voor het beheren van de implementatie en configuratie van de Kubernetes-knooppunten. De *standard* SKU van de Azure load balancer is ook vereist voor tolerantie van de netwerkonderdelen om verkeer te routeren in uw cluster. Registreert de *AvailabilityZonePreview*, *AKSAzureStandardLoadBalancer*, en *VMSSPreview* functie worden gemarkeerd met behulp van de [az functie registreren][az-feature-register] opdracht zoals wordt weergegeven in het volgende voorbeeld:

> [!CAUTION]
> Als u een functie op een abonnement registreert, kunt u niet op dit moment opheffen van de registratie die functie. Nadat u een preview-functies ingeschakeld, kunnen de standaardwaarden worden gebruikt voor alle AKS-clusters wordt gemaakt in het abonnement. Geen preview-functies voor productieabonnementen niet inschakelen. Gebruik een afzonderlijk abonnement voor het testen van de preview-functies en verzamelen van feedback.

```azurecli-interactive
az feature register --name AvailabilityZonePreview --namespace Microsoft.ContainerService
az feature register --name AKSAzureStandardLoadBalancer --namespace Microsoft.ContainerService
az feature register --name VMSSPreview --namespace Microsoft.ContainerService
```

Het duurt enkele minuten duren voordat de status om weer te geven *geregistreerde*. U kunt controleren op de registratie van status met behulp van de [az Functielijst][az-feature-list] opdracht:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AvailabilityZonePreview')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKSAzureStandardLoadBalancer')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/VMSSPreview')].{Name:name,State:properties.state}"
```

Wanneer u klaar bent, vernieuwt u de registratie van de *Microsoft.ContainerService* resourceprovider met behulp van de [az provider register][az-provider-register] opdracht:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="limitations-and-region-availability"></a>Beperkingen en beschikbaarheid in regio

AKS-clusters kunnen op dit moment worden gemaakt met behulp van de beschikbaarheidszones in de volgende regio's:

* US - oost 2
* Europa - noord
* Azië - zuidoost
* Europa -west
* US - west 2

De volgende beperkingen zijn van toepassing wanneer u een AKS-cluster met beschikbaarheidszones maakt:

* U kunt alleen beschikbaarheidszones inschakelen wanneer het cluster is gemaakt.
* Instellingen voor de beschikbaarheid zone kunnen niet worden bijgewerkt nadat het cluster is gemaakt. U bijwerken een bestaande, niet-beschikbaarheid zone-cluster voor het gebruik van beschikbaarheidszones ook niet.
* Beschikbaarheidszones voor een AKS-cluster niet worden uitgeschakeld nadat deze is gemaakt.
* Grootte van het knooppunt (VM-SKU) ingeschakeld moet beschikbaar zijn in alle beschikbaarheidszones.
* Clusters met availability zones ingeschakeld nodig gebruik van Azure Standard Load Balancers voor distributie in zones.
* Moet u Kubernetes versie 1.13.5 of hoger als u wilt implementeren Standard Load Balancers.

AKS-clusters die gebruikmaken van beschikbaarheidszones moeten gebruiken de Azure load balancer *standard* SKU. De standaardwaarde *basic* SKU van de Azure load balancer biedt geen ondersteuning voor distributie in meerdere beschikbaarheidszones. Zie voor meer informatie en de beperkingen van de standard load balancer, [Azure load balancer standard SKU preview beperkingen][standard-lb-limitations].

### <a name="azure-disks-limitations"></a>Beperkingen voor Azure-schijven

Volumes die gebruikmaken van Azure managed disks zijn momenteel niet zonegebonden resources. Schillen opnieuw gepland in een andere zone vanuit hun oorspronkelijke zone niet opnieuw koppelen van hun vorige schijven. Het is raadzaam om uit te voeren van staatloze werkbelastingen waarvoor geen permanente opslag dat wordt geleverd via zonegebonden problemen is vereist.

Als u stateful werkbelastingen uitvoeren moet, gebruiken taints en tolerations in de specificaties van uw schil om te zien van de Kubernetes-scheduler schillen maken in dezelfde regio bevindt als uw schijven. Of gebruik netwerk gebaseerde opslag, zoals Azure-bestanden die u aan schillen koppelen kunt zoals ze zijn gepland tussen zones.

## <a name="overview-of-availability-zones-for-aks-clusters"></a>Overzicht van Beschikbaarheidszones voor AKS-clusters

Beschikbaarheidszones is een aanbieding die uw toepassingen en gegevens beveiligt tegen storingen in datacenters hoge beschikbaarheid. Zones zijn unieke fysieke locaties binnen een Azure-regio. Elke zone bestaat uit een of meer datacenters die zijn uitgerust met onafhankelijke voeding, koeling en netwerken. Om voor tolerantie te zorgen, is er een minimum van drie afzonderlijke zones in alle ingeschakelde regio's. De fysieke scheiding tussen beschikbaarheidszones binnen een Azure-regio beschermt toepassingen en gegevens tegen storingen op zoneniveau. Zone-redundante services repliceren uw toepassingen en gegevens in meerdere Beschikbaarheidszones om te beschermen tegen enkele punten van de fout.

Zie voor meer informatie, [wat zijn Beschikbaarheidszones in Azure?][az-overview].

AKS-clusters die zijn geïmplementeerd met behulp van de beschikbaarheidszones kunnen knooppunten verdelen over meerdere zones binnen één regio. Bijvoorbeeld, een cluster in de *VS-Oost 2* regio knooppunten kunt maken in alle drie beschikbaarheidszones in *VS-Oost 2*. Deze verdeling van resources van AKS-cluster verbetert de beschikbaarheid van het cluster wanneer ze tegen uitval van een specifieke zone.

![AKS knooppunt distributie in meerdere beschikbaarheidszones](media/availability-zones/aks-availability-zones.png)

In een storing in de zone, de knooppunten handmatig kunnen worden uitgevoerd of met behulp van het cluster automatisch schalen. Als een enkele zone niet beschikbaar is, blijven uw toepassingen worden uitgevoerd.

## <a name="create-an-aks-cluster-across-availability-zones"></a>Een AKS-cluster maken in meerdere beschikbaarheidszones

Wanneer u een cluster maken met de [az aks maken][az-aks-create] opdracht, de `--node-zones` parameter wordt gedefinieerd welke agentknooppunten zones worden geïmplementeerd in. De onderdelen AKS besturingselement vlak voor uw cluster ook worden verdeeld in zones in de configuratie van de hoogste beschikbaar bij het maken van een cluster op te geven de `--node-zones` parameter.

Als u niet alle zones voor de standaard-agentpool definiëren wanneer u een AKS-cluster maakt, de onderdelen AKS besturingselement vlak voor uw cluster beschikbaarheidszones niet gebruiken. U kunt extra knooppuntgroepen (momenteel in preview in AKS) toevoegen met behulp van de [az aks nodepool toevoegen][az-aks-nodepool-add] opdracht en geeft u `--node-zones` voor deze nieuwe agentknooppunten, maar het besturingselement vlak onderdelen blijven zonder binnen een beschikbaarheidszone bewustzijn. U kunt de status van de zone niet wijzigen voor een knooppuntgroep of de AKS vlak onderdelen besturingselement nadat deze zijn geïmplementeerd.

Het volgende voorbeeld wordt een AKS-cluster met de naam *myAKSCluster* in de resourcegroep met de naam *myResourceGroup*. Totaal *3* knooppunten zijn gemaakt: één agent in de zone *1*, één in *2*, en vervolgens een in *3*. De onderdelen van AKS besturingselement vlak worden ook verdeeld over de zones in de hoogste beschikbare configuratie omdat ze zijn gedefinieerd als onderdeel van het cluster proces maken.

```azurecli-interactive
az group create --name myResourceGroup --location eastus2

az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --kubernetes-version 1.13.5 \
    --generate-ssh-keys \
    --enable-vmss \
    --load-balancer-sku standard \
    --node-count 3 \
    --node-zones 1 2 3
```

Het duurt een paar minuten om de AKS-cluster te maken.

## <a name="verify-node-distribution-across-zones"></a>Controleer of knooppunt distributie in zones

Wanneer het cluster gereed is, naam van agentknooppunten in de schaalset om te zien welke deze zijn geïmplementeerd in beschikbaarheidszone.

Haal eerst de referenties van AKS-cluster met behulp van de [az aks get-credentials][az-aks-get-credentials] opdracht:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Gebruik vervolgens de [kubectl beschrijven][kubectl-describe] opdracht om een lijst van de knooppunten in het cluster. Filteren op de *failure-domain.beta.kubernetes.io/zone* waarde zoals wordt weergegeven in het volgende voorbeeld:

```console
kubectl describe nodes | grep -e "Name:" -e "failure-domain.beta.kubernetes.io/zone"
```

De volgende voorbeelduitvoer ziet u de drie knooppunten die zijn verdeeld over de opgegeven regio en beschikbaarheidszones, zoals *eastus2-1* voor de eerste binnen een beschikbaarheidszone en *eastus2-2* voor de tweede binnen een beschikbaarheidszone:

```console
Name:       aks-nodepool1-28993262-vmss000000
            failure-domain.beta.kubernetes.io/zone=eastus2-1
Name:       aks-nodepool1-28993262-vmss000001
            failure-domain.beta.kubernetes.io/zone=eastus2-2
Name:       aks-nodepool1-28993262-vmss000002
            failure-domain.beta.kubernetes.io/zone=eastus2-3
```

Wanneer u extra knooppunten aan een groep van de agent toevoegen, verdeelt het Azure-platform de onderliggende virtuele machines automatisch tussen de opgegeven beschikbaarheidszones.

## <a name="next-steps"></a>Volgende stappen

In dit artikel gedetailleerde over het maken van een AKS-cluster dat gebruik maakt van beschikbaarheidszones. Zie voor meer overwegingen over maximaal beschikbare clusters [aanbevolen procedures voor zakelijke continuïteit en herstel na noodgevallen in AKS][best-practices-bc-dr].

<!-- LINKS - internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-overview]: ../availability-zones/az-overview.md
[best-practices-bc-dr]: operator-best-practices-multi-region.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[standard-lb-limitations]: load-balancer-standard.md#limitations
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[az-aks-nodepool-add]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-add
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials

<!-- LINKS - external -->
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

---
title: De automatische clustering van clusters gebruiken in azure Kubernetes service (AKS)
description: Meer informatie over hoe u de cluster-automatische schaal functie kunt gebruiken om uw cluster automatisch te schalen om te voldoen aan de toepassings vereisten in een Azure Kubernetes service-cluster (AKS).
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 07/18/2019
ms.author: mlearned
ms.openlocfilehash: ef3e9a9c68ca524b7f7f86c92130a10952a9f065
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "68949606"
---
# <a name="preview---automatically-scale-a-cluster-to-meet-application-demands-on-azure-kubernetes-service-aks"></a>Voor beeld: een cluster automatisch schalen om te voldoen aan de toepassings vereisten van de Azure Kubernetes-service (AKS)

Als u de vereisten van toepassingen in azure Kubernetes service (AKS) wilt blijven gebruiken, moet u mogelijk het aantal knoop punten aanpassen waarop uw workloads worden uitgevoerd. Het onderdeel voor het automatisch schalen van clusters kan in uw cluster worden bekeken en kan niet worden ingepland vanwege resource beperkingen. Wanneer er problemen worden gedetecteerd, wordt het aantal knoop punten in een knooppunt groep verhoogd om te voldoen aan de vraag van de toepassing. Knoop punten worden ook regel matig gecontroleerd op het ontbreken van een verwerkings hoeveelheid, met het aantal knoop punten dat vervolgens naar behoefte is afgenomen. Met deze mogelijkheid om automatisch het aantal knoop punten in uw AKS-cluster omhoog of omlaag te schalen, kunt u een efficiënt, rendabel cluster uitvoeren.

In dit artikel wordt beschreven hoe u de cluster-automatische schaal functie inschakelt en beheert in een AKS-cluster. Cluster automatisch schalen mag alleen worden getest als preview-versie van AKS-clusters.

> [!IMPORTANT]
> AKS preview-functies zijn self-service en opt-in. Ze zijn bedoeld om feedback en bugs van onze community te verzamelen. In de preview-versie zijn deze functies niet bedoeld voor productie gebruik. Functies in open bare preview vallen onder de ondersteuning voor beste inspanningen. Hulp van de technische ondersteunings teams van AKS is alleen beschikbaar tijdens kantoor uren Pacific time zone (PST). Raadpleeg de volgende ondersteunings artikelen voor meer informatie:
>
> * [AKS-ondersteunings beleid][aks-support-policies]
> * [Veelgestelde vragen over ondersteuning voor Azure][aks-faq]

## <a name="before-you-begin"></a>Voordat u begint

Voor dit artikel moet u de Azure CLI-versie 2.0.65 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren][azure-cli-install] als u de CLI wilt installeren of een upgrade wilt uitvoeren.

### <a name="install-aks-preview-cli-extension"></a>AKS-preview CLI-extensie installeren

Als u de automatische cluster uitbreiding wilt gebruiken, hebt u de *AKS-preview cli-* extensie versie 0.4.4 of hoger nodig. Installeer de Azure CLI *-extensie AKS-preview* met behulp van de opdracht [AZ extension add][az-extension-add] en controleer vervolgens of er beschik bare updates zijn met behulp van de opdracht [AZ extension update][az-extension-update] :

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-scale-set-feature-provider"></a>Functie provider voor het registreren van schaal sets

Als u een AKS wilt maken die gebruikmaakt van schaal sets, moet u ook een functie vlag inschakelen in uw abonnement. Als u de functie vlag *VMSSPreview* wilt registreren, gebruikt u de opdracht [AZ feature REGI ster][az-feature-register] , zoals weer gegeven in het volgende voor beeld:

> [!CAUTION]
> Wanneer u een functie op een abonnement registreert, kunt u de registratie van die functie op dit moment niet ongedaan maken. Nadat u enkele preview-functies hebt ingeschakeld, kunnen standaard waarden worden gebruikt voor alle AKS-clusters die vervolgens in het abonnement zijn gemaakt. Schakel geen preview-functies in voor productie abonnementen. Gebruik een afzonderlijk abonnement om Preview-functies te testen en feedback te verzamelen.

```azurecli-interactive
az feature register --name VMSSPreview --namespace Microsoft.ContainerService
```

Het duurt enkele minuten voordat de status is *geregistreerd*. U kunt de registratie status controleren met de opdracht [AZ Feature List][az-feature-list] :

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/VMSSPreview')].{Name:name,State:properties.state}"
```

Als u klaar bent, vernieuwt u de registratie van de resource provider *micro soft. container service* met de opdracht [AZ provider REGI ster][az-provider-register] :

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="limitations"></a>Beperkingen

De volgende beperkingen zijn van toepassing wanneer u AKS-clusters maakt en beheert die gebruikmaken van de cluster-automatische schaal functie:

* De invoeg toepassing voor het routeren van HTTP-toepassingen kan niet worden gebruikt.

## <a name="about-the-cluster-autoscaler"></a>Over de automatische cluster schaal

Om aan te passen aan de wijziging van de toepassings vereisten, zoals tussen de dag en de avond of in het weekend, moeten clusters vaak een manier hebben om automatisch te schalen. AKS-clusters kunnen op een van de volgende twee manieren worden geschaald:

* De automatische schaal functie van het **cluster** controleert op peulen die niet kunnen worden gepland op knoop punten vanwege resource beperkingen. Het cluster verhoogt het aantal knoop punten automatisch.
* De **pod** van de metrische gegevens in een Kubernetes-cluster wordt gebruikt om de resource vraag van Peul te controleren. Als een service meer resources nodig heeft, wordt het aantal peulen automatisch verhoogd om aan de vraag te voldoen.

![De automatische clustering van clusters en de horizontale pod-automatisch schalen kunnen samen worden gebruikt ter ondersteuning van de vereiste toepassings vereisten](media/autoscaler/cluster-autoscaler.png)

Zowel de horizontale pod voor automatisch schalen als cluster automatisch schalen kunnen het aantal en knoop punten vervolgens naar behoefte verlagen. De automatische schaal aanpassing van het cluster verkleint het aantal knoop punten wanneer er voor een bepaalde tijd ongebruikte capaciteit is. Het Peul op een knoop punt dat door de automatische cluster functie wordt verwijderd, wordt veilig gepland elders in het cluster. De automatische schaal aanpassing van het cluster kan mogelijk niet omlaag worden uitgebreid als er geen peulen kunnen worden verplaatst, zoals in de volgende situaties:

* Een pod die rechtstreeks is gemaakt en niet wordt ondersteund door een controller object, zoals een implementatie of replicaset.
* Een pod-verstorings budget (PDB) is te beperkend en laat niet toe dat het aantal peulen onder een bepaalde drempel waarde valt.
* Een pod maakt gebruik van knooppunt selecties of een anti-affiniteit die niet kan worden gehonoreerd als deze zijn gepland op een ander knoop punt.

Voor meer informatie over hoe het cluster automatisch schalen niet kan worden geschaald, raadpleegt u [welke typen van de cluster automatisch schalen een knoop punt niet kunnen verwijderen?][autoscaler-scaledown]

De automatische schaal functie van het cluster maakt gebruik van opstart parameters voor dingen zoals tijds intervallen tussen schaal gebeurtenissen en resource drempels. Deze para meters worden gedefinieerd door het Azure-platform en zijn momenteel niet beschikbaar voor aanpassing. Zie [Wat zijn de para meters voor cluster-automatische schalen?][autoscaler-parameters]voor meer informatie over de para meters die door de cluster-automatische schaal functie worden gebruikt.

De twee automatische schalen kunnen samen werken en worden vaak in een cluster geïmplementeerd. In combi natie met de horizontale Pod is de automatische schaal aanpassing gericht op het uitvoeren van het aantal vereiste aantallen dat nodig is om te voldoen aan de vraag van de toepassing. De cluster automatisch schalen is gericht op het uitvoeren van het aantal knoop punten dat vereist is voor de ondersteuning van de geplande peulen.

> [!NOTE]
> Hand matig schalen is uitgeschakeld wanneer u de automatische schaal functie van het cluster gebruikt. Laat de cluster automatisch schalen het vereiste aantal knoop punten bepalen. Als u de schaal van het cluster hand matig wilt aanpassen, schakelt u de automatische clustering van [clusters uit](#disable-the-cluster-autoscaler).

## <a name="create-an-aks-cluster-and-enable-the-cluster-autoscaler"></a>Een AKS-cluster maken en de cluster automatisch schalen inschakelen

Als u een AKS-cluster moet maken, gebruikt u de opdracht [AZ AKS Create][az-aks-create] . Als u de cluster-automatische schaal functie wilt inschakelen en configureren voor de knooppunt groep voor het cluster, gebruikt u de para meter *--Enable-cluster-* autoscaler en geeft u een knoop punt *--Min-aantal* en *--maximum-aantal*op.

> [!IMPORTANT]
> De automatische schaal functie van het cluster is een Kubernetes-onderdeel. Hoewel het AKS-cluster gebruikmaakt van een schaalset voor virtuele machines voor de knoop punten, kunt u de instellingen voor het automatisch schalen van schaal sets niet hand matig inschakelen of bewerken in de Azure Portal of de Azure CLI gebruiken. Laat de Kubernetes voor cluster automatisch schalen de vereiste schaal instellingen beheren. Zie [kan ik de AKS-resources in de knooppunt resource groep wijzigen?](faq.md#can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-node-resource-group) voor meer informatie.

In het volgende voor beeld wordt een AKS-cluster gemaakt met één knooppunt groep die wordt ondersteund door een virtuele-machine schaalset. Ook wordt de cluster-automatische schaal functie voor de knooppunt groep voor het cluster ingeschakeld en worden mini maal *1* en Maxi maal *drie* knoop punten ingesteld:

```azurecli-interactive
# First create a resource group
az group create --name myResourceGroup --location eastus

# Now create the AKS cluster and enable the cluster autoscaler
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --node-count 1 \
  --enable-vmss \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

> [!NOTE]
> Als u een *--kubernetes-version* opgeeft wanneer deze `az aks create`wordt uitgevoerd, moet die versie voldoen aan of groter dan het minimum versie nummer dat is vereist, zoals beschreven in de vorige sectie [voordat u begint](#before-you-begin) .

Het duurt enkele minuten om het cluster te maken en de instellingen voor het automatisch schalen van clusters te configureren.

### <a name="update-the-cluster-autoscaler-on-an-existing-node-pool-in-a-cluster-with-a-single-node-pool"></a>De automatische cluster-Scaler bijwerken voor een bestaande knooppunt groep in een cluster met een groep met één knoop punt

U kunt de vorige instellingen van het cluster automatisch schalen bijwerken op een cluster dat voldoet aan de vereisten zoals beschreven in de vorige sectie [voordat u begint](#before-you-begin) . Gebruik de opdracht [AZ AKS update][az-aks-update] om de automatische cluster schaalr in te scha kelen op uw cluster met *één* knooppunt groep.

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --update-cluster-autoscaler \
  --min-count 1 \
  --max-count 5
```

Vervolgens kan de cluster-automatische schaal functie worden in-of `az aks update --enable-cluster-autoscaler` uitgeschakeld `az aks update --disable-cluster-autoscaler` met de opdrachten.

### <a name="enable-the-cluster-autoscaler-on-an-existing-node-pool-in-a-cluster-with-multiple-node-pools"></a>De automatische cluster schaalr inschakelen voor een bestaande knooppunt groep in een cluster met meerdere knooppunt groepen

De cluster automatisch schalen kan ook worden gebruikt met de [Preview-functie voor meerdere knooppunt Pools](use-multiple-node-pools.md) ingeschakeld. U kunt de automatische cluster schaalr inschakelen voor afzonderlijke knooppunt groepen binnen een AKS-cluster dat meerdere knooppunt groepen bevat en voldoet aan de vereisten zoals beschreven in de vorige sectie [voordat u begint](#before-you-begin) . Gebruik de opdracht [AZ AKS nodepool update][az-aks-nodepool-update] om de automatische cluster schaalr in te scha kelen voor een afzonderlijke knooppunt groep.

```azurecli-interactive
az aks nodepool update \
  --resource-group myResourceGroup \
  --cluster-name myAKSCluster \
  --name mynodepool \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

Vervolgens kan de cluster-automatische schaal functie worden in-of `az aks nodepool update --enable-cluster-autoscaler` uitgeschakeld `az aks nodepool update --disable-cluster-autoscaler` met de opdrachten.

## <a name="change-the-cluster-autoscaler-settings"></a>De instellingen voor het automatisch schalen van clusters wijzigen

In de vorige stap om een AKS-cluster te maken of een bestaande knooppunt groep bij te werken, is het minimum aantal knoop punten van het cluster automatisch ingesteld op *1*en is het maximum aantal knoop punten ingesteld op *3*. Als uw toepassing wordt gewijzigd, moet u mogelijk het aantal knoop punten van de cluster automatisch schalen aanpassen.

Als u het aantal knoop punten wilt wijzigen, gebruikt u de opdracht [AZ AKS nodepool update][az-aks-nodepool-update] .

```azurecli-interactive
az aks nodepool update \
  --resource-group myResourceGroup \
  --cluster-name myAKSCluster \
  --name mynodepool \
  --update-cluster-autoscaler \
  --min-count 1 \
  --max-count 5
```

In het bovenstaande voor beeld wordt de cluster- *mynodepool* voor de knooppunt groep in de *myAKSCluster* met een minimum van *1* en Maxi maal *vijf* knoop punten bijgewerkt.

> [!NOTE]
> Tijdens de preview-periode kunt u geen hoger minimum aantal knoop punten instellen dan momenteel is ingesteld voor de knooppunt groep. Als u op dit moment bijvoorbeeld het aantal minuten hebt ingesteld op *1*, kunt u het aantal minuten niet bijwerken naar *3*.

Bewaak de prestaties van uw toepassingen en services en pas het aantal knoop punten van de cluster automatisch aan, zodat dit overeenkomt met de vereiste prestaties.

## <a name="disable-the-cluster-autoscaler"></a>De automatische schaal functie van het cluster uitschakelen

Als u de cluster automatisch schalen niet meer wilt gebruiken, kunt u dit uitschakelen met de opdracht [AZ AKS nodepool update][az-aks-nodepool-update] , waarbij u de para meter *--Disable-cluster-* auto Scaler opgeeft. Knoop punten worden niet verwijderd wanneer de automatische schaal functie van het cluster is uitgeschakeld.

```azurecli-interactive
az aks nodepool update \
  --resource-group myResourceGroup \
  --cluster-name myAKSCluster \
  --name mynodepool \
  --disable-cluster-autoscaler
```

U kunt het cluster hand matig schalen met behulp van de opdracht [AZ AKS Scale][az-aks-scale] . Als u de horizontale pod autoscaleer gebruikt, blijft die functie actief met de automatische cluster schaalr, maar kan het mogelijk zijn dat er niet meer kan worden gepland als de knooppunt resources al in gebruik zijn.

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt uitgelegd hoe u het aantal AKS-knoop punten automatisch kunt schalen. U kunt ook de horizontale pod autoschaalr gebruiken om automatisch het aantal peulen te verhogen waarmee uw toepassing wordt uitgevoerd. Zie [toepassingen schalen in AKS][aks-scale-apps]voor meer informatie over het gebruik van de pod.

<!-- LINKS - internal -->
[aks-upgrade]: upgrade-cluster.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-extension-add]: /cli/azure/extension#az-extension-add
[aks-scale-apps]: tutorial-kubernetes-scale.md
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-scale]: /cli/azure/aks#az-aks-scale
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update

<!-- LINKS - external -->
[az-aks-update]: https://github.com/Azure/azure-cli-extensions/tree/master/src/aks-preview
[az-aks-nodepool-update]: https://github.com/Azure/azure-cli-extensions/tree/master/src/aks-preview#enable-cluster-auto-scaler-for-a-node-pool
[autoscaler-scaledown]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-types-of-pods-can-prevent-ca-from-removing-a-node
[autoscaler-parameters]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-are-the-parameters-to-ca

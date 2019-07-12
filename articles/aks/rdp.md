---
title: Verbinding met Azure Kubernetes Service (AKS)-clusterknooppunten Windows Server
description: Leer hoe u een RDP-verbinding maken met Azure Kubernetes Service (AKS)-cluster met Windows Server-knooppunten voor probleemoplossing en onderhoudstaken.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 06/04/2019
ms.author: mlearned
ms.openlocfilehash: 0238278b81255d735f8a950ca307d0e05100cfec
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67614560"
---
# <a name="connect-with-rdp-to-azure-kubernetes-service-aks-cluster-windows-server-nodes-for-maintenance-or-troubleshooting"></a>Verbinden met RDP naar Azure Kubernetes Service (AKS) Windows Server-clusterknooppunten voor onderhoud of probleemoplossing

Gedurende de levenscyclus van uw cluster Azure Kubernetes Service (AKS) moet u mogelijk toegang tot een AKS Windows Server-knooppunt. Deze toegang kan worden voor onderhoud, logboekverzameling of andere bewerkingen voor het oplossen van problemen. U kunt toegang tot de Windows-Server voor AKS-knooppunten met behulp van RDP. Als u gebruiken van SSH wilt voor toegang tot de AKS-Windows-bestandsserverknooppunten en u toegang hebt tot het hetzelfde sleutelpaar dat is gebruikt tijdens het maken van een cluster, kunt u ook de stappen in volgen [SSH in Azure Kubernetes Service (AKS)-clusterknooppunten][ssh-steps]. Uit veiligheidsoverwegingen worden de AKS-knooppunten niet blootgesteld aan internet.

Ondersteuning voor Windows Server-knooppunt is momenteel in preview in AKS.

Dit artikel ziet u hoe u een RDP-verbinding maakt met een AKS-knooppunten met behulp van hun privé IP-adressen.

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u een bestaand AKS-cluster met een Windows Server-knooppunt hebt. Als u een AKS-cluster nodig hebt, raadpleegt u het artikel [een AKS-cluster maken met een Windows-container met de Azure CLI][aks-windows-cli]. You need the Windows administrator username and password for the Windows Server node you want to troubleshoot. You also need an RDP client such as [Microsoft Remote Desktop][rdp-mac].

U ook moet de Azure CLI versie 2.0.61 of later geïnstalleerd en geconfigureerd. Voer  `az --version` uit om de versie te bekijken. Als u wilt installeren of upgraden, Zie [Azure CLI installeren][install-azure-cli].

## <a name="deploy-a-virtual-machine-to-the-same-subnet-as-your-cluster"></a>Een virtuele machine implementeren in hetzelfde subnet bevindt als uw cluster

De Windows Server-knooppunten van uw AKS-cluster hebben niet extern toegankelijk IP-adressen. Als u een RDP-verbinding, kunt u een virtuele machine met een openbaar IP-adres op hetzelfde subnet bevindt als uw Windows Server-knooppunten implementeren.

Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* in de *myResourceGroup* resourcegroep.

Haal eerst het subnet dat wordt gebruikt door de groep van uw Windows Server-knooppunt. Als u de subnet-id, moet u de naam van het subnet. Als u de naam van het subnet, moet u de naam van het vnet. Haal de naam vnet door het opvragen van uw cluster voor de lijst met netwerken. Om te vragen het cluster, moet u de naam ervan. U kunt al deze waarden door te voeren van het volgende in de Azure Cloud Shell krijgen:

```azurecli-interactive
CLUSTER_RG=$(az aks show -g myResourceGroup -n myAKSCluster --query nodeResourceGroup -o tsv)
VNET_NAME=$(az network vnet list -g $CLUSTER_RG --query [0].name -o tsv)
SUBNET_NAME=$(az network vnet subnet list -g $CLUSTER_RG --vnet-name $VNET_NAME --query [0].name -o tsv)
SUBNET_ID=$(az network vnet subnet show -g $CLUSTER_RG --vnet-name $VNET_NAME --name $SUBNET_NAME --query id -o tsv)
```

Nu dat u de SUBNET_ID hebt, moet u de volgende opdracht uitvoeren in hetzelfde Azure Cloud Shell-venster om de VM te maken:

```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image win2019datacenter \
    --admin-username azureuser \
    --admin-password myP@ssw0rd12 \
    --subnet $SUBNET_ID \
    --query publicIpAddress -o tsv
```

De volgende voorbeelduitvoer ziet u de virtuele machine is gemaakt en wordt het openbare IP-adres van de virtuele machine wordt weergegeven.

```console
13.62.204.18
```

Noteer het openbare IP-adres van de virtuele machine. U gebruikt dit adres in een latere stap.

## <a name="get-the-node-address"></a>Adres van het knooppunt ophalen

Voor het beheren van een Kubernetes-cluster, gebruikt u [kubectl][kubectl], de Kubernetes-opdrachtregelclient. Als u Azure Cloud Shell gebruikt, is `kubectl` al geïnstalleerd. Voor het installeren van `kubectl` lokaal, gebruikt u de [az aks install-cli][az-aks-install-cli] opdracht:
    
```azurecli-interactive
az aks install-cli
```

Gebruik de opdracht [az aks get-credentials][az-aks-get-credentials] om `kubectl` zodanig te configureren dat er verbinding wordt gemaakt met het Kubernetes-cluster. Bij deze opdracht worden referenties gedownload en wordt Kubernetes CLI geconfigureerd voor het gebruik van deze referenties.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Overzicht van het interne IP-adres van de Windows Server-knooppunten met behulp van de [kubectl ophalen][kubectl-get] opdracht:

```console
kubectl get nodes -o wide
```

De volgende voorbeelduitvoer ziet u interne IP-adressen van alle knooppunten in het cluster, met inbegrip van de Windows Server-knooppunten.

```console
$ kubectl get nodes -o wide
NAME                                STATUS   ROLES   AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                    KERNEL-VERSION      CONTAINER-RUNTIME
aks-nodepool1-42485177-vmss000000   Ready    agent   18h   v1.12.7   10.240.0.4    <none>        Ubuntu 16.04.6 LTS          4.15.0-1040-azure   docker://3.0.4
aksnpwin000000                      Ready    agent   13h   v1.12.7   10.240.0.67   <none>        Windows Server Datacenter   10.0.17763.437
```

Noteer het interne IP-adres van het Windows Server-knooppunt dat u wilt oplossen. U gebruikt dit adres in een latere stap.

## <a name="connect-to-the-virtual-machine-and-node"></a>Verbinding maken met de virtuele machine en het knooppunt

Verbinding maken met het openbare IP-adres van de virtuele machine die u hebt gemaakt met een RDP-client eerder zoals [Microsoft Extern bureaublad][rdp-mac].

![Afbeelding van de verbinding te maken met de virtuele machine met behulp van een RDP-client](media/rdp/vm-rdp.png)

Nadat u een verbinding met uw virtuele machine hebt gemaakt, verbinding maken met de *interne IP-adres* van het Windows Server-knooppunt dat u oplossen wilt, met behulp van een RDP-client van uw virtuele machine.

![Afbeelding van de verbinding te maken met het Windows Server-knooppunt met behulp van een RDP-client](media/rdp/node-rdp.png)

U bent nu verbonden met uw Windows Server-knooppunt.

![Afbeelding van het opdrachtvenster in de Windows Server-knooppunt](media/rdp/node-session.png)

U kunt nu het oplossen van problemen opdrachten uitvoeren de *cmd* venster. Omdat Windows Server-knooppunten Windows Server Core, is er niet een volledige GUI of andere GUI-hulpprogramma's wanneer u via RDP verbinding met een Windows Server-knooppunt maken.

## <a name="remove-rdp-access"></a>RDP-toegang verwijderen

Wanneer u klaar bent, sluit u de RDP-verbinding naar het Windows Server-knooppunt en afsluiten van de RDP-sessie met de virtuele machine. Nadat u beide RDP-sessies hebt afgesloten, verwijdert u de virtuele machine met de [az vm verwijderen][az-vm-delete] opdracht:

```azurecli-interactive
az vm delete --resource-group myResourceGroup --name myVM
```

## <a name="next-steps"></a>Volgende stappen

Als u meer gegevens nodig hebt, kunt u [Bekijk de logboeken van Kubernetes-hoofdknooppunt][view-master-logs] or [Azure Monitor][azure-monitor-containers].

<!-- EXTERNAL LINKS -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[rdp-mac]: https://aka.ms/rdmac

<!-- INTERNAL LINKS -->
[aks-windows-cli]: windows-container-cli.md
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-vm-delete]: /cli/azure/vm#az-vm-delete
[azure-monitor-containers]: ../azure-monitor/insights/container-insights-overview.md
[install-azure-cli]: /cli/azure/install-azure-cli
[ssh-steps]: ssh.md
[view-master-logs]: view-master-logs.md

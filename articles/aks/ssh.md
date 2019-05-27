---
title: Voeg SSH toe aan de clusterknooppunten Azure Kubernetes Service (AKS)
description: Informatie over het maken van een SSH-verbinding met de clusterknooppunten Azure Kubernetes Service (AKS) voor het oplossen van problemen en onderhoudstaken.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/20/2019
ms.author: iainfou
ms.openlocfilehash: a85c39fbfbf629e6ba9e668d55dd905c1ce0800c
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65956349"
---
# <a name="connect-with-ssh-to-azure-kubernetes-service-aks-cluster-nodes-for-maintenance-or-troubleshooting"></a>Verbinding maken met SSH naar Azure Kubernetes Service (AKS) clusterknooppunten voor onderhoud of probleemoplossing

Gedurende de levenscyclus van uw cluster Azure Kubernetes Service (AKS) moet u mogelijk toegang tot een AKS-knooppunt. Deze toegang kan worden voor onderhoud, logboekverzameling of andere bewerkingen voor het oplossen van problemen. AKS-knooppunten via SSH, met inbegrip van Windows Server-knooppunten (momenteel in preview in AKS), kunt u openen. U kunt ook [verbinding maken met Windows Server-knooppunten met behulp van remote desktop protocol (RDP)-verbindingen][aks-windows-rdp]. Uit veiligheidsoverwegingen worden de AKS-knooppunten niet blootgesteld aan internet.

Dit artikel ziet u hoe u een SSH-verbinding maakt met een AKS-knooppunten met behulp van hun privé IP-adressen.

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u een bestaand AKS-cluster hebt. Als u een cluster AKS nodig hebt, raadpleegt u de Quick Start voor AKS [met de Azure CLI] [ aks-quickstart-cli] of [met behulp van de Azure-portal][aks-quickstart-portal].

U ook moet de Azure CLI versie 2.0.64 of later geïnstalleerd en geconfigureerd. Voer  `az --version` uit om de versie te bekijken. Als u de Azure CLI wilt installeren of upgraden, raadpleegt u  [Azure CLI installeren][install-azure-cli].

## <a name="add-your-public-ssh-key"></a>Uw openbare SSH-sleutel toevoegen

Standaard worden SSH-sleutels hebt aangeschaft, of die worden gegenereerd en toegevoegd aan de knooppunten bij het maken van een AKS-cluster. Als u nodig hebt om op te geven van verschillende SSH-sleutels dan die worden gebruikt bij het maken van uw AKS-cluster, moet u uw openbare SSH-sleutel toevoegen aan de Linux-AKS-knooppunten. Indien nodig, kunt u een SSH key met [macOS of Linux] [ ssh-nix] of [Windows][ssh-windows]. Als u PuTTY Gen gebruikt voor het maken van het sleutelpaar, sla het sleutelpaar in een OpenSSH-indeling in plaats van de standaard PuTTy persoonlijke sleutel indeling (ppk-bestand).

> [!NOTE]
> SSH-sleutels kunnen worden momenteel alleen toegevoegd aan Linux-knooppunten met de Azure CLI. Als u Windows Server-knooppunten gebruiken, gebruikt u de SSH-sleutels die is opgegeven tijdens het maken van het AKS-cluster en gaat u naar de stap op [over het verkrijgen van het adres van het AKS-knooppunt](#get-the-aks-node-address). Of, [verbinding maken met Windows Server-knooppunten met behulp van remote desktop protocol (RDP)-verbindingen][aks-windows-rdp].

Als u wilt uw SSH-sleutel toevoegen aan een Linux AKS-knooppunt, voert u de volgende stappen uit:

1. De naam van de resource voor de resources van uw AKS-cluster met behulp van [az aks show][az-aks-show]. Geef uw eigen resourcegroep core en de naam van de AKS-cluster:

    ```azurecli-interactive
    az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv
    ```

1. Lijst van de virtuele machines in de AKS-cluster resource groep met de [az vm list] [ az-vm-list] opdracht. Deze virtuele machines zijn uw AKS-knooppunten:

    ```azurecli-interactive
    az vm list --resource-group MC_myResourceGroup_myAKSCluster_eastus -o table
    ```

    De volgende voorbeelduitvoer ziet u de AKS-knooppunten:

    ```
    Name                      ResourceGroup                                  Location
    ------------------------  ---------------------------------------------  ----------
    aks-nodepool1-79590246-0  MC_myResourceGroupAKS_myAKSClusterRBAC_eastus  eastus
    ```

1. U kunt uw SSH-sleutels toevoegen aan het knooppunt met de [az vm gebruikersupdate] [ az-vm-user-update] opdracht. Geef de naam van de resourcegroep en vervolgens een van de AKS-knooppunten in de vorige stap hebt verkregen. De gebruikersnaam voor de AKS-knooppunten is standaard *azureuser*. Geef de locatie van uw eigen SSH public key locatie, zoals *~/.ssh/id_rsa.pub*, of plak de inhoud van uw openbare SSH-sleutel:

    ```azurecli-interactive
    az vm user update \
      --resource-group MC_myResourceGroup_myAKSCluster_eastus \
      --name aks-nodepool1-79590246-0 \
      --username azureuser \
      --ssh-key-value ~/.ssh/id_rsa.pub
    ```

## <a name="get-the-aks-node-address"></a>Het adres van het AKS-knooppunt ophalen

De AKS-knooppunten zijn niet openbaar blootgesteld aan internet. SSH met de AKS-knooppunten, moet u het privé IP-adres gebruiken. In de volgende stap maakt u een helper-schil in uw AKS-cluster waarmee u kunt SSH naar deze privé IP-adres van het knooppunt. De stappen voor het ophalen van het privé IP-adres van de AKS-knooppunten kan verschillen op basis van het type van de AKS-cluster die u uitvoert:

* Volg de stappen voor de meeste AKS clusters [ophalen van het IP-adres voor reguliere AKS clusters](#regular-aks-clusters).
* Als u een preview-functies gebruiken in AKS die gebruikmaken van virtuele-machineschaalsets, zoals meerdere knooppuntgroepen of ondersteuning voor Windows Server-containers [Volg de stappen voor clusters van virtuele machines schalen op basis van een set AKS](#virtual-machine-scale-set-based-aks-clusters).

### <a name="regular-aks-clusters"></a>Reguliere AKS-clusters

Bekijk het privé IP-adres van een AKS-cluster knooppunt met de [az vm list-ip-adressen] [ az-vm-list-ip-addresses] opdracht. Geef uw eigen AKS-cluster groepsnaam voor accountresources hebt verkregen in een vorige [az-aks-show] [ az-aks-show] stap:

```azurecli-interactive
az vm list-ip-addresses --resource-group MC_myResourceGroup_myAKSCluster_eastus -o table
```

De volgende voorbeelduitvoer ziet u het particuliere IP-adressen van de AKS-knooppunten:

```
VirtualMachine            PrivateIPAddresses
------------------------  --------------------
aks-nodepool1-79590246-0  10.240.0.4
```

### <a name="virtual-machine-scale-set-based-aks-clusters"></a>Clusters van virtuele machines schalen op basis van een set AKS

Overzicht van het interne IP-adres van de knooppunten met behulp van de [kubectl opdracht ophalen][kubectl-get]:

```console
kubectl get nodes -o wide
```

De volgende voorbeelduitvoer ziet u interne IP-adressen van alle knooppunten in het cluster, met inbegrip van een Windows Server-knooppunt.

```console
$ kubectl get nodes -o wide

NAME                                STATUS   ROLES   AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                    KERNEL-VERSION      CONTAINER-RUNTIME
aks-nodepool1-42485177-vmss000000   Ready    agent   18h   v1.12.7   10.240.0.4    <none>        Ubuntu 16.04.6 LTS          4.15.0-1040-azure   docker://3.0.4
aksnpwin000000                      Ready    agent   13h   v1.12.7   10.240.0.67   <none>        Windows Server Datacenter   10.0.17763.437
```

Noteer het interne IP-adres van het knooppunt dat u wilt oplossen. U gebruikt dit adres in een latere stap.

## <a name="create-the-ssh-connection"></a>De SSH-verbinding maken

Voor het maken van een SSH-verbinding naar een AKS-knooppunt, kunt u een helper-schil uitvoeren in uw AKS-cluster. Deze pod helper biedt u SSH-toegang in het cluster en klikt u vervolgens aanvullende SSH-knooppunt toegang. Als u wilt maken en deze pod helper gebruiken, voert u de volgende stappen uit:

1. Voer een `debian` container installatiekopie en het koppelen van een terminalsessie toe. Deze container kan worden gebruikt voor het maken van een SSH-sessie met een willekeurig knooppunt in het AKS-cluster:

    ```console
    kubectl run -it --rm aks-ssh --image=debian
    ```

    > [!TIP]
    > Als u Windows Server-knooppunten (momenteel in preview in AKS), moet u een selector knooppunt toevoegen aan de opdracht voor het plannen van de Debian-container op een Linux-knooppunt als volgt:
    >
    > `kubectl run -it --rm aks-ssh --image=debian --overrides='{"apiVersion":"apps/v1","spec":{"template":{"spec":{"nodeSelector":{"beta.kubernetes.io/os":"linux"}}}}}'`

1. De basis Debian-installatiekopie bevat geen SSH-onderdelen. Zodra de sessie is verbonden met de container, installeert u een SSH-client met behulp van `apt-get` als volgt:

    ```console
    apt-get update && apt-get install openssh-client -y
    ```

1. In een nieuw terminalvenster niet is verbonden met uw container lijst de schillen in uw AKS-cluster met de [kubectl ophalen schillen] [ kubectl-get] opdracht. De schil in de vorige stap hebt gemaakt, begint met de naam van de *aks-ssh*, zoals wordt weergegeven in het volgende voorbeeld:

    ```
    $ kubectl get pods
    
    NAME                       READY     STATUS    RESTARTS   AGE
    aks-ssh-554b746bcf-kbwvf   1/1       Running   0          1m
    ```

1. In de eerste stap van dit artikel hebt u uw openbare SSH-sleutel de AKS-knooppunt toegevoegd. Nu uw persoonlijke SSH-sleutel kopiëren naar de schil. Deze persoonlijke sleutel wordt gebruikt voor het maken van de SSH in de AKS-knooppunten.

    Geef uw eigen *aks-ssh* pod-naam in de vorige stap hebt verkregen. Indien nodig, wijzigen *~/.ssh/id_rsa* naar locatie van uw persoonlijke SSH-sleutel:

    ```console
    kubectl cp ~/.ssh/id_rsa aks-ssh-554b746bcf-kbwvf:/id_rsa
    ```

1. Terug in de terminal-sessie naar de container bijwerken van de machtigingen op de gekopieerde `id_rsa` persoonlijke SSH-sleutel zodat deze gebruiker alleen-lezen:

    ```console
    chmod 0600 id_rsa
    ```

1. Maak nu een SSH-verbinding naar uw AKS-knooppunt. Nogmaals, de standaardgebruikersnaam voor AKS-knooppunten is *azureuser*. Accepteer de prompt om door te gaan met de verbinding als de SSH-sleutel wordt vertrouwd. U vindt u vervolgens met de bash-prompt van uw AKS-knooppunt:

    ```console
    $ ssh -i id_rsa azureuser@10.240.0.4
    
    ECDSA key fingerprint is SHA256:A6rnRkfpG21TaZ8XmQCCgdi9G/MYIMc+gFAuY9RUY70.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added '10.240.0.4' (ECDSA) to the list of known hosts.
    
    Welcome to Ubuntu 16.04.5 LTS (GNU/Linux 4.15.0-1018-azure x86_64)
    
     * Documentation:  https://help.ubuntu.com
     * Management:     https://landscape.canonical.com
     * Support:        https://ubuntu.com/advantage
    
      Get cloud support with Ubuntu Advantage Cloud Guest:
        https://www.ubuntu.com/business/services/cloud
    
    [...]
    
    azureuser@aks-nodepool1-79590246-0:~$
    ```

## <a name="remove-ssh-access"></a>SSH-toegang verwijderen

Wanneer u klaar bent, `exit` de SSH-sessie en vervolgens `exit` de interactieve container-sessie. Wanneer deze container-sessie wordt gesloten, wordt de schil wordt gebruikt voor SSH-toegang van het AKS-cluster wordt verwijderd.

## <a name="next-steps"></a>Volgende stappen

Als u meer gegevens nodig hebt, kunt u [Bekijk de logboeken kubelet] [ view-kubelet-logs] of [Bekijk de logboeken van Kubernetes-hoofdknooppunt][view-master-logs].

<!-- EXTERNAL LINKS -->
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- INTERNAL LINKS -->
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-vm-list]: /cli/azure/vm#az-vm-list
[az-vm-user-update]: /cli/azure/vm/user#az-vm-user-update
[az-vm-list-ip-addresses]: /cli/azure/vm#az-vm-list-ip-addresses
[view-kubelet-logs]: kubelet-logs.md
[view-master-logs]: view-master-logs.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[aks-windows-rdp]: rdp.md
[ssh-nix]: ../virtual-machines/linux/mac-create-ssh-keys.md
[ssh-windows]: ../virtual-machines/linux/ssh-from-windows.md

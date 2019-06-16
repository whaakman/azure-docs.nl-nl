---
title: Gebruik van meerdere groepen in Azure Kubernetes Service (AKS)
description: Meer informatie over het maken en beheren van meerdere knooppuntgroepen voor een cluster in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/17/2019
ms.author: iainfou
ms.openlocfilehash: 679d91da774b3e4d2c53c70cdc0abfd4da9c6953
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67059634"
---
# <a name="preview---create-and-manage-multiple-node-pools-for-a-cluster-in-azure-kubernetes-service-aks"></a>Preview - maken en beheren van meerdere knooppuntgroepen voor een cluster in Azure Kubernetes Service (AKS)

In Azure Kubernetes Service (AKS), knooppunten met dezelfde configuratie zijn gegroepeerd tot *knooppuntgroepen*. Deze groepen bevatten de onderliggende virtuele machines waarop uw toepassingen worden uitgevoerd. Het oorspronkelijke aantal knooppunten en de grootte (SKU) zijn gedefinieerd bij het maken van een AKS-cluster maakt een *knooppuntgroep standaard*. Ter ondersteuning van toepassingen met verschillende rekenscenario of opslagbehoefte, kunt u extra knooppuntgroepen maken. Bijvoorbeeld, gebruiken deze extra knooppuntgroepen GPU's bieden voor rekenintensieve toepassingen of de toegang tot SSD-opslag met hoge prestaties.

In dit artikel wordt beschreven hoe u maken en beheren van meerdere groepen in een AKS-cluster. Deze functie is momenteel beschikbaar als preview-product.

> [!IMPORTANT]
> AKS-preview-functies zijn selfservice, aanmelden. Ze zijn bedoeld om het verzamelen van fouten en feedback van onze community. Preview-versie, worden deze functies zijn niet bedoeld voor gebruik in productieomgevingen. Functies in public preview vallen onder 'best effort'-ondersteuning. Hulp van de AKS-teams voor technische ondersteuning is beschikbaar tijdens kantooruren Pacific tijdzone (PST) alleen. Zie de volgende artikelen ondersteuning voor aanvullende informatie:
>
> * [Ondersteuningsbeleid voor AKS][aks-support-policies]
> * [Veelgestelde vragen over ondersteuning van Azure][aks-faq]

## <a name="before-you-begin"></a>Voordat u begint

U moet de Azure CLI versie 2.0.61 of later geïnstalleerd en geconfigureerd. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][install-azure-cli].

### <a name="install-aks-preview-cli-extension"></a>Aks-preview CLI-extensie installeren

De CLI-opdrachten voor het maken en beheren van meerdere groepen zijn beschikbaar in de *aks-preview* CLI-extensie. Installeer de *aks-preview* Azure CLI-extensie met de [az-extensie toevoegen] [ az-extension-add] opdracht, zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive
az extension add --name aks-preview
```

> [!NOTE]
> Als u eerder hebt geïnstalleerd het *aks-preview* extensie, installatie van de beschikbare updates met behulp van de `az extension update --name aks-preview` opdracht.

### <a name="register-multiple-node-pool-feature-provider"></a>Meerdere knooppunt groep functie provider registreren

Voor het maken van een AKS-cluster die meerdere groepen kunt gebruiken, moet u eerst twee functie vlaggen voor uw abonnement inschakelen. Een virtuele-machineschaalset (VMSS) clusters met meerdere knooppunten groep gebruiken voor het beheren van de implementatie en configuratie van de Kubernetes-knooppunten. Registreert de *MultiAgentpoolPreview* en *VMSSPreview* functie worden gemarkeerd met behulp van de [az functie registreren] [ az-feature-register] opdracht zoals wordt weergegeven in de bijvoorbeeld:

```azurecli-interactive
az feature register --name MultiAgentpoolPreview --namespace Microsoft.ContainerService
az feature register --name VMSSPreview --namespace Microsoft.ContainerService
```

> [!NOTE]
> Een AKS-cluster dat u maakt nadat u hebt geregistreerd de *MultiAgentpoolPreview* gebruik van deze preview-cluster-ervaring. Als u wilt doorgaan met het maken van clusters van reguliere, volledig ondersteunde, geen preview-functies voor productieabonnementen inschakelen. Gebruik een afzonderlijke test- en Azure-abonnement voor het testen van de preview-functies.

Het duurt enkele minuten duren voordat de status om weer te geven *geregistreerde*. U kunt controleren op de registratie van status met behulp van de [az Functielijst] [ az-feature-list] opdracht:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/MultiAgentpoolPreview')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/VMSSPreview')].{Name:name,State:properties.state}"
```

Wanneer u klaar bent, vernieuwt u de registratie van de *Microsoft.ContainerService* resourceprovider met behulp van de [az provider register] [ az-provider-register] opdracht:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="limitations"></a>Beperkingen

De volgende beperkingen zijn van toepassing wanneer u maken en beheren van AKS-clusters die ondersteuning bieden voor meerdere groepen:

* Meerdere groepen zijn alleen beschikbaar voor clusters die zijn gemaakt nadat u hebt geregistreerd de *MultiAgentpoolPreview* en *VMSSPreview* functies voor uw abonnement. U kunt toevoegen of beheren van groepen met een bestaand AKS-cluster dat is gemaakt voordat deze functies zijn geregistreerd.
* U kunt het eerste knooppunt van toepassingen niet verwijderen.
* De HTTP-aanvraag routering invoegtoepassing kan niet worden gebruikt.
* U kunt geen knooppuntgroepen toevoegen, bijwerken en verwijderen met behulp van een bestaande Resource Manager-sjabloon net als bij de meeste bewerkingen. In plaats daarvan [een afzonderlijke Resource Manager-sjabloon gebruiken](#manage-node-pools-using-a-resource-manager-template) wijzigingen aanbrengen in groepen in een AKS-cluster.
* Het cluster automatisch schalen (momenteel in preview in AKS) kan niet worden gebruikt.

Hoewel deze functie nog in preview, gelden de volgende aanvullende beperkingen:

* Het AKS-cluster kan maximaal acht knooppuntgroepen hebben.
* Het AKS-cluster kan maximaal 400 knooppunten hebt in die pools acht knooppunten.

## <a name="create-an-aks-cluster"></a>Een AKS-cluster maken

Om te beginnen, moet u een AKS-cluster maken met een groep met één knooppunt. Het volgende voorbeeld wordt de [az-groep maken] [ az-group-create] opdracht voor het maken van een resourcegroep met de naam *myResourceGroup* in de *eastus* de regio. Een AKS-cluster met de naam *myAKSCluster* wordt vervolgens gemaakt met behulp van de [az aks maken] [ az-aks-create] opdracht. Een *--kubernetes-versie* van *1.12.6* wordt gebruikt om u te laten zien hoe u een knooppuntgroep in een volgende stap bijwerken. U kunt elk opgeven [Kubernetes-versie ondersteund][supported-versions].

```azurecli-interactive
# Create a resource group in East US
az group create --name myResourceGroup --location eastus

# Create a basic single-node AKS cluster
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --enable-vmss \
    --node-count 1 \
    --generate-ssh-keys \
    --kubernetes-version 1.12.6
```

Het duurt een paar minuten om het cluster te maken.

Wanneer het cluster gereed is, gebruikt u de [az aks get-credentials] [ az-aks-get-credentials] opdracht voor het ophalen van de clusterreferenties voor gebruik met `kubectl`:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="add-a-node-pool"></a>De knooppuntgroep van een toevoegen

Het cluster hebt gemaakt in de vorige stap heeft een groep met één knooppunt. We voegen een tweede knooppunt groep met de [az aks knooppuntgroep toevoegen] [ az-aks-nodepool-add] opdracht. Het volgende voorbeeld wordt een knooppunt van toepassingen met de naam *mynodepool* die wordt uitgevoerd *3* knooppunten:

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 3
```

Als de status van uw groepen wilt weergeven, gebruikt de [az aks knooppunt groep lijst] [ az-aks-nodepool-list] opdracht en geeft u de naam van uw resource en van het cluster:

```azurecli-interactive
az aks nodepool list --resource-group myResourceGroup --cluster-name myAKSCluster -o table
```

De volgende voorbeelduitvoer ziet u dat *mynodepool* is gemaakt met drie knooppunten in het knooppunt van toepassingen. Wanneer het AKS-cluster is gemaakt in de vorige stap, een standaard *nodepool1* is gemaakt met een aantal knooppunten van *1*.

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup         VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  --------------------  ---------------
VirtualMachineScaleSets  3        110        mynodepool  1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
```

> [!TIP]
> Als er geen *OrchestratorVersion* of *VmSize* is opgegeven bij het toevoegen van een knooppunt van toepassingen, de knooppunten zijn gemaakt op basis van de standaardwaarden voor het AKS-cluster. In dit voorbeeld, dat Kubernetes-versie was *1.12.6* en de knooppuntgrootte van *Standard_DS2_v2*.

## <a name="upgrade-a-node-pool"></a>Upgrade van een knooppunt van toepassingen

Wanneer uw AKS-cluster is gemaakt in de eerste stap van een `--kubernetes-version` van *1.12.6* is opgegeven. Laten we upgrade de *mynodepool* naar Kubernetes *1.12.7*. Gebruik de [az aks knooppunt groep upgrade] [ az-aks-nodepool-upgrade] opdracht bijwerken van de knooppuntgroep, zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive
az aks nodepool upgrade \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --kubernetes-version 1.12.7 \
    --no-wait
```

Lijst van de status van uw groepen opnieuw met behulp van de [az aks knooppunt groep lijst] [ az-aks-nodepool-list] opdracht. Het volgende voorbeeld ziet u dat *mynodepool* is in de *upgraden* status naar *1.12.7*:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup    VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  ---------------  ---------------
VirtualMachineScaleSets  3        110        mynodepool  1.12.7                 100             Linux     Upgrading            myResourceGroup  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.12.6                 100             Linux     Succeeded            myResourceGroup  Standard_DS2_v2
```

Het duurt een paar minuten aan de knooppunten upgraden naar de opgegeven versie.

Als een best practice, dient u alle groepen in een AKS-cluster te upgraden naar dezelfde versie van Kubernetes. De mogelijkheid om afzonderlijke knooppuntgroepen upgraden kunt u een rolling upgrade uitvoeren en schillen tussen knooppuntgroepen te onderhouden, actieve tijdsduur van toepassingen plannen.

## <a name="scale-a-node-pool"></a>Een knooppunt pool schalen

Als uw toepassing werkbelasting daarom vraagt, moet u mogelijk het aantal knooppunten in een knooppunt pool schalen. Het aantal knooppunten kunt omhoog of omlaag worden geschaald.

<!--If you scale down, nodes are carefully [cordoned and drained][kubernetes-drain] to minimize disruption to running applications.-->

Als u wilt schalen het aantal knooppunten in een pool knooppunt, gebruikt u de [az aks knooppunt pool schalen] [ az-aks-nodepool-scale] opdracht. Het volgende voorbeeld wordt het aantal knooppunten in geschaald *mynodepool* naar *5*:

```azurecli-interactive
az aks nodepool scale \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 5 \
    --no-wait
```

Lijst van de status van uw groepen opnieuw met behulp van de [az aks knooppunt groep lijst] [ az-aks-nodepool-list] opdracht. Het volgende voorbeeld ziet u dat *mynodepool* is in de *schalen* status met het nieuwe aantal *5* knooppunten:

```console
$ az aks nodepool list -g myResourceGroupPools --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup         VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  --------------------  ---------------
VirtualMachineScaleSets  5        110        mynodepool  1.12.7                 100             Linux     Scaling              myResourceGroupPools  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
```

Het duurt een paar minuten voor de schaalbewerking is voltooid.

## <a name="delete-a-node-pool"></a>De knooppuntgroep van een verwijderen

Als u een pool niet meer nodig hebt, kunt u deze verwijderen en verwijderen van de onderliggende VM-knooppunten. Als u wilt een knooppuntgroep verwijderen, gebruikt u de [az aks knooppuntgroep verwijderen] [ az-aks-nodepool-delete] opdracht en geeft u de naam van het knooppunt toepassingen. Hiermee verwijdert u het volgende voorbeeld de *mynoodepool* in de vorige stappen hebt gemaakt:

> [!CAUTION]
> Er zijn geen opties voor herstel voor verlies van gegevens die optreden kunnen wanneer u een knooppuntgroep verwijderen. Als schillen kunnen niet worden gepland op andere knooppuntgroepen, kan deze toepassingen zijn niet beschikbaar. Zorg ervoor dat u een knooppuntgroep niet verwijderen wanneer de toepassingen in gebruik niet over een gegevensback-ups of de mogelijkheid om uit te voeren op het andere knooppunt van toepassingen in uw cluster.

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster --name mynodepool --no-wait
```

De volgende voorbeelduitvoer van de [az aks knooppunt groep lijst] [ az-aks-nodepool-list] -opdracht laat zien dat *mynodepool* is in de *verwijderen* status:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup         VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  --------------------  ---------------
VirtualMachineScaleSets  5        110        mynodepool  1.12.7                 100             Linux     Deleting             myResourceGroupPools  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
```

Het duurt een paar minuten de knooppunten en het knooppunt-pool te verwijderen.

## <a name="specify-a-vm-size-for-a-node-pool"></a>Geef een VM-grootte voor een knooppunt van toepassingen

In de vorige voorbeelden om de knooppuntgroep van een te maken, is een standaard-VM-grootte voor de knooppunten die zijn gemaakt in het cluster gebruikt. Een veelvoorkomende scenario is waarmee u kunt groepen maken met andere VM-grootten en mogelijkheden. U kunt bijvoorbeeld een knooppuntgroep met knooppunten met een grote hoeveelheid CPU of geheugen of een knooppunt van toepassingen die ondersteuning bieden voor GPU maken. In de volgende stap maakt u [taints en tolerations](#schedule-pods-using-taints-and-tolerations) in het Kubernetes-scheduler hoe om te beperken van toegang tot schillen die kunnen worden uitgevoerd op deze knooppunten.

In het volgende voorbeeld maakt u een knooppunt op basis van GPU pool die gebruikmaakt van de *Standard_NC6* VM-grootte. Deze VM's worden aangestuurd door de NVIDIA Tesla R80-kaart. Zie voor meer informatie over beschikbare VM-grootten [grootten voor virtuele Linux-machines in Azure][vm-sizes].

Maken van een knooppunt van toepassingen met de [az aks knooppuntgroep toevoegen] [ az-aks-nodepool-add] opdracht opnieuw uit. Geef de naam van dit moment *gpunodepool*, en gebruik de `--node-vm-size` parameter opgeven voor de *Standard_NC6* grootte:

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name gpunodepool \
    --node-count 1 \
    --node-vm-size Standard_NC6 \
    --no-wait
```

De volgende voorbeelduitvoer van de [az aks knooppunt groep lijst] [ az-aks-nodepool-list] -opdracht laat zien dat *gpunodepool* is *maken* knooppunten met de opgegeven *VmSize*:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name         OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup         VmSize
-----------------------  -------  ---------  -----------  ---------------------  --------------  --------  -------------------  --------------------  ---------------
VirtualMachineScaleSets  1        110        gpunodepool  1.12.6                 100             Linux     Creating             myResourceGroupPools  Standard_NC6
VirtualMachineScaleSets  1        110        nodepool1    1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
```

Het duurt een paar minuten voor de *gpunodepool* om te worden gemaakt.

## <a name="schedule-pods-using-taints-and-tolerations"></a>Schillen met behulp van taints en tolerations plannen

U hebt nu twee knooppuntgroepen in het cluster - de standaardgroep knooppunt oorspronkelijk is gemaakt, en de pool op basis van GPU-knooppunt. Gebruik de [kubectl ophalen knooppunten] [ kubectl-get] opdracht om de knooppunten in uw cluster weer te geven. De volgende voorbeelduitvoer ziet u één knooppunt in elk knooppunt van toepassingen:

```console
$ kubectl get nodes

NAME                                 STATUS   ROLES   AGE     VERSION
aks-gpunodepool-28993262-vmss000000  Ready    agent   4m22s   v1.12.6
aks-nodepool1-28993262-vmss000000    Ready    agent   115m    v1.12.6
```

De scheduler Kubernetes kunt taints en tolerations gebruiken om te beperken welke workloads kunnen uitvoeren op de knooppunten.

* Een **taint** wordt toegepast op een knooppunt dat geeft aan dat alleen bepaalde schillen op deze kunnen worden gepland.
* Een **toleration** wordt vervolgens toegepast op een schil waarmee ze *tolereren* beïnvloeding van een knooppunt.

Zie voor meer informatie over het gebruik van functies van Kubernetes is gepland geavanceerde, [aanbevolen procedures voor geavanceerde scheduler-functies in AKS][taints-tolerations]

In dit voorbeeld wordt een beïnvloeding van toepassing op het knooppunt op basis van GPU met behulp van de [kubectl beïnvloeding knooppunt] [ kubectl-taint] opdracht. Geef de naam van uw op basis van GPU-knooppunt uit de uitvoer van de vorige `kubectl get nodes` opdracht. De beïnvloeding wordt toegepast als een *key: value* en vervolgens een optie voor planning. Het volgende voorbeeld wordt de *sku gpu =* worden gekoppeld en definieert schillen anders de *NoSchedule* mogelijkheid:

```console
kubectl taint node aks-gpunodepool-28993262-vmss000000 sku=gpu:NoSchedule
```

Het volgende eenvoudige voorbeeld YAML-manifest maakt gebruik van een toleration om toe te staan de Kubernetes-scheduler om uit te voeren van een NGINX-schil op het knooppunt op basis van GPU. Zie voor een voorbeeld meer geschikt is, maar tijdrovende taak uit te voeren een Tensorflow op basis van de gegevensset MNIST [GPU's gebruiken voor rekenintensieve workloads in AKS][gpu-cluster].

Maak een bestand met de naam `gpu-toleration.yaml` en kopieer de volgende YAML-voorbeeld:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - image: nginx:1.15.9
    name: mypod
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 1
        memory: 2G
  tolerations:
  - key: "sku"
    operator: "Equal"
    value: "gpu"
    effect: "NoSchedule"
```

Plant de schil met de `kubectl apply -f gpu-toleration.yaml` opdracht:

```console
kubectl apply -f gpu-toleration.yaml
```

Het duurt een paar seconden voor het plannen van de schil en haal de NGINX-installatiekopie. Gebruik de [kubectl beschrijven pod] [ kubectl-describe] opdracht om de status van de schil weer te geven. Het volgende verkorte voorbeeld uitvoer toont de *sku = gpu:NoSchedule* toleration wordt toegepast. In de sectie gebeurtenissen de scheduler de schil is toegewezen de *aks-gpunodepool-28993262-vmss000000* op basis van GPU-knooppunt:

```console
$ kubectl describe pod mypod

[...]
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
                 sku=gpu:NoSchedule
Events:
  Type    Reason     Age    From                                          Message
  ----    ------     ----   ----                                          -------
  Normal  Scheduled  4m48s  default-scheduler                             Successfully assigned default/mypod to aks-gpunodepool-28993262-vmss000000
  Normal  Pulling    4m47s  kubelet, aks-gpunodepool-28993262-vmss000000  pulling image "nginx:1.15.9"
  Normal  Pulled     4m43s  kubelet, aks-gpunodepool-28993262-vmss000000  Successfully pulled image "nginx:1.15.9"
  Normal  Created    4m40s  kubelet, aks-gpunodepool-28993262-vmss000000  Created container
  Normal  Started    4m40s  kubelet, aks-gpunodepool-28993262-vmss000000  Started container
```

Alleen schillen waarvoor deze beïnvloeding toegepast kunnen worden gepland op knooppunten in *gpunodepool*. Alle andere pod zou worden gepland de *nodepool1* knooppuntgroep. Als u extra knooppunt-adresgroepen maakt, kunt u extra taints en tolerations om te beperken welke schillen worden gepland voor deze resources knooppunt.

## <a name="manage-node-pools-using-a-resource-manager-template"></a>Beheren van groepen met behulp van een Resource Manager-sjabloon

Wanneer u een Azure Resource Manager-sjabloon gebruikt om te maken en beheerde resources, kunt u doorgaans de instellingen in uw sjabloon en opnieuw implementeren voor het bijwerken van de resource bijwerken. Met groepen in AKS, kan het profiel van de groep Beginaantal knooppunten kan niet worden bijgewerkt nadat het AKS-cluster is gemaakt. Dit gedrag betekent dat u kan niet bijwerken van een bestaande Resource Manager-sjabloon, een wijziging in het knooppuntgroepen aanbrengt en opnieuw implementeren. In plaats daarvan moet u een afzonderlijke Resource Manager-sjabloon die alleen de agentpools voor een bestaand AKS-cluster worden bijgewerkt.

Maak een sjabloon, zoals `aks-agentpools.json` en plak het volgende voorbeeld-manifest. Deze voorbeeldsjabloon configureert u de volgende instellingen:

* Updates de *Linux* agentpool met de naam *myagentpool* om uit te voeren drie knooppunten.
* Hiermee stelt u de knooppunten in de knooppuntgroep om uit te voeren van Kubernetes-versie *1.12.8*.
* Bepaalt de grootte van het knooppunt als *Standard_DS2_v2*.

Deze waarden worden bewerkt omdat nodig hebt om bij te werken, toevoegen of knooppuntgroepen verwijderen indien nodig:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "clusterName": {
      "type": "string",
      "metadata": {
        "description": "The name of your existing AKS cluster."
      }
    },
    "location": {
      "type": "string",
      "metadata": {
        "description": "The location of your existing AKS cluster."
      }
    },
    "agentPoolName": {
      "type": "string",
      "defaultValue": "myagentpool",
      "metadata": {
        "description": "The name of the agent pool to create or update."
      }
    },
    "vnetSubnetId": {
      "type": "string",
      "defaultValue": "",
      "metadata": {
        "description": "The Vnet subnet resource ID for your existing AKS cluster."
      }
    }
  },
  "variables": {
    "apiVersion": {
      "aks": "2019-04-01"
    },
    "agentPoolProfiles": {
      "maxPods": 30,
      "osDiskSizeGB": 0,
      "agentCount": 3,
      "agentVmSize": "Standard_DS2_v2",
      "osType": "Linux",
      "vnetSubnetId": "[parameters('vnetSubnetId')]"
    }
  },
  "resources": [
    {
      "apiVersion": "2019-04-01",
      "type": "Microsoft.ContainerService/managedClusters/agentPools",
      "name": "[concat(parameters('clusterName'),'/', parameters('agentPoolName'))]",
      "location": "[parameters('location')]",
      "properties": {
            "maxPods": "[variables('agentPoolProfiles').maxPods]",
            "osDiskSizeGB": "[variables('agentPoolProfiles').osDiskSizeGB]",
            "count": "[variables('agentPoolProfiles').agentCount]",
            "vmSize": "[variables('agentPoolProfiles').agentVmSize]",
            "osType": "[variables('agentPoolProfiles').osType]",
            "storageProfile": "ManagedDisks",
      "type": "VirtualMachineScaleSets",
            "vnetSubnetID": "[variables('agentPoolProfiles').vnetSubnetId]",
            "orchestratorVersion": "1.12.8"
      }
    }
  ]
}
```

Implementeer deze sjabloon met de [az group deployment maken] [ az-group-deployment-create] opdracht, zoals wordt weergegeven in het volgende voorbeeld. U wordt gevraagd voor de bestaande naam van de AKS-cluster en de locatie:

```azurecli-interactive
az group deployment create \
    --resource-group myResourceGroup \
    --template-file aks-agentpools.json
```

Het duurt een paar minuten om bij te werken van uw AKS-cluster, afhankelijk van de instellingen van knooppunt en de bewerkingen die u in het Resource Manager-sjabloon definieert.

## <a name="clean-up-resources"></a>Resources opschonen

In dit artikel hebt u een AKS-cluster met knooppunten op basis van GPU gemaakt. Om onnodige kosten reduceren, kunt u verwijdert de *gpunodepool*, of het hele AKS-cluster.

Als u wilt verwijderen van het knooppunt op basis van GPU pool, gebruiken de [az aks nodepool verwijderen] [ az-aks-nodepool-delete] opdracht zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster --name gpunodepool
```

Als u wilt verwijderen van het cluster zelf, gebruikt u de [az group delete] [ az-group-delete] opdracht om de AKS-resourcegroep te verwijderen:

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over het maken en beheren van meerdere groepen in een AKS-cluster. Zie voor meer informatie over het beheren van pods in knooppuntgroepen [aanbevolen procedures voor geavanceerde scheduler-functies in AKS][operator-best-practices-advanced-scheduler].

Als u wilt maken en gebruiken van groepen met Windows Server-container, Zie [een Windows Server-container in AKS maakt][aks-windows].

<!-- EXTERNAL LINKS -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-taint]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#taint
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- INTERNAL LINKS -->
[azure-cli-install]: /cli/azure/install-azure-cli
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-nodepool-add]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-add
[az-aks-nodepool-list]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-list
[az-aks-nodepool-upgrade]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-upgrade
[az-aks-nodepool-scale]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-scale
[az-aks-nodepool-delete]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-delete
[vm-sizes]: ../virtual-machines/linux/sizes.md
[taints-tolerations]: operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations
[gpu-cluster]: gpu-cluster.md
[az-group-delete]: /cli/azure/group#az-group-delete
[install-azure-cli]: /cli/azure/install-azure-cli
[supported-versions]: supported-kubernetes-versions.md
[operator-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[aks-windows]: windows-container-cli.md
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md

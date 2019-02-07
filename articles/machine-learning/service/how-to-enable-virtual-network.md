---
title: Experimenten & Deductie uitvoeren in een virtueel netwerk
titleSuffix: Azure Machine Learning service
description: Machine learning-experimenten en inferentietaken beveiligen in een Azure-netwerk worden uitgevoerd. Informatie over het maken van de compute-doelen voor modeltraining en Deductie binnen een virtueel Azure-netwerk. Dit geldt ook voor vereisten voor beveiligde virtuele netwerken, zoals vereisen binnenkomende en uitgaande poorten.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 01/08/2019
ms.openlocfilehash: 2e7f6c066ea254fff90ba2f9ff1f559fdb680ddf
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55766687"
---
# <a name="securely-run-experiments-and-inferencing-inside-an-azure-virtual-network"></a>Experimenten en inferentietaken in een Azure-netwerk veilig uitvoeren

In dit artikel leert u hoe u uw experimenten en inferentietaken binnen een Virtueelnetwerk uit te voeren. Een virtueel netwerk fungeert als een beveiligingsgrens isoleren van uw Azure-resources op het openbare internet. U kunt ook een Azure-netwerk toevoegen aan uw on-premises netwerk. Hiermee kunt u veilig uw modellen trainen en toegang tot uw geïmplementeerde modellen voor inferentietaken.

De Azure Machine Learning-service is afhankelijk van andere Azure-services voor dingen die rekenresources. COMPUTE-resources (compute-doelen) worden gebruikt om te trainen en modellen te implementeren. Deze compute-doelen kunnen worden gemaakt binnen een virtueel netwerk. Bijvoorbeeld, kunt u een Data Science VM naar een model te trainen en vervolgens het model implementeren in Azure Kubernetes Service. Zie voor meer informatie over virtuele netwerken, de [overzicht van virtuele netwerken](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) document.

## <a name="storage-account-for-your-workspace"></a>Storage-account voor uw werkruimte

Wanneer u een Azure Machine Learning-service-werkruimte maakt, moeten een Azure Storage-account. Schakel niet in de firewallregels voor dit opslagaccount wordt gebruikt. De Azure Machine Learning-service is onbeperkte toegang tot het opslagaccount vereist.

Als u niet zeker weet of u deze instellingen hebt gewijzigd of niet, Zie de __wijzigen van de standaardtoegangsregel voor netwerk__ sectie van de [Azure Storage configureren van firewalls en virtuele netwerken](https://docs.microsoft.com/azure/storage/common/storage-network-security) document en gebruiken de stappen voor het _toegang toestaan_ van __alle netwerken__.

## <a name="use-machine-learning-compute"></a>Gebruik van Machine Learning-Computing

Voor het gebruik van Machine Learning-Computing in een virtueel netwerk, gebruikt u de volgende informatie om te begrijpen netwerkvereisten:

- Het virtuele netwerk moet zich in hetzelfde abonnement en dezelfde regio als de werkruimte van de Azure Machine Learning-service.

- Het subnet opgegeven voor de Machine Learning-Computing-cluster moeten hebben onvoldoende vrije IP-adressen voor het aantal virtuele machines het doel voor het cluster. Als het subnet niet genoeg niet-toegewezen IP-adressen, wordt het cluster gedeeltelijk toegewezen.

- Als u van plan bent het virtuele netwerk beveiligen door het beperken van verkeer, moet u enkele poorten openen voor de service Machine Learning-Computing laten. Zie voor meer informatie de [poorten vereist](#mlcports) sectie.

- Controleer of uw beveiligingsbeleid of de vergrendelingen op het abonnement of de resourcegroep van het virtuele netwerk machtigingen voor het beheren van het virtuele netwerk beperken.

- Als u plaatsen van meerdere Machine Learning-Computing-clusters in één virtueel netwerk wilt, moet u mogelijk een verhoging aanvragen voor een of meer resources.

    Machine Learning-Computing, wijst automatisch extra netwerkresources in de resourcegroep met het virtuele netwerk toe. Voor elk cluster Machine Learning-Computing wijst Azure Machine Learning-service de volgende bronnen: 

    - Een netwerkbeveiligingsgroep (NSG)

    - Een openbaar IP-adres

    - Een load balancer

    De beperkingen die voor deze resources gelden, worden bepaald door de [resourcequota](https://docs.microsoft.com/azure/azure-subscription-service-limits) van het abonnement. 

### <a id="mlcports"></a> Vereiste poorten

Machine Learning-Computing, gebruikt momenteel de Azure Batch-Service voor het inrichten van virtuele machines in de opgegeven virtuele netwerk. Het subnet moet binnenkomende communicatie van de Batch-service toestaan. Deze communicatie wordt gebruikt om te plannen wordt uitgevoerd op de Machine Learning-Computing-knooppunten, en om te communiceren met Azure Storage en andere bronnen. Batch voegt nsg's toe op het niveau van netwerkinterfaces (NIC's) die zijn gekoppeld aan virtuele machines. Met deze netwerkbeveiligingsgroepen worden automatisch binnenkomende en uitgaande regels geconfigureerd om het volgende verkeer toe te staan:

- Binnenkomend TCP-verkeer op de poorten 29876 en 29877 van IP-adressen van Batch-servicerollen. 
 
- Binnenkomend TCP-verkeer op poort 22 om externe toegang te verlenen.
 
- Uitgaand verkeer op een willekeurige poort naar het virtuele netwerk.

- Uitgaand verkeer op een willekeurige poort naar internet.

Wees voorzichtig als u wijzigt of regels voor binnenkomend en uitgaand in Batch geconfigureerd nsg's toevoegen. Als een NSG-blokken-communicatie naar de rekenknooppunten, stelt de Machine Learning-Computing services de status van de rekenknooppunten op onbruikbaar.

U hoeft netwerkbeveiligingsgroepen niet op te geven op subnetniveau omdat Batch zijn eigen netwerkbeveiligingsgroepen configureert. Echter, als het opgegeven subnet zijn gekoppeld nsg's en/of een firewall, configureren de binnenkomende en uitgaande beveiligingsregels zoals eerder vermeld. De volgende schermafbeeldingen laten zien hoe de regelconfiguratie van de ziet er uit in Azure portal:

![Schermafbeelding van inkomende NSG-regels voor Machine Learning-Computing](./media/how-to-enable-virtual-network/amlcompute-virtual-network-inbound.png)

![Schermafbeelding van uitgaande NSG-regels voor Machine Learning-Computing](./media/how-to-enable-virtual-network/experimentation-virtual-network-outbound.png)

### <a name="create-machine-learning-compute-in-a-virtual-network"></a>Machine Learning-Computing in een virtueel netwerk maken

Het maken van een Machine Learning-Computing-cluster met de **Azure-portal**, gebruikt u de volgende stappen uit:

1. Uit de [Azure-portal](https://portal.azure.com), selecteert u de werkruimte van uw Azure Machine Learning-service.

1. Uit de __toepassing__ sectie, selecteer __Compute__. Selecteer vervolgens __compute toevoegen__. 

    ![Een compute in Azure Machine Learning-service toevoegen](./media/how-to-enable-virtual-network/add-compute.png)

1. Gebruik deze opties voor het configureren van deze compute-resource voor het gebruik van een virtueel netwerk:

    - __Netwerkconfiguratie__: Selecteer __Geavanceerd__.

    - __Resourcegroep__: Selecteer de resourcegroep waarin het virtuele netwerk.

    - __Virtueel netwerk__: Selecteer het virtuele netwerk waarin het subnet.

    - __Subnet__: Selecteer het subnet te gebruiken.

    ![Een schermafbeelding van virtuele-netwerkinstellingen voor machine learning-Computing](./media/how-to-enable-virtual-network/amlcompute-virtual-network-screen.png)

U kunt ook maken een Machine Learning-Computing-cluster met de **SDK van Azure Machine Learning**. De volgende code maakt een nieuwe Machine Learning-Computing-cluster in de `default` subnet van een virtueel netwerk met de naam `mynetwork`:

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# The Azure Virtual Network name, subnet, and resource group
vnet_name = 'mynetwork'
subnet_name = 'default'
vnet_resourcegroup_name = 'mygroup'

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print("Found existing cpucluster")
except ComputeTargetException:
    print("Creating new cpucluster")
    
    # Specify the configuration for the new cluster
    compute_config = AmlCompute.provisioning_configuration(vm_size="STANDARD_D2_V2",
                                                           min_nodes=0,
                                                           max_nodes=4,
                                                           vnet_resourcegroup_name = vnet_resourcegroup_name,
                                                           vnet_name = vnet_name,
                                                           subnet_name = subnet_name)

    # Create the cluster with the specified name and configuration
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)
    
    # Wait for the cluster to complete, show the output log
    cpu_cluster.wait_for_completion(show_output=True)
```

Als het proces voor het maken is voltooid, kunt u uw model met behulp van het cluster te trainen. Zie voor meer informatie de [selecteert en een compute-doel gebruiken voor training](how-to-set-up-training-targets.md) document.

## <a name="use-a-virtual-machine-or-hdinsight"></a>Een virtuele Machine of HDInsight

Als u een virtuele machine of de HDInsight-cluster in een Virtueelnetwerk met uw werkruimte, gebruikt u de volgende stappen uit:

> [!IMPORTANT]
> De service Azure Machine Learning biedt alleen ondersteuning voor virtuele machines met Ubuntu.

1. Maken van een virtuele machine of de HDInsight-cluster met behulp van Azure portal, Azure CLI, enzovoort, en plaatst deze in een Azure-netwerk. Zie de volgende documenten voor meer informatie:
    * [Maken en beheren van virtuele Azure-netwerken voor virtuele Linux-machines](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)

    * [HDInsight met behulp van Azure Virtual Networks uitbreiden](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network) 

1. Als u wilt toestaan dat de service Azure Machine Learning om te communiceren met de SSH-poort op de virtuele machine of het cluster, moet u een vermelding configureren voor de Netwerkbeveiligingsgroep. De SSH-poort is meestal poort 22. Als u wilt dat verkeer van deze bron, gebruik de volgende informatie:

    * __Bron__: Selecteer __Servicetag__

    * __Bronservicetag__: Select __AzureMachineLearning__

    * __Poortbereiken van bron__: Selecteer __*__

    * __Bestemming__: Selecteer __elke__

    * __Poortbereiken van doel__: Selecteer __22__

    * __Protocol__: Selecteer __elke__

    * __Actie__: Selecteer __toestaan__

   ![Schermafbeelding van regels voor binnenkomende verbindingen voor het uitvoeren van experimenten op virtuele machine of HDInsight binnen een virtueel netwerk](./media/how-to-enable-virtual-network/experimentation-virtual-network-inbound.png)

    Behoud de standaardwaarde regels voor uitgaand verkeer voor de Netwerkbeveiligingsgroep. Zie voor meer informatie de sectie standaard beveiliging regels van de [beveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules) documentatie.
    
1. De virtuele machine of de HDInsight-cluster koppelen aan uw werkruimte van Azure Machine Learning-service. Zie voor meer informatie de [instellen van compute-doelen voor modeltraining](how-to-set-up-training-targets.md) document.

## <a name="use-azure-kubernetes-service-aks"></a>Gebruik Azure Kubernetes Service (AKS)

> [!IMPORTANT]
> Controleer de vereisten en plan de IP-adressen voor uw cluster voordat u doorgaat met de stappen die hieronder worden vermeld. U kunt verwijzen naar [configureren geavanceerde netwerken in Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/configure-advanced-networking)
> 
> Behoud de standaardwaarde regels voor uitgaand verkeer voor de Netwerkbeveiligingsgroep. Zie voor meer informatie de sectie standaard beveiliging regels van de [beveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules) documentatie.
>
> Azure Kubernetes Service en het Azure-netwerk moet zich in dezelfde regio bevinden.

U kunt Azure Kubernetes Service in een Virtueelnetwerk toevoegen aan uw werkruimte met de volgende stappen uit de __Azure-portal__:

1. Uit de [Azure-portal](https://portal.azure.com), selecteert u de werkruimte van uw Azure Machine Learning-service.

1. Uit de __toepassing__ sectie, selecteer __Compute__. Selecteer vervolgens __compute toevoegen__. 

    ![Een compute in Azure Machine Learning-service toevoegen](./media/how-to-enable-virtual-network/add-compute.png)

1. Gebruik deze opties voor het configureren van deze compute-resource voor het gebruik van een virtueel netwerk:

    - __Netwerkconfiguratie__: Selecteer __Geavanceerd__.

    - __Resourcegroep__: Selecteer de resourcegroep waarin het virtuele netwerk.

    - __Virtueel netwerk__: Selecteer het virtuele netwerk waarin het subnet.

    - __Subnet__: Selecteer het subnet.

    - __Kubernetes-Service-adresbereik__: Selecteer het adresbereik van Kubernetes-Service. Dit adresbereik maakt gebruik van een CIDR-notatie IP-adresbereik voor het definiëren van de IP-adressen beschikbaar zijn voor het cluster. Deze mag niet overlappen met Subnet IP-bereiken. Bijvoorbeeld: 10.0.0.0/16.

    - __IP-adres van Kubernetes DNS-service__: Selecteer het IP-adres van de Kubernetes-DNS-service. Dit IP-adres is toegewezen aan de Kubernetes-DNS-service. Het moet binnen het adresbereik van Kubernetes-Service. Bijvoorbeeld: 10.0.0.10.

    - __Docker bridge-adres__: Selecteer het Docker bridge-adres. Dit IP-adres is toegewezen aan Docker Bridge. Het moet niet in Subnet IP-bereiken of het adresbereik van Kubernetes-Service. Bijvoorbeeld: 172.17.0.1/16

   ![Azure Machine Learning-service: Machine Learning-Computing virtuele netwerkinstellingen](./media/how-to-enable-virtual-network/aks-virtual-network-screen.png)

    > [!TIP]
    > Als u al een AKS-cluster in een Virtueelnetwerk hebt, kunt u deze koppelen aan de werkruimte. Zie voor meer informatie, [implementeren naar AKS](how-to-deploy-to-aks.md).

U kunt ook de **SDK van Azure Machine Learning** Azure Kubernetes service toevoegen in een Virtueelnetwerk. De volgende code maakt een nieuwe Azure Kubernetes Service in de `default` subnet van een virtueel netwerk met de naam `mynetwork`:

```python
from azureml.core.compute import ComputeTarget, AksCompute

# Create the compute configuration and set virtual network information
config = AksCompute.provisioning_configuration(location="eastus2")
config.vnet_resourcegroup_name = "mygroup"
config.vnet_name = "mynetwork"
config.subnet_name = "default"
config.service_cidr = "10.0.0.0/16"
config.dns_service_ip = "10.0.0.10"
config.docker_bridge_cidr = "172.17.0.1/16"

# Create the compute target
aks_target = ComputeTarget.create(workspace = ws,
                                  name = "myaks",
                                  provisioning_configuration = config)
```

Zodra het proces voor het maken is voltooid, kunt u inferentietaken op een AKS-cluster achter een virtueel netwerk kunt doen. Zie voor meer informatie, [implementeren naar AKS](how-to-deploy-to-aks.md).

## <a name="next-steps"></a>Volgende stappen

* [Training omgevingen instellen](how-to-set-up-training-targets.md)
* [Waar u modellen implementeren](how-to-deploy-and-where.md)
* [Veilig Implementeer modellen met SSL](how-to-secure-web-service.md)


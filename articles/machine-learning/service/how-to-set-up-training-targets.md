---
title: COMPUTE-doelen voor modeltraining
titleSuffix: Azure Machine Learning service
description: Configureer de training-omgevingen (compute-doelen) voor machine learning-modeltraining. U kunt eenvoudig schakelen tussen omgevingen voor training. Start lokaal training. Als u nodig hebt om uit te schalen, kunt u overschakelen naar een cloud-gebaseerde compute-doel.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.reviewer: larryfr
manager: cgronlun
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 9037f6d7602f186bc30e55acbc050280bca134ee
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53794461"
---
# <a name="set-up-compute-targets-for-model-training"></a>Compute-doelen voor modeltraining instellen

Met de Azure Machine Learning-service, kunt u uw model op verschillende compute-resources te trainen. Deze compute-resources, met de naam __compute-doelen__, lokaal of in de cloud kan zijn. In dit artikel leert u over de ondersteunde compute-doelen en het gebruik ervan.

Een compute-doel is een resource waar u uw trainingsscript, of u host uw model uitvoeren wanneer deze wordt geïmplementeerd als een webservice. U kunt maken en beheren van een compute-doel met behulp van de SDK van Azure Machine Learning, Azure portal of de Azure CLI. Als u de compute-doelen die zijn gemaakt via een andere service (bijvoorbeeld een Azure HDInsight-cluster) hebt, kunt u deze doelen door ze te koppelen aan uw werkruimte van Azure Machine Learning-service.

Er zijn drie hoofdcategorieën worden onderverdeeld van compute-doelen die ondersteuning biedt voor Azure Machine Learning:

* __Lokale__: Uw lokale computer of een cloud-gebaseerde virtuele machine (VM) die u gebruikt als een omgeving voor ontwikkeling en experimenteren. 
* __Beheerde compute__: Azure Machine Learning-Computing is een compute-aanbieding die wordt beheerd door de Azure Machine Learning-service. De aanbieding kunt u eenvoudig een enkele of meerdere knooppunten compute voor trainingen, testen en batch inferentietaken kunt maken.
* __Gekoppelde compute__: U kunt ook doen om uw eigen Azure-cloud-computing en deze koppelen aan Azure Machine Learning. Meer informatie over ondersteunde rekentypen en het gebruik ervan in de volgende secties.


## <a name="supported-compute-targets"></a>Ondersteunde compute-doelen

Azure Machine Learning-service heeft verschillende ondersteuning voor de verschillende compute-doelen. Een typische model ontwikkelingscyclus begint met de ontwikkeling en experimenteren op een kleine hoeveelheid gegevens. In deze fase, raden wij aan u een lokale omgeving, zoals uw lokale computer of een cloud-gebaseerde VM. Als u uw training voor grotere gegevenssets kunt opschalen of gedistribueerde training, gebruik een Azure Machine Learning compute-omgeving te maken van één of meerdere knooppunten cluster die automatisch wordt geschaald telkens wanneer indienen u een uitvoering. U kunt ook uw eigen compute-resource koppelen, hoewel ondersteuning voor verschillende scenario's als variëren kunnen in de volgende tabel beschreven:

|COMPUTE-doel| GPU-versnelling | Geautomatiseerd<br/> hyperparameter afstemmen | Geautomatiseerd</br> machine learning | Geschikt voor pijplijn|
|----|:----:|:----:|:----:|:----:|
|[Lokale computer](#local)| Misschien | &nbsp; | ✓ | &nbsp; |
|[Azure Machine Learning-Computing](#amlcompute)| ✓ | ✓ | ✓ | ✓ |
|[Externe virtuele machine](#vm) | ✓ | ✓ | ✓ | ✓ |
|[Azure Databricks](#databricks)| &nbsp; | &nbsp; | ✓ | ✓[*](#pipeline-only) |
|[Azure Data Lake Analytics](#adla)| &nbsp; | &nbsp; | &nbsp; | ✓[*](#pipeline-only) |
|[Azure HDInsight](#hdinsight)| &nbsp; | &nbsp; | &nbsp; | ✓ |

> [!IMPORTANT]
> <a id="pipeline-only"></a>__*__ _Azure Databricks en Azure Data Lake Analytics kan alleen worden gebruikt in een pijplijn._<br/>
> Zie voor meer informatie over pijplijnen [pijplijnen in Azure Machine Learning](concept-ml-pipelines.md).
>
> De Azure Machine Learning-rekenomgeving moet worden gemaakt uit in een werkruimte. U kunt bestaande exemplaren niet koppelen aan een werkruimte.
>
> Andere compute-doelen moeten worden gemaakt van buiten Azure Machine Learning en vervolgens gekoppeld aan uw werkruimte.
>
> Wanneer u een model te trainen, rekenknooppunten sommige doelen zijn afhankelijk van de Docker-containerinstallatiekopieën. De GPU-basisinstallatiekopie moet alleen worden gebruikt op Microsoft Azure-Services. Voor het trainen van het model, de services zijn onder andere:
> * Azure Machine Learning-Computing
> * Azure Kubernetes Service
> * Windows Data Science Virtual Machine (DSVM)

## <a name="workflow"></a>Werkstroom

De werkstroom voor het ontwikkelen en implementeren van een model met Azure Machine Learning met de volgende stappen:

1. Ontwikkel de machine learning-cursussen scripts in Python.
1. Maken en configureren van de compute-doel, of koppelen van een bestaande compute-doel.
1. Verzenden van de trainingsscripts naar de compute-doel.
1. Inspecteer de resultaten om het beste model.
1. Registreer het model in het register model.
1. Het model implementeren.

> [!NOTE]
> Uw trainingsscript is niet gebonden aan een specifieke compute-doel. U kunt in eerste instantie trainen op uw lokale computer en vervolgens overschakelen van compute-doelen zonder dat u hoeft te herschrijven van het trainingsscript.
> 
> Als u een compute-doel aan uw werkruimte koppelt met het maken van een beheerde berekend of door het koppelen van een bestaande compute, Geef een naam op voor uw computer. De naam moet tussen 2 en 16 tekens lang zijn.

Als u wilt overschakelen van een compute-doel naar een andere, moet u een [uitvoerconfiguratie](concept-azure-machine-learning-architecture.md#run-configuration). De configuratie van de uitvoering wordt gedefinieerd hoe u het script uitvoert op de compute-doel.

## <a name="training-scripts"></a>Trainingsscripts

Wanneer u een uitvoering training start, wordt een momentopname van de map waarin uw trainingsscripts bevat en verzendt dit naar de compute-doel gemaakt. Zie voor meer informatie, [momentopnamen](concept-azure-machine-learning-architecture.md#snapshot).

## <a id="local"></a>Lokale computer

Wanneer u lokaal trainen, gebruikt u de SDK om in te dienen de trainingsbewerking. U kunt trainen met behulp van een gebruiker of systeem beheerde omgeving.

### <a name="user-managed-environment"></a>Gebruiker-beheerde omgeving

Zorg dat alle benodigde pakketten zijn beschikbaar in de Python-omgeving waarin u het script uitvoert in een gebruiker beheerde omgeving. Het volgende codefragment wordt een voorbeeld van hoe u training voor een gebruiker beheerde omgeving configureert:

```python
from azureml.core.runconfig import RunConfiguration

# Edit a run configuration property on the fly.
run_config_user_managed = RunConfiguration()

run_config_user_managed.environment.python.user_managed_dependencies = True

# Choose a specific Python environment by pointing to a Python path. For example:
# run_config.environment.python.interpreter_path = '/home/ninghai/miniconda3/envs/sdk2/bin/python'
```

  
### <a name="system-managed-environment"></a>Systeem-beheerde omgeving

Door systeem beheerde omgevingen, is afhankelijk van conda om de afhankelijkheden te beheren. Conda maakt u een bestand met de naam **conda_dependencies.yml** die een lijst van de afhankelijkheden bevat. U kunt het systeem te bouwen van een nieuwe conda-omgeving en er u uw scripts uitvoeren op te vragen. Systeem beheerde omgevingen kunnen later worden hergebruikt, zolang het conda_dependencies.yml-bestand ongewijzigd ten opzichte is. 

De eerste installatie van een nieuwe omgeving kan enkele minuten duren, op basis van de grootte van de vereiste afhankelijkheden. Het volgende codefragment laat zien hoe u het maken van een systeem beheerde omgeving die afhankelijk zijn van scikit-informatie:

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

run_config_system_managed = RunConfiguration()

run_config_system_managed.environment.python.user_managed_dependencies = False
run_config_system_managed.auto_prepare_environment = True

# Specify the conda dependencies with scikit-learn

run_config_system_managed.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
```

## <a id="amlcompute"></a>Azure Machine Learning-Computing

Azure Machine Learning-Computing is een beheerde-compute-infrastructuur waarmee de gebruiker eenvoudig een rekenkracht van één of meerdere knooppunten te maken. De berekening die wordt gemaakt binnen de regio van uw werkruimte als een resource die kan worden gedeeld met andere gebruikers in uw werkruimte. De berekening die automatisch wordt geschaald als een taak wordt verzonden en kan worden geplaatst in een Azure-netwerk. De berekening die wordt uitgevoerd in een omgeving met beperkte en de afhankelijkheden van uw model in een Docker-container-pakketten.

U kunt Azure Machine Learning-Computing gebruiken voor het distribueren van het trainingsproces in een cluster van de CPU of GPU-computerknooppunten in de cloud. Zie voor meer informatie over de VM-grootten die GPU's omvatten [GPU-geoptimaliseerde VM-grootten](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu).

> [!NOTE]
> Azure Machine Learning-Computing heeft standaardlimieten, zoals het aantal kernen dat kan worden toegewezen. Zie voor meer informatie, [beheren en aanvraag quota's voor Azure-resources](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-quotas).

U kunt een Azure Machine Learning-rekenomgeving maken op aanvraag wanneer u plant een uitvoering, of als een permanente resource.

### <a name="run-based-creation"></a>Uitvoeren op basis van het maken

U kunt een Azure Machine Learning-rekenomgeving maken als een compute-doel tijdens runtime. De compute wordt automatisch gemaakt voor uw uitvoering en kan worden geschaald om naar het aantal **max_nodes** die u opgeeft in de configuratie uitvoeren. De berekening wordt automatisch verwijderd nadat de uitvoering is voltooid.

> [!IMPORTANT]
> Uitvoeren op basis van het maken van een compute-omgeving van Azure Machine Learning is momenteel in Preview. Gebruik niet maken op basis van een uitvoeren als u automatische hyperparameter afstemmen of automatische leerprocessen. Voor het gebruik van hyperparameter afstemmen of geautomatiseerde voor machine learning, de compute-omgeving met Azure Machine Learning te maken voordat u een uitvoering indient.

```python
from azureml.core.compute import ComputeTarget, AmlCompute

# First, list the supported VM families for Azure Machine Learning Compute
AmlCompute.supported_vmsizes()

from azureml.core.runconfig import RunConfiguration

# Create a new runconfig object
run_config = RunConfiguration()

# Signal that you want to use AmlCompute to execute the script
run_config.target = "amlcompute"

# AmlCompute is created in the same region as your workspace
# Set the VM size for AmlCompute from the list of supported_vmsizes
run_config.amlcompute.vm_size = 'STANDARD_D2_V2'

```

### <a name="persistent-compute-basic"></a>Permanente compute: Basic

Een permanente Azure Machine Learning-compute-omgeving kan worden hergebruikt binnen taken. De berekening die kan worden gedeeld met andere gebruikers in de werkruimte en tussen taken wordt bewaard.

Als u wilt een permanente bron van Azure Machine Learning compute-omgeving maakt, geeft u de **vm_size** en **max_nodes** eigenschappen. Azure Machine Learning maakt vervolgens gebruik van slimme standaardinstellingen voor de overige eigenschappen. De compute automatisch wordt geschaald naar nul knooppunten wanneer deze wordt niet gebruikt, en maakt toegewezen virtuele machines om uit te voeren van uw taken, indien nodig. 

* **vm_size**: De VM-reeks van de knooppunten die zijn gemaakt door Azure Machine Learning-Computing.
* **max_nodes**: Het maximale aantal knooppunten automatisch te schalen tot wanneer u een taak op Azure Machine Learning-Computing uitvoert.

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that the cluster doesn't already exist
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                           max_nodes=4)
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

cpu_cluster.wait_for_completion(show_output=True)

```

### <a name="persistent-compute-advanced"></a>Permanente compute: Geavanceerd

U kunt ook verschillende geavanceerde eigenschappen configureren bij het maken van een Azure Machine Learning-rekenomgeving. De eigenschappen kunnen u een permanente cluster vaste grootte of binnen een bestaande Azure-netwerk maken in uw abonnement.

Samen met de **vm_size** en **max_nodes** eigenschappen, u kunt ook de volgende eigenschappen:

* **min_nodes**: Het minimum aantal knooppunten te verkleinen als u een taak op een Azure Machine Learning uitvoert-rekenomgeving. De minimale standaardwaarde is nul (0) knooppunten.
* **vm_priority**: Het type van de virtuele machine moet worden gebruikt bij het maken van een resource van Azure Machine Learning compute-omgeving. Kiezen tussen **toegewezen** (standaard) en **lowpriority**. VM's met lage prioriteit gebruiken de overtollige capaciteit in Azure. Deze VM's goedkoper zijn, maar wordt uitgevoerd kunnen voorrang worden genomen wanneer deze VM's worden gebruikt.
* **idle_seconds_before_scaledown**: De hoeveelheid niet-actieve tijd moet worden gewacht nadat een uitvoering is voltooid en voordat de functie voor automatisch schalen tot maximaal het aantal **min_nodes**. De niet-actieve tijd van de standaardwaarde is 120 seconden.
* **vnet_resourcegroup_name**: De resourcegroep van de __bestaande__ virtueel netwerk. De Azure Machine Learning-compute-omgeving is gemaakt in dit virtuele netwerk.
* **vnet_name**: De naam van het virtuele netwerk. Het virtuele netwerk moet zich in dezelfde regio als uw Azure Machine Learning-werkruimte.
* **subnet_name**: De naam van het subnet binnen het virtuele netwerk. De Azure Machine Learning compute-omgevingsresources worden toegewezen aan IP-adressen uit dit subnetbereik.

> [!TIP]
> Wanneer u een permanente resource van Azure Machine Learning compute-omgeving maakt, kunt u eigenschappen, zoals het aantal bijwerken **min_nodes** of **max_nodes**. Aanroepen voor het bijwerken van een eigenschap, de `update()` functie voor de eigenschap.

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that the cluster doesn't already exist 
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                           vm_priority='lowpriority',
                                                           min_nodes=2,
                                                           max_nodes=4,
                                                           idle_seconds_before_scaledown='300',
                                                           vnet_resourcegroup_name='<my-resource-group>',
                                                           vnet_name='<my-vnet-name>',
                                                           subnet_name='<my-subnet-name>')
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

cpu_cluster.wait_for_completion(show_output=True)

```


## <a id="vm"></a>Externe virtuele machine

Azure Machine Learning ondersteunt ook uw eigen compute-resource halen en deze te koppelen aan uw werkruimte. Een dergelijke resourcetype is een willekeurige externe virtuele machine, zolang deze toegankelijk is vanaf de Azure Machine Learning-service. De bron kan bestaan uit een Azure-VM, een externe server in uw organisatie, of on-premises. Specifiek, kunt gezien het IP-adres en referenties (gebruikersnaam en wachtwoord of SSH-sleutel), u elke VM die toegankelijk is voor extern worden uitgevoerd.
U kunt een systeem gebouwd conda-omgeving, een bestaande Python-omgeving of een Docker-container. Wanneer u hebt uitgevoerd met behulp van een Docker-container, moet u beschikken over Docker-Engine die wordt uitgevoerd op de virtuele machine. De externe VM-functionaliteit is vooral nuttig wanneer u wilt dat een cloud-gebaseerde ontwikkelings- en experimenten omgeving die is flexibeler dan uw lokale computer.

> [!TIP]
> Gebruik de DSVM als de Azure-VM van keuze voor dit scenario. Deze virtuele machine is een vooraf geconfigureerde data science en AI-ontwikkelomgeving in de virtuele machine van Azure biedt een gecureerde ruime keuze aan hulpprogramma's en frameworks voor volledige-levenscyclus van machine learning-ontwikkeling. Zie voor meer informatie over het gebruik van de DSVM met Azure Machine Learning [een ontwikkelomgeving configureren](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-environment#dsvm).

> [!WARNING]
> Azure Machine Learning biedt alleen ondersteuning voor virtuele machines met Ubuntu. Wanneer u een virtuele machine maken of kies een bestaande virtuele machine, moet u een virtuele machine die gebruikmaakt van Ubuntu.

De volgende stappen uit de SDK gebruiken om een DSVM configureren als een doel voor training:

1. Als u wilt koppelen van een bestaande virtuele machine als een compute-doel, moet u de volledig gekwalificeerde domeinnaam (FQDN), de gebruikersnaam en het wachtwoord opgeven voor de virtuele machine. Vervang in het voorbeeld \<FQDN-naam > met de openbare FQDN-naam van de virtuele machine of het openbare IP-adres. Vervang \<username > en \<wachtwoord > met de SSH-gebruikersnaam en het wachtwoord voor de virtuele machine.

    ```python
    from azureml.core.compute import RemoteCompute, ComputeTarget
    
    # Create the compute config
    attach_config = RemoteCompute.attach_configuration(address = "ipaddress",
                                                       ssh_port=22,
                                                       username='<username>',
                                                       password="<password>")

    # If you use SSH instead of a password, use this code:
    #                                                  ssh_port=22,
    #                                                  username='<username>',
    #                                                  password=None,
    #                                                  private_key_file="path-to-file",
    #                                                  private_key_passphrase="passphrase")

    # Attach the compute target
    compute = ComputeTarget.attach(ws, "attach-dsvm", attach_config)

    compute.wait_for_completion(show_output=True)
    ```

1. Maak een configuratie voor de DSVM compute-doel. Docker en conda worden gebruikt voor het maken en configureren van de omgeving instrueren op de DSVM.

    ```python
    from azureml.core.runconfig import RunConfiguration
    from azureml.core.conda_dependencies import CondaDependencies

    # Load into memory the cpu-dsvm.runconfig file created in the previous attach operation
    run_config = RunConfiguration(framework = "python")

    # Set the compute target to the Linux DSVM
    run_config.target = compute_target_name

    # Use Docker in the remote VM
    run_config.environment.docker.enabled = True

    # Use the CPU base image
    # To use GPU in DSVM, you must also use the GPU base Docker image "azureml.core.runconfig.DEFAULT_GPU_IMAGE"
    run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE
    print('Base Docker image is:', run_config.environment.docker.base_image)

    # Ask the system to provision a new conda environment based on the conda_dependencies.yml file
    run_config.environment.python.user_managed_dependencies = False

    # Prepare the Docker and conda environment automatically when they're used for the first time
    run_config.prepare_environment = True

    # Specify the CondaDependencies object
    run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])

    ```

## <a id="databricks"></a>Azure Databricks

Azure Databricks is een omgeving op basis van Apache Spark in de Azure-cloud. De omgeving kan worden gebruikt als een compute-doel bij het trainen van modellen met een Azure Machine Learning-pijplijn.

> [!IMPORTANT]
> Een Azure Databricks compute-doel kan alleen worden gebruikt in een Machine Learning-pijplijn.
>
> U moet een Azure Databricks-werkruimte maken voordat u deze gebruiken om uw model te trainen. Zie voor het maken van deze resource [een Spark-taak uitvoeren op Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal).

Als u wilt koppelen Azure Databricks als een compute-doel, moet u de Azure Machine Learning-SDK gebruiken en geef de volgende informatie:

* __De naam van COMPUTE__: De naam toewijzen aan deze compute-resource.
* __De naam van de Databricks-werkruimte__: De naam van de Azure Databricks-werkruimte.
* __Toegangstoken__: Het toegangstoken dat wordt gebruikt om te verifiëren met Azure Databricks. Zie voor het genereren van een toegangstoken [verificatie](https://docs.azuredatabricks.net/api/latest/authentication.html).

De volgende code ziet u hoe u Azure Databricks als een compute-doel toevoegen:

```python
databricks_compute_name = os.environ.get("AML_DATABRICKS_COMPUTE_NAME", "<databricks_compute_name>")
databricks_workspace_name = os.environ.get("AML_DATABRICKS_WORKSPACE", "<databricks_workspace_name>")
databricks_resource_group = os.environ.get("AML_DATABRICKS_RESOURCE_GROUP", "<databricks_resource_group>")
databricks_access_token = os.environ.get("AML_DATABRICKS_ACCESS_TOKEN", "<databricks_access_token>")

try:
    databricks_compute = ComputeTarget(workspace=ws, name=databricks_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('databricks_compute_name {}'.format(databricks_compute_name))
    print('databricks_workspace_name {}'.format(databricks_workspace_name))
    print('databricks_access_token {}'.format(databricks_access_token))

    # Create the attach config
    attach_config = DatabricksCompute.attach_configuration(resource_group = databricks_resource_group,
                                                           workspace_name = databricks_workspace_name,
                                                           access_token = databricks_access_token)
    databricks_compute = ComputeTarget.attach(
             ws,
             databricks_compute_name,
             attach_config
         )
    
    databricks_compute.wait_for_completion(True)
```

## <a id="adla"></a>Azure Data Lake Analytics

Azure Data Lake Analytics is een analyse van big data-platform in de Azure-cloud. Het platform kan worden gebruikt als een compute-doel bij het trainen van modellen met een Azure Machine Learning-pijplijn.

> [!IMPORTANT]
> Een Azure Data Lake Analytics compute-doel kan alleen worden gebruikt in een Machine Learning-pijplijn.
>
> U moet een Azure Data Lake Analytics-account maken voordat u deze gebruiken om uw model te trainen. Zie voor het maken van deze resource [aan de slag met Azure Data Lake Analytics](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-portal).

Als u wilt koppelen Data Lake Analytics als een compute-doel, moet u de Azure Machine Learning-SDK gebruiken en geef de volgende informatie:

* __De naam van COMPUTE__: De naam toewijzen aan deze compute-resource.
* __Resourcegroep__: De resourcegroep met de Data Lake Analytics-account.
* __Accountnaam__: De naam van de Data Lake Analytics-account.

De volgende code ziet u hoe u Data Lake Analytics als een compute-doel toevoegen:

```python
adla_compute_name = os.environ.get("AML_ADLA_COMPUTE_NAME", "<adla_compute_name>")
adla_resource_group = os.environ.get("AML_ADLA_RESOURCE_GROUP", "<adla_resource_group>")
adla_account_name = os.environ.get("AML_ADLA_ACCOUNT_NAME", "<adla_account_name>")

try:
    adla_compute = ComputeTarget(workspace=ws, name=adla_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('adla_compute_name {}'.format(adla_compute_name))
    print('adla_resource_id {}'.format(adla_resource_group))
    print('adla_account_name {}'.format(adla_account_name))
    
    # Create the attach config
    attach_config = AdlaCompute.attach_configuration(resource_group = adla_resource_group,
                                                     account_name = adla_account_name)
    # Attach the ADLA
    adla_compute = ComputeTarget.attach(
             ws,
             adla_compute_name,
             attach_config
         )
    
    adla_compute.wait_for_completion(True)
```

> [!TIP]
> Azure Machine Learning-pijplijnen werkt alleen met gegevens die zijn opgeslagen in het standaardarchief van gegevens van het Data Lake Analytics-account. Als de benodigde gegevens in een niet-standaard-archief is, kunt u een [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) bewerking voor het kopiëren van de gegevens voordat u het model te trainen.

## <a id="hdinsight"></a>Azure HDInsight 

Azure HDInsight is een populair platform voor big data-analyses. Het platform biedt een Apache Spark, die kunnen worden gebruikt om uw model te trainen.

> [!IMPORTANT]
> Voordat u deze gebruiken om uw model te trainen, moet u het HDInsight-cluster maken. Zie voor informatie over het maken van een Spark op HDInsight-cluster [maken van een Spark-Cluster in HDInsight](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql).
>
> Wanneer u het cluster maakt, moet u een SSH-gebruikersnaam en wachtwoord opgeven. Noteer deze waarden als dat nodig HDInsight gebruiken als een compute-doel.
>
> Nadat het cluster is gemaakt, heeft de FQDN-naam \<clustername >. azurehdinsight.net, waar \<clustername > is de naam die u hebt opgegeven voor het cluster. U moet de FQDN-adres (of het openbare IP-adres van het cluster) voor het gebruik van het cluster als een compute-doel.

Als u wilt configureren HDInsight als een compute-doel, moet u de FQDN-naam, gebruikersnaam en wachtwoord opgeven voor het HDInsight-cluster. Het volgende voorbeeld wordt de SDK te koppelen van een cluster aan uw werkruimte. Vervang in het voorbeeld \<FQDN-naam > met de openbare FQDN van het cluster of het openbare IP-adres. Vervang \<username > en \<wachtwoord > met de SSH-gebruikersnaam en het wachtwoord voor het cluster.

> [!NOTE]
> De FQDN-naam voor uw cluster vinden, gaat u naar de Azure-portal en selecteer uw HDInsight-cluster. Onder __overzicht__, ziet u de FQDN-naam in de __URL__ vermelding. Als u de FQDN-naam, verwijdert u de https:\// voorvoegsel vanaf het begin van de vermelding.

![Ophalen van de FQDN-naam voor een HDInsight-cluster in Azure portal](./media/how-to-set-up-training-targets/hdinsight-overview.png)

```python
from azureml.core.compute import HDInsightCompute, ComputeTarget

try:
    # Attach an HDInsight cluster as a compute target
    attach_config = HDInsightCompute.attach_configuration(address = "fqdn-or-ipaddress",
                                                          ssh_port = 22,
                                                          username = "username",
                                                          password = None, #if using ssh key
                                                          private_key_file = "path-to-key-file",
                                                          private_key_phrase = "key-phrase")
    compute = ComputeTarget.attach(ws, "myhdi", attach_config)
except UserErrorException as e:
    print("Caught = {}".format(e.message))
    print("Compute config already attached.")

# Configure the HDInsight run
# Load the runconfig object from the myhdi.runconfig file generated in the previous attach operation
run_config = RunConfiguration.load(project_object = project, run_config_name = 'myhdi')

# Ask the system to prepare the conda environment automatically when it's used for the first time
run_config.auto_prepare_environment = True
```

## <a name="submit-training-runs"></a>Trainingsuitvoeringen verzenden

Er zijn twee manieren om in te dienen een training uitvoeren:

* Indienen van een training uitvoeren met behulp van een `ScriptRunConfig` object.
* Indienen van een training uitvoeren met behulp van een `Pipeline` object.

> [!IMPORTANT]
> De Azure Databricks en Azure Lake Analytics compute-doelen kunnen alleen worden gebruikt in een pijplijn.
>
> De lokale compute-doel kan niet worden gebruikt in een pijplijn.

### <a name="scriptrunconfig-object"></a>ScriptRunConfig object

Het patroon code om in te dienen een training uitvoeren met de `ScriptRunConfig` object is hetzelfde voor alle typen compute-doelen:

1. Maak een `ScriptRunConfig` object met behulp van de configuratie uitvoeren voor de compute-doel.
1. De uitvoering verzenden.
1. Wacht totdat de uitvoering om te voltooien.

Het volgende voorbeeld wordt de configuratie voor het systeem beheerd lokale compute-doel eerder hebt gemaakt:

```python
src = ScriptRunConfig(source_directory = script_folder, script = 'train.py', run_config = run_config_system_managed)
run = exp.submit(src)
run.wait_for_completion(show_output = True)
```


### <a name="pipeline-object"></a>Pipeline-object

Het patroon code om in te dienen een training uitvoeren met een `Pipeline` object is hetzelfde voor alle typen compute-doelen:

1. Toevoegen van een stap voor de `Pipeline` -object voor de compute-resource.
1. Een uitvoering indienen met behulp van de pijplijn.
1. Wacht totdat de uitvoering om te voltooien.

Het volgende voorbeeld wordt de Azure Databricks-compute-doel eerder hebt gemaakt:

```python
dbStep = DatabricksStep(
    name="databricksmodule",
    inputs=[step_1_input],
    outputs=[step_1_output],
    num_workers=1,
    notebook_path=notebook_path,
    notebook_params={'myparam': 'testparam'},
    run_name='demo run name',
    databricks_compute=databricks_compute,
    allow_reuse=False
)

# List of steps to run
steps = [dbStep]
pipeline = Pipeline(workspace=ws, steps=steps)
pipeline_run = Experiment(ws, 'Demo_experiment').submit(pipeline)
pipeline_run.wait_for_completion()
```

Zie voor meer informatie over machine learning-pijplijnen [pijplijnen en Azure Machine Learning](concept-ml-pipelines.md).

Bijvoorbeeld Jupyter-notitieblokken die laten zien hoe u een model te trainen met behulp van een pijplijn, Zie [ https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline ](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline).

## <a name="access-computes-in-the-azure-portal"></a>Toegang berekent in Azure portal

U kunt toegang tot de compute-doelen die gekoppeld aan uw werkruimte in de Azure-portal zijn. 

### <a name="view-compute-targets"></a>Compute-doelen weergeven

Als de compute-doelen voor uw werkruimte weergeven, gebruikt u de volgende stappen uit:

1. Navigeer naar de [Azure-portal](https://portal.azure.com) en opent u uw werkruimte.
1. Onder __toepassingen__, selecteer __Compute__.

    ![Compute-doelen weergeven](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace.png)

### <a name="create-a-compute-target"></a>Een compute-doel maken

Volg de vorige stappen om de lijst met compute-doelen weer te geven. Gebruik vervolgens deze stappen om een compute-doel te maken:

1. Selecteer het plusteken (+) om toe te voegen een compute-doel.

    ![Een compute-doel toevoegen](./media/how-to-set-up-training-targets/add-compute-target.png)

1. Voer een naam voor de compute-doel.
1. Selecteer **Machine Learning-Computing** als het type compute moet worden gebruikt voor __Training__.

    > [!IMPORTANT]
    > U kunt alleen een Azure Machine Learning-rekenomgeving maken als de beheerde-rekenresource voor de training.

1. Vul het formulier. Geef waarden op voor de vereiste eigenschappen, met name **VM-reeks**, en de **maximum aantal knooppunten** gebruiken instellen de rekenkracht. 
1. Selecteer __Maken__.
1. De status van de maakbewerking weergeven door de compute-doel selecteren in de lijst:

    ![Selecteer een compute-doel om weer te geven van de status van de bewerking maken](./media/how-to-set-up-training-targets/View_list.png)

1. Vervolgens ziet u de details voor de compute-doel:

    ![Bekijk de details van de doel-computer](./media/how-to-set-up-training-targets/compute-target-details.png)

U kunt nu een uitvoering op basis van de computerdoelen verzenden, zoals eerder beschreven.


### <a name="reuse-existing-compute-targets"></a>Hergebruik bestaande compute-doelen

Volg de stappen hierboven om de lijst met compute-doelen weer te geven. Gebruik vervolgens deze stappen om een compute-doel opnieuw te gebruiken:

1. Selecteer het plusteken (+) om toe te voegen een compute-doel.
1. Voer een naam voor de compute-doel.
1. Selecteer het type compute koppelen voor __Training__:

    > [!IMPORTANT]
    > Niet alle compute-typen kunnen worden gekoppeld via de Azure-portal.
    > De compute-typen die op dit moment kunnen worden gekoppeld voor training onder meer:
    >
    > * Een externe virtuele machine
    > * Azure Databricks
    > * Azure Data Lake Analytics
    > * Azure HDInsight

1. Vul het formulier in en geef waarden op voor de vereiste eigenschappen.

    > [!NOTE]
    > Microsoft raadt u SSH-sleutels veiliger dan wachtwoorden zijn te gebruiken. Wachtwoorden zijn kwetsbaar voor beveiligingsaanvallen. SSH-sleutels zijn, is afhankelijk van cryptografische handtekeningen. Zie de volgende documenten voor informatie over het maken van SSH-sleutels voor gebruik met Azure Virtual Machines:
    >
    > * [Maken en gebruiken van SSH-sleutels in Linux of macOS](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Maken en gebruiken van SSH-sleutels op Windows](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

1. Selecteer __koppelen__.
1. Bekijk de status van de koppelingsbewerking door het selecteren van de compute-doel in de lijst.

Nu kunt u indienen van een uitvoering op basis van deze compute-doelen, zoals eerder beschreven.

## <a name="notebook-examples"></a>Laptop-voorbeelden

Zie voor voorbeelden van de notebooks in de volgende locaties:

* [procedure-naar-gebruik-azureml/training](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training)
* [zelfstudies/img-classificatie-deel 1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Volgende stappen

* [Naslaginformatie over Azure Machine Learning-SDK](https://aka.ms/aml-sdk)
* [Zelfstudie: een model trainen](tutorial-train-models-with-aml.md)
* [Waar u modellen implementeren](how-to-deploy-and-where.md)
* [Machine learning-pijplijnen met Azure Machine Learning-service bouwen](concept-ml-pipelines.md)

---
title: Compute-doelen voor modeltraining met Azure Machine Learning-service instellen | Microsoft Docs
description: Informatie over het selecteren en configureren van de training-omgevingen (compute-doelen) gebruikt voor het trainen van uw machine learning-modellen. De service Azure Machine Learning kunt u eenvoudig over training van omgeving. Start lokaal training en als u nodig hebt om uit te schalen, gaat u naar een cloud-gebaseerde compute-doel.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.reviewer: larryfr
manager: cgronlun
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: 65134d78fb011ae707c0629bef1ebb1a66bd8ec2
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48018499"
---
# <a name="select-and-use-a-compute-target-to-train-your-model"></a>Selecteer en gebruik een compute-doel aan uw model te trainen

Met de Azure Machine Learning-service, kunt u uw model in verschillende omgevingen te trainen. Deze omgevingen, met de naam __compute-doelen__, lokaal of in de cloud kan zijn. In dit document leert u over de ondersteunde compute-doelen en het gebruik ervan.

Een compute-doel is de resource die wordt uitgevoerd de trainingsscript of hosts uw model wanneer deze geïmplementeerd als een webservice. Ze kunnen worden gemaakt en beheerd met de Azure Machine Learning-SDK of de CLI. Als u de compute-doelen die zijn gemaakt door een ander proces (bijvoorbeeld, de Azure portal of Azure CLI) hebt, kunt u ze kunt gebruiken door ze te koppelen aan uw werkruimte van Azure Machine Learning-service.

U kunt beginnen met lokaal wordt uitgevoerd op uw computer en klik vervolgens in andere omgevingen zoals externe Data Science virtual machines met GPU of Azure Batch AI opschalen en uitbreiden. 

## <a name="supported-compute-targets"></a>Ondersteunde compute-doelen

Azure Machine Learning ondersteunt de volgende compute-doelen:

|COMPUTE-doel| GPU-versnelling | Geautomatiseerde hyperparameter afstemmen | Geautomatiseerde modelselectie | Kan worden gebruikt in pijplijnen|
|----|:----:|:----:|:----:|:----:|
|[Lokale computer](#local)| Misschien | &nbsp; | ✓ | &nbsp; |
|[Data Science Virtual Machine (DSVM)](#dsvm) | ✓ | ✓ | ✓ | ✓ |
|[Azure Batch AI](#batch)| ✓ | ✓ | ✓ | ✓ | ✓ |
|[Azure HDInsight](#hdinsight)| &nbsp; | &nbsp; | &nbsp; | ✓ |

__[Azure Container Instances (ACI)](#aci)__  kan ook worden gebruikt voor het trainen van modellen. Het is een serverloze cloud-aanbieding waarmee goedkope en eenvoudig te maken en werken met. ACI biedt geen ondersteuning voor GPU-versnelling, geautomatiseerde hyper parameter afstemmen, of geautomatiseerde modelselectie. Het kan niet ook worden gebruikt in een pijplijn.

De belangrijkste verschillen tussen de compute-doelen zijn:
* __GPU-versnelling__: GPU's zijn beschikbaar met de virtuele Machine voor Datatechnologie en de Azure Batch AI. U mogelijk de toegang tot een GPU die op uw lokale computer, afhankelijk van de hardware, stuurprogramma's en frameworks die zijn geïnstalleerd.
* __Automatische afstemming van hyperparameter__: Azure Machine Learning geautomatiseerde hyperparameter optimalisatie helpt u bij het vinden van de beste hyperparameters voor uw model.
* __Geautomatiseerde modelselectie__: Azure Machine Learning kan op intelligente wijze kunt het beste algoritme en hyperparameter selectie bij het bouwen van een model. Geautomatiseerde modelselectie helpt u bij naar een model met hoge kwaliteit sneller dan handmatig probeert verschillende combinaties worden geconvergeerd. Zie voor meer informatie de [zelfstudie: automatisch een classificatie model trainen met Azure geautomatiseerde Machine Learning](tutorial-auto-train-models.md) document.
* __Pijplijnen__: Azure Machine Learning kunt u verschillende taken, zoals training en de implementatie in een pijplijn te combineren. Pijplijnen kunnen worden uitgevoerd parallel of op volgorde en bieden een betrouwbare automation-mechanisme. Zie voor meer informatie de [machine learning-pijplijnen met Azure Machine Learning-service bouwen](concept-ml-pipelines.md) document.

U kunt de SDK van Azure Machine Learning, Azure CLI of Azure-portal gebruiken om te maken van de compute-doelen. U kunt ook bestaande compute-doelen door toe te voegen (koppelen) deze naar uw werkruimte.

> [!IMPORTANT]
> U kunt een bestaand exemplaar van de Azure-Containers niet koppelen aan uw werkruimte. In plaats daarvan moet u een nieuw exemplaar maken.
>
> U kunt een Azure HDInsight-cluster in een werkruimte niet maken. In plaats daarvan moet u een bestaand cluster koppelen.

## <a name="workflow"></a>Werkstroom

De werkstroom voor het ontwikkelen en implementeren van een model met Azure Machine Learning met de volgende stappen:

1. Machine learning-scripts in Python training te ontwikkelen.
1. Maken en configureren of koppelen van een bestaande compute-doel.
1. Verzenden van de trainingsscripts naar de compute-doel.
1. Inspecteer de resultaten om het beste model.
1. Registreer het model in het register model.
1. Het model implementeren.

> [!IMPORTANT]
> Uw trainingsscript is niet gebonden aan een specifieke compute-doel. U kunt in eerste instantie trainen op uw lokale computer en vervolgens overschakelen van compute-doelen zonder dat u hoeft te herschrijven van het trainingsscript.

Overstappen van een compute-doel naar een andere omvat het maken van een [uitvoerconfiguratie](concept-azure-machine-learning-architecture.md#run-configuration). De configuratie van de uitvoering wordt gedefinieerd hoe u het script uitvoert op de compute-doel.

## <a name="training-scripts"></a>Trainingsscripts

Wanneer u een uitvoering training start, wordt de hele map waarin uw trainingsscripts verzonden. Een momentopname is gemaakt en verzonden naar de compute-doel. Zie voor meer informatie, [momentopnamen](concept-azure-machine-learning-architecture.md#snapshot).

## <a id="local"></a>Lokale computer

Bij het trainen van lokaal, kunt u de SDK gebruiken om in te dienen de trainingsbewerking. U kunt trainen met behulp van een gebruiker of systeem beheerde omgeving.

### <a name="user-managed-environment"></a>Gebruiker-beheerde omgeving

In een omgeving gebruiker wordt beheerd bent u verantwoordelijk om ervoor te zorgen dat de vereiste pakketten beschikbaar in de Python-omgeving die u uitvoeren van het script zijn wilt. Het volgende codefragment wordt een voorbeeld van de configuratie van de training voor een gebruiker beheerde omgeving:

```python
from azureml.core.runconfig import RunConfiguration

# Editing a run configuration property on-fly.
run_config_user_managed = RunConfiguration()

run_config_user_managed.environment.python.user_managed_dependencies = True

# You can choose a specific Python environment by pointing to a Python path 
#run_config.environment.python.interpreter_path = '/home/ninghai/miniconda3/envs/sdk2/bin/python'
```

Zie voor een Jupyter-Notebook die laat training in een door de gebruiker beheerde omgeving zien, [ https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb ](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb).
  
### <a name="system-managed-environment"></a>Systeem-beheerde omgeving

Door systeem beheerde omgevingen, is afhankelijk van conda om de afhankelijkheden te beheren. Conda maakt u een bestand met de naam `conda_dependencies.yml` die een lijst van de afhankelijkheden bevat. Vervolgens kunt u het systeem een nieuwe conda-omgeving bouwen en uitvoeren van scripts in deze vragen. Door systeem beheerde omgevingen kunnen zich opnieuw gebruikte later, zolang de `conda_dependencies.yml` bestanden ongewijzigd blijft. 

De eerste installatie van een nieuwe omgeving kan enige tijd duren om uit te voeren, afhankelijk van de grootte van de vereiste afhankelijkheden. Het volgende codefragment ziet u het maken van een systeem-beheerde omgeving die afhankelijk is van scikit-informatie:

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

run_config_system_managed = RunConfiguration()

run_config_system_managed.environment.python.user_managed_dependencies = False
run_config_system_managed.prepare_environment = True

# Specify conda dependencies with scikit-learn

run_config_system_managed.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
```

Zie voor een Jupyter-Notebook die laat training in een systeem-beheerde omgeving zien, [ https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb ](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb).

## <a id="dsvm"></a>Virtuele Machine voor Datatechnologie

Uw lokale computer mogelijk niet de compute of GPU-resources die zijn vereist voor het model te trainen. In dit geval kunt u omhoog schalen of scale-out de trainingsproces door toe te voegen extra compute-doelen, zoals een Data Science Virtual Machines (DSVM).

> [!WARNING]
> Azure Machine Learning biedt alleen ondersteuning voor virtuele machines met Ubuntu. Wanneer u een virtuele machine of Selecteer een bestaande, moet u een selecteren die gebruikmaakt van Ubuntu.

De volgende stappen uit de SDK gebruiken om een Data Science Virtual Machine (DSVM) configureren als een doel voor training:

1. Maken of koppelen van een virtuele Machine
    
    * Voor het maken van een nieuwe DSVM, controleert u eerst om te zien als u een DSVM met dezelfde naam hebben als dit niet een nieuwe virtuele machine maken:
    
        ```python
        from azureml.core.compute import DsvmCompute
        from azureml.core.compute_target import ComputeTargetException

        compute_target_name = 'mydsvm'

        try:
            dsvm_compute = DsvmCompute(workspace = ws, name = compute_target_name)
            print('found existing:', dsvm_compute.name)
        except ComputeTargetException:
            print('creating new.')
            dsvm_config = DsvmCompute.provisioning_configuration(vm_size = "Standard_D2_v2")
            dsvm_compute = DsvmCompute.create(ws, name = compute_target_name, provisioning_configuration = dsvm_config)
            dsvm_compute.wait_for_completion(show_output = True)
        ```
    * Als u wilt koppelen van een bestaande virtuele machine als een compute-doel, moet u de volledig gekwalificeerde domeinnaam, de aanmeldingsnaam en het wachtwoord opgeven voor de virtuele machine.  Vervang in het voorbeeld ```<fqdn>``` met openbare volledig gekwalificeerde domeinnaam van de virtuele machine of het openbare IP-adres. Vervang ```<username>``` en ```<password>``` met de SSH-gebruiker en het wachtwoord voor de virtuele machine:

        ```python
        from azureml.core.compute import RemoteCompute

        dsvm_compute = RemoteCompute.attach(ws,
                                        name="attach-dsvm",
                                        username='<username>',
                                        address="<fqdn>",
                                        ssh_port=22,
                                        password="<password>")

        dsvm_compute.wait_for_completion(show_output=True)
    
   It takes around 5 minutes to create the DSVM instance.

1. Create a configuration for the DSVM compute target. Docker and conda are used to create and configure the training environment on DSVM:

    ```python
    from azureml.core.runconfig import RunConfiguration
    from azureml.core.conda_dependencies import CondaDependencies

    # Load the "cpu-dsvm.runconfig" file (created by the above attach operation) in memory
    run_config = RunConfiguration(framework = "python")

    # Set compute target to the Linux DSVM
    run_config.target = compute_target_name

    # Use Docker in the remote VM
    run_config.environment.docker.enabled = True

    # Use CPU base image from DockerHub
    run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE
    print('Base Docker image is:', run_config.environment.docker.base_image)

    # Ask system to provision a new one based on the conda_dependencies.yml file
    run_config.environment.python.user_managed_dependencies = False

    # Prepare the Docker and conda environment automatically when used the first time.
    run_config.prepare_environment = True

    # specify CondaDependencies obj
    run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])

    ```

1. Als u wilt de compute-resources verwijderen wanneer u klaar bent, gebruik de volgende code:

    ```python
    dsvm_compute.delete()
    ```

Zie voor een Jupyter-Notebook die laat training op een virtuele Machine voor Datatechnologie zien, [ https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/04.train-on-remote-vm/04.train-on-remote-vm.ipynb ](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/04.train-on-remote-vm/04.train-on-remote-vm.ipynb).

## <a id="batch"></a>Azure Batch AI

Als het duurt lang uw model te trainen, kunt u Azure Batch AI gebruiken voor het distribueren van de training in een cluster van rekenbronnen in de cloud. Batch AI kan ook worden geconfigureerd om in te schakelen van een GPU-resource.

Het volgende voorbeeld wordt gezocht naar een bestaand Batch AI-cluster met de naam. Als er geen wordt gevonden, wordt dit gemaakt:

```python
from azureml.core.compute import BatchAiCompute
from azureml.core.compute import ComputeTarget

# choose a name for your cluster
batchai_cluster_name = ws.name + "cpu"

found = False
# see if this compute target already exists in the workspace
for ct in ws.compute_targets():
    print(ct.name, ct.type)
    if (ct.name == batchai_cluster_name and ct.type == 'BatchAI'):
        found = True
        print('found compute target. just use it.')
        compute_target = ct
        break
        
if not found:
    print('creating a new compute target...')
    provisioning_config = BatchAiCompute.provisioning_configuration(vm_size = "STANDARD_D2_V2", # for GPU, use "STANDARD_NC6"
                                                                #vm_priority = 'lowpriority', # optional
                                                                autoscale_enabled = True,
                                                                cluster_min_nodes = 1, 
                                                                cluster_max_nodes = 4)

    # create the cluster
    compute_target = ComputeTarget.create(ws,batchai_cluster_name, provisioning_config)
    
    # can poll for a minimum number of nodes and for a specific timeout. 
    # if no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
     # For a more detailed view of current BatchAI cluster status, use the 'status' property    
    print(compute_target.status.serialize())
```

Als u wilt koppelen van een bestaande Batch AI-cluster als een compute-doel, moet u de Azure-resource-ID. Voor de resource-ID van de Azure-portal, gebruikt u de volgende stappen uit:
1. Zoeken naar `Batch AI` service onder **alle Services**
1. Klik op de naam van de werkruimte waarin het cluster behoort
1. Selecteer het cluster
1. Klik op **eigenschappen**
1. Kopieer de **ID**

Het volgende voorbeeld wordt de SDK te koppelen van een cluster aan uw werkruimte. Vervang in het voorbeeld `<name>` met een willekeurige naam voor de rekenkracht. De naam hoeft niet overeenkomt met de naam van het cluster. Vervang `<resource-id>` met de Azure-resource ID hierboven:

```python
from azureml.core.compute import BatchAiCompute
BatchAiCompute.attach(workspace=ws,
                      name=<name>,
                      resource_id=<resource-id>)
```

U kunt ook controleren de Batch AI-cluster en de status van de volgende Azure CLI-opdrachten:

- Clusterstatus controleren. U kunt zien hoeveel knooppunten worden uitgevoerd met behulp van `az batchai cluster list`.
- Controleer de status van taak. U kunt zien hoeveel taken worden uitgevoerd met behulp van `az batchai job list`.

Het duurt ongeveer vijf minuten om de Batch AI-cluster te maken.

Zie voor een Jupyter-Notebook die laat training in een Batch AI-cluster zien, [ https://github.com/Azure/MachineLearningNotebooks/blob/master/training/03.train-hyperparameter-tune-deploy-with-tensorflow/03.train-hyperparameter-tune-deploy-with-tensorflow.ipynb ](https://github.com/Azure/MachineLearningNotebooks/blob/master/training/03.train-hyperparameter-tune-deploy-with-tensorflow/03.train-hyperparameter-tune-deploy-with-tensorflow.ipynb).

## <a name='aci'></a>Azure Container Instance (ACI)

Azure Container Instances zijn geïsoleerd containers die sneller opstarten en hoeven niet de gebruiker voor het beheren van virtuele Machines. De Azure Machine Learning-service maakt gebruik van Linux-containers, die beschikbaar in de VS West, VS-Oost, Europa West, northeurope, westus2 en southeastasia regio's zijn. Zie voor meer informatie, [beschikbaarheid in regio](https://docs.microsoft.com/azure/container-instances/container-instances-quotas#region-availability). 

Het volgende voorbeeld laat zien hoe het gebruik van de SDK een ACI-compute-doel maken en gebruiken voor het trainen van een model: 

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

# create a new runconfig object
run_config = RunConfiguration()

# signal that you want to use ACI to run script.
run_config.target = "containerinstance"

# ACI container group is only supported in certain regions, which can be different than the region the Workspace is in.
run_config.container_instance.region = 'eastus'

# set the ACI CPU and Memory 
run_config.container_instance.cpu_cores = 1
run_config.container_instance.memory_gb = 2

# enable Docker 
run_config.environment.docker.enabled = True

# set Docker base image to the default CPU-based image
run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE
#run_config.environment.docker.base_image = 'microsoft/mmlspark:plus-0.9.9'

# use conda_dependencies.yml to create a conda environment in the Docker image
run_config.environment.python.user_managed_dependencies = False

# auto-prepare the Docker image when used for the first time (if it is not already prepared)
run_config.auto_prepare_environment = True

# specify CondaDependencies obj
run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
```

Duurt een paar seconden met een paar minuten een ACI-compute-doel maken.

Zie voor een Jupyter-Notebook die laat training voor Azure Container Instances zien, [ https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/03.train-on-aci/03.train-on-aci.ipynb ](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/03.train-on-aci/03.train-on-aci.ipynb).

## <a id="hdinsight"></a>Een HDInsight-cluster koppelen 

HDInsight is een populair platform voor big data-analyses. Het biedt Apache Spark, die kunnen worden gebruikt om uw model te trainen.

> [!IMPORTANT]
> Voordat u deze gebruikt met het trainen van uw model, moet u het HDInsight-cluster maken. Zie voor informatie over het maken van een Spark op HDInsight-cluster de [maken van een Spark-Cluster in HDInsight](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql) document.
>
> Wanneer het cluster te maken, moet u een SSH-gebruikersnaam en wachtwoord opgeven. Noteer deze waarden, zoals u ze nodig hebt bij het gebruik van HDInsight als een compute-doel.
>
> Zodra het cluster is gemaakt, heeft een volledig gekwalificeerde domeinnaam (FQDN) van `<clustername>.azurehdinsight.net`, waarbij `<clustername>` is de naam die u hebt opgegeven voor het cluster. U moet dit adres (of het openbare IP-adres van het cluster) als een compute-doel te gebruiken

Als u wilt configureren HDInsight als een compute-doel, moet u de volledig gekwalificeerde domeinnaam, cluster-aanmeldingsnaam en wachtwoord opgeven voor het HDInsight-cluster. Het volgende voorbeeld wordt de SDK te koppelen van een cluster aan uw werkruimte. Vervang in het voorbeeld `<fqdn>` met de openbare volledig gekwalificeerde domeinnaam van het cluster of het openbare IP-adres. Vervang `<username>` en `<password>` met de SSH-gebruiker en het wachtwoord voor het cluster:

> [!NOTE]
> De FQDN-naam voor uw cluster vinden, gaat u naar de Azure portal en selecteer uw HDInsight-cluster. Uit de __overzicht__ sectie, de FQDN-naam maakt deel uit van de __URL__ vermelding. Verwijdert de `https://` vanaf het begin van de waarde.
>
> ![Schermopname van het overzicht van HDInsight-cluster met de URL-vermelding is gemarkeerd](./media/how-to-set-up-training-targets/hdinsight-overview.png)

```python
from azureml.core.compute import HDInsightCompute

try:
    # Attaches a HDInsight cluster as a compute target.
    HDInsightCompute.attach(ws,name = "myhdi",
                            address = "<fqdn>",
                            username = "<username>",
                            password = "<password>")
except UserErrorException as e:
    print("Caught = {}".format(e.message))
    print("Compute config already attached.")

# Configure HDInsight run
# load the runconfig object from the "myhdi.runconfig" file generated by the attach operaton above.
run_config = RunConfiguration.load(project_object = project, run_config_name = 'myhdi')

# ask system to prepare the conda environment automatically when used for the first time
run_config.auto_prepare_environment = True
```

## <a name="submit-training-run"></a>Indienen van training uitvoeren
    
De code voor het indienen van een uitvoering training is hetzelfde, ongeacht de compute-doel:

* Maak een `ScriptRunConfig` object met behulp van de configuratie uitvoeren voor de compute-doel.
* De uitvoering verzenden.
* Wacht totdat de uitvoering om te voltooien.

Het volgende voorbeeld wordt de configuratie voor het systeem beheerd lokale compute-doel eerder in dit document hebt gemaakt:

```pyghon
src = ScriptRunConfig(source_directory = script_folder, script = 'train.py', run_config = run_config_system_managed)
run = exp.submit(src)
run.wait_for_completion(show_output = True)
```

Zie voor een Jupyter-Notebook die laat training met Spark in HDInsight zien, [ https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/05.train-in-spark/05.train-in-spark.ipynb ](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/05.train-in-spark/05.train-in-spark.ipynb).

## <a name="view-and-set-up-compute-using-the-azure-portal"></a>Weergeven en compute met behulp van de Azure-portal instellen

U kunt bekijken wat compute-doelen zijn gekoppeld aan uw werkruimte vanuit Azure portal. Als u aan de lijst, gebruikt u de volgende stappen uit:

1. Ga naar de [Azure-portal](https://portal.azure.com) en navigeer naar uw werkruimte.
2. Klik op de __Compute__ koppeling onder de __toepassingen__ sectie.

    ![Compute-tabblad weergave](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace.png)

### <a name="create-a-compute-target"></a>Een compute-doel maken

Volg de bovenstaande stappen voor het weergeven van de lijst met compute-doelen en vervolgens de volgende stappen gebruiken om een compute-doel:

1. Klik op de __+__ zich bij het toevoegen van een compute-doel.

    ![Compute toevoegen ](./media/how-to-set-up-training-targets/add-compute-target.png)

1. Voer een naam voor de compute-doel.
1. Selecteer het type compute koppelen voor __Training__. 
1. Selecteer __nieuw__ en vul het formulier vereist. 
1. Selecteer __Maken__
1. U kunt de status bekijken bewerking maken door het selecteren van de compute-doel in de lijst.

    ![Weergavelijst Compute](./media/how-to-set-up-training-targets/View_list.png) vervolgens ziet u de details voor deze compute.
    ![Details weergeven](./media/how-to-set-up-training-targets/vm_view.PNG)
1. U kunt nu een uitvoering op basis van deze doelen als gedetailleerde bovenstaande indienen.

### <a name="reuse-existing-compute-in-your-workspace"></a>Hergebruik bestaande compute in uw werkruimte

Volg de bovenstaande stappen voor het weergeven van de lijst met compute-doelen en vervolgens opnieuw gebruiken van compute-doel met de volgende stappen uit:

1. Klik op de **+** zich bij het toevoegen van een compute-doel.
2. Voer een naam voor de compute-doel.
3. Selecteer het type van de rekencapaciteit te koppelen voor Training. Batch AI- en virtuele Machines worden momenteel ondersteund in de portal voor training.
4. Selecteer bestaande gebruiken.
    - Bij het toevoegen van Batch AI-clusters, selecteert u de compute-doel in de vervolgkeuzelijst, selecteert u de Batch AI-werkruimte en de Batch AI-Cluster en klik vervolgens op **maken**.
    - Bij het toevoegen van een virtuele Machine, voert u het IP-adres, combinatie van gebruikersnaam en wachtwoord, persoonlijke/openbare sleutels en de poort en klik op maken.

    > [!NOTE]
    > Microsoft raadt aan dat u een SSH-sleutels gebruiken zoals ze veiliger dan wachtwoorden zijn. Wachtwoorden zijn kwetsbaar voor beveiligingsaanvallen, terwijl de SSH-sleutels zijn afhankelijk van de cryptografische handtekeningen. Zie de volgende documenten voor informatie over het maken van SSH-sleutels voor gebruik met Azure Virtual Machines:
    >
    > * [Maken en gebruiken van SSH-sleutels in Linux of macOS]( https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Maken en gebruiken van SSH-sleutels op Windows]( https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

5. U kunt de status van de Inrichtingsstatus bekijken door het selecteren van de compute-doel in de lijst van berekeningen.
6. U kunt nu een uitvoering op basis van deze doelen indienen.

## <a name="examples"></a>Voorbeelden
De volgende notebooks illustratie van concepten in dit artikel:
* `01.getting-started/02.train-on-local/02.train-on-local.ipynb`
* `01.getting-started/04.train-on-remote-vm/04.train-on-remote-vm.ipynb`
* `01.getting-started/03.train-on-aci/03.train-on-aci.ipynb`
* `01.getting-started/05.train-in-spark/05.train-in-spark.ipynb`
* `01.getting-started/07.hyperdrive-with-sklearn/07.hyperdrive-with-sklearn.ipynb`

Deze laptops ophalen: [!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Volgende stappen

* [Naslaginformatie over Azure Machine Learning-SDK](http://aka.ms/aml-sdk)
* [Zelfstudie: Een model te trainen](tutorial-train-models-with-aml.md)
* [Waar u modellen implementeren](how-to-deploy-and-where.md)
* [Machine learning-pijplijnen met Azure Machine Learning-service bouwen](concept-ml-pipelines.md)

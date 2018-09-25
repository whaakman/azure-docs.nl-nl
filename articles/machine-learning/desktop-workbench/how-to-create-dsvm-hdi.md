---
title: Over het maken van de DSVM en HDI zoals compute-doelen voor Azure ML
description: DSVM- en HDI Spark-cluster zijn compute-doelen voor Azure ML-experimenten maken.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/26/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 60abe46670353121ad308f8926a7ee178c76e74e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46951658"
---
# <a name="create-dsvm-and-hdi-spark-cluster-as-compute-targets"></a>DSVM- en HDI Spark-cluster zijn compute-doelen maken

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

U kunt eenvoudig omhoog of schaal uit uw machine learning-experiment door toe te voegen extra compute-doelen, zoals Ubuntu gebaseerde DSVM (Data Science Virtual Machine) en Apache Spark voor Azure HDInsight-cluster. In dit artikel wordt beschreven hoe u de stappen voor het maken van deze compute-doelen in Azure. Raadpleeg voor meer informatie over Azure ML compute-doelen [overzicht van Azure Machine Learning experimenten-service](experimentation-service-configuration.md).

>[!NOTE]
>U wilt controleren of u bent gemachtigd om resources zoals VM- en HDI-clusters te maken in Azure voordat u doorgaat. Beide van deze resources kunnen ook gebruikmaken van veel rekenkernen afhankelijk van uw configuratie. Zorg ervoor dat uw abonnement heeft onvoldoende capaciteit voor de virtuele CPU-kernen. U kunt altijd kunnen genieten van contact met ondersteuning van Azure om te verhogen van het maximum aantal kernen toegestaan in uw abonnement.

## <a name="create-an-ubuntu-dsvm-in-azure-portal"></a>Een Ubuntu-DSVM maken in Azure portal

U kunt een DSVM maken vanuit Azure portal. 

1. Meld u aan bij Azure-portal https://portal.azure.com
2. Klik op de **+ nieuw** koppeling en zoek naar "data science virtual machine voor Linux".
    ![Ubuntu](media/how-to-create-dsvm-hdi/ubuntu_dsvm.png)
4. Kies **Data Science Virtual Machine voor Linux (Ubuntu)** in de lijst en volg de aanwijzingen op het scherm instructies voor het maken van de DSVM.

>[!IMPORTANT]
>Zorg ervoor dat u kiest **wachtwoord** als de _verificatietype_.

![pwd gebruiken](media/how-to-create-dsvm-hdi/use_pwd.png)

## <a name="create-an-ubuntu-dsvm-using-azure-cli"></a>Maken van een Ubuntu-DSVM met behulp van azure-cli

U kunt ook een Azure resource Manager-sjabloon gebruiken voor het implementeren van een DSVM.

>[!NOTE]
>Alle volgende opdrachten wordt aangenomen dat vanuit de hoofdmap van een Azure ML-project worden uitgegeven.

Maak eerst een `mydsvm.json` bestand met behulp van uw favoriete teksteditor in de `docs` map. (Als u geen een `docs` map in de hoofdmap van het project, maakt u er een.) We dit bestand gebruiken om enkele eenvoudige parameters voor de Azure resource management-sjabloon configureren. 

Kopieer en plak de volgende JSON-fragment in het `mydsvm.json` bestand en vult u de juiste waarden:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "adminUsername": { "value" : "<admin username>"},
     "adminPassword": { "value" : "<admin password>"},
     "vmName": { "value" : "<vm name>"},
     "vmSize": { "value" : "<vm size>"}
  }
}
```

Voor de _vmSize_ veld, kunt u elke ondersteund VM-grootte die worden vermeld in de [Ubuntu DSVM Azure resource management-sjabloon](https://github.com/Azure/DataScienceVM/blob/master/Scripts/CreateDSVM/Ubuntu/multiazuredeploywithext.json). Raden wij aan u een van de volgende grootten zoals compute-doelen voor Azure ML. 


>[!TIP]
> Voor [deep learning-werkbelastingen](how-to-use-gpu.md) u kunt implementeren op GPU VM's mogelijk gemaakt.

- [Algemeen gebruik virtuele machines](../../virtual-machines/linux/sizes-general.md)
  - Standard_DS2_v2 
  - Standard_DS3_v2 
  - Standard_DS4_v2 
  - Standard_DS12_v2 
  - Standard_DS13_v2 
  - Standard_DS14_v2 
- [GPU gemaakte virtuele machines](../../virtual-machines/linux/sizes-gpu.md)
  - Standard_NC6 
  - Standard_NC12 
  - Standard_NC24 
 

Meer informatie over deze [grootten voor virtuele Linux-machines in Azure](../../virtual-machines/linux/sizes.md) en de bijbehorende [informatie over de prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

CLI-venster starten vanuit de app Azure ML Workbench door te klikken op **bestand** --> **Open Command Prompt**, of **Open PowerShell** menu-item. 

>[!NOTE]
>U kunt dit ook doen in een opdrachtregel omgeving waarin u werkt met az-cli is geïnstalleerd.

Voer in het opdrachtregelvenster in de onderstaande opdrachten:

```azurecli
# first make sure you have a valid Azure authentication token
$ az account get-access-token

# if you don't have a valid token, please log in to Azure first. 
# if you already do, you can skip this step.
$ az login

# list all subscriptions you have access to
$ az account list -o table

# make sure you set the subscription you want to use to create DSVM as the current subscription
$ az account set -s <subscription name or Id>

# it is always a good idea to create a resource group for the VM and associated resources to live in.
# you can use any Azure region, but it is best to create them in the region where your Azure ML Experimentation account is, e.g. eastus2, westcentralus or australiaeast.
# also, only certain Azure regions has GPU-equipped VMs available.
$ az group create -n <resource group name> -l <azure region>

# now let's create the DSVM based on the JSON configuration file you created earlier.
# note we assume the mydsvm.json config file is placed in the "docs" sub-folder.
$ az group deployment create -g <resource group name> --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json --parameters @docs/mydsvm.json

# find the FQDN (fully qualified domain name) of the VM just created. 
# you can also use IP address from the next command if FQDN is not set.
$ az vm show -g <resource group name> -n <vm name> --query "fqdns"

# find the IP address of the VM just created
$ az vm show -g <resource group name> -n <vm name> --query "publicIps"
```
## <a name="attach-a-dsvm-compute-target"></a>Een DSVM compute-doel toevoegen
Zodra de DSVM is gemaakt, kunt u het nu koppelen aan uw Azure ML-project.

```azurecli
# attach the DSVM compute target
# it is a good idea to use FQDN in case the IP address changes after you deallocate the VM and restart it
$ az ml computetarget attach remotedocker --name <compute target name> --address <ip address or FQDN> --username <admin username> --password <admin password> 

# prepare the Docker image on the DSVM 
$ az ml experiment prepare -c <compute target name>
```
U moet zijn nu gereed voor het uitvoeren van experimenten op deze DSVM.

## <a name="deallocate-a-dsvm-and-restart-it-later"></a>Toewijzing ongedaan maken van een DSVM en deze later opnieuw starten
Wanneer u klaar bent met de compute-taken in Azure ML, kunt u de toewijzing van de DSVM ongedaan maken. Deze actie sluit de virtuele machine, worden de rekenresources vrijgegeven, maar dan de virtuele schijven behouden blijft. U betaalt geen voor de compute-kosten wanneer de VM ongedaan is gemaakt.

Toewijzing van een virtuele machine:

```azurecli
$ az vm deallocate -g <resource group name> -n <vm name>
```

Als u wilt de virtuele machine terug tot leven brengen, gebruikt u de `az ml start` opdracht:

```azurecli
$ az vm start -g <resource group name> -n <vm name>
```

## <a name="expand-the-dsvm-os-disk"></a>De DSVM een besturingssysteemschijf uitbreiden
De Ubuntu-DSVM wordt geleverd met een OS-schijf van 50GB en een schijf van 100GB gegevens. Als u meer ruimte er beschikbaar is, docker de afbeeldingen opslaat op de gegevensschijf. Als in combinatie compute-doel voor Azure ML, kan deze schijf worden gebruikt door Docker-engine omlaag Docker-installatiekopieën te schuiven en het bouwen van conda-lagen boven op het. Mogelijk moet u de schijf uitbreiden naar een groter formaat (zoals 200 GB) om te voorkomen dat de fout 'volledige-schijf', terwijl u in het midden van de uitvoering bent. Naslaginformatie over [over het uitbreiden van virtuele harde schijven op een Linux-VM met de Azure CLI](../../virtual-machines/linux/expand-disks.md) voor meer informatie over dit eenvoudig doen vanuit azure cli. 

## <a name="create-an-apache-spark-for-azure-hdinsight-cluster-in-azure-portal"></a>Een Apache Spark voor Azure HDInsight-cluster maken in Azure portal

Als u wilt uitbreiden Spark-taken uitvoeren, moet u een Apache Spark voor Azure HDInsight-cluster maken in Azure portal.

1. Meld u aan bij Azure-portal https://portal.azure.com
2. Klik op de **+ nieuw** koppeling en zoek naar 'HDInsight'.

    ![hdi zoeken](media/how-to-create-dsvm-hdi/hdi.png)
    
3. Kies **HDInsight** in de lijst en klik vervolgens op de **maken** knop.
4. In de **basisbeginselen** Configuratiescherm **clustertype** instellingen, zorg ervoor dat u kiest **Spark** als de _clustertype_, **Linux** als de _besturingssysteem_, en **Spark 2.1.0 (HDI 3.6)** als de _Version.

    ![hdi configureren](media/how-to-create-dsvm-hdi/configure_hdi.png)

    >[!IMPORTANT]
    >U ziet in het bovenstaande scherm het cluster heeft een _de gebruikersnaam voor clusteraanmelding_ veld en een _Secure Shell (SSH)-gebruikersnaam_ veld. Dit zijn twee verschillende gebruikers-id's, zelfs als voor het gemak kunt u hetzelfde wachtwoord voor beide aanmeldingen. De _de gebruikersnaam voor clusteraanmelding_ wordt gebruikt voor aanmelding bij de management-web-UI van het HDI-cluster. De _SSH-gebruikersnaam voor clusteraanmeldgegevens_ wordt gebruikt voor aanmelding bij met het hoofdknooppunt van het cluster, en dit is wat u nodig hebt voor Azure ML verzending van Spark-taken.

5. Kies de clustergrootte en de grootte van het knooppunt u nodig hebt en voltooi de wizard voor het maken. Het kan tot 30 minuten om het cluster te inrichting voltooien duren. 

## <a name="attach-an-hdi-spark-cluster-compute-target"></a>De compute-doel van een HDI Spark-cluster koppelen

Zodra de HDI Spark-cluster is gemaakt, kunt u het nu koppelen aan uw Azure ML-project.

```azurecli
# attach the HDI compute target
$ az ml computetarget attach cluster --name <compute target name> --address <cluster name, such as myhdicluster123.azurehdinsight.net> --username <ssh username> --password <ssh password> 

# prepare the conda environment on HDI
$ az ml experiment prepare -c <compute target name>
```
U moet zijn nu gereed voor het uitvoeren van experimenten in deze Spark-cluster.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over:
- [Overzicht van Azure Machine Learning experimenten-service](experimentation-service-configuration.md)
- [Azure Machine Learning Workbench experimenten-service-configuratiebestanden](experimentation-service-configuration-reference.md)
- [Apache Spark voor Azure HDInsight-cluster](https://azure.microsoft.com/services/hdinsight/apache-spark/)
- [Virtuele Machine voor Datatechnologie](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/)

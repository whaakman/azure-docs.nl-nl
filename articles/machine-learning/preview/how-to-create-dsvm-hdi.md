---
title: DSVM en HDI zoals doelen berekenen voor Azure ML maken
description: Maak DSVM en HDI Spark-cluster zijn compute doelen voor Azure ML experimenteren.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/26/2017
ms.openlocfilehash: 2560ca144f8ce4041aa592554f9945ed546cc49b
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="create-dsvm-and-hdi-spark-cluster-as-compute-targets"></a>DSVM en HDI Spark-cluster zijn compute doelen maken

U kunt eenvoudig omhoog schalen of uw machine learning-experiment uitbreiden door extra Reken-doelen zoals Ubuntu gebaseerde DSVM (gegevens wetenschappelijke virtuele Machine) en Apache Spark voor Azure HDInsight-cluster toe te voegen. Dit artikel begeleidt u stapsgewijs door de procedure voor het maken van deze doelen in Azure berekenen. Raadpleeg voor meer informatie over Azure ML compute doelen [overzicht van Azure Machine Learning experimenteren service](experimentation-service-configuration.md).

>[!NOTE]
>Moet u ervoor zorgen er de juiste machtigingen voor het maken van resources, zoals VM en HDI-clusters in Azure voordat u doorgaat. Ook kunnen zowel deze resources veel compute kernen afhankelijk van uw configuratie gebruiken. Zorg ervoor dat uw abonnement heeft voldoende capaciteit voor de virtuele CPU-kernen. U kunt altijd op de hoogte blijft ondersteuning van Azure ophalen voor het maximum aantal kernen dat is toegestaan in uw abonnement te verhogen.

## <a name="create-an-ubuntu-dsvm-in-azure-portal"></a>Een Ubuntu DSVM maken in Azure portal

U kunt een DSVM maken vanuit Azure-portal. 

1. Meld u aan bij Azure-portal van https://portal.azure.com
2. Klik op de **+ nieuw** koppeling en zoek naar 'data wetenschappelijke virtuele machine voor Linux'.
    ![Ubuntu](media/how-to-create-dsvm-hdi/ubuntu_dsvm.png)
4. Kies **gegevens wetenschappelijke virtuele Machine voor Linux (Ubuntu)** in de lijst en volg de aanwijzingen op het scherm instructies voor het maken van de DSVM.

>[!IMPORTANT]
>Zorg ervoor dat u kiest **wachtwoord** als de _verificatietype_.

![pwd gebruiken](media/how-to-create-dsvm-hdi/use_pwd.png)

## <a name="create-an-ubuntu-dsvm-using-azure-cli"></a>Maken van een Ubuntu DSVM met azure cli

U kunt ook een Azure-resource management-sjabloon gebruiken voor het implementeren van een DSVM.

>[!NOTE]
>Alle volgende opdrachten wordt aangenomen dat vanuit de hoofdmap van een Azure ML-project worden uitgegeven.

Maak eerst een `mydsvm.json` bestand met behulp van uw favoriete teksteditor in de `docs` map. (Als u hebt een `docs` map in de hoofdmap van het project, maak een.) We dit bestand gebruiken voor het configureren van algemene parameters voor de Azure-resource management-sjabloon. 

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

Voor de _vmSize_ veld, kunt u elke suppported VM-grootte die worden vermeld in de [Ubuntu DSVM Azure resource management-sjabloon](https://github.com/Azure/DataScienceVM/blob/master/Scripts/CreateDSVM/Ubuntu/multiazuredeploywithext.json). We raden u een van de onderstaande grootten zoals doelen berekenen voor Azure ML. 


>[!TIP]
> Voor [grondige learning werkbelastingen](how-to-use-gpu.md) u kunt implementeren op GPU VMs ingeschakeld.

- [Virtuele machines voor algemene doeleinden](/virtual-machines/linux/sizes-general.md)
  - Standard_DS2_v2 
  - Standard_DS3_v2 
  - Standard_DS4_v2 
  - Standard_DS12_v2 
  - Standard_DS13_v2 
  - Standard_DS14_v2 
- [GPU VMs ingeschakeld](/virtual-machines/linux/sizes-gpu.md)
  - Standard_NC6 
  - Standard_NC12 
  - Standard_NC24 
 

Meer informatie over deze [grootten voor virtuele Linux-machines in Azure](../../virtual-machines/linux/sizes.md) en hun [prijsgegevens](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

CLI-venster starten vanuit de app Azure ML-Workbench door te klikken op **bestand** --> **opdrachtprompt openen**, of **Open PowerShell** menu-item. 

>[!NOTE]
>U kunt dit ook doen in een opdrachtregel omgeving waarin u az-cli geïnstalleerd hebben.

In het opdrachtregelvenster, voer de onderstaande opdrachten:

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
## <a name="attach-a-dsvm-compute-target"></a>Koppel een DSVM compute-doel
Zodra de DSVM is gemaakt, kunt u het nu koppelen aan uw Azure ML-project.

```azurecli
# attach the DSVM compute target
# it is a good idea to use FQDN in case the IP address changes after you deallocate the VM and restart it
$ az ml computetarget attach remotedocker --name <compute target name> --address <ip address or FQDN> --username <admin username> --password <admin password> 

# prepare the Docker image on the DSVM 
$ az ml experiment prepare -c <compute target name>
```
U moet nu klaar om te experimenten worden uitgevoerd op deze DSVM zijn.

## <a name="deallocate-a-dsvm-and-restart-it-later"></a>Een DSVM ongedaan gemaakt en later opnieuw starten
Wanneer u de compute-taken van Azure ML hebt, kunt u de DSVM ongedaan gemaakt. Deze actie wordt de virtuele machine, worden de rekenresources vrijgegeven afgesloten, maar dan de virtuele schijven behouden blijft. Er zijn niet in rekening gebracht voor de compute-kosten wanneer de toewijzing van de VM ongedaan is gemaakt.

Toewijzing van een virtuele machine:

```azurecli
$ az vm deallocate -g <resource group name> -n <vm name>
```

Voor het maken van de virtuele machine terug naar de levensduur, gebruiken de `az ml start` opdracht:

```azurecli
$ az vm start -g <resource group name> -n <vm name>
```

## <a name="expand-the-dsvm-os-disk"></a>Breid de DSVM OS-schijf
Linux-VM in Azure is doorgaans wordt geleverd met een besturingssysteemschijf van 30 GB. Wanneer gebruikt als doel berekenen voor Azure ML, kunt het geschikt is om snel door Docker-engine binnenhalen van Docker-installatiekopieën en het bouwen van conda lagen boven op het. Er is een goed idee om uit te breiden schijf met het besturingssysteem in een groter formaat (zoals 200 GB) om te voorkomen dat de fout 'volledige-schijf' wanneer u zich in het midden van de uitvoering. Verwijzing [het uitbreiden van de virtuele harde schijven op een Linux-VM met de Azure CLI](../../virtual-machines/linux/expand-disks.md) voor meer informatie over hoe u dit eenvoudig doen vanuit de azure cli. 

## <a name="create-an-apache-spark-for-azure-hdinsight-cluster-in-azure-portal"></a>Maken van een Apache Spark voor Azure HDInsight-cluster in Azure portal

Voor het scale-out Spark taken uitvoeren, moet u een Apache Spark voor Azure HDInsight-cluster maken in Azure portal.

1. Meld u aan bij Azure-portal van https://portal.azure.com
2. Klik op de **+ nieuw** koppeling en zoek naar 'HDInsight'.

    ![hdi vinden](media/how-to-create-dsvm-hdi/hdi.png)
    
3. Kies **HDInsight** in de lijst en klik vervolgens op de **maken** knop.
4. In de **basisbeginselen** Configuratiescherm **type Cluster** instellingen, zorg ervoor dat u kiest **Spark** als de _type Cluster_, **Linux** als de _besturingssysteem_, en **Spark 2.1.0 (HDI 3.6)** als de _Version.

    ![hdi configureren](media/how-to-create-dsvm-hdi/configure_hdi.png)

    >[!IMPORTANT]
    >U ziet in het bovenstaande scherm het cluster heeft een _clusteraanmeldgegevens_ veld en een _Secure Shell (SSH) gebruikersnaam_ veld. Dit zijn twee verschillende gebruikers-id's, zelfs als voor het gemak kunt u hetzelfde wachtwoord voor beide aanmeldingen. De _clusteraanmeldgegevens_ wordt gebruikt voor aanmelding bij de management-webgebruikersinterface van het HDI-cluster. De _SSH-gebruikersnaam voor aanmelding_ wordt gebruikt voor aanmelding bij het hoofdknooppunt van het cluster, en dit is wat u nodig hebt voor Azure ML verzending Spark-taken.

5. Kies de clustergrootte en de grootte van het knooppunt u nodig hebt en voltooi de wizard maken. Het kan duren voordat tot 30 minuten voor het cluster is ingericht. 

## <a name="attach-an-hdi-spark-cluster-compute-target"></a>Koppel een HDI Spark-cluster compute-doel

Zodra het Spark HDI-cluster is gemaakt, kunt u het nu koppelen aan uw Azure ML-project.

```azurecli
# attach the HDI compute target
$ az ml computetarget attach cluster --name <compute target name> --address <cluster name, such as myhdicluster123.azurehdinsight.net> --username <ssh username> --password <ssh password> 

# prepare the conda environment on HDI
$ az ml experiment prepare -c <compute target name>
```
Nu moet u gereed om te experimenten worden uitgevoerd op deze Spark-cluster.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over:
- [Overzicht van service van Azure Machine Learning-experimenten](experimentation-service-configuration.md)
- [Azure Machine Learning Workbench service experimenteren configuratiebestanden](experimentation-service-configuration-reference.md)
- [Apache Spark voor Azure HDInsight-cluster](https://azure.microsoft.com/services/hdinsight/apache-spark/)
- [Gegevens wetenschappelijke virtuele Machine](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/)

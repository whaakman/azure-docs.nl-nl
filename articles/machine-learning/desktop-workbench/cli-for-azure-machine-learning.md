---
title: Installeren en de CLI gebruiken voor veelgebruikte taken - Azure Machine Learning
description: Informatie over het installeren en de CLI gebruiken voor de meest voorkomende machine learning-taken in Azure Machine Learning.
services: machine-learning
author: haining
ms.author: haining
manager: cgronlun
ms.reviewer: mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/10/2018
ms.openlocfilehash: 0e37e1839d2248507a30de08e2ac4c975bd3b859
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35644088"
---
# <a name="install-and-use-the-machine-learning-cli-for-top-tasks-in-azure-machine-learning"></a>Installeren en gebruiken van de machine learning CLI voor veelgebruikte taken in Azure Machine Learning

Azure Machine Learning-services zijn een geïntegreerde, end-to-end gegevenswetenschap en geavanceerde analyse-oplossing. Professionele gegevenswetenschappers kunnen Azure Machine Learning-services gebruiken om gegevens voorbereiden, experimenten ontwikkelen en modellen te implementeren op cloudschaal. 

Azure Machine Learning biedt een opdrachtregelinterface (CLI) die u kunt:
+ Uw werkruimte en projecten beheren?
+ Compute-doelen instellen
+ Training experimenten uitvoeren
+ Geschiedenis weergeven en metrische gegevens voor eerdere uitvoeringen
+ Modellen in productie als webservices implementeren
+ Productie-modellen en services beheren

Dit artikel bevat enkele van de handigste CLI-opdrachten voor uw gemak. 

![Azure Machine Learning CLI](media/cli-for-azure-machine-learning/flow.png)

## <a name="what-you-need-to-get-started"></a>Wat u nodig hebt om aan de slag

Moet u rechten voor bijdragers toegang tot een Azure-abonnement of resourcegroep waarin u uw modellen kunt implementeren. Bovendien moet u Azure Machine Learning Workbench installeren om uit te voeren van de CLI. 

>[!IMPORTANT]
>De CLI met Azure Machine Learning-services geleverd wijkt af van de [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), die wordt gebruikt voor het beheren van Azure-resources.

## <a name="get-and-start-cli"></a>Ophalen en CLI starten

Als u wilt deze CLI, installeert u Azure Machine Learning Workbench, die kan worden gedownload vanaf hier:
    + Windows - https://aka.ms/azureml-wb-msi 
    + MacOS - https://aka.ms/azureml-wb-dmg 

De CLI starten:
+ In Azure Machine Learning Workbench, start u de CLI in het menu **File -> opdrachtprompt openen.**

## <a name="get-command-help"></a>Opdracht help opvragen 

Krijgt u extra informatie over de CLI-opdrachten met `--debug` of `--help` na de opdrachten zoals `az ml <xyz> --debug` waar `<xyz>` is de naam van de opdracht. Bijvoorbeeld:
```azurecli
az ml computetarget --debug 

az ml experiment --help
```

## <a name="common-cli-tasks-for-azure-machine-learning"></a>Algemene CLI-taken voor Azure Machine Learning 

Meer informatie over de meest algemene taken die u met de CLI in deze sectie uitvoeren kunt, met inbegrip van:
+ [Instellen van compute-doelen](#target)
+ [Verzenden van taken voor uitvoering op afstand](#jobs)
+ [Werken met Jupyter-notebooks](#jupyter)
+ [Interactie met uitvoeringsgeschiedenis](#history)
+ [Configureren van uw omgeving voor het operationeel maken](#o16n)

<a name="target"></a>

### <a name="set-up-a-compute-target"></a>Instellen van een compute-doel

U kunt uw machine learning-model in verschillende doelen, met inbegrip van berekenen:
+ in de lokale modus
+ in een Data Science VM (DSVM)
+ op een HDInsight-cluster

Het doel van een Data Science VM koppelen:
```azurecli
az ml computetarget attach remotedocker -n <target name> -a <ip address or FQDN> -u <username> -w <password>
``` 

Koppelen van een HDInsight-doel:
```azurecli
az ml computetarget attach cluster -n <target name> -a <cluster name, e.g. myhdicluster-ssh.azurehdinsight.net> -u <ssh username> -w <ssh password>
```

Binnen de **aml_config** map, kunt u het conda-afhankelijkheden. 

U kunt ook werken met PySpark, Python of Python in een GPU DSVM. 

Voor het definiëren van de bewerkingsmodus Python:
+ Voeg voor Python, `Framework:Python` in `<target name>.runconfig` 

+ Voeg voor PySpark, `Framework:PySpark` in `<target name>.runconfig` 

+ Voor Python in een GPU DSVM
    1. Voeg `Framework:Python` in `<target name>.runconfig` 

    1. Ook toe te voegen `baseDockerImage: microsoft/mmlspark:plus-gpu-0.9.9 and nvidiaDocker:true` in `<target name>.compute`

De compute-doel voorbereiden:
```azurecli
az ml experiment prepare -c <target name>
```

>[!TIP]
>Uw abonnement weergeven:<br/>
>`az account show`<br/>
>
>Uw abonnement instellen:<br/>
>`az account set –s "my subscription name" `

<a name="jobs"></a>

### <a name="submit-remote-jobs"></a>Externe taken verzenden

Een taak met een externe doel verzenden:
```azurecli
az ml experiment submit -c <target name> myscript.py
```

<a name="jupyter"></a>

### <a name="work-with-jupyter-notebooks"></a>Werken met Jupyter-notebooks

Een Jupyter-notebook start:
```azurecli
az ml notebook start
```

Met deze opdracht start een Jupyter-notebook in localhost. U kunt werken in lokale door het selecteren van de kernel Python 3 of werken in uw externe virtuele machine door het selecteren van de kernel `<target name>`.

<a name="history"></a>

### <a name="interact-with-and-explore-the-run-history"></a>Communiceren met en de uitvoeringsgeschiedenis verkennen

De uitvoeringsgeschiedenis weergeven:
```azurecli
az ml history list -o table
```

Overzicht van alle voltooide wordt uitgevoerd:
```azurecli
az ml history list --query "[?status=='Completed']" -o table
```

Wordt uitgevoerd met de hoogste nauwkeurigheid vinden:
```azurecli
az ml history list --query "@[?Accuracy != null] | max_by(@, &Accuracy).Accuracy"
```

U kunt ook de bestanden die zijn gegenereerd door elke uitvoering te downloaden. 

Ter bevordering van een model dat wordt opgeslagen in de uitvoer van de map:
```azurecli
az ml history promote -r <run id> -ap outputs/model.pkl -n <model name>
```

Dat model downloaden:
```azurecli
az ml asset download -l assets/model.pkl.link -d <model folder path>
```

<a name="o16n"></a>

### <a name="configure-your-environment-to-operationalize"></a>Configureren van uw omgeving voor het operationeel maken

Als u uw omgeving instelt, moet u het volgende maken:

> [!div class="checklist"]
> * Een resourcegroep 
> * Een storage-account
> * Een Azure Container Registry (ACR)
> * Een Application insight-account
> * Een Kubernetes-implementatie op een Azure Container Service (ACS)-cluster


Een lokale implementatie voor het testen in een Docker-container instellen:
```azurecli
az ml env setup -l <region, e.g. eastus2> -n <env name> -g <resource group name>
```

Instellen van een ACS-cluster met Kubernetes:
```azurecli
az ml env setup -l <region, e.g. eastus2> -n <env name> -g <resource group name> --cluster
```

De status van de implementatie bewaken:
```azurecli
az ml env show -n <environment name> -g <resource group name>
```

Om in te stellen de omgeving die moet worden gebruikt:
```azurecli
az ml env set -n <environment name> -g <resource group name>
```

## <a name="next-steps"></a>Volgende stappen

Aan de slag met een van de volgende artikelen: 
+ [Installeren en starten met Azure Machine Learning](../service/quickstart-installation.md)
+ [Classificeren Iris-gegevens-zelfstudie: Deel 1](tutorial-classifying-iris-part-1.md)

Dieper graven met een van de volgende artikelen:
+ [CLI-opdrachten voor het beheren van modellen](model-management-cli-reference.md)

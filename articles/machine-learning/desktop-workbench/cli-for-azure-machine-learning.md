---
title: Installeren en gebruiken van de CLI voor veelgebruikte taken - Azure Machine Learning
description: Informatie over het installeren en de CLI gebruiken voor de meest voorkomende machine learning-taken in Azure Machine Learning.
services: machine-learning
author: haining
ms.author: haining
manager: cgronlun
ms.reviewer: mldocs
ms.service: machine-learning
ms.component: desktop-workbench
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/10/2018
ms.openlocfilehash: 4b6ed5c70201b918a5bfa252719b2673303b38e9
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34830857"
---
# <a name="install-and-use-the-machine-learning-cli-for-top-tasks-in-azure-machine-learning"></a>Installeren en gebruiken van de machine learning-CLI voor veelgebruikte taken in Azure Machine Learning

Azure Machine Learning-services zijn een geïntegreerde, end-to-end gegevenswetenschap en geavanceerde analytics-oplossing. Professionele gegevenswetenschappers kunnen Azure Machine Learning-services gebruiken om gegevens voorbereiden, experimenten te ontwikkelen en implementeren van modellen in de cloud. 

Azure Machine Learning biedt die u kunt een opdrachtregelinterface (CLI):
+ Uw werkruimte en projecten beheren
+ Compute doelen instellen
+ Training experimenten uitvoeren
+ Geschiedenis van de weergave en metrische gegevens voor de afgelopen wordt uitgevoerd
+ Modellen implementeren in productie als webservices
+ Productie-modellen en services beheren

Dit artikel biedt een aantal van de handigste CLI-opdrachten voor uw gemak. 

![Azure Machine Learning-CLI](media/cli-for-azure-machine-learning/flow.png)

## <a name="what-you-need-to-get-started"></a>Wat u moet aan de slag

U eigenaarsrechten toegang nodig tot een Azure-abonnement of resourcegroep waarin u uw modellen kunt implementeren. Bovendien moet u Azure Machine Learning-Workbench om uit te voeren van de CLI installeren. 

>[!IMPORTANT]
>De CLI geleverd met Azure Machine Learning services verschilt van de [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), die wordt gebruikt voor het beheren van Azure-resources.

## <a name="get-and-start-cli"></a>Ophalen en CLI starten

Als u deze CLI, Azure Machine Learning Workbench, dat kan worden gedownload vanaf nu te installeren:
    + Windows - https://aka.ms/azureml-wb-msi 
    + MacOS - https://aka.ms/azureml-wb-dmg 

De CLI starten:
+ In Azure Machine Learning Workbench, start u de CLI in het menu **File -> Open de opdrachtprompt.**

## <a name="get-command-help"></a>Hulp bij het opdracht 

Krijgt u extra informatie over de CLI-opdrachten met `--debug` of `--help` na de opdrachten zoals `az ml <xyz> --debug` waar `<xyz>` is de naam van de opdracht. Bijvoorbeeld:
```azurecli
az ml computetarget --debug 

az ml experiment --help
```

## <a name="common-cli-tasks-for-azure-machine-learning"></a>Algemene CLI-taken voor Azure Machine Learning 

Meer informatie over de meest algemene taken die u met de CLI in deze sectie uitvoeren kunt, met inbegrip van:
+ [Compute doelen instellen](#target)
+ [Verzenden van taken voor uitvoering op afstand](#jobs)
+ [Werken met Jupyter-notebooks](#jupyter)
+ [Interactie met de geschiedenis uitvoeren](#history)
+ [Configureren van uw omgeving aan operationeel maken](#o16n)

<a name="target"></a>

### <a name="set-up-a-compute-target"></a>Een compute-doel instellen

U kunt uw machine learning-model in verschillende doelen, inclusief berekenen:
+ in de lokale modus
+ in een virtuele machine (DSVM) voor Gegevenswetenschap
+ op een HDInsight-cluster

Een doel gegevens wetenschappelijke VM koppelen:
```azurecli
az ml computetarget attach remotedocker -n <target name> -a <ip address or FQDN> -u <username> -w <password>
``` 

Koppelen van een HDInsight-doel:
```azurecli
az ml computetarget attach cluster -n <target name> -a <cluster name, e.g. myhdicluster-ssh.azurehdinsight.net> -u <ssh username> -w <ssh password>
```

Binnen de **aml_config** map, kunt u de afhankelijkheden conda wijzigen. 

U kunt ook werken met PySpark, Python of Python in een DSVM GPU. 

De bewerkingsmodus Python definiëren:
+ Voor Python, voegt u `Framework:Python` in `<target name>.runconfig` 

+ Voeg voor PySpark, `Framework:PySpark` in `<target name>.runconfig` 

+ Voor Python in een DSVM GPU
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
>Instellen van uw abonnement:<br/>
>`az account set –s "my subscription name" `

<a name="jobs"></a>

### <a name="submit-remote-jobs"></a>Externe taken verzenden

Voor het verzenden van een taak aan een externe doel:
```azurecli
az ml experiment submit -c <target name> myscript.py
```

<a name="jupyter"></a>

### <a name="work-with-jupyter-notebooks"></a>Werken met Jupyter-notebooks

Een Jupyter-notebook starten:
```azurecli
az ml notebook start
```

Deze opdracht start een Jupyter-notebook in localhost. U kunt werken in lokale door het selecteren van de kernel Python 3 of werken in uw externe virtuele machine door het selecteren van de kernel `<target name>`.

<a name="history"></a>

### <a name="interact-with-and-explore-the-run-history"></a>Communiceren met en bekijk de geschiedenis uitvoeren

Overzicht van de uitvoeringsgeschiedenis:
```azurecli
az ml history list -o table
```

Voor een lijst met alle voltooide wordt uitgevoerd:
```azurecli
az ml history list --query "[?status=='Completed']" -o table
```

Wordt uitgevoerd met de beste nauwkeurigheid vinden:
```azurecli
az ml history list --query "@[?Accuracy != null] | max_by(@, &Accuracy).Accuracy"
```

U kunt ook de bestanden die worden gegenereerd door elke uitvoering downloaden. 

Ter bevordering van een model dat wordt opgeslagen in de map uitvoer:
```azurecli
az ml history promote -r <run id> -ap outputs/model.pkl -n <model name>
```

Dit model downloaden:
```azurecli
az ml asset download -l assets/model.pkl.link -d <model folder path>
```

<a name="o16n"></a>

### <a name="configure-your-environment-to-operationalize"></a>Configureer uw omgeving voor operationeel maken

Als u uw omgeving uitoefening instelt, moet u het volgende maken:

> [!div class="checklist"]
> * Een resourcegroep 
> * een opslagaccount
> * Een Azure Container Registry (ACR)
> * Een toepassing inzicht-account
> * Een Kubernetes-implementatie in een Azure Container Service (ACS)-cluster


Een lokale implementatie voor het testen in een Docker-container instellen:
```azurecli
az ml env setup -l <region, e.g. eastus2> -n <env name> -g <resource group name>
```

Voor het instellen van een ACS-cluster met Kubernetes:
```azurecli
az ml env setup -l <region, e.g. eastus2> -n <env name> -g <resource group name> --cluster
```

De status van de implementatie te bewaken:
```azurecli
az ml env show -n <environment name> -g <resource group name>
```

Instellen van de omgeving die moet worden gebruikt:
```azurecli
az ml env set -n <environment name> -g <resource group name>
```

## <a name="next-steps"></a>Volgende stappen

Aan de slag met een van deze artikelen: 
+ [Installeren en starten met Azure Machine Learning](../service/quickstart-installation.md)
+ [Classificeren gegevens Iris-zelfstudie: Deel 1](tutorial-classifying-iris-part-1.md)

Verdiepen met een van deze artikelen:
+ [CLI-opdrachten voor het beheren van modellen](model-management-cli-reference.md)

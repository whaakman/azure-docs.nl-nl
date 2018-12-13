---
title: Azure Machine Learning-Model Management installatie en configuratie | Microsoft Docs
description: Dit document beschrijft de stappen en concepten betrokken bij het instellen en configureren van Modelbeheer in Azure Machine Learning.
services: machine-learning
author: aashishb
ms.author: aashishb
manager: hjerez
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 12/06/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 2d912f559af98045aaef8fe6fcaa1d83ab5f96bd
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53269359"
---
# <a name="model-management-setup"></a>Instellingen voor het beheer van model

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


Dit document helpt u op weg met het gebruik van Azure ML-Modelbeheer implementeren en beheren van uw machine learning-modellen als webservices. 

Met behulp van Azure ML-Modelbeheer, kunt u efficiënt te implementeren en beheren van Machine Learning-modellen die zijn gebouwd met behulp van een aantal frameworks waaronder SparkML, Keras, TensorFlow, de Microsoft Cognitive Toolkit of Python. 

Aan het einde van dit document, zou het mogelijk dat uw modelbeheeromgeving ingesteld en klaar voor het implementeren van uw machine learning-modellen.

## <a name="what-you-need-to-get-started"></a>Wat u nodig hebt om aan de slag
U optimaal gebruikmaken van deze handleiding, hebt u rechten voor bijdragers toegang tot een Azure-abonnement of resourcegroep die u kunt uw modellen te implementeren.
De CLI is vooraf geïnstalleerd op de Azure Machine Learning Workbench en klik op [Azure-Dsvm](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-virtual-machine-overview).

## <a name="using-the-cli"></a>Met behulp van de CLI
Voor het gebruik van de interfaces opdrachtregelinterface (CLI) van de Workbench, klikt u op **bestand** -> **Open Command Prompt**. 

Op een virtuele Machine voor Datatechnologie, verbinding maken en open de opdrachtprompt. Type `az ml -h` om de opties te bekijken. Voor meer informatie over de opdrachten, gebruikt u de--help-vlag.

U moet de CLI installeren op andere systemen.

>[!NOTE]
> In een Jupyter-notebook op een Linux-DSVM, kunt u de Azure CLI en Azure ML CLI openen met de onderstaande opdrachtindeling.  **Dit is voor een Jupyter-notebook op een Linux-DSVM specifiek**.  Deze opdrachten toegang verkregen tot de huidige Python-kernel in het notitieblok (bijvoorbeeld het conda `py35` omgeving)
>```
>import sys
>! {sys.executable} -m azure.cli login
>! {sys.executable} -m azure.cli ml -h
>```

### <a name="installing-or-updating-on-windows"></a>Installeren (of bijwerken) op Windows

Installeer Python van https://www.python.org/. Zorg ervoor dat u hebt geselecteerd voor het installeren van pip.

Open een opdrachtprompt met behulp van uitvoeren als Administrator en voer de volgende opdrachten uit:

```cmd
pip install -r https://aka.ms/az-ml-o16n-cli-requirements-file
```

### <a name="installing-or-updating-on-linux"></a>Installeren (of bijwerken) in Linux
Voer de volgende opdracht uit vanaf de opdrachtregel en volg de aanwijzingen:

```bash
sudo -i
pip install -r https://aka.ms/az-ml-o16n-cli-requirements-file
```

### <a name="configuring-docker-on-linux"></a>Configureren van Docker op Linux
Als u wilt Docker op Linux configureren voor gebruik door gebruikers van niet-hoofdmap, volg de instructies hier: [Stappen na de installatie voor Linux](https://docs.docker.com/engine/installation/linux/linux-postinstall/)

>[!NOTE]
> U kunt het onderstaande Docker correct wordt geconfigureerd script uitvoeren op een Linux-DSVM. **Vergeet niet om Meld u af en weer aanmelden nadat het script is uitgevoerd.**
>```
>sudo /opt/microsoft/azureml/initial_setup.sh
>```

## <a name="deploying-your-model"></a>Uw model implementeert
CLI gebruiken voor het implementeren van modellen als webservices. De webservices kunnen worden geïmplementeerd, lokaal of naar een cluster.

Beginnen met een lokaal te implementeren, valideren dat uw model en de code werkt, klikt u vervolgens implementeren in een cluster voor gebruik in productieomgevingen schaal.

Als u wilt starten, moet u uw implementatieomgeving instellen. De installatie van de omgeving is een taak. Nadat de installatie voltooid is, kunt u de omgeving voor latere implementaties opnieuw gebruiken. Zie de volgende sectie voor meer informatie.

Bij het voltooien van de installatie van de omgeving:
- U wordt gevraagd om aan te melden bij Azure. Als u wilt aanmelden, kunt u een webbrowser gebruiken om de pagina te openen https://aka.ms/devicelogin en voer de opgegeven code om te verifiëren.
- U wordt gevraagd tijdens het verificatieproces voor een account te verifiëren met. Belangrijk: Selecteer een account dat beschikt over een geldig Azure-abonnement en voldoende machtigingen om resources te maken in het account. Wanneer de aanmelding voltooid is, worden gegevens van uw abonnement wordt weergegeven en u wordt gevraagd of u wilt doorgaan met het geselecteerde account.

### <a name="environment-setup"></a>Omgeving instellen
Als u wilt het installatieproces start, moet u enkele omgeving providers registreren met de volgende opdrachten:

```azurecli
az provider register -n Microsoft.MachineLearningCompute
az provider register -n Microsoft.ContainerRegistry
az provider register -n Microsoft.ContainerService
```
#### <a name="local-deployment"></a>Lokale implementatie
Als u wilt implementeren en testen van uw web-service op de lokale computer, instellen van een lokale omgeving met behulp van de volgende opdracht uit. Naam van de resourcegroep is optioneel.

```azurecli
az ml env setup -l [Azure Region, e.g. eastus2] -n [your environment name] [-g [existing resource group]]
```
>[!NOTE] 
>Implementatie van de lokale webservice moet u om Docker te installeren op de lokale computer. 
>

De lokale omgeving setup-opdracht maakt de volgende bronnen in uw abonnement:
- Een resourcegroep (indien niet opgegeven of als de opgegeven naam niet bestaat)
- Een storage-account
- Een Azure Container Registry (ACR)
- Een Application insights-account

Nadat de installatie is voltooid, stelt u de omgeving moet worden gebruikt met de volgende opdracht:

```azurecli
az ml env set -n [environment name] -g [resource group]
```

#### <a name="cluster-deployment"></a>Implementatie van het cluster
Implementatie van het Cluster voor grootschalige productiescenario's gebruiken. Het stelt u een ACS-cluster met Kubernetes als de orchestrator. Het ACS-cluster kan worden uitgebreid voor het afhandelen van grotere doorvoer voor uw web-serviceaanroepen.

Als u wilt uw webservice implementeert in een productieomgeving, kunt u de omgeving met de volgende opdracht eerst instellen:

```azurecli
az ml env setup --cluster -n [your environment name] -l [Azure region e.g. eastus2] [-g [resource group]]
```

De setup-opdracht van de cluster-omgeving wordt gemaakt van de volgende bronnen in uw abonnement:
- Een resourcegroep (indien niet opgegeven of als de opgegeven naam niet bestaat)
- Een storage-account
- Een Azure Container Registry (ACR)
- Een Kubernetes-implementatie op een Azure Container Service (ACS)-cluster
- Een Application insights-account

>[!IMPORTANT]
> Om te kunnen maken van een clusteromgeving, moet u toegangsrechten van een bijdrager van het Azure-abonnement of de resourcegroep worden hebben.

De resourcegroep, opslagaccount en ACR worden snel gemaakt. De ACS-implementatie kan maximaal 20 minuten duren. 

Om te controleren of de status van een doorlopende clusterinrichting, gebruik de volgende opdracht:

```azurecli
az ml env show -n [environment name] -g [resource group]
```

Nadat setup voltooid is, moet u de omgeving moet worden gebruikt voor deze implementatie instellen. Gebruik de volgende opdracht:

```azurecli
az ml env set -n [environment name] -g [resource group]
```

>[!NOTE] 
> Nadat de omgeving is gemaakt, voor de volgende implementaties, moet u alleen de bovenstaande set-opdracht gebruiken om opnieuw.
>

### <a name="create-a-model-management-account"></a>Een Modelbeheeraccount maken
Een modelbeheeraccount is vereist voor het implementeren van modellen. U moet doen dit maar eenmaal per abonnement, en dezelfde account in meerdere implementaties kunt hergebruiken.

Gebruik de volgende opdracht voor het maken van een nieuw account:

```azurecli
az ml account modelmanagement create -l [Azure region, e.g. eastus2] -n [your account name] -g [resource group name] --sku-instances [number of instances, e.g. 1] --sku-name [Pricing tier for example S1]
```

Voor het gebruik van een bestaand account, gebruikt u de volgende opdracht:
```azurecli
az ml account modelmanagement set -n [your account name] -g [resource group it was created in]
```

Als gevolg van dit proces wordt de omgeving gereed is en het Modelbeheer-account is gemaakt voor de functies die nodig zijn voor het beheren en implementeren van Machine Learning-modellen (Zie [Azure Machine Learning Modelbeheer](model-management-overview.md) voor een overzicht).

## <a name="next-steps"></a>Volgende stappen

* Voor instructies over het implementeren van webservices om uit te voeren op een lokale computer of een cluster gaat u naar [implementeren van een Machine Learning-Model als een webservice](model-management-service-deploy.md).
* Probeer een van de vele voorbeelden in de galerie.

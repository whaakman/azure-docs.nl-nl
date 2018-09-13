---
title: Azure Machine Learning-Model Management installatie en configuratie | Microsoft Docs
description: Dit document beschrijft de stappen en concepten betrokken bij het instellen en configureren van Modelbeheer in Azure Machine Learning.
services: machine-learning
author: raymondlaghaeian
ms.author: raymondl
manager: hjerez
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 08/29/2017
ms.openlocfilehash: 883e3d2c5945a38c8fbca5c9f0f5e8a1e4093be1
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35646078"
---
# <a name="model-management-setup"></a>Instellingen voor het beheer van model

## <a name="overview"></a>Overzicht
Dit document helpt u op weg met het gebruik van Azure ML-Modelbeheer implementeren en beheren van uw machine learning-modellen als webservices. 

Met behulp van Azure ML-Modelbeheer, kunt u efficiënt te implementeren en beheren van Machine Learning-modellen die zijn gebouwd met behulp van een aantal frameworks waaronder SparkML, Keras, TensorFlow, de Microsoft Cognitive Toolkit of Python. 

Aan het einde van dit document, zou het mogelijk dat uw modelbeheeromgeving ingesteld en klaar voor het implementeren van uw machine learning-modellen.

## <a name="what-you-need-to-get-started"></a>Wat u nodig hebt om aan de slag
Om optimaal te profiteren van deze handleiding, hebt u de eigenaar van de toegang tot een Azure-abonnement dat u kunt uw modellen te implementeren.
De CLI is vooraf geïnstalleerd op de Azure Machine Learning Workbench en klik op [Azure-Dsvm](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-virtual-machine-overview).

## <a name="using-the-cli"></a>Met behulp van de CLI
Voor het gebruik van de interfaces opdrachtregelinterface (CLI) van de Workbench, klikt u op **bestand** -] **Open-Interface met opdrachtregel**. 

Op een virtuele Machine voor Datatechnologie, verbinding maken en open de opdrachtprompt. Type `az ml -h` om de opties te bekijken. Voor meer informatie over de opdrachten, gebruikt u de--help-vlag.

U moet de CLI installeren op andere systemen.

### <a name="installing-or-updating-on-windows"></a>Installeren (of bijwerken) op Windows

Installeer Python van https://www.python.org/. Zorg ervoor dat u hebt geselecteerd voor het installeren van pip.

Open een opdrachtprompt met behulp van uitvoeren als Administrator en voer de volgende opdrachten uit:

```cmd
pip install azure-cli
pip install azure-cli-ml
```
 
>[!NOTE]
>Als u een eerdere versie hebt, verwijdert u deze eerst met behulp van de volgende opdracht uit:
>

```cmd
pip uninstall azure-cli-ml
```

### <a name="installing-or-updating-on-linux"></a>Installeren (of bijwerken) in Linux
Voer de volgende opdracht uit vanaf de opdrachtregel en volg de aanwijzingen:

```bash
sudo -i
pip install azure-cli
pip install azure-cli-ml
```

Nadat installatieassembly voltooid is, voert u de volgende opdracht uit:

```bash
sudo /opt/microsoft/azureml/initial_setup.sh
```

>[!NOTE]
>Meld u af en meld u weer aan bij uw SSH-sessie om de wijzigingen pas van kracht.
>U kunt de voorgaande opdrachten gebruiken om bij te werken van een eerdere versie van de CLI's op de DSVM.
>

## <a name="deploying-your-model"></a>Uw model implementeert
Gebruik de CLI's gebruikt om modellen als webservices implementeren. De webservices kunnen worden geïmplementeerd, lokaal of naar een cluster.

Beginnen met een lokaal te implementeren, valideren dat uw model en de code werkt, klikt u vervolgens implementeren in een cluster voor gebruik in productieomgevingen schaal.

Als u wilt starten, moet u uw implementatieomgeving instellen. De installatie van de omgeving is een taak. Nadat de installatie voltooid is, kunt u de omgeving voor latere implementaties opnieuw gebruiken. Zie de volgende sectie voor meer informatie.

Bij het voltooien van de installatie van de omgeving:
- U wordt gevraagd om aan te melden bij Azure. Als u wilt aanmelden, kunt u een webbrowser gebruiken om de pagina te openen https://aka.ms/devicelogin en voer de opgegeven code om te verifiëren.
- U wordt gevraagd tijdens het verificatieproces voor een account te verifiëren met. Belangrijk: Selecteer een account dat beschikt over een geldig Azure-abonnement en voldoende machtigingen om resources te maken in het account. - Als de aanmelding voltooid is, de gegevens van uw abonnement wordt weergegeven en u wordt gevraagd of u doorgaan wilt met de geselecteerde-account.

### <a name="environment-setup"></a>Omgeving instellen
Als u wilt het installatieproces start, moet u de omgeving-provider registreren met de volgende opdracht:

```azurecli
az provider register -n Microsoft.MachineLearningCompute
```

#### <a name="local-deployment"></a>Lokale implementatie
Als u wilt implementeren en testen van uw web-service op de lokale computer, instellen van een lokale omgeving met behulp van de volgende opdracht uit:

```azurecli
az ml env setup -l [location of Azure Region, e.g. eastus2] -n [your environment name] [-g [existing resource group]]
```
>[!NOTE] 
>Implementatie van de lokale webservice moet uw om Docker te installeren op de lokale computer. 
>

De lokale omgeving setup-opdracht maakt de volgende bronnen in uw abonnement:
- Een resourcegroep (indien niet opgegeven)
- Een storage-account
- Een Azure Container Registry (ACR)
- Application Insights

Nadat de installatie is voltooid, stelt u de omgeving moet worden gebruikt met de volgende opdracht:

```azurecli
az ml env set -n [environment name] -g [resource group]
```

#### <a name="cluster-deployment"></a>Implementatie van het cluster
Implementatie van het Cluster voor grootschalige productiescenario's gebruiken. Het stelt u een ACS-cluster met Kubernetes als de orchestrator. Het ACS-cluster kan worden uitgebreid voor het afhandelen van grotere doorvoer voor uw web-serviceaanroepen.

Als u wilt uw webservice implementeert in een productieomgeving, kunt u de omgeving met de volgende opdracht eerst instellen:

```azurecli
az ml env setup -c --name [your environment name] --location [Azure region e.g. eastus2] [-g [resource group]]
```

De setup-opdracht van de cluster-omgeving wordt gemaakt van de volgende bronnen in uw abonnement:
- Een resourcegroep (indien niet opgegeven)
- Een storage-account
- Een Azure Container Registry (ACR)
- Een Kubernetes-implementatie op een Azure Container Service (ACS)-cluster
- Application Insights

De resourcegroep, opslagaccount en ACR worden snel gemaakt. De ACS-implementatie kan maximaal 20 minuten duren. 

Nadat setup voltooid is, moet u de omgeving moet worden gebruikt voor deze implementatie instellen. Gebruik de volgende opdracht:

```azurecli
az ml env set -n [environment name] -g [resource group]
```

>[!NOTE] 
> Nadat de omgeving is gemaakt, voor de volgende implementaties, moet u alleen de bovenstaande set-opdracht gebruiken om opnieuw.
>

>[!NOTE] 
>Geef een SSL-certificaat voor het maken van een HTTPS-eindpunt bij het maken van een cluster met behulp van de--cert-naam en--cert-pem-opties in az ml env setup. Hiermee stelt u het cluster aanvragen verwerken voor https, beveiligd met behulp van het opgegeven certificaat. Nadat setup voltooid is, maakt u een CNAME-DNS-record die naar de FQDN van het cluster verwijst.

### <a name="create-an-account"></a>Een account maken
Er is een account vereist voor het implementeren van modellen. U moet doen dit maar eenmaal per account, en dezelfde account in meerdere implementaties kunt hergebruiken.

Gebruik de volgende opdracht voor het maken van een nieuw account:

```azurecli
az ml account modelmanagement create -l [Azure region, e.g. eastus2] -n [your account name] -g [resource group name] --sku-instances [number of instances, e.g. 1] --sku-name [Pricing tier for example S1]
```

Voor het gebruik van een bestaand account, gebruikt u de volgende opdracht:
```azurecli
az ml account modelmanagement set -n [your account name] -g [resource group it was created in]
```

### <a name="deploy-your-model"></a>Uw model implementeren
U bent nu klaar om uw opgeslagen model als een webservice te implementeren. 

```azurecli
az ml service create realtime --model-file [model file/folder path] -f [scoring file e.g. score.py] -n [your service name] -s [schema file e.g. service_schema.json] -r [runtime for the Docker container e.g. spark-py or python] -c [conda dependencies file for additional python packages]
```

### <a name="next-steps"></a>Volgende stappen
Probeer een van de vele voorbeelden in de galerie.

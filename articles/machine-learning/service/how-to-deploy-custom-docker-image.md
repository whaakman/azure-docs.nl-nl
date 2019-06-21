---
title: Over het implementeren van een model met behulp van een aangepaste Docker-installatiekopie
titleSuffix: Azure Machine Learning service
description: Leer hoe u een aangepaste Docker-installatiekopie gebruiken bij het implementeren van uw modellen van de service Azure Machine Learning. Bij het implementeren van een getraind model, wordt de installatiekopie van het host, webserver en andere onderdelen die nodig zijn om uit te voeren van de service een Docker-installatiekopie gemaakt. Azure Machine Learning-service biedt een standaardinstallatiekopie voor u, kunt u ook uw eigen installatiekopie gebruiken.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 06/05/2019
ms.openlocfilehash: bd0e8099be5422d561541aeb8911c9a1610befcb
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67272763"
---
# <a name="deploy-a-model-using-a-custom-docker-image"></a>Een model met behulp van een aangepaste Docker-installatiekopie implementeren

Leer hoe u een aangepaste Docker-installatiekopie gebruiken bij het implementeren van getrainde modellen met de Azure Machine Learning-service.

Wanneer u een getraind model naar een webservice of IoT Edge-apparaat implementeert, wordt een Docker-installatiekopie gemaakt. Deze installatiekopie bevat het model, conda-omgeving en elementen die nodig zijn voor het gebruik van het model. Het bevat ook een webserver voor het afhandelen van inkomende aanvragen bij de implementatie als een webservice en de onderdelen die nodig zijn om te werken met Azure IoT Hub.

Azure Machine Learning-service biedt een standaard-Docker-installatiekopie, zodat u hoeft zich geen zorgen over het maken van een te. U kunt ook een aangepaste installatiekopie die u als maakt een _basisinstallatiekopie_. Een basisinstallatiekopie wordt als uitgangspunt gebruikt wanneer een installatiekopie is gemaakt voor een implementatie. Het biedt het onderliggende besturingssysteem en onderdelen. Het implementatieproces wordt vervolgens extra onderdelen, zoals het model, conda-omgeving en andere activa, toegevoegd aan de installatiekopie van het voordat u deze implementeert.

Meestal maakt u een aangepaste installatiekopie wanneer u wilt beheren onderdeel versies of Bespaar tijd tijdens de implementatie. Bijvoorbeeld, als u wilt standaardiseren op een specifieke versie van Python, Conda of andere onderdelen. U kunt ook voor het installeren van software vereist voor uw model, waarbij het installatieproces kan lang duren. De software te installeren bij het maken van de basisinstallatiekopie betekent dat u geen hebt om deze te installeren voor elke implementatie.

> [!IMPORTANT]
> Wanneer u implementeert een model, kunt u de belangrijkste onderdelen zoals de webserver of IoT Edge-onderdelen, niet overschrijven. Deze onderdelen bieden een bekende werkomgeving die wordt getest en ondersteund door Microsoft.

> [!WARNING]
> Microsoft kan mogelijk niet over het oplossen van problemen die worden veroorzaakt door een aangepaste installatiekopie. Als u problemen ondervindt, kunt u worden gevraagd om de standaardinstallatiekopie van of een van de installatiekopieën die Microsoft biedt om te zien als het probleem specifiek voor uw installatiekopie is te gebruiken.

Dit document is onderverdeeld in twee secties:

* Een aangepaste installatiekopie maken: Bevat informatie voor beheerders en DevOps op een aangepaste installatiekopie maken en configureren van verificatie naar een Azure Container Registry met behulp van de Azure CLI en Machine Learning CLI.
* Een aangepaste installatiekopie gebruiken: -Informatie biedt aan Data Scientists en DevOps/MLOps over het gebruik van aangepaste installatiekopieën bij het implementeren van een getraind model van de Python-SDK of de ML CLI.

## <a name="prerequisites"></a>Vereisten

* Een Azure Machine Learning-service-werkgroep. Zie voor meer informatie de [maken van een werkruimte](setup-create-workspace.md) artikel.
* The Azure Machine Learning SDK. Zie voor meer informatie de sectie Python-SDK van de [maken van een werkruimte](setup-create-workspace.md#sdk) artikel.
* De [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
* De [CLI-extensie voor Azure Machine Learning](reference-azure-machine-learning-cli.md).
* Een [Azure Container Registry](/azure/container-registry) of andere Docker-register die toegankelijk is via internet.
* De stappen in dit document wordt ervan uitgegaan dat u bekend bent met het maken en gebruiken een __Deductie configuratie__ object als onderdeel van de modelimplementatie van het. Voor meer informatie, Zie de sectie 'voorbereiden om te implementeren' van [waar u wilt implementeren en hoe](how-to-deploy-and-where.md#prepare-to-deploy).

## <a name="create-a-custom-image"></a>Een aangepaste installatiekopie maken

De informatie in deze sectie wordt ervan uitgegaan dat u van een Azure Container Registry gebruikmaakt voor het opslaan van Docker-installatiekopieën. Gebruik de volgende controlelijst bij het plannen van het maken van aangepaste installatiekopieën voor Azure Machine Learning-service:

* Gebruikt u Azure Container Registry gemaakt voor de werkruimte van de Azure Machine Learning-service of een zelfstandige Azure Container Registry?

    Wanneer u installatiekopieën gebruikt die zijn opgeslagen in de __containerregister voor de werkruimte__, hoeft u niet om het register te verifiëren. Verificatie wordt verwerkt door de werkruimte.

    > [!TIP]
    > De container registry voor uw werkruimte wordt gemaakt van de eerste keer dat u trainen of implementeren van een model met behulp van de werkruimte. Als u hebt een nieuwe werkruimte hebt gemaakt, maar niet getraind of een model gemaakt, wordt er geen Azure Container Registry bestaat voor de werkruimte.

    Zie voor meer informatie over het ophalen van de naam van de Azure Container Registry voor uw werkruimte het [Get containerregisternaam](#getname) sectie van dit artikel.

    Wanneer u installatiekopieën gebruikt die zijn opgeslagen in een __zelfstandige containerregister__, moet u een service-principal die minimaal is leestoegang configureren. U geeft de service principal-ID (gebruikersnaam) en het wachtwoord voor iedereen die gebruikmaakt van installatiekopieën uit het register. De uitzondering hierop is als u het containerregister dat openbaar toegankelijk is.

    Zie voor meer informatie over het maken van een persoonlijke Azure Container Registry [maken van een privécontainerregister](/azure/container-registry/container-registery-get-started-azure-cli).

    Zie voor meer informatie over het gebruik van service-principals met Azure Container Registry [Azure Container Registry-verificatie met service-principals](/azure/container-registry/container-registry-auth-service-principal).

* Azure Container Registry en image-gegevens: Geef de installatiekopienaam aan iedereen die moet worden gebruikt. Bijvoorbeeld, een installatiekopie met de naam `myimage`, opgeslagen in een register met de naam `myregistry`, waarnaar wordt verwezen als `myregistry.azurecr.io/myimage` bij het gebruik van de afbeelding voor de implementatie van model

* Vereisten voor installatiekopieën: Azure Machine Learning-service biedt alleen ondersteuning voor Docker-installatiekopieën die de volgende software bieden:

    * Ubuntu 16.04 of hoger.
    * Conda 4.5. # of hoger.
    * Python 3.5. # of 3.6. #.

<a id="getname"></a>

### <a name="get-container-registry-information"></a>Informatie over het register van de container ophalen

In deze sectie leert u hoe u aan de naam van de Azure Container Registry voor uw werkruimte van Azure Machine Learning-service.

> [!TIP]
> De container registry voor uw werkruimte wordt gemaakt van de eerste keer dat u trainen of implementeren van een model met behulp van de werkruimte. Als u hebt een nieuwe werkruimte hebt gemaakt, maar niet getraind of een model gemaakt, wordt er geen Azure Container Registry bestaat voor de werkruimte.

Als u hebt al getraind of modellen met behulp van de Azure Machine Learning-service wordt geïmplementeerd, is een container registry gemaakt voor uw werkruimte. Voor de naam van deze container registry, gebruikt u de volgende stappen uit:

1. Open een nieuwe shell of opdrachtprompt en gebruik de volgende opdracht om te verifiëren bij uw Azure-abonnement:

    ```azurecli-interactive
    az login
    ```

    Volg de aanwijzingen om het abonnement te verifiëren.

2. Gebruik de volgende opdracht om de containerregister voor de werkruimte weer te geven. Vervang `<myworkspace>` met de naam van uw Azure Machine Learning-service-werkruimte. Vervang `<resourcegroup>` met de Azure-resourcegroep waarin uw werkruimte:

    ```azurecli-interactive
    az ml workspace show -w <myworkspace> -g <resourcegroup> --query containerRegistry
    ```

    De informatie die wordt geretourneerd is vergelijkbaar met de volgende tekst:

    ```text
    /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.ContainerRegistry/registries/<registry_name>
    ```

    De `<registry_name>` waarde is de naam van de Azure Container Registry voor uw werkruimte.

### <a name="build-a-custom-image"></a>Bouw een aangepaste installatiekopie

De stappen in deze sectie overzicht maken van een aangepaste Docker-installatiekopie in uw Azure Container Registry.

1. Maak een nieuw tekstbestand met de naam `Dockerfile`, en gebruik de volgende tekst als de inhoud:

    ```text
    FROM ubuntu:16.04

    ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
    ENV PATH /opt/miniconda/bin:$PATH

    RUN apt-get update --fix-missing && \
        apt-get install -y wget bzip2 && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*

    RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-4.5.12-Linux-x86_64.sh -O ~/miniconda.sh && \
        /bin/bash ~/miniconda.sh -b -p /opt/miniconda && \
        rm ~/miniconda.sh && \
        /opt/miniconda/bin/conda clean -tipsy

    RUN conda install -y python=3.6 && \
        conda clean -aqy && \
        rm -rf /opt/miniconda/pkgs && \
        find / -type d -name __pycache__ -prune -exec rm -rf {} \;
    ```

2. Vanuit een shell of opdrachtprompt, gebruikt u de volgende om te verifiëren bij Azure Container Registry. Vervang de `<registry_name>` met de naam van het containerregister dat u wilt opslaan in de afbeelding:

    ```azurecli-interactive
    az acr login --name <registry_name>
    ```

3. Als u de docker-bestand uploaden en bouw het, gebruik de volgende opdracht. Vervang `<registry_name>` met de naam van het containerregister dat u wilt opslaan in de afbeelding:

    ```azurecli-interactive
    az acr build --image myimage:v1 --registry <registry_name> --file Dockerfile .
    ```

    Tijdens het buildproces worden gegevens gestreamd back-ups maken op de opdrachtregel. Als de build voltooid is, ontvangt u een bericht dat lijkt op de volgende tekst:

    ```text
    Run ID: cda was successful after 2m56s
    ```

Zie voor meer informatie over het maken van installatiekopieën met een Azure Container Registry, [bouwen en uitvoeren van een containerinstallatiekopie met behulp van Azure Container Registry-taken](/docs.microsoft.com/azure/container-registry/container-registry-quickstart-task-cli.md)

Zie voor meer informatie over het uploaden van de bestaande installatiekopieën naar een Azure Container Registry [uw eerste installatiekopie naar een persoonlijk Docker-containerregister pushen](/azure/container-registry/container-registry-get-started-docker-cli.md).

## <a name="use-a-custom-image"></a>Een aangepaste installatiekopie gebruiken

Voor het gebruik van een aangepaste installatiekopie, moet u de volgende informatie:

* De __installatiekopienaam__. Bijvoorbeeld, `mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda` is het pad naar een eenvoudige Docker-installatiekopie geleverd door Microsoft.
* Als de afbeelding een __privéopslagplaats__, moet u de volgende informatie:

    * Het register __adres__. Bijvoorbeeld `myregistry.azureecr.io`.
    * Een service-principal __gebruikersnaam__ en __wachtwoord__ die leestoegang heeft tot het register.

    Als u deze informatie hebt, kunt u contact op met de beheerder voor Azure Container Registry met de installatiekopie.

### <a name="publicly-available-images"></a>Openbaar beschikbare installatiekopieën

Microsoft biedt verschillende docker-installatiekopieën op een openbaar toegankelijke opslagplaats, die kan worden gebruikt met de stappen in deze sectie:

| Image | Description |
| ----- | ----- |
| `mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda` | Basic-installatiekopie voor Azure Machine Learning-service |
| `mcr.microsoft.com/azureml/onnxruntime:v0.4.0` | Bevat de ONNX-runtime. |
| `mcr.microsoft.com/azureml/onnxruntime:v0.4.0-cuda10.0-cudnn7` | Bevat de ONNX-runtime en CUDA-onderdelen. |
| `mcr.microsoft.com/azureml/onnxruntime:v0.4.0-tensorrt19.03` | U kunt ONNX-runtime en TensorRT bevat. |

> [!TIP]
> Omdat deze installatiekopieën algemeen beschikbaar zijn, hoeft u niet op te geven van een adres, de gebruikersnaam of het wachtwoord wanneer het gebruik ervan.

> [!IMPORTANT]
> Microsoft-installatiekopieën die gebruikmaken van CUDA of TensorRT moeten alleen worden gebruikt op Microsoft Azure-Services.

> [!TIP]
>__Als uw model wordt getraind op Azure Machine Learning-Computing__, met __versie 1.0.22 of hoger__ van de SDK van Azure Machine Learning, een installatiekopie is gemaakt tijdens de training. Gebruiken voor het detecteren van de naam van deze afbeelding `run.properties["AzureML.DerivedImageName"]`. Het volgende voorbeeld ziet u hoe u deze installatiekopie te gebruiken:
>
> ```python
> # Use an image built during training with SDK 1.0.22 or greater
> image_config.base_image = run.properties["AzureML.DerivedImageName"]
> ```

### <a name="use-an-image-with-the-azure-machine-learning-sdk"></a>Een installatiekopie met de SDK van Azure Machine Learning gebruiken

Instellen voor het gebruik van een aangepaste installatiekopie, het `base_image` eigenschap van de [Deductie configuratieobject](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) naar het adres van de afbeelding:

```python
# use an image from a registry named 'myregistry'
inference_config.base_image = "myregistry.azurecr.io/myimage:v1"
```

Deze indeling werkt voor beide installatiekopieën die zijn opgeslagen in Azure Container Registry voor uw werkruimte en container-registers die openbaar toegankelijk zijn. De volgende code gebruikt bijvoorbeeld een standaardinstallatiekopie geleverd door Microsoft:

```python
# use an image available in public Container Registry without authentication
inference_config.base_image = "mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda"
```

Het gebruik van een installatiekopie van een __privécontainerregister__ dat is niet in uw werkruimte, moet u het adres van de opslagplaats en een gebruikersnaam en wachtwoord opgeven:

```python
# Use an image available in a private Container Registry
inference_config.base_image = "myregistry.azurecr.io/mycustomimage:1.0"
inference_config.base_image_registry.address = "myregistry.azurecr.io"
inference_config.base_image_registry.username = "username"
inference_config.base_image_registry.password = "password"
```

### <a name="use-an-image-with-the-machine-learning-cli"></a>Een installatiekopie met de Machine Learning CLI gebruiken

> [!IMPORTANT]
> De Machine Learning CLI kunt installatiekopieën momenteel gebruiken vanuit de Azure Container Registry voor uw werkruimte of een openbaar toegankelijke opslagplaatsen. Deze installatiekopieën niet gebruiken van zelfstandige persoonlijke registers.

Wanneer u implementeert een model met behulp van de Machine Learning CLI, kunt u een Deductie-configuratiebestand dat verwijst naar de aangepaste installatiekopie opgeven. De volgende JSON-document ziet u hoe u om te verwijzen naar een afbeelding in een openbare container registry:

```json
{
   "entryScript": "score.py",
   "runtime": "python",
   "condaFile": "infenv.yml",
   "extraDockerfileSteps": null,
   "sourceDirectory": null,
   "enableGpu": false,
   "baseImage": "mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda",
   "baseImageRegistry": "mcr.microsoft.com"
}
```

Dit bestand wordt gebruikt met de `az ml model deploy` opdracht. De `--ic` parameter wordt gebruikt om op te geven van het configuratiebestand Deductie.

```azurecli
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json --ct akscomputetarget
```

Voor meer informatie over het implementeren van een model met behulp van de ML CLI, Zie de sectie ' model registratie, profilering en implementatie' van de [CLI-extensie voor Azure Machine Learning-service](reference-azure-machine-learning-cli.md#model-registration-profiling-deployment) artikel.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [waar u wilt implementeren en hoe](how-to-deploy-and-where.md).
* Meer informatie over het [trainen en implementeren van machine learning-modellen met behulp van Azure-pijplijnen](/azure/devops/pipelines/targets/azure-machine-learning?view=azure-devops).
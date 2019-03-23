---
title: Een werkruimte maken
titleSuffix: Azure Machine Learning service
description: De Azure-portal, de SDK, een sjabloon of de CLI gebruiken om te maken van de werkruimte van uw Azure Machine Learning-service. Deze werkruimte biedt een centrale locatie voor het werken met alle artefacten die u maakt wanneer u Azure Machine Learning-service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.author: sgilley
author: sdgilley
ms.date: 03/21/2019
ms.openlocfilehash: 2c3b63e671240a239dc2037ce56e56af4a8f95e7
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369153"
---
# <a name="create-an-azure-machine-learning-service-workspace"></a>Een Azure Machine Learning-service-werkruimte maken

Voor het gebruik van Azure Machine Learning-service, moet u een [ **werkruimte van Azure Machine Learning-service**](concept-azure-machine-learning-architecture.md#workspace).  Deze werkruimte is de resource op het hoogste niveau voor de service en biedt u een centrale locatie voor het werken met alle artefacten die u maakt. 

In dit artikel leert u hoe u een werkruimte maken met behulp van deze methoden: 
* De [Azure-portal](#portal) interface
* De [Azure Machine Learning-SDK voor Python](#sdk)
* Een [Azure Resource Manager-sjabloon](#template)
* De [Azure Machine Learning CLI](#cli)

De werkruimte die u maakt met behulp van de stappen die u hier in kan worden gebruikt als een vereiste voor andere zelfstudies en artikelen met procedures. 

Wanneer u een werkruimte maakt zijn de volgende Azure-resources automatisch (als ze regionaal beschikbaar) toegevoegd:
 
- [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
- [Azure Storage](https://azure.microsoft.com/services/storage/)
- [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 
- [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)

>[!Note]
>Net als bij andere Azure-services gelden er bepaalde limieten en quota voor Machine Learning. [Meer informatie over quota's en hoe u meer kunt aanvragen.](how-to-manage-quotas.md)


## <a name="prerequisites"></a>Vereisten
Een werkruimte te maken, moet u een Azure-abonnement. Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](http://aka.ms/AMLFree).

## <a name="portal"></a> Azure-portal

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

Ongeacht hoe het is gemaakt, vindt u uw werkruimte in de [Azure-portal](https://portal.azure.com/).  Zie [weergeven van een werkruimte](how-to-manage-workspace.md#view) voor meer informatie.

## <a name="sdk"></a> Python-SDK

Maak uw werkruimte met behulp van de Python-SDK. U moet eerst de SDK te installeren.

> [!IMPORTANT]
> Installatie van de SDK overslaan als u een Azure Data Science Virtual Machine of een Azure Databricks gebruiken.
> * Op Data Science Virtual Machines die zijn gemaakt na 27 september 2018 is de Python SDK al geïnstalleerd. De installatie overslaan en beginnen met [een werkruimte maken met de SDK](#sdk-create).
> * Gebruik in de Azure Databricks-omgeving in plaats daarvan de [Databricks-installatiestappen](how-to-configure-environment.md#azure-databricks).

>[!NOTE]
> Volg deze instructies om te installeren en gebruik de SDK van de lokale computer. Voor het gebruik van Jupyter op een externe virtuele machine, instellen van een externe bureaublad of X-terminal-sessie.

Voordat u de SDK installeert, raden we u aan om een geïsoleerde omgeving voor Python te maken. In dit artikel wordt [Miniconda](https://docs.conda.io/en/latest/miniconda.html) gebruikt, maar u kunt ook gebruikmaken van volledig geïnstalleerde [Anaconda](https://www.anaconda.com/) of [Python virtualenv](https://virtualenv.pypa.io/en/stable/).

De instructies in dit artikel installeert alle pakketten die u wilt uitvoeren van de Quick Start- en -zelfstudie-notebooks.  Voor andere voorbeeldnotebooks moeten mogelijk extra onderdelen worden geïnstalleerd.  Zie [De Azure Machine Learning-SDK voor Python installeren](https://docs.microsoft.com/python/api/overview/azure/ml/install) voor meer informatie over deze onderdelen.

### <a name="install-miniconda"></a>Miniconda installeren

[Download en installeer Miniconda](https://docs.conda.io/en/latest/miniconda.html). Selecteer de versie Python 3.7 of hoger voor installatie. Selecteer niet Python 2.x.  

### <a name="create-an-isolated-python-environment"></a>Een geïsoleerde omgeving voor Python maken

1. Open een opdrachtregelvenster, maak een nieuwe conda-omgeving met de naam *myenv* en installeer Python 3.6.5. Azure Machine Learning SDK werkt met Python 3.5.2 of hoger, maar de geautomatiseerde Machine Learning-onderdelen zijn niet volledig functioneel in Python 3.7.  Het duurt enkele minuten om de omgeving te maken terwijl onderdelen en pakketten worden gedownload.

    ```shell
    conda create -n myenv python=3.6.5
    ```

1. Activeer de omgeving.

    ```shell
    conda activate myenv
    ```

1. Schakel omgevingsspecifieke ipython-kernels in:

    ```shell
    conda install notebook ipykernel
    ```

    Maak vervolgens de kernel:

    ```shell
    ipython kernel install --user
    ```

### <a name="install-the-sdk"></a>De SDK installeren

1. Installeer de kernonderdelen van de Machine Learning SDK met Jupyter-notebookmogelijkheden in de geactiveerde conda-omgeving. Afhankelijk van de configuratie van de computer duurt het enkele minuten voordat de installatie is voltooid.

    ```shell
    pip install --upgrade azureml-sdk[notebooks]
    ```

1. Installeer deze pakketten als u deze omgeving wilt gebruiken voor de zelfstudies van Azure Machine Learning.

    ```shell
    conda install -y cython matplotlib pandas
    ```

1. Installeer de geautomatiseerde Machine Learning-onderdelen als u deze omgeving wilt gebruiken voor de zelfstudies van Azure Machine Learning.

    ```shell
    pip install --upgrade azureml-sdk[automl]
    ```

> [!IMPORTANT]
> In sommige opdrachtregelprogramma's moet u mogelijk tussen aanhalingstekens als volgt toevoegen:
> *  'azureml-sdk[notebooks]'
> * 'azureml-sdk[automl]'
>

### <a name='sdk-create'></a> Een werkruimte maken met de SDK

Maak uw werkruimte in een Jupyter Notebook met behulp van de Python SDK.

1. Maak en/of schakel naar de map die u wilt gebruiken voor de quickstart en zelfstudies.

1. Als u Jupyter Notebook wilt starten, voert u de volgende opdracht in:

    ```shell
    jupyter notebook
    ```

1. Maak een nieuw notitieblok met behulp van de standaard-`Python 3`-kernel in het browservenster. 

1. Geef de SDK-versie weer door de volgende Python-code in een notitieblokcel te typen en uit te voeren:

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=import)]

1. Zoek een waarde voor de `<azure-subscription-id>`-parameter in de [-abonnementenlijst in Azure Portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). U kunt elk abonnement gebruiken waarvoor u de rol eigenaar of bijdrager hebt.

   ```python
   from azureml.core import Workspace
   ws = Workspace.create(name='myworkspace',
                         subscription_id='<azure-subscription-id>', 
                         resource_group='myresourcegroup',
                         create_resource_group=True,
                         location='eastus2' 
                        )
   ```

   Als u de code uitvoert, wordt u mogelijk gevraagd u aan te melden bij uw Azure-account. Nadat u zich hebt aangemeld, wordt het verificatietoken opgeslagen in de lokale cache.

1. U kunt de details van de werkruimte, zoals de bijbehorende opslag, containerregister en sleutelkluis, weergeven door de volgende code in te voeren:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=getDetails)]


### <a name="write-a-configuration-file"></a>Een configuratiebestand schrijven

Sla de details van uw werkruimte in een configuratiebestand op in de huidige map. Dit bestand wordt aangeroepen *aml_config/config.json*.  

Dit configuratiebestand van de werkruimte maakt het eenvoudig om dezelfde werkruimte later te laden. U kunt deze laden met andere notitieblokken en -scripts in dezelfde map of in een submap met de code `ws=Workspace.from_config()` . 

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=writeConfig)]

Door deze `write_config()`-API aan te roepen, wordt het configuratiebestand in de huidige map gemaakt. Het bestand *config.json* bevat het volgende:

```json
{
    "subscription_id": "<azure-subscription-id>",
    "resource_group": "myresourcegroup",
    "workspace_name": "myworkspace"
}
```

> [!TIP]
> Voor het gebruik van uw werkruimte in de Python-scripts of Jupyter-Notebooks zich in andere directory's, moet u dit bestand kopiëren naar die map. Het bestand kan worden in dezelfde map, een submap met de naam *aml_config*, of in een bovenliggende map.

## <a name="resource-manager-template"></a>Resource manager-sjabloon

Zie voor informatie over het maken van een werkruimte met een sjabloon [maken van een werkruimte van Azure Machine Learning-service met behulp van een sjabloon](how-to-create-workspace-template.md)

## <a name="cli"></a>CLI

Zie voor informatie over het maken van een werkruimte met de CLI [gebruikt u de CLI-extensie voor Azure Machine Learning-service](reference-azure-machine-learning-cli.md).

## <a name="clean-up-resources"></a>Resources opschonen 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

* Ongeacht hoe het is gemaakt, vindt u uw werkruimte in de [Azure-portal](https://portal.azure.com/).  Zie [weergeven van een werkruimte](how-to-manage-workspace.md#view) voor meer informatie.

* Probeer uit uw werkruimte met deze snelstartgidsen en zelfstudies.

    * Quickstart: [Jupyter-notebook in de cloud uitvoeren](quickstart-run-cloud-notebook.md).
    * Quickstart: [Jupyter-notebook uitvoert op uw eigen server](quickstart-run-local-notebook.md).
    * Tweedelige zelfstudie: [Train](tutorial-train-models-with-aml.md) en [implementeren](tutorial-deploy-models-with-aml.md) de modus van een installatiekopie-classificatie.
    * Tweedelige zelfstudie: [Gegevens voorbereiden](tutorial-data-prep.md) en [geautomatiseerde machine learning gebruiken](tutorial-auto-train-models.md) aan het bouwen van een regressiemodel.

* Meer informatie over het [een ontwikkelomgeving configureren](how-to-configure-environment.md).

* Meer informatie over de [Azure Machine Learning-SDK voor Python](https://aka.ms/aml-sdk).

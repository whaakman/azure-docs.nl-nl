---
title: Een werkruimte maken
titleSuffix: Azure Machine Learning service
description: Gebruik de Azure Portal, de SDK, een sjabloon of de CLI om uw Azure Machine Learning service-werk ruimte te maken. Deze werk ruimte biedt een centrale locatie voor het werken met alle artefacten die u maakt wanneer u Azure Machine Learning-service gebruikt.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.author: sgilley
author: sdgilley
ms.date: 05/21/2019
ms.openlocfilehash: 4d689b51a53a27a0e85a52724752d959c4c2506d
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2019
ms.locfileid: "68775053"
---
# <a name="create-an-azure-machine-learning-service-workspace"></a>Een Azure Machine Learning service-werk ruimte maken

Als u Azure Machine Learning-service wilt gebruiken, hebt u een [**Azure machine learning service-werk ruimte**](concept-workspace.md)nodig.  Deze werk ruimte is de resource op het hoogste niveau voor de service en biedt u een centrale locatie voor het werken met alle artefacten die u maakt. 

In dit artikel leert u hoe u een werk ruimte maakt met behulp van een van de volgende methoden: 
* De [Azure Portal](#portal) -interface
* De [Azure machine learning SDK voor python](#sdk)
* Een Azure Resource Manager sjabloon
* De [Azure machine learning cli](#cli)

De werk ruimte die u maakt met behulp van de hier beschreven stappen, kunnen worden gebruikt als een vereiste voor andere zelf studies en artikelen met instructies.

Als u een script wilt gebruiken om automatische machine learning in te stellen in een lokale python-omgeving raadpleegt u de [Azure-MachineLearningNotebooks github](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning) voor instructies.  

Wanneer u een werk ruimte maakt, worden de volgende Azure-resources automatisch toegevoegd (als deze regionaal beschikbaar zijn):
 
- [Azure container Registry](https://azure.microsoft.com/services/container-registry/): Om de kosten te minimaliseren, wordt ACR **Lazy geladen** totdat implementatie installatie kopieën zijn gemaakt.
- [Azure Storage](https://azure.microsoft.com/services/storage/)
- [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 
- [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)

>[!Note]
>Net als bij andere Azure-services gelden er bepaalde limieten en quota voor Machine Learning. [Meer informatie over quota's en hoe u meer kunt aanvragen.](how-to-manage-quotas.md)


## <a name="prerequisites"></a>Vereisten
Een werkruimte te maken, moet u een Azure-abonnement. Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

## <a name="portal"></a>Azure Portal

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

Ongeacht hoe de app is gemaakt, kunt u uw werk ruimte weer geven in de [Azure Portal](https://portal.azure.com/).  Zie [een werk ruimte weer geven](how-to-manage-workspace.md#view) voor meer informatie.

## <a name="sdk"></a>Python-SDK

Maak uw werk ruimte met behulp van de python-SDK. Eerst moet u de SDK installeren.

> [!IMPORTANT]
> Sla de installatie van de SDK over als u een Azure Data Science Virtual Machine of Azure Databricks gebruikt.
> * Op Data Science Virtual Machines die zijn gemaakt na 27 september 2018 is de Python SDK al geïnstalleerd. Sla de installatie over en begin met [het maken van een werk ruimte met de SDK](#sdk-create).
> * Gebruik in de Azure Databricks-omgeving in plaats daarvan de [Databricks-installatiestappen](how-to-configure-environment.md#azure-databricks).

>[!NOTE]
> Gebruik deze instructies voor het installeren en gebruiken van de SDK vanaf uw lokale computer. Als u Jupyter op een externe virtuele machine wilt gebruiken, stelt u een extern bureau blad-of X-terminal sessie in.

Voordat u de SDK installeert, raden we u aan om een geïsoleerde omgeving voor Python te maken. In dit artikel wordt [Miniconda](https://docs.conda.io/en/latest/miniconda.html) gebruikt, maar u kunt ook gebruikmaken van volledig geïnstalleerde [Anaconda](https://www.anaconda.com/) of [Python virtualenv](https://virtualenv.pypa.io/en/stable/).

Met de instructies in dit artikel worden alle pakketten geïnstalleerd die u nodig hebt om de notitie blokken Snelstartgids en zelf studie uit te voeren.  Voor andere voorbeeldnotebooks moeten mogelijk extra onderdelen worden geïnstalleerd.  Zie [De Azure Machine Learning-SDK voor Python installeren](https://docs.microsoft.com/python/api/overview/azure/ml/install) voor meer informatie over deze onderdelen.

### <a name="install-miniconda"></a>Miniconda installeren

[Download en installeer Miniconda](https://docs.conda.io/en/latest/miniconda.html). Selecteer de versie Python 3.7 of hoger voor installatie. Selecteer niet Python 2.x.  

### <a name="create-an-isolated-python-environment"></a>Een geïsoleerde omgeving voor Python maken

1. Open Anaconda prompt en maak een nieuwe Conda-omgeving met de naam *myenv* en installeer Python 3.6.5. Azure Machine Learning SDK werkt met Python 3.5.2 of hoger, maar de geautomatiseerde Machine Learning-onderdelen zijn niet volledig functioneel in Python 3.7.  Het duurt enkele minuten om de omgeving te maken terwijl onderdelen en pakketten worden gedownload. 

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
> In sommige opdracht regel Programma's moet u mogelijk aanhalings tekens als volgt toevoegen:
> *  'azureml-sdk[notebooks]'
> * 'azureml-sdk[automl]'
>

### <a name='sdk-create'></a>Een werk ruimte maken met de SDK

Maak uw werkruimte in een Jupyter Notebook met behulp van de Python SDK.

1. Maak en/of schakel naar de map die u wilt gebruiken voor de quickstart en zelfstudies.

1. Als u Jupyter Notebook wilt starten, voert u de volgende opdracht in:

    ```shell
    jupyter notebook
    ```

1. Maak een nieuw notitieblok met behulp van de standaard-`Python 3`-kernel in het browservenster. 

1. Geef de SDK-versie weer door de volgende Python-code in een notitieblokcel te typen en uit te voeren:

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=import)]

1. Zoek een waarde voor de `<azure-subscription-id>`-parameter in de [-abonnementenlijst in Azure Portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). U kunt elk abonnement gebruiken waarvoor u de rol eigenaar of bijdrager hebt. Zie [toegang tot een Azure machine learning werkruimte artikel beheren](how-to-assign-roles.md) voor meer informatie over rollen.

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

Sla de details van uw werkruimte in een configuratiebestand op in de huidige map. Dit bestand heet *. azureml/config. json*.  

Dit configuratiebestand van de werkruimte maakt het eenvoudig om dezelfde werkruimte later te laden. U kunt deze met behulp van de code `ws=Workspace.from_config()` laden met andere notitie blokken en scripts in dezelfde map of een submap. 

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=writeConfig)]

Door deze `write_config()`-API aan te roepen, wordt het configuratiebestand in de huidige map gemaakt. Het bestand *. azureml/config. json* bevat het volgende:

```json
{
    "subscription_id": "<azure-subscription-id>",
    "resource_group": "myresourcegroup",
    "workspace_name": "myworkspace"
}
```

> [!TIP]
> Als u uw werk ruimte wilt gebruiken in python-scripts of Jupyter-notebooks die zich in andere directory's bevinden, kopieert u dit bestand naar die map. Het bestand kan zich in dezelfde map bevindt, een submap met de naam *. azureml*of in een bovenliggende map.

## <a name="resource-manager-template"></a>Resource Manager-sjabloon

Zie een [Azure machine learning service-werk ruimte maken met behulp van een sjabloon](how-to-create-workspace-template.md) om een werk ruimte met een sjabloon te maken

<a name="cli"></a>
## <a name="command-line-interface"></a>Opdracht regel interface

Als u een werk ruimte met de CLI wilt maken, raadpleegt [u de CLI-uitbrei ding voor de Azure machine learning-service gebruiken](reference-azure-machine-learning-cli.md).

## <a name="clean-up-resources"></a>Resources opschonen 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

* Ongeacht hoe de app is gemaakt, kunt u uw werk ruimte weer geven in de [Azure Portal](https://portal.azure.com/).  Zie [een werk ruimte weer geven](how-to-manage-workspace.md#view) voor meer informatie.

* Probeer uw werk ruimte uit met deze zelf studies.

    * Zelf studie met twee delen: [Stel de omgeving en werk ruimte](tutorial-1st-experiment-sdk-setup.md) in en [Train uw eerste model](tutorial-1st-experiment-sdk-train.md).
    * Zelf studie met twee delen: [Train](tutorial-train-models-with-aml.md) en [Implementeer](tutorial-deploy-models-with-aml.md) een afbeeldings classificatie modus.
    * Zelf studie met twee delen: [Gegevens voorbereiden](tutorial-data-prep.md) en [geautomatiseerd machine learning gebruiken](tutorial-auto-train-models.md) om een regressie model samen te stellen.

* Meer informatie over het [configureren van een ontwikkel omgeving](how-to-configure-environment.md).

* Meer informatie over de [Azure machine learning SDK voor python](https://aka.ms/aml-sdk).

---
title: Een ontwikkelomgeving configureren voor Azure Machine Learning | Microsoft Docs
description: Informatie over het configureren van een ontwikkelomgeving bij het werken met de Azure Machine Learning-service. In dit document leert u hoe u Conda-omgevingen gebruiken, configuratiebestanden maken en configureren van Jupyter Notebooks, Azure Notebooks, IDE's, code-editors en de virtuele Machine voor Datatechnologie.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.component: core
ms.reviewer: larryfr
manager: cgronlun
ms.topic: conceptual
ms.date: 10/24/2018
ms.openlocfilehash: 6c2d5a776f603161ef730028168b91844c120aec
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50158989"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Een ontwikkelomgeving configureren voor Azure Machine Learning

In dit artikel leert u hoe u het configureren van een ontwikkelomgeving, zodat het werken met de Azure Machine Learning-service, met inbegrip van:

- Het maken van een configuratiebestand dat uw omgeving worden gekoppeld aan een werkruimte van Azure Machine Learning-service.
- Het configureren van de volgende ontwikkelomgevingen:
  - Jupyter Notebooks op uw computer
  - Visual Studio Code
  - Aangepaste code-editor
- Over het instellen van een [conda virtuele omgeving](https://conda.io/docs/user-guide/tasks/manage-environments.html) en deze gebruiken voor Azure Machine Learning. We adviseren Zorgcontinuüm Anaconda gebruiken voor het isoleren van uw werkomgeving om conflicten tussen pakketten afhankelijkheid te voorkomen.

## <a name="prerequisites"></a>Vereisten

- Instellen van een werkruimte van Azure Machine Learning-service. Volg de stappen in [aan de slag met Azure Machine Learning-service](quickstart-get-started.md).
- Installeer de [Zorgcontinuüm Anaconda](https://www.anaconda.com/download/) of [Miniconda](https://conda.io/miniconda.html) Pakketbeheer.
- Als u Visual Studio Code, krijgt de [Python-extensie](https://code.visualstudio.com/docs/python/python-tutorial).

> [!NOTE]
> U kunt de shell-opdrachten die in dit artikel worden weergegeven met behulp van bash (in Linux en Mac OS) testen of opdrachtprompt (in Windows).

## <a name="create-a-workspace-configuration-file"></a>Het configuratiebestand van een werkruimte maken

De Azure Machine Learning-SDK maakt gebruik van het configuratiebestand van de werkruimte om te communiceren met uw werkruimte van Azure Machine Learning-service.

- Voor het maken van het configuratiebestand, voltooi de [Azure Machine Learning-snelstartgids](quickstart-get-started.md).
  - De Quick Start-proces maakt een `config.json` bestand in Azure-notitieblokken. Dit bestand bevat de configuratiegegevens voor uw werkruimte.
  - Download of kopieer de `config.json` in dezelfde map als de scripts of laptops die ernaar verwijzen.

- U kunt het bestand ook handmatig maken door de volgende stappen:

    1. Open de werkruimte in de [Azure-portal](https://portal.azure.com). Kopieer de __Werkruimtenaam__, __resourcegroep__, en __abonnements-ID__. Deze waarden worden gebruikt om te maken van het configuratiebestand.
        ![Azure Portal](./media/how-to-configure-environment/configure.png)

    1. Het bestand met de volgende code voor Python maken en zorg ervoor dat u de code uitvoeren in dezelfde map als de scripts of laptops die verwijzen naar de werkruimte:

        ```python
        from azureml.core import Workspace

        subscription_id ='<subscription-id>'
        resource_group ='<resource-group>'
        workspace_name = '<workspace-name>'

        try:
           ws = Workspace(subscription_id = subscription_id, resource_group = resource_group, workspace_name = workspace_name)
           ws.write_config()
           print('Library configuration succeeded')
        except:
           print('Workspace not found')
        ```
        De code schrijft het volgende `aml_config/config.json` bestand:

        ```json
        {
        "subscription_id": "<subscription-id>",
        "resource_group": "<resource-group>",
        "workspace_name": "<workspace-name>"
        }
        ```
        U kunt kopiëren de `aml_config` directory of alleen `config.json` -bestand in een andere adreslijst die verwijst naar de werkruimte.

       > [!NOTE]
       > Andere scripts of notitieblokken in dezelfde map of lager laden van de werkruimte met `ws=Workspace.from_config()`.

## <a name="azure-notebooks-and-data-science-virtual-machines"></a>Azure-notitieblokken en -virtuele Machines voor Datatechnologie

Azure-notitieblokken en Azure Data Science Virtual Machines (Dsvm) zijn geconfigureerd voor samenwerking met de Azure Machine Learning-service. Deze omgevingen zijn vereiste onderdelen, zoals de Azure Machine Learning-SDK.

- Azure-laptops is een Jupyter-Notebook-service in de Azure-cloud.
- De Data Science Virtual Machine is een aangepaste virtuele machine (VM)-installatiekopie is ontworpen voor data science werk. Het bevat:
  - Populaire hulpprogramma 's
  - Geïntegreerde ontwikkelingsomgevingen (IDE's)
  - Pakketten, zoals Jupyter Notebooks, PyCharm en Tensorflow
- U moet nog steeds een configuratiebestand voor de werkruimte te gebruiken van deze omgevingen.

Zie voor een voorbeeld van het gebruik van Azure-notitieblokken met de service Azure Machine Learning, [aan de slag met Azure Machine Learning-service](quickstart-get-started.md).

Zie voor meer informatie over de virtuele Machines voor Datatechnologie, [virtuele Machines voor Datatechnologie](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a name="configure-jupyter-notebooks-on-your-computer"></a>Jupyter-Notebooks op uw computer configureren

1. Open een opdrachtprompt of de shell.

1. Een conda-omgeving maken met de volgende opdrachten:

    ```shell
    # create a new conda environment with Python 3.6, numpy, and cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the conda environment
    conda activate myenv

    # On Mac OS run
    source activate myenv
    ```

    Het duurt enkele minuten om te maken van de omgeving als Python 3.6 en andere onderdelen moeten worden gedownload.

1. Installeer de SDK van Azure Machine Learning met extra's laptop en de SDK van de voorbereiding van gegevens met behulp van de volgende opdracht uit:

     ```shell
    pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep
    ```

   Hier ziet u de Python-referentiedocumenten voor klassen en methoden in de volgende SDK's:
   + [Azure Machine Learning-SDK voor Python](https://aka.ms/aml-sdk)
   + [Azure Machine Learning Data Prep SDK](https://aka.ms/data-prep-sdk)

   > [!NOTE]
   > Als u een bericht dat krijgt `PyYAML` kan niet worden verwijderd, gebruik de volgende opdracht uit in plaats daarvan:
   >
   > `pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep --ignore-installed PyYAML`

   Het duurt enkele minuten de SDK te installeren.

1. Voor uw machine learning experimenten-pakketten installeren. Gebruik de volgende opdracht en vervangen `<new package>` aan het pakket dat u wilt installeren:

    ```shell
    conda install <new package>
    ```

1. Een conda-bewuste Jupyter-Notebook-server installeren en inschakelen van experiment widgets (uitvoeren om informatie te bekijken). Gebruik de volgende opdrachten:

    ```shell
    # install Jupyter
    conda install nb_conda

    # install experiment widget
    jupyter nbextension install --py --user azureml.train.widgets

    # enable experiment widget
    jupyter nbextension enable --py --user azureml.train.widgets
    ```

1. Jupyter Notebook openen met de volgende opdracht:

    ```shell
    jupyter notebook
    ```

1. Open een nieuwe notebook, 'myenv' als de kernel selecteren en vervolgens te valideren dat u Azure Machine Learning SDK is geïnstalleerd hebt. Voer de volgende opdracht in een cel laptop:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

## <a name="configure-visual-studio-code"></a>Visual Studio Code configureren

1. Open een opdrachtprompt of de shell.

1. Een conda-omgeving maken met de volgende opdrachten:

    ```shell
    # create a new conda environment with Python 3.6, numpy and cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the conda environment
    conda activate myenv

    # If you are running Mac OS you should run
    source activate myenv
    ```

1. De SDK van Azure Machine Learning en Data Preparation SDK installeren met de volgende opdracht:

    ```shell
    pip install --upgrade azureml-sdk[automl] azureml-dataprep
    ```

1. Installeer het Visual Studio code-hulpprogramma's voor AI-extensie. Zie [hulpprogramma's voor AI](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai).

1. Voor uw machine learning experimenten-pakketten installeren. Gebruik de volgende opdracht, vervangt `<new package>` aan het pakket dat u wilt installeren:

    ```shell
    conda install <new package>
    ```

1. Open Visual Studio Code, en gebruik vervolgens **CTRL-SHIFT-P** (in Windows) of **opdracht-SHIFT-P** (in Mac OS) om op te halen de **Command Palette**. Voer _Python: Select Interpreter_ en selecteer het conda-omgeving die u hebt gemaakt.

   > [!NOTE]
   > Visual Studio Code is automatisch op de hoogte van conda-omgevingen op uw computer. Zie voor meer informatie, [documentatie voor Visual Studio code](https://code.visualstudio.com/docs/python/environments#_conda-environments).

1. De configuratie valideren met behulp van Visual Studio Code naar een nieuwe Python-scriptbestand maken met de volgende code en voer het vervolgens uit:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

## <a name="configure-a-custom-code-editor"></a>Configureren van een aangepaste code-editor

Met de SDK van Azure Machine Learning kunt u een code-editor van uw keuze.

1. Uw conda-omgeving maken, zoals beschreven in stap 2 van [Visual Studio-Code configureren](#configure-visual-studio-code) hierboven.
1. Volg de instructies voor elke editor die u wilt de conda-omgeving gebruiken. U kunt bijvoorbeeld als volgt de [PyCharm instructies](https://www.jetbrains.com/help/pycharm/2018.2/conda-support-creating-conda-virtual-environment.html).

## <a name="next-steps"></a>Volgende stappen

- [Een model trainen op Azure Machine Learning met de MNIST-gegevensset](tutorial-train-models-with-aml.md)

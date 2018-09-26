---
title: Een ontwikkelomgeving configureren voor Azure Machine Learning | Microsoft Docs
description: Informatie over het configureren van een ontwikkelomgeving bij het werken met de Azure Machine Learning-service. In dit document leert u hoe u Conda-omgevingen gebruiken, configuratiebestanden maken en configureren van Jupyter Notebooks, Azure Notebooks, IDE's, code-editors en de virtuele Machine voor Datatechnologie.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.reviewer: larryfr
manager: cgronlun
ms.topic: conceptual
ms.date: 8/6/2018
ms.openlocfilehash: 87b3bc4128d800e4f76d71dc5f9d081dffa0e3a7
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47163440"
---
# <a name="how-to-configure-a-development-environment-for-the-azure-machine-learning-service"></a>Een ontwikkelomgeving voor de Azure Machine Learning-service configureren

Informatie over het configureren van uw ontwikkelomgeving, zodat het werken met de Azure Machine Learning-service. U leert hoe u een configuratiebestand dat wordt gekoppeld aan uw omgeving met een Azure Machine Learning-werkruimte te maken. Ook leert u hoe u de volgende ontwikkelomgevingen configureren:

* Jupyter-Notebooks in uw eigen computer
* Visual Studio Code
* Code-editor van uw keuze

De aanbevolen aanpak is het gebruik van Zorgcontinuüm Anaconda [conda virtuele omgevingen](https://conda.io/docs/user-guide/tasks/manage-environments.html) voor het isoleren van uw werkomgeving om het voorkomen van conflicten tussen pakketten afhankelijkheid. In dit artikel bevat instructies voor het instellen van een conda-omgeving en deze gebruiken voor Azure Machine Learning.


## <a name="prerequisites"></a>Vereisten

* Een werkruimte van Azure Machine Learning-service. Als u wilt maken van een gebruikt u de stappen in de [aan de slag met Azure Machine Learning-service](quickstart-get-started.md) document.

* [Anaconda zorgcontinuüm](https://www.anaconda.com/download/) of [Miniconda](https://conda.io/miniconda.html) Pakketbeheer.

 * Voor Visual Studio Code-omgeving, de [Python-extensie](https://code.visualstudio.com/docs/python/python-tutorial).

## <a name="create-workspace-configuration-file"></a>Werkruimte-configuratiebestand maken

Het configuratiebestand van de werkruimte wordt gebruikt door de SDK om te communiceren met uw werkruimte van Azure Machine Learning-service.  Er zijn twee manieren om dit bestand:

* Voltooi de [snelstartgids](quickstart-get-started.md) een werkruimte en de configuratie-bestand te maken. Het bestand `config.json` wordt voor u gemaakt in Azure-notitieblokken.  Dit bestand bevat de configuratiegegevens voor uw werkruimte.  Download of kopieer deze in dezelfde map als de scripts of laptops die ernaar verwijzen.


* Het configuratiebestand zelf maken met de volgende stappen uit:

    1. Open de werkruimte in de [Azure-portal](https://portal.azure.com). Kopieer de __Werkruimtenaam__, __resourcegroep__, en __abonnements-ID__. Deze waarden worden gebruikt om te maken van het configuratiebestand.

        ![Azure Portal](./media/how-to-configure-environment/configure.png) 
    
    1. Maak het bestand met deze code voor Python. Voer de code uit in dezelfde map als de scripts of laptops die verwijzen naar de werkruimte:
        ```
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
        Hiermee schrijft u de volgende `aml_config/config.json` bestand: 
    
        ```json
        {
        "subscription_id": "<subscription-id>",
        "resource_group": "<resource-group>",
        "workspace_name": "<workspace-name>"
        }
        ```
        U kunt kopiëren de `aml_config` directory of alleen `config.json` -bestand in een andere adreslijst die verwijst naar de werkruimte.

>[!NOTE] 
>Andere scripts of notitieblokken in dezelfde map of onder wordt de werkruimte met geladen `ws=Workspace.from_config()`

## <a name="azure-notebooks-and-data-science-virtual-machine"></a>Azure-notitieblokken en -virtuele Machine voor Datatechnologie

Azure-notitieblokken en Azure Data Science Virtual Machines (DSVM) zijn vooraf geconfigureerd voor samenwerking met de Azure Machine Learning-service. Vereiste onderdelen, zoals de SDK van Azure Machine Learning, zijn vooraf geïnstalleerd in deze omgevingen.

Azure-laptops is een Jupyter-Notebook-service in de Azure-cloud. De Data Science Virtual Machine is een VM-installatiekopie die is vooraf geconfigureerd voor data science werk. De virtuele machine bevat populaire hulpprogramma's, IDE's en pakketten, zoals Jupyter Notebooks, PyCharm en Tensorflow.

U moet wel een configuratiebestand voor de werkruimte te gebruiken van deze omgevingen.

Zie voor meer informatie over de virtuele Machines voor Datatechnologie, [virtuele Machines voor Datatechnologie](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/) documentatie.

Zie voor een voorbeeld van het gebruik van Azure-notitieblokken met de Azure Machine Learning-service, de [aan de slag met Azure Machine Learning-service](quickstart-get-started.md) document.

## <a name="configure-jupyter-notebooks-on-your-own-computer"></a>Jupyter-Notebooks op uw eigen computer configureren

1. Open een opdrachtprompt of de shell.

2. Gebruik de volgende opdrachten voor het maken van een conda-omgeving:

    ```shell
    # create a new conda environment with Python 3.6, numpy and cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the conda environment
    conda activate myenv

    # If you are running Mac OS you should run
    source activate myenv
    ```

    Het kan enkele minuten om te maken van de omgeving, duren als Python 3.6 en andere onderdelen mogelijk moeten worden gedownload.

3. Voor het installeren van Azure Machine Learning-SDK met notebook extra's, gebruik de volgende opdracht:

     ```shell
    pip install --upgrade azureml-sdk[notebooks,automl]
    ```

    Het kan enkele minuten voor het installeren van de SDK duren.

4. Gebruik de volgende opdracht om pakketten te installeren voor uw machine learning experimenten, en vervang `<new package>` aan het pakket dat u wilt installeren:

    ```shell
    conda install <new package>
    ```

5. Als u wilt een conda-bewuste Jupyter-Notebook-server installeren en inschakelen van experiment widgets (uitvoeren om informatie te bekijken), gebruik de volgende opdrachten:

    ```shell
    # install Jupyter 
    conda install nb_conda

    # install experiment widget
    jupyter nbextension install --py --user azureml.train.widgets

    # enable experiment widget
    jupyter nbextension enable --py --user azureml.train.widgets
    ```

6. Als u wilt Jupyter-Notebook start, moet u de volgende opdracht gebruiken:

    ```shell
    jupyter notebook
    ```

7. Een nieuwe notebook openen en selecteer 'myenv' als de kernel. Controleer vervolgens of dat u Azure Machine Learning SDK is geïnstalleerd door het uitvoeren van de volgende opdracht in een cel notebook hebt:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

## <a name="configure-visual-studio-code"></a>Visual Studio Code configureren

1. Open een opdrachtprompt of de shell.

2. Gebruik de volgende opdrachten voor het maken van een conda-omgeving:

    ```shell
    # create a new conda environment with Python 3.6, numpy and cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the conda environment
    conda activate myenv

    # If you are running Mac OS you should run
    source activate myenv
    ```

2. Gebruik de volgende opdracht voor het installeren van de SDK van Azure Machine Learning:
 
    ```shell
    pip install --upgrade azureml-sdk[automl]
    ```

4. Zie de Visual Studio marketplace-vermelding voor installeren van de Visual Studio code-hulpprogramma's voor AI [hulpprogramma's voor AI](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai). 

5. Gebruik de volgende opdracht om pakketten te installeren voor uw machine learning experimenten, en vervang `<new package>` aan het pakket dat u wilt installeren:

    ```shell
    conda install <new package>
    ```

6. Start Visual Studio Code, en gebruik vervolgens __CTRL-SHIFT-P__ om op te halen de __Command Palette__. Voer *Python: Select Interpreter*, en selecteer het conda-omgeving die u hebt gemaakt.

    > [!NOTE]
    > Visual Studio Code is automatisch op de hoogte van conda-omgevingen op uw computer. Zie voor meer informatie, [documentatie voor Visual Studio code](https://code.visualstudio.com/docs/python/environments#_conda-environments).

7. Voor het valideren van de configuratie van de Visual Studio Code te gebruiken op een nieuwe Python-scriptbestand maken met de volgende code en voer het vervolgens uit:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

## <a name="configure-code-editor-of-your-choice"></a>Code-editor van uw keuze configureren

Voor het gebruik van een aangepaste code-editor met Azure Machine Learning-SDK, eerst maken conda-omgeving, zoals hierboven beschreven. Volg de instructies voor elke editor die u wilt de conda-omgeving gebruiken. Bijvoorbeeld, de instructies voor het PyCharm bevinden zich op [ https://www.jetbrains.com/help/pycharm/2018.2/conda-support-creating-conda-virtual-environment.html ](https://www.jetbrains.com/help/pycharm/2018.2/conda-support-creating-conda-virtual-environment.html).
 
## <a name="next-steps"></a>Volgende stappen

* [Een model trainen op Azure Machine Learning met de MNIST-gegevensset](tutorial-train-models-with-aml.md)

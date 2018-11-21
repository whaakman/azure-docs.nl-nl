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
ms.date: 11/6/2018
ms.openlocfilehash: fa70e0dfa1f131e38e43faa3d80497d50a52e135
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52275211"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Een ontwikkelomgeving configureren voor Azure Machine Learning

In dit document leert u hoe u een ontwikkelomgeving, zodat het werken met de Azure Machine Learning-service configureren. De Azure Machine Learning-service is een platform-agnostische. De enige vereisten voor uw ontwikkelomgeving zijn __Python 3__, __Conda__ (voor geïsoleerde omgevingen), en een configuratiebestand met gegevens over uw Azure Machine Learning-werkruimte.

Dit document is gericht op de volgende specifieke omgevingen en hulpprogramma's:

* [Azure-notitieblokken](#aznotebooks): Jupyter-Notebooks voor A-service die wordt gehost in de Azure-cloud. Het is __het gemakkelijkst__ manier om te beginnen, zoals de Azure Machine Learning-SDK is al geïnstalleerd.
* [De Data Science Virtual Machine](#dsvm): een virtuele machine in de Azure-cloud die is __ontworpen voor data science werk__. Python 3, Conda, Jupyter-Notebooks en de SDK van Azure Machine Learning zijn al geïnstalleerd. De virtuele machine wordt geleverd met populaire ML-frameworks, hulpprogramma's en editors voor ML-oplossingen ontwikkelen. Is dit waarschijnlijk __de meest volledige__ ontwikkelomgeving voor ML op het Azure-platform.
* [Jupyter-Notebooks](#jupyter): als u al Jupyter Notebooks, de SDK heeft enkele extra's die u moet installeren.
* [Visual Studio Code](#vscode): als u Visual Studio Code, er zijn enkele nuttige extensies die u kunt installeren.

Als u al een Python 3-omgeving hebt, of alleen de basisstappen voor het installeren van de SDK wilt, Zie de [lokale computer](#local) sectie.

## <a name="prerequisites"></a>Vereisten

- Een werkruimte van Azure Machine Learning-service. Volg de stappen in [aan de slag met Azure Machine Learning-service](quickstart-get-started.md) een te maken.

- Een van beide de [Zorgcontinuüm Anaconda](https://www.anaconda.com/download/) of [Miniconda](https://conda.io/miniconda.html) Pakketbeheer.

    > [!IMPORTANT]
    > Anaconda en Miniconda zijn niet vereist wanneer met behulp van Azure-Notebooks.

- Op Linux of Mac OS moet u de bash-shell.

    > [!TIP]
    > Als u in Linux of Mac OS werkt en een shell dan bash (bijvoorbeeld zsh gebruikt) ontvangt u mogelijk fouten bij het uitvoeren van sommige opdrachten. U kunt dit probleem omzeilen, gebruikt u de `bash` opdracht voor het starten van een nieuwe bash-shell en voer de opdrachten er uit.

- Op Windows moet u de opdrachtprompt of Anaconda-prompt (geïnstalleerd door Anaconda en Miniconda).

## <a id="anotebooks"></a>Azure-laptops

[Azure-notitieblokken](https://notebooks.azure.com) (preview) is een interactieve ontwikkelomgeving in de Azure-cloud. Het is __het gemakkelijkst__ manier aan de slag met Azure Machine Learning-ontwikkeling.

* De SDK van Azure Machine Learning is __al geïnstalleerd__.
* Na het maken van een werkruimte van Azure Machine Learning-service in Azure portal, kunt u klikt op een knop automatisch configureren aan de Notebook van Azure-omgeving werkt met de werkruimte.

Om te beginnen met ontwikkelen met Azure-laptops, volgt u de [aan de slag met Azure Machine Learning-service](quickstart-get-started.md) document.

## <a id="dsvm"></a>Virtuele Machine voor Datatechnologie

De Data Science Virtual Machine (DSVM) is de installatiekopie van een aangepaste virtuele machine (VM) **ontworpen voor data science werk**. Het bevat:

  - Populaire data science-hulpprogramma 's
  - Geïntegreerde ontwikkelingsomgevingen (IDE's), zoals PyCharm en RStudio
  - Pakketten, zoals Jupyter Notebooks en Tensorflow

De Azure Machine Learning-SDK werkt op de Ubuntu- of Windows-versie van de DSVM. Voor het gebruik van DSVM als een ontwikkelomgeving, de volgende stappen uit:

1. Volg de stappen in een van de volgende documenten voor het maken van een Data Science Virtual Machine:

    * [Een Ubuntu Data Science Virtual machine maken](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro)
    * [Maken van een Windows Data Science Virtual Machine](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/provision-vm)

1. De SDK van Azure Machine Learning is **al geïnstalleerd** op de DSVM. Als u de Conda-omgeving met de SDK, gebruikt u een van de volgende opdrachten:

    * Op __Ubuntu__ DSVM, gebruikt u deze opdracht:

        ```shell
        conda activate py36
        ```

    * Op __Windows__ DSVM, gebruikt u deze opdracht:

        ```shell
        conda activate AzureML
        ```

1. Als u wilt controleren of u kunt toegang krijgen tot de SDK en controleer de versie, gebruik de volgende Python-code:

    ```python
    import azureml.core
    print(azureml.core.VERSION)
    ```

1. Zie configureren van de DSVM voor het gebruik van de werkruimte van uw Azure Machine Learning-service de [werkruimte configureren](#workspace) sectie.

Zie voor meer informatie over de virtuele Machines voor Datatechnologie, [virtuele Machines voor Datatechnologie](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a id="local"></a>Lokale computer

Wanneer u een lokale computer (die mogelijk ook een externe virtuele machine), gebruikt u de volgende stappen uit om te maken van een conda-omgeving en de SDK installeren:

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

   > [!NOTE]
   > Als u een bericht dat krijgt `PyYAML` kan niet worden verwijderd, gebruik de volgende opdracht uit in plaats daarvan:
   >
   > `pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep --ignore-installed PyYAML`

   Het duurt enkele minuten de SDK te installeren.

1. Voor uw machine learning experimenten-pakketten installeren. Gebruik de volgende opdracht en vervangen `<new package>` aan het pakket dat u wilt installeren:

    ```shell
    conda install <new package>
    ```

1. Als u wilt controleren of de SDK is geïnstalleerd, de volgende Python-code:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

### <a id="jupyter"></a>Jupyter-Notebooks

Jupyter-notitieblokken maken deel uit van de [Jupyter Project](https://jupyter.org/). Ze bieden een interactieve ervaring met codering waar het maken van documenten die live code met verhalende tekst en afbeeldingen combineren. Jupyter-Notebooks zijn ook een uitstekende manier om uw resultaten delen met anderen, zoals u kunt de uitvoer van de Codesecties van uw in het document opslaan. U kunt Jupyter-Notebooks installeren op een aantal verschillende platformen.

De stappen in de [lokale computer](#local) sectie optionele onderdelen installeren voor Jupyter-Notebooks. Zodat deze onderdelen in uw omgeving Jupyter-Notebook gebruikt u de volgende stappen uit:

1. Open een opdrachtprompt of de shell.

1. Als u wilt een conda-bewuste Jupyter-Notebook-server installeren en inschakelen van experiment widgets, gebruik de volgende opdrachten:

    ```shell
    # install Jupyter
    conda install nb_conda

    # install experiment widget
    jupyter nbextension install --py --user azureml.widgets

    # enable experiment widget
    jupyter nbextension enable --py --user azureml.widgets
    ```

1. Jupyter Notebook openen met de volgende opdracht:

    ```shell
    jupyter notebook
    ```

1. Om te controleren dat de SDK door Jupyter-Notebook kunt gebruiken, opent u een nieuwe notebook en selecteer 'myenv' als de kernel. Voer de volgende opdracht uit in een cel laptop:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Als u wilt de Jupyter-Notebook voor het gebruik van de werkruimte van uw Azure Machine Learning-service configureren, Zie de [werkruimte configureren](#workspace) sectie.

### <a id="vscode"></a>Visual Studio Code

Visual Studio Code is een platformoverschrijdende code-editor. Afhankelijk van een lokale installatie van Python 3 en Conda voor Python-ondersteuning, maar het biedt extra hulpmiddelen voor het werken met AI. Het biedt ook ondersteuning voor het selecteren van de Conda-omgeving in de code-editor.

Voor het gebruik van Visual Studio Code voor de ontwikkeling, gebruikt u de volgende stappen uit:

1. Zie voor meer informatie over het gebruik van Visual Studio Code voor Python-ontwikkeling, de [aan de slag met Python vscode](https://code.visualstudio.com/docs/python/python-tutorial) document.

1. Schakel de Conda-omgeving, opent u VS Code en gebruik vervolgens __Ctrl-Shift-P__ (Linux en Windows) of __opdracht-Shift-P__ (Mac) om op te halen de __opdracht palet__. Voer __Python: Select Interpreter__ en selecteer vervolgens de conda-omgeving.

1. Om te valideren dat u de SDK kunt gebruiken, maakt u een nieuwe Python-bestand (.py) met de volgende code. Voer vervolgens het bestand:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Voor het installeren van de Visual Studio code-hulpprogramma's voor AI-extensie, Zie de [hulpprogramma's voor AI](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai) pagina.

    Zie voor meer informatie de [met VS Code-hulpprogramma's voor AI met Azure Machine Learning](how-to-vscode-tools.md) document.

## <a id="workspace"></a>Het configuratiebestand van een werkruimte maken

Het configuratiebestand van de werkruimte is een JSON-document dat de SDK legt uit hoe om te communiceren met uw werkruimte van Azure Machine Learning-service. Het bestand met de naam `config.json` en deze heeft de volgende indeling:

```json
{
    "subscription_id": "<subscription-id>",
    "resource_group": "<resource-group>",
    "workspace_name": "<workspace-name>"
}
```

Dit bestand moet zich in de directory-structuur die uw Python-scripts of Jupyter-Notebooks bevat. Het kan zijn in dezelfde map, een submap met de naam `aml_config`, of in een bovenliggende map.

U kunt dit bestand vanuit uw code gebruiken `ws=Workspace.from_config()`. Deze code wordt de informatie uit het bestand wordt geladen en maakt verbinding met uw werkruimte.

Er zijn drie manieren om te maken van het configuratiebestand:

* Als u volgt de [Azure Machine Learning-snelstartgids](quickstart-get-started.md), een `config.json` bestand wordt gemaakt in uw Azure-notitieblokken-bibliotheek. Dit bestand bevat de configuratiegegevens voor uw werkruimte. U kunt downloaden of kopieer deze `config.json` in andere omgevingen ontwikkeling.

* U kunt **het bestand handmatig aanmaken** met een teksteditor. U vindt de waarden voor die gaan in het configuratiebestand dat door het bezoek aan uw werkruimte in de [Azure-portal](https://portal.azure.com). Kopieer de __Werkruimtenaam__, __resourcegroep__, en __abonnements-ID__ waarden en deze gebruiken in het configuratiebestand.
        ![Azure Portal](./media/how-to-configure-environment/configure.png)

* U kunt **via een programma maken van het bestand**. Het volgende codefragment laat zien hoe u verbinding maken met een werkruimte met de abonnements-ID, resourcegroep en naam van de werkruimte te leveren. Vervolgens wordt opgeslagen in de configuratie van de werkruimte naar bestand:

    ```python
    from azureml.core import Workspace

    subscription_id = '<subscription-id>'
    resource_group  = '<resource-group>'
    workspace_name  = '<workspace-name>'

    try:
        ws = Workspace(subscription_id = subscription_id, resource_group = resource_group, workspace_name = workspace_name)
        ws.write_config()
        print('Library configuration succeeded')
    except:
        print('Workspace not found')
    ```

    Deze code schrijft het configuratiebestand naar de `aml_config/config.json` bestand.

## <a name="next-steps"></a>Volgende stappen

- [Een model trainen op Azure Machine Learning met de MNIST-gegevensset](tutorial-train-models-with-aml.md)
- [Azure Machine Learning-SDK voor Python](https://aka.ms/aml-sdk)
- [Azure Machine Learning Data Prep SDK](https://aka.ms/data-prep-sdk)
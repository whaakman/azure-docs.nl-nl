---
title: Een Python-ontwikkelomgeving instellen
titleSuffix: Azure Machine Learning service
description: Leer hoe u een ontwikkelomgeving configureren wanneer u met de Azure Machine Learning-service werkt. In dit artikel leert u hoe u Conda-omgevingen gebruiken, configuratiebestanden maken en configureren van Jupyter Notebooks, Azure-laptops, Azure Databricks, IDE's, code-editors en de Data Science Virtual Machine.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.component: core
ms.reviewer: larryfr
manager: cgronlun
ms.topic: conceptual
ms.date: 01/18/2019
ms.custom: seodec18
ms.openlocfilehash: 0f53b3cec843ca8016c61a360025b5e731b96f55
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54815868"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Een ontwikkelomgeving configureren voor Azure Machine Learning

In dit artikel leert u hoe u een ontwikkelomgeving, zodat het werken met Azure Machine Learning-service configureren. Machine Learning-service is een platform-agnostische. 

De enige vereisten voor uw ontwikkelomgeving zijn Python 3, Conda (voor geïsoleerde omgevingen) en een configuratiebestand met gegevens over uw Azure Machine Learning-werkruimte.

In dit artikel is gericht op de volgende omgevingen en hulpprogramma's:

* [Azure-notitieblokken](#aznotebooks): Een Jupyter-Notebooks-service die wordt gehost in de Azure-cloud. Dit is de eenvoudigste manier om aan de slag, omdat de SDK van Azure Machine Learning al is geïnstalleerd.

* [De Data Science Virtual Machine (DSVM)](#dsvm): Een vooraf geconfigureerde ontwikkeling of experimenten omgeving in de Azure-cloud die ontworpen voor data science werk en kan worden geïmplementeerd op CPU alleen VM-exemplaren of op basis van GPU-exemplaren. Python 3, Conda, Jupyter-Notebooks en de SDK van Azure Machine Learning zijn al geïnstalleerd. De virtuele machine wordt geleverd met populaire machine learning- en deep learning-frameworks, hulpprogramma's en editors voor het ontwikkelen van machine learning-oplossingen. Het is waarschijnlijk het meest complete ontwikkelomgeving voor machine learning op het Azure-platform.

* [De Jupyter-Notebook](#jupyter): Als u al de Jupyter-Notebook gebruikt, is de SDK enkele extra's die u moet installeren.

* [Visual Studio Code](#vscode): Als u Visual Studio Code gebruikt, worden er enkele nuttige extensies die u kunt installeren.

* [Azure Databricks](#aml-databricks): Een populaire analyseplatform dat gebaseerd op Apache Spark. Informatie over het verkrijgen van de Azure Machine Learning-SDK op uw cluster, zodat u modellen kunt implementeren.

Als u al een Python 3-omgeving hebt, of alleen de basisstappen voor het installeren van de SDK wilt, Zie de [lokale computer](#local) sectie.

## <a name="prerequisites"></a>Vereisten

- Een werkruimte van Azure Machine Learning-service. Zie voor het maken van de werkruimte, [aan de slag met Azure Machine Learning-service](quickstart-get-started.md).

- Een van beide de [Zorgcontinuüm Anaconda](https://www.anaconda.com/download/) of [Miniconda](https://conda.io/miniconda.html) Pakketbeheer.

    > [!IMPORTANT]
    > Anaconda en Miniconda zijn niet vereist als u Azure-notitieblokken.

- In Linux of macOS moet u de bash-shell.

    > [!TIP]
    > Als u in Linux of macOS bent en gebruik een shell dan bash (bijvoorbeeld zsh) ontvangt u mogelijk fouten tijdens het uitvoeren van sommige opdrachten. U kunt dit probleem omzeilen, gebruikt u de `bash` opdracht voor het starten van een nieuwe bash-shell en voer de opdrachten er uit.

- Op Windows moet u de opdrachtprompt of Anaconda-prompt (geïnstalleerd door Anaconda en Miniconda).

## <a id="aznotebooks"></a>Azure-laptops

[Azure-notitieblokken](https://notebooks.azure.com) (preview) is een interactieve ontwikkelomgeving in de Azure-cloud. Het is de eenvoudigste manier om aan de slag met Azure Machine Learning-ontwikkeling.

* De SDK van Azure Machine Learning is al geïnstalleerd.
* Nadat u een werkruimte van de service Azure Machine Learning in Azure portal maakt, kunt u klikt op een knop automatisch configureren aan de Notebook van Azure-omgeving werkt met de werkruimte.

Om te beginnen met ontwikkelen met Azure-laptops, Zie [aan de slag met Azure Machine Learning-service](quickstart-get-started.md).

Azure-notitieblokken gebruikt standaard een gratis service-laag die is beperkt tot 4GB geheugen en 1GB aan gegevens. U kunt deze limieten echter verwijderen door het koppelen van een Data Science Virtual Machine-instantie aan het project Azure notitieblokken. Zie voor meer informatie, [beheren en configureren van Azure-notitieblokken projecten - Compute-laag](/azure/notebooks/configure-manage-azure-notebooks-projects.md#compute-tier).

## <a id="dsvm"></a>Virtuele Machine voor Datatechnologie

De DSVM wordt de installatiekopie van een aangepaste virtuele machine (VM). Het ontworpen voor data science werk dat vooraf geconfigureerd met:

  - Pakketten, zoals TensorFlow, PyTorch, Scikit-informatie, XGBoost en de Azure Machine Learning-SDK
  - Populaire data science-hulpprogramma's zoals Spark zelfstandige en inzoomen
  - Azure-hulpprogramma's zoals de Azure CLI, AzCopy en Storage Explorer
  - Geïntegreerde ontwikkelingsomgevingen (IDE's), zoals Visual Studio Code en PyCharm
  - Jupyter Notebook-Server

De Azure Machine Learning-SDK werkt op de Ubuntu- of Windows-versie van de DSVM. Maar als u van plan bent de DSVM gebruiken als een compute-doel, alleen Ubuntu wordt ondersteund.

Voor het gebruik van de DSVM als een ontwikkelomgeving, het volgende doen:

1. Maak een DSVM in een van de volgende omgevingen:

    * De Azure-portal:

        * [Maken van een Ubuntu Data Science Virtual Machine](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro)

        * [Maken van een Windows Data Science Virtual Machine](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/provision-vm)

    * De Azure CLI:

        > [!IMPORTANT]
        > * Wanneer u de Azure CLI gebruikt, moet u eerst aanmelden met uw Azure-abonnement met behulp van de `az login` opdracht.
        >
        > * Wanneer u de opdrachten in deze stap gebruikt, moet u de naam van een resourcegroep, een naam op voor de virtuele machine, een gebruikersnaam en wachtwoord opgeven.

        * Gebruik de volgende opdracht voor het maken van een Ubuntu Data Science Virtual Machine:

            ```azurecli
            # create a Ubuntu DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            # If you need to create a new resource group use: "az group create --name YOUR-RESOURCE-GROUP-NAME --location YOUR-REGION (For example: westus2)"
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:linux-data-science-vm-ubuntu:linuxdsvmubuntu:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --generate-ssh-keys --authentication-type password
            ```

        * Gebruik de volgende opdracht voor het maken van een Windows Data Science Virtual Machine:

            ```azurecli
            # create a Windows Server 2016 DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:dsvm-windows:server-2016:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --authentication-type password
            ```    

2. De SDK van Azure Machine Learning is al geïnstalleerd op de DSVM. Als u de Conda-omgeving met de SDK, gebruikt u een van de volgende opdrachten:

    * Voor Ubuntu-DSVM:

        ```shell
        conda activate py36
        ```

    * Voor Windows DSVM:

        ```shell
        conda activate AzureML
        ```

1. Als u wilt controleren of u kunt toegang krijgen tot de SDK en controleer de versie, gebruik de volgende Python-code:

    ```python
    import azureml.core
    print(azureml.core.VERSION)
    ```

1. Zie configureren van de DSVM voor het gebruik van de werkruimte van uw Azure Machine Learning-service de [maken van een configuratiebestand werkruimte](#workspace) sectie.

Zie voor meer informatie, [virtuele Machines voor Datatechnologie](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a id="local"></a>Lokale computer

Wanneer u een lokale computer (die mogelijk ook een externe virtuele machine), een Conda-omgeving maken en installeer de SDK door het volgende te doen:

1. Open een opdrachtprompt of de shell.

1. Een Conda-omgeving maken met de volgende opdrachten:

    ```shell
    # create a new Conda environment with Python 3.6, NumPy, and Cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the Conda environment
    conda activate myenv

    # On macOS run
    source activate myenv
    ```

    Het duurt enkele minuten om te maken van de omgeving als Python 3.6 en andere onderdelen moeten worden gedownload.

1. Installeer de SDK van Azure Machine Learning met extra's laptop en de SDK van de voorbereiding van gegevens met behulp van de volgende opdracht uit:

     ```shell
    pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep
    ```

   > [!NOTE]
   > Als u een bericht ontvangt dat PyYAML kan niet worden verwijderd, kunt u de volgende opdracht gebruiken:
   >
   > `pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep --ignore-installed PyYAML`

   Het duurt enkele minuten de SDK te installeren.

1. Voor uw machine learning experimenten-pakketten installeren. Gebruik de volgende opdracht en vervangen  *\<nieuw pakket >* aan het pakket dat u wilt installeren:

    ```shell
    conda install <new package>
    ```

1. Als u wilt controleren of de SDK is geïnstalleerd, moet u de volgende Python-code gebruiken:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

### <a id="jupyter"></a>Jupyter-Notebooks

Jupyter-notitieblokken maken deel uit van de [Jupyter Project](https://jupyter.org/). Ze bieden een interactieve ervaring met codering waar het maken van documenten die live code met verhalende tekst en afbeeldingen combineren. Jupyter-Notebooks zijn ook een uitstekende manier om uw resultaten delen met anderen, omdat u de uitvoer van de Codesecties van uw kunt opslaan in het document. U kunt Jupyter-Notebooks installeren op een aantal verschillende platformen.

De procedure in de [lokale computer](#local) sectie optionele onderdelen worden geïnstalleerd voor de Jupyter-Notebooks. Om in te schakelen deze onderdelen in uw omgeving Jupyter-Notebook, het volgende doen:

1. Open een opdrachtprompt of de shell.

1. Als u wilt installeren een Conda-bewuste Jupyter Notebook-Server, gebruik de volgende opdracht:

    ```shell
    # install Jupyter
    conda install nb_conda
    ```

1. Jupyter Notebook openen met de volgende opdracht:

    ```shell
    jupyter notebook
    ```

1. Selecteer om te controleren dat Jupyter-Notebook kunt gebruiken van de SDK, opent u een nieuwe notebook, **myenv** als de kernel en voer de volgende opdracht in een cel laptop:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Als u wilt de Jupyter-Notebook voor het gebruik van de werkruimte van uw Azure Machine Learning-service configureren, gaat u naar de [maken van een configuratiebestand werkruimte](#workspace) sectie.

### <a id="vscode"></a>Visual Studio Code

Visual Studio Code is een platformoverschrijdende code-editor. Afhankelijk van een lokale installatie van Python 3 en Conda voor Python-ondersteuning, maar het biedt extra hulpmiddelen voor het werken met AI. Het biedt ook ondersteuning voor het selecteren van de Conda-omgeving in de code-editor.

Voor het gebruik van Visual Studio Code voor de ontwikkeling, het volgende doen:

1. Zie voor meer informatie over het gebruik van Visual Studio Code voor Python-ontwikkeling, [aan de slag met Python vscode](https://code.visualstudio.com/docs/python/python-tutorial).

1. Schakel de Conda-omgeving, opent u VS Code en selecteer vervolgens Ctrl + Shift + P (Linux en Windows) of Command + Shift + P (Mac).  
    De __opdracht palet__ wordt geopend. 

1. Voer __Python: Selecteer Interpreter__, en selecteer vervolgens de Conda-omgeving.

1. Als u wilt valideren dat u de SDK kunt gebruiken, maken en voer vervolgens een nieuwe Python-bestand (.py) met de volgende code:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Zie voor informatie over het installeren van de Azure Machine Learning-extensie voor Visual Studio Code [hulpprogramma's voor AI](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai).

    Zie voor meer informatie, [gebruik Azure Machine Learning voor Visual Studio Code](how-to-vscode-tools.md).

<a name="aml-databricks"></a>

## <a name="azure-databricks"></a>Azure Databricks

U kunt een aangepaste versie van de Azure Machine Learning-SDK voor Azure Databricks gebruiken voor end-to-end aangepaste machine learning. Of u kunt uw model binnen Databricks te trainen en implementeren met behulp van [Visual Studio Code](how-to-vscode-train-deploy.md#deploy-your-service-from-vs-code).

Voorbereiden van uw Databricks-cluster en voorbeeldnotitieblokken ophalen:

1. Maak een [Databricks-cluster](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) met de volgende instellingen:

    | Instelling | Waarde |
    |----|---|
    | Clusternaam | yourclustername |
    | Databricks Runtime | Een runtime niet ML (niet ML 4.x, 5.x) |
    | Python-versie | 3 |
    | Medewerkers | 2 of hoger |

    Gebruik deze instellingen alleen als u automatische machine learning met op Databricks:
    
    |   Instelling | Waarde |
    |----|---|
    | VM-typen voor worker-knooppunt | Virtuele machine bij voorkeur is geoptimaliseerd voor geheugen |
    | Automatisch schalen inschakelen | Schakel het selectievakje |
    
    Het aantal worker-knooppunten in uw Databricks-cluster bepaalt het maximum aantal gelijktijdige iteraties in geautomatiseerde ML-instellingen.  

    Het duurt enkele minuten om het cluster te maken. Wacht totdat het cluster wordt uitgevoerd voordat u doorgaat.

1. Installeren en het Azure Machine Learning-SDK-pakket te koppelen aan uw cluster.  

    * [Maken van een bibliotheek](https://docs.databricks.com/user-guide/libraries.html#create-a-library) met een van deze instellingen (_slechts één van deze opties kiest_):
    
        * Azure Machine Learning-SDK installeren _zonder_ geautomatiseerde van machine learning-mogelijkheden:
            | Instelling | Waarde |
            |----|---|
            |Bron | Uploaden van Python EI of PyPI
            |De naam van de PyPi | azureml-sdk[databricks]
    
        * Azure Machine Learning-SDK installeren _met_ geautomatiseerde van machine learning:
            | Instelling | Waarde |
            |----|---|
            |Bron | Uploaden van Python EI of PyPI
            |De naam van de PyPi | azureml-sdk[automl_databricks]
    
    * Schakel niet **automatisch koppelen aan alle clusters**

    * Selecteer **Attach** naast de clusternaam van uw

    * Zorg ervoor dat er geen fouten zijn totdat de status gewijzigd in **gekoppeld**. Het kan enkele minuten duren.

    Als u een oude versie van de SDK hebt, deze van de geïnstalleerde bibliotheken van het cluster uit te schakelen en verplaatsen naar de Prullenbak. De nieuwe versie van de SDK installeren en opnieuw starten van het cluster. Als er een probleem na deze is, loskoppelen en opnieuw koppelen van uw cluster.

    Wanneer u klaar bent, wordt de bibliotheek gekoppeld, zoals wordt weergegeven in de volgende afbeeldingen. Houd rekening met deze [problemen Databricks](resource-known-issues.md#databricks).

    * Als u Azure Machine Learning-SDK geïnstalleerd _zonder_ automatische leerprocessen ![SDK zonder automatische leerprocessen geïnstalleerd op Databricks ](./media/how-to-configure-environment/amlsdk-withoutautoml.jpg)

    * Als u Azure Machine Learning-SDK geïnstalleerd _met_ automatische leerprocessen ![SDK met automatische leerprocessen geïnstalleerd op Databricks ](./media/how-to-configure-environment/automlonadb.jpg)

   Als deze stap mislukt, start u uw cluster opnieuw door het volgende te doen:

   a. Selecteer in het linkerdeelvenster **Clusters**. 
   
   b. Selecteer de clusternaam van uw in de tabel. 

   c. Op de **bibliotheken** tabblad **opnieuw**.

1. Download de [SDK van Azure Databricks/Azure Machine Learning-notebook archiefbestand](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks/Databricks_AMLSDK_1-4_6.dbc).

   >[!Warning]
   > Veel voorbeeldnotitieblokken zijn beschikbaar voor gebruik met Azure Machine Learning-service. Alleen [deze voorbeeldnotitieblokken](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks) werken met Azure Databricks.

1.  [Importeren van het archiefbestand](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-an-archive) naar uw Databricks-cluster en beginnen met het verkennen, zoals wordt beschreven op de [Machine Learning-laptops](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks) pagina.


## <a id="workspace"></a>Het configuratiebestand van een werkruimte maken

Het configuratiebestand van de werkruimte is een JSON-bestand dat de SDK legt uit hoe om te communiceren met uw werkruimte van Azure Machine Learning-service. Het bestand met de naam *config.json*, en deze heeft de volgende indeling:

```json
{
    "subscription_id": "<subscription-id>",
    "resource_group": "<resource-group>",
    "workspace_name": "<workspace-name>"
}
```

Deze JSON-bestand moet zich in de directory-structuur die uw Python-scripts of Jupyter-Notebooks bevat. Kan het zijn in dezelfde map, een submap met de naam *aml_config*, of in een bovenliggende map.

U kunt dit bestand vanuit uw code gebruiken `ws=Workspace.from_config()`. Deze code wordt de informatie uit het bestand wordt geladen en maakt verbinding met uw werkruimte.

U kunt het configuratiebestand op drie manieren maken:

* **Ga als volgt de [Azure Machine Learning-snelstartgids](quickstart-get-started.md)**: Een *config.json* bestand wordt gemaakt in uw Azure-notitieblokken-bibliotheek. Het bestand bevat de configuratiegegevens voor uw werkruimte. U kunt downloaden of kopieer de *config.json* in andere omgevingen ontwikkeling.

* **Maak het bestand handmatig**: Met deze methode maakt u een teksteditor. U vindt de waarden die het configuratiebestand opdoen uw werkruimte in de [Azure-portal](https://portal.azure.com). Kopieer de naam van de werkruimte, de resourcegroep en de abonnement-id-waarden en deze gebruiken in het configuratiebestand.

     ![Azure Portal](./media/how-to-configure-environment/configure.png)

* **Maak het bestand via een programma**: In het volgende codefragment verbindt u aan een werkruimte door op te geven van de abonnements-ID, de resourcegroep en de naam van de werkruimte. Deze slaat de configuratie van de werkruimte op in het bestand:

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

    Deze code schrijft het configuratiebestand naar de *aml_config/config.json* bestand.

## <a name="next-steps"></a>Volgende stappen

- [Een model te trainen](tutorial-train-models-with-aml.md) op Azure Machine Learning met de gegevensset MNIST]
- Weergave de [Azure Machine Learning-SDK voor Python](https://aka.ms/aml-sdk) verwijzing
- Meer informatie over de [SDK voor Azure Machine Learning-Dataprep](https://aka.ms/data-prep-sdk)

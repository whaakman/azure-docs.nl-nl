---
title: Een Python-ontwikkelomgeving instellen
titleSuffix: Azure Machine Learning service
description: Leer hoe u een ontwikkelomgeving configureren wanneer u met de Azure Machine Learning-service werkt. In dit artikel leert u hoe u Conda-omgevingen gebruiken, configuratiebestanden maken en configureren van Jupyter Notebooks, Azure-laptops, Azure Databricks, IDE's, code-editors en de Data Science Virtual Machine.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
manager: cgronlun
ms.topic: conceptual
ms.date: 01/18/2019
ms.custom: seodec18
ms.openlocfilehash: 61c380ee3427afdf40427ed82ed0fd5c4f1b49fd
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56729012"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Een ontwikkelomgeving configureren voor Azure Machine Learning

In dit artikel leert u hoe u een ontwikkelomgeving, zodat het werken met Azure Machine Learning-service configureren. Machine Learning-service is een platform-agnostische.

De enige vereisten voor uw ontwikkelomgeving zijn Python 3, Anaconda (voor geïsoleerde omgevingen) en een configuratiebestand met gegevens over uw Azure Machine Learning-werkruimte.

In dit artikel is gericht op de volgende omgevingen en hulpprogramma's:

* Azure-notitieblokken: Een Jupyter-Notebooks-service die wordt gehost in de Azure-cloud. Dit is de eenvoudigste manier om aan de slag, omdat de SDK van Azure Machine Learning al is geïnstalleerd.

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

Azure-notitieblokken gebruikt standaard een gratis service-laag die is beperkt tot 4GB geheugen en 1GB aan gegevens. U kunt deze limieten echter verwijderen door het koppelen van een Data Science Virtual Machine-instantie aan het project Azure notitieblokken. Zie voor meer informatie, [beheren en configureren van Azure-notitieblokken projecten - Compute-laag](/azure/notebooks/configure-manage-azure-notebooks-projects#compute-tier).

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

Wanneer u een lokale computer (die mogelijk ook een externe virtuele machine), een Anaconda-omgeving maken en installeer de SDK door het volgende te doen:

1. Download en installeer [Anaconda](https://www.anaconda.com/distribution/#download-section) (Python 3.7-versie) als u nog geen hebt.

1. Open een opdrachtprompt Anaconda en een omgeving maken met de volgende opdrachten:

    Voer de volgende opdracht om de omgeving te maken.

    ```shell
    conda create -n myenv python=3.6.5
    ```

    Vervolgens activeert u de omgeving.

    ```shell
    conda activate myenv
    ```

    Dit voorbeeld maakt u een omgeving met behulp van python 3.6.5, maar specifieke subversions kunnen worden gekozen. Compatibiliteit van de SDK kan niet worden gegarandeerd met bepaalde primaire versies (3.5 + wordt aanbevolen), en het is raadzaam om te proberen een verschillende versie/subversion in uw omgeving Anaconda als u fouten ondervindt. Het duurt enkele minuten voor het maken van de omgeving, terwijl onderdelen en -pakketten zijn gedownload.

1. Voer de volgende opdrachten in uw nieuwe omgeving om in te schakelen omgevingsspecifieke ipython-kernels. Dit zorgt ervoor dat het verwachte kernel en pakket importeren gedrag bij het werken met Jupyter-Notebooks in Anaconda omgevingen:

    ```shell
    conda install notebook ipykernel
    ```

    Voer de volgende opdracht om te maken van de kernel:

    ```shell
    ipython kernel install --user
    ```

1. Gebruik de volgende opdrachten om pakketten te installeren:

    Deze opdracht wordt de basis Azure Machine Learning-SDK geïnstalleerd met de notebook en automl extra's. De `automl` extra is een grote installeren en kan worden verwijderd uit de vierkante haken als u niet van plan bent om uit te voeren van geautomatiseerde machine learning-experimenten. De `automl` extra bevat ook de Azure Machine Learning Data Prep SDK standaard als een afhankelijkheid.

     ```shell
    pip install azureml-sdk[notebooks,automl]
    ```

    Deze opdracht gebruiken om de Azure Machine Learning Data Prep SDK installeren op een eigen:

    ```shell
    pip install azureml-dataprep
    ```

   > [!NOTE]
   > Als u een bericht ontvangt dat PyYAML kan niet worden verwijderd, kunt u de volgende opdracht gebruiken:
   >
   > `pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep --ignore-installed PyYAML`

   Het duurt enkele minuten de SDK te installeren. Zie de [installatiehandleiding](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) voor meer informatie over opties voor de installatie.

1. Installatie van andere pakketten voor uw machine learning experimenten.

    Gebruik een van de volgende opdrachten en vervang  *\<nieuw pakket >* aan het pakket dat u wilt installeren. Het installeren van pakketten via `conda install` vereist dat het pakket deel uit van de huidige kanalen maakt (nieuwe kanalen kunnen worden toegevoegd in de Cloud Anaconda).

    ```shell
    conda install <new package>
    ```

    U kunt ook installeren pakketten via `pip`.

    ```shell
    pip install <new package>
    ```

### <a id="jupyter"></a>Jupyter-Notebooks

Jupyter-notitieblokken maken deel uit van de [Jupyter Project](https://jupyter.org/). Ze bieden een interactieve ervaring met codering waar het maken van documenten die live code met verhalende tekst en afbeeldingen combineren. Jupyter-Notebooks zijn ook een uitstekende manier om uw resultaten delen met anderen, omdat u de uitvoer van de Codesecties van uw kunt opslaan in het document. U kunt Jupyter-Notebooks installeren op een aantal verschillende platformen.

De procedure in de [lokale computer](#local) sectie vereiste onderdelen voor het uitvoeren van Jupyter Notebooks in een omgeving Anaconda wordt geïnstalleerd. Om in te schakelen deze onderdelen in uw omgeving Jupyter-Notebook, het volgende doen:

1. Open een opdrachtprompt Anaconda en activeer uw omgeving.

    ```shell
    conda activate myenv
    ```

1. Start de Jupyter-Notebook-server met de volgende opdracht:

    ```shell
    jupyter notebook
    ```

1. Om te controleren dat de SDK door Jupyter-Notebook kunt gebruiken, maakt u een **nieuw** laptop, selecteer **Python 3** als de kernel en voer de volgende opdracht in een cel laptop:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Als u problemen bij het importeren van modules en ontvangt een `ModuleNotFoundError`, zorg ervoor dat uw Jupyter-kernel is verbonden met het juiste pad voor uw omgeving door het uitvoeren van de volgende code in een cel Notebook.

    ```python
    import sys
    sys.path
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

    | Instelling | Value |
    |----|---|
    | Clusternaam | yourclustername |
    | Databricks Runtime | Een runtime niet ML (niet ML 4.x, 5.x) |
    | Python-versie | 3 |
    | Medewerkers | 2 of hoger |

    Gebruik deze instellingen alleen als u automatische machine learning met op Databricks:

    |   Instelling | Value |
    |----|---|
    | VM-typen voor worker-knooppunt | Virtuele machine bij voorkeur is geoptimaliseerd voor geheugen |
    | Automatisch schalen inschakelen | Schakel het selectievakje |

    Het aantal worker-knooppunten in uw Databricks-cluster bepaalt het maximum aantal gelijktijdige iteraties in geautomatiseerde machine learning-configuratie.

    Het duurt enkele minuten om het cluster te maken. Wacht totdat het cluster wordt uitgevoerd voordat u doorgaat.

1. Installeren en het Azure Machine Learning-SDK-pakket te koppelen aan uw cluster.

    * [Maken van een bibliotheek](https://docs.databricks.com/user-guide/libraries.html#create-a-library) met een van deze instellingen (_slechts één van deze opties kiest_):

        * Azure Machine Learning-SDK installeren _zonder_ geautomatiseerde van machine learning-mogelijkheden:
            | Instelling | Value |
            |----|---|
            |Bron | Uploaden van Python EI of PyPI
            |De naam van de PyPi | azureml-sdk[databricks]

        * Azure Machine Learning-SDK installeren _met_ geautomatiseerde van machine learning:
            | Instelling | Value |
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

- [Een model te trainen](tutorial-train-models-with-aml.md) op Azure Machine Learning met de MNIST-gegevensset
- Weergave de [Azure Machine Learning-SDK voor Python](https://aka.ms/aml-sdk) verwijzing
- Meer informatie over de [SDK voor Azure Machine Learning-Dataprep](https://aka.ms/data-prep-sdk)

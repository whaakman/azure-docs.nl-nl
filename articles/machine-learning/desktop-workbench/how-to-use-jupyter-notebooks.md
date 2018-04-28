---
title: Het gebruik van Jupyter Notebooks in Azure Machine Learning Workbench | Microsoft Docs
description: Een handleiding voor het gebruik van de functie Jupyter-Notebook van Azure Machine Learning-Workbench
services: machine-learning
author: rastala
ms.author: roastala
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/09/2017
ms.openlocfilehash: 11d295734fd6d9180468709f0a25cca48b13df4d
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="use-jupyter-notebooks-in-azure-machine-learning-workbench"></a>Jupyter-Notebooks in Azure Machine Learning-Workbench gebruiken

Interactieve gegevens wetenschappelijke experimenteren via de integratie met Jupyter-Notebooks biedt ondersteuning voor Azure Machine Learning-Workbench. Dit artikel wordt beschreven hoe u effectief gebruik van deze functie te verhogen van de snelheid en kwaliteit van uw gegevens op interactieve wetenschappelijke experimenteren.

## <a name="prerequisites"></a>Vereisten
- [Azure Machine Learning-accounts maken en installeer Azure Machine Learning Workbench](../service/quickstart-installation.md).
- Vertrouwd zijn met de [Jupyter-Notebook](http://jupyter.org/). In dit artikel gaat niet over wilt leren Jupyter gebruiken.

## <a name="jupyter-notebook-architecture"></a>Architectuur van Jupyter-Notebook
Op een hoog niveau bevat Jupyter-Notebook architectuur drie onderdelen. Elk kunt uitvoeren in omgevingen met verschillende rekenscenario:

- **Client**: ontvangt gebruikersinvoer en geeft uitvoer weergegeven.
- **Server**: webserver die als host fungeert voor de notebook bestanden (.ipynb).
- **Kernel**: Runtime-omgeving in welke uitvoering van de notebook cellen gebeurt.

Zie voor meer informatie, de officiële [Jupyter documentatie](http://jupyter.readthedocs.io/en/latest/architecture/how_jupyter_ipython_work.html). Het volgende diagram laat zien hoe deze client en server kernel-architectuur wordt toegewezen aan de onderdelen in Azure Machine Learning:

![Architectuur van Jupyter-Notebook](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-architecture.png)

## <a name="kernels-in-azure-machine-learning-workbench-notebooks"></a>Kernels in Azure Machine Learning Workbench notitieblokken
U kunt toegang krijgen tot andere kernels in Azure Machine Learning Workbench uitvoeren configuraties definiëren en compute doelen in de `aml_config` map in uw project. Een nieuwe compute-doel toevoegen door uitgifte van de `az ml computetarget attach` opdracht is het equivalent van het toevoegen van een nieuwe kernel.

>[!NOTE]
>Bekijk [configureren van Azure Machine Learning experimenteren Service](experimentation-service-configuration.md) voor meer informatie over configuraties uitgevoerd en doelen te berekenen.

### <a name="kernel-naming-convention"></a>De naamconventie kernel
Azure Machine Learning Workbench aangepaste kernels voor Jupyter genereert. Deze kernels zijn benoemde  *\<projectnaam > \<config naam uitvoeren >*. Bijvoorbeeld, als er een uitvoeren-configuratie met de naam _docker-python_ in een project met de naam _myIris_, Azure Machine Learning met de naam een kernel beschikbaar stelt *myIris docker-python.* U de actieve kernel instellen in de Jupyter-Notebook **Kernel** menu, in de **wijziging kernel** vervolgmenu. De naam van de actieve kernel weergegeven helemaal rechts op de menubalk.
 
Azure Machine Learning Workbench ondersteunt momenteel de volgende soorten kernels.

### <a name="local-python-kernel"></a>Lokale Python-kernel
Deze kernel Python biedt ondersteuning voor uitvoering op lokale computers. Het geïntegreerd met Azure Machine Learning uitvoeren geschiedenis ondersteuning. De naam van de kernel wordt meestal *my_project_name local.*

>[!NOTE]
>Gebruik niet de Python-3-kernel. Het is een zelfstandige kernel geleverd door Jupyter standaard en is niet geïntegreerd met Azure Machine Learning-mogelijkheden. Bijvoorbeeld, de `%azureml` Jupyter magische functies 'niet gevonden'-fouten retourneren. 

### <a name="python-kernel-in-docker-local-or-remote"></a>Python-kernel in Docker (lokaal of extern)
Deze kernel Python wordt uitgevoerd in een Docker-container op uw lokale computer of op een externe virtuele Linux-machine (VM). De naam van de kernel wordt meestal *my_project docker.* De bijbehorende `docker.runconfig` bestand heeft de `Framework` veld ingesteld op `Python`.

### <a name="pyspark-kernel-in-docker-local-or-remote"></a>PySpark-kernel in Docker (lokaal of extern)
Deze PySpark-kernel voert de scripts in een Spark-context in een Docker-container op uw lokale computer of op een externe Linux VM wordt uitgevoerd. De naam van de kernel wordt meestal *my_project docker.* De bijbehorende `docker.runconfig` bestand heeft de `Framework` veld ingesteld op `PySpark`.

### <a name="pyspark-kernel-in-an-azure-hdinsight-cluster"></a>PySpark-kernel in een Azure HDInsight-cluster
Deze kernel wordt uitgevoerd in de externe Azure HDInsight-cluster dat u gekoppeld als een compute-doel voor uw project. De naam van de kernel wordt meestal *my_project my_hdi.* 

>[!IMPORTANT]
>In de `.compute` -bestand voor de HDI compute doel, moet u de `yarnDeployMode` veld `client` (de standaardwaarde is `cluster`) te gebruiken deze kernel. 

## <a name="start-a-jupyter-server-from-azure-machine-learning-workbench"></a>Een Jupyter-server starten vanuit Azure Machine Learning Workbench
Vanuit Azure Machine Learning Workbench u toegang hebt tot notitieblokken via de **notitieblokken** tabblad. De _Iris classificeren_ voorbeeldproject bevat een `iris.ipynb` voorbeeld notebook.

![tabblad laptops](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-01.png)

Wanneer u een laptop in Azure Machine Learning Workbench opent, wordt weergegeven op een eigen documenttabblad in het **voorbeeldmodus**. Dit is een alleen-lezen weergave is die niet nodig een waarop Jupyter-server en kernel.

![laptop-preview](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-02.png)

Selecteren van de **Notebook Server starten** knop Hiermee start u de Jupyter-server en verandert de notebook in **bewerkingsmodus**. De vertrouwde Jupyter-Notebook-gebruikersinterface wordt weergegeven in de Workbench ingesloten. U kunt nu een kernel van instellen de **Kernel** menu en uw laptop interactieve sessie te starten. 

>[!NOTE]
>Met niet-lokale kernels, kan het enkele minuten duren om te starten als u dit voor het eerst gebruikt duren. U kunt uitvoeren de `az ml experiment prepare` opdracht vanuit het venster CLI de compute-doel voorbereiden, zodat de kernel veel sneller wordt gestart nadat de compute-doel wordt voorbereid.

![De modus bewerken](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-04.png)

Dit is een volledig interactief Jupyter-Notebook-ervaring. Alle bewerkingen van de reguliere notebook en sneltoetsen worden ondersteund door dit venster, met uitzondering van enkele bestandsbewerkingen die kan worden uitgevoerd via de Workbench **notitieblokken** tabblad en **bestand** tabblad.

## <a name="start-a-jupyter-server-from-the-command-line"></a>Een Jupyter-server starten vanaf de opdrachtregel
U kunt ook een laptop-sessie starten door uitgifte van `az ml notebook start` vanuit het venster van de opdrachtregel:
```
$ az ml notebook start
[I 10:14:25.455 NotebookApp] The port 8888 is already in use, trying another port.
[I 10:14:25.464 NotebookApp] Serving notebooks from local directory: /Users/johnpelak/Desktop/IrisDemo
[I 10:14:25.465 NotebookApp] 0 active kernels 
[I 10:14:25.465 NotebookApp] The Jupyter Notebook is running at: http://localhost:8889/?token=1f0161ab88b22fc83f2083a93879ec5e8d0ec18490f0b953
[I 10:14:25.465 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 10:14:25.466 NotebookApp] 
    
Copy and paste this URL into your browser when you connect for the first time, to login with a token: http://localhost:8889/?token=1f0161ab88b22fc83f2083a93879ec5e8d0ec18490f0b953
[I 10:14:25.759 NotebookApp] Accepting one-time-token-authenticated connection from ::1
[I 10:16:52.970 NotebookApp] Kernel started: 7f8932e0-89b9-48b4-b5d0-e8f48d1da159
[I 10:16:53.854 NotebookApp] Adapting to protocol v5.1 for kernel 7f8932e0-89b9-48b4-b5d0-e8f48d1da159
```
De standaardbrowser automatisch geopend met de Jupyter-server die verwijst naar de basismap van het project. U kunt ook de URL en weergegeven in het venster CLI-token lokaal andere browservensters te openen. 

![Project-dashboard](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-07.png)

U kunt nu kiezen een `.ipynb` laptop-bestand, opent u het instellen van de kernel (indien deze nog niet is ingesteld) en de interactieve sessie starten.

![Project-dashboard](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-08.png)

## <a name="use-magic-commands-to-manage-experiments"></a>Magic-opdrachten gebruiken voor het beheren van experimenten

U kunt [magic opdrachten](http://ipython.readthedocs.io/en/stable/interactive/magics.html) binnen de cellen van uw laptop uw uitvoeringsgeschiedenis bijhouden en opslaan van uitvoer zoals modellen of gegevenssets.

Als u wilt bijhouden afzonderlijke notebook cel wordt uitgevoerd, gebruikt u de `%azureml history on` magic-opdracht. Nadat u de geschiedenis inschakelt, wordt elke cel-uitvoering wordt weergegeven als een vermelding in de geschiedenis uitvoeren:

```
%azureml history on
from azureml.logging import get_azureml_logger
logger = get_azureml_logger()
logger.log("Cell","Load Data")
```

Als u wilt uitschakelen cel uitvoeren bijhouden, gebruikt u de `%azureml history off` magic-opdracht.

U kunt de `%azureml upload` magic opdracht bestanden op te slaan model en gegevens van uw uitvoeren. De opgeslagen objecten worden weergegeven als uitvoer in de weergave uitvoeringsgeschiedenis:

```
modelpath = os.path.join("outputs","model.pkl")
with open(modelpath,"wb") as f:
    pickle.dump(model,f)
%azureml upload outputs/model.pkl
```

>[!NOTE]
>De uitvoer moeten worden opgeslagen in een map met de naam *levert.*

## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over het gebruik van Jupyter-Notebook, de [Jupyter officiële documentatie](http://jupyter-notebook.readthedocs.io/en/latest/).    
- Een beter begrip van de uitvoeringsomgeving van het Azure Machine Learning-experimenten Zie [configureren van Azure Machine Learning experimenteren Service](experimentation-service-configuration.md).


---
title: Het gebruik van Jupyter-Notebooks in Azure Machine Learning-Workbench | Microsoft Docs
description: Handleiding voor de met de functie Jupyter-Notebooks van Azure Machine Learning-Workbench
services: machine-learning
author: rastala
ms.author: roastala
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/09/2017
ms.openlocfilehash: 9d8a9f1c32578abff1d98e093469e1a780f6cd80
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2017
---
# <a name="how-to-use-jupyter-notebook-in-azure-machine-learning-workbench"></a>Het gebruik van Jupyter-notebook in Azure Machine Learning Workbench

Interactieve gegevens wetenschappelijke experimenteren dankzij de integratie van Jupyter-notebook biedt ondersteuning voor Azure Machine Learning-Workbench. Dit artikel wordt beschreven hoe u effectief gebruik van deze functie om de snelheid en de kwaliteit van uw gegevens op interactieve wetenschappelijke experimenten te verhogen.

## <a name="prerequisites"></a>Vereisten
- [Installeren en het maken van Azure Machine Learning](quickstart-installation.md).
- Vertrouwd zijn met [Jupyter-notebook](http://jupyter.org/), aangezien dit artikel niet gaat over het gebruik van Jupyter lichten.

## <a name="jupyter-notebook-architecture"></a>Architectuur van Jupyter-notebook
Jupyter-notebook architectuur bevat drie onderdelen op een hoog niveau, elk in omgevingen met verschillende rekenscenario kunt uitvoeren:

- **Client**: ontvangt gebruikersinvoer en geeft uitvoer weergegeven
- **Server**: webserver die als host fungeert voor de notebook bestanden (.ipynb)
- **Kernel**: de runtime-omgeving waarin de werkelijke uitvoering van de cellen notebook gebeurt

Voor meer informatie vindt u over de officiële [Jupyter documentatie](http://jupyter.readthedocs.io/en/latest/architecture/how_jupyter_ipython_work.html). Hier volgt een diagram waarin u ziet hoe deze client, de server en de architectuur van de kernel worden toegewezen aan de Azure ML-onderdelen.

![laptoparchitectuur](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-architecture.png)

## <a name="kernels-in-azure-ml-workbench-notebook"></a>Kernels in Azure ML-Workbench notebook
U kunt toegang tot veel verschillende kernels in Azure ML-Workbench door te configureren uitvoeren configuraties en compute doelen in de `aml_config` map in uw project. Een nieuwe compute-doel toevoegen door uitgifte van `az ml computetarget attach` opdracht is het equivalent van het toevoegen van een nieuwe kernel.

>[!NOTE]
>Controleer de [uitvoering configureren](experimentation-service-configuration.md) voor meer informatie over configuraties uitgevoerd en doelen te berekenen.

### <a name="kernel-naming-convention"></a>De naamconventie kernel
De kernels worden gewoonlijk aangeduid met de notatie '\<projectnaam > \<config naam uitvoeren > '. Bijvoorbeeld, als er een uitvoeren-configuratie met de naam _docker-python_ in een project met de naam _myIris_, vindt u een met de naam 'myIris docker-python' in de kernel-lijst bij het openen van een Jupyter-notebook kernel.

Op dit moment ondersteunt de Workbench de volgende soorten kernels.

### <a name="local-python-kernel"></a>Lokale Python-kernel
Deze kernel Python biedt ondersteuning voor uitvoering op de lokale computer. Het is geïntegreerd met ondersteuning voor Azure Machine Learning-geschiedenis uitvoeren. De naam van de kernel is meestal 'lokale my_project_name'.

>[!NOTE]
>Gebruik niet de kernel 'Python 3'. Het is een zelfstandige kernel geleverd door Jupyter standaard. Het is niet geïntegreerd met Azure Machine Learning-mogelijkheden.

### <a name="python-kernel-in-docker-local-or-remote"></a>Python-Kernel in Docker (lokaal of extern)
Deze kernel Python wordt uitgevoerd in een Docker-container op uw lokale computer of in een externe Linux-VM. De naam van de kernel is meestal 'my_project docker'. De bijbehorende `docker.runconfig` bestand heeft de `Framework` veld ingesteld op `Python`.

### <a name="pyspark-kernel-in-docker-local-or-remote"></a>PySpark-kernel in Docker (lokaal of extern)
Deze PySpark-kernel voert de scripts in een Spark-context uitgevoerd op de Docker-container op uw lokale computer of op een externe Linux-VM. De naam van de kernel is meestal 'my_project docker'. De bijbehorende `docker.runconfig` bestand heeft de `Framework` veld ingesteld op `PySpark`.

### <a name="pyspark-kernel-on-hdinsight-cluster"></a>PySpark-kernel op HDInsight-cluster
Deze kernel wordt uitgevoerd in het externe HDInsight-cluster dat u als een compute-doel voor uw project gekoppeld. De naam van de kernel is meestal 'my_project my_hdi'. 

>[!IMPORTANT]
>In de `.compute` -bestand voor de HDI compute doel, moet u de `yarnDeployMode` veld `client` (de standaardwaarde is `cluster`) om te kunnen gebruiken deze kernel. 

## <a name="start-jupyter-server-from-the-workbench"></a>Jupyter Server starten vanaf de Workbench
Vanuit Azure Machine Learning Workbench notitieblokken toegankelijk via de Workbench **notitieblokken** tabblad. De _Iris classificeren_ voorbeeldproject bevat een `iris.ipynb` voorbeeld notebook.

![tabblad laptops](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-01.png)

Als een laptop in Azure Machine Learning Workbench wordt geopend, wordt deze weergegeven op een eigen documenttabblad in het **voorbeeldmodus**. Dit is een alleen-lezen weergave is die niet nodig een waarop Jupyter-server en kernel.

![laptop-preview](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-02.png)

Te klikken op **Start Notebook Server** knop Start Jupyter-server en verandert de notebook in **bewerkingsmodus**. De vertrouwde Jupyter-notebook-gebruikersinterface wordt weergegeven in de Workbench ingesloten. U kunt nu een kernel van instellen de **Kernel** menu en uw laptop interactieve sessie te starten. 

>[!NOTE]
>Opmerking voor niet-lokale kernels, het kan een paar minuten duren om te starten als u dit voor het eerst gebruikt. U kunt uitvoeren `az ml experiment prepare` opdracht vanuit CLI-venster de compute-doel voorbereiden, zodat de kernel veel sneller starten kunt nadat de compute-doel is voorbereid.

![De modus bewerken](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-04.png)

Dit is een volledig interactief Jupyter-notebook ervaring. Alle reguliere notebook operations en sneltoetsen worden ondersteund in dit venster met uitzondering van bepaalde bestandsbewerkingen, omdat ze kunnen worden uitgevoerd via de Workbench **notitieblokken** tabblad en **bestand** tabblad.

## <a name="start-jupyter-server-from-command-line"></a>Jupyter Server starten vanaf de opdrachtregel
U kunt ook een laptop-sessie starten door uitgifte van een `az ml notebook start` vanuit het venster van de opdrachtregel:
```
$ az ml notebook start
[I 10:14:25.455 NotebookApp] The port 8888 is already in use, trying another port.
[I 10:14:25.464 NotebookApp] Serving notebooks from local directory: /Users/johnpelak/Desktop/IrisDemo
[I 10:14:25.465 NotebookApp] 0 active kernels 
[I 10:14:25.465 NotebookApp] The Jupyter Notebook is running at: http://localhost:8889/?token=1f0161ab88b22fc83f2083a93879ec5e8d0ec18490f0b953
[I 10:14:25.465 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 10:14:25.466 NotebookApp] 
    
    Copy/paste this URL into your browser when you connect for the first time,
    to login with a token:
        http://localhost:8889/?token=1f0161ab88b22fc83f2083a93879ec5e8d0ec18490f0b953
[I 10:14:25.759 NotebookApp] Accepting one-time-token-authenticated connection from ::1
[I 10:16:52.970 NotebookApp] Kernel started: 7f8932e0-89b9-48b4-b5d0-e8f48d1da159
[I 10:16:53.854 NotebookApp] Adapting to protocol v5.1 for kernel 7f8932e0-89b9-48b4-b5d0-e8f48d1da159
```
De standaardbrowser wordt automatisch gestart met Jupyter-server die verwijst naar de basismap van het project. U kunt ook de URL en weergegeven in het venster CLI-token gebruiken andere browservensters lokaal te starten. 

![Project-dashboard](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-07.png)

Nu kunt u klikken op een `.ipynb` laptop-bestand, opent u het, en instellen van de kernel (indien deze nog niet is ingesteld) en de interactieve sessie starten.

![Project-dashboard](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-08.png)

## <a name="use-magic-commands-to-manage-experiments"></a>Magic-opdrachten gebruiken voor het beheren van experimenten

U kunt [magic opdrachten](http://ipython.readthedocs.io/en/stable/interactive/magics.html) binnen de cellen van uw laptop uw uitvoeringsgeschiedenis bijhouden en uitvoer, zoals modellen of gegevenssets opslaan.

Om bij te houden van afzonderlijke notebook cel wordt uitgevoerd, magische opdracht use '% azureml-geschiedenis op'. Nadat u de geschiedenis inschakelt, wordt elke cel uitvoeren in uitvoeringsgeschiedenis worden weergegeven als de vermelding.

```
%azureml history on
from azureml.logging import get_azureml_logger
logger = get_azureml_logger()
logger.log("Cell","Load Data")
```

U kunt cel bijhouden uit uitvoeren, met magische opdracht '% azureml-geschiedenis uit'.

'% Azureml uploaden' magic-opdracht kunt u bestanden op te slaan model en gegevens van uw uitvoeren. De opgeslagen objecten worden weergegeven als uitvoer in de weergave van de uitvoeringsgeschiedenis voor gegeven uitvoeren.

```
modelpath = os.path.join("outputs","model.pkl")
with open(modelpath,"wb") as f:
    pickle.dump(model,f)
%azureml upload outputs/model.pkl
```

>[!NOTE]
>De uitvoer moeten worden opgeslagen in een map met de naam 'uitvoer'

## <a name="next-steps"></a>Volgende stappen
- Voor meer informatie over het gebruik van Jupyter-notebook, gaat u naar de [Jupyter officiële documentatie](http://jupyter-notebook.readthedocs.io/en/latest/).    
- Voor het verkrijgen van een beter begrip van de uitvoeringsomgeving van het Azure ML experimenten bekijken [overzicht van Azure Machine Learning experimenteren service](experimentation-service-configuration.md)


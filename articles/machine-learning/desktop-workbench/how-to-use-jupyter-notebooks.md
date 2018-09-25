---
title: Jupyter-Notebooks gebruiken in Azure Machine Learning Workbench | Microsoft Docs
description: Een handleiding voor het gebruik van de Jupyter-Notebook-functie van Azure Machine Learning Workbench
services: machine-learning
author: rastala
ms.author: roastala
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 11/09/2017
ROBOTS: NOINDEX
ms.openlocfilehash: c59969d5a56bd2eb4eb8c490d4f1b5af20d7f056
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46998692"
---
# <a name="use-jupyter-notebooks-in-azure-machine-learning-workbench"></a>Jupyter-Notebooks in Azure Machine Learning Workbench gebruiken

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


Azure Machine Learning Workbench biedt ondersteuning voor interactive data science experimenten via de integratie met Jupyter-Notebooks. Dit artikel wordt beschreven hoe u effectieve gebruik van deze functie te verhogen de snelheid en kwaliteit van uw interactieve data science-experimenten te maken.

## <a name="prerequisites"></a>Vereisten
- [Azure Machine Learning-accounts maken en installeer Azure Machine Learning Workbench](quickstart-installation.md).
- Vertrouwd zijn met de [Jupyter-Notebook](http://jupyter.org/). Dit artikel is geen over leren hoe u kunt Jupyter gebruiken.

## <a name="jupyter-notebook-architecture"></a>Jupyter-Notebook-architectuur
Op hoog niveau bevat Jupyter-Notebook architectuur drie onderdelen. Elk kunt uitvoeren in verschillende rekenomgevingen:

- **Client**: ontvangt de invoer van de gebruiker en geeft uitvoer weergegeven.
- **Server**: webserver die als host fungeert voor de notebook-bestanden (.ipynb-bestanden).
- **Kernel**: Runtime-omgeving in welke uitvoering van de notebook cellen gebeurt.

Zie voor meer informatie, de officiële [Jupyter documentatie](http://jupyter.readthedocs.io/en/latest/architecture/how_jupyter_ipython_work.html). Het volgende diagram laat zien hoe deze client en server kernel-architectuur wordt toegewezen aan de onderdelen in Azure Machine Learning:

![Jupyter-Notebook-architectuur](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-architecture.png)

## <a name="kernels-in-azure-machine-learning-workbench-notebooks"></a>Kernels in Azure Machine Learning Workbench-laptops
U kunt toegang tot verschillende kernels in Azure Machine Learning Workbench door te voeren configuraties definiëren en compute-doelen in de `aml_config` map in uw project. Een nieuwe compute-doel toevoegen met behulp van de `az ml computetarget attach` opdracht is het equivalent van het toevoegen van een nieuwe-kernel.

>[!NOTE]
>Beoordeling [configureren van Azure Machine Learning experimenten-Service](experimentation-service-configuration.md) voor meer informatie over het uitvoeren van configuraties en-doelen COMPUTE.

### <a name="kernel-naming-convention"></a>De naamconventie kernel
Azure Machine Learning Workbench genereert aangepaste Jupyter-kernels. Deze kernels heten  *\<projectnaam > \<uitvoeren van de naam van de cloudconfiguratie >*. Bijvoorbeeld, als u werkt met een uitvoeren-configuratie met de naam _docker-python_ in een project met de naam _myIris_, Azure Machine Learning beschikbaar stelt een kernel met de naam *myIris docker-python.* U de actieve kernel instellen in de Jupyter-Notebook **Kernel** menu in de **wijziging kernel** submenu. De naam van de actieve kernel weergegeven aan de rechterkant van de menubalk.
 
Azure Machine Learning Workbench ondersteunt momenteel de volgende typen kernels.

### <a name="local-python-kernel"></a>Lokale Python-kernel
Deze Python-kernel biedt ondersteuning voor uitvoering op lokale machines. Dit is geïntegreerd met Azure Machine Learning-Uitvoeringsgeschiedenis ondersteuning. De naam van de kernel is doorgaans *my_project_name lokaal.*

>[!NOTE]
>Gebruik niet de Python 3-kernel. Het is een zelfstandige kernel door Jupyter standaard beschikbaar en is niet geïntegreerd met Azure Machine Learning-mogelijkheden. Bijvoorbeeld, de `%azureml` Jupyter magic functies 'niet gevonden'-fouten retourneren. 

### <a name="python-kernel-in-docker-local-or-remote"></a>Python-kernel in Docker (lokaal of extern)
Deze Python-kernel wordt uitgevoerd in een Docker-container op uw lokale computer of op een externe Linux-machine (VM). De naam van de kernel is doorgaans *my_project docker.* De bijbehorende `docker.runconfig` bestand heeft de `Framework` veld ingesteld op `Python`.

### <a name="pyspark-kernel-in-docker-local-or-remote"></a>PySpark-kernel in Docker (lokaal of extern)
Deze PySpark-kernel voert de scripts in een Spark-context die binnen een Docker-container op uw lokale computer of op een externe Linux-VM wordt uitgevoerd. De naam van de kernel is doorgaans *my_project docker.* De bijbehorende `docker.runconfig` bestand heeft de `Framework` veld ingesteld op `PySpark`.

### <a name="pyspark-kernel-in-an-azure-hdinsight-cluster"></a>PySpark-kernel in een Azure HDInsight-cluster
Deze kernel wordt uitgevoerd in de externe Azure HDInsight-cluster die u als een compute-doel voor uw project gekoppeld. De naam van de kernel is doorgaans *my_project my_hdi.* 

>[!IMPORTANT]
>In de `.compute` -bestand voor de HDI compute-doel, moet u de `yarnDeployMode` veld `client` (de standaardwaarde is `cluster`) gebruik van deze kernel. 

## <a name="start-a-jupyter-server-from-azure-machine-learning-workbench"></a>Een Jupyter-server van Azure Machine Learning Workbench starten
Via Azure Machine Learning Workbench, kunt u toegang tot notitieblokken via de **notitieblokken** tabblad. De _Iris classificeren_ voorbeeldproject bevat een `iris.ipynb` voorbeeld notebook.

![Tabblad notitieblokken](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-01.png)

Als u een notitieblok in Azure Machine Learning Workbench opent, wordt deze weergegeven in een eigen documenttabblad in **Preview-modus**. Dit is een alleen-lezen weergave waarvoor een Jupyter-server wordt uitgevoerd en de kernel niet vereist.

![Laptop-preview](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-02.png)

Selecteren van de **Notebook-Server Start** knop Start de Jupyter-server en verandert u de notebook in **bewerkingsmodus**. De vertrouwde gebruikersinterface voor de Jupyter-Notebook wordt ingesloten in Workbench. U kunt nu een kernel van instellen de **Kernel** menu en uw interactieve sessie te starten. 

>[!NOTE]
>Het kan een minuut of twee te starten als u deze voor het eerst gebruikt duren met niet-lokale kernels. U kunt uitvoeren de `az ml experiment prepare` opdracht vanuit het CLI-venster op de compute-doel voorbereiden, zodat de kernel wordt veel sneller gestart nadat de compute-doel wordt voorbereid.

![De modus bewerken](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-04.png)

Dit is een volledig interactieve ervaring voor de Jupyter-Notebook. Alle sneltoetsen en reguliere notebook bewerkingen worden ondersteund vanuit dit venster, met uitzondering van bepaalde bestandsbewerkingen die kunnen worden uitgevoerd via de Workbench **notitieblokken** tabblad en **bestand** tabblad.

## <a name="start-a-jupyter-server-from-the-command-line"></a>Een Jupyter-server starten vanaf de opdrachtregel
U kunt ook een laptop-sessie starten door uitgifte van `az ml notebook start` uit het opdrachtregelvenster:
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
De standaardbrowser automatisch geopend met de Jupyter-server die verwijst naar de basismap van het project. U kunt ook de URL en de token die worden weergegeven in het CLI-venster te openen van andere browservensters lokaal gebruiken. 

![Projectdashboard](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-07.png)

U kunt nu selecteren een `.ipynb` notebook-bestand, opent u het instellen van de kernel (als deze nog niet zijn ingesteld) en uw interactieve sessie starten.

![Projectdashboard](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-08.png)

## <a name="use-magic-commands-to-manage-experiments"></a>Magic-opdrachten gebruiken voor het beheren van experimenten

U kunt [magic opdrachten](http://ipython.readthedocs.io/en/stable/interactive/magics.html) binnen uw laptop cellen bij te houden van de uitvoeringsgeschiedenis en uitvoer, zoals modellen of gegevenssets opslaan.

Voor het volgen van afzonderlijke notebook cel wordt uitgevoerd, gebruikt u de `%azureml history on` magic-opdracht. Nadat u de geschiedenis inschakelt, wordt elke cel-uitvoering wordt weergegeven als een vermelding in de uitvoeringsgeschiedenis:

```
%azureml history on
from azureml.logging import get_azureml_logger
logger = get_azureml_logger()
logger.log("Cell","Load Data")
```

Als u wilt bijhouden cel uitvoeren wilt uitschakelen, gebruikt u de `%azureml history off` magic-opdracht.

U kunt de `%azureml upload` magic-opdracht voor het opslaan van model- en gegevensbestanden van uw uitvoeren. De opgeslagen objecten worden weergegeven als uitvoer in de uitvoeringsgeschiedenis weergeven:

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
- Als u wilt krijgen een beter begrip van de uitvoeringsomgeving van de Azure Machine Learning-experimenten, Zie [configureren van Azure Machine Learning experimenten-Service](experimentation-service-configuration.md).


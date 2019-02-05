---
title: Voorbeelden van Jupyter-notebooks
titleSuffix: Azure Machine Learning service
description: Jupyter-voorbeeldnotebooks vinden en gebruiken om Azure Machine Learning Service in Python te ontdekken.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 5ba555ad31545e1ae1aa822ec58b0bd22ef486ac
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55295148"
---
# <a name="use-jupyter-notebooks-to-explore-azure-machine-learning-service"></a>Jupyter-notebooks gebruiken om Azure Machine Learning Service te verkennen

We hebben een reeks Jupyter Python-notebooks ontwikkeld zodat u Azure Machine Learning Service gemakkelijk kunt verkennen. 

Ontdek hoe u de service gebruikt met de documentatie op deze site en hoe u de notebooks aan uw situatie aanpast. 

Gebruik een van de onderstaande paden om een notebookserver met deze voorbeeldnotebooks uit te voeren.  Zodra de server wordt uitgevoerd, vindt u notebooks voor zelfstudie in de map **tutorials**. U kunt ook de verschillende functies bekijken in de map **how-to-use-azureml**.


## <a name="try-azure-notebooks-free-jupyter-notebooks-in-the-cloud"></a>Azure Notebooks proberen: Gratis Jupyter-notebooks in de cloud

U kunt eenvoudig aan de slag met Azure Notebooks. De [Azure Machine Learning SDK voor Python](https://aka.ms/aml-sdk) is al voor u geïnstalleerd en geconfigureerd in [Azure Notebooks](https://notebooks.azure.com/). De installatie en toekomstige updates worden automatisch beheerd via Azure-services.
  
[!INCLUDE [aml-azure-notebooks](../../../includes/aml-azure-notebooks.md)]


## <a name="use-a-data-science-virtual-machine-dsvm"></a>Een Data Science Virtual Machine (DSVM) gebruiken

De [Azure Machine Learning SDK voor Python](https://aka.ms/aml-sdk) en de notebookserver zijn al voor u geïnstalleerd en geconfigureerd in een DSVM. 

Nadat u [een DSVM hebt gemaakt](how-to-configure-environment.md#dsvm), volgt u deze stappen op de DSVM om de notebooks uit te voeren.

[!INCLUDE [aml-dsvm-server](../../../includes/aml-dsvm-server.md)]


## <a name="use-your-own-jupyter-notebook-server"></a>Uw eigen Jupyter Notebook-server gebruiken

Volg deze stappen om een lokale Jupyter Notebook-server te maken op uw computer.

[!INCLUDE [aml-your-server](../../../includes/aml-your-server.md)]

<a name="automated-ml-setup"></a>

## <a name="automated-machine-learning-setup"></a>Setup van Gautomatiseerde Machine Learning 

_Deze stappen gelden alleen voor de notebooks in de map **how-to-use-azureml/automated-machine-learning**._

U kunt een van de bovenstaande opties gebruiken, maar u kunt ook de omgeving installeren en tegelijk een werkruimte maken met de volgende instructies. 

1. Installeer [Mini-conda](https://conda.io/miniconda.html). Kies 3.7 of hoger. Volg de aanwijzingen voor de installatie. 
   >[!NOTE]
   >U kunt een bestaande conda gebruiken mits deze versie 4.4.10 of hoger is. Gebruik `conda -V` om de versie weer te geven. U kunt een conda-versie bijwerken met deze opdracht: `conda update conda`. Het is niet nodig om specifiek mini-conda te installeren.

1. Download de voorbeeldnotebooks uit [GitHub](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning
) als een zip en pak de inhoud uit naar een lokale map. De Automated machine learning-notebooks staan de map `how-to-use-azureml/automated-machine-learning`.

1. Stel een nieuwe Conda-omgeving in. 
   1. Open een Conda-opdrachtprompt op uw lokale computer.
   
   1. Navigeer naar de bestanden die u hebt uitgepakt naar uw lokale computer.
   
   1. Open de map **automated-machine-learning**.
   
   1. Voer `automl_setup.cmd` uit in de conda-prompt voor Windows, of het `.sh`-bestand voor uw besturingssysteem. Het uitvoeren kan ongeveer tien minuten duren.

      Het installatiescript:
      + Maakt een nieuwe Conda-omgeving
      + Installeert de benodigde pakketten
      + Configureert de widget
      + Start een Jupyter-notebook
      
   >[!NOTE]
   > Het script gebruikt de naam van de conda-omgeving als een optionele parameter. De standaardnaam van de conda-omgeving is `azure_automl`. De exacte opdracht is afhankelijk van het besturingssysteem. Dit is handig als u een nieuwe omgeving maakt of een upgrade naar een nieuwe versie uitvoert. U kunt bijvoorbeeld de opdracht 'automl_setup.cmd azure_automl_sandbox' gebruiken voor het maken van een omgeving met de naam azure_automl_sandbox. 
      
1. Als het script is voltooid, wordt de startpagina van een Jupyter-notebook weergegeven in uw browser.

1. Ga naar het pad waar u de notebooks hebt opgeslagen. 

1. Open de map automated-machine-learning en open vervolgens de notebook **configuration.ipynb**. 

1. Voer de cellen in de notebook uit om Machine Learning Services Resource Provider te registreren en een werkruimte te maken.

U bent nu klaar om de notebooks die zijn opgeslagen op uw lokale computer te openen en uit te voeren.


## <a name="next-steps"></a>Volgende stappen

Verken de [GitHub-opslagplaats voor notebooks voor Azure Machine Learning Service](https://aka.ms/aml-notebooks)

Volg deze zelfstudies:
+ [Een model voor de classificatie van afbeeldingen trainen en implementeren met MNIST](tutorial-train-models-with-aml.md)

+ [Gegevens voorbereiden en geautomatiseerde machine learning gebruiken om een regressiemodel met de gegevensset voor taxi's in NYC te trainen](tutorial-data-prep.md)

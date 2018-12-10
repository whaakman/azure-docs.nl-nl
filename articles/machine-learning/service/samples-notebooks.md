---
title: Zelfstudies voor Azure Machine Learning Service in Jupyter-notebooks
description: Jupyter-voorbeeldnotebooks vinden en gebruiken om Azure Machine Learning Service in Python te ontdekken.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.openlocfilehash: 5ec010d6e0539e9ba316b48dc02110dc19e4b13e
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52883709"
---
# <a name="use-jupyter-notebooks-to-explore-azure-machine-learning-service"></a>Jupyter-notebooks gebruiken om Azure Machine Learning Service te verkennen


We hebben een reeks Jupyter Python-notebooks ontwikkeld zodat u Azure Machine Learning Service gemakkelijk kunt verkennen. 

Ontdek hoe u de service gebruikt met de documentatie op deze site en hoe u de notebooks aan uw situatie aanpast. 

## <a name="prerequisite"></a>Vereiste

Voltooi de [Python-quickstart voor Azure Machine Learning](quickstart-get-started.md) om een werkruimte te maken en Azure Notebooks te starten.

## <a name="try-azure-notebooks-free-jupyter-notebooks-in-the-cloud"></a>Probeer Azure Notebooks: gratis Jupyter-notebooks in de cloud

U kunt eenvoudig aan de slag met Azure Notebooks. De [Azure Machine Learning SDK](https://aka.ms/aml-sdk) voor Python is al voor u geïnstalleerd en geconfigureerd in Azure Notebooks. De installatie en toekomstige updates worden automatisch beheerd via Azure-services.
  
+ De **belangrijkste zelfstudie voor Notebooks** uitvoeren:
  1. Ga naar [Azure Notebooks](https://notebooks.azure.com/).
    
  1. Zoek de map **zelfstudies** in de bibliotheek **Aan de slag** die u tijdens de vereiste quickstart hebt gemaakt.
    
  1. Open de notebook die u wilt uitvoeren.
    
+ Als u **andere notebooks** wilt uitvoeren:

  1. [Importeer de voorbeeld-notebooks](https://aka.ms/aml-clone-azure-notebooks) in Azure Notebooks.

  1. Voeg een configuratiebestand voor de werkruimte toe aan de bibliotheek met een van deze methoden:
     + Kopieer het **config.json**-bestand uit de bibliotheek **Aan de slag** naar de nieuwe gekloonde bibliotheek.

     + Maak een nieuwe werkruimte met behulp van code in [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb).
    
  1. Open de notebook die u wilt uitvoeren.     


## <a name="use-a-data-science-virtual-machine-dsvm"></a>Een Data Science Virtual Machine (DSVM) gebruiken

De [Azure Machine Learning SDK voor Python](https://aka.ms/aml-sdk) en de notebookserver zijn al voor u geïnstalleerd en geconfigureerd in een DSVM. Volg deze stappen om de notebooks uit te voeren.

1. [Maak een DSVM](how-to-configure-environment.md#dsvm).

1. Kloon [de GitHub-opslagplaats](https://aka.ms/aml-notebooks).

1. Voeg een configuratiebestand voor de werkruimte toe aan de bibliotheek met een van deze methoden:
    * Kopieer het **aml_config\config.json**-bestand dat u met behulp van de vereiste quickstart hebt gemaakt naar de gekloonde map.

    * Maak een nieuwe werkruimte met behulp van code in [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb).

1. Start de notebookserver vanuit de gekloonde map.

## <a name="use-your-own-jupyter-notebook-server"></a>Uw eigen Jupyter Notebook-server gebruiken

Volg deze stappen om een lokale Jupyter Notebook-server te maken op uw computer.

1. Zorg ervoor dat u de vereiste quickstart hebt voltooid waarin u de Azure Machine Learning SDK's hebt geïnstalleerd.

1. Kloon [de GitHub-opslagplaats](https://aka.ms/aml-notebooks).

1. Voeg een configuratiebestand voor de werkruimte toe aan de bibliotheek met een van deze methoden:
    * Kopieer het **aml_config\config.json**-bestand dat u met behulp van de vereiste quickstart hebt gemaakt naar de gekloonde map.
    
    * Maak een nieuwe werkruimte met behulp van code in [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb).

1. Start de notebookserver vanuit de gekloonde map.

1. Ga naar de map met de notebook.

1. Open het notebook.

<a name="auto"></a>

## <a name="automated-ml-setup"></a>Geautomatiseerde ML-installatie 

**Deze stappen gelden alleen voor de notebooks in de map `automated-machine-learning`.**

U kunt een van de bovenstaande opties gebruiken, maar u kunt ook de omgeving installeren en tegelijk een werkruimte maken met de volgende instructies. 

1. Installeer [Mini-conda](https://conda.io/miniconda.html). Kies 3.7 of hoger. Volg de aanwijzingen voor de installatie. 
   >[!NOTE]
   >U kunt een bestaande conda gebruiken mits deze versie 4.4.10 of hoger is. Gebruik `conda -V` om de versie weer te geven. U kunt een conda-versie bijwerken met deze opdracht: `conda update conda`. Het is niet nodig om specifiek mini-conda te installeren.

1. Download de voorbeeldnotebooks uit [Github](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning
) als een zip en pak de inhoud uit naar een lokale map. De Automated machine learning-notebooks staan de map `how-to-use-azureml/automated-machine-learning`.

1. Stel een nieuwe Conda-omgeving in. 
   1. Open een Conda-opdrachtprompt op uw lokale computer.
   
   1. Navigeer naar de bestanden die u hebt uitgepakt naar uw lokale computer.
   
   1. Open de map `automated-machine-learning`.
   
   1. Voer `automl_setup.cmd` uit in de conda-prompt voor Windows, of het `.sh`-bestand voor uw besturingssysteem. Het uitvoeren kan ongeveer tien minuten duren.

      Het installatiescript:
      + Maakt een nieuwe Conda-omgeving
      + Installeert de benodigde pakketten
      + Configureert de widget
      + Start een Jupyter-notebook
      
      Het script gebruikt de naam van de conda-omgeving als een optionele parameter. De standaardnaam van de conda-omgeving is `azure_automl`. De exacte opdracht is afhankelijk van het besturingssysteem. 
      
      Als het script is voltooid, wordt de startpagina van een Jupyter-notebook weergegeven in uw browser.

1. Ga naar het pad waar u de notebooks hebt opgeslagen. 

1. Open de map automated-machine-learning en open vervolgens de notebook `configuration.ipynb`. 

1. Voer de cellen in de notebook uit om Machine Learning Services Resource Provider te registreren en een werkruimte te maken.

U bent nu klaar om de notebooks die zijn opgeslagen op uw lokale computer te openen en uit te voeren.


## <a name="next-steps"></a>Volgende stappen

Verken de [GitHub-opslagplaats voor notebooks voor Azure Machine Learning Service](https://aka.ms/aml-notebooks)

Volg deze zelfstudies:
+ [Een model voor de classificatie van afbeeldingen trainen en implementeren met MNIST](tutorial-train-models-with-aml.md)

+ [Gegevens voorbereiden en geautomatiseerde machine learning gebruiken om een regressiemodel met de gegevensset voor taxi's in NYC te trainen](tutorial-data-prep.md)

---
title: Snelstartgids-een notitieblok in de cloud uitvoeren
titleSuffix: Azure Machine Learning service
description: Aan de slag met Azure Machine Learning Service. Een beheerde notebookserver in de cloud gebruiken voor het uitproberen van uw werkruimte.  Uw werkruimte is het fundamentele blok in de cloud die u gebruiken om te experimenten, te trainen en implementeren van machine learning-modellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: quickstart
author: sdgilley
ms.author: sgilley
ms.date: 03/21/2019
ms.custom: seodec18
ms.openlocfilehash: 9cd643185fb4647b19082980edfd333c507aab8a
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59266253"
---
# <a name="quickstart-use-a-cloud-based-notebook-server-to-get-started-with-azure-machine-learning"></a>Quickstart: Een cloud-gebaseerde notebookserver aan de slag met Azure Machine Learning gebruiken

Maak een cloud-gebaseerde notebookserver en gebruik vervolgens deze code waarmee waarden zich in de Azure Machine Learning-service uit te voeren [werkruimte](concept-azure-machine-learning-architecture.md). Uw werkruimte is het fundamentele blok in de cloud die u gebruiken om te experimenten, te trainen en implementeren van machine learning-modellen met Machine Learning. 

Deze quickstart laat zien hoe u een cloudresource maken in uw Azure Machine Learning-werkruimte, geconfigureerd met de Python-omgeving die nodig zijn om uit te voeren van Azure Machine Learning. Als u wilt gebruiken in plaats daarvan uw eigen omgeving, Zie [Quick Start: Gebruik uw eigen notebook-server aan de slag met Azure Machine Learning](quickstart-run-local-notebook.md).  
 
In deze snelstart voert u de volgende acties uit:

* Maken van een werkstation
* Een Jupyter-Notebook-server op uw werkstation starten
* Open een laptop met code voor een schatting van pi en logboeken voor fouten bij elke iteratie.
* Uitvoeren van het notitieblok.
* Bekijk de geregistreerde foutwaarden in uw werkruimte.  Dit is een voorbeeld van hoe de werkruimte u kan helpen bij het bijhouden van informatie die in een script wordt gegenereerd. 

Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

## <a name="prerequisites"></a>Vereisten

1. [Een Azure Machine Learning-werkruimte maken](setup-create-workspace.md#portal) als u dit niet hebt.

1. Open de werkruimte in de [Azure-portal](https://portal.azure.com/).  Als u niet zeker weet hoe u uw werkruimte niet vinden in de portal, Zie hoe u [vinden van uw werkruimte](how-to-manage-workspace.md#view).

## <a name="create-a-workstation"></a>Maken van een werkstation 

Een laptop-werkstation kunt u een cloud-platform voor Jupyter-notitieblokken die vooraf is geconfigureerd met alles wat die u nodig hebt om uit te voeren van Azure Machine Learning-service. U kunt dit platform aan de slag met behulp van Jupyter notebooks maken vanuit uw werkruimte.

1. Selecteer op de pagina van uw werkruimte in Azure portal **laptop werkstation** aan de linkerkant.

1. Selecteer **notitieblokken maken in een werkstation met Azure Machine Learning (Preview)**

   ![De werkruimte verkennen](./media/quickstart-run-cloud-notebook/explore-aml.png)

1. De **laptops** sectie bevat een overzicht van alle cloud-gebaseerde notebook-servers die beschikbaar zijn in uw werkruimte.  Hier kunt u ook deze resources beheren en te verwijderen wanneer ze niet langer nodig zijn. 

1. Selecteer **toevoegen werkstation** te maken van een laptop-werkstation.

     ![Selecteer toevoegen-werkstation](./media/quickstart-run-cloud-notebook/add-workstation.png)

1. In de sectie toevoegen Notebook werkstation geven uw werkstation een **Compute naam** en selecteer een **Rekentype**. Selecteer vervolgens **Maken**.

    ![Maken van nieuwe werkstation](media/quickstart-run-cloud-notebook/create-new-workstation.png)

    > [!NOTE]
    > Uw werkstation duurt ongeveer twee minuten om te maken. Wanneer u klaar bent, worden de statusupdates van de 'Running' en koppelingen naar Jupyter en Jjupyterlab weergegeven.

## <a name="launch-jupyter-web-interface"></a>Jupyter-webinterface starten

Nadat uw werkstation is gemaakt, gebruik u de sectie laptops om te starten van de Jupyter-webinterface.

* Selecteer **Jupyter** of **Jupyter Lab** in de **starten** kolom voor uw werkstation.

    ![Jupyter-notebookserver starten](./media/quickstart-run-cloud-notebook/start-notebook-server.png)

    Hiermee wordt de notebook-server wordt gestart en wordt de startpagina van de server in een nieuw browsertabblad geopend.  De server bevat voorbeeldnotitieblokken die u kunt gebruiken om aan de slag met Azure Machine Learning-service.

### <a name="run-the-notebook"></a>Het notitieblok uitvoeren

Voer een laptop die u maakt een schatting van pi en de fout vastgelegd in uw werkruimte.

1. Selecteer **01.run experiment.ipynb** om het notebook te openen.

1. In het statusgebied ziet u een melding dat u moet wachten tot de kernel is gestart.  Zodra de kernel gereed is, verdwijnt het bericht.

    ![Wachten tot de kernel is gestart](./media/quickstart-run-cloud-notebook/wait-for-kernel.png)

1. Nadat de kernel is gestart, voert u de cellen één voor één uit met behulp van **Shift + Enter**. U kunt ook **Cellen** > **Alles uitvoeren** selecteren om het hele notebook uit te voeren. Wanneer er een asterisk (__*__) naast een cel, de cel nog steeds wordt uitgevoerd. Wanneer de code voor die cel is voltooid, verschijnt er een getal.  

Nadat u de uitvoering van alle cellen in de notebook hebt voltooid, kunt u de vastgelegde waarden weergeven in uw werkruimte.

## <a name="view-logged-values"></a>Logboekwaarden weergeven

1. De uitvoer van de cel `run` bevat een koppeling terug naar de Microsoft Azure-portal om de resultaten van het experiment in uw werkruimte weer te geven. 

    ![Experimenten bekijken](./media/quickstart-run-cloud-notebook/view-exp.png)

1. Klik op de **koppelen aan Azure-portal** om informatie over het uitvoeren in uw werkruimte weer te geven.  Met deze koppeling wordt uw werkruimte in de Microsoft Azure-portal geopend.

1. De grafieken van vastgelegde waarden die u ziet, zijn automatisch in de werkruimte gemaakt. Wanneer u meerdere waarden met dezelfde naamparameter vastlegt, wordt automatisch een grafiek voor u gegenereerd.

   ![Geschiedenis weergeven](./media/quickstart-run-cloud-notebook/web-results.png)

Omdat er in de code om pi te schatten gebruik wordt gemaakt van willekeurige waarden, worden er in uw plots andere waarden weergegeven.  

## <a name="clean-up-resources"></a>Resources opschonen 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

U kunt de resourcegroep ook bewaren en slechts één werkruimte verwijderen. Bekijk de eigenschappen van de werkruimte en selecteer **Verwijderen**.


## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt voltooid u de volgende:

* Maken van een werkstation
* Een Jupyter-Notebook-server op uw werkstation starten
* Open een laptop met code voor een schatting van pi en logboeken voor fouten bij elke iteratie.
* Uitvoeren van het notitieblok.
* Bekijk de geregistreerde foutwaarden in uw werkruimte.  Dit is een voorbeeld van hoe de werkruimte u kan helpen bij het bijhouden van informatie die in een script wordt gegenereerd. 

Voor een diepgaande werkstroomervaring volgt u de zelfstudies over Machine Learning om een ​​model te trainen en te implementeren:  

> [!div class="nextstepaction"]
> [Zelfstudie: Een installatiekopie van classificatie-model te trainen](tutorial-train-models-with-aml.md)

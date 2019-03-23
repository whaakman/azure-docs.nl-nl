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
ms.openlocfilehash: 761ecefa768914e0c27f46d37050743e4f1f60ff
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58362437"
---
# <a name="quickstart-use-a-cloud-based-notebook-server-to-get-started-with-azure-machine-learning"></a>Quickstart: Een cloud-gebaseerde notebookserver aan de slag met Azure Machine Learning gebruiken

In dit artikel gebruikt u Azure-notitieblokken code die wordt vastgelegd in de Azure Machine Learning-service uit te voeren [werkruimte](concept-azure-machine-learning-architecture.md). Uw werkruimte is het fundamentele blok in de cloud die u gebruiken om te experimenten, te trainen en implementeren van machine learning-modellen met Machine Learning. 

In deze snelstart wordt gebruikgemaakt van cloudresources en er is geen installatie vereist. Als u wilt gebruiken in plaats daarvan uw eigen omgeving, Zie [Quick Start: Gebruik uw eigen notebook-server aan de slag met Azure Machine Learning](quickstart-run-local-notebook.md).  
 
In deze snelstart voert u de volgende acties uit:

* Verbinding maken met uw werkruimte met Python in een Jupyter-notebook. De notebook bevat code voor een schatting van pi en logboeken voor fouten bij elke iteratie. 
* Bekijk de geregistreerde foutwaarden in uw werkruimte.

Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

## <a name="prerequisite"></a>Vereiste

1. [Een Azure Machine Learning-werkruimte maken](setup-create-workspace.md#portal) als u dit niet hebt.

1. Open de werkruimte in de [Azure-portal](https://portal.azure.com/).  Zie hoe u [vinden van uw werkruimte](how-to-manage-workspace.md#view).

## <a name="use-your-workspace"></a>Gebruik van uw werkruimte

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2F9Ad]



Meer informatie over hoe u een werkruimte kunt beheren van machine learning-scripts. In deze sectie voert u de volgende stappen uit:

* Een notitieblok openen in Azure Notebooks.
* Code uitvoeren die enkele vastgelegde waarden maakt.
* De vastgelegde waarden in uw werkruimte weergeven.

Dit is een voorbeeld van hoe de werkruimte u kan helpen bij het bijhouden van informatie die in een script wordt gegenereerd. 

### <a name="open-a-notebook"></a>Een notitieblok openen 

[Azure Notebooks](https://notebooks.azure.com) biedt een gratis cloudplatform voor Jupyter-notebooks dat vooraf is geconfigureerd met alles wat u nodig hebt om Machine Learning uit te voeren. U kunt dit platform aan de slag met behulp van de werkruimte van uw Azure Machine Learning-service starten vanuit uw werkruimte.

1. Selecteer op de pagina overzicht van werkruimte **ophalen gestart Azure notitieblokken** om te proberen uw eerste experiment in Azure-notitieblokken.  Azure Notebooks is een afzonderlijke service waarmee u Jupyter-notebooks gratis kunt uitvoeren in de cloud.  Wanneer u deze koppeling naar de service gebruikt, wordt informatie over hoe u verbinding maakt met uw werkruimte toegevoegd aan de bibliotheek die u in Azure Notebooks maakt.

   ![De werkruimte verkennen](./media/quickstart-run-cloud-notebook/explore-aml.png)

1. Meld u aan bij Azure Notebooks.  Meld u aan met hetzelfde account dat u hebt gebruikt om u aan te melden bij de Microsoft Azure-portal. Mogelijk moet in uw organisatie [beheerderstoestemming](https://notebooks.azure.com/help/signing-up/work-or-school-account/admin-consent) worden verkregen voordat u zich kunt aanmelden.

1. Nadat u zich hebt aangemeld, wordt er een nieuw tabblad geopend en wordt er een `Clone Library`-prompt weergegeven. Wanneer u deze bibliotheek kloont, wordt een verzameling notebooks en andere bestanden in uw Azure Notebooks-account geladen.  Met deze bestanden kunt u de mogelijkheden van Azure Machine Learning verkennen.

1. Schakel het selectievakje **Openbaar** uit zodat u uw werkruimtegegevens niet met anderen deelt.

1. Selecteer **Klonen**.

   ![Een bibliotheek klonen](./media/quickstart-run-cloud-notebook/clone.png)

1. Als u ziet dat de projectstatus is gestopt, klikt u op **Uitvoeren op gratis computer** om de gratis notebookserver te gebruiken.

    ![Uitvoeren op gratis compute](./media/quickstart-run-cloud-notebook/run-project.png)

### <a name="run-the-notebook"></a>Het notitieblok uitvoeren

In de lijst met bestanden voor dit project, ziet u het bestand `config.json`. Dit configuratiebestand bevat informatie over de werkruimte die u hebt gemaakt in de Microsoft Azure-portal.  Met dit bestand kan uw code verbinding maken met en gegevens toevoegen aan uw werkruimte.

1. Selecteer **01.run experiment.ipynb** om het notebook te openen.

1. In het statusgebied ziet u een melding dat u moet wachten tot de kernel is gestart.  Zodra de kernel gereed is, verdwijnt het bericht.

    ![Wachten tot de kernel is gestart](./media/quickstart-run-cloud-notebook/wait-for-kernel.png)

1. Nadat de kernel is gestart, voert u de cellen één voor één uit met behulp van **Shift + Enter**. U kunt ook **Cellen** > **Alles uitvoeren** selecteren om het hele notebook uit te voeren. Als er een sterretje, __*__, naast een cel staat, wordt de cel nog uitgevoerd. Wanneer de code voor die cel is voltooid, verschijnt er een getal. 

1. Volg de instructies in het notebook om uw Azure-abonnement te verifiëren.

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

U hebt de resources gemaakt die u nodig hebt om mee te experimenteren en om modellen te implementeren. U hebt ook code uitgevoerd in een notitieblok. U hebt ook de uitvoeringsgeschiedenis van die code in uw werkruimte in de cloud onderzocht.

Voor een diepgaande werkstroomervaring volgt u de zelfstudies over Machine Learning om een ​​model te trainen en te implementeren:  

> [!div class="nextstepaction"]
> [Zelfstudie: Een model trainen voor de classificatie van afbeeldingen](tutorial-train-models-with-aml.md)

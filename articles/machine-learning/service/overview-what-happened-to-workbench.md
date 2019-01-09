---
title: Wat is er gebeurd met Machine Learning Workbench?
titleSuffix: Azure Machine Learning service
description: Meer informatie over wat er met de Machine Learning Workbench-toepassing is gebeurd, wat er in de Azure Machine Learning-service is veranderd en wat de ondersteuningstijdlijn is.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: j-martens
ms.author: jmartens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: ff6b61874363bbc869bd509174e58640a2487f56
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/28/2018
ms.locfileid: "53811304"
---
# <a name="whats-happening-to-machine-learning-workbench-in-azure-machine-learning-service"></a>Wat gebeurt er met Machine Learning Workbench in Azure Machine Learning Service?

De Azure Machine Learning Workbench-toepassing en een aantal andere vroege functies zijn afgeschaft en vervangen in de versie van september 2018 om plaats te maken voor een verbeterde [architectuur](concept-azure-machine-learning-architecture.md). De nieuwe versie bevat veel belangrijke updates op basis van feedback van klanten, om uw ervaring te verbeteren. De kernfunctionaliteit voor experimentele uitvoeringen tot modelimplementatie is niet gewijzigd. U kunt nu echter de robuuste <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> en de [Azure CLI](reference-azure-machine-learning-cli.md) gebruiken om uw machine learning-taken en -pijplijnen uit te voeren.  

In dit artikel leest u wat er is veranderd en hoe dit van invloed is op uw bestaande werk met de Azure Machine Learning Workbench en de bijbehorende API's.

## <a name="what-changed"></a>Wat is er veranderd?

De meest recente versie van Azure Machine Learning Service bevat de volgende functies:
+ Een [vereenvoudigd model voor Azure-resources](concept-azure-machine-learning-architecture.md).
+ Een [nieuwe gebruikersinterface in de portal](how-to-track-experiments.md) voor het beheren van uw experimenten en compute-doelen.
+ Een nieuwe, uitgebreidere Python-<a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>.
+ De nieuwe, uitgebreide [Azure CLI-extensie](reference-azure-machine-learning-cli.md) voor machine learning.

De [architectuur](concept-azure-machine-learning-architecture.md) is vernieuwd voor meer gebruiksgemak. In plaats van meerdere Azure-resources en -accounts hebt u alleen een [werkruimte voor Azure Machine Learning service](concept-azure-machine-learning-architecture.md#workspace) nodig. U kunt snel werkruimten maken in de [Azure-portal](quickstart-get-started.md). Met een werkruimte kunnen meerdere gebruikers rekendoelen voor training en implementatie, modelexperimenten, Docker-installatiekopieën, geïmplementeerde modellen, enzovoort, opslaan.

De huidige versie bevat nieuwe en verbeterde CLI- en SDK-clients, maar de workbench-desktoptoepassing zelf is afgeschaft. U kunt uw experimenten nu controleren in het [werkruimtedashboard in Azure Portal](how-to-track-experiments.md#view-the-experiment-in-the-azure-portal). Gebruik het dashboard om uw experimentgeschiedenis op te halen, de compute-doelen die zijn gekoppeld aan uw werkruimte te beheren, uw modellen en Docker-installatiekopieën te beheren en zelfs om webservices te implementeren.

## <a name="how-do-i-migrate"></a>Hoe migreer ik?

De meeste artefacten die in de eerdere versie van Azure Machine Learning Service zijn gemaakt, worden opgeslagen in uw eigen lokale cloud of cloudopslag. Deze artefacten worden nooit verwijderd. Als u wilt migreren, moet u de artefacten opnieuw registreren bij de bijgewerkte Azure Machine Learning service. Lees dit [artikel over migreren](how-to-migrate.md) voor informatie over wat u kunt migreren en hoe u dat doet.

<a name="timeline"></a>

## <a name="support-timeline"></a>Ondersteuningstijdlijn

U kunt uw Machine Learning Experimentation- en Modelbeheer-accounts en de toepassing Machine Learning Workbench na september 2018 blijven gebruiken. Ondersteuning voor de volgende resources wordt in de drie tot vier maanden na de release stapsgewijs opgeheven. U kunt de documentatie voor de oude functies nog vinden in het [gedeelte Resources](../desktop-workbench/tutorial-classifying-iris-part-1.md) onderaan de inhoudsopgave.

|Buiten gebruik stellen&nbsp;fase|Ondersteuningsdetails voor vroegere functies|
|:---:|----------------|
|4 december 2018|De mogelijkheid om een Azure Machine Learning Experimentation-account en Modelbeheer-account te maken in Azure Portal en vanaf de CLI, is beëindigd. De mogelijkheid om Machine Learning-rekenomgevingen te maken vanaf de CLI is ook beëindigd. Als u nog een account hebt, blijven de CLI en de bureaubladversie van Machine Learning Workbench tijdens deze fase werken.|
|9 januari 2019|Ondersteuning voor de rest wordt op deze datum beëindigd. Voorbeelden hiervan zijn de resterende API's en de bureaubladversie van Machine Learning Workbench.|

[Migreer vandaag nog](how-to-migrate.md). Alle nieuwste mogelijkheden zijn beschikbaar met de nieuwe <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>, de [CLI](reference-azure-machine-learning-cli.md) en de [portal](quickstart-get-started.md).

## <a name="what-about-run-histories"></a>Hoe zit het met uitvoeringsgeschiedenissen?

Uitvoeringsgeschiedenissen blijven nog een tijdje toegankelijk. Wanneer u klaar bent om naar de bijgewerkte versie van Azure Machine Learning Service over te stappen, kunt u deze uitvoeringsgeschiedenissen exporteren als u een kopie wilt bewaren.

Uitvoeringsgeschiedenissen heten in de huidige versie **experimenten**. U kunt de experimenten van uw model verzamelen en ze verkennen met behulp van de SDK, de CLI of Azure Portal.

Het werkruimtedashboard van de portal wordt alleen ondersteund in Microsoft Edge, Chrome en Firefox:

[![Online portal](./media/overview-what-happened-to-workbench/image001.png)] (./media/overview-what-happened-to-workbench/image001.png#lightbox)


## <a name="can-i-still-prep-data"></a>Kan ik nog steeds gegevens voorbereiden?

Uw bestaande gegevensvoorbereidingsbestanden zijn niet overdraagbaar naar de nieuwste versie omdat Machine Learning Workbench niet meer bestaat. U kunt uw gegevens echter nog steeds voorbereiden voor modellering.  

Met gegevenssets van elke grootte kunt u de [Azure Machine Learning Data Prep SDK](http://aka.ms/data-prep-sdk) gebruiken om uw gegevens snel voor te bereiden voorafgaand aan het modelleren door Python-code te schrijven. 

U kunt [deze zelfstudie](tutorial-data-prep.md) volgen voor meer informatie over het gebruik van de Azure Machine Learning Data Prep SDK.

## <a name="will-projects-persist"></a>Blijven projecten behouden?

U verliest geen code of werk. Projecten zijn in de oudere versie cloudentiteiten met een lokale map. In de nieuwste versie kunt u lokale mappen koppelen aan de werkruimte van Azure Machine Learning Service met behulp van een lokaal configuratiebestand. Bekijk een [diagram van de meest recente architectuur](concept-azure-machine-learning-architecture.md).

Veel van de projectinhoud staat al op uw lokale computer. U hoeft alleen een configuratiebestand in die map te maken en ernaar te verwijzen in uw code om het aan uw werkruimte te koppelen. Informatie over [het migreren van uw bestaande projecten](how-to-migrate.md#projects).

Ontdek hoe u aan de slag kunt gaan [in Python met de belangrijkste SDK](quickstart-create-workspace-with-python.md) of via [Azure Portal](quickstart-get-started.md).

## <a name="what-about-my-registered-models-and-images"></a>Hoe zit het met mijn geregistreerde modellen en installatiekopieën?
 
De modellen die u in uw oude modelregister hebt geregistreerd, moeten worden gemigreerd naar uw nieuwe werkruimte als u ze wilt blijven gebruiken. Voor het migreren van uw modellen [downloadt u de modellen en registreert u deze opnieuw](how-to-migrate.md) in uw nieuwe werkruimte. 

De installatiekopieën die u in uw oude installatiekopieregister hebt gemaakt, moeten opnieuw worden gemaakt in de nieuwe werkruimte om ze te blijven gebruiken. U kunt deze installatiekopieën opnieuw maken door de sectie [Installatiekopie configureren en maken](how-to-deploy-and-where.md#configureimage) te volgen. 

## <a name="what-about-deployed-web-services"></a>Hoe zit het met geïmplementeerde webservices?

De modellen die u als webservices hebt geïmplementeerd met behulp van uw Machine Learning Modelbeheer-account, blijven werken zolang Azure Container Service wordt ondersteund. Deze webservices werken ook na afloop van de ondersteuning voor Machine Learning Modelbeheer-accounts. Wanneer de ondersteuning voor de oude CLI echter wordt beëindigd, geldt dat ook voor de mogelijkheid om deze webservices te beheren.

In de nieuwere versie worden modellen als webservices geïmplementeerd in Azure Container Instances- of AKS-clusters (Azure Kubernetes Service). U kunt ook in FPGA's en Azure IoT Edge implementeren. Zie het artikel [Hoe implementeren en waar](how-to-deploy-and-where.md) voor meer informatie. U kunt uw modellen opnieuw implementeren met de nieuwe SDK of CLI, zonder dat u uw scoring-bestanden, afhankelijkheden of schema's hoeft te wijzigen. 

## <a name="what-about-the-old-sdk-and-cli"></a>Hoe zit het met de oude SDK en CLI?

Ja, deze blijven gewoon werken tot januari. Zie de voorgaande [tijdlijn](#timeline). Het is raadzaam om uw nieuwe experimenten en modellen met de meest recente SDK of CLI te maken.

Met de nieuwe Python-SDK in de nieuwste versie kunt u met Azure Machine Learning Service in elke Python-omgeving communiceren. Informatie over het installeren van de meest recente <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>. U kunt ook de bijgewerkte [Azure Machine Learning CLI-extensie](reference-azure-machine-learning-cli.md) gebruiken met de uitgebreide set `az ml`-opdrachten om in elke opdrachtregelomgeving met de service te communiceren, waaronder Azure Cloud Shell.

## <a name="what-about-azure-machine-learning-for-visual-studio-code"></a>Hoe zit het met Azure Machine Learning voor Visual Studio Code?

In deze nieuwste release is Azure Machine Learning voor Visual Studio Code uitgebreid en verbeterd voor gebruik met de eerdergenoemde nieuwe functies.

[![Azure Machine Learning voor Visual Studio Code](./media/overview-what-happened-to-workbench/vscode.png)] (./media/overview-what-happened-to-workbench/vscode-big.png#lightbox)

## <a name="what-about-domain-packages"></a>Hoe zit het met domeinpakketten?

De domeinpakketten voor [Computer Vision, tekstanalyse en prognose](../desktop-workbench/reference-python-package-overview.md) kunnen niet worden gebruikt met de meest recente versie van Azure Machine Learning. U kunt echter nog steeds Computer Vision-, tekst- en prognosemodellen bouwen en trainen met de meest recente Azure Machine Learning <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> voor Python. Voor informatie over het migreren van bestaande modellen die zijn gebouwd met Computer Vision-, tekstanalyse- en prognosepakketten, kunt u contact opnemen via [AML-Packages@microsoft.com](mailto:AML-Packages@microsoft.com).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [meest recente architectuur voor Azure Machine Learning Service](concept-azure-machine-learning-architecture.md). Probeer een van de snelstarts of zelfstudies:

* [Wat is Azure Machine Learning Service?](overview-what-is-azure-ml.md)
* [Snelstart: een werkruimte maken met Python](quickstart-get-started.md)
* [Zelfstudie: een model trainen](tutorial-train-models-with-aml.md)

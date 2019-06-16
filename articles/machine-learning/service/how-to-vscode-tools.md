---
title: Visual Studio Code gebruiken voor machine learning
titleSuffix: Azure Machine Learning service
description: Informatie over het installeren van Azure Machine Learning voor Visual Studio Code en een eenvoudig experiment maken in Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: shwinne
author: swinner95
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 70f9c34957b977aff9fc6211bf79415ed9abe255
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66016513"
---
# <a name="get-started-with-azure-machine-learning-for-visual-studio-code"></a>Aan de slag met Azure Machine Learning voor Visual Studio Code

In dit artikel leert u hoe u de extensie voor Azure Machine Learning voor Visual Studio Code gebruiken om te trainen en implementeren van machine learning en deep learning-modellen.

De [Azure Machine Learning-service](overview-what-is-azure-ml.md) biedt ondersteuning voor experimenten die u lokaal en op afstand uitvoeren van-doelen COMPUTE. Voor elke experiment, u kunt bijhouden van meerdere wordt uitgevoerd als u wilt vaak iteratief probeert verschillende technieken en hyperparameters. Azure Machine Learning kunt u aangepaste metrische gegevens bijhouden en experimenteren wordt uitgevoerd, data science reproduceerbaarheid en controleerbaarheid inschakelen.

U kunt ook deze modellen implementeren voor uw behoeften testen en productie.

## <a name="prerequisites"></a>Vereisten

+ Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer de [gratis of betaalde versie van de service Azure Machine Learning](https://aka.ms/AMLFree).

+ Visual Studio Code moet worden geïnstalleerd. Visual Studio Code is een toegankelijke, krachtige broncode-editor die wordt uitgevoerd op het bureaublad. Het wordt geleverd met ingebouwde ondersteuning voor Python en andere programmeertalen. Als u Visual Studio Code nog niet hebt geïnstalleerd [meer informatie over](https://code.visualstudio.com/docs/setup/setup-overview).

+ [Python 3.5 of hoger installeren](https://www.anaconda.com/download/).


## <a name="install-the-extension-for-azure-machine-learning-for-visual-studio-code"></a>De extensie voor Azure Machine Learning voor Visual Studio Code installeren

Wanneer u de Azure Machine Learning-extensie installeert, worden twee meer extensies automatisch geïnstalleerd (als u toegang tot internet hebt). Ze de [Azure-accountextensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) en de [Microsoft Python-extensie](https://marketplace.visualstudio.com/items?itemName=ms-python.python).

Als u wilt werken met Azure Machine Learning, moet u Visual Studio Code omzetten in een Python-geïntegreerde ontwikkelomgeving (IDE). U moet de Microsoft Python-extensie voor het gebruik van [Python in Visual Studio Code](https://code.visualstudio.com/docs/languages/python). Deze extensie wordt automatisch geïnstalleerd met de Azure Machine Learning-extensie. De extensie maakt Visual Studio Code een uitstekende IDE, en het werkt in elk besturingssysteem met tal van Python-interpreters van. De Microsoft Python-extensie gebruikt alle van de kracht van Visual Studio Code voor het automatisch aanvullen, IntelliSense, linten, foutopsporing en testen van de eenheden. De extensie kunt u gemakkelijk schakelen tussen de Python-omgevingen, met inbegrip van virtuele en conda-omgevingen. Zie voor meer informatie over het bewerken, uitvoeren en debuggen van Python-code, de [Hallo wereld-zelfstudie voor Python](https://code.visualstudio.com/docs/python/python-tutorial).

De Azure Machine Learning-extensie installeren:

1. Open Visual Studio Code.

1. In een webbrowser, gaat u naar [Azure Machine Learning voor Visual Studio Code-extensie (preview)](https://aka.ms/vscodetoolsforai).

1. Selecteer op deze website, **installeren**. 

1. Selecteer op het tabblad extensie **installeren**.

1. Een Welkom tabblad voor de extensie wordt geopend in Visual Studio Code, en het Azure-symbool (die worden beschreven in het rood in de volgende schermafbeelding) wordt toegevoegd aan de activiteitenbalk.

   ![Azure-pictogram op de balk van de activiteit Visual Studio Code](./media/vscode-tools-for-ai/azure-activity-bar.png)

1. Selecteer in het dialoogvenster **aanmelden** en volg de aanwijzingen om te verifiëren met Azure. 
   
   De extensie Azure-Account, die samen met de Azure Machine Learning voor Visual Studio Code-extensie is geïnstalleerd, kunt u verifiëren met uw Azure-account. Voor een lijst van opdrachten, Zie de pagina voor de [Azure-accountextensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

> [!Tip] 
> Bekijk de [IntelliCode-extensie voor Visual Studio Code (preview)](https://go.microsoft.com/fwlink/?linkid=2006060). IntelliCode biedt een verscheidenheid aan functies AI-ondersteuning voor IntelliSense in Python, zoals het afleiden van de meest relevante autocompletions op basis van de huidige context van de code.

## <a name="install-the-azure-machine-learning-sdk"></a>De Azure Machine Learning-SDK installeren

1. Zorg ervoor dat Python 3.5 of hoger is geïnstalleerd en wordt herkend door Visual Studio Code. Als u deze nu installeren, start u Visual Studio Code opnieuw en [selecteert u een Python-interpreter](https://code.visualstudio.com/docs/python/python-tutorial).

1. In de geïntegreerde terminal-venster, geeft u de Python-interpreter te gebruiken. Of selecteer Enter om te gebruiken de standaard Python-interpreter.

   ![Kies de-interpreter](./media/vscode-tools-for-ai/python.png)

1. In de rechterbenedenhoek van het venster een melding weergegeven dat aangeeft dat de [SDK van Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) automatisch wordt geïnstalleerd. De zojuist gemaakte Python-omgeving is lokaal en particuliere en heeft de Visual Studio Code-vereisten voor het werken met de Azure Machine Learning-service.

   ![De Azure Machine Learning-SDK voor Python installeren](./media/vscode-tools-for-ai/runtimedependencies.png)

## <a name="get-started-with-azure-machine-learning"></a>Aan de slag met Azure Machine Learning

Voordat u training en implementeren van machine learning-modellen in Visual Studio Code, die u wilt maken een [werkruimte van Azure Machine Learning-service](concept-workspace.md) in de cloud. Deze werkruimte bevat uw modellen en resources. 

Een werkruimte maken en toevoegen van uw eerste experiment:

1. Selecteer het Azure-pictogram op de balk van de activiteit Visual Studio Code. De Azure Machine Learning-zijbalk wordt weergegeven.

   [![Een werkruimte maken](./media/vscode-tools-for-ai/CreateaWorkspace.gif)](./media/vscode-tools-for-ai/CreateaWorkspace.gif#lightbox)


1. Met de rechtermuisknop op uw Azure-abonnement en selecteer **werkruimte maken**. Er wordt een lijst weergegeven. In het voorbeeld GIF, de naam van het abonnement is **gratis proefversie**, en de werkruimte is **TeamWorkspace**. 

1. Selecteer een resourcegroep in de lijst of maak een nieuwe met behulp van de wizard in de command palette.

1. Typ in het veld een unieke en duidelijke naam voor uw nieuwe werkruimte. In het voorbeeld, de werkruimte met de naam **TeamWorkspace**.

1. Selecteer Enter om de nieuwe werkruimte te maken. Deze wordt weergegeven in de consolestructuur onder de naam van het abonnement.

1. Met de rechtermuisknop op de **Experiment** knooppunt, en kies **Experiment maken** in het contextmenu.  Experimenten bijhouden van uw uitvoerbewerkingen met Azure Machine Learning.

1. Voer in het veld een naam voor uw experiment. In de schermafbeeldingen, het experiment heet **MNIST**.
 
1. Selecteer Enter om de nieuw experiment te maken. Het experiment wordt weergegeven in de consolestructuur onder de Werkruimtenaam van de.

1. In een werkruimte, kunt u een experiment om in te stellen als met de rechtermuisknop de **Active** experimenteren. De **Active** experiment is uw huidige experiment. De map openen in Visual Studio Code wordt gekoppeld aan dit experiment in de cloud. Deze map moet uw lokale Python-scripts bevatten.

Nu worden uw belangrijke metrische gegevens opgeslagen in de geschiedenis van het experiment. Op dezelfde manier worden de modellen die u trainen automatisch geüpload naar Azure Machine Learning en opgeslagen samen met uw experiment metrische gegevens en Logboeken. 

[![Koppelen van een map in Visual Studio Code](./media/vscode-tools-for-ai/CreateAnExperiment.gif)](./media/vscode-tools-for-ai/CreateAnExperiment.gif#lightbox)


## <a name="create-and-manage-compute-targets"></a>Maken en beheren van compute-doelen

Met Azure Machine Learning voor Visual Studio Code, kunt u uw gegevens voorbereiden, modellen trainen en deze implementeren, zowel lokaal als op de externe compute-doelen.

De extensie biedt ondersteuning voor verschillende externe compute-doelen voor Azure Machine Learning. Zie voor meer informatie, de volledige lijst met ondersteunde [compute-doelen voor Azure Machine Learning](how-to-set-up-training-targets.md).

### <a name="create-compute-targets-for-azure-machine-learning-in-visual-studio-code"></a>Compute-doelen voor Azure Machine Learning in Visual Studio Code maken

Een compute-doel maken:

1. Selecteer het Azure-pictogram op de balk van de activiteit Visual Studio Code. De Azure Machine Learning-zijbalk wordt weergegeven.

2. Vouw in de structuurweergave wordt weergegeven, uw Azure-abonnement en de werkruimte van Azure Machine Learning-service. In het volgende voorbeeld, de naam van het abonnement is **gratis proefversie**, en de werkruimte is **TeamWorkspace**. 

3. Onder het werkruimteknooppunt met de rechtermuisknop op de **Compute** knooppunt en kies **maken Compute**.

4. Kies het doeltype compute in de lijst. 

5. Selecteer op het opdrachtenpalet, de grootte van een virtuele machine.

6. Voer een naam op voor de compute-doel in het veld op het opdrachtenpalet. 

7. Geef in het configuratiebestand JSON die wordt geopend op een nieuw tabblad Geavanceerde eigenschappen. U kunt eigenschappen op, zoals een maximum aantal knooppunten opgeven.

8. Wanneer u klaar bent met het configureren van uw compute-doel in de rechterbenedenhoek van het venster, selecteert u **indienen**.

Hier volgt een voorbeeld van het maken van een Azure Machine Learning-Computing (AMLCompute):

[![AML compute maken in Visual Studio Code](./media/vscode-tools-for-ai/CreateARemoteCompute.gif)](./media/vscode-tools-for-ai/CreateARemoteCompute.gif#lightbox)

#### <a name="the-run-configuration-file"></a>Het configuratiebestand uitvoeren

De Visual Studio Code-extensie maakt automatisch een lokale compute-doel en voer configuraties voor uw lokale en docker-omgeving op de lokale computer. Hier vindt u de configuratiebestanden voor uitvoeren onder het knooppunt gekoppelde compute-doel. 

## <a name="train-and-tune-models"></a>Modellen trainen en afstemmen

Azure Machine Learning voor Visual Studio Code (preview) gebruiken om snel Voortborduren op uw code, doorlopen en fouten opsporen en uw oplossing voor broncodebeheer gebruiken. 

Uw experiment lokaal met behulp van Azure Machine Learning uitvoeren:

1. Selecteer het Azure-pictogram op de balk van de activiteit Visual Studio Code. De Azure Machine Learning-zijbalk wordt weergegeven.

1. Vouw in de structuurweergave wordt weergegeven, uw Azure-abonnement en de werkruimte van Azure Machine Learning-service. 

1. Vouw onder het werkruimteknooppunt, de **Compute** knooppunt en met de rechtermuisknop op de **uitvoeren Config** van de berekening die u wilt gebruiken. 

1. Selecteer **Experiment uit te voeren**.

1. Selecteer het script dat uit te voeren vanuit de Verkenner. 

1. Selecteer **weergave Experiment uitvoeren** om te zien van de geïntegreerde Azure Machine Learning-portal voor de uitvoering van uw controleren en Zie uw getrainde modellen.

Hier volgt een voorbeeld van hoe u een experiment lokaal uitvoeren:

[![Een experiment lokaal uitvoeren](./media/vscode-tools-for-ai/RunExperimentLocally.gif)](./media/vscode-tools-for-ai/RunExperimentLocally.gif#lightbox)

### <a name="use-remote-computes-for-experiments-in-visual-studio-code"></a>Externe berekeningen voor experimenten in Visual Studio Code gebruiken

Voor het gebruik van een externe compute-doel voor de training, moet u een uitvoeren-configuratiebestand maken. Dit bestand vertelt Azure Machine Learning-niet alleen waar u uw experiment uit te voeren, maar ook hoe u de omgeving voorbereidt.

#### <a name="the-conda-dependencies-file"></a>Het conda-afhankelijkheidsbestand

Standaard wordt een nieuwe conda-omgeving voor u gemaakt en de afhankelijkheden van uw installatie worden beheerd. U moet echter opgeven de afhankelijkheden en hun versies in de *aml_config/conda_dependencies.yml* bestand. 

Het volgende fragment van het standaard *aml_config/conda_dependencies.yml* geeft `tensorflow=1.12.0`. Als u de versie van de afhankelijkheid niet opgeeft, wordt de meest recente versie worden gebruikt. U kunt extra afhankelijkheden toevoegen in het configuratiebestand.

```yaml
# The dependencies defined in this file will be automatically provisioned for runs with userManagedDependencies=False.

name: project_environment
dependencies:
  # The python interpreter version.

  # Currently Azure ML only supports 3.5.2 and later.

- python=3.6.2
- tensorflow=1.12.0

- pip:
    # Required packages for AzureML execution, history, and data preparation.

  - --index-url https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
  - --extra-index-url https://pypi.python.org/simple
  - azureml-defaults

```

Uw experiment met Azure Machine Learning op een externe compute-doel om uit te voeren:

1. Selecteer het Azure-pictogram op de balk van de activiteit Visual Studio Code. De Azure Machine Learning-zijbalk wordt weergegeven.

1. Vouw in de structuurweergave wordt weergegeven, uw Azure-abonnement en de werkruimte van Azure Machine Learning-service. 

1. In het editorvenster voor met de rechtermuisknop op het Python-script en selecteer **AML: Run as-Experiment in Azure**. 

1. Selecteer op het opdrachtenpalet de compute-doel. 

1. Voer op het opdrachtenpalet in het veld naam van de configuratie uitvoeren. 

1. Bewerk de *conda_dependencies.yml* bestand op te geven van het experiment runtime-afhankelijkheden. Selecteer in de rechterbenedenhoek van het venster **indienen**. 

1. Selecteer **weergave Experiment uitvoeren** om te zien van de geïntegreerde Azure Machine Learning-portal voor de uitvoering van uw controleren en Zie uw getrainde modellen.

Hier volgt een voorbeeld van een experiment uitvoeren op een externe compute-doel:

[![Een experiment uitvoeren op een externe doel](./media/vscode-tools-for-ai/runningOnARemoteTarget.gif)](./media/vscode-tools-for-ai/runningOnARemoteTarget.gif#lightbox)


## <a name="deploy-and-manage-models"></a>Modellen implementeren en beheren
U kunt in Azure Machine Learning, implementeren en beheren van machine learning-modellen in de cloud en aan de rand. 

### <a name="register-your-model-to-azure-machine-learning-from-visual-studio-code"></a>Het model registreren bij Azure Machine Learning van Visual Studio Code

Nu dat u uw model hebt getraind, kunt u deze kunt registreren in uw werkruimte. U kunt bijhouden en geregistreerde modellen te implementeren.

Het model registreren:

1. Selecteer het Azure-pictogram op de balk van de activiteit Visual Studio Code. De Azure Machine Learning-zijbalk wordt weergegeven.

1. Vouw in de structuurweergave wordt weergegeven, uw Azure-abonnement en de werkruimte van Azure Machine Learning-service.

1. Onder het werkruimteknooppunt met de rechtermuisknop op **modellen** en kies **Model registreren**.

1. Voer de naam van een model in het veld op het opdrachtenpalet. 

1. Kies of u wilt uploaden in de lijst, een **modelbestand** (voor één modellen) of een **model map** (voor modellen met meerdere bestanden, zoals TensorFlow). 

1. Selecteer uw map of bestand.

1. Wanneer u klaar bent met het configureren van de modeleigenschappen van uw in de rechterbenedenhoek van het venster, selecteert u **indienen**. 

Hier volgt een voorbeeld van hoe u uw model voor Azure Machine Learning te registreren:

[![Registreren van een AML-Model](./media/vscode-tools-for-ai/RegisteringAModel.gif)](./media/vscode-tools-for-ai/RegisteringAModel.gif#lightbox)


### <a name="deploy-your-service-from-visual-studio-code"></a>Uw service implementeren vanuit Visual Studio Code

In Visual Studio Code, kunt u uw webservice te implementeren:
+ Azure Container Instances (ACI) voor het testen.
+ Azure Kubernetes Service (AKS) voor de productie.

U hoeft te maken van een ACI-container voor het testen van tevoren regelen, omdat ACI containers op elk gewenst moment worden gemaakt. U hoeft echter AKS clusters vooraf configureren. Zie voor meer informatie, [Implementeer modellen met de service Azure Machine Learning](how-to-deploy-and-where.md).

Een webservice implementeren:

1. Selecteer het Azure-pictogram op de balk van de activiteit Visual Studio Code. De Azure Machine Learning-zijbalk wordt weergegeven.

1. Vouw in de structuurweergave wordt weergegeven, uw Azure-abonnement en de werkruimte van uw Azure Machine Learning-service.

1. Vouw onder het werkruimteknooppunt, de **modellen** knooppunt.

1. Met de rechtermuisknop op het model dat u wilt implementeren en kies **Service implementeren vanuit Model geregistreerd** in het contextmenu.

1. Op het opdrachtenpalet, kiest u de compute-doel dat u implementeren wilt op. 

1. Voer op het opdrachtenpalet in het veld een naam op voor deze service.  

1. Selecteer op het opdrachtenpalet de Enter-toets op het toetsenbord om te zoeken naar en selecteer het scriptbestand.

1. Selecteer op het opdrachtenpalet de Enter-toets op het toetsenbord om te zoeken naar en selecteer het conda-afhankelijkheidsbestand.

1. Wanneer u klaar bent met het configureren van uw service-eigenschappen in de rechterbenedenhoek van het venster, selecteert u **indienen** te implementeren. In het eigenschappenbestand, kunt u een lokale docker-bestand of een bestand schema.json opgeven.

De webservice is nu geïmplementeerd.

Hier volgt een voorbeeld van hoe u een webservice implementeren:

[![Een webservice implementeren](./media/vscode-tools-for-ai/CreatingAnImage.gif)](./media/vscode-tools-for-ai/CreatingAnImage.gif#lightbox)

### <a name="use-keyboard-shortcuts"></a>Sneltoetsen gebruiken

U kunt het toetsenbord gebruiken voor toegang tot Azure Machine Learning-functies in Visual Studio Code. De belangrijkste sneltoets weten is Ctrl + Shift + P, waarin de command palette. Vanuit het opdrachtenpalet hebt u toegang tot alle functionaliteit van Visual Studio Code, met inbegrip van sneltoetsen voor de meest voorkomende bewerkingen.

[![Sneltoetsen voor Azure Machine Learning voor Visual Studio Code](./media/vscode-tools-for-ai/commands.gif)](./media/vscode-tools-for-ai/commands.gif#lightbox)

## <a name="next-steps"></a>Volgende stappen

* Zie voor een overzicht van hoe u met het trainen met Azure Machine Learning buiten Visual Studio Code, [zelfstudie: Trainen met Azure Machine Learning-modellen](tutorial-train-models-with-aml.md).
* Zie voor een overzicht van hoe u kunt bewerken, uitvoeren en fouten opsporen in lokale code, de [Hallo wereld-zelfstudie voor Python](https://code.visualstudio.com/docs/python/python-tutorial).

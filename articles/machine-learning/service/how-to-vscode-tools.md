---
title: Gebruik Visual Studio Code met
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
ms.openlocfilehash: bbeb0dbbd5e9c919eda4b298dc5bee31965e9bac
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59995248"
---
# <a name="get-started-with-azure-machine-learning-for-visual-studio-code"></a>Aan de slag met Azure Machine Learning voor Visual Studio Code

In dit artikel leert u hoe u de **Azure Machine Learning voor Visual Studio Code** uitbreiding van de trein en implementeren van machine learning en deep learning-modellen met Azure Machine Learning-service in Visual Studio Code (VS-Code).

Azure Machine Learning-service biedt ondersteuning voor het uitvoeren van experimenten, lokaal en op externe compute-doelen. Voor elke experiment, u kunt bijhouden van meerdere wordt uitgevoerd als u wilt vaak iteratief probeert verschillende technieken en hyperparameters. Azure Machine Learning kunt u aangepaste metrische gegevens bijhouden en experimenteren wordt uitgevoerd, data science reproduceerbaarheid en controleerbaarheid inschakelen.

Als u deze modellen kunt implementeren voor uw behoeften testen en productie.

## <a name="prerequisites"></a>Vereisten

+ Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

+ Visual Studio Code moet worden geïnstalleerd. VS Code is een toegankelijke, krachtige broncode-editor die wordt uitgevoerd op het bureaublad. Het wordt geleverd met ingebouwde ondersteuning voor Python en meer.  [Informatie over het installeren van VS Code](https://code.visualstudio.com/docs/setup/setup-overview).

+ [Python 3.5 of hoger installeren](https://www.anaconda.com/download/).


## <a name="install-the-azure-machine-learning-for-vs-code-extension"></a>De Azure Machine Learning voor VS Code-extensie installeren

Wanneer u installeert de **Azure Machine Learning** extensie, twee meer uitbreidingen worden automatisch geïnstalleerd (als u toegang tot internet hebt). Ze zijn het [Azure-Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) extensie en de [Microsoft Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) extensie

Als u wilt werken met Azure Machine Learning, moeten we VS Code omzetten in een Python IDE. Werken met [Python in Visual Studio Code](https://code.visualstudio.com/docs/languages/python), vereist de Microsoft Python-extensie, die automatisch wordt geïnstalleerd met de Azure Machine Learning-extensie. De extensie voor VS Code maakt een uitstekende IDE, en werkt in elk besturingssysteem met tal van Python-interpreters van. Het maakt gebruik van alle van de kracht van VS Code voor het automatisch aanvullen en IntelliSense, linten, foutopsporing en Eenheidstesten, naast de mogelijkheid om eenvoudig schakelen tussen de Python-omgevingen, met inbegrip van virtuele en conda-omgevingen. Bekijk deze rondleiding door bewerkt, wordt uitgevoerd, en Python-code opsporen van fouten, Zie de [Python Hello World-zelfstudie](https://code.visualstudio.com/docs/python/python-tutorial)

**De Azure Machine Learning-extensie installeren:**

1. Start VS Code.

1. In een browser, gaat u naar: [Azure Machine Learning voor Visual Studio Code (Preview)](https://aka.ms/vscodetoolsforai) extensie

1. Klik in die webpagina op **installeren**. 

1. Klik op het tabblad extensie **installeren**.

1. Er wordt een Welkom tabblad geopend in VS Code voor de extensie en het Azure-symbool (die worden beschreven in de afbeelding hieronder een rood kader) is toegevoegd aan de activiteitenbalk.

   ![Azure-pictogram in de activiteitenbalk Visual Studio Code](./media/vscode-tools-for-ai/azure-activity-bar.png)

1. In het dialoogvenster, klikt u op **aanmelden** en volg de aanwijzing op het scherm om te verifiëren met Azure. 
   
   De extensie Azure-Account, die samen met de Azure Machine Learning voor VS Code-extensie is geïnstalleerd, kunt u verifiëren met uw Azure-account. Zie de lijst met opdrachten in de [Azure-accountextensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) pagina.

> [!Tip] 
> Bekijk de [IntelliCode-extensie voor Visual Studio Code (preview)](https://go.microsoft.com/fwlink/?linkid=2006060). IntelliCode biedt een verscheidenheid aan functies AI-ondersteuning voor IntelliSense in Python, zoals het afleiden van de meest relevante automatisch aanvullen op basis van de huidige context van de code.

## <a name="azure-ml-sdk-installation"></a>Azure ML-SDK-installatie

1. Zorg ervoor dat Python 3.5 of hoger is geïnstalleerd en wordt herkend door VS Code. Als u deze nu installeren, start VS Code opnieuw op en selecteert u een Python-interpreter met behulp van instructies op https://code.visualstudio.com/docs/python/python-tutorial.

1. Geef de Python-interpreter te gebruiken in de geïntegreerde terminal-venster, of druk je op **Enter** gebruiken de standaard Python-interpreter.

   ![Kies de-interpreter](./media/vscode-tools-for-ai/python.png)

1. In de rechterbenedenhoek van het venster, wordt een melding weergegeven dat aangeeft dat de SDK van Azure ML automatisch wordt geïnstalleerd.    Een lokale persoonlijke Python-omgeving is gemaakt met de Visual Studio Code-vereisten voor het werken met de Azure Machine Learning-service.

   ![Azure Machine Learning-SDK voor Python installeren](./media/vscode-tools-for-ai/runtimedependencies.png)

## <a name="get-started-with-azure-machine-learning"></a>Aan de slag met Azure Machine Learning

Voordat u training en implementeren van machine learning-modellen met behulp van VS Code, die u wilt maken een [werkruimte van Azure Machine Learning-service](concept-azure-machine-learning-architecture.md#workspace) in de cloud om uw modellen als hulpbronnen bevatten. Leer hoe u een te maken en uw eerste experiment maken in deze werkruimte.

1. Klik op het Azure-pictogram in de activiteitenbalk Visual Studio Code. De Azure Machine Learning-zijbalk wordt weergegeven.

   [![Installeren](./media/vscode-tools-for-ai/CreateaWorkspace.gif)](./media/vscode-tools-for-ai/CreateaWorkspace.gif#lightbox)


1. Met de rechtermuisknop op uw Azure-abonnement en selecteer **werkruimte maken**. Er wordt een lijst weergegeven. Naam van het abonnement is 'Gratis' en de werkruimte is 'TeamWorkspace' in de animatie. 

1. Selecteer een bestaande resourcegroep in de lijst of maak een nieuwe met behulp van de wizard in de Command Palette.

1. Typ in het veld een unieke en duidelijke naam voor uw nieuwe werkruimte. De werkruimte is de naam 'TeamWorkspace' in de schermafbeeldingen.

1. Druk op enter en de nieuwe werkruimte is gemaakt. Deze wordt weergegeven in de structuurweergave onder de naam van het abonnement.

1. Met de rechtermuisknop op het knooppunt van het Experiment en kies **Experiment maken** in het contextmenu.  Experimenten bijhouden van uw uitvoerbewerkingen met Azure Machine Learning.

1. Voer in het veld een naam in uw experiment. In de schermafbeeldingen, het experiment is met de naam 'MNIST'.
 
1. Druk op enter en wordt de nieuw experiment gemaakt. Deze wordt weergegeven in de structuurweergave onder de Werkruimtenaam van de.

1. U kunt met de rechtermuisknop op een Experiment in een werkruimte en selecteer 'Instellen als actieve Experiment'. De **'Active'** experiment is het experiment u momenteel gebruikt en uw map openen in VS Code wordt gekoppeld aan dit experiment in de cloud. Deze map moet uw lokale Python-scripts bevatten.

   Elk van uw experiment nu met uw experiment uitgevoerd, zodat al uw belangrijke metrische gegevens worden opgeslagen in de geschiedenis van het experiment en de modellen die u training krijgen automatisch worden geüpload naar Azure Machine Learning en opgeslagen in uw experiment metrische gegevens en Logboeken.

   [![Koppelen van een map in VS Code](./media/vscode-tools-for-ai/CreateAnExperiment.gif)](./media/vscode-tools-for-ai/CreateAnExperiment.gif#lightbox)


## <a name="create-and-manage-compute-targets"></a>Maken en beheren van compute-doelen

Met Azure Machine Learning voor VS Code, kunt u uw gegevens voorbereiden, modellen trainen en deze implementeren, zowel lokaal als op de externe compute-doelen.

Deze uitbreiding ondersteunt diverse verschillende externe compute-doelen voor Azure Machine Learning. Zie de [volledige lijst met ondersteunde compute-doelen](how-to-set-up-training-targets.md) voor Azure Machine Learning.

### <a name="create-compute-targets-for-azure-machine-learning-in-vs-code"></a>Compute-doelen voor Azure Machine Learning in VS Code maken

**Een compute-doel maken:**

1. Klik op het Azure-pictogram in de activiteitenbalk Visual Studio Code. De Azure Machine Learning-zijbalk wordt weergegeven.

2. Vouw in de structuurweergave wordt weergegeven, uw Azure-abonnement en de werkruimte van Azure Machine Learning-service. Naam van het abonnement is 'Gratis' en de werkruimte is 'TeamWorkspace' in de animatie. 

3. Onder het werkruimteknooppunt met de rechtermuisknop op de **Compute** knooppunt en kies **maken Compute**.

4. Kies het doeltype compute in de lijst. 

5. Selecteer de grootte van een virtuele Machine in de Command Palette.

6. Voer een naam op voor de compute-doel in het veld in de Command Palette. 

7. Geavanceerde eigenschappen opgeven in het JSON-configuratiebestand dat wordt geopend in een nieuw tabblad. U kunt eigenschappen op, zoals een maximum aantal knooppunten opgeven.

8. Wanneer u klaar bent uw compute-doel configureren, klikt u op **indienen** in de rechterbenedenhoek van het scherm.

Hier volgt een voorbeeld voor het maken van een Azure Machine Learning-Computing (AMLCompute): [![AML Compute in VS Code maken](./media/vscode-tools-for-ai/CreateARemoteCompute.gif)](./media/vscode-tools-for-ai/CreateARemoteCompute.gif#lightbox)

#### <a name="the-run-configuration-file"></a>Het bestand 'run configuration'

De VS Code-extensie wordt automatisch maken van een lokale compute-doel en uitvoeren van configuraties voor uw **lokale** en **docker** omgevingen op uw lokale computer. Configuratiebestanden voor de uitvoering vindt u onder het knooppunt gekoppelde compute-doel. 

## <a name="train-and-tune-models"></a>Modellen trainen en afstemmen

Azure Machine Learning voor VS Code (Preview) gebruiken om snel Voortborduren op uw code, doorlopen en fouten opsporen en uw oplossing voor bron code naar keuze gebruiken. 

**Uw experiment lokaal uitvoeren met Azure Machine Learning:**

1. Klik op het Azure-pictogram in de activiteitenbalk Visual Studio Code. De Azure Machine Learning-zijbalk wordt weergegeven.

1. Vouw in de structuurweergave wordt weergegeven, uw Azure-abonnement en de werkruimte van Azure Machine Learning-service. 

1. Vouw onder het werkruimteknooppunt, de **Compute** knooppunt en klik met de rechtermuisknop op de **uitvoeren Config** reken-en u wilt gebruiken. 

1. Selecteer **Experiment uit te voeren**.

1. Het script om uit te voeren in de Verkenner te selecteren. 

1. Klik op **weergave Experiment uitvoeren** om te zien van de geïntegreerde Azure Machine Learning-portal voor de uitvoering van uw controleren en Zie uw getrainde modellen.

Hier volgt een voorbeeld van een experiment lokaal worden uitgevoerd: [![Een experiment lokaal worden uitgevoerd](./media/vscode-tools-for-ai/RunExperimentLocally.gif)](./media/vscode-tools-for-ai/RunExperimentLocally.gif#lightbox)

### <a name="use-remote-computes-for-experiments-in-vs-code"></a>Externe berekeningen voor experimenten in VS Code gebruiken

Voor het gebruik van een externe compute-doel bij het trainen, moet u een bestand uitvoerconfiguratieprofiel maken. Dit bestand vertelt Azure Machine Learning-niet alleen waar u uw experiment uit te voeren, maar ook hoe u de omgeving voorbereidt.

#### <a name="the-conda-dependencies-file"></a>Het conda-afhankelijkheidsbestand

Standaard wordt een nieuwe conda-omgeving voor u gemaakt en de afhankelijkheden van uw installatie worden beheerd. U moet echter opgeven de afhankelijkheden en hun versies in de `aml_config/conda_dependencies.yml` bestand. 

Dit is een fragment van het standaard aml_config/conda_dependencies.yml. Bijvoorbeeld, kunt u ' tensorflow = 1.12.0' zoals hieronder wordt weergegeven. Als u de versie van de afhankelijkheid niet opgeeft, wordt de meest recente versie worden gebruikt.  
U kunt extra afhankelijkheden toevoegen in het configuratiebestand.

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

**Uw experiment met Azure Machine Learning op een externe compute-doel om uit te voeren:**

1. Klik op het Azure-pictogram in de activiteitenbalk Visual Studio Code. De Azure Machine Learning-zijbalk wordt weergegeven.

1. Vouw in de structuurweergave wordt weergegeven, uw Azure-abonnement en de werkruimte van Azure Machine Learning-service. 

1. Met de rechtermuisknop op uw python-script in het editorvenster en selecteer **AML: Run as-Experiment in Azure**. 

1. Selecteer in het Opdrachtenpalet de compute-doel. 

1. Voer de configuratienaam van de uitvoeren in het veld in de Command Palette. 

1. Bewerk het bestand conda_dependencies.yml voor het opgeven van het experiment runtime-afhankelijkheden en klik vervolgens op **indienen** in de rechterbenedenhoek van het scherm. 

1. Klik op **weergave Experiment uitvoeren** om te zien van de geïntegreerde Azure Machine Learning-portal voor de uitvoering van uw controleren en Zie uw getrainde modellen.

Hier volgt een voorbeeld van een experiment uitvoert op een externe compute-doel: [![Een experiment uitvoert op een externe doel](./media/vscode-tools-for-ai/runningOnARemoteTarget.gif)](./media/vscode-tools-for-ai/runningOnARemoteTarget.gif#lightbox)


## <a name="deploy-and-manage-models"></a>Modellen implementeren en beheren
Azure Machine Learning kunt implementeren en beheren van uw machine learning-modellen in de cloud en op de rand. 

### <a name="register-your-model-to-azure-machine-learning-from-vs-code"></a>Het model registreren bij Azure Machine Learning van VS Code

Nu dat u uw model hebt getraind, kunt u deze kunt registreren in uw werkruimte.
Geregistreerde modellen kunnen worden bijgehouden en worden geïmplementeerd.

**Het model registreren:**

1. Klik op het Azure-pictogram in de activiteitenbalk Visual Studio Code. De Azure Machine Learning-zijbalk wordt weergegeven.

1. Vouw in de structuurweergave wordt weergegeven, uw Azure-abonnement en de werkruimte van Azure Machine Learning-service.

1. Onder het werkruimteknooppunt met de rechtermuisknop op **modellen** en kies **Model registreren**.

1. Voer de modelnaam van een in het veld in de Command Palette. 

1. Kies of u wilt uploaden in de lijst, een **modelbestand** (voor één modellen) een **model map** (voor modellen met meerdere bestanden, zoals Tensorflow). 

1. Selecteer uw map of bestand.

1. Wanneer u klaar bent u configureert de modeleigenschappen van uw, klikt u op **indienen** in de rechterbenedenhoek van het scherm. 

Hier volgt een voorbeeld voor het registreren van uw AML-model: [![Registreren van een AML-Model](./media/vscode-tools-for-ai/RegisteringAModel.gif)](./media/vscode-tools-for-ai/RegisteringAModel.gif#lightbox)


### <a name="deploy-your-service-from-vs-code"></a>Uw service implementeren vanuit Visual Studio Code

Met behulp van VS Code, kunt u uw webservice te implementeren:
+ Azure Container exemplaar (ACI): voor het testen
+ Azure Kubernetes Service (AKS): voor productie 

U hoeft niet te maken van een ACI-container voor het testen van tevoren nadat ze zijn gemaakt op elk gewenst moment. AKS-clusters hoeft echter vooraf worden geconfigureerd. 

Meer informatie over [implementatie met Azure Machine Learning](how-to-deploy-and-where.md) in het algemeen.

**Een webservice implementeren:**

1. Klik op het Azure-pictogram in de activiteitenbalk Visual Studio Code. De Azure Machine Learning-zijbalk wordt weergegeven.

1. Vouw in de structuurweergave wordt weergegeven, uw Azure-abonnement en de werkruimte van uw Azure Machine Learning-service.

1. Vouw onder het werkruimteknooppunt, de **modellen** knooppunt.

1. Met de rechtermuisknop op het model dat u wilt implementeren en kies **Service implementeren vanuit Model geregistreerd** opdracht in het contextmenu.

1. Kies in het Opdrachtenpalet de compute-doel waarnaar u wilt implementeren in de lijst. 

1. Voer een naam op voor deze service in het veld in de Command Palette.  

1. Druk op de Enter-toets op het toetsenbord om te bladeren en selecteer het scriptbestand in de Command Palette.

1. In het Opdrachtenpalet, drukt u op de Enter-toets op het toetsenbord om te bladeren en selecteer het conda-afhankelijkheidsbestand.

1. Wanneer u klaar bent uw service-eigenschappen te configureren, klikt u op **indienen** in de rechterbenedenhoek van het scherm om te implementeren. In dit servicebestand-eigenschappen kunt u een lokale Docker-bestand of een schema.json-bestand dat u wilt gebruiken.

De webservice is nu geïmplementeerd.

Hier volgt een voorbeeld voor het implementeren van een webservice: [![Een webservice implementeren](./media/vscode-tools-for-ai/CreatingAnImage.gif)](./media/vscode-tools-for-ai/CreatingAnImage.gif#lightbox)

### <a name="use-keyboard-shortcuts"></a>Sneltoetsen gebruiken

De Azure Machine Learning-functies in VS Code zijn, zoals de meeste van VS Code, toegankelijk is vanaf het toetsenbord. De belangrijkste toetsencombinatie om te weten is Ctrl + Shift + P, vindt u de Command Palette. Hier kunt hebt u toegang tot alle functionaliteit van VS Code, met inbegrip van sneltoetsen voor de meest voorkomende bewerkingen.

[![Sneltoetsen voor Azure Machine Learning voor VS Code](./media/vscode-tools-for-ai/commands.gif)](./media/vscode-tools-for-ai/commands.gif#lightbox)

## <a name="next-steps"></a>Volgende stappen

Lees voor een overzicht van training met Machine Learning buiten de VS Code, [zelfstudie: Trainen met Azure Machine Learning-modellen](tutorial-train-models-with-aml.md).

Voor stapsgewijze instructies voor bewerken, uitvoeren en debuggen van code lokaal, Zie de [Python Hello World-zelfstudie](https://code.visualstudio.com/docs/python/python-tutorial)

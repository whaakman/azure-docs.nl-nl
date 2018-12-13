---
title: Trainen en implementeren van modellen van VS Code
titleSuffix: Azure Machine Learning service
description: Meer informatie over Azure Machine Learning voor Visual Studio Code en het starten van training en het implementeren van machine learning en deep learning-modellen in Azure Machine Learning-service met behulp van Visual Studio Code.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: shwinne
author: swinner95
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 0910cce92c410a68dce6e2c44d29e72e594cd153
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53271705"
---
# <a name="use-visual-studio-code-to-train-and-deploy-machine-learning-models"></a>Visual Studio Code gebruiken om te trainen en machine learning-modellen implementeren

In dit artikel leert u hoe u de **Azure Machine Learning voor Visual Studio Code** uitbreiding van de trein en implementeren van machine learning en deep learning-modellen met Azure Machine Learning-service in Visual Studio Code (VS-Code).

Azure Machine Learning biedt ondersteuning voor het uitvoeren van experimenten, lokaal en op externe compute-doelen. Voor elke experiment, u kunt bijhouden van meerdere wordt uitgevoerd als u wilt vaak iteratief probeert verschillende technieken en hyperparameters. Azure Machine Learning kunt u aangepaste metrische gegevens bijhouden en experimenteren wordt uitgevoerd, data science reproduceerbaarheid en controleerbaarheid inschakelen.

Als u deze modellen kunt implementeren voor uw behoeften testen en productie.

## <a name="prerequisites"></a>Vereisten

+ Als u geen Azure-abonnement hebt, een gratis account maken voordat u begint. Probeer de [gratis of betaalde versie van Azure Machine Learning-service](http://aka.ms/AMLFree) vandaag nog.

+ Hebben de [Azure Machine Learning voor VS Code](how-to-vscode-tools.md) extensie instellen.

+ Hebben de [Azure Machine Learning-SDK voor Python geïnstalleerd](how-to-vscode-tools.md) met VS Code.

## <a name="create-and-manage-compute-targets"></a>Maken en beheren van compute-doelen

Met Azure Machine Learning voor VS Code, kunt u uw gegevens voorbereiden, modellen trainen en deze implementeren, zowel lokaal als op de externe compute-doelen.

Deze uitbreiding ondersteunt diverse verschillende externe compute-doelen voor Azure Machine Learning. Zie de [volledige lijst met ondersteunde compute-doelen](how-to-set-up-training-targets.md) voor Azure Machine Learning.

### <a name="create-compute-targets-for-azure-machine-learning-in-vs-code"></a>Compute-doelen voor Azure Machine Learning in VS Code maken

**Een compute-doel maken:**

1. Klik op het Azure-pictogram in de activiteitenbalk Visual Studio Code. De Azure: Machine Learning zijbalk wordt weergegeven.

2. Vouw in de structuurweergave wordt weergegeven, uw Azure-abonnement en de werkruimte van Azure Machine Learning-service. Naam van het abonnement is 'Gratis' en de werkruimte is 'TeamWorkspace' in de animatie. 

3. Onder het werkruimteknooppunt met de rechtermuisknop op de **Compute** knooppunt en kies **maken Compute**.

4. Kies het doeltype compute in de lijst. 

5. Geavanceerde eigenschappen opgeven in het JSON-configuratiebestand dat wordt geopend in een nieuw tabblad. In dit bestand kunt u een unieke naam voor de compute-doel.

6. Wanneer u klaar bent uw compute-doel configureren, klikt u op **indienen** in de rechterbenedenhoek.

Hier volgt een voorbeeld van Azure Machine Learning-Computing (AMLCompute): [![AML Compute in VS Code maken](./media/vscode-tools-for-ai/CreateAMLCompute.gif)](./media/vscode-tools-for-ai/CreateAMLCompute.gif#lightbox)

### <a name="use-remote-computes-for-experiments-in-vs-code"></a>Externe berekeningen voor experimenten in VS Code gebruiken

Voor het gebruik van een externe compute-doel bij het trainen, moet u een bestand uitvoerconfiguratieprofiel maken. Dit bestand vertelt Azure Machine Learning-niet alleen waar u uw experiment uit te voeren, maar ook hoe u de omgeving voorbereidt.

#### <a name="the-run-configuration-file"></a>Het bestand 'run configuration'

De VS Code-extensie wordt automatisch gemaakt voor een configuratie uitvoeren voor uw **lokale** en **docker** omgevingen op uw lokale computer.

Dit is een fragment van de standaard-configuratiebestand worden uitgevoerd.

Als u alle van de bibliotheken/afhankelijkheden zelf installeren wilt, stelt u `userManagedDependencies: True` en vervolgens lokale experimenten uw standaardomgeving Python gebruikt zoals opgegeven in de VS Code Python-extensie.

```yaml
# user_managed_dependencies=True indicates that the environment will be user managed. False indicates that AzureML will manage the user environment.
    userManagedDependencies: False
# The python interpreter path
    interpreterPath: python
# Path to the conda dependencies file to use for this run. If a project
# contains multiple programs with different sets of dependencies, it may be
# convenient to manage those environments with separate files.
    condaDependenciesFile: aml_config/conda_dependencies.yml
# Docker details
    docker:
# Set True to perform this run inside a Docker container.
    enabled: false
```

#### <a name="the-conda-dependencies-file"></a>Het conda-afhankelijkheidsbestand

Standaard wordt een nieuwe conda-omgeving voor u gemaakt en de afhankelijkheden van uw installatie worden beheerd. Echter, moet u de afhankelijkheden in de `aml_config/conda_dependencies.yml` bestand.

Dit is een fragment van het standaard aml_config/conda_dependencies.yml.
U kunt extra afhankelijkheden toevoegen in het configuratiebestand.

```yaml
# The dependencies defined in this file will be automatically provisioned for runs with userManagedDependencies=False.

name: project_environment
dependencies:
  # The python interpreter version.

  # Currently Azure ML only supports 3.5.2 and later.

- python=3.6.2

- pip:
    # Required packages for AzureML execution, history, and data preparation.

  - --index-url https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
  - --extra-index-url https://pypi.python.org/simple
  - azureml-defaults

```

## <a name="train-and-tune-models"></a>Modellen trainen en afstemmen

Azure Machine Learning voor VS Code (Preview) gebruiken om snel Voortborduren op uw code, doorlopen en fouten opsporen en uw oplossing voor bron code naar keuze gebruiken. 

**Uw experiment uitvoeren met Azure Machine Learning:**

1. Klik op het Azure-pictogram in de activiteitenbalk Visual Studio Code. De Azure: Machine Learning zijbalk wordt weergegeven.

1. Vouw in de structuurweergave wordt weergegeven, uw Azure-abonnement en de werkruimte van Azure Machine Learning-service. 

1. Vouw onder het werkruimteknooppunt, de **Compute** knooppunt en met de rechtermuisknop op de **uitvoeren Config** reken-en u wilt gebruiken. 

1. Selecteer **Experiment uit te voeren**.

1. Klik op **weergave Experiment uitvoeren** om te zien van de geïntegreerde Azure Machine Learning-portal voor de uitvoering van uw controleren en Zie uw getrainde modellen.

## <a name="deploy-and-manage-models"></a>Modellen implementeren en beheren
Azure Machine Learning kunt implementeren en beheren van uw machine learning-modellen in de cloud en op de rand. 

### <a name="register-your-model-to-azure-machine-learning-from-vs-code"></a>Het model registreren bij Azure Machine Learning van VS Code

Nu dat u uw model hebt getraind, kunt u deze kunt registreren in uw werkruimte.
Geregistreerde modellen kunnen worden bijgehouden en worden geïmplementeerd.

**Het model registreren:**

1. Klik op het Azure-pictogram in de activiteitenbalk Visual Studio Code. De Azure: Machine Learning zijbalk wordt weergegeven.

1. Vouw in de structuurweergave wordt weergegeven, uw Azure-abonnement en de werkruimte van Azure Machine Learning-service.

1. Onder het werkruimteknooppunt met de rechtermuisknop op **modellen** en kies **Model registreren**.

1. Kies of u wilt uploaden in de lijst, een **modelbestand** (voor één modellen) een **model map** (voor modellen met meerdere bestanden, zoals Tensorflow). 

1. Selecteer uw map of bestand.

1. Wanneer u klaar bent u configureert de modeleigenschappen van uw, klikt u op **indienen** in de rechterbenedenhoek. 



### <a name="deploy-your-service-from-vs-code"></a>Uw service implementeren vanuit Visual Studio Code

Met behulp van VS Code, kunt u uw webservice te implementeren:
+ Azure Container exemplaar (ACI): voor het testen
+ Azure Kubernetes Service (AKS): voor productie 

U hoeft niet te maken van een ACI-container voor het testen van tevoren nadat ze zijn gemaakt op elk gewenst moment. AKS-clusters hoeft echter vooraf worden geconfigureerd. 

Meer informatie over [implementatie met Azure Machine Learning](how-to-deploy-and-where.md) in het algemeen.

**Een webservice implementeren:**

1. Klik op het Azure-pictogram in de activiteitenbalk Visual Studio Code. De Azure: Machine Learning zijbalk wordt weergegeven.

1. Vouw in de structuurweergave wordt weergegeven, uw Azure-abonnement en de werkruimte van uw Azure Machine Learning-service.

1. Vouw onder het werkruimteknooppunt, de **modellen** knooppunt.

1. Met de rechtermuisknop op het model dat u wilt implementeren en kies **Service implementeren vanuit Model geregistreerd** opdracht in het contextmenu.

1. Kies in het Opdrachtenpalet de compute-doel waarnaar u wilt implementeren in de lijst. 

1. Voer in het veld een naam voor deze service. 

1. Druk op de Enter-toets op het toetsenbord om te bladeren en selecteer het scriptbestand in de Command Palette.

1. In het Opdrachtenpalet, drukt u op de Enter-toets op het toetsenbord om te bladeren en selecteer het conda-afhankelijkheidsbestand.

1. Wanneer u klaar bent uw service-eigenschappen te configureren, klikt u op **indienen** in de rechterbenedenhoek. In dit servicebestand-eigenschappen kunt u een lokale Docker-bestand of een schema.json-bestand dat u wilt gebruiken.

De webservice is nu geïmplementeerd.

## <a name="next-steps"></a>Volgende stappen

Lees voor een overzicht van training met Machine Learning buiten de VS Code, [zelfstudie: Trainen met Azure Machine Learning-modellen](tutorial-train-models-with-aml.md).

Voor stapsgewijze instructies voor bewerken, uitvoeren en debuggen van code lokaal, Zie de [Python Hello World-zelfstudie](https://code.visualstudio.com/docs/python/python-tutorial)

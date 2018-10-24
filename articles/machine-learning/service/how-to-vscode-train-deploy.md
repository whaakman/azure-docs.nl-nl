---
title: Met behulp van Visual Studio Code-hulpprogramma's voor AI-extensie met Azure Machine Learning
description: Meer informatie over Visual Studio Code-hulpprogramma's voor AI en hoe u training en een machine learning en deep learning-modellen met Azure Machine Learning-service in VS Code implementeren.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: jmartens
author: j-martens
ms.reviewer: jmartens
ms.date: 10/1/2018
ms.openlocfilehash: 6372aada8c3d380f8d3cefb0479e45bbd7c63898
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945243"
---
# <a name="vs-code-tools-for-ai-train-and-deploy-ml-models-from-vs-code"></a>VS Code-hulpprogramma's voor AI: trainen en implementeren van ML-modellen van VS Code
In dit artikel leert u hoe u de **VS Code-hulpprogramma's voor AI** extensie te trainen en implementeren van machine learning en deep learning-modellen met Azure Machine Learning-service in VS Code.

Azure Machine Learning biedt ondersteuning voor het uitvoeren van experimenten, lokaal en op externe compute-doelen. Voor elke experiment, u kunt bijhouden van meerdere wordt uitgevoerd als u wilt vaak iteratief probeert verschillende technieken en hyperparameters. Azure Machine Learning kunt u aangepaste metrische gegevens bijhouden en experimenteren wordt uitgevoerd, data science reproduceerbaarheid en controleerbaarheid inschakelen.

Als u deze modellen kunt implementeren voor uw behoeften testen en productie.

## <a name="prerequisites"></a>Vereisten

+ [Hebt u VS Code-hulpprogramma's voor AI](how-to-vscode-tools.md) ingesteld voor Azure Machine Learning.

+ Hebben de [Azure Machine Learning-SDK voor Python geïnstalleerd](how-to-vscode-tools.md) met VS Code.

+ Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-and-manage-compute-targets"></a>Maken en beheren van compute-doelen

Met Visual Studio Code-hulpprogramma's voor AI, kunt u uw gegevens voorbereiden, modellen trainen en deze implementeren, zowel lokaal als op de externe compute-doelen.

Deze uitbreiding ondersteunt diverse verschillende externe compute-doelen voor Azure Machine Learning. Zie de [volledige lijst met ondersteunde compute-doelen](how-to-set-up-training-targets.md) voor Azure Machine Learning.

### <a name="create-compute-targets-for-azure-machine-learning-in-vs-code"></a>Compute-doelen voor Azure Machine Learning in VS Code maken

**Een compute-doel maken:**

1. Klik op het Azure-pictogram in de activiteitenbalk Visual Studio Code. De Azure: Machine Learning zijbalk wordt weergegeven.

2. Vouw in de structuurweergave wordt weergegeven, uw Azure-abonnement en de werkruimte van Azure Machine Learning-service. Naam van het abonnement is 'OpenMind Studio' in de bewegende afbeelding en de werkruimte is 'MyWorkspace'. 

3. Onder het werkruimteknooppunt met de rechtermuisknop op de **Compute** knooppunt en kies **maken Compute**.

4. Kies het doeltype compute in de lijst. 

5. Voer een unieke naam voor deze compute-doel in het veld, en geef de grootte van de virtuele machine.

6. Geavanceerde eigenschappen opgeven in het JSON-configuratiebestand dat wordt geopend in een nieuw tabblad. 

7. Wanneer u klaar bent uw compute-doel configureren, klikt u op **voltooien** in de rechterbenedenhoek.

Hier volgt een voorbeeld van Azure Batch AI: [ ![maken Azure Batch AI compute in VS Code](./media/vscode-tools-for-ai/createcompute.gif)](./media/vscode-tools-for-ai/createcompute.gif#lightbox)

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

Azure Machine Learning van VS Code gebruiken om snel Voortborduren op uw code, doorlopen en fouten opsporen en uw oplossing voor bron code naar keuze gebruiken. 

**Uw experiment uitvoeren met Azure Machine Learning:**

1. Klik op het Azure-pictogram in de activiteitenbalk Visual Studio Code. De Azure: Machine Learning zijbalk wordt weergegeven.

1. Vouw in de structuurweergave wordt weergegeven, uw Azure-abonnement en de werkruimte van Azure Machine Learning-service. Naam van het abonnement is 'OpenMind Studio' in de bewegende afbeelding en de werkruimte is 'MyWorkspace'. 

1. Vouw onder het werkruimteknooppunt, de **Compute** knooppunt en met de rechtermuisknop op de **uitvoeren Config** reken-en u wilt gebruiken. 

1. Selecteer **Experiment uit te voeren**.

1. Klik op **weergave Experiment uitvoeren** om te zien van de geïntegreerde Azure Machine Learning-portal voor de uitvoering van uw controleren en Zie uw getrainde modellen.

   [![Machine learning-experiment uitvoeren vanuit Visual Studio Code](./media/vscode-tools-for-ai/runexperiment.gif)](./media/vscode-tools-for-ai/runexperiment.gif#lightbox)

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

1. Gebruik het dialoogvenster voor het bestand om het bestand of map.

   [![COMPUTE](./media/vscode-tools-for-ai/registermodel.gif)](./media/vscode-tools-for-ai/registermodel.gif#lightbox)

> [!Warning]
> Verwijder de labels van het gegenereerde json-bestand op dit moment.

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

1. Kies in het Opdrachtenpalet van VS Code de compute-doel waarnaar u wilt implementeren in de lijst. 

1. Voer in het veld een naam voor deze service. 

1. Klik in het dialoogvenster in de rechterbenedenhoek op **Bladeren** en selecteer de scoring-script. Het dialoogvenster wordt gesloten.

1. Als u een lokale Docker-bestand hebt, klikt u op **Bladeren** in het tweede dialoogvenster dat wordt weergegeven. 

   Als u in het dialoogvenster annuleren en een lokale Docker-bestand niet opgeeft, wordt een 'Azure Machine Learning, een standaard gebruikt.

1. In de derde in het dialoogvenster dat wordt weergegeven, klikt u op **Bladeren** en selecteer het pad naar het lokale conda of invoer van het bestandspad in json-editor later opnieuw.

1. Als u een bestand schema.json hebt u wilt gebruiken, klikt u op **Bladeren** in de vierde dialoogvenster dat wordt weergegeven en selecteer het bestand.

De webservice is nu geïmplementeerd.

Hier volgt een voorbeeld van Azure Container Instances: [ ![Azure Container Instances vanuit Visual Studio Code](./media/vscode-tools-for-ai/deploy.gif)](./media/vscode-tools-for-ai/deploy.gif#lightbox)

## <a name="next-steps"></a>Volgende stappen

Lees voor een overzicht van training met Machine Learning buiten de VS Code, [zelfstudie: trainen van modellen met Azure Machine Learning](tutorial-train-models-with-aml.md).

Voor stapsgewijze instructies voor bewerken, uitvoeren en debuggen van code lokaal, Zie de [Python Hello World-zelfstudie](https://code.visualstudio.com/docs/python/python-tutorial)

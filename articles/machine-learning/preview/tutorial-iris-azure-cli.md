---
title: Zelfstudie-artikel voor Azure Machine Learning preview-functies - opdrachtregelinterface | Microsoft Docs
description: Deze zelfstudie Doorloop de stappen die een Iris classificatie end-to-end van de opdrachtregelinterface.
services: machine-learning
author: ahgyger
ms.author: ahgyger, ritbhat
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: tutorial
ms.date: 10/15/2017
ms.openlocfilehash: 21fb0bca08bca0fe6384bbc9ba2511f7d8b746cf
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="tutorial-classifying-iris-using-the-command-line-interface"></a>Zelfstudie: Classificeren Iris via de opdrachtregelinterface
Azure Machine Learning-services (preview) is een geïntegreerde, end-to-end oplossing voor gegevenswetenschap en geavanceerde analyse voor professionele gegevenswetenschappers. Hiermee kunnen ze gegevens voorbereiden, experimenten ontwikkelen en modellen in de cloud implementeren.

In deze zelfstudie leert u de opdrachtregelinterface (CLI)-hulpprogramma's in Azure Machine Learning preview-functies te gebruiken: 
> [!div class="checklist"]
> * Instellen van een account experimenteren en een werkruimte maken
> * Een project maken
> * Verzenden van een experiment aan verschillende compute-doelen
> * Promoveren en registreren van een getraind model
> * Een webservice implementeren ter beoordeling van nieuwe gegevens

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten
- U moet toegang tot een Azure-abonnement en machtigingen voor het maken van resources in het desbetreffende abonnement. 
- U moet Azure Machine Learing Workbench toepassing installeren door de [installeren en het maken van de Quick Start](quickstart-installation.md). 

  >[!NOTE]
  >U hoeft alleen te installeren van de Azure Machine Learning-Workbench lokaal. U moet alleen de stappen in de secties recht installeren Azure Machine Learning Workbench, sinds de account is gemaakt, en maak een nieuw project stappen worden uitgevoerd met de opdrachtregel in dit artikel.
 
## <a name="getting-started"></a>Aan de slag
Azure Machine Learning-opdrachtregelinterface (CLI) kunt u alle taken die vereist zijn voor een wetenschappelijke werkstroom end-to-end uitvoeren. U kunt de CLI-hulpprogramma's in de volgende manieren openen:

### <a name="option-1-launch-azure-ml-cli-from-azure-ml-workbench-log-in-dialog-box"></a>Optie 1. Azure ML CLI van Azure ML aanmelden dialoogvenster Workbench starten
Wanneer u Azure ML-Workbench en zich aanmelden voor het eerst start, en als u nog geen toegang tot een experimenteren-Account hebt, krijgt u het volgende scherm:

![Er is geen account gevonden](media/tutorial-iris-azure-cli/no_account_found.png)

Klik op de **opdrachtregelvenster** koppeling in het dialoogvenster voor het starten van het venster vanaf de opdrachtregel.

### <a name="option-2-launch-azure-ml-cli-from-azure-ml-workbench-app"></a>Optie 2. Azure ML CLI van Azure ML-Workbench app starten
Als u al toegang tot een experimenteren-Account hebt, kunt u met succes registreren. En vervolgens kunt u de opdrachtregel-venster openen door te klikken op **bestand** --> **opdracht vragen openen** menu.

### <a name="option-3-enable-azure-ml-cli-in-an-arbitrary-command-line-window"></a>Optie 3. Azure ML CLI in een willekeurige opdrachtregelvenster inschakelen
U kunt ook Azure ML CLI in een opdrachtregelvenster inschakelen. Gewoon start u een opdrachtvenster en voert u de volgende opdrachten:

```sh
# Windows Command Prompt
set PATH=%LOCALAPPDATA%\amlworkbench\Python;%LOCALAPPDATA%\amlworkbench\Python\Scripts;%PATH%

# Windows PowerShell
$env:Path = $env:LOCALAPPDATA+"\amlworkbench\Python;"+$env:LOCALAPPDATA+"\amlworkbench\Python\Scripts;"+$env:Path

# macOS Bash Shell
PATH=$HOME/Library/Caches/AmlWorkbench/Python/bin:$PATH
```
U kunt gebruiken om de wijziging permanente `SETX` in Windows. Voor Mac OS, kunt u `setenv`.

>[!TIP]
>U kunt Azure CLI in uw favoriete terminalvenster inschakelen door het instellen van de bovenstaande omgevingsvariabelen.

## <a name="step-1-log-in-to-azure"></a>Step 1. Meld u aan bij Azure.
De eerste stap is de CLI openen vanuit de App AMLWorkbench (bestand > opdrachtprompt openen). Hiermee zorgt u ervoor gebruiken we de juiste python-omgeving en hebben we de ML CLI-opdrachten die beschikbaar zijn. 

Er moet vervolgens stelt u de juiste context in uw CLI raadplegen en beheren van Azure-resources.
 
```azure-cli
# log in
$ az login

# list all subscriptions
$ az account list -o table

# set the current subscription
$ az account set -s <subscription id or name>
```

## <a name="step-2-create-a-new-azure-machine-learning-experimentation-account-and-workspace"></a>Stap 2. Een nieuwe Account voor Azure Machine Learning experimenteren en werkruimte maken
Begin door een nieuw experimenteren-account en een nieuwe werkruimte te maken. Zie [Azure Machine Learning-concepten](overview-general-concepts.md) voor meer informatie over accounts experimenteren en werkruimten.

> [!NOTE]
> Experimenteren accounts vereist een opslagaccount die wordt gebruikt voor het opslaan van de uitvoer van uw experiment wordt uitgevoerd. Naam van het opslagaccount heeft moet globaal uniek zijn in Azure, omdat er een url die is gekoppeld. Als u een bestaand opslagaccount niet opgeeft, wordt de naam van uw experimenteren wordt gebruikt voor het maken van een nieuw opslagaccount. Controleer of u een unieke naam op of u krijgt een foutmelding zoals _' de storage-account met de naam \<storage_account_name > is al in gebruik. "_ U kunt ook de `--storage` argument op te geven van een bestaand opslagaccount.

```azure-cli
# create a resource group 
$ az group create --name <resource group name> --location <supported Azure region>

# create a new experimentation account with a new storage account
$ az ml account experimentation create --name <experimentation account name> --resource-group <resource group name>

# create a new experimentation account with an existing storage account
$ az ml account experimentation create --name <experimentation account name>  --resource-group <resource group name> --storage <storage account Azure Resource ID>

# create a workspace in the experimentation account
az ml workspace create --name <workspace name> --account <experimentation account name> --resource-group <resource group name>
```

## <a name="step-2a-optional-share-a-workspace-with-co-worker"></a>Stap 2. (optioneel) een werkruimte delen met collega
Hier besproken voor het delen van toegang tot een werkruimte met een collega. De stappen voor het delen van toegang tot een experimenteren-account of een project zou zijn hetzelfde. Alleen de manier om de Azure-Resource-ID moet worden bijgewerkt.

```azure-cli
# find the workspace Azure Resource ID
$az ml workspace show --name <workspace name> --account <experimentation account name> --resource-group <resource group name>

# add Bob to this workspace as a new owner
$az role assignment create --assignee bob@contoso.com --role owner --scope <workspace Azure Resource ID>
```

> [!TIP]
> `bob@contoso.com`in de bovenstaande opdracht moet een geldige Azure AD-identiteit in de map waar het huidige abonnement behoort.

## <a name="step-3-create-a-new-project"></a>Stap 3. Een nieuw project maken
De volgende stap is het maken van een nieuw project. Er zijn verschillende manieren om te beginnen met een nieuw project.

### <a name="create-a-new-blank-project"></a>Een nieuw, leeg project maken

```azure-cli
# create a new project
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path>
```

### <a name="create-a-new-project-with-a-default-project-template"></a>Een nieuw project maakt met een standaardsjabloon voor project
U kunt een nieuw project maakt met een standaardsjabloon.

```azure-cli
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --template
```

### <a name="create-a-new-project-associated-with-a-cloud-git-repository"></a>Maak een nieuw project gekoppeld aan een cloud Git-opslagplaats
We kunnen een nieuw project die zijn gekoppeld aan een VSTS (Visual Studio Team Service) Git-opslagplaats maken. Telkens wanneer een experiment wordt ingediend, wordt een momentopname van de hele projectmap is doorgevoerd in de externe Git-opslagplaats. Zie [met behulp van Git-opslagplaats met een Azure Machine Learning Workbench project](using-git-ml-project.md) voor meer informatie.

> [!NOTE]
> Azure Machine Learning biedt alleen ondersteuning voor lege Git repo's in VSTS gemaakt.

```azure-cli
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --repo <VSTS repo URL>
```
> [!TIP]
> Als u krijgt een fout "opslagplaats url is mogelijk ongeldig of gebruikers mogelijk geen toegang", maakt u een beveiligingstoken in VSTS (onder _beveiliging_, _persoonlijke toegangstokens toevoegen_ menu) en gebruiken van de `--vststoken`argument bij het maken van uw project. 

### <a name="sample_create"></a>Een nieuw project maken van een steekproef
In dit voorbeeld maken we een nieuw project met een voorbeeldproject als sjabloon.

```azure-cli
# List the project samples, find the Classifying Iris sample
$ az ml project sample list

# Create a new project from the sample
az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --template-url https://github.com/MicrosoftDocs/MachineLearningSamples-Iris
```
Zodra het project is gemaakt, gebruikt u `cd` opdracht voor het invoeren van de projectmap.

## <a name="step-4-run-the-training-experiment"></a>Stap 4, voer het trainingsexperiment 
De volgende stappen wordt ervan uitgegaan dat u een project met het voorbeeld Iris hebt (Zie [een nieuw project maken van een online voorbeeld](#sample_create)).

### <a name="prepare-your-environment"></a>Uw omgeving voorbereiden 
Voor het voorbeeld Iris moeten we matplotlib installeren.

```azure-cli
$ pip install matplotlib
```

###  <a name="submit-the-experiment"></a>Het experiment verzenden

```azure-cli
# Execute the file
$ az ml experiment submit --run-configuration local iris_sklearn.py
```

### <a name="iterate-on-your-experiment-with-descending-regularization-rates"></a>Uw experiment met aflopende regularisatie tarieven herhalen
Met sommige creativiteit is het eenvoudig om samen te stellen een pythonscript waarmee experimenten met verschillende regularisatie tarieven. (U wellicht Bewerk het bestand om te verwijzen naar het juiste projectpad.)

```azure-cli
$ python run.py
```

## <a name="step-5-view-run-history"></a>Stap 5. Uitvoeringsgeschiedenis weergeven
Na de opdracht geeft een lijst van de vorige uitgevoerd uitgevoerd. 

```azure-cli
$ az ml history list -o table
```
Met de bovenstaande opdracht wordt een lijst weergegeven met alle sessies die horen bij dit project. U kunt zien nauwkeurigheid en regularisatie snelheid metrische gegevens te staan. Deze maken het gemakkelijk kunt herkennen het beste uitvoeren in de lijst.

## <a name="step-5a-view-attachment-created-by-a-given-run"></a>Stap bijlage van 5. weergeven die zijn gemaakt door een bepaalde uitvoering 
Als u wilt weergeven die zijn gekoppeld aan een bepaald run bijlage, kunnen we de opdracht info van uitvoeringsgeschiedenis gebruiken. Een uitvoeren-id van een specifieke reeks die in de bovenstaande lijst vinden.

```azure-cli
$ az ml history info --run <run id> --artifact driver_log
```

Voor het downloaden van de artefacten van een uitvoeren, kunt u onderstaande opdracht:

```azure-cli
# Stream a given attachment 
$ az ml history info --run <run id> --artifact <artifact location>
```

## <a name="step-6-promote-artifacts-of-a-run"></a>Stap 6. Onderdelen van een uitvoering verhogen 
Een van de worden uitgevoerd er heeft een betere AUC zodat we gebruiken wilt voor het maken van een score webservice implementeren naar productie. Als u wilt doen, moeten we eerst de artefacten promoveert naar een asset.

```azure-cli
$ az ml history promote --run <run id> --artifact-path outputs/model.pkl --name model.pkl
```

Hiermee maakt u een `assets` map in de projectmap van uw met een `model.pkl.link` bestand. Deze koppelingsbestand wordt gebruikt om te verwijzen naar een gepromoveerde asset.

## <a name="step-7-download-the-files-to-be-operationalized"></a>Stap 7. De bestanden moeten worden geoperationaliseerd downloaden
We moeten nu downloaden van de gepromoveerde model, zodat we deze gebruiken kunt om onze voorspelling webservice te maken. 

```azure-cli
$ az ml asset download --link-file assets\pickle.link -d asset_download
```

## <a name="step-8-setup-your-model-management-environment"></a>Stap 8. Instellen van uw model-management-omgeving 
We maken een omgeving om webservices implementeren. We kunnen de web-service op de lokale computer met behulp van Docker uitvoeren. Of voor een ACS-cluster voor hoge schaalbewerkingen implementeren. 

```azure-cli
# Create new local operationalization environment
$ az ml env setup -l <supported Azure region> -n <env name>
# Once setup is complete, set your environment for current context
$ az ml env set -g <resource group name> -n <env name>
```

## <a name="step-9-create-a-model-management-account"></a>Stap 9. Een model-management-account maken 
Een model-management-account is vereist voor het implementeren en bijhouden van uw modellen in productie. 

```azure-cli
$ az ml account modelmanagement create -n <model management account name> -g <resource group name> -l <supported Azure region>
```

## <a name="step-10-create-a-web-service"></a>Stap 10. Een webservice maken
Vervolgens maken we een webservice die als resultaat geeft een voorspelling met het model dat wordt geïmplementeerd. 

```azure-cli
$ az ml service create realtime -m asset_download/model.pkl -f score_iris.py -r python –n <web service name>
```

## <a name="step-10-run-the-web-service"></a>Stap 10. De webservice wordt uitgevoerd
Met behulp van de webservice-id uit de uitvoer van de vorige stap, kunnen we de service aanroepen en testen. 

```azure-cli
# Get web service usage infomration
$ az ml service usage realtime -i <web service id>

# Call the web service with the run command:
$ az ml service run realtime -i <web service id> -d <input data>
```

## <a name="deleting-all-the-resources"></a>Alle resources verwijderen 
Laten we in deze zelfstudie voltooid door het verwijderen van alle resources die we hebben gemaakt, tenzij u wilt blijven werken op het! 

Om dit te doen verwijderen we dan gewoon de resourcegroep van alle onze resources. 

```azure-cli
az group delete --name <resource group name>
```

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe de Azure Machine Learning preview-functies te gebruiken 
> [!div class="checklist"]
> * Instellen van een account experimenteren,-werkruimte maken
> * Projecten maken
> * Verzenden van experimenten met meerdere compute-doel
> * Promoveren en registreren van een getraind model
> * Een model management-account voor het Modelbeheer van maken
> * Een omgeving voor het implementeren van een webservice maken
> * Implementeer een web-service en de score bij met nieuwe gegevens

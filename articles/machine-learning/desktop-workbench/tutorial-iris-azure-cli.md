---
title: Zelfstudie-artikel voor de preview-functies van Azure Machine Learning - opdrachtregelinterface | Microsoft Docs
description: In deze zelfstudie doorloopt u alle stappen die nodig zijn om een end-to-end Iris-classificatie te voltooien vanaf de opdrachtregelinterface.
services: machine-learning
author: jpe316
ms.author: jordane
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: tutorial
ms.date: 10/15/2017
ms.openlocfilehash: 10fe861682da6c1d1ac701a565cef11f9b44cd1e
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/21/2018
ms.locfileid: "41918769"
---
# <a name="tutorial-classifying-iris-using-the-command-line-interface"></a>Zelfstudie: Iris classificeren via de opdrachtregelinterface
Azure Machine Learning-services (preview) zijn een geïntegreerde, end-to-end oplossing voor gegevenswetenschap en geavanceerde analyse voor professionele gegevenswetenschappers. Hiermee kunnen ze gegevens voorbereiden, experimenten ontwikkelen en modellen in de cloud implementeren.

In deze zelfstudie leert u de opdrachtregelprogramma's in Azure Machine Learning-preview-functies te gebruiken om het volgende te doen: 
> [!div class="checklist"]
> * Een Experimenten-account instellen en een werkruimte maken
> * Een project maken
> * Een experiment verzenden naar verschillende compute-doelen
> * Het niveau van een getraind model verhogen en het model registreren
> * Een webservice implementeren voor het scoren van nieuwe gegevens

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt u het volgende nodig:
- Toegang tot een Azure-abonnement en machtigingen om resources te maken in dat abonnement. 
  
  Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

- Azure Machine Learning Workbench toepassing geïnstalleerd zoals beschreven in [Snelstart: Azure Machine Learning-services installeren en starten](../service/quickstart-installation.md). 

  >[!IMPORTANT]
  >Maak geen Azure Machine Learning-serviceaccounts aan, want u doet dat met behulp van de CLI in dit artikel.
 
## <a name="getting-started"></a>Aan de slag
Met de CLI (opdrachtregelinterface) van Azure Machine Learning kunt u alle taken uitvoeren die nodig zijn voor een end-to-end gegevenswetenschapwerkstroom. U hebt op de volgende manieren toegang tot de CLI-hulpprogramma’s:

### <a name="option-1-launch-azure-ml-cli-from-azure-ml-workbench-log-in-dialog-box"></a>Optie 1. Azure ML CLI starten vanuit het aanmeldingsvenster van Azure ML Workbench
Wanneer u Azure ML Workbench start en u voor de eerste keer aanmeldt en u geen toegang hebt tot een Experimenten-account, krijgt u het volgende scherm te zien:

![geen account gevonden](media/tutorial-iris-azure-cli/no_account_found.png)

Klik in het dialoogvenster op de koppeling **Opdrachtregelvenster** om het opdrachtregelvenster te openen.

### <a name="option-2-launch-azure-ml-cli-from-azure-ml-workbench-app"></a>Optie 2. Azure ML CLI starten vanuit de Azure ML Workbench-app
Als u al toegang hebt tot een Experimenteren-account, kunt zich daarmee aanmelden. Vervolgens kunt u het opdrachtregelvenster openen door op **Bestand** --> **Opdrachtprompt openen** te klikken.

### <a name="option-3-enable-azure-ml-cli-in-an-arbitrary-command-line-window"></a>Optie 3. Azure ML CLI inschakelen in een willekeurig opdrachtregelvenster
U kunt Azure ML CLI ook inschakelen in een opdrachtregelvenster. Doe dit door een opdrachtvenster te starten en de volgende opdrachten in te voeren:

```sh
# Windows Command Prompt
set PATH=%LOCALAPPDATA%\amlworkbench\Python;%LOCALAPPDATA%\amlworkbench\Python\Scripts;%PATH%

# Windows PowerShell
$env:Path = $env:LOCALAPPDATA+"\amlworkbench\Python;"+$env:LOCALAPPDATA+"\amlworkbench\Python\Scripts;"+$env:Path

# macOS Bash Shell
PATH=$HOME/Library/Caches/AmlWorkbench/Python/bin:$PATH
```
Als u de wijziging permanent wilt maken, kunt u `SETX` gebruiken in Windows. Voor macOS kunt u `setenv` gebruiken.

>[!TIP]
>U kunt Azure CLI in uw favoriete terminalvenster inschakelen door de voorgaande omgevingsvariabelen in te stellen.

## <a name="step-1-log-in-to-azure"></a>Step 1. Meld u aan bij Azure.
De eerste stap is het openen van de CLI vanuit de AMLWorkbench-app (Bestand > Opdrachtprompt openen). Hiermee zorgt u ervoor dat u de juiste python-omgeving hebt en dat de ML CLI-opdrachten beschikbaar zijn. 

Nu kunt u de juiste context instellen in uw CLI om toegang te krijgen tot Azure-resources en deze te beheren.
 
```azure-cli
# log in
$ az login

# list all subscriptions
$ az account list -o table

# set the current subscription
$ az account set -s <subscription id or name>
```

## <a name="step-2-create-a-new-azure-machine-learning-experimentation-account-and-workspace"></a>Stap 2. Een nieuwe Azure Machine Learning Experimenten-account en werkruimte maken

In deze stap maakt u een nieuwe Experimenten-account en een nieuwe werkruimte. Zie [Azure Machine Learning-concepten](overview-general-concepts.md) voor meer informatie over Experimenten-accounts en werkruimten.

> [!NOTE]
> Voor Experimenten-accounts is een opslagaccount vereist, dat wordt gebruikt om de uitvoer van uw experimenten op te slaan. De naam van het opslagaccount moet globaal uniek zijn in Azure, omdat er een URL aan gekoppeld is. Als u geen bestaand opslagaccount opgeeft, wordt de naam van uw Experimenten-account gebruikt om een nieuw opslagaccount te maken. Zorg ervoor dat u een unieke naam gebruikt, anders krijgt u een foutmelding zoals _'Het opslagaccount met de naam \<naam_opslagaccount> is niet meer beschikbaar.'_ U kunt ook het argument `--storage` gebruiken om een bestaand opslagaccount op te geven.

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

## <a name="step-2a-optional-share-a-workspace-with-co-worker"></a>Stap 2.a (optioneel) Een werkruimte delen met een collega
Hier kunt u ontdekken hoe u de toegang tot een werkruimte kunt delen met een collega. De stappen voor het delen van de toegang tot een Experimenten-account of een project zijn dan hetzelfde. Alleen de manier waarop de Azure Resource-id wordt opgehaald, moet worden bijgewerkt.

```azure-cli
# find the workspace Azure Resource ID
$az ml workspace show --name <workspace name> --account <experimentation account name> --resource-group <resource group name>

# add Bob to this workspace as a new owner
$az role assignment create --assignee bob@contoso.com --role owner --scope <workspace Azure Resource ID>
```

> [!TIP]
> `bob@contoso.com` in de bovenstaande opdracht moet een geldige Azure AD-id zijn in de directory waar het huidige abonnement toe behoort.

## <a name="step-3-create-a-new-project"></a>Stap 3. Een nieuw project maken
De volgende stap is het maken van een nieuw project. Er zijn verschillende manieren om van start te gaan met een nieuw project.

### <a name="create-a-new-blank-project"></a>Een nieuw, leeg project maken

```azure-cli
# create a new project
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path>
```

### <a name="create-a-new-project-with-a-default-project-template"></a>Een nieuw project maken met een standaardprojectsjabloon
U kunt een nieuw project maakt met een standaardsjabloon.

```azure-cli
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --template
```

### <a name="create-a-new-project-associated-with-a-cloud-git-repository"></a>Een nieuw project maken dat is gekoppeld aan een Git-opslagplaats in de cloud
U kunt een nieuw project maken dat is gekoppeld aan een VSTS (Visual Studio Team Service) Git-opslagplaats. Telkens wanneer een experiment wordt ingediend, wordt een momentopname van de hele projectmap opgenomen in de externe Git-opslagplaats. Zie [Een Git-opslagplaats gebruiken met een Machine Learning Workbench-project](using-git-ml-project.md) voor meer informatie.

> [!NOTE]
> Azure Machine Learning biedt alleen ondersteuning voor lege Git-opslagplaatsen die in VSTS worden gemaakt.

```azure-cli
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --repo <VSTS repo URL>
```
> [!TIP]
> Als u een foutmelding krijgt met de mededeling dat de URL van de opslagplaats mogelijk ongeldig is of dat de gebruiker geen toegang heeft, kunt u een beveiligingstoken maken in VSTS (onder _Beveiliging_, menu _Persoonlijke toegangstokens toevoegen_) en het argument `--vststoken` gebruiken bij het maken van uw project. 

### <a name="sample_create"></a>Een nieuw project maken van een voorbeeld
In dit voorbeeld maakt u een nieuw project met een voorbeeldproject als sjabloon.

```azure-cli
# List the project samples, find the Classifying Iris sample
$ az ml project sample list

# Create a new project from the sample
az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --template-url https://github.com/MicrosoftDocs/MachineLearningSamples-Iris
```
Wanneer het project is gemaakt, gebruikt u de opdracht `cd` om naar de projectmap te gaan.

## <a name="step-4-run-the-training-experiment"></a>Stap 4. Het trainingsexperiment uitvoeren 
In de volgende stappen wordt ervan uitgegaan dat u een project hebt met het Iris-voorbeeld (zie [Een nieuw project maken op basis van een online voorbeeld](#sample_create)).

### <a name="prepare-your-environment"></a>Uw omgeving voorbereiden 
Voor het Iris-voorbeeld moet u matplotlib installeren.

```azure-cli
$ pip install matplotlib
```

###  <a name="submit-the-experiment"></a>Het experiment verzenden

```azure-cli
# Execute the file
$ az ml experiment submit --run-configuration local iris_sklearn.py
```

### <a name="iterate-on-your-experiment-with-descending-regularization-rates"></a>Uw experiment met aflopende regularisatiegraden herhalen
Met enige creativiteit is het eenvoudig om een Python-script samen te stellen dat experimenten indient met verschillende regularisatiegraden. (Mogelijk moet u het bestand bewerken, zodat het naar het juiste projectpad wijst.)

```azure-cli
$ python run.py
```

## <a name="step-5-view-run-history"></a>Stap 5. Uitvoeringsgeschiedenis weergeven
Met de volgende opdracht maakt u een lijst met alle eerdere uitvoeringen. 

```azure-cli
$ az ml history list -o table
```
Met de voorgaande opdracht wordt een lijst weergegeven met uitvoeringen die tot dit project behoren. U ziet dat er ook metrische gegevens voor de nauwkeurigheid en regularisatiegraad worden vermeld. Hierdoor kunt u gemakkelijk de beste uitvoering in de lijst identificeren.

## <a name="step-5a-view-attachment-created-by-a-given-run"></a>Stap 5. a Door een bepaalde uitvoering gemaakte bijlage bekijken 
Om de bijlage die bij een bepaalde uitvoering hoort, te bekijken, kunt u gebruikmaken van de info-opdracht van de uitvoeringsgeschiedenis. Vind een uitvoerings-id van een specifieke uitvoering uit de voorgaande lijst.

```azure-cli
$ az ml history info --run <run id> --artifact driver_log
```

Voor het downloaden van de artefacten van een uitvoering, kunt u de onderstaande opdracht gebruiken:

```azure-cli
# Stream a given attachment 
$ az ml history info --run <run id> --artifact <artifact location>
```

## <a name="step-6-promote-artifacts-of-a-run"></a>Stap 6. Het niveau verhogen voor artefacten van een uitvoering 
Een van de uitvoeringen heeft een betere AUC. Dat is de uitvoering die u het beste kunt gebruiken wanneer u een scoringwebservice maakt die u in productie wilt implementeren. Als u wilt doen, moet u eerst het niveau van de artefacten verhogen tot een asset.

```azure-cli
$ az ml history promote --run <run id> --artifact-path outputs/model.pkl --name model.pkl
```

Hierdoor wordt een map `assets` in uw projectmap gemaakt met een bestand `model.pkl.link`. Dit koppelingsbestand wordt gebruikt om te verwijzen naar een asset die is gemaakt door verhoging van het niveau.

## <a name="step-7-download-the-files-to-be-operationalized"></a>Stap 7. De te operationaliseren bestanden downloaden
Download het model waarvan het niveau is verhoogd, zodat u de bestanden kunt gebruiken voor het maken van een voorspellingswebservice. 

```azure-cli
$ az ml asset download --link-file assets\pickle.link -d asset_download
```

## <a name="step-8-set-up-your-model-management-environment"></a>Stap 8. Uw modelbeheeromgeving instellen 
Maak een omgeving voor het implementeren van webservices. U kunt de webservice op de lokale computer uitvoeren met behulp van Docker. Of deze implementeren op een ACS-cluster voor grootschalige bewerkingen. 

```azure-cli
# Create new local operationalization environment
$ az ml env setup -l <supported Azure region> -n <env name>
# Once setup is complete, set your environment for current context
$ az ml env set -g <resource group name> -n <env name>
```

## <a name="step-9-create-a-model-management-account"></a>Stap 9. Een modelbeheeraccount maken 
Als u uw modellen in productie wilt implementeren en volgen, hebt u een modelbeheeraccount nodig. 

```azure-cli
$ az ml account modelmanagement create -n <model management account name> -g <resource group name> -l <supported Azure region>
```

## <a name="step-10-create-a-web-service"></a>Stap 10. Een webservice maken
Maak een webservice die een voorspelling retourneert met behulp van het model dat u hebt geïmplementeerd. 

```azure-cli
$ az ml service create realtime -m asset_download/model.pkl -f score_iris.py -r python –n <web service name>
```

## <a name="step-11-run-the-web-service"></a>Stap 11. De webservice uitvoeren
Roep de webservice aan met behulp van de webservice-id uit de uitvoer van de vorige stap en test deze. 

```azure-cli
# Get web service usage information 
$ az ml service usage realtime -i <web service id>

# Call the web service with the run command:
$ az ml service run realtime -i <web service id> -d <input data>
```

## <a name="step-12-deleting-all-the-resources"></a>Stap 12. Alle resources verwijderen 
We voltooien deze zelfstudie door alle gemaakte resources te verwijderen, tenzij u ermee wilt blijven werken. 

U doet dit door de resourcegroep te verwijderen waar de resources zich bevinden. 

```azure-cli
az group delete --name <resource group name>
```

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u Azure Machine Learning kunt gebruiken het volgende te doen: 
> [!div class="checklist"]
> * Een Experimenten-account instellen en een werkruimte maken
> * Projecten maken
> * Experimenten verzenden naar verschillende compute-doelen
> * Het niveau van een getraind model verhogen en het model registreren
> * Een modelbeheeraccount maken voor het beheren van modellen
> * Een omgeving maken voor het implementeren van webservices
> * Een webservice implementeren en scoren met nieuwe gegevens

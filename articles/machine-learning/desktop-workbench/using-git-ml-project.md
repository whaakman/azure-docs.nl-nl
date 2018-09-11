---
title: Een Git-repo gebruiken met een Azure Machine Learning Workbench-project | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u een Git-opslagplaats in combinatie met een Azure Machine Learning Workbench-project.
services: machine-learning
author: hning86
ms.author: haining
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 11/18/2017
ms.openlocfilehash: 58ab1d77218595344c899dff654ba5b7a5bfb0d8
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44296633"
---
# <a name="use-a-git-repo-with-a-machine-learning-workbench-project"></a>Een Git-repo gebruiken met een Machine Learning Workbench-project
Meer informatie over hoe Azure Machine Learning Workbench maakt gebruik van Git voor versiebeheer, en zorg ervoor dat reproduceerbaarheid in uw gegevenswetenschapexperiment. Leer hoe u uw project koppelen aan een cloud Git-opslagplaats (opslagplaats).

Machine Learning Workbench is ontworpen voor Git-integratie. Wanneer u een nieuw project maakt, is de projectmap automatisch 'Git geïnitialiseerd' als een lokale Git-opslagplaats. Een tweede, verborgen lokale Git-repo ook is gemaakt, klikt u met een vertakking met de naam AzureMLHistory /\<project GUID\>. De vertakking houdt, is het bijhouden van wijzigingen in de map voor elke uitvoering van project. 

De Azure Machine Learning-project koppelen aan een Git-opslagplaats, kunt u automatische versiebeheer, lokaal en op afstand. De Git-opslagplaats wordt gehost in Azure DevOps. Omdat de Machine Learning-project gekoppeld aan een Git-opslagplaats is, kan iedereen die toegang tot de externe opslagplaats heeft de meest recente broncode downloaden naar een andere computer (roaming).  

> [!NOTE]
> Azure DevOps heeft een eigen toegangsbeheerlijst (ACL), die onafhankelijk van de Azure Machine Learning experimenten-service is. Gebruikerstoegang verschillen tussen een Git-opslagplaats en een Machine Learning-werkruimte of een project. U moet mogelijk om toegang te beheren. 
> 
> Of u wilt een teamlid geven toegang tot uw Machine Learning-project op serverniveau code of deel de werkruimte, moet u aan de gebruiker de juiste machtigingen voor toegang tot de Azure DevOps Git-opslagplaats. 

Voor het beheren van versiebeheer met Git, kunt u gebruik van de master-vertakking of andere vertakkingen maken in de opslagplaats. U kunt ook gebruik van de lokale Git-opslagplaats en push naar de externe Git-opslagplaats, als deze ingericht.

In dit diagram ziet u de relatie tussen een Azure DevOps Git-repo en een Machine Learning-project:

![Azure Machine Learning Git](media/using-git-ml-project/aml_git.png)

Voltooi de stappen die worden beschreven in de volgende secties om te beginnen met behulp van een externe Git-opslagplaats.

> [!NOTE]
> Op dit moment ondersteunt Azure Machine Learning Git-opslagplaatsen alleen op Azure DevOps-organisaties.

## <a name="step-1-create-a-machine-learning-experimentation-account"></a>Step 1. Een Machine Learning experimenten-account maken
Een Machine Learning experimenten-account maken en de app Azure Machine Learning Workbench te installeren. Zie voor meer informatie, [installeren en maken van de snelstartgids](../service/quickstart-installation.md).

## <a name="step-2-create-an-azure-devops-project-or-use-an-existing-project"></a>Stap 2. Een Azure DevOps-project maken of gebruik een bestaand project
In de [Azure-portal](https://portal.azure.com/), een nieuw project maken:
1. Selecteer **+**.
2. Zoeken naar **Team Project**.
3. Voer de vereiste gegevens:
    - **Naam**: de naam van een team.
    - **Versiebeheer**: Selecteer **Git**.
    - **Abonnement**: Selecteer een abonnement waarvoor u een Machine Learning experimenten-account.
    - **Locatie**: in het ideale geval kiest u een regio die zich in de buurt van uw Machine Learning experimenten-resources.
4. Selecteer **Maken**. 

![Een project maken in Azure portal](media/using-git-ml-project/create_vsts_team.png)

Zorg ervoor dat u zich aanmelden met behulp van de dezelfde Azure Active Directory (Azure AD)-account dat u gebruikt voor toegang tot Machine Learning Workbench. Anders wordt het systeem heeft geen toegang tot Machine Learning Workbench met behulp van uw Azure AD-referenties. Een uitzondering hierop is als u de opdrachtregel gebruiken om de Machine Learning-project te maken en een persoonlijk toegangstoken voor toegang tot de Git-opslagplaats. We bespreken in meer detail later in dit artikel.

Ga direct aan het project dat u hebt gemaakt, gebruikt u de URL- https://\<projectnaam\>. visualstudio.com.

## <a name="step-3-set-up-a-machine-learning-project-and-git-repo"></a>Stap 3. Een Machine Learning-project en de Git-opslagplaats instellen

Als u een Machine Learning-project instelt, hebt u twee opties:
- Een Machine Learning-project met een externe Git-opslagplaats maken
- Een bestaand Machine Learning-project koppelen aan een Azure DevOps Git-repo

### <a name="create-a-machine-learning-project-that-has-a-remote-git-repo"></a>Een Machine Learning-project met een externe Git-opslagplaats maken
Machine Learning Workbench openen en een nieuw project maken. In de **Git-repo** voert u de Azure DevOps Git-opslagplaats-URL uit stap 2. Meestal als volgt uitziet: https://\<Azure DevOps-organisatienaam\>.visualstudio.com/_git/\<projectnaam\>

![Een Machine Learning-project met een Git-opslagplaats maken](media/using-git-ml-project/create_project_with_git_rep.png)

U kunt ook het project maken met behulp van de Azure-opdrachtregelprogramma (Azure CLI). U hebt de mogelijkheid van het invoeren van een persoonlijk toegangstoken. Machine Learning kunt u dit token gebruiken voor toegang tot de Git-opslagplaats in plaats van uw Azure AD-referenties:

```
# Create a new project that has a Git repo by using a personal access token.
$ az ml project create -a <Experimentation account name> -n <project name> -g <resource group name> -w <workspace name> -r <Git repo URL> --vststoken <Azure DevOps personal access token>
```

> [!IMPORTANT]
> Als u ervoor de lege projectsjabloon, maken kiest, is de Git-opslagplaats die u wilt gebruiken mogelijk al een master-vertakking. Machine Learning wordt gewoon de master-vertakking lokaal klonen. Dit wordt de map aml_config en andere metagegevens projectbestanden toegevoegd aan de lokale projectmap. 
>
> Als u een andere projectsjabloon, uw Git-repo *kan geen* hebt u al een master-vertakking. Als dit wel gebeurt, ziet u een fout. Het alternatief is het gebruik van de `az ml project create` opdracht voor het maken van het project met een `--force` overschakelen. Dit verwijdert de bestanden in de oorspronkelijke master-vertakking en wordt deze vervangen door de nieuwe bestanden in de sjabloon die u kiest.

Een nieuwe Machine Learning-project is gemaakt, met de externe Git-repo-integratie ingeschakeld. De projectmap is altijd Git geïnitialiseerd als een lokale Git-opslagplaats. De externe Git is ingesteld op de externe Azure DevOps Git-opslagplaats, zodat u kunt doorvoeracties naar de externe Git-opslagplaats pushen.

### <a name="associate-an-existing-machine-learning-project-with-an-azure-devops-git-repo"></a>Een bestaand Machine Learning-project koppelen aan een Azure DevOps Git-repo
U kunt maken van een Machine Learning-project zonder een Azure DevOps Git-repo en zijn afhankelijk van de lokale Git-opslagplaats voor momentopnamen van de uitvoeringsgeschiedenis. U kunt later een Azure DevOps Git-repo koppelen aan deze bestaande Machine Learning-project met behulp van de volgende opdracht uit:

```azurecli
# Ensure that you are in the project path so Azure CLI has the context of your current project.
$ az ml project update --repo https://<Azure DevOps organization name>.visualstudio.com/_git/<project name>
```

> [!NOTE] 
> U kunt de bewerking update opslagplaats alleen op een Machine Learning-project dat u beschikt niet over een Git-opslagplaats die is gekoppeld aan het uitvoeren. Ook nadat u een Git-repo aan een Machine Learning koppelen, u niet kunt verwijderen.

## <a name="step-4-capture-a-project-snapshot-in-the-git-repo"></a>Stap 4. Een momentopname van een project in de Git-opslagplaats vastleggen
Een paar script uitgevoerd in het project uitvoeren en wijzigingen aanbrengen tussen de wordt uitgevoerd. U kunt dit doen in de desktop-app of Azure CLI met behulp van de `az ml experiment submit` opdracht. Zie voor meer informatie de [zelfstudie Iris classificeren](tutorial-classifying-iris-part-1.md). Voor elke uitvoering, als er een wijziging is aangebracht in een bestand in de projectmap, een momentopname van de hele projectmap is doorgevoerd en gepusht naar de externe Git-opslagplaats onder een vertakking met de naam AzureMLHistory /\<project GUID\>. Vertakkingen en doorvoeringen, met inbegrip van de AzureMLHistory weergeven /\<project GUID\> vertakking, Ga naar de URL van het Azure DevOps Git-opslagplaats. 

> [!NOTE] 
> De momentopname is doorgevoerd alleen voordat een script wordt uitgevoerd. Op dit moment een gegevensvoorbereiding kan worden uitgevoerd of een notitieblok cel uitvoering van de momentopname niet geactiveerd.

![Uitvoeringsgeschiedenis vertakking](media/using-git-ml-project/run_history_branch.png)

> [!IMPORTANT] 
> Het is raadzaam als u niet in de geschiedenis-vertakking werkt met behulp van Git-opdrachten. Dit kan leiden tot problemen met de uitvoeringsgeschiedenis. In plaats daarvan gebruik van de master-vertakking of maak andere vertakkingen van uw eigen Git-bewerkingen.

## <a name="step-5-restore-a-previous-project-snapshot"></a>Stap 5. Herstellen van een eerdere momentopname van project 
De hele projectmap terugzetten naar de status van een vorige uitvoeringsgeschiedenis momentopname in Machine Learning Workbench:
1. Selecteer in de activiteit van de balk (zandloper), **uitvoeringen**.
2. In de **Run List** weergeven, selecteert u de uitvoering die u wilt herstellen.
3. In de **uitvoeren details** weergave, selecteer **herstellen**.

![Uitvoeringsgeschiedenis vertakking](media/using-git-ml-project/restore_project.png)

Desgewenst kunt u de volgende opdrachten in de Azure CLI-venster in Machine Learning Workbench:

```azurecli
# Discover the run I want to restore a snapshot from.
$ az ml history list -o table

# Restore the snapshot from a specific run.
$ az ml project restore --run-id <run ID>
```

Wees voorzichtig wanneer u deze opdracht uitvoert. Deze opdracht wordt uitgevoerd, wordt de hele projectmap overschreven door de momentopname die u die specifieke uitvoering is gestart. Uw project blijft in de huidige vertakking. Dit betekent dat u **gaan alle wijzigingen verloren** in de huidige projectmap.  

Mogelijk wilt u gebruikt Git om door te voeren van uw wijzigingen aan de huidige vertakking, voordat u deze bewerking niet uitvoeren.

## <a name="step-6-use-the-master-branch"></a>Stap 6. Gebruik de master-vertakking
Eén manier om te voorkomen dat per ongeluk verlies van de projectstatus van uw huidige is om door te voeren van het project naar de hoofdvertakking van de Git-opslagplaats (of een vertakking die u zelf hebt gemaakt). U kunt Git gebruiken vanaf de opdrachtregel of vanuit uw favoriete Git-clienthulpprogramma om te worden uitgevoerd op de master-vertakking. Bijvoorbeeld:

```sh
# Check status to make sure you are on the master branch (or branch of your choice).
$ git status

# Stage all changes.
$ git add -A

# Commit all changes locally on the master branch.
$ git commit -m 'these are my updates so far'

# Push changes to the remote Azure DevOps Git repo master branch.
$ git push origin master
```

U kunt nu veilig het project terugzetten naar een eerdere momentopname door het voltooien van stap 5. U kunt altijd terugkeren naar de commit die u zojuist hebt gemaakt op de master-vertakking.

## <a name="authentication"></a>Authenticatie
Als u afhankelijk zijn van alleen de functies van de uitvoeringsgeschiedenis in Machine Learning voor het projectmomentopnamen worden gemaakt en deze terugzetten, moet u geen zorgen te hoeven maken over Git-opslagplaats verificatie. Verificatie wordt verwerkt door de laag van Machine Learning experimenten-service.

Als u uw eigen Git-hulpprogramma's gebruiken voor het beheren van versiebeheer, moet u verificatie op basis van de externe Git-opslagplaats in Azure DevOps afhandelt. In Machine Learning, is de externe Git-opslagplaats toegevoegd aan de lokale opslagplaats als een externe Git met behulp van het HTTPS-protocol. Dit betekent dat wanneer u naar de externe Git-opdrachten (zoals push of pull), moet u uw gebruikersnaam en wachtwoord of een persoonlijk toegangstoken opgeven. Volg de instructies in voor het maken van een persoonlijk toegangstoken in een Azure DevOps Git-opslagplaats, [gebruiken een persoonlijk toegangstoken om te verifiëren](https://docs.microsoft.com/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate).

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het [het Team Data Science Process gebruiken om te organiseren van de projectstructuur van uw](how-to-use-tdsp-in-azure-ml.md).

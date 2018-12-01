---
title: Roaming en samenwerking in Azure Machine Learning Workbench | Microsoft Docs
description: Meer informatie over het instellen van roaming en samenwerking in Machine Learning Workbench.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 11/16/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 0117a08c76288cda25da2196ec18433b3678e24b
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52724635"
---
# <a name="roaming-and-collaboration-in-azure-machine-learning-workbench"></a>Roaming en samenwerking in Azure Machine Learning Workbench

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 

Dit artikel wordt beschreven hoe u Azure Machine Learning Workbench kunt gebruiken om te stellen projecten voor roaming tussen computers en samenwerken met teamleden. 

Wanneer u een Azure Machine Learning-project met een externe Git-opslagplaats (opslagplaats) koppeling maakt, worden de metagegevens van het project en de momentopnamen worden opgeslagen in de cloud. U kunt de koppeling van de cloud gebruiken voor toegang tot het project vanaf een andere computer (roaming). Ook kunt u samenwerken met teamleden door zodat ze toegang hebben tot het project. 

## <a name="prerequisites"></a>Vereisten
1. De Machine Learning Workbench-app installeren. Zorg ervoor dat u toegang tot een Azure Machine Learning experimenten-account hebt. Zie voor meer informatie de [installatiehandleiding](quickstart-installation.md).

2. Toegang tot [Azure DevOps](https://www.visualstudio.com) en maak vervolgens een opslagplaats voor uw project te koppelen. Zie voor meer informatie, [met behulp van een Git-opslagplaats met een Machine Learning Workbench-project](using-git-ml-project.md).

## <a name="create-a-new-machine-learning-project"></a>Een nieuw Machine Learning-project maken
Machine Learning Workbench openen en maak vervolgens een nieuw project (bijvoorbeeld, een project met de naam iris). In de **Visualstudio.com GIT Repository URL** voert u een geldige URL voor een Azure DevOps Git-repo. 

> [!IMPORTANT]
> Als u ervoor de lege projectsjabloon, maken kiest, is de Git-opslagplaats die u wilt gebruiken mogelijk al een master-vertakking. Machine Learning wordt gewoon de master-vertakking lokaal klonen. Dit wordt de map aml_config en andere metagegevens projectbestanden toegevoegd aan de lokale projectmap. 
>
> Als u een andere projectsjabloon, uw Git-repo *kan geen* hebt u al een master-vertakking. Als dit wel gebeurt, ziet u een fout. Het alternatief is het gebruik van de `az ml project create` opdracht voor het maken van het project met een `--force` overschakelen. Dit verwijdert de bestanden in de oorspronkelijke master-vertakking en wordt deze vervangen door de nieuwe bestanden in de sjabloon die u kiest.

Nadat het project is gemaakt, verzendt u een paar wordt uitgevoerd op alle scripts die zich in het project. Deze actie voert de projectstatus op vertakking van de uitvoeringsgeschiedenis van de externe Git-opslagplaats. 

> [!NOTE] 
> Trigger-doorvoeracties alleen script uitgevoerd op de vertakking van de uitvoeringsgeschiedenis. Dataprep worden uitgevoerd en een laptop uitvoeringen projectmomentopnamen in de vertakking van de uitvoeringsgeschiedenis niet activeren.

Als u een Git-verificatie hebt ingesteld, kunt u ook gebruiken in de master-vertakking. Of u kunt een nieuwe vertakking maken. 

Voorbeeld: 
```
# Check current repo status.
$ git status

# Stage all changes in the current repo.
$ git add -A

# Commit changes.
$ git commit -m "my commit fixes this weird bug!"

# Push to the remote repo.
$ git push origin master
```

## <a name="roaming"></a>Roaming
<a name="roaming"></a>

### <a name="open-machine-learning-workbench-on-a-second-computer"></a>Machine Learning Workbench op een tweede computer openen
Nadat de Azure DevOps Git-opslagplaats is gekoppeld aan uw project, kunt u het iris-project openen vanaf elke computer met Machine Learning Workbench zijn geïnstalleerd. 

Voor toegang tot het iris-project op een andere computer, moet u zich aan de app aanmelden met dezelfde referenties die u hebt gebruikt om het project te maken. U moet ook worden in de werkruimte en hetzelfde Machine Learning experimenten-account. Het iris-project op alfabetische volgorde weergegeven met andere projecten in de werkruimte. 

### <a name="download-the-project-on-a-second-computer"></a>Het project op een tweede computer downloaden
Wanneer u de werkruimte is geopend op de tweede computer, wordt het pictogram naast het iris-project verschilt van het pictogram van de typische map. Het downloadpictogram geeft aan dat de inhoud van het project zich in de cloud, en dat het project is gereed om te worden gedownload naar de huidige computer. 

![Project maken](./media/roaming-and-collaboration/downloadable-project.png)

Selecteer het iris-project om te beginnen met een download. Wanneer het downloaden is voltooid, wordt het project is gereed om te worden geopend op de tweede computer. 

Op Windows, het project bevindt zich in C:\Users\\< gebruikersnaam\>\Documents\AzureML.

Op Mac OS, het project bevindt zich in /home/\<gebruikersnaam \> /documenten/AzureML.

We willen verbeteren van functies, zodat u kunt een doelmap selecteren in een toekomstige release. 

> [!NOTE]
> Hebt u een map in de adreslijst voor Machine Learning met exact dezelfde naam als het project, mislukt het downloaden. U kunt dit probleem omzeilen, moet u tijdelijk de bestaande map wijzigen.


### <a name="work-on-the-downloaded-project"></a>Werken op het gedownloade project 
De zojuist gedownloade project weerspiegelt de projectstatus op de laatste uitvoering in het project. Een momentopname van de projectstatus is automatisch doorgevoerd op de vertakking van de uitvoeringsgeschiedenis in de Azure DevOps Git-opslagplaats telkens wanneer u een uitvoering verzenden. De momentopname die is gekoppeld aan de meest recente uitvoering wordt gebruikt voor het starten van het project op de tweede computer. 
 

## <a name="collaboration"></a>Samenwerking
U kunt samenwerken met teamleden op projecten die zijn gekoppeld aan een Azure DevOps Git-repo. U kunt machtigingen toewijzen aan gebruikers voor de Machine Learning experimenten-account, een werkruimte en een project. Op dit moment kunt u Azure Resource Manager-opdrachten uitvoeren met behulp van Azure CLI. U kunt ook de [Azure-portal](https://portal.azure.com). Zie voor meer informatie, [Azure portal gebruiken om toe te voegen gebruikers](#portal).    

### <a name="use-the-command-line-to-add-users"></a>De opdrachtregel gebruiken voor het toevoegen van gebruikers
Els wordt bijvoorbeeld de eigenaar van het iris-project. Alice wil de toegang tot het project delen met Bob. 

Els selecteert de **bestand** menu en selecteert de **opdrachtprompt** menu-item. Het opdrachtpromptvenster geopend met het iris-project. Els kunt vervolgens beslissen welk niveau van toegang die ze wil geven tot Bob. Ze verleent machtigingen door het uitvoeren van de volgende opdrachten:  

```azurecli
# Find the Resource Manager ID of the Experimentation account.
az ml account experimentation show --query "id"

# Add Bob to the Experimentation account as a Contributor.
# Bob now has read/write access to all workspaces and projects under the account by inheritance.
az role assignment create --assignee bob@contoso.com --role Contributor --scope <Experimentation account Resource Manager ID>

# Find the Resource Manager ID of the workspace.
az ml workspace show --query "id"

# Add Bob to the workspace as an Owner.
# Bob now has read/write access to all projects under the workspace by inheritance. Bob can invite or remove other users.
az role assignment create --assignee bob@contoso.com --role Owner --scope <workspace Resource Manager ID>
```

Na de roltoewijzing ziet rechtstreeks of door overname Bob het project in de lijst van de Machine Learning Workbench-project. Bob moet mogelijk opnieuw opstarten van de toepassing om te zien van het project. Berend klikt u vervolgens het project kunt downloaden zoals beschreven in [Roaming](#roaming), en beginnen met Els. 

De uitvoeringsgeschiedenis voor alle gebruikers die aan een project samenwerken is doorgevoerd naar dezelfde externe Git-repo. Wanneer Els wordt uitgevoerd een uitvoering, is Bob ziet de uitvoering in de sectie van de uitvoeringsgeschiedenis van het project in de Machine Learning Workbench-app. Bob kunt ook het project terugzetten naar de status van elke uitvoering, inclusief de uitvoeringen die Alice gestart. 

Een externe Git-opslagplaats voor het project wordt gedeeld, Els en Bob kunnen ook samenwerken in de master-vertakking. Indien nodig, kunnen ze ook persoonlijke vertakkingen maken en gebruiken en samenvoegen van Git pull-aanvragen om samen te werken. 

### <a name="use-the-azure-portal-to-add-users"></a>De Azure portal gebruiken voor het toevoegen van gebruikers
<a name="portal"></a>

Machine Learning experimenten-accounts, werkruimten en projecten zijn Azure Resource Manager-resources. Als u wilt toewijzen van rollen, kunt u de **toegangsbeheer (IAM)** herstelkoppeling in de [Azure-portal](https://portal.azure.com). 

Zoek de resource die u toevoegen van gebruikers wilt met behulp van de **alle Resources** weergeven. Selecteer de **toegangsbeheer (IAM)** koppelen en selecteer vervolgens **roltoewijzing toevoegen**. 

<img src="./media/roaming-and-collaboration/iam.png" width="320px">

## <a name="sample-collaboration-workflow"></a>Voorbeeldwerkstroom voor samenwerking
Ter illustratie van de werkstroom voor samenwerking, gaan we nemen een voorbeeld. Contoso-werknemers Alice en Bob wilt samenwerken aan een data science-project met behulp van Machine Learning Workbench. De identiteit behoren tot dezelfde tenant Contoso Azure Active Directory (Azure AD). Hier volgen de stappen Alice en Bob nemen:

1. Alice maakt een lege Git-repo in een Azure DevOps-project. Het Azure DevOps-project moet zich in een Azure-abonnement dat onder de Contoso Azure AD-tenant wordt gemaakt. 

2. Els wordt gemaakt van een Machine Learning experimenten-account, een werkruimte en een Machine Learning Workbench-project op haar computer. Wanneer ze het project maakt, voert ze de URL van het Azure DevOps Git-opslagplaats.

3. Els begint te werken op het project. Ze sommige scripts maakt en een paar runs wordt uitgevoerd. Voor elke uitvoering, wordt een momentopname van de hele projectmap automatisch doorgestuurd als een wijziging aan een vertakking van de uitvoeringsgeschiedenis van het Azure DevOps Git-repo die Machine Learning Workbench wordt gemaakt.

4. Els wordt tevreden is met het werk in uitvoering. Ze wil haar wijzigingen aan in de lokale master-vertakking en push ze vervolgens naar Azure DevOps Git-opslagplaats master-vertakking. Open het project en klik in Machine Learning Workbench, ze opent u het venster opdrachtprompt en voert vervolgens deze opdrachten:
    
    ```sh
    # Verify that the Git remote is pointing to the Azure DevOps Git repo.
    $ git remote -v

    # Verify that the current branch is master.
    $ git branch

    # Stage all changes.
    $ git add -A

    # Commit changes with a comment.
    $ git commit -m "this is a good milestone"

    # Push the commit to the master branch of the remote Git repo in Azure DevOps.
    $ git push
    ```

5. Els wordt Bob toegevoegd aan de werkruimte als een Bijdrager. Ze kan dit doen in Azure portal of met behulp van de `az role assignment` opdracht, zoals eerder is uitgelegd. Els biedt tevens Bob lezen/schrijven machtigingen op de Azure DevOps Git-opslagplaats.

6. Bob op zijn computer, moet u zich aanmeldt bij Machine Learning Workbench. Hij ziet de werkruimte die Els met hem heeft gedeeld. Hij kunt het iris-project dat is vermeld in deze werkruimte zien. 

7. Bob selecteert de naam van het project. Het project wordt gedownload naar deze computer.
    * Het gedownloade project-bestanden zijn een kopie van de momentopname van de meest recente uitvoering die vastgelegd in de uitvoeringsgeschiedenis. Ze zijn niet de laatste doorvoering van de master-vertakking.
    * De lokale projectmap is ingesteld op de master-vertakking, met de niet-klaargezette wijzigingen.

8. Bob kunt bladeren wordt uitgevoerd die zijn uitgevoerd door Els. Hij kunt momentopnamen van een eerdere uitvoeringen herstellen.

9. Bob wil ophalen van de meest recente wijzigingen die Alice gepusht en vervolgens beginnen met werken in een andere vertakking. In Machine Learning Workbench is Bob opent een opdrachtpromptvenster, en Hiermee voert u de volgende opdrachten:

    ```sh
    # Verify that the Git remote is pointing to the Azure DevOps Git repo.
    $ git remote -v

    # Verify that the current branch is master.
    $ git branch

    # Get the latest commit in the Azure DevOps Git master branch and overwrite current files.
    $ git pull --force

    # Create a new local branch named "bob" so that Bob's work is done in the "bob" branch
    $ git checkout -b bob
    ```

10. Bob Hiermee wijzigt u het project en nieuwe uitvoeringen verzendt. De wijzigingen zijn aangebracht op de vertakking bob. Berend uitgevoerd wordt ook zichtbaar voor Els.

11. Bob is gereed voor zijn wijzigingen naar de externe Git-opslagplaats te pushen. Om te voorkomen dat veroorzaken een conflict met de master-vertakking waar Els werkt, pushes Bob zijn werk aan een nieuwe externe vertakking, ook met de naam bob.

    ```sh
    # Verify that the current branch is "bob," and that it has unstaged changes.
    $ git status
    
    # Stage all changes.
    $ git add -A

    # Commit the changes with a comment.
    $ git commit -m "I found a cool new trick."

    # Create a new branch on the remote Azure DevOps Git repo, and then push the changes.
    $ git push origin bob
    ```

12. Als u wilt laten weten Els over de handige nieuwe truc in zijn code, maakt Bob een pull-aanvraag op de externe Git-opslagplaats van de vertakking bob naar de hoofdvertakking. Els kan vervolgens de pull-aanvraag samenvoegen in de master-vertakking.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [met behulp van een Git-opslagplaats met een Machine Learning Workbench-project](using-git-ml-project.md).

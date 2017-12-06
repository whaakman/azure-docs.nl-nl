---
title: Roaming en samenwerking in Azure Machine Learning Workbench | Microsoft Docs
description: Informatie over het instellen van roaming en samenwerking in Machine Learning-Workbench.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/16/2017
ms.openlocfilehash: 137608007716452ec6468f1e13f494b095a11cb0
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="roaming-and-collaboration-in-azure-machine-learning-workbench"></a>Roaming en samenwerking in Azure Machine Learning Workbench
Dit artikel wordt beschreven hoe u Azure Machine Learning Workbench projecten instellen voor roaming tussen computers en samenwerken met teamleden kunt gebruiken. 

Wanneer u een Azure Machine Learning-project met een externe Git-opslagplaats (opslagplaats) koppeling maakt, worden de project-metagegevens en momentopnamen worden opgeslagen in de cloud. U kunt de koppeling voor cloud gebruiken voor toegang tot het project vanaf een andere computer (roaming). Ook kunt u samenwerken met teamleden door hen toegang geven tot het project. 

## <a name="prerequisites"></a>Vereisten
1. De Machine Learning-Workbench app installeren. Zorg ervoor dat u toegang tot een Azure Machine Learning-experimenten-account hebben. Zie voor meer informatie de [installatiehandleiding](quickstart-installation.md).

2. Toegang [Visual Studio Team Services](https://www.visualstudio.com) (Team Services), en maak vervolgens een opslagplaats voor uw project om te koppelen. Zie voor meer informatie [met behulp van een Git-opslagplaats met een Machine Learning Workbench project](using-git-ml-project.md).

## <a name="create-a-new-machine-learning-project"></a>Een nieuwe Machine Learning-project maken
Machine Learning Workbench openen en maak vervolgens een nieuw project (bijvoorbeeld een project met de naam iris). In de **Visualstudio.com GIT-opslagplaats URL** Voer een geldige URL voor een Team Services Git-opslagplaats. 

> [!IMPORTANT]
> Als u de sjabloon leeg project kiest, kan de Git-opslagplaats die u wilt gebruiken al een hoofdvertakking hebben. Machine Learning kloont gewoon de hoofdvertakking lokaal. Deze wordt de map aml_config en andere metagegevens projectbestanden toegevoegd aan de lokale projectmap. 
>
> Als u een andere projectsjabloon, Git-opslagplaats *kan niet* hebt u al een hoofdvertakking. Als dit het geval is, ziet u een fout opgetreden. Het alternatief is de `az ml project create` opdracht het project te maken met een `--force` overschakelen. Dit wordt verwijderd van de bestanden in de oorspronkelijke hoofdvertakking en ze vervangen door de bestanden in de sjabloon die u kiest.

Nadat het project is gemaakt, indienen enkele wordt uitgevoerd op de scripts die zich in het project. Deze actie voert de projectstatus aan van de externe Git-opslagplaats uitvoeringsgeschiedenis vertakking. 

> [!NOTE] 
> Alleen script uitgevoerd trigger doorvoeracties naar de vertakking uitvoeringsgeschiedenis. Gegevens voorbereiden uitvoering en laptop wordt uitgevoerd geen momentopnamen van het project in de vertakking uitvoeringsgeschiedenis activeren.

Als u Git-verificatie hebt ingesteld, kunt u ook gebruiken in de hoofdvertakking. Of u een nieuwe vertakking kunt maken. 

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

### <a name="open-machine-learning-workbench-on-a-second-computer"></a>Machine Learning-Workbench op een tweede computer openen
Nadat het Team Services Git-opslagplaats is gekoppeld aan uw project, kunt u het project iris kunt openen vanaf elke computer met Machine Learning-Workbench is geïnstalleerd een. 

Voor toegang tot het project iris op een andere computer, moet u zich bij de app aanmelden met dezelfde referenties die u hebt gebruikt om het project te maken. Ook moet u zich in hetzelfde account van de Machine Learning experimenteren en werkruimte. Het project iris wordt alfabetisch met andere projecten in de werkruimte weergegeven. 

### <a name="download-the-project-on-a-second-computer"></a>Het project op een tweede computer downloaden
Wanneer u de werkruimte is geopend op de tweede computer, wordt het pictogram naast het project iris verschilt van het pictogram typische map. De downloadpictogram geeft aan dat de inhoud van het project in de cloud, en dat het project is gereed om te worden gedownload naar de huidige computer. 

![Project maken](./media/roaming-and-collaboration/downloadable-project.png)

Selecteer het project iris om te beginnen met een download. Wanneer het downloaden is voltooid, is het project is gereed om te worden geopend op de tweede computer. 

In Windows, het project bevindt zich op C:\Users\\< gebruikersnaam\>\Documents\AzureML.

Op Mac OS, het project bevindt zich op /home/\<gebruikersnaam \> /documenten/AzureML.

We zullen verbeteren functionaliteit zodat u kunt een doelmap selecteren in een toekomstige release. 

> [!NOTE]
> Als u een map in de map voor Machine Learning met exact dezelfde naam als het project hebt, mislukt het downloaden. U kunt dit probleem omzeilen, tijdelijk Wijzig de naam van de bestaande map.


### <a name="work-on-the-downloaded-project"></a>Werken aan het gedownloade project 
De zojuist gedownloade project weerspiegelt de projectstatus op de laatste uitvoering in het project. Een momentopname van de projectstatus wordt automatisch doorgevoerd in de vertakking uitvoeringsgeschiedenis in het Team Services Git-opslagplaats telkens wanneer het verzenden van een uitvoering. De momentopname die is gekoppeld aan de meest recente uitvoeren wordt gebruikt voor het instantiëren van het project op de tweede computer. 
 

## <a name="collaboration"></a>Samenwerking
U kunt samenwerken met teamleden op projecten die zijn gekoppeld aan een Team Services Git-opslagplaats. U kunt machtigingen toewijzen aan gebruikers voor de Machine Learning-experimenten account werkruimte en project. Op dit moment kunt u Azure Resource Manager-opdrachten uitvoeren met behulp van Azure CLI. U kunt ook de [Azure-portal](https://portal.azure.com). Zie voor meer informatie [Azure portal gebruiken om toe te voegen gebruikers](#portal).    

### <a name="use-the-command-line-to-add-users"></a>Gebruik de opdracht gebruikers toevoegen
Els is bijvoorbeeld de eigenaar van het project iris. Alice wil de toegang tot het project met Bob delen. 

Els selecteert de **bestand** menu en selecteert de **opdrachtprompt** menu-item. Het opdrachtpromptvenster geopend met het project iris. Els kunt vervolgens beslissen welk niveau van toegang wil ze geven tot Bob. Ze verleent machtigingen door het uitvoeren van de volgende opdrachten:  

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

Na de roltoewijzing ziet rechtstreeks of door overname Bob het project in de lijst met Machine Learning-Workbench. Bob moet mogelijk opnieuw opstarten van de toepassing om te zien van het project. Bob kan downloadt u het project zoals beschreven in [Roaming](#roaming), en beginnen met Els samenwerken. 

De uitvoeringsgeschiedenis voor alle gebruikers die aan een project samenwerken is doorgevoerd in de dezelfde externe Git-opslagplaats. Als Els wordt uitgevoerd een uitvoering, is Bob ziet de uitvoeren in de sectie uitvoeringsgeschiedenis van het project in de Workbench van Machine Learning-app. Bob kunt ook het project terugzetten naar de status van alle wordt uitgevoerd, met inbegrip van wordt uitgevoerd die Els gestart. 

Een externe Git-opslagplaats voor het project wordt gedeeld, kunnen Alice en Bob ook samenwerken in de hoofdvertakking. Indien nodig, ze kunnen ook persoonlijke vertakkingen en maken en gebruiken Git pull-aanvragen samenvoegingen om samen te werken. 

### <a name="use-the-azure-portal-to-add-users"></a>De Azure portal gebruiken om gebruikers te voegen
<a name="portal"></a>

Machine Learning-experimenten accounts, -werkruimten en projecten zijn Azure Resource Manager-resources. Als u wilt toewijzen van rollen kunt u de **toegangsbeheer** koppelen de [Azure-portal](https://portal.azure.com). 

Vinden van de resource die u wilt dat gebruikers toevoegen aan met behulp van de **alle Resources** weergeven. Selecteer de **toegangsbeheer (IAM)** koppelen, en selecteer vervolgens **gebruikers toevoegen**. 

<img src="./media/roaming-and-collaboration/iam.png" width="320px">

## <a name="sample-collaboration-workflow"></a>Samenwerking voorbeeldwerkstroom
Ter illustratie van de werkstroom samenwerking, kunt u een voorbeeld gaan we doorlopen. Contoso werknemers Alice en Bob wilt samenwerken aan een project van de wetenschappelijke gegevens met behulp van Machine Learning-Workbench. Hun identiteit deel uitmaken van dezelfde tenant Contoso Azure Active Directory (Azure AD). Hier volgen de stappen Alice en Bob nemen:

1. Els maakt een lege Git-opslagplaats in een Team Services-project. Het Team Services-project moet zich in een Azure-abonnement dat wordt gemaakt onder de Contoso Azure AD-tenant. 

2. Els wordt gemaakt van een Machine Learning-experimenten, een werkruimte, en een Machine Learning Workbench-project op hun computers. Wanneer ze het project maakt, voert ze de URL van de Team Services Git-opslagplaats.

3. Els begint te werken op het project. Ze sommige scripts maakt en voert een paar wordt uitgevoerd. Voor elke run wordt een momentopname van de hele projectmap automatisch doorgegeven als een doorvoer voor een vertakking uitvoeringsgeschiedenis van het Team Services Git-opslagplaats die Machine Learning-Workbench worden gemaakt.

4. Els is tevreden het onderhanden werk. Ze wil haar wijzigingen in de lokale hoofdvertakking en vervolgens toepassen op de hoofdvertakking voor Team Services Git-opslagplaats. Open het project en klik in Machine Learning Workbench, ze Hiermee opent u het opdrachtpromptvenster en voert deze opdrachten:
    
    ```sh
    # Verify that the Git remote is pointing to the Team Services Git repo.
    $ git remote -v

    # Verify that the current branch is master.
    $ git branch

    # Stage all changes.
    $ git add -A

    # Commit changes with a comment.
    $ git commit -m "this is a good milestone"

    # Push the commit to the master branch of the remote Git repo in Team Services.
    $ git push
    ```

5. Els wordt Bob toegevoegd aan de werkruimte als medewerker. Ze kan dit doen in de Azure portal of met behulp van de `az role assignment` opdracht, zoals eerder is uitgelegd. Els verleent ook Bob lezen/schrijven machtigingen voor het Team Services Git-opslagplaats.

6. Bob zich aanmeldt bij Machine Learning-Workbench op deze computer. Hij kunt de werkruimte die Els gedeeld met hem zien. Hij kunt het iris project vermeld in deze werkruimte zien. 

7. Bob selecteert de naam van het project. Het project gedownload op deze computer.
    * Het gedownloade project-bestanden zijn een kopie van de momentopname van de meest recente uitvoeren die zijn vastgelegd in de geschiedenis uitvoeren. Ze zijn niet het laatste doorvoeren op de hoofdvertakking.
    * De lokale projectmap is ingesteld op de hoofdvertakking, met de unstaged wijzigingen.

8. Bob kunt bladeren wordt uitgevoerd die zijn uitgevoerd door Els. Hij kunt herstellen momentopnamen van een eerder wordt uitgevoerd.

9. Bob wil ophalen van de meest recente wijzigingen die Els gepusht en start vervolgens werken in een ander filiaal. In Machine Learning Workbench, Bob opent een opdrachtpromptvenster en voert de volgende opdrachten:

    ```sh
    # Verify that the Git remote is pointing to the Team Services Git repo.
    $ git remote -v

    # Verify that the current branch is master.
    $ git branch

    # Get the latest commit in the Team Services Git master branch and overwrite current files.
    $ git pull --force

    # Create a new local branch named "bob" so that Bob's work is done in the "bob" branch
    $ git checkout -b bob
    ```

10. Bob wijzigt van het project en verzendt nieuwe wordt uitgevoerd. De wijzigingen worden aangebracht op de vertakking bob. Berend wordt uitgevoerd wordt ook zichtbaar voor Els.

11. Bob is gereed voor de push-zijn wijzigingen naar de externe Git-opslagplaats. Om te voorkomen, veroorzaken een conflict met de hoofdvertakking, waarbij Els werkt, duwt Berend zijn werk aan een nieuwe externe vertakking bob ook met de naam.

    ```sh
    # Verify that the current branch is "bob," and that it has unstaged changes.
    $ git status
    
    # Stage all changes.
    $ git add -A

    # Commit the changes with a comment.
    $ git commit -m "I found a cool new trick."

    # Create a new branch on the remote Team Services Git repo, and then push the changes.
    $ git push origin bob
    ```

12. Om te laten Els over de handige nieuwe truc in diens code, maakt Bob een pull-aanvraag op de externe Git-opslagplaats van de vertakking bob naar de hoofdvertakking. Els kan vervolgens samenvoegen met de pull-aanvraag de hoofdvertakking.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [met behulp van een Git-opslagplaats met een Machine Learning Workbench project](using-git-ml-project.md).

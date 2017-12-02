---
title: Roaming en samenwerking in Azure Machine Learning-Workbench | Microsoft Docs
description: Lijst met bekende problemen en een handleiding om op te lossen
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/16/2017
ms.openlocfilehash: 81954835185ebaa86c11a9498a85879e6985897a
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/02/2017
---
# <a name="roaming-and-collaboration-in-azure-machine-learning-workbench"></a>Roaming en samenwerking in Azure Machine Learning Workbench
Dit document leert u hoe u Azure Machine Learning Workbench kan helpen uw projecten roamen tussen machines, evenals inschakelen samenwerking met de teamleden. 

Wanneer u een Azure Machine Learning-project maakt met een externe koppeling voor de Git-opslagplaats (opslagplaats), worden de project-metagegevens en momentopnamen worden opgeslagen in de cloud. De cloud-koppeling kunt u toegang tot het project vanaf een andere computer (Roaming). U kunt ook toegang geven tot uw collega's, waardoor u samenwerking. 

## <a name="prerequisites"></a>Vereisten
Installeer eerst de Workbench van Azure Machine Learning met toegang tot een Account experimenteren. Ga als volgt de [installatiehandleiding](quickstart-installation.md) voor meer informatie.

Ten tweede toegang tot [Visual Studio Team System](https://www.visualstudio.com) en maak een opslagplaats voor uw project om te koppelen. Voor gedetailleerde informatie over Git verwijzen naar de [Git-opslagplaats met behulp van een Azure Machine Learning Workbench project](using-git-ml-project.md) artikel.

## <a name="create-a-new-azure-machine-learning-project"></a>Een nieuw Azure Machine Learning-project maken
Azure Machine Learning Workbench Start en een nieuw project maken (bijvoorbeeld _iris_). Vul de **Visualstudio.com GIT-opslagplaats URL** tekstvak met een geldige URL voor VSTS Git-opslagplaats. 

> [!IMPORTANT]
> Als u de sjabloon leeg project kiest, is het OK als u al de Git-opslagplaats is een _master_ vertakking. Azure ML kloont gewoon de _master_ vertakking lokaal en voegt de `aml_config` map en andere projectbestanden metagegevens op de lokale projectmap. Maar als u een andere projectsjabloon, Git-opslagplaats mag niet al hebben een _master_ vertakking, of u een fout ziet. Het alternatief is `az ml project create` opdrachtregelprogramma voor het maken van het project en geef een `--force` overschakelen. Dit wordt verwijderd van de bestanden op de oorspronkelijke hoofdvertakking en vervang ze door de bestanden in de sjabloon die u kiest.

Zodra het project is gemaakt, indienen enkele wordt uitgevoerd op alle scripts in het project. Deze actie voert project staat in van de externe Git-opslagplaats uitvoeringsgeschiedenis vertakking. 

> [!NOTE] 
> Alleen script uitgevoerd trigger doorvoeracties naar de vertakking uitvoeringsgeschiedenis. Gegevens voorbereiden uitvoering of laptop wordt uitgevoerd geen project momentopnamen op de vertakking uitvoeringsgeschiedenis trigger.

Als u setup Git-verificatie hebt, kunt u ook expliciet in de hoofdvertakking werken, of maak een nieuwe vertakking. 

Als u een voorbeeld: 
```
# check current repo status
$ git status

# stage all changes in the current repo
$ git add -A

# commit changes
$ git commit -m "my commit fixes this weird bug!"

# push to remote repo.
$ git push origin master
```

## <a name="roaming"></a>Roaming
<a name="roaming"></a>

### <a name="open-azure-machine-learning-workbench-on-second-machine"></a>Open Azure Machine Learning-Workbench op de tweede machine
Als de VSTS Git-opslagplaats is gekoppeld aan uw project, kunt u toegang tot de _iris_ project vanaf elke computer waarop u Azure Machine Learning Workbench hebt geïnstalleerd. 

Voor toegang tot het project iris op een andere computer, moet u aanmelden bij de app met dezelfde referenties gebruikt tijdens het maken van het project. Bovendien moet u navigeren naar de Account experimenteren en de werkruimte. De _iris_ project alfabetisch met andere projecten in de werkruimte wordt weergegeven. 

### <a name="download-project-on-second-machine"></a>Project op de tweede machine downloaden
Wanneer de werkruimte wordt geopend op de tweede computer, het pictogram naast de _iris_ project verschilt van het pictogram typische map. Het downloadpictogram geeft aan dat de inhoud van het project in de cloud en moet worden gedownload naar de huidige computer. 

![project maken](./media/roaming-and-collaboration/downloadable-project.png)

Klik op de _iris_ project wordt gestart van een downloadactie. Het project is gereed om te worden geopend op de tweede computer na een korte tijd, wanneer het downloaden is voltooid. 

Op Windows is het`C:\Users\<username>\Documents\AzureML`

Op Mac OS is het hier:`/home/<username>/Documents/AzureML`

We zullen de functies waarmee u Selecteer een doelmap in een toekomstige release. 

> [!NOTE]
> Als u een map in de Azure ML-adreslijst met exact dezelfde naam als het project hebben, mislukt het downloaden. Voor het gebruikt wordt, moet u de naam van de bestaande map om dit probleem omzeilen.


### <a name="work-on-the-downloaded-project"></a>Werken aan het gedownloade project 
De zojuist gedownloade project weerspiegelt de projectstatus vanaf de laatste uitvoering in het project. Een momentopname van de projectstatus wordt automatisch doorgevoerd in de vertakking uitvoeringsgeschiedenis in de VSTS Git-opslagplaats telkens wanneer het verzenden van een uitvoering. We gebruiken de momentopname die is gekoppeld aan de meest recente uitvoeren bij het instantiëren van het project op de tweede computer. 
 

## <a name="collaboration"></a>Samenwerking
U kunt samenwerken met uw teamleden op projecten gekoppeld aan een VSTS Git-opslagplaats. U kunt machtigingen toewijzen aan gebruikers op het experimenteren Account, de werkruimte en het Project. Op dit moment kunt kunt u de Azure Resource Manager-opdrachten met de Azure CLI uitvoeren. U kunt ook [Azure-portal](https://portal.azure.com). Zie [volgende sectie](#portal).    

### <a name="using-command-line-to-add-users"></a>Via de opdrachtregel gebruikers toevoegen
U kunt een voorbeeld gebruiken. Stel Els is de eigenaar van th e_Iris_ project en ze wil access delen met Bob. 

Els klikt op de **bestand** menu en selecteert de **opdrachtprompt** menu-item starten van de opdrachtprompt geconfigureerd voor de _iris_ project. Els kan vervolgens bepalen welke voor toegangsniveau ze wil verlenen aan Bob door het uitvoeren van de volgende opdrachten.  

```azurecli
# Find ARM ID of the experimnetation account
az ml account experimentation show --query "id"

# Add Bob to the Experimentation Account as a Contributor.
# Bob now has read/write access to all workspaces and projects under the Account by inheritance.
az role assignment create --assignee bob@contoso.com --role Contributor --scope <experimentation account ARM ID>

# Find ARM ID of the workspace
az ml workspace show --query "id"

# Add Bob to the workspace as an Owner.
# Bob now has read/write access to all projects under the Workspace by inheritance. And he can invite or remove others.
az role assignment create --assignee bob@contoso.com --role Owner --scope <workspace ARM ID>
```

Na de roltoewijzing ziet rechtstreeks of door overname Bob het project in de lijst van de Workbench-project. De toepassing moet mogelijk opnieuw opstarten om te zien van het project. Bob kan downloadt u het project zoals beschreven in de [sectie Roaming](#roaming) en samenwerken met Els. 

De uitvoeringsgeschiedenis voor alle gebruikers die samenwerken aan een project is doorgevoerd in de dezelfde externe Git-opslagplaats. Dus als Els wordt uitgevoerd een uitvoering, ziet Bob het uitvoeren in de sectie uitvoeringsgeschiedenis van het project in de Workbench-app. Bob kunt ook het project terugzetten naar de status van elke uitvoeren met inbegrip van gestart door Els wordt uitgevoerd. 

Delen van een externe Git-opslagplaats voor het project, kunt u Els en Bob ook samenwerken aan de hoofdvertakking. Indien nodig, ze kunnen ook persoonlijke vertakkingen en maken en gebruiken Git, pull-aanvragen samenvoegingen om samen te werken. 

### <a name="using-azure-portal-to-add-users"></a>Gebruikers toevoegen met behulp van Azure-portal
<a name="portal"></a>

Azure Machine Learning-experimenten Accounts, -werkruimten en projecten zijn Azure Resource Manager-resources. U kunt de Access Control-koppeling in de [Azure-portal](https://portal.azure.com) toewijzen van rollen. 

De bron die u wilt toevoegen van gebruikers om weer te geven van alle Resources vinden. Klik op het toegangsbeheer (IAM) koppeling binnen de pagina. Gebruikers toevoegen 

<img src="./media/roaming-and-collaboration/iam.png" width="320px">

## <a name="sample-collaboration-workflow"></a>Samenwerking voorbeeldwerkstroom
Ter illustratie van de stroom samenwerking, kunt u een voorbeeld gaan we doorlopen. Contoso werknemers Alice en Bob wilt samenwerken aan een gegevens-wetenschappelijke-project met behulp van Azure ML-Workbench. Hun identiteit behoren tot dezelfde Contoso Azure AD-tenant.

1. Els maakt eerst een leeg Git-opslagplaats in een project VSTS. Dit project VSTS moet bevinden zich in een Azure-abonnement gemaakt onder het Contoso-AAD-tenant. 

2. Els maakt vervolgens een account van Azure ML experimenteren, een werkruimte en een project Azure ML-Workbench op hun computers. Ze levert de URL van de Git-opslagplaats wanneer het project wordt gemaakt.

3. Els begint te werken op het project. Ze sommige scripts maakt en voert een paar wordt uitgevoerd. Voor elke run wordt automatisch een momentopname van de hele projectmap doorgeschoven in een vertakking uitvoeringsgeschiedenis van de VSTS Git-opslagplaats gemaakt door de Workbench als een doorvoer.

4. Els is nu tevreden het onderhanden werk. Ze wil haar wijziging van de lokale doorvoeren _master_ vertakking en stuurt deze naar de VSTS Git-opslagplaats _master_ vertakking. Om dit te doen met het openen van het project ze het opdrachtpromptvenster vanuit Azure ML Workbench wordt gestart en geeft de volgende opdrachten:
    
    ```sh
    # verify the Git remote is pointing to the VSTS Git repo
    $ git remote -v

    # verify that the current branch is master
    $ git branch

    # stage all changes
    $ git add -A

    # commit changes with a comment
    $ git commit -m "this is a good milestone"

    # push the commit to the master branch of the remote Git repo in VSTS
    $ git push
    ```

5. Bob Els vervolgens toegevoegd aan de werkruimte als medewerker. Ze kunt dit doen vanuit Azure portal of met behulp van de `az role assignment` opdracht illustreren hierboven. Ze hebben ook Bob lees/schrijftoegang tot de VSTS Git-opslagplaats.

6. Bob registreert nu in de Azure ML-Workbench op deze computer. Hij kunt de werkruimte Els gedeeld met hem en het project dat wordt vermeld in deze werkruimte zien. 

7. Berend klikt op de projectnaam en het project is gedownload naar deze computer.
    
    a. Het gedownloade project-bestanden zijn een kopie van de momentopname van de meest recente uitvoeren in de uitvoeringsgeschiedenis vastgelegd. Ze zijn niet het laatste doorvoeren op de hoofdvertakking.
    
    b. De lokale projectmap is ingesteld op _master_ vertakking met de bovenstaande unstaged wijzigingen.

8. Bob kan nu bladeren door Els en herstel momentopname van een eerder uitgevoerde uitgevoerd wordt uitgevoerd.

9. Bob wil ophalen van de meest recente wijzigingen door Els gepusht en starten om te werken op een andere vertakking. Zodat hij opdrachtpromptvenster geopend vanuit Azure ML Workbench en voert de volgende opdrachten:

    ```sh
    # verify the Git remote is pointing to the VSTS Git repo
    $ git remote -v

    # verify that the current branch is master
    $ git branch

    # get the latest commit in VSTS Git master branch and overwrite current files
    $ git pull --force

    # create a new local branch named "bob" so Bob's work is done on the "bob" branch
    $ git checkout -b bob
    ```

10. Hiermee wijzigt u het project nu Bob en het verzenden van nieuwe wordt uitgevoerd. De wijzigingen worden uitgevoerd op de _bob_ vertakking. En wordt uitgevoerd van Bob zichtbaar Alice ook.

11. Berend is nu gereed voor de push-zijn wijzigingen in de externe Git-opslagplaats. Om te voorkomen dat veroorzaken een conflict met _master_ vertakking waar Els werkt, hij wil zijn werk push naar een nieuwe externe vertakking ook met de naam _bob_.

    ```sh
    # verify that the current branch is "bob" and it has unstaged changes
    $ git status
    
    # stage all changes
    $ git add -A

    # commit them with a comment
    $ git commit -m "I found a cool new trick."

    # create a new branch on the remote VSTS Git repo, and push changes
    $ git push origin bob
    ```

12. Bob kan vervolgens Els over de nieuwe cool slag zien in de code en maakt u een pull-aanvraag op de externe Git-opslagplaats van de _bob_ vertakking naar de _master_ vertakking. Els kunt vervolgens de pull-aanvraag in samen _master_ vertakking.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het gebruik van Git met Azure ML-Workbench: [met behulp van Git-opslagplaats met een Azure Machine Learning Workbench-project](using-git-ml-project.md)
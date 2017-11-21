---
title: Met behulp van de Git-opslagplaats met een Azure Machine Learning-Workbench Project | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u een Git-opslagplaats in combinatie met een Azure Machine Learning-Workbench Project.
services: machine-learning
author: hning86
ms.author: haining
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/18/2017
ms.openlocfilehash: fe2a302a32f1b9ec474416704c6cb613cd384a0e
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2017
---
# <a name="using-git-repository-with-an-azure-machine-learning-workbench-project"></a>Met behulp van de Git-opslagplaats met een Azure Machine Learning Workbench-project
Dit document bevat informatie over hoe Azure Machine Learning Workbench Git versiebeheer opgeven en ervoor zorgen dat reproduceerbaarheid in uw experiment van de wetenschappelijke gegevens gebruikt. Instructies voor het koppelen van uw project met een cloud Git-opslagplaats, zijn ook beschikbaar.

## <a name="introduction"></a>Inleiding
Azure Machine Learning-Workbench is ontworpen met Git-integratie compleet. Wanneer u een nieuw project maakt, is de projectmap automatisch 'Git geïnitialiseerd' als een lokale Git-opslagplaats (opslagplaats). Ondertussen ook een tweede verborgen lokale Git-opslagplaats is gemaakt met een vertakking met de naam _AzureMLHistory / < project_GUID >_ voor het bijhouden van wijzigingen van project-map voor elke uitvoering. 

Het Azure ML-project koppelen aan een Git-opslagplaats, gehost in een project Visual Studio Team Service (VSTS), kunt u automatische versiebeheer lokaal en op afstand. Deze koppeling kan iedereen met toegang tot de externe opslagplaats de meest recente broncode downloaden naar een andere computer (roaming).  

> [!NOTE]
> VSTS heeft een eigen toegangsbeheerlijst die onafhankelijk is van Azure Machine Learning experimenteren Service. Gebruikerstoegang kan variëren tussen een Git-opslagplaats en een werkruimte voor Azure ML- of -project en mogelijk moet worden beheerd. Dus als u wilt delen worden in uw Azure ML-Project met een teamlid toegangsniveau van code, net zoals in aanvulling op de werkruimte delen, moet u expliciet hem juiste toegang verlenen tot de VSTS Git-opslagplaats. 

Met Git, het is ook mogelijk versiebeheer expliciet beheren via de _master_ vertakking of door andere filialen te maken in de opslagplaats. U kunt alleen de lokale Git-opslagplaats en u kunt ook push naar de externe Git-opslagplaats als ingericht.

Dit diagram ziet u de relatie tussen een VSTS Git-opslagplaats en een Azure ML-project:

![AML Git](media/using-git-ml-project/aml_git.png)

Volg deze eenvoudige instructies om te beginnen met een externe Git-opslagplaats.

> [!NOTE]
> Azure Machine Learning ondersteunt momenteel alleen Git-opslagplaatsen op VSTS Accounts. Ondersteuning voor algemene Git repo's (zoals GitHub en enz.) is in de toekomst worden gepland.

## <a name="step-1-create-an-azure-ml-experimentation-account"></a>Step 1. Een Azure ML experimenteren-Account maken
Als dat niet al, een Azure ML experimenteren Account maken en installeer de app Azure ML-Workbench. Zie voor meer informatie de [installeren en maak Quick Start](quickstart-installation.md).

## <a name="step-2-create-a-team-project-or-use-an-existing-team-project"></a>Stap 2. Maken van een teamproject of een bestaande teamproject gebruiken
Van [Azure-portal](https://portal.azure.com/), maak een nieuwe **teamproject**.
1. Klik op**+**
2. Zoeken naar **'Project Team'**
3. Geef de vereiste informatie.
    - Naam: Een Teamnaam.
    - Versiebeheer: **Git**
    - Abonnement: De categorie met een Account met Azure Machine Learning-experimenten.
    - Locatie: Blijven in het ideale geval in een regio dat bijna uw resources voor Azure Machine Learning-Experiment.
4. Klik op **Create**. 

![Een teamproject maken vanuit Azure Portal](media/using-git-ml-project/create_vsts_team.png)

Zorg ervoor dat u zich aanmeldt met hetzelfde Azure Active Directory (AAD)-account dat u gebruikt voor toegang tot de Azure Machine Learning-Workbench. Anders wordt het systeem geen toegang tot uw AAD-referenties met tenzij u vanaf de opdrachtregel gebruiken voor het Azure ML-project maken en geef een persoonlijk toegangstoken voor toegang tot de Git-opslagplaats. Meer informatie.

Zodra het teamproject is gemaakt, bent u klaar om te verplaatsen naar de volgende stap.

Rechtstreeks naar het teamproject zojuist hebt gemaakt wilt gaan, de URL is `https://<team_project_name>.visualstudio.com`.

## <a name="step-3-create-a-new-azure-ml-project-with-a-remote-git-repo"></a>Stap 3. Een nieuwe Azure ML-project maken met een externe Git-opslagplaats
Azure ML-Workbench Start en een nieuw project maken. Vul in het tekstvak voor Git-opslagplaats met de VSTS Git-opslagplaats-URL die u via stap 2. Meestal als volgt uitziet:`http://<vsts_account_name>.visualstudio.com/_git/<project_name>`

![Azure ML-Project maken met Git-opslagplaats](media/using-git-ml-project/create_project_with_git_rep.png)

U kunt ook het opdrachtregelprogramma gebruiken project maken. U hebt de optie om op te geven van een persoonlijk toegangstoken. Azure ML kunt dit token gebruiken voor toegang tot de Git-opslagplaats namens u, in plaats van te vertrouwen op uw AAD-referenties:

```
# create a new project with a Git repo and personal access token.
$ az ml project create -a <experimentation account name> -n <project name> -g <resource group name> -w <workspace name> -r <Git repo URL> --vststoken <VSTS personal access token>
```
> [!IMPORTANT]
> Als u de sjabloon leeg project kiest, is het OK als u al de Git-opslagplaats is een _master_ vertakking. Azure ML kloont gewoon de _master_ vertakking lokaal en voegt de `aml_config` map en andere projectbestanden metagegevens op de lokale projectmap. Maar als u een andere projectsjabloon, Git-opslagplaats mag niet al hebben een _master_ vertakking, of u een fout ziet. Het alternatief is `az ml project create` opdrachtregelprogramma voor het maken van het project en geef een `--force` overschakelen. Dit wordt verwijderd van de bestanden op de oorspronkelijke hoofdvertakking en vervang ze door de bestanden in de sjabloon die u kiest.

Een nieuw Azure ML-project wordt nu gemaakt met externe Git-opslagplaats integratie ingeschakeld en gereed is om te gaan. De projectmap is altijd Git geïnitialiseerd als een lokale Git-opslagplaats. En de Git _externe_ is ingesteld op de externe VSTS Git-opslagplaats zodat doorvoeracties naar de externe Git-opslagplaats kunnen worden geactiveerd.

## <a name="step-3a-associate-an-existing-azure-ml-project-with-a-vsts-git-repo"></a>Stap 3a. Een bestaand Azure ML-project koppelen aan een VSTS Git-opslagplaats
U kunt eventueel ook een Azure ML-project zonder een VSTS Git-opslagplaats maken en alleen afhankelijk zijn van de lokale Git-opslagplaats voor uitvoeringsgeschiedenis momentopnamen. En u kunt een VSTS Git-opslagplaats later koppelen aan dit bestaande Azure ML-project met de volgende opdracht:

```azurecli
# make sure you are in the project path so CLI has context of your current project
$ az ml project update --repo http://<vsts_account_name>.visualstudio.com/_git/<project_name>
```

> [!NOTE] 
> U kunt alleen de bewerking update opslagplaats op een Azure ML-project dat u beschikt niet over een Git-opslagplaats gekoppeld uitvoeren. En wanneer de Git-opslagplaats gekoppeld is, kan niet worden verwijderd.

## <a name="step-4-capture-project-snapshot-in-git-repo"></a>Stap 4. Project momentopname in de Git-opslagplaats vastleggen
Nu u een paar script uitgevoerd in het project uitvoeren kunt, moet u enkele wijzigingen middenweg kiest de wordt uitgevoerd. U kunt dit doen vanuit de bureaublad-app of via CLI `az ml experiment submit` opdracht. Voor meer informatie kunt u de [Iris classificeren zelfstudie](tutorial-classifying-iris-part-1.md). Voor elke run als er een wijziging in alle bestanden in de projectmap een momentopname van de hele projectmap is toegewezen en gepusht naar de externe Git-opslagplaats onder een vertakking met de naam `AzureMLHistory/<Project_GUID>`. U kunt de vertakkingen en doorvoeracties weergeven door te bladeren naar de URL van de VSTS Git-opslagplaats en de vertakking vinden. 

> [!NOTE] 
> De momentopname is alleen doorgevoerde voordat een script wordt uitgevoerd. Op dit moment een prep gegevens kan worden uitgevoerd of de uitvoering van een laptop cel de momentopname niet activeren.

![vertakking van de geschiedenis uitvoeren](media/using-git-ml-project/run_history_branch.png)

> [!IMPORTANT] 
> Het is raadzaam als u werken niet in de geschiedenis vertakking zelf met behulp van de Git-opdrachten. In dat geval mogelijk fouten maken uitvoeringsgeschiedenis. Gebruik hoofdvertakking of andere filialen in plaats daarvan maken voor uw eigen Git-bewerkingen.

## <a name="step-5-restore-a-previous-project-snapshot"></a>Stap 5. Herstellen van een eerdere momentopname van project 
De volledige projectmap terugzetten naar de status van een eerdere uitvoeringsgeschiedenis project staat momentopname, vanuit Azure ML Workbench:
1. Klik op **wordt uitgevoerd** in de activiteit balk (pictogram glas uur).
2. Van de **lijst uitvoeren** weergeven, klikt u op de uitvoeren die u wilt herstellen.
3. Van de **uitvoeren Detail** weergeven, klikt u op **herstellen**.

![vertakking van de geschiedenis uitvoeren](media/using-git-ml-project/restore_project.png)

U kunt ook de volgende opdracht gebruiken in het venster Azure ML-Workbench CLI.

```azurecli
# discover the run I want to restore snapshot from:
$ az ml history list -o table

# restore the snapshot from a particular run
$ az ml project restore --run-id <run_id>
```

Door deze opdracht wordt uitgevoerd, overschrijft we de hele project-map met de momentopname die wordt uitgevoerd wanneer dat bepaalde uitvoering is gestart. Maar uw project blijft op de huidige vertakking. Dit betekent dat u gaat **gaan alle wijzigingen verloren** in de huidige projectmap. Dus zorg extra dat wanneer u deze opdracht uitvoert. Kan u Git uw wijzigingen doorvoeren aan de huidige vertakking voordat u de bovenstaande bewerking uitvoert. Raadpleeg voordat voor meer informatie.

## <a name="step-6-use-the-master-branch"></a>Stap 6. Gebruik de hoofdvertakking
Een manier om te voorkomen dat per ongeluk is verliezen van de huidige projectstatus, het project doorvoeren naar de hoofdvertakking (of een vertakking die u zelf hebt gemaakt) van de Git-opslagplaats. U kunt rechtstreeks Git uit vanaf de opdrachtregel (of uw andere favoriete Git clienthulpprogramma naar keuze) bewerkingen uitvoeren op de hoofdvertakking. Bijvoorbeeld:

```
# check status to make sure you are on the master branch (or branch of your choice)
$ git status

# stage all changes
$ git add -A

# commit all changes locally on the master branch
$ git commit -m 'these are my updates so far'

# push changes into the remote VSTS Git repo master branch.
$ git push origin master
```

Nu u veilig project naar een eerdere momentopname terugzetten kunt volgende stap 5 weten dat u altijd naar het doorvoeren u zojuist hebt terugkeren kunt aangebracht op de masterserver vertakking.

## <a name="authentication"></a>Authentication
Als u alleen de uitvoeringsgeschiedenis functies in Azure ML vertrouwen voor het maken van momentopnamen van het project en deze terug te zetten, u hoeft niet te hoeven maken over de verificatie van Git-opslagplaats. Het is afgehandeld door de laag experimenteren Service.

Echter, als u uw eigen Git-hulpprogramma's gebruiken voor het beheren van versiebeheer, u moet verificatie op basis van de externe Git-opslagplaats op VSTS correct wordt afgehandeld. In de Azure ML externe Git-opslagplaats toegevoegd aan de lokale opslagplaats als Git remote via HTTPS-protocol. Dit betekent dat wanneer u de opdracht Git-opdrachten naar de externe (zoals push of pull), moet u de gebruikersnaam en wachtwoord of persoonlijke toegangstoken te bieden. Ga als volgt [deze instructies](https://docs.microsoft.com/vsts/accounts/use-personal-access-tokens-to-authenticate) persoonlijke toegangstoken maken in een VSTS Git-opslagplaats.

## <a name="next-steps"></a>Volgende stappen
Informatie over het gebruik van het Team gegevens wetenschap proces om te ordenen van de projectstructuur van uw, Zie [structuur van een project met TDSP](how-to-use-tdsp-in-azure-ml.md)

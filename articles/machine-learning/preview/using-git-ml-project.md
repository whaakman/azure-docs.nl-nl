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
ms.date: 11/16/2017
ms.openlocfilehash: c91eadd69eaf16b2496f4d7247e5b0121904e172
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="using-git-repository-with-an-azure-machine-learning-workbench-project"></a>Met behulp van de Git-opslagplaats met een Azure Machine Learning Workbench-project
Dit document bevat informatie over hoe Azure Machine Learning Workbench Git gebruikt om ervoor te zorgen reproduceerbaarheid in uw gegevens wetenschappelijke experiment. Instructies voor het koppelen van uw project met een cloud Git-opslagplaats, zijn ook beschikbaar.

## <a name="introduction"></a>Inleiding
Azure Machine Learning-Workbench is ontworpen met Git-integratie compleet. Wanneer u een nieuw project maakt, de projectmap is automatisch 'Git geïnitialiseerd' als een lokale Git-opslagplaats (opslagplaats) terwijl een tweede verborgen lokale Git-opslagplaats tevens met een vertakking met de naam gemaakt _AzureMLHistory / < project_GUID >_ naar bijhouden van project map wijzigingen voor elke uitvoering. 

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


> [!TIP]
> Zorg ervoor dat u zich aanmeldt met het Azure Active Directory (AAD)-account gebruikt voor toegang tot de Azure Machine Learning-Workbench. Anders wordt het nieuwe teamproject mogelijk end tot onder de verkeerde Tenant-ID en Azure Machine Learning mogelijk niet vinden. In dit geval moet u via de opdrachtregelinterface en geef het token VSTS.

Zodra het teamproject is gemaakt, bent u klaar om te verplaatsen naar de volgende stap.

Rechtstreeks naar het teamproject zojuist hebt gemaakt wilt gaan, de URL is `https://<team_project_name>.visualstudio.com`.

> [!NOTE]
> Azure Machine Learning ondersteunt momenteel alleen lege Git repo's met geen hoofdvertakking. Vanuit de opdrachtregelinterface, kunt u met het--force-argument voor de hoofdvertakking eerst verwijderen. 

## <a name="step-3-create-a-new-azure-ml-project-with-a-remote-git-repo"></a>Stap 3. Een nieuwe Azure ML-project maken met een externe Git-opslagplaats
Azure ML-Workbench Start en een nieuw project maken. Vul in het tekstvak voor Git-opslagplaats met de VSTS Git-opslagplaats-URL die u via stap 2. Meestal als volgt uitziet:`http://<vsts_account_name>.visualstudio.com/_git/<project_name>`

![Azure ML-Project maken met Git-opslagplaats](media/using-git-ml-project/create_project_with_git_rep.png)

Een nieuw Azure ML-project wordt nu gemaakt met externe Git-opslagplaats integratie ingeschakeld en gereed is om te gaan. De projectmap is altijd Git geïnitialiseerd als een lokale Git-opslagplaats. En de Git _externe_ is ingesteld op de externe VSTS Git-opslagplaats zodat doorvoeracties naar de externe Git-opslagplaats kunnen worden geactiveerd.

## <a name="step-3a-associate-an-existing-azure-ml-project-with-a-vsts-git-repo"></a>Stap 3a. Een bestaand Azure ML-project koppelen aan een VSTS Git-opslagplaats
U kunt eventueel ook een Azure ML-project zonder een VSTS Git-opslagplaats maken en alleen afhankelijk zijn van de lokale Git-opslagplaats voor uitvoeringsgeschiedenis momentopnamen. En u kunt een VSTS Git-opslagplaats later koppelen aan dit bestaande Azure ML-project met de volgende opdracht:

```azurecli
# make sure you are in the project path so CLI has context of your current project
$ az ml project update --repo http://<vsts_account_name>.visualstudio.com/_git/<project_name>
```

## <a name="step-4-capture-project-snapshot-in-git-repo"></a>Stap 4. Project momentopname in de Git-opslagplaats vastleggen
Nu u een paar wordt uitgevoerd in het project uitvoeren kunt, moet u sommige wijzigingen middenweg kiest de wordt uitgevoerd. U kunt dit doen vanuit de bureaublad-app of via CLI `az ml experiment submit` opdracht. Voor meer informatie kunt u de [Iris classificeren zelfstudie](tutorial-classifying-iris-part-1.md). Voor elke run als er een wijziging in alle bestanden in de projectmap een momentopname van de hele projectmap is toegewezen en gepusht naar de externe Git-opslagplaats onder een vertakking met de naam `AzureMLHistory/<Project_GUID>`. U kunt de vertakkingen en doorvoeracties weergeven door te bladeren naar de URL van de VSTS Git-opslagplaats en de vertakking vinden. 

![vertakking van de geschiedenis uitvoeren](media/using-git-ml-project/run_history_branch.png)

Opmerking is het beter werkt niet in de geschiedenis vertakking zelf. In dat geval mogelijk fouten maken met de uitvoeringsgeschiedenis. Gebruik hoofdvertakking of andere filialen in plaats daarvan maken voor uw eigen Git-bewerkingen.

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

Door deze opdracht wordt uitgevoerd, overschrijft we de hele project-map met de momentopname die wordt uitgevoerd wanneer dat bepaalde uitvoering is gestart. Maar uw project blijft op de huidige vertakking. Dit betekent dat u gaat **gaan alle wijzigingen verloren** in de huidige projectmap. Dus zorg extra dat wanneer u deze opdracht uitvoert.

## <a name="step-6-use-the-master-branch"></a>Stap 6. Gebruik de hoofdvertakking
Een manier om te voorkomen dat per ongeluk is verliezen van de huidige projectstatus, het project doorvoeren naar de hoofdvertakking (of een vertakking die u zelf hebt gemaakt) van de Git-opslagplaats. U kunt rechtstreeks Git uit vanaf de opdrachtregel (of uw andere favoriete Git clienthulpprogramma naar keuze) bewerkingen uitvoeren op de hoofdvertakking. Bijvoorbeeld:

```
# make sure you are on the master branch (or branch of your choice)
$ git checkout master

# stage all changes
$ git add -A

# commit all changes locally on the master branch
$ git commit -m 'this is my updates so far'

# push changes into the remote VSTS Git repo master branch.
$ git push origin master
```

Nu u veilig project naar een eerdere momentopname terugzetten kunt volgende stap 5 weten dat u altijd naar het doorvoeren u zojuist hebt terugkeren kunt aangebracht op de masterserver vertakking.

## <a name="authentication"></a>Authentication
Als u alleen de uitvoeringsgeschiedenis functies in Azure ML vertrouwen voor het maken van momentopnamen van het project en deze terug te zetten, u hoeft niet te hoeven maken over de verificatie van Git-opslagplaats. Het is afgehandeld door de laag experimenteren Service.

Echter, als u uw eigen Git-hulpprogramma's gebruiken voor het beheren van versiebeheer, u moet verificatie op basis van de externe Git-opslagplaats op VSTS correct wordt afgehandeld. Dat wil zeggen, moet u verificatie met de Git-opslagplaats op de lokale computer instellen voordat u kunt de opdracht Git-opdrachten op die externe Git-opslagplaats. 

De eenvoudigste manier om dit doet, is een SSH-sleutelpaar maken en uploaden van een deel van de openbare sleutel in de beveiligingsinstellingen van de Git-opslagplaats.

### <a name="generate-ssh-key"></a>SSH-sleutel genereren 
Eerst gaan we een paar SSH-sleutels op uw computer te genereren.

#### <a name="on-windows"></a>In Windows:
Start Git GUI bureaublad-app voor Windows en onder _Help_ menu, klik op _weergeven SSH-sleutel_.

![SSH sleutel](media/using-git-ml-project/git_gui.png)

SSH naar Klembord kopiëren.

#### <a name="on-macos"></a>Op Mac OS:
Snelle stappen van de opdrachtshell:
```
# generate the SSH key
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

# start the SSH agent in the background
$ eval "$(ssh-agent -s)"

# add newly generated SSH key to the SSH agent
$ ssh-add -K ~/.ssh/id_rsa

# display the public key so you can copy it.
$ more ~/.ssh/id_rsa.pub
```
Stappen voor meer informatie vindt u op [in dit artikel GitHub](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).

### <a name="upload-public-key-to-git-repo"></a>Openbare sleutel uploaden naar Git-opslagplaats
Ga naar de startpagina van de Visual Studio-account: https://<vsts_account_name>.visualstudio.com en zich aanmelden, klikt u op beveiliging onder uw avatar.

Voegt u een openbare SSH-sleutel en plak de openbare SSH-sleutel van de vorige stap dat u een naam geven. U kunt meerdere sleutels hier toevoegen.

Nu kunt u de Git-opdrachten lokaal op basis van de opslagplaats verwijderen uitgeven met er is geen verdere expliciete verificatie vereist.

### <a name="read-more"></a>Meer informatie
Volg deze twee artikelen (ofwel benadering kunt werken) voor meer informatie over het inschakelen van lokale verificatie van de externe Git-opslagplaats in VSTS.
- [SSH-sleutelverificatie gebruiken](https://www.visualstudio.com/en-us/docs/git/use-ssh-keys-to-authenticate)
- [Gebruik Git referentie Managers](https://www.visualstudio.com/en-us/docs/git/set-up-credential-managers)


## <a name="next-steps"></a>Volgende stappen
Informatie over het gebruik van het Team gegevens wetenschap proces om te ordenen van de projectstructuur van uw, Zie [structuur van een project met TDSP](how-to-use-tdsp-in-azure-ml.md)

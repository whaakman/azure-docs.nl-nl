---
title: Samenwerking coderen met Git - Azure Machine Learning | Microsoft Docs
description: Het gezamenlijke code ontwikkeling voor gegevens wetenschappelijke projecten met Git met de flexibele planning.
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: bradsev;
ms.openlocfilehash: e6808c0d9d4ca14101eeebe8b3e286d64e2328f9
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2017
---
# <a name="collaborative-coding-with-git"></a>Samenwerking coderen met Git

In dit artikel wordt beschreven hoe gezamenlijke code ontwikkeling voor gegevens wetenschappelijke projecten met Git als ontwikkelframework voor de gedeelde code. Er wordt aangegeven hoe deze activiteiten in gepland werk coderen koppelen [flexibele ontwikkeling](agile-development.md) en hoe u kunt beoordelingen code.


## 1. <a name='Linkaworkitemwithagitbranch-1'></a>Een werkitem met een vertakking Git koppelen 

VSTS biedt een handige manier een werkitem (een artikel of taak) verbinding maken met een vertakking Git. Hiermee kunt u uw artikel of de taak directe koppeling naar de code die is gekoppeld. 

Voor een werkitem verbinding met een nieuwe vertakking, dubbelklikt u op een werkitem en klik in het pop-upvenster op **maakt u een nieuwe vertakking** onder **+ koppeling toevoegen**.  

![1](./media/collaborative-coding-with-git/1-sprint-board-view.png)

Geef de gegevens voor deze nieuwe vertakking, zoals de naam van het filiaal, base Git-opslagplaats en de vertakking. De Git-opslagplaats gekozen moet de opslagplaats onder de dezelfde teamproject dat het werkitem behoort. De base vertakking is de hoofdvertakking of andere bestaande vertakken.

![2](./media/collaborative-coding-with-git/2-create-a-branch.png)

Een aanbevolen procedure is het maken van een vertakking Git voor elk artikel werkitem. Vervolgens maakt u voor elk Taakwerkitem een vertakking op basis van de vertakking verhaal. De vertakkingen ordenen op deze hiërarchische manier die overeenkomt met de relaties verhaal-taak is handig als er meerdere personen op andere artikelen van hetzelfde project werkt of er meerdere personen op andere taken van hetzelfde artikel werkt. Conflicten kunnen worden geminimaliseerd wanneer elk teamlid werkt op een andere vertakking en wanneer elk lid op verschillende codes of andere artefacten werkt bij het delen van een vertakking. 

De volgende afbeelding ziet u de aanbevolen vertakkende strategie voor TDSP. U moet u mogelijk niet als veel filialen zoals zijn hier, vooral wanneer u alleen hebt een of twee mensen werken op hetzelfde project of slechts één persoon voor alle taken van een artikel werkt. Maar scheiden van de vertakking van de ontwikkeling van de hoofdvertakking is altijd een goede gewoonte. Dit kan helpen voorkomen dat de vertakking release wordt onderbroken door de van ontwikkelingsactiviteiten. Gedetailleerde beschrijving van Git vertakking model vindt u in [een geslaagde Git vertakking Model](http://nvie.com/posts/a-successful-git-branching-model/).

![3](./media/collaborative-coding-with-git/3-git-branches.png)

Als u wilt overschakelen naar de vertakking die u wilt werken, moet u de volgende opdracht uitvoeren in een shell-opdracht (Windows of Linux). 

    git checkout <branch name>

Het wijzigen van de *< Filiaalnaam\>*  naar **master** switches u terug naar de **master** vertakking. Nadat u naar de vertakking werkt overschakelt, kunt u op dat het werkitem werkt, ontwikkelen van de code of documentatie artefacten nodig voor het voltooien van het item kunt starten. 

U kunt ook een werkitem koppelen aan een bestaande vertakking. In de **Detail** pagina van een werkitem, in plaats van te klikken op **maakt u een nieuwe vertakking**, u klikt op **+ koppeling toevoegen**. Selecteer vervolgens de vertakking die u wilt het werkitem koppelen. 

![4](./media/collaborative-coding-with-git/4-link-to-an-existing-branch.png)

U kunt ook een nieuwe vertakking in Git Bash opdrachten maken. Als < naam base filiaal\> ontbreekt, de < naam van de nieuwe filiaal\> is gebaseerd op _master_ vertakking. 
    
    git checkout -b <new branch name> <base branch name>


## 2. <a name='WorkonaBranchandCommittheChanges-2'></a>Werken met een vertakking en de wijzigingen worden doorgevoerd 

Nu Stel dat u een wijziging in de *gegevens\_opname* vertakking voor het werkitem, zoals het toevoegen van een R-bestand op de vertakking in uw lokale computer. U kunt het R-bestand dat is toegevoegd aan de vertakking voor dit werkitem doorvoeren op voorwaarde dat u in dat filiaal de Git-shell, met de volgende Git-opdrachten:

    git status
    git add .
    git commit -m"added a R scripts"
    git push origin data_ingestion

![5](./media/collaborative-coding-with-git/5-sprint-push-to-branch.png)

## 3. <a name='CreateapullrequestonVSTS-3'></a>Maak een pull-aanvraag op VSTS 

Wanneer u klaar na een paar doorvoeracties en pushes bent voor het samenvoegen van de huidige vertakking in de base vertakking, kunt u indienen een **pull-aanvraag** op VSTS-server. 

Ga naar de hoofdpagina van uw teamproject en klik op **CODE**. Selecteer de vertakking worden samengevoegd en de naam van de Git-opslagplaats die u wilt samenvoegen van de vertakking in. Klik vervolgens op **Pull-aanvragen**, klikt u op **nieuwe pull-aanvraag** te maken van een beoordeling pull-aanvraag voordat het werk op de vertakking is samengevoegd in de base vertakking.

![6](./media/collaborative-coding-with-git/6-spring-create-pull-request.png)

Vul in een beschrijving van deze pull-aanvraag, revisoren toevoegen en verzend.

![7](./media/collaborative-coding-with-git/7-spring-send-pull-request.png)

## 4. <a name='ReviewandMerge-4'></a>Bekijk en samenvoegen 

Als de pull-aanvraag is gemaakt, krijgt uw revisoren een e-mailbericht voor het controleren van de pull-aanvragen. Revisoren moeten controleren of de wijzigingen of niet werkt, en indien mogelijk wijzigingen met de aanvrager testen. Op basis van hun beoordeling, kunnen de revisoren goedkeuren of afwijzen van de pull-aanvraag. 

![8](./media/collaborative-coding-with-git/8-add_comments.png)

![9](./media/collaborative-coding-with-git/9-spring-approve-pullrequest.png)

Nadat de controle is voltooid, de werkvertakking wordt samengevoegd aan de basis vertakking door te klikken op de **Complete** knop. U kunt de werkvertakking verwijderen nadat deze is samengevoegd. 

![10](./media/collaborative-coding-with-git/10-spring-complete-pullrequest.png)

Bevestig op de linkerbovenhoek die de aanvraag is gemarkeerd als **voltooid**. 

![11](./media/collaborative-coding-with-git/11-spring-merge-pullrequest.png)

Wanneer u teruggaan naar de opslagplaats onder **CODE**, wordt vermeld dat u hebt is overgeschakeld naar de hoofdvertakking.

![12](./media/collaborative-coding-with-git/12-spring-branch-deleted.png)

U kunt ook de volgende Git-opdrachten gebruiken de werkvertakking verwijderen na het samenvoegen van en uw werkvertakking aan de basis vertakking samenvoegen:

    git checkout master
    git merge data_ingestion
    git branch -d data_ingestion

![13](./media/collaborative-coding-with-git/13-spring-branch-deleted-commandline.png)


 
## <a name="next-steps"></a>Volgende stappen

[Uitvoeren van taken die gegevens wetenschappelijke](execute-data-science-tasks.md) laat zien hoe u de hulpprogramma's gebruiken om verschillende algemene gegevens wetenschappelijke taken zoals interactieve gegevensverkenning, data-analyse, rapportage en maken van het model te voltooien.

Scenario's die de stappen in het proces voor het demonstreren **specifieke scenario's** worden ook gegeven. Ze worden weergegeven en gekoppeld aan de miniatuur beschrijvingen in de [voorbeeld-scenario's](walkthroughs.md) artikel. Ze te laten zien hoe cloud, 's voor on-premises en -services combineren in een werkstroom of pijplijn een intelligente toepassing maken. 


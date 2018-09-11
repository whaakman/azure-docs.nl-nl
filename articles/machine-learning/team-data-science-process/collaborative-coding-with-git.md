---
title: Gezamenlijke code met Git - Azure Machine Learning | Microsoft Docs
description: Het gezamenlijke code-ontwikkeling voor data science-projecten met behulp van Git met flexibele planning.
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: deguhath
ms.openlocfilehash: 78bbdb244d9bd52a06623f7a6fa3bca123ef3828
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44300066"
---
# <a name="collaborative-coding-with-git"></a>Samenwerken aan code met Git

In dit artikel wordt beschreven hoe u gezamenlijke code-ontwikkeling voor data science projecten met Git gebruiken als de gedeelde code-raamwerk voor ontwikkelaars. Dit wordt uitgelegd hoe u deze activiteiten met het werk in in gepland coderen koppelen [Agile-ontwikkeling](agile-development.md) en hoe u beoordelingen programmeert.


## 1. <a name='Linkaworkitemwithagitbranch-1'></a>Een werkitem met een Git-branch koppelen 

Azure DevOps-Services biedt een handige manier om een werkitem (een artikel of taak) verbinding te maken met een Git-branch. Hiermee kunt u uw verhaal of taak directe koppeling naar de code die is gekoppeld aan deze. 

Voor een werkitem verbinding met een nieuwe vertakking, dubbelklikt u op een werkitem en klik in het pop-upvenster op **maakt u een nieuwe vertakking** onder **+ koppeling toevoegen**.  

![1](./media/collaborative-coding-with-git/1-sprint-board-view.png)

Geef de informatie voor deze nieuwe vertakking, zoals de naam van het filiaal, basis Git-opslagplaats en de vertakking. De Git-opslagplaats gekozen moet de opslagplaats onder hetzelfde project die deel uitmaakt van het werkitem op. De basis-vertakking kan worden de master-vertakking of enige andere bestaande vertakking.

![2](./media/collaborative-coding-with-git/2-create-a-branch.png)

Een goede gewoonte is het maken van een Git-branch voor elk werkitem verhaal. Vervolgens maakt u voor elk Taakwerkitem een vertakking op basis van de vertakking verhaal. De branches organiseren op deze hiërarchische manier die overeenkomt met de relaties verhaal-taak is handig als u meerdere mensen werken aan verschillende verhalen van hetzelfde project, of u hebt meerdere mensen werken aan verschillende taken van hetzelfde artikel. Conflicten kunnen worden geminimaliseerd wanneer elk teamlid werkt op een andere vertakking en op elk lid werkt op verschillende codes of andere artefacten bij het delen van een vertakking. 

De volgende afbeelding ziet u de aanbevolen vertakkingen strategie voor de TDSP. U moet u mogelijk niet veel vertakkingen zoals zijn hier wordt weergegeven, met name wanneer u slechts één hebt of twee personen die samenwerken aan hetzelfde project, of slechts één persoon werkt voor alle taken van een artikel. Maar het scheiden van de vertakking van de ontwikkeling van de master-vertakking is altijd een goede gewoonte. Dit kan helpen te voorkomen dat de releasebranch wordt onderbroken door de van ontwikkelingsactiviteiten. Gedetailleerde beschrijving van de Git-branch-model kunt u vinden in [een geslaagde Git vertakkingen Model](http://nvie.com/posts/a-successful-git-branching-model/).

![3](./media/collaborative-coding-with-git/3-git-branches.png)

Als u wilt overschakelen naar de vertakking die u wilt werken, voer de volgende opdracht in een shell-opdracht (Windows of Linux). 

    git checkout <branch name>

Wijzigen van de *< Filiaalnaam\>*  naar **master** switches u terug naar de **master** vertakking. Nadat u naar de vertakking overschakelt, kunt u aan dat het werkitem werkt, het ontwikkelen van de code, documentatie of artefacten die nodig zijn om uit te voeren van het item starten. 

U kunt ook een werkitem koppelen aan een bestaande vertakking. In de **Detail** pagina van een werkitem, in plaats van te klikken op **maakt u een nieuwe vertakking**, u **+ koppeling toevoegen**. Selecteer vervolgens de vertakking die u wilt koppelen van het werkitem op. 

![4](./media/collaborative-coding-with-git/4-link-to-an-existing-branch.png)

U kunt ook een nieuwe vertakking in de Git Bash opdrachten maken. Als < naam base filiaal\> ontbreekt, de < naam van de nieuwe vertakking\> is gebaseerd op _master_ vertakking. 
    
    git checkout -b <new branch name> <base branch name>


## 2. <a name='WorkonaBranchandCommittheChanges-2'></a>Werken met een vertakking en de wijzigingen doorvoeren 

Nu Stel dat u een wijziging in de *gegevens\_opname* vertakking voor het werkitem, zoals het toevoegen van een R-bestand op de vertakking in uw lokale computer. U kunt het R-bestand dat is toegevoegd aan de vertakking voor dit werkitem doorvoeren op voorwaarde dat u in dat filiaal in de Git shell met de volgende Git-opdrachten:

    git status
    git add .
    git commit -m"added a R scripts"
    git push origin data_ingestion

![5](./media/collaborative-coding-with-git/5-sprint-push-to-branch.png)

## 3. <a name='CreateapullrequestonVSTS-3'></a>Een pull-aanvraag maken op Azure DevOps-Services 

Wanneer u klaar na een paar doorvoeringen en pushes bent, voor het samenvoegen van de huidige vertakking in de basis vertakking, kunt u indienen een **pull-aanvraag** op Azure DevOps-Services. 

Ga naar de hoofdpagina van uw project en klik op **CODE**. Selecteer de vertakking worden samengevoegd en de naam van de Git-opslagplaats die u wilt samenvoegen van de vertakking in. Klik vervolgens op **Pull-aanvragen**, klikt u op **nieuwe pull-aanvraag** te maken van een beoordeling van pull-aanvraag voordat het werk op de vertakking wordt samengevoegd naar de basis-vertakking.

![6](./media/collaborative-coding-with-git/6-spring-create-pull-request.png)

Vul in sommige beschrijving over deze pull-aanvraag, revisoren toevoegen en verzend.

![7](./media/collaborative-coding-with-git/7-spring-send-pull-request.png)

## 4. <a name='ReviewandMerge-4'></a>Controleren en samenvoegen 

Als de pull-aanvraag is gemaakt, krijgt de revisoren een e-mailmelding om te controleren van de pull-aanvragen. De revisoren nodig hebt om te controleren of de wijzigingen of niet werkt en de wijzigingen met de aanvrager indien mogelijk testen. Op basis van hun evaluatie, kunnen de revisoren goedkeuren of afwijzen van de pull-aanvraag. 

![8](./media/collaborative-coding-with-git/8-add_comments.png)

![9](./media/collaborative-coding-with-git/9-spring-approve-pullrequest.png)

Nadat de evaluatie is voltooid, de werkvertakking die aan de basis vertakking is samengevoegd door te klikken op de **voltooid** knop. U kunt kiezen om te verwijderen van de werkvertakking nadat deze is samengevoegd. 

![10](./media/collaborative-coding-with-git/10-spring-complete-pullrequest.png)

Bevestig op de linkerbovenhoek die de aanvraag is gemarkeerd als **voltooid**. 

![11](./media/collaborative-coding-with-git/11-spring-merge-pullrequest.png)

Wanneer u gaat u terug naar de opslagplaats onder **CODE**, bent u laten weten dat u hebt gewijzigd in de master-vertakking.

![12](./media/collaborative-coding-with-git/12-spring-branch-deleted.png)

U kunt ook de volgende Git-opdrachten gebruiken uw werkvertakking aan de basis vertakking samenvoegen en verwijderen van de werkvertakking na het samenvoegen van:

    git checkout master
    git merge data_ingestion
    git branch -d data_ingestion

![13](./media/collaborative-coding-with-git/13-spring-branch-deleted-commandline.png)


 
## <a name="next-steps"></a>Volgende stappen

[Uitvoeren van datatechnologietaken](execute-data-science-tasks.md) laat zien hoe u hulpprogramma's gebruiken voor het uitvoeren van enkele algemene datatechnologietaken zoals interactieve gegevensverkenning, gegevensanalyse, rapportage en model maken.

Scenario's die laten zien van alle de stappen in het proces voor het **specifieke scenario's** worden ook gegeven. Ze worden weergegeven en die is gekoppeld met miniaturen beschrijvingen in de [voorbeeld walkthroughs](walkthroughs.md) artikel. Ze laten zien hoe u naar de cloud, on-premises hulpprogramma's en services combineren in een werkstroom of een pijplijn te maken van een intelligente toepassingen. 


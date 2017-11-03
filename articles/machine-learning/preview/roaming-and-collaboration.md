---
title: Roaming en samenwerking in Azure Machine Learning-Workbench | Microsoft Docs
description: Lijst met bekende problemen en een handleiding om op te lossen
services: machine-learning
author: svankam
ms.author: svankam
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/05/2017
ms.openlocfilehash: 156dd1b7f928df22b3feb9e7a13396d3b53a91d7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="roaming-and-collaboration-in-azure-machine-learning-workbench"></a>Roaming en samenwerking in Azure Machine Learning Workbench
Dit document leert u hoe u Azure Machine Learning Workbench kan helpen uw projecten roamen tussen machines, evenals inschakelen samenwerking met de teamleden. 

Wanneer u een Azure Machine Learning-project maakt met een externe koppeling voor de Git-opslagplaats (opslagplaats), worden de project-metagegevens en momentopnamen worden opgeslagen in de cloud. De cloud-koppeling kunt u toegang tot het project vanaf een andere computer (Roaming). U kunt ook toegang geven tot uw collega's, waardoor u samenwerking. 

## <a name="prerequisites"></a>Vereisten
Installeer eerst de Workbench van Azure Machine Learning met toegang tot een Account experimenteren. Ga als volgt de [installatiehandleiding](quickstart-installation.md) voor meer informatie.

Ten tweede toegang tot [Visual Studio Team System](https://www.visualstudio.com) en maak een opslagplaats voor uw project om te koppelen. Voor gedetailleerde informatie over Git verwijzen naar de [Git-opslagplaats met behulp van een Azure Machine Learning Workbench project](using-git-ml-project.md) artikel.

## <a name="create-a-new-azure-machine-learning-project"></a>Een nieuw Azure Machine Learning-project maken
Azure Machine Learning Workbench Start en een nieuw project maken (bijvoorbeeld _iris_). Vul de **Visualstudio.com GIT-opslagplaats URL** tekstvak met een geldige URL voor VSTS Git-opslagplaats. 
>[!IMPORTANT]
>Maken van het project mislukt als u hebt geen lees-/ schrijftoegang op de Git-opslagplaats en de Git-opslagplaats is niet leeg, dat wil zeggen er al een hoofdvertakking.

Zodra het project is gemaakt, indienen enkele wordt uitgevoerd op alle scripts in het project. Deze actie voert project staat in van de externe Git-opslagplaats uitvoeringsgeschiedenis vertakking. 

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

>Houd er rekening mee als u per ongeluk een map in de Azure ML-map die exact dezelfde naam als het project, de download mislukt is. Voor het gebruikt wordt, moet u de naam van de bestaande map om dit probleem omzeilen.


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

# Add Bob to the Experimentation Account as a Reader.
# Bob now has read access to all workspaces and projects under the Account by inheritance.
az role assignment create --assignee bob@contoso.com --role Reader --scope <experimentation account ARM ID>

# Find ARM ID of the workspace
az ml workspace show --query "id"

# Add Bob to the workspace as a Contributor.
# Bob now has read/write access to all projects under the Workspace by inheritance.
az role assignment create --assignee bob@contoso.com --role Contributor --scope <workspace ARM ID>

# find ARM ID of the project 
az ml project show --query "id"

# Add Bob to the Project as an Owner.
# Bob now has read/write access to the Project, and can add others too.
az role assignment create --assignee bob@contoso.com --role Owner --scope <project ARM ID>
```

Na de roltoewijzing ziet rechtstreeks of door overname Bob het project in de lijst van de Workbench-project. De toepassing moet mogelijk opnieuw opstarten om te zien van het project. Bob kan downloadt u het project zoals beschreven in de [sectie Roaming](#roaming) en samenwerken met Els. 

De uitvoeringsgeschiedenis voor alle gebruikers die samenwerken aan een project is doorgevoerd in de dezelfde externe Git-opslagplaats. Dus als Els wordt uitgevoerd een uitvoering, ziet Bob het uitvoeren in de sectie uitvoeringsgeschiedenis van het project in de Workbench-app. Bob kunt ook het project terugzetten naar de status van elke uitvoeren met inbegrip van gestart door Els wordt uitgevoerd. 

Delen van een externe Git-opslagplaats voor het project, kunt u Els en Bob ook samenwerken aan de hoofdvertakking. Indien nodig, ze kunnen ook persoonlijke vertakkingen en maken en gebruiken Git, pull-aanvragen samenvoegingen om samen te werken. 

### <a name="using-azure-portal-to-add-users"></a>Gebruikers toevoegen met behulp van Azure-portal
<a name="portal"></a>

Azure Machine Learning-experimenten Accounts, -werkruimten en projecten zijn Azure Resource Manager-resources. U kunt de Access Control-koppeling in de [Azure-portal](https://portal.azure.com) toewijzen van rollen. 

De bron die u wilt toevoegen van gebruikers om weer te geven van alle Resources vinden. Klik op het toegangsbeheer (IAM) koppeling binnen de pagina. Gebruikers toevoegen 

<img src="./media/roaming-and-collaboration/iam.png" width="320px">


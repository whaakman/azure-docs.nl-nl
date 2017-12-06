---
title: Gebruik een Git-opslagplaats met een Azure Machine Learning Workbench project | Microsoft Docs
description: Dit artikel wordt uitgelegd hoe u een Git-opslagplaats in combinatie met een Azure Machine Learning Workbench-project.
services: machine-learning
author: hning86
ms.author: haining
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/18/2017
ms.openlocfilehash: f4f1112fe68bdb2a26f68b3da08fe97f9d22d3b7
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="use-a-git-repo-with-a-machine-learning-workbench-project"></a>Gebruik een Git-opslagplaats met een Machine Learning Workbench-project
Meer informatie over hoe Azure Machine Learning Workbench Git gebruikt om de versie voor het beheer en controleer reproduceerbaarheid in uw gegevens wetenschappelijke experiment. Informatie over het koppelen van uw project met een cloud Git-opslagplaats (opslagplaats).

Machine Learning-Workbench is ontworpen voor Git-integratie. Wanneer u een nieuw project maakt, is de projectmap automatisch 'Git geïnitialiseerd' als een lokale Git-opslagplaats. Een tweede, verborgen lokale Git-opslagplaats ook is gemaakt, klikt u met een vertakking met de naam AzureMLHistory /\<GUID project\>. Houdt de vertakking van project map wordt gewijzigd voor elke uitvoering. 

Het Azure Machine Learning-project koppelen aan een Git-opslagplaats, kunt u automatische versiebeheer, lokaal en extern. De Git-opslagplaats wordt in Visual Studio Team Services (Team Services) gehost. Omdat de Machine Learning-project gekoppeld aan een Git-opslagplaats is, kan iemand die toegang tot de externe opslagplaats heeft de meest recente broncode downloaden naar een andere computer (roaming).  

> [!NOTE]
> Teamservices heeft een eigen toegangsbeheerlijst (ACL), die onafhankelijk is van de service Azure Machine Learning experimenteren. Gebruikerstoegang verschillen tussen een Git-opslagplaats en een Machine Learning-werkruimte of het project. Het is mogelijk om toegang te beheren. 
> 
> Of u wilt een teamlid geven code-niveau van toegang tot uw Machine Learning-project of deel de werkruimte, moet u de gebruiker de juiste machtigingen voor toegang tot het Team Services Git-opslagplaats te verlenen. 

Als u wilt beheren versie met Git, moet u de hoofdvertakking gebruiken of andere filialen maken in de opslagplaats. U kunt ook gebruik van de lokale Git-opslagplaats en push naar de externe Git-opslagplaats als deze ingericht.

Dit diagram ziet u de relatie tussen een Team Services Git-opslagplaats en een Machine Learning-project:

![Azure Machine Learning Git](media/using-git-ml-project/aml_git.png)

Voltooi de stappen die worden beschreven in de volgende secties om te beginnen met een externe Git-opslagplaats.

> [!NOTE]
> Op dit moment ondersteunt Azure Machine Learning-opslagplaatsen Git alleen op Team Services accounts. Ondersteuning voor algemene Git-repo's, zoals GitHub, is gepland voor de toekomst.

## <a name="step-1-create-a-machine-learning-experimentation-account"></a>Step 1. Maak een account met Machine Learning-experimenten
Een Machine Learning-experimenten-account maken en installeer de app Azure Machine Learning-Workbench. Zie voor meer informatie [installeren en maak Quick Start](quickstart-installation.md).

## <a name="step-2-create-a-team-project-or-use-an-existing-team-project"></a>Stap 2. Maken van een teamproject of een bestaande teamproject gebruiken
In de [Azure-portal](https://portal.azure.com/), een nieuw teamproject maken:
1. Selecteer  **+** .
2. Zoeken naar **Project in een Team**.
3. Geef de vereiste informatie op:
    - **Naam**: de teamnaam van een.
    - **Versiebeheer**: Selecteer **Git**.
    - **Abonnement**: Selecteer een abonnement met een Machine Learning-experimenten-account.
    - **Locatie**: Kies in het ideale geval een regio die dicht bij de resources van uw Machine Learning experimenteren.
4. Selecteer **Maken**. 

![Maak een teamproject in de Azure-portal](media/using-git-ml-project/create_vsts_team.png)

Zorg ervoor dat u zich aanmeldt met dezelfde account voor Azure Active Directory (Azure AD) waarmee u toegang hebt tot Machine Learning-Workbench. Anders wordt het systeem heeft geen toegang tot Machine Learning Workbench met behulp van uw Azure AD-referenties. Een uitzondering is als u de opdrachtregel gebruiken om de Machine Learning-project te maken, en een persoonlijk toegangstoken voor toegang tot de Git-opslagplaats op te geven. Dit bespreken we in meer detail later in dit artikel.

Als u rechtstreeks het teamproject die u hebt gemaakt, gebruikt de URL https://\<team projectnaam\>. visualstudio.com.

## <a name="step-3-set-up-a-machine-learning-project-and-git-repo"></a>Stap 3. Een Machine Learning-project en Git-opslagplaats instellen

Als u een Machine Learning-project instelt, hebt u twee opties:
- Een Machine Learning-project met een externe Git-opslagplaats maken
- Een bestaand project van Machine Learning koppelen aan een Team Services Git-opslagplaats

### <a name="create-a-machine-learning-project-that-has-a-remote-git-repo"></a>Een Machine Learning-project met een externe Git-opslagplaats maken
Machine Learning Workbench openen en een nieuw project maken. In de **Git-opslagplaats** Voer de URL van de Team Services Git-opslagplaats uit stap 2. Ziet er gewoonlijk als volgt: https://\<Team Services accountnaam\>.visualstudio.com/_git/\<projectnaam\>

![Een Machine Learning-project met een Git-opslagplaats maken](media/using-git-ml-project/create_project_with_git_rep.png)

U kunt ook het project maken met de Azure-opdrachtregelprogramma (Azure CLI). U hebt de optie voor het invoeren van een persoonlijk toegangstoken van. Machine Learning, kan dit token gebruiken voor toegang tot de Git-opslagplaats in plaats van uw Azure AD-referenties:

```
# Create a new project that has a Git repo by using a personal access token.
$ az ml project create -a <Experimentation account name> -n <project name> -g <resource group name> -w <workspace name> -r <Git repo URL> --vststoken <Team Services personal access token>
```

> [!IMPORTANT]
> Als u de sjabloon leeg project kiest, kan de Git-opslagplaats die u wilt gebruiken al een hoofdvertakking hebben. Machine Learning kloont gewoon de hoofdvertakking lokaal. Deze wordt de map aml_config en andere metagegevens projectbestanden toegevoegd aan de lokale projectmap. 
>
> Als u een andere projectsjabloon, Git-opslagplaats *kan niet* hebt u al een hoofdvertakking. Als dit het geval is, ziet u een fout opgetreden. Het alternatief is de `az ml project create` opdracht het project te maken met een `--force` overschakelen. Dit wordt verwijderd van de bestanden in de oorspronkelijke hoofdvertakking en ze vervangen door de bestanden in de sjabloon die u kiest.

Een nieuwe Machine Learning-project wordt gemaakt met externe Git-opslagplaats integratie is ingeschakeld. De projectmap is altijd Git geïnitialiseerd als een lokale Git-opslagplaats. De externe Git is ingesteld op het externe Team Services Git-opslagplaats, zodat u kunt push-doorvoeracties naar de externe Git-opslagplaats.

### <a name="associate-an-existing-machine-learning-project-with-a-team-services-git-repo"></a>Een bestaand project van Machine Learning koppelen aan een Team Services Git-opslagplaats
U kunt maken van een Machine Learning-project zonder een Team Services Git-opslagplaats en zijn afhankelijk van de lokale Git-opslagplaats voor uitvoeringsgeschiedenis momentopnamen. U kunt later een Team Services Git-opslagplaats koppelen aan dit bestaande Machine Learning-project met behulp van de volgende opdracht:

```azurecli
# Ensure that you are in the project path so Azure CLI has the context of your current project.
$ az ml project update --repo https://<Team Services account name>.visualstudio.com/_git/<project name>
```

> [!NOTE] 
> U kunt de bewerking update opslagplaats alleen op een Machine Learning-project dat u beschikt niet over een Git-opslagplaats gekoppeld uitvoeren. Ook, nadat u een Git-opslagplaats aan een Machine Learning koppelt, u niet kunt verwijderen.

## <a name="step-4-capture-a-project-snapshot-in-the-git-repo"></a>Stap 4. Een momentopname van een project in de Git-opslagplaats vastleggen
Uitvoeren van enkele script wordt uitgevoerd in het project en een aantal wijzigingen doorgevoerd tussen de wordt uitgevoerd. U kunt dit doen in de bureaublad-app of Azure CLI met behulp van de `az ml experiment submit` opdracht. Zie voor meer informatie de [Iris classificeren zelfstudie](tutorial-classifying-iris-part-1.md). Voor elke run als er een wijziging is aangebracht in een bestand in de projectmap een momentopname van de hele projectmap is toegewezen en naar de externe Git-opslagplaats onder een vertakking met de naam AzureMLHistory gepusht /\<GUID project\>. De vertakkingen en doorvoeracties, met inbegrip van de AzureMLHistory weergeven /\<project GUID\> vertakking, gaat u naar de URL van de Team Services Git-opslagplaats. 

> [!NOTE] 
> De momentopname is alleen vóór de uitvoering van een script vastgelegd. Op dit moment een prep gegevens kan worden uitgevoerd of de uitvoering van een laptop cel de momentopname niet activeren.

![vertakking van de geschiedenis uitvoeren](media/using-git-ml-project/run_history_branch.png)

> [!IMPORTANT] 
> Het is raadzaam als u niet in de geschiedenis vertakking werkt met behulp van de Git-opdrachten. Dit kan de uitvoeringsgeschiedenis verstoren. In plaats daarvan de hoofdvertakking gebruiken of andere filialen voor uw eigen Git-bewerkingen te maken.

## <a name="step-5-restore-a-previous-project-snapshot"></a>Stap 5. Herstellen van een eerdere momentopname van project 
De volledige projectmap terugzetten naar de status van een vorige uitvoeringsgeschiedenis momentopname in Machine Learning Workbench:
1. Selecteer in de activiteit balk (zandloper) **wordt uitgevoerd**.
2. In de **lijst uitvoeren** weergeven, selecteert u de uitvoering waarmee u wilt herstellen.
3. In de **uitvoeren Detail** weergave, selecteer **herstellen**.

![vertakking van de geschiedenis uitvoeren](media/using-git-ml-project/restore_project.png)

Desgewenst kunt u de volgende opdrachten in het venster Azure CLI in Machine Learning Workbench:

```azurecli
# Discover the run I want to restore a snapshot from.
$ az ml history list -o table

# Restore the snapshot from a specific run.
$ az ml project restore --run-id <run ID>
```

Wees voorzichtig wanneer u deze opdracht uitvoert. Deze opdracht wordt uitgevoerd, wordt de hele projectmap overschreven door de momentopname die is uitgevoerd wanneer dat specifieke uitvoeren is gestart. Uw project blijft in de huidige vertakking. Dit betekent dat u **gaan alle wijzigingen verloren** in de huidige projectmap.  

Mogelijk wilt Git gebruiken uw wijzigingen doorvoeren aan de huidige vertakking voordat u deze bewerking uitvoert.

## <a name="step-6-use-the-master-branch"></a>Stap 6. Gebruik de hoofdvertakking
Een manier om de projectstatus van uw huidige per ongeluk gegevensverlies te voorkomen is om door te voeren van het project naar de hoofdvertakking van de Git-opslagplaats (of de vertakking waarin u zelf hebt gemaakt). U kunt Git gebruiken vanaf de opdrachtregel of van uw favoriete Git-clienthulpprogramma bewerkingen uitvoeren op de hoofdvertakking. Bijvoorbeeld:

```sh
# Check status to make sure you are on the master branch (or branch of your choice).
$ git status

# Stage all changes.
$ git add -A

# Commit all changes locally on the master branch.
$ git commit -m 'these are my updates so far'

# Push changes to the remote Team Services Git repo master branch.
$ git push origin master
```

U kunt nu veilig het project terugzetten naar een eerdere momentopname door het voltooien van stap 5. U kunt altijd terugkeren naar het doorvoeren die u zojuist hebt gemaakt op de hoofdvertakking.

## <a name="authentication"></a>Authentication
Als u alleen op de uitvoeringsgeschiedenis functies in Machine Learning project momentopnamen en herstellen van deze te vertrouwen, moet u geen zorgen te hoeven maken over de verificatie van Git-opslagplaats. Verificatie wordt uitgevoerd door de service-laag van Machine Learning experimenteren.

Als u uw eigen Git-hulpprogramma's voor het beheren van versiebeheer gebruikt, moet u verificatie op basis van de externe Git-opslagplaats in Team Services te verwerken. In Machine Learning is externe Git-opslagplaats toegevoegd aan de lokale opslagplaats als een externe Git met behulp van het HTTPS-protocol. Dit betekent dat wanneer u Git-opdrachten (zoals push of pull) naar de externe verzendt, moet u uw gebruikersnaam en wachtwoord of een persoonlijk toegangstoken opgeven. Volg de instructies in voor het maken van een persoonlijk toegangstoken in een Team Services Git-opslagplaats, [gebruiken een persoonlijk toegangstoken om te verifiëren](https://docs.microsoft.com/vsts/accounts/use-personal-access-tokens-to-authenticate).

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over hoe [Team gegevens wetenschap proces gebruiken om het organiseren van de projectstructuur van uw](how-to-use-tdsp-in-azure-ml.md).

---
title: Implementatiecenter in Azure App Service gebruiken
description: Implementatiecenter in Azure DevOps vereenvoudigt het instellen van een robuuste Azure DevOps-pijplijn voor uw toepassing
ms.author: puagarw
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/12/2019
author: pulkitaggarwl
monikerRange: vsts
ms.openlocfilehash: 8d1e467906b74c97c8b4f4e5c14af0814dd098f7
ms.sourcegitcommit: 470041c681719df2d4ee9b81c9be6104befffcea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67854636"
---
# <a name="deployment-center-launcher"></a>Implementatiecenter starten

Implementatiecenter in Azure DevOps vereenvoudigt het instellen van een robuuste DevOps-pijplijn voor uw toepassing. Standaard configureert een DevOps-pijplijn voor de implementatie van de toepassingsupdates van uw met de kubernetes-cluster. U kunt de standaard geconfigureerde DevOps-pijplijn en toevoegen van de uitgebreidere mogelijkheden in de DevOps - goedkeuringen voor het implementeren, extra Azure-resources inrichten, uitvoeren van scripts, bijwerken van uw toepassing of extra validatietests ook uitvoeren kunt uitbreiden.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een DevOps-pijplijn voor de implementatie van uw toepassingsupdates aan het cluster k8s configureren
> * De CI-pijplijn onderzoeken
> * De CD-pijplijn onderzoeken
> * De resources opschonen

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. U krijgt een gratis tot [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/)

* Azure Kubernetes Service (AKS)-cluster

## <a name="create-aks-cluster"></a>AKS-cluster maken

1. Meld u aan bij uw [Azure-portal](https://portal.azure.com/)

1. Selecteer de [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) knop in het menu in de rechterbovenhoek van de Azure-portal.

1. Voer de volgende opdrachten voor het maken van het AKS-cluster.

    ```cmd
    # The below command creates Resource Group in the south india location

    az group create --name azooaks --location southindia

    # The below command creates a cluster named azookubectl with one node. 

    az aks create --resource-group azooaks --name azookubectl --node-count 1 --enable-addons monitoring --generate-ssh-keys
    ```

## <a name="deploy-application-updates-to-k8s-cluster"></a>Updates van toepassingen aan K8s Cluster implementeren

1. Navigeer naar de bovenstaande resourcegroep gemaakt.

1. Selecteer het AKS-cluster en klik onder instellingen voor de linkerblade op de **Implementatiecenter (preview)** . Klik op **aan de slag**.

   ![instellingen](media/deployment-center-launcher/settings.png)

1. Kies de locatie van de code en klik op **volgende**. Op dit moment de opslagplaatsen die ondersteund worden, **[Azure opslagplaatsen](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)** en **GitHub**. U kunt de volgende stappen op basis van de selectie van de opslagplaats.

    Azure-opslagplaatsen is een set hulpprogramma's besturingselement versie die u gebruiken kunt voor het beheren van uw code. Of je softwareproject groot of klein is, is met behulp van versiebeheer zo snel mogelijk een goed idee.

    - **Azure Repos**: Kies een opslagplaats van uw bestaande Microsoft project en de organisatie.

        ![Azure Repos](media/deployment-center-launcher/azure-repos.gif)

    - **GitHub**: Autoriseren en selecteert u de opslagplaats voor uw GitHub-account.

        ![GitHub](media/deployment-center-launcher/github.gif)


1. We gaan opslagplaats analyseren en detecteren van uw docker-bestand. Als u bijwerken wilt, kunt u het geïdentificeerde poortnummer bewerken.

    ![Toepassingsinstellingen](media/deployment-center-launcher/application-settings.png)

    Als de opslagplaats het bestand Dockerfile bevat wordt, wordt het systeem een bericht om door te voeren een weergegeven. 

    ![Docker-bestand](media/deployment-center-launcher/dockerfile.png)

1. Selecteer of maak een bestaande Container Registry en klik op **voltooien**. De pijplijn wordt automatisch gemaakt en een build in de wachtrij [Azure pijplijnen](https://docs.microsoft.com/azure/devops/pipelines/index?view=azure-devops).

    Azure pijplijnen is een cloudservice waarmee u kunt automatisch samenstellen en testen van uw code van uw project en beschikbaar stellen aan andere gebruikers. Azure-pijplijnen worden gecombineerd voor continue integratie (CI) en continue levering (CD) voortdurend en consistent testen en ontwikkelen van uw code en verzendt deze naar een doel.

    ![Container Registry](media/deployment-center-launcher/container-registry.png)

1. Klik op de koppeling om te zien van de lopende pijplijn.

1. U ziet de geslaagde logboeken zoals wordt weergegeven wanneer de implementatie voltooid is.

    ![Logboeken](media/deployment-center-launcher/logs.png)

## <a name="examine-the-ci-pipeline"></a>De CI-pijplijn onderzoeken

Implementatiecenter configureert van uw organisatie Azure DevOps-CI / CD-pijplijn automatisch. De pijplijn kan worden verkend en aangepast. 

1. Ga naar het dashboard van de Center-implementatie.  

1. Klik op de build-nummer in de lijst van geslaagde Logboeken om de pijplijn bouwen voor uw project weer te geven. 

1. Klik op ellipsis(...) in de rechterbovenhoek. Een menu ziet u verschillende opties, zoals een nieuwe build queuing, behoud van de build en het bewerken van de build-pijplijn. Kies de **bewerken pijplijn**. 

1. U kunt de verschillende taken controleren voor uw build-pijplijn in dit deelvenster. De build voert verschillende taken, zoals het verzamelen van bronnen van de Git-opslagplaats, het maken van een installatiekopie, een installatiekopie naar het containerregister pushen en publiceren van de uitvoer die worden gebruikt voor implementaties.

1. Selecteer de naam van de build-pijplijn aan de bovenkant van de build-pijplijn.

1. Wijzig de naam van uw build-pijplijn in iets meer beschrijvende, selecteer **opslaan en in de wachtrij**, en selecteer vervolgens **opslaan**.

1. Selecteer **geschiedenis** onder uw build-pijplijn. Dit deelvenster toont een audittrail van recente wijzigingen in uw build. Azure DevOps bewaakt alle wijzigingen aan de build-pijplijn en kunt u versies vergelijken.

1. Kies **Triggers**. Vertakkingen kunnen eventueel worden opgenomen of uitgesloten van de CI-proces.

1. Kies **retentie**. U kunt beleidsregels als u wilt behouden of verwijderen van een aantal builds afhankelijk van uw scenario.

## <a name="examine-the-cd-pipeline"></a>De CD-pijplijn onderzoeken

Implementatiecenter maakt en configureert automatisch de benodigde stappen van uw organisatie Azure DevOps voor uw Azure-abonnement. Deze stappen omvatten het instellen van een Azure-service-verbinding om te verifiëren van uw Azure-abonnement met Azure DevOps. Er wordt ook automatisch ook een release-pijplijn gemaakt, die de CD levert aan Azure.

1. Kies **pijplijnen**, en kies vervolgens **Releases**.

1. Als u wilt bewerken in de release-pijplijn, klikt u op **bewerken** .

1. Selecteer **neerzetten** van **artefacten**. In de vorige stappen produceert de bouw pijplijn die u onderzocht de uitvoer die wordt gebruikt voor het artefact. 

1. Selecteer **continue implementatie** trigger aan de rechterkant van de **neerzetten** pictogram. Deze release-pijplijn heeft een ingeschakelde CD-trigger die een implementatie wordt uitgevoerd wanneer een nieuwe build-artefact beschikbaar is. U kunt eventueel de trigger zo nodig handmatig uitvoeren voor uw implementaties uitschakelen.

1. Voor het onderzoeken van de taken voor de pijplijn, klikt u op **taken**. De release stelt u de tiller, configureert u de imagePullSecrets, installeert u Helm-hulpprogramma's en implementeert de Helm-grafieken in het cluster K8s.

1. Versies als geschiedenis wilt weergeven, klikt u op **releases weergeven**. 

1. Als u wilt zien van de samenvatting, klikt u op de **Release**. Klik op een van de fase verkennen meerdere menu's, zoals een samenvatting, release gekoppelde werkitems en tests. 

1. Selecteer **Doorvoeringen**. Deze weergave toont de code doorvoeringen met betrekking tot deze implementatie. Releases om te zien van de commit-verschillen tussen implementaties met elkaar vergelijken.

1. Selecteer **Logboeken**. De logboeken bevatten informatie over de nuttige implementatie. Tijdens en na de implementaties, kunt u ze kunt bekijken.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de gerelateerde resources die u hebt gemaakt toen u ze niet meer nodig verwijderen. Gebruik de functionaliteit voor verwijderen op het dashboard DevOps-projecten.

## <a name="next-steps"></a>Volgende stappen

U kunt de build- en release-pijplijn desgewenst wijzigen in overeenstemming met de behoeften van uw team. U kunt dit CI/CD-patroon ook als een sjabloon voor uw andere pijplijnen gebruiken. In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een DevOps-pijplijn voor de implementatie van uw toepassingsupdates aan het cluster k8s configureren
> * De CI-pijplijn onderzoeken
> * De CD-pijplijn onderzoeken
> * De resources opschonen

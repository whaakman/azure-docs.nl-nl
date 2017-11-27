---
title: Implementeer een Azure Service Fabric-toepassing met continue integratie (Team Services) | Microsoft Docs
description: Informatie over het instellen van continue integratie en implementatie voor een Service Fabric-toepassing met behulp van Visual Studio Team Services.  Een toepassing implementeert op een Service Fabric-cluster in Azure.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: acfeb5a3f27f6451309017bad88c687b408872b6
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2017
---
# <a name="deploy-an-application-with-cicd-to-a-service-fabric-cluster"></a>Implementeer een toepassing met CI/CD naar een Service Fabric-cluster
Deze zelfstudie maakt deel uit drie van een reeks en wordt beschreven hoe u voor het instellen van continue integratie en implementatie voor een Azure Service Fabric-toepassing met behulp van Visual Studio Team Services.  Een bestaande Service Fabric-toepassing is vereist, wordt de toepassing gemaakt in [een .NET-toepassing bouwen](service-fabric-tutorial-create-dotnet-app.md) wordt gebruikt als een voorbeeld.

Deel 3 van de reeks, leert u hoe:

> [!div class="checklist"]
> * Broncodebeheer toevoegen aan uw project
> * Een build-definitie maken in een Team Services
> * De definitie van een release in Team Services maken
> * Automatisch implementeren en bijwerken van een toepassing

In deze zelfstudie reeks leert u hoe:
> [!div class="checklist"]
> * [Een .NET-Service Fabric-toepassing bouwen](service-fabric-tutorial-create-dotnet-app.md)
> * [De toepassing naar een externe-cluster implementeren](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * CI/CD met behulp van Visual Studio Team Services configureren
> * [Controle en diagnostische gegevens voor de toepassing instellen](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Vereisten
Voordat u deze zelfstudie begint:
- Als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Installeer Visual Studio 2017](https://www.visualstudio.com/) en installeer de **ontwikkelen van Azure** en **ASP.NET en web ontwikkeling** werkbelastingen.
- [De Service Fabric SDK installeren](service-fabric-get-started.md)
- Maak een Service Fabric-toepassing, bijvoorbeeld door [deze zelfstudie](service-fabric-tutorial-create-dotnet-app.md). 
- Maak een Windows-Service Fabric-cluster in Azure, bijvoorbeeld door [deze zelfstudie](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
- Maak een [Team Services-account](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).

## <a name="download-the-voting-sample-application"></a>De voorbeeldtoepassing Voting downloaden
Als u hebt niet de voorbeeldtoepassing Voting in [deel uitmaken van een van deze zelfstudie reeks](service-fabric-tutorial-create-dotnet-app.md), u kunt dit downloaden. Voer de volgende opdracht voor het klonen van de opslagplaats van de voorbeeld-app op uw lokale computer in een opdrachtvenster.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="prepare-a-publish-profile"></a>Een publicatieprofiel voorbereiden
Nu dat u hebt [gemaakt van een toepassing](service-fabric-tutorial-create-dotnet-app.md) en [geïmplementeerd van de toepassing in Azure](service-fabric-tutorial-deploy-app-to-party-cluster.md), u klaar bent voor het instellen van continue integratie.  Bereid eerst een publicatieprofiel in uw toepassing voor gebruik door het implementatieproces die wordt uitgevoerd binnen Team Services.  Het publicatieprofiel dat moet worden geconfigureerd voor het doel van het cluster dat u eerder hebt gemaakt.  Visual Studio starten en opent u een bestaand project van de Service Fabric-toepassing.  In **Solution Explorer**, met de rechtermuisknop op de toepassing en selecteer **publiceren...** .

Kies een doelprofiel in uw toepassingsproject om te gebruiken voor uw werkstroom continue integratie, bijvoorbeeld Cloud.  Geef het verbindingseindpunt cluster.  Controleer de **Upgrade van de toepassing** selectievakje in zodat uw toepassing upgrades voor elke implementatie in een Team Services.  Klik op de **opslaan** hyperlink naar de instellingen in het publicatieprofiel opslaan en klik vervolgens op **annuleren** om het dialoogvenster te sluiten.  

![Push-profiel][publish-app-profile]

## <a name="share-your-visual-studio-solution-to-a-new-team-services-git-repo"></a>Delen van uw Visual Studio-oplossing een nieuw Team Services Git-opslagplaats
Delen de bronbestanden van uw toepassing in een teamproject in Team Services, zodat u builds kunt genereren.  

Een nieuwe lokale Git-opslagplaats voor uw project maken door te selecteren **toevoegen aan broncodebeheer** -> **Git** op de statusbalk in de rechterbenedenhoek van Visual Studio. 

In de **Push** weergeven in **Team Explorer**, selecteer de **Git-opslagplaats publiceren** knop onder **Push naar Visual Studio Team Services**.

![Push Git-opslagplaats][push-git-repo]

Controleer of uw e-mailadres en selecteert u uw account in de **Team Services domein** vervolgkeuzelijst. Voer de naam van uw opslagplaats en selecteer **publiceren opslagplaats**.

![Push Git-opslagplaats][publish-code]

Publiceren van de opslagplaats, maakt een nieuw teamproject in uw account met dezelfde naam als de lokale opslagplaats. Klik op om de opslagplaats in een bestaande teamproject **Geavanceerd** naast **opslagplaats** naam en selecteer een teamproject. U kunt uw code op het web bekijken door te selecteren **op het web bekijken**.

## <a name="configure-continuous-delivery-with-vsts"></a>Continue levering met VSTS configureren
De definitie van een Team Services build beschrijft een werkstroom die bestaat uit een reeks build stappen die sequentieel worden uitgevoerd. Een build-definitie maken die die produceert een Service Fabric-toepassingspakket en andere onderdelen, om te implementeren op een Service Fabric-cluster. Meer informatie over [Team Services bouwen definities](https://www.visualstudio.com/docs/build/define/create). 

De definitie van een Team Services release beschrijft een werkstroom die een toepassingspakket in een cluster implementeert. Wanneer samen worden gebruikt, worden de volledige werkstroom beginnen met bronbestanden om te eindigen op een actieve toepassing in uw cluster uitvoeren door de build-definitie en de definitie van de release. Meer informatie over Services Team [definities release](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).

### <a name="create-a-build-definition"></a>Een build-definitie maken
Open een webbrowser en navigeer naar uw nieuw teamproject op: https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting. 

Selecteer de **bouwen & Release** tabblad vervolgens **Builds**, klikt u vervolgens **+ nieuwe definitie**.  In **Selecteer een sjabloon**, selecteer de **Azure Service Fabric-toepassing** sjabloon en klikt u op **toepassen**. 

![Kies build-sjabloon][select-build-template] 

De stemtoepassing bevat een project .NET Core, dus een taak die wordt hersteld van de afhankelijkheden toevoegen. In de **taken** weergave, selecteer **+ taak toevoegen** in de onderste links. Zoeken op 'Opdrachtregel' voor de opdrachtregeltaak vinden en klik vervolgens op **toevoegen**. 

![Taak toevoegen][add-task] 

In de nieuwe taak invoeren 'Dotnet.exe uitvoeren' in **weergavenaam**, 'dotnet.exe' in **hulpprogramma**, en "herstellen" **argumenten**. 

![Nieuwe taak][new-task] 

In de **Triggers** weergeven, klikt u op de **inschakelen van deze trigger** overschakelen onder **continue integratie**. 

Selecteer **opslaan en wachtrij** en voer 'VS2017 gehost' als de **Agent wachtrij**. Selecteer **wachtrij** een build handmatig te starten.  Voortbouwt ook triggers op push of inchecken.

Als u wilt uw build-voortgang controleren, gaat u naar de **Builds** tabblad.  Zodra u hebt gecontroleerd dat de build met succes wordt uitgevoerd, definieert u de definitie van een versie die uw toepassing naar een cluster wordt geïmplementeerd. 

### <a name="create-a-release-definition"></a>Een release-definitie maken  

Selecteer de **bouwen & Release** tabblad vervolgens **Releases**, klikt u vervolgens **+ nieuwe definitie**.  In **maken release definitie**, selecteer de **Azure Service Fabric-implementatie** sjabloon uit de lijst en klikt u op **volgende**.  Selecteer de **bouwen** bron, Controleer de **continue implementatie** vak en klikt u op **maken**. 

In de **omgevingen** weergeven, klikt u op **toevoegen** rechts van **Cluster verbinding**.  Geef de verbindingsnaam van een van 'mysftestcluster', 'tcp://mysftestcluster.westus.cloudapp.azure.com:19000' van een clustereindpunt en de Azure Active Directory of de referenties van het computercertificaat voor het cluster. Voor Azure Active Directory-referenties, definieert u de referenties die u gebruiken wilt voor verbinding met het cluster in de **gebruikersnaam** en **wachtwoord** velden. Voor verificatie op basis van certificaten, definiëren de Base64-codering van het certificaatbestand in de **clientcertificaat** veld.  Zie de pop-help op het veld voor informatie over het verkrijgen van die waarde.  Als uw certificaat beveiligd met een wachtwoord is, definieert u het wachtwoord in de **wachtwoord** veld.  Klik op **opslaan** om op te slaan de release-definitie.

![Cluster-verbinding toevoegen][add-cluster-connection] 

Klik op **uitvoeren op de agent**, selecteer daarna **VS2017 gehost** voor **implementatie wachtrij**. Klik op **opslaan** om op te slaan de release-definitie.

![Voer op de agent][run-on-agent]

Selecteer **+ Release** -> **maken Release** -> **maken** handmatig maken van een release.  Controleer of de implementatie is voltooid en de toepassing wordt uitgevoerd in het cluster.  Open een webbrowser en navigeer naar [http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).  Noteer de versie van de toepassing in dit voorbeeld is '1.0.0.20170616.3'. 

## <a name="commit-and-push-changes-trigger-a-release"></a>Doorvoeren en wijzigingen forceren, een release activeren
Om te controleren of de pijplijn continue integratie werkt door te controleren in een aantal codewijzigingen in Team Services.    

Als u uw code schrijft, worden uw wijzigingen automatisch bijgehouden door Visual Studio. Wijzigingen doorvoeren aan uw lokale Git-opslagplaats door het selecteren van het pictogram wijzigingen in behandeling (![In behandeling][pending]) van de statusbalk onder in het rechterdeelvenster.

Op de **wijzigingen** weergeven in Verkenner-Team, een bericht met een beschrijving van de update toevoegen en uw wijzigingen.

![Alle doorvoeren][changes]

Selecteer het pictogram van niet-gepubliceerde wijzigingen status balk (![niet-gepubliceerde wijzigingen][unpublished-changes]) of de synchronisatie-weergave in de Explorer-Team. Selecteer **Push** bijwerken van uw code in Services/TFS Team.

![Wijzigingen][push]

De wijzigingen worden gepusht naar Team Services automatisch een build wordt geactiveerd.  Wanneer de build-definitie is voltooid, wordt een versie wordt automatisch gemaakt en begint met het upgraden van de toepassing op het cluster.

Als u wilt uw build-voortgang controleren, gaat u naar de **Builds** tabblad **Team Explorer** in Visual Studio.  Zodra u hebt gecontroleerd dat de build met succes wordt uitgevoerd, definieert u de definitie van een versie die uw toepassing naar een cluster wordt geïmplementeerd.

Controleer of de implementatie is voltooid en de toepassing wordt uitgevoerd in het cluster.  Open een webbrowser en navigeer naar [http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).  Noteer de versie van de toepassing in dit voorbeeld is '1.0.0.20170815.3'.

![Service Fabric Explorer][sfx1]

## <a name="update-the-application"></a>De toepassing bijwerken
Codewijzigingen aanbrengen in de toepassing.  Opslaan en de wijzigingen, de vorige stappen te volgen.

Zodra de upgrade van de toepassing is gestart, kunt u de voortgang van de upgrade in Service Fabric Explorer bekijken:

![Service Fabric Explorer][sfx2]

De upgrade van de toepassing kan enkele minuten duren. Wanneer de upgrade voltooid is, wordt de toepassing uitgevoerd de volgende versie.  In dit voorbeeld '1.0.0.20170815.4'.

![Service Fabric Explorer][sfx3]

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Broncodebeheer toevoegen aan uw project
> * Een build-definitie maken
> * Een release-definitie maken
> * Automatisch implementeren en bijwerken van een toepassing

Ga naar de volgende zelfstudie:
> [!div class="nextstepaction"]
> [Controle en diagnostische gegevens voor de toepassing instellen](service-fabric-tutorial-monitoring-aspnet.md) 


<!-- Image References -->
[publish-app-profile]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishAppProfile.png
[push-git-repo]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishGitRepo.png
[publish-code]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishCode.png
[select-build-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectBuildTemplate.png
[add-task]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddTask.png
[new-task]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewTask.png
[set-continuous-integration]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SetContinuousIntegration.png
[add-cluster-connection]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddClusterConnection.png
[sfx1]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX1.png
[sfx2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX2.png
[sfx3]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX3.png
[pending]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Pending.png
[changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Changes.png
[unpublished-changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/UnpublishedChanges.png
[push]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Push.png
[continuous-delivery-with-VSTS]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/VSTS-Dialog.png
[new-service-endpoint]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpoint.png
[new-service-endpoint-dialog]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpointDialog.png
[run-on-agent]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/RunOnAgent.png
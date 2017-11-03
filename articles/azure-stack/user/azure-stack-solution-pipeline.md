---
title: Uw app implementeren in Azure en Azure Stack | Microsoft Docs
description: Informatie over het implementeren van apps op Azure en Azure Stack met een hybride CI/CD-pipeline.
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/25/2017
ms.author: helaw
ms.custom: mvc
ms.openlocfilehash: 83bb401d5d65cd2c34015a1a14673363aeee81d7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-apps-to-azure-and-azure-stack"></a>Apps implementeren op Azure en Azure Stack
*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Een hybride [continue integratie](https://www.visualstudio.com/learn/what-is-continuous-integration/)/[continue levering](https://www.visualstudio.com/learn/what-is-continuous-delivery/)pijplijn (CI/CD) kunt u bouwen, testen en een app implementeren in meerdere clouds.  In deze zelfstudie maakt bouwen u een Voorbeeldomgeving voor meer informatie over hoe een hybride CI/CD-pijplijn kan u helpen:
 
> [!div class="checklist"]
> * Start een nieuwe build op basis van code doorvoeracties naar uw opslagplaats Visual Studio Team Services (VSTS).
> * Implementeer automatisch de zojuist gemaakte code naar Azure voor gebruikersacceptatietests.
> * Wanneer uw code is verstreken testen, automatisch geïmplementeerd op Azure-Stack. 


## <a name="prerequisites"></a>Vereisten
Enkele onderdelen zijn vereist voor het bouwen van een hybride CI/CD-pijplijn en kunnen even duren om voor te bereiden.  Als u al beschikt over sommige van deze onderdelen, zorg er dan voor dat ze voldoen aan de vereisten voor het begin.

In dit onderwerp wordt ervan uitgegaan dat u enige kennis van Azure en Azure-Stack hebben. Als u weten voordat u doorgaat wilt, moet beginnen met de volgende onderwerpen:

- [Inleiding tot Azure](https://docs.microsoft.com/azure/fundamentals-introduction-to-azure)
- [Belangrijkste concepten van Azure Stack](../azure-stack-key-features.md)

### <a name="azure"></a>Azure
 - Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
 - Maak een [Web-App](../../app-service/environment/app-service-web-how-to-create-a-web-app-in-an-ase.md), en configureer deze voor [FTP-publicatie](../../app-service/app-service-deploy-ftp.md).  Noteer de nieuwe Web-App-URL, zoals het wordt later gebruikt.


### <a name="azure-stack"></a>Azure Stack
 - [Azure Stack implementeren](../azure-stack-run-powershell-script.md).  De installatie duurt normaal gesproken een paar uur moet zijn voltooid, dus plan dienovereenkomstig.
 - Implementeer [App Service](../azure-stack-app-service-deploy.md) PaaS-services naar Azure-Stack.
 - Een Web-App maken en configureren voor [FTP-publicatie](../azure-stack-app-service-enable-ftp.md).  Noteer de nieuwe Web-App-URL, zoals het wordt later gebruikt.  

### <a name="developer-tools"></a>Hulpprogramma's voor ontwikkelaars
 - Maak een [VSTS werkruimte](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).  Het aanmeldingsproces maakt een project met de naam 'MyFirstProject'.  
 - [Installeer Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) en [aanmelden bij VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services#connect-and-share-code-from-visual-studio)
 - Verbinding maken met het project en [klonen lokaal](https://www.visualstudio.com/docs/git/gitquickstart).
 - Maak een [agent groep](https://www.visualstudio.com/docs/build/concepts/agents/pools-queues#creating-agent-pools-and-queues) in VSTS.
 - Visual Studio installeren en implementeren van een [VSTS bouwen agent](https://www.visualstudio.com/docs/build/actions/agents/v2-windows) aan een virtuele machine op Azure-Stack. 
 

## <a name="create-app--push-to-vsts"></a>Push naar VSTS & app maken

### <a name="create-application"></a>-Toepassing maken
In deze sectie maakt u een eenvoudige ASP.NET-toepassing maken en dit doorgeven aan VSTS.  Deze stappen vertegenwoordigen de normale developer-werkstroom en kunnen worden aangepast voor hulpprogramma's voor ontwikkelaars en talen. 

1.  Open Visual Studio.
2.  Vanuit de ruimte Team Explorer en **oplossingen...**  gebied, klikt u op **nieuw**.
3.  Selecteer **Visual C#** > **Web** > **ASP.NET-webtoepassing (.NET Framework)**.
4.  Geef een naam voor de toepassing en klik op **OK**.
5.  Behoud de standaardinstellingen (webformulieren) op het volgende scherm en klik op **OK**.

### <a name="commit-and-push-changes-to-vsts"></a>Doorvoeren en wijzigingen naar VSTS forceren
1.  Team Explorer Visual Studio, selecteer de vervolgkeuzelijst en klikt u op **wijzigingen**.
2.  Geef een commit-bericht en selecteer **doorvoeren alle**. U wordt mogelijk gevraagd het oplossingsbestand opslaan, klikt u op Ja om op te slaan alle.
3.  Zodra doorgevoerd, biedt Visual Studio synchronisatie van wijzigingen aan uw project. Selecteer **Sync**.

    ![afbeelding van het scherm doorvoeren nadat de commit is voltooid](./media/azure-stack-solution-pipeline/image1.png)

4.  In het tabblad Synchronisatie onder *uitgaande*, ziet u uw nieuwe doorvoeren.  Selecteer **Push** de wijziging door te VSTS synchroniseren.

    ![afbeelding die laat zien sync stappen](./media/azure-stack-solution-pipeline/image2.png)

### <a name="review-code-in-vsts"></a>De code in VSTS bekijken
Eenmaal een wijziging hebt vastgelegd en gepusht naar VSTS, Controleer uw code vanuit de portal VSTS.  Selecteer **Code**, en vervolgens **bestanden** in de vervolgkeuzelijst.  Hier ziet u de oplossing die u hebt gemaakt.

## <a name="create-build-definition"></a>Build-definitie maken
Het buildproces definieert hoe uw toepassing is gebouwd en verpakt voor een implementatie op elke doorvoer van codewijzigingen. In ons voorbeeld gebruiken we de sjablonen voor het configureren van de procedure voor het maken van een ASP.NET-app Hoewel deze configuratie aangepast afhankelijk van uw toepassing worden kan.

1.  Meld u aan uw werkruimte VSTS vanuit een webbrowser.
2.  Selecteer in de banner **bouwen & Release** en vervolgens **Builds**.
3.  Klik op **+ nieuwe definitie**.
4.  Selecteer in de lijst met sjablonen **ASP.NET (Preview)** en selecteer **toepassen**.
5.  Wijzig de *MSBuild-argumenten* veld in *Build Solution* stap naar:

    `/p:DeployOnBuild=True /p:WebPublishMethod=FileSystem /p:DeployDefaultTarget=WebPublish /p:publishUrl="$(build.artifactstagingdirectory)\\"`

6.  Selecteer de **opties** tabblad en selecteert u de agent-wachtrij voor de build-agent die u hebt geïmplementeerd naar een virtuele machine op Azure-Stack. 
7.  Selecteer de **Triggers** tabblad en in te schakelen **continue integratie**.
7.  Klik op **opslaan en wachtrij** en selecteer vervolgens **opslaan** uit de vervolgkeuzelijst. 

## <a name="create-release-definition"></a>Release-definitie maken
Het proces release definieert hoe builds van de vorige stap zijn geïmplementeerd in een omgeving.  In deze zelfstudie publiceren we onze ASP.NET-app met FTP naar een Azure-Web-App. Gebruik de volgende stappen voor het configureren van een Azure-versie:

1.  Selecteer in de banner VSTS **bouwen & Release** en vervolgens **Releases**.
2.  Klik op de groene **+ nieuwe definitie**.
3.  Selecteer **leeg** en klik op **volgende**.
4.  Schakel het selectievakje voor *continue implementatie*, en klik vervolgens op **maken**.

Nu dat u hebt een definitie van een leeg release hebt gemaakt en deze gekoppeld aan de build, voegen we stappen voor de Azure-omgeving:

1.  Klik op de groene  **+**  taken toevoegen.
2.  Selecteer **alle**, en voeg vervolgens uit de lijst **FTP-uploaden** en selecteer **sluiten**.
3.  Selecteer de **FTP-uploaden** taak u zojuist hebt toegevoegd en configureer de volgende parameters:
    
    | Parameter | Waarde |
    | ----- | ----- |
    |Verificatiemethode| Referenties invoeren|
    |Server-URL | FTP-URL van Web-App opgehaald uit Azure-portal |
    |Gebruikersnaam | Gebruikersnaam die u hebt geconfigureerd bij het maken van de FTP-referenties voor Web-App |
    |Wachtwoord | Wachtwoord dat u hebt gemaakt bij het maken van FTP-referenties voor Web-App|
    |Bronmap | $(System.DefaultWorkingDirectory)\**\ |
    |Externe map | KCC/site/wwwroot / |
    |Bestandspaden behouden | Ingeschakeld (ingeschakeld)|

4.  Klik op **opslaan**

Ten slotte kunt u de definitie van de release voor het gebruik van de agent van toepassingen met de agent geïmplementeerd met behulp van de volgende stappen configureren:
1.  Selecteer de definitie van de release en klik op **bewerken**.
2.  Selecteer **op agent uitgevoerd** van de middelste kolom.  Selecteer in de rechterkolom de agent wachtrij met de build-agent uitgevoerd op Azure-Stack.  
    ![afbeelding weergeeft configuratie van release definitie aan specifieke wachtrij gebruiken](./media/azure-stack-solution-pipeline/image3.png)


## <a name="deploy-your-app-to-azure"></a>Uw app implementeren in Azure
Deze stap maakt gebruik van uw nieuw gebouwde CI/CD-pijplijn de ASP.NET-app implementeren in een Web-App in Azure. 

1.  Selecteer in de banner in VSTS **bouwen & Release**, en selecteer vervolgens **Builds**.
2.  Klik op **...**  op de definitie van de build eerder gemaakte en selecteer **wachtrij nieuwe build**.
3.  Accepteer de standaardinstellingen en klik op **Ok**.  De build begint en voortgang wordt weergegeven.
4.  Zodra de build voltooid is, kunt u de status volgen door te selecteren **bouwen & Release** en het selecteren van **Releases**.
5.  Nadat de build voltooid is, gaat u naar de website met de URL die wordt vermeld bij het maken van de Web-App.    


## <a name="add-azure-stack-to-pipeline"></a>Azure-Stack voor pipeline-toevoegen
Nu dat u uw CI/CD-pijplijn getest hebt door te implementeren in Azure, is het tijd om Azure-Stack toevoegen aan de pijplijn.  In de volgende stappen maakt een nieuwe omgeving en een taak FTP-uploaden om uw app implementeren in Azure Stack toevoegen.  U ook toevoegen een goedkeurder release, die als een manier om te simuleren fungeert 'aan te melden uit' een release code naar Azure Stack.  

1.  Selecteer in de definitie Release **+ omgeving toevoegen** en **maken nieuwe omgeving**.
2.  Selecteer **leeg**, klikt u op **volgende**.
3.  Selecteer **specifieke gebruikers** en uw account opgeven.  Selecteer **Maken**.
4.  Wijzig de naam van de omgeving door de naam van het bestaande selecteren en te typen *Azure Stack*.
5.  Nu de selectie van de Azure-Stack-omgeving, vervolgens selecteert **taken toevoegen**.
6.  Selecteer de **FTP-uploaden** taak en selecteer **toevoegen**, selecteer daarna **sluiten**.


### <a name="configure-ftp-task"></a>Configureren van FTP-taak
Nu dat u een release hebt gemaakt, configureert u de stappen die nodig zijn voor publicatie naar de Web-App op Azure-Stack.  Net zoals u de taak voor het uploaden van de FTP-geconfigureerd voor Azure, kunt u de taak configureren voor Azure-Stack:

1.  Selecteer de **FTP-uploaden** taak u zojuist hebt toegevoegd en configureer de volgende parameters:
    
    | Parameter | Waarde |
    | -----     | ----- |
    |Verificatiemethode| Referenties invoeren|
    |Server-URL | FTP-URL van Web-App opgehaald uit de Stack van de Azure portal |
    |Gebruikersnaam | Gebruikersnaam die u hebt geconfigureerd bij het maken van de FTP-referenties voor Web-App |
    |Wachtwoord | Wachtwoord dat u hebt gemaakt bij het maken van FTP-referenties voor Web-App|
    |Bronmap | $(System.DefaultWorkingDirectory)\**\ |
    |Externe map | KCC/site/wwwroot /|
    |Bestandspaden behouden | Ingeschakeld (ingeschakeld)|

2.  Klik op **opslaan**

Configureer ten slotte de release-definitie voor het gebruik van de agent van toepassingen met de agent geïmplementeerd met behulp van de volgende stappen uit:
1.  Selecteer de definitie van de release en klik op **bewerken**
2.  Selecteer **op agent uitgevoerd** van de middelste kolom. Selecteer in de rechterkolom de agent wachtrij met de build-agent uitgevoerd op Azure-Stack.  
    ![afbeelding weergeeft configuratie van release definitie aan specifieke wachtrij gebruiken](./media/azure-stack-solution-pipeline/image3.png)

## <a name="deploy-new-code"></a>Implementeer nieuwe code
U kunt nu de hybride CI/CD-pijplijn testen met de laatste stap publiceren naar Azure-Stack.  In deze sectie wijzigen van de site-voettekst en start de implementatie via de pipeline.  Als u klaar is, ziet u de wijzigingen die zijn geïmplementeerd in Azure voor controle en klik vervolgens als u tevreden bent met de release worden gepubliceerd naar Azure-Stack.

1. Open in Visual Studio de *site.master* bestands- en wijzigt u deze regel:
    
    `
        <p>&copy; <%: DateTime.Now.Year %> - My ASP.NET Application</p>
    `

    in deze:

    `
        <p>&copy; <%: DateTime.Now.Year %> - My ASP.NET Application delivered by VSTS, Azure, and Azure Stack</p>
    `
3.  De wijzigingen worden doorgevoerd en synchroniseren met VSTS.  
4.  Vanuit de werkruimte VSTS kunt u de build-status controleren door het selecteren van **bouwen & Release** > **bouwen**
5.  U ziet een build uitgevoerd.  Dubbelklik op de status en u kunt de voortgang van de build bekijken.  Eenmaal 'Klaar build' u in de console ziet, gaat u verder met het controleren van de release van **bouwen & Release** > **Release**.  Dubbelklik op de release.
6.  U ontvangt bericht dat een release moet worden gecontroleerd. De Web-App-URL en controleer of dat de nieuwe wijzigingen aanwezig zijn.  De release in VSTS goedkeuren.
    ![afbeelding weergeeft configuratie van release definitie aan specifieke wachtrij gebruiken](./media/azure-stack-solution-pipeline/image4.png)
    
7.  Controleer of de publicatie naar Azure-Stack is voltooid door de website met de URL die wordt vermeld bij het maken van de Web-App te bezoeken.
    ![afbeelding van ASP. NEt-app voor de voettekst gewijzigd](./media/azure-stack-solution-pipeline/image5.png)


U kunt nu uw nieuwe hybride CI/CD-pijplijn gebruiken als een bouwsteen voor andere hybride cloud-patronen.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u een hybride bouwen CI/CD pipeline die:

> [!div class="checklist"]
> * Initieert een nieuwe build op basis van code doorvoert naar uw opslagplaats Visual Studio Team Services (VSTS).
> * Uw nieuw gebouwde code implementeert naar Azure automatisch voor gebruikersacceptatietests.
> * Wanneer uw code is verstreken testen, automatisch wordt geïmplementeerd op Azure-Stack. 

Nu dat u een hybride CI/CD-pijplijn hebt, gaat u door het leren om apps te ontwikkelen voor Azure-Stack.

> [!div class="nextstepaction"]
> [Ontwikkelen voor Azure Stack](azure-stack-developer.md)



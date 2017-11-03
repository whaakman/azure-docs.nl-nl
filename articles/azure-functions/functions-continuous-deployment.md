---
title: Continue implementatie voor Azure Functions | Microsoft Docs
description: Doorlopende implementatie faciliteiten van Azure App Service gebruiken voor het publiceren van uw Azure-functies.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: 361daf37-598c-4703-8d78-c77dbef91643
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/25/2016
ms.author: glenga
ms.openlocfilehash: 35a0b0faa61cf4b42ba1d8696c85f5724ff73f23
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="continuous-deployment-for-azure-functions"></a>Doorlopende implementatie voor Azure Functions
Azure Functions kunt eenvoudig uw functie-app met continue integratie van App Service implementeren. Functions integreert met BitBucket, Dropbox, GitHub en Visual Studio Team Services (VSTS). Hierdoor wordt een werkstroom waar functiecode updates uitgevoerd met behulp van een van deze implementatie van de trigger geïntegreerde services naar Azure. Als u niet bekend met Azure Functions bent, beginnen met een [overzicht van Azure Functions](functions-overview.md).

Continue implementatie is een goede optie voor projecten waar meerdere en regelmatige bijdragen worden geïntegreerd. U kunt ook broncodebeheer op uw code functies onderhouden. De volgende bronnen voor de implementatie worden momenteel ondersteund:

* [Bitbucket](https://bitbucket.org/)
* [Dropbox](https://www.dropbox.com/)
* Externe opslagplaats (Git of volgt)
* [Lokale GIT-opslagplaats](../app-service/app-service-deploy-local-git.md)
* [GitHub](https://github.com)
* [OneDrive](https://onedrive.live.com/)
* [Visual Studio Team Services](https://www.visualstudio.com/team-services/)

Implementaties worden geconfigureerd op basis van de app per functie. Nadat de doorlopende implementatie is ingeschakeld, toegang tot de functiecode in de portal is ingesteld op *alleen-lezen*.

## <a name="continuous-deployment-requirements"></a>Vereisten voor de doorlopende implementatie

In de implementatiebron voordat u continue implementatie instelt, moet u uw implementatiebron geconfigureerd en uw code functies hebben. Elke functie woont in een bepaalde functie app-implementatie in een benoemde submap, waarbij de mapnaam de naam van de functie is.  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

## <a name="set-up-continuous-deployment"></a>Doorlopende implementatie instellen
Gebruik deze procedure voor het configureren van continue implementatie voor een bestaande functie-app. Deze stappen laten zien integratie met een GitHub-opslagplaats, maar gelijksoortige stappen gelden voor Visual Studio Team Services of andere implementatieservices.

1. In de functie-app in de [Azure-portal](https://portal.azure.com), klikt u op **platformfuncties** en **implementatieopties**. 
   
    ![Doorlopende implementatie instellen](./media/functions-continuous-deployment/setup-deployment.png)
 
2. Klik in de **implementaties** Klik op de blade **Setup**.
 
    ![Doorlopende implementatie instellen](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. In de **implementatiebron** blade, klikt u op **bron kiezen**, vul vervolgens de gegevens voor uw gekozen implementatiebron en klikt u op **OK**.
   
    ![Implementatiebron kiezen](./media/functions-continuous-deployment/choose-deployment-source.png)

Nadat doorlopende implementatie is geconfigureerd, alle bestandswijzigingen in de implementatiebron-worden gekopieerd naar de functie-app en een volledige site-implementatie wordt geactiveerd. De site is opnieuw geïmplementeerd wanneer bestanden in de bron worden bijgewerkt.

## <a name="deployment-options"></a>Implementatieopties

Hier volgen enkele typische implementatiescenario's:

- [Een gefaseerde installatie implementatie maken](#staging)
- [Verplaatsen van bestaande functies voor continue implementatie](#existing)

<a name="staging"></a>
### <a name="create-a-staging-deployment"></a>Een gefaseerde installatie implementatie maken

Functie Apps ondersteunt nog geen implementatiesites. U kunt echter nog steeds afzonderlijke fasering en productie-implementaties beheren met behulp van continue integratie.

Het proces voor het configureren en werken met een gefaseerde installatie implementatie ziet er meestal als volgt:

1. Maak twee functie apps in uw abonnement, één voor de productiecode en één voor fasering. 

2. Maak een implementatiebron als u er nog geen hebt. In dit voorbeeld wordt [GitHub].

3. Voor uw app van de functie productie voltooid de stappen **continue implementatie instellen** en stel de vertakking implementatie naar de hoofdvertakking van de GitHub-opslagplaats.
   
    ![Vertakking implementatie kiezen](./media/functions-continuous-deployment/choose-deployment-branch.png)

4. Herhaal deze stap voor fasering functie-app, maar de fasering vertakking in plaats daarvan kiezen in uw GitHub-opslagplaats. Als de implementatiebron-biedt geen ondersteuning voor vertakking, gebruikt u een andere map.
    
5. Updates aanbrengen in uw code in de tijdelijke map of vertakking, controleer dan vervolgens dat deze wijzigingen zijn doorgevoerd in de staging-implementatie.

6. Na het testen verandert het samenvoegen van de staging vertakking naar de hoofdvertakking. Deze samenvoegen activeert implementatie naar de productie-app voor de functie. Als de implementatiebron-biedt geen ondersteuning voor filialen, overschrijven de bestanden in de productie-map met de bestanden van de map.

<a name="existing"></a>
### <a name="move-existing-functions-to-continuous-deployment"></a>Verplaatsen van bestaande functies voor continue implementatie
Wanneer u hebt bestaande functies die u hebt gemaakt en onderhouden in de portal, moet u uw bestaande functie codebestanden met FTP downloaden of het lokale Git-opslagplaats voordat u continue implementatie instellen kunt zoals hierboven is beschreven. U kunt dit doen in de App Service-instellingen voor de functie-app. Nadat de bestanden zijn gedownload, kunt u ze kunt uploaden naar uw bron gekozen continue implementatie.

> [!NOTE]
> Nadat u continue integratie hebt geconfigureerd, kunt u zich niet langer bewerken van de bronbestanden in de portal functies.

- [Hoe: Configureer de referenties voor implementatie](#credentials)
- [Hoe: downloaden van bestanden met FTP](#downftp)
- [Hoe: downloaden van bestanden met behulp van de lokale Git-opslagplaats](#downgit)

<a name="credentials"></a>
#### <a name="how-to-configure-deployment-credentials"></a>Hoe: Configureer de referenties voor implementatie
Voordat u bestanden van de functie-app met FTP- of lokale Git-opslagplaats downloaden kunt, moet u uw referenties voor toegang tot de site configureren. Referenties worden ingesteld op het niveau van de functie-app. Gebruik de volgende stappen uit als implementatiereferenties wilt instellen in de Azure portal:

1. In de functie-app in de [Azure-portal](https://portal.azure.com), klikt u op **platformfuncties** en **implementatiereferenties**.
   
    ![Lokale implementatiereferenties instellen](./media/functions-continuous-deployment/setup-deployment-credentials.png)

2. Typ een gebruikersnaam en wachtwoord en klik vervolgens op **opslaan**. U kunt deze referenties nu gebruiken voor toegang tot uw app in de functie van FTP of de ingebouwde Git-opslagplaats.

<a name="downftp"></a>
#### <a name="how-to-download-files-using-ftp"></a>Hoe: downloaden van bestanden met FTP

1. In de functie-app in de [Azure-portal](https://portal.azure.com), klikt u op **platformfuncties** en **eigenschappen**, kopieert u de waarden voor **FTP-/ Implementatiegebruiker gebruiker**, **FTP-hostnaam**, en **FTPS hostnaam**.  

    **FTP-/ Implementatiegebruiker gebruiker** moet worden ingevoerd zoals weergegeven in de portal, met inbegrip van de naam van de app, zodat de juiste context voor de FTP-server.
   
    ![Uw implementatie-informatie ophalen](./media/functions-continuous-deployment/get-deployment-credentials.png)

2. Gebruik de gegevens van uw FTP-client. u hebt verzameld voor het verbinding maken met uw app en download de bronbestanden voor uw functies.

<a name="downgit"></a>
#### <a name="how-to-download-files-using-a-local-git-repository"></a>Hoe: downloaden van bestanden met behulp van een lokale Git-opslagplaats

1. In de functie-app in de [Azure-portal](https://portal.azure.com), klikt u op **platformfuncties** en **implementatieopties**. 
   
    ![Doorlopende implementatie instellen](./media/functions-continuous-deployment/setup-deployment.png)
 
2. Klik in de **implementaties** Klik op de blade **Setup**.
 
    ![Doorlopende implementatie instellen](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. In de **implementatiebron** blade, klikt u op **lokale Git-opslagplaats** en klik vervolgens op **OK**.

3. In **platformfuncties**, klikt u op **eigenschappen** en noteer de waarde van de Git-URL. 
   
    ![Doorlopende implementatie instellen](./media/functions-continuous-deployment/get-local-git-deployment-url.png)

4. Kloon de opslagplaats op uw lokale computer met een Git-bewuste opdrachtprompt of uw favoriete Git-hulpprogramma. De Git-kloon-opdracht ziet er als volgt:
   
        git clone https://username@my-function-app.scm.azurewebsites.net:443/my-function-app.git

5. Bestanden ophalen van de functie-app op de kloon op uw lokale computer, zoals in het volgende voorbeeld:
   
        git pull origin master
   
    Als u hebt aangevraagd, geeft u uw [implementatiereferenties geconfigureerd](#credentials).  

[GitHub]: https://github.com/

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aanbevolen procedures voor Azure Functions](functions-best-practices.md)

---
title: Continue implementatie voor Azure Functions | Microsoft Docs
description: Continue implementatie faciliteiten van Azure App Service gebruiken voor het publiceren van uw Azure-functies.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: 361daf37-598c-4703-8d78-c77dbef91643
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/25/2016
ms.author: glenga
ms.openlocfilehash: 7529d20535eedab92d164df5a0435efeda83fca2
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44301545"
---
# <a name="continuous-deployment-for-azure-functions"></a>Doorlopende implementatie voor Azure Functions
Azure Functions kunt eenvoudig uw functie-app met behulp van continue integratie van App Service implementeren. Functions integreert met BitBucket, Dropbox, GitHub en DevOps met Azure. Hierdoor wordt een werkstroom waar functiecode updates die worden gemaakt met behulp van een van deze implementatie van de trigger geïntegreerde services naar Azure. Als u niet bekend bent met Azure Functions, begint u met [overzicht van Azure Functions](functions-overview.md).

Continue implementatie is een goede optie voor projecten waar meerdere en regelmatige bijdragen worden geïntegreerd. Ook kunt u de bron-controle voor uw functions-code behoudt. De volgende implementatiebronnen wordt momenteel ondersteund:

* [Bitbucket](https://bitbucket.org/)
* [Dropbox](https://www.dropbox.com/)
* Externe opslagplaats (Git- of Mercurial)
* [Lokale GIT-opslagplaats](../app-service/app-service-deploy-local-git.md)
* [GitHub](https://github.com)
* [OneDrive](https://onedrive.live.com/)
* [Azure DevOps-Services](https://www.visualstudio.com/team-services/)

Implementaties worden geconfigureerd op basis van per functie-app. Nadat u continue implementatie is ingeschakeld, toegang tot de functiecode in de portal is ingesteld op *alleen-lezen*.

## <a name="continuous-deployment-requirements"></a>Vereisten voor de continue implementatie

In de implementatiebron van de voordat u continue implementatie instellen, moet u de implementatiebron geconfigureerd en uw functions-code hebben. In een bepaalde functie-app-implementatie, is elke functie aanwezig in een subdirectory, waar de mapnaam de naam van de functie is.  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Als u wilt implementeren vanuit Azure DevOps, moet u eerst uw Azure DevOps-organisatie koppelen aan uw Azure-abonnement. Zie voor meer informatie, [de facturering voor uw organisatie Azure DevOps instellen](https://docs.microsoft.com/azure/devops/organizations/billing/set-up-billing-for-your-organization-vs?view=vsts#set-up-billing-via-the-azure-portal).

## <a name="set-up-continuous-deployment"></a>Doorlopende implementatie instellen
Gebruik deze procedure om continue implementatie voor een bestaande functie-app te configureren. Deze stappen laten zien dat integratie met een GitHub-opslagplaats, maar gelijksoortige stappen gelden voor Azure DevOps- of andere implementatieservices.

1. In uw functie-app in de [Azure-portal](https://portal.azure.com), klikt u op **platformfuncties** en **implementatieopties**. 
   
    ![Continue implementatie instellen](./media/functions-continuous-deployment/setup-deployment.png)
 
2. Klik in de **implementaties** blade op **Setup**.
 
    ![Continue implementatie instellen](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. In de **implementatiebron** blade, klikt u op **bron kiezen**, en vul vervolgens de gegevens in voor uw gekozen implementatiebron en klikt u op **OK**.
   
    ![Implementatiebron kiezen](./media/functions-continuous-deployment/choose-deployment-source.png)

Nadat u continue implementatie is geconfigureerd, alle bestandswijzigingen in de implementatiebron worden gekopieerd naar de functie-app en de implementatie van een volledige site wordt geactiveerd. De site wordt opnieuw geïmplementeerd wanneer bestanden in de bron worden bijgewerkt.

## <a name="deployment-options"></a>Implementatieopties

Hier volgen enkele typische implementatiescenario's:

- [Een staging-implementatie maken](#staging)
- [Verplaatsen van bestaande functies doorlopende implementatie](#existing)

<a name="staging"></a>
### <a name="create-a-staging-deployment"></a>Een staging-implementatie maken

Functie-Apps ondersteunt geen implementatiesleuven nog. U kunt echter nog steeds afzonderlijke fasering en productie-implementaties beheren met behulp van continue integratie.

Het proces voor het configureren en werken met een staging-implementatie ziet er in het algemeen als volgt:

1. Maak twee functie-apps in uw abonnement, één voor de productiecode en één voor fasering. 

2. Maak een implementatiebron, als u er nog geen hebt. In dit voorbeeld wordt [GitHub].

3. Voor de productie-functie-app voltooid de voorgaande stappen in **continue implementatie instellen** en stel in de vertakking van de implementatie op de master-vertakking van uw GitHub-opslagplaats.
   
    ![Implementatie-vertakking kiezen](./media/functions-continuous-deployment/choose-deployment-branch.png)

4. Herhaal deze stap voor de staging-functie-app, maar in plaats daarvan de staging-vertakking in uw GitHub-opslagplaats kiezen. Als de implementatiebron biedt geen ondersteuning voor vertakkingen, gebruikt u een andere map.
    
5. Updates aanbrengen in uw code in de tijdelijke map of vertakking, en vervolgens controleren of dat deze wijzigingen worden doorgevoerd in de staging-implementatie.

6. Na het testen verandert de samenvoeging van de staging-vertakking in de master-vertakking. Deze samenvoeging wordt geactiveerd voor implementatie naar de productie-functie-app. Als de implementatiebron biedt geen ondersteuning voor filialen, overschrijft u de bestanden in de productie-map met de bestanden uit de map met tijdelijke bestanden.

<a name="existing"></a>
### <a name="move-existing-functions-to-continuous-deployment"></a>Verplaatsen van bestaande functies doorlopende implementatie
Wanneer u hebt bestaande functies die u hebt gemaakt en beheerd in de portal, moet u uw bestaande functie codebestanden via FTP downloaden of de lokale Git-opslagplaats voordat u van continue implementatie instellen kunt, zoals hierboven beschreven. U kunt dit doen in de App Service-instellingen voor uw functie-app. Nadat de bestanden zijn gedownload, kunt u ze kunt uploaden naar uw bron gekozen continue implementatie.

> [!NOTE]
> Nadat u continue integratie configureren, kunt u worden niet meer bewerken van de bronbestanden in de Functions-portal.

- [Hoe: referenties voor implementatie configureren](#credentials)
- [Hoe: downloaden van bestanden via FTP](#downftp)
- [Hoe: downloaden van bestanden met behulp van de lokale Git-opslagplaats](#downgit)

<a name="credentials"></a>
#### <a name="how-to-configure-deployment-credentials"></a>Hoe: referenties voor implementatie configureren
Voordat u bestanden van uw functie-app met FTP- of lokale Git-opslagplaats downloaden kunt, moet u uw referenties in om toegang tot de site configureren. Referenties zijn ingesteld op het niveau van de functie-app. Gebruik de volgende stappen uit om in te stellen van referenties voor implementatie in Azure portal:

1. In uw functie-app in de [Azure-portal](https://portal.azure.com), klikt u op **platformfuncties** en **implementatiereferenties**.
   
    ![Referenties voor lokale implementatie instellen](./media/functions-continuous-deployment/setup-deployment-credentials.png)

2. Typ een gebruikersnaam en wachtwoord in en klik vervolgens op **opslaan**. U kunt nu deze referenties gebruiken voor toegang tot uw functie-app van FTP of de ingebouwde Git-opslagplaats.

<a name="downftp"></a>
#### <a name="how-to-download-files-using-ftp"></a>Hoe: downloaden van bestanden via FTP

1. In uw functie-app in de [Azure-portal](https://portal.azure.com), klikt u op **platformfuncties** en **eigenschappen**, kopieer vervolgens de waarden voor **FTP-/ Implementatiegebruiker**, **FTP-hostnaam**, en **FTPS-hostnaam**.  

    **FTP-/ Implementatiegebruiker** moet worden ingevoerd zoals weergegeven in de portal, met inbegrip van de app-naam voor de juiste context voor de FTP-server.
   
    ![Uw implementatie-informatie ophalen](./media/functions-continuous-deployment/get-deployment-credentials.png)

2. Gebruik de verbindingsgegevens van de FTP-client u verzameld om te verbinden met uw app en download de bronbestanden voor uw functies.

<a name="downgit"></a>
#### <a name="how-to-download-files-using-a-local-git-repository"></a>Hoe: downloaden van bestanden met een lokale gitopslagplaats

1. In uw functie-app in de [Azure-portal](https://portal.azure.com), klikt u op **platformfuncties** en **implementatieopties**. 
   
    ![Continue implementatie instellen](./media/functions-continuous-deployment/setup-deployment.png)
 
2. Klik in de **implementaties** blade op **Setup**.
 
    ![Continue implementatie instellen](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. In de **implementatiebron** blade, klikt u op **lokale Git-opslagplaats** en klik vervolgens op **OK**.

3. In **platformfuncties**, klikt u op **eigenschappen** en noteer de waarde van de Git-URL. 
   
    ![Continue implementatie instellen](./media/functions-continuous-deployment/get-local-git-deployment-url.png)

4. Kloon de opslagplaats op uw lokale computer met een Git-bewuste opdrachtprompt of uw favoriete Git-toepassing. De Git-kloon-opdracht ziet er als volgt:
   
        git clone https://username@my-function-app.scm.azurewebsites.net:443/my-function-app.git

5. Bestanden ophalen uit uw functie-app naar de kloon op uw lokale computer, zoals in het volgende voorbeeld:
   
        git pull origin master
   
    Als dat wordt gevraagd, Geef uw [implementatiereferenties geconfigureerd](#credentials).  

[GitHub]: https://github.com/

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aanbevolen procedures voor Azure Functions](functions-best-practices.md)

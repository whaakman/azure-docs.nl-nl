---
title: Continue implementatie voor Azure Functions | Microsoft Docs
description: De faciliteiten continue implementatie van Azure App Service gebruiken voor het publiceren van uw functies.
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
ms.openlocfilehash: cb3f3ad3bb7b42429654ea4bf9b49f7e230db1da
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64943885"
---
# <a name="continuous-deployment-for-azure-functions"></a>Doorlopende implementatie voor Azure Functions
Azure Functions kunt eenvoudig uw functie-app implementeren met behulp van continue integratie. Functions integreert met primaire opslagplaatsen voor codes en van implementatiebronnen. Deze integratie kunt een werkstroom waar functiecode updates aangebracht via een van deze implementatie van de trigger-services in Azure. Als u geen ervaring met Azure Functions, begint u met de [overzicht van Azure Functions](functions-overview.md).

Continue implementatie is een goede optie voor projecten waar u een integratie uitvoert meerdere en regelmatige bijdragen. Ook kunt u het onderhouden van broncodebeheer in uw functiecode aan te geven. Azure Functions ondersteunt de volgende implementatiebronnen:

* [Bitbucket](https://bitbucket.org/)
* [Dropbox](https://www.dropbox.com/)
* Externe opslagplaats (Git- of Mercurial)
* [Lokale GIT-opslagplaats](../app-service/deploy-local-git.md)
* [GitHub](https://github.com)
* [OneDrive](https://onedrive.live.com/)
* [Azure DevOps](https://azure.microsoft.com/services/devops/)

Implementaties worden geconfigureerd op basis van per functie-app. Nadat u continue implementatie is ingeschakeld, toegang tot de functiecode in de portal is ingesteld op *alleen-lezen*.

## <a name="requirements-for-continuous-deployment"></a>Vereisten voor continue implementatie

Voordat u continue implementatie instellen, moet u de implementatiebron geconfigureerd en uw functiecode aan te geven in de implementatiebron van de hebben. In een functie-app-implementatie is elke functie in een subdirectory, waar de mapnaam de naam van de functie is.  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Als u wilt implementeren vanuit Azure DevOps, moet u eerst uw Azure DevOps-organisatie koppelen aan uw Azure-abonnement. Zie voor meer informatie, [de facturering voor uw organisatie Azure DevOps instellen](https://docs.microsoft.com/azure/devops/organizations/billing/set-up-billing-for-your-organization-vs?view=vsts#set-up-billing-via-the-azure-portal).

## <a name="set-up-continuous-deployment"></a>Doorlopende implementatie instellen
Gebruik deze procedure om continue implementatie voor een bestaande functie-app te configureren. Deze stappen laten zien dat integratie met een GitHub-opslagplaats, maar gelijksoortige stappen gelden voor Azure DevOps- of andere implementatieservices.

1. In uw functie-app in de [Azure-portal](https://portal.azure.com), selecteer **platformfuncties** > **implementatieopties**. 
   
    ![Selecties voor het openen van implementatie-opties](./media/functions-continuous-deployment/setup-deployment.png)
 
1. Op de **implementaties** Selecteer **Setup**.
 
    ![Blade implementaties](./media/functions-continuous-deployment/setup-deployment-1.png)
   
1. Op de **implementatiebron** Selecteer **bron kiezen**. Vul de gegevens voor uw gekozen implementatiebron en selecteer vervolgens **OK**.
   
    ![Een implementatiebron kiezen](./media/functions-continuous-deployment/choose-deployment-source.png)

Na het instellen van continue implementatie, alle bestandswijzigingen in de implementatiebron worden gekopieerd naar de functie-app en de implementatie van een volledige site wordt geactiveerd. De site wordt opnieuw geïmplementeerd wanneer bestanden in de bron worden bijgewerkt.

## <a name="deployment-scenarios"></a>Implementatiescenario's

Normale implementatie scenario's omvatten het maken van een staging-implementatie en het verplaatsen van bestaande functies voor continue implementatie.

<a name="staging"></a>
### <a name="create-a-staging-deployment"></a>Een staging-implementatie maken

Functie-apps ondersteunen geen nog implementatiesites. Maar u kunt nog steeds afzonderlijke fasering en productie-implementaties beheren met behulp van continue integratie.

Het proces voor het configureren en werken met een staging-implementatie ziet er in het algemeen als volgt:

1. Maak twee functie-apps in uw abonnement: één voor de productiecode en één voor fasering. 

1. Maak een implementatiebron, als u er nog geen hebt. In dit voorbeeld wordt [GitHub].

1. Voor de productie-functie-app voltooid de voorgaande stappen in [continue implementatie instellen](#set-up-continuous-deployment) en stel in de vertakking van de implementatie op de master-vertakking van uw GitHub-opslagplaats.
   
    ![Kies een vertakking van de implementatie van selecties](./media/functions-continuous-deployment/choose-deployment-branch.png)

1. Herhaal stap 3 voor de staging-functie-app, maar in plaats daarvan de staging-vertakking in uw GitHub-opslagplaats kiezen. Als de implementatiebron biedt geen ondersteuning voor vertakkingen, gebruikt u een andere map.
    
1. Updates aanbrengen in uw code in de tijdelijke map of vertakking en controleer vervolgens of dat deze wijzigingen in de staging-implementatie worden weergegeven.

1. Na het testen verandert de samenvoeging van de staging-vertakking in de master-vertakking. Deze samenvoeging wordt geactiveerd voor implementatie naar de productie-functie-app. Als de implementatiebron biedt geen ondersteuning voor filialen, overschrijft u de bestanden in de productie-map met de bestanden uit de map met tijdelijke bestanden.

<a name="existing"></a>
### <a name="move-existing-functions-to-continuous-deployment"></a>Verplaatsen van bestaande functies doorlopende implementatie
Wanneer u hebt bestaande functies die u hebt gemaakt en beheerd in de portal, moet u uw functie code-bestanden downloaden met behulp van FTP of de lokale Git-opslagplaats voordat u kunt continue implementatie instellen, zoals eerder beschreven. U kunt dit doen in de Azure App Service-instellingen voor uw functie-app. Nadat u de bestanden gedownload, kunt u ze kunt uploaden naar uw bron gekozen continue implementatie.

> [!NOTE]
> Nadat u continue integratie configureren, kunt u de bronbestanden in de Functions-portal niet meer bewerken.

<a name="credentials"></a>
#### <a name="configure-deployment-credentials"></a>Referenties voor implementatie configureren
Voordat u bestanden van uw functie-app downloaden kunt via FTP of een lokale Git-opslagplaats, moet u uw referenties in om toegang tot de site configureren. Referenties zijn ingesteld op het niveau van de functie-app. Gebruik de volgende stappen uit om in te stellen van referenties voor implementatie in Azure portal:

1. In uw functie-app in de [Azure-portal](https://portal.azure.com), selecteer **platformfuncties** > **implementatiereferenties**.
   
1. Voer een gebruikersnaam en wachtwoord en selecteer vervolgens **opslaan**. 

   ![Selecties lokale implementatiereferenties instellen](./media/functions-continuous-deployment/setup-deployment-credentials.png)

U kunt nu deze referenties gebruiken voor toegang tot uw functie-app van FTP of de ingebouwde Git-opslagplaats.

<a name="downftp"></a>
#### <a name="download-files-by-using-ftp"></a>Downloaden van bestanden via FTP

1. In uw functie-app in de [Azure-portal](https://portal.azure.com), selecteer **platformfuncties** > **eigenschappen**. Kopieer vervolgens de waarden voor **FTP-/ Implementatiegebruiker**, **FTP-hostnaam**, en **FTPS-hostnaam**.  

   **FTP-/ Implementatiegebruiker** moet worden ingevoerd zoals weergegeven in de portal, met inbegrip van de app-naam voor de juiste context voor de FTP-server.
   
   ![Selecties voor de implementatie-informatie](./media/functions-continuous-deployment/get-deployment-credentials.png)

1. Gebruik de verbindingsinformatie bevatten die u hebt verzameld om te verbinden met uw app en download de bronbestanden voor de functies van de FTP-client.

<a name="downgit"></a>
#### <a name="download-files-by-using-a-local-git-repository"></a>Bestanden downloaden met behulp van een lokale Git-opslagplaats

1. In uw functie-app in de [Azure-portal](https://portal.azure.com), selecteer **platformfuncties** > **implementatieopties**. 
   
    ![Selecties voor het openen van implementatie-opties](./media/functions-continuous-deployment/setup-deployment.png)
 
1. Klik op de **implementaties** Selecteer **Setup**.
 
    ![Blade implementaties](./media/functions-continuous-deployment/setup-deployment-1.png)
   
1. Op de **implementatiebron** Selecteer **lokale Git-opslagplaats** > **OK**.

1. In **platformfuncties**, selecteer **eigenschappen** en noteer de waarde van de Git-URL. 
   
    ![Selecties voor het ophalen van de Git-URL](./media/functions-continuous-deployment/get-local-git-deployment-url.png)

1. Kloon de opslagplaats op uw lokale computer met behulp van een Git-bewuste opdrachtprompt of uw favoriete Git-toepassing. De Git-kloon-opdracht ziet er als volgt:
   
        git clone https://username@my-function-app.scm.azurewebsites.net:443/my-function-app.git

1. Bestanden ophalen uit uw functie-app naar de kloon op uw lokale computer, zoals in het volgende voorbeeld:
   
        git pull origin master
   
    Als dat wordt gevraagd, Geef uw [implementatiereferenties geconfigureerd](#credentials).  

[GitHub]: https://github.com/

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aanbevolen procedures voor Azure Functions](functions-best-practices.md)

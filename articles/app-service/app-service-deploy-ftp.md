---
title: Uw app implementeren in Azure App Service met behulp van FTP/S | Microsoft Docs
description: Informatie over het implementeren van uw app in Azure App Service met behulp van de FTP- of FTPS.
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
ms.assetid: ae78b410-1bc0-4d72-8fc4-ac69801247ae
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2016
ms.author: cephalin;dariac
ms.openlocfilehash: 561f317cd7afd740b83709efc8a75ed515626192
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a>Uw app implementeren in Azure App Service met behulp van FTP/S

Dit artikel laat zien hoe u FTP- of FTPS voor het implementeren van uw web-app, back-end voor mobiele app of API-app [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

Het eindpunt van de FTP-/ S voor uw app is al actief. Er is geen configuratie nodig zijn voor de FTP-/ S-implementatie in te schakelen.

<a name="step1"></a>
## <a name="step-1-set-deployment-credentials"></a>Stap 1: Stel de referenties voor implementatie

Voor toegang tot de FTP-server voor uw app, moet u eerst referenties voor implementatie. 

Als u wilt instellen of opnieuw instellen van uw referenties voor implementatie, Zie [referenties voor de Azure App Service-implementatie](app-service-deployment-credentials.md). Deze zelfstudie laat zien dat het gebruik van beveiliging op gebruikersniveau referenties.

## <a name="step-2-get-ftp-connection-information"></a>Stap 2: Gegevens van de FTP-verbinding ophalen

1. In de [Azure-portal](https://portal.azure.com), opent u uw app [bronpagina](../azure-resource-manager/resource-group-portal.md#manage-resources).
2. Selecteer **overzicht** Noteer de waarden voor in de navigatiebalk links **FTP-/ Implementatiegebruiker gebruiker**, **FTP-hostnaam**, en **FTPS hostnaam**. 

    ![FTP-verbindingsgegevens](./media/app-service-deploy-ftp/FTP-Connection-Info.PNG)

    > [!NOTE]
    > Leveren de juiste context voor de FTP-server, de **FTP-/ Implementatiegebruiker gebruiker** waarde die wordt weergegeven door de Azure-portal bevat de appnaam van de.
    > U vindt de dezelfde informatie wanneer u selecteert **eigenschappen** in de linkernavigatiebalk. 
    >
    > Het wachtwoord voor de implementatie wordt nooit weergegeven. Als u uw implementatie wachtwoord bent vergeten, gaat u terug naar [stap 1](#step1) en uw implementatie-wachtwoord opnieuw instellen.
    >
    >

## <a name="step-3-deploy-files-to-azure"></a>Stap 3: Bestanden implementeren in Azure

1. Van uw FTP-client (bijvoorbeeld [Visual Studio](https://www.visualstudio.com/vs/community/) of [FileZilla](https://filezilla-project.org/download.php?type=client)), de verbindingsgegevens die u hebt verzameld om verbinding met uw app te gebruiken.
3. Kopieer uw bestanden en hun respectieve mapstructuur op de [ **/site/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) in Azure (of de **/site/wwwroot/App_Data/taken/** map voor WebJobs).
4. Blader naar uw app-URL om te controleren of dat de app correct wordt uitgevoerd. 

> [!NOTE] 
> In tegenstelling tot [implementaties op basis van Git](app-service-deploy-local-git.md), FTP-implementatie biedt geen ondersteuning voor de volgende implementatie automatische: 
>
> - afhankelijkheid worden hersteld (zoals NuGet, NPM, PIP en Composer automatische)
> - compilatie van binaire bestanden voor .NET
> - generatie van web.config (dit is een [Node.js-voorbeeld](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))
> 
> Genereren van deze bestanden nodig handmatig op uw lokale machine en vervolgens samen met uw app te implementeren.
>
>

## <a name="enforce-ftps"></a>FTPS afdwingen

Voor een betere beveiliging, moet u FTP alleen toestaan via SSL. U kunt ook zowel FTP en FTPS uitschakelen als u geen FTP-implementatie.

In resource-pagina voor uw app in [Azure-portal](https://portal.azure.com), selecteer **appinstellingen** in de linkernavigatiebalk.

Als u niet-versleutelde FTP uitschakelen, inschakelen **FTPS alleen**. Als u wilt zowel FTP en FTPS volledig uitschakelen, selecteert u **uitschakelen**. Wanneer u klaar bent, klikt u op **opslaan**.

![FTP/S uitschakelen](./media/app-service-deploy-ftp/disable-ftp.png)

## <a name="troubleshoot-ftp-deployment"></a>FTP-implementatie oplossen

- [Hoe kan ik de FTP-implementatie oplossen?](#how-can-i-troubleshoot-ftp-deployment)
- [Ik kan geen FTP en publiceren van mijn code. Hoe worden het probleem kunt oplossen?](#im-not-able-to-ftp-and-publish-my-code-how-can-i-resolve-the-issue)
- [Hoe kan ik verbinden met FTP-in Azure App Service via de passieve modus?](#how-can-i-connect-to-ftp-in-azure-app-service-via-passive-mode)

### <a name="how-can-i-troubleshoot-ftp-deployment"></a>Hoe kan ik de FTP-implementatie oplossen?

De eerste stap voor het oplossen van FTP-implementatie is een probleem met de implementatie van een toepassingsprobleem runtime isoleren.

Een probleem met de implementatie resulteert gewoonlijk in geen bestanden of verkeerde bestanden op uw app is geïmplementeerd. Het kan worden verholpen door het onderzoeken van uw FTP-implementatie of een alternatieve implementatie pad (zoals bronbeheer) selecteren.

Een runtime-toepassingsprobleem resulteert gewoonlijk in de juiste set met bestanden die zijn geïmplementeerd voor uw app maar onjuist app-gedrag. Het kan worden verholpen door te focussen op code gedrag tijdens runtime en paden van de specifieke fout onderzoeken.

Om te bepalen een probleem met de implementatie of runtime, Zie [implementatie versus runtime problemen](https://github.com/projectkudu/kudu/wiki/Deployment-vs-runtime-issues).

 
### <a name="im-not-able-to-ftp-and-publish-my-code-how-can-i-resolve-the-issue"></a>Ik kan geen FTP en publiceren van mijn code. Hoe worden het probleem kunt oplossen?
Controleer of u de juiste hostnaam hebt ingevoerd en [referenties](#step-1--set-deployment-credentials). Controleer ook de volgende FTP-poorten op uw computer worden niet geblokkeerd door een firewall:

- Verbindingspoort voor FTP-besturingselement: 21
- FTP-gegevenspoort voor verbinding: 989, 10001 10300
 
### <a name="how-can-i-connect-to-ftp-in-azure-app-service-via-passive-mode"></a>Hoe kan ik verbinden met FTP-in Azure App Service via de passieve modus?
Azure App Service biedt ondersteuning voor verbinding maken via zowel actieve en passieve modus. Passieve modus verdient de voorkeur omdat uw implementatie-machines meestal achter een firewall (in het besturingssysteem of als onderdeel van een thuis- of bedrijfsnetwerk zijn). Zie een [voorbeeld in de documentatie WinSCP](https://winscp.net/docs/ui_login_connection). 

## <a name="next-steps"></a>Volgende stappen

Probeer meer geavanceerde implementatiescenario's voor [implementeren naar Azure met Git](app-service-deploy-local-git.md). Implementatie op basis van GIT in Azure kunt versiebeheer, pakket herstellen en MSBuild.

## <a name="more-resources"></a>Meer bronnen

* [Referenties voor Azure App Service-implementatie](app-service-deploy-ftp.md)

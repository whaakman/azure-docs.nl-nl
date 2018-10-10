---
title: Uw app implementeren in Azure App Service met behulp van FTP/S | Microsoft Docs
description: Leer hoe u uw app implementeren in Azure App Service met behulp van FTP- of FTPS.
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
ms.date: 06/05/2018
ms.author: cephalin;dariac
ms.openlocfilehash: f82a01f3a2b88e66988717eb008a65c8f2aecf28
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900322"
---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a>Uw app implementeren in Azure App Service met behulp van FTP/S

Dit artikel leest u hoe uw web-app, back-end mobiele Apps of API-app implementeren met FTP of FTPS [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

Het eindpunt FTP/S voor uw app is al actief. Er is geen configuratie nodig om in te schakelen van FTP/S-implementatie.

## <a name="open-ftp-dashboard"></a>FTP-dashboard openen

In de [Azure-portal](https://portal.azure.com), opent u van uw app [resourcepagina](../azure-resource-manager/resource-group-portal.md#manage-resources).

De FTP-dashboard, klikt u op **continue levering (Preview)** > **FTP** > **Dashboard**.

![FTP-dashboard openen](./media/app-service-deploy-ftp/open-dashboard.png)

## <a name="get-ftp-connection-information"></a>FTP-verbindingsgegevens ophalen

Klik in het dashboard FTP **kopie** om te kopiëren van de FTPS en de app-referenties.

![FTP-gegevens kopiëren](./media/app-service-deploy-ftp/ftp-dashboard.png)

Het wordt aanbevolen dat u **App referenties** implementeert in uw app omdat het is uniek voor elke app. Echter, als u klikt op **gebruikersreferenties**, kunt u op gebruikersniveau referenties die u voor FTP/S Meld u aan bij alle apps in App Service in uw abonnement gebruiken kunt instellen.

## <a name="deploy-files-to-azure"></a>Bestanden in Azure implementeren

1. Van uw FTP-client (bijvoorbeeld [Visual Studio](https://www.visualstudio.com/vs/community/) of [FileZilla](https://filezilla-project.org/download.php?type=client)), de verbindingsgegevens die u hebt verzameld om verbinding met uw app te gebruiken.
3. Kopieer de bestanden en de mappenstructuur naar de [ **/site/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) in Azure (of de **/site/wwwroot/App_Data/Jobs/** Active directory voor WebJobs).
4. Blader naar de URL van uw app om te controleren of dat de app correct wordt uitgevoerd. 

> [!NOTE] 
> In tegenstelling tot [Git-implementaties](app-service-deploy-local-git.md), FTP-implementatie biedt geen ondersteuning voor de volgende implementatie-automatische: 
>
> - afhankelijkheid worden hersteld (zoals NuGet, NPM, PIP en Composer automatische)
> - compilatie van binaire bestanden voor .NET
> - het genereren van web.config (dit is een [Node.js-voorbeeld](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))
> 
> Deze vereiste bestanden handmatig op uw lokale computer genereren, en vervolgens samen met uw app te implementeren.
>
>

## <a name="enforce-ftps"></a>FTPS afdwingen

Voor een betere beveiliging, moet u FTP alleen toestaan via SSL. U kunt ook FTP- en FTPS uitschakelen als u geen FTP-implementatie.

In de resourcepagina van uw app in [Azure-portal](https://portal.azure.com), selecteer **App-instellingen** in het linkernavigatievenster.

Als u niet-versleutelde FTP uitschakelen, inschakelen **alleen FTPS**. Als u FTP- en FTPS volledig uitschakelen, inschakelen **uitschakelen**. Klik op **Opslaan** als u klaar bent. Als u **alleen FTPS** u TLS 1.1 of hoger moet afdwingen door te navigeren naar de **SSL-instellingen** blade van uw web-app. TLS 1.0 wordt niet ondersteund met **alleen FTPS**.

![Uitschakelen van FTP/S](./media/app-service-deploy-ftp/disable-ftp.png)

## <a name="automate-with-scripts"></a>Automatiseren met scripts

Voor het gebruik van de FTP-implementatie [Azure CLI](/cli/azure), Zie [een web-app maken en bestanden implementeren met FTP (Azure CLI)](./scripts/app-service-cli-deploy-ftp.md).

Voor het gebruik van de FTP-implementatie [Azure PowerShell](/cli/azure), Zie [bestanden uploaden naar een web-app met FTP (PowerShell)](./scripts/app-service-powershell-deploy-ftp.md).

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="troubleshoot-ftp-deployment"></a>FTP-implementatie oplossen

- [Hoe los ik FTP-implementatie?](#how-can-i-troubleshoot-ftp-deployment)
- [Ik kan geen FTP en publiceren van mijn code. Hoe kan ik het probleem oplossen?](#im-not-able-to-ftp-and-publish-my-code-how-can-i-resolve-the-issue)
- [Hoe kan ik verbinding maken met FTP in Azure App Service via de passieve modus?](#how-can-i-connect-to-ftp-in-azure-app-service-via-passive-mode)

### <a name="how-can-i-troubleshoot-ftp-deployment"></a>Hoe los ik FTP-implementatie?

De eerste stap voor het oplossen van FTP-implementatie is een probleem met de implementatie van een toepassingsprobleem runtime isoleren.

Een probleem met de implementatie wordt doorgaans resulteert in geen bestanden of de verkeerde bestanden op uw app is geïmplementeerd. U kunt oplossen door uw FTP-implementatie onderzoeken of een alternatieve implementatie-pad (zoals broncodebeheer) selecteren.

Een runtime-toepassingsprobleem resulteert gewoonlijk in de juiste set bestanden die zijn geïmplementeerd in uw app, maar onjuist app-gedrag. U kunt oplossen door te focussen op code gedrag tijdens runtime en onderzoeken van specifieke paden.

Om te bepalen van een probleem met de implementatie of runtime, Zie [implementatie versus runtimeproblemen](https://github.com/projectkudu/kudu/wiki/Deployment-vs-runtime-issues).

### <a name="im-not-able-to-ftp-and-publish-my-code-how-can-i-resolve-the-issue"></a>Ik kan geen FTP en publiceren van mijn code. Hoe kan ik het probleem oplossen?
Controleer of u de juiste hostnaam hebt ingevoerd en [referenties](#step-1--set-deployment-credentials). Controleer ook de volgende FTP-poorten op uw computer worden niet geblokkeerd door een firewall:

- Verbindingspoort voor FTP-besturingselement: 21
- Verbindingspoort voor FTP-gegevens: 989, 10001-10300
 
### <a name="how-can-i-connect-to-ftp-in-azure-app-service-via-passive-mode"></a>Hoe kan ik verbinding maken met FTP in Azure App Service via de passieve modus?
Azure App Service ondersteunt te verbinden via zowel actieve en passieve modus. Passieve modus verdient de voorkeur omdat uw implementatie-machines meestal achter een firewall (in het besturingssysteem of als onderdeel van een thuis- of business-netwerk zijn). Zie een [voorbeeld in de documentatie WinSCP](https://winscp.net/docs/ui_login_connection). 

## <a name="next-steps"></a>Volgende stappen

Voor meer geavanceerde implementatiescenario's, proberen [implementeren naar Azure met Git](app-service-deploy-local-git.md). Op basis van een GIT-implementatie naar Azure kunt versiebeheer, pakket terugzetten en MSBuild.

## <a name="more-resources"></a>Meer bronnen

* [Referenties voor Azure App Service-implementatie](app-service-deployment-credentials.md)

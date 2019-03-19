---
title: Inhoud met behulp van FTP/S - Azure App Service implementeren | Microsoft Docs
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
ms.date: 11/30/2018
ms.author: cephalin;dariac
ms.custom: seodec18
ms.openlocfilehash: db8445ec2b3dd8bdefa661d7f186e720c6fada09
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57858874"
---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a>Uw app implementeren in Azure App Service met behulp van FTP/S

Dit artikel leest u hoe uw web-app, back-end mobiele Apps of API-app implementeren met FTP of FTPS [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714).

Het eindpunt FTP/S voor uw app is al actief. Er is geen configuratie nodig om in te schakelen van FTP/S-implementatie.

## <a name="open-ftp-dashboard"></a>FTP-dashboard openen

In de [Azure-portal](https://portal.azure.com), opent u van uw app [resourcepagina](../azure-resource-manager/manage-resources-portal.md#manage-resources).

De FTP-dashboard, klikt u op **Implementatiecentrum** > **FTP** > **Dashboard**.

![FTP-dashboard openen](./media/app-service-deploy-ftp/open-dashboard.png)

## <a name="get-ftp-connection-information"></a>FTP-verbindingsgegevens ophalen

Klik in het dashboard FTP **kopie** om te kopiëren van de FTPS en de app-referenties.

![FTP-gegevens kopiëren](./media/app-service-deploy-ftp/ftp-dashboard.png)

Het wordt aanbevolen dat u **App referenties** implementeert in uw app omdat het is uniek voor elke app. Echter, als u klikt op **gebruikersreferenties**, kunt u op gebruikersniveau referenties die u voor FTP/S Meld u aan bij alle apps in App Service in uw abonnement gebruiken kunt instellen.

> [!NOTE]
> Verificatie bij een FTP-/ FTPS-eindpunt met behulp van de referenties op gebruikersniveau requirers een gebruikersnaam in de volgende indeling: 
>
>`<app-name>\<user-name>`
>
> Omdat op gebruikersniveau referenties zijn gekoppeld aan de gebruiker en niet op een specifieke bron, wordt de gebruikersnaam moet in deze indeling om te leiden van de actie aanmelden op de juiste app-eindpunt.
>

## <a name="deploy-files-to-azure"></a>Bestanden in Azure implementeren

1. Van uw FTP-client (bijvoorbeeld [Visual Studio](https://www.visualstudio.com/vs/community/), [Cyberduck](https://cyberduck.io/), of [WinSCP](https://winscp.net/index.php)), de verbindingsgegevens die u hebt verzameld om verbinding met uw app te gebruiken.
2. Kopieer de bestanden en de mappenstructuur naar de [ **/site/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) in Azure (of de **/site/wwwroot/App_Data/Jobs/** Active directory voor WebJobs).
3. Blader naar de URL van uw app om te controleren of dat de app correct wordt uitgevoerd. 

> [!NOTE] 
> In tegenstelling tot [Git-implementaties](deploy-local-git.md), FTP-implementatie biedt geen ondersteuning voor de volgende implementatie-automatische: 
>
> - afhankelijkheid worden hersteld (zoals NuGet, NPM, PIP en Composer automatische)
> - compilatie van binaire bestanden voor .NET
> - het genereren van web.config (dit is een [Node.js-voorbeeld](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))
> 
> Deze vereiste bestanden handmatig op uw lokale computer genereren, en vervolgens samen met uw app te implementeren.
>

## <a name="enforce-ftps"></a>FTPS afdwingen

Voor een betere beveiliging, moet u FTP alleen toestaan via SSL. U kunt ook FTP- en FTPS uitschakelen als u geen FTP-implementatie.

In de resourcepagina van uw app in [Azure-portal](https://portal.azure.com), selecteer **App-instellingen** in het linkernavigatievenster.

Als u niet-versleutelde FTP uitschakelen, inschakelen **alleen FTPS**. Als u FTP- en FTPS volledig uitschakelen, inschakelen **uitschakelen**. Wanneer u klaar bent, klikt u op **Opslaan**. Als u **alleen FTPS** u TLS 1.2 of hoger moet afdwingen door te navigeren naar de **SSL-instellingen** blade van uw web-app. TLS 1.0 en 1.1 worden niet ondersteund met **alleen FTPS**.

![Uitschakelen van FTP/S](./media/app-service-deploy-ftp/disable-ftp.png)

## <a name="automate-with-scripts"></a>Automatiseren met scripts

Voor het gebruik van de FTP-implementatie [Azure CLI](/cli/azure), Zie [een web-app maken en bestanden implementeren met FTP (Azure CLI)](./scripts/cli-deploy-ftp.md).

Voor het gebruik van de FTP-implementatie [Azure PowerShell](/cli/azure), Zie [bestanden uploaden naar een web-app met FTP (PowerShell)](./scripts/powershell-deploy-ftp.md).

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
Controleer of u de juiste hostnaam hebt ingevoerd en [referenties](#open-ftp-dashboard). Controleer ook de volgende FTP-poorten op uw computer worden niet geblokkeerd door een firewall:

- Verbindingspoort voor FTP-besturingselement: 21
- Verbindingspoort voor FTP-gegevens: 989, 10001-10300
 
### <a name="how-can-i-connect-to-ftp-in-azure-app-service-via-passive-mode"></a>Hoe kan ik verbinding maken met FTP in Azure App Service via de passieve modus?
Azure App Service ondersteunt te verbinden via zowel actieve en passieve modus. Passieve modus verdient de voorkeur omdat uw implementatie-machines meestal achter een firewall (in het besturingssysteem of als onderdeel van een thuis- of business-netwerk zijn). Zie een [voorbeeld in de documentatie WinSCP](https://winscp.net/docs/ui_login_connection). 

## <a name="next-steps"></a>Volgende stappen

Voor meer geavanceerde implementatiescenario's, proberen [implementeren naar Azure met Git](deploy-local-git.md). Op basis van een GIT-implementatie naar Azure kunt versiebeheer, pakket terugzetten en MSBuild.

## <a name="more-resources"></a>Meer bronnen

* [Referenties voor Azure App Service-implementatie](deploy-configure-credentials.md)

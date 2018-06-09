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
ms.date: 06/05/2018
ms.author: cephalin;dariac
ms.openlocfilehash: 2ec08b45fab9987e9271c1ff3101eaf321dc84be
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234220"
---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a>Uw app implementeren in Azure App Service met behulp van FTP/S

Dit artikel laat zien hoe u FTP- of FTPS voor het implementeren van uw web-app, back-end voor mobiele app of API-app [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

Het eindpunt van de FTP-/ S voor uw app is al actief. Er is geen configuratie nodig zijn voor de FTP-/ S-implementatie in te schakelen.

## <a name="open-ftp-dashboard"></a>Open FTP-dashboard

In de [Azure-portal](https://portal.azure.com), opent u uw app [bronpagina](../azure-resource-manager/resource-group-portal.md#manage-resources).

De FTP-dashboard, klikt u op **continue levering (Preview)** > **FTP** > **Dashboard**.

![Open FTP-dashboard](./media/app-service-deploy-ftp/open-dashboard.png)

## <a name="get-ftp-connection-information"></a>Gegevens van de FTP-verbinding ophalen

Klik in het dashboard FTP- **kopie** kopiëren van de FTPS-eindpunt en app-referenties.

![FTP-gegevens kopiëren](./media/app-service-deploy-ftp/ftp-dashboard.png)

Het verdient aanbeveling dat u **App referenties** om te implementeren voor uw app omdat deze uniek voor elke app is. Echter, als u op **gebruikersreferenties**, kunt u beveiliging op gebruikersniveau referenties die u voor FTP-/ S aanmelden bij alle App Service-apps in uw abonnement gebruiken kunt instellen.

## <a name="deploy-files-to-azure"></a>Bestanden implementeren in Azure

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

## <a name="automate-with-scripts"></a>Automatiseren met scripts

Voor het gebruik van de FTP-implementatie [Azure CLI](/cli/azure), Zie [een web-app maken en implementeren van bestanden met FTP (Azure CLI)](./scripts/app-service-cli-deploy-ftp.md).

Voor het gebruik van de FTP-implementatie [Azure PowerShell](/cli/azure), Zie [bestanden uploaden naar een web-app met FTP (PowerShell)](./scripts/app-service-powershell-deploy-ftp.md).

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="troubleshoot-ftp-deployment"></a>FTP-implementatie oplossen

- [Hoe kan ik de FTP-implementatie oplossen?](#how-can-i-troubleshoot-ftp-deployment)
- [Ik kan geen FTP en publiceren van mijn code. Hoe worden het probleem kunt oplossen?](#im-not-able-to-ftp-and-publish-my-code-how-can-i-resolve-the-issue)
- [Hoe kan ik verbinden met FTP-in Azure App Service via de passieve modus?](#how-can-i-connect-to-ftp-in-azure-app-service-via-passive-mode)

### <a name="how-can-i-troubleshoot-ftp-deployment"></a>Hoe kan ik de FTP-implementatie oplossen?

De eerste stap voor het oplossen van FTP-implementatie is een probleem met de implementatie van een toepassingsprobleem runtime isoleren.

Een probleem met de implementatie resulteert gewoonlijk in geen bestanden of verkeerde bestanden op uw app is geïmplementeerd. Als u problemen met het onderzoeken van uw FTP-implementatie of een alternatieve implementatie pad (zoals bronbeheer) selecteren.

Een runtime-toepassingsprobleem resulteert gewoonlijk in de juiste set met bestanden die zijn geïmplementeerd voor uw app maar onjuist app-gedrag. U kunt oplossen door te focussen op code gedrag tijdens runtime en paden van de specifieke fout onderzoeken.

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

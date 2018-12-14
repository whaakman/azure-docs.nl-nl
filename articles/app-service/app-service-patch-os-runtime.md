---
title: OS- en runtime patches uitgebracht - Azure App Service | Microsoft Docs
description: Hierin wordt beschreven hoe Azure App Service-updates van het besturingssysteem en runtimes, en hoe u kunt aankondigingen bijwerken.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 3dcaba9c0512cbe2c4cf1cd14338a88209a80e85
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53385309"
---
# <a name="os-and-runtime-patching-in-azure-app-service"></a>OS- en runtime patching uit handen in Azure App Service

Dit artikel leest u hoe u aan een bepaalde versie-informatie met betrekking tot het besturingssysteem of de software in [App Service](app-service-web-overview.md). 

App Service is een Platform-as-a-Service, wat dat betekent het besturingssysteem en de stack-toepassing voor u worden beheerd door Azure gebruikt; u alleen beheren voor uw toepassing en de bijbehorende gegevens. Meer controle over het besturingssysteem en de toepassing stack is beschikbaar in [Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/). Met die in gedachten is het toch nuttig voor u als een App Service-gebruiker over meer informatie, zoals:

-   Hoe en wanneer worden updates van het besturingssysteem dan toegepast?
-   Hoe is de App Service gevuld op basis van aanzienlijke beveiligingsproblemen (zoals nul dagen)?
-   Welke versies van het besturingssysteem en de runtime uw toepassingen worden uitgevoerd?

Uit veiligheidsoverwegingen zijn bepaalde specifieke instellingen van de beveiligingsgegevens niet gepubliceerd. Echter, het artikel beoogd vanwege problemen met transparantie maximaliseren van het proces en hoe u op de hoogte van aankondigingen veiligheidsgerelateerde of runtime-updates kunt blijven.

## <a name="how-and-when-are-os-updates-applied"></a>Hoe en wanneer worden updates van het besturingssysteem dan toegepast?

Azure beheert patchen van het besturingssysteem op twee niveaus, de fysieke servers en de Gast virtuele machines (VM's) die de App Service-resources worden uitgevoerd. Beide per maand, worden bijgewerkt die overeenstemt met de maandelijkse [Patch-dinsdag](https://technet.microsoft.com/security/bulletins.aspx) planning. Deze updates worden automatisch toegepast op een manier die de SLA van Azure services met hoge beschikbaarheid garandeert. 

Zie voor gedetailleerde informatie over hoe updates worden toegepast, [ontrafelen van de kracht achter de updates van het besturingssysteem van App Service](https://blogs.msdn.microsoft.com/appserviceteam/2018/01/18/demystifying-the-magic-behind-app-service-os-updates/).

## <a name="how-does-azure-deal-with-significant-vulnerabilities"></a>Hoe Azure omgaan met aanzienlijke beveiligingsproblemen?

Wanneer ernstige problemen nodig hebt onmiddellijk patching, zoals [nul dagen beveiligingsproblemen](https://wikipedia.org/wiki/Zero-day_(computing)), de updates met hoge prioriteit worden verwerkt op een per geval beoordeeld.

Blijf op de hoogte met kritiek beveiligingsaankondigingen in Azure door naar de pagina [Azure-Beveiligingsblog](https://azure.microsoft.com/blog/topics/security/). 

## <a name="when-are-supported-language-runtimes-updated-added-or-deprecated"></a>Wanneer zijn ondersteunde taal runtimes bijgewerkt, toegevoegd of afgeschaft?

Nieuwe stabiele versie van ondersteunde talen, runtimes (primaire, secundaire of patch) worden regelmatig toegevoegd aan App Service-exemplaren. Sommige updates overschrijven de bestaande installatie, terwijl anderen naast de huidige versies zijn geïnstalleerd. Een installatie overschrijven betekent dat uw app automatisch wordt uitgevoerd in de bijgewerkte runtime. De installatie van een side-by-side betekent dat u moet handmatig migreren van uw app om te profiteren van een nieuwe runtimeversie. Raadpleeg een van de subsecties voor meer informatie.

Runtime-updates en afschaffingen worden hier aangekondigd:

- https://azure.microsoft.com/updates/?product=app-service 
- https://github.com/Azure/app-service-announcements/issues

> [!NOTE] 
> Hier is van toepassing op taal runtimes die zijn ingebouwd in een App Service-app. Een aangepaste runtime die u naar App Service uploadt, bijvoorbeeld ongewijzigd, tenzij u deze handmatig bijwerken.
>
>

### <a name="new-patch-updates"></a>Nieuwe patchupdates

Patch-updates voor .NET, PHP, Java-SDK of Tomcat/Jetty-versie worden automatisch toegepast door de bestaande installatie overschrijven met de nieuwe versie. Node.js-patchupdates worden geïnstalleerd naast de bestaande versies (vergelijkbaar met de primaire en secundaire versies in de volgende sectie). Versies van de nieuwe Python-patch handmatig via kunnen worden geïnstalleerd [site-extensies](https://www.siteextensions.net/packages?q=Tags%3A%22python%22)), naast de ingebouwde Python-installaties.

### <a name="new-major-and-minor-versions"></a>Nieuwe primaire en secundaire versies

Wanneer een nieuwe primaire of secundaire versie is toegevoegd, wordt naast de bestaande versies geïnstalleerd. U kunt uw app handmatig upgraden naar de nieuwe versie. Als u de runtimeversie geconfigureerd in een configuratiebestand (zoals `web.config` en `package.json`), moet u een upgrade uitvoert op dezelfde manier. Als u een App Service gebruikt instellen op de runtimeversie configureren, kunt u wijzigen in de [Azure-portal](https://portal.azure.com) of door het uitvoeren van een [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) opdracht in de [Cloud Shell](../cloud-shell/overview.md), als weergegeven in de volgende voorbeelden:

```azurecli-interactive
az webapp config set --net-framework-version v4.7 --resource-group <groupname> --name <appname>
az webapp config set --php-version 7.0 --resource-group <groupname> --name <appname>
az webapp config appsettings set --settings WEBSITE_NODE_DEFAULT_VERSION=8.9.3 --resource-group <groupname> --name <appname>
az webapp config set --python-version 3.4 --resource-group <groupname> --name <appname>
az webapp config set --java-version 1.8 --java-container Tomcat --java-container-version 9.0 --resource-group <groupname> --name <appname>
```

### <a name="deprecated-versions"></a>Afgeschafte versies  

Wanneer een oudere versie wordt afgeschaft, is de verwijderingsdatum profiel zodat u kunt de upgrade van de runtime-versie dienovereenkomstig plannen aangekondigd. 

## <a name="how-can-i-query-os-and-runtime-update-status-on-my-instances"></a>Hoe kan ik updatestatus OS- en runtime opvragen op mijn exemplaren?  

Terwijl essentiële informatie van het besturingssysteem is vergrendeld tegen toegang (Zie [functionaliteit van het besturingssysteem op Azure App Service](operating-system-functionality.md)), wordt de [Kudu-console](https://github.com/projectkudu/kudu/wiki/Kudu-console) kunt u om op te vragen van uw App Service-exemplaar met betrekking tot het besturingssysteem versie en runtimeversies. 

De volgende tabel ziet u hoe u de versies van Windows en de taal die uw toepassingen worden uitgevoerd:

| Informatie | Waar vind ik het | 
|-|-|
| Windows-versie | Zie `https://<appname>.scm.azurewebsites.net/Env.cshtml` (onder Systeeminfo) |
| .NET-versie | Op `https://<appname>.scm.azurewebsites.net/DebugConsole`, voer de volgende opdracht uit achter de opdrachtprompt: <br>`powershell -command "gci 'Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Net Framework Setup\NDP\CDF'"` |
| Versie van .NET core | Op `https://<appname>.scm.azurewebsites.net/DebugConsole`, voer de volgende opdracht uit achter de opdrachtprompt: <br> `dotnet --version` |
| PHP-versie | Op `https://<appname>.scm.azurewebsites.net/DebugConsole`, voer de volgende opdracht uit achter de opdrachtprompt: <br> `php --version` |
| Standaardversie van Node.js | In de [Cloud Shell](../cloud-shell/overview.md), voer de volgende opdracht uit: <br> `az webapp config appsettings list --resource-group <groupname> --name <appname> --query "[?name=='WEBSITE_NODE_DEFAULT_VERSION']"` |
| Python-versie | Op `https://<appname>.scm.azurewebsites.net/DebugConsole`, voer de volgende opdracht uit achter de opdrachtprompt: <br> `python --version` |  

> [!NOTE]  
> Toegang tot de registerlocatie `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Component Based Servicing\Packages`, waarbij informatie over ["KB" patches](https://docs.microsoft.com/security-updates/SecurityBulletins/securitybulletins) zijn opgeslagen, is vergrendeld.
>
>

## <a name="more-resources"></a>Meer bronnen

[Vertrouwenscentrum van: Beveiliging](https://www.microsoft.com/en-us/trustcenter/security)  
[64-bits ASP.NET Core in Azure App Service](https://gist.github.com/glennc/e705cd85c9680d6a8f1bdb62099c7ac7)

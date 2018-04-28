---
title: OS- en runtime patchen in Azure App Service | Microsoft Docs
description: Hierin wordt beschreven hoe het besturingssysteem van de Azure App Service-updates en runtimes en hoe u toegang krijgen aankondigingen bijwerken.
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
ms.openlocfilehash: 92b6945ad13842e926d53be6dcc0d21554485ff3
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="os-and-runtime-patching-in-azure-app-service"></a>OS- en runtime patchen in Azure App Service

Dit artikel laat zien hoe u bepaalde versie-informatie met betrekking tot het besturingssysteem of de software in [App Service](app-service-web-overview.md). 

App Service is een Platform-as-a-Service, wat dat betekent het besturingssysteem en de toepassing stapelen voor u worden beheerd door Azure. u alleen beheren voor uw toepassing en de bijbehorende gegevens. Meer controle over het besturingssysteem en de toepassing de stack is beschikbaar u in [Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/). Met daarom is het toch nuttig zijn voor u als gebruiker met App Service voor meer informatie, zoals weten:

-   Hoe en wanneer OS-updates gelden?
-   Hoe wordt App Service is een patch uitgevoerd tegen een aanzienlijke beveiligingsproblemen (zoals nul-dag)?
-   Welke versies van besturingssystemen en runtime uw toepassingen worden uitgevoerd?

Uit veiligheidsoverwegingen worden bepaalde details van de beveiligingsgegevens niet gepubliceerd. Het artikel ernaar echter problemen door transparantie het optimaliseren van het proces en hoe u kunt de hoogte blijven van aankondigingen betrekking hebben op beveiliging of runtime-updates te verlichten.

## <a name="how-and-when-are-os-updates-applied"></a>Hoe en wanneer OS-updates gelden?

Azure beheert OS patches op twee niveaus, de fysieke servers en de gast (virtuele machines) met de App Service-resources. Beide maandelijks, worden bijgewerkt die wordt uitgelijnd naar het maandelijks [Patch-dinsdag](https://technet.microsoft.com/security/bulletins.aspx) planning. Deze updates worden automatisch toegepast op een manier die de SERVICEOVEREENKOMST van Azure services met hoge beschikbaarheid garandeert. 

Zie voor gedetailleerde informatie over hoe updates worden toegepast, [opheldering van het geheim achter OS van App Service-updates](https://blogs.msdn.microsoft.com/appserviceteam/2018/01/18/demystifying-the-magic-behind-app-service-os-updates/).

## <a name="how-does-azure-deal-with-significant-vulnerabilities"></a>Hoe Azure omgaan met aanzienlijke beveiligingsproblemen?

Wanneer ernstige beveiligingsproblemen vereisen onmiddellijke patchen, zoals [nul dag beveiligingslekken](https://wikipedia.org/wiki/Zero-day_(computing)), de updates met hoge prioriteit per geval worden afgehandeld.

De hoogte blijven van kritieke veiligheidsberichten in Azure door bezoeken [Azure-Beveiligingsblog](https://azure.microsoft.com/blog/topics/security/). 

## <a name="when-are-supported-language-runtimes-updated-added-or-deprecated"></a>Wanneer zijn ondersteunde taal runtimes bijgewerkt, toegevoegd of afgeschaft?

Nieuwe stabiele versies van de ondersteunde taal runtimes (primaire, secundaire of patch) worden regelmatig toegevoegd aan de App Service-exemplaren. Sommige updates overschrijf de bestaande installatie terwijl anderen en naast elkaar bestaande versies zijn geïnstalleerd. De installatie van een overschrijven betekent dat uw app automatisch wordt uitgevoerd in de bijgewerkte runtime. De installatie van een side-by-side: u moet handmatig migreren van uw app om te profiteren van een nieuwe runtimeversie. Raadpleeg een van de subsecties voor meer informatie.

Runtime-updates en afschaffingen worden hier vermeld:

- https://azure.microsoft.com/updates/?product=app-service 
- https://github.com/Azure/app-service-announcements/issues

> [!NOTE] 
> Hier informatie geldt voor de taal runtimes die zijn ingebouwd in een App Service-app. Een aangepaste runtime die u naar App Service uploadt, bijvoorbeeld ongewijzigd tenzij u deze handmatig bijwerken.
>
>

### <a name="new-patch-updates"></a>Nieuwe patchupdates

Patch voor updates voor .NET, PHP, Java SDK of Tomcat/Jetty-versie worden automatisch toegepast door de bestaande installatie wordt overschreven door de nieuwe versie. Node.js patchupdates worden geïnstalleerd en naast elkaar bestaande versies (vergelijkbaar met de primaire en secundaire versies in de volgende sectie). Versies van de nieuwe Python patch handmatig via kunnen worden geïnstalleerd [site-uitbreidingen](https://www.siteextensions.net/packages?q=Tags%3A%22python%22)), naast elkaar met het ingebouwde Python-installaties.

### <a name="new-major-and-minor-versions"></a>Nieuwe primaire en secundaire versies

Wanneer een nieuwe primaire of secundaire versie wordt toegevoegd, wordt deze geïnstalleerd en naast elkaar bestaande versies. U kunt uw app handmatig upgraden naar de nieuwe versie. Als u de runtimeversie geconfigureerd in een configuratiebestand (zoals `web.config` en `package.json`), moet u een upgrade uitvoert op dezelfde methode. Als u een App Service gebruikt instellen voor uw runtimeversie configureren, kunt u wijzigen in de [Azure-portal](https://portal.azure.com) of door het uitvoeren van een [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) opdracht in de [Cloud Shell](../cloud-shell/overview.md), als weergegeven in de volgende voorbeelden:

```azurecli-interactive
az webapp config set --net-framework-version v4.7 --resource-group <groupname> --name <appname>
az webapp config set --php-version 7.0 --resource-group <groupname> --name <appname>
az webapp config appsettings set --settings WEBSITE_NODE_DEFAULT_VERSION=8.9.3 --resource-group <groupname> --name <appname>
az webapp config set --python-version 3.4 --resource-group <groupname> --name <appname>
az webapp config set --java-version 1.8 --java-container Tomcat --java-container-version 9.0 --resource-group <groupname> --name <appname>
```

### <a name="deprecated-versions"></a>Afgeschafte versies

Wanneer een oudere versie is afgeschaft, wordt de datum van verwijdering aangekondigd zodat u de upgrade van uw runtime versie Hiermee rekening kan houden. 

## <a name="how-can-i-query-os-and-runtime-update-status-on-my-instances"></a>Hoe kan ik updatestatus OS- en runtime opvragen op mijn exemplaren?

Terwijl essentiële OS-informatie van de toegang is vergrendeld (Zie [besturingssysteemfunctionaliteit op Azure App Service](web-sites-available-operating-system-functionality.md)), wordt de [Kudu-console](https://github.com/projectkudu/kudu/wiki/Kudu-console) kunt u een query uitvoeren op uw App Service-exemplaar met betrekking tot het besturingssysteem versie en runtime-versies. 

De volgende tabel toont hoe u de versies van Windows en van de taal die uw toepassingen worden uitgevoerd:

| Informatie | Waar vind ik het |
|-|-|
| Windows-versie | Zie `https://<appname>.scm.azurewebsites.net/Env.cshtml` (onder Systeeminfo) |
| .NET-versie | Op `https://<appname>.scm.azurewebsites.net/DebugConsole`, voer de volgende opdracht in de opdrachtprompt: <br>`powershell -command "gci 'Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Net Framework Setup\NDP\CDF'"` |
| Versie van .NET core | Op `https://<appname>.scm.azurewebsites.net/DebugConsole`, voer de volgende opdracht in de opdrachtprompt: <br> `dotnet --version` |
| PHP-versie | Op `https://<appname>.scm.azurewebsites.net/DebugConsole`, voer de volgende opdracht in de opdrachtprompt: <br> `php --version` |
| Standaardversie van Node.js | In de [Cloud Shell](../cloud-shell/overview.md), voer de volgende opdracht: <br> `az webapp config appsettings list --resource-group <groupname> --name <appname> --query "[?name=='WEBSITE_NODE_DEFAULT_VERSION']"` |
| Python-versie | Op `https://<appname>.scm.azurewebsites.net/DebugConsole`, voer de volgende opdracht in de opdrachtprompt: <br> `python --version` |

> [!NOTE]
> Toegang tot registerlocatie `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Component Based Servicing\Packages`, waarbij informatie over ['KB' patches]((https://docs.microsoft.com/security-updates/SecurityBulletins/securitybulletins)) zijn opgeslagen, is vergrendeld.
>
>

## <a name="more-resources"></a>Meer bronnen

[Vertrouwenscentrum: beveiliging](https://www.microsoft.com/TrustCenter/Security/default.aspx)  
[64-bits ASP.NET Core op Azure App Service](https://gist.github.com/glennc/e705cd85c9680d6a8f1bdb62099c7ac7)

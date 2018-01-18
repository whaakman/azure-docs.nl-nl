---
title: Web-apps configureren in Azure App Service
description: Het configureren van een web-app in Azure App Services
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 9af8a367-7d39-4399-9941-b80cbc5f39a0
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: cephalin
ms.openlocfilehash: 3f735b8742f22110b4de264bfb5661651a899afb
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="configure-web-apps-in-azure-app-service"></a>Web-apps configureren in Azure App Service

In dit onderwerp wordt uitgelegd hoe u een app met de [Azure Portal].

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="application-settings"></a>Toepassingsinstellingen
1. In de [Azure Portal], open de blade voor de web-app.
3. Klik op **Toepassingsinstellingen**.

![Toepassingsinstellingen][configure01]

De **toepassingsinstellingen** blade bevat instellingen die zijn gegroepeerd onder verschillende categorieën.

### <a name="general-settings"></a>Algemene instellingen
**Framework-versies**. Deze opties instellen als uw app gebruikmaakt van een deze frameworks: 

* **.NET framework**: Stel de versie van .NET framework. 
* **PHP**: de versie PHP instellen of **OFF** PHP uitschakelen. 
* **Java**: Selecteer de versie van Java of **OFF** Java uitschakelen. Gebruik de **webcontainer** kunt kiezen uit versies voor Tomcat en Jetty.
* **Python**: Selecteer de versie van Python of **OFF** Python uitschakelen.

Omwille van de technische Java inschakelen voor uw app .NET, PHP en Python worden de opties uitgeschakeld.

<a name="platform"></a>
**Platform**. Hiermee selecteert u of uw web-app wordt uitgevoerd in een 32-bits of 64-bits-omgeving. De 64-bits-omgeving vereist Basic- of Standard-modus. Gratis en gedeelde modi altijd uitgevoerd in een 32-bits-omgeving.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

**Web-Sockets**. Ingesteld **ON** zodat het WebSocket-protocol; bijvoorbeeld, als uw web-app gebruikt [ASP.NET SignalR] of [socket.io](https://socket.io/).

<a name="alwayson"></a>
**AlwaysOn**. Standaard worden web-apps uit het geheugen verwijderd als ze een bepaalde tijd inactief zijn. Hiermee wordt het systeem te besparen. In de modus voor Basic- of Standard, schakelt u **altijd op** te houden van de app geladen voortdurend. Als uw app doorlopende webtaken wordt uitgevoerd of wordt uitgevoerd WebJobs geactiveerd met behulp van een expressie CRON, moet u inschakelen **altijd op**, of de webtaken niet betrouwbaar worden uitgevoerd.

**Beheerde Pipeline-versie**. Hiermee stelt u de IIS [pipeline-modus]. Laat deze set geïntegreerde (standaard) tenzij u hebt een oudere app waarvoor een oudere versie van IIS is vereist.

**Automatisch wisselen**. Als u automatisch wisselen voor een implementatiesleuf inschakelt, wordt App Service automatisch de web-app wisselen naar de productie wanneer u een update push naar die site. Zie voor meer informatie [implementeren naar tijdelijke sleuven voor web-apps in Azure App Service](web-sites-staged-publishing.md).

### <a name="debugging"></a>Foutopsporing
**Foutopsporing op afstand**. Hiermee schakelt u foutopsporing op afstand. Wanneer dit is ingeschakeld, kunt u de externe foutopsporing in Visual Studio direct verbinding maken met uw web-app. Foutopsporing op afstand blijft ingeschakeld voor 48 uur. 

### <a name="app-settings"></a>App-instellingen
Deze sectie bevat de naam/waarde-paren die uw web-app wordt geladen op start. 

* Voor .NET-toepassingen, deze instellingen zijn opgenomen in de configuratie van uw .NET `AppSettings` tijdens runtime, overschrijven bestaande instellingen. 
* PHP, Python, Java en knooppunt toepassingen hebben toegang tot deze instellingen als omgevingsvariabelen tijdens runtime. Twee omgevingsvariabelen zijn gemaakt voor elke app-instelling. een met de naam die is opgegeven door de vermelding van de instelling app en andere met het voorvoegsel APPSETTING_. Beide bevatten dezelfde waarde.

### <a name="connection-strings"></a>Verbindingsreeksen
Tekenreeksen voor databaseverbindingen voor de gekoppelde resources. 

Voor .NET-toepassingen, deze verbindingsreeksen zijn opgenomen in de configuratie van uw .NET `connectionStrings` instellingen tijdens runtime, overschrijven bestaande vermeldingen waarbij de sleutel gelijk is aan de gekoppelde databasenaam. 

Deze instellingen zijn beschikbaar als omgevingsvariabelen tijdens runtime, voorafgegaan door het verbindingstype voor PHP, Python, knooppunt en Java-toepassingen. De omgeving variabele voorvoegsels zijn als volgt: 

* SQL Server: `SQLCONNSTR_`
* MySQL: `MYSQLCONNSTR_`
* SQL-Database:`SQLAZURECONNSTR_`
* Aangepaste:`CUSTOMCONNSTR_`

Bijvoorbeeld, als u een MySql-verbindingsreeks zijn met de naam `connectionstring1`, deze zou worden geopend via de omgevingsvariabele `MYSQLCONNSTR_connectionString1`.

### <a name="default-documents"></a>Standaarddocumenten
Het standaarddocument is de webpagina die in de basis-URL voor een website wordt weergegeven.  Het eerste gevonden bestand in de lijst wordt gebruikt. 

Modules dat route op basis van de URL in plaats voor statische inhoud, in welk geval er is geen standaarddocument zo mogelijk gebruik van web-apps.    

### <a name="handler-mappings"></a>Handlertoewijzingen
Gebruik dit gebied aangepast script processors verwerken van aanvragen voor specifieke bestandsextensies toevoegen. 

* **Extensie**. De bestandsextensie moet worden verwerkt, zoals *.php of handler.fcgi. 
* **Pad van de Processor script**. Het absolute pad van de ScriptProcessor. Aanvragen voor bestanden die overeenkomen met de bestandsextensie wordt verwerkt door de ScriptProcessor. Het pad `D:\home\site\wwwroot` om te verwijzen naar de hoofdmap van uw app.
* **Extra argumenten**. Optionele opdrachtregelargumenten voor de ScriptProcessor 

### <a name="virtual-applications-and-directories"></a>Virtuele toepassingen en mappen
Geef voor het configureren van virtuele toepassingen en mappen op elke virtuele map en de bijbehorende fysiek pad ten opzichte van hoofdmap van de website. Desgewenst kunt u de **toepassing** selectievakje markeren van een virtuele map als een toepassing.

## <a name="enabling-diagnostic-logs"></a>Logboeken met diagnostische gegevens inschakelen
Logboeken met diagnostische gegevens inschakelen:

1. Klik op de blade voor uw web-app **alle instellingen**.
2. Klik op **diagnostische logboeken**. 

Opties voor het schrijven van diagnostische logboeken vanuit een webtoepassing die ondersteuning biedt voor logboekregistratie: 

* **Toepassingslogboeken**. Toepassingslogboeken schrijft naar het bestandssysteem. Logboekregistratie duurt gedurende een periode van 12 uur. 

**Level**. Wanneer de toepassing-logboekregistratie is ingeschakeld, wordt deze optie geeft u de hoeveelheid gegevens die worden geregistreerd (fout, waarschuwing, informatie of uitgebreid).

**Logboekregistratie van webserver**. Logboeken worden opgeslagen in de indeling van het W3C-uitgebreide logboekbestand. 

**Gedetailleerde foutberichten**. Bespaart foutberichten gedetailleerde htm-bestanden. De bestanden worden opgeslagen onder /LogFiles/DetailedErrors. 

**Tracering van mislukte aanvragen**. Logboeken mislukte aanvragen met XML-bestanden. De bestanden worden opgeslagen onder/logboekbestanden/W3SVC*xxx*, waarbij xxx een unieke id. Deze map bevat een XSL-bestand en een of meer XML-bestanden. Zorg voor het downloaden van het XSL-bestand, omdat het programma functionaliteit biedt voor het formatteren en het filteren van de inhoud van de XML-bestanden.

Als u wilt weergeven van de logboekbestanden, moet u referenties van de FTP-, als volgt:

1. Klik op de blade voor uw web-app **alle instellingen**.
2. Klik op **implementatiereferenties**.
3. Voer een gebruikersnaam en wachtwoord.
4. Klik op **Opslaan**.

![Implementatiereferenties instellen][configure03]

De volledige naam van de FTP-gebruiker is 'app\username' waar *app* is de naam van uw web-app. De gebruikersnaam wordt vermeld in de blade web-app onder **Essentials**.

![FTP-implementatiereferenties][configure02]

## <a name="other-configuration-tasks"></a>Andere configuratietaken
### <a name="ssl"></a>SSL
U kunt SSL-certificaten voor een aangepast domein uploaden in de basis- of Standard-modus. Zie voor meer informatie [HTTPS inschakelen voor een web-app]. 

Als u wilt weergeven van uw geüploade certificaten **alle instellingen** > **aangepaste domeinen en SSL**.

### <a name="domain-names"></a>Domeinnamen
Aangepaste domeinnamen voor uw web-app toevoegen. Zie voor meer informatie [configureren voor een web-app in Azure App Service een aangepaste domeinnaam].

U kunt uw domeinnamen op **alle instellingen** > **aangepaste domeinen en SSL**.

### <a name="deployments"></a>Implementaties
* Doorlopende implementatie instellen. Zie [Git Web-Apps in Azure App Service implementeren met behulp van](app-service-deploy-local-git.md).
* Implementatiesites. Zie [implementeren op Faseringsomgevingen voor Web-Apps in Azure App Service].

Als u wilt weergeven van uw implementatiesites **alle instellingen** > **implementatiesites**.

### <a name="monitoring"></a>Bewaking
In de Basic- of Standard-modus, kunt u de beschikbaarheid van HTTP of HTTPS-eindpunten, van maximaal drie locaties geografisch verspreide testen. Een bewakingstest mislukt als de HTTP-antwoordcode een fout (4xx of 5xx is) of het antwoord langer dan 30 seconden duurt. Een eindpunt wordt aangemerkt als beschikbaar als de bewakingstests van de opgegeven locaties slagen. 

Zie voor meer informatie [hoe: website-eindpunt Monitorstatus].

> [!NOTE]
> Als u aan de slag wilt met Azure App Service voordat u zich aanmeldt voor een Azure-account, gaat u naar [App Service uitproberen]. Hier kunt u direct een tijdelijke web-app maken in App Service. U hebt geen creditcard nodig en u gaat geen verplichtingen aan.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* [Een aangepaste domeinnaam configureren in Azure App Service]
* [HTTPS inschakelen voor een app in Azure App Service]
* [Een web-app schalen in Azure App Service]
* [Bewaking van de basisprincipes voor Web-Apps in Azure App Service]

<!-- URL List -->

[ASP.NET SignalR]: http://www.asp.net/signalr
[Azure Portal]: https://portal.azure.com/
[Een aangepaste domeinnaam configureren in Azure App Service]: ./app-service-web-tutorial-custom-domain.md
[implementeren op Faseringsomgevingen voor Web-Apps in Azure App Service]: ./web-sites-staged-publishing.md
[HTTPS inschakelen voor een app in Azure App Service]: ./app-service-web-tutorial-custom-ssl.md
[hoe: website-eindpunt Monitorstatus]: http://go.microsoft.com/fwLink/?LinkID=279906
[Bewaking van de basisprincipes voor Web-Apps in Azure App Service]: ./web-sites-monitor.md
[pipeline-modus]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Een web-app schalen in Azure App Service]: ./web-sites-scale.md
[App Service uitproberen]: https://azure.microsoft.com/try/app-service/

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png

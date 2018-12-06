---
title: Web-apps configureren in Azure App Service
description: Configureren van een web-app in Azure App Services
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
ms.assetid: 9af8a367-7d39-4399-9941-b80cbc5f39a0
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: cephalin
ms.openlocfilehash: 60675d7d40dbab863677ea120c25986cff61a308
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52960138"
---
# <a name="configure-web-apps-in-azure-app-service"></a>Web-apps configureren in Azure App Service

In dit onderwerp wordt uitgelegd hoe u het configureren van een web-app via de [Azure Portal].

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="application-settings"></a>Toepassingsinstellingen
1. In de [Azure Portal], open de blade voor de web-app.
3. Klik op **Toepassingsinstellingen**.

![Toepassingsinstellingen][configure01]

De **toepassingsinstellingen** blade bevat instellingen die zijn gegroepeerd onder verschillende categorieën.

### <a name="general-settings"></a>Algemene instellingen
**Frameworkversies**. Schakel deze optie als uw app gebruikmaakt van een deze frameworks: 

* **.NET framework**: de .NET framework-versie instellen. 
* **PHP**: de PHP-versie instellen of **OFF** om uit te schakelen van PHP. 
* **Java**: Selecteer de Java-versie of **OFF** om uit te schakelen van Java. Gebruik de **webcontainer** kiezen uit versies voor Tomcat en Jetty.
* **Python**: Selecteer de Python-versie, of **OFF** om uit te schakelen van Python.

Omwille van de technische schakelt Java voor uw app u de opties voor .NET, PHP en Python.

<a name="platform"></a>
**Platform**. Hiermee selecteert u of uw web-app wordt uitgevoerd in een 32-bits of 64-bits-omgeving. De 64-bits-omgeving vereist Basic of Standard-laag. Gratis en gedeelde laag wordt altijd uitgevoerd in een 32-bits-omgeving.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

**Web-Sockets**. Stel **ON** om in te schakelen van de WebSocket-protocol; bijvoorbeeld, als uw web-app maakt gebruik van [ASP.NET SignalR] of [socket.io](https://socket.io/).

<a name="alwayson"></a>
**Always On**. Standaard worden web-apps uit het geheugen verwijderd als ze een bepaalde tijd niet actief zijn. Hiermee wordt het systeem te besparen. In de Basic- of Standard-modus, kunt u inschakelen **Always On** om te voorkomen dat de app geladen voortdurend. Als uw app wordt uitgevoerd voor doorlopende webtaken of uitvoeringen WebJobs geactiveerd met behulp van een CRON-expressie, moet u inschakelen **Always On**, of de webtaken mogelijk niet betrouwbaar worden uitgevoerd.

**Beheerde Pipeline-versie**. Hiermee stelt u de IIS [pipeline-modus]. Laat deze geïntegreerde (standaard), tenzij u hebt een verouderde app waarvoor een oudere versie van IIS.

**HTTP-versie**. Ingesteld op **2.0** ondersteuning voor inschakelen [HTTPS/2](https://wikipedia.org/wiki/HTTP/2) protocol. 

> [!NOTE]
> De meeste moderne browsers ondersteunen HTTP/2-protocol via TLS, terwijl niet-versleuteld verkeer gaat door met HTTP/1.1. Om ervoor te zorgen dat de client browsers verbinding maken met uw app met HTTP/2, ofwel [kopen van een App Service Certificate](web-sites-purchase-ssl-web-site.md) voor het aangepaste domein van uw app of [binden van een certificaat van derden](app-service-web-tutorial-custom-ssl.md).

**ARR-affiniteit**. In een app die geschaald naar meerdere VM-instanties, ARR-affiniteit cookies garanderen dat de client wordt gerouteerd naar hetzelfde exemplaar voor de levensduur van de sessie. De prestaties van stateless toepassingen verbeteren, kunt u deze optie instelt op **uit**.   

**Automatische wisselen**. Als u automatisch wisselen voor een implementatiesite inschakelt, wordt App Service automatisch de web-app wisselen in productie wanneer u een update aan dat de sleuf pusht. Zie voor meer informatie, [implementeren in faseringssleuven voor web-apps in Azure App Service](web-sites-staged-publishing.md).

### <a name="debugging"></a>Foutopsporing
**Foutopsporing op afstand**. Foutopsporing op afstand inschakelen Wanneer dit is ingeschakeld, kunt u de externe foutopsporing in Visual Studio rechtstreeks verbinden met uw web-app. Foutopsporing op afstand blijft ingeschakeld gedurende 48 uur. 

### <a name="app-settings"></a>App-instellingen
In deze sectie bevat de naam/waarde-paren die uw web-app wordt geladen op het startscherm. 

* Voor .NET-apps, deze instellingen zijn opgenomen in de configuratie van uw .NET `AppSettings` tijdens runtime, overschrijven bestaande instellingen. 
* Voor App Service op Linux- of Web App for Containers, als u json-sleutelstructuur in uw naam geneste, zoals `ApplicationInsights:InstrumentationKey` moet u hebben `ApplicationInsights__InstrumentationKey` als de naam van de sleutel. Zo ziet u dat elke `:` moet worden vervangen door `__` (dat wil zeggen dubbele onderstrepingstekens).
* PHP, Python, Java, Node en toepassingen hebben toegang tot deze instellingen als omgevingsvariabelen tijdens runtime. Twee omgevingsvariabelen zijn gemaakt voor elke appinstelling. een met de naam die is opgegeven door de vermelding van de instelling app, en een andere met het voorvoegsel van APPSETTING_. Beide bevatten dezelfde waarde.

App-instellingen worden altijd versleuteld wanneer die zijn opgeslagen (versleuteld in rust).

Appinstellingen kunnen worden omgezet vanuit Key Vault met [Key Vault verwijst naar](app-service-key-vault-references.md).

### <a name="connection-strings"></a>Verbindingsreeksen
Verbindingsreeksen voor de gekoppelde resources. 

Voor .NET-apps, deze tekenreeksen voor databaseverbindingen zijn opgenomen in de configuratie van uw .NET `connectionStrings` instellingen tijdens runtime, bestaande vermeldingen van waar de sleutel is gelijk aan de naam van de gekoppelde database overschrijven. 

Voor PHP, Python, Java en knooppunt-toepassingen, worden deze instellingen zijn beschikbaar als omgevingsvariabelen tijdens runtime, voorafgegaan door het verbindingstype. De omgeving variabele voorvoegsels zijn als volgt: 

* SQL Server: `SQLCONNSTR_`
* MySQL: `MYSQLCONNSTR_`
* SQL Database: `SQLAZURECONNSTR_`
* Aangepast: `CUSTOMCONNSTR_`

Bijvoorbeeld, als een MySql-verbindingsreeks zijn met de naam `connectionstring1`, deze zou worden geopend via de omgevingsvariabele `MYSQLCONNSTR_connectionString1`.

Verbindingsreeksen worden altijd versleuteld wanneer die zijn opgeslagen (versleuteld in rust).

Tekenreeksen voor databaseverbindingen, kunnen worden opgelost vanuit Key Vault met [Key Vault verwijst naar](app-service-key-vault-references.md).

### <a name="default-documents"></a>Standaarddocumenten
Het standaarddocument is de webpagina die wordt weergegeven op de basis-URL voor een website.  De eerste overeenkomende bestand in de lijst wordt gebruikt. 

Modules die route op basis van URL, plaats leveren van statische inhoud, in welk geval er is geen standaarddocument als zodanig kunnen gebruikmaken van web-apps.    

### <a name="handler-mappings"></a>Handlertoewijzingen
Gebruik dit gebied toe te voegen processors aangepast script voor het afhandelen van aanvragen voor specifieke bestandsextensies. 

* **Extensie**. De bestandsextensie moet worden verwerkt, zoals *.php of handler.fcgi. 
* **Processorpad script**. Het absolute pad van de ScriptProcessor. Aanvragen voor bestanden die overeenkomen met de bestandsextensie wordt verwerkt door de ScriptProcessor. Het pad gebruiken `D:\home\site\wwwroot` om te verwijzen naar de hoofdmap van uw app.
* **Extra argumenten**. Optionele opdrachtregelargumenten voor de ScriptProcessor 

### <a name="virtual-applications-and-directories"></a>Virtuele toepassingen en mappen
Geef voor het configureren van virtuele toepassingen en mappen, op elke virtuele map en de bijbehorende fysieke pad ten opzichte van hoofdmap van de website. Desgewenst kunt u de **toepassing** selectievakje in om u te markeren van een virtuele map als een toepassing.

## <a name="enabling-diagnostic-logs"></a>Logboeken met diagnostische gegevens inschakelen
Logboeken met diagnostische gegevens inschakelen:

1. Klik op de blade voor uw web-app **alle instellingen**.
2. Klik op **Diagnostische logboeken**. 

Opties voor het schrijven van logboeken met diagnostische gegevens vanuit een webtoepassing die ondersteuning biedt voor logboekregistratie: 

* **Logboekregistratie van toepassingen**. Schrijft de toepassingslogboeken naar het bestandssysteem. Logboekregistratie is gedurende een periode van 12 uur. 

**Niveau**. Als logboekregistratie van toepassingen is ingeschakeld, wordt deze optie geeft u de hoeveelheid gegevens die worden geregistreerd (fout, waarschuwing, informatie of uitgebreid).

**Webserverlogboeken**. Logboeken worden opgeslagen in de uitgebreide indeling W3C. 

**Gedetailleerde foutberichten**. Slaat foutberichten gedetailleerde htm-bestanden. De bestanden worden opgeslagen onder /LogFiles/DetailedErrors. 

**Tracering van mislukte aanvragen**. Logboeken voor mislukte aanvragen met XML-bestanden. De bestanden worden opgeslagen onder/logboekbestanden/W3SVC*xxx*, waarbij xxx een unieke id. Deze map bevat een XSL-bestand en een of meer XML-bestanden. Zorg ervoor dat u de XSL-bestand downloaden omdat het programma functionaliteit biedt voor het formatteren en filteren van de inhoud van de XML-bestanden.

Als u de logboekbestanden, moet u FTP-referenties, als volgt maken:

1. Klik op de blade voor uw web-app **alle instellingen**.
2. Klik op **implementatiereferenties**.
3. Voer een gebruikersnaam en wachtwoord.
4. Klik op **Opslaan**.

![Implementatiereferenties instellen][configure03]

De volledige naam van de FTP-gebruiker is "app\username" waar *app* is de naam van uw web-app. De gebruikersnaam wordt vermeld in de blade web-app onder **Essentials**.

![FTP-implementatiereferenties][configure02]

## <a name="other-configuration-tasks"></a>Andere configuratietaken
### <a name="ssl"></a>SSL
In de Basic- of Standard-modus, kunt u SSL-certificaten voor een aangepast domein uploaden. Zie voor meer informatie, [HTTPS inschakelen voor een web-app](app-service-web-tutorial-custom-ssl.md). 

Klik op om uw geüploade certificaten **alle instellingen** > **aangepaste domeinen en SSL**.

### <a name="domain-names"></a>Domeinnamen
Aangepaste domeinnamen voor uw web-app toevoegen. Zie voor meer informatie, [configureren van een aangepaste domeinnaam voor een web-app in Azure App Service](app-service-web-tutorial-custom-domain.md).

Als u uw domeinnamen, klikt u op **alle instellingen** > **aangepaste domeinen en SSL**.

### <a name="deployments"></a>Implementaties
* Continue implementatie instellen. Zie [Git Web-Apps in Azure App Service implementeren met behulp van](app-service-deploy-local-git.md).
* Implementatiesites gebruiken. Zie [Implementeren naar Faseringsomgevingen voor Web-Apps in Azure App Service].

Als u wilt uw implementatiesites weergeven, klikt u op **alle instellingen** > **implementatiesites**.

### <a name="monitoring"></a>Bewaking
In de Basic- of Standard-modus, kunt u de beschikbaarheid van HTTP of HTTPS-eindpunten, van maximaal drie geografisch gedistribueerde locaties testen. Een controle test mislukt als de HTTP-responscode een fout (4xx of 5xx is) of het antwoord meer dan 30 seconden heeft. Een eindpunt wordt aangemerkt als beschikbaar als de bewakingstests van de opgegeven locaties slagen. 

Zie voor meer informatie, [hoe: web-eindpunt controleren].

> [!NOTE]
> Als u aan de slag wilt met Azure App Service voordat u zich aanmeldt voor een Azure-account, gaat u naar [App Service uitproberen]. Hier kunt u direct een tijdelijke web-app maken in App Service. U hebt geen creditcard nodig en u gaat geen verplichtingen aan.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* [Een aangepaste domeinnaam configureren in Azure App Service]
* [HTTPS inschakelen voor een app in Azure App Service]
* [Een web-app schalen in Azure App Service]
* [Basisbeginselen van controle voor Web-Apps in Azure App Service]

<!-- URL List -->

[ASP.NET SignalR]: https://www.asp.net/signalr
[Azure Portal]: https://portal.azure.com/
[Een aangepaste domeinnaam configureren in Azure App Service]: ./app-service-web-tutorial-custom-domain.md
[Implementeren naar Faseringsomgevingen voor Web-Apps in Azure App Service]: ./web-sites-staged-publishing.md
[HTTPS inschakelen voor een app in Azure App Service]: ./app-service-web-tutorial-custom-ssl.md
[Hoe: web-eindpunt controleren]: https://go.microsoft.com/fwLink/?LinkID=279906
[Basisbeginselen van controle voor Web-Apps in Azure App Service]: ./web-sites-monitor.md
[pipeline-modus]: https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Een web-app schalen in Azure App Service]: ./web-sites-scale.md
[App Service uitproberen]: https://azure.microsoft.com/try/app-service/

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png

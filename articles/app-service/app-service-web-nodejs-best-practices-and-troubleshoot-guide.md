---
title: Aanbevolen procedures en probleemoplossing voor Node.js - Azure App Service
description: Meer informatie over de aanbevolen procedures en de stappen voor probleemoplossing voor knooppunttoepassingen in Azure App Service.
services: app-service\web
documentationcenter: nodejs
author: ranjithr
manager: wadeh
editor: ''
ms.assetid: 387ea217-7910-4468-8987-9a1022a99bef
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 11/09/2017
ms.author: ranjithr
ms.custom: seodec18
ms.openlocfilehash: 6a22600f7d6653da64256d7ec63ba26f272b2184
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56888267"
---
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-app-service-windows"></a>Aanbevolen procedures en gids voor probleemoplossing voor knooppunttoepassingen in Azure App Service-Windows

In dit artikel leert u aanbevolen procedures en stappen voor probleemoplossing voor [knooppunttoepassingen](app-service-web-get-started-nodejs.md) in Azure App Service (met [iisnode](https://github.com/azure/iisnode)).

> [!WARNING]
> Wees voorzichtig bij het gebruik van de stappen voor probleemoplossing in de productiesite. De aanbeveling is het oplossen van uw app op een niet-productie-installatie bijvoorbeeld de staging-site en als het probleem is opgelost, wisselen de staging-site met de productiesite.
>

## <a name="iisnode-configuration"></a>IISNODE-configuratie

Dit [schemabestand](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) ziet u alle instellingen die u voor iisnode configureren kunt. Enkele van de instellingen die nuttig voor uw toepassing zijn:

### <a name="nodeprocesscountperapplication"></a>nodeProcessCountPerApplication

Deze instelling bepaalt het aantal knooppunten processen die worden gestart per IIS-toepassing. De standaardwaarde is 1. U kunt zoveel node.exes starten als het aantal VM-vCPU's door een andere waarde op 0. De aanbevolen waarde is 0 voor de meeste toepassingen, zodat u alle van de vcpu's op uw computer gebruiken kunt. Node.exe is één thread, zodat één node.exe maximaal 1 vCPU verbruikt. Voor maximale prestaties van uw knooppunttoepassing, die u wilt alle vcpu's gebruiken.

### <a name="nodeprocesscommandline"></a>nodeProcessCommandLine

Deze instelling bepaalt het pad naar de node.exe. U kunt deze waarde om te verwijzen naar uw node.exe-versie instellen.

### <a name="maxconcurrentrequestsperprocess"></a>maxConcurrentRequestsPerProcess

Deze instelling bepaalt het maximum aantal gelijktijdige aanvragen door iisnode naar elke node.exe verzonden. In Azure App Service is de standaardwaarde oneindig. U kunt de waarde, afhankelijk van hoeveel aanvragen ontvangt van uw toepassing en hoe snel uw toepassing verwerkt elke aanvraag configureren.

### <a name="maxnamedpipeconnectionretry"></a>maxNamedPipeConnectionRetry

Deze instelling bepaalt u het maximum aantal keren dat het maken van de verbinding op de named pipe voor het verzenden van de aanvragen naar node.exe iisnode-pogingen. Deze instelling in combinatie met namedPipeConnectionRetryDelay bepaalt de totale time-out van elke aanvraag binnen iisnode. De standaardwaarde is 200 op Azure App Service. Totale time-out in seconden = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

### <a name="namedpipeconnectionretrydelay"></a>namedPipeConnectionRetryDelay

Deze instelling bepaalt de hoeveelheid tijd (in ms) iisnode wachttijd tussen nieuwe pogingen op de aanvraag verzenden naar node.exe via de named pipe. De standaardwaarde is 250 ms.
Totale time-out in seconden = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

De totale time-out in iisnode in Azure App Service is standaard 200 \* 250 ms = 50 seconden.

### <a name="logdirectory"></a>logDirectory

Deze instelling bepaalt u de map waar iisnode-stdout/stderr Logboeken. De standaardwaarde is iisnode is ten opzichte van de belangrijkste script-map (directory waarin de belangrijkste server.js zich bevindt)

### <a name="debuggerextensiondll"></a>debuggerExtensionDll

Deze instelling wordt bepaald welke versie van iisnode voor node-inspector gebruikt bij het opsporen van fouten in de node-toepassing. Iisnode-inspector-0.7.3.dll en iisnode-inspector.dll zijn momenteel de enige twee geldige waarden voor deze instelling. De standaardwaarde is iisnode-inspector-0.7.3.dll. De versie van de iisnode-inspector-0.7.3.dll gebruikt node-inspector-0.7.3 en maakt gebruik van websockets. Inschakelen van websockets op uw Azure Web-App voor gebruik van deze versie. Zie <https://ranjithblogs.azurewebsites.net/?p=98> voor meer informatie over het configureren van iisnode voor het gebruik van de nieuwe node-inspector.

### <a name="flushresponse"></a>flushResponse

Het standaardgedrag van IIS is dat deze antwoordgegevens buffert van 4 MB voordat of tot het einde van het antwoord, afhankelijk van wat het eerste komt. iisnode biedt een configuratie-instelling voor dit gedrag negeren: als u wilt een fragment van de antwoordtekst entiteit leegmaken als iisnode van node.exe ontvangt, moet u instellen de iisnode/@flushResponse kenmerk in web.config op 'true':

```xml
<configuration>
    <system.webServer>
        <!-- ... -->
        <iisnode flushResponse="true" />
    </system.webServer>
</configuration>
```

Schakel het leegmaken van elk fragment van het antwoord entiteitshoofdtekst prestatieoverhead die de doorvoer van het systeem ~ 5% (vanaf v0.1.13 vermindert) toegevoegd. Het beste als bereik voor deze instelling alleen voor eindpunten die antwoord streaming (bijvoorbeeld met behulp van de `<location>` -element in het bestand web.config)

Naast dit moet voor het streamen van toepassingen, u ook instellen responseBufferLimit van de iisnode-handler op 0.

```xml
<handlers>
    <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>
</handlers>
```

### <a name="watchedfiles"></a>watchedFiles

Een door puntkomma's gescheiden lijst met bestanden die worden gevolgd voor wijzigingen. Elke wijziging in een bestand zorgt ervoor dat de toepassing worden gerecycled. Elke vermelding bestaat uit een optionele mapnaam, evenals een vereist bestandsnaam, die zijn ten opzichte van de map waarin het toegangspunt van het hoofdvenster van de toepassing zich bevindt. Jokertekens zijn toegestaan in het bestand alleen naamgedeelte. De standaardwaarde is `*.js;iisnode.yml`

### <a name="recyclesignalenabled"></a>recycleSignalEnabled

De standaardwaarde is false. Bij inschakeling uw node-toepassing verbinding kan maken met een named pipe (omgevingsvariabele IISNODE\_BESTURINGSELEMENT\_PIPE) en verzend een bericht 'recyclen van'. Dit zorgt ervoor dat de w3wp worden herhaald zonder problemen.

### <a name="idlepageouttimeperiod"></a>idlePageOutTimePeriod

De standaardwaarde is 0, wat betekent dat deze functie is uitgeschakeld. Als de waarde naar een waarde groter dan 0, iisnode wordt de pagina uit alle onderliggende processen elke idlePageOutTimePeriod in milliseconden. Zie [documentatie](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx) om te begrijpen wat pagina out betekent. Deze instelling is handig voor toepassingen die een grote hoeveelheid geheugen in beslag nemen en pagina uit het geheugen op schijf af en toe om het RAM-geheugen vrij wilt.

> [!WARNING]
> Wees voorzichtig bij het inschakelen van de volgende configuratie-instellingen op de productie-Apps. De aanbeveling is het niet worden ingeschakeld op live productie-Apps.
>

### <a name="debugheaderenabled"></a>debugHeaderEnabled

De standaardwaarde is false. Als een HTTP-antwoordheader worden ingesteld op true, iisnode toegevoegd `iisnode-debug` aan elke HTTP-antwoord verzendt de `iisnode-debug` headerwaarde is een URL. Afzonderlijke stukjes diagnostische gegevens kunnen worden verkregen door te kijken naar de URL-fragment, maar een visualisatie is beschikbaar via de URL in een browser.

### <a name="loggingenabled"></a>loggingEnabled

Deze instelling bepaalt de logboekregistratie van stdout en stderr iisnode. Iisnode vastgelegd stdout/stderr van knooppunt processen deze wordt gestart en schrijft naar de map die is opgegeven in de instelling 'logDirectory'. Zodra dit is ingeschakeld, wordt uw toepassing schrijft de logboeken naar het bestandssysteem en afhankelijk van de hoeveelheid logboekregistratie uitgevoerd door de toepassing, kunnen er gevolgen voor de prestaties.

### <a name="deverrorsenabled"></a>devErrorsEnabled

De standaardwaarde is false. Ingesteld op true, iisnode weergegeven wanneer de HTTP-statuscode en de Win32-foutcode in uw browser. De win32-code is het handig zijn tijdens de foutopsporing van bepaalde typen problemen.

### <a name="debuggingenabled-do-not-enable-on-live-production-site"></a>debuggingEnabled (niet inschakelen op live-productiesite)

Deze instelling bepaalt u de functie voor foutopsporing. Iisnode is geïntegreerd met node-inspector. Als u deze instelling inschakelt, kunt u inschakelen foutopsporing van uw knooppunttoepassing. Bij deze instelling is ingeschakeld, maakt iisnode node-inspector bestanden in de map 'debuggerVirtualDir' op de eerste aanvraag voor foutopsporing voor uw knooppunttoepassing. U kunt de node-inspector laden door het verzenden van een aanvraag voor http://yoursite/server.js/debug. U kunt de URL-segment voor foutopsporing beheren met de instelling 'debuggerPathSegment'. Standaard debuggerPathSegment = 'debug'. U kunt instellen `debuggerPathSegment` naar een GUID, dus deze is bijvoorbeeld moeilijker te vinden zijn door anderen.

Lezen [fouten opsporen in node.js-toepassingen op Windows](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) voor meer informatie over het opsporen van fouten.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Scenario's en aanbevelingen/probleemoplossing

### <a name="my-node-application-is-making-excessive-outbound-calls"></a>Mijn node-toepassing maakt buitensporig aantal aanroepen van uitgaande

Veel toepassingen wilt uitgaande verbindingen maken als onderdeel van hun normale bewerking. Bijvoorbeeld, als een aanvraag binnenkomt, zou uw node-app willen contact op met een REST-API ergens anders en sommige informatie om de aanvraag te verwerken. U zou een keep alive-agent te gebruiken bij het aanroepen van http of https. U kunt de module agentkeepalive gebruiken als uw keep alive-agent bij het maken van deze uitgaande aanroepen.

De module agentkeepalive zorgt ervoor dat sockets worden hergebruikt voor uw Azure Web-App VM. Het maken van een nieuwe socket op elke uitgaande aanvraag wordt toegevoegd overhead aan uw toepassing. Uw toepassing opnieuw gebruiken van sockets voor uitgaande aanvragen, zorgt u ervoor dat uw toepassing niet groter is dan de maxSockets die zijn toegewezen per virtuele machine. De aanbeveling van Azure App Service is de waarde van de maxSockets agentKeepAlive instellen op een totaal van (4 exemplaren van node.exe \* 40 maxSockets/exemplaar) 160 sockets per virtuele machine.

Voorbeeld [agentKeepALive](https://www.npmjs.com/package/agentkeepalive) configuratie:

```nodejs
var keepaliveAgent = new Agent({
    maxSockets: 40,
    maxFreeSockets: 10,
    timeout: 60000,
    keepAliveTimeout: 300000
});
```

> [!IMPORTANT]
> In dit voorbeeld wordt ervan uitgegaan dat u hebt 4 node.exe die worden uitgevoerd op uw virtuele machine. Als u een ander aantal node.exe die worden uitgevoerd op de virtuele machine hebt, moet u de maxSockets instellen dienovereenkomstig wijzigen.
>

#### <a name="my-node-application-is-consuming-too-much-cpu"></a>Mijn node-toepassing is te veel CPU verbruikt

Ontvangt u mogelijk een aanbeveling van Azure App Service op de portal over hoog cpu-verbruik. U kunt ook monitors instellen om te bekijken voor bepaalde [metrische gegevens](web-sites-monitor.md). Tijdens het controleren van het CPU-gebruik op de [Azure Portal-Dashboard](../azure-monitor/app/web-monitor-performance.md), Controleer de MAX-waarden voor CPU, zodat u de piek-waarden niet mist.
Als u denkt dat uw toepassing worden te veel CPU verbruikt en u kunt geen reden, kunt u uw knooppunttoepassing om erachter te profileren.

#### <a name="profiling-your-node-application-on-azure-app-service-with-v8-profiler"></a>Profileren van uw node-toepassing in Azure App Service met V8-Profiler

Stel dat u hebt een hello world-app die u wilt profileren als volgt:

```nodejs
var http = require('http');
function WriteConsoleLog() {
    for(var i=0;i<99999;++i) {
        console.log('hello world');
    }
}

function HandleRequest() {
    WriteConsoleLog();
}

http.createServer(function (req, res) {
    res.writeHead(200, {'Content-Type': 'text/html'});
    HandleRequest();
    res.end('Hello world!');
}).listen(process.env.PORT);
```

Ga naar de Console voor foutopsporing https://yoursite.scm.azurewebsites.net/DebugConsole

Ga naar de map site/wwwroot. Zoals wordt weergegeven in het volgende voorbeeld ziet u een opdrachtprompt:

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

Voer de opdracht `npm install v8-profiler` uit.

Met deze opdracht installeert de v8-profiler onder het knooppunt\_modules map en alle bijbehorende afhankelijkheden.
Nu uw server.js wilt profileren van uw toepassing bewerken.

```nodejs
var http = require('http');
var profiler = require('v8-profiler');
var fs = require('fs');

function WriteConsoleLog() {
    for(var i=0;i<99999;++i) {
        console.log('hello world');
    }
}

function HandleRequest() {
    profiler.startProfiling('HandleRequest');
    WriteConsoleLog();
    fs.writeFileSync('profile.cpuprofile', JSON.stringify(profiler.stopProfiling('HandleRequest')));
}

http.createServer(function (req, res) {
    res.writeHead(200, {'Content-Type': 'text/html'});
    HandleRequest();
    res.end('Hello world!');
}).listen(process.env.PORT);
```

De bovenstaande code profielen de WriteConsoleLog werken en vervolgens de profiel-uitvoer wordt geschreven naar het bestand 'profile.cpuprofile' onder de wwwroot van uw site. Een aanvraag verzenden naar uw toepassing. U ziet een 'profile.cpuprofile'-bestand gemaakt op basis van uw site wwwroot.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Dit bestand downloaden en open het met Chrome F12-hulpprogramma's. Druk op F12 op Chrome en kies vervolgens de **profielen** tabblad. Kies de **Load** knop. Selecteer uw profile.cpuprofile-bestand dat u hebt gedownload. Klik op het profiel dat u zojuist hebt geladen.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

U kunt zien dat 95% van de tijd die is verbruikt door de functie WriteConsoleLog. De uitvoer ziet u ook de exacte regelnummers en de bronbestanden die de oorzaak van het probleem.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Mijn node-toepassing is te veel geheugen verbruikt

Als uw toepassing te veel geheugen verbruikt worden, ziet u een kennisgeving van Azure App Service op de portal over hoog geheugenverbruik. U kunt monitors instellen om te bekijken voor bepaalde [metrische gegevens](web-sites-monitor.md). Tijdens het controleren van het geheugengebruik op de [Azure Portal-Dashboard](../azure-monitor/app/web-monitor-performance.md), Controleer de MAX-waarden voor het geheugen, zodat u de piek-waarden niet mist.

#### <a name="leak-detection-and-heap-diff-for-nodejs"></a>Detectie- en Heap Diff lekken voor node.js

U kunt [knooppunt memwatch](https://github.com/lloyd/node-memwatch) om te herkennen geheugen lekt.
U kunt installeren `memwatch` net als de v8-profiler en bewerk uw code voor het vastleggen en diff-heaps om te bepalen het geheugen lekt in uw toepassing.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>Mijn node.exe-bestanden worden willekeurig ophalen afgesloten.

Er zijn enkele redenen waarom node.exe wordt afgesloten willekeurig:

1. Uw toepassing is dat niet-onderschepte uitzonderingen: selectievakje d:\\home\\logboekbestanden\\toepassing\\logboekregistratie errors.txt-bestand voor de details op de uitzondering veroorzaakt. Dit bestand heeft de stack-trace voor fouten opsporen en oplossen van uw toepassing.
2. Uw toepassing worden te veel geheugen, wat invloed heeft op andere processen uit aan de slag verbruikt. Als het totale geheugen van de virtuele machine bijna 100% is, kan uw node.exe-bestanden worden beëindigd door de process manager. Process manager beëindigt bepaalde processen zodat andere processen krijgen de kans om te werken. U kunt dit probleem oplossen, moet u uw toepassing voor geheugenlekken profileren. Als uw toepassing grote hoeveelheden geheugen vereist, kan worden uitgebreid naar een grotere virtuele machine (Hierdoor wordt het RAM-geheugen beschikbaar is voor de virtuele machine).

### <a name="my-node-application-does-not-start"></a>Mijn node-toepassing niet wordt gestart

Als uw toepassing 500-fouten retourneren is wanneer deze wordt gestart, kan er een aantal oorzaken hebben:

1. Node.exe is niet aanwezig op de juiste locatie. Controleer nodeProcessCommandLine instelling.
2. Belangrijkste scriptbestand is niet aanwezig op de juiste locatie. Controleer web.config en zorg ervoor dat de naam van de belangrijkste scriptbestand in de handlersectie overeenkomt met de belangrijkste scriptbestand.
3. Configuratie van web.config is niet correct – Controleer de instellingen voor namen/waarden.
4. Koude Start: uw toepassing te lang duurt om te starten. Als uw toepassing langer duurt dan (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000 seconden iisnode retourneert een 500-fout. Verhoog de waarden van deze instellingen zodat deze overeenkomen met de begintijd van uw toepassing om te voorkomen dat iisnode van een time-out opgetreden en de 500 fout geretourneerd.

### <a name="my-node-application-crashed"></a>Mijn node-toepassing is vastgelopen

Uw toepassing is dat niet-onderschepte uitzonderingen: selectievakje `d:\\home\\LogFiles\\Application\\logging-errors.txt` -bestand voor de details van de uitzondering veroorzaakt. Dit bestand heeft de stack-trace weergegeven om u te helpen vaststellen en oplossen van uw toepassing.

### <a name="my-node-application-takes-too-much-time-to-start-cold-start"></a>Mijn node-toepassing duurt te lang om te starten (koude Start)

De oorzaak van lange begintijden van toepassing is een groot aantal bestanden in het knooppunt\_modules. De toepassing probeert te laden van de meeste van deze bestanden bij het starten. Standaard, omdat de bestanden worden opgeslagen op de netwerkshare op Azure App Service, kan het laden van veel bestanden even duren.
Sommige oplossingen sneller om dit proces zijn:

1. Zorg ervoor dat u hebt een platte afhankelijkheidsstructuur en er zijn geen dubbele afhankelijkheden met behulp van npm3 om uw modules te installeren.
2. Probeer te langzaam laden van het knooppunt\_modules en alle modules aan begin van de toepassing niet worden geladen. Langzaam geladen modules, moet de aanroep van require('module') worden gemaakt wanneer u echt nodig hebt van de module binnen de functie voor de eerste uitvoering van code van de module.
3. Azure App Service biedt een functie met de naam van de lokale cache. Deze functie kopieert de inhoud van de netwerkshare naar de lokale schijf op de virtuele machine. Omdat de bestanden lokale, de laadtijd van knooppunt zijn\_modules is veel sneller.

## <a name="iisnode-http-status-and-substatus"></a>IISNODE http-status en substatus

De `cnodeconstants` [bronbestand](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) een lijst met alle van de mogelijke status/substatus combinaties iisnode vanwege een fout kunt terugkeren.

FREB voor uw toepassing om te zien van de win32-foutcode inschakelen (Zorg ervoor dat u FREB alleen op niet-productiesites Prestatieoverwegingen inschakelen).

| HTTP-status | HTTP-Substatus | Mogelijke reden? |
| --- | --- | --- |
| 500 |1000 |Er is een probleem opgetreden tijdens het verzenden van de aanvraag voor IISNODE – controleert als node.exe is gestart. Node.exe kan zijn vastgelopen bij het starten. Controleer de configuratie van uw web.config op fouten. |
| 500 |1001 |-Win32Error 0x2 - App reageert niet op de URL. Controleer de herschrijvingsregels voor URL's of het selectievakje als de express-app de juiste routes zijn gedefinieerd heeft. -Win32Error 0x6d – benoemde pipe is bezet – Node.exe geen aanvragen worden geaccepteerd omdat de pipe bezet is. Hoog cpu-gebruik controleren. -Andere fouten – controleren als node.exe is vastgelopen. |
| 500 |1002 |Node.exe is vastgelopen – d: controleren\\home\\logboekbestanden\\logboekregistratie-errors.txt voor stack-trace weergegeven. |
| 500 |1003 |Configuratie van de pipe probleem: de configuratie van de benoemde pipe is onjuist. |
| 500 |1004-1018 |Er is een fout tijdens het verzenden van de aanvraag of het antwoord van node.exe verwerken. Controleer of node.exe is vastgelopen. Controleer d:\\home\\logboekbestanden\\logboekregistratie-errors.txt voor stack-trace weergegeven. |
| 503 |1000 |Onvoldoende geheugen om toe te wijzen meer named pipe-verbindingen. Selectievakje waarom uw app zo veel geheugen is verbruikt. Controleer de waarde van de instelling maxConcurrentRequestsPerProcess. Als het is niet oneindig en hebt u veel aanvragen, verhoogt u deze waarde om te voorkomen dat deze fout. |
| 503 |1001 |Aanvraag kan niet worden verzonden naar node.exe omdat de toepassing wordt gerecycled. Nadat de toepassing is gerecycled, moeten normaal gesproken aanvragen worden bediend. |
| 503 |1002 |Selectievakje win32-foutcode voor de werkelijke reden: de aanvraag kan niet worden verzonden naar een node.exe. |
| 503 |1003 |Benoemde pipe is bezet: Controleer of als node.exe buitensporig CPU verbruikt |

NODE.exe is ingesteld met de naam `NODE_PENDING_PIPE_INSTANCES`. Deze waarde is ingesteld op Azure App Service, tot 5000. Wat betekent dat node.exe 5000 aanvragen kan accepteren op een tijdstip op de named pipe. Deze waarde moet goed genoeg is voor de meeste knooppunttoepassingen in Azure App Service. U moet 503.1003 niet zien in Azure App Service vanwege de hoge waarde voor de `NODE_PENDING_PIPE_INSTANCES`

## <a name="more-resources"></a>Meer bronnen

Volg deze koppelingen voor meer informatie over de node.js-toepassingen in Azure App Service.

* [Aan de slag met Node.js-web-apps in Azure App Service](app-service-web-get-started-nodejs.md)
* [Fouten opsporen in een Node.js web-app in Azure App Service](app-service-web-tutorial-nodejs-mongodb-app.md)
* [Node.js-modules gebruiken met Azure-toepassingen](../nodejs-use-node-modules-azure-apps.md)
* [Azure App Service Web Apps: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Node.js Developer Center](../nodejs-use-node-modules-azure-apps.md)
* [De geheimen van de Kudu-console voor foutopsporing](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)

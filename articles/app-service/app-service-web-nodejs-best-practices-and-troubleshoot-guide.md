---
title: Best practices en gids voor probleemoplossing voor knooppunt toepassingen op Azure-Web-Apps
description: Informatie over de aanbevolen procedures en stappen voor probleemoplossing voor knooppunt toepassingen op Azure-Web-Apps.
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
ms.openlocfilehash: 860874ed49056e6b4695c060b06bf061820c390e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>Best practices en gids voor probleemoplossing voor knooppunt toepassingen op Azure-Web-Apps

In dit artikel leert u de aanbevolen procedures en stappen voor probleemoplossing voor [knooppunt toepassingen](app-service-web-get-started-nodejs.md) uitgevoerd op Azure Web Apps (met [iisnode](https://github.com/azure/iisnode)).

> [!WARNING]
> Wees voorzichtig wanneer u stappen op uw productiesite. U wordt aangeraden voor het oplossen van uw app voor de installatie van een niet-productieve bijvoorbeeld uw faseringssleuf gewisseld wanneer het probleem wordt opgelost, de faseringssleuf met uw productiesite wisselen.
>

## <a name="iisnode-configuration"></a>IISNODE-configuratie

Dit [schemabestand](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) ziet u alle instellingen die u voor iisnode configureren kunt. Enkele van de instellingen die handig voor uw toepassing zijn:

### <a name="nodeprocesscountperapplication"></a>nodeProcessCountPerApplication

Deze instelling bepaalt het aantal knooppunt processen die per IIS-toepassing worden gestart. De standaardwaarde is 1. U kunt zoveel node.exes starten als het aantal VM-vCPU's door een andere waarde op 0. De aanbevolen waarde is 0 voor de meeste toepassingen zodat u alle van de vcpu's op uw computer kunt. Node.exe is één thread zodat één node.exe maximaal 1 vCPU verbruikt. Voor optimale prestaties buiten uw knooppunttoepassing, die u wilt alle vcpu's gebruiken.

### <a name="nodeprocesscommandline"></a>nodeProcessCommandLine

Deze instelling bepaalt het pad naar de node.exe. U kunt deze waarde om te verwijzen naar uw versie node.exe instellen.

### <a name="maxconcurrentrequestsperprocess"></a>maxConcurrentRequestsPerProcess

Deze instelling bepaalt het maximum aantal gelijktijdige aanvragen door iisnode naar elke node.exe verzonden. Op Azure-Web-Apps is de standaardwaarde oneindig. Wanneer deze wordt niet gehost op Azure-Web-Apps, is de standaardwaarde is 1024. U kunt de waarde afhankelijk van hoeveel aanvragen ontvangt van uw toepassing en hoe snel elke aanvraag wordt verwerkt door uw toepassing configureren.

### <a name="maxnamedpipeconnectionretry"></a>maxNamedPipeConnectionRetry

Deze instelling bepaalt u het maximum aantal keren dat het maken van de verbinding op de named pipe voor het versturen van aanvragen naar node.exe iisnode-pogingen. Deze instelling in combinatie met namedPipeConnectionRetryDelay bepaalt de totale time van elke aanvraag binnen iisnode. De standaardwaarde is 200 op Azure-Web-Apps. Totaal aantal time-out in seconden = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

### <a name="namedpipeconnectionretrydelay"></a>namedPipeConnectionRetryDelay

Deze instelling bepaalt de hoeveelheid tijd (in ms) iisnode wacht tussen nieuwe pogingen voor de aanvraag verzenden naar node.exe via de named pipe. De standaardwaarde is 250 ms.
Totaal aantal time-out in seconden = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

Standaard is de totale time-out in iisnode op Azure-Web-Apps 200 \* 250 ms = 50 seconden.

### <a name="logdirectory"></a>logDirectory

Deze instelling bepaalt de map waarin de stdout/stderr voor iisnode-logboekbestanden. De standaardwaarde is iisnode die ten opzichte van de belangrijkste script-map (directory waarin de belangrijkste server.js aanwezig is)

### <a name="debuggerextensiondll"></a>debuggerExtensionDll

Deze instelling bepaalt welke versie van de iisnode knooppunt-inspector gebruikt wanneer u fouten opspoort in uw knooppunttoepassing. Iisnode-inspector-0.7.3.dll en iisnode inspector.dll zijn momenteel de enige twee geldige waarden voor deze instelling. De standaardwaarde is iisnode-inspector-0.7.3.dll. De versie van de iisnode-inspector-0.7.3.dll knooppunt-inspector-0.7.3 gebruikt en maakt gebruik van websockets. Schakel websockets op uw Azure webapp voor gebruik van deze versie. Zie <http://ranjithblogs.azurewebsites.net/?p=98> voor meer informatie over het configureren van iisnode voor het gebruik van het nieuwe knooppunt-inspector.

### <a name="flushresponse"></a>flushResponse

Het standaardgedrag van IIS is dat deze antwoordgegevens buffert up tot 4 MB voordat of tot het einde van het antwoord, afhankelijk van wat zich het eerste voordoet. iisnode biedt een configuratie-instelling voor het onderdrukken van dit gedrag: een fragment van de entiteit antwoordtekst leeggemaakt zodra iisnode deze van node.exe ontvangt, moet u de iisnode/@flushResponse kenmerk in het bestand web.config op 'true':

```xml
<configuration>
    <system.webServer>
        <!-- ... -->
        <iisnode flushResponse="true" />
    </system.webServer>
</configuration>
```

Het leegmaken van inschakelen van elke fragment van het antwoord entiteitshoofdtekst beïnvloedt de systeemprestaties die de doorvoer van het systeem ~ 5% (vanaf v0.1.13 vermindert). Het beste als bereik voor deze instelling alleen voor eindpunten waarvoor antwoord streaming (bijvoorbeeld met behulp van de `<location>` -element in het bestand web.config)

Hiernaast moet voor streaming van toepassingen, u ook instellen responseBufferLimit van uw iisnode-handler op 0.

```xml
<handlers>
    <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>
</handlers>
```

### <a name="watchedfiles"></a>watchedFiles

Puntkomma's gescheiden lijst met bestanden die worden gecontroleerd op wijzigingen. Elke wijziging in een bestand zorgt ervoor dat de toepassing te recyclen. Elke vermelding bestaat uit een optionele mapnaam, evenals een vereiste bestandsnaam, zijn ten opzichte van de map waar het hoofdvenster van de toepassing toegangspunt dat zich bevindt. Jokertekens zijn toegestaan in het bestand alleen naamgedeelte. De standaardwaarde is `*.js;web.config`

### <a name="recyclesignalenabled"></a>recycleSignalEnabled

De standaardwaarde is ingesteld op false. Bij inschakeling uw knooppunttoepassing verbinding kan maken met een named pipe (omgevingsvariabele IISNODE\_BESTURINGSELEMENT\_PIPE) en een 'recyclen van'-bericht te verzenden. Dit zorgt ervoor dat het w3wp wilt recyclen probleemloos.

### <a name="idlepageouttimeperiod"></a>idlePageOutTimePeriod

De standaardwaarde is 0, wat betekent dat deze functie is uitgeschakeld. Als de waarde voor een bepaalde waarde groter dan 0, iisnode wordt de pagina uit alle onderliggende processen elke idlePageOutTimePeriod in milliseconden. Zie [documentatie](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx) om te begrijpen wat pagina out betekent. Deze instelling is nuttig voor toepassingen die een maximale hoeveelheid geheugen in beslag nemen en wilt pagina uit het geheugen op de schijf van tijd tot tijd om vrij te maken RAM-geheugen.

> [!WARNING]
> Wees voorzichtig bij het inschakelen van de volgende configuratie-instellingen voor productietoepassingen. De aanbeveling is niet worden ingeschakeld op live productietoepassingen.
>

### <a name="debugheaderenabled"></a>debugHeaderEnabled

De standaardwaarde is ingesteld op false. Als een HTTP-antwoordheader worden toegevoegd is ingesteld op true, iisnode `iisnode-debug` aan elke HTTP-antwoord verzendt de `iisnode-debug` -kopwaarde is een URL. Afzonderlijke stukjes diagnostische informatie kunnen worden verkregen door te kijken-fragment van de URL, een visualisatie is echter beschikbaar via de URL in een browser.

### <a name="loggingenabled"></a>loggingEnabled

Deze instelling bepaalt de logboekregistratie van stdout en stderr door iisnode. Iisnode vastgelegd stdout/stderr van knooppunt processen, het Start- en schrijfbewerkingen naar de map die is opgegeven in de instelling 'logDirectory'. Zodra dit is ingeschakeld, wordt uw toepassing schrijft de logboeken naar het bestandssysteem en afhankelijk van de hoeveelheid logboekregistratie gedaan door de toepassing, kan er prestatie van netwerkbestandsscenario.

### <a name="deverrorsenabled"></a>devErrorsEnabled

De standaardwaarde is ingesteld op false. Ingesteld op true, iisnode worden wanneer de HTTP-statuscode en de Win32-foutcode van uw browser. De win32-code kan handig zijn in bepaalde typen problemen foutopsporing.

### <a name="debuggingenabled-do-not-enable-on-live-production-site"></a>debuggingEnabled (niet inschakelen op live productiesite)

Deze instelling bepaalt de functie voor foutopsporing. Iisnode is geïntegreerd met node-inspector. Als u deze instelling inschakelt, kunt u inschakelen foutopsporing van uw knooppunttoepassing. Bij het inschakelen van deze instelling maakt iisnode knooppunt-inspector bestanden in de map 'debuggerVirtualDir' op de eerste aanvraag voor foutopsporing voor uw knooppunttoepassing. U kunt de knooppunt-inspector laden door het verzenden van een aanvraag naar http://yoursite/server.js/debug. U kunt het URL-segment foutopsporing beheren met de instelling 'debuggerPathSegment'. Standaard debuggerPathSegment = 'debug'. U kunt instellen `debuggerPathSegment` naar een GUID, bijvoorbeeld zodanig dat deze moeilijker is om te worden gedetecteerd door anderen.

Lees [fouten opsporen in node.js-toepassingen op Windows](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) voor meer informatie over foutopsporing.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Scenario's en aanbevelingen/probleemoplossing

### <a name="my-node-application-is-making-excessive-outbound-calls"></a>Mijn knooppunttoepassing brengen overmatige uitgaande oproepen

Veel toepassingen zou willen uitgaande verbindingen als onderdeel van hun normale werking. Als een aanvraag binnenkomt, wordt door uw app knooppunt wilt contact opnemen met een REST-API elders en sommige informatie aan het verzoek niet verwerken. U zou willen een keep alive-agent te gebruiken bij het aanroepen van http of https. U kunt de module agentkeepalive gebruiken als uw alive agent behouden bij het maken van deze uitgaande aanroepen.

De module agentkeepalive zorgt ervoor dat op uw Azure VM-webapp sockets worden opnieuw gebruikt. Maken van een nieuwe socket op elke uitgaande aanvraag voegt overhead toe aan uw toepassing. Uw toepassing opnieuw sockets voor uitgaande aanvragen wilt gebruiken, zorgt u ervoor dat uw toepassing niet meer dan de maxSockets die zijn toegewezen per VM. De aanbeveling op Azure-Web-Apps is de waarde van de maxSockets agentKeepAlive instellen op een totaal van (4 exemplaren van node.exe \* 40 maxSockets/exemplaar) 160 sockets per VM.

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
> In dit voorbeeld wordt ervan uitgegaan dat u hebt 4 node.exe uitgevoerd op de virtuele machine. Als u een verschillend aantal node.exe uitgevoerd op de virtuele machine hebt, moet u de maxSockets instelling dienovereenkomstig wijzigen.
>

#### <a name="my-node-application-is-consuming-too-much-cpu"></a>Mijn knooppunttoepassing is te veel CPU verbruikt

U kunt een aanbeveling van Azure Web Apps ontvangen op uw portal over hoog cpu-verbruik. U kunt monitors instellen voor bepaalde bekijkt [metrische gegevens](web-sites-monitor.md). Bij het controleren van het CPU-gebruik op de [Azure Portal-Dashboard](../application-insights/app-insights-web-monitor-performance.md), Controleer de MAX-waarden voor CPU zodat u de piek-waarden niet mist.
Als u denkt dat uw toepassing worden te veel CPU verbruikt en u kunt geen reden, kunt u uw knooppunttoepassing om erachter te komen profiel.

#### <a name="profiling-your-node-application-on-azure-web-apps-with-v8-profiler"></a>Profileren van uw knooppunttoepassing op Azure-Web-Apps met V8 Profiler

Stel dat u hebt een Hallo wereld-app die u wilt profiel als volgt:

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

Ga naar de site van de Console voor foutopsporing https://yoursite.scm.azurewebsites.net/DebugConsole

Ga naar de map van uw site/wwwroot. Zoals weergegeven in het volgende voorbeeld ziet u een opdrachtprompt:

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

Voer de opdracht `npm install v8-profiler`.

Deze opdracht wordt de v8-profiler onder het knooppunt geïnstalleerd\_modules directory en alle bijbehorende afhankelijkheden.
Nu uw server.js voor het profiel van uw toepassing bewerken.

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

De voorgaande code profielen de WriteConsoleLog functioneren en vervolgens de profiel-uitvoer wordt geschreven naar het bestand 'profile.cpuprofile' onder uw site wwwroot. Een aanvraag verzenden voor uw toepassing. Een 'profile.cpuprofile'-bestand gemaakt onder uw wwwroot site wordt weergegeven.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Dit bestand downloaden en openen met Chrome F12 hulpprogramma's. Druk op F12 op Chrome en kies vervolgens de **profielen** tabblad. Kies de **Load** knop. Selecteer uw profile.cpuprofile-bestand dat u hebt gedownload. Klik op het profiel dat u zojuist hebt geladen.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

U kunt zien dat 95% van de tijd die door de functie WriteConsoleLog is verbruikt. De uitvoer ziet u ook de exacte regelnummers en bronbestanden die het probleem veroorzaakt.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Mijn knooppunttoepassing is er te veel geheugen

Als uw toepassing is er te veel geheugen, ziet u een aankondiging van Azure Web Apps op uw portal over veel geheugen. U kunt monitors instellen voor bepaalde bekijkt [metrische gegevens](web-sites-monitor.md). Bij het controleren van het geheugengebruik op de [Azure Portal-Dashboard](../application-insights/app-insights-web-monitor-performance.md), Controleer de MAX-waarden voor het geheugen zodat u de piek-waarden niet mist.

#### <a name="leak-detection-and-heap-diff-for-nodejs"></a>Detectie- en Heap Diff lekken voor node.js

U kunt [knooppunt memwatch](https://github.com/lloyd/node-memwatch) voor hulp bij het identificeren van geheugen lekt.
U kunt installeren `memwatch` net als v8 profiler en bewerk uw code te worden vastgelegd en diff-heaps identificeren de geheugenlekken in uw toepassing.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>Mijn node.exe zijn willekeurig ophalen afgesloten.

Er zijn enkele redenen waarom node.exe wordt afgesloten willekeurig:

1. Uw toepassing die niet-onderschepte uitzonderingen – d: selectievakje\\thuis\\logboekbestanden\\toepassing\\logboekregistratie errors.txt-bestand op de uitzondering voor meer informatie. Dit bestand heeft de stack-trace om u te helpen opsporen en oplossen van uw toepassing.
2. Uw toepassing is er te veel geheugen, die van invloed op andere processen uit aan de slag. Als het totale geheugen van de VM dicht bij 100% is, kan uw node.exe worden beëindigd door de proces-manager. Proces manager is funest bepaalde processen zodat andere processen gaan werken. U kunt dit probleem oplossen door uw toepassing voor geheugenlekken-profiel. Als uw toepassing grote hoeveelheden geheugen vereist, kunt u opschalen naar een grotere virtuele machine (waardoor het RAM-geheugen beschikbaar is voor de virtuele machine).

### <a name="my-node-application-does-not-start"></a>Mijn knooppunttoepassing niet wordt gestart

Als uw toepassing 500 fouten retourneert wanneer deze wordt gestart, kan er een aantal oorzaken hebben:

1. Node.exe is niet aanwezig op de juiste locatie. Controleer de instelling nodeProcessCommandLine.
2. Belangrijkste scriptbestand is niet aanwezig op de juiste locatie. Controleer web.config en zorg ervoor dat de naam van de belangrijkste scriptbestand in de sectie-handlers overeenkomt met de belangrijkste scriptbestand.
3. Configuratie van web.config is niet correct: Controleer de instellingen van de namen en-waarden.
4. Koude Start – de toepassing te lang duurt om te starten. Als uw toepassing langer dan duurt (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000 seconden iisnode resulteert in een 500 fout. Verhoogt u de waarden van deze instellingen overeenkomen met de starttijd van de toepassing om te voorkomen dat iisnode van een time-out opgetreden en de 500 fout geretourneerd.

### <a name="my-node-application-crashed"></a>Mijn gecrasht knooppunttoepassing

Uw toepassing die niet-onderschepte uitzonderingen – selectievakje `d:\\home\\LogFiles\\Application\\logging-errors.txt` -bestand op de uitzondering voor meer informatie. Dit bestand heeft de stack-trace opsporen en oplossen van uw toepassing.

### <a name="my-node-application-takes-too-much-time-to-start-cold-start"></a>Mijn knooppunttoepassing duurt te lang om te starten (koude Start)

De voorkomende oorzaak voor lange begintijden van toepassing is een groot aantal bestanden in het knooppunt\_modules. De toepassing probeert te laden van de meeste van deze bestanden bij het starten. Standaard omdat uw bestanden worden opgeslagen op de netwerkshare op Azure-Web-Apps kan bij het laden van veel bestanden duren.
Er zijn een aantal oplossingen voor dit proces sneller maken:

1. Zorg ervoor dat u hebt een platte afhankelijkheidsstructuur en geen dubbele afhankelijkheden met behulp van npm3 uw modules installeren.
2. Probeer te langzaam laden uw knooppunt\_modules en alle modules aan begin van de toepassing niet worden geladen. Vertraagde load-modules, moet de aanroep van require('module') worden gemaakt wanneer u de module binnen de functie voordat de eerste uitvoering van code van de module moet.
3. Azure Web Apps biedt een functie met de naam van de lokale cache. Deze functie kopieert de inhoud van de netwerkshare naar de lokale schijf op de virtuele machine. Aangezien de bestanden lokale computer, de laadtijd van knooppunt zijn\_modules is veel sneller.

## <a name="iisnode-http-status-and-substatus"></a>IISNODE HTTP-status en substatus

De `cnodeconstants` [bronbestand](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) een lijst met alle van de mogelijke status/substatus combinaties iisnode vanwege een fout kunt terugkeren.

FREB voor uw toepassing om te zien van de win32-foutcode inschakelen (Zorg ervoor dat u FREB alleen op niet-productieve sites Prestatieoverwegingen inschakelen).

| HTTP-status | HTTP-substatuscode | Mogelijke reden? |
| --- | --- | --- |
| 500 |1000 |Er is een probleem tijdens het verzenden van de aanvraag voor IISNODE – controleert als node.exe is gestart. Node.exe kan zijn vastgelopen bij het starten. Controleer uw configuratie web.config op fouten. |
| 500 |1001 |-Win32Error 0x2 - App reageert niet op de URL. Controleer de regels voor het herschrijven van URL of het selectievakje als uw snelle app de juiste routes gedefinieerd heeft. -Win32Error 0x6d – benoemde pijp is bezet – Node.exe accepteert geen aanvragen omdat de pipe bezet is. Controleer hoog cpu-gebruik. -Andere fouten – controleren als node.exe gecrasht. |
| 500 |1002 |Node.exe gecrasht – d: controleren\\thuis\\logboekbestanden\\logboekregistratie-errors.txt voor stack-trace. |
| 500 |1003 |Pipe-configuratie probleem: de configuratie van de benoemde pipe is onjuist. |
| 500 |1004-1018 |Er is een fout tijdens het verzenden van de aanvraag of het antwoord van node.exe verwerken. Controleer of node.exe gecrasht. Controleer d:\\thuis\\logboekbestanden\\logboekregistratie-errors.txt voor stack-trace. |
| 503 |1000 |Niet voldoende geheugen toewijzen meer benoemde pipe-verbindingen. Controle van uw app waarom zo veel geheugen verbruikt. Controleer de waarde van de instelling maxConcurrentRequestsPerProcess. Als het is niet oneindig en er veel aanvragen, verhoogt u deze waarde om te voorkomen dat deze fout. |
| 503 |1001 |Aanvraag kan niet worden verzonden naar node.exe omdat het recyclen van de toepassing. Nadat de toepassing is gerecycled, moeten normaal gesproken aanvragen worden geleverd. |
| 503 |1002 |Controle van win32-foutcode voor de werkelijke reden: de aanvraag kan niet worden verzonden naar een node.exe. |
| 503 |1003 |Benoemde pipe is bezet – als node.exe buitensporig CPU verbruikt controleren |

NODE.exe heeft een instelling `NODE_PENDING_PIPE_INSTANCES`. Wanneer niet wordt geïmplementeerd op Azure-Web-Apps is deze waarde standaard 4. Dit betekent dat node.exe slechts vier aanvragen kan accepteren op een tijdstip op de named pipe. Deze waarde is ingesteld op Azure-Web-Apps tot 5000. Deze waarde moet voldoende voor de meeste knooppunt toepassingen uitgevoerd op Azure Web Apps. Niet ziet u 503.1003 op Azure-Web-Apps vanwege de hoge waarde voor de `NODE_PENDING_PIPE_INSTANCES`

## <a name="more-resources"></a>Meer bronnen

Volg deze koppelingen voor meer informatie over node.js-toepassingen in Azure App Service.

* [Aan de slag met Node.js-Web-Apps in Azure App Service](app-service-web-get-started-nodejs.md)
* [Fouten opsporen in een Node.js web-app in Azure App Service](app-service-web-tutorial-nodejs-mongodb-app.md)
* [Node.js-modules gebruiken met Azure-toepassingen](../nodejs-use-node-modules-azure-apps.md)
* [Web-apps van Azure App Service: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Node.js Developer Center](../nodejs-use-node-modules-azure-apps.md)
* [De geheimen van de Kudu-console voor foutopsporing](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)
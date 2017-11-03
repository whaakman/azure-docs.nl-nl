---
title: Best practices en gids voor probleemoplossing voor knooppunt toepassingen op Azure-Web-Apps
description: Informatie over de aanbevolen procedures en stappen voor probleemoplossing voor knooppunt toepassingen op Azure-Web-Apps.
services: app-service\web
documentationcenter: nodejs
author: ranjithr
manager: wadeh
editor: 
ms.assetid: 387ea217-7910-4468-8987-9a1022a99bef
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 06/06/2016
ms.author: ranjithr
ms.openlocfilehash: 8f39b5e6faf5f9121ec2abe347f50653c5c3e4f9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>Best practices en gids voor probleemoplossing voor knooppunt toepassingen op Azure-Web-Apps
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

In dit artikel leert u de aanbevolen procedures en de stappen voor probleemoplossing voor [knooppunt toepassingen](app-service-web-get-started-nodejs.md) uitgevoerd op Azure Webapps (met [iisnode](https://github.com/azure/iisnode)).

> [!WARNING]
> Wees voorzichtig wanneer u stappen op uw productiesite. U wordt aangeraden voor het oplossen van uw app voor de installatie van een niet-productieve bijvoorbeeld uw faseringssleuf gewisseld wanneer het probleem wordt opgelost, de faseringssleuf met uw productiesite wisselen.
> 
> 

## <a name="iisnode-configuration"></a>IISNODE-configuratie
Dit [schemabestand](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) ziet u de instellingen die kunnen worden geconfigureerd voor iisnode. Enkele van de instellingen die handig als uw toepassing zijn zijn:

* nodeProcessCountPerApplication
  
    Deze instelling bepaalt het aantal knooppunt processen die per IIS-toepassing worden gestart. Standaardwaarde is 1. U kunt zoveel node.exe als uw VM core-telling starten door in te stellen op 0. Aanbevolen waarde is 0 voor de meeste toepassing zodat u van alle van de kernen op uw computer gebruikmaken kunt. Node.exe is één thread, zodat één node.exe een maximum van 1 core en voor maximale prestaties buiten uw knooppunttoepassing die u wilt gebruikmaken van alle kernen verbruikt.
* nodeProcessCommandLine
  
    Deze instelling bepaalt het pad naar de node.exe. U kunt deze waarde om te verwijzen naar uw versie node.exe instellen.
* maxConcurrentRequestsPerProcess
  
    Deze instelling bepaalt het maximum aantal gelijktijdige aanvragen door iisnode naar elke node.exe verzonden. Op azure webapps is de standaardwaarde voor deze oneindig. U geen zorgen te hoeven maken over deze instelling. De standaardwaarde is buiten azure webapps 1024. Mogelijk wilt dit wilt configureren, afhankelijk van hoeveel aanvragen opgehaald voor uw toepassing en hoe snel elke aanvraag wordt verwerkt door uw toepassing.
* maxNamedPipeConnectionRetry
  
    Deze instelling bepaalt het maximum aantal keren iisnode aanbrengen verbinding op de named pipe in de aanvraag probeert via naar node.exe verzonden. Deze instelling in combinatie met namedPipeConnectionRetryDelay bepaalt de totale time van elke aanvraag binnen iisnode. Standaardwaarde is 200 op Azure Webapps. Totaal aantal time-out in seconden = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000
* namedPipeConnectionRetryDelay
  
    Deze instelling bepaalt u de hoeveelheid tijd (in ms) iisnode wacht tussen nieuwe pogingen aanvraag verzenden naar node.exe via de named pipe. Standaardwaarde is 250ms.
    Totaal aantal time-out in seconden = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000
  
    Standaard is de totale time-out in iisnode op azure webapps 200 \* 250ms = 50 seconden.
* logDirectory
  
    Deze instelling bepaalt de map waar iisnode stdout/stderr worden geregistreerd. Standaardwaarde is iisnode die ten opzichte van de belangrijkste script-map (directory waarin de belangrijkste server.js aanwezig is)
* debuggerExtensionDll
  
    Deze instelling bepaalt welke versie van de iisnode knooppunt-inspector wordt gebruikt als u fouten opspoort in uw knooppunttoepassing. Iisnode-inspector-0.7.3.dll en iisnode inspector.dll zijn momenteel alleen 2 geldige waarden voor deze instelling. Standaardwaarde is iisnode-inspector-0.7.3.dll. versie van de iisnode-inspector-0.7.3.dll knooppunt-inspector-0.7.3 en gebruikt websockets, dus u inschakelen websockets op uw azure webapp moet voor gebruik van deze versie. Zie <http://www.ranjithr.com/?p=98> voor meer informatie over het configureren van iisnode voor het gebruik van het nieuwe knooppunt-inspector.
* flushResponse
  
    Het standaardgedrag van IIS is dat deze antwoordgegevens buffert up tot 4MB voordat of tot het einde van het antwoord, afhankelijk van wat zich het eerste voordoet. iisnode biedt een configuratie-instelling voor het onderdrukken van dit gedrag: een fragment van de entiteit antwoordtekst leeggemaakt zodra iisnode deze van node.exe ontvangt, moet u de iisnode/@flushResponse kenmerk in het bestand web.config op 'true':
  
    ```
    <configuration>    
        <system.webServer>    
            <!-- ... -->    
            <iisnode flushResponse="true" />    
        </system.webServer>    
    </configuration>
    ```
  
    Inschakelen van domeincontrollerlocaties leeg van elke fragment van de entiteit antwoordtekst beïnvloedt de systeemprestaties die de doorvoer van het systeem vermindert ~ 5% (vanaf v0.1.13), is het verstandig als bereik voor deze instelling alleen voor eindpunten waarvoor antwoord streaming (bijvoorbeeld met de <location> -element in het bestand web.config)
  
    Hiernaast moet voor streaming van toepassingen, u ook responseBufferLimit van uw iisnode-handler ingesteld op 0.
  
    ```
    <handlers>    
        <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>    
    </handlers>
    ```
* watchedFiles
  
    Dit is een puntkomma's gescheiden lijst met bestanden die zal worden gecontroleerd op wijzigingen. Een wijziging in een bestand zorgt ervoor dat de toepassing te recyclen. Elke vermelding bestaat uit een optionele mapnaam plus de vereiste bestandsnaam die zijn ten opzichte van de map waar het hoofdvenster van de toepassing toegangspunt dat zich bevindt. Jokertekens zijn toegestaan in het bestand alleen naamgedeelte. Standaardwaarde is "\*. js;web.config '
* recycleSignalEnabled
  
    Standaard is ingesteld op false. Bij inschakeling uw knooppunttoepassing verbinding kan maken met een named pipe (omgevingsvariabele IISNODE\_BESTURINGSELEMENT\_PIPE) en een 'recyclen van'-bericht te verzenden. Hierdoor wordt het w3wp wilt recyclen probleemloos.
* idlePageOutTimePeriod
  
    Standaardwaarde is 0, wat betekent dat deze functie is uitgeschakeld. Als de waarde voor een bepaalde waarde groter dan 0, iisnode wordt de pagina uit alle onderliggende processen elke milliseconden 'idlePageOutTimePeriod'. Om te begrijpen wat pagina out betekent, raadpleegt u dit [documentatie](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx). Deze instelling is nuttig voor toepassingen die veel geheugen in beslag nemen en pageout geheugen op schijf zo nu en dan wilt vrijmaken RAM-geheugen.

> [!WARNING]
> Wees voorzichtig bij het inschakelen van de volgende configuratie-instellingen voor productietoepassingen. Aanbevolen wordt deze niet inschakelen voor live productietoepassingen.
> 
> 

* debugHeaderEnabled
  
    De standaardwaarde is ingesteld op false. Als is ingesteld op true, iisnode een HTTP-antwoord header iisnode-debug toegevoegd aan elke HTTP-antwoord wordt verzonden de waarde van de iisnode-debug-header is een URL. Afzonderlijke stukjes diagnostische gegevens door te kijken-fragment van de URL kunnen worden achterhaald, maar een veel betere visualisatie wordt bereikt door het openen van de URL in de browser.
* loggingEnabled
  
    Deze instelling bepaalt de logboekregistratie van stdout en stderr door iisnode. Iisnode wordt stdout/stderr van knooppunt processen wordt het vastleggen en schrijven naar de map die is opgegeven in de instelling 'logDirectory'. Zodra dit is ingeschakeld, wordt uw toepassing wordt logboeken worden geschreven naar het bestandssysteem en afhankelijk van de hoeveelheid logboekregistratie gedaan door de toepassing, kan er prestatie van netwerkbestandsscenario.
* devErrorsEnabled
  
    Standaard is ingesteld op false. Wanneer ingesteld op true, iisnode de HTTP-statuscode en de Win32-foutcode op uw browser weergegeven wordt. De win32-code is handig bij de foutopsporing van bepaalde typen problemen.
* debuggingEnabled (niet inschakelen op live productiesite)
  
    Deze instelling bepaalt de functie voor foutopsporing. Iisnode is geïntegreerd met node-inspector. Als u deze instelling inschakelt, kunt u inschakelen foutopsporing van uw knooppunttoepassing. Zodra deze instelling is ingeschakeld, wordt iisnode indeling van de bestanden nodig knooppunt-inspector in 'debuggerVirtualDir' directory op de eerste aanvraag voor foutopsporing voor uw knooppunttoepassing. U kunt de knooppunt-inspector laden door het verzenden van een aanvraag naar http://yoursite/server.js/debug. U kunt het URL-segment foutopsporing beheren met de instelling 'debuggerPathSegment'. Door standaard debuggerPathSegment = 'debug'. U kunt dit instellen op een GUID bijvoorbeeld zodat het moeilijker is om te worden gedetecteerd door anderen.
  
    Dit selectievakje inschakelt [koppeling](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) voor meer informatie over foutopsporing.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Scenario's en aanbevelingen/probleemoplossing
### <a name="my-node-application-is-making-too-many-outbound-calls"></a>Mijn knooppunttoepassing brengen te veel uitgaande aanroepen.
Veel toepassingen zou willen uitgaande verbindingen als onderdeel van hun normale werking. Als een aanvraag binnenkomt, wordt door uw app knooppunt wilt contact opnemen met een REST-API elders en sommige informatie aan het verzoek niet verwerken. U zou willen een keep alive-agent te gebruiken bij het aanroepen van http of https. Bijvoorbeeld, kunt u de module agentkeepalive als uw alive agent behouden bij het maken van deze uitgaande aanroepen. Dit zorgt ervoor dat de sockets opnieuw worden gebruikt op uw azure VM-webapp, waardoor de overhead voor het maken van nieuwe sockets voor elke uitgaande aanvraag. Dit zorgt ook ervoor dat u kleiner aantal sockets uitgaande aanvragen verzenden en daarom u de maxSockets die zijn toegewezen per VM niet overschrijdt. Aanbeveling op Azure Webapps kan worden de agentKeepAlive maxSockets waarde instelt op een totaal van 160 sockets per VM. Dit betekent dat als u 4 node.exe uitgevoerd op de virtuele machine hebt, u wilt de maxSockets agentKeepAlive ingesteld op 40 per node.exe die 160 totaal per VM.

Voorbeeld [agentKeepALive](https://www.npmjs.com/package/agentkeepalive) configuratie:

```
var keepaliveAgent = new Agent({    
    maxSockets: 40,    
    maxFreeSockets: 10,    
    timeout: 60000,    
    keepAliveTimeout: 300000    
});
```

In dit voorbeeld wordt ervan uitgegaan dat u hebt 4 node.exe uitgevoerd op de virtuele machine. Als u een verschillend aantal node.exe uitgevoerd op de virtuele machine hebt, moet u de maxSockets instelling dienovereenkomstig wijzigen.

### <a name="my-node-application-is-consuming-too-much-cpu"></a>Mijn knooppunttoepassing is te veel CPU verbruikt.
Mogelijk krijgt u een aanbeveling van Azure Webapps op uw portal over hoog cpu-verbruik. U kunt ook instellen dat monitors bekijkt voor bepaalde [metrische gegevens](web-sites-monitor.md). Bij het controleren van het CPU-gebruik op de [Azure Portal-Dashboard](../application-insights/app-insights-web-monitor-performance.md), Controleer de MAX-waarden voor CPU zodat u niet de piek-waarden vergeten.
In gevallen waarin u denkt dat uw toepassing worden te veel CPU verbruikt en u kunt geen reden, moet u uw knooppunttoepassing profiel.

### 
#### <a name="profiling-your-node-application-on-azure-webapps-with-v8-profiler"></a>Profileren van uw knooppunttoepassing op azure webapps met V8 Profiler
Bijvoorbeeld, kunt Stel dat u hebt een Hallo wereld-app die u profiel wilt zoals hieronder wordt weergegeven:

```
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

Ga naar uw site scm https://yoursite.scm.azurewebsites.net/DebugConsole

U ziet een opdrachtprompt zoals hieronder wordt weergegeven. Ga naar de map van uw site/wwwroot

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

Voer de opdracht 'npm installeren v8 profiler'

Dit moet v8-profiler onder het knooppunt installeren\_modules directory en alle bijbehorende afhankelijkheden.
Nu uw server.js voor het profiel van uw toepassing bewerken.

```
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

De bovenstaande wijzigingen wordt de functie WriteConsoleLog profiel en vervolgens de profiel-uitvoer schrijven naar bestand onder uw site wwwroot 'profile.cpuprofile'. Een aanvraag verzenden voor uw toepassing. Hier ziet u een 'profile.cpuprofile'-bestand gemaakt voor uw site wwwroot.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Dit bestand downloaden en u moet dit bestand te openen met Chrome F12 hulpprogramma's. Klik op F12 op chrome en klik vervolgens op het tabblad' profielen'. Klik op de knop 'Laden'. Selecteer uw profile.cpuprofile-bestand dat u zojuist hebt gedownload. Klik op het profiel dat u zojuist hebt geladen.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

U ziet dat 95% van de tijd is verbruikt door WriteConsoleLog functie, zoals hieronder wordt weergegeven. Dit ziet u ook de exacte regelnummers en bronbestanden die ertoe leiden het probleem dat.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Mijn knooppunttoepassing is er te veel geheugen.
Mogelijk krijgt u een aanbeveling van Azure Webapps op uw portal over veel geheugen. U kunt ook instellen dat monitors bekijkt voor bepaalde [metrische gegevens](web-sites-monitor.md). Bij het controleren van het geheugengebruik op de [Azure Portal-Dashboard](../application-insights/app-insights-web-monitor-performance.md), Controleer de MAX-waarden voor het geheugen zodat u niet de piek-waarden vergeten.

#### <a name="leak-detection-and-heap-diffing-for-nodejs"></a>Detectie- en Heap vergelijken lekken voor node.js
U kunt [knooppunt memwatch](https://github.com/lloyd/node-memwatch) voor hulp bij het identificeren van geheugen lekt.
U kunt memwatch net als v8 profiler installeren en bewerken van uw code te worden vastgelegd en diff heaps voor het identificeren van het geheugen lekt in uw toepassing.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>Mijn node.exe zijn willekeurig ophalen afgesloten.
Er zijn enkele redenen waarom u dit kan gebeuren:

1. Uw toepassing die niet-onderschepte uitzonderingen – Voer selectievakje d:\\thuis\\logboekbestanden\\toepassing\\logboekregistratie errors.txt-bestand op de uitzondering voor meer informatie. Dit bestand heeft de stack-trace zodat u uw toepassing op basis hiervan kunt oplossen.
2. Uw toepassing is er te veel geheugen die van invloed op andere processen uit aan de slag. Als het totale geheugen van de VM dicht bij 100% is, kan uw node.exe worden beëindigd door het proces manager zodat andere processen gaan werken. U kunt dit verhelpen, zorg ervoor dat uw toepassing geheugen lekt niet. of als uw toepassing heel veel geheugen gebruiken moet, geef opschalen naar een grotere virtuele machine met veel meer RAM-geheugen.

### <a name="my-node-application-does-not-start"></a>Mijn knooppunttoepassing niet wordt gestart
Als uw toepassing als 500 fouten bij het opstarten resultaat, kan er een aantal oorzaken hebben:

1. Node.exe is niet aanwezig op de juiste locatie. Controleer de instelling nodeProcessCommandLine.
2. Belangrijkste scriptbestand is niet aanwezig op de juiste locatie. Controleer web.config en zorg ervoor dat de naam van de belangrijkste scriptbestand in de sectie-handlers overeenkomt met de belangrijkste scriptbestand.
3. Configuratie van web.config is niet correct: Controleer de instellingen van de namen en-waarden.
4. Koude Start – de toepassing is te lang duurt worden gestart. Als uw toepassing langer dan duurt (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000 seconden iisnode 500 fout retourneert. Verhoogt u de waarden van deze instellingen overeenkomen met de starttijd van de toepassing om te voorkomen dat iisnode van een time-out opgetreden en de 500 fout geretourneerd.

### <a name="my-node-application-crashed"></a>Mijn gecrasht knooppunttoepassing
Uw toepassing die niet-onderschepte uitzonderingen – Voer selectievakje d:\\thuis\\logboekbestanden\\toepassing\\logboekregistratie errors.txt-bestand op de uitzondering voor meer informatie. Dit bestand heeft de stack-trace zodat u uw toepassing op basis hiervan kunt oplossen.

### <a name="my-node-application-takes-too-much-time-to-startup-cold-start"></a>Mijn knooppunttoepassing duurt te lang om te starten (koude Start)
Meest voorkomende reden hiervoor is dat de toepassing een groot aantal bestanden in het knooppunt heeft\_modules en de toepassing probeert te laden van de meeste van deze bestanden tijdens het opstarten. Standaard omdat uw bestanden bevinden zich op de netwerkshare op Azure-Webapps kan laden zo veel bestanden enige tijd duren.
Sommige oplossingen om dit te sneller zijn:

1. Zorg ervoor dat u hebt een platte afhankelijkheidsstructuur en geen dubbele afhankelijkheden met behulp van npm3 uw modules installeren.
2. Probeer te langzaam laden uw knooppunt\_modules en alle modules in bij het opstarten niet worden geladen. Dit betekent dat de aanroep van require('module') wanneer u daadwerkelijk binnen de functie die u probeert moet te gebruiken van de module moet worden gedaan.
3. Azure Webapps biedt een functie met de naam van de lokale cache. Deze functie kopieert de inhoud van de netwerkshare naar de lokale schijf op de virtuele machine. Aangezien de bestanden lokale computer, de laadtijd van knooppunt zijn\_modules is veel sneller.

## <a name="iisnode-http-status-and-substatus"></a>IISNODE HTTP-status en substatus
Dit [bronbestand](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) geeft een lijst van alle de mogelijke status/substatus combinatie iisnode kan worden geretourneerd in geval van een fout.

FREB voor uw toepassing om te zien van de win32-foutcode inschakelen (Controleer of u FREB alleen op niet-productieve sites Prestatieoverwegingen inschakelen).

| HTTP-Status | HTTP-substatuscode | Mogelijke reden? |
| --- | --- | --- |
| 500 |1000 |Er is een probleem tijdens het verzenden van de aanvraag voor IISNODE – controleren als node.exe is gestart. Node.exe kan bij het opstarten zijn vastgelopen. Controleer uw configuratie web.config op fouten. |
| 500 |1001 |-Win32Error 0x2 - App reageert niet op de URL. Controleer de regels voor het herschrijven van URL of als uw app in express de juiste routes gedefinieerd heeft. -Win32Error 0x6d – benoemde pijp is bezet – Node.exe accepteert geen aanvragen omdat de pipe bezet is. Controleer hoog cpu-gebruik. -Andere fouten – controleren als node.exe gecrasht. |
| 500 |1002 |Node.exe gecrasht – d: controleren\\thuis\\logboekbestanden\\logboekregistratie-errors.txt voor stack-trace. |
| 500 |1003 |Pipe-configuratie probleem: U nooit ziet dit, maar als u dit doet, de configuratie van de benoemde pipe is onjuist. |
| 500 |1004-1018 |Er is een fout tijdens het verzenden van de aanvraag of het antwoord van node.exe verwerken. Controleer of node.exe gecrasht. Controleer d:\\thuis\\logboekbestanden\\logboekregistratie-errors.txt voor stack-trace. |
| 503 |1000 |Niet voldoende geheugen toewijzen meer benoemde pipe-verbindingen. Controle van uw app waarom zo veel geheugen verbruikt. Controleer de waarde van de instelling maxConcurrentRequestsPerProcess. Als het is niet oneindig en u hebt een groot aantal aanvragen, hier een hogere waarde om te voorkomen dat deze fout. |
| 503 |1001 |Aanvraag kan niet worden verzonden naar node.exe omdat het recyclen van de toepassing. Nadat de toepassing is gerecycled, moeten normaal gesproken aanvragen worden geleverd. |
| 503 |1002 |Controle van win32-foutcode voor de werkelijke reden: de aanvraag kan niet worden verzonden naar een node.exe. |
| 503 |1003 |Benoemde pipe is bezet – als knooppunt veel CPU verbruikt controleren |

Er is een instelling in het knooppunt aangeroepen NODE.exe\_in behandeling\_PIPE\_exemplaren. Deze waarde is standaard buiten azure webapps 4. Dit betekent dat node.exe accepteert alleen 4 aanvragen tegelijk op de named pipe. Deze waarde is ingesteld op 5000 op Azure Webapps, en deze waarde moet voldoende voor de meeste knooppunt toepassingen uitgevoerd op azure webapps. Zie niet 503.1003 op azure webapps omdat we een hoge waarde voor het knooppunt hebben\_in behandeling\_PIPE\_exemplaren.  |

## <a name="more-resources"></a>Meer bronnen
Volg deze koppelingen voor meer informatie over node.js-toepassingen in Azure App Service.

* [Aan de slag met Node.js-web-apps in Azure App Service](app-service-web-get-started-nodejs.md)
* [Fouten opsporen in een Node.js web-app in Azure App Service](app-service-web-tutorial-nodejs-mongodb-app.md)
* [Node.js-modules gebruiken met Azure-toepassingen](../nodejs-use-node-modules-azure-apps.md)
* [Web-apps van Azure App Service: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Node.js Developer Center](../nodejs-use-node-modules-azure-apps.md)
* [De geheimen van de Kudu-console voor foutopsporing](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)


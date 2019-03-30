---
title: Aangepaste statusrapporten van Service Fabric toevoegen | Microsoft Docs
description: Beschrijft hoe u aangepaste statusrapporten verzenden naar Azure Service Fabric health-entiteiten. Biedt aanbevelingen voor het ontwerpen en implementeren van statusrapporten kwaliteit.
services: service-fabric
documentationcenter: .net
author: oanapl
manager: chackdan
editor: ''
ms.assetid: 0a00a7d2-510e-47d0-8aa8-24c851ea847f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2018
ms.author: oanapl
ms.openlocfilehash: 06fedddffd51dc22b45e8ae6e415ad139346c5b6
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58670384"
---
# <a name="add-custom-service-fabric-health-reports"></a>Aangepaste statusrapporten van Service Fabric toevoegen
Azure Service Fabric introduceert een [statusmodel](service-fabric-health-introduction.md) ontworpen om u te markeren niet in orde cluster en de voorwaarden van toepassing op specifieke entiteiten. Maakt gebruik van het statusmodel **health rapporteurs** (onderdelen van het systeem en watchdogs). Het doel is eenvoudig en snel diagnose en herstel. Schrijvers van de service moeten om na te denken over de status van vooraf. Een voorwaarde die van invloed kan status moet worden gerapporteerd, met name als het de problemen met de vlag dicht bij de hoofdmap helpen kan. De statusinformatie bespaart tijd en moeite voor foutopsporing en onderzoek. Het nut is met name wissen nadat de service actief en werkend op schaal in de cloud is (particulier of Azure).

De Service Fabric-rapporteurs monitor geïdentificeerd voorwaarden van belang zijn. Ze een rapport over deze voorwaarden op basis van hun lokale weergave. De [health store](service-fabric-health-introduction.md#health-store) health-gegevens die door alle rapporteurs wordt verzonden om te bepalen of entiteiten wereldwijd in orde zijn. Het model is bedoeld als uitgebreide, flexibele en eenvoudig te gebruiken. De kwaliteit van de statusrapporten bepaalt de nauwkeurigheid van de weergave van de status van het cluster. Fout-positieven waarin ten onrechte niet in orde problemen kunnen negatieve invloed hebben op upgrades of andere services die gebruikmaken van health-gegevens. Voorbeelden van dergelijke services zijn reparatie en waarschuwingsmechanisme. Daarom zetten is dat nodig is voor rapporten waarin de voorwaarden van belang zijn in de beste manier worden vastgelegd.

Voor het ontwerpen en implementeren van de gezondheid van rapportage, watchdogs en onderdelen van het systeem moeten:

* Definieer de voorwaarde dat ze geïnteresseerd zijn, de manier waarop die deze wordt bewaakt en de impact op de functionaliteit van het cluster of de toepassing. Besluit op basis van deze gegevens, op de eigenschap van de gezondheid van het rapport en de status.
* Bepaal de [entiteit](service-fabric-health-introduction.md#health-entities-and-hierarchy) die het rapport is van toepassing op.
* Bepalen waar de rapportage is gereed, van de service of vanuit een watchdog intern of extern.
* Een bron die wordt gebruikt voor het identificeren van de journalist definiëren.
* Kies een strategie voor rapportage, periodiek of op overgangen. De aanbevolen manier is periodiek, omdat deze eenvoudiger code vereist en minder gevoelig voor fouten is.
* Bepalen hoe lang het rapport voor niet in orde voorwaarden moet blijven in het statusarchief en hoe deze moet worden gewist. Met deze informatie kunt besluiten time to live van het rapport en het gedrag van de remove-op-vervaldatum.

Zoals gezegd, rapportage kunt u doen vanaf:

* De bewaakte Service Fabric service-replica.
* Interne watchdogs geïmplementeerd als een Service Fabric-service (bijvoorbeeld, een Service Fabric stateless service waarmee voorwaarden worden gecontroleerd en problemen met rapporten). De watchdogs kunnen worden geïmplementeerd een alle knooppunten of kunnen worden wachtrijen op de bewaakte service.
* Interne watchdogs die worden uitgevoerd op de Service Fabric-knooppunten maar *niet* geïmplementeerd als een Service Fabric-services.
* Externe watchdogs die de resource in test *buiten* de Service Fabric-cluster (bijvoorbeeld monitoring-service, zoals Gomez).

> [!NOTE]
> Het cluster is buiten het vak gevuld met statusrapporten die zijn verzonden door de onderdelen van het systeem. Meer informatie [voor het oplossen van problemen met behulp van de systeemstatus rapporten](service-fabric-understand-and-troubleshoot-with-system-health-reports.md). De Gebruikersrapporten moeten worden verzonden op [de statusentiteiten](service-fabric-health-introduction.md#health-entities-and-hierarchy) die al zijn gemaakt door het systeem.
> 
> 

Zodra de status reporting ontwerp is uitgeschakeld, statusrapporten kunnen worden verzonden eenvoudig. U kunt [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) tot de status van het rapport als het cluster niet [beveiligde](service-fabric-cluster-security.md) of als de client fabric beheerdersbevoegdheden heeft. Rapportage kan worden gedaan via de API door met behulp van [FabricClient.HealthManager.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth), via PowerShell of via REST. Configuratie van knoppen batch-rapporten voor betere prestaties.

> [!NOTE]
> De status van het rapport is synchrone en staat voor de validatie werkt alleen op de client. Het feit dat het rapport wordt geaccepteerd door de health-client of de `Partition` of `CodePackageActivationContext` objecten betekent niet dat deze wordt toegepast in de store. Het is asynchroon verzonden en mogelijk batch verwerkt met andere rapporten. De verwerking op de server nog steeds mislukt: het volgnummer is mogelijk verlopen, de entiteit op waarmee het rapport moet worden toegepast is verwijderd, enzovoort.
> 
> 

## <a name="health-client"></a>Status van client
De health-rapporten worden verzonden naar de health store via een health-client, die zich in de fabric-client. De health-client kan worden geconfigureerd met de volgende instellingen:

* **HealthReportSendInterval**: De vertraging tussen het moment dat het rapport is toegevoegd aan de client en de tijd dat deze wordt verzonden naar de health-store. Gebruikt voor batch-rapporten in een enkel bericht, in plaats van één bericht voor elk rapport. De batchverwerking verbetert de prestaties. Standaard: 30 seconden.
* **HealthReportRetrySendInterval**: Het interval waarmee de client health opnieuw samengevoegde status rapporteert aan de health-store. Standaard: 30 seconden.
* **HealthOperationTimeout**: De time-outperiode voor een rapportbericht verzonden naar de health-store. Als er is een time-out opgetreden voor een bericht, de health-client opnieuw probeert deze totdat het statusarchief bevestigt dat het rapport is verwerkt. Standaard: twee minuten.

> [!NOTE]
> Wanneer de rapporten in batch worden opgenomen, de fabric-client moet worden bewaard actief voor ten minste de HealthReportSendInterval om ervoor te zorgen dat ze worden verzonden. Als het bericht verloren is of de health store niet veroorzaakt door tijdelijke fouten toepassen, de fabric-client moet worden gehouden actief meer, zodat deze een kans om opnieuw te proberen.
> 
> 

De buffer op de client neemt de uniekheid van de rapporten in acht genomen. Als een bepaalde slechte journalist 100 rapporten per seconde op dezelfde eigenschap van dezelfde entiteit rapporteert, worden de rapporten bijvoorbeeld vervangen door de laatste versie. Maximaal één dergelijk rapport bestaat in de wachtrij van de client. Als batchverwerking is geconfigureerd, wordt het aantal rapporten dat is verzonden naar de health-store is slechts een per interval verzenden. Dit rapport is de laatste toegevoegd rapport, dat overeenkomt met de meest recente status van de entiteit.
Parameters voor de configuratie opgeven wanneer `FabricClient` wordt gemaakt door door te geven [FabricClientSettings](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclientsettings) met de gewenste waarden voor vermeldingen die betrekking hebben op status.

Het volgende voorbeeld maakt u een client fabric en geeft aan dat de rapporten moeten worden verzonden wanneer ze worden toegevoegd. Nieuwe pogingen gebeuren in-time-outs en fouten die opnieuw kunnen worden verstuurd, elke 40 seconden.

```csharp
var clientSettings = new FabricClientSettings()
{
    HealthOperationTimeout = TimeSpan.FromSeconds(120),
    HealthReportSendInterval = TimeSpan.FromSeconds(0),
    HealthReportRetrySendInterval = TimeSpan.FromSeconds(40),
};
var fabricClient = new FabricClient(clientSettings);
```

We raden u de standaard-fabric-client-instellingen, die ingesteld `HealthReportSendInterval` tot 30 seconden. Deze instelling zorgt ervoor dat de optimale prestaties vanwege de batchverwerking. Gebruik voor de kritieke rapporten die zo snel mogelijk moeten worden verzonden, `HealthReportSendOptions` met direct `true` in [FabricClient.HealthClient.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth) API. Direct rapporten bypass van de batchverwerkingsindeling interval. Gebruik deze vlag zorgvuldig; We willen profiteren van de health-client batchverwerking indien mogelijk. Direct verzenden is ook nuttig bij het afsluiten van de fabric-client (het proces worden bijvoorbeeld moet worden afgesloten om te voorkomen dat neveneffecten en ongeldige status heeft vastgesteld). Dit zorgt ervoor dat een best-effort verzenden van de cumulatieve rapporten. Wanneer een rapport wordt toegevoegd met onmiddellijke vlag, batches de client de gezondheid van de cumulatieve rapporten sinds laatste verzenden.

Dezelfde parameters kunnen worden opgegeven wanneer een verbinding met een cluster wordt gemaakt via PowerShell. Het volgende voorbeeld wordt een verbinding met een lokaal cluster:

```powershell
PS C:\> Connect-ServiceFabricCluster -HealthOperationTimeoutInSec 120 -HealthReportSendIntervalInSec 0 -HealthReportRetrySendIntervalInSec 40
True

ConnectionEndpoint   :
FabricClientSettings : {
                       ClientFriendlyName                   : PowerShell-1944858a-4c6d-465f-89c7-9021c12ac0bb
                       PartitionLocationCacheLimit          : 100000
                       PartitionLocationCacheBucketCount    : 1024
                       ServiceChangePollInterval            : 00:02:00
                       ConnectionInitializationTimeout      : 00:00:02
                       KeepAliveInterval                    : 00:00:20
                       HealthOperationTimeout               : 00:02:00
                       HealthReportSendInterval             : 00:00:00
                       HealthReportRetrySendInterval        : 00:00:40
                       NotificationGatewayConnectionTimeout : 00:00:00
                       NotificationCacheUpdateTimeout       : 00:00:00
                       }
GatewayInformation   : {
                       NodeAddress                          : localhost:19000
                       NodeId                               : 1880ec88a3187766a6da323399721f53
                       NodeInstanceId                       : 130729063464981219
                       NodeName                             : Node.1
                       }
```

Op dezelfde manier API, rapporten kunnen worden verzonden met behulp van `-Immediate` switch worden onmiddellijk verzonden, ongeacht de `HealthReportSendInterval` waarde.

Rest, worden de rapporten worden verzonden naar de Service Fabric-gateway met een interne fabric-client. Deze client is standaard geconfigureerd voor het verzenden van rapporten batch verwerkt elke 30 seconden. U kunt de batch-interval wijzigen met de configuratie-instelling van het cluster `HttpGatewayHealthReportSendInterval` op `HttpGateway`. Zoals gezegd, een betere optie is voor het verzenden van de rapporten met `Immediate` true. 

> [!NOTE]
> Om ervoor te zorgen dat niet-geautoriseerde services status op basis van de entiteiten in het cluster kunnen niet rapporteren, de server voor het accepteren van aanvragen van beveiligde clients te configureren. De `FabricClient` gebruikt voor rapportage moet hebben beveiliging ingeschakeld om te communiceren met het cluster (bijvoorbeeld met Kerberos of certificaatverificatie). Meer informatie over [cluster security](service-fabric-cluster-security.md).
> 
> 

## <a name="report-from-within-low-privilege-services"></a>Rapport van binnen met beperkte bevoegdheden services
Als de Service Fabric-services geen beheerderstoegang tot het cluster, kunt u de status rapporteren op entiteiten op basis van de huidige context via `Partition` of `CodePackageActivationContext`.

* Gebruik voor stateless services [IStatelessServicePartition.ReportInstanceHealth](https://docs.microsoft.com/dotnet/api/system.fabric.istatelessservicepartition.reportinstancehealth) om te rapporteren over de huidige service-exemplaar.
* Gebruik voor stateful services [IStatefulServicePartition.ReportReplicaHealth](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition.reportreplicahealth) om te rapporteren over de huidige replica.
* Gebruik [IServicePartition.ReportPartitionHealth](https://docs.microsoft.com/dotnet/api/system.fabric.iservicepartition.reportpartitionhealth) om te rapporteren over de huidige partitie-entiteit.
* Gebruik [CodePackageActivationContext.ReportApplicationHealth](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportapplicationhealth) om te rapporteren over de huidige toepassing.
* Gebruik [CodePackageActivationContext.ReportDeployedApplicationHealth](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportdeployedapplicationhealth) om te rapporteren over de huidige toepassing geïmplementeerd op het huidige knooppunt.
* Gebruik [CodePackageActivationContext.ReportDeployedServicePackageHealth](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportdeployedservicepackagehealth) om te rapporteren over een servicepakket voor de toepassing die is geïmplementeerd op het huidige knooppunt.

> [!NOTE]
> Intern maakt de `Partition` en de `CodePackageActivationContext` houdt een health-client geconfigureerd met standaardinstellingen. Zoals uitgelegd voor de [health client](service-fabric-report-health.md#health-client), rapporten zijn verzameld en verzonden op een timer. De objecten moeten worden opgeslagen als actief mogelijkheid voor het verzenden van het rapport.
> 
> 

U kunt opgeven `HealthReportSendOptions` bij het verzenden van rapporten via `Partition` en `CodePackageActivationContext` health API's. Als u hebt de kritieke rapporten die zo snel mogelijk moeten worden verzonden, gebruikt u `HealthReportSendOptions` met direct `true`. Direct rapporten bypass van de batchverwerkingsindeling interval van de interne status-client. Zoals al eerder vermeld, gebruikt u deze vlag zorgvuldig; We willen profiteren van de health-client batchverwerking indien mogelijk.

## <a name="design-health-reporting"></a>Status reporting ontwerpen
De eerste stap bij het genereren van rapporten van hoge kwaliteit is identificeren van de voorwaarden die van invloed kunnen de status van de service. Een voorwaarde waarmee u kunt problemen met de vlag in de service of het cluster wanneer deze wordt gestart, of nog betere voordat er een probleem voordoet, kan mogelijk miljarden aan geïnvesteerde dollars opslaan. De voordelen zijn minder uitvaltijd en minder 's nachts uur besteed aan het onderzoeken en oplossen van problemen en hogere klanttevredenheid.

Zodra de voorwaarden worden geïdentificeerd, moeten watchdog schrijvers om de beste manier om ze te controleren voor balans tussen overhead en bruikbaarheid te achterhalen. Neem bijvoorbeeld een service waarmee complexe berekeningen die gebruikmaken van sommige tijdelijke bestanden op een share heeft. De share om ervoor te zorgen dat er voldoende ruimte beschikbaar is, kan een watchdog controleren. Het kan luisteren voor meldingen van bestand of map verandert. Het kan een waarschuwing worden gerapporteerd als een vooraf drempelwaarde is bereikt en er een fout gemeld als de share vol is. Op een waarschuwing weergegeven, kan een herstel-systeem gestart opschonen van oudere bestanden op de share. Bij een fout kan een systeem herstel de service-replica verplaatsen naar een ander knooppunt. Houd er rekening mee hoe de Staten voorwaarde worden beschreven in termen van health: de status van de voorwaarde die kan worden beschouwd als in orde (ok) of niet in orde (waarschuwing of fout).

Zodra de details van de bewaking zijn ingesteld, moet een watchdog-schrijver bepalen hoe de watchdog implementeren. Als de voorwaarden kunnen worden bepaald uit in de service, kan de watchdog deel uitmaken van de bewaakte service zelf. De servicecode kan bijvoorbeeld Controleer het gebruik van de share en vervolgens rapporteren telkens wanneer er wordt geprobeerd een bestand schrijven. Het voordeel van deze benadering is dat reporting eenvoudig is. Wees voorzichtig om te voorkomen dat watchdog bugs die invloed hebben op de functionaliteit van de service.

Rapportage vanuit is de bewaakte service niet altijd een optie. Een watchdog binnen de service kan niet mogelijk zijn voor het detecteren van de voorwaarden. Het kan geen de logica of gegevens om het te bepalen. De overhead van de bewaking van de voorwaarden zijn hoog. De voorwaarden ook mogelijk niet specifiek zijn voor een service, maar in plaats daarvan invloed hebben op de interactie tussen services. Een andere optie is dat watchdogs in het cluster als afzonderlijke processen. De watchdogs bewaak de voorwaarden en het rapport, zonder gevolgen voor de belangrijkste services op geen enkele manier. Deze watchdogs kunnen bijvoorbeeld worden geïmplementeerd als stateless services in dezelfde toepassing geïmplementeerd op alle knooppunten of op de dezelfde knooppunten als de service.

Soms is een watchdog die worden uitgevoerd in het cluster niet een optie. Als de bewaakte voorwaarde is de beschikbaarheid of de functionaliteit van de service, zoals gebruikers deze zien, is het raadzaam te hebben van de watchdogs op dezelfde plaats als de gebruiker-clients. Daar kunnen ze de bewerkingen op dezelfde manier als gebruikers deze aanroepen testen. U kunt bijvoorbeeld een watchdog die zich buiten het cluster, problemen met aanvragen bij de service en controleert u de latentie en de juistheid van het resultaat hebben. (Voor een service calculator bijvoorbeeld 2 + 2 retourneert 4 in een redelijk tijdsbestek?)

Zodra de watchdog-details zijn is voltooid, moet u bepalen van een bron-ID die deze kan worden aangeduid. Als meerdere watchdogs van hetzelfde type zijn opgenomen in het cluster, moeten ze rapporteren over verschillende entiteiten of, als ze een over de dezelfde entiteit rapport gebruiken verschillende bron-ID of de eigenschap. Op deze manier hun rapporten kunnen worden gecombineerd. De eigenschap van het statusrapport moet de bewaakte voorwaarde vastleggen. (Voor het bovenstaande voorbeeld wordt de eigenschap mogelijk **ShareSize**.) Als u meerdere rapporten op dezelfde voorwaarde toepassen, moet de eigenschap sommige dynamische gegevens waarmee u rapporten updomeincontrollers toe samen te bevatten. Bijvoorbeeld, als meerdere shares worden bewaakt moeten, de eigenschapsnaam mag **ShareSize sharename**.

> [!NOTE]
> Voer *niet* gebruik van de health-store om statusinformatie. Alleen door de health-gerelateerde informatie moet worden gerapporteerd als status, als deze informatie heeft gevolgen voor de evaluatie van de status van een entiteit. De health-store is niet ontworpen als een archief voor algemeen gebruik. Status evaluatielogica wordt gebruikt voor het onderbrengen van alle gegevens in de status van de. Verzenden van gegevens die niet gerelateerd aan health (zoals de rapportage over de status met een status van OK) niet van invloed zijn op de geaggregeerde status, maar deze negatieve invloed hebben op de prestaties van de health-store.
> 
> 

De volgende beslissingspunt is welke entiteit rapport op. De meeste van de tijd, identificeert de voorwaarde duidelijk de entiteit. De entiteit met de best mogelijke granulariteit kiezen. Als een voorwaarde gevolgen heeft voor alle replica's in een partitie, een rapport over de partitie, niet op de service. Hoek gevallen waarbij meer thought is vereist, maar zijn er. Als de voorwaarde van invloed is op een entiteit, zoals een replica, maar de keuze is dat de voorwaarde voor meer dan de duur van de levensduur van de replica is gemarkeerd, wordt dit apparaat op de partitie moet worden gerapporteerd. Anders, wanneer de replica wordt verwijderd, de health store ruimt de rapporten ervan. Watchdog schrijvers moeten goed nadenken over de levensduur van de entiteit en het rapport. Het moet wissen wanneer een rapport moet worden opgeschoond in een store, (bijvoorbeeld wanneer een fout gerapporteerd voor een entiteit is niet langer van toepassing is).

We bekijken een voorbeeld waarin wordt geplaatst samen de punten die ik beschreven. Houd rekening met dat een Service Fabric-toepassing bestaat uit een master stateful, permanente service en secundaire stateless services die zijn geïmplementeerd op alle knooppunten (één secundaire servicetype voor elk type taak). Het model heeft een wachtrij voor verwerking met opdrachten die worden uitgevoerd door de secundaire replica's. De secundaire replica's worden uitgevoerd van de inkomende aanvragen en back bevestiging signalen verzenden. Een voorwaarde die kan worden bewaakt is de lengte van de master-verwerkingswachtrij. Als de master wachtrijlengte een drempel bereikt, wordt een waarschuwing wordt gerapporteerd. De waarschuwing geeft aan dat de secundaire replica's kunnen niet worden gebruikt voor het afhandelen van de belasting. Als de wachtrij de maximale lengte bereikt en opdrachten zijn verwijderd, een fout wordt gerapporteerd, omdat de service kan niet worden hersteld. De rapporten kunnen zich op de eigenschap **QueueStatus**. De watchdog zich in de service en het regelmatig op de master primaire replica wordt verzonden. De time to live van twee minuten is en deze regelmatig elke 30 seconden wordt verzonden. Als de primaire uitvalt, wordt het rapport automatisch opgeschoond uit de store. Als de service-replica is, maar er is een impasse geraakt of andere problemen, het rapport is verlopen in health store. In dit geval wordt is de entiteit geëvalueerd op de fout.

Nog een voorwaarde die kan worden bewaakt is uitvoeringstijd van de taak. Het model distribueert taken naar de secundaire replica's op basis van het taaktype. Afhankelijk van het ontwerp, kan het model de secundaire replica's voor de status van taak controleren. Het kan ook wachten op secundaire replica's voor het verzenden van back-bevestiging signalen wanneer ze klaar bent. In het tweede geval is, moet zorgvuldig worden uitgevoerd voor het detecteren van situaties waarin het secundaire replica's die of berichten verloren gaan. Een optie is voor de master voor het verzenden van een ping-aanvraag naar de dezelfde secundaire, die de status ervan worden teruggestuurd. Als er geen status wordt ontvangen, wordt het model beschouwt een fout en waarschuwing van de taak. Dit gedrag wordt ervan uitgegaan dat de taken idempotent zijn.

De bewaakte voorwaarde kan worden omgezet als een waarschuwing als de taak wordt niet uitgevoerd in een bepaalde periode (**t1**, bijvoorbeeld 10 minuten). Als de taak is niet voltooid in de tijd (**t2**, bijvoorbeeld 20 minuten), de bewaakte voorwaarde als fout kan worden omgezet. Deze rapportage kan worden uitgevoerd op verschillende manieren:

* De primaire replica van de master rapporten periodiek op zichzelf. U kunt één eigenschap voor alle in behandeling zijnde taken hebben in de wachtrij. Als ten minste één taak duurt langer, de rapportstatus voor de eigenschap **PendingTasks** is een waarschuwing of fout, waar nodig. Als er geen taken in behandeling zijn of de uitvoering van alle taken, is de rapportstatus in orde. De taken zijn permanent. Als de primaire uitvalt, wordt de nieuwe opgewaardeerde primaire kunt doorgaan met het correct rapporteren.
* Een ander watchdog-proces (in de cloud of extern) controleert taken (van buiten, op basis van de gewenste Taakresultaat) om te zien als ze zijn voltooid. Als ze niet aansluiten bij de drempelwaarden, wordt een rapport op de masterserver verzonden. Een rapport worden ook verzonden voor elke taak met de taak-id, zoals **PendingTask + taskId**. Rapporten moeten worden verzonden alleen over de status van niet in orde. Tijd live met een paar minuten en de rapporten moeten worden verwijderd als ze verlopen om ervoor te zorgen opschonen markeren instellen.
* De secundaire server die wordt uitgevoerd op een taak rapporten wanneer het duurt langer dan verwacht uit te voeren. Deze rapporten over de service-exemplaar voor de eigenschap **PendingTasks**. Het rapport lokaliseert de service-exemplaar dat problemen heeft, maar deze niet het geval wanneer het exemplaar diezelfde. De rapporten worden vervolgens opgeschoond. Het kan een rapport over de secundaire service. Als de secundaire is de taak voltooid, wordt het rapport uit de store in het tweede exemplaar gewist. Het rapport niet het geval wanneer het bevestigingsbericht verloren en wordt de taak is niet voltooid vanuit het oogpunt van het model niet vastleggen.

Maar de rapportage plaatsvindt in de die hierboven worden beschreven gevallen, worden de rapporten worden vastgelegd in de status van de toepassing wanneer de status wordt geëvalueerd.

## <a name="report-periodically-vs-on-transition"></a>Rapport regelmatig versus op overgang
Met behulp van het statusmodel van de rapportage kunt watchdogs periodiek of op overgangen rapporten verzenden. De aanbevolen manier voor het melden van watchdog is periodiek, omdat de code veel eenvoudiger en minder is gevoelig voor fouten. De watchdogs moeten streven ernaar om zo eenvoudig mogelijk om te voorkomen van fouten waarvoor onjuiste rapporten is geactiveerd. Onjuiste *niet in orde* rapporten invloed hebben op de gezondheid van beoordelingen en scenario's op basis van status, met inbegrip van upgrades. Onjuiste *in orde* rapporten verbergen problemen in het cluster, wat niet gewenst is.

Voor periodieke rapportage, kan de watchdog worden geïmplementeerd met een timer. Op een retouraanroep timer de watchdog Controleer de status en verzend een rapport op basis van de huidige status. Er is niet nodig om te zien welk rapport al eerder hebt verzonden of maken van een optimalisaties in termen van berichten. De health-client heeft batchverwerking logica om u te helpen met prestaties. Terwijl de health-client wordt gehouden actief zijn, het opnieuw probeert intern totdat het rapport wordt bevestigd door de health store of de watchdog een nieuwere rapport met dezelfde entiteit, eigenschap en bron genereert.

Rapportage over overgangen vereist zorgvuldige verwerking van status. De watchdog bewaakt bepaalde voorwaarden en -rapporten alleen wanneer de omstandigheden worden gewijzigd. Het voordeel van deze benadering is dat er minder rapporten nodig zijn. Het nadeel is dat de logica van de watchdog complexe. De watchdog moet de voorwaarden of de rapporten behouden, zodat ze kunnen worden gecontroleerd om te bepalen van de status verandert. Bij failover, Wees voorzichtig met rapporten toegevoegd, maar nog niet verzonden naar de health-store. Het volgnummer moet toenemende. Als dat niet het geval is, moet u de rapporten worden geweigerd als verouderd. In het zeldzame gevallen waarbij verlies van gegevens worden in rekening gebracht, wordt synchronisatie mogelijk nodig tussen de status van de journalist en de status van de health-store.

Rapportage over overgangen zinvol is voor rapportage over zelf, via services `Partition` of `CodePackageActivationContext`. Wanneer het lokaal object (replica of geïmplementeerd servicepakket / toepassing is geïmplementeerd) is verwijderd, worden alle bijbehorende rapporten ook verwijderd. Deze automatisch op te schonen worden de noodzaak voor de synchronisatie tussen journalist en health store. Als het rapport voor de bovenliggende partitie of de bovenliggende toepassing is, moet zorgvuldig worden uitgevoerd voor de failover om te voorkomen dat verouderde rapporten in de health-store. Logische moet worden toegevoegd aan de juiste status onderhouden en schakelt u het rapport uit store wanneer het niet meer nodig hebt.

## <a name="implement-health-reporting"></a>Status reporting implementeren
Zodra de gegevens van de entiteit en rapport duidelijk zijn, kan verzenden van statusrapporten met worden gedaan via de API, PowerShell of REST.

### <a name="api"></a>API
Om aan te melden via de API, moet u een statusrapport specifiek zijn voor het entiteitstype dat ze in het rapport wilt maken. Geef het rapport naar een status-client. U kunt ook een health-gegevens maken en geven deze om op te lossen rapportagemethoden op `Partition` of `CodePackageActivationContext` om te rapporteren over de huidige entiteiten.

Het volgende voorbeeld ziet periodieke rapportage vanuit een watchdog binnen het cluster. De watchdog wordt gecontroleerd of een externe resource vanuit een knooppunt kan worden benaderd. De resource is nodig voor een servicemanifest in de toepassing. Als de resource niet beschikbaar is, kunnen de andere services in de toepassing nog steeds correct werken. Daarom wordt het rapport verzonden op de geïmplementeerde service pakket entiteit elke 30 seconden.

```csharp
private static Uri ApplicationName = new Uri("fabric:/WordCount");
private static string ServiceManifestName = "WordCount.Service";
private static string NodeName = FabricRuntime.GetNodeContext().NodeName;
private static Timer ReportTimer = new Timer(new TimerCallback(SendReport), null, 30 * 1000, 30 * 1000);
private static FabricClient Client = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });

public static void SendReport(object obj)
{
    // Test whether the resource can be accessed from the node
    HealthState healthState = this.TestConnectivityToExternalResource();

    // Send report on deployed service package, as the connectivity is needed by the specific service manifest
    // and can be different on different nodes
    var deployedServicePackageHealthReport = new DeployedServicePackageHealthReport(
        ApplicationName,
        ServiceManifestName,
        NodeName,
        new HealthInformation("ExternalSourceWatcher", "Connectivity", healthState));

    // TODO: handle exception. Code omitted for snippet brevity.
    // Possible exceptions: FabricException with error codes
    // FabricHealthStaleReport (non-retryable, the report is already queued on the health client),
    // FabricHealthMaxReportsReached (retryable; user should retry with exponential delay until the report is accepted).
    Client.HealthManager.ReportHealth(deployedServicePackageHealthReport);
}
```

### <a name="powershell"></a>PowerShell
Verzenden van statusrapporten met **verzenden ServiceFabric*EntityType*HealthReport**.

Het volgende voorbeeld ziet periodieke rapportage op basis van CPU-waarden op een knooppunt. Elke 30 seconden door de rapporten moeten worden verzonden en een time to live van twee minuten van hebben. Als ze zijn verlopen, heeft de journalist problemen, zodat het knooppunt wordt geëvalueerd op de fout. Wanneer de CPU hoger dan een drempelwaarde is, wordt in het rapport een status van de waarschuwing heeft. Wanneer de CPU boven een drempelwaarde voor meer dan de geconfigureerde tijd blijft, wordt dit apparaat gerapporteerd als een fout. Anders, verzendt de journalist een status van OK.

```powershell
PS C:\> Send-ServiceFabricNodeHealthReport -NodeName Node.1 -HealthState Warning -SourceId PowershellWatcher -HealthProperty CPU -Description "CPU is above 80% threshold" -TimeToLiveSec 120

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='PowershellWatcher', Property='CPU', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 5
                        SentAt                : 4/21/2015 8:01:17 AM
                        ReceivedAt            : 4/21/2015 8:02:12 AM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/21/2015 8:02:12 AM

                        SourceId              : PowershellWatcher
                        Property              : CPU
                        HealthState           : Warning
                        SequenceNumber        : 130741236814913394
                        SentAt                : 4/21/2015 9:01:21 PM
                        ReceivedAt            : 4/21/2015 9:01:21 PM
                        TTL                   : 00:02:00
                        Description           : CPU is above 80% threshold
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/21/2015 9:01:21 PM
```

Het volgende voorbeeld rapporteert een tijdelijke waarschuwing over een replica. Het wordt eerst de partitie-ID en klik vervolgens op de replica-ID voor de service die het is geïnteresseerd in. Verzendt vervolgens een rapport van **PowershellWatcher** voor de eigenschap **ResourceDependency**. Het rapport is van belang zijn slechts twee minuten en wordt deze verwijderd uit de store automatisch.

```powershell
PS C:\> $partitionId = (Get-ServiceFabricPartition -ServiceName fabric:/WordCount/WordCount.Service).PartitionId

PS C:\> $replicaId = (Get-ServiceFabricReplica -PartitionId $partitionId | where {$_.ReplicaRole -eq "Primary"}).ReplicaId

PS C:\> Send-ServiceFabricReplicaHealthReport -PartitionId $partitionId -ReplicaId $replicaId -HealthState Warning -SourceId PowershellWatcher -HealthProperty ResourceDependency -Description "The external resource that the primary is using has been rebooted at 4/21/2015 9:01:21 PM. Expect processing delays for a few minutes." -TimeToLiveSec 120 -RemoveWhenExpired

PS C:\> Get-ServiceFabricReplicaHealth  -PartitionId $partitionId -ReplicaOrInstanceId $replicaId


PartitionId           : 8f82daff-eb68-4fd9-b631-7a37629e08c0
ReplicaId             : 130740415594605869
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='PowershellWatcher', Property='ResourceDependency', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130740768777734943
                        SentAt                : 4/21/2015 8:01:17 AM
                        ReceivedAt            : 4/21/2015 8:02:12 AM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/21/2015 8:02:12 AM

                        SourceId              : PowershellWatcher
                        Property              : ResourceDependency
                        HealthState           : Warning
                        SequenceNumber        : 130741243777723555
                        SentAt                : 4/21/2015 9:12:57 PM
                        ReceivedAt            : 4/21/2015 9:12:57 PM
                        TTL                   : 00:02:00
                        Description           : The external resource that the primary is using has been rebooted at 4/21/2015 9:01:21 PM. Expect processing delays for a few minutes.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : ->Warning = 4/21/2015 9:12:32 PM
```

### <a name="rest"></a>REST
De gezondheid van rapporten met behulp van REST met POST-aanvragen die gaat u naar de gewenste entiteit en hebben in de hoofdtekst van de beschrijving van de health rapport verzenden. Zie bijvoorbeeld het verzenden van REST [cluster statusrapporten](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-cluster) of [service statusrapporten](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-service). Alle entiteiten worden ondersteund.

## <a name="next-steps"></a>Volgende stappen
Op basis van de health-gegevens, kunnen schrijvers van de service en toepassing/cluster beheerders van de volgende manieren gebruiken voor de gegevens zien. Ze kunnen bijvoorbeeld waarschuwingen op basis van de integriteitsstatus van de instellen om af te vangen ernstige problemen voordat ze leiden uitval tot. Beheerders kunnen ook instellen herstellen systemen problemen automatisch oplossen.

[Inleiding tot Service Fabric-status controleren](service-fabric-health-introduction.md)

[Service Fabric-statusrapporten weergeven](service-fabric-view-entities-aggregated-health.md)

[Het servicestatus rapporteren en controleren](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Systeemstatusrapporten gebruiken voor het oplossen van problemen](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Controle en diagnose van services lokaal](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Service Fabric-toepassingsupgrade](service-fabric-application-upgrade.md)


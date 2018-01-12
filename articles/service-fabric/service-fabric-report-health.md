---
title: Aangepaste Service Fabric-statusrapporten toevoegen | Microsoft Docs
description: Beschrijft hoe u aangepaste statusrapporten verzenden naar Azure Service Fabric health entiteiten. Geeft aanbevelingen voor het ontwerpen en implementeren van statusrapporten kwaliteit.
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: 0a00a7d2-510e-47d0-8aa8-24c851ea847f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/11/2017
ms.author: oanapl
ms.openlocfilehash: 88d80271e744d6f00afd1ff1c3df29180565b59e
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="add-custom-service-fabric-health-reports"></a>Aangepaste statusrapporten van Service Fabric toevoegen
Azure Service Fabric introduceert een [statusmodel](service-fabric-health-introduction.md) ontworpen om te markeren slecht cluster en de voorwaarden van toepassing op specifieke entiteiten. Maakt gebruik van het statusmodel **health rapporteurs** (onderdelen van het systeem en watchdogs). Het doel is snel en gemakkelijk diagnose en herstel. Schrijvers van de service moeten om na te denken over status vooraf. Een voorwaarde die van invloed kan status moet worden gerapporteerd, vooral als deze vlag problemen dicht bij de hoofdmap kan helpen. Statusgegevens bespaart tijd en moeite op onderzoek naar en foutopsporing. Het nut vooral duidelijk is wanneer de service actief op de gewenste schaal in de cloud is (particulier of Azure).

De Service Fabric-rapporteurs monitor geïdentificeerd voorwaarden van belang. Ze een rapport over deze voorwaarden op basis van hun lokale weergave. De [health store](service-fabric-health-introduction.md#health-store) aggregeert statusgegevens verzonden door alle rapporteurs om te bepalen of entiteiten globaal in orde zijn. Het model is bedoeld om uitgebreide, flexibel en eenvoudig te gebruiken. De kwaliteit van de statusrapporten over bepaalt de nauwkeurigheid van de health-weergave van het cluster. Fout-positieven waarin ten onrechte slecht problemen weergegeven kunnen nadelig upgrades of andere services die gebruikmaken van statusgegevens. Voorbeelden van dergelijke services zijn reparatie en waarschuwen mechanismen. Voorzichtig is daarom nodig is voor rapporten die voorwaarden van belang zijn in de beste manier vastleggen.

Voor het ontwerpen en implementeren van de gezondheid van rapportage, watchdogs en onderdelen van het systeem moeten:

* Definieer de voorwaarde dat ze geïnteresseerd bent in de manier waarop die deze wordt bewaakt en de impact op de cluster- of -functionaliteit. Op de rapporteigenschap gezondheid en status op basis van deze informatie kunt bepalen.
* Bepaal de [entiteit](service-fabric-health-introduction.md#health-entities-and-hierarchy) die het rapport is van toepassing op.
* Bepalen waar de rapportage is gedaan, van de service of via een interne of externe watchdog.
* Definieer een bron die is gebruikt om de Rapportagefout te identificeren.
* Kies een reporting strategie periodiek of op overgangen. De aanbevolen manier is periodiek, omdat deze eenvoudiger code vereist en minder gevoelig voor fouten is.
* Bepalen hoe lang het rapport voor slechte voorwaarden moet blijven in de health-store en hoe deze moet worden gewist. Met deze informatie kunt bepalen levensduur van het rapport en verwijderen op vervaldatum gedrag.

Zoals gezegd, rapportage kunt u doen vanaf:

* De bewaakte Service Fabric-service-replica.
* Interne watchdogs geïmplementeerd als een Service Fabric-service (bijvoorbeeld een Service Fabric staatloze service dat wordt bewaakt voorwaarden en rapporten). De watchdogs kunnen worden geïmplementeerd een alle knooppunten of kunnen worden wachtrijen op de bewaakte service.
* Interne watchdogs die worden uitgevoerd op de Service Fabric-knooppunten maar *niet* geïmplementeerd als een Service Fabric-services.
* Externe watchdogs die waaruit de bron-test *buiten* het Service Fabric-cluster (bijvoorbeeld bewaking service zoals Gomez).

> [!NOTE]
> Het cluster is buiten het vak gevuld met statusrapporten dat is verzonden door de onderdelen van het systeem. Meer informatie op [rapporten system health gebruiken voor het oplossen van](service-fabric-understand-and-troubleshoot-with-system-health-reports.md). De Gebruikersrapporten moeten worden verzonden op [de statusentiteiten](service-fabric-health-introduction.md#health-entities-and-hierarchy) die al zijn gemaakt door het systeem.
> 
> 

Eenmaal de status reporting ontwerp is uitgeschakeld, statusrapporten kunnen worden verzonden eenvoudig. U kunt [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) voor de gezondheid van het rapport als het cluster niet [beveiligde](service-fabric-cluster-security.md) of als de fabric-client beheerdersbevoegdheden heeft. Rapportage kan worden gedaan via de API door met [FabricClient.HealthManager.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth), via PowerShell of via REST. Configuratie knoppen batch-rapporten voor betere prestaties.

> [!NOTE]
> De status van het rapport is synchrone en vertegenwoordigt de validatie werkt alleen op de client. Het feit dat het rapport wordt geaccepteerd door de health-client of de `Partition` of `CodePackageActivationContext` objecten betekent niet dat wordt toegepast in het archief. Het is asynchroon worden verzonden en mogelijk met andere rapporten batch verwerkt. De verwerking op de server mogelijk nog steeds: het volgnummer is mogelijk verlopen, de entiteit waarop het rapport moet worden toegepast is verwijderd, enzovoort.
> 
> 

## <a name="health-client"></a>Health-client
De health-rapporten worden verzonden naar de health store via een health-client, die zich in de fabric-client. De health-client kan worden geconfigureerd met de volgende instellingen:

* **HealthReportSendInterval**: de vertraging tussen het moment dat het rapport is toegevoegd aan de client en de tijd dat deze wordt verzonden naar de health store. Gebruikt voor batch-rapporten in een enkel bericht, in plaats van een bericht verzenden voor elk rapport. De batchverwerking verbetert de prestaties. Standaardwaarde: 30 seconden.
* **HealthReportRetrySendInterval**: het interval waarmee de client health opnieuw samengevoegde status verzendt rapporteert aan de health store. Standaardwaarde: 30 seconden.
* **HealthOperationTimeout**: de time-outperiode voor een rapportbericht verzonden naar de health store. Als er is een time-out opgetreden voor een bericht, de client health opnieuw probeert deze totdat de health store bevestigt dat het rapport is verwerkt. Standaardwaarde: twee minuten.

> [!NOTE]
> Wanneer u de rapporten in batch worden opgenomen, de fabric-client moet worden behouden voor ten minste de HealthReportSendInterval om ervoor te zorgen dat ze worden verzonden. Als het bericht verloren gegaan is of de health store niet veroorzaakt door tijdelijke fouten toepassen, de fabric-client moet worden behouden meer hieraan een kans om opnieuw te proberen.
> 
> 

De buffer op de client neemt de uniekheid van de rapporten in aanmerking. Als een bepaalde slechte Rapportagefout 100 rapporten per seconde op dezelfde eigenschap van dezelfde entiteit rapporteren is, kunt u voor de rapporten, worden vervangen door de laatste versie. Maximaal één zo'n rapport bestaat in de wachtrij van de client. Als batchverwerking is geconfigureerd, is het aantal rapporten die worden verzonden naar de health store slechts één per interval verzenden. Dit is de laatste toegevoegde lijst dat overeenkomt met de meest actuele status van de entiteit.
Geef configuratieparameters wanneer `FabricClient` wordt gemaakt door het doorgeven van [FabricClientSettings](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclientsettings) met de gewenste waarden vermeldingen die betrekking hebben op status.

Het volgende voorbeeld maakt een fabric-client en geeft aan dat de rapporten moeten worden verzonden wanneer ze worden toegevoegd. Nieuwe pogingen gebeuren voor time-outs en fouten die kunnen opnieuw worden geprobeerd, elke 40 seconden.

```csharp
var clientSettings = new FabricClientSettings()
{
    HealthOperationTimeout = TimeSpan.FromSeconds(120),
    HealthReportSendInterval = TimeSpan.FromSeconds(0),
    HealthReportRetrySendInterval = TimeSpan.FromSeconds(40),
};
var fabricClient = new FabricClient(clientSettings);
```

We raden de fabric-standaardclient instellingen, die ingesteld `HealthReportSendInterval` 30 seconden. Deze instelling zorgt ervoor dat de optimale prestaties als gevolg van batchverwerking. Gebruik voor kritieke rapporten die zo snel mogelijk moeten worden verzonden, `HealthReportSendOptions` met direct `true` in [FabricClient.HealthClient.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth) API. Onmiddellijke rapporten overslaan en het batchen interval. Gebruik deze vlag zorgvuldig; We willen profiteren van de health-client batchverwerking indien mogelijk. Onmiddellijke verzenden is ook nuttig bij het afsluiten van de fabric-client (bijvoorbeeld het proces heeft bepaald ongeldige status en moet worden afgesloten om te voorkomen dat bijwerkingen). Hiermee zorgt u ervoor een best-effort verzenden van de totale rapporten. Wanneer een rapport met onmiddellijke vlag wordt toegevoegd, batches de client health de samengevoegde rapporten sinds de laatste verzenden.

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

Op dezelfde manier API, rapporten kunnen worden verzonden met behulp van `-Immediate` switch moet onmiddellijk verzonden, ongeacht de `HealthReportSendInterval` waarde.

Voor REST, worden de rapporten worden verzonden naar de Service Fabric-gateway een interne fabric-client is. Deze client wordt standaard geconfigureerd voor het verzenden van rapporten batch verwerkt elke 30 seconden. U kunt het interval batch wijzigen met de configuratie-instelling van het cluster `HttpGatewayHealthReportSendInterval` op `HttpGateway`. Zoals gezegd, een betere optie is voor het verzenden van de rapporten met `Immediate` true. 

> [!NOTE]
> Om ervoor te zorgen dat niet-gemachtigde services health tegen de entiteiten in het cluster kunnen niet rapporteren, de server voor het accepteren van aanvragen van beveiligde clients alleen te configureren. De `FabricClient` gebruikt voor het melden van de beveiliging is ingeschakeld kunnen communiceren met het cluster (bijvoorbeeld met Kerberos of certificaten verificatie). Lees meer over [beveiliging cluster](service-fabric-cluster-security.md).
> 
> 

## <a name="report-from-within-low-privilege-services"></a>Rapport van binnen services met beperkte bevoegdheden
Als de Service Fabric-services geen beheerderstoegang tot het cluster, kunt u health rapporteren op entiteiten van de huidige context via `Partition` of `CodePackageActivationContext`.

* Gebruik voor stateless services [IStatelessServicePartition.ReportInstanceHealth](https://docs.microsoft.com/dotnet/api/system.fabric.istatelessservicepartition.reportinstancehealth) voor het rapporteren van de huidige service-exemplaar.
* Gebruik voor stateful services [IStatefulServicePartition.ReportReplicaHealth](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition.reportreplicahealth) voor het rapporteren van de huidige replica.
* Gebruik [IServicePartition.ReportPartitionHealth](https://docs.microsoft.com/dotnet/api/system.fabric.iservicepartition.reportpartitionhealth) voor het rapporteren van de huidige partitie entiteit.
* Gebruik [CodePackageActivationContext.ReportApplicationHealth](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportapplicationhealth) voor het rapporteren van de huidige toepassing.
* Gebruik [CodePackageActivationContext.ReportDeployedApplicationHealth](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportdeployedapplicationhealth) voor het rapporteren van de huidige toepassing geïmplementeerd op het huidige knooppunt.
* Gebruik [CodePackageActivationContext.ReportDeployedServicePackageHealth](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportdeployedservicepackagehealth) voor het rapporteren van een servicepakket voor de toepassing geïmplementeerd op het huidige knooppunt.

> [!NOTE]
> Intern maakt de `Partition` en de `CodePackageActivationContext` houdt een health-client met standaardinstellingen geconfigureerd. Zoals uitgelegd voor het [health client](service-fabric-report-health.md#health-client), rapporten zijn batch verwerkt en verzonden via een timer. De objecten moeten worden behouden om kans het rapport te verzenden.
> 
> 

U kunt opgeven `HealthReportSendOptions` bij het verzenden van rapporten via `Partition` en `CodePackageActivationContext` health API's. Als er kritieke rapporten die zo snel mogelijk moeten worden verzonden, gebruikt u `HealthReportSendOptions` met direct `true`. Onmiddellijke rapporten overslaan en het batchen interval van de interne status-client. Zoals al eerder vermeld, gebruikt u deze vlag zorgvuldig; We willen profiteren van de health-client batchverwerking indien mogelijk.

## <a name="design-health-reporting"></a>Health reporting ontwerpen
De eerste stap bij het genereren van rapporten van hoge kwaliteit met het identificeren van de voorwaarden die invloed op de status van de service hebben kunnen. Elke voorwaarde kunt vlag problemen in de service of het cluster wanneer deze wordt-- of zelfs beter voordat er een probleem voordoet--kan mogelijk miljarden bedragen opslaan. De voordelen zijn minder uitvaltijd, uren minder 's nachts besteed aan het onderzoeken en oplossen van problemen en hogere klanttevredenheid.

Nadat de voorwaarden worden geïdentificeerd, moeten watchdog schrijvers nagaan wat de beste manier om ze te controleren voor balans tussen bruikbaarheid en de overhead. Neem bijvoorbeeld een service die complexe berekeningen die gebruikmaken van een aantal tijdelijke bestanden op een share heeft. De share om ervoor te zorgen dat er voldoende ruimte beschikbaar is, kan een watchdog controleren. Het kan luisteren voor meldingen van bestand of map wijzigingen. Dit kan een waarschuwing worden gerapporteerd als een vooraf drempelwaarde is bereikt en een fout gemeld als de share vol is. Op een waarschuwing kan een reparatie systeem gestart opschonen van oudere bestanden op de share. Op een fout optreedt, kan een systeem herstellen de replica van de service naar een ander knooppunt verplaatsen. Houd er rekening mee hoe de statussen voorwaarde worden beschreven in termen van health: de status van de voorwaarde die kan worden overwogen in orde (ok) of slecht (waarschuwing of fout).

Zodra de details van de controle zijn ingesteld, moet een schrijver watchdog om te bepalen hoe de watchdog implementeert. Als de voorwaarden kunnen worden bepaald uit in de service, kan de watchdog deel uitmaken van de bewaakte service zelf. De code kan bijvoorbeeld Controleer het gebruik van de share en vervolgens rapporteren telkens wanneer wordt geprobeerd een bestand te schrijven. Het voordeel van deze benadering is dat reporting eenvoudig is. Moet nauwkeurig om te voorkomen dat watchdog bugs die invloed hebben op de functionaliteit van de service.

Reporting vanuit is de bewaakte service niet altijd een optie. Een watchdog in de service is mogelijk niet de voorwaarden te detecteren. Deze wellicht niet de logica of gegevens om het te bepalen. De overhead voor het controleren van de toestand mogelijk te hoog. De voorwaarden ook mogelijk niet specifiek zijn voor een service, maar in plaats daarvan invloed hebben op de interacties tussen services. Er is een andere optie om watchdogs in het cluster als afzonderlijke processen. De watchdogs bewaken de voorwaarden en het rapport, zonder dat de belangrijkste services op een manier. Deze watchdogs kunnen bijvoorbeeld worden geïmplementeerd als stateless services in dezelfde toepassing geïmplementeerd op alle knooppunten of op de knooppunten van de service.

Soms is een watchdog uitgevoerd in het cluster niet een optie ofwel. Als de bewaakte voorwaarde wordt de beschikbaarheid of de functionaliteit van de service, zoals gebruikers deze zien, is het beste de watchdogs hebben op dezelfde locatie als de gebruiker-clients. Daar kunnen ze de bewerkingen op dezelfde manier als gebruikers deze aanroepen testen. U kunt bijvoorbeeld een watchdog die zich buiten het cluster, verzendt aanvragen naar de service en controleert u de latentie en de juistheid van het resultaat hebben. (Voor een service Rekenmachine bijvoorbeeld 2 + 2 retourneert 4 binnen redelijke tijd?)

Zodra de watchdog details hebt is voltooid, moet u bepalen van een bron-ID die uniek wordt geïdentificeerd. Als meerdere watchdogs van hetzelfde type zijn dat in het cluster, ze moeten rapporteren op verschillende entiteiten, of als ze een rapport over dezelfde entiteit, gebruiken verschillende bron-ID of eigenschap. Op deze manier hun rapporten kunnen worden gecombineerd. De eigenschap van het statusrapport moet de bewaakte voorwaarde vastleggen. (Het bovenstaande voorbeeld is de eigenschap kan worden **ShareSize**.) Als meerdere rapporten toepassing naar dezelfde toestand is, moet de eigenschap sommige dynamische gegevens waarmee u rapporten naast te bevatten. Bijvoorbeeld, als meerdere shares worden bewaakt moeten, de eigenschapsnaam mag **ShareSize sharename**.

> [!NOTE]
> Voer *niet* health archief gebruikt om statusgegevens te houden. Alleen door de health-gerelateerde informatie moet worden gerapporteerd als de status, als deze informatie heeft gevolgen voor de statusevaluatie van een entiteit. De health store is niet ontworpen als een algemene archief. Het health evaluatie logica gebruikt om de status van alle gegevens samenvoegen. Verzenden van informatie die geen verband houdt met health (zoals rapportage over de status met een status van OK) heeft geen invloed op de geaggregeerde status, maar dit een negatieve invloed hebben op de prestaties van de health store.
> 
> 

De volgende beslissingspunt is welke entiteit aan rapport op. De meeste gevallen is de voorwaarde duidelijk idetifies de entiteit. Kies de entiteit met de best mogelijke samenvattingen. Als een voorwaarde heeft gevolgen voor alle replica's in een partitie, meld u op de partitie niet op de service. Er zijn hoek gevallen waar meer gedachte is vereist, hoewel. Als de voorwaarde van invloed is op een entiteit, zoals een replica, maar de wens om de voorwaarde die is gemarkeerd voor meer dan de duur van de levensduur van de replica en klik vervolgens op de partitie moet worden gemeld. Anders wanneer de replica wordt verwijderd, ruimt de health store de rapporten ervan. Watchdog schrijvers moeten Denk na over de levensduur van de entiteit en het rapport. Het moet wissen wanneer een rapport moet worden opgeruimd in een store (bijvoorbeeld wanneer een fout gerapporteerd voor een entiteit wordt niet langer van toepassing).

Bekijk een voorbeeld waarin plaatst samen de punten die ik beschreven. U kunt dat een Service Fabric-toepassing bestaat uit een stateful permanente service master en secundaire stateless services die zijn geïmplementeerd op alle knooppunten (één secundaire servicetype voor elk type taak). Het model heeft een wachtrij voor verwerking die opdrachten bevat die worden uitgevoerd door de secundaire replica's. De secundaire replica's voor het uitvoeren van de inkomende aanvragen en signalen back bevestiging verzenden. Een voorwaarde die kan worden bewaakt, is de lengte van de master-verwerkingswachtrij. Als de wachtrijlengte master een drempel bereikt, wordt een waarschuwing wordt gemeld. De waarschuwing geeft aan dat de belasting kunnen niet worden verwerkt door de secundaire replica's. Als de wachtrij de maximaal toegestane lengte is bereikt en opdrachten worden verwijderd, een fout wordt gerapporteerd, omdat de service kan niet worden hersteld. De rapporten kunnen zich op de eigenschap **QueueStatus**. De watchdog zich in de service en deze regelmatig op de master primaire replica wordt verzonden. Time to live is twee minuten en deze regelmatig elke 30 seconden wordt verzonden. Als de primaire uitvalt, wordt het rapport automatisch opgeschoond vanuit de store. Als de replica van de service actief is, maar er is een impasse of andere problemen hebt met, het rapport verloopt in het archief health. In dit geval wordt wordt de entiteit geëvalueerd op fout.

Nog een voorwaarde die kan worden gecontroleerd is uitvoeringstijd van de taak. De master distribueert taken die u moet de secundaire replica's op basis van het taaktype dat u. Het model kan de secundaire replica's voor de taakstatus pollen afhankelijk van het ontwerp. Het kan ook wachten op secundaire replica's signalen back bevestiging verzenden wanneer ze klaar bent. In het tweede geval extra op worden gelet voor het detecteren van situaties waarbij secundaire replica's die of berichten gaan verloren. Een mogelijkheid is voor de master een ping-aanvraag verzenden naar dezelfde secundaire, waarmee de status ervan terug wordt verzonden. Als geen status wordt ontvangen, wordt de master acht een fout en de taak opnieuw gepland. Dit gedrag wordt ervan uitgegaan dat de taken idempotent zijn.

De bewaakte voorwaarde kan worden omgezet als een waarschuwing als de taak is niet uitgevoerd in een bepaalde tijd (**t1**, bijvoorbeeld 10 minuten). Als de taak is niet voltooid in de tijd (**t2**, bijvoorbeeld 20 minuten), de bewaakte voorwaarde als fout kan worden omgezet. Deze rapportage kunt op verschillende manieren doen:

* De replica van de master primaire rapporten periodiek op zichzelf. U kunt één eigenschap voor alle in behandeling zijnde taken hebben in de wachtrij. Als ten minste één taak duurt langer, de rapportstatus op de eigenschap **PendingTasks** een waarschuwing of fout naar gelang van toepassing is. Als er geen taken in behandeling zijn of heeft de uitvoering van alle taken gestart, is het rapportstatus in orde. De taken zijn permanent. Als de primaire uitvalt, wordt de onlangs bevorderd primaire kunt doorgaan met het correct rapporteren.
* Een ander watchdog proces (in de cloud of extern) controleert taken (van buiten, op basis van het resultaat van taak) om te zien als ze zijn voltooid. Als ze bieden geen ondersteuning voor de drempelwaarden, wordt een rapport op de masterserver verzonden. Een rapport wordt ook op elke taak met de taak-id zoals verzonden **PendingTask + taskId**. Rapporten moeten alleen op slecht statussen worden verzonden. Tijd live naar een paar minuten en de rapporten moeten worden verwijderd wanneer ze zijn verlopen om ervoor te zorgen opschonen markeren instellen.
* De secundaire die een taak wordt uitgevoerd op het rapport wanneer het duurt langer dan verwacht uit te voeren. Bevat informatie over het service-exemplaar op de eigenschap **PendingTasks**. Het rapport lokaliseert het service-exemplaar die problemen heeft, maar deze niet de situatie waarin het exemplaar matrijzen vastleggen. De rapporten worden vervolgens opgeschoond. Dit kan een rapport over de secundaire service. Als de secundaire de taken is voltooid, wordt het rapport uit het archief in het exemplaar van de secundaire gewist. Het rapport vastleggen niet van de situatie waarin het bevestigingsbericht verloren en wordt de taak niet uit het oogpunt van het model is voltooid.

Maar de rapportage in de bovenstaande gevallen is voltooid, worden de rapporten worden vastgelegd in toepassingsstatus health wordt geëvalueerd.

## <a name="report-periodically-vs-on-transition"></a>Rapport regelmatig versus op overgang
Met behulp van het reporting statusmodel kunt watchdogs periodiek of overgangen rapporten verzenden. De aanbevolen manier voor het melden van watchdog is periodiek, omdat de code veel eenvoudiger en minder is gevoelig voor fouten. De watchdogs moeten willen zo eenvoudig mogelijk om te voorkomen dat bugs die onjuiste rapporten activeren. Onjuiste *slecht* rapporten gevolgen hebben voor health-beoordelingen en scenario's op basis van status, met inbegrip van upgrades. Onjuiste *orde* rapporten problemen in het cluster niet verbergen.

Voor periodieke rapportage kan de watchdog worden geïmplementeerd met een timer. Op een retouraanroep timer de watchdog Controleer de status en verzend een rapport op basis van de huidige status. Er is niet nodig om te bekijken welke rapport eerder is verzonden of maken van alle optimalisaties in termen van berichten. De health-client heeft batchverwerking logica om te helpen bij de prestaties. Terwijl de client health wordt gehouden actief is, het opnieuw probeert intern totdat het rapport is bevestigd door de health store of de watchdog een nieuwere rapport met dezelfde entiteit, eigenschap en bron genereert.

Rapportage over overgangen vereist zorgvuldige verwerking van de status. De watchdog bepaalde voorwaarden wordt gecontroleerd en -rapporten alleen wanneer de voorwaarden wijzigen. De opwaartse van deze benadering is dat er minder rapporten nodig zijn. Het nadeel is dat de logica van de watchdog complexe. De watchdog moet de voorwaarden of de rapporten onderhouden zodat ze kunnen worden gecontroleerd om te bepalen van de status verandert. Failover, Wees voorzichtig met rapporten is toegevoegd, maar nog niet verzonden naar de health store. Het volgnummer moet steeds groter wordende. Als dat niet het geval is, moet u de rapporten worden geweigerd als verouderd. In het zeldzame gevallen waarin het verlies van gegevens is gemaakt, zijn mogelijk tussen de status van de Rapportagefout en de status van de health store synchronisatie vereist.

Rapportage over overgangen zinvol is voor services die worden gerapporteerd met betrekking tot zelf, via `Partition` of `CodePackageActivationContext`. Wanneer het lokale object (replica of geïmplementeerd servicepakket / toepassing is geïmplementeerd) is verwijderd, worden de rapporten ervan ook verwijderd. Deze automatisch opschonen worden de noodzaak van de synchronisatie tussen Rapportagefout en health store. Als het rapport voor de bovenliggende partitie of de bovenliggende toepassing is, moet nauwkeurig op failover om te voorkomen dat verouderde rapporten in de health store. Logica moet worden toegevoegd aan de juiste status onderhouden en schakelt u het rapport uit de store wanneer het niet meer nodig hebt.

## <a name="implement-health-reporting"></a>Health reporting implementeren
Zodra de details van de entiteit en dit rapport uitgeschakeld zijn, kan statusrapporten verzenden worden gedaan via de API, PowerShell of REST.

### <a name="api"></a>API
Om aan te melden via de API, moet u een statusrapport specifiek zijn voor het entiteitstype dat ze in het rapport wilt maken. Het rapport geeft een client health. U kunt ook een statusgegevens maken en geef dit om op te lossen rapportagemethoden op `Partition` of `CodePackageActivationContext` voor het rapporteren van de huidige entiteiten.

Het volgende voorbeeld ziet periodieke rapportage vanuit een watchdog binnen het cluster. De watchdog controleert of een externe resource vanuit een knooppunt kan worden benaderd. De bron nodig is voor een servicemanifest in de toepassing. Als de bron niet beschikbaar is, kunnen de andere services binnen de toepassing nog steeds goed werken. Daarom wordt het rapport verzonden op de geïmplementeerde service pakket entiteit elke 30 seconden.

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
Verzenden van statusrapporten met  **verzenden ServiceFabric*EntityType*HealthReport **.

Het volgende voorbeeld ziet periodieke rapportage over CPU-waarden op een knooppunt. De rapporten elke 30 seconden moeten worden verzonden en hebben een levensduur van twee minuten. Als ze zijn verlopen, heeft de Rapportagefout problemen, zodat het knooppunt wordt geëvalueerd op fout. Wanneer de CPU hoger dan een drempelwaarde is, wordt in het rapport een status van de waarschuwing heeft. Wanneer de CPU boven een drempelwaarde komt meer dan de geconfigureerde tijd blijft, wordt als een fout gerapporteerd. Anders wordt verzendt de Rapportagefout een status OK.

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

Het volgende voorbeeld wordt een tijdelijke waarschuwing rapporten op een replica. Eerst krijgt de partitie-ID en klik vervolgens op de replica-ID voor de service die het is geïnteresseerd in. Verzendt vervolgens een rapport van **PowershellWatcher** in de eigenschap **ResourceDependency**. Het rapport is van belang zijn voor slechts twee minuten en wordt deze verwijderd uit de store automatisch.

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
Statusrapporten met REST met POST-aanvragen die gaat u naar de gewenste entiteit en hebben in de hoofdtekst van de beschrijving van de health-rapport verzenden. Zie bijvoorbeeld het verzenden van REST [cluster statusrapporten](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-cluster) of [service statusrapporten](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-service). Alle entiteiten worden ondersteund.

## <a name="next-steps"></a>Volgende stappen
Op basis van de statusgegevens, kunnen schrijvers van de service en cluster/toepassing beheerders van de volgende manieren gebruiken voor de gegevens zien. Ze kunnen bijvoorbeeld waarschuwingen op basis van status van instellen om af te vangen ernstige problemen voordat ze leiden uitval tot. Beheerders kunnen ook instellen reparatie systemen automatisch oplossen van problemen.

[Inleiding tot Service Fabric health Monitoring](service-fabric-health-introduction.md)

[Service Fabric-statusrapporten weergeven](service-fabric-view-entities-aggregated-health.md)

[Het rapport en controleer de servicestatus van de](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Systeemstatusrapporten gebruiken voor het oplossen van problemen](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Controle en diagnose van lokaal services](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Upgrade van de service Fabric-toepassing](service-fabric-application-upgrade.md)


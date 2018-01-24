---
title: Het oplossen van Azure Diagnostics | Microsoft Docs
description: Problemen oplossen bij het gebruik van Azure diagnostics in Azure Virtual Machines, Service Fabric of Cloud Services.
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: 66469bce-d457-4d1e-b550-a08d2be4d28c
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/12/2017
ms.author: robb
ms.openlocfilehash: ae99085a37162a883d18976181be198a2f21a60c
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="azure-diagnostics-troubleshooting"></a>Azure Diagnostics probleemoplossing
Dit artikel wordt beschreven informatie over probleemoplossing die relevant is voor het gebruik van Azure Diagnostics. Zie voor meer informatie over Azure diagnostics [overzicht van Azure Diagnostics](azure-diagnostics.md).

## <a name="logical-components"></a>Logische onderdelen
**Starten van de invoegtoepassing Diagnostics (DiagnosticsPluginLauncher.exe)**: Hiermee start u de extensie Azure Diagnostics. Fungeert als de vermelding wijst proces.

**Diagnostics-invoegtoepassing (DiagnosticsPlugin.exe)**: configureert en beheert de levensduur van de bewakingsagent wordt gestart. Dit is het belangrijkste proces dat door het starten is gestart.

**Monitoring Agent (MonAgent\*.exe processen)**: Monitors, verzamelt en de diagnostics-gegevens worden overgedragen.  

## <a name="logartifact-paths"></a>Logboek/artefact paden
Hieronder vindt u de paden naar een aantal belangrijke logboeken en artefacten. We verwijzen naar deze informatie in de rest van het document.

### <a name="azure-cloud-services"></a>Azure Cloud Services
| Artefacten | Pad |
| --- | --- |
| **Azure Diagnostics-configuratiebestand** | %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<version>\Config.txt |
| **Logboekbestanden** | C:\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<version>\ |
| **Lokale archief voor diagnostische gegevens** | C:\Resources\Directory\<CloudServiceDeploymentID>.\<RoleName>.DiagnosticStore\WAD0107\Tables |
| **Monitoring agent-configuratiebestand** | C:\Resources\Directory\<CloudServiceDeploymentID>.\<RoleName>.DiagnosticStore\WAD0107\Configuration\MaConfig.xml |
| **Azure Diagnostics-extensie-pakket** | %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<version> |
| **Pad naar het logboek verzameling hulpprogramma** | %SystemDrive%\Packages\GuestAgent\ |
| **Logboekbestand MonAgentHost** | C:\Resources\Directory\<CloudServiceDeploymentID>.\<RoleName>.DiagnosticStore\WAD0107\Configuration\MonAgentHost.<seq_num>.log |

### <a name="virtual-machines"></a>Virtuele machines
| Artefacten | Pad |
| --- | --- |
| **Azure Diagnostics-configuratiebestand** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<version>\RuntimeSettings |
| **Logboekbestanden** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<version>\Logs\ |
| **Lokale archief voor diagnostische gegevens** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD0107\Tables |
| **Monitoring agent-configuratiebestand** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD0107\Configuration\MaConfig.xml |
| **Statusbestand** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<version>\Status |
| **Azure Diagnostics-extensie-pakket** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>|
| **Pad naar het logboek verzameling hulpprogramma** | C:\WindowsAzure\Packages |
| **Logboekbestand MonAgentHost** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD0107\Configuration\MonAgentHost.<seq_num>.log |

## <a name="metric-data-doesnt-appear-in-the-azure-portal"></a>Metrische gegevens worden niet weergegeven in de Azure portal
Azure Diagnostics biedt metrische gegevens die kunnen worden weergegeven in de Azure portal. Als u problemen met de gegevens in de portal ziet, controleert u de WADMetrics\* tabel in de diagnostische gegevens van Azure storage-account om te zien als de bijbehorende metrische gegevens er zijn. 

Hier de **PartitionKey** van de tabel is van de resource-ID, de virtuele machine of virtuele machine de set van de schaal. **RowKey** de metrische naam (ook wel bekend als de prestatiemeternaam).

Als de resource-ID onjuist is, controleert u **Diagnostics** **configuratie** > **metrische gegevens** > **ResourceId**om te zien als de resource-ID correct is ingesteld.

Als er geen gegevens voor de specifieke metrische gegevens zijn, controleert u **configuratie van diagnostische** > **PerformanceCounter** om te zien als de metriek (prestatiemeteritem) opgenomen is. We standaard ingeschakeld in de volgende prestatiemeteritems:
- \Processor(_Total)\% Processor Time
- \Memory\Available Bytes
- \ASP.NET Applications(__Total__)\Requests/Sec
- \ASP.NET Applications(__Total__)\Errors Total/Sec
- \ASP.NET\Requests in de wachtrij
- \ASP.NET\Requests geweigerd
- \Processor(W3wp)\% processortijd
- \Process(w3wp)\Private Bytes
- \Process(WaIISHost)\% Processor Time
- \Process(WaIISHost)\Private Bytes
- \Process(WaWorkerHost)\% processortijd
- \Process(WaWorkerHost)\Private Bytes
- \Memory\Page fouten per seconde
- \.NET CLR-geheugen (_globale_)\% tijd in %
- \LogicalDisk(C:)\Disk Write Bytes/sec
- \LogicalDisk(C:)\Disk Read Bytes/sec
- \LogicalDisk(D:)\Disk Write Bytes/sec
- \LogicalDisk(D:)\Disk Read Bytes/sec

Als de configuratie correct is ingesteld, maar nog steeds de metrische gegevens niet weergegeven, gebruikt u de volgende richtlijnen om op te lossen.


## <a name="azure-diagnostics-isnt-starting"></a>Azure Diagnostics is niet gestart
Zie voor informatie over waarom Azure Diagnostics kan niet worden gestart, de **DiagnosticsPluginLauncher.log** en **DiagnosticsPlugin.log** bestanden op de locatie van het logboek-bestanden die eerder is opgegeven. 

Als deze logboeken aangeven `Monitoring Agent not reporting success after launch`, betekent dit er is een fout opgetreden bij het starten van MonAgentHost.exe. Bekijk de logboeken op de locatie die opgegeven voor `MonAgentHost log file` in de vorige sectie.

De laatste regel van de logboekbestanden bevat de afsluitcode.  

```
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0
```
Als u vindt een **negatieve** afsluitcode, raadpleeg dan de [afsluiten codetabel](#azure-diagnostics-plugin-exit-codes) in de [verwijst naar de sectie](#references).

## <a name="diagnostics-data-is-not-logged-to-azure-storage"></a>Diagnostics-gegevens is niet naar Azure Storage aangemeld
Bepalen of geen van de gegevens wordt weergegeven of een deel van de gegevens wordt weergegeven.

### <a name="diagnostics-infrastructure-logs"></a>Diagnostische gegevens infrastructuur Logboeken
Diagnostische gegevens registreert alle fouten in de logboeken van de Diagnostics-infrastructuur. Zorg ervoor dat u hebt ingeschakeld de [vastleggen van de infrastructuur van diagnostische logboeken in uw configuratie](#how-to-check-diagnostics-extension-configuration). Vervolgens u snel naar eventuele relevante fouten die worden weergegeven zoeken kunt in de `DiagnosticInfrastructureLogsTable` tabel in uw geconfigureerde storage-account.

### <a name="no-data-is-appearing"></a>Er zijn geen gegevens wordt weergegeven
De meest voorkomende reden die gebeurtenisgegevens helemaal niet wordt weergegeven, is dat de gegevens van de storage-account onjuist is gedefinieerd.

Oplossing: Los van de configuratie van diagnostische en opnieuw installeren van diagnostische gegevens.

Als het opslagaccount correct geconfigureerde externe toegang naar de machine is en controleer of DiagnosticsPlugin.exe en MonAgentCore.exe worden uitgevoerd. Als ze niet zijn uitgevoerd, volgt u de stappen in [Azure Diagnostics is niet gestart](#azure-diagnostics-is-not-starting). 

Als de processen worden uitgevoerd, gaat u naar [gegevens ophalen van lokaal vastgelegd?](#is-data-getting-captured-locally) en volg de instructies.

### <a name="part-of-the-data-is-missing"></a>Deel van de gegevens ontbreekt
Als u wel een aantal gegevens, maar niet alle, betekent dit dat de gegevens verzameling per overdracht pijplijn correct is ingesteld. Volg de subsecties hier als u het probleem wilt beperken.

#### <a name="is-the-collection-configured"></a>Is de verzameling geconfigureerd? 
De configuratie van diagnostische bevat instructies voor een bepaald type gegevens moeten worden verzameld. [Controleer de configuratie van uw](#how-to-check-diagnostics-extension-configuration) om te controleren dat u alleen op zoek naar gegevens die u hebt geconfigureerd voor de verzameling.

#### <a name="is-the-host-generating-data"></a>De host is genereren van gegevens?
- **Prestatiemeteritems**: perfmon openen en controleren van de teller.

- **Traceerlogboeken**: externe toegang in de virtuele machine en een TextWriterTraceListener toevoegen aan het configuratiebestand van de app.  Zie http://msdn.microsoft.com/library/sk36c28t.aspx voor het instellen van de listener tekst.  Zorg ervoor dat de `<trace>` element heeft `<trace autoflush="true">`.<br />
Als er geen traceerlogboeken wordt gegenereerd, Zie [meer over traceerlogboeken ontbreekt](#more-about-trace-logs-missing).

- **ETW-traceringen**: externe toegang in de virtuele machine en installeer de PerfView.  Voer in PerfView, **bestand** > **opdracht User** > **luisteren etwprovder1** > **etwprovider2**, enzovoort. De **luisteren** opdracht is hoofdlettergevoelig en kan niet worden spaties tussen de door komma's gescheiden lijst met ETW-providers. Als de opdracht niet kan worden uitgevoerd, kunt u de **logboek** knop in de rechterbenedenhoek van het hulpprogramma Perfview om te zien wat probeerde uit te voeren en wat het resultaat is.  Ervan uitgaande dat de invoer juist is, weergegeven een nieuw venster. In enkele seconden, kunt u beginnen ETW traceringen te zien.

- **Gebeurtenislogboeken**: externe toegang in de virtuele machine. Open `Event Viewer`, en verzeker u ervan dat de gebeurtenissen bestaan.

#### <a name="is-data-getting-captured-locally"></a>Gegevens ophalen van vastgelegd lokaal?
Controleer vervolgens dat de gegevens wordt lokaal ophalen vastgelegd.
De gegevens worden lokaal opgeslagen `*.tsf` bestanden in [het lokale archief voor diagnostische gegevens](#log-artifacts-path). Verschillende soorten logboeken ophalen die zijn verzameld in andere `.tsf` bestanden. De namen zijn vergelijkbaar met de tabelnamen in Azure Storage. 

Bijvoorbeeld: `Performance Counters` ophalen die worden verzameld in `PerformanceCountersTable.tsf`. Gebeurtenislogboeken ophalen verzameld in `WindowsEventLogsTable.tsf`. Volg de instructies in de [lokaal logboek extractie](#local-log-extraction) sectie aan de lokale verzamelingsbestanden openen en controleren of ze ophalen die zijn verzameld op schijf te zien.

Als u Logboeken lokaal ophalen verzameld niet ziet, en al hebt gecontroleerd dat de host van gegevens genereren, hebt u waarschijnlijk een configuratieprobleem. Uw configuratie zorgvuldig te controleren. 

Bekijk ook de configuratie die is gegenereerd voor MonitoringAgent [MaConfig.xml](#log-artifacts-path). Controleer of er een sectie die de bron van de relevante logboekbestanden beschrijft. Controleer vervolgens niet gaat verloren in de conversie van de configuratie van diagnostische gegevens en de configuratie van de monitoring agent.

#### <a name="is-data-getting-transferred"></a>Is ophalen overgedragen gegevens?
Als u hebt gecontroleerd dat de gegevens wordt lokaal ophalen vastgelegd, maar nog steeds deze niet wordt weergegeven in uw opslagaccount, moet u de volgende stappen uitvoeren: 

- Controleer of dat u een juiste storage-account hebt opgegeven en dat u dit nog niet hebt vernieuwd sleutels voor het opgegeven opslagaccount. Voor Azure Cloud Services soms zien we dat mensen niet bijwerken `useDevelopmentStorage=true`.

- Controleer of het opgegeven opslagaccount klopt. Zorg ervoor dat u hebt geen netwerkbeperkingen die voorkomen dat de onderdelen openbare opslag eindpunten bereikt. Een manier om dit voor externe toegang tot verbinding met de machine is en probeer schrijven iets naar hetzelfde opslagaccount zelf.

- Ten slotte kunt u bekijken welke fouten zijn gemeld door monitoring Agent. De bewakingsagent schrijft de logboeken `maeventtable.tsf`, die zich bevindt in [het lokale archief voor diagnostische gegevens](#log-artifacts-path). Volg de instructies in de [lokaal logboek extractie](#local-log-extraction) sectie voor het openen van het bestand. Probeer om te bepalen of er zijn `errors` die wijzen op mislukte lezen van lokale bestanden schrijven naar de opslag.

### <a name="capturing-and-archiving-logs"></a>Vastleggen en logboeken archiveren
Als u nadenkt weet over het contact opnemen met ondersteuning, is het eerste wat dat kunnen ze u vragen voor het verzamelen van Logboeken vanaf uw computer. U kunt tijd besparen door dit zelf doen. Voer de `CollectGuestLogs.exe` hulpprogramma op [logboekpad verzameling hulpprogramma](#log-artifacts-path). Genereert een ZIP-bestand met alle relevante Azure wordt geregistreerd in dezelfde map.

## <a name="diagnostics-data-tables-not-found"></a>Diagnostische gegevenstabellen is niet gevonden
De tabellen in Azure storage waarin ETW-gebeurtenissen worden benoemd met behulp van de volgende code:

```csharp
        if (String.IsNullOrEmpty(eventDestination)) {
            if (e == "DefaultEvents")
                tableName = "WADDefault" + MD5(provider);
            else
                tableName = "WADEvent" + MD5(provider) + eventId;
        }
        else
            tableName = "WAD" + eventDestination;
```

Hier volgt een voorbeeld:

```XML
        <EtwEventSourceProviderConfiguration provider="prov1">
          <Event id="1" />
          <Event id="2" eventDestination="dest1" />
          <DefaultEvents />
        </EtwEventSourceProviderConfiguration>
        <EtwEventSourceProviderConfiguration provider="prov2">
          <DefaultEvents eventDestination="dest2" />
        </EtwEventSourceProviderConfiguration>
```
```JSON
"EtwEventSourceProviderConfiguration": [
    {
        "provider": "prov1",
        "Event": [
            {
                "id": 1
            },
            {
                "id": 2,
                "eventDestination": "dest1"
            }
        ],
        "DefaultEvents": {
            "eventDestination": "DefaultEventDestination",
            "sinks": ""
        }
    },
    {
        "provider": "prov2",
        "DefaultEvents": {
            "eventDestination": "dest2"
        }
    }
]
```
Deze code genereert vier tabellen:

| Gebeurtenis | Tabelnaam |
| --- | --- |
| provider = 'prov1' &lt;gebeurtenis-id = "1" /&gt; |WADEvent+MD5(“prov1”)+”1” |
| provider = 'prov1' &lt;gebeurtenis-id = "2" eventDestination = "dest1" /&gt; |WADdest1 |
| provider = 'prov1' &lt;DefaultEvents /&gt; |WADDefault+MD5("prov1") |
| provider = 'prov2' &lt;DefaultEvents eventDestination = "dest2" /&gt; |WADdest2 |

## <a name="references"></a>Verwijzingen

### <a name="how-to-check-diagnostics-extension-configuration"></a>Configuratie van diagnostische-extensie controleren
De eenvoudigste manier om te controleren van uw configuratie voor de uitbreiding is naar [Azure Resource Explorer](http://resources.azure.com), en ga vervolgens naar de virtuele machine of cloud service waar u de extensie Azure Diagnostics (IaaSDiagnostics / PaaDiagnostics) is.

U kunt ook extern bureaublad in de machine en bekijk de configuratie van diagnostische Azure-bestand dat wordt beschreven in de [Meld artefacten padsectie](#log-artifacts-path).

Zoeken in beide gevallen **Microsoft.Azure.Diagnostics**, en vervolgens voor de **xmlCfg** of **WadCfg** veld. 

Als u op een virtuele machine zoekt en de **WadCfg** veld aanwezig is, betekent dit dat de configuratie in JSON-indeling. Als de **xmlCfg** veld aanwezig is, betekent dit config XML en base64-gecodeerd. U moet [dit decoderen](http://www.bing.com/search?q=base64+decoder) om te zien van de XML-bestand dat is geladen door diagnostische gegevens.

Voor de rol van cloud-service, als u de configuratie van de schijf, kiest de gegevens zijn base64-gecodeerd, dus u moet [dit decoderen](http://www.bing.com/search?q=base64+decoder) om te zien van de XML-bestand dat is geladen door diagnostische gegevens.

### <a name="azure-diagnostics-plugin-exit-codes"></a>Azure Diagnostics-invoegtoepassing afsluitcodes definiëren
De invoegtoepassing wordt de volgende afsluitcodes geretourneerd:

| Afsluitcode | Beschrijving |
| --- | --- |
| 0 |Geslaagd. |
| -1 |Algemene fout. |
| -2 |De rcf kan bestand niet laden.<p>Deze interne fout vindt alleen plaats als u de guest agent invoegtoepassing launcher handmatig onjuist wordt aangeroepen op de virtuele machine. |
| -3 |Kan het configuratiebestand van de diagnostische gegevens niet laden.<p><p>Oplossing: Veroorzaakt door een configuratiebestand niet schemavalidatie wordt doorgegeven. De oplossing is om een configuratiebestand die voldoet aan het schema. |
| -4 |De lokale resource-map wordt al gebruikt door een ander exemplaar van de bewakingsagent diagnostische gegevens.<p><p>Oplossing: Geef een andere waarde voor **LocalResourceDirectory**. |
| -6 |De Gast-agent invoegtoepassing launcher geprobeerd diagnostische gegevens starten met een opdrachtregel ongeldig is.<p><p>Deze interne fout vindt alleen plaats als u de guest agent invoegtoepassing launcher handmatig onjuist wordt aangeroepen op de virtuele machine. |
| -10 |De Diagnostics-invoegtoepassing is afgesloten met een niet-verwerkte uitzondering. |
| -11 |De gastagent kon het proces is verantwoordelijk voor het starten en het controleren van de bewakingsagent gemaakt.<p><p>Oplossing: Controleer of voldoende systeembronnen beschikbaar zijn voor het nieuwe processen te starten.<p> |
| -101 |Ongeldige argumenten bij het aanroepen van de Diagnostics-invoegtoepassing.<p><p>Deze interne fout vindt alleen plaats als u de guest agent invoegtoepassing launcher handmatig onjuist wordt aangeroepen op de virtuele machine. |
| -102 |Het proces van de invoegtoepassing is niet geïnitialiseerd.<p><p>Oplossing: Controleer of voldoende systeembronnen beschikbaar zijn voor het nieuwe processen te starten. |
| -103 |Het proces van de invoegtoepassing is niet geïnitialiseerd. Het is in het bijzonder kan de logboek-object niet maken.<p><p>Oplossing: Controleer of voldoende systeembronnen beschikbaar zijn voor het nieuwe processen te starten. |
| -104 |Kan niet laden van de rcf-bestand dat is opgegeven door de gastagent.<p><p>Deze interne fout vindt alleen plaats als u de guest agent invoegtoepassing launcher handmatig onjuist wordt aangeroepen op de virtuele machine. |
| -105 |De Diagnostics-invoegtoepassing kan het configuratiebestand van de diagnostische gegevens niet openen.<p><p>Deze interne fout vindt alleen plaats als de Diagnostics-invoegtoepassing handmatig onjuist wordt aangeroepen op de virtuele machine. |
| -106 |Kan het configuratiebestand van de diagnostische gegevens niet lezen.<p><p>Veroorzaakt door een configuratiebestand niet schemavalidatie wordt doorgegeven. <br><br>Oplossing: Geef een configuratiebestand die voldoet aan het schema. Zie voor meer informatie [het controleren van de configuratie van diagnostische extensie](#how-to-check-diagnostics-extension-configuration). |
| -107 |De bron directory op te geven met de monitoring agent is ongeldig.<p><p>Deze interne fout vindt alleen plaats als de bewakingsagent handmatig onjuist wordt aangeroepen op de virtuele machine.</p> |
| -108 |Kan niet converteren van het configuratiebestand van de diagnostische gegevens naar het monitoring agent-configuratiebestand.<p><p>Deze interne fout vindt alleen plaats als de Diagnostics-invoegtoepassing handmatig wordt aangeroepen met een ongeldig configuratiebestand. |
| -110 |Algemene Diagnostics configuratiefout.<p><p>Deze interne fout vindt alleen plaats als de Diagnostics-invoegtoepassing handmatig wordt aangeroepen met een ongeldig configuratiebestand. |
| -111 |Kan niet starten van de bewakingsagent.<p><p>Oplossing: Controleer of er voldoende systeembronnen beschikbaar zijn. |
| -112 |Algemene fout |

### <a name="local-log-extraction"></a>Uitpakken van de lokale logboek
De monitoring agent verzamelt logboeken en artefacten zoals `.tsf` bestanden. De `.tsf` bestand kan niet worden gelezen, maar u kunt converteren naar een `.csv` als volgt: 

```
<Azure diagnostics extension package>\Monitor\x64\table2csv.exe <relevantLogFile>.tsf
```
Een nieuw bestand genaamd `<relevantLogFile>.csv` wordt gemaakt in hetzelfde pad hebben als de bijbehorende `.tsf` bestand.

>[!NOTE] 
> U hoeft dit hulpprogramma uitvoeren op basis van de belangrijkste .tsf-bestand (bijvoorbeeld PerformanceCountersTable.tsf). De bijbehorende bestanden (bijvoorbeeld PerformanceCountersTables_\*\*001.tsf, PerformanceCountersTables_\*\*002.tsf, enzovoort) worden automatisch verwerkt.

### <a name="more-about-missing-trace-logs"></a>Meer informatie over ontbrekende traceerlogboeken 

>[!NOTE]
> De volgende informatie geldt vooral voor Azure Cloud Services, tenzij u de DiagnosticsMonitorTraceListener op een toepassing die wordt uitgevoerd op uw IaaS VM hebt geconfigureerd. 

- Zorg ervoor dat de **DiagnosticMonitorTraceListener** is geconfigureerd in web.config of app.config.  Dit is standaard in cloudserviceprojecten geconfigureerd. Evenwel sommige klanten het commentaar uit, waardoor de trace-instructies moeten niet worden verzameld door diagnostische gegevens. 

- Als Logboeken niet ophalen van van geschreven de **OnStart** of **uitvoeren** methode, zorg ervoor dat de **DiagnosticMonitorTraceListener** bevindt zich in het app.config.  Standaard is dit in het bestand web.config, maar die alleen van toepassing op code die wordt uitgevoerd binnen w3wp.exe. Daarom moet u deze in app.config voor het vastleggen van traces die worden uitgevoerd in WaIISHost.exe.

- Zorg ervoor dat u gebruikt **Diagnostics.Trace.TraceXXX** in plaats van **Diagnostics.Debug.WriteXXX.** De foutopsporingsinstructies worden verwijderd van een release-build.

- Zorg ervoor dat de gecompileerde code daadwerkelijk de **Diagnostics.Trace regels** (Routereflector, ildasm of ILSpy gebruiken om te controleren). **Diagnostics.Trace** opdrachten worden verwijderd uit de gecompileerde binary, tenzij u het symbool TRACE-voorwaardelijke compilatie. Dit is een veelvoorkomend probleem dat optreedt wanneer u msbuild om een project te bouwen.   

## <a name="known-issues-and-mitigations"></a>Bekende problemen en oplossingen
Hier volgt een lijst met bekende problemen met bekende oplossingen:

**1. .NET 4.5-afhankelijkheid**

Windows Azure Diagnostics-uitbreiding heeft een runtime-afhankelijkheid framework .NET 4.5 of hoger. Op het moment van schrijven van alle machines die zijn ingericht voor Azure Cloud Services, evenals alle officiële installatiekopieën die zijn gebaseerd op virtuele machines in Azure, .NET 4.5 of hoger geïnstalleerd. 

Het is nog steeds mogelijk ondervindt een situatie waarin u probeert uit te voeren van extensie voor diagnostische gegevens van Windows Azure op een computer die geen .NET 4.5 of hoger. Dit gebeurt wanneer u uw computer uit een oude afbeelding of een momentopname maakt of wanneer u uw eigen aangepaste schijf brengt.

Dit in het algemeen als een afsluitcode manifesten **255** bij het uitvoeren van **DiagnosticsPluginLauncher.exe.** Fout gebeurt vanwege de volgende niet-verwerkte uitzondering: 
```
System.IO.FileLoadException: Could not load file or assembly 'System.Threading.Tasks, Version=1.5.11.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies
```

**Risicobeperking:** .NET 4.5 of hoger installeren op uw machine.

**2. Prestatiegegevens van prestatiemeteritems is beschikbaar in de opslag, maar niet worden weergegeven in de portal**

De portal ervaring in de virtuele machines worden bepaalde prestatiemeteritems standaard weergegeven. Als u de prestatiemeteritems niet ziet, en u weet dat de gegevens ophalen van gegenereerd omdat deze beschikbaar zijn in de opslagruimte, Controleer het volgende:

- Hiermee wordt aangegeven of de gegevens in de opslag namen heeft voor de teller in het Engels. Als de namen van prestatiemeteritems niet in het Engels, de portal metrische grafiek niet het geval kunnen herkennen deze.

- Als u gebruik van jokertekens (\*) in de namen van prestatiemeteritems, de portal won't kunnen correleren van de teller geconfigureerd en die worden verzameld.

**Risicobeperking**: de taal van de machine naar het Engels voor systeemaccounts wijzigen. Om dit te doen, selecteert u **Configuratiescherm** > **regio** > **Administrative** > **instellingen**. Schakel vervolgens **en systeemaccounts Welkom** zodat de aangepaste taal niet op het systeem-account toegepast wordt. Controleer ook of dat u geen jokertekens gebruiken als u wilt dat de portal om te worden van uw ervaring primaire verbruik.

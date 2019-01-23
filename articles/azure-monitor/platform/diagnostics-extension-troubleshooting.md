---
title: Oplossen van problemen met Azure Diagnostics-extensie
description: Problemen oplossen bij het gebruik van Azure diagnostics in Azure Virtual Machines, Service Fabric of Cloud Services.
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 07/12/2017
ms.author: robb
ms.subservice: diagnostic-extension
ms.openlocfilehash: 305aa28127e453c01de9b55ab6cb0ff3471afad9
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54473806"
---
# <a name="azure-diagnostics-troubleshooting"></a>Azure Diagnostics oplossen van problemen
Dit artikel bevat informatie over probleemoplossing die relevant is voor het gebruik van Azure Diagnostics. Zie voor meer informatie over Azure diagnostics [overzicht van Azure Diagnostics](diagnostics-extension-overview.md).

## <a name="logical-components"></a>Logische onderdelen
**Startprogramma voor diagnostische gegevens van de invoegtoepassing (DiagnosticsPluginLauncher.exe)**: Hiermee start u de Azure Diagnostics-extensie. Fungeert als de vermelding punt proces.

**Diagnostics-invoegtoepassing (DiagnosticsPlugin.exe)**: Configureert en beheert de levensduur van de bewakingsagent wordt gestart. Het is het belangrijkste proces dat door het startprogramma voor apps wordt gestart.

**Monitoring Agent (MonAgent\*.exe processen)**: Bewaakt, verzamelt en brengt de diagnostische gegevens.  

## <a name="logartifact-paths"></a>Logboek/artefact paden
Hieronder vindt u de paden naar enkele belangrijke logboeken en artefacten. We verwijzen naar deze informatie in de rest van het document.

### <a name="azure-cloud-services"></a>Azure Cloud Services
| Artefact | Pad |
| --- | --- |
| **Azure Diagnostics-configuratiebestand** | %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<version>\Config.txt |
| **Logboekbestanden** | C:\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<version>\ |
| **Lokale archief voor diagnostische gegevens** | C:\Resources\Directory\<CloudServiceDeploymentID>.\<RoleName>.DiagnosticStore\WAD0107\Tables |
| **Het configuratiebestand Monitoring agent** | C:\Resources\Directory\<CloudServiceDeploymentID>.\<RoleName>.DiagnosticStore\WAD0107\Configuration\MaConfig.xml |
| **Azure Diagnostics-extensie-pakket** | %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<version> |
| **Pad naar het hulpprogramma logboek verzameling** | %SystemDrive%\Packages\GuestAgent\ |
| **Het logboekbestand MonAgentHost** | C:\Resources\Directory\<CloudServiceDeploymentID>.\<RoleName>.DiagnosticStore\WAD0107\Configuration\MonAgentHost.<seq_num>.log |

### <a name="virtual-machines"></a>Virtuele machines
| Artefact | Pad |
| --- | --- |
| **Azure Diagnostics-configuratiebestand** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<version>\RuntimeSettings |
| **Logboekbestanden** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\ |
| **Lokale archief voor diagnostische gegevens** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD0107\Tables |
| **Het configuratiebestand Monitoring agent** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD0107\Configuration\MaConfig.xml |
| **Statusbestand** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<version>\Status |
| **Azure Diagnostics-extensie-pakket** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>|
| **Pad naar het hulpprogramma logboek verzameling** | C:\WindowsAzure\Packages |
| **Het logboekbestand MonAgentHost** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD0107\Configuration\MonAgentHost.<seq_num>.log |

## <a name="metric-data-doesnt-appear-in-the-azure-portal"></a>Metrische gegevens worden niet weergegeven in de Azure-portal
Azure Diagnostics kunt u metrische gegevens die kunnen worden weergegeven in de Azure-portal. Als u problemen met de gegevens in de portal ziet, controleert u de WADMetrics\* tabel in de Azure Diagnostics-storage-account om te zien als de bijbehorende metrische gegevens er zijn.

Hier de **PartitionKey** van de tabel is de resource-ID, de virtuele machine of virtuele-machineschaalset. **RowKey** is de naam van de meetwaarde (ook wel bekend als de prestatiemeternaam).

Als de resource-ID onjuist is, controleert u **Diagnostics** **configuratie** > **metrische gegevens** > **ResourceId**om te zien als de resource-ID juist is ingesteld.

Als er geen gegevens voor de specifieke metrische gegevens zijn, controleert u **configuratie van diagnostische** > **PerformanceCounter** om te zien als de metriek (prestatiemeteritem) opgenomen is. We standaard ingeschakeld in de volgende prestatiemeteritems:
- \Processor(_Total)\% Processor Time
- \Memory\Available Bytes
- \ASP.NET toepassingen (__totale__) \Requests/Sec
- \ASP.NET toepassingen (__totale__) \Errors in totaal/Sec
- \ASP.NET\Requests in de wachtrij geplaatst
- \ASP.NET\Requests Rejected
- \Processor(W3wp)\% processortijd
- \Process(w3wp)\Private Bytes
- \Process(WaIISHost)\% processortijd
- \Process(WaIISHost)\Private Bytes
- \Process(WaWorkerHost)\% processortijd
- \Process(WaWorkerHost)\Private Bytes
- \Memory\Page fouten per seconde
- \.NET CLR-geheugen (_globale_)\% GC-tijd in
- \LogicalDisk(C:)\Disk Write Bytes/sec
- \LogicalDisk(C:)\Disk Read Bytes/sec
- \LogicalDisk(D:)\Disk Write Bytes/sec
- \LogicalDisk(D:)\Disk Read Bytes/sec

Als de configuratie juist is ingesteld, maar kunt u de metrische gegevens nog steeds niet zien, gebruikt u de volgende richtlijnen om op te lossen.


## <a name="azure-diagnostics-isnt-starting"></a>Azure Diagnostics is niet gestart
Zie voor informatie over waarom Azure Diagnostics kan niet worden gestart, de **DiagnosticsPluginLauncher.log** en **DiagnosticsPlugin.log** bestanden in de locatie van het logboek-bestanden die eerder is opgegeven.

Als u deze logboeken geven aan `Monitoring Agent not reporting success after launch`, betekent dit dat er is een fout opgetreden bij het starten van MonAgentHost.exe. Bekijk de logboeken op de locatie die opgegeven voor `MonAgentHost log file` in de vorige sectie.

De laatste regel van de logboekbestanden bevat de afsluitcode.  

```
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0
```
Als u een **negatieve** afsluitcode, raadpleeg dan de [afsluiten codetabel](#azure-diagnostics-plugin-exit-codes) in de [verwijst naar de sectie](#references).

## <a name="diagnostics-data-is-not-logged-to-azure-storage"></a>Diagnostische gegevens worden niet geregistreerd voor Azure Storage
Bepalen of geen van de gegevens wordt weergegeven of deel van de gegevens wordt weergegeven.

### <a name="diagnostics-infrastructure-logs"></a>Logboeken met diagnostische gegevens over infrastructuur
Diagnostische logboeken van alle fouten in de logboeken met diagnostische gegevens over infrastructuur. Zorg ervoor dat u hebt ingeschakeld de [vastleggen van diagnostische gegevens over infrastructuur-Logboeken in uw configuratie](#how-to-check-diagnostics-extension-configuration). Vervolgens u snel naar eventuele relevante fouten die worden weergegeven zoeken kunt in de `DiagnosticInfrastructureLogsTable` tabel in uw geconfigureerde storage-account.

### <a name="no-data-is-appearing"></a>Er zijn geen gegevens wordt weergegeven
De meest voorkomende reden die gebeurtenisgegevens niet helemaal wordt weergegeven, is dat de gegevens van de storage-account onjuist is gedefinieerd.

Oplossing: Corrigeer de configuratie van de diagnostische gegevens en diagnostische gegevens opnieuw.

Als het opslagaccount is correct geconfigureerde externe toegang in de machine en controleer of DiagnosticsPlugin.exe en MonAgentCore.exe worden uitgevoerd. Als ze niet worden uitgevoerd, volgt u de stappen in [Azure Diagnostics wordt niet gestart](#azure-diagnostics-is-not-starting).

Als de processen worden uitgevoerd, gaat u naar [gegevens lokaal ophalen vastgelegd?](#is-data-getting-captured-locally) en volg de instructies.

### <a name="part-of-the-data-is-missing"></a>Onderdeel van de gegevens ontbreekt
Als u bepaalde gegevens, maar niet alle ontvangt, betekent dit dat de gegevenspijplijn verzameling per overdracht correct is ingesteld. Volg de subsecties hier als u het probleem wilt beperken.

#### <a name="is-the-collection-configured"></a>Is de verzameling geconfigureerd?
De configuratie van diagnostische bevat instructies voor een bepaald type van de gegevens moeten worden verzameld. [De configuratie controleren](#how-to-check-diagnostics-extension-configuration) om te controleren of dat u alleen op zoek naar gegevens die u hebt geconfigureerd voor de verzameling.

#### <a name="is-the-host-generating-data"></a>De host is genereren van gegevens?
- **Prestatiemeteritems**: Open Prestatiemeter en controleren van de teller.

- **Logboeken met traceringen**:  Externe toegang in de virtuele machine en een TextWriterTraceListener toevoegen aan het configuratiebestand van de app.  Zie https://msdn.microsoft.com/library/sk36c28t.aspx voor het instellen van de listener van tekst.  Zorg ervoor dat de `<trace>` element heeft `<trace autoflush="true">`.<br />
Als er geen logboeken met traceringen die worden gegenereerd, Zie [meer informatie over ontbrekende traceerlogboeken](#more-about-trace-logs-missing).

- **ETW-traceringen**: Externe toegang in de virtuele machine en installeer voorbeeld.  Voer in het voorbeeld, **bestand** > **opdracht van de gebruiker** > **luisteren etwprovder1** > **etwprovider2**, enzovoort. De **luisteren** opdracht is hoofdlettergevoelig en er mag spaties tussen de door komma's gescheiden lijst van ETW-providers. Als de opdracht niet kan worden uitgevoerd, kunt u de **Log** knop in de rechterbenedenhoek van de voorbeeld-hulpprogramma om te zien wat er is geprobeerd om uit te voeren en wat het resultaat is.  Ervan uitgaande dat de invoer juist is, verschijnt een nieuw venster. U begint de ETW-traceringen zien uit te voeren in een paar seconden.

- **Gebeurtenislogboeken**: Externe toegang in de virtuele machine. Open `Event Viewer`, en zorg ervoor dat de gebeurtenissen bestaat.

#### <a name="is-data-getting-captured-locally"></a>Gegevens ophalen van vastgelegd lokaal?
Controleer vervolgens of dat de gegevens worden lokaal ophalen vastgelegd.
De gegevens worden lokaal opgeslagen in `*.tsf` bestanden in [het lokale archief voor diagnostische gegevens](#log-artifacts-path). Verschillende soorten logboeken ophalen die zijn verzameld in verschillende `.tsf` bestanden. De namen zijn vergelijkbaar met de namen van tabellen in Azure Storage.

Bijvoorbeeld, `Performance Counters` ophalen die worden verzameld in `PerformanceCountersTable.tsf`. Gebeurtenislogboeken ophalen die zijn verzameld `WindowsEventLogsTable.tsf`. Volg de instructies in de [lokale log extractie](#local-log-extraction) sectie aan de lokale verzameling-bestanden openen en te controleren of ze ophalen die zijn verzameld op schijf te zien.

Als u Logboeken ophalen verzameld lokaal niet ziet en u al hebt gecontroleerd dat de host is worden gebruikt voor het genereren van gegevens, hebt u waarschijnlijk een configuratieprobleem. Zorgvuldig te werk om uw configuratie te controleren.

Bekijk ook de configuratie die is gegenereerd voor MonitoringAgent [MaConfig.xml](#log-artifacts-path). Controleer of er een sectie waarin wordt beschreven van de relevante logboek-bron. Controleer vervolgens of dat deze niet in de conversie van de configuratie van de diagnostische gegevens en de agentconfiguratie van de bewaking gaat verloren.

#### <a name="is-data-getting-transferred"></a>Is de gegevens overgedragen?
Als u hebt gecontroleerd dat de gegevens worden lokaal ophalen vastgelegd, maar u nog steeds niet er in uw storage-account ziet, kunt u de volgende stappen:

- Controleer of dat u een juiste storage-account hebt opgegeven en dat u dit nog niet hebt geïmplementeerd sleutels voor het gegeven opslagaccount. Voor Azure Cloud Services, soms zien we dat mensen niet bijwerken `useDevelopmentStorage=true`.

- Controleer of het opgegeven opslagaccount juist is. Zorg ervoor dat u hebt geen netwerkbeperkingen die verhinderen dat de onderdelen openbare opslag eindpunten is bereikt. Een manier om dat te doen is voor externe toegang tot bij de computer en vervolgens probeert te schrijven iets naar hetzelfde opslagaccount zelf.

- Ten slotte kunt u bekijken welke fouten zijn gemeld door de monitoring Agent. De bewakingsagent schrijft de logboeken `maeventtable.tsf`, bevindt zich in [het lokale archief voor diagnostische gegevens](#log-artifacts-path). Volg de instructies in de [lokale log extractie](#local-log-extraction) sectie voor het openen van dit bestand. Probeer om te bepalen of er zijn `errors` die duiden op fouten bij het lezen van lokale bestanden wegschrijven naar opslag.

### <a name="capturing-and-archiving-logs"></a>Vastleggen en logboeken archiveren
Als u overweegt u over het contact opnemen met ondersteuning, is de eerste plaats die kunnen ze u vragen voor het verzamelen van Logboeken vanaf uw computer. U kunt tijd besparen door dit zelf doen. Voer de `CollectGuestLogs.exe` hulpprogramma [logboekpad verzameling hulpprogramma](#log-artifacts-path). Genereert een ZIP-bestand met alle relevante Azure-Logboeken in dezelfde map.

## <a name="diagnostics-data-tables-not-found"></a>Diagnostische gegevenstabellen is niet gevonden
De tabellen in Azure storage die ETW-gebeurtenissen bevatten, worden met de naam met behulp van de volgende code:

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
Deze code wordt gegenereerd vier tabellen:

| Gebeurtenis | Tabelnaam |
| --- | --- |
| provider=”prov1” &lt;Event id=”1” /&gt; |WADEvent + MD5("prov1") + "1" |
| Provider = "prov1" &lt;gebeurtenis-id = "2" eventDestination = "dest1" /&gt; |WADdest1 |
| provider=”prov1” &lt;DefaultEvents /&gt; |WADDefault+MD5("prov1") |
| provider=”prov2” &lt;DefaultEvents eventDestination=”dest2” /&gt; |WADdest2 |

## <a name="references"></a>Verwijzingen

### <a name="how-to-check-diagnostics-extension-configuration"></a>Het controleren van de configuratie van de Diagnostics-extensie
De eenvoudigste manier om te controleren of de configuratie van de extensie is naar [Azure Resource Explorer](http://resources.azure.com), en ga vervolgens naar de virtuele machine of cloud service waar u de Azure Diagnostics-extensie (IaaSDiagnostics / PaaDiagnostics) is.

U kunt ook extern bureaublad op de machine en bekijk de Azure Diagnostics-configuratiebestand dat wordt beschreven in de [melden artefacten padsectie](#log-artifacts-path).

Zoek in beide gevallen **Microsoft.Azure.Diagnostics**, en vervolgens voor de **xmlCfg** of **WadCfg** veld.

Als u op een virtuele machine zoekt en de **WadCfg** veld niet aanwezig is, betekent dit dat de configuratie in JSON-indeling. Als de **xmlCfg** veld niet aanwezig is, betekent dit dat de configuratie is in XML en base64-gecodeerd. U moet [dit decoderen](https://www.bing.com/search?q=base64+decoder) om te zien van de XML-bestand dat is geladen met diagnostische gegevens.

Voor de rol van cloud service, als u de configuratie van de schijf, kiest de gegevens zijn base64-gecodeerd, dus u moet [dit decoderen](https://www.bing.com/search?q=base64+decoder) om te zien van de XML-bestand dat is geladen met diagnostische gegevens.

### <a name="azure-diagnostics-plugin-exit-codes"></a>Azure Diagnostics-invoegtoepassing afsluitcodes definiëren
De invoegtoepassing wordt de volgende afsluitcodes geretourneerd:

| Afsluitcode | Description |
| --- | --- |
| 0 |Geslaagd. |
| -1 |Algemene fout. |
| -2 |De rcf kan bestand niet laden.<p>Deze interne fout moet alleen gebeuren als het startprogramma voor guest agent-invoegtoepassing handmatig onjuist wordt aangeroepen op de virtuele machine. |
| -3 |Kan het configuratiebestand van de diagnostische gegevens niet laden.<p><p>Oplossing: Dit wordt veroorzaakt door een configuratiebestand niet doorgegeven schemavalidatie is. De oplossing is voor een configuratiebestand dat compatibel is met het schema. |
| -4 |De map lokale resource wordt al gebruikt door een ander exemplaar van de bewakingsagent diagnostische gegevens.<p><p>Oplossing: Geef een andere waarde voor **LocalResourceDirectory**. |
| -6 |De invoegtoepassing startprogramma voor de Gast-agent heeft geprobeerd diagnostische gegevens starten met een ongeldige opdrachtregel.<p><p>Deze interne fout moet alleen gebeuren als het startprogramma voor guest agent-invoegtoepassing handmatig onjuist wordt aangeroepen op de virtuele machine. |
| -10 |De Diagnostics-invoegtoepassing is afgesloten met een onverwerkte uitzondering. |
| -11 |De gastagent is niet verantwoordelijk voor het starten en controleren van de bewakingsagent maken van het proces.<p><p>Oplossing: Controleer of voldoende systeembronnen zijn beschikbaar voor nieuwe processen te starten.<p> |
| -101 |Ongeldige argumenten bij het aanroepen van de invoegtoepassing diagnostische gegevens.<p><p>Deze interne fout moet alleen gebeuren als het startprogramma voor guest agent-invoegtoepassing handmatig onjuist wordt aangeroepen op de virtuele machine. |
| -102 |Het proces van de invoegtoepassing is niet geïnitialiseerd.<p><p>Oplossing: Controleer of voldoende systeembronnen zijn beschikbaar voor nieuwe processen te starten. |
| -103 |Het proces van de invoegtoepassing is niet geïnitialiseerd. Het is specifiek, kan de logger-object niet maken.<p><p>Oplossing: Controleer of voldoende systeembronnen zijn beschikbaar voor nieuwe processen te starten. |
| -104 |Kan niet laden van de rcf-bestand dat is opgegeven door de gastagent.<p><p>Deze interne fout moet alleen gebeuren als het startprogramma voor guest agent-invoegtoepassing handmatig onjuist wordt aangeroepen op de virtuele machine. |
| -105 |De invoegtoepassing diagnostische gegevens kan het configuratiebestand van de diagnostische gegevens niet openen.<p><p>Deze interne fout moet alleen gebeuren als de Diagnostics-invoegtoepassing handmatig onjuist wordt aangeroepen op de virtuele machine. |
| -106 |Kan het configuratiebestand van de diagnostische gegevens niet lezen.<p><p>Dit wordt veroorzaakt door een configuratiebestand niet doorgegeven schemavalidatie is. <br><br>Oplossing: Geef een configuratiebestand dat compatibel is met het schema. Zie voor meer informatie, [het controleren van de configuratie van diagnostische gegevens van de extensie](#how-to-check-diagnostics-extension-configuration). |
| -107 |De bron directory pas voor de bewakingsagent is ongeldig.<p><p>Deze interne fout moet alleen gebeuren als de bewakingsagent handmatig onjuist wordt aangeroepen op de virtuele machine.</p> |
| -108 |Kan niet converteren van het configuratiebestand van de diagnostische gegevens in het configuratiebestand van de monitoring agent.<p><p>Deze interne fout moet alleen gebeuren als de Diagnostics-invoegtoepassing handmatig wordt aangeroepen met een ongeldige configuratie-bestand. |
| -110 |Configuratiefout van de algemene diagnostische gegevens.<p><p>Deze interne fout moet alleen gebeuren als de Diagnostics-invoegtoepassing handmatig wordt aangeroepen met een ongeldige configuratie-bestand. |
| -111 |Kan niet starten van de bewakingsagent.<p><p>Oplossing: Controleer of dat er onvoldoende systeembronnen beschikbaar zijn. |
| -112 |Algemene fout |

### <a name="local-log-extraction"></a>Lokale log extractie
De monitoring agent verzamelt logboeken en artefacten als `.tsf` bestanden. De `.tsf` bestand kan niet worden gelezen, maar u kunt converteren naar een `.csv` als volgt:

```
<Azure diagnostics extension package>\Monitor\x64\table2csv.exe <relevantLogFile>.tsf
```
Een nieuw bestand met de naam `<relevantLogFile>.csv` wordt gemaakt in hetzelfde pad hebben als de bijbehorende `.tsf` bestand.

>[!NOTE]
> U hoeft dit hulpprogramma uitvoeren op basis van de belangrijkste .tsf-bestand (bijvoorbeeld PerformanceCountersTable.tsf). De bijbehorende bestanden (bijvoorbeeld PerformanceCountersTables_\*\*001.tsf, PerformanceCountersTables_\*\*002.tsf, enzovoort) worden automatisch verwerkt.

### <a name="more-about-missing-trace-logs"></a>Meer informatie over ontbrekende traceerlogboeken

>[!NOTE]
> De volgende informatie is voornamelijk in Azure Cloud Services, tenzij u de DiagnosticsMonitorTraceListener op een toepassing die wordt uitgevoerd op uw IaaS-VM hebt geconfigureerd.

- Zorg ervoor dat de **DiagnosticMonitorTraceListener** is geconfigureerd in het bestand web.config of app.config.  Dit is standaard in cloudserviceprojecten geconfigureerd. Evenwel sommige klanten opmerkingen plaatsen uit, waardoor de trace-instructies moeten niet worden verzameld door diagnostische gegevens.

- Als Logboeken niet ophalen vanuit geschreven worden het **OnStart** of **uitvoeren** methode, zorg ervoor dat de **DiagnosticMonitorTraceListener** is in het app.config.  Standaard is deze in het bestand web.config, maar die alleen van toepassing op code die wordt uitgevoerd binnen w3wp.exe. Daarom moet u deze in app.config het vastleggen van traces die worden uitgevoerd in WaIISHost.exe.

- Zorg ervoor dat u gebruikmaakt van **Diagnostics.Trace.TraceXXX** in plaats van **Diagnostics.Debug.WriteXXX.** De foutopsporingsinstructies worden verwijderd uit een release-build.

- Zorg ervoor dat de gecompileerde code bevat de **Diagnostics.Trace regels** (Reflector, ildasm of ILSpy gebruiken om te controleren). **Diagnostics.Trace** opdrachten zijn verwijderd uit het gecompileerde binaire bestand, tenzij u het symbool TRACE-voorwaardelijke compilatie. Dit is een veelvoorkomend probleem dat optreedt wanneer u msbuild om een project te bouwen.   

## <a name="known-issues-and-mitigations"></a>Bekende problemen en oplossingen
Hier volgt een lijst met bekende problemen met bekende oplossingen:

**1. .NET 4.5-afhankelijkheid**

Windows Azure Diagnostics-extensie heeft een runtime-afhankelijkheid framework .NET 4.5 of hoger. Op het moment van schrijven, alle machines die zijn ingericht voor Azure Cloud Services, evenals alle officiële installatiekopieën die zijn gebaseerd op Azure virtual machines, .NET 4.5 of hoger hebben geïnstalleerd.

Het is nog steeds mogelijk u een situatie tegenkomt wanneer u probeert uit te voeren van Windows Azure Diagnostics-extensie op een computer die geen .NET 4.5 of hoger. Dit gebeurt wanneer u de computer van een oude installatiekopie of een momentopname maakt, of als u uw eigen aangepaste schijf.

Dit in het algemeen als een afsluitcode manifesten **255** bij het uitvoeren van **DiagnosticsPluginLauncher.exe.** Fout doet zich voor vanwege de volgende niet-verwerkte uitzondering:
```
System.IO.FileLoadException: Could not load file or assembly 'System.Threading.Tasks, Version=1.5.11.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies
```

**Risicobeperking:** Installeer .NET 4.5 of hoger op uw computer.

**2. Prestatiegegevens van prestatiemeteritems is beschikbaar in de opslag, maar niet wordt weergegeven in de portal**

De portal-ervaring in de virtuele machines worden bepaalde prestatiemeteritems standaard weergegeven. Als u de prestatiemeteritems niet ziet en u weet dat de gegevens wordt gegenereerd omdat deze beschikbaar in de opslag is, controleert u het volgende:

- De gegevens in storage heeft of namen van prestatiemeteritems in het Engels. Als de namen van prestatiemeteritems niet in het Engels, niet de portal grafiek met metrische gegevens kunt u deze zien. **Risicobeperking**: Taal van de machine naar het Engels voor systeemaccounts wijzigen. Om dit te doen, selecteert u **Configuratiescherm** > **regio** > **Administrative** > **instellingen**. Schakel vervolgens **scherm en systeemaccounts Welkom** zodat de aangepaste taal niet op het systeem-account toegepast wordt.

- Als u jokertekens (\*) in de namen van prestatiemeteritems, de portal wordt niet kunnen correleren van de teller geconfigureerd en die worden verzameld als de prestatiemeteritems worden verzonden naar de Azure Storage-sink. **Risicobeperking**: Om te controleren of u kunt jokertekens gebruiken en de portal Vouw de (\*), de prestatiemeteritems te routeren de ['Azure Monitor'-sink](diagnostics-extension-schema.md#diagnostics-extension-111).


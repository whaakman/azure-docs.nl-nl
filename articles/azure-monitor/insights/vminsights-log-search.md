---
title: Hoe u Logboeken voor query's van Azure Monitor voor virtuele machines (preview) | Microsoft Docs
description: Azure Monitor voor virtuele machines oplossing verzamelt metrische gegevens en logboekgegevens en in dit artikel beschrijft de records en bevat voorbeeldquery's.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2019
ms.author: magoedte
ms.openlocfilehash: 12f8b3d9dd461dc5d09d76245aa02f0e1cefc343
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58188965"
---
# <a name="how-to-query-logs-from-azure-monitor-for-vms-preview"></a>Hoe u Logboeken voor query's van Azure Monitor voor virtuele machines (preview)
Azure Monitor voor virtuele machines verzamelt prestaties en metrische verbindingsgegevens, computer- en inventarisgegevens proces en informatie over de status en stuurt deze door naar de Log Analytics-werkruimte in Azure Monitor.  Deze gegevens zijn beschikbaar voor [query](../../azure-monitor/log-query/log-query-overview.md) in Azure Monitor. U kunt deze gegevens toepassen op scenario's met migratieplanning, analyse, detectie en het oplossen van prestaties op aanvraag.

## <a name="map-records"></a>Records worden toegewezen
Een record wordt gegenereerd per uur voor elke unieke computernaam en het proces, naast de records die worden gegenereerd wanneer een proces of de computer wordt gestart of toegevoegd aan Azure Monitor voor de functie voor virtuele machines toewijzen is. Deze records hebben de eigenschappen in de volgende tabellen. De velden en waarden in de ServiceMapComputer_CL-gebeurtenissen toewijzen aan velden van de bron van de Machine in de ServiceMap Azure Resource Manager-API. De velden en waarden in de gebeurtenissen ServiceMapProcess_CL worden toegewezen aan de velden van de proces-resource in de ServiceMap Azure Resource Manager-API. Het veld ResourceName_s komt overeen met het naamveld in de overeenkomende Resource Manager-resource. 

Er zijn intern gegenereerde eigenschappen die u gebruiken kunt om unieke processen en computers te identificeren:

- Computer: Gebruik *ResourceId* of *ResourceName_s* voor het aanduiden van een computer binnen een Log Analytics-werkruimte.
- Proces: Gebruik *ResourceId* voor het aanduiden van een proces in een Log Analytics-werkruimte. *ResourceName_s* uniek is binnen de context van de computer waarop het proces (MachineResourceName_s) wordt uitgevoerd 

Omdat meerdere records kunnen voor een opgegeven proces en de computer in een opgegeven tijdperiode, kunnen meer dan één record voor dezelfde computer of proces-query's retourneren. Zodat alleen de meest recente record toevoegen ' | Ontdubbeling ResourceId"aan de query.

### <a name="connections-and-ports"></a>Verbindingen en poorten
De functie metrische verbindingsgegevens bevat twee nieuwe tabellen in Azure Monitor logs - VMConnection en VMBoundPort. Deze tabellen bevatten informatie over de verbindingen voor een virtuele machine (binnenkomend en uitgaand), evenals de server openen/actief zijn op deze poorten. ConnectionMetrics worden ook beschikbaar gesteld via API's die de mogelijkheid om op te halen van specifieke metrische gegevens gedurende een bepaalde periode. TCP-verbindingen die voortvloeien uit *accepteren* op luisterende socket inkomend, terwijl deze die zijn gemaakt door zijn *verbinding* naar een bepaald IP en poort zijn uitgaande. De richting van een verbinding wordt vertegenwoordigd door de eigenschap Direction, die kan worden ingesteld op **inkomende** of **uitgaande**. 

Records in deze tabellen zijn gegenereerd op basis van gegevens die zijn gerapporteerd door de Agent voor afhankelijkheden. Elke record vertegenwoordigt een waarneming gedurende een tijdsinterval van 1 minuut. De eigenschap TimeGenerated geeft het begin van het tijdsinterval. Elke record bevat informatie om te identificeren van de respectieve entiteit, dat wil zeggen, de verbinding of poort, evenals metrische gegevens die zijn gekoppeld aan die entiteit. Op dit moment wordt alleen netwerkactiviteit die wordt uitgevoerd met behulp van de TCP via IPv4 gerapporteerd. 

#### <a name="common-fields-and-conventions"></a>Algemene velden, en overeenkomsten 
De volgende velden en conventies van toepassing op zowel VMConnection en VMBoundPort: 

- Computer: FQDN-naam van machine-rapportage 
- AgentID: De unieke id voor een machine met de Log Analytics-agent  
- Apparaat: De naam van de Azure Resource Manager-resource voor de machine door ServiceMap beschikbaar gesteld. Het is van het formulier *m-{GUID}*, waarbij *GUID* is de GUID als AgentID  
- Proces: De naam van de Azure Resource Manager-resource voor het proces door ServiceMap beschikbaar gesteld. Het is van het formulier *p-{hex-tekenreeks}*. Proces is uniek binnen het bereik van een machine en voor het genereren van unieke proces-ID op computers, velden Machine en het proces te combineren. 
- ProcessName: Naam van uitvoerbaar bestand van het proces voor rapportage.
- Alle IP-adressen zijn tekenreeksen in IPv4-notatie, bijvoorbeeld *13.107.3.160* 

Voor het beheren van kosten en complexiteit, vertegenwoordigen verbinding records geen afzonderlijke fysieke netwerkverbindingen. Verbindingen van meerdere fysieke netwerk zijn gegroepeerd in een logische verbinding, die vervolgens wordt weergegeven in de bijbehorende tabel.  Betekenis, registreert in *VMConnection* tabel staan voor een logische groepering en niet de afzonderlijke fysieke verbindingen die zijn waargenomen. Fysieke netwerkverbinding delen dezelfde waarde voor de volgende kenmerken tijdens een opgegeven interval van één minuut worden samengevoegd in één logische record in *VMConnection*. 

| Eigenschap | Description |
|:--|:--|
|Richting |Richting van de verbinding met de waarde is *inkomende* of *uitgaande* |
|Machine |De FQDN-naam van de computer |
|Verwerken |Identiteit van het proces of groepen van processen, de verbinding initiëren/accepteren |
|SourceIp |IP-adres van de bron |
|DestinationIp |IP-adres van de bestemming |
|DestinationPort |Poortnummer van de bestemming |
|Protocol |Het protocol dat wordt gebruikt voor de verbinding.  Waarden is *tcp*. |

Voor het account voor de impact van groepering, informatie over het aantal gegroepeerde fysieke verbindingen vindt u in de volgende eigenschappen van de record:

| Eigenschap | Description |
|:--|:--|
|LinksEstablished |Het aantal fysieke netwerkverbindingen die zijn gemaakt tijdens de rapportage venster |
|LinksTerminated |Het aantal fysieke netwerkverbindingen die tijdens de rapportage venster zijn beëindigd |
|LinksFailed |Het aantal fysieke netwerkverbindingen die zijn mislukt tijdens het rapportage venster. Deze informatie is momenteel alleen beschikbaar voor uitgaande verbindingen. |
|LinksLive |Het aantal fysieke netwerkverbindingen die aan het einde van de rapportage venster geopend waren|

#### <a name="metrics"></a>Metrische gegevens

Naast de verbinding aantal metrische gegevens, informatie over de hoeveelheid gegevens verzonden en ontvangen op een bepaalde logische verbinding of netwerkpoort zijn ook opgenomen in de volgende eigenschappen van de record:

| Eigenschap | Description |
|:--|:--|
|BytesSent |Totaal aantal bytes dat is verzonden tijdens het rapportage venster |
|BytesReceived |Totaal aantal bytes dat is ontvangen tijdens het rapportage venster |
|Antwoorden |Het aantal antwoorden is waargenomen tijdens het rapportage venster. 
|ResponseTimeMax |De grootste reactietijd (milliseconden) waargenomen tijdens het rapportage venster. Als er geen waarde is de eigenschap leeg.|
|ResponseTimeMin |De kleinste reactietijd (milliseconden) waargenomen tijdens het rapportage venster. Als er geen waarde is de eigenschap leeg.|
|ResponseTimeSum |De som van alle reactietijden (milliseconden) die is waargenomen tijdens het rapportage venster. Als er geen waarde is de eigenschap leeg.|

Het derde type gegevens dat wordt gerapporteerd reactietijd is: hoe lang bij een aanroeper wachten op een aanvraag die wordt verzonden via een verbinding moet worden verwerkt en beantwoord door het externe eindpunt uitgeven. De reactietijd gerapporteerd is een schatting van de waarde true reactietijd van de onderliggende toepassingsprotocol. Dit wordt berekend op basis van heuristiek op basis van de observatie van de stroom van gegevens tussen de bron en doel-end van een fysieke netwerkverbinding. Conceptueel gezien, is het verschil tussen het moment dat de laatste byte van een aanvraag verlaat de afzender, en de tijd wanneer de laatste byte van het antwoord terug naar het binnenkomt. Deze twee tijdstempels worden gebruikt om de aanvraag- en gebeurtenissen op een bepaalde fysieke verbinding afbakenen. Het verschil tussen deze vertegenwoordigt de reactietijd van een enkele aanvraag. 

In deze eerste versie van deze functie is onze algoritme een benadering die met verschillende mate van succes, afhankelijk van de werkelijke toepassingsprotocol gebruikt voor een bepaalde netwerkverbinding werken. Bijvoorbeeld, de huidige methode werkt goed voor de request response-gebaseerde protocollen zoals HTTP (S), maar niet werken met één richting of message queue-protocollen.

Hier volgen enkele belangrijke punten om te overwegen:

1. Als een proces op hetzelfde IP-adres, maar meerdere netwerkinterfaces accepteert, kunt u een afzonderlijke record voor elke interface worden gerapporteerd. 
2. Records met jokertekens IP-adres bevat geen activiteiten. Ze zijn opgenomen om weer te geven van het feit dat een poort op de machine geopend voor inkomend verkeer is.
3. Als u wilt verkleinen uitgebreidheid en gegevensvolume, wordt records met jokertekens IP-adres worden weggelaten wanneer er een overeenkomende record (voor de hetzelfde proces, poort en protocol) met een specifiek IP-adres. Wanneer een jokerteken IP-adresrecord wordt weggelaten, wordt de record, eigenschap IsWildcardBind met specifieke IP-adres, worden ingesteld op 'True' om aan te geven dat de poort beschikbaar wordt gemaakt via elke interface van de rapportage-machine.
4. Poorten die afhankelijk zijn van alleen op een specifieke interface hebt ingesteld op IsWildcardBind *False*.

#### <a name="naming-and-classification"></a>Naamgeving en classificatie
Het IP-adres van het externe uiteinde van een verbinding is voor het gemak opgenomen in de eigenschap RemoteIp. Voor binnenkomende verbindingen, RemoteIp is hetzelfde als SourceIp, terwijl voor uitgaande verbindingen, is hetzelfde als DestinationIp. De eigenschap RemoteDnsCanonicalNames vertegenwoordigt de DNS-namen canonieke gerapporteerd door de virtuele machine voor RemoteIp. De eigenschappen RemoteDnsQuestions en RemoteClassification zijn gereserveerd voor toekomstig gebruik. 

#### <a name="geolocation"></a>Geolocatie
*VMConnection* omvat ook geolocatie-informatie voor het externe einde van elke verbindingsrecord in de volgende eigenschappen van de record: 

| Eigenschap | Description |
|:--|:--|
|RemoteCountry |De naam van het betreffende land RemoteIp hosten.  Bijvoorbeeld, *Verenigde Staten* |
|RemoteLatitude |De breedtegraad geolocatie. Bijvoorbeeld, *47.68* |
|RemoteLongitude |De lengtegraad geolocatie. Bijvoorbeeld, *-122.12* |

#### <a name="malicious-ip"></a>Schadelijk IP
Elke eigenschap RemoteIp in *VMConnection* tabel aan de hand van IP-adressen is ingeschakeld met bekende schadelijke activiteiten. Als de RemoteIp wordt geïdentificeerd als schadelijk de volgende eigenschappen worden ingevuld (ze zijn leeg is, wanneer het IP-adres wordt niet als schadelijk beschouwd) in de volgende eigenschappen van de record:

| Eigenschap | Description |
|:--|:--|
|MaliciousIp |Het adres RemoteIp |
|IndicatorThreadType |Threat indicator gedetecteerd is een van de volgende waarden *Botnet*, *C2*, *CryptoMining*, *Darknet*, *DDos* , *MaliciousUrl*, *Malware*, *Phishing*, *Proxy*, *pua's*, *Watchlist*.   |
|Description |Beschrijving van de waargenomen bedreiging. |
|TLPLevel |Niveau van stoplicht Protocol (TLP) is een van de gedefinieerde waarden, *wit*, *groen*, *oranje*, *Red*. |
|Betrouwbaarheid |Waarden zijn *0-100*. |
|Severity |Waarden zijn *0 – 5*, waarbij *5* is het meest ernstige en *0* is helemaal niet ernstig. Standaardwaarde is *3*.  |
|FirstReportedDateTime |De eerste keer dat de provider meldt de indicator. |
|LastReportedDateTime |De laatste keer dat de indicator is gezien door Interflow. |
|IsActive |Geeft aan dat indicatoren zijn uitgeschakeld met *waar* of *False* waarde. |
|ReportReferenceLink |Koppelingen naar rapporten met betrekking tot een bepaalde zichtbaar zijn. |
|AdditionalInformation |Bevat aanvullende informatie, indien van toepassing, over de bedreiging waargenomen. |

### <a name="ports"></a>Poorten 
Poorten op een virtuele machine die actief accepteren van binnenkomend verkeer of verkeer kunnen mogelijk worden geaccepteerd, maar niet actief zijn tijdens de periode reporting worden geschreven naar de tabel VMBoundPort.  

Gegevens worden standaard niet geschreven in deze tabel. Als u wilt dat gegevens die naar deze tabel geschreven, stuur een e-mail naar vminsights@microsoft.com samen met uw werkruimte-ID en de regio van de werkruimte.   

Elke record in VMBoundPort wordt geïdentificeerd door de volgende velden: 

| Eigenschap | Description |
|:--|:--|
|Verwerken | De identiteit van proces (of groepen van processen) waarmee de poort gekoppeld is.|
|IP | IP-adres-poort (mag jokertekens IP-adres, *0.0.0.0*) |
|Poort |Het poortnummer |
|Protocol | Het protocol.  Bijvoorbeeld *tcp* of *udp* (alleen *tcp* wordt momenteel ondersteund).|
 
De identiteit een poort is afgeleid van de bovenstaande vijf velden en wordt opgeslagen in de eigenschap PortId. Deze eigenschap kan worden gebruikt om snel te vinden records voor een specifieke poort in de tijd. 

#### <a name="metrics"></a>Metrische gegevens 
Poort-records bevatten voor de verbindingen die zijn gekoppeld aan deze metrische gegevens. Op dit moment de volgende metrische gegevens worden gerapporteerd (de details voor elke metrische gegevens worden beschreven in de vorige sectie): 

- BytesSent en BytesReceived 
- LinksEstablished, LinksTerminated, LinksLive 
- ResposeTime, ResponseTimeMin, ResponseTimeMax, ResponseTimeSum 

Hier volgen enkele belangrijke punten om te overwegen:

- Als een proces op hetzelfde IP-adres, maar meerdere netwerkinterfaces accepteert, kunt u een afzonderlijke record voor elke interface worden gerapporteerd.  
- Records met jokertekens IP-adres bevat geen activiteiten. Ze zijn opgenomen om weer te geven van het feit dat een poort op de machine geopend voor inkomend verkeer is. 
- Als u wilt verkleinen uitgebreidheid en gegevensvolume, wordt records met jokertekens IP-adres worden weggelaten wanneer er een overeenkomende record (voor de hetzelfde proces, poort en protocol) met een specifiek IP-adres. Wanneer een jokerteken IP-adresrecord wordt weggelaten, de *IsWildcardBind* eigenschap voor de record met het specifieke IP-adres wordt ingesteld op *waar*.  Dit geeft aan dat de poort beschikbaar wordt gemaakt via elke interface van de rapportage-machine. 
- Poorten die afhankelijk zijn van alleen op een specifieke interface hebt ingesteld op IsWildcardBind *False*. 

### <a name="servicemapcomputercl-records"></a>ServiceMapComputer_CL records
Records met een type *ServiceMapComputer_CL* inventarisgegevens voor servers met de agent voor afhankelijkheden hebben. Deze records hebben de eigenschappen in de volgende tabel:

| Eigenschap | Description |
|:--|:--|
| Type | *ServiceMapComputer_CL* |
| SourceSystem | *OpsManager* |
| ResourceId | De unieke id voor een computer in de werkruimte |
| ResourceName_s | De unieke id voor een computer in de werkruimte |
| ComputerName_s | De FQDN-naam van de computer |
| Ipv4Addresses_s | Een lijst van de server IPv4-adressen |
| Ipv6Addresses_s | Een lijst van de server IPv6-adressen |
| DnsNames_s | Een matrix van DNS-namen |
| OperatingSystemFamily_s | Windows- of Linux |
| OperatingSystemFullName_s | De volledige naam van het besturingssysteem  |
| Bitness_s | De bitness van de machine (32-bits of 64-bits)  |
| PhysicalMemory_d | Het fysieke geheugen in MB |
| Cpus_d | Het aantal CPU 's |
| CpuSpeed_d | De CPU-snelheid in MHz|
| VirtualizationState_s | *unknown*, *physical*, *virtual*, *hypervisor* |
| VirtualMachineType_s | *Hyper-v*, *vmware*, enzovoort |
| VirtualMachineNativeMachineId_g | De ID van de VM zoals die door de hypervisor toegewezen |
| VirtualMachineName_s | De naam van de virtuele machine |
| BootTime_t | De opstarttijd |

### <a name="servicemapprocesscl-type-records"></a>Type ServiceMapProcess_CL records
Records met een type *ServiceMapProcess_CL* beschikken over inventarisgegevens voor TCP verbonden processen op servers met de agent voor afhankelijkheden. Deze records hebben de eigenschappen in de volgende tabel:

| Eigenschap | Description |
|:--|:--|
| Type | *ServiceMapProcess_CL* |
| SourceSystem | *OpsManager* |
| ResourceId | De unieke id voor een proces in de werkruimte |
| ResourceName_s | De unieke id voor een proces binnen de computer waarop deze wordt uitgevoerd|
| MachineResourceName_s | De naam van de resource van de machine |
| ExecutableName_s | De naam van het uitvoerbare bestand van proces |
| StartTime_t | De begintijd van de proces-groep |
| FirstPid_d | De eerste PID in de procesgroep |
| Description_s | De beschrijving van proces |
| CompanyName_s | De naam van het bedrijf |
| InternalName_s | De interne naam |
| ProductName_s | De naam van het product |
| ProductVersion_s | De versie van het product |
| FileVersion_s | Versie van het bestand |
| CommandLine_s | Vanaf de opdrachtregel |
| ExecutablePath _s | Het pad naar het uitvoerbare bestand |
| WorkingDirectory_s | De werkmap |
| Gebruikersnaam | Het account waaronder het proces wordt uitgevoerd |
| UserDomain | Het domein waarin het proces wordt uitgevoerd |

## <a name="sample-log-searches"></a>Voorbeeldzoekopdrachten in logboeken

### <a name="list-all-known-machines"></a>Lijst van alle bekende machines
```kusto
ServiceMapComputer_CL | summarize arg_max(TimeGenerated, *) by ResourceId`
```

### <a name="when-was-the-vm-last-rebooted"></a>Wanneer is de virtuele machine laatst opnieuw opgestart
```kusto
let Today = now(); ServiceMapComputer_CL | extend DaysSinceBoot = Today - BootTime_t | summarize by Computer, DaysSinceBoot, BootTime_t | sort by BootTime_t asc`
```

### <a name="summary-of-azure-vms-by-image-location-and-sku"></a>Overzicht van Azure-VM's per afbeelding, de locatie en SKU
```kusto
ServiceMapComputer_CL | where AzureLocation_s != "" | summarize by ComputerName_s, AzureImageOffering_s, AzureLocation_s, AzureImageSku_s`
```

### <a name="list-the-physical-memory-capacity-of-all-managed-computers"></a>Een lijst van de capaciteit van het fysieke geheugen van alle beheerde computers.
```kusto
ServiceMapComputer_CL | summarize arg_max(TimeGenerated, *) by ResourceId | project PhysicalMemory_d, ComputerName_s`
```

### <a name="list-computer-name-dns-ip-and-os"></a>Computernaam van de lijst, DNS, IP- en OS.
```kusto
ServiceMapComputer_CL | summarize arg_max(TimeGenerated, *) by ResourceId | project ComputerName_s, OperatingSystemFullName_s, DnsNames_s, Ipv4Addresses_s`
```

### <a name="find-all-processes-with-sql-in-the-command-line"></a>Alle processen met 'sql' niet vinden in de opdrachtregel
```kusto
ServiceMapProcess_CL | where CommandLine_s contains_cs "sql" | summarize arg_max(TimeGenerated, *) by ResourceId`
```

### <a name="find-a-machine-most-recent-record-by-resource-name"></a>Een virtuele machine (meest recente record) vinden op resourcenaam
```kusto
search in (ServiceMapComputer_CL) "m-4b9c93f9-bc37-46df-b43c-899ba829e07b" | summarize arg_max(TimeGenerated, *) by ResourceId`
```

### <a name="find-a-machine-most-recent-record-by-ip-address"></a>Een virtuele machine (meest recente record) vinden door IP-adres
```kusto
search in (ServiceMapComputer_CL) "10.229.243.232" | summarize arg_max(TimeGenerated, *) by ResourceId`
```

### <a name="list-all-known-processes-on-a-specified-machine"></a>Lijst van alle bekende processen op een opgegeven computer
```kusto
ServiceMapProcess_CL | where MachineResourceName_s == "m-559dbcd8-3130-454d-8d1d-f624e57961bc" | summarize arg_max(TimeGenerated, *) by ResourceId`
```

### <a name="list-all-computers-running-sql-server"></a>Lijst van alle computers met SQL Server
```kusto
ServiceMapComputer_CL | where ResourceName_s in ((search in (ServiceMapProcess_CL) "\*sql\*" | distinct MachineResourceName_s)) | distinct ComputerName_s`
```

### <a name="list-all-unique-product-versions-of-curl-in-my-datacenter"></a>Lijst van alle unieke productversies van curl in mijn datacenter
```kusto
ServiceMapProcess_CL | where ExecutableName_s == "curl" | distinct ProductVersion_s`
```

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>Maak een computergroep van alle computers waarop CentOS wordt uitgevoerd
```kusto
ServiceMapComputer_CL | where OperatingSystemFullName_s contains_cs "CentOS" | distinct ComputerName_s`
```

### <a name="bytes-sent-and-received-trends"></a>Bytes verzonden en ontvangen van trends
```kusto
VMConnection | summarize sum(BytesSent), sum(BytesReceived) by bin(TimeGenerated,1hr), Computer | order by Computer desc | render timechart`
```

### <a name="which-azure-vms-are-transmitting-the-most-bytes"></a>Welke Azure-VM's zijn het meest bytes verzenden
```kusto
VMConnection | join kind=fullouter(ServiceMapComputer_CL) on $left.Computer == $right.ComputerName_s | summarize count(BytesSent) by Computer, AzureVMSize_s | sort by count_BytesSent desc`
```

### <a name="link-status-trends"></a>Koppeling status trends
```kusto
VMConnection | where TimeGenerated >= ago(24hr) | where Computer == "acme-demo" | summarize  dcount(LinksEstablished), dcount(LinksLive), dcount(LinksFailed), dcount(LinksTerminated) by bin(TimeGenerated, 1h) | render timechart`
```

### <a name="connection-failures-trend"></a>Trend van mislukte verbinding
```kusto
VMConnection | where Computer == "acme-demo" | extend bythehour = datetime_part("hour", TimeGenerated) | project bythehour, LinksFailed | summarize failCount = count() by bythehour | sort by bythehour asc | render timechart`
```

### <a name="bound-ports"></a>Gebonden poorten
```kusto
VMBoundPort
| where TimeGenerated >= ago(24hr)
| where Computer == 'admdemo-appsvr'
| distinct Port, ProcessName
```

### <a name="number-of-open-ports-across-machines"></a>Het aantal poorten openen tussen computers
```kusto
VMBoundPort
| where Ip != "127.0.0.1"
| summarize by Computer, Machine, Port, Protocol
| summarize OpenPorts=count() by Computer, Machine
| order by OpenPorts desc
```

### <a name="score-processes-in-your-workspace-by-the-number-of-ports-they-have-open"></a>Score processen in uw werkruimte door het aantal poorten dat ze hebben openen
```kusto
VMBoundPort
| where Ip != "127.0.0.1"
| summarize by ProcessName, Port, Protocol
| summarize OpenPorts=count() by ProcessName
| order by OpenPorts desc
```

### <a name="aggregate-behavior-for-each-port"></a>Cumulatieve gedrag voor elke poort
Deze query kan vervolgens worden gebruikt om poorten te beoordelen door activiteit, bijvoorbeeld poorten met het meeste inkomende/uitgaande verkeer en poorten met de meeste verbindingen
```kusto
// 
VMBoundPort
| where Ip != "127.0.0.1"
| summarize BytesSent=sum(BytesSent), BytesReceived=sum(BytesReceived), LinksEstablished=sum(LinksEstablished), LinksTerminated=sum(LinksTerminated), arg_max(TimeGenerated, LinksLive) by Machine, Computer, ProcessName, Ip, Port, IsWildcardBind
| project-away TimeGenerated
| order by Machine, Computer, Port, Ip, ProcessName
```

### <a name="summarize-the-outbound-connections-from-a-group-of-machines"></a>Samenvatting van de uitgaande verbindingen van een groep machines
```kusto
// the machines of interest
let machines = datatable(m: string) ["m-82412a7a-6a32-45a9-a8d6-538354224a25"];
// map of ip to monitored machine in the environment
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
// all connections to/from the machines of interest
let out=materialize(VMConnection
| where Machine in (machines)
| summarize arg_max(TimeGenerated, *) by ConnectionId);
// connections to localhost augmented with RemoteMachine
let local=out
| where RemoteIp startswith "127."
| project ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine=Machine;
// connections not to localhost augmented with RemoteMachine
let remote=materialize(out
| where RemoteIp !startswith "127."
| join kind=leftouter (ips) on $left.RemoteIp == $right.ips
| summarize by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine=MonitoredMachine);
// the remote machines to/from which we have connections
let remoteMachines = remote | summarize by RemoteMachine;
// all augmented connections
(local)
| union (remote)
//Take all outbound records but only inbound records that come from either //unmonitored machines or monitored machines not in the set for which we are computing dependencies.
| where Direction == 'outbound' or (Direction == 'inbound' and RemoteMachine !in (machines))
| summarize by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine
// identify the remote port
| extend RemotePort=iff(Direction == 'outbound', DestinationPort, 0)
// construct the join key we'll use to find a matching port
| extend JoinKey=strcat_delim(':', RemoteMachine, RemoteIp, RemotePort, Protocol)
// find a matching port
| join kind=leftouter (VMBoundPort 
| where Machine in (remoteMachines) 
| summarize arg_max(TimeGenerated, *) by PortId 
| extend JoinKey=strcat_delim(':', Machine, Ip, Port, Protocol)) on JoinKey
// aggregate the remote information
| summarize Remote=makeset(iff(isempty(RemoteMachine), todynamic('{}'), pack('Machine', RemoteMachine, 'Process', Process1, 'ProcessName', ProcessName1))) by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol
```

## <a name="next-steps"></a>Volgende stappen
* Als u niet bekend bent met Logboeken-query's schrijven in Azure Monitor, raadpleegt u [over het gebruik van Log Analytics](../../azure-monitor/log-query/get-started-portal.md) in Azure portal om te schrijven logboeken-query's.
* Meer informatie over [zoekquery's schrijven](../../azure-monitor/log-query/search-queries.md).

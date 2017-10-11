---
title: Prestatiemeteritems gebruiken in Azure Diagnostics | Microsoft Docs
description: Prestatiemeteritems van Azure-cloudservices of virtuele machine gebruiken om te zoeken van knelpunten en afstemmen van de prestaties.
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: tysonn
ms.assetid: fc4c61e9-d49d-4ed9-a32c-b91cdf851909
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/29/2016
ms.author: robb
ms.openlocfilehash: 2cf765cb034725199127c547a9b8b997a4a6089c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-use-performance-counters-in-an-azure-application"></a>Maken en gebruiken van prestatie-items in een Azure-toepassing
In dit artikel beschrijft de voordelen van en hoe prestatiemeteritems in uw Azure-toepassing te plaatsen. U kunt deze gebruiken voor het verzamelen van gegevens, knelpunten vinden en systeem- en -prestaties afstemmen.

Prestatie-items beschikbaar voor Windows Server, IIS en ASP.NET kunnen ook worden verzameld en gebruikt om te bepalen van de status van uw Azure-web-functies, werkrollen en virtuele Machines. U kunt ook maken en gebruiken van aangepaste prestatiemeteritems.  

U kunt gegevens van prestatiemeteritems controleren

1. Rechtstreeks op de toepassingshost met het hulpprogramma Prestatiemeter is toegankelijk via Extern bureaublad
2. Met System Center Operations Manager via de Azure Management Pack
3. Met andere controleprogramma's die toegang hebben tot de diagnostische gegevens overgebracht naar Azure-opslag. Zie [Store en diagnostische gegevens weergeven in Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) voor meer informatie.  

Voor meer informatie over het controleren van de prestaties van uw toepassing in de [Azure-portal](http://portal.azure.com/), Zie [hoe Monitor Cloudservices](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).

Zie voor meer gedetailleerde informatie over het maken van een logboekregistratie en tracering strategie en met diagnostische gegevens en andere technieken voor het oplossen van problemen en Azure-toepassingen te optimaliseren, [Best Practices probleemoplossing voor Azure-toepassingen ontwikkelen](https://msdn.microsoft.com/library/azure/hh771389.aspx).

## <a name="enable-performance-counter-monitoring"></a>Schakel de bewaking van prestatiemeteritems
Prestatiemeteritems zijn standaard niet ingeschakeld. Uw toepassing of een taak starten van de moet de standaardconfiguratie voor het agent van diagnostische gegevens om op te nemen van de specifieke prestatiemeteritems die u wilt controleren voor elk rolexemplaar wijzigen.

### <a name="performance-counters-available-for-microsoft-azure"></a>Prestatie-items voor Microsoft Azure
Azure biedt een subset van de beschikbare prestatiemeteritems voor Windows Server, IIS en de ASP.NET-stack. De volgende tabel bevat enkele van de prestatiemeteritems met name van belang voor Azure-toepassingen.

| Prestatiemeteritem-categorie: Object (exemplaar) | Naam van het meteritem | Naslaginformatie |
| --- | --- | --- |
| .NET CLR-uitzonderingen (*globale*) |# Uitzonderingen veroorzaakt per seconde |Uitzondering-prestatiemeteritems |
| .NET CLR-geheugen (*globale*) |% Tijd in % |Prestatiemeteritems voor geheugen |
| ASP.NET |Toepassing opnieuw wordt gestart |Prestatiemeteritems voor ASP.NET |
| ASP.NET |Uitvoertijd van aanvraag |Prestatiemeteritems voor ASP.NET |
| ASP.NET |Verbroken aanvragen |Prestatiemeteritems voor ASP.NET |
| ASP.NET |Opnieuw gestarte werkprocessen |Prestatiemeteritems voor ASP.NET |
| ASP.NET-toepassingen (**totale**) |Totaal aantal aanvragen |Prestatiemeteritems voor ASP.NET |
| ASP.NET-toepassingen (**totale**) |Aanvragen per seconde |Prestatiemeteritems voor ASP.NET |
| ASP.NET v4.0.30319 |Uitvoertijd van aanvraag |Prestatiemeteritems voor ASP.NET |
| ASP.NET v4.0.30319 |Wachttijd van aanvraag |Prestatiemeteritems voor ASP.NET |
| ASP.NET v4.0.30319 |Huidige aanvragen |Prestatiemeteritems voor ASP.NET |
| ASP.NET v4.0.30319 |Aanvragen in wachtrij |Prestatiemeteritems voor ASP.NET |
| ASP.NET v4.0.30319 |Aanvragen dat is geweigerd |Prestatiemeteritems voor ASP.NET |
| Geheugen |Beschikbare megabytes (MB) |Prestatiemeteritems voor geheugen |
| Geheugen |Toegewezen Bytes |Prestatiemeteritems voor geheugen |
| Processor(_Totaal) |Percentage processortijd |Prestatiemeteritems voor ASP.NET |
| TCPv4 |Verbindingsfouten |TCP-Object |
| TCPv4 |Verbindingen |TCP-Object |
| TCPv4 |Verbindingen opnieuw instellen |TCP-Object |
| TCPv4 |Segmenten verzonden per seconde |TCP-Object |
| Netwerk Interface(*) |Ontvangen bytes per seconde |Netwerkinterface-Object |
| Netwerk Interface(*) |Verzonden bytes per seconde |Netwerkinterface-Object |
| Netwerkinterface (Microsoft Virtual Machinebus netwerk Adapter _2) |Ontvangen bytes per seconde |Netwerkinterface-Object |
| Netwerkinterface (Microsoft Virtual Machinebus netwerk Adapter _2) |Verzonden bytes per seconde |Netwerkinterface-Object |
| Netwerkinterface (Microsoft Virtual Machinebus netwerk Adapter _2) |Totaal aantal bytes per seconde |Netwerkinterface-Object |

## <a name="create-and-add-custom-performance-counters-to-your-application"></a>Maken en aangepaste prestatiemeteritems toevoegen aan uw toepassing
Azure heeft ondersteuning voor het maken en wijzigen van de aangepaste prestatiemeteritems voor webrollen en werkrollen. De items kunnen worden gebruikt om te volgen en controleren van toepassingsspecifieke gedrag. U kunt maken en aangepaste categorieën voor prestatiemeteritems en specificaties verwijderen uit een starten van de taak, Webrol of werkrol met verhoogde machtigingen.

> [!NOTE]
> Code die wijzigingen in aangepaste prestatiemeteritems aanbrengt moet wel verhoogde machtigingen om uit te voeren. Als de code in een Webrol of worker-rol, moet de rol van de tag bevatten <Runtime executionContext="elevated" /> in het bestand ServiceDefinition.csdef voor de functie naar behoren worden geïnitialiseerd.
>
>

U kunt aangepaste prestatiemeteritemgegevens verzenden naar Azure-opslag met behulp van de diagnostics-agent.

De standaard gegevens van prestatiemeteritems wordt gegenereerd door de processen voor Azure. Aangepaste gegevens van prestatiemeteritems moeten worden gemaakt door de functie- of worker-rol webtoepassing. Zie [prestaties itemtypen](https://msdn.microsoft.com/library/z573042h.aspx) voor informatie over de verschillende typen gegevens die kunnen worden opgeslagen in de aangepaste prestatiemeteritems. Zie [PerformanceCounters voorbeeld](http://code.msdn.microsoft.com/azure/) voor een voorbeeld maakt en stelt u aangepaste prestatiemeteritem-gegevens in een Webrol.

## <a name="store-and-view-performance-counter-data"></a>Gegevens van prestatiemeteritems opslaan en weergeven
Azure in de cache opgeslagen gegevens met andere diagnostische gegevens van prestatiemeteritems. Deze gegevens is beschikbaar voor externe controle terwijl de rolinstantie wordt uitgevoerd met behulp van externe bureaublad toegang om hulpprogramma's zoals Prestatiemeter weer te geven. Om te blijven behouden de gegevens die buiten de rolinstantie, moet de agent diagnostische gegevens van de gegevens overdragen naar Azure-opslag. De maximale grootte van de in cache opgeslagen gegevens van prestatiemeteritems kan worden geconfigureerd in de diagnostics-agent of mogelijk worden geconfigureerd als onderdeel van een gedeelde limiet voor de diagnostische gegevens. Zie voor meer informatie over het instellen van de buffergrootte [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) en [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx). Zie [Store en diagnostische gegevens weergeven in Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) voor een overzicht van het instellen van de agent diagnostische gegevens overdraagt naar een opslagaccount.

Elke geconfigureerde exemplaar van prestatiemeteritem op een opgegeven samplefrequentie wordt vastgelegd en de steekproefgegevens wordt overgedragen aan het opslagaccount door een geplande aanvraag of een aanvraag voor op aanvraag. Automatische transfers kunnen zo vaak als één keer per minuut worden gepland. Gegevens van prestatiemeteritems overgedragen door de agent diagnostische gegevens wordt opgeslagen in een tabel WADPerformanceCountersTable, in het opslagaccount. Deze tabel kan worden geopend en methoden die standaard Azure storage-API worden opgevraagd. Zie [Microsoft Azure PerformanceCounters Sample](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) voor een voorbeeld van het uitvoeren van query's en om gegevens van prestatiemeteritems uit de tabel WADPerformanceCountersTable weer te geven.

> [!NOTE]
> De meest recente prestatiemeteritemgegevens in het opslagaccount mogelijk enkele minuten verouderd, afhankelijk van de diagnostische gegevens agent overdracht frequentie en de latentie van de wachtrij.
>
>

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a>Prestatiemeteritems configuratiebestand diagnostische gegevens inschakelen
Gebruik de volgende procedure om in te schakelen prestatiemeters in uw Azure-toepassing.

## <a name="prerequisites"></a>Vereisten
Deze sectie wordt ervan uitgegaan dat u hebt de diagnostische monitor geïmporteerd in uw toepassing en het configuratiebestand van de diagnostische gegevens toegevoegd aan uw Visual Studio-oplossing (diagnostics.wadcfg in 2.4 SDK en de onderliggende of diagnostics.wadcfgx in SDK 2.5 en hoger). Zie stap 1 en 2 in [diagnostische gegevens inschakelen in Azure Cloud Services en virtuele Machines](cloud-services-dotnet-diagnostics.md)) voor meer informatie.

## <a name="step-1-collect-and-store-data-from-performance-counters"></a>Stap 1: Verzamelen en opslaan van gegevens van prestatiemeteritems
Nadat u het bestand diagnostische gegevens aan uw Visual Studio-oplossing hebt toegevoegd, kunt u de verzameling en de opslag van gegevens van prestatiemeteritems configureren in een Azure-toepassing. Dit wordt gedaan door prestatiemeteritems toe te voegen aan het bestand diagnostische gegevens. Diagnostics-gegevens, prestatiemeteritems, eerst worden verzameld op het exemplaar. De gegevens wordt vervolgens permanent in de tabel WADPerformanceCountersTable in de service Azure Table daarom moet u ook de storage-account in uw toepassing opgeven. Als u uw toepassing lokaal in de Emulator berekenen testen bent, kunt u diagnostische gegevens ook lokaal opslaan in de Opslagemulator. Voordat u de diagnostics-gegevens opslaat, moet u eerst gaan naar de [Azure-portal](http://portal.azure.com/) en een klassieke opslagaccount maken. Er is een best practice vinden van uw opslagaccount in de dezelfde geografische locatie als uw Azure-toepassing. Door houdt die de Azure-toepassing en het opslagaccount zich in dezelfde geografische locatie, te voorkomen dat externe bandbreedtekosten te betalen en de latentie te verminderen.

### <a name="add-performance-counters-to-the-diagnostics-file"></a>Prestatie-items toevoegen aan het bestand diagnostische gegevens
Er zijn veel prestatiemeteritems die u kunt gebruiken. Het volgende voorbeeld ziet enkele prestatiemeteritems die worden aanbevolen voor web- en bewaking van worker-rol.

Open het bestand diagnostics (diagnostics.wadcfg in 2.4 SDK en de onderliggende of diagnostics.wadcfgx in SDK 2.5 en hoger) en voeg het volgende toe aan het element DiagnosticMonitorConfiguration:

```xml
<PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
    <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use the Process(w3wp) category counters in a web role -->

    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use the Process(WaWorkerHost) category counters in a worker role.
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
</PerformanceCounters>
```

Het kenmerk bufferQuotaInMB geeft de maximale hoeveelheid opslagruimte op het bestand systeem die beschikbaar is voor het gegevenstype voor verzameling (Azure Logboeken, IIS-logboeken, enz.). De standaardwaarde is 0. Wanneer het quotum is bereikt, wordt de oudste gegevens verwijderd als er nieuwe gegevens worden toegevoegd. De som van alle eigenschappen van de bufferQuotaInMB moet groter dan de waarde van het kenmerk OverallQuotaInMB. Voor meer gedetailleerde informatie om te bepalen hoeveel opslagruimte zijn vereist voor het verzamelen van diagnostische gegevens, Zie de sectie Setup af [Best Practices probleemoplossing voor Azure-toepassingen ontwikkelen](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx).

Het kenmerk scheduledTransferPeriod het interval tussen de geplande overdrachten van gegevens geeft, afgerond op het dichtstbijzijnde minuut. In de volgende voorbeelden is deze ingesteld op PT30M (30 minuten). Wanneer de overdracht periode instelt op een kleine waarde, zoals 1 minuut, de prestaties van uw toepassing in productie nadelig beïnvloedt, maar kan handig zijn voor zien diagnostische gegevens snel te werken als u wilt testen. De geplande overdracht periode moet klein genoeg zijn om ervoor te zorgen dat de diagnostische gegevens niet van het exemplaar, maar groot genoeg is dat de prestaties van uw toepassing niet van invloed is overschreven.

Het kenmerk counterSpecifier Hiermee geeft u het prestatiemeteritem verzamelen. Het kenmerk sampleRate geeft de snelheid waarmee het prestatiemeteritem moet actieve, in dit geval 30 seconden.

Nadat u de prestatiemeteritems die u wilt verzamelen hebt toegevoegd, sla de wijzigingen naar het bestand diagnostische gegevens. Vervolgens moet u het opslagaccount dat de diagnostics-gegevens gehandhaafd voor opgeven.

### <a name="specify-the-storage-account"></a>De storage-account opgeven
Om te blijven behouden de diagnostische gegevens naar uw Azure Storage-account, moet u een verbindingsreeks in uw serviceconfiguratiebestand (ServiceConfiguration.cscfg).

Voor de Azure SDK 2.5, kan het Storage-Account worden opgegeven in het bestand diagnostics.wadcfgx.

> [!NOTE]
> Deze instructies zijn alleen van toepassing op Azure SDK 2.4 en hieronder. Voor de Azure SDK 2.5, kan het Storage-Account worden opgegeven in het bestand diagnostics.wadcfgx.
>
>

De verbindingsreeksen instellen:

1. Open het ServiceConfiguration.Cloud.cscfg-bestand met behulp van uw favoriete teksteditor en stel de verbindingsreeks voor de opslag. De *AccountName* en *AccountKey* waarden in de Azure-portal in het dashboard van de storage-account onder toegangstoetsen zijn gevonden.

    ```xml
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. Sla het bestand ServiceConfiguration.Cloud.cscfg.
3. Open het bestand ServiceConfiguration.Local.cscfg en controleren of UseDevelopmentStorage is ingesteld op true.

    ```xml
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
   Nu dat de verbindingsreeksen zijn ingesteld, wordt uw toepassing diagnostics-gegevens naar uw opslagaccount bewaard wanneer uw toepassing wordt geïmplementeerd.
4. Opslaan op het project bouwen en implementeren van uw toepassing.

## <a name="step-2-optional-create-custom-performance-counters"></a>Stap 2: (Optioneel) Maak aangepaste prestatiemeteritems
Naast de vooraf gedefinieerde prestatiemeteritems kunt u uw eigen aangepaste prestatiemeteritems voor het bewaken van web-of werkrollen toevoegen. Aangepaste prestatiemeteritems kunnen worden gebruikt voor het bijhouden en toepassingsspecifieke gedrag in de gaten en kan worden gemaakt of verwijderd in een opstarttaak Webrol of een werkrol met verhoogde machtigingen.

De prestaties teller configuratie uit het bestand .wadcfg een minuut na het starten van vernieuwen de Azure diagnostics-agent  Als u aangepaste prestatiemeteritems in de OnStart-methode maakt en uw opstarttaken duurt langer dan een minuut om uit te voeren, worden uw aangepaste prestatiemeteritems niet hebt gemaakt wanneer de agent Azure Diagnostics probeert te laden.  In dit scenario ziet u dat Azure Diagnostics correct worden alle diagnostische gegevens, behalve van uw aangepaste prestatiemeteritems vastgelegd.  U lost dit probleem door de prestatiemeteritems maken in een taak starten of verplaats de werken sommige van uw taak starten van de OnStart-methode na het maken van de prestatiemeteritems.

Voer de volgende stappen voor het maken van een eenvoudige aangepaste Prestatiemeter '\MyCustomCounterCategory\MyButton1Counter':

1. Open het servicedefinitiebestand (CSDEF) voor uw toepassing.
2. Voeg de Runtime-element op de WebRole of WorkerRole element dat moet worden uitgevoerd met verhoogde bevoegdheden:

    ```xml
    <runtime executioncontext="elevated"/>
    ```
3. Sla het bestand op.
4. Open het bestand van de diagnostische gegevens (diagnostics.wadcfg in 2.4 SDK en de onderliggende of diagnostics.wadcfgx in SDK 2.5 en hoger) en voeg het volgende toe aan de DiagnosticMonitorConfiguration

    ```xml
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
      <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. Sla het bestand op.
6. Maak de aangepaste Prestatiemeteritem-categorie in de OnStart-methode van uw rol voordat base wordt aangeroepen. OnStart. De volgende C#-voorbeeld maakt een aangepaste categorie als deze niet al bestaat:

    ```csharp
    public override bool OnStart()
    {
      if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
      {
         CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

         // add a counter tracking user button1 clicks
         CounterCreationData operationTotal1 = new CounterCreationData();
         operationTotal1.CounterName = "MyButton1Counter";
         operationTotal1.CounterHelp = "My Custom Counter for Button1";
         operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
         counterCollection.Add(operationTotal1);

         PerformanceCounterCategory.Create(
           "MyCustomCounterCategory",
           "My Custom Counter Category",
           PerformanceCounterCategoryType.SingleInstance, counterCollection);

         Trace.WriteLine("Custom counter category created.");
      }
      else {
        Trace.WriteLine("Custom counter category already exists.");
      }

    return base.OnStart();
    }
    ```
7. Bijwerken van de items in uw toepassing. Het volgende voorbeeld updates van een aangepaste prestatiemeteritem op Button1_Click gebeurtenissen:

    ```csharp
    protected void Button1_Click(object sender, EventArgs e)
    {
      PerformanceCounter button1Counter = new PerformanceCounter(
        "MyCustomCounterCategory",
        "MyButton1Counter",
        string.Empty,
        false);
      button1Counter.Increment();
      this.Button1.Text = "Button 1 count: " +
        button1Counter.RawValue.ToString();
    }
    ```
8. Sla het bestand op.  

Aangepaste prestatiemeteritemgegevens worden nu verzameld door de monitor Azure diagnostics.

## <a name="step-3-query-performance-counter-data"></a>Stap 3: De gegevens van prestatiemeteritems opvragen
Als uw toepassing wordt geïmplementeerd en uitgevoerd, de monitor Diagnostics gaat verder met het verzamelen van prestatiemeteritems en het behouden blijven van die gegevens naar Azure-opslag. U hulpprogramma's zoals Server Explorer gebruiken in Visual Studio [Azure Opslagverkenner](http://azurestorageexplorer.codeplex.com/), of [Azure Diagnostics Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) door Cerebrata om weer te geven de prestatiemeteritems van de gegevens in de tabel WADPerformanceCountersTable. U kunt ook programmatisch query voor de tabel-service via [C#](../cosmos-db/table-storage-how-to-use-dotnet.md), [Java](../cosmos-db/table-storage-how-to-use-java.md), [Node.js](../cosmos-db/table-storage-how-to-use-nodejs.md), [Python](../cosmos-db/table-storage-how-to-use-python.md), [Ruby](../cosmos-db/table-storage-how-to-use-ruby.md), of [PHP](../cosmos-db/table-storage-how-to-use-php.md).

De volgende C#-voorbeeld ziet u een eenvoudige query op de tabel WADPerformanceCountersTable en slaat de diagnostics-gegevens op een CSV-bestand. Zodra de prestatiemeteritems zijn opgeslagen op een CSV-bestand, kunt u de grafische mogelijkheden in Microsoft Excel of een ander hulpprogramma gebruiken om de gegevens te visualiseren. Zorg ervoor dat een verwijzing naar Microsoft.WindowsAzure.Storage.dll, toevoegen die wordt opgenomen in de Azure SDK voor .NET oktober 2012 en hoger. De assembly is naar de map % Program Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\ geïnstalleerd.

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Table;
...

// Get the connection string. When using Microsoft Azure Cloud Services, it is recommended
// you store your connection string using the Microsoft Azure service configuration
// system (*.csdef and *.cscfg files). You can you use the CloudConfigurationManager type
// to retrieve your storage connection string.  If you're not using Cloud Services, it's
// recommended that you store the connection string in your web.config or app.config file.
// Use the ConfigurationManager type to retrieve your storage connection string.

string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
//string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

// Get a reference to the storage account using the connection string.  You can also use the development
// storage account (Storage Emulator) for local debugging.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
//CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "WADPerformanceCountersTable" table.
CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

// Create the table query, filter on a specific CounterName, DeploymentId and RoleInstance.
TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
  .Where(
    TableQuery.CombineFilters(
      TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
      TableOperators.And,
      TableQuery.CombineFilters(
      TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
      TableOperators.And,
      TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
    )
  )
);

// Execute the table query.
IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

// Process the query results and build a CSV file.
StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

foreach (PerformanceCountersEntity entity in result)
{
  sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
    + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
}

StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
sw.Write(sb.ToString());
sw.Close();
```

Entiteiten worden toegewezen aan C#-objecten met behulp van een aangepaste klasse die is afgeleid van **TableEntity**. De volgende code definieert een entiteitsklasse die staat voor een prestatiemeteritem in de **WADPerformanceCountersTable** tabel.

```csharp
public class PerformanceCountersEntity : TableEntity
{
  public long EventTickCount { get; set; }
  public string DeploymentId { get; set; }
  public string Role { get; set; }
  public string RoleInstance { get; set; }
  public string CounterName { get; set; }
  public double CounterValue { get; set; }
}
```

## <a name="next-steps"></a>Volgende stappen
[Aanvullende artikelen op Azure Diagnostics weergeven](../azure-diagnostics.md)

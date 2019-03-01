---
title: Registratie in diagnoselogboek in Azure Cosmos DB
description: Meer informatie over de verschillende manieren om te leggen en te bewaken gegevens die zijn opgeslagen in Azure Cosmos DB.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: 2a08097b42f395bd0009353635cabbd264c3c421
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56992087"
---
# <a name="diagnostic-logging-in-azure-cosmos-db"></a>Registratie in diagnoselogboek in Azure Cosmos DB 

Nadat u begint met het gebruik van een of meer Azure Cosmos DB-databases, kunt u controleren hoe en wanneer uw databases zijn geraadpleegd. Dit artikel bevat een overzicht van de logboeken die beschikbaar op het Azure-platform zijn. Leert u hoe u Diagnostische logboekregistratie inschakelen voor bewakingsdoeleinden voor het verzenden van logboeken naar [Azure Storage](https://azure.microsoft.com/services/storage/), stream logboeken naar het [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), en hoe u Logboeken om te exporteren [vanAzureMonitor-Logboeken](https://azure.microsoft.com/services/log-analytics/).

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="logs-available-in-azure"></a>Logboeken beschikbaar zijn in Azure

Voordat we praten over het bewaken van uw Azure Cosmos DB-account, laten we een paar dingen voor logboekregistratie en bewaking te verduidelijken. Er zijn verschillende soorten logboeken op de Azure-platform. Er zijn [Azure-activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), [diagnostische logboeken van Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs), [metrische gegevens van Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics), gebeurtenissen, heartbeat-bewaking, operations-Logboeken, enzovoort. Er is een breed spectrum van Logboeken. U ziet de volledige lijst van Logboeken in [logboeken van Azure Monitor](https://azure.microsoft.com/services/log-analytics/) in Azure portal. 

De volgende afbeelding ziet u de verschillende soorten logboeken in Azure die beschikbaar zijn:

![Verschillende soorten logboeken in Azure](./media/logging/azurelogging.png)

In de afbeelding, de **Rekenresources** vertegenwoordigen de Azure-resources waarvoor u toegang hebt tot het Gastbesturingssysteem van Microsoft. Bijvoorbeeld: Azure Virtual Machines, virtuele machine schaalsets, Azure Container Service en enzovoort, worden beschouwd als compute-resources. COMPUTE-resources activiteitenlogboeken, diagnostische logboeken en toepassingslogboeken te genereren. Raadpleeg voor meer informatie, de [bronnen van bewakingsgegevens in Azure](../azure-monitor/platform/data-sources.md) artikel.

De **Non-rekenresources** zijn resources in die u kunt geen toegang tot het onderliggende besturingssysteem en werk rechtstreeks met de resource. Bijvoorbeeld, Network Security Groups, Logic Apps, enzovoort. Azure Cosmos DB is een niet-compute-resource. U kunt de logboeken voor niet-compute-resources in het activiteitenlogboek weergeven of schakelt u de optie Logboeken met diagnostische gegevens in de portal. Raadpleeg voor meer informatie, de [bronnen van de gegevens in Azure Monitor](../azure-monitor/platform/data-sources.md) artikel.

Het activiteitenlogboek registreert de bewerkingen op het abonnementsniveau van een voor Azure Cosmos DB. Bewerkingen zoals ListKeys, DatabaseAccounts schrijven en meer worden geregistreerd. Diagnoselogboeken bieden meer gedetailleerde logboekregistratie en kunnen u aan te melden DataPlaneRequests (maken, lezen, Query, enzovoort) en MongoRequests.


In dit artikel hebben we ons richten op de Azure-activiteitenlogboek, Azure diagnostische logboeken en metrische gegevens van Azure. Wat is het verschil tussen deze drie logboeken? 

### <a name="azure-activity-log"></a>Azure-activiteitenlogboek

De Azure-activiteitenlogboek is een abonnementlogboek biedt inzicht in gebeurtenissen op abonnementsniveau die hebben plaatsgevonden in Azure. Het activiteitenlogboek rapporten controlelaag gebeurtenissen voor uw abonnementen onder de beheercategorie. U kunt het activiteitenlogboek gebruiken om te bepalen de ' wat, wie, en wanneer ' voor een schrijfbewerking (PUT, POST, DELETE) op de resources in uw abonnement. U kunt ook de status van de bewerking en andere relevante eigenschappen begrijpen. 

Het activiteitenlogboek verschilt van diagnostische logboeken. Het activiteitenlogboek biedt gegevens over de bewerkingen op een resource van buitenaf (de _controlelaag_). Besturingselement vlak bewerkingen zijn onder meer maken in de context van Azure Cosmos DB, container, een lijst met sleutels, verwijderen, lijst database, enzovoort. Logboeken met diagnostische gegevens worden gegenereerd door een resource en geef informatie op over de werking van die resource (de _gegevenslaag_). Enkele voorbeelden van de bewerkingen voor het vlak van gegevens in de diagnostische logboeken zijn Delete, Insert en ReadFeed.

Activiteitenlogboeken (bewerkingen voor de controlelaag) kunnen uitgebreidere van aard en het volledige e-mailadres van de oproepende functie, IP-adres van oproepende functie, Resourcenaam, de naam van bewerking, tenant-id en meer kunnen opnemen. Het activiteitenlogboek bevat verschillende [categorieën](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-activity-log-schema) van gegevens. Zie voor volledige informatie over de schema's van deze categorieën vallen [gebeurtenisschema in het Azure-activiteitenlogboek](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-activity-log-schema). Logboeken met diagnostische gegevens kan echter zijn beperkingen in de natuur als persoonlijke gegevens vaak wordt verwijderd uit deze logboeken. Mogelijk hebt u het IP-adres van de oproepende functie, maar de laatste octant is verwijderd.

### <a name="azure-metrics"></a>Metrische gegevens van Azure

[Metrische gegevens van Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) zijn de belangrijkste type Azure telemetrische gegevens (ook wel genoemd _prestatiemeteritems_) die wordt verzonden door de meeste Azure-resources. Metrische gegevens kunt u informatie bekijken over de doorvoer, opslag, consistentie, beschikbaarheid en de latentie van uw Azure Cosmos DB-resources. Zie voor meer informatie, [bewaking en foutopsporing met metrische gegevens in Azure Cosmos DB](use-metrics.md).

### <a name="azure-diagnostic-logs"></a>Diagnostische logboeken in Azure

Diagnostische logboeken in Azure worden gegenereerd door een resource en biedt uitgebreide, regelmatig gegevens over de werking van die resource. Resourcetype is afhankelijk van de inhoud van deze logboeken. Diagnostische logboeken resourceniveau afwijken van de Gast OS-niveau diagnostische logboeken. Gastbesturingssysteem logboeken met diagnostische gegevens worden verzameld door een agent die wordt uitgevoerd binnen een virtuele machine of andere ondersteund resourcetype. Diagnostische logboeken resourceniveau vereisen geen gegevens van de resource-specifieke agent en vastleggen van de Azure-platform zelf. Gast-OS-niveau logboeken met diagnostische gegevens vastleggen van gegevens uit het besturingssysteem en toepassingen die worden uitgevoerd op een virtuele machine.

![Diagnostische logboekregistratie naar opslag, Event Hubs of Azure Monitor-Logboeken](./media/logging/azure-cosmos-db-logging-overview.png)

### <a name="what-is-logged-by-azure-diagnostic-logs"></a>Wat is vastgelegd door diagnoselogboeken van Azure?

* Alle geverifieerde back-end-aanvragen (TCP/REST) voor alle API's worden vastgelegd, met inbegrip van mislukte aanvragen als gevolg van toegangsmachtigingen, systeemfouten of ongeldige aanvragen. Ondersteuning voor de gebruiker geïnitieerde Graph, Cassandra en Table-API-aanvragen zijn niet op dit moment beschikbaar.
* Bewerkingen op de database zelf, waaronder CRUD-bewerkingen op alle documenten, containers en -databases.
* Bewerkingen voor sleutels, waaronder het maken, wijzigen of verwijderen van de sleutels.
* Niet-geverifieerde aanvragen die in een 401-respons resulteren. Bijvoorbeeld, aanvragen die geen bearer-token of ongeldige of verlopen zijn, of een ongeldig token.

<a id="#turn-on"></a>
## <a name="turn-on-logging-in-the-azure-portal"></a>Schakel logboekregistratie in Azure portal

Als u wilt logboekregistratie van diagnostische gegevens inschakelen, hebt u de volgende bronnen:

* Een bestaande Azure Cosmos DB-account, database en -container. Zie voor instructies over het maken van deze resources [een databaseaccount maken met behulp van de Azure-portal](create-sql-api-dotnet.md#create-a-database-account), [Azure CLI-voorbeelden](cli-samples.md), of [PowerShell-voorbeelden](powershell-samples.md).

Als u wilt inschakelen voor diagnostische gegevens vastleggen in Azure portal, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com), in uw Azure Cosmos DB-account, selecteert u **diagnostische logboeken** in de navigatiebalk aan de linkerkant en selecteer vervolgens **diagnostische gegevens inschakelen**.

    ![Schakel registratie in diagnoselogboek voor Azure Cosmos DB in Azure portal](./media/logging/turn-on-portal-logging.png)

2. In de **diagnostische instellingen** pagina, de volgende stappen uit: 

    * **Naam**: Voer een naam voor de logboeken om te maken.

    * **Archiveren naar een opslagaccount**: Als u wilt deze optie gebruikt, moet u een bestaand opslagaccount verbinden. Zie voor informatie over het maken van een nieuw opslagaccount in de portal [een opslagaccount maken](../storage/common/storage-create-storage-account.md) en volg de instructies voor het maken van een Azure Resource Manager, account voor algemeen gebruik. Keer vervolgens terug naar deze pagina in de portal voor uw storage-account selecteren. Het duurt enkele minuten duren voordat de zojuist gemaakte storage-accounts worden weergegeven in de vervolgkeuzelijst.
    * **Stream naar een event hub**: Als u wilt deze optie gebruikt, moet u een bestaande Event Hubs-naamruimte en event hub te verbinden. Zie voor het maken van een Event Hubs-naamruimte, [een Event Hubs-naamruimte en een event hub maken met behulp van de Azure-portal](../event-hubs/event-hubs-create.md). Keer vervolgens terug naar deze pagina in de portal voor het selecteren van de naam van de Event Hubs-naamruimte en het beleid.
    * **Verzenden naar Log Analytics**: Als u wilt deze optie gebruikt, gebruik een bestaande werkruimte of maak een nieuwe Log Analytics-werkruimte met de volgende stappen om te [Maak een nieuwe werkruimte](../azure-monitor/learn/quick-collect-azurevm.md#create-a-workspace) in de portal. Zie de dat weergave-logboeken in Logboeken van Azure Monitor voor meer informatie over het weergeven van uw logboeken in Logboeken van Azure Monitor.
    * **Meld u DataPlaneRequests**: Selecteer deze optie om aan te melden van aanvragen van de back-end van de onderliggende Azure Cosmos DB gedistribueerd platform voor SQL, grafiek, MongoDB, Cassandra en Table-API-accounts. Als u naar een opslagaccount archiveren bent, kunt u de bewaarperiode voor de diagnostische logboeken. Logboeken worden automatisch verwijderd nadat de bewaarperiode is verlopen.
    * **Meld u MongoRequests**: Selecteer deze optie om aan te melden gebruiker geïnitieerde aanvragen van de Azure Cosmos DB-front-end voor het uitvoeren van Cosmos-accounts die zijn geconfigureerd met Azure Cosmos DB-API voor MongoDB. Als u naar een opslagaccount archiveren bent, kunt u de bewaarperiode voor de diagnostische logboeken. Logboeken worden automatisch verwijderd nadat de bewaarperiode is verlopen.
    * **Metrische aanvragen**: Selecteer deze optie voor het opslaan van uitgebreide gegevens in [metrische gegevens van Azure](../azure-monitor/platform/metrics-supported.md). Als u naar een opslagaccount archiveren bent, kunt u de bewaarperiode voor de diagnostische logboeken. Logboeken worden automatisch verwijderd nadat de bewaarperiode is verlopen.

3. Selecteer **Opslaan**.

    Als u een foutbericht ontvangt met de tekst ' kan niet bijwerken van diagnostische gegevens voor \<Werkruimtenaam >. Het abonnement \<abonnements-id > is niet geregistreerd voor het gebruik van microsoft.insights, "Voer de [oplossen Azure Diagnostics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage) instructies voor het registreren van het account en voer deze procedure vervolgens opnieuw uit.

    Als u wijzigen hoe uw logboeken met diagnostische gegevens in de toekomst op elk gewenst moment worden opgeslagen wilt, kunt u terugkeren naar deze pagina om de instellingen diagnostische logboeken voor uw account te wijzigen.

## <a name="turn-on-logging-by-using-azure-cli"></a>Logboekregistratie inschakelen met behulp van Azure CLI

Om in te schakelen metrische gegevens en logboekregistratie van diagnostische gegevens met behulp van Azure CLI, gebruikt u de volgende opdrachten:

- Om in te schakelen opslag van logboeken met diagnostische gegevens in een opslagaccount, gebruikt u deze opdracht:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   De `resourceId` is de naam van het Azure Cosmos DB-account. De `storageId` is de naam van het opslagaccount waarnaar u wilt de logboeken te zenden.

- Om in te schakelen van diagnostische logboeken streamen naar een event hub, gebruikt u deze opdracht:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   De `resourceId` is de naam van het Azure Cosmos DB-account. De `serviceBusRuleId` is een tekenreeks zijn met deze indeling:

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- Als u wilt verzenden, diagnostische logboeken naar Log Analytics-werkruimte inschakelen, gebruikt u deze opdracht:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
   ```

U kunt deze parameters voor het inschakelen van meerdere uitvoeropties combineren.

## <a name="turn-on-logging-by-using-powershell"></a>Logboekregistratie inschakelen met behulp van PowerShell

Als u wilt vastleggen van diagnostische gegevens inschakelen met behulp van PowerShell, moet u Azure Powershell met een minimale versie 1.0.1 of hoger.

Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) om Azure PowerShell te installeren en te koppelen aan uw Azure-abonnement.

Als u Azure PowerShell al hebt geïnstalleerd en u niet welke versie van het type van de console PowerShell weet `(Get-Module azure -ListAvailable).Version`.  

### <a id="connect"></a>Verbinding maken met uw abonnementen
Start een Azure PowerShell-sessie en gebruik de volgende opdracht om u aan te melden bij uw Azure-account:  

```powershell
Connect-AzureRmAccount
```

Voer in het pop-upvenster in de browser uw gebruikersnaam en wachtwoord voor uw Azure-account in. Azure PowerShell haalt alle abonnementen die zijn gekoppeld aan dit account en standaard het eerste certificaat gebruikt.

Als u meer dan één abonnement hebt, hebt u mogelijk om op te geven van het specifieke abonnement dat is gebruikt voor het maken van uw Azure key vault. Als u wilt zien van de abonnementen voor uw account, typ de volgende opdracht:

```powershell
Get-AzureRmSubscription
```

Als u het abonnement dat is gekoppeld aan het Azure Cosmos DB-account dat u zich aanmeldt, typ de volgende opdracht uit:

```powershell
Set-AzureRmContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> Als u meer dan één abonnement dat is gekoppeld aan uw account hebt, is het belangrijk om op te geven van het abonnement dat u wilt gebruiken.
>
>

Zie voor meer informatie over het configureren van Azure PowerShell [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview).

### <a id="storage"></a>Een nieuw opslagaccount voor uw logboeken maken
Hoewel u een bestaand opslagaccount voor uw logboeken in deze zelfstudie gebruiken kunt, maken we een nieuw opslagaccount dat toegewezen aan Azure Cosmos DB-Logboeken. Voor het gemak slaan we details van het opslagaccount in een variabele met de naam **sa**.

Voor nog gemakkelijker te maken van beheer in deze zelfstudie gebruiken we dezelfde resourcegroep bevinden als het account dat die de Azure Cosmos DB-database bevat. Vervang de waarden voor de **ContosoResourceGroup**, **contosocosmosdblogs**, en **Noord-centraal VS** parameters, zoals van toepassing:

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup `
-Name contosocosmosdblogs -Type Standard_LRS -Location 'North Central US'
```

> [!NOTE]
> Als u een bestaand opslagaccount gebruiken besluit, moet het account hetzelfde abonnement als uw Azure Cosmos DB-abonnement gebruiken. Het account moet ook gebruiken voor het Resource Manager-implementatiemodel, in plaats van het klassieke implementatiemodel.
>
>

### <a id="identify"></a>De Azure Cosmos DB-account voor uw logboeken identificeren
Stel de naam van het Azure Cosmos DB aan een variabele met de naam **account**, waarbij **ResourceName** is de naam van het Azure Cosmos DB-account.

```powershell
$account = Get-AzureRmResource -ResourceGroupName ContosoResourceGroup `
-ResourceName contosocosmosdb -ResourceType "Microsoft.DocumentDb/databaseAccounts"
```

### <a id="enable"></a>Logboekregistratie inschakelen
Als u wilt logboekregistratie inschakelen voor Azure Cosmos DB, gebruikt u de `Set-AzureRmDiagnosticSetting` cmdlet met variabelen voor de nieuwe storage-account, Azure Cosmos DB-account en de categorie voor logboekregistratie wilt inschakelen. Voer de volgende opdracht uit en stel de **-ingeschakeld** markering **$true**:

```powershell
Set-AzureRmDiagnosticSetting  -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories DataPlaneRequests
```

De uitvoer voor de opdracht moet lijken op het volgende voorbeeld:

```powershell
    StorageAccountId            : /subscriptions/<subscription-ID>/resourceGroups/ContosoResourceGroup/providers`
    /Microsoft.Storage/storageAccounts/contosocosmosdblogs
    ServiceBusRuleId            :
    EventHubAuthorizationRuleId :
    Metrics
        TimeGrain       : PT1M
        Enabled         : False
        RetentionPolicy
        Enabled : False
        Days    : 0
    
    Logs
        Category        : DataPlaneRequests
        Enabled         : True
        RetentionPolicy
        Enabled : False
        Days    : 0
    
    WorkspaceId                 :
    Id                          : /subscriptions/<subscription-ID>/resourcegroups/ContosoResourceGroup/providers`
    /microsoft.documentdb/databaseaccounts/contosocosmosdb/providers/microsoft.insights/diagnosticSettings/service
    Name                        : service
    Type                        :
    Location                    :
    Tags                        :
```

De uitvoer van de opdracht wordt bevestigd dat logboekregistratie nu is ingeschakeld voor uw database en dat informatie wordt opgeslagen naar uw opslagaccount.

U kunt eventueel ook het bewaarbeleid instellen voor uw Logboeken, zodat oudere logboeken automatisch worden verwijderd. Bijvoorbeeld, stel het bewaarbeleid met de **- RetentionEnabled** vlag ingesteld op **$true**. Stel de **- RetentionInDays** parameter **90** zodat logboeken die ouder zijn dan 90 dagen automatisch worden verwijderd.

```powershell
Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`
 -StorageAccountId $sa.Id -Enabled $true -Categories DataPlaneRequests`
  -RetentionEnabled $true -RetentionInDays 90
```

### <a id="access"></a>Toegang tot uw logboeken
Azure Cosmos DB-logboeken voor de **DataPlaneRequests** categorie worden opgeslagen in de **insights-logs-gegevens-gegevenslaag-aanvragen** container in het opslagaccount die u hebt opgegeven. 

Maak eerst een variabele voor de containernaam. De variabele wordt gebruikt tijdens de procedure.

```powershell
    $container = 'insights-logs-dataplanerequests'
```

Als alle blobs in deze container, typt u:

```powershell
Get-AzureStorageBlob -Container $container -Context $sa.Context
```

De uitvoer voor de opdracht moet lijken op het volgende voorbeeld:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 10510193
ContentType       : application/octet-stream
LastModified      : 9/28/2017 7:49:04 PM +00:00
SnapshotTime      :
ContinuationToken:
Context           : Microsoft.WindowsAzure.Commands.Common.Storage.`
                    LazyAzureStorageContext
Name              : resourceId=/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS`
/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/CONTOSOCOSMOSDB/y=2017/m=09/d=28/h=19/m=00/PT1H.json
```

Zoals u in deze uitvoer zien kunt, volgen de blobs een naamconventie: `resourceId=/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<Database Account Name>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/filename.json`

De datum- en tijdwaarden maken gebruik van UTC.

Aangezien hetzelfde opslagaccount kan worden gebruikt voor het verzamelen van Logboeken voor meerdere resources, kunt u de volledig gekwalificeerde resource-ID in de blob-naam om te openen en downloaden van de specifieke blobs die u nodig hebt. Voordat we dat gaan doen, behandelen we het downloaden van alle blobs.

Maak eerst een map waarin u de blobs wilt downloaden. Bijvoorbeeld:

```powershell
New-Item -Path 'C:\Users\username\ContosoCosmosDBLogs'`
 -ItemType Directory -Force
```

Haal vervolgens een lijst met alle van de blobs:  

```powershell
$blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context
```

Sluis deze lijst via de `Get-AzureStorageBlobContent` opdracht voor het downloaden van de blobs in de doelmap:

```powershell
$blobs | Get-AzureStorageBlobContent `
 -Destination 'C:\Users\username\ContosoCosmosDBLogs'
```

Wanneer u deze tweede opdracht uitvoert de **/** scheidingsteken in de blobnamen een volledige mapstructuur onder de doelmap gemaakt. Deze structuur wordt gebruikt om te downloaden en opslaan van de blobs als bestanden.

Als u alleen specifieke blobs wilt downloaden, moet u jokertekens gebruiken. Bijvoorbeeld:

* Als u meerdere databases hebt en wilt downloaden van Logboeken voor slechts één database met de naam **CONTOSOCOSMOSDB3**, gebruikt u de opdracht:

    ```powershell
    Get-AzureStorageBlob -Container $container `
     -Context $sa.Context -Blob '*/DATABASEACCOUNTS/CONTOSOCOSMOSDB3
    ```

* Als u hebt meerdere resourcegroepen en logboeken voor slechts één resourcegroep downloaden die u wilt, gebruikt u de opdracht `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

    ```powershell
    Get-AzureStorageBlob -Container $container `
    -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
    ```
* Als u alle logboeken voor de maand van juli 2017 downloaden wilt, gebruikt u de opdracht `-Blob '*/year=2017/m=07/*'`:

    ```powershell
    Get-AzureStorageBlob -Container $container `
     -Context $sa.Context -Blob '*/year=2017/m=07/*'
    ```

U kunt ook de volgende opdrachten uitvoeren:

* Als u wilt de status van diagnostische instellingen voor uw database opvragen, gebruikt u de opdracht `Get-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`.
* Uitschakelen van logboekregistratie van het **DataPlaneRequests** categorie voor uw account, gebruikt u de opdracht `Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories DataPlaneRequests`.


De blobs die worden geretourneerd in elk van deze query's worden opgeslagen als tekst en opgemaakt als een JSON-blob, zoals wordt weergegeven in de volgende code:

```json
{
    "records":
    [
        {
           "time": "Fri, 23 Jun 2017 19:29:50.266 GMT",
           "resourceId": "contosocosmosdb",
           "category": "DataPlaneRequests",
           "operationName": "Query",
           "resourceType": "Database",
           "properties": {"activityId": "05fcf607-6f64-48fe-81a5-f13ac13dd1eb",`
           "userAgent": "documentdb-dotnet-sdk/1.12.0 Host/64-bit MicrosoftWindowsNT/6.2.9200.0 AzureSearchIndexer/1.0.0",`
           "resourceType": "Database","statusCode": "200","documentResourceId": "",`
           "clientIpAddress": "13.92.241.0","requestCharge": "2.260","collectionRid": "",`
           "duration": "9250","requestLength": "72","responseLength": "209", "resourceTokenUserRid": ""}
        }
    ]
}
```

Zie voor meer informatie over de gegevens in elk JSON-blob, [Azure Cosmos DB-logboekgegevens interpreteren](#interpret).

## <a name="manage-your-logs"></a>Uw logboeken beheren

Logboeken met diagnostische gegevens zijn beschikbaar in uw account gedurende twee uur vanaf het moment dat de Azure Cosmos DB-bewerking is uitgevoerd. Het is aan u om uw logboeken in uw opslagaccount te beheren:

* Standaard Azure access controlemethoden voor het beveiligen van uw logboeken en beperken wie toegang heeft tot deze gebruiken.
* Verwijder de logboeken die u niet meer in uw opslagaccount wilt bewaren.
* De bewaarperiode voor gegevens vlak aanvragen die zijn gearchiveerd in een Storage-account is geconfigureerd in de portal wanneer de **Log DataPlaneRequests** instelling is geselecteerd. Als u wilt wijzigen die instelling, Zie [Schakel logboekregistratie in Azure portal](#turn-on-logging-in-the-azure-portal).


<a id="#view-in-loganalytics"></a>
## <a name="view-logs-in-azure-monitor-logs"></a>Weergave-logboeken in Logboeken van Azure Monitor

Als u hebt geselecteerd de **verzenden naar Log Analytics** optie bij het inschakelen van diagnostische gegevens vastleggen, diagnostische gegevens van de container wordt doorgestuurd naar de logboeken van Azure Monitor binnen twee uur. Wanneer u logboeken van Azure Monitor bekijkt onmiddellijk nadat u logboekregistratie inschakelt, kunt u de gegevens niet zien. Slechts twee uur wacht en probeer het opnieuw. 

Voordat u uw logboeken bekijken, controleren en zien als uw werkruimte voor logboekanalyse is bijgewerkt naar de nieuwe Kusto-querytaal gebruiken. Als u wilt controleren, opent u de [Azure-portal](https://portal.azure.com), selecteer **Log Analytics-werkruimten** uiterst links, selecteer vervolgens de naam van de werkruimte zoals weergegeven in de volgende afbeelding. De **Log Analytics-werkruimte** pagina wordt weergegeven:

![Azure Monitor beschikbaar zijn in Azure portal](./media/logging/azure-portal.png)

>[!NOTE]
>OMS-werkruimten worden nu aangeduid als Log Analytics-werkruimten.  

Als u het volgende bericht wordt weergegeven op de **Log Analytics-werkruimte** pagina uw werkruimte is niet bijgewerkt voor het gebruik van de nieuwe taal. Zie voor meer informatie over het upgraden naar de nieuwe querytaal [uw Azure Log Analytics-werkruimte upgraden voor nieuwe zoekopdrachten](../log-analytics/log-analytics-log-search-upgrade.md). 

![Logboeken in Azure Monitor bijwerken bericht](./media/logging/upgrade-notification.png)

Voor de diagnostische gegevens in Azure Monitor Logboeken opent de **zoeken in logboeken** pagina in het menu links of de **Management** gebied van de pagina, zoals wordt weergegeven in de volgende afbeelding:

![Opties voor het doorzoeken van logboekbestanden in de Azure-portal](./media/logging/log-analytics-open-log-search.png)

Nu dat u gegevensverzameling hebt ingeschakeld, het volgende voorbeeld van de zoekopdracht in Logboeken uitvoeren met behulp van de nieuwe querytaal om te zien van de 10 meest recente logboeken `AzureDiagnostics | take 10`.

![Voorbeeld van zoeken in Logboeken voor de 10 meest recente Logboeken](./media/logging/log-analytics-query.png)

<a id="#queries"></a>
### <a name="queries"></a>Query's

Hier volgen enkele extra query's die u kunt invoeren in de **zoeken in logboeken** in om u te helpen bij het controleren van uw Azure Cosmos DB-containers. Deze query's werken met de [nieuwe taal](../log-analytics/log-analytics-log-search-upgrade.md). 

Zie voor meer informatie over de betekenis van de gegevens die wordt geretourneerd door elke zoeken in Logboeken, [Azure Cosmos DB-logboekgegevens interpreteren](#interpret).

* Zoeken naar alle diagnostische logboeken van Azure Cosmos DB voor een opgegeven periode:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
    ```

* Als u wilt zoeken naar de 10 meest recent geregistreerde gebeurtenissen:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | take 10
    ```

* Query uitvoeren voor alle bewerkingen, gegroepeerd op bewerkingstype:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by OperationName
    ```

* Query voor alle bewerkingen, gegroepeerd op **Resource**:

    ```
    AzureActivity | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by Resource
    ```

* Zoeken naar alle gebruikersactiviteiten, gegroepeerd op resource:

    ```
    AzureActivity | where Caller == "test@company.com" and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by Resource
    ```
    > [!NOTE]
    > Met deze opdracht is voor een activiteitenlogboek, niet een diagnostisch logboek.

* Query uitvoeren op waarvoor operations duurt langer dan 3 milliseconden:

    ```
    AzureDiagnostics | where toint(duration_s) > 30000 and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by clientIpAddress_s, TimeGenerated
    ```

* Query uitvoeren voor welke agent de bewerkingen wordt uitgevoerd:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by OperationName, userAgent_s
    ```

* Zoeken naar wanneer de langlopende bewerkingen zijn uitgevoerd:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | project TimeGenerated , toint(duration_s)/1000 | render timechart
    ```

Zie voor meer informatie over het gebruik van de nieuwe taal voor zoeken in logboeken [begrijpen zoekopdrachten in Logboeken van Azure Monitor](../log-analytics/log-analytics-log-search-new.md). 

## <a id="interpret"></a>-Logboekgegevens interpreteren

Diagnostische gegevens die zijn opgeslagen in Logboeken in Azure Storage en Azure Monitor maakt gebruik van een soortgelijke schema. 

De volgende tabel beschrijft de inhoud van elke logboekvermelding.

| Azure Storage-veld of eigenschap | Azure Monitor-eigenschap Logboeken | Description |
| --- | --- | --- |
| **tijd** | **TimeGenerated** | De datum en tijd (UTC) wanneer de bewerking opgetreden. |
| **ResourceId** | **Resource** | Het Azure Cosmos DB-account waarvoor de logboeken zijn ingeschakeld.|
| **Categorie** | **Categorie** | Voor Azure Cosmos DB-Logboeken is **DataPlaneRequests** is de enige beschikbare waarde. |
| **OperationName** | **OperationName** | Naam van de bewerking. Deze waarde kan zijn dat een van de volgende bewerkingen: Maken, bijwerken, lezen, ReadFeed, verwijderen, vervangen, worden uitgevoerd, SqlQuery, Query, JSQuery, Head, HeadFeed of Upsert.   |
| **Eigenschappen** | N.v.t. | De inhoud van dit veld worden beschreven in de rijen die volgen. |
| **ActivityId** | **activityId_g** | De unieke GUID voor de geregistreerde bewerking. |
| **UserAgent** | **userAgent_s** | Een tekenreeks waarmee de clientagent van de gebruiker die de aanvraag uitvoert. De indeling is {gebruiker agent name} / {version}.|
| **requestResourceType** | **requestResourceType_s** | Het type van de toegang tot bronnen. Deze waarde kan een van de volgende resourcetypen zijn: Database, Container, Document, bijlage, gebruikers, machtigingen, StoredProcedure, Trigger, UserDefinedFunction of aanbieding. |
| **statusCode** | **statusCode_s** | De reactiestatus van de bewerking. |
| **requestResourceId** | **ResourceId** | De resourceId die betrekking hebben op de aanvraag. De waarde kan verwijzen naar databaseRid, collectionRid of documentRid, afhankelijk van de bewerking die wordt uitgevoerd.|
| **clientIpAddress** | **clientIpAddress_s** | IP-adres van de client. |
| **requestCharge** | **requestCharge_s** | Het aantal ru's die worden gebruikt door de bewerking |
| **collectionRid** | **collectionId_s** | De unieke ID voor de verzameling.|
| **Duur** | **duration_s** | De duur van de bewerking, in tikken. |
| **requestLength** | **requestLength_s** | De lengte van de aanvraag, in bytes. |
| **responseLength** | **responseLength_s** | De lengte van het antwoord, in bytes.|
| **resourceTokenUserRid** | **resourceTokenUserRid_s** | Deze waarde is niet leeg zijn wanneer [brontokens](https://docs.microsoft.com/azure/cosmos-db/secure-access-to-data#resource-tokens) worden gebruikt voor verificatie. De waarde verwijst naar de resource-ID van de gebruiker. |

## <a name="next-steps"></a>Volgende stappen

- Lees voor meer informatie over het inschakelen van logboekregistratie en metrische gegevens en logboekbestanden categorieën die worden ondersteund door de verschillende Azure-services, zowel de [overzicht van metrische gegevens in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md) en [overzicht van Azure diagnostische logboeken ](../azure-monitor/platform/diagnostic-logs-overview.md) artikelen.
- Lees deze artikelen voor meer informatie over eventhubs:
   - [Wat is Azure Event Hubs?](../event-hubs/event-hubs-what-is-event-hubs.md)
   - [Aan de slag met Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
- Lezen [metrische gegevens en logboeken met diagnostische gegevens downloaden uit Azure Storage](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-blobs).
- Lezen [begrijpen zoekopdrachten in Logboeken van Azure Monitor](../log-analytics/log-analytics-log-search-new.md).

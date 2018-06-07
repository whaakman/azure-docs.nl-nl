---
title: Diagnostische logboekregistratie van Azure DB Cosmos | Microsoft Docs
description: Met deze zelfstudie kunt u aan de slag met Azure Cosmos DB logboekregistratie.
services: cosmos-db
author: SnehaGunda
manager: kfile
tags: azure-resource-manager
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/07/2018
ms.author: sngun
ms.openlocfilehash: 66ee0856851a301a6849b71b64cb904c925ad18d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34612211"
---
# <a name="azure-cosmos-db-diagnostic-logging"></a>Diagnostische logboekregistratie van Azure DB Cosmos

Nadat u begint met het gebruik van een of meer Azure DB die Cosmos-databases, u kunt controleren hoe en wanneer uw databases worden geopend. Dit artikel bevat een overzicht van de logboeken die beschikbaar op de Azure-platform zijn. U informatie over het inschakelen van diagnostische logboekregistratie voor controledoeleinden logboeken te verzenden [Azure Storage](https://azure.microsoft.com/services/storage/), hoe u de logboeken om te streamen [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), en de logboeken om te exporteren [Azure Log Analytics ](https://azure.microsoft.com/services/log-analytics/).

## <a name="logs-available-in-azure"></a>Logboeken beschikbaar in Azure

Voordat we over het bewaken van uw Azure DB die Cosmos-account hebben, laten we een aantal zaken over logboekregistratie en controle te verduidelijken. Er zijn verschillende typen logboeken op de Azure-platform. Er zijn [Azure activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), [Azure diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs), [Azure metrische gegevens](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics), gebeurtenissen, heartbeat-bewaking, operations-Logboeken, enzovoort. Er is een breed spectrum van Logboeken. Ziet u de volledige lijst met Logboeken in [Azure Log Analytics](https://azure.microsoft.com/services/log-analytics/) in de Azure portal. 

De volgende afbeelding toont de verschillende soorten Azure logboeken die beschikbaar zijn:

![Verschillende soorten Azure Logboeken](./media/logging/azurelogging.png)

In de installatiekopie, het **Rekenresources** vertegenwoordigen de waarvoor u toegang hebt tot het Gastbesturingssysteem van de Microsoft Azure-resources. Bijvoorbeeld, virtuele Azure-Machines, virtuele machine schalen sets, Azure Container Service en enzovoort, worden beschouwd als rekenresources. COMPUTE resources activiteitenlogboeken, diagnostische logboeken en toepassingslogboeken genereren. Raadpleeg voor meer informatie de [Azure Monitoring: Rekenresources](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#azure-monitor-sources---compute-subset) artikel.

De **niet rekenresources** bronnen kan u toegang tot het onderliggende besturingssysteem en werk rechtstreeks met de resource. Bijvoorbeeld, Netwerkbeveiligingsgroepen, Logic Apps, enzovoort. Azure Cosmos-database is een niet-compute-resource. U kunt de logboeken voor niet-rekenresources in het activiteitenlogboek weergeven of schakelt u de optie Logboeken met diagnostische gegevens in de portal. Raadpleeg voor meer informatie de [Azure Monitoring: niet-compute resources](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#azure-monitor-sources---everything-else) artikel.

Het activiteitenlogboek registreert de bewerkingen op het abonnementsniveau van een voor Azure Cosmos DB. Bewerkingen zoals ListKeys en Write DatabaseAccounts worden geregistreerd. Diagnostische logboeken bieden meer gedetailleerde logboekregistratie en kunnen u aan te melden DataPlaneRequests (maken, lezen, Query, enzovoort) en MongoRequests.


We richten op de Azure Activity Log, Azure diagnostische logboeken en Azure metrische gegevens in dit artikel. Wat is het verschil tussen deze drie logboeken? 

### <a name="azure-activity-log"></a>Azure Activity Log

De Azure Activity Log is een abonnementlogboek biedt inzicht in het abonnement op gebeurtenissen die hebben plaatsgevonden in Azure. Het activiteitenlogboek rapporten besturingselement vlak gebeurtenissen voor uw abonnementen onder de beheercategorie. U kunt het activiteitenlogboek gebruiken om te bepalen de ' wat, wie, en wanneer ' voor een schrijfbewerking (PUT, POST, verwijderen) op de resources in uw abonnement. U kunt ook de status van de bewerking en andere relevante eigenschappen begrijpen. 

Het activiteitenlogboek verschilt van diagnostische logboeken. Het activiteitenlogboek biedt gegevens over de bewerkingen van een resource van buitenaf (de _besturingselement vlak_). In de context Azure Cosmos DB maken besturingselement vlak bewerkingen behoren verzameling, lijst met sleutels, delete-sleutels, lijst database, enzovoort. Logboeken met diagnostische gegevens worden gegenereerd door een resource en bieden informatie over de werking van de bron (de _gegevens vlak_). Enkele voorbeelden van de bewerkingen voor het vlak van gegevens in de diagnostische logboeken zijn Delete, Insert en ReadFeed.

Activiteitenlogboeken (vlak beheerbewerkingen) kunnen uitgebreidere van aard en de volledige e-mailadres van de aanroeper, aanroeper IP-adres, Resourcenaam, naam van de bewerking, TenantId en meer kunnen opnemen. Het activiteitenlogboek bevat verschillende [categorieën](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-activity-log-schema) van gegevens. Zie voor volledige informatie over de schema's uit deze categorieën [Azure Activity Log gebeurtenis schema](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-activity-log-schema). Diagnostische logboeken kan echter zijn beperkende aard als persoonlijke gegevens vaak wordt verwijderd uit deze logboeken. Mogelijk hebt u het IP-adres van de aanroepfunctie, maar de laatste octant is verwijderd.

### <a name="azure-metrics"></a>Metrische gegevens voor Azure

[Azure metrische gegevens](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) het belangrijkste soort Azure telemetrische gegevens (ook wel _prestatiemeteritems_) die wordt verzonden door de meeste Azure-resources. Metrische gegevens kunt u informatie weergeven over de doorvoer, opslag, consistentie, beschikbaarheid en de latentie van uw Azure DB die Cosmos-resources. Zie voor meer informatie [bewaking en foutopsporing met metrische gegevens in Azure Cosmos DB](use-metrics.md).

### <a name="azure-diagnostic-logs"></a>Azure diagnostische logboeken

Azure diagnostische logboeken worden gegenereerd door een resource en leveren van uitgebreide, regelmatig gegevens over de werking van de bron. De inhoud van deze logboeken varieert per resourcetype. Diagnostische logboeken niveau resource afwijken van de Gast OS-niveau diagnostische logboeken. Gastbesturingssysteem logboeken met diagnostische gegevens worden verzameld door een agent die wordt uitgevoerd binnen een virtuele machine of andere ondersteund resourcetype. Diagnostische logboeken niveau resource vereisen geen gegevens van de resource-specifieke agent en vastleggen van de Azure-platform zelf. Diagnostische logboeken van Gast OS-niveau vastleggen van gegevens van het besturingssysteem en toepassingen die worden uitgevoerd op een virtuele machine.

![Diagnostische logboekregistratie voor opslag, Event Hubs of Log Analytics](./media/logging/azure-cosmos-db-logging-overview.png)

### <a name="what-is-logged-by-azure-diagnostic-logs"></a>Wat is vastgelegd door diagnostische logboeken van Azure?

* Alle back-end voor geverifieerde aanvragen (TCP/REST) voor alle API's worden vastgelegd, met inbegrip van mislukte aanvragen als gevolg van toegangsmachtigingen, systeemfouten of ongeldige aanvragen. Ondersteuning voor de gebruiker gestart grafiek, Cassandra en tabel API-aanvragen zijn niet beschikbaar.
* Bewerkingen in de database zelf, waaronder CRUD-bewerkingen op alle documenten, containers en -databases.
* Bewerkingen voor sleutels, waaronder maken, wijzigen of verwijderen van de sleutels.
* Niet-geverifieerde aanvragen die in een 401-respons resulteren. Bijvoorbeeld, aanvragen die geen bearer-token hebt of ongeldige of verlopen, of een ongeldig token.

<a id="#turn-on"></a>
## <a name="turn-on-logging-in-the-azure-portal"></a>Schakelt logboekregistratie in de Azure portal

Diagnostische gegevens als logboekregistratie wilt inschakelen, hebt u de volgende bronnen:

* Een bestaande Azure DB die Cosmos-account, database en container. Zie voor instructies over het maken van deze resources [een databaseaccount maken met behulp van de Azure-portal](create-sql-api-dotnet.md#create-a-database-account), [voorbeelden van Azure CLI](cli-samples.md), of [voorbeelden PowerShell](powershell-samples.md).

Om diagnostische gegevens vastleggen in de Azure portal, voer de volgende stappen uit:

1. In de [Azure-portal](https://portal.azure.com), in uw Azure DB-Cosmos-account, selecteert u **diagnostische logboeken** in de navigatiebalk aan de linkerkant en selecteer vervolgens **diagnostische gegevens inschakelen**.

    ![Schakel Diagnostische logboekregistratie voor Azure Cosmos DB in de Azure portal](./media/logging/turn-on-portal-logging.png)

2. In de **diagnostische instellingen** pagina, voert u de volgende stappen uit: 

    * **Naam**: Voer een naam voor de logboeken te maken.

    * **Archiveren naar een opslagaccount**: als u deze optie, moet u een bestaand opslagaccount verbinding maken met. Zie voor informatie over het maken van een nieuw opslagaccount in de portal [een opslagaccount maken](../storage/common/storage-create-storage-account.md) en volg de instructies voor het maken van een Azure Resource Manager-account voor algemene doeleinden. Keer vervolgens terug naar deze pagina in de portal voor uw storage-account selecteren. Het kan enkele minuten duren voordat nieuwe storage-accounts worden weergegeven in de vervolgkeuzelijst.
    * **Stream naar een event hub**: als u deze optie, moet u een bestaande Event Hubs-naamruimte en event hub verbinding maken met. Zie het maken van een naamruimte Event Hubs [een Event Hubs-naamruimte en een event hub maken met de Azure-portal](../event-hubs/event-hubs-create.md). Keer vervolgens terug naar deze pagina in de portal om de naam van naamruimte en beleid Event Hubs te selecteren.
    * **Verzenden met logboekanalyse**: voor deze optie gebruikt, gebruik een bestaande werkruimte of maak een nieuwe werkruimte voor logboekanalyse volgens de stappen voor het [Maak een nieuwe werkruimte](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace) in de portal. Zie voor meer informatie over het weergeven van uw logboeken in logboekanalyse [weergave wordt geregistreerd in logboekanalyse](#view-in-loganalytics).
    * **Meld u DataPlaneRequests**: Selecteer deze optie om de back-end logboekaanvragen van het onderliggende Azure Cosmos DB gedistribueerde platform voor SQL, grafiek, MongoDB, Cassandra en tabel API-accounts. Als u naar een opslagaccount archiveren wilt, kunt u de bewaarperiode voor de diagnostische logboeken. Logboeken worden automatisch verwijderd nadat de bewaartermijn is verstreken.
    * **Meld u MongoRequests**: Selecteer deze optie om de gebruiker gestart aanvragen van de Azure DB die Cosmos-front-end voor API MongoDB-accounts voor aanmelden. Als u naar een opslagaccount archiveren wilt, kunt u de bewaarperiode voor de diagnostische logboeken. Logboeken worden automatisch verwijderd nadat de bewaartermijn is verstreken.
    * **Metrische aanvragen**: Selecteer deze optie om de uitgebreide gegevens opslaan in [Azure metrische gegevens](../monitoring-and-diagnostics/monitoring-supported-metrics.md). Als u naar een opslagaccount archiveren wilt, kunt u de bewaarperiode voor de diagnostische logboeken. Logboeken worden automatisch verwijderd nadat de bewaartermijn is verstreken.

3. Selecteer **Opslaan**.

    Als u een foutbericht ontvangt ' kan niet bijwerken van diagnostische gegevens voor \<Werkruimtenaam >. Het abonnement \<abonnements-id > is niet geregistreerd voor het gebruik van microsoft.insights, ' Ga als volgt de [oplossen Azure Diagnostics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage) instructies voor het registreren van het account en voer deze procedure vervolgens opnieuw uit.

    Als u wijzigen hoe uw logboeken met diagnostische gegevens in de toekomst op elk gewenst moment worden opgeslagen wilt, kunt u terugkeren naar deze pagina om de instellingen diagnostische logboeken voor uw account te wijzigen.

## <a name="turn-on-logging-by-using-azure-cli"></a>Logboekregistratie inschakelen met behulp van Azure CLI

Om metrische gegevens en logboekregistratie van diagnostische gegevens inschakelen met behulp van Azure CLI, gebruikt u de volgende opdrachten:

- Om de opslag van diagnostische logboeken in een opslagaccount, gebruikt u deze opdracht:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   De `resourceId` is de naam van de Azure DB die Cosmos-account. De `storageId` is de naam van het opslagaccount waarnaar u wilt de logboeken verzenden.

- Om streaming van logboeken met diagnostische gegevens naar een event hub, gebruikt u deze opdracht:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   De `resourceId` is de naam van de Azure DB die Cosmos-account. De `serviceBusRuleId` is een tekenreeks met deze indeling:

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- Gebruik deze opdracht zodat verzenden logboeken met diagnostische gegevens naar een Log Analytics-werkruimte:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
   ```

U kunt deze parameters zodat meerdere uitvoeropties combineren.

## <a name="turn-on-logging-by-using-powershell"></a>Logboekregistratie inschakelen met behulp van PowerShell

Als u wilt vastleggen van diagnostische gegevens inschakelen met behulp van PowerShell, moet u Azure Powershell met een versie 1.0.1 of hoger.

Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) om Azure PowerShell te installeren en te koppelen aan uw Azure-abonnement.

Als u Azure PowerShell al hebt geïnstalleerd en u niet welke versie van het type van de console PowerShell weet `(Get-Module azure -ListAvailable).Version`.  

### <a id="connect"></a>Verbinding maken met uw abonnementen
Start een Azure PowerShell-sessie en gebruik de volgende opdracht om u aan te melden bij uw Azure-account:  

```powershell
Connect-AzureRmAccount
```

Voer in het pop-upvenster in de browser uw gebruikersnaam en wachtwoord voor uw Azure-account in. Azure PowerShell haalt alle abonnementen op die zijn gekoppeld aan dit account en standaard gebruikt de eerste.

Als u meer dan één abonnement hebt, moet u wellicht de specifiek abonnement dat is gebruikt voor het maken van uw Azure sleutelkluis opgeven. Typ de volgende opdracht voor de abonnementen voor uw account:

```powershell
Get-AzureRmSubscription
```

Als u het abonnement dat is gekoppeld met de Azure DB die Cosmos-account dat u zich aanmeldt, typ de volgende opdracht:

```powershell
Set-AzureRmContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> Als u meer dan één abonnement dat is gekoppeld aan uw account hebt, is het belangrijk op te geven van het abonnement dat u wilt gebruiken.
>
>

Zie voor meer informatie over het configureren van Azure PowerShell [installeren en configureren van Azure PowerShell](/powershell/azure/overview).

### <a id="storage"></a>Een nieuw opslagaccount voor uw logboeken maken
Hoewel in deze zelfstudie u een bestaand opslagaccount voor uw Logboeken gebruiken kunt, maken we een nieuw opslagaccount dat toegewezen aan Azure DB die Cosmos-Logboeken. Voor het gemak slaan we de accountdetails van de opslag in een variabele met de naam **sa**.

Nog gemakkelijker te maken van beheer in deze zelfstudie gebruiken we dezelfde resourcegroep als degene die de Azure DB die Cosmos-database bevat. Vervang uw waarden voor de **ContosoResourceGroup**, **contosocosmosdblogs**, en **Noordelijk Centraal, VS** parameters, indien van toepassing:

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup `
-Name contosocosmosdblogs -Type Standard_LRS -Location 'North Central US'
```

> [!NOTE]
> Als u een bestaand opslagaccount gebruiken, moet het account hetzelfde abonnement gebruiken als uw abonnement Azure Cosmos DB. Het account moet ook gebruiken voor het Resource Manager-implementatiemodel, in plaats van het klassieke implementatiemodel.
>
>

### <a id="identify"></a>De Azure DB die Cosmos-account voor uw logboeken identificeren
De naam van de Azure DB die Cosmos-account aan een variabele met de naam ingesteld **account**, waarbij **ResourceName** is de naam van de Azure DB die Cosmos-account.

```powershell
$account = Get-AzureRmResource -ResourceGroupName ContosoResourceGroup `
-ResourceName contosocosmosdb -ResourceType "Microsoft.DocumentDb/databaseAccounts"
```

### <a id="enable"></a>Logboekregistratie inschakelen
U schakelt de logboekregistratie voor Azure Cosmos DB gebruiken de `Set-AzureRmDiagnosticSetting` cmdlet uit met de variabelen voor het nieuwe opslagaccount, Azure DB die Cosmos-account en de categorie voor logboekregistratie wilt inschakelen. Voer de volgende opdracht en stel de **-ingeschakeld** markering **$true**:

```powershell
Set-AzureRmDiagnosticSetting  -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories DataPlaneRequests
```

De uitvoer voor de opdracht moet eruitzien als in het volgende voorbeeld:

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

De uitvoer van de opdracht wordt bevestigd dat logboekregistratie nu is ingeschakeld voor uw database en de informatie wordt opgeslagen naar uw opslagaccount.

U kunt eventueel ook het bewaarbeleid instellen voor uw Logboeken, zodat oudere logboeken worden automatisch verwijderd. Bijvoorbeeld, stel het bewaarbeleid met de **- RetentionEnabled** vlag ingesteld op **$true**. Stel de **- RetentionInDays** -parameter voor **90** zodat logboeken ouder is dan 90 dagen automatisch worden verwijderd.

```powershell
Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`
 -StorageAccountId $sa.Id -Enabled $true -Categories DataPlaneRequests`
  -RetentionEnabled $true -RetentionInDays 90
```

### <a id="access"></a>Toegang tot uw logboeken
Azure DB van de Cosmos-logboeken voor de **DataPlaneRequests** categorie worden opgeslagen in de **insights-logboeken--vlak-gegevensaanvragen** container in het opslagaccount dat u hebt opgegeven. 

Maak eerst een variabele voor de containernaam. De variabele wordt gebruikt in de procedure.

```powershell
    $container = 'insights-logs-dataplanerequests'
```

Als u alle blobs in deze container wilt weergeven, typt u:

```powershell
Get-AzureStorageBlob -Container $container -Context $sa.Context
```

De uitvoer voor de opdracht moet eruitzien als in het volgende voorbeeld:

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

Als u in deze uitvoer zien kunt, de blobs de volgende naamgevingsregels: `resourceId=/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<Database Account Name>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/filename.json`

De datum- en tijdwaarden maken gebruik van UTC.

Aangezien hetzelfde opslagaccount kan worden gebruikt voor het verzamelen van Logboeken voor meerdere resources, kunt u de volledig gekwalificeerde resource-ID in de blob-naam voor toegang tot en het downloaden van de specifieke blobs die u nodig hebt. Voordat we dat doen, uitgelegd we hoe alle blobs downloaden.

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

Wanneer u deze tweede opdracht uitvoert de **/** scheidingsteken in de blobnamen een volledige mapstructuur onder de doelmap maakt. Deze mapstructuur wordt gebruikt om te downloaden en opslaan van de blobs als bestanden.

Als u alleen specifieke blobs wilt downloaden, moet u jokertekens gebruiken. Bijvoorbeeld:

* Als u meerdere databases hebt en wilt downloaden van Logboeken voor slechts één database met de naam **CONTOSOCOSMOSDB3**, gebruikt u de opdracht:

    ```powershell
    Get-AzureStorageBlob -Container $container `
     -Context $sa.Context -Blob '*/DATABASEACCOUNTS/CONTOSOCOSMOSDB3
    ```

* Als u meerdere resourcegroepen en wilt downloaden van Logboeken voor slechts één resourcegroep hebt, gebruikt u de opdracht `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

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

* Als u wilt de status van diagnostische instellingen voor uw resource database opvragen, gebruikt u de opdracht `Get-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`.
* Uitschakelen van logboekregistratie van het **DataPlaneRequests** categorie voor uw resource database-account gebruikt u de opdracht `Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories DataPlaneRequests`.


De blobs die worden geretourneerd in elk van deze query's zijn opgeslagen als tekst en opgemaakt als een JSON-blob, zoals wordt weergegeven in de volgende code:

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

Zie voor meer informatie over de gegevens in elke blob JSON, [uw logboeken Azure Cosmos DB interpreteren](#interpret).

## <a name="manage-your-logs"></a>Uw logboeken beheren

Diagnostische logboeken beschikbaar gesteld in uw account gedurende 2 uur vanaf het moment dat de Azure DB die Cosmos-bewerking is uitgevoerd. Het is aan u om uw logboeken in uw opslagaccount te beheren:

* Gebruik standaard toegang tot Azure besturingselement methoden voor het beveiligen van uw logboeken en beperken wie er toegang toe hebben.
* Verwijder de logboeken die u niet meer in uw opslagaccount wilt bewaren.
* De bewaarperiode voor gegevens vlak aanvragen die naar een opslagaccount worden opgeslagen in de portal is geconfigureerd als de **logboek DataPlaneRequests** instelling is geselecteerd. Als u wilt wijzigen die instelling, Zie [schakelt logboekregistratie in de Azure portal](#turn-on-logging-in-the-azure-portal).


<a id="#view-in-loganalytics"></a>
## <a name="view-logs-in-log-analytics"></a>Logboeken bekijken in Log Analytics

Als u hebt geselecteerd de **verzenden met logboekanalyse** optie bij het inschakelen van diagnostische logboekregistratie diagnostische gegevens van uw verzameling wordt doorgestuurd naar logboekanalyse binnen twee uur. Wanneer u Log Analytics bekijkt onmiddellijk nadat u logboekregistratie inschakelt, kunt u geen gegevens niet zien. Twee uur wachten en probeer het opnieuw 

Voordat u uw logboeken bekijken, te zien als uw werkruimte voor logboekanalyse is bijgewerkt naar de nieuwe Log Analytics query language gebruiken. Als u wilt controleren, opent u de [Azure-portal](https://portal.azure.com), selecteer **logboekanalyse** uiterst links, selecteer vervolgens de naam van de werkruimte zoals weergegeven in de volgende afbeelding. De **OMS-werkruimte** pagina wordt weergegeven:

![Log Analytics in de Azure portal](./media/logging/azure-portal.png)

Als u het volgende bericht wordt weergegeven op de **OMS-werkruimte** pagina uw werkruimte is niet bijgewerkt voor het gebruik van de nieuwe taal. Zie voor meer informatie over het upgraden naar de nieuwe querytaal [Upgrade van uw Azure-logboekanalyse-werkruimte naar nieuwe logboek zoekopdracht](../log-analytics/log-analytics-log-search-upgrade.md). 

![Log Analytics bijwerken bericht](./media/logging/upgrade-notification.png)

U kunt uw diagnostische gegevens bekijken in logboekanalyse de **logboek zoeken** pagina in het menu links of de **Management** gebied van de pagina, zoals wordt weergegeven in de volgende afbeelding:

![Logboek zoekopties in de Azure portal](./media/logging/log-analytics-open-log-search.png)

Nu dat u gegevensverzameling hebt ingeschakeld, worden uitgevoerd in het volgende voorbeeld van logboek zoeken met behulp van de nieuwe querytaal om te zien van de logboeken van de 10 meest recente `AzureDiagnostics | take 10`.

![Voorbeeld logboek zoeken naar de 10 meest recente Logboeken](./media/logging/log-analytics-query.png)

<a id="#queries"></a>
### <a name="queries"></a>Query's

Hier volgen enkele aanvullende query's die u kunt invoeren in de **logboek zoeken** in om u te helpen bij het bewaken van uw Azure DB die Cosmos-containers. Deze query's werken met de [nieuwe taal](../log-analytics/log-analytics-log-search-upgrade.md). 

Zie voor meer informatie over de betekenis van de gegevens die door elke logboek-zoekopdracht wordt geretourneerd, [uw logboeken Azure Cosmos DB interpreteren](#interpret).

* Zoeken naar alle diagnostische logboeken van de Azure Cosmos-database voor een opgegeven periode:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
    ```

* Als u wilt zoeken naar de 10 meest recent vastgelegde gebeurtenissen:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | take 10
    ```

* Zoeken naar alle bewerkingen, gegroepeerd op bewerkingstype:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by OperationName
    ```

* Aan de query voor alle bewerkingen, gegroepeerd op **Resource**:

    ```
    AzureActivity | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by Resource
    ```

* Query uitvoeren voor alle gebruikersactiviteit, gegroepeerd per resource:

    ```
    AzureActivity | where Caller == "test@company.com" and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by Resource
    ```
    > [!NOTE]
    > Deze opdracht is voor een logboek, niet een diagnostische logboeken.

* Query uitvoeren waarvoor operations langer duren dan 3 milliseconden:

    ```
    AzureDiagnostics | where toint(duration_s) > 30000 and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by clientIpAddress_s, TimeGenerated
    ```

* Query voor welke agent de bewerkingen uitgevoerd:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by OperationName, userAgent_s
    ```

* Zoeken naar wanneer de langlopende bewerkingen zijn uitgevoerd:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | project TimeGenerated , toint(duration_s)/1000 | render timechart
    ```

Zie voor meer informatie over het gebruik van de nieuwe zoekopdracht logboek taal [Understand logboek zoekopdrachten in logboekanalyse](../log-analytics/log-analytics-log-search-new.md). 

## <a id="interpret"></a>Uw logboeken interpreteren

Diagnostische gegevens die zijn opgeslagen in Azure Storage en Log Analytics maakt gebruik van een vergelijkbaar schema. 

De volgende tabel beschrijft de inhoud van elke logboekvermelding.

| Azure Storage-veld of eigenschap | Log Analytics-eigenschap | Beschrijving |
| --- | --- | --- |
| **Tijd** | **TimeGenerated** | De datum en tijd (UTC) wanneer de bewerking opgetreden. |
| **ResourceId** | **Resource** | Het account voor Azure Cosmos DB waarvoor logboeken zijn ingeschakeld.|
| **Categorie** | **Categorie** | Voor Azure DB die Cosmos-logboeken **DataPlaneRequests** is de enige beschikbare waarde. |
| **OperationName** | **OperationName** | Naam van de bewerking. Deze waarde kan een van de volgende bewerkingen zijn: maken, bijwerken, lezen, ReadFeed, verwijderen, vervangen, uitvoeren, SqlQuery, Query, JSQuery, Head, HeadFeed of Upsert.   |
| **Eigenschappen** | N.v.t. | De inhoud van dit veld worden beschreven in de volgende rijen. |
| **ActivityId** | **activityId_g** | De unieke GUID voor de geregistreerde bewerking. |
| **UserAgent** | **userAgent_s** | Een tekenreeks die opgeeft van de clientagent van de gebruiker die de aanvraag uitvoert. De indeling is {agent gebruikersnaam} / {version}.|
| **resourceType** | **ResourceType** | Het type van de toegang tot bronnen. Deze waarde kan een van de volgende resourcetypen zijn: Database, verzameling, Document, bijlage, gebruiker, machtiging, opgeslagen procedure, Trigger, UserDefinedFunction of aanbieding. |
| **statusCode** | **statusCode_s** | De status van de reactie van de bewerking. |
| **requestResourceId** | **ResourceId** | De resourceId die betrekking op de aanvraag hebben. De waarde kan verwijzen naar databaseRid, collectionRid of documentRid afhankelijk van de bewerking uitgevoerd.|
| **clientIpAddress** | **clientIpAddress_s** | De client-IP-adres. |
| **requestCharge** | **requestCharge_s** | Het aantal RUs die worden gebruikt door de bewerking |
| **collectionRid** | **collectionId_s** | De unieke ID voor de verzameling.|
| **Duur** | **duration_s** | De duur van de bewerking, in de maatstreepjes. |
| **requestLength** | **requestLength_s** | De lengte van de aanvraag, in bytes. |
| **responseLength** | **responseLength_s** | De lengte van het antwoord, in bytes.|
| **resourceTokenUserRid** | **resourceTokenUserRid_s** | Deze waarde is niet-lege wanneer [resource tokens](https://docs.microsoft.com/azure/cosmos-db/secure-access-to-data#resource-tokens) worden gebruikt voor verificatie. De waarde verwijst naar de resource-ID van de gebruiker. |

## <a name="next-steps"></a>Volgende stappen

- Lees voor informatie over het inschakelen van logboekregistratie en de metrische gegevens en logboekbestanden categorieën die worden ondersteund door de verschillende Azure-services, zowel de [overzicht van metrische gegevens in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md) en [overzicht van Azure diagnostische logboeken ](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artikelen.
- Deze artikelen voor meer informatie over event hubs gelezen:
   - [Wat is Azure Event Hubs?](../event-hubs/event-hubs-what-is-event-hubs.md)
   - [Aan de slag met Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
- Lees [metrische gegevens en diagnostische logboeken downloaden vanaf Azure Storage](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-blobs).
- Lees [Understand logboek zoekopdrachten in logboekanalyse](../log-analytics/log-analytics-log-search-new.md).

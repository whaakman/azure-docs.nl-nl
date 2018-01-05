---
title: Logboekregistratie voor Azure Analysis Services Diganostic | Microsoft Docs
description: Meer informatie over het instellen van diagnostische logboekregistratie voor Azure Analysis Services.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: 
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 12/29/2017
ms.author: owend
ms.openlocfilehash: 02c25de980b399812676285ad3f87f60af93265f
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="setup-diagnostic-logging"></a>Diagnostische logboekregistratie instellen

Een belangrijk onderdeel van een Analysis Services-oplossing wordt bewaakt hoe uw servers uitvoert. Met [diagnostische logboeken van Azure-resource](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md), u kunt controleren en logboeken verzenden [Azure Storage](https://azure.microsoft.com/services/storage/), ze streamt [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), en ze te exporteren [logboek Analytics](https://azure.microsoft.com/services/log-analytics/), deel uit van [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite). 

![Diagnostische logboekregistratie voor opslag, Event Hubs of Operations Management Suite via Log Analytics](./media/analysis-services-logging/aas-logging-overview.png)


## <a name="whats-logged"></a>Wat wordt geregistreerd?

U kunt selecteren **Engine**, **Service**, en **metrische gegevens** categorieën.

### <a name="engine"></a>Engine

Selecteren **Engine** registreert alle [xEvents](https://docs.microsoft.com/sql/analysis-services/instances/monitor-analysis-services-with-sql-server-extended-events). U kunt geen afzonderlijke gebeurtenissen selecteren. 

|XEvent-categorieën |De naam van gebeurtenis  |
|---------|---------|
|Beveiligingscontrole    |   Audit aanmelding      |
|Beveiligingscontrole    |   Audit afmelden      |
|Beveiligingscontrole    |   Controleren van de Server wordt gestart of gestopt      |
|Van voortgangsrapporten     |   Voortgang rapport Begin      |
|Van voortgangsrapporten     |   Voortgang rapport einde      |
|Van voortgangsrapporten     |   Voortgang rapport huidige      |
|Query's     |  Begin van de query       |
|Query's     |   Einde van de query      |
|Opdrachten     |  Begin van de opdracht       |
|Opdrachten     |  Einde van de opdracht       |
|Fouten en waarschuwingen     |   Fout      |
|Ontdekken     |   Einde detecteren      |
|Melding     |    Melding     |
|Sessie     |  Sessie initialiseren       |
|Vergrendelingen    |  impasse       |
|Verwerking van query 's     |   Begin van VertiPaq SE-Query      |
|Verwerking van query 's     |   Einde van VertiPaq SE-Query      |
|Verwerking van query 's     |   Cache overeenkomst van VertiPaq SE-Query      |
|Verwerking van query 's     |   Begin van de directe Query      |
|Verwerking van query 's     |  Einde van de directe Query       |

### <a name="service"></a>Service

|Naam van bewerking  |Deze gebeurtenis treedt op wanneer  |
|---------|---------|
|CreateGateway     |   Gebruiker configureert een gateway op server      |
|ResumeServer     |    Hervatten van een server     |
|SuspendServer    |   Onderbreken van een server      |
|DeleteServer     |    Een server verwijderen     |
|RestartServer    |     Gebruiker opnieuw wordt opgestart een server via SSMS of PowerShell    |
|GetServerLogFiles    |    Gebruiker exporteert u het logboek van de server via PowerShell     |
|ExportModel     |   Gebruiker exporteert een model in de portal met behulp van openen in Visual Studio     |

### <a name="all-metrics"></a>Alle metrische gegevens

Hiermee wordt de categorie metrische gegevens geregistreerd dezelfde [serverstatistieken](analysis-services-monitor.md#server-metrics) weergegeven in de metrische gegevens.

## <a name="setup-diagnostics-logging"></a>Diagnostische logboekregistratie instellen

### <a name="azure-portal"></a>Azure Portal

1. In [Azure-portal](https://portal.azure.com) > server, klikt u op **diagnostische logboeken** in de navigatiebalk aan de linkerkant en klik vervolgens op **diagnostische gegevens inschakelen**.

    ![Schakel Diagnostische logboekregistratie voor Azure Cosmos DB in de Azure portal](./media/analysis-services-logging/aas-logging-turn-on-diagnostics.png)

2. In **diagnostische instellingen**, het volgende doen: 

    * **Naam**. Voer een naam voor de logboeken te maken.

    * **Archiveren naar een opslagaccount**. Om deze optie gebruikt, moet u een bestaand opslagaccount verbinding maken met. Zie [een opslagaccount maken](../storage/common/storage-create-storage-account.md). Volg de instructies voor het maken van een Resource Manager voor algemene doeleinden-account. Keer vervolgens terug naar deze pagina in de portal voor uw storage-account selecteren. Duurt enkele minuten duren voordat de zojuist gemaakte storage-accounts worden weergegeven in de vervolgkeuzelijst.
    * **Stream naar een event hub**. Om deze optie gebruikt, moet u een Event Hub-naamruimte en event hub verbinding maken met. Zie het maken van een naamruimte Event Hubs [een Event Hubs-naamruimte en een event hub met de Azure portal maken](../event-hubs/event-hubs-create.md). Keer vervolgens terug naar deze pagina in de portal om de Event Hub-naamruimte en de beleid-naam te selecteren.
    * **Verzenden met logboekanalyse**. Om deze optie gebruikt, gebruik een bestaande werkruimte of maak een nieuwe werkruimte voor logboekanalyse volgens de stappen voor het [Maak een nieuwe werkruimte](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace) in de portal. Zie voor meer informatie over het weergeven van uw logboeken in logboekanalyse [weergave wordt geregistreerd in logboekanalyse](#view-in-loganalytics).

    * **Engine**. Selecteer deze optie om aan te melden xEvents. Als u naar een opslagaccount archiveren wilt, kunt u de bewaarperiode voor de diagnostische logboeken. Logboeken zijn autodeleted nadat de bewaartermijn is verstreken.
    * **Service**. Selecteer deze optie om service level gebeurtenissen. Als u naar een opslagaccount archiveert, kunt u de bewaarperiode voor de diagnostische logboeken. Logboeken zijn autodeleted nadat de bewaartermijn is verstreken.
    * **Metrische gegevens**. Selecteer deze optie om de uitgebreide gegevens opslaan in [metrische gegevens](analysis-services-monitor.md#server-metrics). Als u naar een opslagaccount archiveert, kunt u de bewaarperiode voor de diagnostische logboeken. Logboeken zijn autodeleted nadat de bewaartermijn is verstreken.

3. Klik op **Opslaan**.

    Als u een foutbericht ontvangt ' kan niet bijwerken van diagnostische gegevens voor \<Werkruimtenaam >. Het abonnement \<abonnements-id > is niet geregistreerd voor het gebruik van microsoft.insights. " Ga als volgt de [oplossen Azure Diagnostics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage) instructies voor het registreren van het account en probeer deze procedure.

    Als u wilt wijzigen hoe uw logboeken met diagnostische gegevens in de toekomst op elk gewenst moment worden opgeslagen, kunt u terugkeren naar deze pagina om instellingen te wijzigen.

### <a name="powershell"></a>PowerShell

Hier volgen de basisopdrachten weer om u doorgaat. Als u stapsgewijze instructies wilt over het instellen van logboekregistratie in een opslagaccount met behulp van PowerShell, raadpleegt u de zelfstudie verderop in dit artikel.

Gebruik de volgende opdrachten zodat metrische gegevens en diagnostische logboekregistratie met behulp van PowerShell:

- Om opslag van diagnostische logboeken in een opslagaccount, gebruikt u deze opdracht:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   De storage-account-ID is de resource-ID voor het opslagaccount waar u de logboeken verzenden.

- Om streaming van diagnostische logboeken naar een event hub, gebruikt u deze opdracht:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   De regel-ID van Azure Service Bus is een tekenreeks met deze indeling:

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- Als u wilt verzenden diagnostische logboeken naar een werkruimte voor logboekanalyse inschakelen, gebruikt u deze opdracht:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
   ```

- U kunt de bron-ID van de werkruimte voor logboekanalyse verkrijgen via de volgende opdracht:

   ```powershell
   (Get-AzureRmOperationalInsightsWorkspace).ResourceId
   ```

U kunt deze parameters zodat meerdere uitvoeropties combineren.

### <a name="rest-api"></a>REST-API

Meer informatie over hoe [diagnostische instellingen wijzigen met behulp van de REST-API van Azure Monitor](https://msdn.microsoft.com/library/azure/dn931931.aspx). 

### <a name="resource-manager-template"></a>Resource Manager-sjabloon

Meer informatie over hoe [diagnostische instellingen bij het maken van de resource inschakelen met behulp van een Resource Manager-sjabloon](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md). 

## <a name="manage-your-logs"></a>Uw logboeken beheren

Logboeken zijn gewoonlijk beschikbaar binnen een paar uur voor het instellen van logboekregistratie. Het is aan u om uw logboeken in uw opslagaccount te beheren:

* Gebruik standaardmethoden van Azure voor toegangsbeheer om uw logboeken te beveiligen door het aantal gebruikers te beperken dat toegang heeft tot de logboeken.
* Verwijder de logboeken die u niet meer in uw opslagaccount wilt bewaren.
* Zorg ervoor dat een bewaarperiode voor instellen zodat oude logboeken worden verwijderd uit uw storage-account.

## <a name="view-logs-in-log-analytics"></a>Logboeken bekijken in Log Analytics

Metrische gegevens en server gebeurtenissen zijn geïntegreerd met xEvents in Log Analytics voor side-by-side-analyse. Log Analytics kan ook worden geconfigureerd voor het ontvangen van gebeurtenissen van andere Azure-services biedt een holistische weergave van de diagnostische logboekgegevens over uw architectuur.

Als u wilt weergeven van de diagnostische gegevens in logboekanalyse door de zoekpagina logboek vanuit het menu links of de module beheer openen zoals hieronder wordt weergegeven.

![Meld u opties voor zoeken in de Azure portal](./media/analysis-services-logging/aas-logging-open-log-search.png)

Nu dat u gegevens verzamelen hebt ingeschakeld in **logboek zoeken**, klikt u op **alle verzamelde gegevens**.

In **Type**, klikt u op **AzureDiagnostics**, en klik vervolgens op **toepassen**. AzureDiagnostics bevat-Engine en Service-gebeurtenissen. U ziet dat een Log Analytics-query is gemaakt tijdens het. De EventClass\_s veld bevat xEvent-namen bekend als u xEvents hebt gebruikt voor on-premises logboekregistratie zien er mogelijk.

Klik op **EventClass\_s** of een van de gebeurtenisnamen en Log Analytics blijft samenstellen van een query. Zorg ervoor dat u uw query's voor hergebruik.

Zorg ervoor dat afhandeling Operations Management Suite waarmee een website met uitgebreide query, dashboarding en waarschuwen mogelijkheden op Log Analytics-gegevens.

### <a name="queries"></a>Query's

Er zijn query's die kunt u honderden. Hier volgen enkele voor u op weg.
Zie voor meer informatie over het gebruik van de nieuwe zoekopdracht logboek-querytaal, [Understanding logboek zoekt in logboekanalyse](../log-analytics/log-analytics-log-search-new.md). 

* Query retourneren query's verzonden naar Azure Analysis Services die meer dan vijf minuten (300.000 milliseconden) duurde om te voltooien.

    ```
    search * | where ( Type == "AzureDiagnostics" ) | where ( EventClass_s == "QUERY_END" ) | where toint(Duration_s) > 300000
    ```

* Scale-out replica's identificeren.

    ```
    search * | summarize count() by ServerName_s
    ```
    Wanneer u scale-out, kunt u alleen-lezen-replica's identificeren omdat de servernaam\_s veldwaarden het exemplaarnummer replica is toegevoegd aan de naam hebben. Het resourceveld bevat de Azure-resource-naam, die overeenkomt met de naam van de server die de gebruikers te zien. Het veld IsQueryScaleoutReadonlyInstance_s is gelijk aan de waarde true voor replica's.



> [!TIP]
> Hebt u een goede Log Analytics-query die u wilt delen? Als u een GitHub-account hebt, kunt u deze toevoegen aan dit artikel. Klik op **bewerken** in de rechterbovenhoek van deze pagina.


## <a name="tutorial---turn-on-logging-by-using-powershell"></a>Zelfstudie - logboekregistratie met behulp van PowerShell inschakelen
In deze snelle zelfstudie maakt u een opslagaccount in hetzelfde abonnement en dezelfde resourcegroep als de Analysis Services-server. Vervolgens gebruikt u Set-AzureRmDiagnosticSetting Diagnostische logboekregistratie, uitvoer verzenden naar het nieuwe opslagaccount inschakelen.

### <a name="prerequisites"></a>Vereisten
Deze zelfstudie hebt u de volgende bronnen:

* Een bestaande Azure Analysis Services-server. Zie voor instructies over het maken van een serverresource [maken van een server in Azure portal](analysis-services-create-server.md), of [een Azure Analysis Services-server maken met behulp van PowerShell](analysis-services-create-powershell.md).

### <a name="aconnect-to-your-subscriptions"></a></a>Verbinding maken met uw abonnementen

Start een Azure PowerShell-sessie en gebruik de volgende opdracht om u aan te melden bij uw Azure-account:  

```powershell
Login-AzureRmAccount
```

Voer in het pop-upvenster in de browser uw gebruikersnaam en wachtwoord voor uw Azure-account in. Azure PowerShell haalt alle abonnementen op die zijn gekoppeld aan dit account en gebruikt standaard het eerste abonnement.

Als u meerdere abonnementen hebt, moet u wellicht specifiek opgeven welk abonnement is gebruikt voor het maken van uw Azure Sleutelkluis. Typ het volgende als u de abonnementen voor uw account wilt zien:

```powershell
Get-AzureRmSubscription
```

Als u het abonnement dat is gekoppeld aan het Azure Analysis Services-account dat u zich aanmeldt, typ:

```powershell
Set-AzureRmContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> Als u meerdere abonnementen die zijn gekoppeld aan uw account hebt, is het belangrijk op te geven van het abonnement.
>
>

### <a name="create-a-new-storage-account-for-your-logs"></a>Een nieuw opslagaccount voor uw logboeken maken

U kunt een bestaand opslagaccount gebruiken voor uw Logboeken, mits deze zich in hetzelfde abonnement als uw server. Voor deze zelfstudie maakt u een nieuw opslagaccount toegewezen aan de logboeken van de Analysis Services. Om het gemakkelijk maken, u de accountdetails van de opslag opslaat in een variabele met de naam **sa**.

U kunt ook dezelfde resourcegroep gebruiken als de database met de Analysis Services-server. Vervang de waarden voor `awsales_resgroup`, `awsaleslogs`, en `West Central US` met uw eigen waarden:

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName awsales_resgroup `
-Name awsaleslogs -Type Standard_LRS -Location 'West Central US'
```

### <a name="identify-the-server-account-for-your-logs"></a>De serveraccount voor uw logboeken identificeren

De accountnaam ingesteld op een variabele met de naam **account**, waarbij ResourceName de naam van het account.

```powershell
$account = Get-AzureRmResource -ResourceGroupName awsales_resgroup `
-ResourceName awsales -ResourceType "Microsoft.AnalysisServices/servers"
```

### <a name="enable-logging"></a>Logboekregistratie inschakelen

Als logboekregistratie wilt inschakelen, gebruikt u de cmdlet Set-AzureRmDiagnosticSetting samen met de variabelen voor het nieuwe opslagaccount, serveraccount en de categorie. Voer de volgende opdracht, instellen van de **-ingeschakeld** markering **$true**:

```powershell
Set-AzureRmDiagnosticSetting  -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories Engine
```

De uitvoer ziet er ongeveer als volgt:

```powershell
StorageAccountId            : 
/subscriptions/a23279b5-xxxx-xxxx-xxxx-47b7c6d423ea/resourceGroups/awsales_resgroup/providers/Microsoft.Storage/storageAccounts/awsaleslogs
ServiceBusRuleId            :
EventHubAuthorizationRuleId :
Metrics                    
    TimeGrain       : PT1M
    Enabled         : False
    RetentionPolicy
    Enabled : False
    Days    : 0


Logs                       
    Category        : Engine
    Enabled         : True
    RetentionPolicy
    Enabled : False
    Days    : 0


    Category        : Service
    Enabled         : False
    RetentionPolicy
    Enabled : False
    Days    : 0


WorkspaceId                 :
Id                          : /subscriptions/a23279b5-xxxx-xxxx-xxxx-47b7c6d423ea/resourcegroups/awsales_resgroup/providers/microsoft.analysisservic
es/servers/awsales/providers/microsoft.insights/diagnosticSettings/service
Name                        : service
Type                        :
Location                    :
Tags                        :
```

Hiermee bevestigt u dat logboekregistratie nu is ingeschakeld voor de server, gegevens opslaat in de storage-account.

U kunt ook bewaarbeleid instellen voor uw Logboeken, zodat oudere logboeken worden automatisch verwijderd. Bijvoorbeeld ingesteld bewaren beleid met **- RetentionEnabled** markering **$true**, en stel **- RetentionInDays** -parameter voor **90**. Logboeken ouder is dan 90 dagen automatisch verwijderd.

```powershell
Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`
 -StorageAccountId $sa.Id -Enabled $true -Categories Engine`
  -RetentionEnabled $true -RetentionInDays 90
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [Diagnostische logboekregistratie van Azure-resource](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).

Zie [Set-AzureRmDiagnosticSetting](https://docs.microsoft.com/powershell/module/azurerm.insights/Set-AzureRmDiagnosticSetting) in de PowerShell-help.
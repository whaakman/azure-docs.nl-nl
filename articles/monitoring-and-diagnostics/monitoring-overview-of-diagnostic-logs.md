---
title: Overzicht van diagnostische logboeken in Azure
description: Leer wat diagnostische logboeken in Azure zijn en hoe u deze kunt gebruiken om te begrijpen van gebeurtenissen die plaatsvinden binnen een Azure-resource.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: johnkem
ms.component: logs
ms.openlocfilehash: a6435f74141429cbe4f9a169fd2f234161d486c4
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37918737"
---
# <a name="collect-and-consume-log-data-from-your-azure-resources"></a>Verzamelen en gebruiken van logboekgegevens van uw Azure-resources

## <a name="what-are-azure-resource-diagnostic-logs"></a>Wat zijn Azure-resource diagnostische logboeken

**Diagnostische logboeken in Azure resourceniveau** zijn Logboeken door een resource met uitgebreide, regelmatig gegevens over de werking van die resource. Resourcetype is afhankelijk van de inhoud van deze logboeken. Tellers van regels voor Network Security Group en Key Vault audits zijn bijvoorbeeld twee categorieën van Logboeken van de resource.

Diagnostische logboeken van de resource op het niveau verschillen van de [activiteitenlogboek](monitoring-overview-activity-logs.md). Het activiteitenlogboek biedt inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement met behulp van Resource Manager, bijvoorbeeld, het maken van een virtuele machine of een logische app verwijderen. Het activiteitenlogboek is een logboek op abonnementsniveau. Resourceniveau diagnoselogboeken bieden inzicht in bewerkingen die zijn uitgevoerd binnen die resource zelf, bijvoorbeeld een geheim ophalen uit een Key Vault.

Diagnostische logboeken resourceniveau afwijken van de Gast OS-niveau van diagnostische logboeken. Diagnostische logboeken van Guest OS zijn deze die worden verzameld door een agent die wordt uitgevoerd op een virtuele machine of andere ondersteund resourcetype. Diagnostische logboeken resourceniveau vereisen geen gegevens van de resource-specifieke agent en vastleggen van de Azure-platform zelf wordt geboden, terwijl Gast OS-niveau logboeken met diagnostische gegevens vastleggen van gegevens uit het besturingssysteem en toepassingen die worden uitgevoerd op een virtuele machine.

Niet alle resources ondersteuning voor het nieuwe type resource diagnostische logboeken die hier worden beschreven. In dit artikel bevat een sectie met welke resourcetypen ondersteunen de nieuwe resource-niveau van diagnostische logboeken.

![Resource diagnostische logboeken en andere typen logboeken ](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_vs_other_logs_v5.png)

## <a name="what-you-can-do-with-resource-level-diagnostic-logs"></a>U kunt doen met diagnostische logboeken resourceniveau
Hier volgen enkele dingen die u met diagnostische logboeken van resources doen kunt:

![Logische plaatsing van de Resource diagnostische logboeken](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_Actions.png)

* Opslaan naar een [ **Opslagaccount** ](monitoring-archive-diagnostic-logs.md) voor controle of handmatige controle. U kunt opgeven de bewaartermijn (in dagen) via **instellingen voor resourcediagnose**.
* [Stream ze **Event Hubs** ](monitoring-stream-diagnostic-logs-to-event-hubs.md) voor opname van een service van derden of aangepaste analyseoplossing zoals Power BI.
* Analyseren met [Log Analytics](../log-analytics/log-analytics-azure-storage.md)

U kunt een opslagaccount of Event Hubs-naamruimte die zich niet in hetzelfde abonnement bevinden als het abonnement dat Logboeken verzendt. De gebruiker die de instelling configureert, moet de juiste RBAC-toegang tot beide abonnementen hebben.

> [!NOTE]
>  U kan momenteel niet archiveren gegevens aan een storage-account die achter een beveiligd virtueel netwerk.

> [!WARNING]
> De indeling van de logboekgegevens in de storage-account wordt gewijzigd in JSON-regels op 1 november 2018. [Raadpleeg dit artikel voor een beschrijving van de impact en het bijwerken van uw hulpprogramma's voor het afhandelen van de nieuwe indeling.](./monitor-diagnostic-logs-append-blobs.md) 
>
> 

## <a name="resource-diagnostic-settings"></a>Diagnostische instellingen voor resources

Diagnostische logboeken van de resource voor niet-Compute resources met behulp van resource diagnostische instellingen zijn geconfigureerd. **Instellingen voor resourcediagnose** voor een resource-besturingselement:

* Waar resource diagnostische logboeken en metrische gegevens worden verzonden (Storage-Account, Event Hubs en/of Log Analytics).
* Welke logboekcategorieën worden verzonden en of u metrische gegevens ook worden verzonden.
* Hoe lang elke logboekcategorie moet worden bewaard in een storage-account
    - Een bewaarperiode van nul dagen betekent dat Logboeken altijd worden bewaard. De waarde kan anders een willekeurig aantal dagen tussen 1 en 2147483647 zijn.
    - Als Logboeken opslaan in een Storage-Account is uitgeschakeld (bijvoorbeeld, als er alleen Event Hubs of Log Analytics-opties zijn geselecteerd), bewaarbeleid worden ingesteld, maar hebben het bewaarbeleid geen effect.
    - Bewaarbeleid zijn toegepast per dag, dus aan het einde van een dag (UTC), logboeken van de dag dat nu is buiten de bewaarperiode van beleid worden verwijderd. Bijvoorbeeld, als u een beleid voor het bewaren van één dag had, worden aan het begin van de dag vandaag nog de logboeken van de dag voor gisteren vernietigd. De verwijderbewerking begint bij middernacht UTC, maar houd er rekening mee dat het kan tot 24 uur duren voor de logboeken worden verwijderd uit uw storage-account.

Deze instellingen gemakkelijk worden geconfigureerd via de diagnostische instellingen voor een resource in Azure portal, via Azure PowerShell en CLI-opdrachten of via de [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx).

> [!NOTE]
> Het verzenden van multidimensionale metrische gegevens via diagnostische instellingen wordt momenteel niet ondersteund. Metrische gegevens met dimensies worden geëxporteerd als platte eendimensionale metrische gegevens, als totaal van alle dimensiewaarden.
>
> *Een voorbeeld*: de meetwaarde 'Binnenkomende berichten' voor een Event Hub kan worden verkend en uitgezet op wachtrijniveau. Wanneer de waarde wordt geëxporteerd via diagnostische instellingen, wordt deze echter voorgesteld als alle binnenkomende berichten voor alle wachtrijen in de Event Hub.
>
>

> [!WARNING]
> Diagnostische logboeken en metrische gegevens van de Gast-OS-laag van Compute-resources (bijvoorbeeld virtuele machines of Service Fabric) Gebruik [een afzonderlijk mechanisme voor configuratie en de selectie van uitvoer](../azure-diagnostics.md).

## <a name="how-to-enable-collection-of-resource-diagnostic-logs"></a>Het inschakelen van verzamelen van diagnostische logboeken van resource

Verzamelen van diagnostische logboeken resource kan worden ingeschakeld [als onderdeel van een resource maken in Resource Manager-sjabloon](./monitoring-enable-diagnostic-logs-using-template.md) of wanneer een resource is gemaakt op de pagina van de resource in de portal. U kunt ook de verzameling op elk gewenst moment met behulp van Azure PowerShell of CLI-opdrachten, of de REST-API van Azure Monitor inschakelen.

> [!TIP]
> Deze instructies mogelijk niet van toepassing rechtstreeks aan elke resource. Zie de koppelingen schema onderaan deze pagina om te begrijpen speciale stappen die van toepassing op bepaalde resourcetypen zijn.

### <a name="enable-collection-of-resource-diagnostic-logs-in-the-portal"></a>Verzamelen van diagnostische logboeken van de resource in de portal inschakelen

U kunt verzamelen van diagnostische logboeken van de resource in Azure portal inschakelen nadat een resource is gemaakt door te gaan naar een specifieke resource of door te navigeren naar Azure Monitor. Dit inschakelen via Azure Monitor:

1. In de [Azure-portal](http://portal.azure.com), gaat u naar Azure Monitor en klikt u op **diagnostische instellingen**

    ![Sectie van Azure Monitor bewaking](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

2. (Optioneel) de lijst met door de resourcegroep of resourcetype filteren en klik vervolgens op de resource waarvoor u wilt een diagnostische instelling instellen.

3. Als er geen instellingen zijn op de resource hebt u geselecteerd, wordt u gevraagd om een instelling te maken. Klik op "Diagnostische gegevens inschakelen."

   ![Diagnostische instelling - er zijn geen bestaande instellingen toevoegen](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-none.png)

   Als er bestaande instellingen op de resource, ziet u een lijst met instellingen die al zijn geconfigureerd voor deze resource. Klik op 'Diagnostische instelling toevoegen'.

   ![Diagnostische instelling - bestaande instellingen toevoegen](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-multiple.png)

3. Geef een naam van uw instelling, schakel de selectievakjes voor elk doel waarnaar u wilt verzenden van gegevens en het configureren welke resource moet worden gebruikt voor elk doel. (Optioneel) Stel een aantal dagen te bewaren van deze logboeken met behulp van de **bewaarperiode (dagen)** schuifregelaars (alleen van toepassing op de bestemming van storage-account). Een bewaarperiode van nul dagen worden de logboeken voor onbepaalde tijd opgeslagen.

   ![Diagnostische instelling - bestaande instellingen toevoegen](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-configure.png)

4. Klik op **Opslaan**.

Na enkele ogenblikken wordt de nieuwe instelling wordt weergegeven in de lijst met instellingen voor deze resource en logboeken met diagnostische gegevens worden verzonden naar de opgegeven bestemmingen zodra de gegevens van een nieuwe gebeurtenis wordt gegenereerd.

### <a name="enable-collection-of-resource-diagnostic-logs-via-powershell"></a>Inschakelen van verzamelen van diagnostische logboeken van de resource via PowerShell

Om het verzamelen van diagnostische logboeken van de resource via Azure PowerShell mogelijk, gebruik de volgende opdrachten:

Om in te schakelen opslag van logboeken met diagnostische gegevens in een opslagaccount, gebruikt u deze opdracht:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
```

De storage-account-ID is de resource-ID voor het opslagaccount waarnaar u wilt de logboeken te zenden.

Als u wilt inschakelen voor streaming van logboeken met diagnostische gegevens naar een event hub, gebruikt u deze opdracht:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your Service Bus rule id] -Enabled $true
```

De regel-ID van service bus is een tekenreeks zijn met deze indeling: `{Service Bus resource ID}/authorizationrules/{key name}`.

Als u wilt inschakelen met het verzenden van diagnostische logboeken naar Log Analytics-werkruimte, moet u deze opdracht gebruiken:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
```

U vindt de resource-ID van uw Log Analytics-werkruimte met de volgende opdracht:

```powershell
(Get-AzureRmOperationalInsightsWorkspace).ResourceId
```

U kunt deze parameters voor het inschakelen van meerdere uitvoeropties combineren.

### <a name="enable-collection-of-resource-diagnostic-logs-via-azure-cli-20"></a>Inschakelen van verzamelen van diagnostische logboeken van de resource via Azure CLI 2.0

Om het verzamelen van diagnostische logboeken van de resource via de Azure CLI 2.0 mogelijk, gebruikt u de [az monitor diagnostic-settings maken](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) opdracht.

Opslag van logboeken met diagnostische gegevens in een Storage-Account inschakelen:

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --storage-account <name or ID of storage account> \
    --resource <target resource object ID> \
    --resource-group <storage account resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true,
        "retentionPolicy": {
            "days": <# days to retain>,
            "enabled": true
        }
    }]'
```

De `--resource-group` argument is alleen vereist als `--storage-account` is niet een object-ID.

Om in te schakelen voor streaming van logboeken met diagnostische gegevens naar een event hub:

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --event-hub <event hub name> \
    --event-hub-rule <event hub rule ID> \
    --resource <target resource object ID> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true
    }
    ]'
```

De regel-ID is een tekenreeks zijn met deze indeling: `{Service Bus resource ID}/authorizationrules/{key name}`.

Voor het verzenden van diagnostische logboeken naar Log Analytics-werkruimte:

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --workspace <log analytics name or object ID> \
    --resource <target resource object ID> \
    --resource-group <log analytics workspace resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true
    }
    ]'
```

De `--resource-group` argument is alleen vereist als `--workspace` is geen object-ID

Met een opdracht kunt u extra categorieën toevoegen aan de diagnostische logboeken van woordenlijsten toe te voegen aan de JSON-matrix die is doorgegeven als de `--logs` parameter. U kunt combineren de `--storage-account`, `--event-hub`, en `--workspace` parameters voor het inschakelen van meerdere opties voor uitvoer.

### <a name="enable-collection-of-resource-diagnostic-logs-via-rest-api"></a>Inschakelen van verzamelen van diagnostische logboeken van de resource via REST-API

Diagnostische instellingen met de Azure Monitor REST API, Zie [dit document](https://msdn.microsoft.com/library/azure/dn931931.aspx).

## <a name="manage-resource-diagnostic-settings-in-the-portal"></a>Diagnostische instellingen van de resource in de portal beheren

Zorg ervoor dat al uw resources met de diagnostische instellingen zijn ingesteld. Navigeer naar **Monitor** in de portal en open **diagnostische instellingen**.

![Blade met diagnostische logboeken in de portal](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-nav.png)

Mogelijk moet u klikt u op 'Alle services' op zoek naar de sectie Monitor.

Hier kunt u weergeven en filteren van alle resources die ondersteuning bieden voor diagnostische instellingen om te zien als ze diagnostische gegevens zijn ingeschakeld. U kunt ook inzoomen op Zie als meerdere instellingen zijn ingesteld op een resource en welke storage-account, Event Hubs-naamruimte en/of Log Analytics-werkruimte die gegevens om te stromen controleren.

![Diagnostische logboeken resultaten in de portal](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

Een diagnostische instelling toe te voegen, wordt de diagnostische instellingen weergeven, waar u kunt inschakelen, uitschakelen of wijzigen van de diagnostische instellingen voor de geselecteerde resource.

## <a name="supported-services-categories-and-schemas-for-resource-diagnostic-logs"></a>Ondersteunde services, categorieën en schema's voor diagnostische logboeken van resource

[Raadpleeg dit artikel](monitoring-diagnostic-logs-schema.md) voor een volledige lijst van ondersteunde services en de logboekcategorieën en schema's die worden gebruikt door deze services.

## <a name="next-steps"></a>Volgende stappen

* [Diagnostische logboeken van de resource naar Stream **Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Diagnostische instellingen voor resources met behulp van de REST-API van Azure Monitor wijzigt](https://msdn.microsoft.com/library/azure/dn931931.aspx)
* [Logboeken van Azure storage met Log Analytics analyseren](../log-analytics/log-analytics-azure-storage.md)

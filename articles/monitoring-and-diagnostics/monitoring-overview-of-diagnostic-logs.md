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
ms.openlocfilehash: 9d2a20ce681ea7e7c4ff2f9b492653e9d9a57b2b
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39248163"
---
# <a name="collect-and-consume-log-data-from-your-azure-resources"></a>Verzamelen en gebruiken van logboekgegevens van uw Azure-resources

## <a name="what-are-azure-monitor-diagnostic-logs"></a>Wat zijn diagnostische logboeken van Azure Monitor

**Diagnostische logboeken in Azure Monitor** zijn logboeken op een Azure-service met uitgebreide, regelmatig gegevens over de werking van die service. Met Azure Monitor worden de beschikbare twee typen logboeken met diagnostische gegevens:
* **Logboeken voor de tenantsleutel** -deze logboeken afkomstig zijn van het tenantniveau-services die buiten een Azure-abonnement bestaan, zoals Azure Active Directory-Logboeken.
* **Logboeken van de resource** -deze logboeken afkomstig zijn van de Azure-services die resources binnen een Azure-abonnement, zoals Network Security Groups of Storage-Accounts implementeren.

    ![Resource diagnostische logboeken en andere typen logboeken ](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_vs_other_logs_v5.png)

De inhoud van deze logboeken is afhankelijk van het Azure-service en resource-type. Tellers van regels voor Network Security Group en Key Vault controles zijn bijvoorbeeld twee typen logboeken met diagnostische gegevens.

Deze logboeken afwijken van de [activiteitenlogboek](monitoring-overview-activity-logs.md). Het activiteitenlogboek biedt inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement met behulp van Resource Manager, bijvoorbeeld, het maken van een virtuele machine of een logische app verwijderen. Het activiteitenlogboek is een logboek op abonnementsniveau. Resourceniveau diagnoselogboeken bieden inzicht in bewerkingen die zijn uitgevoerd binnen die resource zelf, bijvoorbeeld een geheim ophalen uit een Key Vault.

Deze logboeken afwijken van de Gast OS-niveau van diagnostische logboeken. Diagnostische logboeken van Guest OS zijn deze die worden verzameld door een agent die wordt uitgevoerd op een virtuele machine of andere ondersteund resourcetype. Diagnostische logboeken resourceniveau vereisen geen gegevens van de resource-specifieke agent en vastleggen van de Azure-platform zelf wordt geboden, terwijl Gast OS-niveau logboeken met diagnostische gegevens vastleggen van gegevens uit het besturingssysteem en toepassingen die worden uitgevoerd op een virtuele machine.

Niet alle services die ondersteuning voor de diagnostische logboeken die hier worden beschreven. [In dit artikel bevat een sectie aanbieding welke services diagnostische logboeken ondersteunen](./monitoring-diagnostic-logs-schema.md).

## <a name="what-you-can-do-with-diagnostic-logs"></a>U kunt doen met diagnostische logboeken
Hier volgen enkele dingen die u met Logboeken met diagnostische gegevens doen kunt:

![Logische plaatsing van diagnostische logboeken](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_Actions.png)

* Opslaan naar een [ **Opslagaccount** ](monitoring-archive-diagnostic-logs.md) voor controle of handmatige controle. U kunt opgeven de bewaartermijn (in dagen) via **instellingen voor resourcediagnose**.
* [Stream ze **Event Hubs** ](monitoring-stream-diagnostic-logs-to-event-hubs.md) voor opname van een service van derden of aangepaste analyseoplossing zoals Power BI.
* Analyseren met [Log Analytics](../log-analytics/log-analytics-azure-storage.md)

U kunt een opslagaccount of Event Hubs-naamruimte die zich niet in hetzelfde abonnement bevinden als het abonnement dat Logboeken verzendt. De gebruiker die de instelling configureert, moet de juiste RBAC-toegang tot beide abonnementen hebben.

> [!NOTE]
>  U kan momenteel niet archiveren gegevens aan een storage-account die achter een beveiligd virtueel netwerk.

> [!WARNING]
> De indeling van de logboekgegevens in het opslagaccount wordt op 1 november 2018 gewijzigd in JSON Lines. [Raadpleeg dit artikel voor een beschrijving van de gevolgen en hoe u uw tooling kunt bijwerken om de nieuwe indeling te verwerken. ](./monitor-diagnostic-logs-append-blobs.md) 
>
> 

## <a name="diagnostic-settings"></a>Diagnostische instellingen

Diagnostische logboeken van resources zijn geconfigureerd met behulp van de instellingen voor resourcediagnose. Diagnostische logboeken tenant zijn geconfigureerd met behulp van de diagnostische instelling van een tenant. **Diagnostische instellingen** voor een service-besturingselement:

* Waar diagnostische logboeken en metrische gegevens worden verzonden (Storage-Account, Event Hubs en/of Log Analytics).
* Welke logboekcategorieën worden verzonden en of u metrische gegevens ook worden verzonden.
* Hoe lang elke logboekcategorie moet worden bewaard in een storage-account
    - Een bewaarperiode van nul dagen betekent dat Logboeken altijd worden bewaard. De waarde kan anders een willekeurig aantal dagen tussen 1 en 2147483647 zijn.
    - Als Logboeken opslaan in een Storage-Account is uitgeschakeld (bijvoorbeeld, als er alleen Event Hubs of Log Analytics-opties zijn geselecteerd), bewaarbeleid worden ingesteld, maar hebben het bewaarbeleid geen effect.
    - Bewaarbeleid zijn toegepast per dag, dus aan het einde van een dag (UTC), logboeken van de dag dat nu is buiten de bewaarperiode van beleid worden verwijderd. Bijvoorbeeld, als u een beleid voor het bewaren van één dag had, worden aan het begin van de dag vandaag nog de logboeken van de dag voor gisteren vernietigd. De verwijderbewerking begint bij middernacht UTC, maar houd er rekening mee dat het kan tot 24 uur duren voor de logboeken worden verwijderd uit uw storage-account.

Deze instellingen gemakkelijk worden geconfigureerd via de diagnostische instellingen in de portal, via Azure PowerShell en CLI-opdrachten of via de [Azure Monitor REST API](https://docs.microsoft.com/rest/api/monitor/).

> [!NOTE]
> Het verzenden van multidimensionale metrische gegevens via diagnostische instellingen wordt momenteel niet ondersteund. Metrische gegevens met dimensies worden geëxporteerd als platte eendimensionale metrische gegevens, als totaal van alle dimensiewaarden.
>
> *Een voorbeeld*: de meetwaarde 'Binnenkomende berichten' voor een Event Hub kan worden verkend en uitgezet op wachtrijniveau. Wanneer de waarde wordt geëxporteerd via diagnostische instellingen, wordt deze echter voorgesteld als alle binnenkomende berichten voor alle wachtrijen in de Event Hub.
>
>

## <a name="how-to-enable-collection-of-diagnostic-logs"></a>Het inschakelen van verzamelen van diagnostische logboeken

Verzamelen van diagnostische logboeken kan worden ingeschakeld [als onderdeel van een resource maken in Resource Manager-sjabloon](./monitoring-enable-diagnostic-logs-using-template.md) of wanneer een resource is gemaakt op de pagina van de resource in de portal. U kunt ook de verzameling op elk gewenst moment met behulp van Azure PowerShell of CLI-opdrachten, of de REST-API van Azure Monitor inschakelen.

> [!TIP]
> Deze instructies mogelijk niet van toepassing rechtstreeks aan elke resource. Zie de koppelingen schema onderaan deze pagina om te begrijpen speciale stappen die van toepassing op bepaalde resourcetypen zijn.

### <a name="enable-collection-of-diagnostic-logs-in-the-portal"></a>Verzamelen van logboeken met diagnostische gegevens in de portal inschakelen

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

Diagnostische instellingen voor tenant kunnen alleen worden geconfigureerd in de portalblade voor de tenant-service - deze instellingen worden niet weergegeven in de blade van de diagnostische instellingen Azure Monitor. Bijvoorbeeld: auditlogboeken van Azure Active Directory zijn geconfigureerd door te klikken op de **instellingen exporteren van gegevens** in de blade van de logboeken controleren.

![Diagnostische instellingen van AAD](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-aad.png)

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

U configureren momenteel diagnostische instellingen van tenant met behulp van Azure PowerShell niet.

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

U configureren momenteel diagnostische instellingen van tenant met behulp van de CLI niet.

### <a name="enable-collection-of-resource-diagnostic-logs-via-rest-api"></a>Inschakelen van verzamelen van diagnostische logboeken van de resource via REST-API

Diagnostische instellingen met de Azure Monitor REST API, Zie [dit document](https://docs.microsoft.com/rest/api/monitor/).

U configureren diagnostische instellingen van tenant met behulp van de Azure Monitor REST API op dit moment niet.

## <a name="manage-resource-diagnostic-settings-in-the-portal"></a>Diagnostische instellingen van de resource in de portal beheren

Zorg ervoor dat al uw resources met de diagnostische instellingen zijn ingesteld. Navigeer naar **Monitor** in de portal en open **diagnostische instellingen**.

![Blade met diagnostische logboeken in de portal](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-nav.png)

Mogelijk moet u klikt u op 'Alle services' op zoek naar de sectie Monitor.

Hier kunt u weergeven en filteren van alle resources die ondersteuning bieden voor diagnostische instellingen om te zien als ze diagnostische gegevens zijn ingeschakeld. U kunt ook inzoomen op Zie als meerdere instellingen zijn ingesteld op een resource en welke storage-account, Event Hubs-naamruimte en/of Log Analytics-werkruimte die gegevens om te stromen controleren.

![Diagnostische logboeken resultaten in de portal](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

Een diagnostische instelling toe te voegen, wordt de diagnostische instellingen weergeven, waar u kunt inschakelen, uitschakelen of wijzigen van de diagnostische instellingen voor de geselecteerde resource.

## <a name="supported-services-categories-and-schemas-for-diagnostic-logs"></a>Ondersteunde services, categorieën en schema's voor diagnostische logboeken

[Raadpleeg dit artikel](monitoring-diagnostic-logs-schema.md) voor een volledige lijst van ondersteunde services en de logboekcategorieën en schema's die worden gebruikt door deze services.

## <a name="next-steps"></a>Volgende stappen

* [Diagnostische logboeken van de resource naar Stream **Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Diagnostische instellingen voor resources met behulp van de REST-API van Azure Monitor wijzigt](https://msdn.microsoft.com/library/azure/dn931931.aspx)
* [Logboeken van Azure storage met Log Analytics analyseren](../log-analytics/log-analytics-azure-storage.md)

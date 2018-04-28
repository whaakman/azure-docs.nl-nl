---
title: Overzicht van Azure diagnostische logboeken | Microsoft Docs
description: Informatie over wat Azure diagnostische logboeken zijn en hoe u ze kunt gebruiken om te begrijpen van gebeurtenissen die plaatsvinden binnen een Azure-resource.
author: johnkemnetz
manager: orenr
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: fe8887df-b0e6-46f8-b2c0-11994d28e44f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2018
ms.author: johnkem; magoedte
ms.openlocfilehash: 00b990181e5bb96f122ff1ae530dd027a772b210
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="collect-and-consume-log-data-from-your-azure-resources"></a>Verzamelen en gebruiken van de logboekgegevens van uw Azure-resources

## <a name="what-are-azure-resource-diagnostic-logs"></a>Wat zijn Azure-resource diagnostische logboeken

**Diagnostische logboeken van Azure resource niveau** logboeken die door een resource die uitgebreide, regelmatig gegevens over de werking van die bron leveren. De inhoud van deze logboeken varieert per resourcetype. Netwerkbeveiligingsgroep regel tellers en Sleutelkluis audits zijn bijvoorbeeld twee categorieën van Logboeken van de resource.

Diagnostische logboeken niveau resource afwijken van de [activiteitenlogboek](monitoring-overview-activity-logs.md). Het activiteitenlogboek verschaft inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement met behulp van Resource Manager, bijvoorbeeld een virtuele machine maken of verwijderen van een logische app. Het activiteitenlogboek is een logboek abonnement. Niveau van de resource diagnostische logboeken bieden inzicht in bewerkingen die zijn uitgevoerd binnen die bron zelf, bijvoorbeeld een geheim ophalen uit een Sleutelkluis.

Diagnostische logboeken niveau resource afwijken van de Gast OS-niveau diagnostische logboeken. Gastbesturingssysteem diagnostische logboeken zijn deze die worden verzameld door een agent wordt uitgevoerd binnen een virtuele machine of andere ondersteund resourcetype. Diagnostische logboeken niveau resource vereisen geen gegevens van de resource-specifieke agent en vastleggen van de Azure-platform zelf, terwijl de Gast OS-niveau diagnostische logboeken vastleggen van gegevens van het besturingssysteem en toepassingen die worden uitgevoerd op een virtuele machine.

Niet alle resources ondersteuning voor het nieuwe type resource diagnostische logboeken die hier wordt beschreven. In dit artikel bevat een sectie vermelden welke resourcetypen ondersteuning voor de nieuwe resource niveau diagnostische logboeken.

![Resource diagnostische logboeken tegenover andere typen logboeken ](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_vs_other_logs_v5.png)

## <a name="what-you-can-do-with-resource-level-diagnostic-logs"></a>Wat u kunt doen met het niveau van de resource diagnostische logboeken
Hier volgen enkele dingen die u met resource diagnostische logboeken doen kunt:

![Logische plaatsing van diagnostische logboeken van Resource](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_Actions.png)

* Bewaar ze op een [ **Opslagaccount** ](monitoring-archive-diagnostic-logs.md) voor inspectie controle of handmatig. Kunt u de bewaarperiode (in dagen) met behulp van **diagnostische broninstellingen**.
* [Ze streamt **Event Hubs** ](monitoring-stream-diagnostic-logs-to-event-hubs.md) voor opname door een service van derden of aangepaste analytics-oplossing zoals Power BI.
* Analyseer ze met [Log Analytics](../log-analytics/log-analytics-azure-storage.md)

U kunt een opslagaccount of de Event Hubs-naamruimte die zich niet in hetzelfde abonnement als de tekensetcodering Logboeken gebruiken. De gebruiker die de instelling configureert, moet de juiste RBAC-toegang tot beide abonnementen hebben.

## <a name="resource-diagnostic-settings"></a>Diagnostische instellingen voor bronnen

Diagnostische logboeken van de resource voor niet-Compute bronnen worden geconfigureerd met behulp van de diagnostische instellingen resource. **Diagnostische instellingen voor resource** voor een besturingselement resource:

* Waar resource diagnostische logboeken en metrische gegevens worden verzonden (Storage-Account, Event Hubs en/of Log Analytics).
* Welke categorieën logboek worden verzonden en of u metrische gegevens ook verzonden.
* Hoe lang elke categorie van het logboek moet worden bewaard in een opslagaccount
    - Een bewaartermijn van nul dagen betekent logboeken permanent worden bewaard. Anders wordt mag de waarde een onbeperkt aantal dagen tussen 1 en 2147483647.
    - Als bewaarbeleid worden ingesteld, maar Logboeken opslaan in een Opslagaccount is uitgeschakeld (bijvoorbeeld, als er alleen Event Hubs of Log Analytics-opties zijn geselecteerd), is het bewaarbeleid hebben geen effect.
    - Bewaarbeleid zijn toegepaste per dag, dus aan het einde van een dag (UTC), logboeken van de dag dat nu is buiten de bewaarperiode beleid worden verwijderd. Bijvoorbeeld, als u had een bewaarbeleid van één dag, zou aan het begin van vandaag de dag de logboeken van de dag voordat gisteren worden verwijderd.

Deze instellingen gemakkelijk worden geconfigureerd via de diagnostische instellingen voor een resource in de Azure portal, via Azure PowerShell en CLI-opdrachten of via de [REST-API van Azure-Monitor](https://msdn.microsoft.com/library/azure/dn931943.aspx).

> [!NOTE]
> Het verzenden van multidimensionale metrische gegevens via diagnostische instellingen wordt momenteel niet ondersteund. Metrische gegevens met dimensies worden geëxporteerd als platte eendimensionale metrische gegevens, als totaal van alle dimensiewaarden.
>
> *Een voorbeeld*: de metriek 'Binnenkomende berichten' voor een Event Hub kan worden verkend en uitgezet op wachtrijniveau. Maar wanneer de metriek wordt geëxporteerd via diagnostische instellingen, geeft de metriek alle binnenkomende berichten in alle wachtrijen in de Event Hub aan.
>
>

> [!WARNING]
> Diagnostische logboeken en metrische gegevens van de Gast OS-laag van Compute-resources (bijvoorbeeld virtuele machines of Service Fabric) Gebruik [een afzonderlijk mechanisme voor configuratie en de selectie van uitvoer](../azure-diagnostics.md).

## <a name="how-to-enable-collection-of-resource-diagnostic-logs"></a>Het inschakelen van verzamelen van diagnostische logboeken van resource

Verzamelen van diagnostische logboeken resource kan worden ingeschakeld [als onderdeel van het maken van een resource in het Resource Manager-sjabloon](./monitoring-enable-diagnostic-logs-using-template.md) of nadat een bron wordt gemaakt van de pagina die resource in de portal. U kunt ook de verzameling op elk gewenst moment met behulp van Azure PowerShell of CLI-opdrachten of met behulp van de Monitor REST-API van Azure inschakelen.

> [!TIP]
> Deze instructies kunnen niet rechtstreeks toepassen op elke resource. Zie de schema-koppelingen onder aan deze pagina om te begrijpen speciale stappen die van toepassing zijn op bepaalde resourcetypen.

### <a name="enable-collection-of-resource-diagnostic-logs-in-the-portal"></a>Verzamelen van diagnostische logboeken van de resource in de portal

U kunt verzamelen van diagnostische logboeken van de resource in de Azure portal inschakelen nadat een resource is gemaakt door te gaan op een specifieke bron of door te navigeren naar de Azure-Monitor. Dit via Azure Monitor inschakelen:

1. In de [Azure-portal](http://portal.azure.com), navigeer naar de Azure-Monitor en klikt u op **diagnostische instellingen**

    ![Sectie van de Monitor Azure bewaking](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

2. Optioneel de lijst filteren op resourcegroep of brontype en klik vervolgens op de bron die u wilt geen diagnostische instellen.

3. Als er geen instellingen bestaan op de bron voor hebt u geselecteerd, u wordt gevraagd om een instelling te maken. Klik op "Diagnostische gegevens inschakelen."

   ![Diagnostische instelling - geen bestaande instellingen toevoegen](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-none.png)

   Als er bestaande instellingen op de bron, ziet u een lijst met instellingen die al zijn geconfigureerd op deze resource. Klik op 'Diagnostische instelling toevoegen'.

   ![Diagnostische instelling - bestaande instellingen toevoegen](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-multiple.png)

3. Geef een naam van de instelling, schakel de selectievakjes voor elk doel waarnaar u wilt gegevens verzenden, en welke resource wordt gebruikt voor elk doel configureren. Eventueel, stel een aantal dagen wilt bewaren van deze logboeken met behulp van de **bewaartermijn (dagen)** schuifregelaars (alleen van toepassing op de opslaglocatie voor de account). Een bewaartermijn van nul dagen worden de logboeken voor onbepaalde tijd opgeslagen.

   ![Diagnostische instelling - bestaande instellingen toevoegen](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-configure.png)

4. Klik op **Opslaan**.

De nieuwe instelling wordt weergegeven in de lijst met instellingen voor deze bron na enkele ogenblikken en logboeken met diagnostische gegevens worden verzonden naar de opgegeven bestemmingen zodra er nieuwe gebeurtenisgegevens wordt gegenereerd.

### <a name="enable-collection-of-resource-diagnostic-logs-via-powershell"></a>Verzamelen van diagnostische logboeken van de resource via PowerShell

Als u wilt verzamelen van diagnostische logboeken van de resource via Azure PowerShell, gebruik de volgende opdrachten:

Om opslag van diagnostische logboeken in een opslagaccount, gebruikt u deze opdracht:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
```

De storage-account-ID is de resource-ID voor het opslagaccount waarnaar u wilt de logboeken verzenden.

Om streaming van logboeken met diagnostische gegevens naar een event hub, gebruikt u deze opdracht:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your Service Bus rule id] -Enabled $true
```

De regel-ID van service bus is een tekenreeks met deze indeling: `{Service Bus resource ID}/authorizationrules/{key name}`.

Gebruik deze opdracht zodat verzending van diagnostische logboeken naar een Log Analytics-werkruimte:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
```

Kunt u de bron-ID van de werkruimte voor logboekanalyse met de volgende opdracht:

```powershell
(Get-AzureRmOperationalInsightsWorkspace).ResourceId
```

U kunt deze parameters zodat meerdere uitvoeropties combineren.

### <a name="enable-collection-of-resource-diagnostic-logs-via-azure-cli-20"></a>Verzamelen van diagnostische logboeken van de resource via Azure CLI 2.0

Als u wilt verzamelen van diagnostische logboeken van de bron via de Azure CLI 2.0, die u gebruikt de [az monitor diagnose-instellingen maken](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) opdracht.

Opslag van diagnostische logboeken in een Opslagaccount inschakelen:

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

De `--resource-group` -argument is alleen vereist als `--storage-account` is niet een object-ID.

Inschakelen streaming van logboeken met diagnostische gegevens naar een event hub:

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

De regel-ID is een tekenreeks met deze indeling: `{Service Bus resource ID}/authorizationrules/{key name}`.

Inschakelen van verzending van diagnostische logboeken naar een Log Analytics-werkruimte:

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

De `--resource-group` -argument is alleen vereist als `--workspace` is niet de object-ID

Met een opdracht kunt u extra categorieën toevoegen aan de diagnostische logboeken door woordenlijsten toe te voegen aan de JSON-matrix doorgegeven als de `--logs` parameter. U kunt combineren de `--storage-account`, `--event-hub`, en `--workspace` parameters voor het inschakelen van meerdere opties voor uitvoer.

### <a name="enable-collection-of-resource-diagnostic-logs-via-rest-api"></a>Verzamelen van diagnostische logboeken van de resource via REST-API

Zie het wijzigen van diagnostische instellingen met de REST-API van Azure Monitor [dit document](https://msdn.microsoft.com/library/azure/dn931931.aspx).

## <a name="manage-resource-diagnostic-settings-in-the-portal"></a>Diagnostische instellingen van de resource in de portal beheren

Zorg ervoor dat alle resources met diagnostische instellingen zijn ingesteld. Navigeer naar **Monitor** in de portal en open **diagnostische instellingen**.

![Diagnostische logboeken blade in de portal](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-nav.png)

Wellicht hebt u klikken op 'Alle services' vinden van de Monitor-sectie.

Hier kunt u bekijken en alle resources die ondersteuning bieden voor diagnostische instellingen om te zien als ze ingeschakeld diagnostische gegevens hebben filteren. U kunt ook inzoomen om te controleren als er meerdere instellingen zijn ingesteld op een resource en welke storage-account, de Event Hubs-naamruimte en/of de werkruimte voor logboekanalyse die gegevens om te stromen controleren.

![Diagnostische logboeken resultaten in de portal](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

Een diagnostische instelling toe te voegen, wordt de weergave van de diagnostische instellingen, waar u kunt inschakelen, uitschakelen of wijzigen van de diagnostische instellingen voor de geselecteerde resource.

## <a name="supported-services-categories-and-schemas-for-resource-diagnostic-logs"></a>Ondersteunde services, categorieën en schema's voor resource diagnostische logboeken

[Raadpleeg dit artikel](monitoring-diagnostic-logs-schema.md) voor een volledige lijst met ondersteunde services en de logboek-categorieën en schema's die worden gebruikt door deze services.

## <a name="next-steps"></a>Volgende stappen

* [Diagnostische logboeken van de resource te streamen **Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Diagnostische instellingen voor bronnen met de REST-API van Azure Monitor wijzigen](https://msdn.microsoft.com/library/azure/dn931931.aspx)
* [Logboeken van de Azure-opslag met logboekanalyse analyseren](../log-analytics/log-analytics-azure-storage.md)

---
title: Azure CLI-1.0 Monitor snel starten-voorbeelden. | Microsoft Docs
description: CLI-1.0 Voorbeeldopdrachten voor Monitor van de Azure-functies. Monitor voor Azure is een Microsoft Azure-service zodat u kunt het verzenden van meldingen van waarschuwingen, aanroepen van web-URL's op basis van waarden van de geconfigureerde telemetriegegevens, en voor automatisch schalen Cloudservices, virtuele Machines en Web-Apps.
author: kamathashwin
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1653aa81-0ee6-4622-9c65-d4801ed9006f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: ashwink
ms.openlocfilehash: ec4512500dc3c77a40d2ebd1e6b460d5bb005811
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-monitor--cross-platform-cli-10-quick-start-samples"></a>Azure Monitor platformoverschrijdende CLI 1.0 snel starten-voorbeelden
Dit artikel ziet u voorbeelden opdrachtregelinterface (CLI)-opdrachten kunt u toegang tot Azure Monitor functies. Monitor voor Azure kunt u voor automatisch schalen Cloud Services, virtuele Machines en Web-Apps en voor het verzenden van meldingen van waarschuwingen of web-URL's op basis van waarden van de geconfigureerde telemetriegegevens aanroepen.

> [!NOTE]
> Monitor voor Azure is de nieuwe naam voor wat 'Azure Insights' is aangeroepen tot 25 september 2016. Echter bevatten de naamruimten en dus de onderstaande opdrachten nog steeds de 'inzichten'.
> 
> 

## <a name="prerequisites"></a>Vereisten
Als u nog niet de Azure CLI hebt geïnstalleerd, raadpleegt u [Azure CLI installeren](../cli-install-nodejs.md). Als u niet bekend met Azure CLI bent, vindt u meer over op het [gebruik van de Azure CLI voor Mac, Linux en Windows Azure Resource Manager](../xplat-cli-azure-resource-manager.md).

In Windows, installeert u npm uit de [Node.js website](https://nodejs.org/). Nadat u de installatie voltooid met behulp van CMD.exe met bevoegdheden als Administrator uitvoeren, voert u het volgende vanuit de map waarin de npm is geïnstalleerd:

```console
npm install azure-cli --global
```

Vervolgens gaat u naar een map of de locatie u wilt en typt u bij de opdrachtregel:

```console
azure help
```

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.
De eerste stap is het aanmelden bij uw Azure-account.

```console
azure login
```

Wanneer u deze opdracht uitvoert, beschikt u over aanmelden via de instructies op het scherm. Hierna ziet u uw Account, TenantId en standaard abonnements-id. Alle opdrachten werkt in de context van uw standaardabonnement.

U kunt de details van uw huidige abonnement gebruiken de volgende opdracht.

```console
azure account show
```

Gebruik de volgende opdracht om te werken context wijzigen naar een ander abonnement.

```console
azure account set "subscription ID or subscription name"
```

Azure Resource Manager en Azure-Monitor als opdrachten wilt gebruiken, moet u zich in de modus Azure Resource Manager.

```console
azure config mode arm
```

Een lijst met alle ondersteunde Azure-Monitor opdrachten wilt weergeven, door het volgende te doen.

```console
azure insights
```

## <a name="view-activity-log-for-a-subscription"></a>Activiteitenlogboek weergeven voor een abonnement
Een lijst van de activiteit logboekgebeurtenissen wilt weergeven, door het volgende te doen.

```console
azure insights logs list [options]
```

Probeer de volgende stappen om alle beschikbare opties weer te geven.

```console
azure insights logs list -help
```

Hier volgt een voorbeeld in de logboeken van de lijst door een resourceGroup

```console
azure insights logs list --resourceGroup "myrg1"
```

Voorbeeld van Logboeken van de lijst door de oproepende functie

```console
azure insights logs list --caller "myname@company.com"
```

Voorbeeld van Logboeken van de lijst door de oproepende functie op een resourcetype binnen een begin- en datum

```console
azure insights logs list --resourceProvider "Microsoft.Web" --caller "myname@company.com" --startTime 2016-03-08T00:00:00Z --endTime 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Werken met waarschuwingen
Gebruik de informatie in de sectie kunt u werken met waarschuwingen.

### <a name="get-alert-rules-in-a-resource-group"></a>Waarschuwingsregels in een resourcegroep ophalen
```console
azure insights alerts rule list abhingrgtest123
azure insights alerts rule list abhingrgtest123 --ruleName andy0323
```

### <a name="create-a-metric-alert-rule"></a>Een metriek waarschuwingsregel maken
```console
azure insights alerts actions email create --customEmails foo@microsoft.com
azure insights alerts actions webhook create https://someuri.com
azure insights alerts rule metric set andy0323 eastus abhingrgtest123 PT5M GreaterThan 2 /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Web-EastUS/providers/Microsoft.Web/serverfarms/Default1 BytesReceived Total
```

### <a name="create-webtest-alert-rule"></a>Waarschuwingsregel voor webtest maken
```console
azure insights alerts rule webtest set leowebtestr1-webtestr1 eastus Default-Web-WestUS PT5M 1 GSMT_AvRaw /subscriptions/b67f7fec-69fc-4974-9099-a26bd6ffeda3/resourcegroups/Default-Web-WestUS/providers/microsoft.insights/webtests/leowebtestr1-webtestr1
```

### <a name="delete-an-alert-rule"></a>Een waarschuwingsregel verwijderen
```console
azure insights alerts rule delete abhingrgtest123 andy0323
```

## <a name="log-profiles"></a>Logboek-profielen
Gebruik de informatie in deze sectie om te werken met profielen voor logboekbestanden.

### <a name="get-a-log-profile"></a>Een profiel voor een logboek ophalen
```console
azure insights logprofile list
azure insights logprofile get -n default
```


### <a name="add-a-log-profile-without-retention"></a>Een profiel logboek zonder bewaren toevoegen
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Een logboek-profiel verwijderen
```console
azure insights logprofile delete --name default
```

### <a name="add-a-log-profile-with-retention"></a>Een profiel van het logboek met bewaren toevoegen
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Een profiel van het logboek met bewaren en EventHub toevoegen
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --serviceBusRuleId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/testshoeboxeastus/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```


## <a name="diagnostics"></a>Diagnostiek
Gebruik de informatie in deze sectie om te werken met diagnostische instellingen.

### <a name="get-a-diagnostic-setting"></a>Een diagnostische instelling ophalen
```console
azure insights diagnostic get --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp
```

### <a name="disable-a-diagnostic-setting"></a>Een diagnostische instelling uitschakelen
```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled false
```

### <a name="enable-a-diagnostic-setting-without-retention"></a>Een diagnostische instelling zonder bewaren inschakelen
```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled true
```


## <a name="autoscale"></a>Automatisch schalen
Gebruik de informatie in deze sectie om te werken met instellingen voor automatisch schalen. U moet deze voorbeelden wijzigen.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Instellingen voor automatisch schalen voor een resourcegroep
```console
azure insights autoscale setting list montest2
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Instellingen voor automatisch schalen die u op naam in een resourcegroep
```console
azure insights autoscale setting list montest2 -n setting2
```


### <a name="set-auotoscale-settings"></a>Instellingen voor het auotoscale
```console
azure insights autoscale setting set montest2 -n setting2 --settingSpec
```

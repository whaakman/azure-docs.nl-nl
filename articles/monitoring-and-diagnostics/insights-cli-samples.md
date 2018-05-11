---
title: Azure CLI-2.0 Monitor snel starten-voorbeelden. | Microsoft Docs
description: CLI 2.0 Voorbeeldopdrachten voor Monitor van de Azure-functies. Monitor voor Azure is een Microsoft Azure-service zodat u kunt het verzenden van meldingen van waarschuwingen, aanroepen van web-URL's op basis van waarden van de geconfigureerde telemetriegegevens, en voor automatisch schalen Cloudservices, virtuele Machines en Web-Apps.
author: kamathashwin
manager: ''
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1653aa81-0ee6-4622-9c65-d4801ed9006f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: ashwink
ms.openlocfilehash: a9c6cc0fb81b094e1c980e4c209184a0c0ebd428
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="azure-monitor-cli-20-quick-start-samples"></a>Azure CLI-2.0 Monitor snel starten-voorbeelden
Dit artikel ziet u voorbeelden opdrachtregelinterface (CLI)-opdrachten kunt u toegang tot Azure Monitor functies. Monitor voor Azure kunt u voor automatisch schalen Cloud Services, virtuele Machines en Web-Apps en voor het verzenden van meldingen van waarschuwingen of web-URL's op basis van waarden van de geconfigureerde telemetriegegevens aanroepen.

## <a name="prerequisites"></a>Vereisten

Als u nog niet hebt geïnstalleerd op de Azure CLI, volg de instructies voor [2.0 Azure CLI installeren](/cli/azure/install-azure-cli). U kunt ook [Azure Cloud Shell](/azure/cloud-shell) uit te voeren van de CLI als een interactieve ervaring in uw browser. 

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.
De eerste stap is het aanmelden bij uw Azure-account.

```azurecli
az login
```

Wanneer u deze opdracht uitvoert, beschikt u over aanmelden via de instructies op het scherm. Alle opdrachten werkt in de context van uw standaardabonnement.

U kunt de details van uw huidige abonnement gebruiken de volgende opdracht.

```azurecli
az account show
```

Gebruik de volgende opdracht om te werken context wijzigen naar een ander abonnement.

```azurecli
az account set -s <Subscription ID or name>
```

Een lijst met alle ondersteunde Azure-Monitor opdrachten wilt weergeven, door het volgende te doen.

```azurecli
az monitor -h
```

## <a name="view-activity-log-for-a-subscription"></a>Activiteitenlogboek weergeven voor een abonnement

Een lijst van de activiteit logboekgebeurtenissen wilt weergeven, door het volgende te doen.

```azurecli
az monitor activity-log list
```

Probeer de volgende stappen om alle beschikbare opties weer te geven.

```azurecli
az monitor activity-log list -h
```

Hier volgt een voorbeeld in de logboeken van de lijst door een resourceGroup

```azurecli
az monitor activity-log list --resource-group <group name>
```

Voorbeeld van Logboeken van de lijst door de oproepende functie

```azurecli
az monitor activity-log list --caller myname@company.com
```

Voorbeeld van Logboeken van de lijst door de oproepende functie op een resourcetype binnen een bepaald datumbereik

```azurecli
az monitor activity-log list --resource-provider Microsoft.Web \
    --caller myname@company.com \
    --start-time 2016-03-08T00:00:00Z \
    --end-time 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Werken met waarschuwingen 
[!NOTE] Alleen waarschuwingen (klassiek) wordt ondersteund in de CLI op dit moment. 

### <a name="get-alert-classic-rules-in-a-resource-group"></a>Waarschuwingsregels (klassiek) in een resourcegroep ophalen

```azurecli
az monitor activity-log alert list --resource-group <group name>
az monitor activity-log alert show --resource-group <group name> --name <alert name>
```

### <a name="create-a-metric-alert-classic-rule"></a>Een metriek (klassiek) waarschuwingsregel maken

```azurecli
az monitor alert create --name <alert name> --resource-group <group name> \
    --action email <email1 email2 ...> \
    --action webhook <URI> \
    --target <target object ID> \
    --condition "<METRIC> {>,>=,<,<=} <THRESHOLD> {avg,min,max,total,last} ##h##m##s"
```

### <a name="delete-an-alert-classic-rule"></a>Verwijderen van een waarschuwingsregel (klassiek)

```azurecli
az monitor alert delete --name <alert name> --resource-group <group name>
```

## <a name="log-profiles"></a>Logboek-profielen

Gebruik de informatie in deze sectie om te werken met profielen voor logboekbestanden.

### <a name="get-a-log-profile"></a>Een profiel voor een logboek ophalen

```azurecli
az monitor log-profiles list
az monitor log-profiles show --name <profile name>
```

### <a name="add-a-log-profile-with-retention"></a>Een profiel van het logboek met bewaren toevoegen

```azurecli
az monitor log-profiles create --name <profile name> --location <location of profile> \
    --locations <locations to monitor activity in: location1 location2 ...> \
    --categories <categoryName1 categoryName2 ...> \
    --days <# days to retain> \
    --enabled true \
    --storage-account-id <storage account ID to store the logs in>
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Een profiel van het logboek met bewaren en EventHub toevoegen

```azurecli
az monitor log-profiles create --name <profile name> --location <location of profile> \
    --locations <locations to monitor activity in: location1 location2 ...> \
    --categories <categoryName1 categoryName2 ...> \
    --days <# days to retain> \
    --enabled true
    --storage-account-id <storage account ID to store the logs in>
    --service-bus-rule-id <service bus rule ID to stream to>
```

### <a name="remove-a-log-profile"></a>Een logboek-profiel verwijderen

```azurecli
az monitor log-profiles delete --name <profile name>
```

## <a name="diagnostics"></a>Diagnostiek

Gebruik de informatie in deze sectie om te werken met diagnostische instellingen.

### <a name="get-a-diagnostic-setting"></a>Een diagnostische instelling ophalen

```azurecli
az monitor diagnostic-settings list --resource <target resource ID>
```

### <a name="create-a-diagnostic-log-setting"></a>Een instelling diagnostische logboeken maken 

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --storage-account <storage account ID> \
    --resource <target resource object ID> \
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

### <a name="delete-a-diagnostic-setting"></a>Een diagnostische instelling niet verwijderen

```azurecli
az monitor diagnostic-settings delete --name <diagnostic name> \
    --resource <target resource ID>
```

## <a name="autoscale"></a>Automatisch schalen

Gebruik de informatie in deze sectie om te werken met instellingen voor automatisch schalen. U moet deze voorbeelden wijzigen.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Instellingen voor automatisch schalen voor een resourcegroep

```azurecli
az monitor autoscale list --resource-group <group name>
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Instellingen voor automatisch schalen die u op naam in een resourcegroep

```azurecli
az monitor autoscale show --name <settings name> --resource-group <group name>
```

### <a name="set-autoscale-settings"></a>Instellingen voor automatisch schalen

```azurecli
az monitor autoscale create --name <settings name> --resource-group <group name> \
    --count <# instances> \
    --resource <target resource ID>
```

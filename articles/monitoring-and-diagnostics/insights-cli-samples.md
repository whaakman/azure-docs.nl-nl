---
title: Voorbeelden om snel aan de slag met Azure Monitor CLI
description: Voorbeeld-CLI 2.0-opdrachten voor Azure Monitor-functies. Azure Monitor is een Microsoft Azure-service zodat u kunt het verzenden van meldingen van waarschuwingen, aanroepen van web-URL's op basis van waarden van geconfigureerde telemetriegegevens, en voor automatisch schalen, Cloudservices, virtuele Machines en Web-Apps.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: robb
ms.component: ''
ms.openlocfilehash: dfc9a65325d25889c9d8d3c10aa0dcda2b5a9980
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213204"
---
# <a name="azure-monitor-cli-20-quick-start-samples"></a>Voorbeelden om snel aan de slag met Azure Monitor CLI 2.0
Dit artikel laat u voorbeelden opdrachtregelinterface (CLI)-opdrachten kunt u toegang tot de functies van Azure Monitor. Azure Monitor kunt u om de Cloudservices voor automatisch schalen, virtuele Machines en Web-Apps te verzenden van meldingen van waarschuwingen of aanroepen van web-URL's op basis van waarden van de geconfigureerde telemetrische gegevens.

## <a name="prerequisites"></a>Vereisten

Als u de Azure CLI nog niet hebt geïnstalleerd, volgt u de instructies voor het [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). U kunt ook [Azure Cloud Shell](/azure/cloud-shell) uit te voeren van de CLI als een interactieve ervaring opdoen in uw browser. Zie een volledige verwijzing van alle beschikbare opdrachten in de [naslaginformatie over de CLI van Azure Monitor](https://docs.microsoft.com/cli/azure/monitor?view=azure-cli-latest). 

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.
De eerste stap is het aanmelden bij uw Azure-account.

```azurecli
az login
```

Na deze opdracht uitvoert, moet u zich aanmelden via de instructies op het scherm. Alle opdrachten werkt in de context van uw standaardabonnement.

Als u de details van uw huidige abonnement, de volgende opdracht te gebruiken.

```azurecli
az account show
```

Als u wilt werken context wijzigen naar een ander abonnement, gebruik de volgende opdracht.

```azurecli
az account set -s <Subscription ID or name>
```

Een lijst met alle ondersteunde Azure Monitor-opdrachten wilt weergeven, door het volgende te doen.

```azurecli
az monitor -h
```

## <a name="view-activity-log-for-a-subscription"></a>Het activiteitenlogboek weergeven voor een abonnement

Een lijst van gebeurtenissen in het activiteitenlogboek wilt weergeven, door het volgende te doen.

```azurecli
az monitor activity-log list
```

Probeer de volgende stappen om alle beschikbare opties weer te geven.

```azurecli
az monitor activity-log list -h
```

Hier volgt een voorbeeld in lijst Logboeken door een resourceGroup

```azurecli
az monitor activity-log list --resource-group <group name>
```

Voorbeeld van de logboeken van de lijst met door de oproepende functie

```azurecli
az monitor activity-log list --caller myname@company.com
```

Voorbeeld van de logboeken van de lijst met door de oproepende functie op een resourcetype binnen een bepaalde periode

```azurecli
az monitor activity-log list --resource-provider Microsoft.Web \
    --caller myname@company.com \
    --start-time 2016-03-08T00:00:00Z \
    --end-time 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Werken met waarschuwingen 
> [!NOTE]
> Alleen waarschuwingen (klassiek) wordt ondersteund in CLI op dit moment. 

### <a name="get-alert-classic-rules-in-a-resource-group"></a>Regels voor waarschuwingen (klassiek) in een resourcegroep ophalen

```azurecli
az monitor activity-log alert list --resource-group <group name>
az monitor activity-log alert show --resource-group <group name> --name <alert name>
```

### <a name="create-a-metric-alert-classic-rule"></a>Maak een regel voor metrische gegevens waarschuwingen (klassiek)

```azurecli
az monitor alert create --name <alert name> --resource-group <group name> \
    --action email <email1 email2 ...> \
    --action webhook <URI> \
    --target <target object ID> \
    --condition "<METRIC> {>,>=,<,<=} <THRESHOLD> {avg,min,max,total,last} ##h##m##s"
```

### <a name="delete-an-alert-classic-rule"></a>Verwijderen van een regel voor waarschuwingen (klassiek)

```azurecli
az monitor alert delete --name <alert name> --resource-group <group name>
```

## <a name="log-profiles"></a>Logboekprofielen

Gebruik de informatie in deze sectie om te werken met logboekprofielen.

### <a name="get-a-log-profile"></a>Ophalen van een logboekprofiel

```azurecli
az monitor log-profiles list
az monitor log-profiles show --name <profile name>
```

### <a name="add-a-log-profile-with-retention"></a>Een logboekprofiel met een bewaarperiode toevoegen

```azurecli
az monitor log-profiles create --name <profile name> --location <location of profile> \
    --locations <locations to monitor activity in: location1 location2 ...> \
    --categories <categoryName1 categoryName2 ...> \
    --days <# days to retain> \
    --enabled true \
    --storage-account-id <storage account ID to store the logs in>
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Een logboekprofiel met retentie en het Event hub toevoegen

```azurecli
az monitor log-profiles create --name <profile name> --location <location of profile> \
    --locations <locations to monitor activity in: location1 location2 ...> \
    --categories <categoryName1 categoryName2 ...> \
    --days <# days to retain> \
    --enabled true
    --storage-account-id <storage account ID to store the logs in>
    --service-bus-rule-id <service bus rule ID to stream to>
```

### <a name="remove-a-log-profile"></a>Een logboekprofiel verwijderen

```azurecli
az monitor log-profiles delete --name <profile name>
```

## <a name="diagnostics"></a>Diagnostiek

Gebruik de informatie in deze sectie om te werken met de diagnostische instellingen.

### <a name="get-a-diagnostic-setting"></a>Een diagnostische instelling ophalen

```azurecli
az monitor diagnostic-settings list --resource <target resource ID>
```

### <a name="create-a-diagnostic-log-setting"></a>Maken van een instelling voor diagnostische logboeken 

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

### <a name="delete-a-diagnostic-setting"></a>Een diagnostische instelling verwijderen

```azurecli
az monitor diagnostic-settings delete --name <diagnostic name> \
    --resource <target resource ID>
```

## <a name="autoscale"></a>Automatisch schalen

Gebruik de informatie in deze sectie om te werken met instellingen voor automatisch schalen. U moet deze voorbeelden te wijzigen.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Instellingen voor automatisch schalen voor een resourcegroep ophalen

```azurecli
az monitor autoscale list --resource-group <group name>
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Instellingen voor automatisch schalen met de naam in een resourcegroep ophalen

```azurecli
az monitor autoscale show --name <settings name> --resource-group <group name>
```

### <a name="set-autoscale-settings"></a>Instellingen voor automatisch schalen instellen

```azurecli
az monitor autoscale create --name <settings name> --resource-group <group name> \
    --count <# instances> \
    --resource <target resource ID>
```

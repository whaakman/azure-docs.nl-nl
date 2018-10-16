---
title: Starter query's van Azure Resource Graph
description: Gebruik Azure Resource Graph om een aantal starter query's uit te voeren.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: quickstart
ms.service: resource-graph
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: bcc6c6e9d38948cb4be09c9f52049790ccd5ac7e
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227024"
---
# <a name="starter-resource-graph-queries"></a>Starter query's van Resource Graph

Om inzicht te krijgen in query's met Azure Resource Graph moet u eerst enige basiskennis hebben van de [querytaal](../concepts/query-language.md). Als u nog geen ervaring hebt met [Azure Data Explorer](../../../data-explorer/data-explorer-overview.md), is het raadzaam om eerst de basisbeginselen door te nemen, zodat u weet hoe u aanvragen opstelt voor de resources die u zoekt.

We nemen de volgende starter query's door:

> [!div class="checklist"]
> - [Azure-resources tellen](#count-resources)
> - [Een lijst van resources weergeven, gesorteerd op naam](#list-resources)
> - [Alle virtuele machines weergeven, aflopend geordend op naam](#show-vms)
> - [De eerste vijf virtuele machines weergeven op naam en met hun type besturingssysteem](#show-sorted)
> - [Virtuele machines tellen op type besturingssysteem](#count-os)
> - [Resources weergeven die opslag bevatten](#show-storage)
> - [Een lijst van alle openbare IP-adressen weergeven](#list-publicip)
> - [Resources tellen met IP-adressen die zijn geconfigureerd op abonnement](#count-resources-by-ip)
> - [Een lijst weergeven van resources met een specifieke tagwaarde](#list-tag)
> - [Een lijst weergeven van alle opslagaccounts met een specifieke tagwaarde](#list-specific-tag)

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free) aan voordat u begint.

## <a name="language-support"></a>Taalondersteuning

Azure CLI (met een extensie) en Azure PowerShell (met een module) ondersteunen Azure Resource Graph. Controleer voordat u een van de volgende query's uitvoert of uw omgeving gereed is. Zie [Azure CLI](../first-query-azurecli.md#add-the-resource-graph-extension) en [Azure PowerShell](../first-query-powershell.md#add-the-resource-graph-module) voor stappen voor het installeren en valideren van uw gewenste shellomgeving.

## <a name="count-resources"></a>Azure-resources tellen

Deze query retourneert het aantal Azure-resources in de abonnementen waartoe u toegang hebt. Het is ook een goede query om te valideren of in uw gekozen shell de juiste Azure Resource Graph-onderdelen correct zijn geïnstalleerd.

```Query
summarize count()
```

```azurecli-interactive
az graph query -q "summarize count()"
```

```powershell
Search-AzureRmGraph -Query "summarize count()"
```

## <a name="list-resources"></a>Een lijst van resources weergeven, gesorteerd op naam

Zonder zich te beperken tot een bepaald type resource of een bepaalde eigenschap, retourneert deze query alleen de **naam**, het **type** en de **locatie** van de Azure-resources. `order by` wordt echter gebruikt om ze in oplopende (`asc`) volgorde te sorteren op de eigenschap **naam**.

```Query
project name, type, location
| order by name asc
```

```azurecli-interactive
az graph query -q "project name, type, location | order by name asc"
```

```powershell
Search-AzureRmGraph -Query "project name, type, location | order by name asc"
```

## <a name="show-vms"></a>Alle virtuele machines weergeven, aflopend geordend op naam

Als we alleen een lijst van virtuele machines willen (die het type `Microsoft.Compute/virtualMachines` hebben), kunnen we in plaats van alle Azure-resources op te vragen, de resultaten filteren op de eigenschap **type**.
Net als in de vorige query verandert u met `desc` de `order by` in aflopend. De `=~` in de type-overeenkomst betekent in Resource Graph dat de sortering hoofdlettergevoelig is.

```Query
project name, location, type
| where type =~ 'Microsoft.Compute/virtualMachines'
| order by name desc
```

```azurecli-interactive
az graph query -q "project name, location, type| where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

```powershell
Search-AzureRmGraph -Query "project name, location, type| where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

## <a name="show-sorted"></a>De eerste vijf virtuele machines weergeven op naam en met hun type besturingssysteem

Deze query gebruikt `limit` om slechts vijf overeenkomende records op te halen, gesorteerd op naam. Het type van de Azure-resource is `Microsoft.Compute/virtualMachines`. `project` geeft in Azure Resource Graph aan welke eigenschappen u wilt opnemen.

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| project name, properties.storageProfile.osDisk.osType
| top 5 by name desc
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

```powershell
Search-AzureRmGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

## <a name="count-os"></a>Virtuele machines tellen op type besturingssysteem

Voortbouwend op de vorige query, beperken we de Azure-resources nog steeds op het type `Microsoft.Compute/virtualMachines`, maar we beperken niet meer het aantal records dat wordt geretourneerd.
In plaats daarvan hebben we `summarize` en `count()` gebruikt om te definiëren hoe we de waarden willen groeperen en aggregeren op basis van de eigenschap. In dit voorbeeld is dat `properties.storageProfile.osDisk.osType`. Voor een voorbeeld van hoe deze tekenreeks er uitziet in het volledige object, raadpleegt u [Resources verkennen - detectie van virtuele machines](../concepts/explore-resources.md#virtual-machine-discovery).

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| summarize count() by tostring(properties.storageProfile.osDisk.osType)
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

```powershell
Search-AzureRmGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

Een andere manier om dezelfde query te schrijven, is door het `extend` van een eigenschap en deze een tijdelijke naam te geven voor gebruik in de query, in dit geval **os**. **os** wordt vervolgens door `summarize` en `count()` gebruikt, zoals in het vorige voorbeeld.

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| extend os = properties.storageProfile.osDisk.osType
| summarize count() by tostring(os)
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | extend os = properties.storageProfile.osDisk.osType | summarize count() by tostring(os)"
```

```powershell
Search-AzureRmGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | extend os = properties.storageProfile.osDisk.osType | summarize count() by tostring(os)"
```

> [!NOTE]
> Hoewel u met `=~` naar overeenkomsten kunt zoeken zonder onderscheid te maken tussen hoofd- en kleine letters, moet het hoofdlettergebruik in de query correct zijn als u eigenschappen gebruikt (zoals **properties.storageProfile.osDisk.osType**). Als de hoofd-/kleine letters in de eigenschap niet overeenkomen, kan het zijn dat er alsnog een waarde wordt geretourneerd, maar de groepering of samenvatting zou dan onjuist juist.

## <a name="show-storage"></a>Resources weergeven die opslag bevatten

In plaats van expliciet het type te definiëren dat u zoekt, zoekt deze voorbeeldquery elke Azure-resource die het woord **opslag** `contains`.

```Query
where type contains 'storage' | distinct type
```

```azurecli-interactive
az graph query -q "where type contains 'storage' | distinct type"
```

```powershell
Search-AzureRmGraph -Query "where type contains 'storage' | distinct type"
```

## <a name="list-publicip"></a>Een lijst van alle openbare IP-adressen weergeven

Net als bij de vorige query vindt deze query alles dat een type heeft met het woord **publicIPAddresses**. Deze query borduurt voort op dit patroon om de resultaten uit te sluiten waarvan **properties.ipAddress** null is, zodat alleen **properties.ipAddress** wordt geretourneerd, en om de resultaten te `limit` tot de top 100. Afhankelijk van uw gekozen shell, kan het zijn dat u de aanhalingstekens tussen escape-tekens moet plaatsen.

```Query
where type contains 'publicIPAddresses' and properties.ipAddress != ''
| project properties.ipAddress
| limit 100
```

```azurecli-interactive
az graph query -q "where type contains 'publicIPAddresses' and properties.ipAddress != '' | project properties.ipAddress | limit 100"
```

```powershell
Search-AzureRmGraph -Query "where type contains 'publicIPAddresses' and properties.ipAddress != '' | project properties.ipAddress | limit 100"
```

## <a name="count-resources-by-ip"></a>Resources tellen met IP-adressen die zijn geconfigureerd op abonnement

Door aan de vorige voorbeeldquery `summarize` en `count()` toe te voegen, krijgen we een lijst per abonnement van resources met geconfigureerde IP-adressen.

```Query
where type contains 'publicIPAddresses' and properties.ipAddress != ''
| summarize count () by subscriptionId
```

```azurecli-interactive
az graph query -q "where type contains 'publicIPAddresses' and properties.ipAddress != '' | summarize count () by subscriptionId"
```

```powershell
Search-AzureRmGraph -Query "where type contains 'publicIPAddresses' and properties.ipAddress != '' | summarize count () by subscriptionId"
```

## <a name="list-tag"></a>Een lijst weergeven van resources met een specifieke tagwaarde

We kunnen de resultaten beperken op basis van andere eigenschappen dan het type Azure-resource, bijvoorbeeld op basis van een tag. In dit voorbeeld filteren we op Azure-resources met de tagnaam **omgeving** en de waarde **Intern**.

```Query
where tags.environment=~'internal'
| project name
```

```azurecli-interactive
az graph query -q "where tags.environment=~'internal' | project name"
```

```powershell
Search-AzureRmGraph -Query "where tags.environment=~'internal' | project name"
```

Als het ook nodig is om op te geven welke tags een resource heeft en welke waarde die tags hebben, kan dit voorbeeld worden uitgebreid door de **tags** van de eigenschap toe te voegen aan het trefwoord `project`.

```Query
where tags.environment=~'internal'
| project name, tags
```

```azurecli-interactive
az graph query -q "where tags.environment=~'internal' | project name, tags"
```

```powershell
Search-AzureRmGraph -Query "where tags.environment=~'internal' | project name, tags"
```

## <a name="list-specific-tag"></a>Een lijst weergeven van alle opslagaccounts met een specifieke tagwaarde

Door de filtermogelijkheid van het vorige voorbeeld te combineren door het Azure-resourcetype te filteren op de eigenschap **type**, kunnen we onze zoekfunctie naar bepaalde typen Azure-resources beperken tot een specifieke tagnaam en een specifieke tagwaarde.

```Query
where type =~ 'Microsoft.Storage/storageAccounts'
| where tags['tag with a space']=='Custom value'
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

```powershell
Search-AzureRmGraph -Query "where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

> [!NOTE]
> In dit voorbeeld wordt `==` in plaats van het voorwaardelijke `=~` gebruikt om naar overeenkomsten te zoeken. `==` is een hoofdlettergevoelige overeenkomst.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [querytaal](../concepts/query-language.md)
- [Resources verkennen](../concepts/explore-resources.md)
- Voorbeelden uit [Geavanceerde query's](advanced.md) bekijken
---
title: Starter query's van Azure Resource Graph
description: Gebruik Azure Resource Graph om een aantal starter query's uit te voeren.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/22/2018
ms.topic: quickstart
ms.service: resource-graph
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: d5b2bb719bcd5c2145740a02bc408385953ff739
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50084527"
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

Azure CLI (met een extensie) en Azure PowerShell (met een module) ondersteunen Azure Resource Graph. Controleer voordat u een van de volgende query's uitvoert, of uw omgeving gereed is. Zie [Azure CLI](../first-query-azurecli.md#add-the-resource-graph-extension) en [Azure PowerShell](../first-query-powershell.md#add-the-resource-graph-module) voor stappen voor het installeren en valideren van uw gewenste shellomgeving.

## <a name="count-resources"></a>Azure-resources tellen

Deze query retourneert het aantal Azure-resources in de abonnementen waartoe u toegang hebt. Het is ook een goede query om te valideren of in uw gekozen shell de juiste Azure Resource Graph-onderdelen correct zijn geïnstalleerd.

```Query
summarize count()
```

```azurecli-interactive
az graph query -q "summarize count()"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "summarize count()"
```

## <a name="list-resources"></a>Een lijst van resources weergeven, gesorteerd op naam

Deze query retourneert een willekeurig resourcetype, maar alleen de eigenschappen **naam**, **type** en **locatie**. De query maakt gebruik van `order by` om de eigenschappen in oplopende volgorde (`asc`) op de eigenschap **naam** te sorteren.

```Query
project name, type, location
| order by name asc
```

```azurecli-interactive
az graph query -q "project name, type, location | order by name asc"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "project name, type, location | order by name asc"
```

## <a name="show-vms"></a>Alle virtuele machines weergeven, aflopend geordend op naam

Als u alleen virtuele machines wilt vermelden (die van het type `Microsoft.Compute/virtualMachines` zijn), kan een overeenkomst worden gezocht met de eigenschap **type** in de resultaten. Net als in de vorige query verandert u met `desc` de `order by` in aflopend. De `=~` in de type-overeenkomst betekent in Resource Graph dat de sortering hoofdlettergevoelig is.

```Query
project name, location, type
| where type =~ 'Microsoft.Compute/virtualMachines'
| order by name desc
```

```azurecli-interactive
az graph query -q "project name, location, type| where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

```azurepowershell-interactive
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

```azurepowershell-interactive
Search-AzureRmGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

## <a name="count-os"></a>Virtuele machines tellen op type besturingssysteem

Voortbouwend op de vorige query zijn we nog steeds een grens aan het stellen aan het aantal Azure-resources van het type `Microsoft.Compute/virtualMachines`, maar beperken we niet langer het aantal geretourneerde records.
In plaats daarvan hebben we `summarize` en `count()` gebruikt om te definiëren hoe we de waarden willen groeperen en aggregeren op basis van de eigenschap. In dit voorbeeld is dat `properties.storageProfile.osDisk.osType`. Voor een voorbeeld van hoe deze tekenreeks er uitziet in het volledige object, raadpleegt u [Resources verkennen - detectie van virtuele machines](../concepts/explore-resources.md#virtual-machine-discovery).

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| summarize count() by tostring(properties.storageProfile.osDisk.osType)
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

```azurepowershell-interactive
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

```azurepowershell-interactive
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

```azurepowershell-interactive
Search-AzureRmGraph -Query "where type contains 'storage' | distinct type"
```

## <a name="list-publicip"></a>Een lijst van alle openbare IP-adressen weergeven

Net als bij de vorige query vindt deze query alles dat een type is met het woord **publicIPAddresses**.
Deze query borduurt voort op dit patroon om de resultaten uit te sluiten waarvan **properties.ipAddress** null is, zodat alleen **properties.ipAddress** wordt geretourneerd, en om de resultaten te `limit` tot de top 100. Afhankelijk van uw gekozen shell, kan het zijn dat u de aanhalingstekens tussen escape-tekens moet plaatsen.

```Query
where type contains 'publicIPAddresses' and properties.ipAddress != ''
| project properties.ipAddress
| limit 100
```

```azurecli-interactive
az graph query -q "where type contains 'publicIPAddresses' and properties.ipAddress != '' | project properties.ipAddress | limit 100"
```

```azurepowershell-interactive
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

```azurepowershell-interactive
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

```azurepowershell-interactive
Search-AzureRmGraph -Query "where tags.environment=~'internal' | project name"
```

Als u ook wilt opgeven welke tags plus de bijbehorende waarden een resource heeft, voegt u de eigenschap **tags** aan het trefwoord `project` toe.

```Query
where tags.environment=~'internal'
| project name, tags
```

```azurecli-interactive
az graph query -q "where tags.environment=~'internal' | project name, tags"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "where tags.environment=~'internal' | project name, tags"
```

## <a name="list-specific-tag"></a>Een lijst weergeven van alle opslagaccounts met een specifieke tagwaarde

Combineer de filterfunctie uit het vorige voorbeeld en filter het Azure-resourcetype op de eigenschap **type**. Deze query beperkt ook het zoeken naar specifieke typen Azure-resources met een specifieke tagnaam- en waarde.

```Query
where type =~ 'Microsoft.Storage/storageAccounts'
| where tags['tag with a space']=='Custom value'
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

> [!NOTE]
> In dit voorbeeld wordt `==` in plaats van het voorwaardelijke `=~` gebruikt om naar overeenkomsten te zoeken. `==` is een hoofdlettergevoelige overeenkomst.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [querytaal](../concepts/query-language.md)
- [Resources verkennen](../concepts/explore-resources.md)
- Voorbeelden uit [Geavanceerde query's](advanced.md) bekijken
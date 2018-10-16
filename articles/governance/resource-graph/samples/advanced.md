---
title: Geavanceerde query's van Azure Resource Graph
description: Gebruik Azure Resource Graph om een aantal geavanceerde query's uit te voeren.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: quickstart
ms.service: resource-graph
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: a9281bc333b3edd12501a6813f12b9e7ad1bf59f
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47219264"
---
# <a name="advanced-resource-graph-queries"></a>Geavanceerde query's van Resource Graph

Om inzicht te krijgen in query's met Azure Resource Graph moet u eerst enige basiskennis hebben van de [querytaal](../concepts/query-language.md). Als u nog geen ervaring hebt met [Azure Data Explorer](../../../data-explorer/data-explorer-overview.md), is het raadzaam om eerst de basisbeginselen door te nemen, zodat u weet hoe u aanvragen opstelt voor de resources die u zoekt.

We nemen de volgende geavanceerde query's door:

> [!div class="checklist"]
> - [VMSS-capaciteit en -grootte opvragen](#vmss-capacity)
> - [Een lijst weergeven van alle tagnamen](#list-all-tags)
> - [Virtuele machines zoeken met reguliere expressies](#vm-regex)

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free) aan voordat u begint.

## <a name="language-support"></a>Taalondersteuning

Azure CLI (met een extensie) en Azure PowerShell (met een module) ondersteunen Azure Resource Graph. Controleer voordat u een van de volgende query's uitvoert of uw omgeving gereed is. Zie [Azure CLI](../first-query-azurecli.md#add-the-resource-graph-extension) en [Azure PowerShell](../first-query-powershell.md#add-the-resource-graph-module) voor stappen voor het installeren en valideren van uw gewenste shellomgeving.

## <a name="vmss-capacity"></a>VMSS-capaciteit en -grootte opvragen

Met deze query zoekt u VMSS-resources (virtuele-machineschaalset) en verkrijgt u diverse informatie, waaronder de grootte van de virtuele machines en de capaciteit van de schaalset. Voor deze informatie wordt de functie `toint()` gebruikt om de capaciteit te converteren naar een waarde die kan worden gesorteerd. Hiermee worden ook de namen van de geretourneerde waarden gewijzigd in eigenschappen met aangepaste namen.

```Query
where type=~ 'microsoft.compute/virtualmachinescalesets'
| where name contains 'contoso'
| project subscriptionId, name, location, resourceGroup, Capacity = toint(sku.capacity), Tier = sku.name
| order by Capacity desc
```

```azurecli-interactive
az graph query -q "where type=~ 'microsoft.compute/virtualmachinescalesets' | where name contains 'contoso' | project subscriptionId, name, location, resourceGroup, Capacity = toint(sku.capacity), Tier = sku.name | order by Capacity desc"
```

```powershell
Search-AzureRmGraph -Query "where type=~ 'microsoft.compute/virtualmachinescalesets' | where name contains 'contoso' | project subscriptionId, name, location, resourceGroup, Capacity = toint(sku.capacity), Tier = sku.name | order by Capacity desc"
```

## <a name="list-all-tags"></a>Een lijst weergeven van alle tagnamen

Deze query start met de tag en bouwt een JSON-object dat alle unieke tagnamen en hun overeenkomende typen vermeldt.

```Query
project tags
| summarize buildschema(tags)
```

```azurecli-interactive
az graph query -q "project tags | summarize buildschema(tags)"
```

```powershell
Search-AzureRmGraph -Query "project tags | summarize buildschema(tags)"
```

## <a name="vm-regex"></a>Virtuele machines zoeken met reguliere expressies

Deze query zoekt virtuele machines die overeenkomen met een [reguliere expressie](/dotnet/standard/base-types/regular-expression-language-quick-reference) (ook wel _regex_ genoemd).
Met de **reguliere overeenkomstexpressie @** kunt u de betreffende reguliere expressie definiëren. Deze is **^Contoso(.*)[0-9]+$**. De definitie van deze reguliere expressie wordt als volgt uitgelegd:

- `^`: Overeenkomst moet beginnen aan het begin van de tekenreeks.
- `Contoso`: De hoofdtekenreeks die we zoeken (hoofdlettergevoelig).
- `(.*)`: Een subexpressieovereenkomst:
  - `.`: Komt overeen met een willekeurig teken (met uitzondering van een nieuwe regel).
  - `*`: Komt nul keer of vaker overeen met vorig element.
- `[0-9]`: Tekengroepovereenkomst met de cijfers 0 t/m 9.
- `+`: Komt één keer of vaker overeen met vorig element.
- `$`: Overeenkomst met het vorige element moet voorkomen aan het einde van de tekenreeks.

Na de vergelijking op naam worden de namen in oplopende volgorde weergegeven.

```Query
where type =~ 'microsoft.compute/virtualmachines' and name matches regex @'^Contoso(.*)[0-9]+$'
| project name
| order by name asc
```

```azurecli-interactive
az graph query -q "where type =~ 'microsoft.compute/virtualmachines' and name matches regex @'^Contoso(.*)[0-9]+$' | project name | order by name asc"
```

```powershell
Search-AzureRmGraph -Query "where type =~ 'microsoft.compute/virtualmachines' and name matches regex @'^Contoso(.*)[0-9]+$' | project name | order by name asc"
```

## <a name="next-steps"></a>Volgende stappen

- Voorbeelden uit [Starter query's](starter.md) bekijken
- Meer informatie over de [querytaal](../concepts/query-language.md)
- [Resources verkennen](../concepts/explore-resources.md)
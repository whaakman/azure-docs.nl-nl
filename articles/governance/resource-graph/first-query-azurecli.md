---
title: Uw eerste Resource Graph-query uitvoeren met Azure CLI
description: Dit artikel helpt u bij de stappen om de Resource Graph-extensie voor Azure CLI in te schakelen en uw eerste query uit te voeren.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/22/2018
ms.topic: quickstart
ms.service: resource-graph
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: a252dd6698a5e4f724fcbbf821b01f63ab4f529b
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50086808"
---
# <a name="run-your-first-resource-graph-query-using-azure-cli"></a>Uw eerste Resource Graph-query uitvoeren met Azure CLI

De eerste stap voor het gebruik van Azure Resource Graph bestaat uit het controleren of de extensie voor [Azure CLI](/cli/azure/) is geïnstalleerd. In deze snelstartgids doorloopt u het proces voor het toevoegen van de extensie aan uw Azure CLI-installatie. U kunt de extensie gebruiken met een lokale installatie van Azure CLI of via de [Azure Cloud Shell](https://shell.azure.com).

Aan het einde van dit proces hebt u de extensie toegevoegd aan de Azure CLI-installatie van uw keuze en hebt u uw eerste Resource Graph-query uitgevoerd.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="add-the-resource-graph-extension"></a>De Resource Graph-extensie toevoegen

De extensie moet worden toegevoegd opdat Azure CLI query's kan uitvoeren voor Azure Resource Graph. Deze extensie werkt overal waar Azure CLI kan worden gebruikt, met inbegrip van [bash in Windows 10](/windows/wsl/install-win10), [Cloud Shell](https://shell.azure.com) (zowel zelfstandige als in de portal), de [Azure CLI Docker-installatiekopie](https://hub.docker.com/r/microsoft/azure-cli/), of lokaal geïnstalleerd.

1. Controleer of de meest recente versie van Azure CLI is geïnstalleerd (minimaal **2.0.45**). Als deze nog niet is geïnstalleerd, volgt u [deze instructies](/cli/azure/install-azure-cli-windows?view=azure-cli-latest) op.

1. In de gewenste Azure CLI-omgeving kunt u deze importeren met de volgende opdracht:

   ```azurecli-interactive
   # Add the Resource Graph extension to the Azure CLI environment
   az extension add --name resource-graph
   ```

1. Valideer of de extensie is geïnstalleerd en of het de verwachte versie is (ten minste **0.1.7**):

   ```azurecli-interactive
   # Check the extension list (note that you may have other extensions installed)
   az extension list

   # Run help for graph query options
   az graph query -h
   ```

## <a name="run-your-first-resource-graph-query"></a>Uw eerste Resource Graph-query uitvoeren

Nu de Azure CLI-extensie is toegevoegd aan uw gewenste omgeving, kunt u een eenvoudige Resource Graph-query uitvoeren. De query retourneert de eerste vijf Azure-resources met de **naam** en het **resourcetype** van elke resource.

1. Voer als volgt uw eerste Azure Resource Graph-query uit met de extensie `graph` en de opdracht `query`:

   ```azurecli-interactive
   # Login first with az login if not using Cloud Shell

   # Run Azure Resource Graph query
   az graph query -q 'project name, type | limit 5'
   ```

   > [!NOTE]
   > Omdat deze voorbeeldquery geen sorteermodificator geeft, bijvoorbeeld `order by`, zal deze query waarschijnlijk per aanvraag een andere set resources opleveren als de query meerdere keren wordt uitgevoerd.

1. Werk de query als volgt bij om de eigenschap **naam** te `order by`:

   ```azurecli-interactive
   # Run Azure Resource Graph query with 'order by'
   az graph query -q 'project name, type | limit 5 | order by name asc'
   ```

  > [!NOTE]
  > Net als bij de eerste query zal deze query waarschijnlijk per aanvraag een andere set resources opleveren als de query meerdere keren wordt uitgevoerd. De volgorde van de queryopdrachten is belangrijk. In dit voorbeeld komt `order by` na `limit`. Hiermee worden de queryresultaten eerst beperkt en daarna geordend.

1. Werk de query als volgt bij om eerst te `order by` op de eigenschap **naam** en daarna de resultaten van de top vijf te `limit`:

   ```azurecli-interactive
   # Run Azure Resource Graph query with `order by` first, then with `limit`
   az graph query -q 'project name, type | order by name asc | limit 5'
   ```

Wanneer de laatste query meerdere keren wordt uitgevoerd, ervan uitgaande dat niets in uw omgeving verandert, zijn de geretourneerde resultaten consistent en zoals verwacht. Ze zijn gesorteerd op de eigenschap **naam**, maar nog steeds beperkt tot de top 5-resultaten.

## <a name="cleanup"></a>Opschonen

Als u de Resource Graph-extensie uit uw Azure CLI-omgeving wilt verwijderen, kunt u dit doen met de volgende opdracht:

```azurecli-interactive
# Remove the Resource Graph extension from the Azure CLI environment
az extension remove -n resource-graph
```

> [!NOTE]
> Hiermee verwijdert u niet het extensiebestand dat u eerder hebt gedownload. U verwijdert het alleen uit de actieve Azure CLI-omgeving.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [querytaal](./concepts/query-language.md)
- [Resources verkennen](./concepts/explore-resources.md)
- Uw eerste query uitvoeren met [Azure PowerShell](first-query-powershell.md)
- Voorbeelden uit [Starter query's](./samples/starter.md) bekijken
- Voorbeelden uit [Geavanceerde query's](./samples/advanced.md) bekijken
- Feedback geven op [UserVoice](https://feedback.azure.com/forums/915958-azure-governance)
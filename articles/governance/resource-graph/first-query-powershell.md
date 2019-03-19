---
title: Uw eerste query uitvoeren met Azure PowerShell
description: In dit artikel helpt u bij de stappen om de Resource Graph-module voor Azure PowerShell in te schakelen en uw eerste query uit te voeren.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 01/23/2019
ms.topic: quickstart
ms.service: resource-graph
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: b3ab3552ad1b9bdd14a3d07cd426f8c0bc9dc865
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57759233"
---
# <a name="run-your-first-resource-graph-query-using-azure-powershell"></a>Uw eerste Resource Graph-query uitvoeren met Azure PowerShell

De eerste stap voor het gebruik van Azure Resource Graph bestaat uit het controleren of de module voor Azure PowerShell is geïnstalleerd. In deze snelstartgids doorloopt u het proces voor het toevoegen van de module aan uw Azure PowerShell-installatie.

Aan het einde van dit proces hebt u de module toegevoegd aan de Azure PowerShell-installatie van uw keuze en hebt u uw eerste Resource Graph-query uitgevoerd.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="add-the-resource-graph-module"></a>De Resource Graph-module toevoegen

De module moet worden toegevoegd opdat Azure PowerShell query's kan uitvoeren voor Azure Resource Graph. Deze module kan worden gebruikt met lokaal geïnstalleerde PowerShell, met [Azure Cloud Shell](https://shell.azure.com) of met de [Azure PowerShell Docker-installatiekopie](https://hub.docker.com/r/azuresdk/azure-powershell/).

### <a name="base-requirements"></a>Basisvereisten

Voor de Azure Resource Graph-module is de volgende software vereist:

- Azure PowerShell 1.0.0 of hoger. Als deze nog niet is geïnstalleerd, volgt u [deze instructies](/powershell/azure/install-az-ps) op.

- PowerShellGet 2.0.1 of hoger. Als deze nog niet is geïnstalleerd of bijgewerkt, volgt u [deze instructies](/powershell/gallery/installing-psget) op.

### <a name="install-the-module"></a>Installeer de module

De Resource Graph-module voor PowerShell is **Az.ResourceGraph**.

1. Voer vanuit een PowerShell-prompt met **beheerdersrechten** de volgende opdracht uit:

   ```azurepowershell-interactive
   # Install the Resource Graph module from PowerShell Gallery
   Install-Module -Name Az.ResourceGraph
   ```

1. Valideer als volgt of de module is geïmporteerd en de juiste versie (0.7.1) heeft:

   ```azurepowershell-interactive
   # Get a list of commands for the imported Az.ResourceGraph module
   Get-Command -Module 'Az.ResourceGraph' -CommandType 'Cmdlet'
   ```

## <a name="run-your-first-resource-graph-query"></a>Uw eerste Resource Graph-query uitvoeren

Nu de Azure PowerShell-module is toegevoegd aan uw gewenste omgeving, kunt u een eenvoudige Resource Graph-query uitvoeren. De query retourneert de eerste vijf Azure-resources met de **naam** en het **resourcetype** van elke resource.

1. Voer als volgt uw eerste Azure Resource Graph-query uit met de cmdlet `Search-AzGraph`:

   ```azurepowershell-interactive
   # Login first with Connect-AzAccount if not using Cloud Shell

   # Run Azure Resource Graph query
   Search-AzGraph -Query 'project name, type | limit 5'
   ```

   > [!NOTE]
   > Omdat deze voorbeeldquery geen sorteermodificator geeft, bijvoorbeeld `order by`, zal deze query waarschijnlijk per aanvraag een andere set resources opleveren als de query meerdere keren wordt uitgevoerd.

1. Werk de query als volgt bij om de eigenschap **naam** te `order by`:

   ```azurepowershell-interactive
   # Run Azure Resource Graph query with 'order by'
   Search-AzGraph -Query 'project name, type | limit 5 | order by name asc'
   ```

   > [!NOTE]
   > Net als bij de eerste query zal deze query waarschijnlijk per aanvraag een andere set resources opleveren als de query meerdere keren wordt uitgevoerd. De volgorde van de queryopdrachten is belangrijk. In dit voorbeeld komt `order by` na `limit`. Hiermee worden de queryresultaten eerst beperkt en daarna geordend.

1. Werk de query als volgt bij om eerst te `order by` op de eigenschap **naam** en daarna de resultaten van de top vijf te `limit`:

   ```azurepowershell-interactive
   # Run Azure Resource Graph query with `order by` first, then with `limit`
   Search-AzGraph -Query 'project name, type | order by name asc | limit 5'
   ```

Wanneer de laatste query meerdere keren wordt uitgevoerd, ervan uitgaande dat niets in uw omgeving verandert, zijn de geretourneerde resultaten consistent en zoals verwacht. Ze zijn gesorteerd op de eigenschap **naam**, maar nog steeds beperkt tot de top 5-resultaten.

## <a name="cleanup"></a>Opschonen

Als u de Resource Graph-module uit uw Azure PowerShell-omgeving wilt verwijderen, kunt u dit doen met de volgende opdracht:

```powershell
# Remove the Resource Graph module from the Azure PowerShell environment
Remove-Module -Name 'Az.ResourceGraph'
```

> [!NOTE]
> Hiermee verwijdert u niet het modulebestand dat u eerder hebt gedownload. U verwijdert deze alleen uit de actieve PowerShell-sessie.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [querytaal](./concepts/query-language.md)
- [Resources verkennen](./concepts/explore-resources.md)
- Uw eerste query uitvoeren met [Azure CLI](first-query-azurecli.md)
- Voorbeelden uit [Starter query's](./samples/starter.md) bekijken
- Voorbeelden uit [Geavanceerde query's](./samples/advanced.md) bekijken
- Feedback geven op [UserVoice](https://feedback.azure.com/forums/915958-azure-governance)
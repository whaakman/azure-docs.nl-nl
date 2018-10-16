---
title: Uw eerste Resource Graph-query uitvoeren met Azure PowerShell
description: In dit artikel helpt u bij de stappen om de Resource Graph-module voor Azure PowerShell in te schakelen en uw eerste query uit te voeren.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: quickstart
ms.service: resource-graph
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 1a2bc5626e94f5fcb0ec8c2be8d91c8fc6484e0b
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224559"
---
# <a name="run-your-first-resource-graph-query-using-azure-powershell"></a>Uw eerste Resource Graph-query uitvoeren met Azure PowerShell

De eerste stap voor het gebruik van Azure Resource Graph bestaat uit het controleren of de module voor Azure PowerShell is geïnstalleerd. In deze snelstartgids doorloopt u het proces voor het toevoegen van de module aan uw Azure PowerShell-installatie.

Aan het einde van dit proces hebt u de module toegevoegd aan de Azure PowerShell-installatie van uw keuze en hebt u uw eerste Resource Graph-query uitgevoerd.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="add-the-resource-graph-module"></a>De Resource Graph-module toevoegen

De module moet worden toegevoegd opdat Azure PowerShell query's kan uitvoeren voor Azure Resource Graph. Deze module kan worden gebruikt met lokaal geïnstalleerde Windows PowerShell en PowerShell Core, en met de [Azure PowerShell Docker-installatiekopie](https://hub.docker.com/r/azuresdk/azure-powershell/).

### <a name="base-requirements"></a>Basisvereisten

Voor de Azure Resource Graph-module is de volgende software vereist:

- Azure PowerShell 6.3.0 of hoger. Als deze nog niet is geïnstalleerd, voert u [deze instructies](/powershell/azure/install-azurerm-ps) uit.

  - Voor PowerShell Core gebruikt u de **Az**-versie van de Azure PowerShell-module.

  - Voor Windows PowerShell gebruikt u de **AzureRm**-versie van de Azure PowerShell-module.

  > [!NOTE]
  > Het wordt momenteel niet aanbevolen om de module te installeren in Cloud Shell.

- PowerShellGet. Als deze nog niet is geïnstalleerd of bijgewerkt, voert u [deze instructies](/powershell/gallery/installing-psget) uit.

### <a name="powershell-core"></a>PowerShell Core

De Resource Graph-module voor PowerShell Core is **Az.ResourceGraph**.

1. Voer vanuit een PowerShell Core-prompt met **beheerdersrechten** de volgende opdracht uit:

   ```powershell
   # Install the Resource Graph module from PowerShell Gallery
   Install-Module -Name Az.ResourceGraph
   ```

1. Valideer als volgt of de module is geïmporteerd en de juiste versie (0.2.0) heeft:

   ```powershell
   # Get a list of commands for the imported Az.ResourceGraph module
   Get-Command -Module 'Az.ResourceGraph' -CommandType 'Cmdlet'
   ```

1. Schakel met de volgende opdracht achterwaartse aliassen in voor **Az** naar **AzureRm**:

   ```powershell
   # Enable backwards alias compatibility
   Enable-AzureRmAlias
   ```

### <a name="windows-powershell"></a>Windows Powershell

De Resource Graph-module voor Windows PowerShell is **AzureRm.ResourceGraph**.

1. Voer vanuit een Windows PowerShell-prompt met **beheerdersrechten** de volgende opdracht uit:

   ```powershell
   # Install the Resource Graph (prerelease) module from PowerShell Gallery
   Install-Module -Name AzureRm.ResourceGraph -AllowPrerelease
   ```

1. Valideer als volgt of de module is geïmporteerd en de juiste versie (0.1.0 preview) heeft:

   ```powershell
   # Get a list of commands for the imported AzureRm.ResourceGraph module
   Get-Command -Module 'AzureRm.ResourceGraph' -CommandType 'Cmdlet'
   ```

## <a name="run-your-first-resource-graph-query"></a>Uw eerste Resource Graph-query uitvoeren

Nu de Azure PowerShell-module is toegevoegd aan uw gewenste omgeving, kunt u een eenvoudige Resource Graph-query proberen uit te voeren. De query retourneert de eerste vijf Azure-resources met de **naam** en het **resourcetype** van elke resource.

1. Voer als volgt uw eerste Azure Resource Graph-query uit met de cmdlet `Search-AzureRmGraph`:

   ```powershell
   # Login first with Connect-AzureRmAccount

   # Run Azure Resource Graph query
   Search-AzureRmGraph -Query 'project name, type | limit 5'
   ```

   > [!NOTE]
   > Omdat deze voorbeeldquery geen sorteermodificator geeft, bijvoorbeeld `order by`, zal deze query waarschijnlijk per aanvraag een andere set resources opleveren als de query meerdere keren wordt uitgevoerd.

1. Werk de query als volgt bij om de eigenschap **naam** te `order by`:

   ```powershell
   # Run Azure Resource Graph query with 'order by'
   Search-AzureRmGraph -Query 'project name, type | limit 5 | order by name asc'
   ```

  > [!NOTE]
  > Net als bij de eerste query zal deze query waarschijnlijk per aanvraag een andere set resources opleveren als de query meerdere keren wordt uitgevoerd. De volgorde van de queryopdrachten is belangrijk. In dit voorbeeld komt `order by` na `limit`. Hiermee worden de queryresultaten eerst beperkt en daarna geordend.

1. Werk de query als volgt bij om eerst te `order by` op de eigenschap **naam** en daarna de resultaten van de top 5 te `limit`:

   ```powershell
   # Run Azure Resource Graph query with `order by` first, then with `limit`
   Search-AzureRmGraph -Query 'project name, type | order by name asc | limit 5'
   ```

Wanneer de laatste query meerdere keren wordt uitgevoerd, ervan uitgaande dat niets in uw omgeving verandert, zijn de geretourneerde resultaten consistent en zoals verwacht. Ze zijn gesorteerd op de eigenschap **naam**, maar nog steeds beperkt tot de top 5-resultaten.

## <a name="cleanup"></a>Opschonen

Als u de Resource Graph-module uit uw Azure PowerShell-omgeving wilt verwijderen, kunt u dit doen met de volgende opdracht:

```powershell
# Remove the Resource Graph module from the Azure PowerShell environment
Remove-Module -Name 'AzureRm.ResourceGraph'
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
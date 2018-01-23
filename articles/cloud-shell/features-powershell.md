---
title: PowerShell in functies voor Azure Cloud-Shell (Preview) | Microsoft Docs
description: Overzicht van de functies van PowerShell in Azure Cloud-Shell
services: Azure
documentationcenter: 
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: damaerte
ms.openlocfilehash: 1113589507934d814bc8924267c07c99f8864657
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="features--tools-for-powershell-in-azure-cloud-shell-preview"></a>Functies en hulpprogramma's voor PowerShell in Azure Cloud-Shell (Preview)

[!include [features-introblock](../../includes/cloud-shell-features-introblock.md)]

> [!TIP]
> Functies en hulpprogramma's voor [Bash](features.md) is ook beschikbaar.

PowerShell in de Cloud-Shell (Preview) wordt uitgevoerd op `Windows Server 2016`.

## <a name="features"></a>Functies

### <a name="secure-automatic-authentication"></a>Automatische verificatie beveiligen

PowerShell in de Cloud-Shell (Preview) verifieert veilig en automatisch accounttoegang voor Azure PowerShell.

### <a name="files-persistence-across-sessions"></a>Persistentie bestanden over de sessies

Om te blijven behouden bestanden over de sessies, helpt Cloud Shell u bij het koppelen van een share Microsoft Azure-bestanden op de eerste keer opstarten.
Zodra de voltooid, Cloud Shell uw opslag automatisch koppelen (gekoppeld als `$home\clouddrive`) voor alle toekomstige sessies.
Omdat elke aanvraag voor Cloud-Shell is een tijdelijke machine toewijzen buiten bestanden uw `$home\clouddrive` en status van de machine blijven niet bestaan tussen sessies.

[Meer informatie over de Azure-bestandsshares gekoppeld aan Cloud Shell.](persisting-shell-storage-powershell.md)

### <a name="azure-drive-azure"></a>Azure-station (Azure:)

PowerShell in de Cloud-Shell (Preview) begint u in Azure station (`Azure:`).
Azure-station kunt u eenvoudig detectie en navigatie van Azure-resources zoals Compute, netwerk, opslag enzovoort vergelijkbaar met het bestandssysteem navigatie.
U kunt blijven gebruiken van de vertrouwde [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure) om deze resources te beheren.
Wijzigingen in de Azure-resources, hetzij rechtstreeks in Azure-portal of via Azure PowerShell-cmdlets zijn onmiddellijk worden deze weergegeven in het Azure-station.

![](media/features-powershell/azure-drive.png)

#### <a name="contextual-awareness"></a>Contextafhankelijke awareness

- **Bron op een doelgroep**: wanneer u in de context van een pad van de groep resource in het Azure-station (`Azure:`), de Resourcegroepnaam wordt automatisch doorgegeven aan de Azure PowerShell-cmdlets.

    ![](media/features-powershell/resource-group-autocomplete.png)

- **Get-AzureRmCommand**: deze cmdlet retourneert de lijst met opdrachten die van toepassing zijn in de context van de locatie in Azure station (`Azure:`). Deze ziet u bijvoorbeeld alleen opslag-gerelateerde opdrachten wanneer de gebruiker zich onder`Azure:\<subscription name>\StorageAccounts`

    ![](media/features-powershell/get-azurermcommand.png)

### <a name="rich-powershell-script-editing"></a>Rich PowerShell-script bewerken

Wanneer u VIM gebruikt PowerShell bestanden bewerken (`.ps1,.psm1,.psd1`), u syntaxismarkering en IntelliSense-ondersteuning voor het automatisch krijgen.
IntelliSense-ondersteuning is geïmplementeerd via een vim-invoegtoepassing die met een lokaal exemplaar van communiceert [PowerShell Editor Services](https://github.com/PowerShell/PowerShellEditorServices).

> [!TIP]
> Gebruik `TAB` voltooiing (IntelliSense) op opdrachtnamen, parameternamen en parameterwaarden ophalen (indien van toepassing).

![](media/features-powershell/powershell-editing-vim.png)

### <a name="extensible-model"></a>Extensible model

Met behulp van [PowerShellGet](https://docs.microsoft.com/powershell/module/powershellget), kunt u eenvoudig installeren (en bijwerken) aangepaste modules en de scripts van de [PowerShell Gallery](https://www.powershellgallery.com).
Na de installatie, worden uw modules automatisch vastgehouden over de Cloud Shell-sessies.

> [!TIP]
> Modules die zijn geïnstalleerd door gebruikers worden opgeslagen in de `$Home\CloudDrive\.pscloudshell\WindowsPowerShell` map. Geen symbolische koppeling voor deze map wordt gemaakt in de map documenten van de gebruiker (`$home\Documents\WindowsPowerShell`).

![](media/features-powershell/powershellget-module.png)

### <a name="management-of-guest-vms"></a>Beheer van Gast-VM 's

Met behulp van twee ingebouwde opdrachten - `Enter-AzureRmVM` en `Invoke-AzureRmVMCommand`, kunt u uw Azure VM's op afstand beheren.
Deze opdrachten zijn gebouwd op PowerShell voor externe toegang en PowerShell verbinding vereist met de Azure VM's.

## <a name="tools"></a>Hulpprogramma's

|**Categorie**    |**Naam**                                 |
|----------------|-----------------------------------------|
|Azure-hulpprogramma 's     |[Azure PowerShell (5.1.1)](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-5.1.1)<br> [Azure CLI (2.0.25)](https://docs.microsoft.com/cli/azure/overview)|
|Teksteditors    |VIM<br> nano                             |
|Pakketbeheer |PowerShellGet<br> PackageManagement<br> npm<br> PIP |
|Resourcebeheer  |GIT                                      |
|Databases       |[SQL Server-module](https://www.powershellgallery.com/packages/SqlServer)<br> [Sqlcmd-hulpprogramma](https://docs.microsoft.com/sql/tools/sqlcmd-utility)      |
|Hulpprogramma's testen      |Pester                                   |

## <a name="language-support"></a>Taalondersteuning

|**Taal**|**Versie**|
|------------|-----------|
|.NET        |4.6        |
|Node.js     |6.10       |
|PowerShell  |5.1 en [6.0 (bèta)](https://github.com/PowerShell/powershell/releases)       |
|Python      |2.7        |

## <a name="next-steps"></a>Volgende stappen

[Snel aan de slag met PowerShell in de Cloud-Shell (Preview)](quickstart-powershell.md)

[Meer informatie over Azure PowerShell](https://docs.microsoft.com/powershell/azure/)

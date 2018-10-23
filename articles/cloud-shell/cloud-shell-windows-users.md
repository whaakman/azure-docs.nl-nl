---
title: Azure Cloud Shell voor Windows-gebruikers | Microsoft Docs
description: Handleiding voor gebruikers die niet bekend met Linux-systemen bent
services: azure
documentationcenter: ''
author: maertendMSFT
manager: hemantm
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/03/2018
ms.author: damaerte
ms.openlocfilehash: 4fc4f6523eb19294cabdf6b5b910dd346a877502
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49645012"
---
# <a name="powershell-in-azure-cloud-shell-for-windows-users"></a>PowerShell in Azure Cloud Shell voor Windows-gebruikers

In mei 2018 worden de wijzigingen zijn [aangekondigd](https://azure.microsoft.com/blog/pscloudshellrefresh/) naar PowerShell in Azure Cloud Shell.
De PowerShell-ervaring in Azure Cloud Shell nu uitvoeringen [PowerShell Core 6](https://github.com/powershell/powershell) in een Linux-omgeving.
Met deze wijziging, mogelijk zijn er enkele verschillen in de PowerShell-ervaring in Cloud Shell in vergelijking met wat er wordt verwacht in een Windows PowerShell-ervaring.

## <a name="file-system-case-sensitivity"></a>Bestand system hoofdlettergevoeligheid

Het bestandssysteem is hoofdlettergevoelig in Windows, terwijl op Linux, het bestandssysteem hoofdlettergevoelig is.
Eerder `file.txt` en `FILE.txt` werden meegenomen naar het bestand zijn hetzelfde, maar nu ze worden beschouwd als andere bestanden.
Juiste hoofdlettergebruik moet worden gebruikt terwijl `tab-completing` in het bestandssysteem.
PowerShell-specifieke optreedt, zoals `tab-completing` namen van de cmdlets, parameters en waarden, zijn niet hoofdlettergevoelig.

## <a name="windows-powershell-aliases-vs-linux-utilities"></a>Windows PowerShell-aliassen vs Linux-hulpprogramma 's

Sommige bestaande PowerShell-aliassen hebben dezelfde naam als ingebouwde Linux-opdrachten, zoals `cat`,`ls`, `sort`, `sleep`, enzovoort. In PowerShell Core 6 zijn aliassen die met ingebouwde Linux-opdrachten conflicteren verwijderd.
Hieronder vindt u de algemene aliassen die zijn verwijderd, evenals hun gelijkwaardige opdrachten:  

|Alias wordt verwijderd   |Gelijkwaardige opdracht   |
|---|---|
|`cat`    | `Get-Content` |
|`curl`   | `Invoke-WebRequest` |
|`diff`   | `Compare-Object` |
|`ls`     | `dir` <br> `Get-ChildItem` |
|`mv`     | `Move-Item`   |
|`rm`     | `Remove-Item` |
|`sleep`  | `Start-Sleep` |
|`sort`   | `Sort-Object` |
|`wget`   | `Invoke-WebRequest` |

## <a name="persisting-home"></a>Permanente $HOME

Eerdere gebruikers kunnen alleen behouden scripts en andere bestanden in hun Cloud-station.
$HOME-map van de gebruiker is nu ook blijven behouden tussen sessies.

## <a name="powershell-profile"></a>PowerShell-profiel

Een gebruikersprofiel PowerShell is standaard niet gemaakt.
Als u wilt maken van uw profiel, een `PowerShell` map onder `$HOME/.config`.

```azurepowershell-interactive
mkdir (Split-Path $profile.CurrentUserAllHosts)
```

Onder `$HOME/.config/PowerShell`, kunt u de profielbestanden van uw - `profile.ps1` en/of `Microsoft.PowerShell_profile.ps1`.

## <a name="whats-new-in-powershell-core-6"></a>Wat is er nieuw in PowerShell Core 6

Voor meer informatie over wat er nieuw in PowerShell Core 6 verwijzen naar de [PowerShell-documenten](https://docs.microsoft.com/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6) en de [aan de slag met PowerShell Core](https://blogs.msdn.microsoft.com/powershell/2017/06/09/getting-started-with-powershell-core-on-windows-mac-and-linux/) blogbericht.

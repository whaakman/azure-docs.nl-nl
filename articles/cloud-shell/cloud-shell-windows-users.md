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
ms.date: 07/03/2018
ms.author: damaerte
ms.openlocfilehash: 5e318a0f64033aa0c4b306e547c11e1994afa229
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37861700"
---
# <a name="powershell-in-azure-cloud-shell-for-windows-users"></a>PowerShell in Azure Cloud Shell voor Windows-gebruikers

In mei 2018 worden de wijzigingen zijn [aangekondigd](https://azure.microsoft.com/blog/pscloudshellrefresh/) naar PowerShell in Azure Cloud Shell.  De PowerShell-ervaring in Azure Cloud Shell is nu 6 van de PowerShell Core in Linux.
Met deze wijziging zijn er bepaalde aspecten van PowerShell in Cloud Shell die mogelijk anders dan in Windows PowerShell 5.1 wordt verwacht.

## <a name="case-sensitivity"></a>Hoofdlettergevoeligheid

In Windows, moet aan het bestandssysteem is hoofdlettergevoelig.  In Linux is het bestandssysteem hoofdlettergevoelig.
Dit betekent dat eerder `file.txt` en `FILE.txt` werden meegenomen naar hetzelfde bestand, worden nu ze worden beschouwd als andere bestanden.
Juiste hoofdlettergebruik moet worden gebruikt terwijl `tab` voltooien van het bestandssysteem.  PowerShell-specifieke optreedt, zoals `tab` cmdlets, zijn niet hoofdlettergevoelig. 

## <a name="windows-powershell-alias-vs-linux-utilities"></a>Windows PowerShell-alias vs Linux-hulpprogramma 's

In Linux, bestaande opdrachten, zoals `ls`, `sort`, en `sleep` hebben voorrang op hun aliassen PowerShell.  Hieronder vindt u algemene verwijderde aliassen, evenals de gelijkwaardige opdrachten:  

|Alias wordt verwijderd   |Gelijkwaardige opdracht   |
|---|---|
|`ls`     | `dir` <br> `Get-ChildItem` |
|`sort`   | `Sort-Object` |
|`sleep`  | `Start-Sleep` |

## <a name="persisting-home-vs-homeclouddrive"></a>$Home vs $home\clouddrive persistent maken

Gebruikers van wie scripts en andere bestanden in hun Cloud-station persistent, is nu de map $HOME blijven behouden tussen sessies.

## <a name="powershell-profile"></a>PowerShell-profiel

Een PowerShell-profiel is standaard niet gemaakt.  Als u wilt maken van uw profiel, een `PowerShell` map onder `$HOME/.config`.  In `$HOME/.config/PowerShell`, kunt u uw profiel onder de naam `Microsoft.PowerShell_profile.ps1`.

## <a name="whats-new-in-powershell-core-6"></a>Wat is er nieuw in PowerShell Core 6

Voor meer informatie over wat er nieuw in PowerShell Core 6 verwijzen naar de [PowerShell-documenten](https://docs.microsoft.com/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6) en de [aan de slag met PowerShell Core](https://blogs.msdn.microsoft.com/powershell/2017/06/09/getting-started-with-powershell-core-on-windows-mac-and-linux/) blogbericht

---
title: Pas de feed voor virtuele Windows-bureaublad - gebruikers van Azure
description: Over het aanpassen van feed voor virtuele Windows-bureaublad-gebruikers met een PowerShell-cmdlets.
services: virtual-desktop
author: v-hevem
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 05/30/2019
ms.author: v-hevem
ms.openlocfilehash: 869760a12089ed7453199ad8a3fa18a6cca87511
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67157088"
---
# <a name="customize-feed-for-windows-virtual-desktop-users"></a>Feed aanpassen voor Windows Virtual Desktop-gebruikers

U kunt de feed aanpassen zodat de RemoteApp- en bronnen voor extern bureaublad wordt weergegeven in een herkenbare manier voor uw gebruikers.

Eerste, [downloaden en importeren van de Windows virtuele bureaublad PowerShell-module](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) te gebruiken in uw PowerShell-sessie als u dat nog niet gedaan hebt.

## <a name="customize-the-display-name-for-a-remoteapp"></a>De weergavenaam voor een RemoteApp-aanpassen

U kunt de weergavenaam voor een gepubliceerde RemoteApp-wijzigen door in te stellen de beschrijvende naam. De beschrijvende naam is standaard hetzelfde als de naam van de RemoteApp-programma.

Om op te halen een lijst met gepubliceerde RemoteApps voor een app-groep, kunt u de volgende PowerShell-cmdlet uitvoeren:

```powershell
Get-RdsRemoteApp -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname>
```
![Een schermafbeelding van de PowerShell-cmdlet Get-RDSRemoteApp met de naam en FriendlyName gemarkeerd.](media/get-rdsremoteapp.png)

Als u wilt een beschrijvende naam toewijzen aan een RemoteApp, voer de volgende PowerShell-cmdlet:

```powershell
Set-RdsRemoteApp -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname> -Name <existingappname> -FriendlyName <newfriendlyname>
```
![Een schermafbeelding van de PowerShell-cmdlet Set-RDSRemoteApp met de naam en nieuw FriendlyName gemarkeerd.](media/set-rdsremoteapp.png)

## <a name="customize-the-display-name-for-a-remote-desktop"></a>De weergavenaam voor een extern bureaublad aanpassen

U kunt de weergavenaam voor een gepubliceerde extern bureaublad wijzigen door in te stellen van een beschrijvende naam. Als u een host handmatig van toepassingen en bureaublad-app-groep via PowerShell hebt gemaakt, is de beschrijvende naam die standaard "Sessie-Desktop." Als u een host gemaakt van toepassingen en bureaublad-app-groep via de GitHub Azure Resource Manager-sjabloon of de Azure Marketplace-aanbieding, is de beschrijvende naam die standaard hetzelfde als de hostnaam van de groep van toepassingen.

Als u wilt ophalen van de remote desktop resource, voer de volgende PowerShell-cmdlet:

```powershell
Get-RdsRemoteDesktop -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname>
```
![Een schermafbeelding van de PowerShell-cmdlet Get-RDSRemoteApp met de naam en FriendlyName gemarkeerd.](media/get-rdsremotedesktop.png)

Als u wilt een beschrijvende naam toewijzen aan de remote desktop resource, voer de volgende PowerShell-cmdlet:

```powershell
Set-RdsRemoteDesktop -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname> -FriendlyName <newfriendlyname>
```
![Een schermafbeelding van de PowerShell-cmdlet Set-RDSRemoteApp met de naam en nieuw FriendlyName gemarkeerd.](media/set-rdsremotedesktop.png)

## <a name="next-steps"></a>Volgende stappen

Nu dat u de feed voor gebruikers hebt aangepast, kunt u aanmelden bij een virtuele Windows-bureaublad-client om te testen. Om dit te doen, gaat u verder naar het verbinding maken met Windows virtuele bureaublad uitleg:
    
 * [Verbinding maken vanaf Windows 10 of Windows 7](connect-windows-7-and-10.md)
 * [Verbinding maken via een webbrowser](connect-web.md) 

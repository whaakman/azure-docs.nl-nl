---
title: Eigenschappen van RDP met PowerShell - Azure aanpassen
description: Het RDP-eigenschappen voor Windows virtueel bureaublad aanpassen met PowerShell-cmdlets.
services: virtual-desktop
author: v-hevem
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 06/03/2019
ms.author: v-hevem
ms.openlocfilehash: 21d0b45b974f4bc806b26423b7deaef96e4bf350
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082649"
---
# <a name="customize-remote-desktop-protocol-properties-for-a-host-pool"></a>Remote Desktop Protocol-eigenschappen voor een groep host aanpassen

Eigenschappen van een host-pool Remote Desktop Protocol (RDP), zoals meerdere beeldschermen ervaring en audio-omleiding, aanpassen, kunt u een optimale gebruikerservaring voor uw gebruikers op basis van hun behoeften. U kunt de RDP-eigenschappen in met behulp van virtuele Windows-bureaublad aanpassen de **- CustomRdpProperty** parameter in de **Set RdsHostPool** cmdlet.

Zie [Remote Desktop RDP-bestandsinstellingen](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/rdp-files) voor een volledige lijst met ondersteunde eigenschappen en hun standaardwaarden.

Eerste, [downloaden en importeren van de Windows virtuele bureaublad PowerShell-module](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) te gebruiken in uw PowerShell-sessie als u dat nog niet gedaan hebt.

## <a name="add-or-edit-a-single-custom-rdp-property"></a>Toevoegen of bewerken van een aangepaste eigenschap RDP

Als u wilt toevoegen of bewerken van een aangepaste eigenschap RDP, voer de volgende PowerShell-cmdlet:

```powershell
Set-RdsHostPool -TenantName <tenantname> -Name <hostpoolname> -CustomRdpProperty "<property>"
```
![Een schermafbeelding van de PowerShell-cmdlet Get-RDSRemoteApp met de naam en FriendlyName gemarkeerd.](media/singlecustomrdpproperty.png)

## <a name="add-or-edit-multiple-custom-rdp-properties"></a>Toevoegen of bewerken aangepaste meerdere RDP-eigenschappen

Als u wilt toevoegen of bewerken aangepaste meerdere RDP-eigenschappen, de volgende PowerShell-cmdlets worden uitgevoerd door te geven van de aangepaste RDP-eigenschappen als een tekenreeks met door puntkomma's gescheiden:

```powershell
$properties="<property1>;<property2>;<property3>"
Set-RdsHostPool -TenantName <tenantname> -Name <hostpoolname> -CustomRdpProperty $properties
```
![Een schermafbeelding van de PowerShell-cmdlet Get-RDSRemoteApp met de naam en FriendlyName gemarkeerd.](media/multiplecustomrdpproperty.png)

## <a name="reset-all-custom-rdp-properties"></a>Opnieuw instellen van alle aangepaste RDP-eigenschappen

U kunt afzonderlijke aangepaste RDP-eigenschappen op hun standaardwaarden herstellen door de instructies in [toevoegen of bewerken van een aangepaste eigenschap RDP](#add-or-edit-a-single-custom-rdp-property), of u kunt alle aangepaste RDP-eigenschappen voor een groep host opnieuw instellen door het uitvoeren van de volgende PowerShell-cmdlet:

```powershell
Set-RdsHostPool -TenantName <tenantname> -Name <hostpoolname> -CustomRdpProperty ""
```
![Een schermafbeelding van de PowerShell-cmdlet Get-RDSRemoteApp met de naam en FriendlyName gemarkeerd.](media/resetcustomrdpproperty.png)

## <a name="next-steps"></a>Volgende stappen

Nu dat u de RDP-eigenschappen voor een bepaalde host pool hebt aangepast, kunt u zich een virtuele Windows-bureaublad-client om te testen als onderdeel van een gebruikerssessie. Om dit te doen, gaat u verder naar het verbinding maken met Windows virtuele bureaublad uitleg:

- [Verbinding maken vanaf Windows 10 en Windows 7](connect-windows-7-and-10.md)
- [Verbinding maken via een webbrowser](connect-web.md)

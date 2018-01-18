---
title: Geheimen in Azure-Stack draaien | Microsoft Docs
description: Ontdek hoe u uw geheimen in Azure-Stack draaien.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 49071044-6767-4041-9EDD-6132295FA551
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/08/2018
ms.author: mabrigg
ms.openlocfilehash: e2e9d93af3889714ade1d0364a6f747c184e6d75
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="rotate-secrets-in-azure-stack"></a>Geheimen in Azure-Stack draaien

*Van toepassing op: Azure Stack geïntegreerd systemen*

De wachtwoorden voor Azure-Stack-onderdelen op een reguliere uitgebracht bijwerkt.

## <a name="updating-the-passwords-for-the-baseboard-management-controller-bmc"></a>Bijwerken van de wachtwoorden voor de BMC (baseboard management controller)

De baseboard management controllers (BMC) bewaken de fysieke status van uw servers. De specificaties en instructies voor het bijwerken van het wachtwoord van de BMC variëren, afhankelijk van de leverancier van uw oorspronkelijke leveranciers (OEM)-hardware.

1. Werk de BMC op fysieke servers van de Azure-Stack door de OEM-instructies te volgen. Het wachtwoord voor elke BMC in uw omgeving moet hetzelfde zijn.
2. Open een bevoorrechte eindpunt in Azure Stack-sessies. Zie voor instructies [met behulp van de bevoegde eindpunt in Azure-Stack](azure-stack-privileged-endpoint.md).
3. Nadat uw PowerShell-prompt is gewijzigd in **[IP-adres of ERCS VM name]: PS >** of **[azs ercs01]: PS >**, afhankelijk van de omgeving, voeren `Set-BmcPassword` door het uitvoeren van `invoke-command`. Uw sessievariabele bevoorrechte eindpunt als parameter doorgeven. Bijvoorbeeld:

    ```powershell
    # Interactive Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PECred = Get-Credential "<Domain>\CloudAdmin" -Message "PE Credentials" 
    $NewBMCpwd = Read-Host -Prompt "Enter New BMC password" -AsSecureString 

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        Set-Bmcpassword -bmcpassword $using:NewBMCpwd
    }
    ```
    
    U kunt ook de statische PowerShell-versie met de wachtwoorden gebruiken als coderegels:
    
    ```powershell
    # Static Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PEUser = "<Privileged Endpoint user for exmaple Domain\CloudAdmin>"
    $PEpwd = ConvertTo-SecureString "<Privileged Endpoint Password>" -AsPlainText -Force
    $PECred = New-Object System.Management.Automation.PSCredential ($PEUser, $PEpwd) 
    $NewBMCpwd = ConvertTo-SecureString "<New BMC Password>" -AsPlainText -Force 

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        Set-Bmcpassword -bmcpassword $using:NewBMCpwd
    }
    ```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de beveiligings- en Azure-Stack, [Azure Stack infrastructuur beveiligingspostuur](azure-stack-security-foundations.md).

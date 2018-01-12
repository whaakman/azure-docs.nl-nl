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
ms.openlocfilehash: 9b65a3cb5cdcc8a558e5c989026f2eee2f527bb5
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="rotate-secrets-in-azure-stack"></a>Geheimen in Azure-Stack draaien

*Van toepassing op: Azure Stack geïntegreerd systemen*

De wachtwoorden voor Azure-Stack-onderdelen op een reguliere uitgebracht bijwerkt.

## <a name="updating-the-passwords-for-the-baseboard-management-controller-bmc"></a>Bijwerken van de wachtwoorden voor de BMC (baseboard management controller)

De baseboard management controllers (BMC) bewaken de fysieke status van uw servers. De specificaties en instructies voor het bijwerken van het wachtwoord van de BMC variëren, afhankelijk van de leverancier van uw oorspronkelijke leveranciers (OEM)-hardware.

1. Werk de BMC op uw server door de OEM-instructies te volgen. Het wachtwoord voor elke BMC in uw omgeving moet hetzelfde zijn.
2. Open een bevoorrechte eindpunt in Azure Stack-sessies. Zie voor instructies [met behulp van de bevoegde eindpunt in Azure-Stack](azure-stack-privileged-endpoint.md).
3. Nadat uw PowerShell-prompt is gewijzigd in **[IP-adres of ERCS VM name]: PS >** of **[azs ercs01]: PS >**, afhankelijk van de omgeving, voeren `Set-BmcPassword` door het uitvoeren van `invoke-command`. Uw sessievariabele bevoorrechte eindpunt als parameter doorgeven.  
Bijvoorbeeld:
    ```powershell
    $PEPSession = New-PSSession -ComputerName <ERCS computer name> -Credential <CloudAdmin credential> -ConfigurationName "PrivilegedEndpoint"  
    
    Invoke-Command -Session $PEPSession -ScriptBlock {
        param($password)
        set-bmcpassword -bmcpassword $password
    } -ArgumentList (<LatestPassword as a SecureString>) 
    ```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de beveiligings- en Azure-Stack, [Azure Stack infrastructuur beveiligingspostuur](azure-stack-security-foundations.md).
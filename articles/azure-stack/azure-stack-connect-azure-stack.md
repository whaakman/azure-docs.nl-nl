---
title: Verbinding maken met Azure Stack | Microsoft Docs
description: Informatie over het verbinding maken met Azure-Stack.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 3cebbfa6-819a-41e3-9f1b-14ca0a2aaba3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: d9d7beae9ea81f2568377ee3593362871aae98fe
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="connect-to-azure-stack"></a>Verbinding maken met Azure Stack

*Van toepassing op: Azure stapelen Development Kit*

Voor het beheren van resources, moet u eerst verbinding met de Azure-Stack Development Kit. In dit artikel wordt beschreven welke stappen u verbinding maken met de development kit uitvoeren. U kunt een van de volgende verbindingsopties:

* [Verbinding met extern bureaublad](#connect-with-remote-desktop). Wanneer u verbinding maakt met behulp van de verbinding met extern bureaublad, één gebruiker kan snel verbinding te maken van de development kit.
* [Virtueel particulier netwerk (VPN)](#connect-with-vpn). Wanneer u verbinding maakt met behulp van een VPN, meerdere gebruikers kunnen gelijktijdig verbinding maken via clients buiten de Azure-Stack-infrastructuur (setup vereist).

<a name="connect-to-azure-stack-with-remote-desktop"></a>
##  <a name="connect-to-azure-stack-by-using-remote-desktop-connection"></a>Verbinding maken met Azure-Stack via verbinding met extern bureaublad
Meerdere gelijktijdige gebruikers kan resources in de portal operator of de gebruikersportal via de verbinding met extern bureaublad beheren.

1. Verbinding met extern bureaublad openen en verbinding maken met de development kit. Voer voor de gebruikersnaam, **AzureStack\AzureStackAdmin**. Gebruik de operator wachtwoord die u hebt opgegeven bij het instellen van Azure-Stack.  

2. Open Serverbeheer op de ontwikkelcomputer kit. Selecteer **lokale Server**, schakelt de **verbeterde beveiliging van Internet Explorer** selectievakje en sluit u Server Manager.

3. Openen van de [gebruikersportal](azure-stack-key-features.md#portal), gaat u naar https://portal.local.azurestack.external/. Meld u met behulp van gebruikersreferenties. Openen van de Azure-Stack [operator portal](azure-stack-key-features.md#portal), gaat u naar https://adminportal.local.azurestack.external/. Meld u met de Azure Active Directory (Azure AD)-referenties die u hebt opgegeven tijdens de installatie.

<a name="connect-to-azure-stack-with-vpn"></a>
## <a name="connect-to-azure-stack-by-using-vpn"></a>Verbinding maken met Azure-Stack met behulp van VPN

U kunt een gesplitste tunnel VPN-verbinding met een Azure-Stack Development Kit maken. U kunt een VPN-verbinding gebruiken voor toegang tot de Azure-Stack operator-portal, de gebruikersportal en lokaal geïnstalleerde hulpmiddelen zoals Visual Studio en PowerShell om Azure-Stack-resources te beheren. VPN-connectiviteit wordt ondersteund in Azure AD, en ook in implementaties van Active Directory Federation Services (AD FS). VPN-verbindingen maken het mogelijk dat meerdere clients verbinding maken met Azure-Stack op hetzelfde moment. 

> [!NOTE] 
> Een VPN-verbinding biedt geen verbinding met Azure-Stack infrastructuur virtuele machines. 

### <a name="prerequisites"></a>Vereisten

1. Installeer [Azure Stack-compatibele Azure PowerShell](azure-stack-powershell-install.md) op uw lokale computer.  
2. Download de [hulpprogramma's voor het werken met Azure-Stack](azure-stack-powershell-download.md). 

### <a name="set-up-vpn-connectivity"></a>VPN-verbinding instellen

Open Windows PowerShell als beheerder op de lokale Windows-computer voor het maken van een VPN-verbinding met de development kit. Voer het volgende script (update de IP-adres en het wachtwoord waarden voor uw omgeving):

```PowerShell 
# Configure Windows Remote Management (WinRM), if it's not already configured.
winrm quickconfig  

Set-ExecutionPolicy RemoteSigned

# Import the Connect module.
Import-Module .\Connect\AzureStack.Connect.psm1 

# Add the development kit computer’s host IP address and certificate authority (CA) to the list of trusted hosts. Make sure you update the IP address and password values for your environment. 

$hostIP = "<Azure Stack host IP address>"

$Password = ConvertTo-SecureString `
  "<operator's password provided when deploying Azure Stack>" `
  -AsPlainText `
  -Force

Set-Item wsman:\localhost\Client\TrustedHosts `
  -Value $hostIP `
  -Concatenate

# Create a VPN connection entry for the local user.
Add-AzsVpnConnection `
  -ServerAddress $hostIP `
  -Password $Password

```

Als setup is voltooid, **azurestack** wordt weergegeven in de lijst met VPN-verbindingen.

![Netwerkverbindingen](media/azure-stack-connect-azure-stack/image3.png)  

### <a name="connect-to-azure-stack"></a>Verbinding maken met Azure Stack

Verbinding maken met de Azure-Stack-exemplaar met behulp van een van de volgende methoden:  

* Gebruik de `Connect-AzsVpn ` opdracht: 
    
  ```PowerShell
  Connect-AzsVpn `
    -Password $Password
  ```

  Wanneer u wordt gevraagd, vertrouwt de Azure-Stack-host en installeer het certificaat van **AzureStackCertificateAuthority** in het certificaatarchief van de lokale computer. (U kunt de prompt mogelijk verborgen door het PowerShell-venster.) 

* Selecteer op de lokale computer **netwerkinstellingen** > **VPN** > **azurestack** > **verbinding**. Geef de gebruikersnaam bij de prompt aanmelden (**AzureStack\AzureStackAdmin**) en uw wachtwoord.

### <a name="test-vpn-connectivity"></a>VPN-verbindingen testen

De portal om verbinding te testen, open een webbrowser en gaat u naar de gebruikersportal (https://portal.local.azurestack.external/) of de operator-portal (https://adminportal.local.azurestack.external/). Meld u aan en resources maken.  

## <a name="next-steps"></a>Volgende stappen

[Virtuele machines beschikbaar maken voor uw Azure-Stack-gebruikers](azure-stack-tutorial-tenant-vm.md)


---
title: Verbinding maken met Azure Stack | Microsoft Docs
description: Leer hoe u verbinding maken met Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 3cebbfa6-819a-41e3-9f1b-14ca0a2aaba3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/28/2018
ms.author: mabrigg
ms.openlocfilehash: 1cdf013325afe4b217f5f56043e06f60a4933419
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49993168"
---
# <a name="connect-to-azure-stack-development-kit"></a>Verbinding maken met Azure Stack Development Kit

*Is van toepassing op: Azure Stack Development Kit*

Voor het beheren van resources, moet u eerst verbinding maken met de Azure Stack Development Kit. In dit artikel wordt beschreven welke stappen u ondernemen om te verbinden met de development kit. U kunt een van de volgende verbindingsopties:

* [Verbinding met extern bureaublad](#connect-with-remote-desktop). Wanneer u verbinding maakt met behulp van de verbinding met extern bureaublad, een enkele gebruiker kan snel verbinding maken met de development kit.
* [Virtueel particulier netwerk (VPN)](#connect-with-vpn). Wanneer u verbinding maakt met behulp van een VPN, meerdere gebruikers kunnen gelijktijdig verbinding maken van clients buiten de Azure Stack-infrastructuur. Een VPN-verbinding, moet u instellingen.

<a name="connect-to-azure-stack-with-remote-desktop"></a>
##  <a name="connect-to-azure-stack-by-using-remote-desktop-connection"></a>Verbinding maken met Azure Stack met behulp van de verbinding met extern bureaublad

Meerdere gelijktijdige gebruikers kan resources in de operator-portal of de gebruikersportal via verbinding met extern bureaublad beheren.

1. Verbinding met extern bureaublad openen en verbinding maken met de development kit. Voer voor de naam van de gebruiker, **AzureStack\AzureStackAdmin**. Gebruik de operator-wachtwoord die u hebt opgegeven bij het instellen van Azure Stack.  

2. Open Serverbeheer op de ontwikkelcomputer kit. Selecteer **lokale Server**, schakel de **verbeterde beveiliging van Internet Explorer** selectievakje en sluit Serverbeheer.

3. Om te openen de [gebruikersportal](azure-stack-key-features.md#portal), gaat u naar https://portal.local.azurestack.external/. Meld u aan met behulp van de referenties van gebruiker. Openen van de Azure Stack [operator portal](azure-stack-key-features.md#portal), gaat u naar https://adminportal.local.azurestack.external/. Meld u aan met behulp van de Azure Active Directory (Azure AD)-referenties die u hebt opgegeven tijdens de installatie.

<a name="connect-to-azure-stack-with-vpn"></a>
## <a name="connect-to-azure-stack-by-using-vpn"></a>Verbinding maken met Azure Stack met VPN

U kunt een split-tunnel VPN-verbinding met een Azure Stack Development Kit maken. U kunt een VPN-verbinding gebruiken voor toegang tot de portal voor Azure Stack-operator, de gebruikersportal en de lokaal geïnstalleerde programma's zoals Visual Studio en PowerShell om Azure Stack-resources te beheren. VPN-connectiviteit wordt ondersteund in Azure AD en implementaties van Active Directory Federation Services (AD FS). VPN-verbindingen maken het mogelijk dat meerdere clients verbinding maken met Azure Stack op hetzelfde moment.

> [!NOTE]
> Een VPN-verbinding biedt geen verbinding met Azure Stack-infrastructuur virtuele machines.

### <a name="prerequisites"></a>Vereisten

1. Installeer [Azure Stack-compatibel is met Azure PowerShell](azure-stack-powershell-install.md) op uw lokale computer.  
2. In het [Azure Stack development Kit, worden de blobEndpoint](azure-stack-powershell-download.md) .

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

Maak verbinding met de Azure Stack-exemplaar met behulp van een van de volgende methoden:  

* Gebruik de `Connect-AzsVpn ` opdracht:
    
  ```PowerShell
  Connect-AzsVpn `
    -Password $Password
  ```

  Wanneer u hierom wordt gevraagd, vertrouwt de Azure Stack-host en installeert u het certificaat van **AzureStackCertificateAuthority** in het certificaatarchief van de lokale computer. (U kunt de prompt mogelijk verborgen door het PowerShell-venster.)

* Selecteer op de lokale computer **netwerkinstellingen** > **VPN** > **azurestack** > **verbinding**. Voer de gebruikersnaam in bij de prompt aanmelden (**AzureStack\AzureStackAdmin**) en het wachtwoord.

### <a name="test-vpn-connectivity"></a>VPN-connectiviteit testen

De portal om verbinding te testen, open een webbrowser en ga vervolgens naar de gebruikersportal (https://portal.local.azurestack.external/) of de operator-portal (https://adminportal.local.azurestack.external/). Meld u aan en resources maken.  

## <a name="next-steps"></a>Volgende stappen

[Virtuele machines beschikbaar maken voor uw Azure Stack-gebruikers](azure-stack-tutorial-tenant-vm.md)

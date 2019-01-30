---
title: Verbinding maken met Azure Stack | Microsoft Docs
description: Leer hoe u verbinding maken met de ASDK.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 3cebbfa6-819a-41e3-9f1b-14ca0a2aaba3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: jeffgilb
ms.reviewer: knithinc
ms.lastreviewed: 10/25/2018
ms.openlocfilehash: 44f12c72716e98e2f28796359467685e0af579dd
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55238243"
---
# <a name="connect-to-the-asdk"></a>Verbinding maken met de ASDK

Voor het beheren van resources, moet u eerst naar de Azure Stack Development Kit (ASDK) verbinden. In dit artikel beschrijven we de stappen die u ondernemen om te verbinden met de ASDK met behulp van de volgende verbindingsopties:

* [Verbinding met extern bureaublad (RDP)](#connect-with-rdp). Wanneer u verbinding maakt met behulp van de verbinding met extern bureaublad, een enkele gebruiker kan snel verbinding maken met de development kit.
* [Virtueel particulier netwerk (VPN)](#connect-with-vpn). Wanneer u verbinding maakt met behulp van een VPN, meerdere gebruikers kunnen gelijktijdig verbinding maken van clients buiten de Azure Stack-infrastructuur. Een VPN-verbinding, moet u instellingen.

<a name="connect-with-rdp"></a>
## <a name="connect-to-azure-stack-using-rdp"></a>Verbinding maken met Azure Stack met behulp van RDP

Meerdere gelijktijdige gebruikers kan resources in de Azure Stack-beheerportal of de gebruikersportal via verbinding met extern bureaublad rechtstreeks vanuit de hostcomputer ASDK beheren. 

> [!TIP]
> Deze optie kunt u RDP opnieuw gebruiken terwijl u aangemeld bij de computer ASDK voor het aanmelden bij virtuele machines die op de hostcomputer ASDK is gemaakt. 

1. Open verbinding met extern bureaublad (mstc.exe) en verbinding maken met development kit host IP-adres van de computer met een account dat is gemachtigd voor het extern aanmelden bij de computer ASDK. Standaard **AzureStack\AzureStackAdmin** toegangsmachtigingen voor externe in op de hostcomputer ASDK.  

2. Open Serverbeheer (ServerManager.exe) op de hostcomputer development kit. Selecteer **lokale Server**, uitschakelen **verbeterde beveiliging van Internet Explorer**, en sluit Serverbeheer.

3. Aanmelden bij de beheerportal als **AzureStack\CloudAdmin** of andere referenties voor Azure Stack-operators gebruiken. Het adres van ASDK-beheer-portal is [ https://adminportal.local.azurestack.external ](https://adminportal.local.azurestack.external).

4. Meld u aan bij de gebruikersportal aanmeldt als **AzureStack\CloudAdmin** of andere Azure Stack-gebruikersreferenties gebruiken. De portal ASDK gebruiker-adres is [ https://portal.local.azurestack.external ](https://portal.local.azurestack.external).

> [!NOTE]
> Zie voor meer informatie over het gebruik van welk account [basisprincipes van beheer ASDK](asdk-admin-basics.md#what-account-should-i-use).

<a name="connect-with-vpn"></a>
## <a name="connect-to-azure-stack-using-vpn"></a>Verbinding maken met Azure Stack met VPN

U kunt een split-tunnel VPN-verbinding met een hostcomputer ASDK voor toegang tot de Azure Stack-portals en lokaal geïnstalleerde programma's zoals Visual Studio en PowerShell maken. Met VPN-verbindingen tussen meerdere gebruikers verbinding kunnen maken op hetzelfde moment op Azure Stack-resources die worden gehost door de ASDK.

VPN-connectiviteit wordt ondersteund voor zowel Azure AD en Active Directory Federatieservices (AD FS)-implementaties.

> [!NOTE]
> Een VPN-verbinding *niet* bieden connectiviteit met Azure Stack-VM's. U wordt pas weer verbinding met Azure Stack virtuele machines terwijl u bent verbonden via VPN.

### <a name="prerequisites"></a>Vereisten
Voordat u een VPN-verbinding met de ASDK instelt, zorg ervoor dat u de volgende vereisten wordt voldaan.

- Installeer [Azure Stack-compatibel is met Azure PowerShell](asdk-post-deploy.md#install-azure-stack-powershell) op uw lokale computer.  
- In het [Azure Stack development Kit, worden de blobEndpoint](asdk-post-deploy.md#download-the-azure-stack-tools) .

### <a name="set-up-vpn-connectivity"></a>VPN-verbinding instellen

Voor het maken van een VPN-verbinding met de ASDK, open PowerShell als beheerder op de lokale Windows-computer. Voer het volgende script (update de IP-adres en het wachtwoord waarden voor uw omgeving):

```PowerShell
# Change directories to the default Azure Stack tools directory
cd C:\AzureStack-Tools-master

# Configure Windows Remote Management (WinRM), if it's not already configured.
winrm quickconfig  

Set-ExecutionPolicy RemoteSigned

# Import the Connect module.
Import-Module .\Connect\AzureStack.Connect.psm1

# Add the development kit host computer’s IP address as the ASDK certificate authority (CA) to the list of trusted hosts. Make sure you update the IP address and password values for your environment.

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

![Netwerkverbindingen](media/asdk-connect/vpn.png)  

### <a name="connect-to-azure-stack"></a>Verbinding maken met Azure Stack

  Maak verbinding met de Azure Stack-exemplaar met behulp van een van de volgende methoden:  

  * Gebruik de `Connect-AzsVpn ` opdracht:
      
    ```PowerShell
    Connect-AzsVpn `
      -Password $Password
    ```

  * Selecteer op de lokale computer **netwerkinstellingen** > **VPN** > **azurestack** > **verbinding**. Voer de gebruikersnaam in bij de prompt aanmelden (**AzureStack\AzureStackAdmin**) en het wachtwoord.

De eerste keer dat u verbinding maakt, wordt u gevraagd voor het installeren van de Azure Stack-basiscertificaat van **AzureStackCertificateAuthority** in het certificaatarchief van de lokale computer. Deze stap worden de ASDK-certificeringsinstantie (CA) toegevoegd aan de lijst met vertrouwde hosts. Klik op **Ja** om het certificaat te installeren.

![Basiscertificaat](media/asdk-connect/cert.png)  
  
  > [!IMPORTANT]
  > De prompt kan verborgen door het PowerShell-venster of andere toepassingen.

### <a name="test-vpn-connectivity"></a>VPN-connectiviteit testen

De portal om verbinding te testen, open een webbrowser en ga vervolgens naar de gebruikersportal (https://portal.local.azurestack.external/) of de beheerportal (https://adminportal.local.azurestack.external/). 

Meld u aan met de referenties van het juiste abonnement maken en beheren van resources.  

## <a name="next-steps"></a>Volgende stappen

[Problemen oplossen](asdk-troubleshooting.md)

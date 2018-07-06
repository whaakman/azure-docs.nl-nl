---
title: Verbinding maken met Azure Stack | Microsoft Docs
description: Meer informatie over het verbinding maken met Azure Stack
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
ms.date: 08/22/2017
ms.author: mabrigg
ms.openlocfilehash: 21015d31a738d3ad57048fe4a703bf78dda7e40c
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37865767"
---
# <a name="connect-to-azure-stack"></a>Verbinding maken met Azure Stack

Voor het beheren van resources, moet u verbinding maken met de Azure Stack Development Kit. Dit artikel vindt u de stappen die worden vereist om verbinding maken met de development kit. U kunt een van de volgende verbindingsopties gebruiken:

* [Extern bureaublad](#connect-with-remote-desktop): Hiermee kunt u meerdere gelijktijdige gebruikers snel verbinding vanaf de development kit.
* [Virtuele particuliere netwerk (VPN)](#connect-with-vpn): kunnen meerdere gelijktijdige gebruikers verbinding van clients buiten de Azure Stack-infrastructuur maken (configuratie vereist).

## <a name="connect-to-azure-stack-with-remote-desktop"></a>Verbinding maken met Azure Stack met extern bureaublad
Met een extern bureaublad-verbinding, kan meerdere gelijktijdige gebruikers werken met de portal om resources te beheren.

1. Een verbinding met extern bureaublad openen en verbinding maken met de development kit. Voer **AzureStack\AzureStackAdmin** als de gebruikersnaam en het beheerderswachtwoord dat u hebt opgegeven tijdens de installatie van de Azure Stack.  

2. In de development kit computer, open Serverbeheer, klikt u op **lokale Server**, verbeterde beveiliging van Internet Explorer uitschakelen en sluit Serverbeheer.

3. Als u wilt openen in de portal, gaat u naar (https://portal.local.azurestack.external/) en meld u aan met behulp van de referenties van gebruiker.


## <a name="connect-to-azure-stack-with-vpn"></a>Verbinding maken met Azure Stack met VPN

U kunt een split-tunnel virtuele particuliere netwerk (VPN)-verbinding met een Azure Stack Development Kit maken. Via de VPN-verbinding, moet u toegang tot de beheerdersportal van de, de gebruikersportal, en lokaal geïnstalleerde hulpprogramma's zoals Visual Studio en PowerShell om Azure Stack-resources te beheren. VPN-connectiviteit wordt ondersteund in zowel Azure Active Directory(AAD) en Active Directory Federation Services(AD FS) op basis van implementaties. VPN-verbindingen kunnen meerdere clients verbinding maken met Azure Stack op hetzelfde moment. 

> [!NOTE] 
> Deze VPN-verbinding biedt geen verbinding met Azure Stack-infrastructuur virtuele machines. 

### <a name="prerequisites"></a>Vereisten

* Installeer [Azure Stack compatibele Azure PowerShell](azure-stack-powershell-install.md) op uw lokale computer.  
* In het [Azure Stack development Kit, worden de blobEndpoint](azure-stack-powershell-download.md) . 

### <a name="configure-vpn-connectivity"></a>VPN-connectiviteit configureren

Voor het maken van een VPN-verbinding met de development kit, een PowerShell-sessie met verhoogde bevoegdheden openen vanuit uw lokale Windows-computer en voer het volgende script (Zorg ervoor dat de waarden voor IP-adres en het wachtwoord voor uw omgeving):

```PowerShell 
# Configure winrm if it's not already configured
winrm quickconfig  

Set-ExecutionPolicy RemoteSigned

# Import the Connect module
Import-Module .\Connect\AzureStack.Connect.psm1 

# Add the development kit computer’s host IP address & certificate authority (CA) to the list of trusted hosts. Make sure to update the IP address and password values for your environment. 

$hostIP = "<Azure Stack host IP address>"

$Password = ConvertTo-SecureString `
  "<Administrator password provided when deploying Azure Stack>" `
  -AsPlainText `
  -Force

Set-Item wsman:\localhost\Client\TrustedHosts `
  -Value $hostIP `
  -Concatenate

# Create a VPN connection entry for the local user
Add-AzsVpnConnection `
  -ServerAddress $hostIP `
  -Password $Password

```

Als de installatie is geslaagd, ziet u **azurestack** in uw lijst met VPN-verbindingen.

![Netwerkverbindingen](media/azure-stack-connect-azure-stack/image3.png)  

### <a name="connect-to-azure-stack"></a>Verbinding maken met Azure Stack

Maak verbinding met de Azure Stack-exemplaar met behulp van een van de volgende twee methoden:  

* Met behulp van de `Connect-AzsVpn ` opdracht: 
    
  ```PowerShell
  Connect-AzsVpn `
    -Password $Password
  ```

  Wanneer u hierom wordt gevraagd, vertrouwt de Azure Stack-host en installeert u het certificaat van **AzureStackCertificateAuthority** naar het certificaatarchief van de lokale computer. (de prompt mogelijk achter de PowerShell-sessie-venster weergegeven). 

* Open uw lokale computer **netwerkinstellingen** > **VPN** > klikt u op **azurestack** > **verbinding**. Voer de gebruikersnaam (AzureStack\AzureStackAdmin) en het wachtwoord bij de prompt aanmelden.

### <a name="test-the-vpn-connectivity"></a>De VPN-verbinding testen

De portal om verbinding te testen, open een internetbrowser en navigeer naar de gebruikersportal (https://portal.local.azurestack.external/), meld u aan en resources te maken.  

## <a name="next-steps"></a>Volgende stappen




---
title: Verbinding maken met Azure Stack | Microsoft Docs
description: Informatie over het aansluiten van Azure-Stack
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
ms.date: 08/22/2017
ms.author: mabrigg
ms.openlocfilehash: 7479202a8afabf5a84560691a2bccf849206c077
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="connect-to-azure-stack"></a>Verbinding maken met Azure Stack

Voor het beheren van resources, moet u verbinding met de Azure-Stack Development Kit. De details van dit artikel de stappen die worden vereist om verbinding maken met de development kit. U kunt een van de volgende verbindingsopties:

* [Extern bureaublad](#connect-with-remote-desktop): Hiermee kunt u meerdere gelijktijdige gebruikers snel verbinding te maken van de development kit.
* [Virtuele particuliere netwerk (VPN)](#connect-with-vpn): kunnen meerdere gelijktijdige gebruikers verbinding van clients buiten de Azure-Stack-infrastructuur maken (configuratie vereist).

## <a name="connect-to-azure-stack-with-remote-desktop"></a>Verbinding maken met Azure Stack met extern bureaublad
Met een verbinding met extern bureaublad kunt meerdere gelijktijdige gebruikers werken met de portal om resources te beheren.

1. Een verbinding met extern bureaublad openen en verbinding maken met de development kit. Voer **AzureStack\AzureStackAdmin** als de gebruikersnaam en het beheerderswachtwoord die u hebt opgegeven tijdens de installatie van de Azure-Stack.  

2. Vanaf de computer van de kit ontwikkeling, open Serverbeheer, klikt u op **lokale Server**, verbeterde beveiliging van Internet Explorer uitschakelen en sluit u Server Manager.

3. De portal openen, gaat u naar (https://portal.local.azurestack.external/) en meld u aan met de referenties van gebruiker.


## <a name="connect-to-azure-stack-with-vpn"></a>Verbinding maken met Azure Stack met VPN

U kunt een gesplitste tunnel virtuele particuliere netwerk (VPN)-verbinding met een Azure-Stack Development Kit maken. Via de VPN-verbinding, moet u toegang tot de beheerdersportal, gebruikersportal, en lokaal geïnstalleerde hulpprogramma's, zoals Visual Studio en PowerShell om Azure-Stack-resources te beheren. VPN-verbinding wordt ondersteund in beide Active Directory(AAD) Azure en Active Directory Federation Services(AD FS) gebaseerde implementaties. VPN-verbindingen kunnen meerdere clients verbinding maken met Azure-Stack op hetzelfde moment. 

> [!NOTE] 
> Deze VPN-verbinding biedt geen verbinding met Azure-Stack infrastructuur virtuele machines. 

### <a name="prerequisites"></a>Vereisten

* Installeer [Azure Stack compatibele Azure PowerShell](azure-stack-powershell-install.md) op uw lokale computer.  
* Download de [hulpprogramma's voor het werken met Azure-Stack](azure-stack-powershell-download.md). 

### <a name="configure-vpn-connectivity"></a>VPN-connectiviteit configureren

Een VPN-verbinding met de development kit maken, een PowerShell-sessie met verhoogde bevoegdheid openen vanuit uw lokale Windows-computer en voer het volgende script (Zorg ervoor dat de waarden voor IP-adres en het wachtwoord voor uw omgeving):

```PowerShell 
# Configure winrm if it's not already configured
winrm quickconfig  

Set-ExecutionPolicy RemoteSigned

# Import the Connect module
Import-Module .\Connect\AzureStack.Connect.psm1 

# Add the development kit computer’s host IP address & certificate authority (CA) to the list of trusted hosts. Make sure to update the the IP address and password values for your environment. 

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

Verbinding maken met de Azure-Stack-exemplaar met een van de volgende twee methoden:  

* Met behulp van de `Connect-AzsVpn ` opdracht: 
    
  ```PowerShell
  Connect-AzsVpn `
    -Password $Password
  ```

  Wanneer u wordt gevraagd, vertrouwt de Azure-Stack-host en installeer het certificaat van **AzureStackCertificateAuthority** naar het certificaatarchief van de lokale computer. (de prompt mogelijk achter de PowerShell-sessie-venster weergegeven). 

* Open uw lokale computer **netwerkinstellingen** > **VPN** > klikt u op **azurestack** > **verbinding**. Voer de gebruikersnaam (AzureStack\AzureStackAdmin) en het wachtwoord bij de prompt aanmelden.

### <a name="test-the-vpn-connectivity"></a>De VPN-verbindingen testen

De portal om verbinding te testen, open een internetbrowser en navigeer naar de gebruikersportal (https://portal.local.azurestack.external/), aanmelden en resources maken.  

## <a name="next-steps"></a>Volgende stappen




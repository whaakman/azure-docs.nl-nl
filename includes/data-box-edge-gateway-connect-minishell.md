---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/06/2019
ms.author: alkohli
ms.openlocfilehash: 348f7bdd333da4f4a6cb41a438b7aee08d6a6bbb
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554950"
---
De procedures voor het op afstand verbinding maken met het apparaat zijn afhankelijk van het besturingssysteem van de client, anders.

### <a name="remotely-connect-from-a-windows-client"></a>Extern verbinding te maken van een Windows-client

Voordat u begint, zorg ervoor dat uw Windows-client wordt uitgevoerd met Windows PowerShell 5.0 of hoger.

Volg deze stappen om extern verbinding te maken van een Windows-client.

1. Een Windows PowerShell-sessie als administrator uitvoeren.
2. Zorg ervoor dat de Windows Remote Management-service wordt uitgevoerd op de client. Typ in de opdrachtprompt:

    `winrm quickconfig`

3. Een variabele toewijzen aan IP-adres van het apparaat.

    $ip = "<device_ip>"

    Vervang `<device_ip>` met het IP-adres van uw apparaat.

4. Het IP-adres van uw apparaat toevoegen aan lijst met vertrouwde hosts van de client, typ de volgende opdracht:

    `Set-Item WSMan:\localhost\Client\TrustedHosts $ip -Concatenate -Force`

5. Een Windows PowerShell-sessie starten op het apparaat:

    `Enter-PSSession -ComputerName $ip -Credential $ip\EdgeUser -ConfigurationName Minishell`

6. Geef het wachtwoord wanneer hierom wordt gevraagd. Gebruik hetzelfde wachtwoord dat wordt gebruikt voor aanmelding bij de lokale webgebruikersinterface. Het wachtwoord van de lokale web-UI voor de standaard is *Wachtwoord1*. Als u bent met het apparaat met externe PowerShell verbonden, ziet u de volgende voorbeelduitvoer:  

    ```
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.
    
    PS C:\WINDOWS\system32> winrm quickconfig
    WinRM service is already running on this machine.
    PS C:\WINDOWS\system32> $ip = "10.100.10.10"
    PS C:\WINDOWS\system32> Set-Item WSMan:\localhost\Client\TrustedHosts $ip -Concatenate -Force
    PS C:\WINDOWS\system32> Enter-PSSession -ComputerName $ip -Credential $ip\EdgeUser -ConfigurationName Minishell

    WARNING: The Windows PowerShell interface of your device is intended to be used only for the initial network configuration. Please engage Microsoft Support if you need to access this interface to troubleshoot any potential issues you may be experiencing. Changes made through this interface without involving Microsoft Support could result in an unsupported configuration.
    [10.100.10.10]: PS>
    ```

### <a name="remotely-connect-from-a-linux-client"></a>Extern verbinding te maken van een Linux-client

Op de Linux-client die u gebruiken gaat om verbinding te maken:

- [Installeer de meest recente PowerShell Core voor Linux](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-linux?view=powershell-6) van GitHub om op te halen van de SSH-functie voor externe toegang. 
- [Installeer alleen de `gss-ntlmssp` pakket van de NTLM-module](https://github.com/Microsoft/omi/blob/master/Unix/doc/setup-ntlm-omi.md). Voor Ubuntu-clients, gebruikt u de volgende opdracht uit:
    - `sudo apt-get install gss-ntlmssp`

Ga voor meer informatie naar [PowerShell voor externe toegang via SSH](https://docs.microsoft.com/powershell/scripting/learn/remoting/ssh-remoting-in-powershell-core?view=powershell-6).

Volg deze stappen om extern verbinding te maken van een NFS-client.

1. Als u wilt open PowerShell-sessie, typt u:

    `sudo pwsh`
 
2. Om verbinding te maken met behulp van de externe client, typt u:

    `Enter-PSSession -ComputerName $ip -Authentication Negotiate -ConfigurationName Minishell -Credential ~\EdgeUser`

    Geef desgevraagd het wachtwoord waarmee u zich aanmelden bij uw apparaat.
 
> [!NOTE]
> Deze procedure werkt niet op Mac OS.

---
title: Azure VM-Gast-OS-firewall is onjuist geconfigureerd | Microsoft Docs
description: ''
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: willchen
editor: ''
tags: ''
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.date: 11/22/2018
ms.author: delhan
ms.openlocfilehash: fcea5e4e6bb108f1a8d8036e51a5dae8a9e6431b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57839743"
---
# <a name="azure-vm-guest-os-firewall-is-misconfigured"></a>Azure VM-Gast OS firewall is onjuist geconfigureerd

In dit artikel over het oplossen van onjuist geconfigureerde gastbesturingssysteem system-firewall op de virtuele machine in Azure introduceren.

## <a name="symptoms"></a>Symptomen

1.  Het welkomstscherm van de virtuele machine (VM) ziet u dat de virtuele machine volledig geladen is.

2.  Afhankelijk van hoe de Gast-besturingssysteem is geconfigureerd, kunnen er bepaalde of geen van de bereiken van de virtuele machine netwerkverkeer.

## <a name="cause"></a>Oorzaak

Een onjuiste configuratie van de firewall van de Gast-systeem kan sommige of alle soorten netwerkverkeer naar de virtuele machine blokkeren.

## <a name="solution"></a>Oplossing

Voordat u deze stappen hebt uitgevoerd, maakt u een momentopname van de schijf van de betrokken virtuele machine als een back-up. Zie voor meer informatie, [momentopname maken van een schijf](../windows/snapshot-copy-managed-disk.md).

Om dit probleem wilt oplossen, gebruikt u de seriële Console of [herstel de virtuele machine offline](troubleshoot-rdp-internal-error.md#repair-the-vm-offline) door het koppelen van de schijf van de virtuele machine aan een virtuele machine voor herstel.

## <a name="online-mitigations"></a>Online oplossingen

Verbinding maken met de [seriële Console en open vervolgens een PowerShell-sessie](serial-console-windows.md#use-cmd-or-powershell-in-serial-console). Als de seriële Console is niet ingeschakeld op de virtuele machine, gaat u naar de sectie 'Herstel de virtuele machine Offline' van de volgende Azure-artikel:

 [Een interne fout treedt op wanneer u probeert verbinding maken met een Azure-VM via Extern bureaublad](troubleshoot-rdp-internal-error.md#repair-the-vm-offline)

De volgende regels kunnen worden bewerkt voor de toegang tot de virtuele machine (via RDP) of om een makkelijker troubleshooting ervaring te bieden:

*   Extern bureaublad (TCP-In): Dit is de standaard-regel die de primaire toegang tot de virtuele machine biedt door toe te staan van RDP in Azure.

*   Windows Remote Management (HTTP-In): Deze regel kunt u verbinding maken met de virtuele machine met behulp van PowerShell., In Azure, dit soort toegang kunt u gebruik van het uitvoeren van scripts aspect van externe scripts en probleemoplossing.

*   Bestands- en printerdeling (SMB-In): Deze regel kunnen de toegang tot het netwerk delen als een optie voor het oplossen van problemen.

*   Bestands- en printerdeling (Echoaanvraag - ICMPv4-In): Deze regel kunt u de virtuele machine te pingen.

U kunt de huidige status van de firewallregel opvragen in de toegang tot de seriële Console-exemplaar.

*   Query's uitvoeren met behulp van de weergegeven naam als een parameter:

    ```cmd
    netsh advfirewall firewall show rule dir=in name=all | select-string -pattern "(DisplayName.*<FIREWALL RULE NAME>)" -context 9,4 | more
    ```

*   Query's uitvoeren met behulp van de lokale poort die gebruikmaakt van de toepassing:

    ```cmd
    netsh advfirewall firewall show rule dir=in name=all | select-string -pattern "(LocalPort.*<APPLICATION PORT>)" -context 9,4 | more
    ```

*   Query's uitvoeren met behulp van het lokale IP-adres die gebruikmaakt van de toepassing:

    ```cmd
    netsh advfirewall firewall show rule dir=in name=all | select-string -pattern "(LocalIP.*<CUSTOM IP>)" -context 9,4 | more
    ```

*   Als u ziet dat de regel is uitgeschakeld, kunt u deze inschakelen door het uitvoeren van de volgende opdracht uit:

    ```cmd
    netsh advfirewall firewall set rule name="<RULE NAME>" new enable=yes
    ```

*   Voor het oplossen van problemen, kunt u de firewallprofielen uitschakelen:

    ```cmd
    netsh advfirewall set allprofiles state off
    ```

    Als u dit om in te stellen de firewall correct doet, opnieuw de firewall inschakelen nadat u klaar bent met het oplossen van problemen.

    > [!Note]
    > U hebt geen opnieuw opstarten van de virtuele machine als deze wijziging wilt toepassen.

*   Probeer het opnieuw verbinding maken met de virtuele machine via RDP.

### <a name="offline-mitigations"></a>Offline oplossingen

1.  Als u wilt in- of uitschakelen van firewall-regels, verwijzen naar [in- of uitschakelen van een firewallregel in een Azure VM-Gastbesturingssysteem](enable-disable-firewall-rule-guest-os.md).

2.  Controleer of u zich in de [Guest OS firewall geblokkeerd binnenkomend verkeer scenario](guest-os-firewall-blocking-inbound-traffic.md).

3.  Als u nog steeds niet zeker weet of de firewall de toegang blokkeert, verwijzen naar [uitschakelen van de Gast OS-Firewall in Azure VM](disable-guest-os-firewall-windows.md), en vervolgens de firewall van de Gast-systeem met behulp van de juiste regels weer inschakelen.


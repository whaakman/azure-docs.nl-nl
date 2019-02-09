---
title: Kan geen verbinding maken met RDP naar een Windows-VM in Azure | Microsoft Docs
description: Oplossen van problemen wanneer u geen verbinding maken met uw Windows virtuele machine in Azure met behulp van extern bureaublad
keywords: 'Fout bij extern bureaublad, verbinding met extern bureaublad-fout: kan geen verbinding maken met virtuele machine, extern bureaublad oplossen'
services: virtual-machines-windows
documentationcenter: ''
author: roiyz-msft
manager: jeconnoc
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: 0d740f8e-98b8-4e55-bb02-520f604f5b18
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 03/23/2018
ms.author: roiyz
ms.openlocfilehash: a4fb31721da679b21fa311340269cf07f93cd903
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55981261"
---
# <a name="troubleshoot-remote-desktop-connections-to-an-azure-virtual-machine"></a>Problemen oplossen met extern bureaublad-verbindingen met een Azure-machine
De Remote Desktop Protocol (RDP)-verbinding met uw op basis van Windows Azure-machine (VM) kan mislukken voor verschillende oorzaken hebben, zodat u geen toegang tot uw virtuele machine. Het probleem is met de extern bureaublad-service op de virtuele machine, de netwerkverbinding of de extern bureaublad-client op de hostcomputer. In dit artikel begeleidt u bij sommige van de meest voorkomende methoden voor het oplossen van problemen met RDP-verbinding. 

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op [het Azure MSDN en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

<a id="quickfixrdp"></a>

## <a name="quick-troubleshooting-steps"></a>Snelle stappen voor probleemoplossing
Probeer opnieuw verbinding maken met de virtuele machine na elke stap:

1. Extern bureaublad-configuratie opnieuw instellen.
2. Controleer Network Security Group regels / Cloud Services-eindpunten.
3. Bekijk de logboeken van de VM-console.
4. De NIC voor de virtuele machine opnieuw instellen.
5. Controleer de status van de VM-Resource.
6. Uw VM-wachtwoord opnieuw instellen.
7. Start opnieuw op uw virtuele machine.
8. Uw virtuele machine opnieuw implementeren.

Lees verder als u meer gedetailleerde stappen en uitleg nodig hebt. Controleer of deze lokale netwerkapparatuur zoals routers en uitgaande TCP-poort 3389, worden niet door firewalls worden geblokkeerd, zoals vermeld [gedetailleerde RDP probleemoplossende scenario's](detailed-troubleshoot-rdp.md).

> [!TIP]
> Als de **Connect** knop voor uw virtuele machine wordt grijs weergegeven in de portal en u niet met Azure via verbonden bent een [Express Route](../../expressroute/expressroute-introduction.md) of [Site-naar-Site VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) verbinding, moet u maken en de virtuele machine een openbare IP-adres toewijzen voordat u extern bureaublad kunt gebruiken. Er is meer informatie over [openbare IP-adressen in Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) beschikbaar.


## <a name="ways-to-troubleshoot-rdp-issues"></a>Manieren om problemen met RDP
U kunt virtuele machines die zijn gemaakt met behulp van de Resource Manager-implementatiemodel met behulp van een van de volgende manieren oplossen:

* Azure portal - handig als u wilt snel opnieuw instellen van de RDP-configuratie of de gebruiker de referenties en u hebt de Azure-hulpprogramma's geïnstalleerd.
* Azure PowerShell - als u vertrouwd met een PowerShell-prompt bent snel opnieuw instellen van de RDP-configuratie of de gebruiker referenties met behulp van de Azure PowerShell-cmdlets.

U vindt hier ook stappen over het oplossen van virtuele machines die zijn gemaakt met behulp van de [klassieke implementatiemodel](#troubleshoot-vms-created-using-the-classic-deployment-model).

<a id="fix-common-remote-desktop-errors"></a>

## <a name="troubleshoot-using-the-azure-portal"></a>Problemen oplossen met behulp van de Azure-portal
Probeer opnieuw verbinding te maken met uw virtuele machine na elke stap. Als u nog steeds geen verbinding maken, probeert u de volgende stap.

1. **Stel de RDP-verbinding opnieuw**. Deze stap voor het oplossen van problemen Hiermee stelt u de RDP-configuratie als externe verbindingen zijn uitgeschakeld of Windows Firewall-regels RDP, bijvoorbeeld blokkeren.
   
    Selecteer de virtuele machine in Azure portal. Schuif omlaag in het deelvenster instellingen naar de **ondersteuning + probleemoplossing** sectie aan de onderkant van de lijst. Klik op de **wachtwoord opnieuw instellen** knop. Stel de **modus** naar **alleen configuratie opnieuw instellen van** en klik vervolgens op de **Update** knop:
   
    ![De RDP-configuratie in Azure portal opnieuw instellen](./media/troubleshoot-rdp-connection/reset-rdp.png)
2. **Controleer of netwerkbeveiligingsgroepsregels**. Gebruik [IP-stroom controleren](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md) om te bevestigen of verkeer naar of van een virtuele machine wordt geblokkeerd door een regel in een netwerkbeveiligingsgroep. U kunt ook bekijken beveiligingsgroepsregels om ervoor te zorgen inkomende 'toestaan' NSG-regel bestaat en met prioriteit wordt toegepast voor RDP-poort (standaard 3389). Zie voor meer informatie, [effectieve beveiligingsregels om op te lossen van de virtuele machine met behulp van de verkeersstroom](../../virtual-network/diagnose-network-traffic-filter-problem.md).

3. **VM-diagnostische gegevens over opstarten bekijken**. Deze stap controleert de logboeken van de VM-console om te bepalen of de virtuele machine een probleem is rapporteert. Niet alle virtuele machines hebben diagnostische gegevens over opstarten ingeschakeld, zodat deze stap is niet verplicht.
   
    Specifieke stappen voor probleemoplossing zijn buiten het bereik van dit artikel, maar kunnen duiden op een breder probleem die invloed heeft op RDP-connectiviteit. Zie voor meer informatie over het controleren van de consolelogboeken en de VM-schermopname [diagnostische gegevens over opstarten voor virtuele machines](boot-diagnostics.md).

4. **Opnieuw instellen van de NIC voor de virtuele machine**. Zie voor meer informatie, [opnieuw instellen van NIC voor Azure Windows VM](../windows/reset-network-interface.md).
5. **Controleer de resourcestatus van virtuele machine**. Deze stap controleert u of er zijn geen bekende problemen met de Azure-platform die mogelijk van invloed op de verbinding met de virtuele machine.
   
    Selecteer de virtuele machine in Azure portal. Schuif omlaag in het deelvenster instellingen naar de **ondersteuning + probleemoplossing** sectie aan de onderkant van de lijst. Klik op de **resourcestatus** knop. Een gezonde VM rapporteert als zijnde **beschikbaar**:
   
    ![Controleer de resourcestatus van virtuele machine in Azure portal](./media/troubleshoot-rdp-connection/check-resource-health.png)
6. **Opnieuw instellen van gebruikersreferenties**. Deze stap stelt het wachtwoord op een lokale administrator-account wanneer u niet zeker weet of de referenties zijn vergeten.  Nadat u de virtuele machine hebt aangemeld, moet u het wachtwoord voor die gebruiker opnieuw instellen.
   
    Selecteer de virtuele machine in Azure portal. Schuif omlaag in het deelvenster instellingen naar de **ondersteuning + probleemoplossing** sectie aan de onderkant van de lijst. Klik op de **wachtwoord opnieuw instellen** knop. Zorg ervoor dat de **modus** is ingesteld op **wachtwoord opnieuw instellen** en voer uw gebruikersnaam en een nieuw wachtwoord. Ten slotte klikt u op de **Update** knop:
   
    ![De referenties van de gebruiker in Azure portal opnieuw instellen](./media/troubleshoot-rdp-connection/reset-password.png)
7. **Start de VM opnieuw**. Deze stap voor het oplossen van problemen kunt corrigeren eventuele onderliggende problemen de virtuele machine zelf heeft.
   
    Selecteer de virtuele machine in Azure portal en klikt u op de **overzicht** tabblad. Klik op de **opnieuw** knop:
   
    ![Opnieuw opstarten van de virtuele machine in Azure portal](./media/troubleshoot-rdp-connection/restart-vm.png)
8. **Uw virtuele machine opnieuw implementeren**. Deze stap implementeert u uw virtuele machine naar een andere host binnen Azure om op te lossen van eventuele onderliggende platform of netwerkproblemen opnieuw.
   
    Selecteer de virtuele machine in Azure portal. Schuif omlaag in het deelvenster instellingen naar de **ondersteuning + probleemoplossing** sectie aan de onderkant van de lijst. Klik op de **opnieuw implementeren** knop en klik vervolgens op **opnieuw implementeren**:
   
    ![De virtuele machine in Azure portal opnieuw implementeren](./media/troubleshoot-rdp-connection/redeploy-vm.png)
   
    Nadat deze is voltooid, schijfgegevens van de tijdelijke verloren zijn gegaan en dynamische IP-adressen die gekoppeld aan de virtuele machine zijn worden bijgewerkt.

9. **Controleer de routering**. Gebruik van Network Watcher van [van volgende hop](../../network-watcher/network-watcher-check-next-hop-portal.md) mogelijkheid om te bevestigen dat een route is niet zo wordt voorkomen verkeer dat worden gerouteerd naar of van een virtuele machine. U kunt ook effectieve routes om te zien van alle effectieve routes voor een netwerkinterface bekijken. Zie voor meer informatie, [effectieve routes gebruiken om op te lossen VM verkeersstroom](../../virtual-network/diagnose-network-routing-problem.md).

10. Zorg ervoor dat een on-premises firewall of een firewall op uw computer uitgaand TCP 3389-verkeer naar Azure kunt.

Als u nog steeds problemen met RDP regelmatig, kunt u [een ondersteuningsaanvraag](https://azure.microsoft.com/support/options/) of Lees [meer gedetailleerde RDP-problemen oplossen met concepten en stappen](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-using-azure-powershell"></a>Problemen oplossen met behulp van Azure PowerShell
Als u dat nog niet gedaan hebt, [installeren en configureren van de nieuwste Azure PowerShell](/powershell/azure/overview).

De volgende voorbeelden gebruiken de variabelen zoals `myResourceGroup`, `myVM`, en `myVMAccessExtension`. Vervang deze namen en locaties door uw eigen waarden.

> [!NOTE]
> U referenties van de gebruiker en de RDP-configuratie herstellen met behulp van de [Set AzVMAccessExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmaccessextension) PowerShell-cmdlet. In de volgende voorbeelden `myVMAccessExtension` is een naam die u als onderdeel van het proces opgeeft. Als u eerder met de VMAccessAgent hebt gewerkt, kunt u de naam van de bestaande extensie met behulp van `Get-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"` om te controleren of de eigenschappen van de virtuele machine. Als u de naam, kijkt u bij de sectie 'Extensies' van de uitvoer.

Probeer opnieuw verbinding te maken met uw virtuele machine na elke stap. Als u nog steeds geen verbinding maken, probeert u de volgende stap.

1. **Stel de RDP-verbinding opnieuw**. Deze stap voor het oplossen van problemen Hiermee stelt u de RDP-configuratie als externe verbindingen zijn uitgeschakeld of Windows Firewall-regels RDP, bijvoorbeeld blokkeren.
   
    In het volgende voorbeeld wordt de RDP-verbinding op een virtuele machine met de naam `myVM` in de `WestUS` locatie en in de resourcegroep met de naam `myResourceGroup`:
   
    ```powershell
    Set-AzVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location Westus -Name "myVMAccessExtension"
    ```
2. **Controleer of netwerkbeveiligingsgroepsregels**. Deze stap controleert u of dat er een regel in de Netwerkbeveiligingsgroep voor het RDP-verkeer toestaan. De standaardpoort voor RDP is TCP-poort 3389. Een regel om RDP-verkeer toe te kan niet automatisch worden gemaakt wanneer u uw virtuele machine maakt.
   
    Eerst alle configuratiegegevens toewijzen voor uw Netwerkbeveiligingsgroep aan de `$rules` variabele. Het volgende voorbeeld wordt informatie over de Netwerkbeveiligingsgroep met de naam `myNetworkSecurityGroup` in de resourcegroep met de naam `myResourceGroup`:
   
    ```powershell
    $rules = Get-AzNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
        -Name "myNetworkSecurityGroup"
    ```
   
    Bekijk nu de regels die zijn geconfigureerd voor deze Netwerkbeveiligingsgroep. Controleer of een regel bestaat om toe te staan van TCP-poort 3389 voor binnenkomende verbindingen als volgt:
   
    ```powershell
    $rules.SecurityRules
    ```
   
    Het volgende voorbeeld ziet een geldige beveiligingsregel om de RDP-verkeer toestaat. U kunt zien `Protocol`, `DestinationPortRange`, `Access`, en `Direction` correct zijn geconfigureerd:
   
    ```powershell
    Name                     : default-allow-rdp
    Id                       : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/default-allow-rdp
    Etag                     : 
    ProvisioningState        : Succeeded
    Description              : 
    Protocol                 : TCP
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : *
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 1000
    Direction                : Inbound
    ```
   
    Als u nog geen een regel waarmee de RDP-verkeer, [maakt u een regel voor Netwerkbeveiligingsgroep](../windows/nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Toestaan dat TCP-poort 3389.
3. **Opnieuw instellen van gebruikersreferenties**. Deze stap voor het oplossen van problemen Hiermee stelt u het wachtwoord op het lokale administrator-account die u opgeeft wanneer u niet zeker weet of u bent vergeten, de referenties.
   
    Eerst, de gebruikersnaam en een nieuw wachtwoord opgeven door toe te wijzen referenties voor de `$cred` variabele als volgt te werk:
   
    ```powershell
    $cred=Get-Credential
    ```
   
    De referenties op de virtuele machine nu bijwerken. Het volgende voorbeeld worden de referenties op een virtuele machine met de naam bijgewerkt `myVM` in de `WestUS` locatie en in de resourcegroep met de naam `myResourceGroup`:
   
    ```powershell
    Set-AzVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location WestUS -Name "myVMAccessExtension" `
        -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password
    ```
4. **Start de VM opnieuw**. Deze stap voor het oplossen van problemen kunt corrigeren eventuele onderliggende problemen de virtuele machine zelf heeft.
   
    Het volgende voorbeeld wordt opnieuw opgestart van de virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`:
   
    ```powershell
    Restart-AzVM -ResourceGroup "myResourceGroup" -Name "myVM"
    ```
5. **Uw virtuele machine opnieuw implementeren**. Deze stap implementeert u uw virtuele machine naar een andere host binnen Azure om op te lossen van eventuele onderliggende platform of netwerkproblemen opnieuw.
   
    Het volgende voorbeeld implementeert de virtuele machine met de naam opnieuw `myVM` in de `WestUS` locatie en in de resourcegroep met de naam `myResourceGroup`:
   
    ```powershell
    Set-AzVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

6. **Controleer de routering**. Gebruik van Network Watcher van [van volgende hop](../../network-watcher/network-watcher-check-next-hop-portal.md) mogelijkheid om te bevestigen dat een route is niet zo wordt voorkomen verkeer dat worden gerouteerd naar of van een virtuele machine. U kunt ook effectieve routes om te zien van alle effectieve routes voor een netwerkinterface bekijken. Zie voor meer informatie, [effectieve routes gebruiken om op te lossen VM verkeersstroom](../../virtual-network/diagnose-network-routing-problem.md).

7. Zorg ervoor dat een on-premises firewall of een firewall op uw computer uitgaand TCP 3389-verkeer naar Azure kunt.

Als u nog steeds problemen met RDP regelmatig, kunt u [een ondersteuningsaanvraag](https://azure.microsoft.com/support/options/) of Lees [meer gedetailleerde RDP-problemen oplossen met concepten en stappen](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-vms-created-using-the-classic-deployment-model"></a>Oplossen van virtuele machines die zijn gemaakt met het klassieke implementatiemodel
Probeer opnieuw verbinding maken met de virtuele machine na elke stap.

1. **Stel de RDP-verbinding opnieuw**. Deze stap voor het oplossen van problemen Hiermee stelt u de RDP-configuratie als externe verbindingen zijn uitgeschakeld of Windows Firewall-regels RDP, bijvoorbeeld blokkeren.
   
    Selecteer de virtuele machine in Azure portal. Klik op de **... Meer** knop en klik vervolgens op **externe toegang opnieuw instellen**:
   
    ![De RDP-configuratie in Azure portal opnieuw instellen](./media/troubleshoot-rdp-connection/classic-reset-rdp.png)
2. **Controleer of Cloud Services-eindpunten**. Deze stap controleert u of dat u hebt de eindpunten in uw Cloud-Services toe te staan van RDP-verkeer. De standaardpoort voor RDP is TCP-poort 3389. Een regel om RDP-verkeer toe te kan niet automatisch worden gemaakt wanneer u uw virtuele machine maakt.
   
   Selecteer de virtuele machine in Azure portal. Klik op de **eindpunten** knop om de eindpunten die momenteel zijn geconfigureerd voor uw virtuele machine weer te geven. Controleer of de eindpunten die RDP-verkeer op TCP-poort 3389 bestaan.
   
   Het volgende voorbeeld ziet u geldige eindpunten die RDP-verkeer toestaan:
   
   ![Controleer of Cloud Services-eindpunten in Azure portal](./media/troubleshoot-rdp-connection/classic-verify-cloud-services-endpoints.png)
   
   Als u nog geen een eindpunt waarmee RDP-verkeer, [maken van een Cloud Services-eindpunt](../windows/classic/setup-endpoints.md). Kan TCP particuliere poort 3389.
3. **VM-diagnostische gegevens over opstarten bekijken**. Deze stap controleert de logboeken van de VM-console om te bepalen of de virtuele machine een probleem is rapporteert. Niet alle virtuele machines hebben diagnostische gegevens over opstarten ingeschakeld, zodat deze stap is niet verplicht.
   
    Specifieke stappen voor probleemoplossing zijn buiten het bereik van dit artikel, maar kunnen duiden op een breder probleem die invloed heeft op RDP-connectiviteit. Zie voor meer informatie over het controleren van de consolelogboeken en de VM-schermopname [diagnostische gegevens over opstarten voor virtuele machines](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).
4. **Controleer de resourcestatus van virtuele machine**. Deze stap controleert u of er zijn geen bekende problemen met de Azure-platform die mogelijk van invloed op de verbinding met de virtuele machine.
   
    Selecteer de virtuele machine in Azure portal. Schuif omlaag in het deelvenster instellingen naar de **ondersteuning + probleemoplossing** sectie aan de onderkant van de lijst. Klik op de **Resource Health** knop. Een gezonde VM rapporteert als zijnde **beschikbaar**:
   
    ![Controleer de resourcestatus van virtuele machine in Azure portal](./media/troubleshoot-rdp-connection/classic-check-resource-health.png)
5. **Opnieuw instellen van gebruikersreferenties**. Deze stap voor het oplossen van problemen Hiermee stelt u het wachtwoord op het lokale administrator-account die u opgeeft wanneer u niet zeker weet of de referenties bent vergeten.  Nadat u de virtuele machine hebt aangemeld, moet u het wachtwoord voor die gebruiker opnieuw instellen.
   
    Selecteer de virtuele machine in Azure portal. Schuif omlaag in het deelvenster instellingen naar de **ondersteuning + probleemoplossing** sectie aan de onderkant van de lijst. Klik op de **wachtwoord opnieuw instellen** knop. Voer uw gebruikersnaam en een nieuw wachtwoord. Ten slotte klikt u op de **opslaan** knop:
   
    ![De referenties van de gebruiker in Azure portal opnieuw instellen](./media/troubleshoot-rdp-connection/classic-reset-password.png)
6. **Start de VM opnieuw**. Deze stap voor het oplossen van problemen kunt corrigeren eventuele onderliggende problemen de virtuele machine zelf heeft.
   
    Selecteer de virtuele machine in Azure portal en klikt u op de **overzicht** tabblad. Klik op de **opnieuw** knop:
   
    ![Opnieuw opstarten van de virtuele machine in Azure portal](./media/troubleshoot-rdp-connection/classic-restart-vm.png)

7. Zorg ervoor dat een on-premises firewall of een firewall op uw computer uitgaand TCP 3389-verkeer naar Azure kunt.

Als u nog steeds problemen met RDP regelmatig, kunt u [een ondersteuningsaanvraag](https://azure.microsoft.com/support/options/) of Lees [meer gedetailleerde RDP-problemen oplossen met concepten en stappen](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-specific-rdp-errors"></a>Specifieke RDP-fouten oplossen
Een specifiek foutbericht kunnen optreden tijdens het verbinden met uw virtuele machine via RDP. Hier volgen de meest voorkomende foutberichten:

* [De externe sessie is beëindigd omdat er geen extern bureaublad-licentieservers beschikbaar, voor een licentie](troubleshoot-specific-rdp-errors.md#rdplicense).
* [Extern bureaublad kan de computer 'name' niet vinden](troubleshoot-specific-rdp-errors.md#rdpname).
* [Er is een verificatiefout opgetreden. Geen verbinding kan niet worden gemaakt met de lokale certificeringsinstantie](troubleshoot-specific-rdp-errors.md#rdpauth).
* [Fout bij het Windows-beveiliging: Uw referenties niet werkte](troubleshoot-specific-rdp-errors.md#wincred).
* [Deze computer kan geen verbinding maken met de externe computer](troubleshoot-specific-rdp-errors.md#rdpconnect).

## <a name="additional-resources"></a>Aanvullende resources
Als geen van deze fouten is opgetreden en u nog steeds geen verbinding met de virtuele machine via Extern bureaublad, leest u de gedetailleerde [probleemoplossingsgids voor extern bureaublad](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Zie voor stappen bij het openen van toepassingen die worden uitgevoerd op een virtuele machine voor probleemoplossing, [problemen oplossen met toegang tot een toepassing die wordt uitgevoerd op een Azure-VM](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Als u problemen met Secure Shell (SSH) verbinding maken met een Linux-VM in Azure, Zie [oplossen SSH-verbindingen met een Linux-VM in Azure](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).



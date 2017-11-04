---
title: Kan geen verbinding maken met RDP met een Windows-virtuele machine in Azure | Microsoft Docs
description: Problemen oplossen wanneer u geen verbinding maken met uw Windows virtuele machine in Azure met behulp van extern bureaublad
keywords: Extern bureaublad-fout, verbinding met extern bureaublad-fout, kan geen verbinding met virtuele machine, probleemoplossing extern bureaublad
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: 0d740f8e-98b8-4e55-bb02-520f604f5b18
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 74beeffe04fa4b6dba4db8687bdebe79e0d750f4
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="troubleshoot-remote-desktop-connections-to-an-azure-virtual-machine"></a>Extern bureaublad-verbindingen met een virtuele machine van Azure oplossen
De Remote Desktop Protocol (RDP)-verbinding met uw op basis van Windows Azure virtuele machine (VM) kan verschillende oorzaken hebben, zodat u geen toegang tot uw virtuele machine mislukken. Het probleem is met de extern bureaublad-service op de virtuele machine, de netwerkverbinding of de extern bureaublad-client op de hostcomputer. In dit artikel begeleidt u bij een aantal veelgebruikte methoden voor RDP-verbindingsproblemen oplossen. 

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op [MSDN Azure en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook een incident voor ondersteuning van Azure indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.

<a id="quickfixrdp"></a>

## <a name="quick-troubleshooting-steps"></a>Snelle stappen voor probleemoplossing
Probeer opnieuw verbinding te maken met de virtuele machine na elke stap voor probleemoplossing:

1. Extern bureaublad-configuratie opnieuw instellen.
2. Controleer de Netwerkbeveiligingsgroep regels / Cloud Services-eindpunten.
3. Bekijk de logboeken van de VM-console.
4. De NIC voor de virtuele machine opnieuw instellen.
5. Controleer de status van de VM-resources.
6. Uw VM-wachtwoord opnieuw instellen.
7. Opnieuw opstarten van de VM.
8. Implementeer uw virtuele machine opnieuw.

Blijven lezen als u meer gedetailleerde stappen en uitleg nodig. Controleer of deze lokale netwerkapparatuur zoals routers en firewalls worden niet geblokkeerd door uitgaande TCP-poort 3389, zoals aangegeven [RDP probleemoplossende scenario's beschreven](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!TIP]
> Als de **Connect** knop voor uw virtuele machine af grijs wordt weergegeven in de portal en u niet met Azure via verbonden bent een [Express Route](../../expressroute/expressroute-introduction.md) of [Site-naar-Site VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) verbinding, moet u maken en toewijzen van de VM een openbaar IP-adres maken voordat u RDP kunt gebruiken. Er is meer informatie over [openbare IP-adressen in Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) beschikbaar.


## <a name="ways-to-troubleshoot-rdp-issues"></a>Manieren om problemen met RDP
U kunt virtuele machines die zijn gemaakt met behulp van het implementatiemodel van Resource Manager met behulp van een van de volgende manieren oplossen:

* [Azure-portal](#using-the-azure-portal) - handig als u opnieuw wilt instellen snel de RDP-configuratie of gebruikersreferenties en u de Azure-hulpprogramma's geïnstalleerd hoeft.
* [Azure PowerShell](#using-azure-powershell) : als u vertrouwd met een PowerShell-prompt bent snel opnieuw instellen van de RDP-configuratie of gebruikersreferenties zijn met de Azure PowerShell-cmdlets.

U vindt ook stappen over het oplossen van virtuele machines die zijn gemaakt met behulp van de [klassieke implementatiemodel](#troubleshoot-vms-created-using-the-classic-deployment-model).

<a id="fix-common-remote-desktop-errors"></a>

## <a name="troubleshoot-using-the-azure-portal"></a>Problemen oplossen met de Azure-portal
Probeer opnieuw verbinding te maken met uw virtuele machine na elke stap voor probleemoplossing. Als u nog steeds geen verbinding maken, probeert u de volgende stap.

1. **Opnieuw instellen van uw RDP-verbinding**. Deze stap herstelt de RDP-configuratie als externe verbindingen zijn uitgeschakeld of als Windows Firewall-regels zijn RDP, bijvoorbeeld worden geblokkeerd.
   
    Selecteer uw virtuele machine in de Azure-portal. Schuif omlaag in het deelvenster instellingen naar de **ondersteuning + probleemoplossing** sectie aan de onderkant van de lijst. Klik op de **wachtwoord opnieuw instellen** knop. Stel de **modus** naar **alleen opnieuw instellen van configuratie** en klik vervolgens op de **Update** knop:
   
    ![De RDP-configuratie in de Azure portal opnieuw instellen](./media/troubleshoot-rdp-connection/reset-rdp.png)
2. **Controleer of de Netwerkbeveiligingsgroep regels**. Gebruik [IP-stroom controleren](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md) om te bevestigen of verkeer naar of van een virtuele machine wordt geblokkeerd door een regel in een netwerkbeveiligingsgroep. U kunt ook een effectieve beveiligingsmethode groep regels om ervoor te zorgen inkomende 'toestaan' NSG bekijken regel bestaat en is geplaatst voor RDP-poort (standaard 3389). Zie voor meer informatie [effectieve beveiligingsregels gebruiken om op te lossen VM verkeer stroom](../../virtual-network/virtual-network-nsg-troubleshoot-portal.md#using-effective-security-rules-to-troubleshoot-vm-traffic-flow).

3. **Bekijk VM boot diagnostics**. Deze stap controleert de logboeken van de VM-console om te bepalen of de virtuele machine van een probleem melden. Niet alle virtuele machines hebben diagnostische gegevens over opstarten is ingeschakeld, zodat het mogelijk is deze stap optioneel.
   
    Specifieke stappen voor probleemoplossing buiten het bereik van dit artikel, maar kunnen duiden op een breder probleem dat invloed op RDP-verbinding. Zie voor meer informatie over het weergeven van de console en de schermopname VM [diagnostische gegevens over opstarten voor virtuele machines](boot-diagnostics.md).

4. **Opnieuw instellen van de NIC voor de virtuele machine**. Zie voor meer informatie [NIC herstellen voor virtuele machine van Windows Azure](reset-network-interface.md).
5. **Controleer de status van de Resource VM**. Deze stap controleert u of er zijn geen bekende problemen met de Azure-platform die mogelijk van invloed op de verbinding met de virtuele machine.
   
    Selecteer uw virtuele machine in de Azure-portal. Schuif omlaag in het deelvenster instellingen naar de **ondersteuning + probleemoplossing** sectie aan de onderkant van de lijst. Klik op de **resourcestatus** knop. Een gezonde VM rapporten als **beschikbaar**:
   
    ![Controleer de status van de VM-resource in de Azure portal](./media/troubleshoot-rdp-connection/check-resource-health.png)
6. **Opnieuw instellen van gebruikersreferenties**. Deze stap wordt het wachtwoord op een lokale administrator-account opnieuw ingesteld wanneer u niet zeker weet of de referenties bent vergeten.
   
    Selecteer uw virtuele machine in de Azure-portal. Schuif omlaag in het deelvenster instellingen naar de **ondersteuning + probleemoplossing** sectie aan de onderkant van de lijst. Klik op de **wachtwoord opnieuw instellen** knop. Zorg ervoor dat de **modus** is ingesteld op **wachtwoord opnieuw instellen** en voer uw gebruikersnaam en een nieuw wachtwoord. Ten slotte op de **Update** knop:
   
    ![Opnieuw instellen van de referenties van de gebruiker in de Azure portal](./media/troubleshoot-rdp-connection/reset-password.png)
7. **Opnieuw opstarten van uw virtuele machine**. Deze stap kunt eventuele onderliggende problemen die de virtuele machine zelf heeft problemen te corrigeren.
   
    Selecteer uw virtuele machine in de Azure portal en klik op de **overzicht** tabblad. Klik op de **opnieuw** knop:
   
    ![Opnieuw opstarten van de virtuele machine in de Azure-portal](./media/troubleshoot-rdp-connection/restart-vm.png)
8. **Implementeer uw virtuele machine opnieuw**. Deze stap redeploys uw virtuele machine naar een andere host binnen Azure Corrigeer eventuele onderliggende platform of netwerkproblemen.
   
    Selecteer uw virtuele machine in de Azure-portal. Schuif omlaag in het deelvenster instellingen naar de **ondersteuning + probleemoplossing** sectie aan de onderkant van de lijst. Klik op de **implementeren** knop en klik vervolgens op **implementeren**:
   
    ![De virtuele machine in de Azure portal implementeren](./media/troubleshoot-rdp-connection/redeploy-vm.png)
   
    Nadat deze is voltooid, kortstondige schijfgegevens niet verloren en dynamische IP-adressen die gekoppeld aan de virtuele machine zijn worden bijgewerkt.

Als u nog steeds er RDP problemen zijn, kunt u [opent u een ondersteuningsaanvraag](https://azure.microsoft.com/support/options/) of lezen [meer gedetailleerde probleemoplossing concepten en stappen RDP](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-using-azure-powershell"></a>Problemen oplossen met Azure PowerShell
Als u dat nog niet gedaan hebt, [de meest recente Azure PowerShell installeren en configureren](/powershell/azure/overview).

Gebruik de volgende voorbeelden variabelen, zoals `myResourceGroup`, `myVM`, en `myVMAccessExtension`. Deze variabele bestandsnamen en locaties vervangen door uw eigen waarden.

> [!NOTE]
> U referenties van de gebruiker en de RDP-configuratie herstellen met behulp van de [Set AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-cmdlet. In de volgende voorbeelden `myVMAccessExtension` is een naam die u als onderdeel van het proces opgeeft. Als u de VMAccessAgent eerder hebt gewerkt, kunt u de naam van de bestaande extensie krijgen met behulp van `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` om te controleren van de eigenschappen van de virtuele machine. Als u wilt de naam, onder de sectie 'Extensions' van de uitvoer te zien.

Probeer opnieuw verbinding te maken met uw virtuele machine na elke stap voor probleemoplossing. Als u nog steeds geen verbinding maken, probeert u de volgende stap.

1. **Opnieuw instellen van uw RDP-verbinding**. Deze stap herstelt de RDP-configuratie als externe verbindingen zijn uitgeschakeld of als Windows Firewall-regels zijn RDP, bijvoorbeeld worden geblokkeerd.
   
    Hiermee stelt u in het volgende voorbeeld de RDP-verbinding op een virtuele machine met de naam `myVM` in de `WestUS` locatie en in de resourcegroep met de naam `myResourceGroup`:
   
    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location Westus -Name "myVMAccessExtension"
    ```
2. **Controleer of de Netwerkbeveiligingsgroep regels**. Deze stap controleert u of u een regel hebt in uw netwerk beveiligingsgroep toe te staan van RDP-verkeer. De standaardpoort voor RDP is TCP-poort 3389. Een regel toe te staan van RDP-verkeer kan niet automatisch gemaakt wanneer u uw virtuele machine maakt.
   
    Eerst toewijzen van de configuratiegegevens voor de Netwerkbeveiligingsgroep voor de `$rules` variabele. Het volgende voorbeeld krijgt u informatie over de Netwerkbeveiligingsgroep met de naam `myNetworkSecurityGroup` in de resourcegroep met de naam `myResourceGroup`:
   
    ```powershell
    $rules = Get-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
        -Name "myNetworkSecurityGroup"
    ```
   
    Bekijk nu de regels die zijn geconfigureerd voor deze Netwerkbeveiligingsgroep. Controleer of u een regel bestaat om toe te staan van TCP-poort 3389 voor binnenkomende verbindingen als volgt:
   
    ```powershell
    $rules.SecurityRules
    ```
   
    Het volgende voorbeeld ziet u een geldige beveiligings-regel waarmee RDP-verkeer is toegestaan. U kunt zien `Protocol`, `DestinationPortRange`, `Access`, en `Direction` correct zijn geconfigureerd:
   
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
   
    Als u een regel waarmee RDP-verkeer niet hebt [maakt u een regel Netwerkbeveiligingsgroep](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). TCP-poort 3389 toestaan.
3. **Opnieuw instellen van gebruikersreferenties**. Het wachtwoord op het lokale beheerdersaccount dat u opgeeft wanneer u niet zeker weet of bent vergeten, de referenties opnieuw instellen in deze stap.
   
    Eerst geeft u de gebruikersnaam en een nieuw wachtwoord door het toewijzen van referenties voor de `$cred` variabele als volgt:
   
    ```powershell
    $cred=Get-Credential
    ```
   
    De referenties op de virtuele machine nu bijwerken. Het volgende voorbeeld de referenties op een virtuele machine met de naam bijgewerkt `myVM` in de `WestUS` locatie en in de resourcegroep met de naam `myResourceGroup`:
   
    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location WestUS -Name "myVMAccessExtension" `
        -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password
    ```
4. **Opnieuw opstarten van uw virtuele machine**. Deze stap kunt eventuele onderliggende problemen die de virtuele machine zelf heeft problemen te corrigeren.
   
    Het volgende voorbeeld start de virtuele machine met de naam opnieuw `myVM` in de resourcegroep met de naam `myResourceGroup`:
   
    ```powershell
    Restart-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
    ```
5. **Implementeer uw virtuele machine opnieuw**. Deze stap redeploys uw virtuele machine naar een andere host binnen Azure Corrigeer eventuele onderliggende platform of netwerkproblemen.
   
    Het volgende voorbeeld de virtuele machine met de naam redeploys `myVM` in de `WestUS` locatie en in de resourcegroep met de naam `myResourceGroup`:
   
    ```powershell
    Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

Als u nog steeds er RDP problemen zijn, kunt u [opent u een ondersteuningsaanvraag](https://azure.microsoft.com/support/options/) of lezen [meer gedetailleerde probleemoplossing concepten en stappen RDP](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-vms-created-using-the-classic-deployment-model"></a>Problemen met virtuele machines die zijn gemaakt met behulp van het klassieke implementatiemodel
Probeer opnieuw verbinding te maken met de virtuele machine na elke stap.

1. **Opnieuw instellen van uw RDP-verbinding**. Deze stap herstelt de RDP-configuratie als externe verbindingen zijn uitgeschakeld of als Windows Firewall-regels zijn RDP, bijvoorbeeld worden geblokkeerd.
   
    Selecteer uw virtuele machine in de Azure-portal. Klik op de **... Meer** knop en klik vervolgens op **externe toegang opnieuw instellen**:
   
    ![De RDP-configuratie in de Azure portal opnieuw instellen](./media/troubleshoot-rdp-connection/classic-reset-rdp.png)
2. **Controleer of de Cloud Services-eindpunten**. Deze stap controleert u of u eindpunten hebt in uw Cloudservices toe te staan van RDP-verkeer. De standaardpoort voor RDP is TCP-poort 3389. Een regel toe te staan van RDP-verkeer kan niet automatisch gemaakt wanneer u uw virtuele machine maakt.
   
   Selecteer uw virtuele machine in de Azure-portal. Klik op de **eindpunten** knop om de eindpunten die momenteel geconfigureerd voor uw virtuele machine weer te geven. Controleer of de eindpunten die RDP-verkeer op TCP-poort 3389 toestaan bestaan.
   
   Het volgende voorbeeld ziet u geldige eindpunten die RDP-verkeer toestaan:
   
   ![Controleer of de Cloud Services-eindpunten in de Azure portal](./media/troubleshoot-rdp-connection/classic-verify-cloud-services-endpoints.png)
   
   Als u een eindpunt dat RDP-verkeer laat niet hebt [een Cloud Services-eindpunt maken](classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Laat de particuliere poort 3389 TCP.
3. **Bekijk VM boot diagnostics**. Deze stap controleert de logboeken van de VM-console om te bepalen of de virtuele machine van een probleem melden. Niet alle virtuele machines hebben diagnostische gegevens over opstarten is ingeschakeld, zodat het mogelijk is deze stap optioneel.
   
    Specifieke stappen voor probleemoplossing buiten het bereik van dit artikel, maar kunnen duiden op een breder probleem dat invloed op RDP-verbinding. Zie voor meer informatie over het weergeven van de console en de schermopname VM [diagnostische gegevens over opstarten voor virtuele machines](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).
4. **Controleer de status van de Resource VM**. Deze stap controleert u of er zijn geen bekende problemen met de Azure-platform die mogelijk van invloed op de verbinding met de virtuele machine.
   
    Selecteer uw virtuele machine in de Azure-portal. Schuif omlaag in het deelvenster instellingen naar de **ondersteuning + probleemoplossing** sectie aan de onderkant van de lijst. Klik op de **resourcestatus** knop. Een gezonde VM rapporten als **beschikbaar**:
   
    ![Controleer de status van de VM-resource in de Azure portal](./media/troubleshoot-rdp-connection/classic-check-resource-health.png)
5. **Opnieuw instellen van gebruikersreferenties**. Deze stap voor het oplossen van problemen Hiermee stelt u het wachtwoord op het lokale beheerdersaccount dat u opgeeft wanneer u niet zeker weet of de referenties bent vergeten.
   
    Selecteer uw virtuele machine in de Azure-portal. Schuif omlaag in het deelvenster instellingen naar de **ondersteuning + probleemoplossing** sectie aan de onderkant van de lijst. Klik op de **wachtwoord opnieuw instellen** knop. Voer uw gebruikersnaam en een nieuw wachtwoord. Ten slotte op de **opslaan** knop:
   
    ![Opnieuw instellen van de referenties van de gebruiker in de Azure portal](./media/troubleshoot-rdp-connection/classic-reset-password.png)
6. **Opnieuw opstarten van uw virtuele machine**. Deze stap kunt eventuele onderliggende problemen die de virtuele machine zelf heeft problemen te corrigeren.
   
    Selecteer uw virtuele machine in de Azure portal en klik op de **overzicht** tabblad. Klik op de **opnieuw** knop:
   
    ![Opnieuw opstarten van de virtuele machine in de Azure-portal](./media/troubleshoot-rdp-connection/classic-restart-vm.png)

Als u nog steeds er RDP problemen zijn, kunt u [opent u een ondersteuningsaanvraag](https://azure.microsoft.com/support/options/) of lezen [meer gedetailleerde probleemoplossing concepten en stappen RDP](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-specific-rdp-errors"></a>Specifieke RDP-fouten oplossen
U kunt een specifiek foutbericht mogelijk ondervindt bij het verbinding maken met uw virtuele machine via RDP. Hier volgen de meestvoorkomende foutberichten:

* [De externe sessie is beëindigd omdat er geen extern bureaublad-licentieservers beschikbaar zijn voor het bieden van een licentie](troubleshoot-specific-rdp-errors.md#rdplicense).
* [Extern bureaublad computer 'name' is niet gevonden](troubleshoot-specific-rdp-errors.md#rdpname).
* [Er is een verificatiefout opgetreden. Local Security Authority is niet bereikbaar](troubleshoot-specific-rdp-errors.md#rdpauth).
* [Fout bij de Windows-beveiliging: uw referenties werken niet](troubleshoot-specific-rdp-errors.md#wincred).
* [Deze computer kan geen verbinding maken met de externe computer](troubleshoot-specific-rdp-errors.md#rdpconnect).

## <a name="additional-resources"></a>Aanvullende bronnen
Als geen van deze fouten is opgetreden en u nog steeds geen verbinding met de virtuele machine via Extern bureaublad, leest u de gedetailleerde [problemen oplossen met extern bureaublad](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Zie voor probleemoplossing bij het openen van toepassingen die worden uitgevoerd op een virtuele machine, [toegang tot een toepassing die wordt uitgevoerd op een virtuele machine in Azure oplossen](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Als u problemen met behulp van Secure Shell (SSH ondervindt) als u wilt verbinding maken met een Linux-VM in Azure, Zie [oplossen SSH-verbindingen met een Linux VM in Azure](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


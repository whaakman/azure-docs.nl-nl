---
title: Een lokale Windows-wachtwoord zonder Azure-agent opnieuw instellen | Microsoft Docs
description: "Het opnieuw instellen van het wachtwoord van een lokale Windows-gebruikersaccount wanneer u de Azure guest-agent is niet geïnstalleerd of op een virtuele machine werkt"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: cf353dd3-89c9-47f6-a449-f874f0957013
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/07/2017
ms.author: iainfou
ms.openlocfilehash: 880f5e5967298401fc2522124af3746d9906ffa8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-reset-local-windows-password-for-azure-vm"></a>Het lokale Windows-wachtwoord opnieuw instellen voor de virtuele machine in Azure
U kunt opnieuw instellen van het lokale Windows-wachtwoord van een virtuele machine in Azure worden verkregen met de [Azure-portal of Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) opgegeven in de Azure guest-agent is geïnstalleerd. Deze methode is de eenvoudigste manier om in te stellen van een wachtwoord voor een virtuele machine in Azure. Als u problemen ondervindt met de Azure guest-agent niet reageert of niet worden geïnstalleerd na het uploaden van een aangepaste installatiekopie, u handmatig kunt moet u een Windows-wachtwoord opnieuw instellen. Dit artikel wordt uitgelegd hoe u een lokaal account-wachtwoord opnieuw instellen door de virtuele bron OS-schijf koppelen aan een andere virtuele machine. 

> [!WARNING]
> Gebruik deze methode alleen als een laatste toevlucht. Probeert altijd opnieuw instellen van een wachtwoord met de [Azure-portal of Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) eerste.
> 
> 

## <a name="overview-of-the-process"></a>Overzicht van het proces
De belangrijkste stappen voor het uitvoeren van een lokale wachtwoord opnieuw instellen voor een Windows-VM in Azure als er geen toegang tot de Azure guest-agent is als volgt:

* Verwijder de bron-VM. De virtuele schijven worden bewaard.
* De besturingssysteemschijf van de bron-VM koppelen aan een andere virtuele machine op dezelfde locatie binnen uw Azure-abonnement. Deze virtuele machine wordt de probleemoplossing VM genoemd.
* Maak met de probleemoplossing VM enkele configuratiebestanden van de besturingssysteemschijf van de bron-VM.
* Loskoppelen van de VM-besturingssysteemschijf van de VM voor het oplossen van problemen.
* Gebruik Resource Manager-sjabloon maken van een virtuele machine met behulp van de oorspronkelijke virtuele schijf.
* Wanneer de nieuwe virtuele machine wordt opgestart, werk de configuratiebestanden die u maakt het wachtwoord van de gebruiker vereist.

## <a name="detailed-steps"></a>Gedetailleerde stappen
Probeert altijd opnieuw instellen van een wachtwoord met de [Azure-portal of Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voordat u de volgende stappen uit. Zorg ervoor dat er een back-up van uw virtuele machine voordat u begint. 

1. Verwijder de betrokken virtuele machine in Azure-portal. Verwijderen van de virtuele machine verwijdert u alleen de metagegevens van de referentie van de virtuele machine in Azure. De virtuele schijven worden bewaard wanneer de virtuele machine wordt verwijderd:
   
   * Selecteer de virtuele machine in de Azure portal, klik op *verwijderen*:
     
     ![Bestaande virtuele machine verwijderen](./media/reset-local-password-without-agent/delete_vm.png)
2. De besturingssysteemschijf van de bron-VM voor het oplossen van problemen VM koppelen. De probleemoplossing VM moet zich in dezelfde regio bevinden als de besturingssysteemschijf van de bron-VM (zoals `West US`):
   
   * Selecteer de probleemoplossing VM in de Azure-portal. Klik op *schijven* | *Attach bestaande*:
     
     ![Bestaande schijf koppelen](./media/reset-local-password-without-agent/disks_attach_existing.png)
     
     Selecteer *VHD-bestand* en selecteer vervolgens het opslagaccount met de bron-VM:
     
     ![Opslagaccount selecteren](./media/reset-local-password-without-agent/disks_select_storageaccount.PNG)
     
     Selecteer de Broncontainer. De Broncontainer is doorgaans *VHD's*:
     
     ![Storage-container selecteren](./media/reset-local-password-without-agent/disks_select_container.png)
     
     Selecteer de OS-vhd koppelen. Klik op *Selecteer* om het proces te voltooien:
     
     ![Selecteer de virtuele bronschijf](./media/reset-local-password-without-agent/disks_select_source_vhd.png)
3. Verbinding maken met de probleemoplossing virtuele machine via Extern bureaublad en zorg ervoor dat de besturingssysteemschijf van de bron-VM wordt weergegeven:
   
   * Selecteer de probleemoplossing VM in de Azure portal en klik op *Connect*.
   * Open het RDP-bestand die kan worden gedownload. Geef de gebruikersnaam en wachtwoord van de VM voor het oplossen van problemen.
   * Zoek in Windows Verkenner naar de door u bijgevoegde gegevensschijf. Als de bron van de virtuele machine VHD de enige gegevensschijf is gekoppeld aan de VM voor het oplossen van problemen is, moet het station F::
     
     ![De schijf bijgesloten gegevens weergeven](./media/reset-local-password-without-agent/troubleshooting_vm_fileexplorer.png)
4. Maak `gpt.ini` in `\Windows\System32\GroupPolicy` op de bron-VM-station (als gpt.ini bestaat, wijzig in gpt.ini.bak):
   
   > [!WARNING]
   > Zorg ervoor dat u niet per ongeluk de volgende bestanden in C:\Windows, de OS-schijf voor het oplossen van problemen VM maakt. De volgende bestanden in het station van het besturingssysteem voor de bron-VM die is gekoppeld als een gegevensschijf maken.
   > 
   > 
   
   * Voeg de volgende regels in de `gpt.ini` dat u hebt gemaakt:
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     ![Gpt.ini maken](./media/reset-local-password-without-agent/create_gpt_ini.png)
5. Maak `scripts.ini` in `\Windows\System32\GroupPolicy\Machine\Scripts`. Zorg ervoor dat verborgen mappen worden weergegeven. Maak indien nodig de `Machine` of `Scripts` mappen.
   
   * Voeg de volgende regels de `scripts.ini` dat u hebt gemaakt:
     
     ```
     [Startup]
     0CmdLine=C:\Windows\System32\FixAzureVM.cmd
     0Parameters=
     ```
     
     ![Scripts.ini maken](./media/reset-local-password-without-agent/create_scripts_ini.png)
6. Maak `FixAzureVM.cmd` in `\Windows\System32` met de volgende inhoud, vervangen `<username>` en `<newpassword>` met uw eigen waarden:
   
    ```
    net user <username> <newpassword> /add
    net localgroup administrators <username> /add
    net localgroup "remote desktop users" <username> /add

    ```

    ![FixAzureVM.cmd maken](./media/reset-local-password-without-agent/create_fixazure_cmd.png)
   
    U moet voldoen aan de complexiteitsvereisten van het ingestelde wachtwoord voor uw virtuele machine bij het definiëren van het nieuwe wachtwoord.
7. Ontkoppel de schijf van de VM voor het oplossen van problemen in Azure-portal:
   
   * Selecteer de probleemoplossing VM in de Azure portal, klik op *schijven*.
   * Selecteer de gegevensschijf gekoppeld in stap 2, klik op *Detach*:
     
     ![Loskoppelen van schijf](./media/reset-local-password-without-agent/detach_disk.png)
8. Voordat u een virtuele machine maakt, krijgen de URI naar de bron-OS-schijf:
   
   * Selecteer het opslagaccount in de Azure portal, klikt u op *Blobs*.
   * Selecteer de container. De Broncontainer is doorgaans *VHD's*:
     
     ![Storage-blob voor account selecteren](./media/reset-local-password-without-agent/select_storage_details.png)
     
     Selecteer de bron-VM OS VHD en klik op de *kopie* naast de *URL* naam:
     
     ![Kopiëren van schijf URI](./media/reset-local-password-without-agent/copy_source_vhd_uri.png)
9. Een virtuele machine maken van de besturingssysteemschijf van de bron-VM:
   
   * Gebruik [deze Azure Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) voor het maken van een virtuele machine vanaf een speciale VHD. Klik op de `Deploy to Azure` knop voor het openen van de Azure-portal met de details van de sjablonen die voor u wordt gevuld.
   * Als u behouden van de vorige instellingen voor de virtuele machine wilt, selecteert u *template bewerken* om te bieden uw bestaande VNet, subnet, netwerkadapter of openbare IP-adres.
   * In de `OSDISKVHDURI` tekstvak parameter, plak de URI van de bron-VHD verkrijgen in de vorige stap:
     
     ![Maak een VM van sjabloon](./media/reset-local-password-without-agent/create_new_vm_from_template.png)
10. Nadat de nieuwe virtuele machine wordt uitgevoerd, verbinding maken met de virtuele machine via Extern bureaublad met het nieuwe wachtwoord dat u hebt opgegeven in de `FixAzureVM.cmd` script.
11. Verwijderen van de externe sessie naar de nieuwe virtuele machine de volgende bestanden om de omgeving op te schonen:
    
    * Van %windir%\System32
      * FixAzureVM.cmd verwijderen
    * Van %windir%\System32\GroupPolicy\Machine\
      * scripts.ini verwijderen
    * Van %windir%\System32\GroupPolicy
      * gpt.ini verwijderen (indien gpt.ini bestond en u een naam gewijzigd in gpt.ini.bak, wijzig de naam van het .bak-bestand terug naar gpt.ini)

## <a name="next-steps"></a>Volgende stappen
Als u nog steeds geen verbinding maken met extern bureaublad, raadpleegt u de [RDP-probleemoplossingsgids](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). De [RDP probleemoplossingsgids gedetailleerde](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) gekeken naar het oplossen van methoden in plaats van specifieke stappen uitvoeren. U kunt ook [opent u een Azure ondersteuningsaanvraag](https://azure.microsoft.com/support/options/) voor praktische hulp.


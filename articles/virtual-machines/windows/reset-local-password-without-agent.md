---
title: Een lokale Windows-wachtwoord zonder Azure-agent opnieuw instellen | Microsoft Docs
description: Het opnieuw instellen van het wachtwoord van een lokale Windows-gebruikersaccount wanneer de Azure-gastagent niet geïnstalleerd of op een virtuele machine werkt is
services: virtual-machines-windows
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
ms.assetid: cf353dd3-89c9-47f6-a449-f874f0957013
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/25/2018
ms.author: iainfou
ms.openlocfilehash: 6745d5f7c31ca00c7915874b038488f4487959a9
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37342919"
---
# <a name="reset-local-windows-password-for-azure-vm-offline"></a>Lokale Windows-wachtwoord offline voor Azure-VM herstellen
U kunt opnieuw instellen van het lokale Windows-wachtwoord van een virtuele machine in Azure met de [Azure portal of Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voorwaarde dat de Azure-gastagent is geïnstalleerd. Deze methode is de belangrijkste manier om een wachtwoord opnieuw instellen voor een Azure-VM. Als u problemen ondervindt met de Azure-gastagent niet reageert of niet worden geïnstalleerd na het uploaden van een aangepaste installatiekopie, u handmatig kunt moet u een Windows-wachtwoord opnieuw instellen. Dit artikel wordt uitgelegd hoe u een lokaal account-wachtwoord opnieuw instellen door de virtuele bron-OS-schijf koppelen aan een andere virtuele machine. De stappen in dit artikel niet van toepassing op Windows-domeincontrollers. 

> [!WARNING]
> Gebruik deze methode alleen als laatste redmiddel. Altijd probeer het opnieuw instellen van een wachtwoord met de [Azure portal of Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) eerste.
> 
> 

## <a name="overview-of-the-process"></a>Overzicht van het proces
De belangrijkste stappen voor het uitvoeren van een lokaal wachtwoord opnieuw instellen voor een Windows-VM in Azure als er geen toegang tot de Azure-gastagent is als volgt:

* Verwijder de bron-VM. De virtuele schijven worden bewaard.
* Koppel de besturingssysteemschijf van de bron-VM aan een andere virtuele machine op dezelfde locatie binnen uw Azure-abonnement. Deze virtuele machine wordt aangeduid als de virtuele machine voor probleemoplossing.
* Met behulp van de virtuele machine voor probleemoplossing, sommige configuratiebestanden op de besturingssysteemschijf van de bron-VM maken.
* Loskoppelen van de besturingssysteemschijf van de virtuele machine van de VM voor probleemoplossing.
* Resource Manager-sjabloon gebruiken om te maken van een VM met behulp van de oorspronkelijke virtuele schijf.
* Wanneer de nieuwe virtuele machine wordt opgestart, werk de configuratiebestanden die u maakt het wachtwoord van de gebruiker vereist.

## <a name="detailed-steps"></a>Gedetailleerde stappen

> [!NOTE]
> De stappen niet van toepassing op Windows-domeincontrollers. Dit werkt alleen op de zelfstandige server of een server die deel uitmaakt van een domein.
> 
> 

Altijd probeer het opnieuw instellen van een wachtwoord met de [Azure portal of Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voordat u de volgende stappen uit. Zorg ervoor dat u hebt een back-up van uw virtuele machine voordat u begint. 

1. Verwijder de betrokken virtuele machine in Azure portal. De virtuele machine verwijderen, verwijdert u alleen de metagegevens worden de referentie van de virtuele machine in Azure. De virtuele schijven worden bewaard wanneer de virtuele machine wordt verwijderd:
   
   * Selecteer de virtuele machine in Azure portal, klikt u op *verwijderen*:
     
     ![Bestaande virtuele machine verwijderen](./media/reset-local-password-without-agent/delete_vm.png)
2. Koppel de besturingssysteemschijf van de bron-VM aan de virtuele machine voor probleemoplossing. De virtuele machine voor probleemoplossing moet zich in dezelfde regio bevinden als de besturingssysteemschijf van de bron-VM (zoals `West US`):
   
   * Selecteer de virtuele machine voor probleemoplossing in Azure portal. Klik op *schijven* | *koppelen aan bestaande*:
     
     ![Bestaande schijf koppelen](./media/reset-local-password-without-agent/disks_attach_existing.png)
     
     Selecteer *VHD-bestand* en selecteer vervolgens het opslagaccount waarin uw bron-VM:
     
     ![Opslagaccount selecteren](./media/reset-local-password-without-agent/disks_select_storageaccount.PNG)
     
     Selecteer de Broncontainer. De Broncontainer is doorgaans *VHD's*:
     
     ![Storage-container selecteren](./media/reset-local-password-without-agent/disks_select_container.png)
     
     Selecteer de OS vhd te koppelen. Klik op *Selecteer* om het proces te voltooien:
     
     ![Selecteer de virtuele schijf die als bron](./media/reset-local-password-without-agent/disks_select_source_vhd.png)
3. Verbinding maken met de VM voor probleemoplossing met behulp van extern bureaublad en zorg ervoor dat de besturingssysteemschijf van de bron-VM wordt weergegeven:
   
   * Selecteer de virtuele machine voor probleemoplossing in Azure portal en klik op *Connect*.
   * Open het RDP-bestand die kan worden gedownload. Voer de gebruikersnaam en wachtwoord van de virtuele machine voor probleemoplossing.
   * Zoeken in Windows Verkenner naar de gegevensschijf die u hebt gekoppeld. Als de bron van de virtuele machine VHD de enige gegevensschijf die is gekoppeld aan de virtuele machine voor probleemoplossing is, moet het station F::
     
     ![Gekoppelde gegevensschijf weergeven](./media/reset-local-password-without-agent/troubleshooting_vm_fileexplorer.png)
4. Maak `gpt.ini` in `\Windows\System32\GroupPolicy` op de bron-VM-schijf (als gpt.ini bestaat, naam wijzigen in gpt.ini.bak):
   
   > [!WARNING]
   > Zorg ervoor dat u per ongeluk de volgende bestanden in C:\Windows, de besturingssysteemschijf voor de virtuele machine voor probleemoplossing maakt. Maak de volgende bestanden in de besturingssysteemschijf voor de bron-VM die is gekoppeld als een gegevensschijf.
   > 
   > 
   
   * Voeg de volgende regels in de `gpt.ini` bestand dat u hebt gemaakt:
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     ![Gpt.ini maken](./media/reset-local-password-without-agent/create_gpt_ini.png)
5. Maak `scripts.ini` in `\Windows\System32\GroupPolicy\Machine\Scripts\Startup`. Zorg ervoor dat verborgen mappen worden weergegeven. Maak indien nodig de `Machine` of `Scripts` mappen.
   
   * Voeg de volgende regels toe de `scripts.ini` bestand dat u hebt gemaakt:
     
     ```
     [Startup]
     0CmdLine=C:\Windows\System32\FixAzureVM.cmd
     0Parameters=
     ```
     
     ![Scripts.ini maken](./media/reset-local-password-without-agent/create_scripts_ini.png)
6. Maak `FixAzureVM.cmd` in `\Windows\System32` met de volgende inhoud, vervangen `<username>` en `<newpassword>` door uw eigen waarden:
   
    ```
    net user <username> <newpassword> /add
    net localgroup administrators <username> /add
    net localgroup "remote desktop users" <username> /add
    ```

    ![Create FixAzureVM.cmd](./media/reset-local-password-without-agent/create_fixazure_cmd.png)
   
    U moet voldoen aan de complexiteitsvereisten van het ingestelde wachtwoord voor uw virtuele machine bij het definiëren van het nieuwe wachtwoord.
7. Ontkoppel de schijf van de virtuele machine voor probleemoplossing in Azure-portal:
   
   * Selecteer de virtuele machine voor probleemoplossing in Azure portal, klik op *schijven*.
   * Selecteer de gegevensschijf die is gekoppeld in stap 2, klik op *loskoppelen*:
     
     ![Schijf loskoppelen](./media/reset-local-password-without-agent/detach_disk.png)
8. Voordat u een virtuele machine maken, verkrijgen van de URI voor de bron-OS-schijf:
   
   * Selecteer het opslagaccount in Azure portal, klikt u op *Blobs*.
   * Selecteer de container. De Broncontainer is doorgaans *VHD's*:
     
     ![Selecteer de logboekopslagaccount-blob](./media/reset-local-password-without-agent/select_storage_details.png)
     
     Selecteer de bron-VM OS VHD en klik op de *kopie* naast de *URL* naam:
     
     ![Kopiëren van schijf-URI](./media/reset-local-password-without-agent/copy_source_vhd_uri.png)
9. Een virtuele machine maken van de besturingssysteemschijf van de bron-VM:
   
   * Gebruik [deze Azure Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-new-or-existing-vnet) aan een virtuele machine maken vanaf een gespecialiseerde VHD. Klik op de `Deploy to Azure` knop voor het openen van de Azure-portal met de details van de sjablonen die voor u wordt gevuld.
   * Als u behouden van alle voorgaande instellingen voor de virtuele machine wilt, selecteert u *template bewerken* voor uw bestaande VNet, subnet, netwerkadapter of openbaar IP-adres.
   * In de `OSDISKVHDURI` parameter tekstvak, plak de URI van de bron-VHD verkrijgen in de vorige stap:
     
     ![Een virtuele machine maken van sjabloon](./media/reset-local-password-without-agent/create_new_vm_from_template.png)
10. Nadat de nieuwe virtuele machine wordt uitgevoerd, verbinding maken met de virtuele machine via Extern bureaublad met het nieuwe wachtwoord dat u hebt opgegeven in de `FixAzureVM.cmd` script.
11. Verwijderen van de externe sessie moet de nieuwe virtuele machine, de volgende bestanden voor het opschonen van de omgeving:
    
    * From %windir%\System32
      * remove FixAzureVM.cmd
    * From %windir%\System32\GroupPolicy\Machine\
      * scripts.ini verwijderen
    * Van %windir%\System32\GroupPolicy
      * gpt.ini verwijderen (als gpt.ini bestond, en u deze hebt gewijzigd in gpt.ini.bak, het bestand .bak terug naar gpt.ini wijzigen)

## <a name="next-steps"></a>Volgende stappen
Als u nog steeds geen verbinding met behulp van extern bureaublad maken, raadpleegt u de [RDP-gids voor probleemoplossing](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). De [gedetailleerde RDP-problemen oplossen met](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) kijkt naar het oplossen van methoden in plaats van specifieke stappen. U kunt ook [opent u een Azure-ondersteuningsaanvraag](https://azure.microsoft.com/support/options/) voor praktische hulp.


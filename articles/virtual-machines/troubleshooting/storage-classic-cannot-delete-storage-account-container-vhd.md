---
title: Fouten oplossen wanneer u Azure-klassieke opslagaccounts, containers of VHD's verwijderen | Microsoft Docs
description: Klik hier voor meer informatie over het oplossen van problemen bij het verwijderen van storage-resources met gekoppelde VHD's.
services: storage
author: AngshumanNayakMSFT
tags: top-support-issue,azure-service-management
ms.service: storage
ms.topic: troubleshooting
ms.date: 01/11/2019
ms.author: annayak
ms.openlocfilehash: 72493c6bba556314c3652be5251463d1d1e005bd
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54383442"
---
# <a name="troubleshoot-classic-storage-resource-deletion-errors"></a>Oplossen van fouten bij het verwijderen van het klassieke opslag-resource
Dit artikel bevat richtlijnen voor probleemoplossing bij een van de volgende fouten optreedt bij het verwijderen van Azure klassieke storage-account, container of *.vhd pagina-blob-bestand. 


In dit artikel heeft alleen betrekking op problemen met klassieke opslagresources. Als een gebruiker verwijdert een klassieke virtuele machine met behulp van de Azure portal, PowerShell of CLI en vervolgens de schijven worden niet automatisch verwijderd. De gebruiker beschikt over de optie voor het verwijderen van de resource 'Disk'. Als de optie niet is geselecteerd, wordt de 'Schijf'-resource te voorkomen dat verwijderen van het opslagaccount, container en de werkelijke *.vhd pagina-blob-bestand.

Meer informatie over Azure-schijven kunt u vinden [hier](../../virtual-machines/windows/about-disks-and-vhds.md). Azure wordt voorkomen dat het verwijderen van een schijf die is gekoppeld aan een virtuele machine om beschadiging te voorkomen. Dit voorkomt ook dat het verwijderen van containers en storage-accounts waarvoor een pagina-blob die is gekoppeld aan een virtuele machine. 

## <a name="what-is-a-disk"></a>Wat is een 'Disk'?
Een resource 'Disk' wordt gebruikt om een bestand met *.vhd pagina blob aan een virtuele machine, als een besturingssysteemschijf of een gegevensschijf koppelen. Een besturingssysteemschijf of gegevens schijfbron, tot verwijderd, blijft een lease op het bestand *.vhd bevatten. De storage-resources in het pad bovenstaande afbeelding kan niet worden verwijderd als een resource 'Disk' naar deze verwijst.

![Schermafbeelding van de portal, met het deelvenster van de schijf (klassiek) 'Eigenschap' openen](./media/storage-classic-cannot-delete-storage-account-container-vhd/Disk_Lease_Illustration.jpg) 


## <a name="steps-while-deleting-a-classic-virtual-machine"></a>Stappen bij het verwijderen van een klassieke virtuele machine 
1. Verwijder het klassieke virtuele machine.
2. Als het selectievakje 'Schijf' is ingeschakeld, de **schijflease** (weergegeven in de afbeelding hierboven) die zijn gekoppeld aan de pagina-blob *.vhd is verbroken. De werkelijke pagina blob *.vhd bestand nog steeds aanwezig in de storage-account.
![Schermafbeelding van de portal, met de virtuele machine (klassiek) "Verwijderen" fout deelvenster openen](./media/storage-classic-cannot-delete-storage-account-container-vhd/steps_while_deleting_classic_vm.jpg) 

3. Zodra de lease of meer schijven verbroken wordt, kan de pagina BLOB (s) zelf worden verwijderd. Een opslagaccount of container kan worden verwijderd zodra alle 'Disk' resource aanwezig zijn in deze zijn verwijderd.

>[!NOTE] 
>Als gebruiker Hiermee verwijdert u de virtuele machine, maar niet de VHD, blijft kosten voor opslag op het wisselbestand van de blob-*.vhd doorlopen. De kosten worden in overeenstemming met het type opslagaccount, Controleer de [pagina met prijzen](https://azure.microsoft.com/en-us/pricing/details/storage/) voor meer informatie. Als de gebruiker wil niet langer de virtuele harde schijven kunt gebruiken, verwijder it/ze om toekomstige kosten te voorkomen. 

## <a name="unable-to-delete-storage-account"></a>Kan niet worden verwijderd van de storage-account 

Wanneer de gebruiker probeert te verwijderen van een klassiek opslagaccount die niet meer nodig is, kan de gebruiker het volgende gedrag zien.

#### <a name="azure-portal"></a>Azure Portal 
Gebruiker navigeert naar de klassieke storage-account op de [Azure-portal](https://portal.azure.com) en klikt op **verwijderen**, gebruiker ziet het volgende bericht: 

Met een of meer schijven 'gekoppeld' aan een virtuele machine

![Schermafbeelding van de portal, met de virtuele machine (klassiek) "Verwijderen" fout deelvenster openen](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_storage_account_disks_attached_portal.jpg) 


Met 'niet-gekoppelde' aan een virtuele machine (s)

![Schermafbeelding van de portal, met de virtuele machine (klassiek) "Verwijderen" zonder fout deelvenster openen](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_storage_account_disks_unattached_portal.jpg)


#### <a name="azure-powershell"></a>Azure PowerShell
Gebruiker probeert te verwijderen van een storage-account, dat niet meer wordt gebruikt, met behulp van klassieke PowerShell-cmdlets. Gebruiker ziet het volgende bericht:

><span style="color:cyan">**Remove-AzureStorageAccount -StorageAccountName myclassicaccount**</span>

><span style="color:red">Remove-AzureStorageAccount : BadRequest: Storage-account myclassicaccount heeft een of meer actieve installatiekopieën en/of schijven, bijvoorbeeld  
myclassicaccount. Zorg ervoor dat deze installatiekopieën en/of schijven worden verwijderd voordat u dit opslagaccount verwijdert.</span>

## <a name="unable-to-delete-storage-container"></a>Kan niet worden verwijderd van de storage-container

Wanneer de gebruiker probeert te verwijderen van een klassieke opslag-blob-container die niet meer nodig is, kan de gebruiker het volgende gedrag zien.

#### <a name="azure-portal"></a>Azure Portal 
Azure-portal wouldn't toestaan dat de gebruiker verwijderen van een container als een lease "Of meer schijven" bestaat die verwijst naar een bestand voor de blob *.vhd in de container. Het is om te voorkomen dat per ongeluk verwijderen van een bestand van de virtuele harde schijven met een of meer schijven lease op deze standaard. 

![Schermafbeelding van de portal, met de storage-container 'list' deelvenster openen](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_container_portal.jpg)


#### <a name="azure-powershell"></a>Azure PowerShell
Als de gebruiker ervoor kiest om te verwijderen met behulp van PowerShell, resulteert dit in de volgende fout. 

><span style="color:cyan">**Remove-AzureStorageContainer -Context $context -Name vhds**</span>

><span style="color:red">Remove-AzureStorageContainer : De externe server heeft een fout geretourneerd: (412) er is momenteel een lease voor de container en er is geen lease-ID is opgegeven in de aanvraag... HTTP-statuscode: 412 - HTTP-foutbericht: Er is momenteel een lease voor de container en er is geen lease-ID is opgegeven in de aanvraag.</span>

## <a name="unable-to-delete-a-vhd"></a>Kan niet worden verwijderd van een vhd 

Na het verwijderen van de virtuele machine van Azure, worden de gebruiker probeert te verwijderen van het vhd-bestand (pagina-blobs) en wordt de hieronder weergegeven:

#### <a name="azure-portal"></a>Azure Portal 
In de portal, kunnen er twee ervaringen, afhankelijk van de lijst met blobs die zijn geselecteerd voor verwijdering.

1. Als er slechts 'Geleased' blobs zijn geselecteerd, wordt niet klikt u vervolgens de knop verwijderen weergegeven.
![Schermafbeelding van de portal, met de container blob 'list' deelvenster openen](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_vhd_leased_portal.jpg)


2. Als er een combinatie van 'Geleased' en 'Beschikbaar'-blobs zijn geselecteerd, wordt de knop 'Verwijderen' wordt weergegeven. Maar de bewerking 'Delete' wordt niet achter de pagina-blobs, waarvoor een schijflease op deze. 
![Schermafbeelding van de portal, met het deelvenster container blob 'list' open](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_vhd_leased_and_unleased_portal_1.jpg)
![schermafbeelding van de portal, met de geselecteerde blob "verwijderen" deelvenster openen](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_vhd_leased_and_unleased_portal_2.jpg)

#### <a name="azure-powershell"></a>Azure PowerShell 
Als de gebruiker ervoor kiest om te verwijderen met behulp van PowerShell, resulteert dit in de volgende fout. 

><span style="color:cyan">**Remove-AzureStorageBlob -Context $context -Container vhds -Blob "classicvm-os-8698.vhd"**</span>

><span style="color:red">Remove-AzureStorageBlob : De externe server heeft een fout geretourneerd: (412) er is momenteel een lease op de blob en er is geen lease-ID is opgegeven in de aanvraag... HTTP-statuscode: 412 - HTTP-foutbericht: Er is momenteel een lease op de blob en er is geen lease-ID is opgegeven in de aanvraag.</span>


## <a name="resolution-steps"></a>Oplossingen

### <a name="to-remove-classic-disks"></a>Klassieke schijven verwijderen
Volg deze stappen op de Azure-portal:
1.  Navigeer naar [Azure Portal](https://portal.azure.com).
2.  Navigeer naar de Disks(classic). 
3.  Klik op het tabblad Schijven. ![Schermafbeelding van de portal, met de container blob 'list' deelvenster openen](./media/storage-classic-cannot-delete-storage-account-container-vhd/resolution_click_disks_tab.jpg)
 
4.  Selecteer de gegevensschijf en klik vervolgens op Schijf verwijderen.
 ![Schermafbeelding van de portal, met de container blob 'list' deelvenster openen](./media/storage-classic-cannot-delete-storage-account-container-vhd/resolution_click_delete_disk.jpg)
 
5.  Opnieuw proberen te verwijderen die eerder is mislukt.
6.  Een opslagaccount of container kan niet worden verwijderd, zolang er één schijf.

### <a name="to-remove-classic-images"></a>Klassieke installatiekopieën verwijderen   
Volg deze stappen op de Azure-portal:
1.  Navigeer naar [Azure Portal](https://portal.azure.com).
2.  Navigeer naar de OS-installatiekopieën (klassiek).
3.  De installatiekopie verwijderen.
4.  Opnieuw proberen te verwijderen die eerder is mislukt.
5.  Een opslagaccount of container kan niet worden verwijderd, zolang er één installatiekopie.

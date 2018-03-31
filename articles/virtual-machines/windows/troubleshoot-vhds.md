---
title: Problemen met gekoppelde VHD's op Windows Azure virtuele machines | Microsoft Docs
description: Het oplossen van problemen zoals onverwacht opnieuw wordt opgestart Windows virtuele machines of problemen bij het verwijderen van een storage-account met VHD's gekoppeld.
keywords: SSH verbinding geweigerd, ssh fout, azure ssh, SSH-verbinding is mislukt
services: virtual-machines-windows
author: roygara
manager: jeconnoc
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines-windows
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 02/28/2018
ms.author: rogarana
ms.openlocfilehash: d0103d8ea608014e53f70402220b302b6da445e9
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/30/2018
---
# <a name="troubleshoot-attached-vhds-on-azure-windows-virtual-machines"></a>Problemen met gekoppelde VHD's op Windows Azure virtuele machines

Virtuele Machines in Azure, is afhankelijk van virtuele harde schijven (VHD's) voor de besturingssysteemschijf en alle gekoppeld gegevensschijven. VHD's worden opgeslagen als pagina-blobs in een of meer Azure Storage-accounts. In dit artikel wordt beschreven hoe algemene problemen die zich met VHD's voordoen kunnen. 

  * [Virtuele machines met gekoppelde VHD's onverwacht opnieuw wordt opgestart]
  * [Fouten bij het verwijderen van opslag in de implementatie van Resource Manager]

## <a name="you-are-experiencing-unexpected-reboots"></a>Virtuele machines met gekoppelde VHD's onverwacht opnieuw wordt opgestart

Als een Azure-virtuele Machine (VM) een groot aantal gekoppelde VHD's die zich in hetzelfde opslagaccount heeft, mag u de schaalbaarheidsdoelen voor een afzonderlijke opslagaccount, waardoor de virtuele machine niet overschrijden. Controleer de minuut metrische gegevens voor het opslagaccount (**TotalRequests**/**TotalIngress**/**TotalEgress**) pieken die groter is dan de schaalbaarheidsdoelen voor een opslagaccount. Zie de sectie '[metrische gegevens tonen een toename van PercentThrottlingError]' voor hulp bij het bepalen of beperking is opgetreden op uw opslagaccount.

In het algemeen elke afzonderlijke invoer of uitvoerbewerking op een VHD van een virtuele Machine wordt omgezet in **downloaden pagina** of **pagina plaatsen** bewerkingen op de onderliggende paginablob. Daarom kunt u het geschatte aantal IOPS dat voor uw omgeving om af te stemmen hoeveel VHD's die u kunt hebben in een één opslagaccount op basis van het specifieke gedrag van uw toepassing. Microsoft raadt 40 of minder schijven in een enkel opslagaccount. Zie [Azure Storage Scalability and Performance Targets](../../storage/common/storage-scalability-targets.md) voor meer informatie over schaalbaarheidsdoelen voor storage-accounts, met name het totale percentage en de totale bandbreedte van het type opslagaccount u gebruikt.

Als u de van schaalbaarheidsdoelen voor uw opslagaccount overschreden, plaatst u uw VHD's in meerdere opslagaccounts te verminderen van de activiteit in elke rekening.

## <a name="storage-delete-errors-in-rm"></a>Fouten bij het verwijderen van opslag in de implementatie van Resource Manager

Deze sectie bevat richtlijnen voor probleemoplossing bij een van de volgende fouten optreden terwijl u probeert te verwijderen van een Azure storage-account, container of blob in een Azure Resource Manager-implementatie.

>**Storage-account 'StorageAccountName' verwijderen is mislukt. Fout: Het storage-account kan niet worden verwijderd omdat de artefacten die in gebruik.**

>**# Buiten # container (s) verwijderen is mislukt:<br>VHD's: Er is momenteel een lease voor de container en geen lease-ID is opgegeven in de aanvraag.**

>**# Buiten # blobs verwijderen is mislukt:<br>BlobName.vhd: Er is momenteel een lease op de blob en geen lease-ID is opgegeven in de aanvraag.**

De VHD's in Azure VM's zijn VHD-bestanden die zijn opgeslagen als pagina-blobs in een standard- of premium storage-account in Azure.  Meer informatie over Azure-schijven kunt u vinden [hier](../../virtual-machines/windows/about-disks-and-vhds.md). Azure voorkomt u dat de verwijdering van een schijf die is gekoppeld aan een virtuele machine om beschadiging te voorkomen. Dit voorkomt ook dat de verwijdering van containers en storage-accounts waarvoor een pagina-blob die is gekoppeld aan een virtuele machine. 

Het proces voor het verwijderen van een opslagaccount, container of blob tijdens het ontvangen van een van deze fouten is: 
1. [Blobs die zijn gekoppeld aan een VM identificeren](#step-1-identify-blobs-attached-to-a-vm)
2. [Virtuele machines verwijderen met gekoppeld **besturingssysteemschijf**](#step-2-delete-vm-to-detach-os-disk)
3. [Los alle **gegevens een of meer schijven** van resterende VM('s)](#step-3-detach-data-disk-from-the-vm)

Probeer verwijderd opslagaccount, container of blob nadat deze stappen zijn voltooid.

### <a name="step-1-identify-blob-attached-to-a-vm"></a>Stap 1: Blob is gekoppeld aan een VM identificeren

#### <a name="scenario-1-deleting-a-blob--identify-attached-vm"></a>Scenario 1: Verwijderen van een blob – gekoppelde VM identificeren
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu Hub **alle resources**. Ga naar het opslagaccount onder **Blob-Service** Selecteer **Containers**, en navigeer naar de blob te verwijderen.
3. Als de blob **Lease status** is **huurlijnen**, klik met de rechtermuisknop en selecteer **metagegevens bewerken** om Blob metagegevens deelvenster te openen. 

    ![Schermafbeelding van de portal met de opslag blobs account in en klik met de rechtermuisknop > 'Bewerken metagegevens' gemarkeerd](./media/troubleshoot-vhds/utd-edit-metadata-sm.png)

4. Controleer in het deelvenster voor Blob-metagegevens en leg de waarde voor **MicrosoftAzureCompute_VMName**. Deze waarde is de naam van de virtuele machine waaraan de VHD is gekoppeld. (Zie **belangrijke** als dit veld niet bestaat)
5. Controleer in het deelvenster voor Blob-metagegevens en leg de waarde van **MicrosoftAzureCompute_DiskType**. Deze waarde wordt aangeduid als de gekoppelde schijf OS- of schijf (Zie **belangrijke** als dit veld niet bestaat). 

     ![Schermafbeelding van de portal, met het deelvenster van de 'Blobmetagegevens' opslag openen](./media/troubleshoot-vhds/utd-blob-metadata-sm.png)

6. Als het schijftype blob **OSDisk** Volg [stap 2: verwijderen VM loskoppelen van de besturingssysteemschijf](#step-2-delete-vm-to-detach-os-disk). Anders wordt als het schijftype blob **DataDisk** Volg de stappen in [stap 3: gegevensschijf loskoppelen van de virtuele machine](#step-3-detach-data-disk-from-the-vm). 

> [!IMPORTANT]
> Als **MicrosoftAzureCompute_VMName** en **MicrosoftAzureCompute_DiskType** worden niet weergegeven in de blobmetagegevens van de, betekent dit dat de blob expliciet geleasete is en is niet gekoppeld aan een virtuele machine. Geleaste blobs kunnen niet worden verwijderd zonder dat u de lease eerste. Lease opsplitsen, met de rechtermuisknop op de blob en selecteren **einde lease**. Geleaste blobs die niet zijn gekoppeld aan een virtuele machine voorkomen dat de blob wordt verwijderd, maar niet voorkomen dat verwijdering van de account-container of opslag.

#### <a name="scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms"></a>Scenario 2: Verwijderen van een container - alle blob(s) in container die zijn gekoppeld aan virtuele machines identificeren
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu Hub **alle resources**. Ga naar het opslagaccount onder **Blob-Service** Selecteer **Containers**, en zoek de container moet worden verwijderd.
3. Klik om te openen van de container en wordt de lijst met blobs erin weergegeven. Identificeren van de blobs met Blobtype = **pagina-blob** en status van de Lease = **huurlijnen** uit deze lijst. Ga als volgt [Scenario 1](#step-1-identify-blobs-attached-to-a-vm) voor het identificeren van de virtuele machine die is gekoppeld aan elk van deze blobs.

    ![Schermafbeelding van de portal met de Storage-account-blobs en de 'Lease staat' 'Huurlijnen' gemarkeerd](./media/troubleshoot-vhds/utd-disks-sm.png)

4. Ga als volgt [stap 2](#step-2-delete-vm-to-detach-os-disk) en [stap 3](#step-3-detach-data-disk-from-the-vm) verwijderen VM('s) met **OSDisk** ont **DataDisk**. 

#### <a name="scenario-3-deleting-storage-account---identify-all-blobs-within-storage-account-that-are-attached-to-vms"></a>Scenario 3: Verwijderen van opslag rekening - alle blob(s) binnen opslagaccount die zijn gekoppeld aan virtuele machines identificeren
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu Hub **alle resources**. Ga naar het opslagaccount onder **Blob-Service** Selecteer **Containers**.

    ![Schermafbeelding van de portal met de storage-account-containers en de 'Lease staat' 'Huurlijnen' gemarkeerd](./media/troubleshoot-vhds/utd-containers-sm.png)

3. In **Containers** deelvenster alle containers identificeren waar **Lease status** is **huurlijnen** en volg [Scenario 2](#scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms) voor elk  **Geleasete** container.
4. Ga als volgt [stap 2](#step-2-delete-vm-to-detach-os-disk) en [stap 3](#step-3-detach-data-disk-from-the-vm) verwijderen VM('s) met **OSDisk** ont **DataDisk**. 

### <a name="step-2-delete-vm-to-detach-os-disk"></a>Stap 2: VM loskoppelen van de besturingssysteemschijf verwijderen
Als de VHD een besturingssysteemschijf is, moet u de virtuele machine verwijderen voordat de gekoppelde VHD kan worden verwijderd. Er is geen verdere actie zijn vereist voor gegevensschijven gekoppeld aan dezelfde virtuele machine nadat deze stappen zijn uitgevoerd:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu Hub **virtuele Machines**.
3. Selecteer de virtuele machine waaraan de VHD is gekoppeld.
4. Zorg ervoor dat er niets is actief met behulp van de virtuele machine, en dat u de virtuele machine niet meer nodig hebt.
5. Aan de bovenkant van de **details van de virtuele Machine** deelvenster **verwijderen**, en klik vervolgens op **Ja** om te bevestigen.
6. De virtuele machine moet worden verwijderd, maar de VHD kan worden bewaard. De VHD wordt echter niet langer moet worden gekoppeld aan een virtuele machine of een lease hebben op het. Het kan enkele minuten duren voordat de lease worden vrijgegeven. Om te bevestigen dat de lease is uitgebracht, blader naar de bloblocatie en in de **Blob-eigenschappen** deelvenster de **Lease Status** moet **beschikbaar**.

### <a name="step-3-detach-data-disk-from-the-vm"></a>Stap 3: Gegevensschijf loskoppelen van de virtuele machine
Als de VHD een gegevensschijf is, ontkoppel het VHD van de virtuele machine te verwijderen van de lease:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu Hub **virtuele Machines**.
3. Selecteer de virtuele machine waaraan de VHD is gekoppeld.
4. Selecteer **schijven** op de **details van de virtuele Machine** deelvenster.
5. Selecteer de gegevensschijf moet worden verwijderd de VHD is gekoppeld aan. U kunt bepalen welke blob in de schijf is gekoppeld aan de hand van de URL van de VHD.
6. U kunt de bloblocatie controleren door te klikken op de schijf om te controleren van het pad **VHD-URI** veld.
7. Selecteer **bewerken** op boven van **schijven** deelvenster.
8. Klik op **pictogram loskoppelen** van de gegevensschijf moet worden verwijderd.

     ![Schermafbeelding van de portal, met het deelvenster van de 'Blobmetagegevens' opslag openen](./media/troubleshoot-vhds/utd-vm-disks-edit.png)

9. Selecteer **Opslaan**. De schijf wordt nu losgekoppeld van de virtuele machine en de VHD niet langer lease. Het kan enkele minuten duren voordat de lease worden vrijgegeven. Om te bevestigen dat de lease is vrijgegeven, blader naar de bloblocatie en in de **Blob-eigenschappen** deelvenster de **Lease Status** waarde moet **ontgrendeld** of **Beschikbaar**.

[Virtuele machines met gekoppelde VHD's onverwacht opnieuw wordt opgestart]: #you-are-experiencing-unexpected-reboots
[Fouten bij het verwijderen van opslag in de implementatie van Resource Manager]: #storage-delete-errors-in-rm
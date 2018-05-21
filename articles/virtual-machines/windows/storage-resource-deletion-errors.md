---
title: Storage resource verwijdering fouten in een Azure Resource Manager-implementatie op Windows-VM's oplossen | Microsoft Docs
description: Klik hier voor meer informatie over het oplossen van problemen bij het verwijderen van opslagbronnen met gekoppelde VHD's.
keywords: SSH verbinding geweigerd, ssh fout, azure ssh, SSH-verbinding is mislukt
services: virtual-machines-windows
author: genlin
manager: cshepard
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines-windows
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 05/01/2018
ms.author: genli
ms.openlocfilehash: ba2876919540d4d6c0853ca1e8bf0d88855b951b
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="troubleshoot-storage-resource-deletion-errors"></a>Storage resource verwijdering fouten oplossen

In bepaalde gevallen kunnen optreden een van de volgende fouten zijn opgetreden tijdens het verwijderen van een Azure storage-account, container of blob in een Azure Resource Manager-implementatie testen:

>**Storage-account 'StorageAccountName' verwijderen is mislukt. Fout: Het storage-account kan niet worden verwijderd omdat de artefacten die in gebruik.**

>**# Buiten # container (s) verwijderen is mislukt:<br>VHD's: Er is momenteel een lease voor de container en geen lease-ID is opgegeven in de aanvraag.**

>**# Buiten # blobs verwijderen is mislukt:<br>BlobName.vhd: Er is momenteel een lease op de blob en geen lease-ID is opgegeven in de aanvraag.**

De VHD's in Azure VM's zijn VHD-bestanden die zijn opgeslagen als pagina-blobs in een standard- of premium storage-account in Azure. Zie voor meer informatie over Azure-schijven, [over niet-beheerd en beheerde schijfopslag voor Microsoft Azure VM's van Windows](about-disks-and-vhds.md). 

Azure voorkomt u dat de verwijdering van een schijf die is gekoppeld aan een virtuele machine om beschadiging te voorkomen. Dit voorkomt ook dat de verwijdering van containers en storage-accounts waarvoor een pagina-blob die is gekoppeld aan een virtuele machine. 

Het proces voor het verwijderen van een opslagaccount, container of blob tijdens het ontvangen van een van deze fouten is: 
1. [Blobs die zijn gekoppeld aan een VM identificeren](#step-1-identify-blobs-attached-to-a-vm)
2. [Virtuele machines verwijderen met gekoppeld **besturingssysteemschijf**](#step-2-delete-vm-to-detach-os-disk)
3. [Los alle **gegevens een of meer schijven** van resterende VM('s)](#step-3-detach-data-disk-from-the-vm)

Probeer de storage-account, container of blob verwijderen nadat deze stappen zijn voltooid.

## <a name="step-1-identify-blob-attached-to-a-vm"></a>Stap 1: Blob is gekoppeld aan een VM identificeren

### <a name="scenario-1-deleting-a-blob--identify-attached-vm"></a>Scenario 1: Verwijderen van een blob – gekoppelde VM identificeren
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

### <a name="scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms"></a>Scenario 2: Verwijderen van een container - alle blob(s) in container die zijn gekoppeld aan virtuele machines identificeren
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu Hub **alle resources**. Ga naar het opslagaccount onder **Blob-Service** Selecteer **Containers**, en zoek de container moet worden verwijderd.
3. Klik om te openen van de container en wordt de lijst met blobs erin weergegeven. Identificeren van de blobs met Blobtype = **pagina-blob** en status van de Lease = **huurlijnen** uit deze lijst. Ga als volgt [Scenario 1](#step-1-identify-blobs-attached-to-a-vm) voor het identificeren van de virtuele machine die is gekoppeld aan elk van deze blobs.

    ![Schermafbeelding van de portal met de Storage-account-blobs en de 'Lease staat' 'Huurlijnen' gemarkeerd](./media/troubleshoot-vhds/utd-disks-sm.png)

4. Ga als volgt [stap 2](#step-2-delete-vm-to-detach-os-disk) en [stap 3](#step-3-detach-data-disk-from-the-vm) verwijderen VM('s) met **OSDisk** ont **DataDisk**. 

### <a name="scenario-3-deleting-storage-account---identify-all-blobs-within-storage-account-that-are-attached-to-vms"></a>Scenario 3: Verwijderen van opslag rekening - alle blob(s) binnen opslagaccount die zijn gekoppeld aan virtuele machines identificeren
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu Hub **alle resources**. Ga naar het opslagaccount onder **Blob-Service** Selecteer **Containers**.

    ![Schermafbeelding van de portal met de storage-account-containers en de 'Lease staat' 'Huurlijnen' gemarkeerd](./media/troubleshoot-vhds/utd-containers-sm.png)

3. In **Containers** deelvenster alle containers identificeren waar **Lease status** is **huurlijnen** en volg [Scenario 2](#scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms) voor elk  **Geleasete** container.
4. Ga als volgt [stap 2](#step-2-delete-vm-to-detach-os-disk) en [stap 3](#step-3-detach-data-disk-from-the-vm) verwijderen VM('s) met **OSDisk** ont **DataDisk**. 

## <a name="step-2-delete-vm-to-detach-os-disk"></a>Stap 2: VM loskoppelen van de besturingssysteemschijf verwijderen
Als de VHD een besturingssysteemschijf is, moet u de virtuele machine verwijderen voordat de gekoppelde VHD kan worden verwijderd. Er is geen verdere actie zijn vereist voor gegevensschijven gekoppeld aan dezelfde virtuele machine nadat deze stappen zijn uitgevoerd:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu Hub **virtuele Machines**.
3. Selecteer de virtuele machine waaraan de VHD is gekoppeld.
4. Zorg ervoor dat er niets is actief met behulp van de virtuele machine, en dat u de virtuele machine niet meer nodig hebt.
5. Aan de bovenkant van de **details van de virtuele Machine** deelvenster **verwijderen**, en klik vervolgens op **Ja** om te bevestigen.
6. De virtuele machine moet worden verwijderd, maar de VHD kan worden bewaard. De VHD wordt echter niet langer moet worden gekoppeld aan een virtuele machine of een lease hebben op het. Het kan enkele minuten duren voordat de lease worden vrijgegeven. Om te bevestigen dat de lease is uitgebracht, blader naar de bloblocatie en in de **Blob-eigenschappen** deelvenster de **Lease Status** moet **beschikbaar**.

## <a name="step-3-detach-data-disk-from-the-vm"></a>Stap 3: Gegevensschijf loskoppelen van de virtuele machine
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

[Storage deletion errors in Resource Manager deployment]: #storage-delete-errors-in-rm


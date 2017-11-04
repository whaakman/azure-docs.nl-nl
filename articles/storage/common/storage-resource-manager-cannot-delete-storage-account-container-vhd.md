---
title: Fouten oplossen wanneer u Azure storage-accounts, containers of VHD's verwijderen | Microsoft Docs
description: Fouten oplossen wanneer u Azure storage-accounts, containers of VHD's verwijderen
services: storage
documentationcenter: 
author: passaree
manager: cshepard
editor: na
tags: storage
ms.assetid: 17403aa1-fe8d-45ec-bc33-2c0b61126286
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 7a3c9422887592c29c2eae8bd75cf960865808fd
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="troubleshoot-storage-delete-errors-in-resource-manager-deployment"></a>Opslag verwijderingsfouten in het Resource Manager-implementatie oplossen
Dit artikel bevat richtlijnen voor probleemoplossing bij een van de volgende fouten optreden tijdens het verwijderen van Azure storage-account-container of blob in Azure Resource Manager-implementatie.

>**Storage-account 'StorageAccountName' verwijderen is mislukt. Fout: Het storage-account kan niet worden verwijderd omdat de artefacten die in gebruik.**

>**# Buiten # container (s) verwijderen is mislukt:<br>VHD's: Er is momenteel een lease voor de container en geen lease-ID is opgegeven in de aanvraag.**

>**# Buiten # blobs verwijderen is mislukt:<br>BlobName.vhd: Er is momenteel een lease op de blob en geen lease-ID is opgegeven in de aanvraag.**

De virtuele harde schijven gebruikt in Azure VM's zijn VHD-bestanden die zijn opgeslagen als pagina-blobs in een standard- of premium storage-account in Azure.  Meer informatie over Azure-schijven kunt u vinden [hier](../../virtual-machines/windows/about-disks-and-vhds.md). Azure voorkomt u dat de verwijdering van een schijf die is gekoppeld aan een virtuele machine om beschadiging te voorkomen. Dit voorkomt ook dat de verwijdering van containers en storage-accounts waarvoor een pagina-blob die is gekoppeld aan een virtuele machine. 

## <a name="solution"></a>Oplossing
Het proces voor het verwijderen van een opslagaccount, container of blob tijdens het ontvangen van een van de bovenstaande fouten is: 
1. [Blobs die zijn gekoppeld aan een VM identificeren](#step-1-identify-blobs-attached-to-a-vm)
2. [Virtuele machines verwijderen met gekoppeld **besturingssysteemschijf**](#step-2-delete-vm-to-detach-os-disk)
3. [Los alle **gegevens een of meer schijven** van resterende VM('s)](#step-3-detach-data-disk-from-the-vm)

Verwijderen van opslagaccount, container of blob het opnieuw nadat de bovenstaande stappen zijn voltooid.

### <a name="step-1-identify-blob-attached-to-a-vm"></a>Stap 1: Blob is gekoppeld aan een VM identificeren

#### <a name="scenario-1-deleting-a-blob--identify-attached-vm"></a>Scenario 1: Verwijderen van een blob – gekoppelde VM identificeren
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu Hub **alle resources**. Ga naar het opslagaccount onder **Blob-Service** Selecteer **Containers** en navigeer naar de blob moet worden verwijderd.
3. Als de blob **Lease status** is **huurlijnen**, klik met de rechtermuisknop en selecteer **metagegevens bewerken** om Blob metagegevens deelvenster te openen. 

    ![Schermafbeelding van de portal met de opslag blobs account in en klik met de rechtermuisknop > 'Metagegevens bewerken' hilighted](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/utd-edit-metadata-sm.png)

4. Controleer in het deelvenster voor Blob-metagegevens en leg de waarde voor **MicrosoftAzureCompute_VMName**. Deze waarde is de naam van de virtuele machine waaraan de VHD is gekoppeld. (Zie **belangrijke** als dit veld niet bestaat)
5. Controleer in het deelvenster voor Blob-metagegevens en leg de waarde van **MicrosoftAzureCompute_DiskType**. Hiermee wordt aangeduid als de gekoppelde schijf OS- of schijf (Zie **belangrijke** als dit veld niet bestaat). 

     ![Schermafbeelding van de portal, met het deelvenster van de 'Blobmetagegevens' opslag openen](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/utd-blob-metadata-sm.png)

6. Als het schijftype blob **OSDisk** Volg [stap 2: verwijderen VM loskoppelen van de besturingssysteemschijf](#step-2-delete-vm-to-detach-os-disk). Anders wordt als het schijftype blob **DataDisk** Volg de stappen in [stap 3: gegevensschijf loskoppelen van de virtuele machine](#step-3-detach-data-disk-from-the-vm). 

> [!IMPORTANT]
> Als **MicrosoftAzureCompute_VMName** en **MicrosoftAzureCompute_DiskType** worden niet weergegeven in de blobmetagegevens van de, betekent dit dat de blob expliciet geleasete is en is niet gekoppeld aan een virtuele machine. Geleaste blobs kunnen niet worden verwijderd zonder dat u de lease eerste. Lease opsplitsen, klik met de rechtermuisknop op de blob en selecteren **einde lease**. Geleaste blobs die niet zijn gekoppeld aan een virtuele machine voorkomen dat de blob wordt verwijderd, maar niet voorkomen dat verwijdering van de account-container of opslag.

#### <a name="scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms"></a>Scenario 2: Verwijderen van een container - alle blob(s) in container die zijn gekoppeld aan virtuele machines identificeren
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu Hub **alle resources**. Ga naar het opslagaccount onder **Blob-Service** Selecteer **Containers** en zoek de container moet worden verwijderd.
3. Klik om te openen van de container en wordt de lijst met blobs erin weergegeven. Identificeren van de blobs met Blobtype = **pagina-blob** en status van de Lease = **huurlijnen** uit deze lijst. Ga als volgt [Scenario 1](#step-1-identify-blobs-attached-to-a-vm) voor het identificeren van de virtuele machine die is gekoppeld aan elk van deze blobs.

    ![Schermafbeelding van de portal met de Storage-account-blobs en de 'Lease staat' 'Huurlijnen' gemarkeerd](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/utd-disks-sm.png)

4. Ga als volgt [stap 2](#step-2-delete-vm-to-detach-os-disk) en [stap 3](#step-3-detach-data-disk-from-the-vm) verwijderen VM('s) met **OSDisk** ont **DataDisk**. 

#### <a name="scenario-3-deleting-storage-account---identify-all-blobs-within-storage-account-that-are-attached-to-vms"></a>Scenario 3: Verwijderen van opslag rekening - alle blob(s) binnen opslagaccount die zijn gekoppeld aan virtuele machines identificeren
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu Hub **alle resources**. Ga naar het opslagaccount onder **Blob-Service** Selecteer **Containers**.

    ![Schermafbeelding van de portal met de storage-account-containers en de 'Lease staat' 'Huurlijnen' gemarkeerd](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/utd-containers-sm.png)

3. In **Containers** blade alle containers identificeren waar **Lease status** is **huurlijnen** en volg [Scenario 2](#scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms) voor elk  **Geleasete** container.
4. Ga als volgt [stap 2](#step-2-delete-vm-to-detach-os-disk) en [stap 3](#step-3-detach-data-disk-from-the-vm) verwijderen VM('s) met **OSDisk** ont **DataDisk**. 

### <a name="step-2-delete-vm-to-detach-os-disk"></a>Stap 2: VM loskoppelen van de besturingssysteemschijf verwijderen
Als de VHD een besturingssysteemschijf is, moet u de virtuele machine verwijderen voordat de gekoppelde VHD kan worden verwijderd. Er is geen verdere actie zijn vereist voor gegevensschijven gekoppeld aan dezelfde virtuele machine nadat deze stappen zijn uitgevoerd:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu Hub **virtuele Machines**.
3. Selecteer de virtuele machine waaraan de VHD is gekoppeld.
4. Zorg ervoor dat er niets is actief met behulp van de virtuele machine, en dat u de virtuele machine niet meer nodig hebt.
5. Aan de bovenkant van de **details van de virtuele Machine** blade Selecteer **verwijderen**, en klik vervolgens op **Ja** om te bevestigen.
6. De virtuele machine moet worden verwijderd, maar de VHD kan worden bewaard. De VHD wordt echter niet langer moet worden gekoppeld aan een virtuele machine of een lease hebben op het. Het kan enkele minuten duren voordat de lease worden vrijgegeven. Om te bevestigen dat de lease is uitgebracht, blader naar de bloblocatie en in de **Blob-eigenschappen** deelvenster de **Lease Status** moet **beschikbaar**.

### <a name="step-3-detach-data-disk-from-the-vm"></a>Stap 3: Gegevensschijf loskoppelen van de virtuele machine
Als de VHD een gegevensschijf is, ontkoppel het VHD van de virtuele machine te verwijderen van de lease:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu Hub **virtuele Machines**.
3. Selecteer de virtuele machine waaraan de VHD is gekoppeld.
4. Selecteer **schijven** op de **details van de virtuele Machine** blade.
5. Selecteer de gegevensschijf moet worden verwijderd de VHD is gekoppeld aan. U kunt bepalen welke blob in de schijf is gekoppeld aan de hand van de URL van de VHD.
6. U kunt de bloblocatie controleren door te klikken op de schijf om te controleren van het pad **VHD-URI** veld.
7. Selecteer **bewerken** op boven van **schijven** blade.
8. Klik op **pictogram loskoppelen** van de gegevensschijf moet worden verwijderd.

     ![Schermafbeelding van de portal, met het deelvenster van de 'Blobmetagegevens' opslag openen](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/utd-vm-disks-edit.png)

9. Selecteer **Opslaan**. De schijf moet nu worden ontkoppeld van de virtuele machine en de VHD mag niet langer een lease erop. Het kan enkele minuten duren voordat de lease worden vrijgegeven. Om te bevestigen dat de lease is vrijgegeven, blader naar de bloblocatie en in de **Blob-eigenschappen** deelvenster de **Lease Status** waarde moet **ontgrendeld** of **Beschikbaar**.

## <a name="next-steps"></a>Volgende stappen
Probeer de verwijdering van opslagobject dat eerder is mislukt.


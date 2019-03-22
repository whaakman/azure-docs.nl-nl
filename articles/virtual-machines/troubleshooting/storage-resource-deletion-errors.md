---
title: Storage resource verwijderen oplossen op virtuele Linux-machines in Azure | Microsoft Docs
description: Klik hier voor meer informatie over het oplossen van problemen bij het verwijderen van storage-resources met gekoppelde VHD's.
keywords: ''
services: virtual-machines
author: genlin
manager: cshepard
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 11/01/2018
ms.author: genli
ms.openlocfilehash: a1eb946d3f1b18aaa86735dedcfbaa1fd6a89621
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58089978"
---
# <a name="troubleshoot-storage-resource-deletion-errors"></a>Oplossen van fouten bij het verwijderen van het storage-resource

In bepaalde scenario's, verschijnt een van de volgende fouten zijn opgetreden terwijl u probeert te verwijderen van een Azure storage-account, container of blob in een Azure Resource Manager-implementatie:

> **Storage-account 'StorageAccountName' verwijderen is mislukt. Fout: Het storage-account kan niet worden verwijderd omdat de artefacten die in gebruik.**
> 
> **# Buiten # container (s) verwijderen is mislukt:<br>VHD's: Er is momenteel een lease voor de container en er is geen lease-ID is opgegeven in de aanvraag.**
> 
> **# Buiten # blobs verwijderen is mislukt:<br>BlobName.vhd: Er is momenteel een lease op de blob en er is geen lease-ID is opgegeven in de aanvraag.**

De VHD's in virtuele machines van Azure zijn .vhd-bestanden die zijn opgeslagen als pagina-blobs in een standard- of premium storage-account in Azure. Zie voor meer informatie over Azure-schijven, onze [Inleiding tot beheerde schijven](../linux/managed-disks-overview.md).

Azure wordt voorkomen dat het verwijderen van een schijf die is gekoppeld aan een virtuele machine om beschadiging te voorkomen. Dit voorkomt ook dat het verwijderen van containers en storage-accounts waarvoor een pagina-blob die is gekoppeld aan een virtuele machine. 

Het proces voor het verwijderen van een opslagaccount, container of blob bij de ontvangst van een van deze fouten is: 
1. Blobs die zijn gekoppeld aan een virtuele machine identificeren
2. [VM's verwijderen met gekoppeld **besturingssysteemschijf**](#step-2-delete-vm-to-detach-os-disk)
3. [Los alle **gegevensschijven** van resterende of meer virtuele machines](#step-3-detach-data-disk-from-the-vm)

Opnieuw proberen te verwijderen van de storage-account, container of blob nadat deze stappen zijn voltooid.

## <a name="step-1-identify-blob-attached-to-a-vm"></a>Stap 1: Identificeren van de blob die is gekoppeld aan een virtuele machine

### <a name="scenario-1-deleting-a-blob--identify-attached-vm"></a>Scenario 1: Verwijderen van een blob-gekoppelde virtuele machine identificeren
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu Hub **alle resources**. Ga naar het opslagaccount onder **Blob-Service** Selecteer **Containers**, en navigeer naar de blob te verwijderen.
3. Als de blob **leasestatus** is **geleased**, met de rechtermuisknop op en selecteer **metagegevens bewerken** om Blob-metagegevens deelvenster te openen. 

    ![Schermafbeelding van de portal, met de Storage-blobs-account en klik met de rechtermuisknop > "bewerken metagegevens' is gemarkeerd](./media/troubleshoot-vhds/utd-edit-metadata-sm.png)

4. Controleer in de Blob-metagegevens in het deelvenster en noteer de waarde voor **MicrosoftAzureCompute_VMName**. Deze waarde is de naam van de virtuele machine waaraan de VHD is gekoppeld. (Zie **belangrijk** als dit veld niet bestaat)
5. Controleer in de Blob-metagegevens in het deelvenster en noteer de waarde van **MicrosoftAzureCompute_DiskType**. Deze waarde wordt aangeduid als de gekoppelde schijf besturingssysteem of de gegevensschijf (Zie **belangrijk** als dit veld niet bestaat). 

     ![Schermafbeelding van de portal, met het deelvenster van de "Blob-metagegevens" opslag openen](./media/troubleshoot-vhds/utd-blob-metadata-sm.png)

6. Als de blob-schijftype **OSDisk** Volg [stap 2: Virtuele machine als u wilt loskoppelen van de schijf met besturingssysteem verwijdert](#step-2-delete-vm-to-detach-os-disk). Als het schijftype blob **DataDisk** Volg de stappen in [stap 3: Een gegevensschijf van de virtuele machine ontkoppelen](#step-3-detach-data-disk-from-the-vm). 

> [!IMPORTANT]
> Als **MicrosoftAzureCompute_VMName** en **MicrosoftAzureCompute_DiskType** worden niet weergegeven in de metagegevens van de blob, betekent dit dat de blob-lease expliciet en is niet gekoppeld aan een virtuele machine. Lease blobs kunnen niet worden verwijderd zonder het verbreken van de lease eerste. Lease verbreken, met de rechtermuisknop op de blob en selecteert u **lease Break**. Lease blobs die niet zijn gekoppeld aan een virtuele machine voorkomen dat de blob wordt verwijderd, maar nog steeds verwijderen van de container of storage-account.

### <a name="scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms"></a>Scenario 2: Verwijderen van een container - alle blobs in de container die zijn gekoppeld aan VM's identificeren
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu Hub **alle resources**. Ga naar het opslagaccount onder **Blob-Service** Selecteer **Containers**, en zoek de container moet worden verwijderd.
3. Klik om te openen van de container en wordt de lijst met blobs erin weergegeven. Identificeren van de blobs met Blob-Type = **pagina-blob** en leasestatus = **geleased** uit deze lijst. Ga als volgt Scenario 1 voor het identificeren van de virtuele machine die is gekoppeld aan elk van deze blobs.

    ![Schermafbeelding van de portal, met de opslagaccountblobs en de 'leasestatus"met 'Geleased' is gemarkeerd](./media/troubleshoot-vhds/utd-disks-sm.png)

4. Ga als volgt [stap 2](#step-2-delete-vm-to-detach-os-disk) en [stap 3](#step-3-detach-data-disk-from-the-vm) te verwijderen of meer virtuele machines met **OSDisk** en loskoppelen **DataDisk**. 

### <a name="scenario-3-deleting-storage-account---identify-all-blobs-within-storage-account-that-are-attached-to-vms"></a>Scenario 3: Verwijderen van storage-account - alle blobs in de storage-account die zijn gekoppeld aan VM's identificeren
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu Hub **alle resources**. Ga naar het opslagaccount onder **Blob-Service** Selecteer **Blobs**.
3. In **Containers** deelvenster identificeren van alle containers waar **leasestatus** is **geleased** en volg [Scenario 2](#scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms) voor elk  **Lease** container.
4. Ga als volgt [stap 2](#step-2-delete-vm-to-detach-os-disk) en [stap 3](#step-3-detach-data-disk-from-the-vm) te verwijderen of meer virtuele machines met **OSDisk** en loskoppelen **DataDisk**. 

## <a name="step-2-delete-vm-to-detach-os-disk"></a>Stap 2: Virtuele machine als u wilt loskoppelen van de schijf met besturingssysteem verwijderen
Als de VHD een besturingssysteemschijf is, moet u de virtuele machine verwijderen voordat de gekoppelde VHD kan worden verwijderd. Er is geen verdere actie is vereist voor gegevensschijven gekoppeld aan dezelfde virtuele machine nadat deze stappen zijn uitgevoerd:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu Hub **virtuele Machines**.
3. Selecteer de virtuele machine waaraan de VHD is gekoppeld.
4. Zorg ervoor dat er niets is actief met behulp van de virtuele machine, en dat u de virtuele machine niet meer nodig hebt.
5. Aan de bovenkant van de **details van de virtuele Machine** venster **verwijderen**, en klik vervolgens op **Ja** om te bevestigen.
6. De virtuele machine moet worden verwijderd, maar de VHD kan worden bewaard. De VHD wordt echter langer moet worden gekoppeld aan een virtuele machine of een lease daarop hebben. Het duurt een paar minuten voor de lease worden vrijgegeven. Om te controleren dat de lease wordt vrijgegeven, blader naar de blob-locatie en in de **Blob-eigenschappen** in het deelvenster de **Lease-Status** moet **beschikbaar**.

## <a name="step-3-detach-data-disk-from-the-vm"></a>Stap 3: Van de virtuele machine een gegevensschijf ontkoppelen
Als de VHD een gegevensschijf wordt loskoppelen van de VHD van de virtuele machine te verwijderen van de lease:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu Hub **virtuele Machines**.
3. Selecteer de virtuele machine waaraan de VHD is gekoppeld.
4. Selecteer **schijven** op de **details van de virtuele Machine** deelvenster.
5. Selecteer de gegevensschijf moet worden verwijderd de VHD is gekoppeld aan. U kunt bepalen welke blob door het controleren van de URL van de VHD in de schijf is gekoppeld.
6. U kunt controleren of de blob-locatie door te klikken op de schijf om te controleren of het pad **VHD-URI** veld.
7. Selecteer **bewerken** boven in het **schijven** deelvenster.
8. Klik op **loskoppelen pictogram** van de gegevensschijf moet worden verwijderd.

     ![Schermafbeelding van de portal, met het deelvenster van de "Blob-metagegevens" opslag openen](./media/troubleshoot-vhds/utd-vm-disks-edit.png)

9. Selecteer **Opslaan**. De schijf wordt nu losgekoppeld van de virtuele machine, en de VHD is niet meer in lease gegeven. Het duurt een paar minuten voor de lease worden vrijgegeven. Om te controleren dat de lease is vrijgegeven, blader naar de blob-locatie en in de **Blob-eigenschappen** in het deelvenster de **Lease-Status** waarde moet **ontgrendeld** of **Beschikbaar**.

[Storage deletion errors in Resource Manager deployment]: #storage-delete-errors-in-rm


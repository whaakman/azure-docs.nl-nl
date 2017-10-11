---
title: Maken en een Linux-VHD uploaden naar Azure | Microsoft Docs
description: Maken en uploaden van een Azure virtuele harde schijf (VHD) met het Linux-besturingssysteem met behulp van het klassieke implementatiemodel
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 8058ff98-db03-4309-9bf4-69842bd64dd4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: iainfou
ms.openlocfilehash: 23c30c954875598ce3e01db137b0ef8cda9779f4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="creating-and-uploading-a-virtual-hard-disk-that-contains-the-linux-operating-system"></a>Een virtuele harde schijf met het Linux-besturingssysteem maken en uploaden
> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. U kunt ook [uploaden van een aangepaste installatiekopie met Azure Resource Manager](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

In dit artikel leest u hoe maken en uploaden van een virtuele harde schijf (VHD), zodat u deze als uw eigen installatiekopie gebruiken kunt maken van virtuele machines in Azure. Informatie over het voorbereiden van het besturingssysteem, zodat u deze gebruiken kunt voor het maken van meerdere virtuele machines op basis van die installatiekopie. 


## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u de volgende items hebt:

* **Linux-besturingssysteem is geïnstalleerd in een .vhd-bestand** -u hebt geïnstalleerd een [door Azure goedgekeurde Linux-distributie](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (of Raadpleeg [informatie voor niet-goedgekeurde distributies](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) naar een virtuele schijf in de VHD-indeling. Er bestaan meerdere hulpprogramma's om een virtuele machine en de VHD te maken:
  * Installeer en configureer [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) of [KVM](http://www.linux-kvm.org/page/RunningKVM), zorg ervoor dat de VHD gebruiken als uw afbeeldingsindeling. Indien nodig, kunt u [converteren van een installatiekopie van een](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) met `qemu-img convert`.
  * U kunt ook de Hyper-V [op Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) of [op Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> De nieuwe VHDX-indeling wordt niet ondersteund in Azure. Wanneer u een virtuele machine maakt, geeft u de VHD als de notatie. Indien nodig, kunt u de VHDX-schijven converteren naar VHD gebruiken [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) of de [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-cmdlet. Azure biedt bovendien geen ondersteuning dynamische VHD's uploaden, dus u deze schijven worden geconverteerd naar vaste VHD's moet voordat u uploadt. U kunt hulpprogramma's gebruiken zoals [Azure VHD-hulpprogramma's voor Ga](https://github.com/Microsoft/azure-vhd-utils-for-go) naar dynamische schijven converteren tijdens het proces van het uploaden naar Azure.

* **Azure-opdrachtregelinterface** -Installeer de meest recente [Azure-opdrachtregelinterface](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) om de VHD te uploaden.

<a id="prepimage"> </a>

## <a name="step-1-prepare-the-image-to-be-uploaded"></a>Stap 1: Bereid de afbeelding die moet worden geüpload
Azure biedt ondersteuning voor verschillende Linux-distributies (Zie [goedgekeurde distributies](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). De volgende artikelen helpen u bij het voorbereiden van de verschillende Linux-distributies die worden ondersteund in Azure. Nadat u de stappen in de volgende handleidingen hebt voltooid, kun je hier als u een VHD-bestand dat is gereed om te uploaden naar Azure hebt:

* **[Op basis van centOS distributies](../create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](../debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](../oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](../redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](../suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](../create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Andere - niet-goedgekeurde distributies](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

> [!NOTE]
> De Azure-platform SLA van toepassing op virtuele machines met Linux-besturingssysteem alleen als een van de aangebracht distributies wordt gebruikt met de configuratie van de gegevens zoals opgegeven in de ondersteunde versies' in [Linux op Azure-Endorsed distributies](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Alle Linux-distributies in de afbeelding voor Azure-galerie zijn aangebracht distributies en de vereiste configuratie.
> 
> 

Zie ook de  **[opmerkingen bij de installatie van Linux](../create-upload-generic.md#general-linux-installation-notes)**  voor meer algemene tips voor het Linux-installatiekopieën voorbereiden voor Azure.

<a id="connect"> </a>

## <a name="step-2-prepare-the-connection-to-azure"></a>Stap 2: De verbinding met Azure voorbereiden
Controleer of u de Azure CLI in het klassieke implementatiemodel (`azure config mode asm`), meld u vervolgens aan bij uw account:

```azurecli
azure login
```


<a id="upload"> </a>

## <a name="step-3-upload-the-image-to-azure"></a>Stap 3: De installatiekopie uploaden naar Azure
U moet een opslagaccount voor uw VHD-bestand te uploaden. Kunt u een bestaand opslagaccount kiezen of [Maak een nieuwe](../../../storage/common/storage-create-storage-account.md).

De Azure CLI gebruiken voor het uploaden van de installatiekopie met behulp van de volgende opdracht:

```azurecli
azure vm image create <ImageName> `
    --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
    --os Linux <PathToVHDFile>
```

In het vorige voorbeeld:

* **BlobStorageURL** is de URL voor het opslagaccount dat u wilt gebruiken
* **YourImagesFolder** is een container in blob storage waar u uw installatiekopieën opslaan
* **VHDName** wordt het label dat wordt weergegeven in de portal voor het identificeren van de virtuele harde schijf.
* **PathToVHDFile** is het volledige pad en de naam van het VHD-bestand op uw computer.

Een compleet voorbeeld ziet u de volgende opdracht:

```azurecli
azure vm image create myImage `
    --blob-url https://mystorage.blob.core.windows.net/vhds/myimage.vhd `
    --os Linux /home/ahmet/myimage.vhd
```

## <a name="step-4-create-a-vm-from-the-image"></a>Stap 4: Een virtuele machine van de installatiekopie maken
U maakt een virtuele machine met `azure vm create` op dezelfde manier als een gewone virtuele machine. Geef de naam die u hebt uw installatiekopie gegeven in de vorige stap. In het volgende voorbeeld gebruiken we de **myImage** de naam van de installatiekopie is opgegeven in de vorige stap:

```azurecli
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
    --location "West US" "myDeployedVM" myImage
```

Geef uw eigen gebruikersnaam en wachtwoord, locatie, DNS-naam en installatiekopie met de naam voor het maken van uw eigen virtuele machines.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie [Azure CLI-verwijzing voor het klassieke implementatiemodel Azure](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

[Step 1: Prepare the image to be uploaded]:#prepimage
[Step 2: Prepare the connection to Azure]:#connect
[Step 3: Upload the image to Azure]:#upload

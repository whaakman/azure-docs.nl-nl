---
title: Het maken van de installatiekopie van een on-premises virtuele machine voor de Azure Marketplace | Microsoft Docs
description: Inzicht in en voer de stappen uit voor een on-premises VM-installatiekopie maken en implementeren op Azure Marketplace voor anderen om aan te schaffen.
services: marketplace-publishing
documentationcenter: ''
author: msmbaldwin
manager: mbaldwin
editor: ''
ms.assetid: 26dfbd5a-8685-4b19-987e-c20ca60540ec
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: mbaldwin
ms.openlocfilehash: 58be44e05a0b293b1f8f200cb01b4a483bae10b2
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39006509"
---
# <a name="develop-an-on-premises-virtual-machine-image-for-the-azure-marketplace"></a>De installatiekopie van een on-premises virtuele machine ontwikkelen voor Azure Marketplace
Het is raadzaam dat u Azure virtuele harde schijven (VHD's) rechtstreeks in de cloud ontwikkelen met behulp van Remote Desktop Protocol. Als u moet is het echter mogelijk om te downloaden van een VHD en het ontwikkelen met behulp van on-premises infrastructuur.  

Voor de ontwikkeling van on-premises, moet u de VHD van de gemaakte virtuele machine van het besturingssysteem downloaden. Deze stappen kunnen worden uitgevoerd als onderdeel van de stap 3.3, hierboven.  

## <a name="download-a-vhd-image"></a>Downloaden van een VHD-installatiekopie
### <a name="locate-a-blob-url"></a>Ga naar een blob-URL
Om te kunnen downloaden van de VHD, moet u eerst de blob-URL voor de besturingssysteemschijf vinden.

Ga naar de blob-URL van de nieuwe [Microsoft Azure portal](https://portal.azure.com):

1. Ga naar **Bladeren** > **VMs**, en selecteer vervolgens de geïmplementeerde virtuele machine.
2. Onder **configureren**, selecteer de **schijven** tegel, die de schijven-blade wordt geopend.
   
   ![tekenen](media/marketplace-publishing-vm-image-creation-on-premise/img01.png)
3. Selecteer de **Besturingssysteemschijf**, die een andere blade met de eigenschappen van de schijf, met inbegrip van de locatie van de VHD wordt geopend.
4. Kopieer deze blob-URL.
   
   ![tekenen](media/marketplace-publishing-vm-image-creation-on-premise/img02.png)
5. Verwijder de geïmplementeerde virtuele machine nu zonder te verwijderen van de back-ups maken-schijven. U kunt ook de virtuele machine in plaats van het verwijderen stoppen. Download het VHD-besturingssysteem niet wanneer de virtuele machine wordt uitgevoerd.
   
   ![tekenen](media/marketplace-publishing-vm-image-creation-on-premise/img03.png)

### <a name="download-a-vhd"></a>Een VHD downloaden
Nadat u de blob-URL weet, kunt u de VHD downloaden met behulp van de [Azure-portal](http://manage.windowsazure.com/) of PowerShell.  

> [!NOTE]
> Op het moment van deze handleiding voor het maken van is de functionaliteit voor het downloaden van een VHD nog niet aanwezig in de nieuwe Microsoft Azure-portal.  
> 
> 

**Downloaden van het besturingssysteem VHD via de huidige [Azure-portal](http://manage.windowsazure.com/)**

1. Meld u aan de Azure-portal als u dit nog niet hebt gedaan.
2. Klik op de **opslag** tabblad.
3. Selecteer het opslagaccount waarin de VHD is opgeslagen.
   
   ![tekenen](media/marketplace-publishing-vm-image-creation-on-premise/img04.png)
4. U ziet nu eigenschappen van het opslagaccount. Selecteer de **Containers** tabblad.
   
   ![tekenen](media/marketplace-publishing-vm-image-creation-on-premise/img05.png)
5. Selecteer de container waar de VHD is opgeslagen. Standaard wordt de VHD opgeslagen in een VHD-container wanneer gemaakt op basis van de portal.
   
   ![tekenen](media/marketplace-publishing-vm-image-creation-on-premise/img06.png)
6. Selecteer het juiste besturingssysteem VHD door het vergelijken van de URL van de reeks die u hebt opgeslagen.
7. Klik op **Downloaden**.
   
   ![tekenen](media/marketplace-publishing-vm-image-creation-on-premise/img07.png)

### <a name="download-a-vhd-by-using-powershell"></a>Een VHD downloaden met behulp van PowerShell
Naast het gebruik van de Azure-portal, kunt u de [opslaan-AzureVhd](http://msdn.microsoft.com/library/dn495297.aspx) cmdlet voor het downloaden van het besturingssysteem-VHD.

        Save-AzureVhd –Source <storageURIOfVhd> `
        -LocalFilePath <diskLocationOnWorkstation> `
        -StorageKey <keyForStorageAccount>
Bijvoorbeeld opslaan-AzureVhd-bron 'https://baseimagevm.blob.core.windows.net/vhds/BaseImageVM-6820cq00-BaseImageVM-os-1411003770191.vhd' - LocalFilePath "C:\Users\Administrator\Desktop\baseimagevm.vhd" - StorageKey <String>

> [!NOTE]
> **Opslaan-AzureVhd** heeft ook een **NumberOfThreads** -optie die kan worden gebruikt voor het verhogen van parallelle uitvoering zodat het beste gebruik van de beschikbare bandbreedte voor het downloaden.
> 
> 

## <a name="upload-vhds-to-an-azure-storage-account"></a>VHD's uploaden naar Azure storage-account
Als u uw VHD's on-premises hebt voorbereid, moet u ze uploaden naar een opslagaccount in Azure. Deze stap plaatsvindt na het maken van uw VHD on-premises, maar voordat het certificaat voor uw VM-installatiekopie verkrijgen.

### <a name="create-a-storage-account-and-container"></a>Een storage-account en een container maken
U wordt aangeraden dat de VHD's worden geüpload naar een opslagaccount in een regio in de Verenigde Staten. Alle VHD's voor een enkele SKU moeten worden geplaatst in een enkele container binnen een enkel opslagaccount.

Voor het maken van een storage-account, kunt u de [Microsoft Azure portal](https://portal.azure.com/), PowerShell of de Linux-opdrachtregelprogramma.  

**Storage-account maken vanuit de Microsoft Azure portal**

1. Klik op **Een resource maken**.
2. Selecteer **Opslag**.
3. Vul in de naam van het opslagaccount en selecteer vervolgens een locatie.
   
   ![tekenen](media/marketplace-publishing-vm-image-creation-on-premise/img08.png)
4. Klik op **Create**.
5. De blade voor het opslagaccount moet open zijn. Als dit niet het geval is, selecteert u **Bladeren** > **Opslagaccounts**. Selecteer op de blade Opslagaccount, de storage-account gemaakt.
6. Selecteer **Containers**.
   
   ![tekenen](media/marketplace-publishing-vm-image-creation-on-premise/img09.png) 
7. Selecteer op de blade Containers **toevoegen**, en voer de containernaam van een en de containermachtigingen. Selecteer **persoonlijke** voor containermachtigingen.

> [!TIP]
> Het is raadzaam dat u maakt een container per SKU die u van plan bent om te publiceren.
> 
> 

  ![tekenen](media/marketplace-publishing-vm-image-creation-on-premise/img10.png)

### <a name="create-a-storage-account-by-using-powershell"></a>Een opslagaccount maken met behulp van PowerShell
Met behulp van PowerShell een storage-account maken met behulp van de [New-AzureStorageAccount](http://msdn.microsoft.com/library/dn495115.aspx) cmdlet.

        New-AzureStorageAccount -StorageAccountName “mystorageaccount” -Location “West US”

U kunt een container in het storage-account maken met behulp van de [NewAzureStorageContainer](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontainer) cmdlet.

        New-AzureStorageContainer -Name “containername” -Permission “Off”

> [!NOTE]
> Deze opdrachten wordt ervan uitgegaan dat de huidige context van de storage-account is al ingesteld in PowerShell.   Raadpleeg [instellen van Azure PowerShell](marketplace-publishing-powershell-setup.md) voor meer informatie over PowerShell-installatie.  
> 
> ### <a name="create-a-storage-account-by-using-the-command-line-tool-for-mac-and-linux"></a>Een opslagaccount maken met behulp van de opdrachtregel-hulpprogramma voor Mac en Linux
> Van [Linux-opdrachtregelprogramma](../virtual-machines/linux/cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), als volgt een opslagaccount maken.
> 
> 

        azure storage account create mystorageaccount --location "West US"

Als volgt te werk om een container te maken.

        azure storage container create containername --account-name mystorageaccount --accountkey <accountKey>

## <a name="upload-a-vhd"></a>Een VHD uploaden
Nadat het opslagaccount en container zijn gemaakt, kunt u uw voorbereide VHD's uploaden. U kunt PowerShell, het Linux-opdrachtregelprogramma of andere Azure Storage-beheerhulpprogramma's gebruiken.

### <a name="upload-a-vhd-via-powershell"></a>Uploaden van een VHD via PowerShell
Gebruik de [Add-AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) cmdlet.

        Add-AzureVhd –Destination “http://mystorageaccount.blob.core.windows.net/containername/vmsku.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\vmsku.vhd”

### <a name="upload-a-vhd-by-using-the-command-line-tool-for-mac-and-linux"></a>Een VHD uploaden met behulp van de opdrachtregel-hulpprogramma voor Mac en Linux
Met de [Linux-opdrachtregelprogramma](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2), gebruikt u de volgende: azure vm-installatiekopie maken <image name> --locatie <Location of the data center> --OS Linux <LocationOfLocalVHD>

## <a name="see-also"></a>Zie ook
* [Het maken van een VM-installatiekopie voor de Marketplace](marketplace-publishing-vm-image-creation.md)
* [Instellen van Azure PowerShell](marketplace-publishing-powershell-setup.md)


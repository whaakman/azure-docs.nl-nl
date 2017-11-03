---
title: Maken van een installatiekopie van een lokale virtuele machine voor Azure Marketplace | Microsoft Docs
description: Inzicht en voer de stappen voor de installatiekopie van een lokale virtuele machine maken en implementeren in Azure Marketplace voor anderen om aan te schaffen.
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 26dfbd5a-8685-4b19-987e-c20ca60540ec
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: 8f6b9a9293dc149586e6e5fd55028170ea825b07
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="develop-an-on-premises-virtual-machine-image-for-the-azure-marketplace"></a>Een installatiekopie van een lokale virtuele machine ontwikkelen voor Azure Marketplace
Wij raden u aan Azure virtuele harde schijven (VHD's) rechtstreeks in de cloud te ontwikkelen met behulp van Remote Desktop Protocol. Als u moet is het echter mogelijk te downloaden van een VHD en het ontwikkelen met behulp van on-premises infrastructuur.  

Voor lokale ontwikkeling, moet u de VHD van de gemaakte virtuele machine van het besturingssysteem downloaden. Deze stappen zou worden uitgevoerd als onderdeel van de stap 3.3, hierboven.  

## <a name="download-a-vhd-image"></a>Een VHD-installatiekopie te downloaden
### <a name="locate-a-blob-url"></a>Ga naar een blob-URL
Om te downloaden van de VHD, moet u eerst de blob-URL voor de besturingssysteemschijf vinden.

Zoek de blob-URL van de nieuwe [Microsoft Azure-portal](https://portal.azure.com):

1. Ga naar **Bladeren** > **VMs**, en selecteer vervolgens de geïmplementeerde virtuele machine.
2. Onder **configureren**, selecteer de **schijven** tegel, waarmee u de blade schijven opent.
   
   ![tekenen](media/marketplace-publishing-vm-image-creation-on-premise/img01.png)
3. Selecteer de **Besturingssysteemschijf**, die een andere blade die wordt weergegeven van de eigenschappen van de schijf, met inbegrip van de VHD-locatie wordt geopend.
4. Kopieer deze blob-URL.
   
   ![tekenen](media/marketplace-publishing-vm-image-creation-on-premise/img02.png)
5. Verwijder de geïmplementeerde virtuele machine nu zonder te verwijderen van de schijven van de back-ups maken. U kunt ook stop de virtuele machine in plaats van te verwijderen. Download het VHD-besturingssysteem niet wanneer de virtuele machine wordt uitgevoerd.
   
   ![tekenen](media/marketplace-publishing-vm-image-creation-on-premise/img03.png)

### <a name="download-a-vhd"></a>Een VHD downloaden
Nadat u de URL van de blob weet, kunt u de VHD downloaden met behulp van de [Azure-portal](http://manage.windowsazure.com/) of PowerShell.  

> [!NOTE]
> Op het moment van het maken van deze handleiding is de functionaliteit voor het downloaden van een VHD nog niet aanwezig in de nieuwe Microsoft Azure-portal.  
> 
> 

**Downloaden van het besturingssysteem-VHD via de huidige [Azure-portal](http://manage.windowsazure.com/)**

1. Meld u aan bij de Azure portal als u dit nog niet hebt gedaan.
2. Klik op de **opslag** tabblad.
3. Selecteer het opslagaccount waarin de VHD wordt opgeslagen.
   
   ![tekenen](media/marketplace-publishing-vm-image-creation-on-premise/img04.png)
4. U ziet nu eigenschappen van het opslagaccount. Selecteer de **Containers** tabblad.
   
   ![tekenen](media/marketplace-publishing-vm-image-creation-on-premise/img05.png)
5. Selecteer de container waarin de VHD wordt opgeslagen. Standaard wordt de VHD opgeslagen in een VHD-container wanneer gemaakt vanuit de portal.
   
   ![tekenen](media/marketplace-publishing-vm-image-creation-on-premise/img06.png)
6. Selecteer het juiste besturingssysteem VHD door het vergelijken van de URL die u hebt opgeslagen.
7. Klik op **Downloaden**.
   
   ![tekenen](media/marketplace-publishing-vm-image-creation-on-premise/img07.png)

### <a name="download-a-vhd-by-using-powershell"></a>Een VHD te downloaden met behulp van PowerShell
Naast de Azure-portal gebruikt, kunt u de [opslaan AzureVhd](http://msdn.microsoft.com/library/dn495297.aspx) cmdlet voor het downloaden van het besturingssysteem-VHD.

        Save-AzureVhd –Source <storageURIOfVhd> `
        -LocalFilePath <diskLocationOnWorkstation> `
        -StorageKey <keyForStorageAccount>
Bijvoorbeeld opslaan-AzureVhd-bron "https://baseimagevm.blob.core.windows.net/vhds/BaseImageVM-6820cq00-BaseImageVM-os-1411003770191.vhd" - LocalFilePath 'C:\Users\Administrator\Desktop\baseimagevm.vhd' - StorageKey<String>

> [!NOTE]
> **Opslaan AzureVhd** heeft ook een **NumberOfThreads** -optie die kan worden gebruikt voor het verhogen van parallelle uitvoering om optimaal gebruik van de beschikbare bandbreedte voor het downloaden.
> 
> 

## <a name="upload-vhds-to-an-azure-storage-account"></a>VHD's uploaden naar Azure storage-account
Als u uw VHD's on-premises voorbereid, moet u ze uploaden naar een opslagaccount in Azure. Deze stap vindt plaats nadat het maken van uw VHD lokale maar voordat het verkrijgen van de certificeringsinstantie voor uw VM-installatiekopie.

### <a name="create-a-storage-account-and-container"></a>Een opslagaccount en container maken
We raden aan dat de VHD's worden geüpload naar een opslagaccount in een regio in de Verenigde Staten. Alle VHD's voor een enkele SKU worden geplaatst in een enkele container binnen een enkele opslagaccount.

Als u wilt een opslagaccount maakt, kunt u de [Microsoft Azure-portal](https://portal.azure.com/), PowerShell of de Linux-opdrachtregelprogramma.  

**Een opslagaccount maken vanuit de Microsoft Azure-portal**

1. Klik op **Nieuw**.
2. Selecteer **opslag**.
3. Vul de opslagaccountnaam en selecteer vervolgens een locatie.
   
   ![tekenen](media/marketplace-publishing-vm-image-creation-on-premise/img08.png)
4. Klik op **Create**.
5. De blade voor het opslagaccount is geopend. Zo niet, selecteer **Bladeren** > **Opslagaccounts**. Selecteer op de blade Opslagaccount, het opslagaccount.
6. Selecteer **Containers**.
   
   ![tekenen](media/marketplace-publishing-vm-image-creation-on-premise/img09.png) 
7. Selecteer op de blade Containers **toevoegen**, en voer vervolgens een containernaam en de machtigingen van de container. Selecteer **persoonlijke** voor machtigingen van de container.

> [!TIP]
> U wordt aangeraden dat u maakt een container per SKU die u van plan bent om te publiceren.
> 
> 

  ![tekenen](media/marketplace-publishing-vm-image-creation-on-premise/img10.png)

### <a name="create-a-storage-account-by-using-powershell"></a>Een opslagaccount maken met behulp van PowerShell
Met behulp van PowerShell, een opslagaccount maken met behulp van de [nieuw AzureStorageAccount](http://msdn.microsoft.com/library/dn495115.aspx) cmdlet.

        New-AzureStorageAccount -StorageAccountName “mystorageaccount” -Location “West US”

Vervolgens u een container in dat storage-account maken met behulp van kunt de [NewAzureStorageContainer](http://msdn.microsoft.com/library/dn495291.aspx) cmdlet.

        New-AzureStorageContainer -Name “containername” -Permission “Off”

> [!NOTE]
> Deze opdrachten wordt ervan uitgegaan dat de huidige context van de storage-account is al ingesteld in PowerShell.   Raadpleeg [instellen van Azure PowerShell](marketplace-publishing-powershell-setup.md) voor meer informatie over de installatie van PowerShell.  
> 
> ### <a name="create-a-storage-account-by-using-the-command-line-tool-for-mac-and-linux"></a>Een opslagaccount maken met behulp van het opdrachtregelprogramma voor Mac en Linux
> Van [Linux opdrachtregelprogramma](../virtual-machines/linux/cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), als volgt een opslagaccount maken.
> 
> 

        azure storage account create mystorageaccount --location "West US"

Als volgt te werk om een container te maken.

        azure storage container create containername --account-name mystorageaccount --accountkey <accountKey>

## <a name="upload-a-vhd"></a>Een VHD uploaden
Nadat het opslagaccount en container zijn gemaakt, kunt u uw voorbereide VHD's uploaden. U kunt PowerShell, het opdrachtregelprogramma Linux of andere Azure Storage management-hulpprogramma's gebruiken.

### <a name="upload-a-vhd-via-powershell"></a>Een VHD via PowerShell uploaden
Gebruik de [Add-AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) cmdlet.

        Add-AzureVhd –Destination “http://mystorageaccount.blob.core.windows.net/containername/vmsku.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\vmsku.vhd”

### <a name="upload-a-vhd-by-using-the-command-line-tool-for-mac-and-linux"></a>Een VHD uploaden met behulp van het opdrachtregelprogramma voor Mac en Linux
Met de [Linux opdrachtregelprogramma](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2), gebruikt u de volgende: azure vm-installatiekopie maken <image name> --locatie <Location of the data center> --OS Linux<LocationOfLocalVHD>

## <a name="see-also"></a>Zie ook
* [De installatiekopie van een virtuele machine maken voor de Marketplace](marketplace-publishing-vm-image-creation.md)
* [Instellen van Azure PowerShell](marketplace-publishing-powershell-setup.md)


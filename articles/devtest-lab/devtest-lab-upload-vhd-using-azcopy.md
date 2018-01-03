---
title: VHD-bestand uploaden naar Azure DevTest Labs met behulp van AzCopy | Microsoft Docs
description: VHD-bestand uploaden naar het lab-opslagaccount met behulp van AzCopy
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: v-craic
ms.openlocfilehash: 11a9d03e62c674c4311c74f78e4cb2e709940941
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="upload-vhd-file-to-labs-storage-account-using-azcopy"></a>VHD-bestand uploaden naar het lab-opslagaccount met behulp van AzCopy

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

In Azure DevTest Labs kunnen VHD-bestanden worden gebruikt voor het maken van aangepaste installatiekopieën die worden gebruikt voor het inrichten van virtuele machines. De volgende stappen doorlopen die u met het AzCopy-opdrachtregelprogramma een VHD-bestand uploaden naar een lab-opslagaccount. Nadat u uw VHD-bestand hebt geüpload de [Vervolgstappen sectie](#next-steps) geeft een lijst van sommige artikelen die laten zien hoe u een aangepaste installatiekopie van het geüploade VHD-bestand. Zie voor meer informatie over schijven en VHD's in Azure [over schijven en virtuele harde schijven voor virtuele machines](../virtual-machines/linux/about-disks-and-vhds.md)

> [!NOTE] 
>  
> AzCopy is een alleen-Windows-opdrachtregelprogramma.

## <a name="step-by-step-instructions"></a>Stapsgewijze instructies

De volgende stappen helpt u bij het uploaden van een VHD-bestand voor het gebruik van Azure DevTest Labs [AzCopy](http://aka.ms/downloadazcopy). 

1. Haal de naam van het lab-opslagaccount met de Azure portal:

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selecteer **Meer services** en selecteer in de lijst vervolgens **DevTest Labs**.

1. Selecteer de gewenste testomgeving uit de lijst van labs.  

1. Selecteer op de labblade **configuratie**. 

1. Op de testomgeving **configuratie** blade Selecteer **aangepaste installatiekopieën (VHD's)**.

1. Op de **aangepaste installatiekopieën** blade Selecteer **+ toevoegen**. 

1. Op de **aangepaste installatiekopie** blade Selecteer **VHD**.

1. Op de **VHD** blade Selecteer **een VHD uploaden met PowerShell**.

    ![Virtuele harde schijf met behulp van PowerShell geüpload](./media/devtest-lab-upload-vhd-using-azcopy/upload-image-using-psh.png)

1. De **een installatiekopie uploaden met PowerShell** blade wordt een aanroep van de **Add-AzureVhd** cmdlet. De eerste parameter (*bestemming*) bevat de URI voor een blob-container (*uploadt*) in de volgende indeling:

    ```
    https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...
    ``` 

1. Noteer de volledige URI omdat deze wordt gebruikt in latere stappen.

1. Het VHD-bestand met behulp van AzCopy uploaden:
 
1. [Download en installeer de nieuwste versie van AzCopy](http://aka.ms/downloadazcopy).

1. Open een opdrachtvenster en navigeer naar de installatiemap van AzCopy. U kunt eventueel de installatielocatie van AzCopy toevoegen aan het systeempad. AzCopy is standaard geïnstalleerd op de volgende map:

    ```command-line
    %ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy
    ```

1. Met behulp van de storage-account sleutel en de blob-container URI, voer de volgende opdracht achter de opdrachtprompt. De *vhdFileName* waarde moet tussen aanhalingstekens. Het proces van het uploaden van een VHD-bestand kan worden langdurige, afhankelijk van de grootte van het VHD-bestand en de verbindingssnelheid.   

    ```command-line
    AzCopy /Source:<sourceDirectory> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Pattern:"<vhdFileName>" /BlobType:page
    ```

## <a name="next-steps"></a>Volgende stappen

- [Een aangepaste installatiekopie maken in Azure DevTest Labs van een VHD-bestand met de Azure portal](devtest-lab-create-template.md)
- [Een aangepaste installatiekopie maken in Azure DevTest Labs van een VHD-bestand met behulp van PowerShell](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
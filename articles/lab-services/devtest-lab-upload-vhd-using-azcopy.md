---
title: VHD-bestand uploaden naar Azure DevTest Labs met behulp van AzCopy | Microsoft Docs
description: VHD-bestand uploaden naar het lab storage-account met behulp van AzCopy
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 482bfe41ebfec7353466f7e243e9e767caee7828
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51228280"
---
# <a name="upload-vhd-file-to-labs-storage-account-using-azcopy"></a>VHD-bestand uploaden naar het lab storage-account met behulp van AzCopy

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

In Azure DevTest Labs, kunnen de VHD-bestanden worden gebruikt om aangepaste installatiekopieën, die worden gebruikt voor het inrichten van virtuele machines te maken. De volgende stappen helpen u bij het gebruik van de AzCopy-opdrachtregelprogramma voor het uploaden van een VHD-bestand met de storage-account van een lab. Als u uw VHD-bestand hebt geüpload de [sectie volgende stappen](#next-steps) geeft een lijst van sommige artikelen die laten zien hoe u een aangepaste installatiekopie van het geüploade VHD-bestand maken. Zie voor meer informatie over schijven en VHD's in Azure, [over schijven en VHD's voor virtuele machines](../virtual-machines/linux/about-disks-and-vhds.md)

> [!NOTE] 
>  
> AzCopy is een alleen-Windows-opdrachtregelprogramma.

## <a name="step-by-step-instructions"></a>Stapsgewijze instructies

De volgende stappen helpen u bij het uploaden van een VHD-bestand voor het gebruik van Azure DevTest Labs [AzCopy](https://aka.ms/downloadazcopy). 

1. Haal de naam van de storage-account van de testomgeving met behulp van de Azure-portal:

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selecteer **alle services**, en selecteer vervolgens **DevTest Labs** in de lijst.

1. Selecteer de gewenste lab in de lijst met labs.  

1. Selecteer op de blade van de testomgeving **configuratie**. 

1. In het lab **configuratie** Selecteer **aangepaste installatiekopieën (VHD's)**.

1. Op de **aangepaste installatiekopieën** blade, selecteer **+ toevoegen**. 

1. Op de **aangepaste installatiekopie** Selecteer **VHD**.

1. Op de **VHD** Selecteer **uploaden van een VHD met behulp van PowerShell**.

    ![Met behulp van PowerShell VHD uploaden](./media/devtest-lab-upload-vhd-using-azcopy/upload-image-using-psh.png)

1. De **Upload een afbeelding met behulp van PowerShell** blade geeft een aanroep naar de **Add-AzureVhd** cmdlet. De eerste parameter (*bestemming*) bevat de URI voor een blob-container (*uploadt*) in de volgende indeling:

    ```
    https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...
    ``` 

1. Noteer de volledige URI als deze wordt gebruikt in latere stappen.

1. De VHD-bestand uploaden met AzCopy:
 
1. [Download en installeer de nieuwste versie van AzCopy](https://aka.ms/downloadazcopy).

1. Open een opdrachtvenster en navigeer naar de installatiemap van AzCopy. U kunt de locatie van de AzCopy-installatie (optioneel) toevoegen aan het systeempad staat. AzCopy wordt standaard geïnstalleerd in de volgende map:

    ```command-line
    %ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy
    ```

1. Met behulp van de storage-account-sleutel en blob-container URI, voer de volgende opdracht uit vanaf de opdrachtprompt. De *vhdFileName* waarde moet liggen tussen aanhalingstekens. Het proces van het uploaden van een VHD-bestand mag langdurige, afhankelijk van de grootte van het VHD-bestand en de verbindingssnelheid.   

    ```command-line
    AzCopy /Source:<sourceDirectory> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Pattern:"<vhdFileName>" /BlobType:page
    ```

## <a name="next-steps"></a>Volgende stappen

- [Een aangepaste installatiekopie maken in Azure DevTest Labs van een VHD-bestand met de Azure portal](devtest-lab-create-template.md)
- [Een aangepaste installatiekopie maken in Azure DevTest Labs van een VHD-bestand met behulp van PowerShell](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
---
title: VHD-bestand uploaden naar Azure DevTest Labs met behulp van PowerShell | Microsoft Docs
description: VHD-bestand uploaden naar het lab storage-account met behulp van PowerShell
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
ms.openlocfilehash: b96e83980f746fde2c421a9247da17ace4a9efda
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51235357"
---
# <a name="upload-vhd-file-to-labs-storage-account-using-powershell"></a>VHD-bestand uploaden naar het lab storage-account met behulp van PowerShell

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

In Azure DevTest Labs, kunnen de VHD-bestanden worden gebruikt om aangepaste installatiekopieën, die worden gebruikt voor het inrichten van virtuele machines te maken. De volgende stappen helpen u bij het uploaden van een VHD-bestand met de storage-account van een lab met behulp van PowerShell. Als u uw VHD-bestand hebt geüpload de [sectie volgende stappen](#next-steps) geeft een lijst van sommige artikelen die laten zien hoe u een aangepaste installatiekopie van het geüploade VHD-bestand maken. Zie voor meer informatie over schijven en VHD's in Azure, [over schijven en VHD's voor virtuele machines](../virtual-machines/linux/about-disks-and-vhds.md)

## <a name="step-by-step-instructions"></a>Stapsgewijze instructies

De volgende stappen helpen u bij het uploaden van een VHD-bestand naar Azure DevTest Labs met behulp van PowerShell. 

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selecteer **alle services**, en selecteer vervolgens **DevTest Labs** in de lijst.

1. Selecteer de gewenste lab in de lijst met labs.  

1. Selecteer op de blade van de testomgeving **configuratie**. 

1. In het lab **configuratie** Selecteer **aangepaste installatiekopieën (VHD's)**.

1. Op de **aangepaste installatiekopieën** blade, selecteer **+ toevoegen**. 

1. Op de **aangepaste installatiekopie** Selecteer **VHD**.

1. Op de **VHD** Selecteer **uploaden van een VHD met behulp van PowerShell**.

    ![Met behulp van PowerShell VHD uploaden](./media/devtest-lab-upload-vhd-using-powershell/upload-image-using-psh.png)

1. Op de **Upload een afbeelding met behulp van PowerShell** blade, Kopieer de gegenereerde PowerShell-script naar een teksteditor.

1. Wijzig de **LocalFilePath** parameter van de **Add-AzureVhd** cmdlet om te verwijzen naar de locatie van het VHD-bestand dat u wilt uploaden.

1. Voer bij een PowerShell-prompt de **Add-AzureVhd** cmdlet (met de gewijzigde **LocalFilePath** parameter).

> [!WARNING] 
> 
> Het proces van het uploaden van een VHD-bestand mag langdurige, afhankelijk van de grootte van het VHD-bestand en de verbindingssnelheid.

## <a name="next-steps"></a>Volgende stappen

- [Een aangepaste installatiekopie maken in Azure DevTest Labs van een VHD-bestand met de Azure portal](devtest-lab-create-template.md)
- [Een aangepaste installatiekopie maken in Azure DevTest Labs van een VHD-bestand met behulp van PowerShell](devtest-lab-create-custom-image-from-vhd-using-powershell.md)

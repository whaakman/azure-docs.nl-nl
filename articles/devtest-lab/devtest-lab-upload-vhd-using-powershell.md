---
title: VHD-bestand uploaden naar Azure DevTest Labs met behulp van PowerShell | Microsoft Docs
description: VHD-bestand uploaden naar het lab-opslagaccount met behulp van PowerShell
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
ms.openlocfilehash: 27a80ddb110ba47bf9c1284aa48d2f3af1a87145
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="upload-vhd-file-to-labs-storage-account-using-powershell"></a>VHD-bestand uploaden naar het lab-opslagaccount met behulp van PowerShell

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

In Azure DevTest Labs kunnen VHD-bestanden worden gebruikt voor het maken van aangepaste installatiekopieën die worden gebruikt voor het inrichten van virtuele machines. De volgende stappen maakt u een VHD-bestand uploaden naar een lab-opslagaccount met behulp van PowerShell. Nadat u uw VHD-bestand hebt geüpload de [Vervolgstappen sectie](#next-steps) geeft een lijst van sommige artikelen die laten zien hoe u een aangepaste installatiekopie van het geüploade VHD-bestand. Zie voor meer informatie over schijven en VHD's in Azure [over schijven en virtuele harde schijven voor virtuele machines](../virtual-machines/linux/about-disks-and-vhds.md)

## <a name="step-by-step-instructions"></a>Stapsgewijze instructies

De volgende stappen maakt u een VHD-bestand uploaden naar Azure DevTest Labs met behulp van PowerShell. 

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selecteer **Meer services** en selecteer in de lijst vervolgens **DevTest Labs**.

1. Selecteer de gewenste testomgeving uit de lijst van labs.  

1. Selecteer op de labblade **configuratie**. 

1. Op de testomgeving **configuratie** blade Selecteer **aangepaste installatiekopieën (VHD's)**.

1. Op de **aangepaste installatiekopieën** blade Selecteer **+ toevoegen**. 

1. Op de **aangepaste installatiekopie** blade Selecteer **VHD**.

1. Op de **VHD** blade Selecteer **een VHD uploaden met PowerShell**.

    ![Virtuele harde schijf met behulp van PowerShell geüpload](./media/devtest-lab-upload-vhd-using-powershell/upload-image-using-psh.png)

1. Op de **een installatiekopie uploaden met PowerShell** blade het gegenereerde PowerShell-script kopiëren naar een teksteditor.

1. Wijzig de **LocalFilePath** parameter van de **Add-AzureVhd** cmdlet om te verwijzen naar de locatie van het VHD-bestand dat u wilt uploaden.

1. Voer bij een PowerShell-prompt de **Add-AzureVhd** cmdlet (met de gewijzigde **LocalFilePath** parameter).

> [!WARNING] 
> 
> Het proces van het uploaden van een VHD-bestand kan worden langdurige, afhankelijk van de grootte van het VHD-bestand en de verbindingssnelheid.

## <a name="next-steps"></a>Volgende stappen

- [Een aangepaste installatiekopie maken in Azure DevTest Labs van een VHD-bestand met de Azure portal](devtest-lab-create-template.md)
- [Een aangepaste installatiekopie maken in Azure DevTest Labs van een VHD-bestand met behulp van PowerShell](devtest-lab-create-custom-image-from-vhd-using-powershell.md)

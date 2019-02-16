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
ms.openlocfilehash: 56a66c3eb1dad93fad3ad1572989dc0c0aa14632
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56326525"
---
# <a name="upload-vhd-file-to-labs-storage-account-using-powershell"></a>VHD-bestand uploaden naar het lab storage-account met behulp van PowerShell

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

In Azure DevTest Labs, kunnen de VHD-bestanden worden gebruikt om aangepaste installatiekopieën, die worden gebruikt voor het inrichten van virtuele machines te maken. De volgende stappen helpen u bij het uploaden van een VHD-bestand met de storage-account van een lab met behulp van PowerShell. Als u uw VHD-bestand hebt geüpload de [sectie volgende stappen](#next-steps) geeft een lijst van sommige artikelen die laten zien hoe u een aangepaste installatiekopie van het geüploade VHD-bestand maken. Zie voor meer informatie over schijven en VHD's in Azure, [Inleiding tot beheerde schijven](../virtual-machines/linux/managed-disks-overview.md)

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

---
title: Een Azure DevTest Labs aangepaste installatiekopie maken van een VHD-bestand | Microsoft Docs
description: Meer informatie over het maken van een aangepaste installatiekopie in Azure DevTest Labs van een VHD-bestand met de Azure portal
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: b795bc61-7c28-40e6-82fc-96d629ee0568
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 9983ea9b847f44ed18a6169a4bdb224b63626a64
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-custom-image-from-a-vhd-file"></a>Een aangepaste installatiekopie van een VHD-bestand maken

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Stapsgewijze instructies

De volgende stappen maakt u een aangepaste installatiekopie van een VHD-bestand met de Azure portal maken:

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selecteer **Meer services** en selecteer in de lijst vervolgens **DevTest Labs**.

1. Selecteer de gewenste testomgeving uit de lijst van labs.  

1. Selecteer op de labblade **configuratie**. 

1. Op de testomgeving **configuratie** blade Selecteer **aangepaste installatiekopieën (VHD's)**.

1. Op de **aangepaste installatiekopieën** blade Selecteer **+ toevoegen**.

    ![Aangepaste installatiekopie toe te voegen](./media/devtest-lab-create-template/add-custom-image.png)

1. Voer de naam van de aangepaste afbeelding. Deze naam wordt weergegeven in de lijst met basisinstallatiekopieën bij het maken van een virtuele machine.

1. Geef de beschrijving van de aangepaste installatiekopie. Deze beschrijving wordt weergegeven in de lijst met basisinstallatiekopieën bij het maken van een virtuele machine.

1. Selecteer **VHD**.

1. Van de **VHD** blade selecteert de gewenste VHD-bestand.

1. Selecteer **OK** sluiten de **VHD** blade.

1. Selecteer **configuratie van het besturingssysteem**.

1. Op de **configuratie van het besturingssysteem** tabblad, selecteert u **Windows** of **Linux**.

1. Als **Windows** is geselecteerd, geeft u via het selectievakje of *Sysprep* is uitgevoerd op de machine. 

1. Selecteer **OK** sluiten de **configuratie van het besturingssysteem** blade.

1. Selecteer **OK** om de aangepaste installatiekopie te maken.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Verwante blogberichten

- [Aangepaste installatiekopieën of formules?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Kopiëren van aangepaste installatiekopieën tussen Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Volgende stappen

- [Een virtuele machine toevoegen aan uw testomgeving](./devtest-lab-add-vm-with-artifacts.md)

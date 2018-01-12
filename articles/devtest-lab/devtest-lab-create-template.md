---
title: Een Azure DevTest Labs aangepaste installatiekopie maken van een VHD-bestand | Microsoft Docs
description: Meer informatie over het maken van een aangepaste installatiekopie in Azure DevTest Labs van een VHD-bestand met de Azure portal
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: b795bc61-7c28-40e6-82fc-96d629ee0568
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: v-craic
ms.openlocfilehash: d1f1b9948fb591484c107818a01e141932effbba
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="create-a-custom-image-from-a-vhd-file"></a>Een aangepaste installatiekopie van een VHD-bestand maken

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Stapsgewijze instructies

De volgende stappen maakt u een aangepaste installatiekopie van een VHD-bestand met de Azure portal maken:

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selecteer **alle services**, en selecteer vervolgens **DevTest Labs** uit de lijst.

1. Selecteer de gewenste testomgeving uit de lijst van labs.  

1. Selecteer in het lab hoofdvenster **configuratie en het beleid**. 

1. Op de **configuratie en het beleid** deelvenster **aangepaste installatiekopieën**.

1. Op de **aangepaste installatiekopieën** deelvenster **+ toevoegen**.

    ![Aangepaste installatiekopie toe te voegen](./media/devtest-lab-create-template/add-custom-image.png)

1. Voer de naam van de aangepaste afbeelding. Deze naam wordt weergegeven in de lijst met basisinstallatiekopieën bij het maken van een virtuele machine.

1. Geef de beschrijving van de aangepaste installatiekopie. Deze beschrijving wordt weergegeven in de lijst met basisinstallatiekopieën bij het maken van een virtuele machine.

1. Voor **type besturingssysteem**, selecteert u **Windows** of **Linux**.

    - Als u selecteert **Windows**, Geef via het selectievakje of *sysprep* is uitgevoerd op de machine. 
    - Als u selecteert **Linux**, Geef via het selectievakje of *deprovision* is uitgevoerd op de machine. 

1. Selecteer een **VHD** uit de vervolgkeuzelijst. Dit is de VHD die wordt gebruikt om de nieuwe aangepaste installatiekopie te maken. Selecteer indien nodig **een VHD uploaden met PowerShell**.

1. U kunt ook een naam, plan aanbieding en plan publisher invoeren als de installatiekopie die wordt gebruikt voor het maken van de aangepaste installatiekopie niet is gepubliceerd door Microsoft.

1. Selecteer **OK** om de aangepaste installatiekopie te maken.

Na een paar minuten de aangepaste installatiekopie wordt gemaakt en wordt opgeslagen in het lab-opslagaccount. Wanneer een lab-gebruiker wil een nieuwe virtuele machine maken, is de installatiekopie beschikbaar in de lijst met basisinstallatiekopieën.

![Aangepaste installatiekopie beschikbaar in de lijst met basisinstallatiekopieën](./media/devtest-lab-create-template/custom-image-available-as-base.png)


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Verwante blogberichten

- [Aangepaste installatiekopieën of formules?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Kopiëren van aangepaste installatiekopieën tussen Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Volgende stappen

- [Een virtuele machine toevoegen aan uw testomgeving](./devtest-lab-add-vm.md)

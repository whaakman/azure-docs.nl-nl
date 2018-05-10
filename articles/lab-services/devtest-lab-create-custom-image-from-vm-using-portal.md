---
title: Een Azure DevTest Labs aangepaste installatiekopie maken van een virtuele machine | Microsoft Docs
description: Meer informatie over het maken van een aangepaste installatiekopie in Azure DevTest Labs van een ingerichte virtuele machine met de Azure portal
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
ms.date: 04/05/2018
ms.author: spelluru
ms.openlocfilehash: 9c98918dcd08c2e8ec72cc995c633b8320da8057
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="create-a-custom-image-from-a-vm"></a>Een aangepaste installatiekopie van een virtuele machine maken

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a>Stapsgewijze instructies

U kunt een aangepaste installatiekopie maken van een ingerichte virtuele machine en gebruik daarna die aangepaste installatiekopie identieke virtuele machines maken. De volgende stappen uit te laten zien hoe u een aangepaste installatiekopie maken van een virtuele machine:

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selecteer **alle services**, en selecteer vervolgens **DevTest Labs** uit de lijst.

1. Selecteer de gewenste testomgeving uit de lijst van labs.  

1. Selecteer in het lab hoofdvenster **mijn virtuele machines**.
 
1. Op de **mijn virtuele machines** deelvenster, selecteert u de virtuele machine van waaruit u wilt maken van de aangepaste installatiekopie.

1. Selecteer op de VM management deelvenster **maken aangepaste installatiekopie (VHD)**.

    ![Afbeelding van aangepaste menu-item maken](./media/devtest-lab-create-template/create-custom-image.png)

1. Op de **aangepaste installatiekopie** deelvenster, voer een naam en beschrijving voor de aangepaste installatiekopie. Deze informatie wordt weergegeven in de lijst met databases wanneer u een virtuele machine maken.

    ![Deelvenster van de aangepaste installatiekopie maken](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. Geef op of sysprep is uitgevoerd op de virtuele machine. Als de sysprep is niet uitgevoerd op de virtuele machine, moet u opgeven of u wilt dat sysprep uitgevoerd wanneer een virtuele machine wordt gemaakt van deze aangepaste installatiekopie.

1. Selecteer **OK** wanneer u klaar bent met het maken van de aangepaste installatiekopie.

Na een paar minuten de aangepaste installatiekopie wordt gemaakt en wordt opgeslagen in het lab-opslagaccount. Wanneer een lab-gebruiker wil een nieuwe virtuele machine maken, is de installatiekopie beschikbaar in de lijst met basisinstallatiekopieën.

![Aangepaste installatiekopie beschikbaar in de lijst met basisinstallatiekopieën](./media/devtest-lab-create-template/custom-image-available-as-base.png)


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Verwante blogberichten

- [Aangepaste installatiekopieën of formules?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Kopiëren van aangepaste installatiekopieën tussen Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Volgende stappen

- [Een virtuele machine toevoegen aan uw testomgeving](devtest-lab-add-vm.md)

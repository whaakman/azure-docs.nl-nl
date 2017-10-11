---
title: Een Azure DevTest Labs aangepaste installatiekopie maken van een virtuele machine | Microsoft Docs
description: Meer informatie over het maken van een aangepaste installatiekopie in Azure DevTest Labs van een ingerichte virtuele machine met de Azure portal
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 9d2dcf7164985508d691e8a0c123efaf3b8aa19a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-custom-image-from-a-vm"></a>Een aangepaste installatiekopie van een virtuele machine maken

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a>Stapsgewijze instructies

U kunt een aangepaste installatiekopie maken van een ingerichte virtuele machine en gebruik daarna die aangepaste installatiekopie identieke virtuele machines maken. De volgende stappen uit te laten zien hoe u een aangepaste installatiekopie maken van een virtuele machine:

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selecteer **Meer services** en selecteer in de lijst vervolgens **DevTest Labs**.

1. Selecteer de gewenste testomgeving uit de lijst van labs.  

1. Selecteer op de labblade **mijn virtuele machines**.
 
1. Op de **mijn virtuele machines** blade, selecteert u de virtuele machine van waaruit u wilt maken van de aangepaste installatiekopie.

1. Selecteer op de VM-blade **maken aangepaste installatiekopie (VHD)**.

    ![Afbeelding van aangepaste menu-item maken](./media/devtest-lab-create-template/create-custom-image.png)

1. Op de **afbeelding maken** blade een naam en beschrijving voor de aangepaste installatiekopie opgeven. Deze informatie wordt weergegeven in de lijst met databases wanneer u een virtuele machine maken.

    ![Blade voor aangepaste installatiekopie maken](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. Geef op of sysprep is uitgevoerd op de virtuele machine. Als de sysprep is niet uitgevoerd op de virtuele machine, moet u opgeven of u wilt dat sysprep uitgevoerd wanneer een virtuele machine wordt gemaakt van deze aangepaste installatiekopie.

1. Selecteer **OK** wanneer u klaar bent met het maken van de aangepaste installatiekopie.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Verwante blogberichten

- [Aangepaste installatiekopieën of formules?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Kopiëren van aangepaste installatiekopieën tussen Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Volgende stappen

- [Een virtuele machine toevoegen aan uw testomgeving](./devtest-lab-add-vm-with-artifacts.md)

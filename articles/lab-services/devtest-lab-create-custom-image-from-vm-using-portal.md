---
title: Een aangepaste installatiekopie van Azure DevTest Labs van een virtuele machine maken | Microsoft Docs
description: Meer informatie over het maken van een aangepaste installatiekopie in Azure DevTest Labs van een ingerichte virtuele machine met behulp van de Azure portal
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
ms.openlocfilehash: a91e14afa8325e75507b2f65cd9d71ab38df4524
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51235238"
---
# <a name="create-a-custom-image-from-a-vm"></a>Een aangepaste installatiekopie van een virtuele machine maken

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a>Stapsgewijze instructies

U kunt een aangepaste installatiekopie van een ingerichte virtuele machine maken en daarna die aangepaste installatiekopie gebruiken om te maken van identieke VM's. De volgende stappen laten zien hoe u een aangepaste installatiekopie maken van een virtuele machine:

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selecteer **alle services**, en selecteer vervolgens **DevTest Labs** in de lijst.

1. Selecteer de gewenste lab in de lijst met labs.  

1. Selecteer in het hoofdvenster van de testomgeving, **mijn virtuele machines**.
 
1. Op de **mijn virtuele machines** deelvenster, selecteert u de virtuele machine van waaruit u wilt maken van de aangepaste installatiekopie.

1. Selecteer op de VM beheer in het deelvenster **maken aangepaste installatiekopie (VHD)**.

    ![Aangepaste installatiekopie menu-item maken](./media/devtest-lab-create-template/create-custom-image.png)

1. Op de **aangepaste installatiekopie** deelvenster, voer een naam en beschrijving op voor uw aangepaste installatiekopie. Deze informatie wordt weergegeven in de lijst met databases bij het maken van een virtuele machine. De aangepaste installatiekopie bevat de besturingssysteemschijf en alle gegevensschijven die zijn gekoppeld aan de virtuele machine.

    ![Deelvenster van de aangepaste installatiekopie maken](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. Geef aan of sysprep is uitgevoerd op de virtuele machine. Als de sysprep is niet uitgevoerd op de virtuele machine, moet u opgeven of u wilt dat sysprep wordt uitgevoerd wanneer een virtuele machine is gemaakt op basis van deze aangepaste installatiekopie.

1. Selecteer **OK** wanneer u klaar bent om de aangepaste installatiekopie te maken.

Na een paar minuten, wordt de aangepaste installatiekopie wordt gemaakt en wordt opgeslagen in het opslagaccount van het lab. Wanneer een gebruiker lab wil maken van een nieuwe virtuele machine, is de installatiekopie is beschikbaar in de lijst met basisinstallatiekopieën.

![Aangepaste installatiekopie beschikbaar in de lijst met basisinstallatiekopieën](./media/devtest-lab-create-template/custom-image-available-as-base.png)


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Gerelateerde blogberichten

- [Aangepaste installatiekopieën of formules?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Kopiëren van aangepaste installatiekopieën tussen Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Volgende stappen

- [Een VM toevoegen aan uw testomgeving](devtest-lab-add-vm.md)

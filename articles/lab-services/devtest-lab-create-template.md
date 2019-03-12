---
title: Een aangepaste installatiekopie van Azure DevTest Labs maken van een VHD-bestand | Microsoft Docs
description: Meer informatie over het maken van een aangepaste installatiekopie in Azure DevTest Labs van een VHD-bestand met de Azure portal
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: b795bc61-7c28-40e6-82fc-96d629ee0568
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 853c138c8cf73b41b0cebb6c1d349865e18eab6a
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57550102"
---
# <a name="create-a-custom-image-from-a-vhd-file"></a>Een aangepaste installatiekopie van een VHD-bestand maken

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Stapsgewijze instructies

De volgende stappen helpen u bij het maken van een aangepaste installatiekopie van een VHD-bestand met de Azure-portal:

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selecteer **alle services**, en selecteer vervolgens **DevTest Labs** in de lijst.

1. Selecteer de gewenste lab in de lijst met labs.  

1. Selecteer in het hoofdvenster van de testomgeving, **configuratie en het beleid**. 

1. Op de **configuratie en het beleid** venster **aangepaste installatiekopieën**.

1. Op de **aangepaste installatiekopieën** venster **+ toevoegen**.

    ![Aangepaste installatiekopie toevoegen](./media/devtest-lab-create-template/add-custom-image.png)

1. Voer de naam van de aangepaste installatiekopie. Deze naam wordt weergegeven in de lijst van basisinstallatiekopieën bij het maken van een virtuele machine.

1. Voer de beschrijving van de aangepaste installatiekopie. Deze beschrijving wordt weergegeven in de lijst met basisinstallatiekopieën bij het maken van een virtuele machine.

1. Voor **besturingssysteemtype**, selecteert u **Windows** of **Linux**.

    - Als u selecteert **Windows**, Geef via het selectievakje in of *sysprep* is uitgevoerd op de machine. 
    - Als u selecteert **Linux**, Geef via het selectievakje in of *inrichting* is uitgevoerd op de machine. 

1. Selecteer een **VHD** uit de vervolgkeuzelijst. Dit is de VHD die wordt gebruikt om de nieuwe aangepaste installatiekopie te maken. Selecteer indien nodig **uploaden van een VHD met behulp van PowerShell**.

1. U kunt ook een naam, abonnement en plan publisher invoeren als de installatiekopie die wordt gebruikt voor het maken van de aangepaste installatiekopie niet een gelicentieerde installatiekopie is (gepubliceerd door Microsoft).

   - **Naam van het plan:** Voer de naam van de Marketplace-installatiekopie (SKU) van deze aangepaste installatiekopie is gemaakt 
   - **Aanbieding voor abonnement:** Voer de productcode (aanbieding) van de Marketplace-installatiekopie waaruit deze aangepaste installatiekopie is gemaakt 
   - **Plan uitgever:** Voer de uitgever van de Marketplace-installatiekopie waaruit deze aangepaste installatiekopie is gemaakt

   > [!NOTE]
   > Als de afbeelding die u gebruikt voor het maken van een aangepaste installatiekopie is **niet** een gelicentieerde installatiekopie en vervolgens deze velden leeg zijn en kunnen worden ingevuld indien gewenst. Als de afbeelding **is** een gelicentieerde installatiekopie en vervolgens de velden worden automatisch ingevuld met de planningsinformatie. Als u probeert te wijzigen in dit geval is, wordt er een waarschuwingsbericht weergegeven.
   >
   >

1. Selecteer **OK** om de aangepaste installatiekopie te maken.

Na een paar minuten, wordt de aangepaste installatiekopie wordt gemaakt en wordt opgeslagen in het opslagaccount van het lab. Wanneer een gebruiker lab wil maken van een nieuwe virtuele machine, is de installatiekopie is beschikbaar in de lijst met basisinstallatiekopieën.

![Aangepaste installatiekopie beschikbaar in de lijst met basisinstallatiekopieën](./media/devtest-lab-create-template/custom-image-available-as-base.png)


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Gerelateerde blogberichten

- [Aangepaste installatiekopieën of formules?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Kopiëren van aangepaste installatiekopieën tussen Azure DevTest Labs](https://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Volgende stappen

- [Een VM toevoegen aan uw testomgeving](./devtest-lab-add-vm.md)

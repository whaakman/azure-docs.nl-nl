---
title: Gebruik een galerie met gedeelde installatiekopieën in Azure Lab-Services | Microsoft Docs
description: Informatie over het configureren van een lab-account voor het gebruik van een galerie met installatiekopieën van de gedeelde zodat een gebruiker een installatiekopie met andere delen kunt en een andere gebruiker met de installatiekopie kunt maken van een VM-sjabloon in het lab.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2019
ms.author: spelluru
ms.openlocfilehash: 93136c7d685bd9fc8ec4bcdea3a900b28029059b
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58652984"
---
# <a name="use-a-shared-image-gallery-in-azure-lab-services"></a>Gebruik een galerie met gedeelde installatiekopieën in Azure Lab Services
Dit artikel wordt beschreven hoe docenten/lab-beheerder een sjabloon voor VM-installatiekopie voor het opnieuw worden gebruikt door anderen kan opslaan. Deze installatiekopieën worden opgeslagen in een Azure [gedeelde afbeeldingengalerie](../../virtual-machines/windows/shared-image-galleries.md). Als een eerste stap koppelt u de lab-beheerder een bestaande gedeelde afbeeldingengalerie aan het lab-account. Zodra de galerie met installatiekopieën van de gedeelde is gekoppeld, kunnen labs gemaakt in het lab-account afbeeldingen opslaan in de galerie met installatiekopieën van de gedeelde. Andere docenten kunnen deze installatiekopie in de galerie gedeelde installatiekopie te maken van een sjabloon voor de klassen selecteren. 

## <a name="prerequisites"></a>Vereisten
Een gedeelde afbeeldingengalerie maken met behulp van [Azure PowerShell](../../virtual-machines/windows/shared-images.md) of [Azure CLI](../../virtual-machines/linux/shared-images.md).

## <a name="attach-a-shared-image-gallery-to-a-lab-account"></a>Een galerie met installatiekopieën van de gedeelde koppelen aan een lab-account
De volgende procedure laat zien hoe u een galerie met installatiekopieën van de gedeelde koppelen aan een lab-account. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu links **Alle services**. Selecteer **Lab Services** in de **DEVOPS** sectie. Als u een ster (`*`) naast **Lab Services**, wordt deze toegevoegd aan de **Favorieten** sectie in het menu links. In de volgende keer dat en hoger, selecteert u **Lab Services** onder **Favorieten**.

    ![Alle Services -> Lab-Services](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. Selecteer uw lab-account om te zien de **Lab-Account** pagina. 
4. Selecteer **gedeelde afbeeldingengalerie** op het menu aan de linkerkant en selecteer **Attach** op de werkbalk. 

    ![Gedeelde galerie met installatiekopieën: knop toevoegen](../media/how-to-use-shared-image-gallery/sig-attach-button.png)
5. Op de **koppelen van een galerie met installatiekopieën voor bestaande gedeelde** pagina, selecteer de galerie met installatiekopieën van de gedeelde en selecteer **OK**.

    ![Selecteer een bestaande galerie](../media/how-to-use-shared-image-gallery/select-image-gallery.png)
6. U ziet het volgende scherm: 

    ![De galerie in de lijst](../media/how-to-use-shared-image-gallery/my-gallery-in-list.png)
    
    In dit voorbeeld zijn er geen afbeeldingen in de galerie met installatiekopieën van de gedeelde nog.

Azure Lab Services identiteit wordt toegevoegd als Inzender aan de galerie met gedeelde installatiekopieën die is gekoppeld aan het lab. Hierdoor kan docenten / IT-beheerder om op te slaan van de virtuele machine naar de galerie met installatiekopieën van de gedeelde afbeeldingen. Alle labs gemaakt in dit lab-account hebben toegang tot het gekoppelde gedeelde afbeeldingengalerie. 

Alle installatiekopieën in de galerie met installatiekopieën van gekoppelde gedeelde zijn standaard ingeschakeld. U kunt inschakelen of uitschakelen van geselecteerde installatiekopieën door ze te selecteren in de lijst en het gebruik van de **inschakelen van de geselecteerde kopieën** of **uitschakelen van de geselecteerde kopieën** knop. 

## <a name="detach-a-shared-image-gallery"></a>Loskoppelen van een gedeelde Afbeeldingengalerie
Slechts één gedeelde afbeeldingengalerie kan worden gekoppeld aan een lab. Als u wilt koppelen van een andere gedeelde afbeeldingengalerie, loskoppelen van het huidige abonnement voordat u het nieuwe certificaat koppelt. Als u wilt loskoppelen van een galerie met installatiekopieën van de gedeelde in uw testomgeving, selecteer **loskoppelen** op de werkbalk en controleer of de bewerking loskoppelen. 

## <a name="save-an-image-to-the-shared-image-gallery"></a>Een afbeelding opgeslagen in de galerie met installatiekopieën van de gedeelde
Nadat een galerie met installatiekopieën van de gedeelde is gekoppeld, kan een docent een sjablooninstallatiekopie naar de galerie met installatiekopieën van de gedeelde opslaan, zodat deze kan worden gebruikt door andere docenten.

![Afbeelding van de virtuele machine opslaan in de galerie](../media/how-to-use-shared-image-gallery/save-virtual-machine.png)

## <a name="use-an-image-from-the-shared-image-gallery"></a>Een installatiekopie uit de galerie met gedeelde installatiekopie gebruiken
Een docent kan een aangepaste installatiekopie beschikbaar in de galerie met installatiekopieën van de gedeelde voor de sjabloon kiezen tijdens het maken van nieuwe lab.

![Installatiekopie voor virtuele machines uit de galerie gebruiken](../media/how-to-use-shared-image-gallery/use-shared-image.png)

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over gedeelde afbeeldingsgalerieën [gedeelde afbeeldingengalerie](../../virtual-machines/windows/shared-image-galleries.md).

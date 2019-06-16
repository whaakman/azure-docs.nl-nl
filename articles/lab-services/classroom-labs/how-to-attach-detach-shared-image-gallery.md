---
title: Koppelen of ontkoppelen van een galerie met gedeelde installatiekopieën in Azure Lab-Services | Microsoft Docs
description: Leer hoe u een galerie met installatiekopieën van de gedeelde koppelen aan een lab in Azure Lab-Services.
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
ms.date: 05/07/2019
ms.author: spelluru
ms.openlocfilehash: de4e9fb4b15f4c346926fe46f23255c668204c2e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65413892"
---
# <a name="attach-or-detach-a-shared-image-gallery-in-azure-lab-services"></a>Koppelen of ontkoppelen van een galerie met gedeelde installatiekopieën in Azure Lab Services
Docenten/lab-beheerder een sjabloon voor VM-installatiekopie kunt opslaan in een Azure [gedeelde afbeeldingengalerie](../../virtual-machines/windows/shared-image-galleries.md) voor het opnieuw door anderen worden gebruikt. Als een eerste stap koppelt u de lab-beheerder een bestaande gedeelde afbeeldingengalerie aan het lab-account. Zodra de galerie met installatiekopieën van de gedeelde is gekoppeld, kunnen labs gemaakt in het lab-account afbeeldingen opslaan in de galerie met installatiekopieën van de gedeelde. Andere docenten kunnen deze installatiekopie in de galerie gedeelde installatiekopie te maken van een sjabloon voor de klassen selecteren. 

In dit artikel laat u zien hoe koppelen of ontkoppelen van een gedeelde afbeeldingengalerie aan een lab-account. 

## <a name="configure-at-the-time-of-lab-account-creation"></a>Configureer op het moment van lab-account maken
Wanneer u een lab-account maakt, kunt u een galerie met installatiekopieën van de gedeelde koppelen aan het lab-account. U kunt een bestaande gedeelde afbeeldingengalerie selecteren in de vervolgkeuzelijst of een nieuwe maken. Als u wilt maken en een galerie met installatiekopieën van de gedeelde koppelen aan het lab-account, selecteert u **nieuw**, voer een naam in voor de galerie en voer **OK**. 

![Galerie met installatiekopieën van gedeelde configureren op het moment van lab-account maken](../media/how-to-use-shared-image-gallery/new-lab-account.png)

## <a name="configure-after-the-lab-account-is-created"></a>Configureer nadat het lab-account is gemaakt
Nadat het lab-account is gemaakt, kunt u de volgende taken uitvoeren:

- Maken en koppelen van een gedeelde Afbeeldingengalerie
- Een galerie met installatiekopieën van de gedeelde koppelen aan het lab-account
- Een galerie met installatiekopieën van de gedeelde uit het lab-account loskoppelen

## <a name="create-and-attach-a-shared-image-gallery"></a>Maken en koppelen van een gedeelde Afbeeldingengalerie
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu links **Alle services**. Selecteer **Lab Services** in de **DEVOPS** sectie. Als u een ster (`*`) naast **Lab Services**, wordt deze toegevoegd aan de **Favorieten** sectie in het menu links. In de volgende keer dat en hoger, selecteert u **Lab Services** onder **Favorieten**.

    ![Alle Services -> Lab-Services](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. Selecteer uw lab-account om te zien de **Lab-Account** pagina. 
4. Selecteer **gedeelde afbeeldingengalerie** op het menu aan de linkerkant en selecteer **+ maken** op de werkbalk.  

    ![Gedeelde installatiekopie galerie knop maken](../media/how-to-use-shared-image-gallery/new-shared-image-gallery-button.png)
5. In de **maken gedeelde afbeeldingengalerie** venster, Geef een **naam** voor de galerie, en voer **OK**. 

    ![Gedeelde installatiekopie galerie venster maken](../media/how-to-use-shared-image-gallery/create-shared-image-gallery-window.png)

    Azure Lab-Services de galerie met installatiekopieën van de gedeelde maakt en deze vervolgens aan het lab-account gekoppeld. Alle labs gemaakt in dit lab-account hebben toegang tot het gekoppelde gedeelde afbeeldingengalerie. 

    ![Galerie met installatiekopieën van gekoppelde](../media/how-to-use-shared-image-gallery/image-gallery-in-list.png)

    In het onderste deelvenster ziet u afbeeldingen in de galerie met installatiekopieën van de gedeelde. Er zijn geen afbeeldingen in deze nieuwe galerie. Wanneer u afbeeldingen naar de galerie uploaden, ziet u ze op deze pagina.     

    Alle installatiekopieën in de galerie met installatiekopieën van gekoppelde gedeelde zijn standaard ingeschakeld. U kunt inschakelen of uitschakelen van geselecteerde installatiekopieën door ze te selecteren in de lijst en het gebruik van de **inschakelen van de geselecteerde kopieën** of **uitschakelen van de geselecteerde kopieën** knop.

## <a name="attach-an-existing-shared-image-gallery"></a>Koppelen van een bestaande gedeelde Afbeeldingengalerie
De volgende procedure laat zien hoe u een bestaande gedeelde afbeeldingengalerie koppelen aan een lab-account. 

1. Op de **Lab-Account** weergeeft, schakelt **gedeelde afbeeldingengalerie** op het menu aan de linkerkant en selecteer **Attach** op de werkbalk. 

    ![Gedeelde galerie met installatiekopieën: knop toevoegen](../media/how-to-use-shared-image-gallery/sig-attach-button.png)
5. Op de **koppelen van een galerie met installatiekopieën voor bestaande gedeelde** pagina, selecteer de galerie met installatiekopieën van de gedeelde en selecteer **OK**.

    ![Selecteer een bestaande galerie](../media/how-to-use-shared-image-gallery/select-image-gallery.png)
6. U ziet het volgende scherm: 

    ![De galerie in de lijst](../media/how-to-use-shared-image-gallery/my-gallery-in-list.png)
    
    In dit voorbeeld zijn er geen afbeeldingen in de galerie met installatiekopieën van de gedeelde nog.

    Azure Lab Services identiteit wordt toegevoegd als Inzender aan de galerie met gedeelde installatiekopieën die is gekoppeld aan het lab. Hierdoor kan docenten / IT-beheerder om op te slaan van de virtuele machine naar de galerie met installatiekopieën van de gedeelde afbeeldingen. Alle labs gemaakt in dit lab-account hebben toegang tot het gekoppelde gedeelde afbeeldingengalerie. 

    Alle installatiekopieën in de galerie met installatiekopieën van gekoppelde gedeelde zijn standaard ingeschakeld. U kunt inschakelen of uitschakelen van geselecteerde installatiekopieën door ze te selecteren in de lijst en het gebruik van de **inschakelen van de geselecteerde kopieën** of **uitschakelen van de geselecteerde kopieën** knop. 

## <a name="save-an-image-to-the-shared-image-gallery"></a>Een afbeelding opgeslagen in de galerie met installatiekopieën van de gedeelde
Nadat een galerie met installatiekopieën van de gedeelde is gekoppeld, kunt een lab-accountbeheerder of een docent opslaan of upload een afbeelding naar de galerie met installatiekopieën van de gedeelde zodat deze kan worden gebruikt door andere docenten. Zie voor instructies voor het uploaden van een installatiekopie naar de galerie met installatiekopieën van de gedeelde [overzicht van de galerie met installatiekopieën van gedeelde](../../virtual-machines/windows/shared-images.md). 

> [!NOTE]
> Momenteel, biedt geen de Leslokaallabs-gebruikersinterface (UI) ondersteuning voor opslaan van de installatiekopie van een lab in de galerie met installatiekopieën van de gedeelde. 

## <a name="detach-a-shared-image-gallery"></a>Loskoppelen van een gedeelde Afbeeldingengalerie
Slechts één gedeelde afbeeldingengalerie kan worden gekoppeld aan een lab. Als u wilt koppelen van een andere gedeelde afbeeldingengalerie, loskoppelen van het huidige abonnement voordat u het nieuwe certificaat koppelt. Als u wilt loskoppelen van een galerie met installatiekopieën van de gedeelde in uw testomgeving, selecteer **loskoppelen** op de werkbalk en controleer of de bewerking loskoppelen. 

![De galerie met gedeelde installatiekopieën uit het lab-account loskoppelen](../media/how-to-use-shared-image-gallery/detach.png)

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over hoe u een lab-afbeelding opgeslagen in de galerie met installatiekopieën van de gedeelde of een installatiekopie uit de galerie met installatiekopieën van de gedeelde gebruiken om te maken van een virtuele machine, [over het gebruik van gedeelde afbeeldingengalerie](how-to-use-shared-image-gallery.md).

Zie voor meer informatie over afbeeldingsgalerieën in het algemeen gedeelde, [gedeelde afbeeldingengalerie](../../virtual-machines/windows/shared-image-galleries.md).

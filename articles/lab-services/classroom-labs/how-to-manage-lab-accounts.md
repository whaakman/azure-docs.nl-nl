---
title: Lab-accounts in Azure Lab Services beheren | Microsoft Docs
description: Informatie over het maken van een lab-account, alle lab-accounts weergeven of verwijderen van een lab-account in een Azure-abonnement.
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
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: ff2968f8e2fa9a705817b020f2daa6582d78029c
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225299"
---
# <a name="manage-lab-accounts-in-azure-lab-services"></a>Lab-accounts in Azure Lab Services beheren 
Een lab-account is in Azure Lab-Services, een container voor beheerde labs zoals leslokaallabs. Een beheerder stelt u een lab-account met Azure Lab-Services en biedt toegang tot een lab-eigenaren die labs in het account maken kunnen. In dit artikel wordt beschreven hoe u een lab-account maken, alle lab-accounts weergeven of verwijderen van een lab-account.

## <a name="create-a-lab-account"></a>Een lab-account maken
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Een resource maken** in het hoofdmenu aan de linkerkant.
3. Zoek naar **Lab Services** in Azure Marketplace en selecteer **Lab Services** in de vervolgkeuzelijst. 
4. Selecteer **Lab Services (preview)** in de gefilterde lijst met services. 
5. Selecteer **Maken** in het venster **Een lab-account maken**.
7. Voer in het venster **Lab-account** de volgende acties uit: 
    1. Voer een naam in bij **lab-accountnaam**. 
    2. Selecteer het **Azure-abonnement** waarin u het lab-account wilt maken.
    3. Selecteer voor **Resourcegroep** de optie **Nieuwe maken** en voer een naam in voor de resourcegroep.
    4. Selecteer voor **Locatie** een locatie/regio waarin het lab-account moet worden gemaakt. 
    5. Selecteer **Maken**. 

        ![Venster Een lab-account maken](../media/how-to-manage-lab-accounts/lab-account-settings.png)
5. Als u de pagina voor het lab-account niet ziet, selecteert u de knop **Meldingen** en klikt u vervolgens op de knop **Ga naar resource** in de meldingen. 

    ![Venster Een lab-account maken](../media/how-to-manage-lab-accounts/notification-go-to-resource.png)    
6. U ziet de volgende pagina **lab-account**:

    ![Pagina lab-account](../media/how-to-manage-lab-accounts/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Een gebruiker toevoegen aan de rol Labmaker
Als u een leslokaallab in een lab-account instelt, moet de gebruiker een lid van de **Labmaker** rol in het lab-account. Het account dat u gebruikt voor het maken van het lab-account wordt automatisch toegevoegd aan deze rol. Als u van plan bent een leslokaallab maken met hetzelfde gebruikersaccount, kunt u deze stap overslaan. Als u een ander gebruikersaccount wilt maken van een leslokaallab, voer de volgende stappen uit: 

1. Selecteer op de pagina **lab-account** de optie **Toegangsbeheer (IAM)** en klik op **+ Toevoegen** in de werkbalk. 

    ![Pagina lab-account](../media/tutorial-setup-lab-account/access-control.png)
2. Selecteer op de pagina **Machtigingen toevoegen** de optie **Labmaker** als **rol**. Selecteer de gebruiker die u wilt toevoegen aan de rol Labmaker en selecteer **Opslaan**. 

    ![Gebruiker toevoegen aan de rol Labmaker](../media/tutorial-setup-lab-account/add-user-to-lab-creator-role.png)

## <a name="specify-marketplace-images-available-to-lab-owners"></a>Marketplace-installatiekopieën die beschikbaar zijn voor lab-eigenaars opgeven
In deze sectie geeft u de Marketplace-installatiekopieën die lab eigenaren gebruiken kunnen om te maken van leslokaallabs. 

1. Selecteer **Marketplace-installatiekopieën** in het menu aan de linkerkant. Standaard ziet u de volledige lijst met installatiekopieën (zowel ingeschakelde en uitgeschakelde). U kunt alleen ingeschakeld/uitgeschakeld installatiekopieën bekijken door te selecteren in de lijst filteren de **ingeschakeld alleen**/**alleen uitgeschakeld** optie in de vervolgkeuzelijst aan de bovenkant. 

    ![Marketplace-installatiekopieën-pagina](../media/tutorial-setup-lab-account/marketplace-images-page.png)
2. Naar **uitschakelen** een Marketplace-installatiekopie die is ingeschakeld, voer een van de volgende acties: 
    1. Selecteer **... (ellips)**  in de laatste kolom en selecteert u **uitschakelen installatiekopie**. 

        ![Een afbeelding uitschakelen](../media/tutorial-setup-lab-account/disable-one-image.png) 
    2. Selecteer een of meer installatiekopieën in de lijst door het selecteren van de selectievakjes in voor de namen van de installatiekopie in de lijst en selecteer **uitschakelen van de geselecteerde kopieën**. 

        ![Meerdere installatiekopieën uitschakelen](../media/tutorial-setup-lab-account/disable-multiple-images.png) 
1. Op dezelfde manier naar **inschakelen** een Marketplace-installatiekopie, voert u een van de volgende acties: 
    1. Selecteer **... (ellips)**  in de laatste kolom en selecteert u **inschakelen installatiekopie**. 
    2. Selecteer een of meer installatiekopieën in de lijst door het selecteren van de selectievakjes in voor de namen van de installatiekopie in de lijst en selecteer **inschakelen van de geselecteerde kopieën**. 

## <a name="view-lab-accounts"></a>Lab-accounts weergeven
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle resources** in het menu. 
3. Selecteer **Lab Services** voor de **type**. 
    U kunt ook filteren op abonnement, resourcegroep, locaties en tags. 

## <a name="delete-a-lab-account"></a>Een lab-account verwijderen
Volg de instructies uit de vorige sectie die lab-accounts in een lijst weergegeven. Gebruik de volgende instructies om een lab-account te verwijderen: 

1. Selecteer de **lab-account** die u wilt verwijderen. 
2. Selecteer **verwijderen** via de werkbalk. 
3. Type **Ja** ter bevestiging.
4. Selecteer **Verwijderen**. 

## <a name="next-steps"></a>Volgende stappen
Aan de slag met het installeren van een lab met Azure Lab Services:

- [Een leslokaallab instellen](tutorial-setup-classroom-lab.md)
- [Een lab instellen](../tutorial-create-custom-lab.md)

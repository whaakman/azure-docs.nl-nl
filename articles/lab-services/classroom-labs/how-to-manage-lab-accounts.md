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
ms.date: 02/07/2018
ms.author: spelluru
ms.openlocfilehash: 6cd06778ad54fa698c5bc2fe4ccf02f4be2ee2ec
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/25/2019
ms.locfileid: "56807037"
---
# <a name="manage-lab-accounts-in-azure-lab-services"></a>Lab-accounts in Azure Lab Services beheren 
Een lab-account is in Azure Lab-Services, een container voor beheerde labtypen zoals leslokaallabs. Een beheerder stelt u een lab-account met Azure Lab-Services en biedt toegang tot een lab-eigenaren die labs in het account maken kunnen. In dit artikel wordt beschreven hoe u een lab-account maken, alle lab-accounts weergeven of verwijderen van een lab-account.

## <a name="create-a-lab-account"></a>Een lab-account maken
In de volgende stappen ziet u hoe u Azure Portal kunt gebruiken om een lab te maken met Azure Lab Services. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu links **Alle services**. Selecteer **Lab-accounts** in de sectie **DEVOPS**. Selecteer de ster (`*`) naast de **Lab-accounts** om er een toe te voegen aan de sectie **FAVORIETEN** in het menu links. Vanaf de volgende keer selecteert u **Lab-accounts** onder **Favorieten**.

    ![Alle Services -> Lab-accounts](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. Selecteer op de pagina **Lab-accounts** **Toevoegen** op de werkbalk. 

    ![Selecteer de Toevoegen op de pagina Lab-accounts](../media/tutorial-setup-lab-account/add-lab-account-button.png)
4. Voer op de pagina **Lab-account** de volgende acties uit: 
    1. Voer een naam in bij **lab-accountnaam**. 
    2. Selecteer het **Azure-abonnement** waarin u het lab-account wilt maken.
    3. Selecteer voor **Resourcegroep** de optie **Nieuwe maken** en voer een naam in voor de resourcegroep.
    4. Selecteer voor **Locatie** een locatie/regio waarin het lab-account moet worden gemaakt. 
    5. Selecteer **Maken**. 

        ![Venster Een lab-account maken](../media/tutorial-setup-lab-account/lab-account-settings.png)
5. Selecteer het **Klokpictogram** op de werkbalk (**Meldingen**), controleer of de implementatie is voltooid en selecteer vervolgens **Naar de resource gaan**. 

    U kunt ook **Vernieuwen** selecteren op de **Lab-accounts**-pagina en het lab-account selecteren dat u hebt gemaakt. 

    ![Venster Een lab-account maken](../media/tutorial-setup-lab-account/go-to-lab-account.png)    
6. U ziet de volgende pagina **lab-account**:

    ![Pagina lab-account](../media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Een gebruiker toevoegen aan de rol Labmaker
Om een leslokaallab in een labaccount in te kunnen stellen, moet de gebruiker lid zijn van de rol **Labmaker** in het labaccount. Het account dat u hebt gebruikt voor het maken van het lab-account wordt automatisch toegevoegd aan deze rol. Als u van plan bent een leslokaallab te maken met hetzelfde gebruikersaccount, kunt u deze stap overslaan. Als u een ander gebruikersaccount wilt gebruiken om een leslokaallab te maken, voert u de volgende stappen uit: 

Als u machtigingen wilt toekennen aan docenten om labs te maken voor hun lessen, voegt u hen toe aan de rol **Labmaker**:

1. Selecteer op de pagina **Lab-account** de optie **Toegangsbeheer (IAM)** en klik op **+ Roltoewijzing toevoegen** in de werkbalk. 

    ![Access Control -> Knop Roltoewijzing toevoegen](../media/tutorial-setup-lab-account/add-role-assignment-button.png)
1. Selecteer op de pagina **Roltoewijzing toevoegen** de optie **Labmaker** als **Rol**. Selecteer de gebruiker die u wilt toevoegen aan de rol Labmaker en selecteer **Opslaan**. 

    ![Labmaker toevoegen](../media/tutorial-setup-lab-account/add-lab-creator.png)


## <a name="specify-marketplace-images-available-to-lab-creators"></a>Microsoft Azure Marketplace-installatiekopieën opgeven die beschikbaar zijn voor labmakers
Als eigenaar van een labaccount kunt u de Marketplace-installatiekopieën opgeven die labmakers kunnen gebruiken in het labaccount. 

1. Selecteer **Marketplace-installatiekopieën** in het menu aan de linkerkant. Standaard ziet u de volledige lijst met installatiekopieën (zowel ingeschakelde als uitgeschakelde). U kunt de lijst filteren om alleen ingeschakelde/uitgeschakelde installatiekopieën te bekijken door de optie **Alleen ingeschakeld**/**Alleen uitgeschakeld** in de vervolgkeuzelijst bovenaan te selecteren. 
    
    ![Pagina Microsoft Azure Marketplace-installatiekopieën](../media/tutorial-setup-lab-account/marketplace-images-page.png)

    De Marketplace-installatiekopieën die worden weergegeven in de lijst, zijn de enige die voldoen aan de volgende voorwaarden:
        
    - Hiermee wordt een enkele VM gemaakt.
    - Maakt gebruik van Azure Resource Manager om VM’s in te richten
    - Hiervoor hoeft u geen extra licentieabonnement aan te schaffen
2. Als u een Microsoft Azure Marketplace-installatiekopie die is ingeschakeld wilt **uitschakelen**, voert u een van de volgende acties uit: 
    1. Selecteer **... (beletselteken)**  in de laatste kolom en selecteer **Installatiekopie uitschakelen**. 

        ![Een installatiekopie uitschakelen](../media/tutorial-setup-lab-account/disable-one-image.png) 
    2. Selecteer een of meer installatiekopieën in de lijst door de selectievakjes bij de namen van de installatiekopieën in de lijst te selecteren en **Geselecteerde installatiekopieën uitschakelen** te selecteren. 

        ![Meerdere installatiekopieën uitschakelen](../media/tutorial-setup-lab-account/disable-multiple-images.png) 
1. Als u een Microsoft Azure Marketplace-installatiekopie wilt **inschakelen**, voert u een van de volgende acties uit: 
    1. Selecteer **... (beletselteken)**  in de laatste kolom en selecteer **Installatiekopie inschakelen**. 
    2. Selecteer een of meer installatiekopieën in de lijst door de selectievakjes bij de namen van de installatiekopieën in de lijst te selecteren en **Geselecteerde installatiekopieën inschakelen** te selecteren. 

## <a name="view-lab-accounts"></a>Lab-accounts weergeven
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle resources** in het menu. 
3. Selecteer **Lab-Accounts** voor de **type**. 
    U kunt ook filteren op abonnement, resourcegroep, locaties en tags. 

    ![Alle resources -> Lab-Accounts](../media/how-to-manage-lab-accounts/all-resources-lab-accounts.png)


## <a name="delete-a-lab-account"></a>Een lab-account verwijderen
Volg de instructies uit de vorige sectie die lab-accounts in een lijst weergegeven. Gebruik de volgende instructies om een lab-account te verwijderen: 

1. Selecteer de **lab-account** die u wilt verwijderen. 
2. Selecteer **verwijderen** via de werkbalk. 

    ![Lab-Accounts -> knop verwijderen](../media/how-to-manage-lab-accounts/delete-button.png)
1. Type **Ja** ter bevestiging.
1. Selecteer **Verwijderen**. 

    ![Lab-account - bevestiging verwijderen](../media/how-to-manage-lab-accounts/delete-lab-account-confirmation.png)

## <a name="view-and-manage-labs-in-the-lab-account"></a>Weergeven en labs in het lab-account beheren

1. Op de **Lab-Account** weergeeft, schakelt **Labs** in het menu links.

    ![Labs in het account](../media/how-to-manage-lab-accounts/labs-in-account.png)
1. U ziet een **lijst met labs** in het account met de volgende informatie: 
    1. De naam van het testlab.
    2. De datum waarop het lab is gemaakt. 
    3. E-mailadres van de gebruiker die het lab gemaakt. 
    4. Maximum aantal gebruikers dat is toegestaan in het lab. 
    5. De status van het testlab. 

## <a name="delete-a-lab-in-the-lab-account"></a>Een lab in het lab-account verwijderen
Volg de instructies in de vorige sectie voor een overzicht van de labs in het lab-account.

1. Selecteer **... (ellips)** , en selecteer **verwijderen**. 

    ![Verwijderen van een lab - knop](../media/how-to-manage-lab-accounts/delete-lab-button.png)
2. Selecteer **Ja** op het waarschuwingsbericht staan aangegeven. 

    ![Lab verwijderen bevestigen](../media/how-to-manage-lab-accounts/confirm-lab-delete.png)

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

- [Labs maken en beheren als labeigenaar](how-to-manage-classroom-labs.md)
- [Sjablonen instellen en publiceren als labeigenaar](how-to-create-manage-template.md)
- [Het gebruik van een lab configureren en beheren als labeigenaar](how-to-configure-student-usage.md)
- [Als een lab-gebruiker toegang krijgen tot leslokaallabs](how-to-use-classroom-lab.md)
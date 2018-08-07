---
title: Een lab-account met Azure Lab Services instellen | Microsoft Docs
description: In deze zelfstudie stelt u een lab-account in met Azure Lab Services.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 07/17/2018
ms.author: spelluru
ms.openlocfilehash: b60c1e84eb5b62bfce0eb2ba96129deeee2fc3c3
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39345305"
---
# <a name="tutorial-set-up-a-lab-account-with-azure-lab-services"></a>Zelfstudie: Een lab-account instellen met Azure Lab Services
In Azure Lab Services fungeert een lab-account als het centrale account waarin alle labs in uw organisatie worden beheerd. In uw lab-account machtigt u anderen om labs te maken en stelt u beleidsregels in die van toepassing zijn op alle labs in het lab-account. In deze zelfstudie leert u hoe u een lab-account maakt als lab-beheerder. 

In deze zelfstudie voert u de volgende acties uit:

> [!div class="checklist"]
> * Een lab-account maken
> * Een gebruiker toevoegen aan de rol Labmaker
> * Microsoft Azure Marketplace-installatiekopieën die beschikbaar zijn voor eigenaars van een lab opgeven

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="create-a-lab-account"></a>Een lab-account maken
In de volgende stappen ziet u hoe u Azure Portal kunt gebruiken om een lab te maken met Azure Lab Services. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Een resource maken** in het hoofdmenu aan de linkerkant.
3. Zoek naar **Lab Services** in Azure Marketplace en selecteer **Lab Services** in de vervolgkeuzelijst. 
4. Selecteer **Lab Services (preview)** in de gefilterde lijst met services. 
1. Selecteer **Maken** in het venster **Een lab-account maken**.
2. Voer in het venster **Lab-account** de volgende acties uit: 
    1. Voer een naam in bij **lab-accountnaam**. 
    2. Selecteer het **Azure-abonnement** waarin u het lab-account wilt maken.
    3. Selecteer voor **Resourcegroep** de optie **Nieuwe maken** en voer een naam in voor de resourcegroep.
    4. Selecteer voor **Locatie** een locatie/regio waarin het lab-account moet worden gemaakt. 
    5. Selecteer **Maken**. 

        ![Venster Een lab-account maken](../media/tutorial-setup-lab-account/lab-account-settings.png)
5. Als u de pagina voor het lab-account niet ziet, selecteert u de knop **Meldingen** en klikt u vervolgens op de knop **Ga naar resource** in de meldingen. 

    ![Venster Een lab-account maken](../media/tutorial-setup-lab-account/notification-go-to-resource.png)    
6. U ziet de volgende pagina **lab-account**:

    ![Pagina lab-account](../media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Een gebruiker toevoegen aan de rol Labmaker
Om een leslokaallab in een labaccount in te kunnen stellen, moet de gebruiker lid zijn van de rol **Labmaker** in het labaccount. Het account dat u hebt gebruikt voor het maken van het lab-account wordt automatisch toegevoegd aan deze rol. Als u van plan bent een leslokaallab te maken met hetzelfde gebruikersaccount, kunt u deze stap overslaan. Als u een ander gebruikersaccount wilt gebruiken om een leslokaallab te maken, voert u de volgende stappen uit: 

Als u machtigingen wilt toekennen aan docenten om labs te maken voor hun lessen, voegt u hen toe aan de rol **Labmaker**:

1. Selecteer op de pagina **lab-account** de optie **Toegangsbeheer (IAM)** en klik op **+ Toevoegen** in de werkbalk. 

    ![Pagina lab-account](../media/tutorial-setup-lab-account/access-control.png)
2. Selecteer op de pagina **Machtigingen toevoegen** de optie **Labmaker** als **rol**. Selecteer de gebruiker die u wilt toevoegen aan de rol Labmaker en selecteer **Opslaan**. 

    ![Gebruiker toevoegen aan de rol Labmaker](../media/tutorial-setup-lab-account/add-user-to-lab-creator-role.png)

## <a name="specify-marketplace-images-available-to-lab-owners"></a>Microsoft Azure Marketplace-installatiekopieën die beschikbaar zijn voor eigenaars van een lab opgeven
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

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u een lab-account gemaakt. Ga voor meer informatie over het maken van een lab voor het leslokaal naar de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Een leslokaal-lab instellen](tutorial-setup-classroom-lab.md)


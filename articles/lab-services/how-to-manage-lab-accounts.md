---
title: Lab-accounts in Azure Lab Services beheren | Microsoft Docs
description: Informatie over het maken van een account lab, alle lab accounts weergeven of verwijderen van een lab-account in een Azure-abonnement.
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
ms.date: 04/20/2018
ms.author: spelluru
ms.openlocfilehash: 53494abead5701052f6e08f68b01ccdf1bfa211c
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="manage-lab-accounts-in-azure-lab-services-formerly-azure-devtest-labs"></a>Lab-accounts in Azure Lab Services (voorheen Azure DevTest Labs) beheren
In dit artikel wordt beschreven hoe een lab-account maken, alle lab accounts weergeven of verwijderen van een lab-account.

## <a name="create-a-lab-account"></a>Een lab-account maken
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het hoofdmenu aan de linkerkant **maken van een resource**.
3. Zoeken naar **Lab Services** in de Azure Marketplace en selecteer **Lab Services** in de vervolgkeuzelijst. 
4. Selecteer **Lab Sercices (Preview)** in de lijst flitered van services. 
5. In de **een lab-account maken** Selecteer **maken**.
7. In de **Lab account** venster de volgende acties uitvoeren: 
    1. Voor **Lab accountnaam**, voer een naam in. 
    2. Selecteer de **Azure-abonnement** in die u wilt maken van het lab-account.
    3. Voor **resourcegroep**, selecteer **nieuw**, en voer een naam voor de resourcegroep.
    4. Voor **locatie**, selecteer een locatie/de regio waarin u wilt dat het lab-account moet worden gemaakt. 
    5. Selecteer **Maken**. 

        ![Maken van een venster van de account lab](./media/how-to-manage-lab-accounts/lab-account-settings.png)
5. Als u de pagina voor het lab-account niet ziet, selecteert u de **meldingen** knop en klik vervolgens op **gaat u naar de resource** knop in de meldingen. 

    ![Maken van een venster van de account lab](./media/how-to-manage-lab-accounts/notification-go-to-resource.png)    
6. U ziet het volgende **lab account** pagina:

    ![De pagina serviceaccount Lab](./media/how-to-manage-lab-accounts/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Een gebruiker toevoegen aan de rol van Lab
De machtiging voor het maken van labs voor hun klassen, toe te voegen aan de rol van Lab voor onderwijsinstellingen opgeven:

1. Op de **Lab Account** pagina **toegangsbeheer (IAM)**, en klik op **+ toevoegen** op de werkbalk. 

    ![De pagina serviceaccount Lab](./media/tutorial-setup-lab-account/access-control.png)
2. Op de **machtigingen toevoegen** pagina **Lab Maker** voor **rol**, selecteert u de gebruiker die u wilt toevoegen aan de rol Beveiligingsbeheerder Lab en selecteer **opslaan**. 

    ![Gebruiker toevoegen aan de rol van Lab](./media/tutorial-setup-lab-account/add-user-to-lab-creator-role.png)


## <a name="view-lab-accounts"></a>Weergave lab accounts
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle resources** in het menu. 
3. Selecteer **Lab Services** voor de **type**. 
    U kunt ook filteren op abonnement, resourcegroep, locaties en labels. 

## <a name="delete-a-lab-account"></a>Een lab-account verwijderen
Volg de instructies van de vorige sectie lab accounts in een lijst weergegeven. Gebruik de volgende instructies om een lab-account te verwijderen: 

1. Selecteer de **lab account** die u wilt verwijderen. 
2. Selecteer **verwijderen** van de werkbalk. 
3. Type **Ja** ter bevestiging.
4. Selecteer **Verwijderen**. 

## <a name="next-steps"></a>Volgende stappen
Aan de slag met het instellen van een testomgeving met Azure Lab Services:

- [Een testomgeving leslokaal instellen](tutorial-setup-classroom-lab.md)
- [Een aangepaste testomgeving instellen](tutorial-create-custom-lab.md)

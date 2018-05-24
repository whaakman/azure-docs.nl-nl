---
title: Een lab-account met Azure Lab Services instellen | Microsoft Docs
description: In deze zelfstudie stelt u een lab-account in met Azure Lab Services (voorheen DevTest Labs).
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
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: 22a9e90404475e8ff1f1ea72c233b1abfed938f6
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34361372"
---
# <a name="tutorial-set-up-a-lab-account-with-azure-lab-services-formerly-azure-devtest-labs"></a>Zelfstudie: Een lab-account instellen met Azure Lab Services (voorheen DevTest Labs)
In deze zelfstudie fungeert u als lab-beheerder om een lab-account te maken met Azure Lab Services. Vervolgens kent u machtigingen toe aan docenten om labs te maken voor hun lessen in dit lab-account. Een docent kan een lab instellen voor een lesomgeving met de [Azure Lab Services-website](https://labs.azure.com).   

In deze zelfstudie voert u de volgende acties uit:

> [!div class="checklist"]
> * Een lab-account maken
> * Een gebruiker toevoegen aan de rol Labmaker

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

        ![Venster Een lab-account maken](./media/tutorial-setup-lab-account/lab-account-settings.png)
5. Als u de pagina voor het lab-account niet ziet, selecteert u de knop **Meldingen** en klikt u vervolgens op de knop **Ga naar resource** in de meldingen. 

    ![Venster Een lab-account maken](./media/tutorial-setup-lab-account/notification-go-to-resource.png)    
6. U ziet de volgende pagina **lab-account**:

    ![Pagina lab-account](./media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Een gebruiker toevoegen aan de rol Labmaker
Als u machtigingen wilt toekennen aan docenten om labs te maken voor hun lessen, voegt u hen toe aan de rol Labmaker:

1. Selecteer op de pagina **lab-account** de optie **Toegangsbeheer (IAM)** en klik op **+ Toevoegen** in de werkbalk. 

    ![Pagina lab-account](./media/tutorial-setup-lab-account/access-control.png)
2. Selecteer op de pagina **Machtigingen toevoegen** de optie **Labmaker** als **rol**. Selecteer de gebruiker die u wilt toevoegen aan de rol Labmaker en selecteer **Opslaan**. 

    ![Gebruiker toevoegen aan de rol Labmaker](./media/tutorial-setup-lab-account/add-user-to-lab-creator-role.png)


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u een lab-account gemaakt. Ga voor meer informatie over het maken van een lab voor het leslokaal naar de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Een leslokaal-lab instellen](tutorial-setup-classroom-lab.md)


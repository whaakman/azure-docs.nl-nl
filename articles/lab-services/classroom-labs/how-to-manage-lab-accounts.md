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
ms.date: 05/07/2019
ms.author: spelluru
ms.openlocfilehash: 6f283ce007e96547e01a01a3753ddcb60574bfc3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65412806"
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
    5. Selecteer een bestaande **gedeelde afbeeldingengalerie** of er een maken. U kunt de VM-sjabloon opslaan in de galerie met installatiekopieën van de gedeelde voor het opnieuw door anderen worden gebruikt. Zie voor gedetailleerde informatie over gedeelde afbeeldingsgalerieën [een galerie met gedeelde installatiekopieën in Azure Lab Services gebruikt](how-to-use-shared-image-gallery.md).
    6. Voor **virtuele peernetwerk**, selecteert u een peer-netwerk (VNet voor het testlabnetwerk). Labs gemaakt in dit account zijn verbonden met het geselecteerde VNet en hebben toegang tot de resources in het geselecteerde VNet. 
    7. Geef een **adresbereik** voor virtuele machines in het lab. Het adresbereik moet zich in de notatie klasseloze routing tussen domeinen (CIDR) (voorbeeld: 10.20.0.0/23). Virtuele machines in de testomgeving wordt in dit adresbereik worden gemaakt. Zie voor meer informatie, [Geef een adresbereik voor virtuele machines in het lab](how-to-configure-lab-accounts.md#specify-an-address-range-for-vms-in-the-lab).    
    8. Voor het veld **Labmaker toestaan om lablocatie te kiezen** geeft u op of u wilt dat labmakers een locatie voor het lab kunnen selecteren. De optie is standaard uitgeschakeld. Wanneer deze is uitgeschakeld, kunnen labmakers geen locatie opgeven voor het lab dat ze maken. De labs worden gemaakt in de dichtstbijzijnde geografische locatie in het labaccount. Wanneer deze is ingeschakeld, kan de labmaker een locatie selecteren wanneer hij een lab maakt.      
    9. Selecteer **Maken**. 

        ![Venster Een lab-account maken](../media/tutorial-setup-lab-account/lab-account-settings.png)
5. Selecteer het **Klokpictogram** op de werkbalk (**Meldingen**), controleer of de implementatie is voltooid en selecteer vervolgens **Naar de resource gaan**. 

    U kunt ook **Vernieuwen** selecteren op de **Lab-accounts**-pagina en het lab-account selecteren dat u hebt gemaakt. 

    ![Venster Een lab-account maken](../media/tutorial-setup-lab-account/go-to-lab-account.png)    
6. U ziet de volgende pagina **lab-account**:

    ![Pagina lab-account](../media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="view-lab-accounts"></a>Lab-accounts weergeven
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle resources** in het menu. 
3. Selecteer **Lab-Accounts** voor de **type**. 
    U kunt ook filteren op abonnement, resourcegroep, locaties en tags. 

    ![Alle resources -> Lab-Accounts](../media/how-to-manage-lab-accounts/all-resources-lab-accounts.png)

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

## <a name="delete-a-lab-account"></a>Een lab-account verwijderen
Volg de instructies uit de vorige sectie die lab-accounts in een lijst weergegeven. Gebruik de volgende instructies om een lab-account te verwijderen: 

1. Selecteer de **lab-account** die u wilt verwijderen. 
2. Selecteer **verwijderen** via de werkbalk. 

    ![Lab-Accounts -> knop verwijderen](../media/how-to-manage-lab-accounts/delete-button.png)
1. Type **Ja** ter bevestiging.
1. Selecteer **Verwijderen**. 

    ![Lab-account - bevestiging verwijderen](../media/how-to-manage-lab-accounts/delete-lab-account-confirmation.png)


## <a name="next-steps"></a>Volgende stappen
Zie het volgende artikel: [Lab-accounts configureren](how-to-configure-lab-accounts.md).
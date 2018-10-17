---
title: Opzetten van een leslokaallab met Azure Lab Services | Microsoft Docs
description: In deze zelfstudie stelt u een lab in voor gebruik in een leslokaal.
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
ms.date: 10/05/2018
ms.author: spelluru
ms.openlocfilehash: 6696d6e7e53e98dfab2a65c7c66825936020f33c
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48856631"
---
# <a name="tutorial-set-up-a-classroom-lab"></a>Zelfstudie: Een leslokaallab instellen 
In deze zelfstudie stelt u een leslokaallab in met virtuele machines die worden gebruikt door studenten in het leslokaal.  

In deze zelfstudie voert u de volgende acties uit:

> [!div class="checklist"]
> * Een leslokaallab maken
> * Het leslokaallab configureren
> * Registratiekoppeling naar studenten verzenden

## <a name="prerequisites"></a>Vereisten
Als u een leslokaallab in een labaccount instelt, moet u lid zijn van de rol **Labmaker** in het labaccount. Het account dat u hebt gebruikt voor het maken van een labaccount wordt automatisch toegevoegd aan deze rol. Een labeigenaar kan andere gebruikers toevoegen aan de rol Labmaker met behulp van de stappen in het volgende artikel: [Een gebruiker toevoegen aan de rol Labmaker](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).


## <a name="create-a-classroom-lab"></a>Een leslokaallab maken

1. Navigeer naar de [Azure Lab Services-website](https://labs.azure.com). 
2. Selecteer **Aanmelden** en voer uw referenties in. Azure Lab Services ondersteunt organisatieaccounts en Microsoft-accounts. 
3. Voer in het venster **Nieuw lab** de volgende acties uit: 
    1. Geef een **naam** voor uw lab op. 
    2. Geef het maximale **aantal gebruikers** op dat in het lab is toegestaan. 
    6. Selecteer **Opslaan**.

        ![Een leslokaallab maken](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. Voer op de pagina **Specificaties van virtuele machines selecteren** de volgende stappen uit:
    1. Selecteer een **grootte** voor virtuele machines (VM's) die in het lab worden gemaakt. 
    2. Selecteer de **regio** waarin de virtuele machines moeten worden gemaakt. 
    3. Selecteer de **VM-installatiekopie** die moet worden gebruikt voor het maken van virtuele machines in het lab. 
    4. Selecteer **Volgende**.

        ![VM-specificaties opgeven](../media/tutorial-setup-classroom-lab/select-vm-specifications.png)    
5. Geef op de pagina **Referenties instellen** de standaardreferenties voor alle virtuele machines in het lab op. 
    1. Geef de **naam van de gebruiker** op voor alle virtuele machines in het lab.
    2. Geef het **wachtwoord** voor de gebruiker op. 

        > [!IMPORTANT]
        > Noteer de gebruikersnaam en het wachtwoord. Deze worden niet opnieuw weergegeven.
    3. Selecteer **Maken**. 

        ![Referenties instellen](../media/tutorial-setup-classroom-lab/set-credentials.png)
6. Op de pagina **Sjabloon configureren** wordt de status van het maken van het lab weergegeven. Het maken van de sjabloon in het lab duurt maximaal 20 minuten. 

    ![Sjabloon configureren](../media/tutorial-setup-classroom-lab/configure-template.png)
7. Nadat de configuratie van de sjabloon is voltooid, wordt de volgende pagina weergegeven: 

    ![Sjabloonpagina configureren nadat deze is voltooid](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)
8. In deze zelfstudie zijn de volgende stappen optioneel: 
    1. De sjabloon-VM starten door **Starten** te selecteren.
    2. De sjabloon-VM verbinden door **Verbinding maken** te selecteren. 
    3. Software installeren en configureren op uw sjabloon-VM. 
    4. **Stop** de virtuele machine.  
    5. Een **Beschrijving** voor de sjabloon invoeren

        ![Volgende op de pagina Sjabloon configureren](../media/tutorial-setup-classroom-lab/configure-template-next.png)
9. Selecteer **Volgende** op de sjabloonpagina. 
10. Voer op de pagina **De sjabloon publiceren** de volgende acties uit. 
    1. Als u de sjabloon onmiddellijk wilt publiceren, schakelt u het selectievakje in voor *Ik begrijp dat ik de sjabloon niet kan wijzigen nadat deze is gepubliceerd. Dit proces kan slechts één keer worden uitgevoerd en kan tot een uur duren*. Selecteer vervolgens **publiceren**.  

        > [!WARNING]
        > Zodra u de sjabloon hebt gepubliceerd, kan dit niet ongedaan worden gemaakt. 
    2. Als u later wilt publiceren, selecteert u **Opslaan voor later**. Nadat de wizard is voltooid, kunt u de VM-sjabloon publiceren. Zie het gedeelte [De sjabloon publiceren](how-to-manage-classroom-labs.md#publish-the-template) in het artikel [Leslokaallabs beheren](how-to-manage-classroom-labs.md) voor meer informatie over het configureren en publiceren nadat de wizard is voltooid.

        ![Sjabloon publiceren](../media/tutorial-setup-classroom-lab/publish-template.png)
11. U kunt de **voortgang van het publiceren** van de sjabloon bekijken. Dit proces duurt maximaal een uur. 

    ![Sjabloon publiceren - voortgang](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. De volgende pagina wordt weergegeven wanneer de sjabloon is gepubliceerd. Selecteer **Done**.

    ![Sjabloon publiceren - gelukt](../media/tutorial-setup-classroom-lab/publish-success.png)
1. U ziet het **dashboard** voor het lab. 
    
    ![Dashboard Leslokaallab](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)
4. Ga naar de pagina **Virtuele machines** en controleer of u virtuele machines ziet met de status **Niet-toegewezen**. Deze virtuele machines zijn nog niet toegewezen aan studenten. Deze horen de status **Gestopt** te hebben. Op deze pagina kunt u een student-VM starten, verbinding maken met de virtuele machine, de virtuele machine stoppen en de virtuele machine verwijderen. U kunt de virtuele machines zelf starten vanaf deze pagina of ze laten starten door de studenten. 

    ![Virtuele machines met de status Gestopt](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)

## <a name="send-registration-link-to-students"></a>Registratiekoppeling naar studenten verzenden

1. Schakel over naar de weergave **Dashboard** door in het menu links **Dashboard** te selecteren. 
2. Selecteer de tegel **Gebruikersregistratie**.

    ![Registratiekoppeling voor studenten](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. Selecteer in het dialoogvenster **Gebruikersregistratie** de knop **Kopiëren**. De koppeling wordt naar het klembord gekopieerd. Plak deze in een e-mailprogramma en verstuur een e-mail naar de student. 

    ![Registratiekoppeling voor studenten](../media/tutorial-setup-classroom-lab/registration-link.png)
2. Selecteer in het dialoogvenster **Gebruikersregistratie** de optie **Sluiten**. 
3. Deel de registratielink met een student zodat de student zich voor de les kan registreren. 


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u een leslokaallab gemaakt en het lab geconfigureerd. Ga voor meer informatie over hoe een student toegang kan krijgen tot een virtuele machine in het lab met behulp van de registratiekoppeling naar de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Verbinding maken met een virtuele machine in het leslokaallab](tutorial-connect-virtual-machine-classroom-lab.md)


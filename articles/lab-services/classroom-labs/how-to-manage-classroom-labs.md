---
title: Leslokaallabs in Azure Lab Services beheren | Microsoft Docs
description: Informatie over het maken en configureren van een leslokaallab, alle leslokaallabs weergeven, de registratiekoppeling delen met een lab-gebruiker of verwijderen van een lab.
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
ms.date: 02/07/2019
ms.author: spelluru
ms.openlocfilehash: c6ae28e076d14faa7c2173f3a23d92daad4bd59e
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58651120"
---
# <a name="manage-classroom-labs-in-azure-lab-services"></a>Leslokaallabs in Azure Lab Services beheren 
In dit artikel wordt beschreven hoe u maken en verwijderen van een leslokaallab. Deze leest u ook alle leslokaallabs weergeven in een lab-account. 

## <a name="prerequisites"></a>Vereisten
Als u een leslokaallab in een labaccount instelt, moet u lid zijn van de rol **Labmaker** in het labaccount. Het account dat u hebt gebruikt voor het maken van een labaccount wordt automatisch toegevoegd aan deze rol. Een labeigenaar kan andere gebruikers toevoegen aan de rol Labmaker met behulp van de stappen in het volgende artikel: [Een gebruiker toevoegen aan de rol Labmaker](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).

## <a name="create-a-classroom-lab"></a>Een leslokaallab maken

1. Navigeer naar de [Azure Lab Services-website](https://labs.azure.com). 
2. Selecteer **aanmelden**. Selecteer of typ een **gebruikers-ID** dat wil zeggen een lid van de **Labmaker** rol in het lab-account en wachtwoord invoeren. Azure Lab Services ondersteunt organisatieaccounts en Microsoft-accounts. 
3. Voer in het venster **Nieuw lab** de volgende acties uit: 
    1. Geef een **naam** voor uw lab op. 
    2. Geef het maximale **aantal virtuele machines** in het lab op. U kunt vergroten of verkleinen van het aantal virtuele machines in het lab later opnieuw. 
    6. Selecteer **Opslaan**.

        ![Een leslokaallab maken](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. Voer op de pagina **Specificaties van virtuele machines selecteren** de volgende stappen uit:
    1. Selecteer een **grootte** voor virtuele machines (VM's) die in het lab worden gemaakt. Op dit moment **kleine**, **gemiddeld**, **grote**, en **GPU** grootten zijn toegestaan.
    2. Selecteer de **regio** waarin de virtuele machines moeten worden gemaakt. 
    3. Selecteer de **VM-installatiekopie** die moet worden gebruikt voor het maken van virtuele machines in het lab. Als u een installatiekopie van Linux selecteert, ziet u een optie voor het inschakelen van de verbinding met extern bureaublad voor. Zie voor meer informatie, [verbinding met extern bureaublad voor Linux inschakelen](how-to-enable-remote-desktop-linux.md).
    4. Selecteer **Volgende**.

        ![VM-specificaties opgeven](../media/tutorial-setup-classroom-lab/select-vm-specifications.png)    
5. Geef op de pagina **Referenties instellen** de standaardreferenties voor alle virtuele machines in het lab op. 
    1. Geef de **naam van de gebruiker** op voor alle virtuele machines in het lab.
    2. Geef het **wachtwoord** voor de gebruiker op. 

        > [!IMPORTANT]
        > Noteer de gebruikersnaam en het wachtwoord. Deze worden niet opnieuw weergegeven.
    3. Selecteer **Maken**. 

        ![Referenties instellen](../media/tutorial-setup-classroom-lab/set-credentials.png)
6. Op de pagina **Sjabloon configureren** wordt de status van het maken van het lab weergegeven. Het maken van de sjabloon in het lab duurt maximaal 20 minuten. Een sjabloon in een lab is een basiskopie van de virtuele machine waarvan de virtuele machines van alle gebruikers worden gemaakt. Stel de sjabloon voor de virtuele machine zo in dat deze is geconfigureerd met precies wat u de labgebruikers wilt bieden.  

    ![Sjabloon configureren](../media/tutorial-setup-classroom-lab/configure-template.png)
7. Nadat de configuratie van de sjabloon is voltooid, wordt de volgende pagina weergegeven: 

    ![Sjabloonpagina configureren nadat deze is voltooid](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)
8. In deze zelfstudie zijn de volgende stappen optioneel: 
    1. De sjabloon-VM starten door **Starten** te selecteren.
    2. De sjabloon-VM verbinden door **Verbinding maken** te selecteren. Als het een basis van Linux virtuele machine, kunt u kiezen of u verbinding maken met SSH of RDP wilt (als RDP is ingeschakeld).
    3. Software installeren en configureren op uw sjabloon-VM. 
    4. **Stop** de virtuele machine.  
    5. Een **Beschrijving** voor de sjabloon invoeren

        ![Volgende op de pagina Sjabloon configureren](../media/tutorial-setup-classroom-lab/configure-template-next.png)
9. Selecteer **Volgende** op de sjabloonpagina. 
10. Voer op de pagina **De sjabloon publiceren** de volgende acties uit. 
    1. Als u de sjabloon onmiddellijk wilt publiceren, schakelt u het selectievakje in voor *Ik begrijp dat ik de sjabloon niet kan wijzigen nadat deze is gepubliceerd. Dit proces kan slechts één keer worden uitgevoerd en kan tot een uur duren*. Selecteer vervolgens **publiceren**.  Publiceer de sjabloon om exemplaren van de sjabloon-VM beschikbaar te maken voor de labgebruikers.

        > [!WARNING]
        > Zodra u de sjabloon hebt gepubliceerd, kan dit niet ongedaan worden gemaakt. 
    2. Als u later wilt publiceren, selecteert u **Opslaan voor later**. Nadat de wizard is voltooid, kunt u de VM-sjabloon publiceren. Zie voor meer informatie over het configureren en publiceren nadat de wizard is voltooid, voor meer informatie over het configureren en publiceren nadat de wizard is voltooid, ziet u de sjabloon in sectie publiceren de [leslokaallabs beheren](how-to-manage-classroom-labs.md) artikel.

        ![Sjabloon publiceren](../media/tutorial-setup-classroom-lab/publish-template.png)
11. U kunt de **voortgang van het publiceren** van de sjabloon bekijken. Dit proces duurt maximaal een uur. 

    ![Sjabloon publiceren - voortgang](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. De volgende pagina wordt weergegeven wanneer de sjabloon is gepubliceerd. Selecteer **Done**.

    ![Sjabloon publiceren - gelukt](../media/tutorial-setup-classroom-lab/publish-success.png)
1. U ziet het **dashboard** voor het lab. 
    
    ![Dashboard Leslokaallab](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)
4. Ga naar de pagina **Virtuele machines** en controleer of u virtuele machines ziet met de status **Niet-toegewezen**. Deze virtuele machines zijn nog niet toegewezen aan studenten. Deze horen de status **Gestopt** te hebben. Op deze pagina kunt u een student-VM starten, verbinding maken met de virtuele machine, de virtuele machine stoppen en de virtuele machine verwijderen. U kunt de virtuele machines zelf starten vanaf deze pagina of ze laten starten door de studenten. 

    ![Virtuele machines met de status Gestopt](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)


## <a name="view-all-classroom-labs"></a>Alle leslokaallabs weergeven
1. Navigeer naar [Azure Lab Services-portal](https://labs.azure.com).
2. Selecteer **aanmelden**. Selecteer of typ een **gebruikers-ID** dat wil zeggen een lid van de **Labmaker** rol in het lab-account en wachtwoord invoeren. Azure Lab Services ondersteunt organisatieaccounts en Microsoft-accounts. 
3. Bevestig dat u ziet dat de labs in de geselecteerde lab-account. 

    ![Alle labs](../media/how-to-manage-classroom-labs/all-labs.png)
3. Gebruik de vervolgkeuzelijst boven een andere lab-account selecteren. Ziet u labs in de geselecteerde lab-account. 

## <a name="delete-a-classroom-lab"></a>Een leslokaallab verwijderen
1. Selecteer de drie puntjes (...) in de linkerbovenhoek op de tegel voor de testomgeving. 

    ![Het lab selecteren](../media/how-to-manage-classroom-labs/select-three-dots.png)
2. Selecteer **Verwijderen**. 

    ![De knop Verwijderen](../media/how-to-manage-classroom-labs/delete-button.png)
3. Op de **verwijderen lab** in het dialoogvenster, selecteer **verwijderen**. 

    ![In het dialoogvenster verwijderen](../media/how-to-manage-classroom-labs/delete-lab-dialog-box.png)

## <a name="switch-to-another-classroom-lab"></a>Schakel over naar een andere leslokaallab
Als u wilt overschakelen naar een andere leslokaallab vanuit de huidige, selecteert u de vervolgkeuzelijst van labs in het lab-account aan de bovenkant.

![Selecteer in het lab in vervolgkeuzelijst boven](../media/how-to-manage-classroom-labs/switch-lab.png)


## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

- [Sjablonen instellen en publiceren als labeigenaar](how-to-create-manage-template.md)
- [Het gebruik van een lab configureren en beheren als labeigenaar](how-to-configure-student-usage.md)
- [Als een lab-gebruiker toegang krijgen tot leslokaallabs](how-to-use-classroom-lab.md)


---
title: Leslokaallabs in Azure Lab Services beheren | Microsoft Docs
description: Meer informatie over het maken en configureren van een leslokaallab, alle de leslokaallabs, gedeeld de registratie te koppelen met een lab-gebruiker of verwijderen van een lab te bekijken.
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
ms.date: 10/01/2018
ms.author: spelluru
ms.openlocfilehash: eebdc98db5ecdf518d3b0b58e6757a2b7ecd5dd7
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48043801"
---
# <a name="manage-classroom-labs-in-azure-lab-services"></a>Leslokaallabs in Azure Lab Services beheren 
In dit artikel wordt beschreven hoe u maken en configureren van een leslokaallab, alle leslokaallabs weergeven of een leslokaallab verwijderen.

## <a name="prerequisites"></a>Vereisten
Als u een leslokaallab in een labaccount instelt, moet u lid zijn van de rol **Labmaker** in het labaccount. Het account dat u hebt gebruikt voor het maken van een labaccount wordt automatisch toegevoegd aan deze rol. Een labeigenaar kan andere gebruikers toevoegen aan de rol Labmaker met behulp van de stappen in het volgende artikel: [Een gebruiker toevoegen aan de rol Labmaker](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).

## <a name="create-a-classroom-lab"></a>Een leslokaallab maken

1. Navigeer naar de [Azure Lab Services-website](https://labs.azure.com). 
2. Selecteer **Aanmelden** en voer uw referenties in. Azure Lab Services ondersteunt organisatieaccounts en Microsoft-accounts. 
3. Voer in het venster **Nieuw lab** de volgende acties uit: 
    1. Geef een **naam** voor uw testomgeving. 
    2. Geef de maximale **aantal gebruikers** toegestaan in het lab. 
    6. Selecteer **Opslaan**.

        ![Een leslokaallab maken](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. Op de **specificaties van virtuele machines selecteren** pagina, de volgende stappen uit:
    1. Selecteer een **grootte** voor virtuele machines (VM's) gemaakt in het lab. 
    2. Selecteer de **regio** in die u wilt dat de virtuele machines moet worden gemaakt. 
    3. Selecteer de **VM-installatiekopie** om te worden gebruikt voor het maken van virtuele machines in het lab. 
    4. Selecteer **Volgende**.

        ![Geef de specificaties van virtuele machine](../media/tutorial-setup-classroom-lab/select-vm-specifications.png)    
5. Op de **referenties instellen** pagina, geef de standaardreferenties voor alle virtuele machines in het lab. 
    1. Geef de **naam van de gebruiker** voor alle virtuele machines in het lab.
    2. Geef de **wachtwoord** voor de gebruiker. 

        > [!IMPORTANT]
        > Maak een notitie van gebruikersnaam en wachtwoord. Deze niet opnieuw weergegeven.
    3. Selecteer **Maken**. 

        ![Referenties instellen](../media/tutorial-setup-classroom-lab/set-credentials.png)
6. Op de **configureren sjabloon** pagina, ziet u de status van het proces voor het maken van lab. Het maken van de sjabloon in het lab duurt maximaal 20 minuten. 

    ![Sjabloon configureren](../media/tutorial-setup-classroom-lab/configure-template.png)
7. Nadat de configuratie van de sjabloon voltooid is, ziet u de volgende pagina: 

    ![Sjabloonpagina configureren nadat deze voltooid](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)
8. De volgende stappen zijn optionele stappen in deze zelfstudie: 
    1. Selecteer eerst de VM-sjabloon **Start**.
    2. Verbinding maken met de VM-sjabloon door het selecteren van **Connect**. 
    3. Installeren en configureren van software op uw VM-sjabloon. 
    4. **Stop** de virtuele machine.  
    5. Voer een **beschrijving** voor de sjabloon

        ![Volgende klikken op de pagina sjabloon configureren](../media/tutorial-setup-classroom-lab/configure-template-next.png)
9. Selecteer **volgende** op de sjabloonpagina. 
10. Op **publiceren van de sjabloon** pagina, de volgende acties uitvoeren. 
    1. Als u de sjabloon onmiddellijk publiceren, selecteert u het selectievakje voor *ik begrijp dat ik kan de sjabloon niet wijzigen nadat deze is gepubliceerd. Dit proces kan slechts één keer uitgevoerd en een uur kan duren*, en selecteer **publiceren**.  

        > [!WARNING]
        > Wanneer u publiceert, kunt u kan niet ongedaan. 
    2. Als u later publiceren, selecteert u **opslaan voor later**. Nadat de wizard is voltooid, kunt u de VM-sjabloon publiceren. Zie voor meer informatie over het configureren en publiceren nadat de wizard is voltooid, Zie voor meer informatie over het configureren en publiceren nadat de wizard is voltooid, [publiceren van de sjabloon](#publish-the-template) sectie de [leslokaallabs beheren ](how-to-manage-classroom-labs.md) artikel.

        ![Sjabloon publiceren](../media/tutorial-setup-classroom-lab/publish-template.png)
11. U ziet de **voortgang van de publicatie** de sjabloon. Dit proces kan een uur duren. 

    ![Sjabloon - voortgang publiceren](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. U ziet de volgende pagina wanneer de sjabloon is gepubliceerd. Selecteer **Done**.

    ![Publiceren van de sjabloon: geslaagd](../media/tutorial-setup-classroom-lab/publish-success.png)
1. U ziet het **dashboard** voor het lab. 
    
    ![Dashboard Leslokaallab](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)
4. Ga naar de pagina **Virtuele machines** en controleer of u virtuele machines ziet met de status **Niet-toegewezen**. Deze virtuele machines zijn nog niet toegewezen aan studenten. Deze horen de status **Gestopt** te hebben. Op deze pagina kunt u een student-VM starten, verbinding maken met de virtuele machine, de virtuele machine stoppen en de virtuele machine verwijderen. U kunt de virtuele machines zelf starten vanaf deze pagina of ze laten starten door de studenten. 

    ![Virtuele machines met de status Gestopt](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)

## <a name="configure-usage-policy"></a>Gebruiksbeleid configureren

1. Selecteer **Gebruiksbeleid**. 
2. Voer in het **Gebruiksbeleid**, Instellingen, het **aantal gebruikers** in dat het lab mag gebruiken.
3. Selecteer **Opslaan**. 

    ![Gebruiksbeleid](../media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="set-up-the-template"></a>De sjabloon instellen
Een sjabloon in een lab is een basiskopie van de virtuele machine waarvan de virtuele machines van alle gebruikers worden gemaakt. Stel de sjabloon voor de virtuele machine zo in dat deze is geconfigureerd met precies wat u de labgebruikers wilt bieden. U kunt een naam en een beschrijving opgeven voor de sjabloon die de gebruikers van het lab kunnen zien. Publiceer de sjabloon om exemplaren van de sjabloon-VM beschikbaar te maken voor de labgebruikers.  

### <a name="set-template-title-and-description"></a>Set-Sjabloontitel en beschrijving
1. Selecteer in de sectie **Sjabloon** de optie **Bewerken** (potloodpictogram) voor de sjabloon. 
2. Voer in het venster **Gebruikersweergave** een **titel** in voor de sjabloon.
3. Voer een **Beschrijving** in voor de sjabloon.
4. Selecteer **Opslaan**.

    ![Beschrijving van leslokaallab](../media/how-to-manage-classroom-labs/lab-description.png)

### <a name="set-up-the-template-vm"></a>De sjabloon-VM instellen
 Maak verbinding met de sjabloon-VM en installeer de vereiste software erop voordat u deze beschikbaar maakt voor uw studenten. 

1. Wacht totdat de sjabloon-VM gereed is. Zodra de VM gereed is, wordt de knop **Starten** beschikbaar. Selecteer **Starten** om de VM te starten.

    ![De sjabloon-VM starten](../media/tutorial-setup-classroom-lab/start-template-vm.png)
1. Selecteer **Verbinding maken** en volg de aanwijzingen om de verbinding met de VM tot stand te brengen. 

    ![Verbinding maken met de sjabloon-VM](../media/tutorial-setup-classroom-lab/connect-template-vm.png)
1. Installeer alle software die de studenten nodig hebben om het lab te doorlopen (bijvoorbeeld Visual Studio, Azure Storage Explorer, enzovoort). 
2. Verbreek de verbinding met de sjabloon-VM (sluit de Extern bureaublad-sessie). 
3. **Stop** de sjabloon-VM door **Stoppen** te selecteren. 

    ![De sjabloon-VM stoppen](../media/tutorial-setup-classroom-lab/stop-template-vm.png)


## <a name="publish-the-template"></a>De sjabloon publiceren 
Wanneer u een sjabloon publiceert, maakt Azure Lab Services virtuele machines in het lab met behulp van de sjabloon. Het aantal virtuele machines dat in dit proces wordt gemaakt, is hetzelfde als het maximum aantal gebruikers dat in het lab is toegestaan. Dit kunt u instellen in het gebruiksbeleid van het lab. Alle virtuele machines hebben dezelfde configuratie als de sjabloon. 

1. Selecteer **Publiceren** in de sectie **Sjabloon**. 

    ![De sjabloon-VM publiceren](../media/tutorial-setup-classroom-lab/public-access.png)
1. Selecteer in het venster **Publiceren** de optie **Gepubliceerd**. 
2. Selecteer nu de knop **Publiceren**. Dit proces kan enige tijd duren, afhankelijk van het aantal virtuele machines dat wordt gemaakt. Het aantal VM’s dat wordt gemaakt, is gelijk aan het aantal gebruikers dat is toegestaan in het lab.
    
    > [!IMPORTANT]
    > Zodra een sjabloon is gepubliceerd, kan dit niet meer ongedaan worden gemaakt. 
4. Ga naar de pagina **Virtuele machines** en controleer of u virtuele machines ziet met de status **Niet-toegewezen**. Deze virtuele machines zijn nog niet toegewezen aan studenten. 

    ![Virtuele machines](../media/tutorial-setup-classroom-lab/virtual-machines.png)
5. Wacht totdat de virtuele machines zijn gemaakt. Deze horen de status **Gestopt** te hebben. Op deze pagina kunt u een student-VM starten, verbinding maken met de virtuele machine, de virtuele machine stoppen en de virtuele machine verwijderen. U kunt de virtuele machines zelf starten vanaf deze pagina of ze laten starten door de studenten. 

    ![Virtuele machines met de status Gestopt](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)


## <a name="send-registration-link-to-students"></a>Registratiekoppeling naar studenten verzenden

1. Schakel over naar de weergave **Dashboard**. 
2. Selecteer de tegel **Gebruikersregistratie**.

    ![Registratiekoppeling voor studenten](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. Selecteer in het dialoogvenster **Gebruikersregistratie** de knop **Kopiëren**. De koppeling wordt naar het klembord gekopieerd. Plak deze in een e-mailprogramma en verstuur een e-mail naar de student. 

    ![Registratiekoppeling voor studenten](../media/tutorial-setup-classroom-lab/registration-link.png)
2. Selecteer in het dialoogvenster **Gebruikersregistratie** de optie **Sluiten**. 

## <a name="view-all-classroom-labs"></a>Alle leslokaallabs weergeven
1. Navigeer naar [Azure Lab Services-portal](https://labs.azure.com).
2. Bevestig dat u ziet dat de labs in de geselecteerde lab-account. 

    ![Alle labs](../media/how-to-manage-classroom-labs/all-labs.png)
3. Gebruik de vervolgkeuzelijst boven een andere lab-account selecteren. Ziet u labs in de geselecteerde lab-account. 

## <a name="delete-a-classroom-lab"></a>Een leslokaallab verwijderen
1. Selecteer de drie puntjes (...) in de linkerbovenhoek op de tegel voor de testomgeving. 

    ![Het lab selecteren](../media/how-to-manage-classroom-labs/select-three-dots.png)
2. Selecteer **Verwijderen**. 

    ![Knop verwijderen](../media/how-to-manage-classroom-labs/delete-button.png)
3. Op de **verwijderen lab** in het dialoogvenster, selecteer **verwijderen**. 

    ![In het dialoogvenster verwijderen](../media/how-to-manage-classroom-labs/delete-lab-dialog-box.png)
 
## <a name="manage-student-vms"></a>Virtuele machines van studenten beheren
Wanneer studenten registreren met Azure Lab Services met behulp van de registratie van de koppeling opgegeven, ziet u de virtuele machines toegewezen aan studenten op de **virtuele machines** tabblad. 

![Virtuele machines die zijn toegewezen aan studenten](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

U kunt de volgende taken op een student VM kunt doen: 

- Een virtuele machine stoppen als de virtuele machine wordt uitgevoerd. 
- Start een virtuele machine als de virtuele machine is gestopt. 
- Maak verbinding met de VM. 
- De virtuele machine verwijderen. 

## <a name="next-steps"></a>Volgende stappen
Aan de slag met het installeren van een lab met Azure Lab Services:

- [Een leslokaallab instellen](how-to-manage-classroom-labs.md)
- [Een lab instellen](../tutorial-create-custom-lab.md)

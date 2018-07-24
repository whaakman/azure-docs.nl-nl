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
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: 48056d6e2988dd674351aca83526032175c355b6
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39214389"
---
# <a name="manage-classroom-labs-in-azure-lab-services"></a>Leslokaallabs in Azure Lab Services beheren 
In dit artikel wordt beschreven hoe u maken en configureren van een leslokaallab, alle leslokaallabs weergeven of een leslokaallab verwijderen.

## <a name="prerequisites"></a>Vereisten
Als u een leslokaallab in een labaccount instelt, moet u lid zijn van de rol **Labmaker** in het labaccount. Het account dat u gebruikt voor het maken van een lab-account wordt automatisch toegevoegd aan deze rol. Een lab-eigenaar kan andere gebruikers toevoegen aan de rol Labmaker met behulp van de stappen in het volgende artikel: [een gebruiker toevoegen aan de rol Labmaker](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).

## <a name="create-a-classroom-lab"></a>Een leslokaallab maken

1. Navigeer naar de [Azure Lab Services-website](https://labs.azure.com).
2. Selecteer **Aanmelden** en voer uw referenties in. Azure Lab Services biedt ondersteuning voor organisatie-accounts en Microsoft-accounts.
3. Voer in het venster **Nieuw lab** de volgende acties uit: 
    1. Geef een **naam** op voor het leslokaallab. 
    2. Selecteer de **grootte** van de virtuele machine die u wilt gebruiken in het leslokaal.
    3. Kies de **afbeelding**  die u wilt gebruiken om de virtuele machine te maken.
    4. Geef de **standaardreferenties** moet worden gebruikt voor aanmelding bij de virtuele machines in het lab.
    7. Selecteer **Opslaan**.

        ![Een leslokaallab maken](../media/how-to-manage-classroom-labs/new-lab-window.png)
1. U ziet het **dashboard** voor het lab. 
    
    ![Dashboard Leslokaallab](../media/how-to-manage-classroom-labs/classroom-lab-home-page.png)

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


### <a name="publish-the-template"></a>De sjabloon publiceren 
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
 
## <a name="manage-student-vms"></a>Studenten virtuele machines beheren
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

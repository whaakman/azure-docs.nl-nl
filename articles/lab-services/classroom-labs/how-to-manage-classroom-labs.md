---
title: Leslokaal labs in Azure Lab Services beheren | Microsoft Docs
description: Meer informatie over het maken en configureren van een testomgeving leslokaal, weergeven van alle de leslokaal labs gedeeld de registratie te koppelen met een lab-gebruiker of verwijderen van een testomgeving.
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
ms.openlocfilehash: f8cf5a46e1d1e3242fd46900b56f7d8cf15d3fc5
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37082519"
---
# <a name="manage-classroom-labs-in-azure-lab-services"></a>Leslokaal labs in Azure Lab Services beheren 
In dit artikel wordt beschreven hoe maken en configureren van een testomgeving leslokaal, alle leslokaal labs weergeven of verwijderen van een testomgeving leslokaal.

## <a name="prerequisites"></a>Vereisten
Als u een testomgeving leslokaal in een lab-account instelt, moet u lid zijn van de **Lab Maker** rol in het lab-account. De labeigenaar van een een gebruiker kunt toevoegen aan de rol van testlab met behulp van de stappen in het volgende artikel: [een gebruiker toevoegen aan de rol van Lab](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).

## <a name="create-a-classroom-lab"></a>Een leslokaallab maken

1. Navigeer naar de [Azure Lab Services-website](https://labs.azure.com).
2. Voer in het venster **Nieuw lab** de volgende acties uit: 
    1. Geef een **naam** op voor het leslokaallab. 
    2. Selecteer de **grootte** van de virtuele machine die u wilt gebruiken in het leslokaal.
    3. Kies de **afbeelding**  die u wilt gebruiken om de virtuele machine te maken.
    4. Geef de **standaardreferenties** moet worden gebruikt voor aanmelden bij de virtuele machines in de testomgeving.
    7. Selecteer **Opslaan**.

        ![Een leslokaallab maken](../media/how-to-manage-classroom-labs/new-lab-window.png)
1. U ziet de **dashboard** voor de testomgeving. 
    
    ![Leslokaal lab-dashboard](../media/how-to-manage-classroom-labs/classroom-lab-home-page.png)

## <a name="configure-usage-policy"></a>Gebruiksbeleid configureren

1. Selecteer **Gebruiksbeleid**. 
2. Voer in het **Gebruiksbeleid**, Instellingen, het **aantal gebruikers** in dat het lab mag gebruiken.
3. Selecteer **Opslaan**. 

    ![Gebruiksbeleid](../media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="set-up-the-template"></a>De sjabloon instellen
Een sjabloon in een lab is een basiskopie van de virtuele machine waarvan de virtuele machines van alle gebruikers worden gemaakt. Stel de sjabloon voor de virtuele machine zo in dat deze is geconfigureerd met precies wat u de labgebruikers wilt bieden. U kunt een naam en een beschrijving opgeven voor de sjabloon die de gebruikers van het lab kunnen zien. Publiceer de sjabloon om exemplaren van de VM-sjabloon beschikbaar stellen aan uw lab-gebruikers.  

### <a name="set-template-title-and-description"></a>Set Sjabloontitel en beschrijving
1. Selecteer in de sectie **Sjabloon** de optie **Bewerken** (potloodpictogram) voor de sjabloon. 
2. Voer in het venster **Gebruikersweergave** een **titel** in voor de sjabloon.
3. Voer een **Beschrijving** in voor de sjabloon.
4. Selecteer **Opslaan**.

    ![Beschrijving van leslokaallab](../media/how-to-manage-classroom-labs/lab-description.png)

### <a name="set-up-the-template-vm"></a>De VM-sjabloon instellen
 U verbinding maken met de VM-sjabloon en de vereiste software installeren op deze vooraleer het beschikbaar op uw studenten. 

1. Wacht totdat de sjabloon voor virtuele machine gereed is. Zodra u klaar bent, is de **Start** knop moet worden ingeschakeld. Selecteer voor het starten van de virtuele machine **Start**.

    ![Start de VM-sjabloon](../media/tutorial-setup-classroom-lab/start-template-vm.png)
1. Voor verbinding met de VM, selecteer **Connect**, en volg de instructies. 

    ![Verbinding maken met de VM-sjabloon](../media/tutorial-setup-classroom-lab/connect-template-vm.png)
1. Software die zijn voor studenten vereist te doen in het lab (bijvoorbeeld Visual Studio, Azure Storage Explorer, enzovoort) te installeren. 
2. Verbinding verbreken (Sluit uw extern bureaublad-sessiehost) uit de VM-sjabloon. 
3. **Stop** de VM-sjabloon door het selecteren van **stoppen**. 

    ![Stoppen van de VM-sjabloon](../media/tutorial-setup-classroom-lab/stop-template-vm.png)


### <a name="publish-the-template"></a>Publiceer de sjabloon 
Wanneer u een sjabloon publiceert, maakt Azure Lab Services virtuele machines in de testomgeving met behulp van de sjabloon. Het aantal virtuele machines dat in dit proces wordt gemaakt, is hetzelfde als het maximum aantal gebruikers dat in het lab is toegestaan. Dit kunt u instellen in het gebruiksbeleid van het lab. Alle virtuele machines hebben dezelfde configuratie als de sjabloon. 

1. Selecteer **publiceren** in de **sjabloon** sectie. 

    ![Publiceren van de VM-sjabloon](../media/tutorial-setup-classroom-lab/public-access.png)
1. In de **publiceren** Selecteer de **gepubliceerde** optie. 
2. Selecteer nu de **publiceren** knop. Dit proces duurt een bepaalde tijd, afhankelijk van hoeveel virtuele machines worden gemaakt, dit is hetzelfde als het aantal gebruikers dat is toegestaan in de testomgeving.
    
    > [!IMPORTANT]
    > Als een sjabloon is gepubliceerd, kan deze niet-gepubliceerde niet. 
4. Overschakelen naar de **virtuele machines** pagina en controleer of u virtuele machines die zich in **niet langer toegewezen** status. Deze virtuele machines worden nog niet toegewezen aan studenten. 

    ![Virtuele machines](../media/tutorial-setup-classroom-lab/virtual-machines.png)
5. Wacht totdat de virtuele machines worden gemaakt. Ze moeten **gestopt** status. U kunt een student VM starten, verbinding maken met de virtuele machine, stop de virtuele machine en verwijderen van de virtuele machine op deze pagina. U kunt ze worden gestart op deze pagina of kunt uw studenten starten van de virtuele machines. 

    ![Virtuele machines gestopt](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)


## <a name="send-registration-link-to-students"></a>Registratiekoppeling naar studenten verzenden

1. Overschakelen naar de **Dashboard** weergeven. 
2. Selecteer de tegel **Gebruikersregistratie**.

    ![Registratiekoppeling voor studenten](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. Selecteer in het dialoogvenster **Gebruikersregistratie** de knop **Kopiëren**. De koppeling wordt naar het klembord gekopieerd. Plak deze in een e-mailprogramma en verstuur een e-mail naar de student. 

    ![Registratiekoppeling voor studenten](../media/tutorial-setup-classroom-lab/registration-link.png)
2. Op de **gebruikersregistratie** dialoogvenster, **sluiten**. 

## <a name="view-all-classroom-labs"></a>Alle leslokaal labs weergeven
1. Navigeer naar [Lab Services van Azure-portal](https://labs.azure.com).
2. Controleer of u de labs in het geselecteerde lab-account. 

    ![Alle labs](../media/how-to-manage-classroom-labs/all-labs.png)
3. Gebruik de vervolgkeuzelijst boven aan een andere lab-account selecteren. U ziet labs in het geselecteerde lab-account. 

## <a name="delete-a-classroom-lab"></a>Een lab leslokaal verwijderen
1. Selecteer op de tegel voor de testomgeving drie puntjes (...) in de hoek. 

    ![Het lab selecteren](../media/how-to-manage-classroom-labs/select-three-dots.png)
2. Selecteer **Verwijderen**. 

    ![Knop verwijderen](../media/how-to-manage-classroom-labs/delete-button.png)
3. Op de **verwijderen lab** dialoogvenster, **verwijderen**. 

    ![In het dialoogvenster verwijderen](../media/how-to-manage-classroom-labs/delete-lab-dialog-box.png)
 
## <a name="manage-student-vms"></a>Studenten VM's beheren
Zodra de studenten registreren met Azure Lab Services met behulp van de registratie van de koppeling opgegeven, ziet u de virtuele machines aan studenten toegewezen op de **virtuele machines** tabblad. 

![Virtuele machines die zijn toegewezen aan studenten](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

U kunt de volgende taken op een student VM doen: 

- Een virtuele machine stoppen als de virtuele machine wordt uitgevoerd. 
- Start een virtuele machine als de virtuele machine is gestopt. 
- Maak verbinding met de VM. 
- Verwijder de virtuele machine. 

## <a name="next-steps"></a>Volgende stappen
Aan de slag met het installeren van een lab met Azure Lab Services:

- [Een leslokaallab instellen](how-to-manage-classroom-labs.md)
- [Een lab instellen](../tutorial-create-custom-lab.md)

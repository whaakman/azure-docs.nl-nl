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
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: 39683c89db57dbeefd190a51415c783d012785e0
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36303772"
---
# <a name="tutorial-set-up-a-classroom-lab"></a>Zelfstudie: Een leslokaallab instellen 
In deze zelfstudie stelt u een leslokaallab in met virtuele machines die worden gebruikt door studenten in het leslokaal.  

In deze zelfstudie voert u de volgende acties uit:

> [!div class="checklist"]
> * Een leslokaallab maken
> * Het leslokaallab configureren
> * Registratiekoppeling naar studenten verzenden

## <a name="prerequisites"></a>Vereisten
Als u een leslokaallab in een labaccount instelt, moet u lid zijn van de rol **Labmaker** in het labaccount. De labeigenaar kan een gebruiker toevoegen aan de rol Labmaker met behulp van de stappen in het volgende artikel: [Een gebruiker toevoegen aan de rol Labmaker](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).


## <a name="create-a-classroom-lab"></a>Een leslokaallab maken

1. Navigeer naar de [Azure Lab Services-website](https://labs.azure.com).
2. Selecteer **Aanmelden** en voer uw referenties in. 
3. Voer in het venster **Nieuw lab** de volgende acties uit: 
    1. Geef een **naam** op voor het leslokaallab. 
    2. Selecteer de **grootte** van de virtuele machine die u wilt gebruiken in het leslokaal.
    3. Kies de **afbeelding**  die u wilt gebruiken om de virtuele machine te maken.
    4. Geef de **standaardgegevens** op voor het aanmelden bij de virtuele machines in het lab. 
    7. Selecteer **Opslaan**.

        ![Een leslokaallab maken](../media/tutorial-setup-classroom-lab/new-lab-window.png)
1. U ziet het **dashboard** voor het lab. 
    
    ![Dashboard Leslokaallab](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)

## <a name="configure-usage-policy"></a>Gebruiksbeleid configureren

1. Selecteer **Gebruiksbeleid**. 
2. Voer in het **Gebruiksbeleid**, Instellingen, het **aantal gebruikers** in dat het lab mag gebruiken.
3. Selecteer **Opslaan**. 

    ![Gebruiksbeleid](../media/tutorial-setup-classroom-lab/usage-policy-settings.png)


## <a name="set-up-the-template"></a>De sjabloon instellen 
Een sjabloon in een lab is een basiskopie van de virtuele machine waarvan de virtuele machines van alle gebruikers worden gemaakt. Stel de sjabloon voor de virtuele machine zo in dat deze is geconfigureerd met precies wat u de labgebruikers wilt bieden. U kunt een naam en een beschrijving opgeven voor de sjabloon die de gebruikers van het lab kunnen zien. Publiceer de sjabloon (maak deze openbaar) om exemplaren van de sjabloon-VM voor de gebruikers van uw lab beschikbaar te maken. 

### <a name="set-title-and-description"></a>Titel en beschrijving instellen
1. Selecteer in de sectie **Sjabloon** de optie **Bewerken** (potloodpictogram) voor de sjabloon. 
2. Voer in het venster **Gebruikersweergave** een **titel** in voor de sjabloon.
3. Voer een **Beschrijving** in voor de sjabloon.
4. Selecteer **Opslaan**.

    ![Beschrijving van leslokaallab](../media/tutorial-setup-classroom-lab/lab-description.png)

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
    > Wanneer een sjabloon openbaar beschikbaar is, kan de toegang kan niet worden gewijzigd naar privé. 
4. Ga naar de pagina **Virtuele machines** en controleer of u vijf virtuele machines ziet met de status **Niet-toegewezen**. Deze virtuele machines zijn nog niet toegewezen aan studenten. 

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


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u een leslokaallab gemaakt en het lab geconfigureerd. Ga voor meer informatie over hoe een student toegang kan krijgen tot een virtuele machine in het lab met behulp van de registratiekoppeling naar de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Verbinding maken met een virtuele machine in het leslokaallab](tutorial-connect-virtual-machine-classroom-lab.md)


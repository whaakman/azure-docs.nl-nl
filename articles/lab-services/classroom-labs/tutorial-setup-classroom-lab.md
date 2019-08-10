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
ms.date: 06/11/2019
ms.author: spelluru
ms.openlocfilehash: 964ecca015e440439885bbbd85cb720a3abd10a9
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68883517"
---
# <a name="tutorial-set-up-a-classroom-lab"></a>Zelfstudie: Een leslokaallab instellen 
In deze zelfstudie stelt u een leslokaallab in met virtuele machines die worden gebruikt door studenten in het leslokaal.  

In deze zelfstudie voert u de volgende acties uit:

> [!div class="checklist"]
> * Een leslokaallab maken
> * Gebruikers toevoegen aan het lab
> * Registratiekoppeling naar studenten verzenden

## <a name="prerequisites"></a>Vereisten
Als u een leslokaal Lab in een Lab-account wilt instellen, moet u lid zijn van een van deze rollen in het lab-account: Eigenaar, Labmaker of Inzender. Het account dat u hebt gebruikt voor het maken van een labaccount wordt automatisch toegevoegd aan de eigenaarsrol.

Een labeigenaar kan andere gebruikers toevoegen aan de rol **Labmaker**. Zo kan een labeigenaar bijvoorbeeld professoren toevoegen aan de rol Labmaker. Vervolgens maken de professoren labs met VM’s voor hun lessen. Studenten gebruiken de registratiekoppeling die ze ontvangen van professoren om zich te registreren bij het lab. Wanneer ze zijn geregistreerd, kunnen ze VM’s in de labs gebruiken voor het werk in de les en voor hun huiswerk. Zie voor gedetailleerde stappen voor het toevoegen van gebruikers aan de rol Labmaker [Een gebruiker toevoegen aan de rol Labmaker](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).


## <a name="create-a-classroom-lab"></a>Een leslokaallab maken

1. Navigeer naar de [Azure Lab Services-website](https://labs.azure.com). Houd er rekening mee dat Internet Explorer 11 nog niet wordt ondersteund. 
2. Selecteer **Aanmelden** en voer uw referenties in. Azure Lab Services ondersteunt organisatieaccounts en Microsoft-accounts. 
3. Voer in het venster **Nieuw lab** de volgende acties uit: 
    1. Geef een **naam** voor uw lab op. 
    2. Geef het maximale **aantal virtuele machines** in het lab op. U kunt het aantal Vm's verhogen of verlagen nadat u het lab of een bestaand Lab hebt gemaakt. Zie voor meer informatie [Aantal virtuele machines in een testomgeving bijwerken](how-to-configure-student-usage.md#update-number-of-virtual-machines-in-lab)
    6. Selecteer **Opslaan**.

        ![Een leslokaallab maken](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. Voer op de pagina **Specificaties van virtuele machines selecteren** de volgende stappen uit:
    1. Selecteer een **grootte** voor virtuele machines (VM's) die in het lab worden gemaakt. Momenteel zijn **kleine**, **gemiddelde**, middel grote **(virtualisatie)** , **grote**en **GPU** -grootten toegestaan.
    3. Selecteer de **VM-installatiekopie** die moet worden gebruikt voor het maken van virtuele machines in het lab. Als u een Linux-installatie kopie selecteert, ziet u een optie om verbinding met extern bureau blad in te scha kelen. Zie [verbinding met extern bureau blad inschakelen voor Linux](how-to-enable-remote-desktop-linux.md)voor meer informatie.
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
8. Voer de volgende stappen uit op de pagina **Sjabloon configureren**: Deze stappen zijn **optioneel** voor de zelfstudie.
    2. De sjabloon-VM verbinden door **Verbinding maken** te selecteren. Als het een Linux-sjabloon-VM is, kiest u of u verbinding wilt maken via SSH of RDP (als RDP is ingeschakeld).
    1. Selecteer **wacht woord opnieuw instellen** om het wacht woord voor de virtuele machine opnieuw in te stellen. 
    1. Software installeren en configureren op uw sjabloon-VM. 
    1. **Stop** de virtuele machine.  
    1. Een **Beschrijving** voor de sjabloon invoeren
9. Selecteer **Volgende** op de sjabloonpagina. 
10. Voer op de pagina **De sjabloon publiceren** de volgende acties uit. 
    1. Selecteer **publiceren**om de sjabloon direct te publiceren.  

        > [!WARNING]
        > Zodra u de sjabloon hebt gepubliceerd, kan dit niet ongedaan worden gemaakt. 
    2. Als u later wilt publiceren, selecteert u **Opslaan voor later**. U kunt de sjabloon-VM publiceren nadat de wizard is voltooid. Zie voor meer informatie over het configureren en publiceren nadat de wizard is voltooid [de sectie sjabloon publiceren](how-to-create-manage-template.md#publish-the-template-vm) in het artikel [klassikale Labs beheren](how-to-manage-classroom-labs.md) .

        ![Sjabloon publiceren](../media/tutorial-setup-classroom-lab/publish-template.png)
11. U kunt de **voortgang van het publiceren** van de sjabloon bekijken. Dit proces duurt maximaal een uur. 

    ![Sjabloon publiceren - voortgang](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. De volgende pagina wordt weergegeven wanneer de sjabloon is gepubliceerd. Selecteer **Done**.

    ![Sjabloon publiceren - gelukt](../media/tutorial-setup-classroom-lab/publish-success.png)
1. U ziet het **dashboard** voor het lab. 
    
    ![Dashboard Leslokaallab](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)
4. Schakel over naar de pagina **Virtuele machines** door Virtuele machines te selecteren in het menu links of door de tegel Virtuele machines te selecteren. Controleer of u virtuele machines ziet met de status **Niet-toegewezen**. Deze virtuele machines zijn nog niet toegewezen aan studenten. Deze horen de status **Gestopt** te hebben. Op deze pagina kunt u een student-VM starten, verbinding maken met de virtuele machine, de virtuele machine stoppen en de virtuele machine verwijderen. U kunt de virtuele machines zelf starten vanaf deze pagina of ze laten starten door de studenten. 

    ![Virtuele machines met de status Gestopt](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)

## <a name="add-users-to-the-lab"></a>Gebruikers toevoegen aan het lab

1. Selecteer **Gebruikers** in het menu links. De optie **Toegang beperken** is standaard ingeschakeld. Wanneer deze instelling is ingeschakeld, kan een gebruiker zich niet registreren bij het lab, zelfs niet als deze de registratiekoppeling heeft, tenzij de gebruiker in de lijst met gebruikers staat. Alleen gebruikers in de lijst kunnen zich registreren bij het lab door de registratiekoppeling te gebruiken die u verzendt. In deze procedure kunt u gebruikers toevoegen aan de lijst. U kunt ook **Toegang beperken** uitschakelen, zodat gebruikers zich bij het lab kunnen registreren zolang ze de registratiekoppeling hebben. 
2. Selecteer **Gebruikers toevoegen** op de werkbalk. 

    ![Knop Gebruikers toevoegen](../media/how-to-configure-student-usage/add-users-button.png)
1. Op de pagina **Gebruikers toevoegen** voert u e-mailadressen van gebruikers in op afzonderlijke regels of op één regel gescheiden door puntkomma's. 

    ![E-mailadressen van gebruikers toevoegen](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. Selecteer **Opslaan**. U ziet de e-mailadressen van gebruikers en hun status (al dan niet geregistreerd) in de lijst. 

    ![Lijst met gebruikers](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="set-quotas-for-users"></a>Quota instellen voor gebruikers
U kunt quota per gebruiker instellen met behulp van de volgende stappen: 

1. Selecteer **gebruikers** in het menu links als de pagina nog niet actief is. 
2. Quotum **per gebruiker selecteren: 10 uur** op de werk balk. 
3. Geef op de pagina **quotum per gebruiker** het aantal uur op dat u aan elke gebruiker (student) wilt geven: 
    1. **Totaal aantal Lab-uren per gebruiker**. Gebruikers kunnen hun Vm's gebruiken voor het ingestelde aantal uren (opgegeven voor dit veld) **naast de geplande tijd**. Als u deze optie selecteert, voert u het **aantal uren** in het tekstvak in. 

        ![Aantal uren per gebruiker](../media/how-to-configure-student-usage/number-of-hours-per-user.png). 
    1. **0 uur (alleen schema)** . Gebruikers kunnen hun Vm's alleen tijdens de geplande tijd gebruiken of wanneer de eigenaar van het lab de Vm's voor de virtuele machines inschakelt.

        ![Geen enkel uur, alleen geplande tijd](../media/how-to-configure-student-usage/zero-hours.png)
    4. Selecteer **Opslaan**. 
5. U ziet nu de gewijzigde waarden op de werk balk: **Quotum per gebruiker: &lt;aantal uren&gt;** . 

    ![Quotum per gebruiker](../media/how-to-configure-student-usage/quota-per-user.png)

## <a name="set-a-schedule-for-the-lab"></a>Een planning voor het lab instellen
Als u de quotum instelling hebt geconfigureerd op **0 uur (alleen plannen)** , moet u een planning voor het lab instellen. In deze zelf studie stelt u de planning in op een terugkerende wekelijkse planning.

1. Ga naar de pagina **schema's** en selecteer **planning toevoegen** op de werk balk. 

    ![Knop schema toevoegen op de pagina planningen](../media/how-to-create-schedules/add-schedule-button.png)
2. Schakel op de pagina **schema toevoegen** de optie **wekelijks** aan de bovenkant in. 
3. Selecteer voor **plannings dagen (vereist)** de dagen waarop de planning van kracht moet worden. In het volgende voor beeld wordt maandag t/m vrijdag geselecteerd. 
4. Voer in het veld van de **Start datum** **van** de planning in of kies een datum door de knop **kalender** te selecteren. Dit veld is vereist. 
5. Voor de **eind datum**van de planning voert u een eind datum in waarop de vm's moeten worden afgesloten. 
6. Selecteer bij **begin tijd**het tijdstip waarop u wilt dat de vm's worden gestart. De begin tijd is vereist als de eind tijd niet is ingesteld. Selecteer **begin gebeurtenis verwijderen** als u alleen de eind tijd wilt opgeven. Als de **begin tijd** is uitgeschakeld, selecteert u **Start gebeurtenis toevoegen** naast de vervolg keuzelijst om deze optie in te scha kelen. 
7. Selecteer bij **eind tijd**het tijdstip waarop u wilt dat de virtuele machines worden afgesloten. De stop tijd is vereist als de begin tijd niet is ingesteld. Selecteer **Stop gebeurtenis verwijderen** als u alleen de begin tijd wilt opgeven. Als de **Stop tijd** is uitgeschakeld, selecteert u **Stop gebeurtenis toevoegen** naast de vervolg keuzelijst om deze optie in te scha kelen.
8. Selecteer voor **tijd zone (vereist)** de tijd zone voor de start-en stop tijden die u hebt opgegeven.  
9. Voor **notities**voert u een beschrijving of notities in voor de planning. 
10. Selecteer **Opslaan**. 

    ![Wekelijks schema](../media/how-to-create-schedules/add-schedule-page-weekly.png)

## <a name="send-an-email-with-the-registration-link"></a>Een e-mail verzenden met de registratie koppeling

1. Schakel over naar de weergave **Gebruikers** als u nog niet op die pagina bent. 
2. Selecteer specifieke of alle gebruikers in de lijst. Als u specifieke gebruikers wilt selecteren, selecteert u de selectie vakjes in de eerste kolom van de lijst. Als u alle gebruikers wilt selecteren, schakelt u het selectie vakje voor de titel van de eerste kolom (**naam**) in of selecteert u alle selectie vakjes voor alle gebruikers in de lijst. U kunt de status van de uitnodigings **status** bekijken in deze lijst.  In de volgende afbeelding is de uitnodigings status voor alle studenten ingesteld op **uitnodiging niet verzonden**. 

    ![Studenten selecteren](../media/tutorial-setup-classroom-lab/select-students.png)
1. Selecteer het **e-mail pictogram (envelop)** in een van de rijen (of) Selecteer **uitnodiging verzenden** op de werk balk. U kunt ook de muis aanwijzer op een student naam in de lijst klikken om het pictogram e-mail weer te geven. 

    ![Registratie koppeling via e-mail verzenden](../media/tutorial-setup-classroom-lab/send-email.png)
4. Voer op de pagina **registratie koppeling via E-mail verzenden** de volgende stappen uit: 
    1. Typ een **optioneel bericht** dat u naar de studenten wilt verzenden. Het e-mail bericht bevat automatisch de registratie koppeling. 
    2. Selecteer **verzenden**op de pagina **registratie koppeling per e-mail verzenden** . U ziet de status van de uitnodiging voor het verzenden van de **uitnodiging** en vervolgens naar de **uitnodiging verzonden**. 
        
        ![Verzonden uitnodigingen](../media/tutorial-setup-classroom-lab/invitations-sent.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u een leslokaallab gemaakt en het lab geconfigureerd. Ga voor meer informatie over hoe een student toegang kan krijgen tot een virtuele machine in het lab met behulp van de registratiekoppeling naar de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Verbinding maken met een virtuele machine in het leslokaallab](tutorial-connect-virtual-machine-classroom-lab.md)


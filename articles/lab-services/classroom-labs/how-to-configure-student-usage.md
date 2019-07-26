---
title: Gebruiks instellingen configureren in de klassikale Labs van Azure Lab Services | Microsoft Docs
description: Meer informatie over het configureren van het aantal gebruikers voor het lab, het ontvangen van de items die zijn geregistreerd bij het lab, het aantal uren bepalen dat ze de virtuele machine mogen gebruiken en nog veel meer.
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
ms.date: 06/11/2019
ms.author: spelluru
ms.openlocfilehash: 86f22864c416ad2a90bea09c02675d6eb3322308
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68385614"
---
# <a name="configure-usage-settings-and-policies"></a>Instellingen en beleids regels voor het gebruik configureren
In dit artikel wordt beschreven hoe u gebruikers toevoegt aan het lab, ze aan het lab kunt toevoegen, het aantal uren bepaalt dat ze de virtuele machine kunnen gebruiken, en nog veel meer. 


## <a name="add-users-to-the-lab"></a>Gebruikers toevoegen aan het lab
Als u de **toegang beperken** hebt ingeschakeld, voegt u gebruikers (e-mail adressen) toe aan de lijst.

1. Selecteer **Gebruikers** in het menu links.
2. Selecteer **Gebruikers toevoegen** op de werkbalk. 

    ![Knop Gebruikers toevoegen](../media/how-to-configure-student-usage/add-users-button.png)
1. Op de pagina **Gebruikers toevoegen** voert u e-mailadressen van gebruikers in op afzonderlijke regels of op één regel gescheiden door puntkomma's. 

    ![E-mailadressen van gebruikers toevoegen](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. Selecteer **Opslaan**. U ziet de e-mailadressen van gebruikers en hun status (al dan niet geregistreerd) in de lijst. 

    ![Lijst met gebruikers](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="share-registration-link-with-students"></a>Registratie koppeling met studenten delen
Als u de registratie koppeling naar studenten wilt verzenden, gebruikt u een van de volgende methoden. De eerste methode laat zien hoe u e-mail berichten naar studenten verzendt met de registratie koppeling en een optioneel bericht. In de tweede methode ziet u hoe u de registratie koppeling kunt ophalen die u op een wille keurige manier met anderen wilt delen. 

Als de **toegang beperken** voor het lab is ingeschakeld, kunnen alleen gebruikers in de lijst met gebruikers de registratie koppeling gebruiken om zich bij het lab te registreren. Deze optie is standaard ingeschakeld. 

### <a name="send-email-to-users"></a>E-mail verzenden naar gebruikers
Met Azure Lab Services kunnen docenten de uitnodigingen van Lab-berichten verzenden naar alle of geselecteerde studenten zonder dat ze een andere e-mailclient hoeven te gebruiken. Docenten kunnen de afzonderlijke studenten in de lijst aanwijzen om het e-mail pictogram voor elke student te zien of een of meer studenten te selecteren en een **uitnodiging verzenden** te gebruiken op de werk balk. Deze functie verzendt een e-mail met een registratie koppeling en een bericht (indien aanwezig) dat door de docent is toegevoegd. Nadat de uitnodiging is verzonden, wordt de uitnodigings status gewijzigd in de **uitnodiging** , zodat docenten kunnen bijhouden welke studenten de registratie koppeling al hebben ontvangen en op welke datum deze is verzonden.

1. Schakel over naar de weergave **Gebruikers** als u nog niet op die pagina bent. 
2. Selecteer specifieke of alle gebruikers in de lijst. Als u specifieke gebruikers wilt selecteren, selecteert u de selectie vakjes in de eerste kolom van de lijst. Als u alle gebruikers wilt selecteren, schakelt u het selectie vakje voor de titel van de eerste kolom (**naam**) in of selecteert u alle selectie vakjes voor alle gebruikers in de lijst. U kunt de status van de uitnodigings **status** bekijken in deze lijst.  In de volgende afbeelding is de uitnodigings status voor alle studenten ingesteld op **uitnodiging niet verzonden**. 

    ![Studenten selecteren](../media/tutorial-setup-classroom-lab/select-students.png)
1. Selecteer het **e-mail pictogram (envelop)** in een van de rijen (of) Selecteer **uitnodiging verzenden** op de werk balk. U kunt ook de muis aanwijzer op een student naam in de lijst klikken om het pictogram e-mail weer te geven. 

    ![Registratie koppeling via e-mail verzenden](../media/tutorial-setup-classroom-lab/send-email.png)
4. Voer op de pagina **registratie koppeling via E-mail verzenden** de volgende stappen uit: 
    1. Typ een **optioneel bericht** dat u naar de studenten wilt verzenden. Het e-mail bericht bevat automatisch de registratie koppeling. 
    2. Selecteer **verzenden**op de pagina **registratie koppeling per e-mail verzenden** . U ziet de status van de uitnodiging voor het verzenden van de **uitnodiging** en vervolgens naar de **uitnodiging verzonden**. 
        
        ![Verzonden uitnodigingen](../media/tutorial-setup-classroom-lab/invitations-sent.png)

## <a name="get-registration-link"></a>Registratie koppeling ophalen
1. Schakel over naar de weer gave **gebruikers** door **gebruikers** te selecteren in het menu links. 
2. Selecteer de tegel **Registratiekoppeling ophalen**.

    ![Registratiekoppeling voor studenten](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. Selecteer in het dialoogvenster **Gebruikersregistratie** de knop **Kopiëren**. De koppeling wordt naar het klembord gekopieerd. Plak deze in een e-mailprogramma en verstuur een e-mail naar de student. 

    ![Registratiekoppeling voor studenten](../media/tutorial-setup-classroom-lab/registration-link.png)
2. Selecteer in het dialoogvenster **Gebruikersregistratie** de optie **Sluiten**. 
4. Deel de **registratie koppeling** met een student zodat de student zich voor de klasse kan registreren. 

## <a name="view-users-registered-with-the-lab"></a>Gebruikers weergeven die zijn geregistreerd bij het lab

Selecteer **gebruikers** in het linkermenu om de lijst met gebruikers weer te geven die zijn geregistreerd bij het lab. 

![Lijst met gebruikers die zijn geregistreerd bij het lab](../media/how-to-configure-student-usage/users-list-new.png)

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



> [!IMPORTANT]
> Voordat u de registratie koppeling naar de studenten verzendt, moeten docenten het schema voor de klasse instellen als ze 0 quotum uren kiezen of de quotum uren voor het lab opgeven.
>
> De [geplande uitvoerings tijd van vm's](how-to-create-schedules.md) telt niet op het quotum dat aan een gebruiker is toegewezen. Het quotum geldt voor de tijd buiten de plannings uren die een student op Vm's doorbrengt. 

### <a name="add-users-by-uploading-a-csv-file"></a>Gebruikers toevoegen door een CSV-bestand te uploaden
U kunt ook gebruikers toevoegen door een CSV-bestand te uploaden met e-mail adressen van gebruikers.

1. Maak een CSV-bestand met e-mail adressen van gebruikers in één kolom.

    ![Quotum per gebruiker](../media/how-to-configure-student-usage/csv-file-with-users.png)
2. Selecteer op de pagina **gebruikers** van het lab de optie **CSV uploaden** op de werk balk.

    ![CSV-knop uploaden](../media/how-to-configure-student-usage/upload-csv-button.png)
3. Selecteer het CSV-bestand met gebruikers-e-mail adressen. Wanneer u **openen** selecteert nadat u het CSV-bestand hebt geselecteerd, ziet u het volgende **gebruikers venster toevoegen** . De e-mailadres lijst is gevuld met e-mail adressen uit het CSV-bestand. 

    ![Venster gebruikers toevoegen ingevuld met e-mail adressen uit CSV-bestand](../media/how-to-configure-student-usage/add-users-window.png)
4. Selecteer **Opslaan** in het venster **gebruikers toevoegen** . 
5. Bevestig dat u gebruikers in de lijst met gebruikers ziet. 

    ![Lijst met toegevoegde gebruikers](../media/how-to-configure-student-usage/list-of-added-users.png)

## <a name="manage-user-vms"></a>Gebruikers-Vm's beheren
Zodra studenten zich bij Azure Lab Services registreren met behulp van de registratie koppeling die u aan hen hebt gegeven, ziet u de Vm's die aan studenten zijn toegewezen op het tabblad **virtuele machines** . 

![Virtuele machines die aan studenten zijn toegewezen](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

U kunt de volgende taken uitvoeren op een student-VM: 

- Een virtuele machine stoppen als de virtuele machine wordt uitgevoerd. 
- Start een virtuele machine als de virtuele machine is gestopt. 
- Maak verbinding met de VM. 
- Verwijder de virtuele machine. 
- Het aantal uren weer geven dat gebruikers de virtuele machine hebben gebruikt. 

## <a name="update-number-of-virtual-machines-in-lab"></a>Het aantal virtuele machines in het lab bijwerken
Als u het aantal virtuele machines in de test omgeving wilt bijwerken, voert u de volgende stappen uit op de pagina **virtual machines** :

1. Selecteer **virtuele machines** in het menu links. 
2. Selecteer **lab-capaciteit &lt;:&gt; aantal machine (s)** op de werk balk. 
3. Voer het **aantal** virtuele machines in.
4. Selecteer **Opslaan**.

    ![Virtuele machines in het lab](../media/how-to-configure-student-usage/number-virtual-machines.png)


## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

- [Labaccounts maken en beheren als beheerder](how-to-manage-lab-accounts.md)
- [Labs maken en beheren als labeigenaar](how-to-manage-classroom-labs.md)
- [Sjablonen instellen en publiceren als labeigenaar](how-to-create-manage-template.md)
- [Als test gebruiker toegang krijgen tot klassikale Labs](how-to-use-classroom-lab.md)

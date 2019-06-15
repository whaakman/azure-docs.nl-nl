---
title: Configureren van instellingen voor het gebruik in leslokaallabs van Azure Lab-Services | Microsoft Docs
description: Informatie over het configureren van het aantal gebruikers voor de testomgeving, ze geregistreerd bij de testomgeving, bepalen het aantal uren dat de virtuele machine, en meer kan worden gebruikt.
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
ms.openlocfilehash: 67faf268d265fd045c21b75b6f64840511a371d3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67067330"
---
# <a name="configure-usage-settings-and-policies"></a>Instellingen voor het gebruik en het beleid configureren
In dit artikel wordt beschreven hoe u gebruikers toevoegen aan het lab, ze geregistreerd bij de testomgeving, bepalen het aantal uren dat de virtuele machine, en meer kan worden gebruikt. 


## <a name="add-users-to-the-lab"></a>Gebruikers toevoegen aan het lab
Als u hebt de **toegang beperken** ingeschakeld, gebruikers (e-mailadressen) toevoegen aan de lijst.

1. Selecteer **Gebruikers** in het menu links.
2. Selecteer **Gebruikers toevoegen** op de werkbalk. 

    ![Knop Gebruikers toevoegen](../media/how-to-configure-student-usage/add-users-button.png)
1. Op de pagina **Gebruikers toevoegen** voert u e-mailadressen van gebruikers in op afzonderlijke regels of op één regel gescheiden door puntkomma's. 

    ![E-mailadressen van gebruikers toevoegen](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. Selecteer **Opslaan**. U ziet de e-mailadressen van gebruikers en hun status (al dan niet geregistreerd) in de lijst. 

    ![Lijst met gebruikers](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="share-registration-link-with-students"></a>Registratie met koppeling delen met studenten
Voor het verzenden van de registratiekoppeling voor studenten, een van de volgende methoden te gebruiken. De eerste methode laat zien hoe u e-mailberichten voor studenten met de registratiekoppeling en een optioneel bericht verzenden. De tweede methode laat zien hoe de registratiekoppeling die kunt u elke gewenste manier kunt u met anderen delen. 

Als de **toegang beperken** is ingeschakeld voor de testomgeving, alleen gebruikers in de lijst met gebruikers de registratiekoppeling gebruiken kunnen om u te registreren met het lab. Deze optie is standaard ingeschakeld. 

### <a name="send-email-to-users"></a>E-mailbericht verzenden naar gebruikers
Azure Lab-Services kunt docenten e-lab uitnodigingen voor alle of geselecteerde studenten zonder gebruik te maken van een ander e-client. Docenten kunnen Beweeg de muisaanwijzer op afzonderlijke studenten in de lijst wilt weergeven van het e-mailpictogram voor elke student of Selecteer een of meer studenten en gebruik **uitnodiging verzenden** op de werkbalk. Deze functie verzendt een e-mailbericht met een registratiekoppeling en een bericht (indien aanwezig) die zijn toegevoegd door de docent. Als de uitnodiging is verzonden, wordt de status van de uitnodiging verandert in **uitnodiging verzonden** zodat docenten kunnen bijhouden van welke studenten al hebt ontvangen de registratiekoppeling en de datum waarop het is verzonden.

1. Schakel over naar de weergave **Gebruikers** als u nog niet op die pagina bent. 
2. Selecteer specifieke of alle gebruikers in de lijst. Selecteer specifieke gebruikers, schakel selectievakjes in de eerste kolom van de lijst. Selecteer alle gebruikers, schakel het selectievakje voor de titel van de eerste kolom (**naam**) of selecteert u alle selectievakjes uit voor alle gebruikers in de lijst. U ziet de status van de **uitnodiging status** in deze lijst.  In de volgende afbeelding, de status van de uitnodiging voor alle studenten is ingesteld op **uitnodiging niet verzonden**. 

    ![Studenten selecteren](../media/tutorial-setup-classroom-lab/select-students.png)
1. Selecteer de **e-mailpictogram (envelop)** in een van de rijen (of) select **uitnodiging verzenden** op de werkbalk. U kunt ook de muisaanwijzer boven de Studentnaam van een in de lijst om te zien van het e-mailpictogram houdt. 

    ![Registratiekoppeling per e-mail verzenden](../media/tutorial-setup-classroom-lab/send-email.png)
4. Op de **registratiekoppeling verzenden via e-mail** pagina, als volgt te werk: 
    1. Typ een **optioneel bericht** die u wilt verzenden naar de studenten. Het e-mailbericht omvat automatisch de registratiekoppeling. 
    2. Op de **registratiekoppeling verzenden via e-mail** weergeeft, schakelt **verzenden**. U ziet de status van de uitnodiging te wijzigen in **uitnodiging verzenden** en van daaruit naar **uitnodiging verzonden**. 
        
        ![Uitnodiging verzonden](../media/tutorial-setup-classroom-lab/invitations-sent.png)

## <a name="get-registration-link"></a>Registratiekoppeling ophalen
1. Schakel over naar de **gebruikers** weergeven door te selecteren **gebruikers** in het menu links. 
2. Selecteer de tegel **Registratiekoppeling ophalen**.

    ![Registratiekoppeling voor studenten](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. Selecteer in het dialoogvenster **Gebruikersregistratie** de knop **Kopiëren**. De koppeling wordt naar het klembord gekopieerd. Plak deze in een e-mailprogramma en verstuur een e-mail naar de student. 

    ![Registratiekoppeling voor studenten](../media/tutorial-setup-classroom-lab/registration-link.png)
2. Selecteer in het dialoogvenster **Gebruikersregistratie** de optie **Sluiten**. 
4. Share de **registratiekoppeling** met een student zodat de student voor de klasse registreren kan. 

## <a name="view-users-registered-with-the-lab"></a>Gebruikers weergeven die zijn geregistreerd bij het lab

Selecteer **gebruikers** geregistreerd in het menu links om te zien van de lijst met gebruikers met de testomgeving. 

![Lijst met gebruikers die zijn geregistreerd bij de testomgeving](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="set-quotas-per-user"></a>Quota instellen per gebruiker
U kunt quota's per gebruiker instellen met behulp van de volgende stappen uit: 

1. Selecteer **Gebruikers** in het menu links.
2. Selecteer **quotum per gebruiker:** op de werkbalk. 
3. Op de **quotum per gebruiker** pagina, geef het aantal uren dat u wilt verlenen aan elke gebruiker (studenten): 
    1. **0 uur (alleen schedule)** . Alleen tijdens het geplande tijdstip, of wanneer u als de eigenaar van het lab voor deze virtuele machines inschakelt, kunnen gebruikers hun VM's gebruiken.

        ![Nul uur - alleen geplande tijd](../media/how-to-configure-student-usage/zero-hours.png)
    1. **Totale aantal uren per gebruiker lab**. Gebruikers kunnen hun VM's gebruiken voor het aantal uren (opgegeven voor dit veld) **naast het geplande tijdstip**. Als u deze optie selecteert, voert u de **aantal uren** in het tekstvak in. 

        ![Aantal uren per gebruiker](../media/how-to-configure-student-usage/number-of-hours-per-user.png)
    4. Selecteer **Opslaan**. 
5. Ziet u de gewijzigde waarden op de werkbalk nu: **Quotum per gebruiker: &lt;aantal uren&gt;** . 

    ![Quotum per gebruiker](../media/how-to-configure-student-usage/quota-per-user.png)



> [!IMPORTANT]
> Voordat u de registratiekoppeling wilt verzenden naar de studenten, docenten moeten een instellen van de planning voor de klasse als ze Kies 0 quotum uur of geef de uren quotum voor de testomgeving.
>
> De [geplande uitvoeringstijd van virtuele machines](how-to-create-schedules.md) worden niet meegeteld in het quotum dat is toegewezen aan een gebruiker. Het quotum is voor de tijd buiten schema voor uren dat een student op virtuele machines spendeert. 

### <a name="add-users-by-uploading-a-csv-file"></a>Gebruikers toevoegen door een CSV-bestand te uploaden
U kunt ook gebruikers toevoegen door het uploaden van een CSV-bestand met e-mailadressen van gebruikers.

1. Maak een CSV-bestand met e-mailadressen van gebruikers in één kolom.

    ![Quotum per gebruiker](../media/how-to-configure-student-usage/csv-file-with-users.png)
2. Op de **gebruikers** pagina van het testlab, selecteer **uploaden CSV** op de werkbalk.

    ![CSV-knop uploaden](../media/how-to-configure-student-usage/upload-csv-button.png)
3. Selecteer het CSV-bestand met e-mailadressen. Wanneer u selecteert **Open** na het selecteren van het CSV-bestand, ziet u het volgende **gebruikers toevoegen** venster. De e-adreslijst wordt gevuld met e-mailadressen van het CSV-bestand. 

    ![Gebruikers venster is gevuld met e-mailadressen uit CSV-bestand toevoegen](../media/how-to-configure-student-usage/add-users-window.png)
4. Selecteer **opslaan** in de **gebruikers toevoegen** venster. 
5. Bevestig dat u gebruikers in de lijst met gebruikers. 

    ![Lijst met toegevoegde gebruikers](../media/how-to-configure-student-usage/list-of-added-users.png)

## <a name="manage-user-vms"></a>Gebruiker virtuele machines beheren
Wanneer studenten registreren met Azure Lab Services met behulp van de registratie van de koppeling opgegeven, ziet u de virtuele machines toegewezen aan studenten op de **virtuele machines** tabblad. 

![Virtuele machines die zijn toegewezen aan studenten](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

U kunt de volgende taken op een student VM kunt doen: 

- Een virtuele machine stoppen als de virtuele machine wordt uitgevoerd. 
- Start een virtuele machine als de virtuele machine is gestopt. 
- Maak verbinding met de VM. 
- De virtuele machine verwijderen. 
- Bekijk het aantal uren dat gebruikers de virtuele machine gebruikt. 

## <a name="update-number-of-virtual-machines-in-lab"></a>Aantal virtuele machines in het lab bijwerken
Voor het bijwerken van het aantal virtuele machines in het lab, voert u de volgende stappen uit de **virtuele Machines** pagina:

1. Selecteer **virtuele machines** in het menu links. 
2. Selecteer **Lab capaciteit: &lt;getal&gt; computer (s)** op de werkbalk. 
3. Voer de **getal** van virtuele machines.
4. Selecteer **Opslaan**.

    ![Virtuele machines in het lab](../media/how-to-configure-student-usage/number-virtual-machines.png)


## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

- [Labaccounts maken en beheren als beheerder](how-to-manage-lab-accounts.md)
- [Labs maken en beheren als labeigenaar](how-to-manage-classroom-labs.md)
- [Sjablonen instellen en publiceren als labeigenaar](how-to-create-manage-template.md)
- [Als een lab-gebruiker toegang krijgen tot leslokaallabs](how-to-use-classroom-lab.md)

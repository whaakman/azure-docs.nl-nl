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
ms.date: 11/14/2018
ms.author: spelluru
ms.openlocfilehash: 30c033b487fe58d017080b02c257502f82338164
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51710037"
---
# <a name="configure-usage-settings-and-policies"></a>Instellingen voor het gebruik en het beleid configureren
In dit artikel wordt beschreven hoe u het aantal gebruikers voor de testomgeving configureren, ze geregistreerd bij de testomgeving, bepalen het aantal uren dat de virtuele machine, en meer kan worden gebruikt. 


## <a name="specify-the-number-of-users-allowed-into-the-lab"></a>Geef het aantal gebruikers dat is toegestaan in de testomgeving

1. Selecteer **Gebruiksbeleid**. 
2. Voer in het **Gebruiksbeleid**, Instellingen, het **aantal gebruikers** in dat het lab mag gebruiken.
3. Selecteer **Opslaan**. 

    ![Gebruiksbeleid](../media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="send-registration-link-to-students"></a>Registratiekoppeling naar studenten verzenden

1. Schakel over naar de **gebruikers** weergeven door te selecteren **gebruikers** in het menu links. 
2. Selecteer **Get registratiekoppeling** tegel.

    ![Registratiekoppeling voor studenten](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. Selecteer in het dialoogvenster **Gebruikersregistratie** de knop **Kopiëren**. De koppeling wordt naar het klembord gekopieerd. Plak deze in een e-mailprogramma en verstuur een e-mail naar de student. 

    ![Registratiekoppeling voor studenten](../media/tutorial-setup-classroom-lab/registration-link.png)
2. Selecteer in het dialoogvenster **Gebruikersregistratie** de optie **Sluiten**. 
4. Deel de registratielink met een student zodat de student zich voor de les kan registreren. Als u hebt de **beperken optie** instelling ingeschakeld en een lijst van gebruikers in de lijst, de volgende acties uitvoeren:
    1. Selecteer de **e-mailadres** van de gebruiker in de lijst. 
    2. Ziet u een venster van de standaard-e-mailprogramma met de **aan** adres ingevuld. 
    3. Plak de **registratie-URL** u eerder hebt gekopieerd. 
    4. Verzendt de **e**. 

## <a name="view-users-registered-with-the-lab"></a>Gebruikers weergeven die zijn geregistreerd bij het lab

Selecteer **gebruikers** geregistreerd in het menu links om te zien van de lijst met gebruikers met de testomgeving. 

![Lijst met gebruikers die zijn geregistreerd bij de testomgeving](../media/how-to-configure-student-usage/users-list.png)

## <a name="set-quotas-per-user"></a>Quota instellen per gebruiker

1. Selecteer **gebruikers** in het menu links.
2. Selecteer **quotum per gebruiker: onbeperkte** op de werkbalk. 
3. Op de **quotum per gebruiker** weergeeft, schakelt **beperken het aantal uren dat een gebruiker kan een virtuele machine gebruiken**. 
4. Voor **hoeveel uur wilt u geven aan elke gebruiker**, voer het aantal uren en selecteert u **opslaan**. 

    ![Aantal uren per gebruiker](../media/how-to-configure-student-usage/number-of-hours-per-user.png)
5. Ziet u het aantal uren op de werkbalk nu: **quotum per gebruiker: &lt;aantal uren&gt;**. 

    ![Quotum per gebruiker](../media/how-to-configure-student-usage/quota-per-user.png)

## <a name="add-users-to-the-lab"></a>Gebruikers toevoegen aan het lab
Als u hebt de **toegang beperken** ingeschakeld, gebruikers (e-mailadressen) toevoegen aan de lijst.

1. Selecteer **gebruikers** in het menu links.
2. Selecteer **gebruikers toevoegen** op de werkbalk. 
3. Op de **gebruikers toevoegen** Voer e-mailadressen van gebruikers in de afzonderlijke regels of in een enkele regel, gescheiden door puntkomma's. 

    ![Gebruiker e-mailadressen toevoegen](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. Selecteer **Opslaan**. Ziet u het e-mailadressen van gebruikers en hun status (of niet geregistreerd) in de lijst. 

    ![Lijst met gebruikers](../media/how-to-configure-student-usage/users-list-new.png)

### <a name="add-users-by-uploading-a-csv-file"></a>Gebruikers toevoegen door een CSV-bestand te uploaden
U kunt ook gebruikers toevoegen door het uploaden van een CSV-bestand met e-mailadressen van gebruikers.

1. Selecteer **uploaden CSV** op de werkbalk.
2. Selecteer het CSV-bestand met e-mailadressen. Alle e-mailadressen moeten zich in één kolom, wanneer u het opent in Excel. 

## <a name="manage-user-vms"></a>Gebruiker virtuele machines beheren
Wanneer studenten registreren met Azure Lab Services met behulp van de registratie van de koppeling opgegeven, ziet u de virtuele machines toegewezen aan studenten op de **virtuele machines** tabblad. 

![Virtuele machines die zijn toegewezen aan studenten](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

U kunt de volgende taken op een student VM kunt doen: 

- Een virtuele machine stoppen als de virtuele machine wordt uitgevoerd. 
- Start een virtuele machine als de virtuele machine is gestopt. 
- Maak verbinding met de VM. 
- De virtuele machine verwijderen. 
- Bekijk het aantal uren dat gebruikers de virtuele machine gebruikt. 


## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

- [Als beheerder, maken en beheren van lab-accounts](how-to-manage-lab-accounts.md)
- [Als de eigenaar van een lab maken en beheren van labs](how-to-manage-classroom-labs.md)
- [Als de eigenaar van een lab instellen en sjablonen publiceren](how-to-create-manage-template.md)
- [Als een lab-gebruiker toegang krijgen tot leslokaallabs](how-to-use-classroom-lab.md)

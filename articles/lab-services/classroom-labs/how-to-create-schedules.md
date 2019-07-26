---
title: Planning voor klassikale Labs maken in Azure Lab Services | Microsoft Docs
description: Meer informatie over het maken van planningen voor klassikale Labs in Azure Lab Services zodat Vm's in de Labs op een opgegeven tijdstip worden gestart en afgesloten.
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
ms.date: 01/30/2019
ms.author: spelluru
ms.openlocfilehash: f607ba68563aa92797f45cf77db0575ae6802fee
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68385596"
---
# <a name="create-and-manage-schedules-for-classroom-labs-in-azure-lab-services"></a>Planningen voor het maken en beheren van klassikale Labs in Azure Lab Services 
Met planningen kunt u een leslokaal Lab zodanig configureren dat de virtuele machines in het lab automatisch worden gestart en op een opgegeven tijdstip worden afgesloten. U kunt een eenmalig schema of een terugkerende planning definiëren. De volgende procedures geven u stappen voor het maken en beheren van planningen voor een klas Lab: 

> [!IMPORTANT]
> De geplande uitvoerings tijd van Vm's telt niet op het [quotum dat aan een gebruiker is toegewezen](how-to-configure-student-usage.md#set-quotas-for-users). Het quotum geldt voor de tijd buiten de plannings uren die een student op Vm's doorbrengt. 

## <a name="add-a-schedule-once"></a>Een planning toevoegen (eenmaal)

1. Ga naar de pagina **schema's** en selecteer **planning toevoegen** op de werk balk. 

    ![Knop schema toevoegen op de pagina planningen](../media/how-to-create-schedules/add-schedule-button.png)
2. Op de pagina **schema toevoegen** controleert u of de optie **eenmaal** is geselecteerd bovenaan. Als dat niet het geval is, selecteert u **eenmaal**. 
3. Voer voor **plannings datum (vereist)** de datum in of selecteer het kalender pictogram om een datum te selecteren. 
4. Selecteer bij **begin tijd**het tijdstip waarop u wilt dat de vm's worden gestart. De begin tijd is vereist als de eind tijd niet is ingesteld. Selecteer **begin gebeurtenis verwijderen** als u alleen de eind tijd wilt opgeven. Als de **begin tijd** is uitgeschakeld, selecteert u **Start gebeurtenis toevoegen** naast de vervolg keuzelijst om deze optie in te scha kelen. 
5. Selecteer bij **eind tijd**het tijdstip waarop u wilt dat de virtuele machines worden afgesloten. De stop tijd is vereist als de begin tijd niet is ingesteld. Selecteer **Stop gebeurtenis verwijderen** als u alleen de begin tijd wilt opgeven. Als de **Stop tijd** is uitgeschakeld, selecteert u **Stop gebeurtenis toevoegen** naast de vervolg keuzelijst om deze optie in te scha kelen.
6. Selecteer voor **tijd zone (vereist)** de tijd zone voor de start-en stop tijden die u hebt opgegeven. 
7. Voor **notities**voert u een beschrijving of notities in voor de planning. 
8. Selecteer **Opslaan**. 

    ![Eenmalige-schema](../media/how-to-create-schedules/add-schedule-page.png)

## <a name="add-a-recurring-schedule-weekly"></a>Een terugkerend schema toevoegen (wekelijks)

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

## <a name="view-schedules-in-calendar"></a>Schema's in de agenda weer geven
U ziet dat de geplande datums en tijden zijn gemarkeerd in de weer gave kalender, zoals wordt weer gegeven in de volgende afbeelding:

![Schema's in de agenda weergave](../media/how-to-create-schedules/schedules-in-calendar.png)

Selecteer de knop **vandaag** in de rechter bovenhoek om over te scha kelen naar de huidige datum in de agenda. Selecteer **pijl-links** om over te scha kelen naar de vorige week en de **pijl-rechts** om over te scha kelen naar de volgende week in de agenda. 

## <a name="edit-a-schedule"></a>Een planning bewerken
Wanneer u op een gemarkeerde planning in de agenda dubbelklikt of de knop **Potlood** op de werk balk selecteert, ziet u de pagina **schema bewerken** . Het bijwerken van instellingen op deze pagina is hetzelfde als het bijwerken van de instellingen op de pagina **schema toevoegen** zoals beschreven in de sectie [een terugkerende planning toevoegen](#add-a-recurring-schedule-weekly) . 

![Schema pagina bewerken](../media/how-to-create-schedules/edit-schedule-page.png)

## <a name="delete-a-schedule"></a>Een schema verwijderen

1. Als u een planning wilt verwijderen, selecteert u de knop prullenbak kan (verwijderen) op de werk balk, zoals wordt weer gegeven in de volgende afbeelding:

    ![Knop verwijderen op de werk balk](../media/how-to-create-schedules/delete-schedule-button.png)

    U kunt de knop verwijderen gebruiken voor een van de geplande datums en tijden in de kalender en selecteer **verwijderen**. 
2. Selecteer op de pagina **planningen verwijderen** de optie **Ja**.

    ![De bevestiging schema verwijderen](../media/how-to-create-schedules/delete-schedules-confirmation.png)




## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

- [Labaccounts maken en beheren als beheerder](how-to-manage-lab-accounts.md)
- [Labs maken en beheren als labeigenaar](how-to-manage-classroom-labs.md)
- [Het gebruik van een lab configureren en beheren als labeigenaar](how-to-configure-student-usage.md)
- [Als test gebruiker toegang krijgen tot klassikale Labs](how-to-use-classroom-lab.md)

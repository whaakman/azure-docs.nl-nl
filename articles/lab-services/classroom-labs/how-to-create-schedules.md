---
title: Planning voor leslokaallabs maken in Azure Lab Services | Microsoft Docs
description: Informatie over het maken van planningen voor leslokaallabs in Azure Lab Services zodat virtuele machines in de labs opstarten en op een bepaald tijdstip afsluiten.
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
ms.openlocfilehash: 34bc8263053cd4a701c16ee1832cf1b27340a345
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55501359"
---
# <a name="create-and-manage-schedules-for-classroom-labs-in-azure-lab-services"></a>Maken en beheren van schema's voor leslokaallabs in Azure Lab Services 
Schema's kunt u een leslokaallab configureren zodat virtuele machines in het lab automatisch starten en op een bepaald tijdstip afsluiten. U kunt een eenmalige planning of een terugkerend schema definiëren. De volgende procedures kunnen u de stappen voor het maken en beheren van schema's voor een leslokaallab: 

> [!IMPORTANT]
> De geplande uitvoertijd van virtuele machines worden niet meegeteld in het [quota die zijn toegewezen aan een gebruiker](how-to-configure-student-usage.md#set-quotas-per-user). Het quotum is voor de tijd buiten schema voor uren dat een student op virtuele machines spendeert. 

## <a name="add-a-schedule-once"></a>Een schema (één keer) toevoegen

1. Schakel over naar de **planningen** pagina en selecteer **toevoegen planning** op de werkbalk. 

    ![Schema-knop op de pagina schema toevoegen](../media/how-to-create-schedules/add-schedule-button.png)
2. Op de **toevoegen planning** pagina, controleert **eenmaal** optie is geselecteerd op de bovenkant. Als dat niet het geval is, selecteert u **eenmaal**. 
3. Voor **datum (vereist) plannen**, de datum invoeren of Selecteer het kalenderpictogram een datum te selecteren. 
4. Voor **begintijd**, selecteert u het tijdstip waarop de virtuele machines worden gestart. De begintijd is vereist als de tijd voor het stoppen is niet ingesteld. Selecteer **verwijderen start gebeurtenis** als u wilt om op te geven alleen de tijd voor het stoppen. Als de **begintijd** is uitgeschakeld, selecteer **toevoegen begingebeurtenis** naast de vervolgkeuzelijst zodat het. 
5. Voor **stoptijd**, selecteert u het tijdstip waarop de virtuele machines worden afgesloten. De tijd voor het stoppen is vereist als de begintijd is niet ingesteld. Selecteer **stop-gebeurtenis Remove** als u wilt opgeven voor alleen de begintijd. Als de **stoptijd** is uitgeschakeld, selecteer **stop-gebeurtenis toevoegen** naast de vervolgkeuzelijst zodat het.
6. Voor **tijdzone (vereist)**, selecteert u de tijdzone voor het starten en stoppen keren dat u hebt opgegeven. 
7. Voor **opmerkingen bij de**, geen beschrijving of notities invoeren voor de planning. 
8. Selecteer **Opslaan**. 

    ![Eenmalige planning](../media/how-to-create-schedules/add-schedule-page.png)

## <a name="add-a-recurring-schedule-weekly"></a>Toevoegen van een terugkerend schema (wekelijks)

1. Schakel over naar de **planningen** pagina en selecteer **toevoegen planning** op de werkbalk. 

    ![Schema-knop op de pagina schema toevoegen](../media/how-to-create-schedules/add-schedule-button.png)
2. Op de **toevoegen planning** pagina, gaat u naar **wekelijkse** aan de bovenkant. 
3. Voor **plannen dagen (vereist)**, selecteer de dagen waarop de planning van kracht. In het volgende voorbeeld, is maandag tot vrijdag geselecteerd. 
4. Voor de **van** en voer de **plannen begindatum** of kies een datum selecteert de **agenda** knop. Dit veld is vereist. 
5. Voor **planning einddatum**Typ of Selecteer een einddatum waarop de virtuele machines moeten worden afgesloten. 
6. Voor **begintijd**, selecteert u het tijdstip waarop u wilt dat de virtuele machines worden gestart. De begintijd is vereist als de tijd voor het stoppen is niet ingesteld. Selecteer **verwijderen start gebeurtenis** als u wilt om op te geven alleen de tijd voor het stoppen. Als de **begintijd** is uitgeschakeld, selecteer **toevoegen begingebeurtenis** naast de vervolgkeuzelijst zodat het. 
7. Voor **stoptijd**, selecteert u het tijdstip waarop u wilt dat de virtuele machines worden afgesloten. De tijd voor het stoppen is vereist als de begintijd is niet ingesteld. Selecteer **stop-gebeurtenis Remove** als u wilt opgeven voor alleen de begintijd. Als de **stoptijd** is uitgeschakeld, selecteer **stop-gebeurtenis toevoegen** naast de vervolgkeuzelijst zodat het.
8. Voor **tijdzone (vereist)**, selecteert u de tijdzone voor het starten en stoppen keren dat u hebt opgegeven.  
9. Voor **opmerkingen bij de**, geen beschrijving of notities invoeren voor de planning. 
10. Selecteer **Opslaan**. 

    ![Wekelijks schema](../media/how-to-create-schedules/add-schedule-page-weekly.png)

## <a name="view-schedules-in-calendar"></a>Schema's weergeven in de agenda
U ziet de geplande datums en tijden die zijn gemarkeerd in de weergave van de kalender, zoals wordt weergegeven in de volgende afbeelding:

![Schema's in de kalenderweergave](../media/how-to-create-schedules/schedules-in-calendar.png)

Selecteer de **vandaag** knop in de rechterbovenhoek om over te schakelen naar de huidige datum in de agenda. Selecteer **pijl-links** overschakelen naar de vorige week en **pijl-rechts** overschakelen naar de volgende week in de agenda. 

## <a name="edit-a-schedule"></a>Een schema bewerken
Wanneer u dubbelklikt op een gemarkeerde schema in de agenda of Selecteer de **potlood** knop op de werkbalk, ziet u de **planning bewerken** pagina. Het bijwerken van instellingen op deze pagina is hetzelfde als het bijwerken van instellingen in de **toevoegen planning** pagina zoals beschreven in de [toevoegen van een terugkerend schema](#add-a-recurring-schedule-weekly) sectie. 

![De pagina Schema bewerken](../media/how-to-create-schedules/edit-schedule-page.png)

## <a name="delete-a-schedule"></a>Een schema verwijderen

1. U wilt verwijderen van een planning, selecteert u de Prullenbak kunt (verwijderen) op de werkbalk zoals weergegeven in de volgende afbeelding:

    ![Op de werkbalk verwijderen](../media/how-to-create-schedules/delete-schedule-button.png)

    Gebruik de verwijderknop voor het gebruik van de geplande datums en tijden in de kalender en selecteer **verwijderen**. 
2. Op de **verwijderen van schema's** weergeeft, schakelt **Ja**.

    ![Schema's bevestiging van verwijderen](../media/how-to-create-schedules/delete-schedules-confirmation.png)




## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

- [Labaccounts maken en beheren als beheerder](how-to-manage-lab-accounts.md)
- [Labs maken en beheren als labeigenaar](how-to-manage-classroom-labs.md)
- [Het gebruik van een lab configureren en beheren als labeigenaar](how-to-configure-student-usage.md)
- [Als een lab-gebruiker toegang krijgen tot leslokaallabs](how-to-use-classroom-lab.md)

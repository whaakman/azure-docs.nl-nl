---
title: Azure Privileged Identity Management goedkeuringswerkstromen | Microsoft Docs
description: Meer informatie over goedkeuringswerkstromen in Privileged Identity Management (PIM)
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: protection
ms.date: 04/28/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 025bcd0cde8d73cfdd4d79a77256a1705950f90a
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444643"
---
# <a name="approvals"></a>Approvals

## <a name="overview"></a>Overzicht

Met goedkeuringen voor Privileged Identity Management, kunt u rollen om te vereisen van goedkeuring voor activering configureren en kies een of meer gebruikers of groepen als gedelegeerde fiatteurs. Blijf lezen voor meer informatie over het configureren van rollen en fiatteurs selecteren.


## <a name="new-terminology"></a>Nieuwe terminologie

*In aanmerking komende rol gebruiker* : een gebruiker in aanmerking komende rol is een gebruiker binnen uw organisatie die is toegewezen aan een Azure AD-rol als in aanmerking komende (rol vereist activering).

*Gedelegeerde fiatteur* : een gedelegeerde fiatteur is een of meerdere personen of groepen in uw Azure AD die verantwoordelijk is voor het goedkeuren van aanvragen voor rollen activeren.

## <a name="scenarios"></a>Scenario's

De beperkte Preview-versie ondersteunt de volgende scenario's:

**Als een bevoorrechte rol beheerder (PRA) kunt u de:**

-   [goedkeuring voor specifieke rollen inschakelen](#enable-approval-for-specific-roles)

-   [Geef gebruikers van de fiatteur en/of groepen voor het goedkeuren van aanvragen](#specify-approver-users-and/or-groups-to-approve-requests)

-   [Geschiedenis van een aanvraag en goedkeuring voor alle bevoorrechte rollen weergeven](#view-request-and-approval-history-for-all-privileged-roles)

**Als een aangewezen fiatteur, kunt u het volgende doen:**

-   [weergeven in afwachting van goedkeuring (aanvragen)](#view-pending-approvals-requests)

-   [goedkeuren of afwijzen van aanvragen om benodigde bevoegdheden rol (één en/of bulk)](#approve-or-reject-requests-for-role-elevation-single-and/or-bulk)

-   [reden voor Mijn goedkeuring of afkeuring opgeven](#provide-justification-for-my-approval/rejection) 

**Als de gebruiker van een in aanmerking komende rol kunt u het volgende doen:**

-   [aanvraag voor activering van een rol waarvoor goedkeuring is vereist](#request-activation-of-a-role-that-requires-approval)

-   [Bekijk de status van uw aanvraag voor activeren](#view-the-status-of-your-request-to-activate)

-   [Uw taak te voltooien in Azure AD als de activering is goedgekeurd](#complete-your-task-in-azure-ad-if-activation-was-approved)

### <a name="navigation"></a>Navigatie

We hebben de navigatie ter ondersteuning van goedkeuringen bijgewerkt

![](media/azure-ad-pim-approval-workflow/image001.png)

De standaardwaarde landingspagina biedt een gemakkelijke manier toegang tot informatie over PIM en de nieuwe goedkeuringen-documentatie.

![](media/azure-ad-pim-approval-workflow/image002.png)

We hebben ook een nieuwe sectie voor alle gebruikers van PIM, 'Mijn controlegeschiedenis' toegevoegd. Hier vindt u alle informatie die relevant zijn voor uw identiteit. Dit omvat alle aanvragen voor de in behandeling en voltooid, alle beslissingen over de aanvragen die u kunt oplossen en uw laatste rolactiveringen in één handige locatie.

![](media/azure-ad-pim-approval-workflow/image003.png)

### <a name="enable-approval-for-specific-roles"></a>Goedkeuring voor specifieke rollen inschakelen

Als u wilt goedkeuren voor een specifieke functie hebt ingeschakeld, moet u eerst Directory-rollen selecteren in de linkernavigatiebalk.

![](media/azure-ad-pim-approval-workflow/image004.png)

Zoek en selecteer de instellingen in de navigatiebalk aan de Directory-rollen

![](media/azure-ad-pim-approval-workflow/image006.png)

Bevoorrechte rollen selecteren:

![](media/azure-ad-pim-approval-workflow/image009.png)

Selecteert u 'Inschakelen' in de goedkeuringssectie vereisen:

![](media/azure-ad-pim-approval-workflow/image011.png)

Eenmaal is ingeschakeld, wordt de blade uitvouwen om weer te geven van de volgende gegevens:

![](media/azure-ad-pim-approval-workflow/image013.png)

>[!NOTE]
Als u niet alle goedkeurders opgeeft, worden de PRA(s) de standaard fiatteur (s). PRA(s) zou zijn vereist voor het goedkeuren van alle activeringsaanvragen voor deze rol.

### <a name="specify-approver-users-andor-groups-to-approve-requests"></a>Geef gebruikers van de fiatteur en/of groepen voor het goedkeuren van aanvragen

Klik op de optie 'Select goedkeurders' voor het delegeren van goedkeuring:

![](media/azure-ad-pim-approval-workflow/image015.png)

Wanneer de optie fiatteurs-blade wordt geladen, u kunt zoeken naar een specifieke gebruiker of groep met behulp van de zoekbalk boven of selecteren in de vooraf gevulde lijst en klik vervolgens op "Selecteren" wanneer u klaar bent:

![](media/azure-ad-pim-approval-workflow/image017.png)

Opmerking: Kunt u meerdere gebruikers of groepen selecteren op een tijdstip.

Uw selectie wordt weergegeven in de lijst met geselecteerde goedkeurders zoals hieronder wordt weergegeven:

![](media/azure-ad-pim-approval-workflow/image019.png)

Als u wilt een goedkeurder verwijderen, klikt u op de knop verwijderen naast hun naam.

Als u wilt meer fiatteurs toevoegen, moet u het proces herhalen.

## <a name="view-request-and-approval-history-for-all-privileged-roles"></a>Geschiedenis van een aanvraag en goedkeuring voor alle bevoorrechte rollen weergeven

Aanvraag- en goedkeuringsprocessen als geschiedenis wilt weergeven voor alle bevoorrechte rollen, selecteert u controlegeschiedenis vanuit het dashboard:

![](media/azure-ad-pim-approval-workflow/image021.png)

>[!NOTE]
Sorteer de gegevens door de actie en zoek naar "Activeren goedgekeurd"

### <a name="view-pending-approvals-requests"></a>Weergeven in afwachting van goedkeuring (aanvragen)

Als een gedelegeerde fiatteur ontvangt u e-mailmeldingen wanneer een aanvraag voor de goedkeuring in behandeling is. Als u wilt weergeven van deze aanvragen in de portal voor PIM, vanuit het dashboard (in de nieuwe navigatiefunctie) Selecteer het tabblad 'Goedkeuringsaanvragen in behandeling' in de linker navigatiebalk.

![](media/azure-ad-pim-approval-workflow/image023.png)

Daar ziet u een lijst met aanvragen in afwachting van goedkeuring:

![](media/azure-ad-pim-approval-workflow/image024.png)

### <a name="approve-or-reject-requests-for-role-elevation-single-andor-bulk"></a>Goedkeuren of afwijzen van aanvragen om benodigde bevoegdheden rol (één en/of bulk)

Selecteer de aanvragen die u wilt goedkeuren of afwijzen en klik op de knop in de actiebalk die met uw beslissing overeenkomt:

![](media/azure-ad-pim-approval-workflow/image025.png)

### <a name="provide-justification-for-my-approvalrejection"></a>Reden voor Mijn goedkeuring of afkeuring opgeven

Hiermee wordt een nieuwe blade als u wilt goedkeuren of weigeren van meerdere aanvragen in één keer geopend. Voer een reden voor uw beslissing, en klik op (toestaan of weigeren) aan de onderkant of de blade:

![](media/azure-ad-pim-approval-workflow/image029.png)

Wanneer het proces voor aanvragen voltooid is, het statussymbool de beslissing weer (in dit voorbeeld wordt de beslissing is goedgekeurd):

![](media/azure-ad-pim-approval-workflow/image031.png)

### <a name="request-activation-of-a-role-that-requires-approval"></a>Activering van een rol waarvoor goedkeuring aanvragen

Activering van een rol waarvoor goedkeuring aanvragen kan worden gestart via de oude PIM-navigatie, of de nieuwe navigatiefunctie, als het proces voor rolactivering hetzelfde blijft. Selecteer een rol gewoon uit de lijst met rollen te activeren:

![](media/azure-ad-pim-approval-workflow/image033.png)

Als een bevoorrechte rol is vereist voor multi-factor Authentication, wordt u gevraagd om te voltooien die taak eerst:

![](media/azure-ad-pim-approval-workflow/image035.png)

Wanneer u klaar bent, klikt u op activeren en een reden opgeven (indien nodig):

![](media/azure-ad-pim-approval-workflow/image037.png)

De aanvrager ziet een melding dat de aanvraag in afwachting van goedkeuring is:

![](media/azure-ad-pim-approval-workflow/image039.png)

### <a name="view-the-status-of-your-request-to-activate"></a>Bekijk de status van uw aanvraag voor activeren

De status van een aanvraag in behandeling voor het activeren van weer te geven moet worden geopend in de nieuwe navigatie. Selecteer het tabblad 'Mijn Requests' in de linker navigatiebalk:

![](media/azure-ad-pim-approval-workflow/image041.png)

De status van de standaard ingesteld op 'In behandeling', maar u kunt in-of uitschakelen om te zien of de aanvragen is geweigerd.

### <a name="complete-your-task-in-azure-ad-if-activation-was-approved"></a>Uw taak te voltooien in Azure AD als de activering is goedgekeurd

Zodra de aanvraag is goedgekeurd, de rol actief is en u kunt doorgaan met werk dat deze rol is vereist.

![](media/azure-ad-pim-approval-workflow/image043.png)

## <a name="next-steps"></a>Volgende stappen

Uw feedback is belangrijk voor ons. Neem gerust opmerkingen of feedback met ons hier!

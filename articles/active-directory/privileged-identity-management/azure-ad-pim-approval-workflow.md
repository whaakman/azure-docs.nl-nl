---
title: Azure Privileged Identity Management goedkeuring werkstromen | Microsoft Docs
description: Meer informatie over werkstromen voor goedkeuring in Privileged Identity Management (PIM)
services: active-directory
documentationcenter: 
author: barclayn
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2017
ms.author: barclayn
ms.custom: pim
ms.openlocfilehash: cf6a9213fa0a1cba8725aabb42abe51b805ece7a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="approvals-preview"></a>Goedkeuringen (Preview)

## <a name="overview"></a>Overzicht

Met goedkeuringen voor Privileged Identity Management, kunt u rollen configureren om te goedkeuring vereisen voor activering en kies een of meerdere gebruikers of groepen als gedelegeerde fiatteurs. Houd lezen voor meer informatie over het configureren van rollen en fiatteurs selecteren.

>[!NOTE]
Houd er rekening mee dat deze functie nog in ontwikkeling is en fouten kunnen optreden. De functionaliteit, met inbegrip van tekst naamconventies nog worden gewijzigd en mogen niet worden beschouwd als laatste.


## <a name="key-terminology"></a>Belangrijkste termen

*In aanmerking komende gebruiker van de rol* – een in aanmerking komende rol is een gebruiker binnen uw organisatie die is toegewezen aan een Azure AD-rol als in aanmerking komende (rol activering vereist is).

*Gedelegeerde goedkeurder* – een gemachtigde goedkeurder is een of meerdere personen of groepen binnen uw Azure AD die verantwoordelijk zijn voor het goedkeuren van aanvragen voor activering van rollen.

## <a name="scenarios"></a>Scenario's

De private preview ondersteunt de volgende scenario's:

**Als een bevoorrechte rol beheerder (PRA) kunt u:**

-   [goedkeuring voor specifieke rollen inschakelen](#enable-approval-for-specific-roles)

-   [Geef goedkeurder gebruikers en/of groepen voor het goedkeuren van aanvragen](#specify-approver-users-and/or-groups-to-approve-requests)

-   [Geschiedenis van de aanvraag en goedkeuring voor alle bevoorrechte rollen weergeven](#view-request-and-approval-history-for-all-privileged-roles)

**Als een specifieke goedkeurder kunt u:**

-   [in afwachting van goedkeuring (aanvragen) weergeven](#view-pending-approvals-requests)

-   [goedkeuren of afwijzen aanvragen voor uitbreiding van de rol (één en/of bulk)](#approve-or-reject-requests-for-role-elevation-single-and/or-bulk)

-   [Geef de reden voor Mijn goedkeuring/afwijzing](#provide-justification-for-my-approval/rejection) 

**Als een in aanmerking komende gebruiker voor de rol kunt u:**

-   [activering van de aanvraag van een rol die goedkeuring vereist](#request-activation-of-a-role-that-requires-approval)

-   [de status van uw aanvraag voor het activeren van weergeven](#view-the-status-of-your-request-to-activate)

-   [uw taak te voltooien in Azure AD als activering is goedgekeurd](#complete-your-task-in-azure-ad-if-activation-was-approved)

### <a name="navigation"></a>Navigatie

We hebben de navigatie ter ondersteuning van goedkeuringen bijgewerkt

![](media/azure-ad-pim-approval-workflow/image001.png)

De standaardwaarde landingspagina biedt snel toegang tot informatie over PIM en de nieuwe goedkeuringen-documentatie.

![](media/azure-ad-pim-approval-workflow/image002.png)

We hebben hebt ook een nieuwe sectie voor alle gebruikers van PIM, 'Mijn controlegeschiedenis' toegevoegd. Hier vindt u alle informatie die relevant zijn voor uw identiteit. Dit omvat alle uw in behandeling en voltooid aanvragen, alle beslissingen over de aanvragen die u hebt opgelost en de afgelopen rol activeringen op één handige locatie.

![](media/azure-ad-pim-approval-workflow/image003.png)

### <a name="enable-approval-for-specific-roles"></a>Goedkeuring voor specifieke rollen inschakelen

Om goedkeuring voor een specifieke rol, moet u eerst Directory rollen selecteren in de linkernavigatiebalk.

![](media/azure-ad-pim-approval-workflow/image004.png)

Zoek en selecteer de instellingen in het linkernavigatievenster van Directory-functies

![](media/azure-ad-pim-approval-workflow/image006.png)

Selecteer de bevoorrechte rollen:

![](media/azure-ad-pim-approval-workflow/image009.png)

Selecteert u 'Inschakelen' in de goedkeuringssectie vereisen:

![](media/azure-ad-pim-approval-workflow/image011.png)

Eenmaal is ingeschakeld, wordt de blade uitvouwen om weer te geven van de volgende details:

![](media/azure-ad-pim-approval-workflow/image013.png)

>[!NOTE]
Als u niet alle goedkeurders opgeeft, worden de PRA(s) de standaard fiatteur (s). PRA(s) nodig zijn voor het goedkeuren van alle activeringsaanvragen voor deze rol.

### <a name="specify-approver-users-andor-groups-to-approve-requests"></a>Geef goedkeurder gebruikers en/of groepen voor het goedkeuren van aanvragen

Als u wilt delegeren goedkeuring, klik op de optie 'Select goedkeurders':

![](media/azure-ad-pim-approval-workflow/image015.png)

Wanneer de blade Selecteer goedkeurders wordt geladen, u kunt zoeken naar een specifieke gebruiker of groep met behulp van de zoekbalk boven of selecteren in de lijst met vooraf ingestelde en klik vervolgens op 'Selecteren' als voltooid:

![](media/azure-ad-pim-approval-workflow/image017.png)

Opmerking: U kunt meerdere gebruikers of groepen tegelijkertijd selecteren.

Uw selectie wordt weergegeven in de lijst met geselecteerde goedkeurders zoals hieronder wordt weergegeven:

![](media/azure-ad-pim-approval-workflow/image019.png)

Als u wilt een goedkeurder verwijderen, klikt u op de knop verwijderen naast hun naam.

Aanvullende als fiatteurs wilt toevoegen, moet u het proces herhalen.

## <a name="view-request-and-approval-history-for-all-privileged-roles"></a>Geschiedenis van de aanvraag en goedkeuring voor alle bevoorrechte rollen weergeven

Aanvraag en goedkeuring als geschiedenis wilt weergeven voor alle bevoorrechte rollen, selecteer controlegeschiedenis vanuit het dashboard:

![](media/azure-ad-pim-approval-workflow/image021.png)

>[!NOTE]
Sorteer de gegevens door de actie en zoek naar 'Activering goedgekeurd'

### <a name="view-pending-approvals-requests"></a>In afwachting van goedkeuring (aanvragen) weergeven

U zult als een gemachtigde goedkeurder e-mailmeldingen ontvangen wanneer een aanvraag wacht op uw goedkeuring wordt. Selecteer het tabblad 'goedkeuringsaanvragen in behandeling' in de linkernavigatiebalk om weer te geven deze aanvragen in de PIM-portal, vanuit het dashboard (in het navigatievenster aan de nieuwe).

![](media/azure-ad-pim-approval-workflow/image023.png)

Van daaruit ziet u een lijst met aanvragen in afwachting van goedkeuring:

![](media/azure-ad-pim-approval-workflow/image024.png)

### <a name="approve-or-reject-requests-for-role-elevation-single-andor-bulk"></a>Goedkeuren of afwijzen aanvragen voor uitbreiding van de rol (één en/of bulk)

Selecteer de aanvragen die u wilt goedkeuren of weigeren en klik op de knop in de actiebalk die met uw beslissing overeenkomt:

![](media/azure-ad-pim-approval-workflow/image025.png)

### <a name="provide-justification-for-my-approvalrejection"></a>Geef de reden voor Mijn goedkeuring/afwijzing

Hiermee opent u een nieuwe blade als u wilt goedkeuren of weigeren van meerdere aanvragen tegelijk. Voer een reden voor uw beslissing en klikt u op (toestaan of weigeren) aan de onderkant of de blade:

![](media/azure-ad-pim-approval-workflow/image029.png)

Wanneer het proces voor aanvragen voltooid is, geeft het statussymbool hand van de beslissing (in dit voorbeeld wordt de beslissing is goedkeuren):

![](media/azure-ad-pim-approval-workflow/image031.png)

### <a name="request-activation-of-a-role-that-requires-approval"></a>Activering van de aanvraag van een rol die goedkeuring vereist

Activering van een rol die goedkeuring vereist aanvraagt kan van de oude PIM-navigatie of de nieuwe navigatie worden gestart omdat het proces voor rolactivering hetzelfde is gebleven. Selecteer een rol gewoon uit de lijst met rollen te activeren:

![](media/azure-ad-pim-approval-workflow/image033.png)

Als een bevoorrechte rol meerledige verificatie vereist, wordt u gevraagd eerst die taak te voltooien:

![](media/azure-ad-pim-approval-workflow/image035.png)

Wanneer u klaar bent, klikt u op activeren en een reden opgeven (indien nodig):

![](media/azure-ad-pim-approval-workflow/image037.png)

De aanvrager ziet een melding dat de aanvraag in afwachting van goedkeuring is:

![](media/azure-ad-pim-approval-workflow/image039.png)

### <a name="view-the-status-of-your-request-to-activate"></a>De status van uw aanvraag voor het activeren van weergeven

De statuscontrole van een aanvraag in behandeling activeren moet worden geopend in de nieuwe navigatie. Selecteer het tabblad 'Mijn aanvragen' in de linkernavigatiebalk:

![](media/azure-ad-pim-approval-workflow/image041.png)

De aanvraagstatus wordt standaard ingesteld op 'In behandeling', maar u kunt schakelen om alle te zien of afgewezen aanvragen.

### <a name="complete-your-task-in-azure-ad-if-activation-was-approved"></a>Uw taak te voltooien in Azure AD als activering is goedgekeurd

Zodra de aanvraag is goedgekeurd, wordt de rol actief is en u kunt doorgaan met werk waarvoor deze rol is vereist.

![](media/azure-ad-pim-approval-workflow/image043.png)

## <a name="next-steps"></a>Volgende stappen

Uw feedback is belangrijk voor ons. Aarzel niet om te delen opmerkingen of feedback met ons hier!

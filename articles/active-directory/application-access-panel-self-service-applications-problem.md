---
title: Probleem met toegang tot de toepassing zelf | Microsoft Docs
description: Problemen met betrekking tot toegang tot de toepassing zelf oplossen
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.reviewer: japere
ms.openlocfilehash: dcd3ea1269ff40e4777cc9b0ca46f0b64560f923
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="problem-using-self-service-application-access"></a>Probleem met toegang tot selfservice-toepassingen

Toegang tot de toepassing zelf is een uitstekende manier om toestaan dat gebruikers zelf detecteren toepassingen, toestaan de bedrijfsgroep goedkeuren van toegang tot deze toepassingen. U kunt de bedrijfsgroep voor het beheren van de referenties die zijn toegewezen aan deze gebruikers voor het recht wachtwoord eenmalige aanmelding op toepassingen van hun panelen toegang toestaan.

Voordat uw gebruikers toepassingen van hun Toegangsvenster automatisch detecteren kunnen, moet u inschakelen **toegang tot toepassingen Self-service** naar elke toepassing die u wilt toestaan dat gebruikers zelf detecteren en aanvragen toegang tot.

## <a name="general-issues-to-check-first"></a>Algemene problemen om eerst te controleren

-   Zorg ervoor dat de toegang tot selfservice toepassing correct is geconfigureerd. Zie ' toegang tot de toepassing zelf configureren '.

-   Zorg ervoor dat de gebruiker of groep is ingeschakeld voor het aanvragen van toegang tot de toepassing zelf.

-   Zorg ervoor dat de gebruiker de juiste plaats voor toegang tot toepassingen selfservice bezoekt. gebruikers kunnen navigeren naar hun [toepassing Toegangspaneel](https://myapps.microsoft.com/) en klik op de **+ toevoegen** om te zoeken van de apps waarmee u toegang tot selfservice hebt ingeschakeld.

-   Als toegang tot de toepassing zelf onlangs is geconfigureerd, probeert aan te melden en afmelden opnieuw naar de gebruiker toegang deelvenster na een paar minuten om te zien als de wijzigingen access selfservicegebruikers hebben gestaan.

## <a name="how-to-configure-self-service-application-access"></a>Toegang tot selfservice-toepassingen configureren

Voor self-service toepassing toegang tot een toepassing, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u wilt inschakelen, Self-service toegang tot in de lijst.

7.  Nadat de toepassing wordt geladen, klikt u op **Self-service** van navigatiemenu links aan de van de toepassing.

8.  Voor self-service toepassing toegang voor deze toepassing, schakelt u de **toestaan dat gebruikers toegang tot deze toepassing aanvragen?** in-of uitschakelen op **Ja.**

9.  Klik vervolgens op de selector naast het label om te selecteren in de groep waartoe gebruikers die aanvragen toegang tot deze toepassing moet worden toegevoegd, **voor welke groep toegewezen gebruikers worden toegevoegd?** en selecteert u een groep.

10. **Optioneel:** als u wilt een zakelijke goedkeuringsprocedure voordat gebruikers toegang hebben, stelt u de **moeten worden goedgekeurd voordat het verlenen van toegang tot deze toepassing?** in-of uitschakelen op **Ja**.

11. **Optioneel: voor toepassingen die gebruikmaken van wachtwoord eenmalige aanmelding alleen op** als u deze bedrijven fiatteurs om op te geven van de wachtwoorden die worden verzonden naar deze toepassing voor goedgekeurde gebruikers toestaan wilt, stelt u de **fiatteurs instellen van wachtwoorden voor deze toepassing van de gebruiker toestaan?** in-of uitschakelen op **Ja**.

12. **Optioneel:** om op te geven de zakelijke fiatteurs die zijn toegestaan voor het goedkeuren van toegang tot deze toepassing, klikt u op de selector naast het label **die is toegestaan voor het goedkeuren van toegang tot deze toepassing?** maximaal 10 afzonderlijke business goedkeurders selecteren.

 >[!NOTE]
 > Groepen worden niet ondersteund.
 >
 >

13. **Optioneel:** **voor toepassingen die functies zichtbaar**, als u wilt Self-service goedgekeurde gebruikers toewijzen aan een rol, klikt u op de selector naast de **welke rol u gebruikers wilt toewijzen in deze toepassing?** om de rol waaraan u deze gebruikers worden toegewezen te selecteren.

14. Klik op de **opslaan** knop aan de bovenkant van de blade om te voltooien.

Nadat u de configuratie van Self-Service toepassing hebt voltooid, gebruikers kunnen navigeren naar hun [toepassing Toegangspaneel](https://myapps.microsoft.com/) en klik op de **+ toevoegen** om te zoeken van de apps waarmee u toegang tot selfservice hebt ingeschakeld. Zakelijke goedkeurders er ook een melding in hun [Toegangspaneel toepassing](https://myapps.microsoft.com/). U kunt een melding wanneer een gebruiker heeft toegang tot een toepassing die hun goedkeuring vereist aangevraagd e-mail inschakelen. 

Deze goedkeuringen ondersteuning voor één werkstromen voor goedkeuring, wat betekent dat als u meerdere goedkeurders opgeeft, een enkele fiatteur toegang tot de toepassing goedkeuren kan.

## <a name="if-these-troubleshooting-steps-do-not-resolve-the-issue"></a>Als bovenstaande stappen voor probleemoplossing het probleem niet oplossen 

een ondersteuningsticket opent met de volgende informatie, indien beschikbaar:

-   Correlatie fout-ID

-   UPN (e-mailadres van de gebruiker)

-   TenantID

-   Browsertype

-   Tijdzone en de tijd/tijdsbestek tijdens fout optreedt

-   Fiddler traceringen

## <a name="next-steps"></a>Volgende stappen
[Azure Active Directory instellen voor selfservicegroepsbeheer](active-directory-accessmanagement-self-service-group-management.md)

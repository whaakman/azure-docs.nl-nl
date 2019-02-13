---
title: Problemen met toegang tot Self-servicetoepassingen | Microsoft Docs
description: Oplossen van problemen met betrekking tot toegang tot Self-servicetoepassingen
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: celested
ms.reviewer: japere,asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: debee359c155eef92a0161c30759ffdfb9ac3358
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56169459"
---
# <a name="problem-using-self-service-application-access"></a>Problemen met toegang tot Self-servicetoepassingen

Toegang tot Self-servicetoepassingen is een uitstekende manier om toe te staan gebruikers voor het zelf detecteren van toepassingen, toestaan de groep bedrijven voor het goedkeuren van toegang tot deze toepassingen. U kunt de bedrijfsgroep voor het beheren van de referenties die zijn toegewezen aan gebruikers voor eenmalige aanmelding op toepassingen wachtwoord rechts van hun toegangsvensters.

Voordat uw gebruikers zelf toepassingen in hun Toegangsvenster detecteren kunnen, moet u om in te schakelen **toegang tot Self-servicetoepassingen** op alle toepassingen die u wilt toestaan dat gebruikers zelf detecteren en aanvragen toegang tot.

## <a name="general-issues-to-check-first"></a>Algemene problemen om eerst te controleren

-   Zorg ervoor dat de toegang tot Self-servicetoepassingen correct is geconfigureerd. Zie ' toegang tot Self-servicetoepassingen configureren '.

-   Zorg ervoor dat de gebruiker of groep is ingeschakeld voor het aanvragen van toegang tot Self-servicetoepassingen.

-   Zorg ervoor dat de gebruiker de juiste plaats voor toegang tot Self-servicetoepassingen bezoekt. gebruikers kunnen navigeren naar hun [Toegangsvenster](https://myapps.microsoft.com/) en klikt u op de **+ toevoegen** knop om de apps waarmee u toegang tot Self-servicegebruikers hebt ingeschakeld te vinden.

-   Als toegang tot Self-servicetoepassingen onlangs is geconfigureerd, proberen aan te melden en afmelden opnieuw in het toegangsvenster van de gebruiker na een paar minuten om te zien als de wijzigingen zelf toegang hebben gestaan.

## <a name="how-to-configure-self-service-application-access"></a>Self-servicetoepassingen toegang configureren

Om in te schakelen Self-servicetoepassingen toegang tot een toepassing, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** boven aan het navigatiemenu links belangrijkste.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linker navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt inschakelen, selfservice toegang tot in de lijst.

7.  Nadat de toepassing wordt geladen, klikt u op **selfservice** vanuit navigatiemenu voor links van de toepassing.

8.  Om in te schakelen toegang tot Self-servicetoepassingen voor deze toepassing, schakelt u de **toestaan dat gebruikers toegang tot deze toepassing aanvragen?** overzet naar **Ja.**

9.  Klik vervolgens op de selector naast het label te selecteren in de groep waaraan de gebruikers die aanvragen toegang tot deze toepassing moet worden toegevoegd, **aan welke groep moeten toegewezen gebruikers worden toegevoegd?** en selecteert u een groep.

10. **Optioneel:** Als u wilt een zakelijke goedkeuring vereisen voordat gebruikers toegang hebben, stelt u de **goedkeuring vereisen voordat het verlenen van toegang tot deze toepassing?** overzet naar **Ja**.

11. **Optioneel: Voor toepassingen die gebruikmaken van eenmalige wachtwoord alleen op** als u toestaan dat de fiatteur om op te geven van de wachtwoorden die worden verzonden naar deze toepassing voor goedgekeurde gebruikers wilt, stelt u de **fiatteurs instellen van wachtwoorden van gebruikers voor dit toestaan toepassing is?**  overzet naar **Ja**.

12. **Optioneel:** Als u wilt de fiatteur die gemachtigd zijn om goed te keuren toegang tot deze toepassing opgeven, klikt u op de kiezer naast het label **die is toegestaan toegang tot deze toepassing goedkeuren?** maximaal 10 afzonderlijke fiatteur selecteren.

 >[!NOTE]
 > Groepen worden niet ondersteund.
 >
 >

13. **Optioneel:** **Voor toepassingen waardoor rollen worden blootgesteld**, als u wilt Self-service goedgekeurde gebruikers toewijzen aan een rol, klik op de selector naast de **aan welke rol moeten gebruikers worden toegewezen in deze toepassing?** die de rol selecteren Deze gebruikers moeten worden toegewezen.

14. Klik op de **opslaan** knop aan de bovenkant van de blade om te voltooien.

Nadat u Self-servicetoepassingen configuratie is voltooid, ernaartoe kunnen navigeren hun [Toegangsvenster](https://myapps.microsoft.com/) en klikt u op de **+ toevoegen** knop om de apps waarop u selfservice hebt ingeschakeld te vinden de toegang. Fiatteur ziet ook een melding in hun [Toegangsvenster](https://myapps.microsoft.com/). U kunt een melding wanneer een gebruiker heeft toegang tot een toepassing waarvoor goedkeuring aangevraagd e-mail inschakelen. 

Deze goedkeuringen één goedkeuringsworkflows ondersteunen alleen, wat betekent dat als u meerdere goedkeurders opgeeft, een één goedkeurder een aanvraag toegang tot de toepassing goedkeuren kan.

## <a name="if-these-troubleshooting-steps-do-not-resolve-the-issue"></a>Als deze stappen voor probleemoplossing het probleem niet verhelpen 

Open een ondersteuningsticket met de volgende informatie, indien beschikbaar:

-   Correlatie-ID van fout

-   UPN (e-mailadres van gebruiker)

-   TenantID

-   Browsertype

-   Tijdzone en de tijd/tijdsbestek tijdens de fout optreedt

-   Fiddler traceringen

## <a name="next-steps"></a>Volgende stappen
[Azure Active Directory instellen voor groepsbeheer met Self-service](../users-groups-roles/groups-self-service-management.md)

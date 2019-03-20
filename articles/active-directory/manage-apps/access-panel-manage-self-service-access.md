---
title: Het gebruik van toegang tot Self-servicetoepassingen | Microsoft Docs
description: Self-servicetoepassingen toegang tot toestaan dat gebruikers hun eigen toepassingen zoeken inschakelen
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
ms.openlocfilehash: 3afe915f4fa1d9c1c36860d23912ac0e01088724
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58093504"
---
# <a name="how-to-use-self-service-application-access"></a>Het gebruik van toegang tot Self-servicetoepassingen

Voordat uw gebruikers zelf toepassingen in hun Toegangsvenster detecteren kunnen, moet u om in te schakelen **toegang tot Self-servicetoepassingen** op alle toepassingen die u wilt toestaan dat gebruikers zelf detecteren en aanvragen toegang tot.

Deze functie is een goede manier waarop u kunt tijd en geld besparen als een IT-groep, en wordt aangeraden als onderdeel van een implementatie van moderne toepassingen met Azure Active Directory.

Met deze functie kunt u het volgende doen:

-   Toestaan dat gebruikers zelf toepassingen detecteren in de [Toegangsvenster](https://myapps.microsoft.com/) zonder te proberen alles van de IT-afdeling.

-   Deze gebruikers toevoegen aan een vooraf geconfigureerde groep, zodat u kunt zien wie heeft toegang aangevraagd, toegang verwijderen en de aan hen toegewezen rollen beheren.

-   Een fiatteur om goed te keuren aanvragen voor toepassingstoegang, zodat de IT-afdeling niet te hoeven toestaan.

-   Configureer desgewenst maximaal 10 personen die mogelijk toegang tot deze toepassing goedkeuren.

-   Een bedrijf toestaan goedkeurder om in te stellen de wachtwoorden die gebruikers kunt gebruiken om aan te melden bij de toepassing rechtstreeks vanuit de fiatteur [Toegangsvenster](https://myapps.microsoft.com/).

-   Self-service gebruikers rechtstreeks toegewezen aan een toepassingsrol (optioneel) automatisch laten toewijzen.

## <a name="enable-self-service-application-access-to-allow-users-to-find-their-own-applications"></a>Self-servicetoepassingen toegang tot toestaan dat gebruikers hun eigen toepassingen zoeken inschakelen

Toegang tot Self-servicetoepassingen is een uitstekende manier om toe te staan gebruikers voor het zelf detecteren van toepassingen, toestaan de groep bedrijven voor het goedkeuren van toegang tot deze toepassingen. U kunt de bedrijfsgroep voor het beheren van de referenties die zijn toegewezen aan gebruikers voor eenmalige aanmelding op toepassingen wachtwoord rechts van hun toegangsvensters.

Om in te schakelen Self-servicetoepassingen toegang tot een toepassing, de volgende stappen uit te voeren:

1. Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2. Open de **Azure Active Directory-extensie** door te klikken op **alle services** boven aan het navigatiemenu links belangrijkste.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linker navigatie.

5. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6. Selecteer de toepassing die u wilt inschakelen, selfservice toegang tot in de lijst.

7. Nadat de toepassing wordt geladen, klikt u op **selfservice** vanuit navigatiemenu voor links van de toepassing.

8. Om in te schakelen toegang tot Self-servicetoepassingen voor deze toepassing, schakelt u de **toestaan dat gebruikers toegang tot deze toepassing aanvragen?** overzet naar **Ja.**

9. Klik vervolgens op de selector naast het label te selecteren in de groep waaraan de gebruikers die aanvragen toegang tot deze toepassing moet worden toegevoegd, **aan welke groep moeten toegewezen gebruikers worden toegevoegd?** en selecteert u een groep.

10. **Optioneel:** Als u wilt een zakelijke goedkeuring vereisen voordat gebruikers toegang hebben, stelt u de **goedkeuring vereisen voordat het verlenen van toegang tot deze toepassing?** overzet naar **Ja**.

11. **Optioneel: Voor toepassingen die gebruikmaken van eenmalige wachtwoord alleen op** als u toestaan dat de fiatteur om op te geven van de wachtwoorden die worden verzonden naar deze toepassing voor goedgekeurde gebruikers wilt, stelt u de **fiatteurs instellen van wachtwoorden van gebruikers voor dit toestaan toepassing is?**  overzet naar **Ja**.

12. **Optioneel:** Als u wilt de fiatteur die gemachtigd zijn om goed te keuren toegang tot deze toepassing opgeven, klikt u op de kiezer naast het label **die is toegestaan toegang tot deze toepassing goedkeuren?** maximaal 10 afzonderlijke fiatteur selecteren.

    * Groepen worden niet ondersteund.

13. **Optioneel:** **Voor toepassingen waardoor rollen worden blootgesteld**, als u wilt Self-service goedgekeurde gebruikers toewijzen aan een rol, klik op de selector naast de **aan welke rol moeten gebruikers worden toegewezen in deze toepassing?** die de rol selecteren Deze gebruikers moeten worden toegewezen.

14. Klik op de **opslaan** knop aan de bovenkant van de blade om te voltooien.

Nadat u Self-servicetoepassingen configuratie is voltooid, ernaartoe kunnen navigeren hun [Toegangsvenster](https://myapps.microsoft.com/) en klikt u op de **+ toevoegen** knop om de apps waarop u selfservice hebt ingeschakeld te vinden de toegang. Fiatteur ziet ook een melding in hun [Toegangsvenster](https://myapps.microsoft.com/). U kunt een melding wanneer een gebruiker heeft toegang tot een toepassing waarvoor goedkeuring aangevraagd e-mail inschakelen. 

Deze goedkeuringen één goedkeuringsworkflows ondersteunen alleen, wat betekent dat als u meerdere goedkeurders opgeeft, een één goedkeurder een aanvraag toegang tot de toepassing goedkeuren kan.

## <a name="next-steps"></a>Volgende stappen
[Azure Active Directory instellen voor groepsbeheer met Self-service](../users-groups-roles/groups-self-service-management.md)

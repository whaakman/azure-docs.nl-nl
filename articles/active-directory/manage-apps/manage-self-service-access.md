---
title: Het configureren van self-servicetoepassingen toewijzing | Microsoft Docs
description: Self-servicetoepassingen toegang tot toestaan dat gebruikers hun eigen toepassingen zoeken inschakelen
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: barbkess
ms.openlocfilehash: 1876ff0cea2e46f8159b9ffb24de72a6272b3a79
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53077071"
---
# <a name="how-to-configure-self-service-application-assignment"></a>Self-servicetoepassingen toewijzing configureren

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
  
    > [!NOTE]
    > Groepen die zijn gesynchroniseerd van on-premises worden niet ondersteund om te worden gebruikt voor de groep waaraan de gebruikers die toegang tot deze toepassing willen moeten worden toegevoegd.
  
10. **Optioneel:** als u wilt een zakelijke goedkeuring vereisen voordat gebruikers toegang hebben, stelt u de **goedkeuring vereisen voordat het verlenen van toegang tot deze toepassing?** overzet naar **Ja**.

11. **Optioneel: voor toepassingen die gebruikmaken van eenmalige wachtwoord alleen op** als u toestaan dat de fiatteur om op te geven van de wachtwoorden die worden verzonden naar deze toepassing voor goedgekeurde gebruikers wilt, stelt u de **fiatteurs om in te stellen van de gebruiker toestaan de wachtwoorden voor deze toepassing?**  overzet naar **Ja**.

12. **Optioneel:** om op te geven van de fiatteur die gemachtigd zijn om goed te keuren toegang tot deze toepassing, klikt u op de kiezer naast het label **die is toegestaan toegang tot deze toepassing goedkeuren?** maximaal 10 afzonderlijke selecteren fiatteur.

     > [!NOTE]
     > Groepen worden niet ondersteund.
     >
     >

13. **Optioneel:** **voor toepassingen waardoor rollen worden blootgesteld**, als u wilt Self-service goedgekeurde gebruikers toewijzen aan een rol, klik op de selector naast de **aan welke rol moeten gebruikers worden toegewezen in deze toepassing?** Selecteer de rol die deze gebruikers moeten worden toegewezen.

14. Klik op de **opslaan** knop aan de bovenkant van de blade om te voltooien.

Nadat u Self-servicetoepassingen configuratie is voltooid, ernaartoe kunnen navigeren hun [Toegangsvenster](https://myapps.microsoft.com/) en klikt u op de **+ toevoegen** knop om de apps waarop u selfservice hebt ingeschakeld te vinden de toegang. Fiatteur ziet ook een melding in hun [Toegangsvenster](https://myapps.microsoft.com/). U kunt een melding wanneer een gebruiker heeft toegang tot een toepassing waarvoor goedkeuring aangevraagd e-mail inschakelen. 

Deze goedkeuringen ondersteunen slechts één goedkeuringswerkstromen wat betekent dat als u meerdere goedkeurders opgeeft, kan alle één goedkeurder goedkeurder toegang tot de toepassing.

## <a name="next-steps"></a>Volgende stappen
[Azure Active Directory instellen voor groepsbeheer met Self-service](../users-groups-roles/groups-self-service-management.md)

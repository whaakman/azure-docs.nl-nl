---
title: Gebruikers en groepen toewijzen aan een toepassing | Microsoft Docs
description: Gebruikers toewijzen aan de toepassing om toegang te verlenen
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
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 2c0cdd10c23b8511a757673a1e2046bc454e49b8
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39363520"
---
# <a name="how-to-assign-users-and-groups-to-an-application"></a>Gebruikers en groepen toewijzen aan een toepassing

Voordat uw gebruikers een van de volgende onderwerpen voor een bepaalde toepassing uitvoeren kunnen, moet u eerst **toewijzen aan de toepassing** ze om toegang te verlenen:

-   Toegang tot een toepassing door **te navigeren naar de URL van de toepassing rechtstreeks** (ook wel bekend als Serviceprovider geïnitieerde eenmalige aanmelding).

-   Toegang tot een toepassing met behulp van de **URL van gebruikerstoegang** op van een toepassing **eigenschappen** pagina (ook wel bekend als id-provider geïnitieerde eenmalige aanmelding).

-   Zie een toepassing worden weergegeven op hun [Toegangsvenster](https://myapps.microsoft.com/) of mobiele toepassing.

-   Zie een toepassing worden weergegeven op hun [startprogramma voor toepassingen van Office 365](https://support.office.com/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a).

## <a name="methods-to-assign-applications-with-azure-active-directory"></a>Methoden voor het toewijzen van toepassingen met Azure Active Directory 

Er zijn 3 manieren kunt u toepassingen met Azure Active Directory toewijzen:

-   [Een gebruiker toewijzen rechtstreeks aan een toepassing als een beheerder](#assign-a-user-directly-as-an-administrator)

-   [Een groep worden toegewezen om rechtstreeks naar een toepassing als een beheerder](#assign-a-group-directly-to-an-application-as-an-administrator)

-   [Self-servicetoepassingen toegang tot toestaan dat gebruikers hun eigen toepassingen zoeken inschakelen](#enable-self-service-application-access-to-allow-users-to-find-their-own-applications)

## <a name="assign-a-user-directly-as-an-administrator"></a>Een gebruiker toewijzen rechtstreeks als een beheerder

Als u wilt toewijzen een of meer gebruikers rechtstreeks aan een toepassing, de volgende stappen uit te voeren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** boven aan het navigatiemenu links belangrijkste.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linker navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt toewijzen van een gebruiker in de lijst.

7.  Nadat de toepassing wordt geladen, klikt u op **gebruikers en groepen** vanuit navigatiemenu voor links van de toepassing.

8.  Klik op de **toevoegen** knop boven de **gebruikers en groepen** lijst om de **toevoegen toewijzing** deelvenster.

9.  Klik op de **gebruikers en groepen** selector vanaf de **toevoegen toewijzing** deelvenster.

10. Typ in het **volledige naam** of **e-mailadres** van de gebruiker die u geïnteresseerd bent in toewijzen in de **zoeken op naam of e-mailadres** zoekvak.

11. Beweeg de muisaanwijzer over de **gebruiker** in de lijst om weer te geven een **selectievakje**. Klik op het selectievakje naast de profielfoto van de gebruiker of het logo om toe te voegen van de gebruiker naar de **geselecteerde** lijst.

12. **Optioneel:** als u wilt **toevoegen van meer dan één gebruiker**, type in een andere **volledige naam** of **e-mailadres** in de **zoeken op naam of e-mailadres** zoekvak en klik op het selectievakje voor deze gebruiker toevoegen aan de **geselecteerde** lijst.

13. Wanneer u klaar bent met gebruikers te selecteren, klikt u op de **Selecteer** knop aan de lijst met gebruikers en groepen die moeten worden toegewezen aan de toepassing wilt toevoegen.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** deelvenster te selecteren van een rol toewijzen aan de gebruikers die u hebt geselecteerd.

15. Klik op de **toewijzen** knop om de toepassing aan de geselecteerde gebruikers te wijzen.

Na een korte periode, de gebruikers die u hebt geselecteerd mogelijk om deze toepassingen met behulp van de methoden die worden beschreven in de sectie oplossing beschrijving te starten.

## <a name="assign-a-group-directly-to-an-application-as-an-administrator"></a>Een groep worden toegewezen om rechtstreeks naar een toepassing als een beheerder

Als u wilt toewijzen een of meer groepen rechtstreeks aan een toepassing, de volgende stappen uit te voeren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** boven aan het navigatiemenu links belangrijkste.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linker navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt toewijzen van een gebruiker in de lijst.

7.  Nadat de toepassing wordt geladen, klikt u op **gebruikers en groepen** vanuit navigatiemenu voor links van de toepassing.

8.  Klik op de **toevoegen** knop boven de **gebruikers en groepen** lijst om de **toevoegen toewijzing** deelvenster.

9.  Klik op de **gebruikers en groepen** selector vanaf de **toevoegen toewijzing** deelvenster.

10. Typ in het **volledige groepsnaam** van de groep die u geïnteresseerd bent in toewijzen in de **zoeken op naam of e-mailadres** zoekvak.

11. Beweeg de muisaanwijzer over de **groep** in de lijst om weer te geven een **selectievakje**. Klik op het selectievakje naast de profielfoto van de groep of het logo om toe te voegen van de gebruiker naar de **geselecteerde** lijst.

12. **Optioneel:** als u wilt **toevoegen van meer dan één groep**, type in een andere **volledige groepsnaam** in de **zoeken op naam of e-mailadres** zoekvak typt, en Klik op het selectievakje in als u wilt toevoegen van deze groep de **geselecteerde** lijst.

13. Wanneer u klaar bent met groepen te selecteren, klikt u op de **Selecteer** knop aan de lijst met gebruikers en groepen die moeten worden toegewezen aan de toepassing wilt toevoegen.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** deelvenster te selecteren van een rol toewijzen aan de groepen die u hebt geselecteerd.

15. Klik op de **toewijzen** om toe te wijzen van de toepassing aan de geselecteerde groepen.

Na een korte periode, de gebruikers binnen de groepen die u hebt geselecteerd mogelijk om deze toepassingen met behulp van de methoden die worden beschreven in de sectie oplossing beschrijving te starten. Als dit dynamische groepen zijn, mogelijk zijn er enkele aanvullende verwerkingsvertraging in deze toewijzingen worden weergegeven voor gebruikers binnen deze groepen toegewezen.

## <a name="enable-self-service-application-access-to-allow-users-to-find-their-own-applications"></a>Self-servicetoepassingen toegang tot toestaan dat gebruikers hun eigen toepassingen zoeken inschakelen

Toegang tot Self-servicetoepassingen is een uitstekende manier om toe te staan gebruikers voor het zelf detecteren van toepassingen, toestaan de groep bedrijven voor het goedkeuren van toegang tot deze toepassingen. U kunt de bedrijfsgroep voor het beheren van de referenties die zijn toegewezen aan gebruikers voor eenmalige aanmelding op toepassingen wachtwoord rechts van hun toegangsvensters.

Om in te schakelen Self-servicetoepassingen toegang tot een toepassing, de volgende stappen uit te voeren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** boven aan het navigatiemenu links belangrijkste.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linker navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt inschakelen, selfservice toegang tot in de lijst.

7.  Nadat de toepassing wordt geladen, klikt u op **selfservice** vanuit navigatiemenu voor links van de toepassing.

8.  Om in te schakelen toegang tot Self-servicetoepassingen voor deze toepassing, schakelt u de **toestaan dat gebruikers toegang tot deze toepassing aanvragen?** overzet naar **Ja.**

9.  Klik vervolgens op de selector naast het label te selecteren in de groep waaraan de gebruikers die aanvragen toegang tot deze toepassing moet worden toegevoegd, **aan welke groep moeten toegewezen gebruikers worden toegevoegd?** en selecteert u een groep.

10. **Optioneel:** als u wilt een zakelijke goedkeuring vereisen voordat gebruikers toegang hebben, stelt u de **goedkeuring vereisen voordat het verlenen van toegang tot deze toepassing?** overzet naar **Ja**.

11. **Optioneel: voor toepassingen die gebruikmaken van eenmalige wachtwoord alleen op** als u toestaan dat de fiatteur om op te geven van de wachtwoorden die worden verzonden naar deze toepassing voor goedgekeurde gebruikers wilt, stelt u de **fiatteurs om in te stellen van de gebruiker toestaan de wachtwoorden voor deze toepassing?**  overzet naar **Ja**.

12. **Optioneel:** om op te geven van de fiatteur die gemachtigd zijn om goed te keuren toegang tot deze toepassing, klikt u op de kiezer naast het label **die is toegestaan toegang tot deze toepassing goedkeuren?** maximaal 10 afzonderlijke selecteren fiatteur.

  >[!NOTE]
  >Groepen worden niet ondersteund.
  >
  >

13. **Optioneel:** **voor toepassingen waardoor rollen worden blootgesteld**, als u wilt Self-service goedgekeurde gebruikers toewijzen aan een rol, klik op de selector naast de **aan welke rol moeten gebruikers worden toegewezen in deze toepassing?** Selecteer de rol die deze gebruikers moeten worden toegewezen.

14. Klik op de **opslaan** knop aan de bovenkant van het deelvenster om te voltooien.

Nadat u Self-servicetoepassingen configuratie is voltooid, ernaartoe kunnen navigeren hun [Toegangsvenster](https://myapps.microsoft.com/) en klikt u op de **+ toevoegen** knop om de apps waarop u selfservice hebt ingeschakeld te vinden de toegang. Fiatteur ziet ook een melding in hun [Toegangsvenster](https://myapps.microsoft.com/). U kunt een melding wanneer een gebruiker heeft toegang tot een toepassing waarvoor goedkeuring aangevraagd e-mail inschakelen. 

Deze goedkeuringen ondersteunen slechts één goedkeuringswerkstromen wat betekent dat als u meerdere goedkeurders opgeeft, kan alle één goedkeurder goedkeurder toegang tot de toepassing.

## <a name="next-steps"></a>Volgende stappen
[Geef single sign-on bij uw apps met Application Proxy](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)

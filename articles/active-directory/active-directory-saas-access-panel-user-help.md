---
title: Wilt u hulp nodig bij de portal voor mijn Apps in Azure Active Directory | Microsoft Docs
description: Instructies voor het uitvoeren van algemene taken bij het werken met het toegangsvenster ophalen.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: c67cd675-b567-41e1-8bc2-e06fe0b38d3b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: japere
ms.openlocfilehash: 7a7a5d04c55adc33db5ccce761efd622935acefb
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2018
---
# <a name="do-you-need-help-with-the-my-apps-portal"></a>Moet u helpen bij de portal voor mijn Apps?

U hebt deze pagina waarschijnlijk bereikt omdat u helaas in een probleem uitvoert wanneer u de portal mijn Apps. Hoewel er gevallen moet u contact op met de helpdesk of uw beheerder om een probleem is opgelost, vindt hier u enkele onderwerpen voor probleemoplossing die kan helpen eerst mogelijk.

## <a name="i-am-having-trouble-signing-into-the-my-apps-portal"></a>Ik ondervind problemen bij het aanmelden bij de portal mijn Apps

Algemene problemen om te controleren:

- Controleer als u bij de juiste URL aanmeldt zich: [https://myapps.microsoft.com](https://myapps.microsoft.com)

- Probeer de URL van uw browser vertrouwde sites toevoegen.

- Zorg ervoor dat uw wachtwoord niet is verlopen of is vergeten. Controleer [hier](active-directory-passwords-update-your-own-password.md) voor meer informatie over hoe u uw wachtwoord bij te werken.

- Controleer of uw contactgegevens bij verificatie is up-to-date is en de toegang niet blokkeert. Controleer [hier](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/multi-factor-authentication-end-user) voor meer informatie over het instellen van de verificatie voor je account.

- Cookies van uw browser te wissen en probeer het vervolgens opnieuw aan te melden.

Als u nog steeds er problemen zijn tijdens het aanmelden, neem voor verdere assistentie contact op met uw beheerder.


## <a name="how-do-i-update-my-password"></a>Hoe kan ik mijn wachtwoord bijwerken?

Als u uw wachtwoord bent vergeten, nooit ontvangen van uw IT-personeel, is vergrendeld aan uw account of wilt wijzigen, Zie [hulp nodig hebt, ik ben mijn Azure AD-wachtwoord vergeten](active-directory-passwords-update-your-own-password.md) voor meer informatie.

## <a name="how-do-i-register-for-password-reset"></a>Hoe kan ik registreren voor wachtwoordherstel

Als een eindgebruiker, kunt u uw wachtwoord opnieuw instellen of uw account ontgrendelen zonder contact op met de selfservice voor wachtwoordherstel (SSPR) met behulp van een persoon. Voordat u deze functie kunt gebruiken, moet u verificatiemethoden registreren of de vooraf gedefinieerde verificatiemethoden bevestigen die uw beheerder heeft ingevuld. Zie voor meer informatie [registreren voor selfservice voor wachtwoordherstel](active-directory-passwords-reset-register.md).


## <a name="i-am-having-trouble-installing-the-my-apps-secure-sign-in-extension"></a>Ik ondervind problemen bij het installeren van de extensie mijn Apps beveiligen aanmelden

Controleer of u voldoet aan de vereisten voor browsers:

- De portal is een browser die JavaScript ondersteunt vereist en CSS is ingeschakeld. Als u op basis van wachtwoorden eenmalige aanmelding apps gebruikt, moet u ook de bijbehorende uitbreiding geïnstalleerd. Deze extensie wordt automatisch gedownload wanneer u een toepassing die is geconfigureerd voor op basis van wachtwoorden eenmalige aanmelding apps starten.

- De browservereisten voor de uitbreiding zijn:
    - Rand verjaardagseditie van Windows 10 of hoger
    - Chrome op Windows 7 of hoger, en op Mac OS X of hoger
    - Firefox 26,0 op Windows XP SP2 of hoger, en op Mac OS X 10.6 of hoger
    - Internet Explorer 8, 9, 10, 11 op Windows 7 of hoger (beperkte ondersteuning)

U kunt ook de uitbreiding voor Chrome en rand downloaden via de rechtstreekse koppelingen hieronder:

- [Chrome-uitbreiding](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

- [Edge-uitbreiding](https://www.microsoft.com/store/apps/9pc9sckkzk84)

Nadat de installatie de volgende stappen uit als er problemen zijn:

- Controleer in de instellingen van uw browser extensie dat de extensie is ingeschakeld.

- Start uw browser en meld u aan bij de portal mijn Apps.

- Cookies van uw browser te wissen en meld u aan bij de portal mijn Apps.
- Ga als volgt de [oplossen met de extensie van het Configuratiescherm toegang voor Internet Explorer](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-ie-troubleshooting) handleiding voor toegang voor een hulpprogramma voor diagnostische gegevens en stapsgewijze instructies over het configureren van de extensie voor IE.

## <a name="how-do-i-use-the-my-apps-secure-sign-in-extension"></a>Hoe gebruik ik de extensie mijn Apps beveiligen aanmelden?
Het wijzigen van de standaard-URL voor mijn Apps voor de extensie

Als u van een andere mijn Apps URL dan https://myapps.microsoft.com gebruikmaakt vervolgens moet u uw standaard-URL echter de volgende stappen uit:
1. Terwijl niet aangemeld bij de extensie **Klik met de rechtermuisknop** het extensie-pictogram.
2. Klik op **URL voor mijn Apps selecteren** in het menu.
3. **Selecteer** uw standaard-URL.
4. Klik op het pictogram extensie.
5. Aanmelden bij de extensie door te selecteren **melden aan de slag**.

Meld u aan rechtstreeks in een app van de browser
1. Na de installatie van de extensie, aanmelding bij de extensie door te selecteren **melden aan de slag**.
2. Navigeer naar de **aanmelding URL** van de app die u aanmelden wilt bij dit wordt meestal de URL van de app die wordt weergegeven het aanmeldingsformulier.
3. De extensie moet een statuswijziging en kunt u dat een wachtwoord verkrijgbaar is, klikt u op de **extensie pictogram** aan te melden

Een app van de extensie openen
1. Na de installatie van de extensie, aanmelding bij de extensie door te selecteren **melden aan de slag**.
2. Klik op het pictogram extensie om het menu te openen.
3. **Search** voor een app in de portal voor mijn Apps beschikbaar.
4. Klik op de app vanuit de **zoekresultaten** zodat deze toepassing.
5. De laatste drie apps gestart wordt ook weergegeven in de **recent gebruikte** snelkoppeling lijst

> [!NOTE]
> Deze opties zijn alleen beschikbaar voor de rand, Chrome, Firefox.

## <a name="how-do-i-add-a-new-app"></a>Hoe kan ik een nieuwe app toevoegen

1.  Op de **Apps** pagina, klikt u op **App toevoegen**.

2.  De app die u wilt toevoegen en klik vervolgens op Zoeken **toevoegen**.

**Opmerkingen:**

- U hebt alleen toegang tot deze optie als de beheerder deze heeft ingeschakeld voor uw account.

- Als de app heeft een machtiging nodig, moet u mogelijk wachten op goedkeuring van de beheerder.


## <a name="how-do-i-manage-my-group-memberships"></a>Hoe beheer ik mijn groepslidmaatschappen?

1. Klik op de **groepen** tegel. 
2. Als u wilt maken van een groep, onder de groepen die ik eigenaar bent, klikt u op **groep maken**, en volg de instructies.
3. Als u wilt deelnemen aan een groep, onder de groepen die ik in, klikt u op **lid worden van groep**, en volg de instructies.

**Opmerkingen:**

- U hebt alleen toegang tot deze optie als de beheerder deze heeft ingeschakeld voor uw account.

- U lid van zijn groepen kunnen u details weergeven en zorgt dat de groep.

- Kunt u meer informatie weergeven groepen waarvoor u een eigenaar van het toevoegen of verwijderen van leden en zorgt dat de groep.


## <a name="next-steps"></a>Volgende stappen

Zie voor informatie over het oplossen van problemen [problemen bij het gebruik van de toepassing toegang Configuratiescherm website of mobiele toepassingen](active-directory-application-access-panel-content-map.md)


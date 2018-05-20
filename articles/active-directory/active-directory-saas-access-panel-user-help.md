---
title: Hulp bij het openen en gebruiken van de portal voor mijn Apps in Azure Active Directory | Microsoft Docs
description: Hulp bij het aanmelden bij en veelvoorkomende taken uitvoeren in het deelvenster toegang.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: c67cd675-b567-41e1-8bc2-e06fe0b38d3b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: japere
ms.openlocfilehash: f0604007255d829cab0502e93895ca541da3b93a
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="troubleshoot-issues-with-accessing-and-using-the-my-apps-portal"></a>Problemen oplossen met toegang tot en met behulp van de portal mijn Apps

Als u problemen met aanmelden bij of met behulp van de portal voor mijn Apps ondervindt, kunt u deze tips voor het oplossen voordat u contact opneemt met de helpdesk of beheerder voor hulp.

## <a name="i-am-having-trouble-signing-into-the-my-apps-portal"></a>Ik ondervind problemen bij het aanmelden bij de portal mijn Apps

Probeer deze algemene tips:

- Eerst moet controleren om te zien of u van de juiste URL gebruikmaakt [ https://myapps.microsoft.com ](https://myapps.microsoft.com).
- Probeer de URL van uw browser vertrouwde sites toevoegen.
- Zorg ervoor dat uw wachtwoord juist is en niet is verlopen. Zie voor meer informatie [uw werk of school-wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).
- Controleren om ervoor te zorgen dat uw contactgegevens verificatie up-to-date is en de toegang niet blokkeert. Zie voor meer informatie [wat Azure multi-factor Authentication betekent voor mij?](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/multi-factor-authentication-end-user).
- Cookies van uw browser te wissen en probeer het vervolgens opnieuw aan te melden.

Als u nog steeds er problemen zijn tijdens het aanmelden, moet u contact op met uw beheerder.


## <a name="i-seem-to-be-having-password-issues"></a>Ik lijken te ondervindt problemen met wachtwoord

Als u uw wachtwoord bent vergeten, nooit van uw IT-personeel ontvangen, uw account zijn vergrendeld of uw wachtwoord wilt wijzigen, Zie [hulp nodig hebt, ik ben mijn Azure AD-wachtwoord vergeten](active-directory-passwords-update-your-own-password.md).

## <a name="i-need-to-register-for-password-reset"></a>Ik moet registreren voor wachtwoordherstel

U kunt uw wachtwoord opnieuw instellen of uw account ontgrendelen zonder contact op met iemand met behulp van de selfservice voor wachtwoordherstel (SSPR). Voordat u deze functionaliteit gebruiken kunt, moet u uw verificatiemethoden registreren of de vooraf gedefinieerde verificatiemethoden die uw beheerder moet bevestigen. Zie voor meer informatie [registreren voor selfservice voor wachtwoordherstel](active-directory-passwords-reset-register.md).


## <a name="i-am-having-trouble-installing-the-my-apps-secure-sign-in-extension"></a>Ik ondervind problemen bij het installeren van de extensie mijn Apps beveiligen aanmelden

De portal voor mijn Apps vereist een browser die JavaScript ondersteunt en CSS is ingeschakeld. Als u op basis van wachtwoorden eenmalige aanmelding apps gebruikt, moet u ook de bijbehorende uitbreiding geïnstalleerd. Deze extensie wordt automatisch gedownload wanneer u een toepassing die is geconfigureerd voor op basis van wachtwoorden eenmalige aanmelding apps start.

Controleren om ervoor te zorgen dat u voldoet aan de volgende browservereisten:
- **Rand**: verjaardagseditie van Windows 10 of hoger.
- **Chrome**: Windows 7 of hoger, en op Mac OS X of hoger.
- **Firefox 26,0 of hoger**: Windows XP SP2 of hoger, en op Mac OS X 10.6 of hoger.
- **Internet Explorer 11**: op Windows 7 of hoger (beperkte ondersteuning).

U kunt ook de uitbreiding downloaden rechtstreeks vanuit de volgende sites:

- [Chrome](https://go.microsoft.com/fwlink/?linkid=866367)
- [Rand](https://go.microsoft.com/fwlink/?linkid=845176)
- [Firefox](https://go.microsoft.com/fwlink/?linkid=866366)

Als u de uitbreiding hebt geïnstalleerd en nog steeds problemen ondervindt, probeert u het volgende:

- Controleer uw browserinstellingen extensie om ervoor te zorgen dat de extensie is ingeschakeld.
- Start uw browser en meld u aan bij de portal mijn Apps.
- Cookies van uw browser te wissen en meld u aan bij de portal mijn Apps.
- Zie voor toegang tot een hulpprogramma voor diagnostische gegevens en stapsgewijze instructies over het configureren van de uitbreiding voor Internet Explorer, [oplossen met de extensie van het Configuratiescherm toegang voor Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-troubleshooting).

## <a name="use-the-my-apps-secure-sign-in-extension"></a>Gebruik de mijn Apps beveiligen aanmelden extensie
* Als u een URL voor mijn Apps dan `https://myapps.microsoft.com`, standaard-URL configureren door het volgende te doen:
   1. Terwijl u bent *niet* aangemeld bij de extensie, met de rechtermuisknop op het pictogram extensie.
   2. Selecteer in het menu **URL voor mijn Apps**.
   3. Selecteer uw standaard-URL.
   4. Selecteer het pictogram extensie.
   5. Als u wilt aanmelden bij de extensie, selecteer **melden aan de slag**.

* Om aan te melden rechtstreeks in een app van de browser, het volgende doen:
   1. Nadat u de uitbreiding hebt geïnstalleerd, zich aanmelden bij het selecteren van **melden aan de slag**.
   2. Aanmelden bij de app met de aanmeldings-URL.  
       De aanmeldings-URL is meestal de URL van de app die het formulier aanmeldingspagina wordt weergegeven.
      De extensie moet een statuswijziging en laat u weten dat een wachtwoord beschikbaar is.
   3. Als u wilt aanmelden, selecteer het extensie-pictogram.

* Als u wilt een app van de extensie openen, het volgende doen:
   1. Nadat u de uitbreiding hebt geïnstalleerd, zich aanmelden bij het selecteren van **melden aan de slag**.
   2. Selecteer het pictogram extensie om het menu te openen.
   3. Zoeken naar een app die beschikbaar is in de portal voor mijn Apps.
   4. Selecteer de app in de lijst met zoekresultaten.  
       De laatste drie apps die u hebt gebruikt, worden weergegeven in de **recent gebruikte** snelkoppeling lijst.

> [!NOTE]
> Deze opties zijn alleen beschikbaar voor rand en Chrome, Firefox.

## <a name="how-do-i-add-a-new-app"></a>Hoe kan ik een nieuwe app toevoegen

1.  Op de **Apps** pagina **App toevoegen**.
2.  Zoeken naar de app die u wilt toevoegen en selecteer vervolgens **toevoegen**.

   > [!NOTE]
   > * Alleen als de beheerder deze heeft ingeschakeld voor uw account, kunt u deze optie openen.
   > * Als de app heeft een machtiging nodig, moet u mogelijk wachten op goedkeuring van de beheerder.
   > 

## <a name="how-do-i-manage-my-group-memberships"></a>Hoe beheer ik mijn groepslidmaatschappen?

Selecteer de **groepen** tegel en voert u een van de volgende: 
* Een groep te maken, klikt u onder **groepen ik eigenaar**, selecteer **groep maken**, en volg de instructies.
* Lid worden van een groep onder **groepen in de**, selecteer **lid worden van groep**, en volg de instructies.

   > [!NOTE]
   > * Alleen als de beheerder deze heeft ingeschakeld voor uw account, kunt u deze optie openen.
   > * Als u lid zijn van een groep, kunt u details weergeven en zorgt dat de groep.
   > * Als u een eigenaar van een groep, kunt u details weergeven, toevoegen of verwijderen van leden en zorgt dat de groep.
   >


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over probleemoplossing [problemen bij het gebruik van de toepassing toegang Configuratiescherm website of mobiele toepassing](active-directory-application-access-panel-content-map.md).


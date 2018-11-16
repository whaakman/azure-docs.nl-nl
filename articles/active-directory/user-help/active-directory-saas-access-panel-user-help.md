---
title: Hulp bij het verkrijgen van toegang tot en met behulp van de MyApps-portal in Azure Active Directory | Microsoft Docs
description: Hulp bij het aanmelden bij en het uitvoeren van algemene taken in het toegangsvenster.
services: active-directory
author: eross-msft
manager: mtillman
ms.assetid: c67cd675-b567-41e1-8bc2-e06fe0b38d3b
ms.service: active-directory
ms.component: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: lizross
ms.reviewer: japere
ms.openlocfilehash: b4229ca7ff96a9806ac82cf9452de496d858f6b4
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51705681"
---
# <a name="troubleshoot-issues-with-accessing-and-using-the-myapps-portal"></a>Problemen oplossen met toegang tot en met behulp van de MyApps-portal

Als u problemen ondervindt met aanmelden bij of via de MyApps-portal, kunt u deze tips voor probleemoplossing voordat u contact opnemen met helpdesk of beheerder voor hulp.

## <a name="i-am-having-trouble-signing-into-the-myapps-portal"></a>Ik ondervind problemen bij het aanmelden bij de MyApps-portal

Probeer de volgende algemene tips:

- Eerste, Controleer om te zien of u de juiste URL gebruiken [ https://myapps.microsoft.com ](https://myapps.microsoft.com).
- Probeer toe te voegen van de URL van uw browser vertrouwde sites.
- Zorg ervoor dat uw wachtwoord juist is en niet is verlopen. Zie voor meer informatie, [uw werk- of schoolaccount wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).
- Controleren om ervoor te zorgen dat uw contactgegevens voor verificatie is aan de datum en de toegang niet blokkeert. Zie voor meer informatie, [wat Azure multi-factor Authentication betekent voor mij?](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/multi-factor-authentication-end-user).
- Wist u cookies van uw browser en probeer het vervolgens opnieuw aanmelden.

Als u nog steeds problemen regelmatig tijdens het aanmelden, moet u contact op met uw beheerder.

## <a name="i-seem-to-be-having-password-issues"></a>Ik lijkt het alsof ondervindt wachtwoordproblemen met

Als u uw wachtwoord bent vergeten, nooit van uw IT-personeel ontvangen, toegang tot uw account worden geblokkeerd of uw wachtwoord wilt wijzigen, raadpleegt u [Help, ik ben mijn wachtwoord voor Azure AD vergeten](active-directory-passwords-update-your-own-password.md).

## <a name="i-need-to-register-for-password-reset"></a>Moet ik me registreren voor wachtwoord opnieuw instellen

U kunt uw wachtwoord opnieuw instellen of uw account ontgrendelen zonder iemand anders via selfservice voor wachtwoordherstel (SSPR). Voordat u deze functie gebruiken kunt, moet u uw verificatiemethoden registreren of bevestigen van de vooraf gedefinieerde verificatiemethoden die uw beheerder vereist. Zie voor meer informatie, [registreren voor self-service voor wachtwoord opnieuw instellen](active-directory-passwords-reset-register.md).

## <a name="i-am-having-trouble-installing-the-my-apps-secure-sign-in-extension"></a>Ik ondervind problemen bij het installeren van de extensie mijn Apps beveiligen aanmelden

De MyApps-portal vereist een browser die ondersteuning biedt voor JavaScript en CSS is ingeschakeld. Als u wachtwoord-apps op basis van eenmalige aanmelding gebruikt, moet ook de begeleidende extensie worden geïnstalleerd. Deze extensie wordt automatisch gedownload wanneer u een toepassing die is geconfigureerd voor wachtwoord-apps op basis van eenmalige aanmelding te starten.

Controleren om ervoor te zorgen dat u voldoet aan de volgende browservereisten:

- **Edge**: Windows 10 Anniversary Edition of hoger.
- **Chrome**: op Windows 7 of hoger, en in Mac OS X of hoger.
- **Firefox 26,0 of hoger**: in Windows XP SP2 of hoger, en op Mac OS X 10.6 of hoger.
- **Internet Explorer 11**: op Windows 7 of hoger (beperkte ondersteuning).

U kunt ook de extensie downloaden rechtstreeks vanuit de volgende sites:

- [Chrome](https://go.microsoft.com/fwlink/?linkid=866367)
- [Microsoft Edge](https://go.microsoft.com/fwlink/?linkid=845176)
- [Firefox](https://go.microsoft.com/fwlink/?linkid=866366)

Als u de uitbreiding hebt geïnstalleerd en nog steeds problemen ondervindt, probeert u het volgende:

- Controleer de instellingen van uw browser-extensie om ervoor te zorgen dat de uitbreiding is ingeschakeld.
- Start uw browser en meld u aan bij de MyApps-portal.
- Schakel cookies van uw browser en meld u aan bij de MyApps-portal.
- Zie voor toegang tot een hulpprogramma voor diagnostische gegevens en stapsgewijze instructies over het configureren van de extensie voor Internet Explorer, [oplossen met de extensie van het Configuratiescherm toegang voor Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-troubleshooting).

## <a name="use-the-my-apps-secure-sign-in-extension"></a>Gebruik de mijn Apps beveiligde aanmelding-extensie
* Als u een URL voor mijn Apps dan `https://myapps.microsoft.com`, de standaard-URL configureren door het volgende te doen:
   1. Terwijl u bent *niet* aangemeld bij de extensie, met de rechtermuisknop op het pictogram van de extensie.
   2. Selecteer in het menu **URL voor mijn Apps**.
   3. Selecteer de standaard-URL.
   4. Selecteer het pictogram van de extensie.
   5. Als u wilt aanmelden bij de extensie, selecteer **Meld u aan de slag**.

* Om te melden bij rechtstreeks in een app vanuit de browser, het volgende doen:
   1. Nadat u de uitbreiding hebt geïnstalleerd, aanmelden bij het selecteren van **Meld u aan de slag**.
   2. Meld u aan de app met de aanmeldings-URL.  
       De aanmeldings-URL is meestal de URL van de app die wordt weergegeven het aanmeldingsformulier.
      De extensie moet een statuswijziging en laat u weten dat een wachtwoord beschikbaar is.
   3. Om aan te melden, selecteert u het pictogram van de extensie.

* Als u wilt starten een app uit de extensie, het volgende doen:
   1. Nadat u de uitbreiding hebt geïnstalleerd, aanmelden bij het selecteren van **Meld u aan de slag**.
   2. Selecteer het pictogram van de extensie om het menu te openen.
   3. Zoeken naar een app die beschikbaar is in de MyApps-portal.
   4. Selecteer de app in de lijst met zoekresultaten.  
       De laatste drie apps die u hebt gebruikt, worden weergegeven in de **onlangs gebruikte** snelkoppeling naar de lijst.

> [!NOTE]
> Deze opties zijn alleen beschikbaar voor Microsoft Edge, Chrome en Firefox.

## <a name="how-do-i-add-a-new-app"></a>Hoe kan ik een nieuwe app toevoegen

1.  Op de **Apps** weergeeft, schakelt **App toevoegen**.
2.  Zoek naar de app die u wilt toevoegen en selecteer vervolgens **toevoegen**.

   > [!NOTE]
   > * U kunt toegang krijgen tot deze optie alleen als de beheerder deze heeft ingeschakeld voor uw account.
   > * Als de app is een machtiging vereist, moet u mogelijk wachten op goedkeuring van de beheerder.

## <a name="how-do-i-manage-my-group-memberships"></a>Hoe kan ik mijn groepslidmaatschappen beheren?

Selecteer de **groepen** tegel en vervolgens een van de volgende handelingen:
* Een groep te maken, onder **mijn groepen**, selecteer **groep maken**, en volg de instructies.
* Voor deelname aan een groep, onder **groepen waarin ik zit**, selecteer **lid worden van groep**, en volg de instructies.

   > [!NOTE]
   > * U kunt toegang krijgen tot deze optie alleen als de beheerder deze heeft ingeschakeld voor uw account.
   > * Als u een lid van een groep bent, kunt u details weergeven en zorgt dat de groep.
   > * Als u een eigenaar van een groep bent, kunt u details weergeven, toevoegen of verwijderen van leden en zorgt dat de groep.
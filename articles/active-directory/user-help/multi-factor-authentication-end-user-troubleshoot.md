---
title: Verificatie - Azure AD oplossen | Microsoft Docs
description: Dit document bevatten gebruikers informatie over wat te doen als ze een probleem met Azure multi-factor Authentication ondervindt.
services: multi-factor-authentication
keywords: Multi-factor authentication-client, verificatieprobleem, correlatie-ID
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.assetid: 8f3aef42-7f66-4656-a7cd-d25a971cb9eb
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/06/2017
ms.author: lizross
ms.reviewer: richagi
ms.custom: end-user
ms.openlocfilehash: deb75c2601fa55f7cdb1681d8f73e94d6b01310a
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39060073"
---
# <a name="get-help-with-two-step-verification"></a>Help-informatie weergeven bij verificatie in twee stappen
In dit artikel vindt u antwoorden op de meest voorkomende vragen over verificatie in twee stappen.

## <a name="why-do-i-have-to-perform-two-step-verification-can-i-turn-it-off"></a>Waarom heb ik om uit te voeren van verificatie in twee stappen? Kan ik deze uitschakelen inschakelen?

Verificatie in twee stappen is een beveiligingsfunctie die uw organisatie hebt gekozen voor het beveiligen van uw accounts. Het is veiliger dan alleen met een wachtwoord, omdat deze is gebaseerd op twee soorten verificatie: iets dat u weet en iets dat u hebt met u. Er is iets u weet dat uw wachtwoord. Iets met u is een telefoon of een apparaat dat u doorgaans bij u hebben. Wanneer uw account is beveiligd met verificatie in twee stappen, dit betekent dat dat een kwaadwillende persoon zich niet als u als ze uw wachtwoord op omdat ze geen toegang tot uw telefoon te hebben.

Microsoft biedt verificatie in twee stappen, maar uw organisatie, kiest u de functie. U kunt geen zich afmelden als het ondersteuningsteam van uw bedrijf van u, vereist net zoals u kunnen zich niet met een wachtwoord afmelden voor de beveiliging van uw account.

Als u verificatie in twee stappen ingeschakeld voor uw persoonlijke Microsoft-account hebt en wilt de instellingen wijzigen, lezen [over verificatie in twee stappen](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) in plaats daarvan.

## <a name="i-dont-have-my-phone-with-me-today"></a>Ik heb mijn telefoon met mij vandaag nog

Een aantal dagen dat u uw telefoon bij u thuis, maar nog steeds laat moeten zich aanmelden op het werk. Allereerst die moet u proberen is het aanmelden met een andere verificatiemethode. Bij de registratie voor verificatie in twee stappen, u meer dan één telefoonnummer instellen? Als u wilt proberen zich aanmeldt met een andere methode, de volgende stappen uit:

1. Meld u zoals u gewend bent.
2. Wanneer de pagina van de verificatie in twee stappen wordt geopend, kiest u **gebruiken een andere verificatieoptie**.

   ![Andere verificatie](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

3. Selecteer de verificatiemethode die u wilt gebruiken.
4. Ga door met verificatie in twee stappen.

Als er geen de **gebruiken een andere verificatieoptie** koppelen, en dit betekent u geen alternatieve methoden instellen dat bij de eerste registratie voor verificatie in twee stappen. Neem contact op met de bedrijfsondersteuning voor hulp bij het aanmelden bij uw account. Nadat u bent aangemeld, zorg ervoor dat u [uw instellingen beheren](multi-factor-authentication-end-user-manage-settings.md) om toe te voegen extra verificatiemethoden voor later.

Als u ziet de **gebruiken een andere verificatieoptie** koppeling, maar u hebt geen toegang tot uw alternatieve methoden beide, neem contact op met de bedrijfsondersteuning voor hulp bij het aanmelden bij uw account.

## <a name="i-lost-my-phone-or-got-a-new-number"></a>Kan ik mijn telefoon verlies of hebt u een nieuw getal
Er zijn twee manieren om terug te gaan op uw account. De eerste is aan te melden met uw telefoonnummer alternatieve verificatie als u een hebt ingesteld. De tweede is om te vragen van het ondersteuningsteam van uw bedrijf om uw instellingen te wissen.

Als uw telefoon is verloren of gestolen is, wordt ook aangeraden dat u instellen uw bedrijf ondersteuning dat, zodat ze kunnen uw appwachtwoorden opnieuw instellen en wis eventuele onthouden apparaten.

### <a name="use-an-alternate-phone-number"></a>Een alternatief telefoonnummer gebruiken
Als u meerdere opties voor verificatie, met inbegrip van een tweede telefoonnummer of een verificator-app op een ander apparaat hebt ingesteld kunt u een van deze zich aanmeldt.

Om aan te melden met behulp van het alternatieve telefoonnummer, de volgende stappen uit:

1. Meld u zoals u gewend bent.
2. Wanneer u hierom wordt gevraagd verder uw account te verifiëren, kiest u **gebruiken een andere verificatieoptie**.

   ![Andere verificatie](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

3. Selecteer het telefoonnummer of een apparaat dat u toegang tot hebt.
4. Nadat u weer terug in uw account bent [uw instellingen beheren](multi-factor-authentication-end-user-manage-settings.md) om het telefoonnummer van uw verificatie te wijzigen.

### <a name="clear-your-settings"></a>Uw instellingen wissen
Als u een telefoonnummer voor secundaire verificatie niet hebt geconfigureerd, moet u contact op met uw bedrijf voor hulp. Ze duidelijk hebben de instellingen, zodat de volgende keer dat u zich aanmelden, wordt u gevraagd naar [registreren voor verificatie in twee stappen](multi-factor-authentication-end-user-first-time.md) opnieuw.

## <a name="i-am-not-receiving-a-text-or-call-on-my-phone"></a>Ik ben niet ontvangen van een SMS-bericht of neem contact op mijn telefoon
Er zijn diverse redenen waarom u wilt mogelijk aanmelden, maar niet de tekst- of telefonische oproep te ontvangen. Als u met succes teksten of telefoontjes naar uw telefoon in het verleden ontvangen hebt, is dit waarschijnlijk een probleem met de provider van de telefoon, niet op uw account. Zorg ervoor dat u goed cel signaal hebt en als u probeert te ontvangen van een SMS-bericht Zorg ervoor dat u zich kunt SMS-berichten ontvangen. Vraag een vriend te bellen u of sturen u als een test.

Als u enkele minuten een tekstbericht of telefoongesprek hebt gewacht, is de snelste manier om toegang te krijgen tot uw account om te proberen een andere optie.

1. Selecteer **gebruiken een andere verificatieoptie** op de pagina waarop wordt gewacht tot de verificatie.

    ![Andere verificatie](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)
2. Selecteer de telefoon-nummer of delivery-methode die u wilt gebruiken.

    Als u meerdere verificatiecodes ontvangen, gebruikt u de nieuwste versie.

Als u een andere methode geconfigureerd hebt, neem contact op met het ondersteuningsteam van uw bedrijf en vraag deze om uw instellingen te wissen. De volgende keer dat u zich aanmeldt, wordt u gevraagd naar [instellen van multi-factor authentication](multi-factor-authentication-end-user-first-time.md) opnieuw.

Als u vaak vertragingen vanwege ongeldige cel signaal hebt, raden wij aan u de [Microsoft Authenticator-app](microsoft-authenticator-app-how-to.md) op uw smartphone. De app kunt genereren willekeurige beveiligingscodes die u gebruikt om te melden en deze codes elke cel signaal of internet verbinding vereisen.

## <a name="app-passwords-are-not-working"></a>App-wachtwoorden werken niet
Controleer eerst of u het app-wachtwoord juist hebt ingevoerd. De gegenereerde app-wachtwoord wordt vervangen door uw normale wachtwoord, maar alleen voor oudere bureaubladtoepassingen die verificatie in twee stappen niet ondersteunen. Als deze nog steeds niet werkt, probeer het aanmelden en [maken van een nieuw app-wachtwoord](multi-factor-authentication-end-user-app-passwords.md).  Als dit nog steeds niet werkt, neem contact op met het ondersteuningsteam van uw bedrijf en deze [je bestaande app-wachtwoorden verwijderen](../authentication/howto-mfa-userdevicesettings.md) en vervolgens kunt u een nieuwe maken.

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Kan ik vinden een antwoord op mijn probleem niet.
Als u deze stappen voor probleemoplossing hebt geprobeerd maar nog steeds actief in problemen met, neem dan contact op met het ondersteuningsteam van uw bedrijf. Ze zou het mogelijk om u te helpen.

## <a name="related-topics"></a>Verwante onderwerpen
* [Uw instellingen beheren voor verificatie in twee stappen](multi-factor-authentication-end-user-manage-settings.md)  
* [Veelgestelde vragen over Microsoft Authenticator-toepassing](microsoft-authenticator-app-faq.md)

---
title: Verificatie - Azure AD oplossen | Microsoft Docs
description: Bevat instructies voor gebruikers over wat te doen als ze worden uitgevoerd in een probleem met Azure multi-factor Authentication en verificatie in twee stappen.
services: multi-factor-authentication
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.assetid: 8f3aef42-7f66-4656-a7cd-d25a971cb9eb
ms.service: multi-factor-authentication
ms.workload: identity
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: lizross
ms.reviewer: kexia
ms.openlocfilehash: ec79adcaf7cbef778b8c9e572ae932d5ceb47aa6
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090374"
---
# <a name="get-help-with-two-step-verification"></a>Help-informatie weergeven bij verificatie in twee stappen

Verificatie in twee stappen is een beveiligingsfunctie die uw organisatie worden gebruikt voor het beveiligen van uw accounts. Verificatie in twee stappen is veiliger dan alleen met een wachtwoord, omdat deze is gebaseerd op twee soorten verificatie: iets dat u weet en iets dat u hebt met u. De iets dat die u weet is uw wachtwoord, terwijl de iets dat die u bij u hebben uw telefoon of een apparaat is. Met behulp van verificatie in twee stappen kan helpen om te stoppen schadelijke hackers aanmelden als u, zelfs als ze uw wachtwoord.

Microsoft biedt verificatie in twee stappen, is uw organisatie die bepaalt of u de functie gebruiken. U kunt geen zich afmelden als uw organisatie vereist, net zoals u kunnen zich niet met een wachtwoord afmelden voor de beveiliging van uw account.

>[!Note]
>Als u meer informatie over het gebruik van verificatie in twee stappen aan uw persoonlijke Microsoft-account zoekt, raadpleegt u de [over verificatie in twee stappen](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) artikel.

## <a name="why-do-i-need-to-use-another-verification-method"></a>Waarom heb ik nodig om een andere verificatiemethode te gebruiken?

Er zijn diverse redenen waarom u moet mogelijk een andere verificatiemethode gebruiken met uw account. Bijvoorbeeld:

- **U bent vergeten uw telefoon of het apparaat.** Een aantal dagen dat u uw telefoon bij u thuis, maar nog steeds laat moeten zich aanmelden op het werk. Eerst moet u proberen aanmelden met een andere methode die uw telefoon niet nodig.

- **U uw telefoon verliest of hebt u een nieuw telefoonnummer.** Als u uw telefoon verloren bent of een nieuw getal verkregen, kunt u zich aanmeldt met een andere methode of vraag uw beheerder om uw instellingen te wissen. Het is raadzaam dat uw beheerder weten als uw telefoon is zoekgeraakt of gestolen, zodat de juiste updates kunnen worden gesteld aan uw account. Nadat u uw instellingen zijn uitgeschakeld, wordt u gevraagd naar [registreren voor verificatie in twee stappen](multi-factor-authentication-end-user-first-time.md) de volgende keer dat u zich aanmeldt.

- **Niet ophalen van de tekst van de verificatie of automatische oproep.** Er zijn diverse redenen waarom u mogelijk niet aan de tekst of automatische oproep. Als u teksten of telefoongesprekken worden gevoerd met succes in het verleden geworden hebt, maar dit is waarschijnlijk een probleem met de provider van de telefoon, niet op uw account. Als u vaak vertragingen vanwege een ongeldige signaal hebt, raden wij aan u de [Microsoft Authenticator-app](microsoft-authenticator-app-how-to.md) op uw mobiele apparaat. Deze app kunt willekeurige beveiligingscodes genereren voor aanmelden, zonder eventuele cel signaal of de verbinding met Internet.<br><br>Als u een SMS-bericht ontvangen probeert, vraagt u een vriend naar tekst u als een test om te controleren of u kunt een, en als u akkoord bent gegaan met verschillende berichten, zorg ervoor dat u de code van de meest recente versie gebruikt.

- **Uw app-wachtwoorden werken niet.** App-wachtwoorden vervangen door uw normale wachtwoord voor oudere bureaubladtoepassingen die verificatie in twee stappen niet ondersteunen. Controleer eerst of dat u het wachtwoord correct hebt ingevoerd. Als dat het niet oplossen, probeert u aan te melden bij [maken van een nieuw app-wachtwoord](multi-factor-authentication-end-user-app-passwords.md) of contact opnemen met uw beheerder om [je bestaande app-wachtwoorden verwijderen](../authentication/howto-mfa-userdevicesettings.md) zodat u kunt een nieuwe maken.

## <a name="sign-in-using-another-verification-method"></a>Meld u aan met behulp van een andere verificatiemethode

1. Meld u aan bij uw account normaal en kies de **Meld u aan een andere manier** koppelen op de **verificatie in twee stappen** pagina.

    ![Meld u in de verificatiemethode wijzigen](./media/multi-factor-authentication-end-user-troubleshoot/two-factor-auth-signin-another-way.png)

    >[!Note]
    >Als er geen de **Meld u aan een andere manier** koppeling, betekent dit dat u andere verificatiemethoden hebt ingesteld. Hebt u aan contact op met uw beheerder voor hulp bij het aanmelden bij uw account. Nadat u zich hebt aangemeld, zorg er dan voor dat u extra verificatiemethoden toevoegen. Zie voor meer informatie over het toevoegen van verificatiemethoden de [uw instellingen beheren voor verificatie in twee stappen](multi-factor-authentication-end-user-manage-settings.md) artikel.<br><br>Als u de koppeling wordt weergegeven, maar andere verificatiemethoden nog steeds niet ziet, hebt u contact op met uw beheerder voor hulp bij het aanmelden bij uw account.

2. Kies uw alternatieve verificatiemethode en doorgaan met het proces voor de verificatie in twee stappen.

3. Nadat u weer terug in uw account bent, kunt u uw verificatiemethoden bijwerken (indien nodig). Voor meer informatie over toevoegen of wijzigen van de methoden, Zie de [uw instellingen beheren voor verificatie in twee stappen](multi-factor-authentication-end-user-manage-settings.md) artikel.

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Kan ik vinden een antwoord op mijn probleem niet

Als u deze stappen hebt geprobeerd maar nog steeds actief in problemen met, neem dan contact op met uw beheerder voor meer informatie.

## <a name="related-topics"></a>Verwante onderwerpen

* [Uw instellingen beheren voor verificatie in twee stappen](multi-factor-authentication-end-user-manage-settings.md)

* [Veelgestelde vragen over Microsoft Authenticator-toepassing](microsoft-authenticator-app-faq.md)

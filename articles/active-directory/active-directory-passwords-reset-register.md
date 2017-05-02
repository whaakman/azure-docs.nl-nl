---
title: 'Azure AD: Registratie selfservice voor het opnieuw instellen van een wachtwoord | Microsoft Docs'
description: Verificatiegegevens voor selfservice voor het opnieuw instellen van een wachtwoord registreren
services: active-directory
keywords: Active Directory-wachtwoordbeheer, wachtwoordbeheer, selfservice voor het opnieuw instellen van een wachtwoord van Azure AD, selfservice voor het opnieuw instellen van een wachtwoord
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/16/2017
ms.author: joflore
ms.custom: end-user
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: c6d3d9d8f399816928e794e8956bc35825462fb9
ms.lasthandoff: 04/25/2017


---
# <a name="register-for-self-service-password-reset"></a>Registreren voor de selfservice voor het opnieuw instellen van een wachtwoord

Als uw beheerder dit heeft ingesteld, kunt u als eindgebruiker uw wachtwoord opnieuw instellen of uw account ontgrendelen zonder iemand anders te hoeven vragen door gebruik te maken van selfservice voor het opnieuw instellen van een wachtwoord (SSPR). Voordat u deze functie kunt gebruiken, moet u verificatiemethoden registreren of de vooraf gedefinieerde verificatiemethoden bevestigen die uw beheerder heeft ingevuld.

## <a name="register-or-confirm-authentication-data-with-sspr"></a>Verificatiegegevens registreren of bevestigen met SSPR

1. Open de webbrowser op uw apparaat en ga naar de [registratiepagina voor het opnieuw instellen van een wachtwoord](http://aka.ms/ssprsetup)
2. Voer uw gebruikersnaam en wachtwoord in die door uw beheerder zijn verstrekt
3. Afhankelijk van de opties die de beheerder heeft goedgekeurd, ziet u een of meer van de volgende items die u moet configureren of controleren voor gebruik als u uw wachtwoord opnieuw moet instellen
    * Telefoon werk: deze optie kan alleen worden ingesteld door de beheerder.
    * Telefoon voor authenticatie: deze optie moet worden ingesteld op een ander telefoonnummer waar u toegang toe hebt, zoals een mobiele telefoon. (De beheerder kan dit voor uw mobiele telefoonnummer instellen als de beheerder al uw toestemming heeft om die informatie te gebruiken.)
    * E-mailadres voor authenticatie: deze optie moet worden ingesteld op een alternatief e-mailadres waar u toegang toe hebt zonder dat het benodigde wachtwoord opnieuw moet worden ingesteld.
    * Beveiligingsvragen: deze optie biedt een lijst met door de beheerder goedgekeurde vragen die u moet beantwoorden. U kunt niet hetzelfde antwoord voor meer dan één vraag geven.
4. Geef en controleer de informatie die door de beheerder wordt vereist. Als er meer dan één optie beschikbaar is, is het raadzaam dat u ten behoeve van de flexibiliteit verschillende methoden registreert wanneer een bepaalde methode niet beschikbaar is (bijvoorbeeld wanneer u onderweg bent en geen toegang hebt tot uw werktelefoon)

    ![Registreer verificatiemethoden en klik op Voltooien][Register]

5. Als u na stap 4 de optie **Voltooien** kiest, kunt u de selfservice voor het opnieuw instellen van een wachtwoord gebruiken wanneer dat nodig is.

Als u gegevens in de telefoon voor authenticatie of het e-mailadres voor authenticatie invoert, zijn deze niet zichtbaar in de algemene map. De enige personen die deze gegevens kunnen zien, zijn u en uw beheerder. Alleen u kunt de antwoorden op uw beveiligingsvragen zien.

Beheerders kunnen eisen dat u uw verificatiemethoden na een bepaalde tijd bevestigt om er zeker van te zijn dat u nog steeds de juiste methoden hebt geregistreerd.

## <a name="next-steps"></a>Volgende stappen

* [Hoe u uw wachtwoord kunt wijzigen met de selfservice voor het opnieuw instellen van een wachtwoord](active-directory-passwords-update-your-own-password.md)
* [Registratiepagina voor het opnieuw instellen van een wachtwoord](http://aka.ms/ssprsetup)
* [Portal voor het opnieuw instellen van een wachtwoord](https://passwordreset.microsoftonline.com/)
* [Kunt u zich niet aanmelden bij uw Microsoft-account?](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)

[Register]: ./media/active-directory-passwords-reset-register/register-2-methods.png "Registratiepagina voor het opnieuw instellen van een wachtwoord met geregistreerde methoden en knop Voltooien"


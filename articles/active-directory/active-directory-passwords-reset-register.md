---
title: 'Azure AD: SSPR-registratie | Microsoft Docs'
description: Registreren verificatiegegevens voor self-service Azure AD-wachtwoord opnieuw instellen
services: active-directory
keywords: 
documentationcenter: 
author: barlanmsft
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: barlan
ms.custom: end-user
ms.openlocfilehash: b884f8bc3a20052fa0cb40772deef591b69d7b10
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="register-for-self-service-password-reset"></a>Registreren voor de selfservice voor het opnieuw instellen van een wachtwoord

> [!IMPORTANT]
> **Bent u hier terechtgekomen omdat u problemen ondervindt met het aanmelden?** Als dat het geval is, vindt u hier meer informatie over het [wijzigen en opnieuw instellen van uw eigen wachtwoord](active-directory-passwords-update-your-own-password.md).

Als een eindgebruiker, kunt u uw wachtwoord opnieuw instellen of uw account ontgrendelen zonder contact op met de selfservice voor wachtwoordherstel (SSPR) met behulp van een persoon. Voordat u deze functie kunt gebruiken, moet u verificatiemethoden registreren of de vooraf gedefinieerde verificatiemethoden bevestigen die uw beheerder heeft ingevuld.

## <a name="register-or-confirm-authentication-data-with-sspr"></a>Verificatiegegevens registreren of bevestigen met SSPR

1. Open de webbrowser op uw apparaat en ga naar de [registratiepagina voor het opnieuw instellen van een wachtwoord](http://aka.ms/ssprsetup)
2. Voer uw gebruikersnaam en wachtwoord in die door uw beheerder zijn verstrekt
3. Afhankelijk van hoe uw IT-personeel dingen hebt geconfigureerd, zijn een of meer van de volgende opties zijn beschikbaar om te configureren en te verifiëren. De beheerder kan vult sommige van deze voor u als ze uw toestemming voor het gebruik van de gegevens hebben.
    * Telefoon (werk), kan alleen worden ingesteld door uw beheerder
    * Telefoon voor authenticatie moet worden ingesteld op een ander telefoonnummer dat u toegang zou hebben soortgelijke van een mobiele telefoon die een tekstbericht of telefoongesprek kunnen ontvangen.
    * Verificatie-e-mailbericht moet worden ingesteld op een alternatieve e-mailadres dat u toegang hebt tot zonder dat het wachtwoord dat u opnieuw wilt instellen.
    * Beveiligingsvragen biedt u een lijst met uw beheerder heeft goedgekeurd voor u antwoord te vragen. U kunt niet gebruiken deze vraag of meer dan één keer te beantwoorden.
4. Geef en controleer de informatie die door de beheerder wordt vereist. Als meer dan één optie beschikbaar is, het is raadzaam dat u meerdere methoden om aan te bieden flexibiliteit bij een andere methode niet beschikbaar is registreren (voorbeeld: onderweg en geen toegang tot uw zakelijke telefoon)

    ![Registreer verificatiemethoden en klik op Voltooien][Register]

5. Als u na stap 4 de optie **Voltooien** kiest, kunt u de selfservice voor het opnieuw instellen van een wachtwoord gebruiken wanneer dat nodig is.

Als u gegevens in de telefoon voor authenticatie of het e-mailadres voor authenticatie invoert, zijn deze niet zichtbaar in de algemene map. De enige personen die deze gegevens kunnen zien, zijn u en uw beheerder. Alleen u kunt de antwoorden op uw beveiligingsvragen zien.

Beheerders kunnen eisen dat u uw verificatiemethoden na een bepaalde tijd bevestigt om er zeker van te zijn dat u nog steeds de juiste methoden hebt geregistreerd.

## <a name="common-problems-and-their-solutions"></a>Veelvoorkomende problemen en oplossingen

 Hier volgen enkele veelvoorkomende foutgevallen en hun oplossingen:

| Foutaanvraag| Welke fout ziet u?| Oplossing |
| --- | --- | --- |
| Een pagina 'Neem contact op met uw beheerder' verschijnt na het invoeren van mijn gebruikers-ID | Neem contact op met uw beheerder <br> <br> We hebben vastgesteld dat het wachtwoord voor uw gebruikersaccount niet wordt beheerd door Microsoft. Als gevolg hiervan zijn er niet automatisch opnieuw instellen van uw wachtwoord. <br> <br> Zo moet u contact op met uw IT-personeel voor verdere hulp. | U ziet dit bericht omdat uw IT-personeel beheert uw wachtwoord in uw on-premises omgeving en kunt u uw wachtwoord opnieuw instellen niet de heeft geen toegang tot uw account koppelen. <br> <br> Om uw wachtwoord opnieuw instellen, rechtstreeks voor hulp contact op met uw IT-personeel en laat ze weten dat u wilt uw wachtwoord opnieuw instellen zodat ze voor u deze functie kunnen inschakelen.|
| Wordt een foutbericht 'uw account is niet ingeschakeld voor wachtwoordherstel' na het invoeren van mijn gebruikers-ID | Uw account is niet ingeschakeld voor wachtwoordherstel <br> <br> We vinden het jammer, maar uw IT-personeel niet uw account voor gebruik met deze service is ingesteld. <br> <br> Als u wilt, kunnen we contact met een beheerder in uw organisatie om uw wachtwoord opnieuw instellen voor u. | U ziet dit bericht omdat uw IT-personeel niet ingeschakeld voor wachtwoordherstel voor uw organisatie van de heeft geen toegang tot uw accountkoppeling of u de functie is niet een licentie. <br> <br> Klik om uw wachtwoord opnieuw instellen, de contact op met de koppeling van een administrator voor het verzenden van een e-mailbericht naar uw bedrijf laten weten u wilt uw wachtwoord opnieuw instellen zodat ze voor u deze functie kunnen inschakelen en van IT-personeel. |
| Wordt een foutbericht ' we kunnen uw account niet verifiëren' na het invoeren van mijn gebruikers-ID | We kunnen uw account niet verifiëren <br> <br> Als u wilt, kunnen we contact met een beheerder in uw organisatie om uw wachtwoord opnieuw instellen voor u. | U ziet dit bericht omdat u zijn ingeschakeld voor het wachtwoord opnieuw instellen, maar u nog niet geregistreerd voor het gebruik van de service. Als u wilt registreren voor wachtwoord opnieuw instellen, gaat u naar http://aka.ms/ssprsetup wanneer u toegang tot je account hebt hersteld. <br> <br> Klik om uw wachtwoord opnieuw instellen, de contact op met de koppeling van een administrator voor het verzenden van een e-mailbericht naar uw bedrijf IT-personeel. |

## <a name="next-steps"></a>Volgende stappen

* [Hoe u uw wachtwoord kunt wijzigen met de selfservice voor het opnieuw instellen van een wachtwoord](active-directory-passwords-update-your-own-password.md)
* [Registratiepagina voor het opnieuw instellen van een wachtwoord](http://aka.ms/ssprsetup)
* [Portal voor het opnieuw instellen van een wachtwoord](https://passwordreset.microsoftonline.com/)
* [Kunt u zich niet aanmelden bij uw Microsoft-account?](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)

[Register]: ./media/active-directory-passwords-reset-register/register-2-methods.png "Registratiepagina voor het opnieuw instellen van een wachtwoord met geregistreerde methoden en knop Voltooien"

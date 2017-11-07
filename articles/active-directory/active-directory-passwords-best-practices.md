---
title: 'Implementeren: Azure AD-self-service voor wachtwoordherstel | Microsoft Docs'
description: Tips voor een geslaagde implementatie van Azure AD-self-service voor wachtwoordherstel
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: f8cd7e68-2c8e-4f30-b326-b22b16de9787
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: e28324fe0d7e52f1721af6cd835369f024d4c58f
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="how-to-successfully-rollout-self-service-password-reset"></a>Selfservice voor wachtwoordherstel implementeren

De meeste klanten volgen deze stappen voor een soepele implementatie van selfservice voor wachtwoordherstel.

1. [Schakel wachtwoordherstel in in uw directory](active-directory-passwords-getting-started.md).
2. [Configureer on-premises AD-machtigingen voor write-back van wachtwoord](active-directory-passwords-writeback.md#active-directory-permissions).
3. [Configureer write-back van wachtwoord](active-directory-passwords-writeback.md#configuring-password-writeback) om wachtwoorden van Azure AD terug te schrijven naar uw on-premises directory.
4. [Wijs de vereiste licenties toe en verifieer ze](active-directory-passwords-licensing.md).
5. Als u selfservice voor wachtwoordherstel geleidelijk wilt implementeren, kunt u toegang tot een groep gebruikers beperken zodat u met een specifieke groep kunt testen. Stel hiervoor de wisselknop **Selfservice voor wachtwoord opnieuw instellen is ingeschakeld** in op **Geselecteerd** en selecteer de beveiligingsgroep waarvoor u wachtwoordherstel wilt inschakelen. 
6. Vul [Authenticatiegegevens](active-directory-passwords-data.md) in voor uw gebruikers, zoals hun zakelijke telefoonnummer, mobiele telefoonnummer en een alternatief e-mailadres.
7. [Pas de Azure Active Directory-aanmeldingservaring aan met de huisstijl van uw bedrijf](active-directory-passwords-customize.md).
8. Instrueer uw gebruikers over het gebruik van self-service voor wachtwoordherstel door hen instructies te sturen om hen te laten zien hoe ze zich kunnen registreren en het wachtwoord opnieuw kunnen instellen.
9. U kunt ervoor kiezen registratie op elk gewenst moment af te dwingen en gebruikers te vereisen hun verificatiegegevens na een bepaalde periode opnieuw te bevestigen.
10. Controleer in de loop van de tijd gebruikers die zich hebben geregistreerd en gebruikmaken van de service door de [rapporten die worden verstrekt door Azure AD](active-directory-passwords-reporting.md) te bekijken.
11. Schakel wanneer u klaar bent wachtwoordherstel in voor alle gebruikers door de wisselknop **Selfservice voor wachtwoord opnieuw instellen is ingeschakeld** op **Alle** te zetten. 

    > [!IMPORTANT]
    > Test self-service voor wachtwoordherstel met een gebruiker en niet als beheerder, aangezien Microsoft sterke verificatievereisten afdwingt voor Azure-accounts van beheerders. Zie ons [artikel met gedetailleerde informatie](active-directory-passwords-how-it-works.md) voor meer informatie over het wachtwoordbeleid voor beheerders.

## <a name="email-based-rollout"></a>Implementatie op basis van e-mail

Veel klanten vinden een e-mailcampagne, met eenvoudig te gebruiken instructies, de eenvoudigste manier om gebruikers self-service voor wachtwoordherstel te laten gebruiken. [We hebben drie eenvoudige e-mailberichten gemaakt die u als sjabloon kunt gebruiken om te helpen bij uw implementatie.](https://onedrive.live.com/?authkey=%21AD5ZP%2D8RyJ2Cc6M&id=A0B59A91C740AB16%2125063&cid=A0B59A91C740AB16)

* De e-mailsjabloon **Binnenkort beschikbaar** die in de weken of dagen vóór implementatie moet worden gebruikt om gebruikers te laten weten dat ze iets moeten gaan doen.
* De e-mailsjabloon **Nu beschikbaar** die moet worden gebruikt op de dag van de lancering om gebruikers ertoe te zetten zich te registreren en hun verificatiegegevens te bevestigen, zodat ze self-service voor wachtwoordherstel kunnen gebruiken als ze het nodig hebben.
* De e-mailsjabloon **Herinnering voor aanmelding** die enkele dagen tot weken na implementatie moet worden gebruikt om gebruikers eraan te herinneren zich te registreren hun verificatiegegevens te bevestigen.

![E-mail][Email]

## <a name="creating-your-own-password-portal"></a>Uw eigen wachtwoordportal maken

Veel van onze klanten kiezen ervoor een webpagina te hosten en een DNS-basisvermelding te maken, zoals https://passwords.contoso.com. Ze vullen deze pagina met koppelingen naar de functionaliteit van Azure AD voor wachtwoordherstel, registratie voor wachtwoordherstel, portals voor het wijzigen van het wachtwoord en andere organisatiespecifieke informatie. In e-mailberichten of flyers die u verstuurt, kunt u een in huisstijl opgemaakte, gemakkelijk te onthouden URL opnemen die gebruikers kunnen raadplegen wanneer ze de services moeten gebruiken.

* Portal voor wachtwoordherstel: https://aka.ms/sspr
* Portal voor registratie voor wachtwoordherstel: http://aka.ms/ssprsetup
* Portal voor wijzigen wachtwoord: https://account.activedirectory.windowsazure.com/ChangePassword.aspx

We hebben een voorbeeldpagina gemaakt die u kunt gebruiken en aanpassen aan de behoeften van uw organisatie. U kunt deze via [GitHub](https://github.com/ajamess/password-reset-page) downloaden.

## <a name="using-enforced-registration"></a>Gedwongen registratie gebruiken

Als u wilt dat uw gebruikers zich registreren voor wachtwoordherstel, kunt u hen dwingen zich te registreren wanneer ze zich aanmelden via Azure AD. U kunt deze optie inschakelen via de blade **Wachtwoordherstel** van uw directory door de optie **Vereisen dat gebruikers zich bij aanmelding registreren?** te kiezen op het tabblad **Registratie**.

Beheerders kunnen gebruikers verplichten om zich na een bepaalde periode opnieuw te registreren door **Aantal dagen waarna gebruikers wordt gevraagd om hun verificatiegegevens te bevestigen** tussen 0-730 dagen in te stellen.

Nadat u deze optie hebt ingeschakeld, zien gebruikers een bericht dat hen informeert dat de beheerder vereist dat ze hun verificatiegegevens controleren.

## <a name="populate-authentication-data"></a>Verificatiegegevens invullen

Als u [verificatiegegevens voor uw gebruikers invult](active-directory-passwords-data.md), hoeven gebruikers zich niet te registreren voor wachtwoordherstel om gebruik te kunnen maken van self-service voor wachtwoordherstel. Zolang gebruikers de verificatiegegevens hebben gedefinieerd die voldoen aan het beleid voor wachtwoordherstel dat u hebt gedefinieerd hebben, kunnen gebruikers hun wachtwoord opnieuw instellen.

## <a name="disabling-self-service-password-reset"></a>Self-service voor wachtwoordherstel uitschakelen

Het uitschakelen van self-service voor wachtwoordherstel is heel eenvoudig. Open uw Azure AD-tenant en ga naar **Wachtwoordherstel**, **Eigenschappen**. Kies **Niemand** onder **Self-service voor wachtwoord opnieuw instellen is ingeschakeld**

## <a name="next-steps"></a>Volgende stappen

* [Stel uw wachtwoord opnieuw in of wijzig het](active-directory-passwords-update-your-own-password.md).
* [Registreer u voor selfservice voor wachtwoordherstel](active-directory-passwords-reset-register.md).
* [Hebt u een vraag over licenties?](active-directory-passwords-licensing.md)
* [Welke gegevens worden gebruikt door selfservice voor wachtwoordherstel en welke gegevens moet u voor uw gebruikers invullen?](active-directory-passwords-data.md)
* [Wat zijn de beleidsopties bij selfservice voor wachtwoordherstel?](active-directory-passwords-policy.md)
* [Wat is Wachtwoord terugschrijven en waarom is dit van belang?](active-directory-passwords-writeback.md)
* [Hoe maak ik rapporten van activiteit in selfservice voor wachtwoordherstel?](active-directory-passwords-reporting.md)
* [Wat zijn alle opties in selfservice voor wachtwoordherstel en wat houden ze in?](active-directory-passwords-how-it-works.md)
* [Ik denk dat er iets misgaat. Hoe los ik problemen in selfservice voor wachtwoordherstel op?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag die nog niet is beantwoord](active-directory-passwords-faq.md)

[Email]: ./media/active-directory-passwords-best-practices/sspr-emailtemplates.png "Deze e-mailsjablonen aan de behoeften van uw organisatie aanpassen"
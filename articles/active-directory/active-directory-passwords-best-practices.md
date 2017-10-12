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
ms.date: 08/28/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 4cfc1652377f0cfd059e336aec6994b40d32c559
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="roll-out-password-reset-for-users"></a>Wachtwoordherstel implementeren voor gebruikers

De meeste klanten volgen de onderstaande stappen voor een soepel verloop van de implementatie van de functionaliteit voor self-service voor wachtwoordherstel.

1. [Wachtwoordherstel inschakelen in uw directory](active-directory-passwords-getting-started.md)
2. [On-premises AD-machtigingen configureren voor write-back van wachtwoord](active-directory-passwords-how-it-works.md#active-directory-permissions)
3. [Configureer write-back van wachtwoord](active-directory-passwords-writeback.md#configuring-password-writeback) om wachtwoorden van Azure AD terug te schrijven naar uw on-premises directory
4. [Vereiste licenties toewijzen en verifiëren](active-directory-passwords-licensing.md)
5. Als u geleidelijk wilt implementeren, kunt u eventueel wachtwoordherstel beperken tot een groep van gebruikers om de functionaliteit in de loop van de tijd langzaamaan te implementeren. Wijzig hiervoor de schakeloptie **Self-service voor wachtwoord opnieuw instellen is ingeschakeld** van **Iedereen** in **Een groep** en selecteer de beveiligingsgroep die u wilt inschakelen voor wachtwoordherstel. Aan alle leden van deze groep moet een licentie zijn toegewezen. Dit is een goede manier om [licentieverlening op basis van groepen](active-directory-passwords-licensing.md#enable-group-or-user-based-licensing) in te schakelen.
6. Vul het minimale aantal [verificatiegegevens](active-directory-passwords-data.md) in, op basis van uw beleid.
7. Instrueer uw gebruikers over het gebruik van self-service voor wachtwoordherstel door hen instructies te sturen om hen te laten zien hoe ze zich kunnen registreren en het wachtwoord opnieuw kunnen instellen.
    > [!NOTE]
    > Test self-service voor wachtwoordherstel met een gebruiker en niet als beheerder, aangezien Microsoft sterke verificatievereisten afdwingt voor Azure-accounts van beheerders. Zie ons [artikel met gedetailleerde informatie](active-directory-passwords-how-it-works.md) voor meer informatie over het wachtwoordbeleid voor beheerders.

8. U kunt ervoor kiezen registratie op elk gewenst moment af te dwingen en gebruikers te vereisen hun verificatiegegevens na een bepaalde periode opnieuw te bevestigen. Als u niet wilt dat uw gebruikers zich moeten registreren, kunt u [wachtwoordherstel implementeren zonder registratie door eindgebruikers te vereisen](active-directory-passwords-data.md).
9. Controleer in de loop van de tijd gebruikers die zich hebben geregistreerd en gebruikmaken van de service door de [rapporten die worden verstrekt door Azure AD](active-directory-passwords-reporting.md) te bekijken.

## <a name="email-based-rollout"></a>Implementatie op basis van e-mail

Veel klanten vinden een e-mailcampagne, met eenvoudig te gebruiken instructies, de eenvoudigste manier om gebruikers self-service voor wachtwoordherstel te laten gebruiken. [We hebben drie eenvoudige e-mailberichten gemaakt die u als sjabloon kunt gebruiken om te helpen bij uw implementatie.](https://onedrive.live.com/?authkey=%21AD5ZP%2D8RyJ2Cc6M&id=A0B59A91C740AB16%2125063&cid=A0B59A91C740AB16)

* De e-mailsjabloon **Binnenkort beschikbaar** die in de weken of dagen vóór implementatie moet worden gebruikt om gebruikers te laten weten dat ze iets moeten gaan doen.
* De e-mailsjabloon **Nu beschikbaar** die moet worden gebruikt op de dag van de lancering om gebruikers ertoe te zetten zich te registreren en hun verificatiegegevens te bevestigen, zodat ze self-service voor wachtwoordherstel kunnen gebruiken als ze het nodig hebben.
* De e-mailsjabloon **Herinnering voor aanmelding** die enkele dagen tot weken na implementatie moet worden gebruikt om gebruikers eraan te herinneren zich te registreren hun verificatiegegevens te bevestigen.

## <a name="creating-your-own-password-portal"></a>Uw eigen wachtwoordportal maken

Veel van onze klanten kiezen ervoor een webpagina te hosten en een DNS-basisvermelding te maken, zoals https://passwords.contoso.com. Ze vullen deze pagina met koppelingen naar de functionaliteit van Azure AD voor wachtwoordherstel, registratie voor wachtwoordherstel, portals voor het wijzigen van het wachtwoord en andere organisatiespecifieke informatie. In e-mailberichten of flyers die u verstuurt, kunt u een in huisstijl opgemaakte, gemakkelijk te onthouden URL opnemen die gebruikers kunnen raadplegen wanneer ze de services moeten gebruiken.

* Portal voor wachtwoordherstel: https://passwordreset.microsoftonline.com/
* Portal voor registratie voor wachtwoordherstel: http://aka.ms/ssprsetup
* Portal voor wijzigen wachtwoord: https://account.activedirectory.windowsazure.com/ChangePassword.aspx

## <a name="using-enforced-registration"></a>Gedwongen registratie gebruiken

Als u wilt dat uw gebruikers zich registreren voor wachtwoordherstel, kunt u hen dwingen zich te registreren wanneer ze zich aanmelden via Azure AD. U kunt deze optie inschakelen via de blade **Wachtwoordherstel** van uw directory door de optie **Vereisen dat gebruikers zich bij aanmelding registreren?** te kiezen op het tabblad **Registratie**.

Beheerders kunnen gebruikers verplichten om zich na een bepaalde periode opnieuw te registreren door **Aantal dagen waarna gebruikers wordt gevraagd om hun verificatiegegevens te bevestigen** tussen 0-730 dagen in te stellen.

Nadat u deze optie hebt ingeschakeld, zien gebruikers een bericht dat hen informeert dat de beheerder vereist dat ze hun verificatiegegevens controleren.

## <a name="populate-authentication-data"></a>Verificatiegegevens invullen

Als u [verificatiegegevens voor uw gebruikers invult](active-directory-passwords-data.md), hoeven gebruikers zich niet te registreren voor wachtwoordherstel om gebruik te kunnen maken van self-service voor wachtwoordherstel. Zolang gebruikers de verificatiegegevens hebben gedefinieerd die voldoen aan het beleid voor wachtwoordherstel dat u hebt gedefinieerd hebben, kunnen gebruikers hun wachtwoord opnieuw instellen.

## <a name="disabling-self-service-password-reset"></a>Self-service voor wachtwoordherstel uitschakelen

Het uitschakelen van self-service voor wachtwoordherstel is heel eenvoudig. Open uw Azure AD-tenant en ga naar **Wachtwoordherstel**, **Eigenschappen**. Kies **Niemand** onder **Self-service voor wachtwoord opnieuw instellen is ingeschakeld**

## <a name="next-steps"></a>Volgende stappen

De volgende koppelingen bieden aanvullende informatie over wachtwoordherstel met behulp van Azure AD

* [**Snel starten**](active-directory-passwords-getting-started.md): aan de slag met self-service wachtwoordbeheer van Azure AD 
* [**Licentieverlening**](active-directory-passwords-licensing.md): uw Azure AD-licentieverlening configureren
* [**Gegevens**](active-directory-passwords-data.md): informatie over de gegevens die nodig zijn en hoe deze worden gebruikt voor wachtwoordbeheer
* [**Aanpassen**](active-directory-passwords-customize.md): het uiterlijk van de ervaring van self-service voor wachtwoordherstel aanpassen voor uw bedrijf.
* [**Beleid**](active-directory-passwords-policy.md): Azure AD-wachtwoordbeleid begrijpen en instellen
* [**Write-back van wachtwoord**](active-directory-passwords-writeback.md): hoe werkt write-back van wachtwoord met uw on-premises directory
* [**Rapportage**](active-directory-passwords-reporting.md): detecteren of, waar en wanneer uw gebruikers de functionaliteit voor self-service voor wachtwoordherstel gebruiken
* [**Gedetailleerde technische informatie**](active-directory-passwords-how-it-works.md): neem een kijkje achter de schermen om te begrijpen hoe het werkt
* [**Veelgestelde vragen**](active-directory-passwords-faq.md): hoe? Hoe komt dat? Wat? Waar? Wie? Wanneer? - Antwoorden op vragen die u altijd wilde stellen
* [**Probleemoplossing**](active-directory-passwords-troubleshoot.md): informatie over het oplossen van algemene problemen die optreden bij de self-service voor wachtwoordherstel
---
title: Azure Active Directory B2B-samenwerking richtlijnen licentieverlening | Microsoft Docs
description: Azure Active Directory B2B collaboration geen vereist betaald Azure AD-licenties, maar u kunt ook ophalen functies voor betaald B2B gastgebruikers
services: active-directory
documentationcenter: 
author: sasubram
manager: mtillman
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: sasubram
ms.custom: it-pro
ms.openlocfilehash: 664398eb71501ff450b785928992729f91740a19
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Licentierichtlijnen voor B2B-samenwerking van Azure Active Directory

Mogelijkheden van Azure AD B2B-samenwerking kunt u gastgebruikers in uw Azure AD-tenant zodat ze toegang krijgen tot Azure AD-services en andere bronnen in uw organisatie uitnodigen. Als u wilt toegang bieden tot betaalde Azure AD-functies, passende B2B-samenwerking gastgebruikers moeten beschikken over licenties voor Azure AD. 

Specifiek:
* Gratis mogelijkheden van Azure AD zijn beschikbaar voor gastgebruikers zonder aanvullende licenties.
* Als u toegang bieden tot betaalde Azure AD-functies voor B2B-gebruikers wilt, moet u voldoende licenties voor de ondersteuning van deze B2B gastgebruikers hebben.
* Een uitnodiging tenant met een Azure AD betaalde licentie heeft B2B-samenwerking rechten om een extra vijf B2B gastgebruikers uitgenodigd voor de tenant te gebruiken.
* De klant die eigenaar is van de uitnodiging tenant moet zijn om te bepalen hoeveel B2B-samenwerking gebruikers moeten betaalde Azure AD-mogelijkheden. Afhankelijk van de betaalde Azure AD betaald gewenste functies voor uw gastgebruikers, moet u genoeg Azure AD hebben licenties voor B2B-samenwerking gebruikers in dezelfde verhouding 5:1.

B2B-samenwerking gastgebruiker wordt toegevoegd als een gebruiker vanuit een partnerbedrijf, niet een werknemer van uw organisatie of een werknemer van een ander bedrijf in uw conglomeraat. Een B2B-gastgebruiker kan zich aanmelden met externe referenties of referenties die eigendom zijn van uw organisatie, zoals beschreven in dit artikel. 

Met andere woorden, B2B-licentieverlening ingesteld niet op basis van hoe de gebruiker wordt geverifieerd, maar in plaats daarvan door de relatie van de gebruiker voor uw organisatie. Als deze gebruikers niet partners, worden ze anders behandeld in de licentievoorwaarden. Ze worden niet beschouwd als een gebruiker B2B-samenwerking voor licentieverlening doeleinden, zelfs als hun UserType is gemarkeerd als 'Gast'. Ze moeten normaal gesproken licentie op één licentie per gebruiker. Deze gebruikers zijn onder andere:
* Uw werknemers
* Personeel aanmelden via externe identiteit
* Een werknemer van een ander bedrijf in uw conglomeraat


## <a name="licensing-examples"></a>Voorbeelden-licentieverlening
- Een klant wil 100 B2B-samenwerking gebruikers uitnodigen voor de Azure AD-tenant. De klant toegewezen toegang tot beheer en inrichting voor alle gebruikers, maar 50 gebruikers verplichten MFA en voorwaardelijke toegang. De klant moet aanschaffen 10 Azure AD Basic licenties en 10 Azure AD Premium-P1 softwarelicenties voor deze gebruikers B2B correct. Als de klant wil Identity Protection-functies gebruiken met B2B-gebruikers, moeten ze Azure AD Premium-P2 licenties omvatten de uitgenodigde gebruikers in dezelfde verhouding 5:1 hebben.
- Een klant heeft 10 werknemers die alle licenties met Azure AD Premium-P1. Ze willen nu uitnodigen 60 B2B-gebruikers die alle meervoudige authenticatie (MFA). De klant moet minstens 12 Azure AD Premium-P1 licenties omvatten alle 60 B2B-samenwerking gebruikers hebben onder de licentieverlening regel 5:1. Omdat ze al 10 Premium P1-licenties voor hun 10 werknemers hebben, hebben ze rechten om uit te nodigen 50 B2B van gebruikers met Premium-P1 functies zoals MFA. In dit voorbeeld moeten ze vervolgens 2 extra licenties voor Premium-P1 ten aanzien van de resterende 10 B2B-samenwerking gebruikers aanschaffen.

> [!NOTE]
> Er is geen enkele manier nog licenties toewijzen rechtstreeks aan de gebruikers B2B zodat deze gebruikersrechten voor B2B-samenwerking.

De klant die eigenaar is van de uitnodiging tenant moet zijn om te bepalen hoeveel B2B-samenwerking gebruikers moeten betaalde Azure AD-mogelijkheden. Afhankelijk van die Azure AD-functies die u voor uw gastgebruikers wilt instellen betaalde, moet u genoeg Azure AD betaald licenties voor B2B-samenwerking gebruikers in de verhouding 5:1 hebben. 

## <a name="additional-licensing-details"></a>Aanvullende licenties details
- Hoeft niet daadwerkelijk licenties toewijzen aan gebruikersaccounts B2B. Op basis van de verhouding 5:1, wordt licentieverlening automatisch berekend en gerapporteerd.
- Wanneer dit niet het betaalde Azure AD-licentie bestaat in de tenant elke uitgenodigde gebruiker ontvangt de rechten die de editie Free van Azure AD biedt.
- Als een gebruiker van de samenwerking al een betaalde Azure AD heeft B2B licentie van hun organisatie, komen ze niet een van de licenties B2B-samenwerking van de tenant nodigen verbruiken.

## <a name="advanced-discussion-what-are-the-licensing-considerations-when-we-add-users-from-a-conglomerate-organization-as-members-using-your-apis"></a>Geavanceerde discussie: wat zijn de licenties we gebruikers toevoegen van een organisatie conglomeraat als 'leden' uw API's gebruiken?
Een gastgebruiker B2B is een die werken met de organisatie van de host van een partnerorganisatie wordt verzocht. Normaal gesproken ander komt niet in aanmerking als B2B zelfs het B2B-functies gebruikt. Bekijk twee aanvragen in het bijzonder:

1. Als een host met een consumer-adres van een werknemer nodigt
  * Dit scenario is niet compatibel zijn met onze licentiebeleid en wordt niet aanbevolen.

2. Als de organisatie van een host wordt toegevoegd een gebruiker van een andere conglomeraat organisatie
  1. In dit geval wordt de gebruiker wordt verzocht met B2B-API's, maar deze aanvraag is niet normaal B2B. In het ideale geval hebben we deze organisaties de andere organisaties gebruikers uitnodigen als leden (onze API staat die). Licenties hebben in dit geval moet worden toegewezen aan deze leden ze toegang krijgen tot bronnen in de organisatie uitnodigen.

  2. Sommige organisaties willen gebruikers de andere organisatie moet worden toegevoegd als 'Gast' als een beleid toevoegen. Er zijn twee gevallen hier:
      * De conglomeraat bedrijf reeds gebruikmaakt van Azure AD en worden de uitgenodigde gebruikers in licentie gegeven in de andere organisatie: in dit geval wordt niet verwacht dat uitgenodigde gebruikers hoeven te volgen de lay-out van eerder in dit document 1:5-formule. 

      * De conglomeraat organisatie geen gebruik maakt van Azure AD of geen voldoende licenties: In dit geval, volgt u de lay-out van eerder in dit document 1:5-formule.

## <a name="next-steps"></a>Volgende stappen

Lees ook onze andere artikelen over Azure AD B2B-samenwerking:

* [Wat is Azure AD B2B-samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Hoe voeg beheerders van Azure Active Directory B2B-samenwerking gebruikers?](active-directory-b2b-admin-add-users.md)
* [Hoe kunnen IT-medewerkers B2B-samenwerking gebruikers toevoegen](active-directory-b2b-iw-add-users.md)
* [De elementen van de uitnodigingsmail voor B2B-samenwerking](active-directory-b2b-invitation-email.md)
* [B2B-samenwerking uitnodiging inwisseling](active-directory-b2b-redemption-experience.md)
* [Het oplossen van Azure Active Directory B2B-samenwerking](active-directory-b2b-troubleshooting.md)
* [Azure Active Directory B2B-samenwerking Veelgestelde vragen (FAQ)](active-directory-b2b-faq.md)
* [Azure Active Directory B2B-samenwerking API en de aanpassing](active-directory-b2b-api.md)
* [Multi-Factor Authentication voor gebruikers van B2B-samenwerking](active-directory-b2b-mfa-instructions.md)
* [B2B-samenwerking gebruikers zonder een uitnodiging toevoegen](active-directory-b2b-add-user-without-invite.md)
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)

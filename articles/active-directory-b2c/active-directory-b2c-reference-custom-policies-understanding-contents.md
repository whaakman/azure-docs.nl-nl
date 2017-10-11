---
title: 'Azure Active Directory B2C: Informatie over aangepaste beleidsregels van het starter pack | Microsoft Docs'
description: Een onderwerp op Azure Active Directory B2C aangepast beleid
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/25/2017
ms.author: joroja
ms.openlocfilehash: 9847bcfcc139a769847678c1cca6a8b9c3a30e93
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="understanding-the-custom-policies-of-the-azure-ad-b2c-custom-policy-starter-pack"></a>Wat is de aangepaste beleidsregels van het Azure AD B2C aangepast beleid starter pack?

Deze sectie vindt u de belangrijkste elementen van het beleid B2C_1A_base die wordt geleverd met de **Starter Pack** en die wordt gebruikt voor het ontwerpen van uw eigen beleid via de overname van de *B2C_1A_base_extensions beleid*.

Als zodanig het met name is gericht op de reeds gedefinieerde claimtypen, claimtransformaties, definities van inhoud, claimproviders met hun technische profielen en de gebruiker core trajecten.

> [!IMPORTANT]
> Microsoft biedt geen enkele expliciete of impliciete met betrekking tot de informatie hieronder. Wijzigingen kunnen worden ingevoerd op elk moment vóór GA tijdstip, GA tijdens of na.

Zowel uw eigen beleid en het beleid B2C_1A_base_extensions deze definities overschrijven en dit beleid bovenliggende door extra velden naar behoefte uitbreiden.

De belangrijkste elementen van de *B2C_1A_base beleid* zijn claimtypen, claimtransformaties en inhoud definities. Deze elementen kunnen vatbaar verwezen in uw eigen beleid ook als in de *B2C_1A_base_extensions beleid*.

## <a name="claims-schemas"></a>Claims schema 's

Dit claims schema's is onderverdeeld in drie secties:

1.  Een gedeelte van het eerste met een lijst met de minimale claims die vereist zijn voor de gebruiker trajecten goed te laten werken.
2.  Een tweede sectie de claims worden die zijn vereist voor queryreeksparameters en andere speciale parameters worden doorgegeven aan andere claimproviders, met name login.microsoftonline.com voor verificatie. **Wijzig geen deze claims**.
3.  En uiteindelijk een derde sectie met een lijst met aanvullende, optionele claims die kunnen worden verzameld van de gebruiker opgeslagen in de map en tokens verzonden tijdens het aanmelden. Nieuwe claims type verzameld van de gebruiker en/of verzonden in het token kan in deze sectie worden toegevoegd.

> [!IMPORTANT]
> Het schema claims bevat beperkingen op bepaalde claims zoals wachtwoorden en gebruikersnamen. Het beleid vertrouwen Framework (TF) behandelt Azure AD als andere claimprovider en alle bijbehorende beperkingen zijn gemodelleerd in het premium-beleid. Een beleid kan worden gewijzigd zodat meer beperkingen toevoegen, of gebruik een andere claimprovider voor opslag van referenties waarvoor een eigen beperkingen.

De beschikbare claimtypen worden hieronder vermeld.

### <a name="claims-that-are-required-for-the-user-journeys"></a>Claims die vereist voor de gebruiker trajecten zijn

De volgende claims zijn vereist voor de gebruiker trajecten goed te laten werken:

| Claims type | Beschrijving |
|-------------|-------------|
| *Gebruikers-id* | Gebruikersnaam |
| *signInName* | Meld u aan de naam |
| *tenantId* | Tenant-id (ID) van het gebruikersobject in Azure AD B2C Premium |
| *object-id* | Object-id (ID) van het gebruikersobject in Azure AD B2C Premium |
| *wachtwoord* | Wachtwoord |
| *NieuwWachtwoord* | |
| *reenterPassword* | |
| *passwordPolicies* | Wachtwoordbeleid gebruikt door Azure AD B2C Premium om te bepalen van de Wachtwoordsterkte, verlopen, enzovoort. |
| *Sub* | |
| *alternativeSecurityId* | |
| *identityProvider* | |
| *Weergavenaam* | |
| *strongAuthenticationPhoneNumber* | Het telefoonnummer van de gebruiker |
| *Verified.strongAuthenticationPhoneNumber* | |
| *E-mail* | E-mailadres dat kan worden gebruikt om contact op met de gebruiker |
| *signInNamesInfo.emailAddress* | E-mailadres waarmee de gebruiker kunt aanmelden |
| *otherMails* | E-mailadressen die kunnen worden gebruikt om contact op met de gebruiker |
| *userPrincipalName* | De gebruikersnaam die is opgeslagen in de Azure AD B2C Premium |
| *upnUserName* | Gebruikersnaam voor het maken van de UPN-naam |
| *mailNickName* | De naam van gebruiker e-mail nick die is opgeslagen in de Azure AD B2C Premium |
| *nieuwegebruiker* | |
| *uitgevoerd SelfAsserted invoer* | Claim die aangeeft of de kenmerken van de gebruiker zijn verzameld |
| *uitgevoerd PhoneFactor invoer* | Claim waarmee wordt aangegeven of een nieuw telefoonnummer van de gebruiker is verzameld |
| *authenticationSource* | Hiermee geeft u op of de gebruiker is geverifieerd op sociale id-Provider, login.microsoftonline.com of lokale account |

### <a name="claims-required-for-query-string-parameters-and-other-special-parameters"></a>Claims die vereist zijn voor queryreeksparameters en andere speciale parameters

De volgende claims zijn vereist om door te geven op de speciale parameters (inclusief een aantal queryreeksparameters) voor andere claimproviders:

| Claims type | Beschrijving |
|-------------|-------------|
| *Nux* | Speciale parameter doorgegeven voor de verificatie van lokale account aan login.microsoftonline.com |
| *NCA* | Speciale parameter doorgegeven voor de verificatie van lokale account aan login.microsoftonline.com |
| *prompt* | Speciale parameter doorgegeven voor de verificatie van lokale account aan login.microsoftonline.com |
| *Mkt* | Speciale parameter doorgegeven voor de verificatie van lokale account aan login.microsoftonline.com |
| *kredietbrief* | Speciale parameter doorgegeven voor de verificatie van lokale account aan login.microsoftonline.com |
| *grant_type* | Speciale parameter doorgegeven voor de verificatie van lokale account aan login.microsoftonline.com |
| *bereik* | Speciale parameter doorgegeven voor de verificatie van lokale account aan login.microsoftonline.com |
| *client_id* | Speciale parameter doorgegeven voor de verificatie van lokale account aan login.microsoftonline.com |
| *objectIdFromSession* | Parameter geleverd door de standaardprovider voor het beheer van sessie om aan te geven dat de object-id is opgehaald van een sessie voor eenmalige aanmelding |
| *isActiveMFASession* | Opgegeven door de MFA-sessiebeheer om aan te geven dat de gebruiker een actieve sessie voor MFA heeft-parameter |

### <a name="additional-optional-claims-that-can-be-collected"></a>Aanvullende (optioneel) claims die kunnen worden verzameld

De volgende claims zijn extra claims die kunnen worden verzameld van de gebruikers, opgeslagen in de map en in het token is verzonden. Zoals beschreven voordat, kunnen aanvullende claims worden toegevoegd aan deze lijst.

| Claims type | Beschrijving |
|-------------|-------------|
| *Voornaam* | Van de gebruiker opgegeven naam (ook wel bekend als voornaam) |
| *Achternaam* | De achternaam van de gebruiker (ook wel bekend als familienaam of achternaam) |
| *Extension_picture* | Afbeelding van de gebruiker op sociale |

## <a name="claim-transformations"></a>Claimtransformaties

De beschikbare claimtransformaties worden hieronder vermeld.

| Claim-transformatie | Beschrijving |
|----------------------|-------------|
| *CreateOtherMailsFromEmail* | |
| *CreateRandomUPNUserName* | |
| *CreateUserPrincipalName* | |
| *CreateSubjectClaimFromObjectID* | |
| *CreateSubjectClaimFromAlternativeSecurityId* | |
| *CreateAlternativeSecurityId* | |

## <a name="content-definitions"></a>Definities van inhoud

Deze sectie beschrijft de inhoud definities is al gedeclareerd in de *B2C_1A_base* beleid. Deze inhoud definities zijn vatbaar voor worden waarnaar wordt verwezen, genegeerd en/of uitgebreid naar behoefte in uw eigen beleid ook als in de *B2C_1A_base_extensions* beleid.

| Claimprovider | Beschrijving |
|-----------------|-------------|
| *Facebook* | |
| *Aanmelding voor lokaal Account* | |
| *PhoneFactor* | |
| *Azure Active Directory* | |
| *Self die wordt beweerd* | |
| *Lokaal Account* | |
| *Sessiebeheer* | |
| *Beleidsengine Trustframework* | |
| *TechnicalProfiles* | |
| *Uitgever van beveiligingstoken* | |

## <a name="technical-profiles"></a>Technische profielen

Deze sectie beschrijft de technische profielen is al gedeclareerd voor de claimprovider in de *B2C_1A_base* beleid. Deze technische profielen zijn vatbaar voor worden verder waarnaar wordt verwezen, genegeerd en/of uitgebreid naar behoefte in uw eigen beleid ook als in de *B2C_1A_base_extensions* beleid.

### <a name="technical-profiles-for-facebook"></a>Technische profielen voor Facebook

| Technische profiel | Beschrijving |
|-------------------|-------------|
| *Facebook OAUTH* | |

### <a name="technical-profiles-for-local-account-signin"></a>Technische profielen voor lokale aanmelding met Account

| Technische profiel | Beschrijving |
|-------------------|-------------|
| *Aanmelding niet-interactieve* | |

### <a name="technical-profiles-for-phone-factor"></a>Technische profielen voor Phone Factor

| Technische profiel | Beschrijving |
|-------------------|-------------|
| *PhoneFactor-invoer* | |
| *PhoneFactor-InputOrVerify* | |
| *PhoneFactor controleren* | |

### <a name="technical-profiles-for-azure-active-directory"></a>Technische profielen voor Azure Active Directory

| Technische profiel | Beschrijving |
|-------------------|-------------|
| *AAD-algemeen* | Technische profiel die zijn opgenomen in de andere technische AAD-xxx-profielen |
| *AAD-UserWriteUsingAlternativeSecurityId* | Technische profiel voor het sociaal-aanmeldingen |
| *AAD-UserReadUsingAlternativeSecurityId* | Technische profiel voor het sociaal-aanmeldingen |
| *AAD-UserReadUsingAlternativeSecurityId-NoError* | Technische profiel voor het sociaal-aanmeldingen |
| *AAD-UserWritePasswordUsingLogonEmail* | Technische profiel voor lokale accounts |
| *AAD-UserReadUsingEmailAddress* | Technische profiel voor lokale accounts |
| *AAD-UserWriteProfileUsingObjectId* | Technische profiel voor het bijwerken van de gebruikersrecord met object-id |
| *AAD-UserWritePhoneNumberUsingObjectId* | Technische profiel voor het bijwerken van de gebruikersrecord met object-id |
| *AAD-UserWritePasswordUsingObjectId* | Technische profiel voor het bijwerken van de gebruikersrecord met object-id |
| *AAD-UserReadUsingObjectId* | Technische profiel wordt gebruikt om gegevens te lezen nadat de gebruiker wordt geverifieerd |

### <a name="technical-profiles-for-self-asserted"></a>Technische profielen voor Self die wordt beweerd

| Technische profiel | Beschrijving |
|-------------------|-------------|
| *SelfAsserted sociale* | |
| *SelfAsserted ProfileUpdate* | |

### <a name="technical-profiles-for-local-account"></a>Technische profielen voor lokaal Account

| Technische profiel | Beschrijving |
|-------------------|-------------|
| *LocalAccountSignUpWithLogonEmail* | |

### <a name="technical-profiles-for-session-management"></a>Technische profielen voor sessiebeheer

| Technische profiel | Beschrijving |
|-------------------|-------------|
| *SM-NoOperation* | |
| *SM-AAD* | |
| *SM-SocialSignup* | Profielnaam wordt gebruikt voor het AAD-sessie tussen sign up heffen en aanmelden |
| *SM-SocialLogin* | |
| *SM-MFA* | |

### <a name="technical-profiles-for-trustframework-policy-engine-technicalprofiles"></a>Technische profielen voor Trustframework beleid Engine TechnicalProfiles

Momenteel geen technische profielen zijn gedefinieerd voor de **Trustframework beleid Engine TechnicalProfiles** claimprovider.

### <a name="technical-profiles-for-token-issuer"></a>Technische profielen voor uitgever van beveiligingstoken

| Technische profiel | Beschrijving |
|-------------------|-------------|
| *JwtIssuer* | |

## <a name="user-journeys"></a>Gebruiker trajecten

Deze sectie beschrijft de trajecten gebruiker is al gedeclareerd in de *B2C_1A_base* beleid. Deze gebruiker trajecten zijn vatbaar voor worden verder waarnaar wordt verwezen, genegeerd en/of uitgebreid naar behoefte in uw eigen beleid ook als in de *B2C_1A_base_extensions* beleid.

| Gebruiker reis | Beschrijving |
|--------------|-------------|
| *Aanmelden* | |
| *Aanmelding* | |
| *SignUpOrSignIn* | |
| *EditProfile* | |
| *PasswordReset* | |

---
title: Informatie over aangepaste beleidsregels van het starter pack in Azure Active Directory B2C | Microsoft Docs
description: Een onderwerp op Azure Active Directory B2C aangepast beleid.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/25/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: ebcd7a677acde12558b0f566bce9172a0d00233b
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37442471"
---
# <a name="understanding-the-custom-policies-of-the-azure-ad-b2c-custom-policy-starter-pack"></a>Inzicht krijgen in het aangepaste beleid van de Azure AD B2C aangepast beleid beginnerspakket

Deze sectie vindt u de belangrijkste elementen van het beleid B2C_1A_base die wordt geleverd met de **Beginnerspakket** en die wordt gebruikt voor het ontwerpen van uw eigen beleid via de overname van de *B2C_1A_base_extensions beleid* .

Als zodanig het met name richt zich op de reeds gedefinieerde claimtypen, claimtransformaties, inhoudsdefinities, claimproviders met hun technische profielen en de gebruiker core reizen.

> [!IMPORTANT]
> Microsoft biedt geen garanties, expliciet of impliciet, met betrekking tot de informatie hieronder. Wijzigingen kunnen worden ingevoerd op elk gewenst moment, voor de tijd van de algemene beschikbaarheid, GA tijdens of na.

Zowel uw eigen beleid en het beleid B2C_1A_base_extensions overschrijven deze definities en dit beleid bovenliggende uitbreiden door nieuwe zijn in indien nodig.

De belangrijkste elementen van de *B2C_1A_base beleid* zijn claimtypen, claimtransformaties en definities van inhoud. Deze elementen kunnen vatbaar voor naar worden verwezen in uw eigen beleid ook als in de *B2C_1A_base_extensions beleid*.

## <a name="claims-schemas"></a>Claims schema 's

Deze vorderingen schema's is onderverdeeld in drie secties:

1.  Een eerste sectie met een lijst met de minimale claims die zijn vereist voor de gebruiker reizen goed te laten werken.
2.  Een tweede sectie met een lijst met de claims vereist zijn voor queryreeksparameters en andere speciale parameters worden doorgegeven aan andere claims-providers, met name login.microsoftonline.com voor verificatie. **Mag niet worden gewijzigd deze claims**.
3.  En uiteindelijk een derde sectie met een lijst met aanvullende, optionele claims die kunnen worden verzameld van de gebruiker opgeslagen in de map en tokens verzonden tijdens het aanmelden. Nieuwe claims dat moet worden verzameld van de gebruiker en/of in het token verzonden, kan in deze sectie worden toegevoegd.

> [!IMPORTANT]
> Het schema claims bevat beperkingen met betrekking tot bepaalde claims, zoals wachtwoorden en gebruikersnamen. Het beleid vertrouwen Framework (TF) behandelt Azure AD als elke andere claimprovider en alle bijbehorende beperkingen zijn gemodelleerd in het aangepaste beleid. Een beleid kan worden gewijzigd om meer beperkingen toevoegen, of gebruik een andere claimprovider voor opslag van referenties die een eigen beperkingen hebben.

De beschikbare claimtypen worden hieronder vermeld.

### <a name="claims-that-are-required-for-the-user-journeys"></a>Claims die vereist voor de gebruiker reizen zijn

De volgende claims zijn vereist voor de gebruiker reizen goed te laten werken:

| Claims type | Beschrijving |
|-------------|-------------|
| *Gebruikers-id* | Gebruikersnaam |
| *signInName* | Meld u aan de naam |
| *tenantId* | Tenant-id (ID) van het gebruikersobject in Azure AD B2C |
| *objectId* | Object-id (ID) van het gebruikersobject in Azure AD B2C |
| *Wachtwoord* | Wachtwoord |
| *newPassword* | |
| *reenterPassword* | |
| *passwordPolicies* | Beleid voor wachtwoorden die door Azure AD B2C wordt gebruikt om te bepalen van de Wachtwoordsterkte, vervaldatum, enzovoort. |
| *sub* | |
| *alternativeSecurityId* | |
| *identityProvider* | |
| *displayName* | |
| *strongAuthenticationPhoneNumber* | Telefoonnummer van gebruiker |
| *Verified.strongAuthenticationPhoneNumber* | |
| *E-mail* | E-mailadres dat kan worden gebruikt om contact op met de gebruiker |
| *signInNamesInfo.emailAddress* | E-mailadres dat de gebruiker gebruiken kunt om aan te melden bij |
| *otherMails* | E-mailadressen die kunnen worden gebruikt om contact op met de gebruiker |
| *userPrincipalName* | De gebruikersnaam die is opgeslagen in de Azure AD B2C |
| *upnUserName* | Gebruikersnaam voor het maken van UPN-naam |
| *mailNickName* | E-mail bijnaam van de gebruiker die is opgeslagen in de Azure AD B2C |
| *newUser* | |
| *executed-SelfAsserted-Input* | Claim waarmee wordt aangegeven of de kenmerken van de gebruiker zijn verzameld |
| *executed-PhoneFactor-Input* | Claim waarmee wordt aangegeven of een nieuw telefoonnummer van de gebruiker is vastgelegd |
| *authenticationSource* | Hiermee geeft u op of de gebruiker is geverifieerd op sociale id-Provider, login.microsoftonline.com of een lokaal account |

### <a name="claims-required-for-query-string-parameters-and-other-special-parameters"></a>Claims vereist zijn voor queryreeksparameters en andere speciale parameters

De volgende claims zijn vereist om door te geven op speciale parameters (met inbegrip van sommige queryreeksparameters) voor andere claims-providers:

| Claims type | Beschrijving |
|-------------|-------------|
| *nux* | Speciale parameter voor de verificatie van lokale account wordt doorgegeven aan login.microsoftonline.com |
| *NCA* | Speciale parameter voor de verificatie van lokale account wordt doorgegeven aan login.microsoftonline.com |
| *prompt* | Speciale parameter voor de verificatie van lokale account wordt doorgegeven aan login.microsoftonline.com |
| *Mkt* | Speciale parameter voor de verificatie van lokale account wordt doorgegeven aan login.microsoftonline.com |
| *lc* | Speciale parameter voor de verificatie van lokale account wordt doorgegeven aan login.microsoftonline.com |
| *grant_type* | Speciale parameter voor de verificatie van lokale account wordt doorgegeven aan login.microsoftonline.com |
| *Bereik* | Speciale parameter voor de verificatie van lokale account wordt doorgegeven aan login.microsoftonline.com |
| *client_id* | Speciale parameter voor de verificatie van lokale account wordt doorgegeven aan login.microsoftonline.com |
| *objectIdFromSession* | Parameter opgegeven door de standaardprovider voor het beheer van sessie om aan te geven dat de object-ID is opgehaald vanuit een sessie voor eenmalige aanmelding |
| *isActiveMFASession* | Parameter opgegeven door de MFA-sessiebeheer om aan te geven dat de gebruiker een actieve sessie voor MFA heeft |

### <a name="additional-optional-claims-that-can-be-collected"></a>Aanvullende (optioneel) claims die kunnen worden verzameld

De volgende claims zijn extra claims die kunnen worden verzameld van de gebruikers, opgeslagen in de map en in het token verzonden. Zoals wordt beschreven voordat, kunnen aanvullende claims worden toegevoegd aan deze lijst.

| Claims type | Beschrijving |
|-------------|-------------|
| *givenName* | De opgegeven naam van gebruiker (ook wel bekend als voornaam) |
| *Achternaam* | De achternaam van de gebruiker (ook wel bekend als familienaam of achternaam) |
| *Extension_picture* | Afbeelding van de gebruiker op sociale |

## <a name="claim-transformations"></a>Claimtransformaties

De beschikbare claimtransformaties worden hieronder vermeld.

| Claimtransformatie | Beschrijving |
|----------------------|-------------|
| *CreateOtherMailsFromEmail* | |
| *CreateRandomUPNUserName* | |
| *CreateUserPrincipalName* | |
| *CreateSubjectClaimFromObjectID* | |
| *CreateSubjectClaimFromAlternativeSecurityId* | |
| *CreateAlternativeSecurityId* | |

## <a name="content-definitions"></a>Definities van inhoud

Deze sectie beschrijft de inhoudsdefinities is al gedeclareerd in de *B2C_1A_base* beleid. Deze inhoud definities zijn vatbaar voor worden waarnaar wordt verwezen, worden genegeerd en/of verlengd zo nodig in uw eigen beleid ook als in de *B2C_1A_base_extensions* beleid.

| Claimprovider | Beschrijving |
|-----------------|-------------|
| *Facebook* | |
| *Aanmelden met lokaal Account* | |
| *PhoneFactor* | |
| *Azure Active Directory* | |
| *Zelf een door de bevestigde* | |
| *Lokaal Account* | |
| *Sessiebeheer* | |
| *Trustframework-beleid-Engine* | |
| *TechnicalProfiles* | |
| *Uitgever van het token* | |

## <a name="technical-profiles"></a>Technische profielen

In deze sectie ziet u de technische profielen is al gedeclareerd per claimprovider in de *B2C_1A_base* beleid. Deze technische profielen zijn vatbaar voor worden verder waarnaar wordt verwezen, worden genegeerd en/of verlengd zo nodig in uw eigen beleid ook als in de *B2C_1A_base_extensions* beleid.

### <a name="technical-profiles-for-facebook"></a>Technische profielen voor Facebook

| Technisch profiel | Beschrijving |
|-------------------|-------------|
| *Facebook-OAUTH* | |

### <a name="technical-profiles-for-local-account-signin"></a>Technische profielen voor aanmelden met lokaal Account

| Technisch profiel | Beschrijving |
|-------------------|-------------|
| *Aanmelding niet-interactieve* | |

### <a name="technical-profiles-for-phone-factor"></a>Technische profielen voor Phone Factor

| Technisch profiel | Beschrijving |
|-------------------|-------------|
| *PhoneFactor-invoer* | |
| *PhoneFactor-InputOrVerify* | |
| *Controleer of van PhoneFactor* | |

### <a name="technical-profiles-for-azure-active-directory"></a>Technische profielen voor Azure Active Directory

| Technisch profiel | Beschrijving |
|-------------------|-------------|
| *AAD-Common* | Technisch profiel opgenomen door de andere technische AAD-xxx-profielen |
| *AAD-UserWriteUsingAlternativeSecurityId* | Technisch profiel voor sociale aanmeldingen |
| *AAD-UserReadUsingAlternativeSecurityId* | Technisch profiel voor sociale aanmeldingen |
| *AAD-UserReadUsingAlternativeSecurityId-NoError* | Technisch profiel voor sociale aanmeldingen |
| *AAD-UserWritePasswordUsingLogonEmail* | Technisch profiel voor lokale accounts |
| *AAD-UserReadUsingEmailAddress* | Technisch profiel voor lokale accounts |
| *AAD-UserWriteProfileUsingObjectId* | Technisch profiel voor het bijwerken van de record van de gebruiker met object-id |
| *AAD-UserWritePhoneNumberUsingObjectId* | Technisch profiel voor het bijwerken van de record van de gebruiker met object-id |
| *AAD-UserWritePasswordUsingObjectId* | Technisch profiel voor het bijwerken van de record van de gebruiker met object-id |
| *AAD-UserReadUsingObjectId* | Technisch profiel wordt gebruikt voor het lezen van gegevens nadat de gebruiker wordt geverifieerd |

### <a name="technical-profiles-for-self-asserted"></a>Technische profielen voor selfservice door de bevestigde

| Technisch profiel | Beschrijving |
|-------------------|-------------|
| *SelfAsserted-Social* | |
| *SelfAsserted-ProfileUpdate* | |

### <a name="technical-profiles-for-local-account"></a>Technische profielen voor lokaal Account

| Technisch profiel | Beschrijving |
|-------------------|-------------|
| *LocalAccountSignUpWithLogonEmail* | |

### <a name="technical-profiles-for-session-management"></a>Technische profielen voor het beheer van sessie

| Technisch profiel | Beschrijving |
|-------------------|-------------|
| *SM-NoOperation* | |
| *SM-AAD* | |
| *SM-SocialSignup* | Profielnaam wordt gebruikt voor het AAD-sessie tussen aanmelding dubbelzinnigheid van en aanmelden |
| *SM-SocialLogin* | |
| *SM-MFA* | |

### <a name="technical-profiles-for-the-trust-framework-policy-engine"></a>Voor de beleidsengine van vertrouwensrelatie framework-technische profielen

Op dit moment geen technische profielen zijn gedefinieerd voor de **Trustframework-beleid Engine TechnicalProfiles** claimprovider.

### <a name="technical-profiles-for-token-issuer"></a>Technische profielen voor de uitgever van het Token

| Technisch profiel | Beschrijving |
|-------------------|-------------|
| *JwtIssuer* | |

## <a name="user-journeys"></a>Gebruikers reizen

In deze sectie ziet u de transporten gebruiker is al gedeclareerd in de *B2C_1A_base* beleid. Deze gebruiker reizen zijn vatbaar voor worden verder waarnaar wordt verwezen, worden genegeerd en/of verlengd zo nodig in uw eigen beleid ook als in de *B2C_1A_base_extensions* beleid.

| Gebruikersbeleving | Beschrijving |
|--------------|-------------|
| *Aanmelden* | |
| *Aanmelden met* | |
| *SignUpOrSignIn* | |
| *EditProfile* | |
| *PasswordReset* | |

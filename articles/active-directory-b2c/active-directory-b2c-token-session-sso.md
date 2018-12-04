---
title: Token-, sessie en configuratie voor eenmalige aanmelding in Azure Active Directory B2C | Microsoft Docs
description: Token-, sessie en configuratie voor eenmalige aanmelding in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: c93bc018aea92a63adac4889d9496356543c1e52
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52842727"
---
# <a name="token-session-and-single-sign-on-configuration-in-azure-active-directory-b2c"></a>Token-, sessie en configuratie voor eenmalige aanmelding in Azure Active Directory B2C

Hiermee kunt u heel nauwkeurig bepalen, op een [stroom gebruikersbasis](active-directory-b2c-reference-policies.md), van:

- Levensduur van beveiligingstokens die door Azure Active Directory (Azure AD) B2C wordt verzonden.
- Levensduur van web application sessies worden beheerd door Azure AD B2C.
- De indelingen van belangrijke claims in de beveiligingstokens die door Azure AD B2C wordt verzonden.
- Eenmalige aanmelding (SSO) gedrag voor meerdere apps en gebruikersstromen in uw Azure AD B2C-tenant.

U kunt deze functie op elk gewenst beleidstype, maar in dit voorbeeld laten zien hoe u de functie met een gebruikersstroom registreren of aanmelden. Voor gebruikersstromen, kunt u deze functie in uw Azure AD B2C-directory als volgt:

1. Klik op **gebruikersstromen**.
2. Open een gebruikersstroom door erop te klikken. Bijvoorbeeld, klikt u op **B2C_1_SiUpIn**.
3. Klik op **Eigenschappen**.
4. Onder **Token compatibiliteitsinstellingen**, breng de gewenste wijzigingen aan. Meer informatie over de beschikbare eigenschappen in de volgende secties.
5. Klik op **opslaan** boven aan het menu.

## <a name="token-lifetimes-configuration"></a>Configuratie van de levensduur van tokens

Azure AD B2C ondersteunt de [OAuth 2.0-protocol voor autorisatie](active-directory-b2c-reference-protocols.md) voor het inschakelen van beveiligde toegang tot beveiligde bronnen. Voor het implementeren van deze ondersteuning, Azure AD B2C verzendt verschillende [beveiligingstokens](active-directory-b2c-reference-tokens.md). 

De volgende eigenschappen worden gebruikt voor het beheren van de levensduur van beveiligingstokens die zijn gegenereerd door Azure AD B2C:

- **Toegang en ID-token levensduur (minuten)** - de levensduur van het OAuth 2.0-bearer-token gebruikt voor toegang tot een beveiligde resource.
    - Standaard = 60 minuten.
    - Minimum (inclusief) = 5 minuten.
    - Maximum aantal (inclusief) = 1440 minuten.
- **Levensduur van vernieuwingstoken (dagen)** : de maximale periode waarbinnen een vernieuwingstoken kan worden gebruikt voor het verkrijgen van een nieuwe toegang of de ID-token (en eventueel een nieuwe vernieuwingstoken, als uw toepassing is verleend de `offline_access` bereik).
    - Standaardinstelling = 14 dagen.
    - Minimum (inclusief) = 1 dag.
    - Maximum aantal (inclusief) = 90 dagen.
- **Levensduur van sliding window token vernieuwen (dagen)** - na deze tijd is verstreken periode de gebruiker wordt gedwongen om te opnieuw te verifiëren, ongeacht de geldigheidsperiode van de meest recente vernieuwingstoken verkregen door de toepassing. Kan alleen worden opgegeven als de switch is ingesteld op **gebonden**. Het moet groter zijn dan of gelijk zijn aan de **levensduur van vernieuwingstoken (dagen)** waarde. Als de switch is ingesteld op **Unbounded**, u niet een bepaalde waarde opgeven.
    - Standaardwaarde = 90 dagen.
    - Minimum (inclusief) = 1 dag.
    - Maximum aantal (inclusief) = 365 dagen.

De volgende gevallen gebruik worden ingeschakeld met behulp van deze eigenschappen:

- Toestaan dat een gebruiker aangemeld bij een mobiele toepassing blijven voor onbepaalde tijd, zolang de gebruiker op de toepassing voortdurend actief is. U kunt instellen **Refresh token levensduur van sliding window (dagen)** naar **Unbounded** in uw gebruikersstroom aanmelden.
- Voldoen aan uw toonaangevende beveiligings- en nalevingsvereisten door in te stellen van de levensduur van de juiste toegang-tokens.

Deze instellingen zijn niet beschikbaar voor wachtwoord opnieuw instellen van gebruikersstromen. 

## <a name="token-compatibility-settings"></a>Instellingen voor tokencompatibiliteit

De volgende eigenschappen kunnen klanten opt-in indien nodig:

- **Claim van verlener (iss)** -deze eigenschap geeft u de Azure AD B2C-tenant die het token is uitgegeven.
    - `https://<domain>/{B2C tenant GUID}/v2.0/` -Dit is de standaardwaarde.
    - `https://<domain>/tfp/{B2C tenant GUID}/{Policy ID}/v2.0/` -Deze waarde bevat de id's voor zowel de B2C-tenant en de gebruikersstroom gebruikt in het token aan te vragen. Als uw app of de tapewisselaar moet Azure AD B2C om te voldoen aan de [OpenID Connect Discovery 1.0-specificatie](https://openid.net/specs/openid-connect-discovery-1_0.html), gebruikt deze waarde.
- **Onderwerp (sub) claim** -deze eigenschap geeft u de entiteit die het token worden bevestigd met informatie.
    - **Object-id** -deze eigenschap is de standaardwaarde. Het wordt gevuld met de object-ID van de gebruiker in de map in de `sub` claim in het token.
    - **Niet ondersteund** : deze eigenschap wordt alleen aangeboden voor achterwaartse compatibiliteit en het is raadzaam dat u naar overschakelt **ObjectID** zodra u zich kunt.
- **Claim voor beleids-ID** -deze eigenschap geeft het claimtype waarin de beleids-ID die wordt gebruikt in de aanvraag voor een toegangstoken wordt gevuld.
    - **TFP** -deze eigenschap is de standaardwaarde.
    - **ACR** -deze eigenschap wordt alleen aangeboden voor achterwaartse compatibiliteit.

## <a name="session-behavior"></a>Sessiegedrag

Azure AD B2C ondersteunt de [OpenID Connect-verificatieprotocol](active-directory-b2c-reference-oidc.md) voor het inschakelen van beveiligde aanmelding tot webtoepassingen. U kunt de volgende eigenschappen voor het beheren van web application sessies:

- **Web-appsessie (minuten)** - de levensduur van Azure AD B2C-sessiecookie die zijn opgeslagen op de browser van de gebruiker bij een geslaagde verificatie.
    - Standaardinstelling = 1440 minuten.
    - Minimum (inclusief) = 15 minuten.
    - Maximum aantal (inclusief) = 1440 minuten.
- **Web-appsessie** : als deze schakeloptie is ingesteld op **Absolute**, de gebruiker wordt gedwongen om te verifiëren opnieuw na de periode die is opgegeven door **Web-appsessie (minuten)** is verstreken. Als deze schakeloptie is ingesteld op **Rolling** (de standaardinstelling), de gebruiker blijft aangemeld zolang de gebruiker voortdurend actief zijn in uw webtoepassing is.

De volgende gevallen gebruik worden ingeschakeld met behulp van deze eigenschappen:

- Voldoen aan de beveiligings- en vereisten van uw branche door in te stellen van de juiste toepassing websessie levensduur.
- De verificatie van de Force na een bepaalde tijd instellen tijdens een gebruikersinteractie met hoogwaardige beveiliging onderdeel van uw webtoepassing. 

Deze instellingen zijn niet beschikbaar voor wachtwoord opnieuw instellen van gebruikersstromen.

## <a name="single-sign-on-sso-configuration"></a>Configuratie van eenmalige aanmelding (SSO)

Als u meerdere toepassingen en gebruikersstromen in uw B2C-tenant hebt, kunt u de interactie van gebruikers beheren binnen deze met behulp van de **configuratie voor eenmalige aanmelding** eigenschap. U kunt de eigenschap instellen op een van de volgende instellingen:

- **Tenant** -dit is de standaardinstelling. Met deze instelling kan meerdere toepassingen en gebruiker stromen in uw B2C-tenant voor het delen van dezelfde sessie van de gebruiker. Bijvoorbeeld, wanneer een gebruiker zich in een toepassing, de gebruiker kan ook naadloos zich aanmelden bij een andere ene, Contoso geneesmiddelen, bij toegang tot het.
- **Toepassing** -deze instelling kunt u een gebruikerssessie uitsluitend bedoeld voor een toepassing, onafhankelijk van andere toepassingen te onderhouden. Bijvoorbeeld, als u wilt dat de gebruiker zich aanmeldt bij Contoso geneesmiddelen (met dezelfde referenties), zelfs als de gebruiker is al aangemeld bij Contoso winkelen, een andere toepassing op dezelfde B2C-tenant. 
- **Beleid** -deze instelling kunt u een gebruikerssessie uitsluitend bedoeld voor een gebruikersstroom, onafhankelijk van de toepassingen te onderhouden. Bijvoorbeeld, als de gebruiker is al aangemeld en een multi-factor authentication (MFA) stap voltooid, kan de gebruiker toegang krijgen tot delen van betere beveiliging van meerdere toepassingen, zolang de sessie die is gekoppeld aan de gebruikersstroom niet verloopt.
- **Uitgeschakelde** -deze instelling zorgt ervoor dat de gebruiker uit te voeren via de gehele gebruikersstroom op elke uitvoering van het beleid. Bijvoorbeeld: Hierdoor kunnen meerdere gebruikers voor het aanmelden bij uw toepassing (in een gedeelde bureaublad scenario), zelfs bij een enkele gebruiker blijft aangemeld gedurende de hele tijd.

Deze instellingen zijn niet beschikbaar voor wachtwoord opnieuw instellen van gebruikersstromen. 


---
title: Token-, sessie en configuratie voor eenmalige aanmelding in Azure Active Directory B2C | Microsoft Docs
description: Token-, sessie en configuratie voor eenmalige aanmelding in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/16/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 061e2257200b6d660a421a86c540f43597112c5e
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43337882"
---
# <a name="azure-active-directory-b2c-token-session-and-single-sign-on-configuration"></a>Azure Active Directory B2C: Token-, sessie en configuratie voor eenmalige aanmelding

Hiermee kunt u heel nauwkeurig bepalen, op een [per beleid basis](active-directory-b2c-reference-policies.md), van:

1. Levensduur van beveiligingstokens die door Azure Active Directory (Azure AD) B2C wordt verzonden.
2. Levensduur van web application sessies worden beheerd door Azure AD B2C.
3. De indelingen van belangrijke claims in de beveiligingstokens die door Azure AD B2C wordt verzonden.
4. Eenmalige aanmelding (SSO) het gedrag voor meerdere apps en beleidsregels in uw B2C-tenant.

Voor de ingebouwde beleidsregels, kunt u deze functie in uw Azure AD B2C-directory als volgt:

1. Volg deze stappen om [gaat u naar het menu met B2C functies](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in Azure portal.
2. Klik op **beleid voor registreren of aanmelden**. * Opmerking: U kunt deze functie op elk gewenst beleidstype niet alleen op ** registreren of aanmelden beleid ***.
3. Open een beleid door erop te klikken. Bijvoorbeeld, klikt u op **B2C_1_SiUpIn**.
4. Klik op **bewerken** aan de bovenkant van het menu.
5. Klik op **Token-, sessie- en configuratie voor eenmalige aanmelding**.
6. Breng de gewenste wijzigingen. Meer informatie over de beschikbare eigenschappen in de volgende secties.
7. Klik op **OK**.
8. Klik op **opslaan** boven aan het menu.

## <a name="token-lifetimes-configuration"></a>Configuratie van de levensduur van tokens

Azure AD B2C ondersteunt de [OAuth 2.0-protocol voor autorisatie](active-directory-b2c-reference-protocols.md) voor het inschakelen van beveiligde toegang tot beveiligde bronnen. Voor het implementeren van deze ondersteuning, Azure AD B2C verzendt verschillende [beveiligingstokens](active-directory-b2c-reference-tokens.md). Dit zijn de eigenschappen die u gebruiken kunt voor het beheren van de levensduur van beveiligingstokens die zijn gegenereerd door Azure AD B2C:

* **Toegang en ID-token levensduur (minuten)**: de levensduur van het OAuth 2.0-bearer-token gebruikt voor toegang tot een beveiligde resource.
  * Standaard = 60 minuten.
  * Minimum (inclusief) = 5 minuten.
  * Maximum aantal (inclusief) = 1440 minuten.
* **Levensduur van vernieuwingstoken (dagen)**: de maximale periode waarbinnen een vernieuwingstoken kan worden gebruikt voor het verkrijgen van een nieuwe toegang of de ID-token (en eventueel een nieuwe vernieuwingstoken, als uw toepassing is verleend de `offline_access` bereik).
  * Standaardinstelling = 14 dagen.
  * Minimum (inclusief) = 1 dag.
  * Maximum aantal (inclusief) = 90 dagen.
* **Levensduur van sliding window token vernieuwen (dagen)**: nadat deze periode is verstreken voor de gebruiker wordt geforceerd opnieuw worden geverifieerd, ongeacht de geldigheidsperiode van de meest recente vernieuwingstoken verkregen door de toepassing. Kan alleen worden opgegeven als de switch is ingesteld op **gebonden**. Het moet groter zijn dan of gelijk zijn aan de **levensduur van vernieuwingstoken (dagen)** waarde. Als de switch is ingesteld op **Unbounded**, u niet een bepaalde waarde opgeven.
  * Standaardwaarde = 90 dagen.
  * Minimum (inclusief) = 1 dag.
  * Maximum aantal (inclusief) = 365 dagen.

Dit zijn een aantal van use cases die u kunt inschakelen met behulp van deze eigenschappen:

* Toestaan dat een gebruiker om te blijven voor onbepaalde tijd, in een mobiele toepassing, zolang hij of zij op de toepassing voortdurend actief is. U kunt dit doen door in te stellen de **Refresh token levensduur van sliding window (dagen)** overschakelen naar **Unbounded** in uw beleid voor aanmelden.
* Voldoen aan uw toonaangevende beveiligings- en nalevingsvereisten door in te stellen van de levensduur van de juiste toegang-tokens.

    > [!NOTE]
    > Deze instellingen zijn niet beschikbaar voor wachtwoord opnieuw instellen van beleidsregels.
    > 
    > 

## <a name="token-compatibility-settings"></a>Instellingen voor tokencompatibiliteit

We opmaakwijzigingen aangebracht in belangrijke claims in beveiligingstokens verzonden door Azure AD B2C. Dit is gedaan ter verbetering van de standaard-protocol-ondersteuning en voor betere interoperabiliteit met derden identity-bibliotheken. Echter om te voorkomen dat bestaande apps te analyseren, maken we de volgende eigenschappen om te kunnen aanmelden indien nodig klanten:

* **Claim van verlener (iss)**: Hiermee wordt aangegeven met de Azure AD B2C-tenant die het token is uitgegeven.
  * `https://<domain>/{B2C tenant GUID}/v2.0/`: Dit is de standaardwaarde.
  * `https://<domain>/tfp/{B2C tenant GUID}/{Policy ID}/v2.0/`: Id's bevat deze waarde voor zowel de B2C-tenant en het beleid dat wordt gebruikt in het token aan te vragen. Als uw app of de tapewisselaar moet Azure AD B2C om te voldoen aan de [OpenID Connect Discovery 1.0-specificatie](http://openid.net/specs/openid-connect-discovery-1_0.html), gebruikt deze waarde.
* **Onderwerp (sub) claim**: Hiermee wordt aangegeven met de entiteit, dat wil zeggen, de gebruiker die het token worden bevestigd met informatie.
  * **Object-id**: dit is de standaardwaarde. Het wordt gevuld met de object-ID van de gebruiker in de map in de `sub` claim in het token.
  * **Niet ondersteund**: dit is alleen beschikbaar voor neerwaartse compatibiliteit, en het is raadzaam dat u naar overschakelt **ObjectID** zodra u zich kunt.
* **Claim voor beleids-ID**: Hiermee wordt aangegeven met het claimtype waarin de beleids-ID die wordt gebruikt in de aanvraag voor een toegangstoken wordt gevuld.
  * **TFP**: dit is de standaardwaarde.
  * **ACR**: dit is alleen beschikbaar voor neerwaartse compatibiliteit, en het is raadzaam dat u naar overschakelt `tfp` zodra u zich kunt.

## <a name="session-behavior"></a>Sessiegedrag

Azure AD B2C ondersteunt de [OpenID Connect-verificatieprotocol](active-directory-b2c-reference-oidc.md) voor het inschakelen van beveiligde aanmelding tot webtoepassingen. Dit zijn de eigenschappen die u gebruiken kunt voor het beheren van web application sessies:

* **Web-appsessie (minuten)**: de levensduur van Azure AD B2C-sessiecookie die zijn opgeslagen op de browser van de gebruiker bij een geslaagde verificatie.
  * Standaardinstelling = 1440 minuten.
  * Minimum (inclusief) = 15 minuten.
  * Maximum aantal (inclusief) = 1440 minuten.
* **Web-appsessie**: als deze schakeloptie is ingesteld op **Absolute**, de gebruiker wordt geforceerd opnieuw worden geverifieerd na de periode die is opgegeven door **Web-appsessie (minuten)** is verstreken. Als deze schakeloptie is ingesteld op **Rolling** (de standaardinstelling), de gebruiker blijft aangemeld zolang de gebruiker voortdurend actief zijn in uw webtoepassing is.

Dit zijn een aantal van use cases die u kunt inschakelen met behulp van deze eigenschappen:

* Voldoen aan de beveiligings- en vereisten van uw branche door in te stellen van de juiste toepassing websessie levensduur.
* Geforceerd opnieuw worden geverifieerd na een bepaalde tijd instellen tijdens een gebruikersinteractie met hoogwaardige beveiliging onderdeel van uw webtoepassing. 

    > [!NOTE]
    > Deze instellingen zijn niet beschikbaar voor wachtwoord opnieuw instellen van beleidsregels.
    > 
    > 

## <a name="single-sign-on-sso-configuration"></a>Configuratie van eenmalige aanmelding (SSO)
Als u meerdere toepassingen en het beleid in uw B2C-tenant hebt, kunt u de interactie van gebruikers beheren binnen deze met behulp van de **configuratie voor eenmalige aanmelding** eigenschap. U kunt de eigenschap instellen op een van de volgende instellingen:

* **Tenant**: dit is de standaardinstelling. Met deze instelling kan meerdere toepassingen en het beleid in uw B2C-tenant voor het delen van dezelfde sessie van de gebruiker. Bijvoorbeeld, wanneer een gebruiker zich in een toepassing, Contoso winkelen, hij of zij kan ook naadloos zich aanmelden bij een andere ene, Contoso geneesmiddelen, bij toegang tot het.
* **Toepassing**: Hiermee kunt u een gebruikerssessie uitsluitend bedoeld voor een toepassing, onafhankelijk van andere toepassingen te onderhouden. Bijvoorbeeld, als u wilt dat de gebruiker zich aanmeldt bij Contoso geneesmiddelen (met dezelfde referenties), zelfs als hij of zij is al aangemeld bij Contoso winkelen, een andere toepassing op dezelfde B2C-tenant. 
* **Beleid**: Hiermee kunt u een gebruikerssessie uitsluitend bedoeld voor een beleid, onafhankelijk van de toepassingen te onderhouden. Bijvoorbeeld, als de gebruiker is al aangemeld en een multi-factor authentication (MFA) stap voltooid, kan hij of zij toegang krijgen tot delen van betere beveiliging van meerdere toepassingen, zolang de sessie die is gekoppeld aan het beleid niet verloopt.
* **Uitgeschakelde**: dit zorgt ervoor dat de gebruiker uit te voeren via de gehele gebruikersbeleving op elke uitvoering van het beleid. Bijvoorbeeld: Hierdoor kunnen meerdere gebruikers voor het aanmelden bij uw toepassing (in een gedeelde bureaublad scenario), zelfs bij een enkele gebruiker blijft aangemeld gedurende de hele tijd.

    > [!NOTE]
    > Deze instellingen zijn niet beschikbaar voor wachtwoord opnieuw instellen van beleidsregels.
    > 
    > 


---
title: Token, sessie en eenmalige aanmelding-configuratie - Azure AD B2C | Microsoft Docs
description: Token, sessie en configuratie voor eenmalige aanmelding in Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: mtillman
editor: parakhj
ms.assetid: e78e6344-0089-49bf-8c7b-5f634326f58c
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: parakhj
ms.openlocfilehash: 76feb172ce9b08d4feef9c86e74a592553a3c7f4
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-token-session-and-single-sign-on-configuration"></a>Azure Active Directory B2C: Token, sessie en configuratie voor één aanmelding

Deze functie kunt u fijnmazig bepalen, op een [per beleid basis](active-directory-b2c-reference-policies.md), van:

1. De levensduur van beveiligingstokens die door Azure Active Directory (Azure AD) B2C.
2. De levensduur van web application sessies worden beheerd door Azure AD B2C.
3. De indelingen van belangrijke claims in de beveiligingstokens die door Azure AD B2C.
4. Eenmalige aanmelding (SSO) de gedrag voor meerdere apps en beleidsregels in uw B2C-tenant.

Voor ingebouwde beleidsregels, kunt u deze functie in uw Azure AD B2C-directory als volgt:

1. Volg deze stappen voor [gaat u naar het menu B2C-functies](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in de Azure portal.
2. Klik op **registreren of aanmelden beleid**. *Opmerking: U kunt deze functie op elk beleidstype niet alleen op **registreren of aanmelden beleid***.
3. Open een beleid door erop te klikken. Bijvoorbeeld, klikt u op **B2C_1_SiUpIn**.
4. Klik op **bewerken** aan de bovenkant van het menu.
5. Klik op **Token, sessie & eenmalige aanmelding config**.
6. Breng de gewenste wijzigingen. Meer informatie over de beschikbare eigenschappen in de volgende secties.
7. Klik op **OK**.
8. Klik op **opslaan** boven aan het menu.

## <a name="token-lifetimes-configuration"></a>Configuratie van de levensduur van token

Azure AD B2C ondersteunt de [OAuth 2.0-protocol voor autorisatie](active-directory-b2c-reference-protocols.md) voor het inschakelen van beveiligde toegang tot beveiligde bronnen. Voor het implementeren van deze ondersteuning voor Azure AD B2C verzendt verschillende [beveiligingstokens](active-directory-b2c-reference-tokens.md). Dit zijn de eigenschappen die u voor het beheren van de levensduur van beveiligingstokens die door Azure AD B2C kunt gebruiken:

* **Toegang tot & ID token levensduur (minuten)**: de levensduur van het OAuth 2.0-bearer-token gebruikt voor toegang tot een beveiligde bron.
  * Standaard = 60 minuten.
  * Minimaal (inclusief) = 5 minuten.
  * Maximum (inclusief) = 1440 minuten.
* **Vernieuwen van de levensduur van token (dagen)**: de maximale periode waarvoor een vernieuwingstoken kan worden gebruikt voor het verkrijgen van een nieuwe toegang of het token ID (en desgewenst een nieuwe vernieuwingstoken, als uw toepassing had gekregen de `offline_access` scope).
  * Standaardinstelling = 14 dagen.
  * Minimaal (inclusief) = 1 dag.
  * Maximum (inclusief) = 90 dagen.
* **Vernieuwen van de levensduur van token verschuivende venster (dagen)**: nadat deze periode is verstreken van de gebruiker moet opnieuw worden geverifieerd, ongeacht de geldigheidsduur van de meest recente vernieuwen token verkregen door de toepassing. Kan alleen worden opgegeven als de switch is ingesteld op **dat wordt begrensd**. Het moet groter zijn dan of gelijk zijn aan de **levensduur van token vernieuwen (dagen)** waarde. Als de switch is ingesteld op **Unbounded**, u niet een bepaalde waarde opgeven.
  * Standaardwaarde = 90 dagen.
  * Minimaal (inclusief) = 1 dag.
  * Maximum (inclusief) = 365 dagen.

Dit zijn enkele gebruiksvoorbeelden die u kunt inschakelen met behulp van deze eigenschappen:

* Toestaan dat een gebruiker voor onbepaalde tijd aangemeld in een mobiele toepassing blijven zolang hij of zij voortdurend actief op de toepassing is. U kunt dit doen door in te stellen de **vernieuwen verschuivende venster levensduur van token (dagen)** overschakelen naar **Unbounded** in uw beleid voor aanmelden.
* Voldoen aan uw branche beveiligings- en nalevingsvereisten door in te stellen van de juiste toegangsrechten token levensduur.

    > [!NOTE]
    > Deze instellingen zijn niet beschikbaar voor wachtwoord opnieuw instellen van beleidsregels.
    > 
    > 

## <a name="token-compatibility-settings"></a>Token compatibiliteitsinstellingen

Er opmaakwijzigingen aangebracht om belangrijke claims in beveiligingstokens die door Azure AD B2C. Dit is gedaan ter verbetering van onze support standaardprotocol en voor een betere compatibiliteit met bibliotheken van de identiteit van de derde partij. Echter, om te voorkomen dat bestaande apps, gemaakt we de volgende eigenschappen zodat klanten aanmelden indien nodig:

* **Certificaatverlener (iss) claim**: Hiermee wordt aangeduid met de Azure AD B2C-tenant die het token heeft uitgegeven.
  * `https://login.microsoftonline.com/{B2C tenant GUID}/v2.0/`: Dit is de standaardwaarde.
  * `https://login.microsoftonline.com/tfp/{B2C tenant GUID}/{Policy ID}/v2.0/`: Id's bevat deze waarde voor zowel de B2C-tenant en het beleid dat wordt gebruikt in de aanvraag-token. Als uw app of de tapewisselaar Azure AD B2C moet te voldoen aan de [OpenID Connect detectie 1.0-specificatie](http://openid.net/specs/openid-connect-discovery-1_0.html), gebruikt deze waarde.
* **Onderwerpnaam (sub) claim**: Hiermee wordt de entiteit, dat wil zeggen, de gebruiker, waarvoor het token asserts informatie geïdentificeerd.
  * **ObjectID**: dit is de standaardwaarde. Het wordt gevuld met de object-ID van de gebruiker in de map in de `sub` claim in het token.
  * **Niet ondersteund**: dit is alleen beschikbaar voor neerwaartse compatibiliteit en het is raadzaam dat u naar overschakelt **ObjectID** zodra u zich kunt.
* **Claim die beleids-ID vertegenwoordigt**: Hiermee wordt aangeduid met het claimtype waarin de beleids-ID die is gebruikt in de aanvraag-token is gevuld.
  * **TFP**: dit is de standaardwaarde.
  * **ACR**: dit is alleen beschikbaar voor neerwaartse compatibiliteit en het is raadzaam dat u naar overschakelt `tfp` zodra u zich kunt.

## <a name="session-behavior"></a>Sessie-gedrag

Azure AD B2C ondersteunt de [OpenID Connect-verificatieprotocol](active-directory-b2c-reference-oidc.md) voor het inschakelen van beveiligde aanmelding bij webtoepassingen. Dit zijn de eigenschappen die u gebruiken kunt voor het beheren van de sessies van een toepassing:

* **Sessie-levensduur (minuten) van de Web-app**: de levensduur van Azure AD B2C sessiecookie opgeslagen op de browser van de gebruiker bij de verificatie is geslaagd.
  * Standaardinstelling = 1440 minuten.
  * Minimaal (inclusief) = 15 minuten.
  * Maximum (inclusief) = 1440 minuten.
* **Web-app sessietime-out**: als deze switch is ingesteld op **Absolute**, de gebruiker moet opnieuw worden geverifieerd na de periode die is opgegeven door **Web-app sessie levensduur (minuten)** is verstreken. Als deze switch is ingesteld op **rollend** (de standaardinstelling), de gebruiker blijft aangemeld, zolang de gebruiker in uw webtoepassing voortdurend actief is.

Dit zijn enkele gebruiksvoorbeelden die u kunt inschakelen met behulp van deze eigenschappen:

* Voldoen aan uw branche beveiliging en naleving door het instellen van de juiste toepassing websessie levensduur.
* Herauthenticatie na een gegeven periode tijdens gebruikersinteractie met hoogwaardige beveiliging deel uit van uw webtoepassing te forceren. 

    > [!NOTE]
    > Deze instellingen zijn niet beschikbaar voor wachtwoord opnieuw instellen van beleidsregels.
    > 
    > 

## <a name="single-sign-on-sso-configuration"></a>Configuratie van eenmalige aanmelding (SSO)
Als u meerdere toepassingen en het beleid in uw B2C-tenant hebt, kunt u de interactie van gebruikers beheren onder te brengen met behulp van de **configuratie voor één aanmelding** eigenschap. U kunt de eigenschap instellen op een van de volgende instellingen:

* **Tenant**: dit is de standaardinstelling. Met deze instelling kan meerdere toepassingen en beleidsregels in uw B2C-tenant voor het delen van dezelfde sessie van de gebruiker. Bijvoorbeeld wanneer een gebruiker zich in een toepassing, Contoso winkelen hij of zij kan ook naadloos Meld u aan bij een andere afbeelding, Contoso faculteit, bij toegang tot deze.
* **Toepassing**: Hiermee kunt u een gebruikerssessie uitsluitend bedoeld is voor een toepassing, onafhankelijk van andere toepassingen te onderhouden. Bijvoorbeeld, als u wilt dat de gebruiker zich aanmeldt bij de faculteit van Contoso (met dezelfde aanmeldingsgegevens), zelfs als hij of zij is al aangemeld bij Contoso winkelen, een andere toepassing op dezelfde B2C-tenant. 
* **Beleid**: Hiermee kunt u een gebruikerssessie uitsluitend bedoeld is voor een beleid, onafhankelijk van de toepassingen met behulp van het onderhouden. Bijvoorbeeld, als de gebruiker is al aangemeld en een multi-factor authentication (MFA) stap voltooid, kan hij of zij toegang worden verleend voor het delen van betere beveiliging van meerdere toepassingen, zolang de sessie die is gekoppeld aan het beleid niet verloopt.
* **Uitgeschakelde**: dit zorgt ervoor dat de gebruiker worden uitgevoerd via het traject gehele gebruikersobject op elke uitvoering van het beleid. Bijvoorbeeld Hierdoor kunnen meerdere gebruikers zich aanmelden voor uw toepassing (in een gedeelde bureaublad scenario), zelfs bij een enkele gebruiker blijft aangemeld tijdens de gehele periode.

    > [!NOTE]
    > Deze instellingen zijn niet beschikbaar voor wachtwoord opnieuw instellen van beleidsregels.
    > 
    > 


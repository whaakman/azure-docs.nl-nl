---
title: Sessie en eenmalige aanmelding-configuratie - Azure Active Directory B2C | Microsoft Docs
description: Sessie en configuratie voor eenmalige aanmelding in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 674a20fc96cf5b86219222d746525a3559ae9d09
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59681094"
---
# <a name="session-and-single-sign-on-configuration-in-azure-active-directory-b2c"></a>Sessie- en configuratie voor eenmalige aanmelding in Azure Active Directory B2C

Hiermee kunt u heel nauwkeurig bepalen, op een [stroom gebruikersbasis](active-directory-b2c-reference-policies.md), van:

- Levensduur van web application sessies worden beheerd door Azure AD B2C.
- Eenmalige aanmelding (SSO) gedrag voor meerdere apps en gebruikersstromen in uw Azure AD B2C-tenant.

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
- **Uitgeschakelde** -deze instelling zorgt ervoor dat de gebruiker uit te voeren via de gehele gebruikersstroom op elke uitvoering van het beleid.

Deze instellingen zijn niet beschikbaar voor wachtwoord opnieuw instellen van gebruikersstromen. 


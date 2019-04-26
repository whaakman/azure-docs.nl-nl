---
title: Configureren van sessiegedrag - Azure Active Directory B2C | Microsoft Docs
description: Sessiegedrag van de configureren in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 1140a3cbb43e86bf222c73c95a03b11871f7a2d0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60360420"
---
# <a name="configure-session-behavior-in-azure-active-directory-b2c"></a>Sessiegedrag configureren in Azure Active Directory B2C

Hiermee kunt u heel nauwkeurig bepalen, op een [stroom gebruikersbasis](active-directory-b2c-reference-policies.md), van:

- Levensduur van web application sessies worden beheerd door Azure AD B2C.
- Eenmalige aanmelding (SSO) gedrag voor meerdere apps en gebruikersstromen in uw Azure AD B2C-tenant.

Deze instellingen zijn niet beschikbaar voor wachtwoord opnieuw instellen van gebruikersstromen.

Azure AD B2C ondersteunt de [OpenID Connect-verificatieprotocol](active-directory-b2c-reference-oidc.md) voor het inschakelen van beveiligde aanmelding tot webtoepassingen. U kunt de volgende eigenschappen voor het beheren van web application sessies:

## <a name="session-behavior-properties"></a>Eigenschappen van de sessie-gedrag

- **Web-appsessie (minuten)** - de levensduur van Azure AD B2C-sessiecookie die zijn opgeslagen op de browser van de gebruiker bij een geslaagde verificatie.
    - Standaardinstelling = 1440 minuten.
    - Minimum (inclusief) = 15 minuten.
    - Maximum aantal (inclusief) = 1440 minuten.
- **Web-appsessie** : als deze schakeloptie is ingesteld op **Absolute**, de gebruiker wordt gedwongen om te verifiëren opnieuw na de periode die is opgegeven door **Web-appsessie (minuten)** is verstreken. Als deze schakeloptie is ingesteld op **Rolling** (de standaardinstelling), de gebruiker blijft aangemeld zolang de gebruiker voortdurend actief zijn in uw webtoepassing is.
- **Configuratie voor eenmalige aanmelding** als u meerdere toepassingen en gebruikersstromen in uw B2C-tenant hebt, kunt u de interactie van gebruikers beheren binnen deze met behulp van de **configuratie voor eenmalige aanmelding** eigenschap. U kunt de eigenschap instellen op een van de volgende instellingen:
    - **Tenant** -dit is de standaardinstelling. Met deze instelling kan meerdere toepassingen en gebruiker stromen in uw B2C-tenant voor het delen van dezelfde sessie van de gebruiker. Bijvoorbeeld, wanneer een gebruiker zich in een toepassing, de gebruiker kan ook naadloos zich aanmelden bij een andere ene, Contoso geneesmiddelen, bij toegang tot het.
    - **Toepassing** -deze instelling kunt u een gebruikerssessie uitsluitend bedoeld voor een toepassing, onafhankelijk van andere toepassingen te onderhouden. Bijvoorbeeld, als u wilt dat de gebruiker zich aanmeldt bij Contoso geneesmiddelen (met dezelfde referenties), zelfs als de gebruiker is al aangemeld bij Contoso winkelen, een andere toepassing op dezelfde B2C-tenant. 
    - **Beleid** -deze instelling kunt u een gebruikerssessie uitsluitend bedoeld voor een gebruikersstroom, onafhankelijk van de toepassingen te onderhouden. Bijvoorbeeld, als de gebruiker is al aangemeld en een multi-factor authentication (MFA) stap voltooid, kan de gebruiker toegang krijgen tot delen van betere beveiliging van meerdere toepassingen, zolang de sessie die is gekoppeld aan de gebruikersstroom niet verloopt.
    - **Uitgeschakelde** -deze instelling zorgt ervoor dat de gebruiker uit te voeren via de gehele gebruikersstroom op elke uitvoering van het beleid.

De volgende gevallen gebruik worden ingeschakeld met behulp van deze eigenschappen:

- Voldoen aan de beveiligings- en vereisten van uw branche door in te stellen van de juiste toepassing websessie levensduur.
- De verificatie van de Force na een bepaalde tijd instellen tijdens een gebruikersinteractie met hoogwaardige beveiliging onderdeel van uw webtoepassing. 

## <a name="configure-the-properties"></a>De eigenschappen configureren

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map met uw Azure AD B2C-tenant te kiezen.
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **gebruikersstromen (beleid)**.
5. Open het beleid dat u eerder hebt gemaakt. 
6. Selecteer **eigenschappen**.
7. Configureer **Web-appsessie (minuten)**, **Web-appsessie**, **configuratie voor eenmalige aanmelding**, en **id-Token vereisen in afmeldingsaanvragen**  indien nodig.

    ![Sessiegedrag configureren](./media/session-behavior/session-behavior.png)
    
8. Klik op **Opslaan**.




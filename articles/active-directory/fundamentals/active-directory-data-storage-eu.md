---
title: Waar slaat Azure AD identiteitsgegevens op voor Europese klanten? | Microsoft Docs
description: Meer informatie over waar identiteitsgegevens in Microsoft Azure Active Directory worden opgeslagen voor de Europese klanten.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.author: lizross
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 05/17/2018
ms.custom: it-pro
ms.openlocfilehash: 0baa499f56fa6c074ac1c0f604e74f9e7adb5502
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42024034"
---
# <a name="where-does-microsoft-azure-active-directory-azure-ad-store-identity-data-for-european-customers"></a>Waar slaat Microsoft Azure Active Directory (Azure AD) identiteitsgegevens op voor Europese klanten?
Azure Active Directory helpt u gebruikersidentiteiten te beheren en op informatie gebaseerd toegangsbeleid te maken om de resources van uw organisatie te beveiligen. Identiteitsgegevens worden opgeslagen op een locatie die uw bedrijf heeft opgegeven toen u zich hebt aangemeld bij de service. Bijvoorbeeld bij het abonneren op Office 365 of Azure. Voor specifieke informatie over waar uw identiteitsgegevens worden opgeslagen, kunt u het gedeelte [Waar bevinden uw gegevens zich?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) van het Microsoft Trust Center gebruiken.

De meeste Azure Active Directory-gerelateerde Europese identiteitsgegevens blijven in Europese datacenters, maar er zijn vijf kenmerken voor gebruikers die meestal in Amerikaanse datacentra opgeslagen worden. Deze kenmerken zijn GivenName, Surname, userPrincipalName, Domain en PasswordHash. Het kenmerk PasswordHash kan een uitzondering zijn en niet in de Verenigde Staten zijn opgeslagen als iemand gebruikmaakt van een on-premises, federatieve verificatiemethode waarmee het synchroniseren van de waarde PasswordHash met Azure Active Directory kan worden gestopt. Er zijn bovendien nog operationele, servicegebonden gegevens die zijn vereist om Azure Active Directory normaal te laten werken die worden opgeslagen in de Verenigde Staten en geen persoonlijke gegevens bevatten.

## <a name="data-stored-outside-of-european-datacenters-for-european-customers"></a>Gegevens die zijn opgeslagen buiten de Europese datacenters voor Europese klanten

De meeste Azure Active Directory-gerelateerde Europese identiteitsgegevens, voor organisaties met adressen in Europa, blijven in Europese datacenters. Azure Active Directory-gegevens die niet worden opgeslagen in Europese datacenters zijn onder meer:

- **Identiteitsgerelateerde kenmerken**

    De volgende identiteitsgerelateerde kenmerken worden gerepliceerd naar de Verenigde Staten:

    - GivenName
    - Achternaam
    - userPrincipalName
    - Domain
    - PasswordHash
    - SourceAnchor
    - AccountEnabled
    - PasswordPolicies
    - StrongAuthenticationRequirement
    - ApplicationPassword
    - PUID

- **Microsoft Azure Multi-Factor Authentication (MFA) en Azure AD Self-Service Password Reset (SSPR)**
    
    MFA slaat alle gebruikersgegevens in rust op in Europese datacenters. Bepaalde gegevens die specifiek zijn voor MFA worden echter opgeslagen in de Verenigde Staten, met inbegrip van:
    
    - Tweeledige verificatie en de bijbehorende persoonlijke gegevens worden mogelijk opgeslagen in de VS als u MFA of SSPR gebruikt.
        - Alle tweeledige verificatie met behulp van telefoongesprekken of SMS kan worden uitgevoerd door Amerikaanse providers.
        - Pushmeldingen verzenden met behulp van de Microsoft Authenticator-app vereist meldingen van een notificatieservice van de fabrikant (Apple of Google), die zich mogelijk buiten Europa bevindt.
        - OATH-codes worden altijd gevalideerd in de Verenigde Staten. 
    - Sommige MFA- en SSPR-logboeken worden voor 30 dagen opgeslagen in de Verenigde Staten, ongeacht het verificatietype.

- **Microsoft Azure Active Directory B2C (Azure AD B2C)**

    Azure AD B2C slaat alle gebruikersgegevens in rust op in Europese datacenters. Operationele logboeken (zonder persoonlijke gegevens) blijven echter op de locatie vanwaar de persoon toegang heeft tot de services. Bijvoorbeeld, als een B2C-gebruiker toegang heeft tot de service in de Verenigde Staten, blijven de operationele logboeken in de Verenigde Staten. Bovendien worden alle beleidsgegevens voor de configuratie zonder persoonsgegevens alleen in de Verenigde Staten opgeslagen. Zie voor meer informatie over configuraties voor beleid het artikel [Azure Active Directory B2C: ingebouwde beleidsregels](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies).

- **Microsoft Azure Active Directory B2B (Azure AD B2B)** 
    
    Azure Active Directory B2B slaat alle gebruikersgegevens in rust op in Europese datacenters. B2B slaat echter de niet-persoonlijke metagegevens op in tabellen in Amerikaanse datacentra. Deze tabel bevat velden als redeemUrl, invitationTicket, resource tenant Id, InviteRedirectUrl en InviterAppId.

- **Microsoft Azure Active Directory Domain Services (Azure AD DS)**

    Azure Active Directory Domain Services slaat gebruikersgegevens op dezelfde locatie op als het door de klant geselecteerde Azure Virtual Network. Dus als het netwerk zich buiten Europa bevindt, worden de gegevens gerepliceerd en opgeslagen buiten Europa.

- **Services en toepassingen die zijn geïntegreerd met Azure Active Directory**

    Alle services en apps die met Azure Active Directory zijn geïntegreerd, hebben toegang tot identiteitsgegevens. Evalueer elke service en app om te bepalen hoe identiteitsgegevens worden verwerkt door die specifieke service en app en of ze voldoen aan de vereisten voor gegevensopslag van uw bedrijf.

    Zie de sectie [Waar bevinden uw gegevens zich?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) van het Microsoft Trust Center voor meer informatie over de gegevenslocatie van Microsoft-services.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over een van de functies en functionaliteiten die hierboven worden beschreven.
- [Aan de slag met Azure Active Directory](get-started-azure-ad.md)
- [Wat is Multi-Factor Authentication?](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication)
- [Azure Active Directory Self-Service Password Reset](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-passwords-overview)
- [Wat is Azure Active Directory B2C?](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)
- [Wat is Azure AD B2B-samenwerking?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)
- [Azure Active Directory (AD) Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview)

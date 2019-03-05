---
title: Identiteit gegevensopslag voor Europese klanten - Azure Active Directory | Microsoft Docs
description: Meer informatie over waar identiteit gerelateerde gegevens in Azure Active Directory worden opgeslagen voor de Europese klanten.
services: active-directory
author: eross-msft
manager: daveba
ms.author: lizross
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 03/04/2019
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: b21f82dc0a1eb8edf571da13e0d34fecae5f401b
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57337682"
---
# <a name="identity-data-storage-for-european-customers-in-azure-active-directory"></a>Identiteit gegevensopslag voor Europese klanten in Azure Active Directory
Azure Active Directory (Azure AD) helpt u bij het beheren van gebruikers-id's en informatie gebaseerd toegangsbeleid die helpen bij het beveiligen van resources van uw organisatie maken. Identiteitsgegevens worden opgeslagen op een locatie die uw bedrijf heeft opgegeven toen u zich hebt aangemeld bij de service. Bijvoorbeeld bij het abonneren op Office 365 of Azure. Voor specifieke informatie over waar uw identiteitsgegevens worden opgeslagen, kunt u het gedeelte [Waar bevinden uw gegevens zich?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) van het Microsoft Trust Center gebruiken.

De meeste Azure AD-gerelateerde Europese identiteitsgegevens blijft in Europese datacenters, maar er zijn enkele operationele, servicespecifieke gegevens die zijn voor normale vereist Azure AD-bewerking, die worden opgeslagen in de Verenigde Staten en bevat geen persoonlijke gegevens.

## <a name="data-stored-outside-of-european-datacenters-for-european-customers"></a>Gegevens die zijn opgeslagen buiten de Europese datacenters voor Europese klanten

De meeste Azure Active Directory-gerelateerde Europese identiteitsgegevens, voor organisaties met adressen in Europa, blijven in Europese datacenters. Azure AD-gegevens die worden opgeslagen in Europese datacenters, en tevens worden gerepliceerd naar de datacenters in de Verenigde Staten, omvat:

- **Microsoft Azure Multi-Factor Authentication (MFA) en Azure AD Self-Service Password Reset (SSPR)**
    
    MFA slaat alle gebruikersgegevens in rust op in Europese datacenters. Bepaalde gegevens die specifiek zijn voor MFA worden echter opgeslagen in de Verenigde Staten, met inbegrip van:
    
    - Tweeledige verificatie en de bijbehorende persoonlijke gegevens worden mogelijk opgeslagen in de VS als u MFA of SSPR gebruikt.

        - Alle tweeledige verificatie met behulp van telefoongesprekken of SMS kan worden uitgevoerd door Amerikaanse providers.
    
        - Pushmeldingen verzenden met behulp van de Microsoft Authenticator-app vereist meldingen van een notificatieservice van de fabrikant (Apple of Google), die zich mogelijk buiten Europa bevindt.
    
        - OATH-codes worden altijd gevalideerd in de Verenigde Staten. 
    
    - Sommige MFA- en SSPR-logboeken worden voor 30 dagen opgeslagen in de Verenigde Staten, ongeacht het verificatietype.

- **Microsoft Azure Active Directory B2C (Azure AD B2C)**

    Azure AD B2C slaat alle gebruikersgegevens in rust op in Europese datacenters. Operationele logboeken (zonder persoonlijke gegevens) blijven echter op de locatie vanwaar de persoon toegang heeft tot de services. Bijvoorbeeld, als een B2C-gebruiker toegang heeft tot de service in de Verenigde Staten, blijven de operationele logboeken in de Verenigde Staten. Bovendien worden alle beleidsgegevens voor de configuratie zonder persoonsgegevens alleen in de Verenigde Staten opgeslagen. Zie voor meer informatie over configuraties voor beleid voor de [Azure Active Directory B2C: Ingebouwd beleid](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies) artikel.

- **Microsoft Azure Active Directory B2B (Azure AD B2B)** 
    
    Azure Active Directory B2B slaat alle gebruikersgegevens in rust op in Europese datacenters. B2B slaat echter de niet-persoonlijke metagegevens op in tabellen in Amerikaanse datacentra. Deze tabel bevat velden als redeemUrl, invitationTicket, resource tenant Id, InviteRedirectUrl en InviterAppId.

- **Microsoft Azure Active Directory Domain Services (Azure AD DS)**

    Azure Active Directory Domain Services slaat gebruikersgegevens op dezelfde locatie op als het door de klant geselecteerde Azure Virtual Network. Dus als het netwerk zich buiten Europa bevindt, worden de gegevens gerepliceerd en opgeslagen buiten Europa.

- **Services en toepassingen die zijn geïntegreerd met Azure Active Directory**

    Alle services en apps die met Azure Active Directory zijn geïntegreerd, hebben toegang tot identiteitsgegevens. Evalueer elke service en app om te bepalen hoe identiteitsgegevens worden verwerkt door die specifieke service en app en of ze voldoen aan de vereisten voor gegevensopslag van uw bedrijf.

    Zie de sectie [Waar bevinden uw gegevens zich?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) van het Microsoft Trust Center voor meer informatie over de gegevenslocatie van Microsoft-services.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over een van de functies en functionaliteit die hierboven worden beschreven:
- [Wat is Multi-Factor Authentication?](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication)

- [Azure Active Directory Self-Service Password Reset](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-passwords-overview)

- [Wat is Azure Active Directory B2C?](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)

- [Wat is Azure AD B2B-samenwerking?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)

- [Azure Active Directory (AD) Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview)

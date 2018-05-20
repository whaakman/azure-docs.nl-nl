---
title: Waar Azure AD identiteitsgegevens opslaat voor Europese klanten | Microsoft Docs
description: Meer informatie over waar gegevens met betrekking tot de identiteit in Microsoft Azure Active Directory worden opgeslagen voor de Europese klanten.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.author: lizross
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2018
ms.custom: it-pro
ms.openlocfilehash: 19dc163dbb6dd296a417f5c313a36c7f7c9e50d7
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="where-does-microsoft-azure-active-directory-azure-ad-store-identity-data-for-european-customers"></a>Wanneer Microsoft Azure Active Directory (Azure AD) slaat identiteitsgegevens voor Europese klanten
Azure AD kunt u voor het beheren van gebruikers-id's en intelligence aangestuurde toegangsbeleid maken dat beter beveiligen van bronnen van uw organisatie. Id-gegevens wordt opgeslagen in een locatie die is gebaseerd op het adres van uw organisatie die is opgegeven toen u zich hebt geabonneerd met de service. Bijvoorbeeld, wanneer u zich hebt geabonneerd op Office 365 of Azure. Voor specifieke informatie over waar de identiteitsgegevens van uw is opgeslagen, kunt u de [waar zijn de gegevens zich bevinden?](https://www.microsoft.com/en-us/trustcenter/privacy/where-your-data-is-located) sectie van de Microsoft Trust Center.

De meeste Azure AD-gerelateerde Europese identiteitsgegevens blijft in Europese datacenters, maar er zijn vijf kenmerken voor gebruikers die gewoonlijk zijn opgeslagen in VS-datacenters. Deze kenmerken zijn GivenName, achternaam userPrincipalName, domein en PasswordHash. Het kenmerk PasswordHash kan een uitzondering en niet opgeslagen in de Verenigde Staten als iemand gebruikmaakt van een on-premises, federatieve verificatiemethode die Hiermee stopt u de waarde PasswordHash wordt gesynchroniseerd met Azure AD. Er is bovendien sommige operationele, service-specifieke gegevens die zijn voor normaal vereist Azure AD-bewerking die is opgeslagen in de Verenigde Staten en niet persoonlijke gegevens bevatten.

## <a name="data-stored-outside-of-european-datacenters-for-european-customers"></a>Gegevens die zijn opgeslagen buiten Europese datacenters voor Europese klanten

De meeste Azure AD-gerelateerde Europese identiteitsgegevens, voor organisaties met adressen op basis van een Europese blijft in Europese datacenters. Azure AD-gegevens die niet zijn opgeslagen in de Europese datacenters, omvat:

- **Identity-gerelateerde kenmerken**

    De volgende identiteitsgerelateerde kenmerken worden gerepliceerd naar de Verenigde Staten:

    - Voornaam
    - Achternaam
    - userPrincipalName
    - Domein
    - PasswordHash
    - sourceAnchor
    - accountEnabled
    - passwordPolicies
    - StrongAuthenticationRequirement
    - ApplicationPassword
    - PUID

- **Microsoft Azure multi-factor authentication (MFA) en Azure AD selfservice voor wachtwoordherstel (SSPR)**
    
    MFA slaat alle gebruikersgegevens in rust in Europese datacenters. Echter bepaalde MFA servicespecifieke gegevens wordt opgeslagen in de Verenigde Staten, met inbegrip van:
    
    - Verificatie met twee factoren en de bijbehorende persoonlijke gegevens worden opgeslagen in de Verenigde Staten als u MFA of SSPR.
        - Alle tweeledige verificatie via telefoongesprekken of SMS kan worden uitgevoerd door VS-providers.
        - Pushmeldingen in via de Microsoft Authenticator-app is vereist voor meldingen van de fabrikant notification-service (Apple of Google), die mogelijk buiten Europa.
        - OATH-codes worden altijd gevalideerd in de Verenigde Staten 
    - Sommige MFA en de SSPR-logboeken worden opgeslagen in de Verenigde Staten voor 30 dagen, ongeacht het verificatietype.

- **Microsoft Azure Active Directory B2C (Azure AD B2C)**

    Azure AD B2C slaat alle gebruikersgegevens in rust in Europese datacenters. Operationele Logboeken (met persoonlijke gegevens verwijderd) blijven echter op de locatie waar de persoon die toegang tot de services. Bijvoorbeeld, als een B2C-gebruiker toegang heeft tot de service in de Verenigde Staten, blijven de operationele Logboeken in de Verenigde Staten Bovendien wordt alle beleid configuratiegegevens niet met persoonlijke gegevens opgeslagen alleen in de Verenigde Staten Zie voor meer informatie over beleidsconfiguraties de [Azure Active Directory B2C: ingebouwde beleid](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-reference-policies) artikel.

- **Microsoft Azure Active Directory B2B (Azure AD B2B)** 
    
    Azure AD B2B slaat alle gebruikersgegevens in rust in Europese datacenters. B2B worden de metagegevens van de niet-persoonlijke echter opgeslagen in de tabellen in VS-datacenters. Deze tabel bevat de velden zoals redeemUrl, invitationTicket resource tenant-Id, InviteRedirectUrl en InviterAppId.

- **Microsoft Azure Active Directory Domain Services (Azure AD DS)**

    Azure AD DS slaat gebruikersgegevens in dezelfde locatie als de klant geselecteerd Azure Virtual Network. Dus als het netwerk buiten Europa is, de gegevens worden gerepliceerd en opgeslagen buiten Europa.

- **Services en apps die zijn geïntegreerd met Azure AD**

    Alle services en apps die zijn geïntegreerd met Azure AD hebben toegang tot identiteitsgegevens. Evalueren van elke service en de app om te bepalen hoe de identiteitsgegevens door die specifieke service en de app is verwerkt en of ze voldoen aan de vereisten voor gegevensopslag van uw bedrijf.

    Zie voor meer informatie over Microsoft-services de hand van gegevens vestigingsplaats de [waar zijn de gegevens zich bevinden?](https://www.microsoft.com/en-us/trustcenter/privacy/where-your-data-is-located) sectie van de Microsoft Trust Center.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de functies en functionaliteit die hierboven worden beschreven in deze artikelen.
- [Aan de slag met Azure Active Directory](get-started-azure-ad.md)
- [Wat is multi-factor Authentication?](https://docs.microsoft.com/en-us/azure/active-directory/authentication/multi-factor-authentication)
- [Azure AD selfservice voor wachtwoordherstel](https://docs.microsoft.com/en-us/azure/active-directory/authentication/active-directory-passwords-overview)
- [Wat is Azure Active Directory B2C?](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-overview)
- [Wat is Azure AD B2B-samenwerking?](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)
- [(AD) voor Azure Active Directory Domain Services](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/active-directory-ds-overview)

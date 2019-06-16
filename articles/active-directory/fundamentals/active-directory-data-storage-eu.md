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
ms.openlocfilehash: 93ac5ef5f03f800a8f90259db3e382b3bc5c5e2c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66235167"
---
# <a name="identity-data-storage-for-european-customers-in-azure-active-directory"></a>Identiteit gegevensopslag voor Europese klanten in Azure Active Directory
Id-gegevens wordt opgeslagen door Azure AD in een geografische locatie gebaseerd op het adres dat is geleverd door uw organisatie bij het abonneren op een Microsoft Online service, zoals Office 365 en Azure. Voor informatie over waar de identiteitsgegevens van uw worden opgeslagen, kunt u de [waar zijn uw gegevens zich?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) gedeelte van het Microsoft Trust Center.

Voor klanten die voorzien van een adres in Europa, houdt de Azure AD de meeste van de identiteitsgegevens in Europese datacenters. Dit document bevat informatie over alle gegevens die zijn opgeslagen buiten Europa door Azure AD-services.

## <a name="microsoft-azure-multi-factor-authentication-mfa"></a>Microsoft Azure multi-factor authentication (MFA)
    
- Alle tweeledige verificatie met behulp van telefoongesprekken of SMS-afkomstig zijn uit de Amerikaanse datacentra en ook door leveranciers worden gerouteerd.
- Pushmeldingen verzenden met behulp van de Microsoft Authenticator app afkomstig zijn van ons datacenters. Apparaat de leverancier van specifieke services kunnen bovendien ook afkomstig zijn in de play en deze services mogelijk buiten Europa.
- OATH-codes worden altijd gevalideerd in de Verenigde Staten. 

## <a name="microsoft-azure-active-directory-b2c-azure-ad-b2c"></a>Microsoft Azure Active Directory B2C (Azure AD B2C)

Azure AD B2C-beleid-configuratiegegevens en -sleutelcontainers worden opgeslagen in Amerikaanse datacentra. Deze bevatten geen persoonlijke gegevens van elke gebruiker. Zie voor meer informatie over configuraties voor beleid voor de [Azure Active Directory B2C: Ingebouwd beleid](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies) artikel.

## <a name="microsoft-azure-active-directory-b2b-azure-ad-b2b"></a>Microsoft Azure Active Directory B2B (Azure AD B2B) 
    
Azure AD B2B-winkels uitnodigingen met verzilveren koppelen en Omleidings-URL-gegevens in Amerikaanse datacentra. Bovendien worden de e-mailadres van gebruikers die af voor B2B uitnodigingen ontvangen ook opgeslagen in Amerikaanse datacentra.

## <a name="microsoft-azure-active-directory-domain-services-azure-ad-ds"></a>Microsoft Azure Active Directory Domain Services (Azure AD DS)

Azure Active Directory Domain Services slaat gebruikersgegevens op dezelfde locatie op als het door de klant geselecteerde Azure Virtual Network. Dus als het netwerk zich buiten Europa bevindt, worden de gegevens gerepliceerd en opgeslagen buiten Europa.

## <a name="other-considerations"></a>Andere overwegingen

Services en toepassingen die kunnen worden geïntegreerd met Azure AD hebt toegang tot identiteitsgegevens. Elke service en toepassing die u gebruikt om te bepalen hoe identiteitsgegevens worden verwerkt door die specifieke service en de toepassing en of ze voldoen aan vereisten voor gegevensopslag van uw bedrijf evalueren.

Zie de sectie [Waar bevinden uw gegevens zich?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) van het Microsoft Trust Center voor meer informatie over de gegevenslocatie van Microsoft-services.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over een van de functies en functionaliteit die hierboven worden beschreven:
- [Wat is Multi-Factor Authentication?](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication)

- [Azure Active Directory Self-Service Password Reset](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-passwords-overview)

- [Wat is Azure Active Directory B2C?](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)

- [Wat is Azure AD B2B-samenwerking?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)

- [Azure Active Directory (AD) Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview)

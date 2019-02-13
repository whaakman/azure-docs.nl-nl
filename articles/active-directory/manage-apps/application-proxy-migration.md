---
title: Een upgrade uitvoeren naar Azure AD-toepassingsproxy | Microsoft Docs
description: Kiezen welke oplossing proxy wordt aanbevolen als u een van Microsoft Forefront of Unified Access-Gateway upgrade uitvoert.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/27/2017
ms.author: celested
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 15e831bbcb956401149d8c33fce4d00a3be5a11d
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56170870"
---
# <a name="compare-remote-access-solutions"></a>Oplossingen voor externe toegang vergelijken

Azure Active Directory Application Proxy is een van de twee oplossingen voor externe toegang die Microsoft aanbiedt. De andere is een Web Application Proxy, de on-premises versie. Deze twee oplossingen vervangt oudere producten die Microsoft aangeboden: Microsoft Forefront Threat Management Gateway (TMG) en geïntegreerde Access-Gateway (UAG). Gebruik dit artikel om te begrijpen hoe deze vier oplossingen zich verhouden tot elkaar. Voor het geval u nog steeds met behulp van de afgeschafte TMG of UAG oplossingen, in dit artikel gebruiken om u te helpen bij het plannen van uw migratie naar een van de toepassingsproxy. 


## <a name="feature-comparison"></a>Vergelijking van functies

Gebruik deze tabel om te begrijpen hoe Threat Management Gateway (TMG), Unified Access-Gateway (UAG), Webtoepassingsproxyserver (WAP) en Azure AD Application Proxy (AP) zich tot elkaar verhouden.

| Functie | TMG | UAG | WAP | AP |
| ------- | --- | --- | --- | --- |
| Certificaatverificatie | Ja | Ja | - | - |
| Browser-apps selectief publiceren | Ja | Ja | Ja | Ja |
| Vooraf-verificatie en eenmalige aanmelding | Ja | Ja | Ja | Ja | 
| Laag-2/3-firewall | Ja | Ja | - | - |
| Proxy voor doorsturen mogelijkheden | Ja | - | - | - |
| VPN-mogelijkheden | Ja | Ja | - | - |
| Uitgebreide protocolondersteuning | - | Ja | Ja, als uitgevoerd via HTTP | Ja, als via HTTP of via Extern bureaublad-Gateway |
| Fungeert als AD FS-proxyserver | - | Ja | Ja | - |
| Een portal voor toegang tot toepassingen | - | Ja | - | Ja |
| Antwoord hoofdtekst koppeling vertaling | Ja | Ja | - | Ja | 
| Verificatie met kopteksten | - | Ja | - | Ja, met PingAccess | 
| Schaalbare beveiliging | - | - | - | Ja | 
| Voorwaardelijke toegang | - | Ja | - | Ja |
| Er zijn geen onderdelen in de gedemilitariseerde zone (DMZ) | - | - | - | Ja |
| Er is geen binnenkomende verbindingen | - | - | - | Ja |

Voor de meeste scenario's raden wij Azure AD-toepassing als de moderne oplossing. Web Application Proxy wordt alleen aanbevolen in scenario's waarin een proxyserver voor AD FS en u kunt geen aangepaste domeinen in Azure Active Directory gebruiken. 

Azure AD Application Proxy biedt unieke voordelen in vergelijking met vergelijkbare producten, waaronder:

- Uitbreiding van Azure AD met on-premises bronnen
   - Schaalbare beveiliging en bescherming
   - Functies zoals voorwaardelijke toegang en multi-Factor Authentication zijn gemakkelijk in te schakelen
- Er zijn geen onderdelen in de gedemilitariseerde zone
- Er is geen binnenkomende verbindingen die vereist zijn
- Een Toegangsvenster dat uw gebruikers voor al hun toepassingen raadplegen kunnen, met inbegrip van Office 365, Azure AD SaaS-apps geïntegreerde en uw on-premises web-apps. 


## <a name="next-steps"></a>Volgende stappen

- [Azure AD-toepassing gebruiken voor veilige externe toegang tot on-premises toepassingen](application-proxy.md)
- [Overgang van Forefront TMG en UAG naar toepassingsproxy](https://blogs.technet.microsoft.com/isablog/2015/06/30/modernizing-microsoft-application-access-with-web-application-proxy-and-azure-active-directory-application-proxy/).

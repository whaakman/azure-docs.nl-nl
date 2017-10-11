---
title: Upgrade uitvoeren naar Azure AD-toepassingsproxy | Microsoft Docs
description: Kies welke proxy-oplossing werkt het beste als u een van Microsoft Forefront of Unified Access-Gateway upgrade uitvoert.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 6c9f70493155de6989b26fd4e8bcf1dff01c835c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="compare-remote-access-solutions"></a>Oplossingen voor externe toegang vergelijken

Azure Active Directory-toepassingsproxy is een van twee oplossingen voor externe toegang die Microsoft aanbiedt. Het andere type is de on-premises versie van Web Application Proxy. Deze twee oplossingen voor eerdere producten die Microsoft aangeboden vervangen: Microsoft Forefront Threat Management Gateway (TMG) en Unified Access Gateway (UAG). Gebruik dit artikel om te begrijpen hoe deze vier oplossingen met elkaar wilt vergelijken. Gebruik dit artikel bij het plannen van uw migratie naar een van de toepassingsproxy voor die van u nog steeds met behulp van de afgeschafte TMG of UAG oplossingen. 


## <a name="feature-comparison"></a>Vergelijking van functies

Gebruik deze tabel om te begrijpen hoe Threat Management Gateway (TMG), Unified Access Gateway (UAG), Webtoepassingsproxyserver (WAP) en Azure AD Application Proxy (AP) met elkaar wilt vergelijken.

| Functie | TMG | UAG | WAP | AP |
| ------- | --- | --- | --- | --- |
| Verificatie via certificaat | Ja | Ja | - | - |
| Browser-apps selectief publiceren | Ja | Ja | Ja | Ja |
| Vooraf-verificatie en eenmalige aanmelding | Ja | Ja | Ja | Ja | 
| Laag-2/3-firewall | Ja | Ja | - | - |
| Proxy-mogelijkheden voor doorsturen | Ja | - | - | - |
| VPN-mogelijkheden | Ja | Ja | - | - |
| Uitgebreide protocolondersteuning | - | Ja | Ja, als via HTTP | Ja, als via HTTP of via Extern bureaublad-Gateway |
| Fungeert als AD FS-proxyserver | - | Ja | Ja | - |
| Een portal voor toegang tot toepassingen | - | Ja | - | Ja |
| Antwoord hoofdtekst koppeling vertaling | Ja | Ja | - | Ja | 
| Verificatie met koppen | - | Ja | - | Ja, met PingAccess | 
| Cloud-scale-beveiliging | - | - | - | Ja | 
| Voorwaardelijke toegang | - | Ja | - | Ja |
| Er zijn geen onderdelen in de gedemilitariseerde zone (DMZ) | - | - | - | Ja |
| Er is geen binnenkomende verbindingen | - | - | - | Ja |

Voor de meeste scenario's, wordt aangeraden Azure AD-toepassing als de moderne oplossing. Web Application Proxy wordt alleen aanbevolen in scenario's waarvoor een proxyserver voor AD FS en u kunt geen aangepaste domeinen in Azure Active Directory gebruiken. 

Azure AD-toepassingsproxy biedt unieke voordelen ten opzichte van vergelijkbare producten, waaronder:

- Azure AD voor lokale bronnen uitbreiden
   - Cloud-scale beveiliging en bescherming
   - Functies zoals voorwaardelijke toegang en multi-factor Authentication kunnen eenvoudig worden inschakelen
- Er is geen component in de gedemilitariseerde zone
- Er is geen inkomende verbindingen vereist
- Een toegangspaneel dat uw gebruikers voor alle bijbehorende toepassingen raadplegen kunnen, inclusief O365, Azure AD-SaaS-apps geïntegreerde en uw on-premises web-apps. 


## <a name="next-steps"></a>Volgende stappen

- [Azure AD-toepassing gebruiken voor veilige externe toegang tot on-premises toepassingen](active-directory-application-proxy-get-started.md)
- [Overgang van Forefront TMG en UAG naar de toepassingsproxy](https://blogs.technet.microsoft.com/isablog/2015/06/30/modernizing-microsoft-application-access-with-web-application-proxy-and-azure-active-directory-application-proxy/).

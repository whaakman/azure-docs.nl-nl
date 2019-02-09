---
title: Resources voor het migreren van apps aan Azure Active Directory | Microsoft Docs
description: Bronnen voor hulp bij het migreren van toegang tot toepassingen en -verificatie met Azure Active Directory (Azure AD).
services: active-directory
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 09/19/2018
ms.author: celested
ms.reviewer: baselden
ms.openlocfilehash: 5aaef7b2d281254037a051b1e91bfface653a013
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55961938"
---
# <a name="resources-for-migrating-applications-to-azure-active-directory"></a>Resources voor het migreren van toepassingen naar Azure Active Directory

Bronnen voor hulp bij het migreren van toegang tot toepassingen en -verificatie met Azure Active Directory (Azure AD). Vul deze korte enquête (https://aka.ms/AppsMigrationFeedback) feedback wilt geven over uw ervaring migreren apps naar Azure AD (met inbegrip van blokkeringen voor de migratie, moet u voor hulpprogramma's / richtlijnen of de redenen voor het behoud van uw on-premises id-provider). 

| Resource  | Description  |
|:-----------|:-------------|
|[Migreren van uw apps naar Azure AD](https://aka.ms/migrateapps/whitepaper) | Dit technische document biedt de voordelen van de migratie en wordt beschreven hoe u van plan bent voor migratie in vier fasen duidelijk beschreven: detectie, classificatie, migratie en beheer. U wordt geholpen bij het denken over het proces en u uw project in stukken eenvoudig te gebruiken. In het hele document vindt u koppelingen naar belangrijke bronnen waarmee u weg. |
|[Gids met oplossingen: Apps uit Active Directory Federatieservices (AD FS) migreren naar Azure AD](https://aka.ms/migrateapps/adfssolutionguide) | Deze handleiding helpt u bij de dezelfde vier fasen van het plannen en uitvoeren van een toepassing migration-project op een hoger niveau in de migratie technisch document worden beschreven. In deze handleiding leert u hoe u deze fasen van toepassing op het specifieke doel van het verplaatsen van een toepassing van Azure Directory Federated Services (AD FS) naar Azure AD.|
| [Hulpprogramma voor: Active Directory Federation Services Migration Readiness Script](https://aka.ms/migrateapps/adfstools) | Dit is een script dat u kunt uitvoeren op uw on-premises Active Directory Federation Services (AD FS)-server om te bepalen van de gereedheid van apps voor migratie naar Azure AD.|
| [Implementatieplan: Migreren van AD FS naar synchronisatie van wachtwoordhashes](https://aka.ms/ADFSTOPHSDPDownload) | -Hashes van wachtwoorden van gebruikers zijn met wachtwoord-hashsynchronisatie van on-premises Active Directory gesynchroniseerd met Azure AD. Hiermee kunt Azure AD om gebruikers te verifiëren zonder interactie met de on-premises Active Directory.| 
| [Implementatieplan: Migreren van AD FS naar pass-through-verificatie](https://aka.ms/ADFSTOPTADPDownload)|Azure AD-Pass through-verificatie kan gebruikers zich aanmelden bij zowel on-premises en cloudgebaseerde toepassingen met behulp van hetzelfde wachtwoord. Deze functie biedt uw gebruikers met een betere ervaring omdat ze een minder wachtwoord hebben te onthouden. Het vermindert ook de kosten van IT-helpdesk omdat gebruikers minder waarschijnlijk, u aanmelden als ze maar nodig hebt om te onthouden van een wachtwoord vergeet. Als iemand zich aanmeldt bij Azure AD, worden met deze functie de wachtwoorden rechtstreeks gecontroleerd tegen de on-premises Active Directory.|
| [Implementatieplan: Inschakelen van Single Sign-on bij een SaaS-app met Azure AD](https://aka.ms/SSODPDownload) | Eenmalige aanmelding (SSO) kunt die u toegang tot alle apps en bronnen die u nodig hebt om zaken te doen, tijdens het aanmelden slechts eenmaal met behulp van een gebruikersaccount. Bijvoorbeeld, nadat een gebruiker is aangemeld, kunt de gebruiker verplaatsen van Microsoft Office, met SalesForce, Box zonder verificatie (bijvoorbeeld een wachtwoord te typen) van een tweede keer voor. 
| [Implementatieplan: Apps uitbreiden naar Azure AD met Application Proxy](https://aka.ms/AppProxyDPDownload)| Toegang van werknemers laptops en andere apparaten on-premises toepassingen hebben doorgaans we virtuele particuliere netwerken (VPN's) of gedemilitariseerde zones (DMZ's). Dit is niet alleen ingewikkeld en moeilijk te beveiligen, maar het instellen en beheren ervan is duur. Azure AD-toepassingsproxy, kunt u gemakkelijker toegang tot on-premises toepassingen. |
| [Implementatieplannen](../fundamentals/active-directory-deployment-plans.md) | Zoek meer implementatieplannen voor het implementeren van functies zoals multi-factor authentication, voorwaardelijke toegang, inrichten van gebruikers, naadloze eenmalige aanmelding, self-service voor wachtwoord opnieuw instellen, en meer! |



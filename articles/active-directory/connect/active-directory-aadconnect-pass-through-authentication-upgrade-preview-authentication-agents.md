---
title: Azure AD Connect - Pass through-verificatie - Upgrade auth-agents | Microsoft Docs
description: In dit artikel wordt beschreven hoe u uw Azure Active Directory (Azure AD) Pass through-verificatie-configuratie bijwerken.
services: active-directory
keywords: Azure AD Connect Pass through-verificatie, installatie van Active Directory, vereiste onderdelen voor Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.component: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: be76965e99a20c1f7164187255e26f6463926c2f
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39214724"
---
# <a name="azure-active-directory-pass-through-authentication-upgrade-preview-authentication-agents"></a>Azure Active Directory Pass through-verificatie: Upgrade preview verificatie-Agents

## <a name="overview"></a>Overzicht

Dit artikel is bedoeld voor klanten die gebruikmaken van Azure AD Pass-through-verificatie via de Preview-versie. We onlangs bijgewerkt (en rebranded) de software van de verificatie-Agent. U moet _handmatig_ upgrade preview verificatie-Agents geïnstalleerd op uw on-premises servers. Deze handmatige upgrade wordt alleen een eenmalige bewerking. Alle toekomstige updates voor de verificatie-Agents worden automatisch uitgevoerd. De redenen om bij te werken zijn als volgt:

- De preview-versies van verificatie-Agents ontvangt geen eventuele verdere beveiligings- of oplossingen voor problemen.
-   De preview-versies van verificatie-Agents kunnen niet worden geïnstalleerd op extra servers voor hoge beschikbaarheid.

## <a name="check-versions-of-your-authentication-agents"></a>Versies van de verificatie-Agents controleren

### <a name="step-1-check-where-your-authentication-agents-are-installed"></a>Stap 1: Controleer waar uw verificatie-Agents zijn geïnstalleerd

Volg deze stappen als u wilt controleren waarop de verificatie-Agents zijn geïnstalleerd:

1. Aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) met de hoofdbeheerdersreferenties voor uw tenant.
2. Selecteer **Azure Active Directory** op de navigatiebalk aan de linkerkant.
3. Selecteer **Azure AD Connect**. 
4. Selecteer **Pass through-verificatie**. Deze blade geeft een lijst van de servers waarop de verificatie-Agents zijn geïnstalleerd.

![Azure Active Directory-beheercentrum - Pass through-verificatie-blade](./media/active-directory-aadconnect-pass-through-authentication/pta8.png)

### <a name="step-2-check-the-versions-of-your-authentication-agents"></a>Stap 2: De versies van de verificatie-Agents controleren

Volg deze instructies om te controleren of de versies van de verificatie-Agents, op elke server die is geïdentificeerd in de vorige stap:

1. Ga naar **Configuratiescherm -> programma's -> programma's en onderdelen** op de on-premises server.
2. Als er een vermelding voor '**Microsoft Azure AD Connect Authentication-Agent**', niet hoeft geen actie te ondernemen op deze server.
3. Als er een vermelding voor '**Microsoft Azure AD Application Proxy Connector**', moet u handmatig een upgrade uitvoert op deze server.

![Preview-versie van verificatie-Agent](./media/active-directory-aadconnect-pass-through-authentication/pta6.png)

## <a name="best-practices-to-follow-before-starting-the-upgrade"></a>Aanbevolen procedures te volgen voordat u begint met de upgrade

Voordat u een upgrade uitvoert, moet u de volgende items hebt in plaats:

1. **Maken van cloud-only hoofdbeheerdersaccount**: upgrade niet uitvoeren zonder een alleen-cloud globale beheerdersaccount te gebruiken in noodsituaties waar uw Pass through-verificatie-Agents niet goed werken. Meer informatie over [toevoegen van een alleen-cloud globale beheerdersaccount](../active-directory-users-create-azure-portal.md). U deze stap uitvoert, is van essentieel belang en zorgt ervoor dat u geen toegang meer hebt tot uw tenant.
2.  **Zorg voor hoge beschikbaarheid**: als u niet eerder is voltooid, installeert u een tweede zelfstandige verificatie-Agent voor hoge beschikbaarheid van aanmeldingsaanvragen, gebruik van deze [instructies](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-4-ensure-high-availability).

## <a name="upgrading-the-authentication-agent-on-your-azure-ad-connect-server"></a>Bijwerken van de verificatie-Agent op uw Azure AD Connect-server

U moet Azure AD Connect upgraden vóór de upgrade van de verificatie-Agent op dezelfde server. Volg deze stappen op uw primaire- en staging-Azure AD Connect-servers:

1. **Azure AD Connect upgraden**: Volg deze [artikel](./active-directory-aadconnect-upgrade-previous-version.md) en een upgrade uit naar de nieuwste versie van Azure AD Connect.
2. **De preview-versie van de verificatie-Agent verwijderen**: downloaden [dit PowerShell-script](https://aka.ms/rmpreviewagent) en uitvoeren als beheerder op de server.
3. **Download de nieuwste versie van de verificatie-Agent (versies 1.5.193.0 of hoger)**: aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) met de hoofdbeheerdersreferenties van uw tenant. Selecteer **Azure Active Directory -> Azure AD Connect Pass through-verificatie -> Download agent ->**. Accepteer de [servicevoorwaarden](https://aka.ms/authagenteula) en download de nieuwste versie van de verificatie-Agent. U kunt ook de verificatie-Agent vanuit downloaden [hier](https://aka.ms/getauthagent).
4. **Installeer de nieuwste versie van de verificatie-Agent**: Voer het uitvoerbare bestand in stap 3 hebt gedownload. Bieden van uw tenant-hoofdbeheerder referenties wanneer hierom wordt gevraagd.
5. **Controleer of de meest recente versie is geïnstalleerd**: zoals wordt weergegeven vóór, gaat u naar **Configuratiescherm -> programma's -> programma's en onderdelen** en controleer of er een vermelding voor '**Microsoft Azure AD Connect Verificatie-Agent**'.

>[!NOTE]
>Als u de blade Pass through-verificatie inschakelt op de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) na het voltooien van de voorgaande stappen, ziet u twee Verificatieagent vermeldingen per server - één vermelding weergegeven van de verificatie Agent als **Active** en andere als **inactieve**. Dit is _verwacht_. De **inactief** vermelding wordt automatisch verbroken na een paar dagen.

## <a name="upgrading-the-authentication-agent-on-other-servers"></a>Bijwerken van de verificatie-Agent op andere servers

Volg deze stappen voor het upgraden van verificatie-Agents op andere servers (waarop Azure AD Connect is niet geïnstalleerd):

1. **De preview-versie van de verificatie-Agent verwijderen**: downloaden [dit PowerShell-script](https://aka.ms/rmpreviewagent) en uitvoeren als beheerder op de server.
2. **Download de nieuwste versie van de verificatie-Agent (versies 1.5.193.0 of hoger)**: aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) met de hoofdbeheerdersreferenties van uw tenant. Selecteer **Azure Active Directory -> Azure AD Connect Pass through-verificatie -> Download agent ->**. Accepteer de voorwaarden van de service en de nieuwste versie downloaden.
3. **Installeer de nieuwste versie van de verificatie-Agent**: Voer het uitvoerbare bestand in stap 2 hebt gedownload. Bieden van uw tenant-hoofdbeheerder referenties wanneer hierom wordt gevraagd.
4. **Controleer of de meest recente versie is geïnstalleerd**: zoals wordt weergegeven vóór, gaat u naar **Configuratiescherm -> programma's -> programma's en onderdelen** en controleer of er een vermelding die met de naam **Microsoft Azure AD Connect Verificatie-Agent**.

>[!NOTE]
>Als u de blade Pass through-verificatie inschakelt op de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) na het voltooien van de voorgaande stappen, ziet u twee Verificatieagent vermeldingen per server - één vermelding weergegeven van de verificatie Agent als **Active** en andere als **inactieve**. Dit is _verwacht_. De **inactief** vermelding wordt automatisch verbroken na een paar dagen.

## <a name="next-steps"></a>Volgende stappen
- [**Problemen oplossen** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.

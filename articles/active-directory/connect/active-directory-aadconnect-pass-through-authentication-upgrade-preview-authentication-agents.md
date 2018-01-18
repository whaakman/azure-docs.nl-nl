---
title: Azure AD Connect - Pass through-verificatie - Upgrade auth-agents | Microsoft Docs
description: Dit artikel wordt beschreven voor het bijwerken van de configuratie van uw Azure Active Directory (Azure AD) Pass through-verificatie.
services: active-directory
keywords: Azure AD Connect Pass through-verificatie, install Active Directory onderdelen vereist voor Azure AD, SSO, Single Sign-on
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/14/2018
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: f0a254b7216ca6fda40e26bafb7de57e796a5218
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="azure-active-directory-pass-through-authentication-upgrade-preview-authentication-agents"></a>Azure Active Directory Pass-through-verificatie: Upgrade preview verificatie Agents

## <a name="overview"></a>Overzicht

In dit artikel is bedoeld voor klanten die gebruikmaken van Azure AD Pass-through-verificatie via preview. We onlangs bijgewerkt (en rebranded) de verificatie-Agent-software. U moet _handmatig_ upgrade preview verificatie Agents moeten worden geïnstalleerd op uw on-premises servers. Deze handmatige upgrade wordt alleen een eenmalige bewerking. Alle toekomstige updates voor de verificatie-Agents worden automatisch. De redenen om bij te werken zijn als volgt:

- De preview-versies van Agents verificatie ontvangen niet alle verdere beveiligings- of oplossingen voor problemen.
-   De preview-versies van Agents verificatie kunnen niet worden geïnstalleerd op extra servers voor hoge beschikbaarheid.

## <a name="check-versions-of-your-authentication-agents"></a>Versies van de Agents verificatie controleren

### <a name="step-1-check-where-your-authentication-agents-are-installed"></a>Stap 1: Controleer waarbij de verificatie-Agents zijn geïnstalleerd

Volg deze stappen om te controleren waarop de verificatie-Agents zijn geïnstalleerd:

1. Aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) met de referenties van de globale beheerder voor uw tenant.
2. Selecteer **Azure Active Directory** op de linkernavigatiebalk.
3. Selecteer **Azure AD Connect**. 
4. Selecteer **Pass through-verificatie**. Deze blade worden de servers waarop de verificatie-Agents zijn geïnstalleerd.

![Azure Active Directory-beheercentrum - blade Pass through-verificatie](./media/active-directory-aadconnect-pass-through-authentication/pta8.png)

### <a name="step-2-check-the-versions-of-your-authentication-agents"></a>Stap 2: De versies van de Agents verificatie controleren

Volg deze instructies om te controleren van de versies van de verificatie-Agents op elke server die wordt geïdentificeerd in de vorige stap:

1. Ga naar **Configuratiescherm -> programma's -> programma's en onderdelen** op de on-premises server.
2. Als er een vermelding voor '**Microsoft Azure AD Connect-verificatie Agent**', niet hoeft geen actie te ondernemen op deze server.
3. Als er een vermelding voor '**Microsoft Azure AD Connector voor toepassingsproxy**', versies 1.5.132.0 of eerder, moet u een handmatige upgrade uitvoert op deze server.

![Preview-versie van de verificatie-Agent](./media/active-directory-aadconnect-pass-through-authentication/pta6.png)

## <a name="best-practices-to-follow-before-starting-the-upgrade"></a>Aanbevolen procedures te volgen voordat u een upgrade

Voordat u bijwerkt, zorg ervoor dat u de volgende items voldaan hebt:

1. **Alleen in de cloud globale beheerdersaccount maken**: geen upgrade zonder een cloudconfiguratie globale beheerdersaccount te gebruiken in noodsituaties, waarbij de Agents van Pass through-verificatie niet goed werken. Meer informatie over [toevoegen van een cloudconfiguratie globale beheerdersaccount](../active-directory-users-create-azure-portal.md). Deze stap is van essentieel belang en zorgt ervoor dat u geen toegang buiten uw tenant.
2.  **Hoge beschikbaarheid garanderen**: als niet eerder is voltooid, installeert u een tweede zelfstandige verificatie-Agent voor maximale beschikbaarheid voor aanmelden aanvragen, gebruik van deze [instructies](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability).

## <a name="upgrading-the-authentication-agent-on-your-azure-ad-connect-server"></a>De verificatie-Agent op uw Azure AD Connect-server bijwerken

U moet Azure AD Connect upgraden voordat u de verificatie-Agent op dezelfde server de upgrade. Volg deze stappen op uw primaire- en staging-Azure AD Connect-servers:

1. **Azure AD Connect upgraden**: voert u [artikel](./active-directory-aadconnect-upgrade-previous-version.md) en upgrade uitvoeren naar de meest recente versie van Azure AD Connect.
2. **Verwijderen van de preview-versie van de verificatie-Agent**: downloaden [dit PowerShell-script](https://aka.ms/rmpreviewagent) en het uitvoeren als beheerder op de server.
3. **Download de nieuwste versie van de verificatie-Agent (versies 1.5.193.0 of hoger)**: aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) met uw globale tenantbeheerderreferenties. Selecteer **Azure Active Directory -> Azure AD Connect -> Pass through-verificatie-agent downloaden >**. Accepteer de [servicevoorwaarden](https://aka.ms/authagenteula) en download de nieuwste versie van de verificatie-Agent. U kunt ook de verificatie-Agent van downloaden [hier](https://aka.ms/getauthagent).
4. **Installeer de nieuwste versie van de Agent verificatie**: het uitvoerbare bestand gedownload in stap 3 uitvoeren. Geef uw tenant hoofdbeheerder referenties wanneer u wordt gevraagd.
5. **Controleer of de meest recente versie is geïnstalleerd**: zoals eerder aangegeven, gaat u naar **Configuratiescherm -> programma's -> programma's en onderdelen** en controleer of er een vermelding voor '**Microsoft Azure AD Connect Verificatie-Agent**'.

>[!NOTE]
>Als u de blade Pass through-verificatie inschakelt op de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) na het voltooien van de voorgaande stappen, ziet u twee verificatie-Agent vermeldingen per server - één vermelding in de verificatie wordt weergegeven Agent als **Active** en de andere as **inactieve**. Dit is _verwacht_. De **inactief** vermelding automatisch wordt verbroken na een paar dagen.

## <a name="upgrading-the-authentication-agent-on-other-servers"></a>De verificatie-Agent op andere servers bijwerken

Volg deze stappen voor het upgraden van verificatie-Agents op andere servers (waar Azure AD Connect is niet geïnstalleerd):

1. **Verwijderen van de preview-versie van de verificatie-Agent**: downloaden [dit PowerShell-script](https://aka.ms/rmpreviewagent) en het uitvoeren als beheerder op de server.
2. **Download de nieuwste versie van de verificatie-Agent (versies 1.5.193.0 of hoger)**: aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) met uw globale tenantbeheerderreferenties. Selecteer **Azure Active Directory -> Azure AD Connect -> Pass through-verificatie-agent downloaden >**. Accepteer de voorwaarden van de service en de nieuwste versie downloaden.
3. **Installeer de nieuwste versie van de Agent verificatie**: uitvoeren van het uitvoerbare bestand in stap 2 hebt gedownload. Geef uw tenant hoofdbeheerder referenties wanneer u wordt gevraagd.
4. **Controleer of de meest recente versie is geïnstalleerd**: zoals eerder aangegeven, gaat u naar **Configuratiescherm -> programma's -> programma's en onderdelen** en controleer of er een vermelding aangeroepen **Microsoft Azure AD Connect Verificatie-Agent**.

>[!NOTE]
>Als u de blade Pass through-verificatie inschakelt op de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) na het voltooien van de voorgaande stappen, ziet u twee verificatie-Agent vermeldingen per server - één vermelding in de verificatie wordt weergegeven Agent als **Active** en de andere as **inactieve**. Dit is _verwacht_. De **inactief** vermelding automatisch wordt verbroken na een paar dagen.

## <a name="next-steps"></a>Volgende stappen
- [**Problemen met** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.

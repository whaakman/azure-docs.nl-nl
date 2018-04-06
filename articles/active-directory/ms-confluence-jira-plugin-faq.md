---
title: Veelgestelde vragen over Azure Active Directory SSO invoegtoepassing | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over het configureren van eenmalige aanmelding tussen Azure Active Directory en Jira/samenloop.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4b663047-7f88-443b-97bd-54224b232815
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2018
ms.author: jeedes
ms.openlocfilehash: b642d6de9fd9265d7f7ee11b827a36d20e5bcce7
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="faq-for-the-azure-active-directory-sso-plug-in"></a>Veelgestelde vragen over Azure Active Directory SSO invoegtoepassing 

## <a name="what-does-the-plug-in-do"></a>Wat doet de invoegtoepassing?

De invoegtoepassing biedt mogelijkheden voor eenmalige aanmelding (SSO) voor Atlassian Jira (inclusief Jira Core, Jira Software, Jira servicedesk) en samenloop lokale software. De invoegtoepassing werkt met Azure Active Directory (Azure AD) als een identiteitprovider (IdP).

## <a name="which-atlassian-products-does-the-plug-in-work-with"></a>Welke producten Atlassian werkt de invoegtoepassing met?

De invoegtoepassing werkt met lokale versies van Jira en samenloop.

## <a name="does-the-plug-in-work-on-cloud-versions"></a>De invoegtoepassing werkt op versies van de cloud?

Nee. De invoegtoepassing ondersteunt alleen on-premises versies van Jira en samenloop.

## <a name="which-versions-of-jira-and-confluence-does-the-plug-in-support"></a>Welke versies van Jira en samenloop biedt ondersteuning voor de invoegtoepassing?

De invoegtoepassing biedt ondersteuning voor deze versies:

* Jira Core en Software: 6.0-7.2.2 
* Jira servicedesk: 3.0 en 3.2 
* Samenloop: 5.0 naar versie 5.10

## <a name="is-the-plug-in-free-or-paid"></a>De invoegtoepassing is gratis of betaald?

Het is een gratis add-on. U kunt deze installeren via de Atlassian Marketplace.

## <a name="do-i-need-to-restart-jira-or-confluence-after-i-deploy-the-plug-in"></a>Moet ik Jira of samenloop opnieuw opstarten nadat ik de invoegtoepassing implementeren?

Opnieuw opstarten is niet vereist. U kunt starten direct met de invoegtoepassing.

## <a name="how-do-i-get-support-for-the-plug-in"></a>Hoe krijg ik ondersteuning voor de invoegtoepassing?

Bereiken de [SSO van Azure AD-integratie Team](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). Het team reageert in 24-48 kantooruren. 

U kunt ook een ondersteuningsticket met Microsoft via het Azure-portal kanaal verhogen.

## <a name="would-the-plug-in-work-on-a-mac-or-ubuntu-installation-of-jira-and-confluence"></a>De invoegtoepassing werkzaamheden op een Mac of Ubuntu installatie van Jira en samenloop zou?

We hebben de invoegtoepassing alleen op 64-bits Windows Server-installaties van Jira en samenloop getest.

## <a name="does-the-plug-in-work-with-idps-other-than-azure-ad"></a>De invoegtoepassing werkt met IdPs dan Azure AD?

Nee. Dit werkt alleen met Azure AD.

## <a name="what-version-of-saml-does-the-plug-in-work-with"></a>Welke versie van SAML werkt de invoegtoepassing met?

Dit proces werkt met SAML 2.0.

## <a name="does-the-plug-in-do-user-provisioning"></a>De invoegtoepassing doet gebruikersaanvragen?

Nee. De invoegtoepassing biedt alleen op basis van SAML 2.0 SSO. De gebruiker heeft in de toepassing vóór de SSO-aanmeldingspagina worden ingericht.

## <a name="does-the-plug-in-support-cluster-versions-of-jira-and-confluence"></a>Ondersteunt de versies van het cluster ondersteuning voor invoegtoepassingen van Jira en samenloop?

Nee. De invoegtoepassing werkt met lokale versies van Jira en samenloop.

## <a name="does-the-plug-in-work-with-http-versions-of-jira-and-confluence"></a>De invoegtoepassing werkt met HTTP-versies van Jira en samenloop?

Nee. De invoegtoepassing werkt met installaties alleen HTTPS-functionaliteit.
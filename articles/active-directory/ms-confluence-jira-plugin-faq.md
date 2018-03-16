---
title: "Microsoft Azure Active Directory één aanmelding invoegtoepassing Veelgestelde vragen | Microsoft Docs"
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Microsoft Azure Active Directory eenmalige aanmelding voor JIRA.
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
ms.openlocfilehash: 571fbd5078f66375f6e81cba2a790121366f9d60
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="microsoft-azure-active-directory-single-sign-on-plugin-faq"></a>Microsoft Azure Active Directory één aanmelding invoegtoepassing Veelgestelde vragen 

## <a name="1-whats-the-microsoft-sso-add-on"></a>1. Wat is de invoegtoepassing voor Microsoft SSO?

Deze invoegtoepassing biedt de mogelijkheid voor eenmalige aanmelding voor Atlassian van JIRA (inclusief JIRA Core, JIRA Software, JIRA servicedesk) en samenloop On-premises software. De invoegtoepassing werkt met Azure AD als IdP.

## <a name="2-add-on-works-with-which-atlassian-products"></a>2. Invoegtoepassing werkt met welke producten Atlassian?

Vanaf nu werkt de invoegtoepassing met On-premises-versies van JIRA en samenloop.

## <a name="3-does-this-add-on-work-on-cloud-version"></a>3. Werkt deze invoegtoepassing op cloudversie?

Nee. On-premises-versies van JIRA en samenloop worden alleen ondersteund.

## <a name="4-which-versions-of-jira-and-confluence-are-supported"></a>4. Welke versies van JIRA en samenloop worden ondersteund?

Hieronder volgt de lijst met versies die worden ondersteund:

* JIRA Core en Software: 6.0-7.2.2 
* JIRA servicedesk: 3.0 en 3.2 
* Samenloop: 5.0 naar versie 5.10

## <a name="5-is-this-add-on-free-or-paid"></a>5. Deze invoegtoepassing Free of betaalde is?

Dit is een gratis add-on en kan worden geïnstalleerd vanaf Atlassian marktplaats.

## <a name="6-do-i-need-to-restart-jiraconfluence-once-i-deploy-the-add-on"></a>6. Moet ik JIRA/samenloop opnieuw te starten wanneer ik de invoegtoepassing implementeren

Opnieuw opstarten is niet vereist post-invoegtoepassing voor implementatie. U kunt starten met de invoegtoepassing onmiddellijk na de implementatie.

## <a name="7-how-do-i-get-support-for-the-add-on"></a>7. Hoe krijg ik ondersteuning voor de invoegtoepassing?

Bereiken aan ons op: <email> . We zullen reageren binnen <> uur. U kunt ook een ondersteuningsticket met Microsoft via het Azure-portal kanaal verhogen. U kunt ook ons te bellen op: <Number> ben tussen <> naar <> pm op weekdagen.

## <a name="8-would-this-add-on-work-on-mac-or-ubuntu-installation-of-jira-and-confluence"></a>8. Deze invoegtoepassing werkt op JIRA en samenloop Mac of Ubuntu installeren?

We hebben deze invoegtoepassing alleen op 64-bits Windows server-installaties van JIRA en samenloop getest.

## <a name="9-does-this-add-on-work-with-other-idps-than-azure-ad"></a>9. Werkt deze invoegtoepassing in combinatie met andere IdPs dan Azure AD?

Nee. Invoegtoepassing werkt alleen met Azure AD.

## <a name="10-what-version-of-saml-does-the-add-on-work-with"></a>10. Welke versie van SAML werkt de invoegtoepassing met?

Invoegtoepassing werkt met SAML 2.0.

## <a name="11-does-the-add-on-do-use-provisioning-as-well"></a>11. Biedt de invoegtoepassing gebruik inrichting ook?

Nee. Vanaf nu hier invoegtoepassing alleen SAML 2.0 SSO. Gebruiker heeft om te worden ingericht in de toepassing vóór de aanmelding voor eenmalige aanmelding.

## <a name="12-are-cluster-versions-of-jira-and-confluence-supported-by-add-on"></a>12. Clusterversies van JIRA en samenloop ondersteund door de invoegtoepassing zijn?

Nee. De invoegtoepassing werkt met On-premises-versies van JIRA en samenloop.

## <a name="13-would-this-plugin-work-with-http-version-of-jira-and-confluence"></a>13. Deze invoegtoepassing werkt met HTTP-versie van JIRA en samenloop?

Nee. De invoegtoepassing werkt met HTTPS installaties alleen ingeschakeld.

## <a name="14-do-i-need-to-buy-license-of-the-add-on"></a>14. Moet ik aanschaffen licentie van de invoegtoepassing?

Het is een gratis add-on.
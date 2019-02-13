---
title: Azure Active Directory-verificatieprotocollen | Microsoft Docs
description: Een overzicht van de verificatieprotocollen die wordt ondersteund door Azure Active Directory (AD)
documentationcenter: dev-center-name
author: CelesteDG
services: active-directory
manager: mtillman
editor: ''
ms.assetid: 7a838ae2-c24c-4304-b6c0-e77fb888e6c0
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: celested
ms.custom: aaddev
ms.reviewer: hirsin
ms.collection: M365-identity-device-management
ms.openlocfilehash: 937aa5d5874ff2915bfb51d289bfb1aac44a0530
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56207819"
---
# <a name="azure-active-directory-authentication-protocols"></a>Azure Active Directory-verificatieprotocollen
Azure Active Directory (Azure AD) biedt ondersteuning voor diverse van de meest gebruikte protocollen voor verificatie en autorisatie. De onderwerpen in deze sectie beschrijven de ondersteunde protocollen en hun implementatie in Azure AD. De onderwerpen een overzicht van ondersteunde claimtypen, een inleiding tot het gebruik van federatiemetagegevens opgenomen, gedetailleerde OAuth 2.0. en naslagdocumentatie van SAML 2.0-protocol, en een gedeelte voor probleemoplossing.

## <a name="authentication-protocols-articles-and-reference"></a>Verificatieprotocollen artikelen en referentie
* [Belangrijke informatie over de ondertekening van sleutelrollover in Azure AD](active-directory-signing-key-rollover.md) : meer informatie over het ondertekenen van Azure AD proces, wijzigingen kunt u de sleutel automatisch bijgewerkt en beschrijving voor het bijwerken van de meest voorkomende toepassingsscenario's.
* [Ondersteunde Token- en claimtypen](v1-id-and-access-tokens.md) -meer informatie over de claims in de tokens die problemen met Azure AD.
* [Federatiemetagegevens](azure-ad-federation-metadata.md) -informatie over het vinden en interpreteren van de metagegevens van documenten die Azure AD wordt gegenereerd.
* [OAuth 2.0 in Azure AD](v1-protocols-oauth-code.md) -meer informatie over de implementatie van OAuth 2.0 in Azure AD.
* [OpenID Connect 1.0](v1-protocols-openid-connect-code.md) -informatie over het gebruik van OAuth 2.0, een autorisatieprotocol, voor verificatie.
* [Service-to-Service aanroepen met referenties Client](v1-oauth2-client-creds-grant-flow.md) -informatie over het gebruik van OAuth 2.0 verlenen clientreferentiestroom voor service-naar-serviceaanroepen.
* [Service-to-Service aanroepen met de namens-stroom](v1-oauth2-on-behalf-of-flow.md) -informatie over het gebruik van OAuth 2.0 namens-stroom voor service-naar-serviceaanroepen.
* [Naslaginformatie over de SAML-Protocol](active-directory-saml-protocol-reference.md) -meer informatie over de profielen Single Sign-On en SAML voor eenmalige afmelding van Azure AD.

## <a name="see-also"></a>Zie ook
[Ontwikkelaarshandleiding voor Azure Active Directory](v1-overview.md)

[Active Directory-codevoorbeelden](sample-v1-code.md)

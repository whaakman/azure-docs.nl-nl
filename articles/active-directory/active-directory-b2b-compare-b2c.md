---
title: Vergelijk B2B-samenwerking en B2C in Azure Active Directory | Microsoft Docs
description: Wat is het verschil tussen Azure Active Directory B2B-samenwerking en Azure AD B2C?
services: active-directory
documentationcenter: 
author: sasubram
manager: mtillman
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 03/15/2017
ms.author: sasubram
ms.openlocfilehash: beca92e9259835fffa9178f558e9762637594570
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="compare-b2b-collaboration-and-b2c-in-azure-active-directory"></a>Vergelijk B2B-samenwerking en B2C in Azure Active Directory

Zowel Azure Active Directory (Azure AD) B2B-samenwerking en Azure AD B2C kunt u werken met externe gebruikers in Azure AD. Maar hoe vergelijk ze?


Mogelijkheden voor B2B-samenwerking |     Azure AD B2C zelfstandige aanbieding
-------- | --------
Bedoeld voor: organisaties die willen verifiëren van gebruikers uit een andere organisatie, ongeacht de id-provider. | Bedoeld voor: uitnodigen klanten van uw mobiele apparaat en web-apps, of personen, institutionele of zakelijke klanten in uw Azure AD.
Identiteiten die worden ondersteund: werknemers met een werk- of schoolaccounts, samen met een werk-of schoolaccounts of elk e-mailadres. Snel ter ondersteuning van federatieve direct.  | Identiteiten die worden ondersteund: Consumer gebruikers met lokale toepassing accounts (een e-adres of groepsnaam) of een ondersteund sociale identiteit met directe Federatie.
Welke de partner-gebruikers zich in directory: gebruikers van de Partner van de externe organisatie worden beheerd in dezelfde map als werknemers, maar speciaal van aantekeningen voorzien. Ze kunnen op dezelfde manier als werknemers worden beheerd, kunnen worden toegevoegd aan dezelfde groepen, enzovoort  | Welke directory de klant gebruiker entiteiten zijn: In de toepassingsmap. Afzonderlijk beheerd vanaf de directory van de organisatie werknemer en partner (indien aanwezig.
Eenmalige aanmelding (SSO) naar alle Azure AD verbonden apps wordt ondersteund. Bijvoorbeeld, kunt u toegang bieden aan Office 365- of lokale apps en tot andere SaaS-apps zoals Salesforce of werkdag.  |  Eenmalige aanmelding aan de klant die eigendom zijn van apps in de Azure AD B2C-tenants wordt ondersteund. Eenmalige aanmelding bij Office 365 of andere Microsoft- en Microsoft SaaS-Apps wordt niet ondersteund.
De levenscyclus van de partner: beheerd door de host/uitgenodigd organisatie.  | De levenscyclus van de klant: eigen beheer of beheerd door de toepassing.
Beveiligingsbeleid en naleving: beheerd door de host/uitgenodigd organisatie.  | Beveiligingsbeleid en naleving: beheerd door de toepassing.
De huisstijl: Merk van Host/uitgenodigd organisatie wordt gebruikt.  |    De huisstijl: Beheerd door de toepassing. Doorgaans is doorgaans product huisstijl met de organisatie geleidelijk in de achtergrond.
Meer informatie: [blogbericht](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/), [documentatie](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)  | Meer informatie: [productpagina](https://azure.microsoft.com/en-us/services/active-directory-b2c/), [documentatie](https://docs.microsoft.com/azure/active-directory-b2c/)


### <a name="next-steps"></a>Volgende stappen

Lees ook onze andere artikelen over Azure AD B2B-samenwerking:

* [Wat is Azure AD B2B-samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Gebruikerseigenschappen B2B-samenwerking](active-directory-b2b-user-properties.md)
* [Een B2B-samenwerking-gebruiker toe te voegen aan een rol](active-directory-b2b-add-guest-to-role.md)
* [B2B-samenwerking uitnodigingen delegeren](active-directory-b2b-delegate-invitations.md)
* [Dynamische groepen en B2B-samenwerking](active-directory-b2b-dynamic-groups.md)
* [SaaS-apps voor B2B-samenwerking configureren](active-directory-b2b-configure-saas-apps.md)
* [B2B-samenwerking gebruikerstokens](active-directory-b2b-user-token.md)
* [Gebruikersclaims voor B2B-samenwerking toewijzing](active-directory-b2b-claims-mapping.md)
* [Office 365 extern delen](active-directory-b2b-o365-external-user.md)
* [Huidige beperkingen voor B2B-samenwerking](active-directory-b2b-current-limitations.md)
* [Ondersteuning krijgen voor B2B-samenwerking](active-directory-b2b-support.md)

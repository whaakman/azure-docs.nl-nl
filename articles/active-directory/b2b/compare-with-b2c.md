---
title: Vergelijk B2B-samenwerking en B2C in Azure Active Directory | Microsoft Docs
description: Wat is het verschil tussen Azure Active Directory B2B-samenwerking en Azure AD B2C?
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 03/15/2017
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 19e8e9d22938fa8a18299d67aa77824aaae3f6da
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
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
Meer informatie: [blogbericht](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/), [documentatie](what-is-b2b.md)  | Meer informatie: [productpagina](https://azure.microsoft.com/services/active-directory-b2c/), [documentatie](https://docs.microsoft.com/azure/active-directory-b2c/)


### <a name="next-steps"></a>Volgende stappen

- [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
- [Gebruikerseigenschappen B2B-samenwerking](user-properties.md)


---
title: B2B-samenwerking vergelijken met B2C - Azure Active Directory | Microsoft Docs
description: Wat is het verschil tussen Azure Active Directory B2B-samenwerking en Azure AD B2C?
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: overview
ms.date: 01/30/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: d2fb881678c694754c8cfc0714038c030d4b2218
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67113132"
---
# <a name="compare-b2b-collaboration-and-b2c-in-azure-active-directory"></a>B2B-samenwerking vergelijken met B2C in Azure Active Directory

Zowel met Azure Active Directory (Azure AD) B2B-samenwerking en Azure AD B2C kunt u werken met externe gebruikers in Azure AD. Maar hoe vergelijken ze?

**Azure AD B2B** is voor bedrijven die veilig bestanden en resources willen delen met externe gebruikers zodat ze kunnen samenwerken. Een Azure-administrator stelt B2B in Azure Portal in en Azure AD zorgt voor de federatie tussen uw bedrijf en uw externe partner. Gebruikers melden zich aan bij de gedeelde resources met behulp van een eenvoudige uitnodiging en een inwisselproces met hun werk- of school-account of een e-mailaccount.
 
**Azure AD B2C** is voornamelijk bedoeld voor bedrijven en ontwikkelaars die klantgerichte apps maken. Met Azure AD B2C kunnen ontwikkelaars Azure AD als het volledig functionele identiteitssysteem gebruiken voor hun toepassing, zodat klanten zich kunnen aanmelden met een identiteit die ze al hebt gemaakt (zoals Facebook of Gmail).

De onderstaande tabel geeft een gedetailleerde vergelijking.


B2B-samenwerkingsfunctionaliteiten |     Azure AD B2C zelfstandige aanbieding
-------- | --------
Bedoeld voor: Organisaties die gebruikers van een partnerorganisatie willen kunnen verifiëren, ongeacht de id-provider. | Bedoeld voor: uitnodigen van klanten van uw mobiele apps en web-apps, zoals personen, institutionele of organisatie-klanten in uw Azure AD.
Ondersteunde identiteiten: werknemers met werk- of schoolaccounts, partners met werk- of schoolaccounts of een e-mailadres. Binnenkort wordt directe federatie ondersteund.  | Ondersteunde identiteiten: consumentgebruikers met lokale toepassingen onder accounts (een e-adres of gebruikersnaam) of een ondersteund sociale id met directe federatie.
Externe gebruikers worden beheerd in dezelfde map als werknemers, maar met een speciale aantekening. Ze kunnen op dezelfde manier als werknemers worden beheerd, kunnen worden toegevoegd aan dezelfde groepen, enzovoort  | Externe gebruikers worden beheerd in de toepassingsmap. Ze worden afzonderlijk beheerd vanaf de werknemers- en partnermap van de organisatie (indien van toepassing).
Eenmalige aanmelding (SSO) voor alle Azure AD verbonden apps wordt ondersteund. U kunt bijvoorbeeld toegang bieden voor Office 365 of on-premises apps en andere SaaS-apps, zoals Salesforce of Workday.  |  SSO voor apps van klanten in de Azure AD B2C-tenants wordt ondersteund. SSO bij Office 365 of andere Microsoft- en niet-Microsoft SaaS-apps wordt niet ondersteund.
Levenscyclus van de partner: beheerd door de host/uitnodigende organisatie.  | Levenscyclus van de klant: self-service of beheerd door de toepassing.
Beveiligingsbeleid en naleving: Beheerd door de host/uitnodigen organisatie (bijvoorbeeld met [beleid voor voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/b2b/conditional-access)).  | Beveiligingsbeleid en naleving: beheerd door de toepassing.
Branding: merk van host/uitnodigende organisatie wordt gebruikt.  |    Branding: beheerd door de toepassing. Doorgaans het merkproduct, terwijl de organisatie vervaagt in de achtergrond.
Meer informatie: [Blogpost](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/), [Documentatie](what-is-b2b.md)  | Meer informatie: [Productpagina](https://azure.microsoft.com/services/active-directory-b2c/), [Documentatie](https://docs.microsoft.com/azure/active-directory-b2c/)


### <a name="next-steps"></a>Volgende stappen

- [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
- [Eigenschappen van gebruikers van B2B-samenwerking](user-properties.md)


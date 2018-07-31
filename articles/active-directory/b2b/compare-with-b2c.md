---
title: B2B-samenwerking vergelijken met B2C in Azure Active Directory | Microsoft Docs
description: Wat is het verschil tussen Azure Active Directory B2B-samenwerking en Azure AD B2C?
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 03/15/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: b59dba541394c105370cfd3af0768a8477ddb4e6
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39348317"
---
# <a name="compare-b2b-collaboration-and-b2c-in-azure-active-directory"></a>B2B-samenwerking vergelijken met B2C in Azure Active Directory

Zowel Azure Active Directory (Azure AD) B2B-samenwerking en Azure AD B2C kunt u werken met externe gebruikers in Azure AD. Maar hoe vergelijk ze?


Functionaliteit voor B2B-samenwerking |     Azure AD B2C zelfstandige te bieden
-------- | --------
Bedoeld voor: organisaties die willen kunnen verificatie van gebruikers van een andere organisatie, ongeacht het id-provider. | Bedoeld voor: uitnodigen van klanten van uw mobiele en web-apps, of personen, institutionele of organisatie-klanten in uw Azure AD.
Identiteiten die worden ondersteund: werknemers met een werk- of schoolaccounts, partners met een werk-of schoolaccount of elk e-mailadres. Binnenkort ter ondersteuning van directe federation.  | Identiteiten die worden ondersteund: consumenten gebruikers met lokale toepassingen onder accounts (een e-adres of de gebruiker name) of een ondersteund sociale id met directe Federatie.
Welke uitmaken van de gebruikers van partnerorganisaties directory: gebruikers van partnerorganisaties van de externe organisatie worden beheerd in dezelfde map als werknemers, maar speciaal van aantekeningen voorzien. Ze kunnen op dezelfde manier als werknemers worden beheerd, kunnen worden toegevoegd aan dezelfde groepen, enzovoort  | Welke directory uitmaken van de klant-gebruikersentiteiten: In de toepassingsmap. Afzonderlijk beheerd vanaf de werknemer en partner directory organisatie (indien van toepassing.
Eenmalige aanmelding (SSO) voor alle Azure AD verbonden apps wordt ondersteund. U kunt bijvoorbeeld toegang bieden voor Office 365 of on-premises toepassingen en andere SaaS-Apps, zoals Salesforce of Workday.  |  Eenmalige aanmelding voor apps in de Azure AD B2C-tenants van klanten wordt ondersteund. Eenmalige aanmelding bij Office 365 of andere Microsoft- en niet - Microsoft SaaS-apps wordt niet ondersteund.
Levenscyclus van de partner: beheerd door de host/uitnodigen organisatie.  | Levenscyclus van klanten: self-service voor stroomactiviteitvoortgang of beheerd door de toepassing.
Beveiligingsbeleid en naleving: beheerd door de host/uitnodigen organisatie (bijvoorbeeld met [beleid voor voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/b2b/conditional-access)).  | Beveiligingsbeleid en naleving: beheerd door de toepassing.
Huisstijl: Merk van Host/uitnodigen organisatie wordt gebruikt.  |    Huisstijl: Beheerd door de toepassing. Normaal gesproken doorgaans product van het merk, met de organisatie vervagen in de achtergrond.
Meer informatie: [blogbericht](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/), [documentatie](what-is-b2b.md)  | Meer informatie: [productpagina](https://azure.microsoft.com/services/active-directory-b2c/), [documentatie](https://docs.microsoft.com/azure/active-directory-b2c/)


### <a name="next-steps"></a>Volgende stappen

- [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
- [Eigenschappen van gebruikers van B2B-samenwerking](user-properties.md)


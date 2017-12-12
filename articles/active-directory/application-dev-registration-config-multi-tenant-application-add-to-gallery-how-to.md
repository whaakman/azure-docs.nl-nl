---
title: Het toevoegen van een toepassing met meerdere tenants naar de galerie van Azure AD-toepassing | Microsoft Docs
description: Legt uit hoe u kunt uw aangepaste ontwikkelde multitenant toepassing weergeven in de Azure AD-Toepassingsgalerie
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 969273d1bc0750685a0b2010670915f4183ac8ed
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-add-a-multi-tenant-application-to-the-azure-ad-application-gallery"></a>Het toevoegen van een multitenant-toepassing naar de Azure AD-toepassingsgalerie

## <a name="what-is-the-azure-ad-application-gallery"></a>Wat is de Azure AD-Toepassingsgalerie?

De Azure AD-Toepassingsgalerie is een uitstekende manier om uw toepassing voor alle miljoenen klanten Azure Active Directory uitbreiden van de impact en ze bereiken van uw toepassing in de marketplace. De onderstaande stappen uitgelegd hoe u uw toepassing in de Azure AD-Toepassingsgalerie kunt weergeven.

## <a name="if-your-application-supports-saml-or-openidconnect"></a>Als uw toepassing biedt ondersteuning voor SAML- of OpenIDConnect
Als u een multitenant-toepassing die u wilt weergeven in de Azure AD-Toepassingsgalerie hebt, moet u eerst controleren of uw toepassing een van de volgende eenmalige aanmelding technologieën ondersteunt:

1. **OpenID Connect** -directe integratie met Azure AD met OpenID Connect voor verificatie en de Azure AD API toestemming voor configuratie. Als u een integratie net begint en SAML biedt geen ondersteuning voor uw toepassing, is dit de aanbevolen modus.
2. **SAML** – de toepassing al heeft de mogelijkheid voor het configureren van derden identiteitsproviders met het SAML-protocol.

Als uw toepassing ondersteuning biedt voor een van deze modi voor eenmalige aanmelding en u wilt uw toepassing met meerdere tenants in de Azure AD-Toepassingsgalerie weergeven, kunt u de stappen in het onderstaande document. Snel aan de slag sturen een e-mail naar  **waadpartners@microsoft.com** .

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a>Als uw toepassing biedt geen ondersteuning voor SAML of OpenIDConnect
Zelfs als uw toepassing biedt geen ondersteuning voor een van deze modi, kunnen we het nog steeds integreren in onze galerie met onze wachtwoord Single Sign-on-technologie. Als u wilt verkennen met deze optie kunt u een e-mailbericht te verzenden  **waadpartners@microsoft.com** .

## <a name="next-steps"></a>Volgende stappen
[Hoe u uw toepassing in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)

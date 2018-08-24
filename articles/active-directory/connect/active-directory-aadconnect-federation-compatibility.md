---
title: Azure AD-federation compatibiliteitslijst
description: Deze pagina bevat een niet-Microsoft-id-providers die kunnen worden gebruikt voor het implementeren van eenmalige aanmelding.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: curtand
ms.assetid: 22c8693e-8915-446d-b383-27e9587988ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 3b2313b79c57a95af40d29bca3d7522c83e10e4c
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42818711"
---
# <a name="azure-ad-federation-compatibility-list"></a>Azure AD-federation compatibiliteitslijst
Azure Active Directory biedt eenmalige aanmelding op en verbeterde beveiliging van toepassingen toegang voor Office 365 en andere Microsoft Online services voor hybride en alleen-cloud-implementaties zonder een oplossing van derden. Office 365, zoals de meeste van de Microsoft Online services, is geïntegreerd met Azure Active Directory voor adreslijstservices, verificatie en autorisatie. Azure Active Directory biedt ook single sign-on bij duizenden SaaS-toepassingen en on-premises webtoepassingen. Zie de Azure Active Directory [toepassingsgalerie](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) ondersteund voor SaaS-toepassingen. 

## <a name="idp-validation"></a>Validatie van de id-provider
Als uw organisatie gebruikmaakt van een oplossing van derden, kunt u eenmalige aanmelding voor uw on-premises Active Directory-gebruikers met Microsoft Online services, zoals Office 365, mits de federatieoplossing van derden compatibel met Azure is Active Directory.  Neem contact op met uw id-provider voor vragen met betrekking tot compatibiliteit.  Als u een overzicht van id-providers die eerder zijn getest voor compatibiliteit met Azure AD, door Microsoft, klikt u op [hier](https://www.microsoft.com/download/details.aspx?id=56843). 

>[!NOTE]
>Microsoft biedt niet langer validatietests voor onafhankelijke id-providers voor compatibiliteit met Azure Active Directory. Als u wilt testen van uw product voor interoperabiliteit raadpleegt u deze [richtlijnen](https://www.microsoft.com/download/details.aspx?id=56843). 

## <a name="next-steps"></a>Volgende stappen

- [Uw on-premises directory's integreren met Azure Active Directory](active-directory-aadconnect.md)
- [Azure AD Connect en federatie](active-directory-aadconnectfed-whatis.md)

---
title: Azure AD-federation compatibiliteitslijst
description: Deze pagina bevat een niet-Microsoft-id-providers die kunnen worden gebruikt voor het implementeren van eenmalige aanmelding.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: 22c8693e-8915-446d-b383-27e9587988ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/23/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 6ee772054a823ae0607672d41a17a38cd44b2fb8
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55494830"
---
# <a name="azure-ad-federation-compatibility-list"></a>Azure AD-federation compatibiliteitslijst
Azure Active Directory biedt eenmalige aanmelding op en verbeterde beveiliging van toepassingen toegang voor Office 365 en andere Microsoft Online services voor hybride en alleen-cloud-implementaties zonder een oplossing van derden. Office 365, zoals de meeste van de Microsoft Online services, is geïntegreerd met Azure Active Directory voor adreslijstservices, verificatie en autorisatie. Azure Active Directory biedt ook single sign-on bij duizenden SaaS-toepassingen en on-premises webtoepassingen. Zie de Azure Active Directory [toepassingsgalerie](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) ondersteund voor SaaS-toepassingen. 

## <a name="idp-validation"></a>Validatie van de id-provider
Als uw organisatie gebruikmaakt van een oplossing van derden, kunt u eenmalige aanmelding voor uw on-premises Active Directory-gebruikers met Microsoft Online services, zoals Office 365, mits de federatieoplossing van derden compatibel met Azure is Active Directory.  Neem contact op met uw id-provider voor vragen met betrekking tot compatibiliteit.  Als u een overzicht van id-providers die eerder zijn getest voor compatibiliteit met Azure AD, door Microsoft, klikt u op [hier](https://www.microsoft.com/download/details.aspx?id=56843). 

>[!NOTE]
>Microsoft biedt niet langer validatietests voor onafhankelijke id-providers voor compatibiliteit met Azure Active Directory. Als u wilt testen van uw product voor interoperabiliteit raadpleegt u deze [richtlijnen](https://www.microsoft.com/download/details.aspx?id=56843). 

## <a name="next-steps"></a>Volgende stappen

- [Uw on-premises directory's integreren met Azure Active Directory](whatis-hybrid-identity.md)
- [Azure AD Connect en federatie](how-to-connect-fed-whatis.md)

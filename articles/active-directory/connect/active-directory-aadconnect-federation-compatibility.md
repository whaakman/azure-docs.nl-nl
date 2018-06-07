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
ms.date: 04/27/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: f5e85a551181a3dd4618db2611a99675329cfabd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34592006"
---
# <a name="azure-ad-federation-compatibility-list"></a>Azure AD-federation compatibiliteitslijst
Azure Active Directory biedt eenmalige aanmelding op en verbeterde beveiliging van toepassingen toegang voor Office 365 en andere Microsoft Online services voor hybride en alleen in de cloud-implementaties zonder een oplossing van derden. Office 365, zoals de meeste van de Microsoft Online services, is geïntegreerd met Azure Active Directory voor adreslijstservices, verificatie en autorisatie. Azure Active Directory biedt ook eenmalige aanmelding tot duizenden SaaS-toepassingen en lokale webtoepassingen. Zie de Azure Active Directory [toepassingsgalerie](https://azuremarketplace.microsoft.com/marketplace/apps) voor ondersteunde SaaS-toepassingen. 

## <a name="idp-validation"></a>IDP-validatie
Als uw organisatie gebruikmaakt van een oplossing van derden federation, kunt u eenmalige aanmelding voor uw on-premises Active Directory-gebruikers met Microsoft Online services, zoals Office 365, mits de oplossing van derden federation compatibel met Azure is Active Directory.  Neem contact op met de id-provider voor vragen met betrekking tot compatibiliteit.  Als u een overzicht van id-providers die eerder zijn getest voor compatibiliteit met Azure AD, door Microsoft, klikt u op [hier](https://www.microsoft.com/download/details.aspx?id=56843). 

>[!NOTE]
>Microsoft biedt niet langer validatie testen van onafhankelijke identiteitsproviders voor compatibiliteit met Azure Active Directory. Als u wilt testen van uw product voor interoperabiliteit Raadpleeg deze [richtlijnen](https://www.microsoft.com/download/details.aspx?id=56843). 

## <a name="next-steps"></a>Volgende stappen

- [Uw on-premises directory's integreren met Azure Active Directory](active-directory-aadconnect.md)
- [Azure AD Connect en federatie](active-directory-aadconnectfed-whatis.md)

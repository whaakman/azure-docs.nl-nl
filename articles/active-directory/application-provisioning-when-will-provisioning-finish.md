---
title: Gebruikers inrichten tot een galerie van Azure AD-toepassing is nemen uur of langer | Microsoft Docs
description: Nagaan waarom het inrichten van uw toepassing duurt langer dan verwacht
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: dc582ff3dac8f128972f070309d5c8a4ce21fb70
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "36335380"
---
# <a name="user-provisioning-to-an-azure-ad-gallery-application-is-taking-hours-or-more"></a>Nemen uur of meer is gebruikers inrichten tot een galerie van Azure AD-toepassing

Tijdens het inschakelen van automatische inrichting voor een toepassing, kan de eerste synchronisatie overal van 20 minuten tot enkele uren duren, afhankelijk van de grootte van de Azure AD-directory en het aantal gebruikers in het bereik voor het inrichten. 

Volgende synchronisaties na de eerste synchronisatie worden sneller, omdat de inrichting service watermerken die de status van beide systemen na de eerste synchronisatie slaat, verbeterde prestaties van de volgende synchronisaties vertegenwoordigen.

## <a name="how-to-improve-provisioning-performance"></a>Inrichting prestaties verbeteren

Als de eerste synchronisatie meer dan een paar uur duurt, is er één dat die u doen kunt om prestaties te verbeteren:

-   **Gebruiker het bereik van de filters.** Bereik filters kunt u nauwkeurig afstemmen de gegevens die de inrichting service van Azure AD extraheert door het filteren van gebruikers op basis van specifieke kenmerkwaarden. Zie voor meer informatie over het bereikfilters [inrichten van toepassing op basis van kenmerken met bereikfilters](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

## <a name="next-steps"></a>Volgende stappen
[Gebruiker inrichting en het opheffen van inrichting voor SaaS-toepassingen met Azure Active Directory automatiseren](active-directory-saas-app-provisioning.md)


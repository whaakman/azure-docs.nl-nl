---
title: Inrichten van gebruikers naar een toepassing in Azure AD-galerie is duurt uren of meer | Microsoft Docs
description: Nagaan waarom inrichten naar uw toepassing duurt mogelijk langer dan verwacht
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 94859aa57ef6f565c1fe7ed4b199aff997661d0b
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54478075"
---
# <a name="user-provisioning-to-an-azure-ad-gallery-application-is-taking-hours-or-more"></a>Inrichten van gebruikers naar een toepassing in Azure AD-galerie is duurt uren of meer

Tijdens het inschakelen van automatische inrichting voor een toepassing, kan de initiële synchronisatie duren vanaf 20 minuten tot enkele uren, afhankelijk van de grootte van de Azure AD-map en het aantal gebruikers in het bereik voor het inrichten. 

Volgende synchronisaties na de initiële synchronisatie worden sneller, zoals de inrichtingsservice watermerken die staan voor de status van beide systemen na de initiële synchronisatie, het verbeteren van de prestaties van de volgende synchronisatie worden opgeslagen.

## <a name="how-to-improve-provisioning-performance"></a>Hoe u de inrichting prestaties te verbeteren

Als de initiële synchronisatie meer dan een paar uur duurt is, is er een dingen die u doen kunt om prestaties te verbeteren:

-   **Bereikfilters toevoegen van gebruiker.** Bereikfilters kunnen u nauwkeurig afstemmen de gegevens die de inrichtingsservice worden geëxtraheerd uit Azure AD door het filteren van gebruikers op basis van specifieke kenmerkwaarden. Zie voor meer informatie over het bereikfilters [op kenmerken gebaseerde toepassing inrichten met bereikfilters](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

## <a name="next-steps"></a>Volgende stappen
[Automate User Provisioning and Deprovisioning to SaaS Applications with Azure Active Directory](user-provisioning.md) (Automatisch gebruikers inrichten en de inrichting ongedaan maken voor SaaS-toepassingen met Azure Active Directory)


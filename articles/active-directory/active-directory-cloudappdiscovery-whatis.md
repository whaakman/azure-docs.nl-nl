---
title: Niet-beheerde cloud-toepassingen met Cloud App Discovery niet vinden in Azure Active Directory | Microsoft Docs
description: Bevat informatie over het zoeken en beheren van toepassingen met Cloud App Discovery, wat zijn de voordelen en hoe het werkt.
services: active-directory
keywords: cloud app discovery, het beheren van toepassingen
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: db968bf5-22ae-489f-9c3e-14df6e1fef0a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 1d0ad06fc7eec07f8e1e0ba47121b6eec01c87df
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="find-unmanaged-cloud-applications-with-cloud-app-discovery"></a>Niet-beheerde cloudtoepassingen met Cloud App Discovery zoeken
## <a name="summary"></a>Samenvatting

Cloud App Discovery in Azure Active Directory biedt nu een verbeterde zonder agent discovery-ervaring mogelijk gemaakt door Microsoft Cloud App Security. Voor het gebruik van Cloud App Discovery, meld u aan met uw Azure AD Premium P1-referenties. Deze update is beschikbaar op zonder extra kosten voor alle klanten van Azure AD Premium-P1. Aan de slag met het artikel [Cloud App Discovery instellen in Azure AD](https://docs.microsoft.com/azure/active-directory/cloudappdiscovery-get-started), probeer het vervolgens uit [Microsoft Cloud App Security](https://portal.cloudappsecurity.com/).

> [!IMPORTANT] 
> De huidige Azure AD Cloud App Discovery ervaring met detectie op basis van een agent is uitgeschakeld op 5 maart 2018, waarna de agents zijn uitgeschakeld en de gegevens verwijderd. Voer de actie vóór maart 5e actief en werkend ophalen op de nieuwe ervaring om te voorkomen dat onderbreking van de service.  
 
**Met Cloud App Discovery, kunt u het volgende doen:**

* Zoeken naar de cloudtoepassingen die worden gebruikt en dat gebruik meten door het aantal gebruikers, hoeveelheid verkeer of aantal webaanvragen naar de toepassing.
* De gebruikers die van een toepassing gebruikmaken te identificeren.
* Exporteer gegevens voor offline-analyse.
* Zorg ervoor dat deze toepassingen onder het IT-beheer en eenmalige aanmelding voor gebruikersbeheer inschakelen.

## <a name="how-it-works"></a>Hoe werkt het?
1. Toepassing gebruik agents zijn geïnstalleerd op de computers van gebruikers.
2. De gebruiksgegevens van de toepassing wordt vastgelegd door de agents is verzonden via een beveiligde, gecodeerde kanaal naar de cloud app discovery-service.
3. De Cloud App Discovery-service evalueert de gegevens en genereert rapporten.

![Cloud App Discovery-diagram](./media/active-directory-cloudappdiscovery/cad01.png)


## <a name="next-steps"></a>Volgende stappen
* [Cloud App Discovery beveiligings- en privacyoverwegingen](active-directory-cloudappdiscovery-security-and-privacy-considerations.md)  
* [Cloud App Discovery-registerinstellingen voor proxyservers met aangepaste poorten](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md)
* [Cloud App Discovery Agent Changelog](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx)
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)


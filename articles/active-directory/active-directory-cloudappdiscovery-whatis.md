---
title: Niet-beheerde cloud-toepassingen met Cloud App Discovery niet vinden in Azure Active Directory | Microsoft Docs
description: Bevat informatie over het zoeken en beheren van toepassingen met Cloud App Discovery, wat zijn de voordelen en hoe het werkt.
services: active-directory
keywords: cloud app discovery, het beheren van toepassingen
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: db968bf5-22ae-489f-9c3e-14df6e1fef0a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 35b898aa3c03aeef914a7df574ac65a22a6c7bec
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="find-unmanaged-cloud-applications-with-cloud-app-discovery"></a>Niet-beheerde cloudtoepassingen met Cloud App Discovery zoeken
## <a name="summary"></a>Samenvatting

Cloud App Discovery is een functie van Azure Active Directory Premium waarmee u voor het detecteren van niet-beheerde cloud-toepassingen wordt gebruikt door mensen in uw organisatie. In moderne ondernemingen zijn IT-afdelingen vaak niet op de hoogte van alle cloudtoepassingen die leden van hun organisatie gebruikmaken voor hun werk. Het is gemakkelijk om te zien waarom beheerders zou twijfels hebt over niet-geautoriseerde toegang tot zakelijke gegevens, mogelijk gegevenslekken en andere beveiligingsrisico's. Dit gebrek aan bewustzijn over kunt maken met het maken van een plan voor het afhandelen van deze beveiligingsrisico's afschrikken.

> [!TIP] 
> Bekijk de verbeteringen in de Cloud App Discovery in Azure Active Directory (Azure AD), die worden verhoogd door [integratie met Microsoft Cloud App Security](https://portal.cloudappsecurity.com).

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


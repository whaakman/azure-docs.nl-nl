---
title: Wat zijn serviceafhankelijkheden in Azure Active Directory voor voorwaardelijke toegang? | Microsoft Docs
description: Meer informatie over hoe voorwaarden worden gebruikt in Azure Active Directory voor voorwaardelijke toegang voor het activeren van een beleid.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleid voor voorwaardelijke toegang
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/18/2019
ms.author: markvi
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7e15dd12da2fbd03d342b4eb2a32f34db958b6ed
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58259463"
---
# <a name="what-are-service-dependencies-in-azure-active-directory-conditional-access"></a>Wat zijn serviceafhankelijkheden in Azure Active Directory voor voorwaardelijke toegang? 


U kunt toegangsvereisten voor tot websites en services opgeven met beleid voor voorwaardelijke toegang. Bijvoorbeeld: uw toegangsvereisten meervoudige verificatie (MFA) vereisen kunnen opnemen of [beheerde apparaten](require-managed-devices.md). 


Wanneer u rechtstreeks toegang hebben tot een site of service, is de impact van een beleidsregel doorgaans gemakkelijk om vast te stellen. Als u een beleid dat MFA voor SharePoint Online geconfigureerd vereist hebt, kunt u voor MFA, wordt afgedwongen voor elke aanmelding bij de SharePoint-webportal. Het is echter niet altijd eenvoudig om te beoordelen van de impact van een beleid, omdat er cloud-apps met afhankelijkheden van andere cloud-apps. Bijvoorbeeld, Microsoft Teams maakt gebruik van SharePoint online. Dus als u toegang hebben tot Microsoft Teams in onze huidige scenario, bent u ook onderhevig aan de SharePoint-MFA-beleid.   


## <a name="policy-enforcement"></a>Beleidsafdwinging 

Als u een serviceafhankelijkheid geconfigureerd hebt, kan het beleid worden toegepast met behulp van vroege gebonden of gekoppeld. 

**Afdwingen van beleid voor vroege gebonden** betekent dat een gebruiker moet voldoen aan het beleid van de afhankelijke service voordat u toegang tot de aanroepende toepassing. Bijvoorbeeld, een gebruiker moet voldoen aan beleid voor SharePoint voordat u zich in MS Teams. 

**Gekoppeld beleidsafdwinging** vindt plaats nadat de gebruiker zich bij de app aanroepen. Afdwinging is uitgesteld naar bij het aanroepen van app-aanvragen, een token voor de downstream-service. Voorbeelden zijn onder meer MS Teams toegang tot Planner en Office.com toegang tot SharePoint. 

Het volgende diagram ziet u serviceafhankelijkheden MS Teams. Effen de pijlen geven early-bound-afdwinging onderbroken pijl voor Planner gekoppeld afdwingen geeft. 



![Serviceafhankelijkheden MS Teams](./media/service-dependencies/01.png)



  

Als een best practice, moet u de algemene beleidsregels instellen voor gerelateerde apps en services waar mogelijk. Met een consistente beveiligingspostuur biedt u de beste gebruikerservaring. Bijvoorbeeld, minder het instellen van een algemene beleid voor Exchange Online, SharePoint Online, MS Teams en Skype voor bedrijven aanzienlijk onverwachte prompts die kunnen voortvloeien uit verschillende soorten beleid wordt toegepast op de downstream-services. 

De onderstaande tabel bevat aanvullende service-afhankelijkheden, waar de client-apps moeten voldoen aan  

| Client-apps         | Downstream-service                          | Afdwinging |
| :--                 | :--                                         | ---         | 
| Azure Data Lake     | Microsoft Azure-beheer (portal en API) | Vroege gebonden |
| Microsoft Classroom | Exchange                                    | Vroege gebonden |
|                     | SharePoint                                  | Vroege gebonden  |
| Microsoft Teams     | Exchange                                    | Vroege gebonden |
|                     | MS Planner                                  | Gekoppeld  |
|                     | SharePoint                                  | Vroege gebonden |
|                     | Skype voor Bedrijven Online                   | Vroege gebonden |
| Office-Portal       | Exchange                                    | Gekoppeld  |
|                     | SharePoint                                  | Gekoppeld  |
| Outlook-groepen      | Exchange                                    | Vroege gebonden |
|                     | SharePoint                                  | Vroege gebonden |
| PowerApps           | Microsoft Azure-beheer (portal en API) | Vroege gebonden |
|                     | Windows Azure Active Directory              | Vroege gebonden |
| Project             | Dynamics CRM                                | Vroege gebonden |
| Skype voor Bedrijven  | Exchange                                    | Vroege gebonden |
| Visual Studio       | Microsoft Azure-beheer (portal en API) | Vroege gebonden |



## <a name="next-steps"></a>Volgende stappen

Zie [Uw implementatie van voorwaardelijke toegang in Azure Active Directory plannen](plan-conditional-access.md) voor informatie over het implementeren van voorwaardelijke toegang tot uw omgeving.

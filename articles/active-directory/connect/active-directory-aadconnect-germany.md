---
title: Azure AD Connect in Microsoft Cloud Duitsland
description: "Azure AD Connect integreert uw on-premises directory's met Azure Active Directory. Hiermee kunt u een algemene identiteit bieden voor Office 365, Azure en SaaS toepassingen die zijn geïntegreerd met Azure AD."
keywords: inleiding tot Azure AD Connect, overzicht Azure AD Connect, wat is Azure AD Connect, Active Directory installeren, Duitsland, Zwarte Woud
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 2bcb0caf-5d97-46cb-8c32-bda66cc22dad
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 2b0c05b12877fdda11b941d822c265d34d5c04ca
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a>Azure AD Connect in Microsoft Cloud Duitsland - Openbare preview
## <a name="introduction"></a>Inleiding
Azure AD Connect voorziet in synchronisatie tussen uw on-premises Active Directory en Azure Active Directory.
Op dit moment moeten veel van de scenario's in [Microsoft Cloud Duitsland](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) worden uitgevoerd door de operator. Wanneer u Microsoft Cloud Duitsland gebruikt, moet u zich van de volgende zaken bewust zijn:

* De volgende URL's moeten via een proxyserver worden geopend als u wilt dat de synchronisatie succesvol verloopt:
  
  * *.microsoftonline.de
  * *.windows.net
  * * Certificaatintrekkingslijsten
* Wanneer u zich bij uw Azure AD-directory aanmeldt, moet u een account uit het domein onmicrosoft.de gebruiken.
* De volgende functies zijn niet beschikbaar:
  * Azure AD Connect Health (Engelstalig)
  * Automatische updates
 
## <a name="download"></a>Downloaden
U kunt Azure AD Connect downloaden via de Azure AD Connect-blade in de portal.  Volg de onderstaande instructies om de Azure AD Connect-blade te vinden.

### <a name="the-azure-ad-connect-blade"></a>De Azure AD Connect-blade
Wanneer u zich bij Azure Portal hebt aangemeld, doet u het volgende:

1. Ga naar Bladeren
2. Selecteer Azure Active Directory
3. Selecteer vervolgens Azure AD Connect

U ziet nu het volgende:

![De Azure AD Connect-blade](media/active-directory-aadconnect-germany/germany1.png)

In de volgende tabel staan welke functies worden weergegeven in de blade.

| Titel | Beschrijving |
| --- | --- |
| SYNCHRONISATIESTATUS |Hier ziet u of synchronisatie is in- of uitgeschakeld. |
| LAATSTE SYNCHRONISATIE |De laatste keer dat een synchronisatie is voltooid. |
| FEDERATIEVE DOMEINEN |Hier ziet u hoeveel federatieve domeinen er momenteel zijn geconfigureerd. |

## <a name="installation"></a>Installatie
Als u Azure AD Connect wilt installeren, kunt u [deze](active-directory-aadconnect.md#install-azure-ad-connect) documentatie gebruiken.

## <a name="advanced-features-and-additional-information"></a>Geavanceerde functies en aanvullende informatie
Zie [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md) voor meer informatie en tips over aangepaste instellingen en geavanceerde configuraties.  Deze pagina bevat informatie over en koppelingen naar aanvullende richtlijnen.


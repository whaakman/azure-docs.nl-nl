---
title: Azure Active Directory-beleid voor voorwaardelijke toegang op basis van apparaten configureren | Microsoft Docs
description: Informatie over het configureren van Azure Active Directory-beleid voor voorwaardelijke toegang op basis van apparaten.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: a27862a6-d513-43ba-97c1-1c0d400bf243
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: fb646a56b51960d5b076027d942dabe8f2afbe97
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="configure-azure-active-directory-device-based-conditional-access-policies"></a>Azure Active Directory-beleid voor voorwaardelijke toegang op basis van apparaten configureren

Met [voorwaardelijke toegang van Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), kunt u aanpassen hoe geautoriseerde gebruikers toegang uw resources tot hebben. Bijvoorbeeld, beperkt u de toegang tot bepaalde bronnen op vertrouwde apparaten. Beleid voor voorwaardelijke toegang waarvoor een vertrouwd apparaat wordt ook wel beleid voor voorwaardelijke toegang op basis van apparaten.

In dit onderwerp vindt u informatie over het configureren van beleidsregels voor voorwaardelijke toegang op basis van apparaten voor Azure AD-toepassingen. 


## <a name="before-you-begin"></a>Voordat u begint

Voorwaardelijke toegang op basis van apparaten ties **voorwaardelijke toegang van Azure AD** en **Azure AD-Apparaatbeheer samen**. Als u nog niet bekend bent met een van deze gebieden, moet u eerst de volgende onderwerpen lezen:

- **[Voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal.md)**  -dit onderwerp vindt u een conceptueel overzicht van voorwaardelijke toegang en de verwante terminologie.

- **[Inleiding tot beheer van apparaten in Azure Active Directory](device-management-introduction.md)**  -in dit onderwerp geeft een overzicht van de verschillende opties die u hebt om apparaten te verbinden met Azure AD. 


## <a name="trusted-devices"></a>Vertrouwde apparaten

In een wereld mobiel eerste, cloud eerste kunnen Azure Active Directory eenmalige aanmelding aan apparaten, apps en services vanaf elke locatie. Voor bepaalde bronnen in uw omgeving, het verlenen van toegang aan de juiste gebruikers mogelijk niet voldoende. Naast de juiste gebruikers, kunt u ook een vertrouwd apparaat moet worden gebruikt voor toegang tot een bron vereisen. In uw omgeving, kunt u definiëren wat een vertrouwd apparaat is gebaseerd op de volgende onderdelen:

- De [apparaatplatforms](active-directory-conditional-access-azure-portal.md#device-platforms) op een apparaat
- Hiermee wordt aangegeven of een apparaat is compatibel met
- Hiermee wordt aangegeven of een apparaat is lid van een domein 

De [apparaatplatforms](active-directory-conditional-access-azure-portal.md#device-platforms) wordt gekenmerkt door het besturingssysteem dat wordt uitgevoerd op uw apparaat. In uw beleid voor voorwaardelijke toegang op basis van apparaten, kunt u toegang tot bepaalde bronnen voor specifieke apparaatplatforms beperken.



In een beleid voor voorwaardelijke toegang op basis van apparaten, kunt u de vertrouwde apparaten zijn gemarkeerd als compatibel vereisen.

![Cloud-apps](./media/active-directory-conditional-access-policy-connected-applications/24.png)

Apparaten kunnen worden gemarkeerd als compatibel zijn in de map aan:

- Intune 
- Een derde partij mobiel apparaat beheerd systeem waarmee Windows 10-apparaten via Azure AD-integratie worden beheerd 
 
  

Alleen apparaten die zijn verbonden met Azure AD kunnen worden gemarkeerd als compatibel. Voor een apparaat verbinding met Azure Active Directory, hebt u de volgende opties: 

- Azure AD geregistreerd
- Azure AD die lid zijn van
- Hybride die lid zijn van Azure AD

    ![Cloud-apps](./media/active-directory-conditional-access-policy-connected-applications/26.png)

Als u de voetafdruk van een lokale Active Directory (AD) hebt, kunt u apparaten die niet zijn verbonden met Azure AD maar lid is van uw AD te worden vertrouwd.

![Cloud-apps](./media/active-directory-conditional-access-policy-connected-applications/25.png)


## <a name="next-steps"></a>Volgende stappen

Voordat u configureert een beleid voor voorwaardelijke toegang op basis van apparaten in uw omgeving, dient u te kijken hoe de [best practices voor voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-best-practices.md).


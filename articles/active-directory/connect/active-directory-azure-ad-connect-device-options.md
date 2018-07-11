---
title: 'Azure AD Connect: Apparaatopties | Microsoft Docs'
description: In dit document worden de opties voor apparaten beschikbaar in Azure AD Connect
services: active-directory
documentationcenter: ''
author: billmath
manager: samueld
editor: billmath
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: e52f691c75d491897b06a4ebb492d87fda682e38
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37917843"
---
#<a name="azure-ad-connect-device-options"></a>Azure AD Connect: Opties voor

De volgende documentatie bevat informatie over de verschillende opties voor apparaten beschikbaar in Azure AD Connect. Azure AD Connect kunt u de volgende twee bewerkingen configureren: 
* **Hybride Azure AD join**: als uw omgeving een on-premises heeft AD-footprint en u ook voordeel van de mogelijkheden van Azure Active Directory, kunt u hybride Azure AD gekoppelde apparaten implementeren. Dit zijn apparaten die beide zijn gekoppeld aan uw on-premises Active Directory en uw Azure Active Directory.
* **Apparaat terugschrijven**: Write-back van apparaat wordt gebruikt voor het inschakelen van voorwaardelijke toegang op basis van apparaten naar AD FS (2012 R2 of hoger) apparaten beveiligd

## <a name="configure-device-options-in-azure-ad-connect"></a>Apparaatopties configureren in Azure AD Connect

1.  Voer Azure AD Connect. In de **extra taken** weergeeft, schakelt **Apparaatopties configureren**.
    ![Apparaatopties configureren](./media/active-directory-aadconnect-device-options/deviceoptions.png) 

    Op de pijl naar volgende, een **overzicht** pagina wordt weergegeven, die details van de bewerkingen die kunnen worden uitgevoerd.
    ![Overzicht](./media/active-directory-aadconnect-device-options/deviceoverview.png)

    >[!NOTE]
    > De nieuwe opties voor het apparaat configureren is alleen beschikbaar in versie 1.1.819.0 en nieuwer.

2.  Na het opgeven van de referenties voor Azure AD, kunt u ervoor kiezen de bewerking op de pagina van de opties voor apparaat moet worden uitgevoerd.
    ![Apparaatbewerkingen](./media/active-directory-aadconnect-device-options/deviceoptionsselection.png)

## <a name="next-steps"></a>Volgende stappen

* [Hybrid Azure AD join configureren](../device-management-hybrid-azuread-joined-devices-setup.md)
* [Configureren / uitschakelen van Write-back van apparaat](./active-directory-aadconnect-feature-device-writeback.md)


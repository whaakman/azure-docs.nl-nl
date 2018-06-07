---
title: 'Azure AD Connect: Opties voor apparaten | Microsoft Docs'
description: In dit document worden beschikbare apparaatopties in Azure AD Connect
services: active-directory
documentationcenter: ''
author: anandy
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
ms.author: anandy
ms.openlocfilehash: 0eb3a33ee030dcda6a811a771578c9e976e36619
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34593279"
---
#<a name="azure-ad-connect-device-options"></a>Azure AD Connect: Opties voor apparaten

De volgende documentatie bevat informatie over de verschillende Apparaatopties die beschikbaar zijn in Azure AD Connect. U kunt Azure AD Connect gebruiken voor het configureren van de volgende twee bewerkingen: 
* **Hybride Azure AD join**: als uw omgeving een on-premises AD-footprint en ook voordeel van de mogelijkheden van Azure Active Directory, kunt u hybride Azure AD die lid zijn van apparaten implementeren. Dit zijn apparaten die beide zijn gekoppeld aan uw on-premises Active Directory en uw Azure Active Directory.
* **Apparaat terugschrijven**: Write-back van apparaat wordt gebruikt voor het inschakelen van voorwaardelijke toegang op basis van apparaten naar AD FS (2012 R2 of hoger) apparaten beveiligd

## <a name="configure-device-options-in-azure-ad-connect"></a>Opties voor apparaten configureren in Azure AD Connect

1.  Voer Azure AD Connect. In de **extra taken** pagina **Apparaatopties configureren**.
    ![Opties voor apparaten configureren](./media/active-directory-aadconnect-device-options/deviceoptions.png) 

    Op Volgende klikt, wordt een **overzicht** pagina wordt weergegeven, die details van de bewerkingen die kunnen worden uitgevoerd.
    ![Overzicht](./media/active-directory-aadconnect-device-options/deviceoverview.png)

    >[!NOTE]
    > De nieuwe opties voor het apparaat configureren is alleen beschikbaar in versie 1.1.819.0 en nieuwer.

2.  Na het opgeven van de referenties voor Azure AD, kunt u de bewerking wordt uitgevoerd op de pagina apparaat opties hebt opgegeven.
    ![Apparaatbewerkingen](./media/active-directory-aadconnect-device-options/deviceoptionsselection.png)

## <a name="next-steps"></a>Volgende stappen

* [Hybride Azure AD join configureren](../device-management-hybrid-azuread-joined-devices-setup.md)
* [Configureer / apparaat terugschrijven uitschakelen](./active-directory-aadconnect-feature-device-writeback.md)


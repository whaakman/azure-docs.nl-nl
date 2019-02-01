---
title: 'Azure AD Connect: Apparaatopties | Microsoft Docs'
description: In dit document worden de opties voor apparaten beschikbaar in Azure AD Connect
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: billmath
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/13/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: aac0de0e39245835122ede1d99a14ce523bdd8ca
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55498281"
---
# <a name="azure-ad-connect-device-options"></a>Azure AD Connect: Apparaatopties

De volgende documentatie bevat informatie over de verschillende opties voor apparaten beschikbaar in Azure AD Connect. Azure AD Connect kunt u de volgende twee bewerkingen configureren: 
* **Hybride Azure AD join**: Als uw omgeving een on-premises heeft AD-footprint en u wilt dat de voordelen van Azure AD, kunt u hybride Azure AD gekoppelde apparaten implementeren. Deze apparaten lid zijn van zowel uw on-premises Active Directory en uw Azure Active Directory.
* **Apparaat terugschrijven**: Write-back van apparaat wordt gebruikt voor het inschakelen van voorwaardelijke toegang op basis van apparaten naar AD FS (2012 R2 of hoger) apparaten beveiligd

## <a name="configure-device-options-in-azure-ad-connect"></a>Apparaatopties configureren in Azure AD Connect

1.  Voer Azure AD Connect. In de **extra taken** weergeeft, schakelt **Apparaatopties configureren**.  Klik op **volgende**.
    ![Apparaatopties configureren](./media/how-to-connect-device-options/deviceoptions.png) 

    De **overzicht** pagina worden de details weergegeven.
    ![Overzicht](./media/how-to-connect-device-options/deviceoverview.png)

    >[!NOTE]
    > De nieuwe opties voor het apparaat configureren is alleen beschikbaar in versie 1.1.819.0 en nieuwer.

2.  Na het opgeven van de referenties voor Azure AD, kunt u ervoor kiezen de bewerking op de pagina van de opties voor apparaat moet worden uitgevoerd.
    ![Apparaatbewerkingen](./media/how-to-connect-device-options/deviceoptionsselection.png)

## <a name="next-steps"></a>Volgende stappen

* [Hybrid Azure AD join configureren](../device-management-hybrid-azuread-joined-devices-setup.md)
* [Configureren / uitschakelen van Write-back van apparaat](how-to-connect-device-writeback.md)


---
title: Wat is federatie met Azure AD? | Microsoft Docs
description: Geeft een beschrijving van federatie met Azure AD.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/28/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 278935ae7d29680e4fa741d59370e75ee0e1e93d
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197701"
---
# <a name="what-is-federation-with-azure-ad"></a>Wat is federatie met Azure AD?

Federatie is een verzameling van domeinen waarmee een vertrouwensrelatie is ingesteld. Het niveau van de vertrouwensrelatie kan verschillen, maar omvat gewoonlijk verificatie en bijna altijd autorisatie. Een typische federatie kan een aantal organisaties omvatten waarmee een vertrouwensrelatie is ingesteld voor gedeelde toegang tot een reeks resources.

U kunt uw on-premises-omgeving met Azure AD federeren en deze federatie gebruiken voor verificatie en autorisatie.  Deze aanmeldingsmethode zorgt ervoor dat alle gebruikersverificatie on-premises wordt uitgevoerd.  Via deze methode kunnen beheerders strengere niveaus van toegangsbeheer implementeren. Federatie met AD FS en PingFederate is beschikbaar.

![Federatieve identiteit](./media/whatis-hybrid-identity/federated-identity.png)


> [!TIP]
> Als u besluit federatie met Active Directory Federation Services (AD FS) te gebruiken, kunt u optioneel als back-up wachtwoordhashsynchronisatie instellen in geval de AD FS-infrastructuur mislukt.


## <a name="next-steps"></a>Volgende stappen

- [Wat is hybride identiteit?](whatis-phs.md)
- [Wat zijn Azure AD Connect en Connect Health?](whatis-azure-ad-connect.md)
- [Wat is synchronisatie van wachtwoord-hashes?](whatis-phs.md)
- [Wat is federatie?](whatis-fed.md)
- [Wat is eenmalige aanmelding?](how-to-connect-sso.md)
- [Hoe federatie werkt](how-to-connect-fed-whatis.md)
- [Federatie met PingFederate](how-to-connect-install-custom.md#configuring-federation-with-pingfederate)

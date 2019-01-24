---
title: Wat is synchronisatie van wachtwoord-hashes met Azure AD? | Microsoft Docs
description: Beschrijving van wachtwoord-hashsynchronisatie.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: get-started-article
ms.date: 12/05/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: ae4244a9ce17cdcb5942100e97b67916d455d019
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54470245"
---
# <a name="what-is-password-hash-synchronization-with-azure-ad"></a>Wat is synchronisatie van wachtwoord-hashes met Azure AD?
Wachtwoord-hashsynchronisatie is een van de aanmeldingsmethoden voor hybride identiteit. Azure AD Connect synchroniseert een hash, van de hash, van het wachtwoord van een gebruiker vanuit een on-premises Active Directory-exemplaar naar een Azure AD-exemplaar in de cloud.

Wachtwoord-hashsynchronisatie is een uitbreiding op de adreslijstsynchronisatiefunctie die is geïmplementeerd door Azure AD Connect-synchronisatie. U kunt deze functie gebruiken voor aanmelding bij Azure AD-services zoals Office 365. U meldt zich aan bij de service met hetzelfde wachtwoord als waarmee u zich aanmeldt bij uw on-premises Active Directory-exemplaar.

![Wat is Azure AD Connect?](./media/how-to-connect-password-hash-synchronization/arch1.png)

Wachtwoord-hashsynchronisatie helpt u om het aantal wachtwoorden te verminderen. Hiermee hoeven uw gebruikers slechts één wachtwoord te onderhouden. Met wachtwoord-hashsynchronisatie kunt u:

* De productiviteit van uw gebruikers verhogen.
* De kosten van uw helpdesk verlagen.  

Als u besluit [federatie met Active Directory Federation Services (AD FS)](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect) te gebruiken, kunt u optioneel als back-up wachtwoordhashsynchronisatie instellen als aanmeldingsmethode.

Als u wachtwoord-hashsynchronisatie in uw omgeving wilt gebruiken, moet u:

* Azure AD Connect installeren.  
* Adreslijstsynchronisatie tussen uw on-premises Active Directory-exemplaar en uw Azure Active Directory-exemplaar configureren.
* Wachtwoord-hashsynchronisatie inschakelen.



Zie [Wat is hybride identiteit?](whatis-hybrid-identity.md) voor meer informatie.




## <a name="next-steps"></a>Volgende stappen

- [Wat is hybride identiteit?](whatis-phs.md)
- [Wat zijn Azure AD Connect en Connect Health?](whatis-azure-ad-connect.md)
- [Wat is pass-through-verificatie (PTA)?](how-to-connect-pta.md)
- [Wat is federatie?](whatis-fed.md)
- [Wat is eenmalige aanmelding?](how-to-connect-sso.md)
- [Hoe synchronisatie van wachtwoord-hashes werkt](how-to-connect-password-hash-synchronization.md)
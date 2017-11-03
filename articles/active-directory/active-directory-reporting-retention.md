---
title: Bewaarbeleid voor Azure Active Directory rapport | Microsoft Docs
description: Bewaarbeleid voor gegevens in uw Azure Active Directory
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/17/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 933d251903f4ca74902e984e7a1581a75345ee7f
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2017
---
# <a name="azure-active-directory-report-retention-policies"></a>Bewaarbeleid voor Azure Active Directory rapport


Dit onderwerp vindt u antwoorden op veelgestelde vragen in combinatie met het bewaren van gegevens voor de verschillende activiteitsrapporten in Azure Active Directory. 

**V: hoe vind u de verzameling van gegevens over de activiteit is gestart?**

**A:**

| Azure AD-editie | Verzameling starten |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Wanneer u registreert voor een abonnement |
| Azure AD Free | De eerste keer dat u opent de [Azure Active Directory-blade](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) of gebruik de [rapportage-API's](https://aka.ms/aadreports)  |

---
**V: waarop uw activiteitsgegevens beschikbaar is in de Azure-portal?**

**A:**

- **Onmiddellijk** : als u al met rapporten in de klassieke Azure portal werkt
- **Binnen 2 uur** : als u dit nog niet hebt ingeschakeld rapportage in de klassieke Azure portal

---
**V: hoe kunt u de verzameling van beveiliging signalen gestart ophalen?**  

**A:** voor beveiliging opgeven, een verzamelingsproces wordt gestart wanneer u zich aanmelden bij gebruik van het Identity Protection Center. 


---
**V: hoe lang de verzamelde gegevens opgeslagen?**

**A:**

**Activiteitsrapporten**    

| Rapport                 | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| Directorycontrole        | 7 dagen        | 30 dagen             | 30 dagen             |
| Aanmeldingsactiviteit       | N.v.t.           | 30 dagen             | 30 dagen             |
| Gebruik Azure MFA        | 90 dagen       | 90 dagen             | 90 dagen             |

**Beveiliging signalen**

| Rapport         | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--            | :--           | :--                 | :--                 |
| Gebruikers die risico lopen  | 7 dagen        | 30 dagen             | 90 dagen             |
| Riskant aanmeldingen | 7 dagen        | 30 dagen             | 90 dagen             |

---

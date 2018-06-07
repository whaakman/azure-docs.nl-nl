---
title: Bewaarbeleid voor Azure Active Directory rapport | Microsoft Docs
description: Bewaarbeleid voor gegevens in uw Azure Active Directory
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/10/2018
ms.author: rolyon
ms.reviewer: dhanyahk
ms.openlocfilehash: fac160d2d5916097afcbb9825bb82a52789e4a89
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34589192"
---
# <a name="azure-active-directory-report-retention-policies"></a>Bewaarbeleid voor Azure Active Directory rapport


Dit onderwerp vindt u antwoorden op veelgestelde vragen in combinatie met het bewaren van gegevens voor de verschillende activiteitsrapporten in Azure Active Directory. 

### <a name="q-how-can-you-get-the-collection-of-activity-data-started"></a>V: hoe vind u de verzameling van gegevens over de activiteit is gestart?

**A:**

| Azure AD-editie | Verzameling starten |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Wanneer u registreert voor een abonnement |
| Azure AD Free | De eerste keer dat u opent de [Azure Active Directory-blade](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) of gebruik de [rapportage-API's](https://aka.ms/aadreports)  |

---
### <a name="q-when-is-your-activity-data-available-in-the-azure-portal"></a>V: waarop uw activiteitsgegevens beschikbaar is in de Azure-portal?

**A:**

- **Onmiddellijk** : als u al met rapporten in de Azure portal werkt
- **Binnen 2 uur** : als u dit nog niet hebt ingeschakeld rapportage in Azure portal

---

### <a name="q-how-can-you-get-the-collection-of-security-signals-started"></a>V: hoe kunt u de verzameling van beveiliging signalen gestart ophalen?  

**A:** voor beveiliging opgeven, een verzamelingsproces wordt gestart wanneer u zich aanmelden bij gebruik van het Identity Protection Center. 


---

### <a name="q-for-how-long-is-the-collected-data-stored"></a>V: hoe lang de verzamelde gegevens opgeslagen?

**A:**

**Activiteitsrapporten**    

| Rapport                 | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| Directorycontrole        | 7 dagen        | 30 dagen             | 30 dagen             |
| Aanmeldingsactiviteit       | N/A           | 30 dagen             | 30 dagen             |
| Gebruik Azure MFA        | 30 dagen       | 30 dagen             | 30 dagen             |

**Beveiliging signalen**

| Rapport         | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--            | :--           | :--                 | :--                 |
| Gebruikers die risico lopen  | 7 dagen        | 30 dagen             | 90 dagen             |
| Riskante aanmeldingen | 7 dagen        | 30 dagen             | 90 dagen             |

---

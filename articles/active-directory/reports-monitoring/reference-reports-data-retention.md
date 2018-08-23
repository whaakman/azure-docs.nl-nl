---
title: Bewaarbeleid Azure Active Directory-rapport | Microsoft Docs
description: Beleid voor het bewaren van gegevens in uw Azure Active Directory
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: ''
ms.topic: reference
ms.tgt_pltfrm: ''
ms.workload: identity
ms.component: report-monitor
ms.date: 05/10/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 68028fd1ba116251860e5c370e9e9ce61fd314bb
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "42054236"
---
# <a name="azure-active-directory-report-retention-policies"></a>Bewaarbeleid Azure Active Directory-rapporten


In dit artikel vindt u antwoorden op de meest voorkomende vragen in combinatie met het bewaren van gegevens voor de verschillende aanmeldactiviteitenrapporten in Azure Active Directory. 

### <a name="q-how-can-you-get-the-collection-of-activity-data-started"></a>V: op welke manier krijgt u het verzamelen van gegevens over de activiteit is gestart?

**A:**

| Azure AD-editie | Verzameling starten |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Wanneer u zich aanmeldt voor een abonnement |
| Azure AD Free | De eerste keer dat u opent de [Azure Active Directory-blade](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) of gebruik de [reporting API's](https://aka.ms/aadreports)  |

---
### <a name="q-when-is-your-activity-data-available-in-the-azure-portal"></a>V: wanneer wordt de activiteitsgegevens beschikbaar zijn in Azure portal?

**A:**

- **Onmiddellijk** : als u al met rapporten in Azure portal werkt.
- **Binnen 2 uur** : als u dit nog niet hebt ingeschakeld op rapportage in Azure portal.

---

### <a name="q-how-can-you-get-the-collection-of-security-signals-started"></a>V: op welke manier krijgt u de verzameling van beveiliging signalen gestart?  

**A:** voor beveiliging signalen, de verzamelingsproces wordt gestart wanneer u zich aanmelden voor gebruik van het Identity Protection Center. 


---

### <a name="q-for-how-long-is-the-collected-data-stored"></a>V: hoe lang de verzamelde gegevens opgeslagen?

**A:**

**Activiteitenrapporten**    

| Rapport                 | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| Directorycontrole        | 7 dagen        | 30 dagen             | 30 dagen             |
| Aanmeldingsactiviteit       | N/A           | 30 dagen             | 30 dagen             |
| Azure MFA-gebruik        | 30 dagen       | 30 dagen             | 30 dagen             |

**Beveiliging signalen**

| Rapport         | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--            | :--           | :--                 | :--                 |
| Gebruikers die risico lopen  | 7 dagen        | 30 dagen             | 90 dagen             |
| Riskante aanmeldingen | 7 dagen        | 30 dagen             | 90 dagen             |

---

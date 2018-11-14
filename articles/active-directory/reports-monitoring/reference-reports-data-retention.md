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
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 601169cc62a99438f661adc06ab166b545606edb
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624587"
---
# <a name="azure-active-directory-report-retention-policies"></a>Bewaarbeleid Azure Active Directory-rapporten

In dit artikel hebt u meer informatie over het bewaarbeleid voor gegevens voor de verschillende aanmeldactiviteitenrapporten in Azure Active Directory. 

### <a name="when-does-azure-ad-start-collecting-data"></a>Wanneer begint Azure AD het verzamelen van gegevens?

| Azure AD-editie | Verzameling starten |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Wanneer u zich aanmeldt voor een abonnement |
| Azure AD Free | De eerste keer dat u opent de [Azure Active Directory-blade](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) of gebruik de [reporting API's](https://aka.ms/aadreports)  |

---

### <a name="when-is-the-activity-data-available-in-the-azure-portal"></a>Wanneer is de activiteitsgegevens beschikbaar zijn in Azure portal?

- **Onmiddellijk** : als u al met rapporten in Azure portal werkt.
- **Binnen 2 uur** : als u dit nog niet hebt ingeschakeld op rapportage in Azure portal.

---

### <a name="when-does-azure-ad-start-collecting-security-signal-data"></a>Wanneer begint Azure AD signaal beveiligingsgegevens verzamelen?  

Voor beveiliging signalen, de verzamelingsproces wordt gestart wanneer u zich aanmelden voor gebruik van de **Identity Protection Center**. 

---

### <a name="how-long-does-azure-ad-store-the-data"></a>Hoe lang de gegevens opslaan in Azure AD?

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

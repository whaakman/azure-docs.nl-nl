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
ms.openlocfilehash: 6188b141ec1a514d999f290366cd9ffbafc1d96c
ms.sourcegitcommit: 7862449050a220133e5316f0030a259b1c6e3004
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2018
ms.locfileid: "53753129"
---
# <a name="azure-active-directory-report-retention-policies"></a>Bewaarbeleid Azure Active Directory-rapporten

In dit artikel hebt u meer informatie over het bewaarbeleid voor gegevens voor de verschillende aanmeldactiviteitenrapporten in Azure Active Directory. 

### <a name="when-does-azure-ad-start-collecting-data"></a>Wanneer begint Azure AD het verzamelen van gegevens?

| Azure AD-editie | Verzameling starten |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Wanneer u zich aanmeldt voor een abonnement |
| Azure AD Free <br /> Azure AD Basic | De eerste keer dat u opent de [Azure Active Directory-blade](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) of gebruik de [reporting API's](https://aka.ms/aadreports)  |

---

### <a name="when-is-the-activity-data-available-in-the-azure-portal"></a>Wanneer is de activiteitsgegevens beschikbaar zijn in Azure portal?

- **Onmiddellijk** : als u al met rapporten in Azure portal werkt.
- **Binnen 2 uur** : als u dit nog niet hebt ingeschakeld op rapportage in Azure portal.

---

### <a name="when-does-azure-ad-start-collecting-security-signal-data"></a>Wanneer begint Azure AD signaal beveiligingsgegevens verzamelen?  

Voor beveiliging signalen, de verzamelingsproces wordt gestart wanneer u zich aanmelden voor gebruik van de **Identity Protection Center**. 

---

### <a name="how-long-does-azure-ad-store-the-data"></a>Hoe lang de gegevens opslaan in Azure AD?

**Activiteitsrapporten**    

| Rapport                 | Azure AD Free | Azure AD Basic | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--            | :--                 | :--                 |
| Directorycontrole        | 7 dagen        |  7 dagen        | 30 dagen             | 30 dagen             |
| Aanmeldingsactiviteit       | N/A           |  N/A           | 30 dagen             | 30 dagen             |
| Azure MFA-gebruik        | 30 dagen       |  30 dagen       | 30 dagen             | 30 dagen             |

U kunt de gegevens van de activiteit controleren en meld u langer dan de bewaartermijn die hierboven worden beschreven door het routering naar Azure storage-account met behulp van Azure Monitor behouden. Zie voor meer informatie, [archief Azure AD vastgelegd in een Azure storage-account](quickstart-azure-monitor-route-logs-to-storage-account.md).

**Beveiliging signalen**

| Rapport         | Azure AD Free | Azure AD Basic | Azure AD Premium P1 | Azure AD Premium P2 |
| :--            | :--           | :--            | :--                 | :--                 |
| Gebruikers die risico lopen  | 7 dagen        | 7 dagen         | 30 dagen             | 90 dagen             |
| Riskante aanmeldingen | 7 dagen        | 7 dagen         |  30 dagen            | 90 dagen             |

---

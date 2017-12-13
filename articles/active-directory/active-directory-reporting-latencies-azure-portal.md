---
title: Azure Active Directory-rapportage latenties | Microsoft Docs
description: Meer informatie over de hoeveelheid tijd die nodig is voor rapportage gebeurtenissen worden weergegeven in uw Azure-portal
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/11/2017
ms.author: markvi;dhanyahk
ms.reviewer: dhanyahk
ms.openlocfilehash: fa9ffa8f5380659674301f7e738879c8efb25b7f
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="azure-active-directory-reporting-latencies"></a>Azure Active Directory-rapportage latenties

Met [reporting](active-directory-preview-explainer.md) in Azure Active Directory, dat u alle informatie die u nodig hebt om te bepalen hoe uw omgeving doet. De hoeveelheid tijd die nodig is voor het melden van gegevens worden weergegeven in de Azure-portal wordt ook wel latentie. 

Dit onderwerp worden de latentie-informatie voor de categorieën alle rapportage in Azure portal. 


## <a name="activity-reports"></a>Activiteitsrapporten

Er zijn twee gebieden van de activiteit reporting:

- **Aanmeldactiviteiten**: informatie over het gebruik van beheerde toepassingen en aanmeldactiviteiten van gebruikers
- **Controlelogboeken**: informatie over systeemactiviteit van gebruikers, groepsbeheer, uw beheerde toepassingen en directory-activiteiten

De volgende tabel bevat de latentie-informatie voor activiteitsrapporten.

| Rapport | Minimum | Gemiddeld |
| :-- | --- | --- |
| Controlelogboeken | 30 minuten  | 1 uur  |
| Aanmeldingen | 15 minuten  | 2 uur |

In sommige gevallen rand kunnen:

- 2 uur voor controle activiteitsgegevens worden weergegeven.
- 24 uur voor aanmeldingsactiviteiten gegevens worden weergegeven. Dit omvat activiteitsgegevens aanmeldingen die afkomstig zijn van verouderde office-toepassingen. 


## <a name="security-reports"></a>Beveiligingsrapporten

Er zijn twee soorten beveiliging reporting:

- **Riskante aanmeldingen** - Een riskante aanmelding is een indicator van een aanmeldingspoging die mogelijk is uitgevoerd door iemand die geen rechtmatige eigenaar van een gebruikersaccount is. 
- **Gebruikers voor wie wordt aangegeven dat ze risico lopen** - Een riskante gebruiker is een indicator van een gebruikersaccount dat mogelijk is aangetast. 

De volgende tabel bevat de latentie-informatie voor beveiligingsrapporten.

| Rapport | Minimum | Gemiddeld | Maximum |
| :-- | --- | --- | --- |
| Gebruikers die risico lopen          | 5 minuten   | 15 minuten  | 2 uur  |
| Riskant aanmeldingen         | 5 minuten   | 15 minuten  | 2 uur  |

## <a name="risk-events"></a>Risicogebeurtenissen

Azure Active Directory maakt gebruik van geavanceerde machine learning-algoritmen en methodiek voor het detecteren van verdachte acties die zijn gekoppeld aan uw gebruikersaccounts. Elke verdachte actie wordt opgeslagen in een gebeurtenis vastleggen genoemd risico gedetecteerd.

De volgende tabel bevat de latentie-informatie voor risico's.

| Rapport | Minimum | Gemiddeld | Maximum |
| :-- | --- | --- | --- |
| Aanmeldingen vanaf anonieme IP-adressen |5 minuten |15 minuten |2 uur |
| Aanmeldingen vanaf onbekende locaties |5 minuten |15 minuten |2 uur |
| Gebruikers van wie de referenties zijn gelekt |2 uur |4 uur |8 uur |
| Onmogelijke reis naar ongewone locaties |5 minuten |1 uur |8 uur  |
| Aanmeldingen vanaf geïnfecteerde apparaten |2 uur |4 uur |8 uur  |
| Aanmeldingen van IP-adressen met verdachte activiteit |2 uur |4 uur |8 uur  |



## <a name="next-steps"></a>Volgende stappen

Als u meer weten over de activiteitsrapporten in de Azure portal wilt, raadpleegt u:

- [Aanmeldingsactiviteiten rapporten in de Azure Active Directory-portal](active-directory-reporting-activity-sign-ins.md)
- [Controlerapporten van activiteit in de Azure Active Directory-portal](active-directory-reporting-activity-audit-logs.md)

Als u meer weten over de beveiligingsrapporten in de Azure portal wilt, raadpleegt u:

- [Gebruikers op beveiligingsrapport risico's in de Azure Active Directory-portal](active-directory-reporting-security-user-at-risk.md)
- [Riskant aanmeldingen rapport in de Azure Active Directory-portal](active-directory-reporting-security-risky-sign-ins.md)

Als u meer weten over de risico's wilt, Zie [Azure Active Directory-risicogebeurtenissen](active-directory-reporting-risk-events.md).

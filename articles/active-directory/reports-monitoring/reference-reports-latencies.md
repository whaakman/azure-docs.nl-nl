---
title: Azure Active Directory reporting latenties | Microsoft Docs
description: Meer informatie over de hoeveelheid tijd die nodig is voor rapportage gebeurtenissen worden weergegeven in uw Azure-portal
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 12/15/2017
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: b81c66acc0a90ba9b74cf1f4fb34ef7a545837f9
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45736603"
---
# <a name="azure-active-directory-reporting-latencies"></a>Azure Active Directory reporting latenties

Met [reporting](../active-directory-preview-explainer.md) in de Azure Active Directory, krijgt u alle informatie die u nodig hebt om te bepalen hoe het gaat met uw omgeving. De hoeveelheid tijd die nodig is voor het melden van gegevens worden weergegeven in de Azure-portal is ook wel bekend als latentie. 

In dit onderwerp vindt u de latentiegegevens voor alle reporting categorieën in de Azure-portal. 


## <a name="activity-reports"></a>Activiteitsrapporten

Er zijn twee gebieden van het melden van activiteit:

- **Aanmeldactiviteiten**: informatie over het gebruik van beheerde toepassingen en aanmeldactiviteiten van gebruikers
- **Controlelogboeken**: informatie over systeemactiviteit van gebruikers, groepsbeheer, uw beheerde toepassingen en directory-activiteiten

De volgende tabel bevat de latentie-informatie voor activiteitenrapporten.

| Rapport | Latentie (95%) |Latentie (99%)|
| :-- | --- | --- | 
| Controlelogboeken | 2 minuten  | 5 minuten  |
| Aanmeldingen | 2 minuten  | 5 minuten |


## <a name="security-reports"></a>Beveiligingsrapporten

Er zijn twee soorten beveiligingsrapporten:

- **Riskante aanmeldingen** - Een riskante aanmelding is een indicator van een aanmeldingspoging die mogelijk is uitgevoerd door iemand die geen rechtmatige eigenaar van een gebruikersaccount is. 
- **Gebruikers voor wie wordt aangegeven dat ze risico lopen** - Een riskante gebruiker is een indicator van een gebruikersaccount dat mogelijk is aangetast. 

De volgende tabel bevat de latentie-informatie voor beveiligingsrapporten.

| Rapport | Minimum | Gemiddeld | Maximum |
| :-- | --- | --- | --- |
| Gebruikers die risico lopen          | 5 minuten   | 15 minuten  | 2 uur  |
| Riskante aanmeldingen         | 5 minuten   | 15 minuten  | 2 uur  |

## <a name="risk-events"></a>Risicogebeurtenissen

Azure Active Directory maakt gebruik van geavanceerde machine learning-algoritmen en methodieken voor het detecteren van verdachte activiteit die is gekoppeld aan uw gebruikersaccounts. Elke gedetecteerde verdachte actie wordt opgeslagen in een record met de naam risicogebeurtenis.

De volgende tabel bevat de latentie-informatie voor risicogebeurtenissen.

| Rapport | Minimum | Gemiddeld | Maximum |
| :-- | --- | --- | --- |
| Aanmeldingen vanaf anonieme IP-adressen |5 minuten |15 minuten |2 uur |
| Aanmeldingen vanaf onbekende locaties |5 minuten |15 minuten |2 uur |
| Gebruikers van wie de referenties zijn gelekt |2 uur |4 uur |8 uur |
| Onmogelijke reis naar ongewone locaties |5 minuten |1 uur |8 uur  |
| Aanmeldingen vanaf geïnfecteerde apparaten |2 uur |4 uur |8 uur  |
| Aanmeldingen van IP-adressen met verdachte activiteit |2 uur |4 uur |8 uur  |



## <a name="next-steps"></a>Volgende stappen

Als u meer weten over de aanmeldactiviteitenrapporten in Azure portal wilt, Zie:

- [Rapporten van aanmeldingsactiviteiten in de Azure Active Directory-portal](concept-sign-ins.md)
- [Controleactiviteitenrapporten in de Azure Active Directory-portal](concept-audit-logs.md)

Als u meer weten over de beveiligingsrapporten in Azure portal wilt, Zie:

- [Gebruikers op risico's security rapport in de Azure Active Directory-portal](concept-user-at-risk.md)
- [Rapport riskante aanmeldingen in de Azure Active Directory-portal](concept-risky-sign-ins.md)

Als u meer weten over risicogebeurtenissen wilt, Zie [Azure Active Directory-risicogebeurtenissen](concept-risk-events.md).

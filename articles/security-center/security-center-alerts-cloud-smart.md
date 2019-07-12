---
title: Slimme waarschuwing correlatie in Azure Security Center (incidenten) in de cloud | Microsoft Docs
description: In dit onderwerp wordt uitgelegd hoe fusion maakt gebruik van cloud slimme waarschuwing correlatie voor het genereren van beveiligingsincidenten in Azure Security Center.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: e9d5a771-bfbe-458c-9a9b-a10ece895ec1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: 2ab4dab8cb7729b0c2ca023f22066f7b5d166a02
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571775"
---
# <a name="cloud-smart-alert-correlation-in-azure-security-center-incidents"></a>Slimme waarschuwing correlatie in Azure Security Center (incidenten) in de cloud

Security Center analyseert voortdurend de workloads van hybride Clouds met behulp van geavanceerde analyses en bedreigingsinformatie om u te waarschuwen over schadelijke activiteiten.

Als de breedte van de bedreiging dekking groeit en de noodzaak voor het detecteren van zelfs de geringste indicatie van toeneemt aangetast, kan het lastig zijn voor beveiligingsanalisten sorteren van de verschillende waarschuwingen en een werkelijke aanval worden geïdentificeerd. Analisten kunnen omgaan met waarschuwing intensief gebruik en onderzoeken van aanvallen als deze zich voordoen, Azure Security Center helpt door de verschillende waarschuwingen en geringe signalen in beveiligingsincidenten.

Fusion is de technologie en analytische back-end die door Security Center incidenten, waardoor het correleren van verschillende waarschuwingen en contextuele signalen samen. Passen werkt door te kijken naar de andere signalen gerapporteerd voor een abonnement voor de resources en zoeken naar bekende patronen die laat zien hoe aanval of signalen met gedeelde contextuele informatie die een procedure geïntegreerde antwoord geeft aan dat moet worden voor deze genomen.

Kennis van de beveiliging combineren Fusion analytics met AI voor het analyseren van waarschuwingen, nieuwe aanvalspatronen detecteren zodra ze zich voordoen. 

Security Center maakt gebruik van MITRE aanval Matrix in om waarschuwingen te koppelen met de bedoeling waargenomen, zodat formaliseren kennis van de beveiliging. Security Center kunt bovendien met behulp van de informatie die verzameld voor elke stap van een aanval, regel activiteit die wordt weergegeven om te worden de stappen van een aanval, maar is niet uit.  

Aangezien aanvallen vaak in andere tenants voorkomen, kunt AI-algoritmen voor het analyseren van de aanval reeksen die worden gerapporteerd in elk abonnement om te bepalen of deze bekende patronen van waarschuwing in plaats van alleen incidenteel niet gekoppeld aan elk combineren door Security Center andere.

Tijdens een onderzoek van een incident moeten analisten vaak extra context een eindconclusie over de aard van de bedreiging en het oplossen van deze bereiken. Bijvoorbeeld, zelfs wanneer een netwerk van afwijkingen wordt gedetecteerd, is zonder inzicht krijgen in wat er gebeurt in het netwerk of met betrekking tot de betreffende resource moeilijk te begrijpen welke acties op kunt uitvoeren. Om te helpen, kan een beveiligingsincident artefacten, gerelateerde gebeurtenissen en informatie bevatten. De aanvullende informatie beschikbaar voor beveiligingsincidenten, is afhankelijk van het type bedreiging is gedetecteerd en de configuratie van uw omgeving. 

![Beveiligingsdetails van incident](./media/security-center-alerts-cloud-smart/security-incident.png)

Zie voor meer informatie over beveiligingsincidenten, [hoe u voor het afhandelen van beveiligingsincidenten in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-incident).


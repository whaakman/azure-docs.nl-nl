---
title: Beveiligingswaarschuwingen in Azure Security Center | Microsoft Docs
description: In dit onderwerp wordt uitgelegd wat zijn beveiligingswaarschuwingen en de verschillende typen in Azure Security Center.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: 1b71e8ad-3bd8-4475-b735-79ca9963b823
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: 06f41bbfd97d5deb59e7bfd08615b2f28e256070
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571671"
---
# <a name="security-alerts-in-azure-security-center"></a>Beveiligingswaarschuwingen in Azure Security Center

In dit artikel geeft de verschillende typen beveiligingswaarschuwingen er beschikbaar in Azure Security Center (ASC). Er zijn tal van waarschuwingen voor de vele verschillende resourcetypen. ASC genereert waarschuwingen voor beide resources die zijn geïmplementeerd op Azure en resources die worden geïmplementeerd op on-premises en hybride cloudomgevingen. 

## <a name="what-are-security-alerts"></a>Wat zijn beveiligingswaarschuwingen?

Meldingen zijn de berichten die Security Center worden gegenereerd wanneer bedreigingen voor uw resources zijn gedetecteerd. Dit bepaalt de volgorde en geeft een lijst van de waarschuwingen samen met de informatie die nodig zijn voor u het probleem snel te onderzoeken. Security Center bevat ook aanbevelingen voor het herstellen van een aanval.

## <a name="how-does-security-center-detect-threats"></a>Hoe detecteert Security Center bedreigingen?

Echte bedreigingen te detecteren en fout-positieven, Security Center verzamelt, analyseert, en kan worden geïntegreerd logboekgegevens van uw Azure-resources, het netwerk en verbonden partneroplossingen, zoals firewall en oplossingen voor eindpuntbeveiliging. Security Center analyseert deze gegevens vaak correleren van gegevens uit meerdere bronnen, om te identificeren van bedreigingen.

ASC bewaakt de resources in Azure geïmplementeerd of geïmplementeerd op een andere on-premises en hybride cloudomgevingen. Zie voor meer informatie over het opsporen en reageren op bedreigingen, [hoe Security Center detecteert en erop reageert op bedreigingen](security-center-detection-capabilities.md#asc-detects).

## <a name="security-alert-types"></a>Beveiliging Waarschuwingstypen

De volgende onderwerpen leiden u door de verschillende ASC-waarschuwingen op basis van resourcetypen:

* [Waarschuwingen voor IaaS VM's en Servers](security-center-alerts-iaas.md)
* [Systeemeigen rekenkracht waarschuwingen](security-center-alerts-compute.md)
* [Data services-waarschuwingen](security-center-alerts-data-services.md)

De volgende onderwerpen wordt uitgelegd hoe Security Center maakt gebruik van de verschillende telemetrie wordt verzameld van integratie met de Azure-infrastructuur om toe te passen van lagen van de extra beveiliging voor resources die zijn geïmplementeerd op Azure:

* [Servicewaarschuwingen met laag](security-center-alerts-service-layer.md)
* [Integratie met Azure Security-producten](security-center-alerts-integration.md)

## <a name="what-are-alert-incidents"></a>Wat zijn waarschuwing incidenten?

Een beveiligingsincident is een verzameling van gerelateerde waarschuwingen, in plaats van elke waarschuwing afzonderlijk aanbieding. Security Center gebruikt fusion voor verschillende waarschuwingen en geringe signalen in beveiligingsincidenten correleren.

Met behulp van incidenten, biedt Security Center u een weergave van een aanval en alle gerelateerde waarschuwingen. In deze weergave kunt u snel inzicht in wat de aanvaller deed en welke resources betrokken zijn. Zie voor meer informatie, [Cloud slimme waarschuwing correlatie](security-center-alerts-cloud-smart.md).

## <a name="get-started-with-alerts"></a>Aan de slag met waarschuwingen

Zie de volgende onderwerpen, Leer meer over de resources die wordt bewaakt door ASC, en voor richtlijnen over het reageren op de waarschuwingen weergegeven door ASC te begrijpen.

* Zie voor welke platformen en onderdelen zijn beveiligd door ASC [Platforms en functies die worden ondersteund door Azure Security Center](security-center-os-coverage.md).  
* Zie voor meer informatie over wat beveiligingsincidenten zijn en hoe ze ASC beantwoordt, [hoe u voor het afhandelen van beveiligingsincidenten in Azure Security Center](security-center-incident.md). 
* Zie voor meer informatie over het beheren van waarschuwingen die u ontvangt, [beheren en erop reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md).
* Voor informatie over het valideren dat Security Center correct is geconfigureerd en stimuleren van een testwaarschuwing voor een, Zie [validatie van waarschuwingen in Azure Security Center](security-center-alert-validation.md).  


## <a name="upgrade-to-standard-for-advanced-detections"></a>Upgrade naar Standard voor geavanceerde detectie

Als u geavanceerde detectie wilt instellen, voert u een upgrade uit naar Azure Security Center Standard. 

1. Selecteer in het menu Security Center **beveiligingsbeleid**.
2. Voor de abonnementen die u wilt verplaatsen naar de Standard-laag, klikt u op **instellingen bewerken**. 
3. Selecteer in de pagina instellingen **prijscategorie**. 
   Er is een gratis proefversie beschikbaar voor een maand. Voor meer informatie over meer naar de [pagina met prijzen](https://azure.microsoft.com/pricing/details/security-center/). 

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd wat zijn beveiligingswaarschuwingen en de verschillende typen waarschuwingen die beschikbaar zijn in Security Center. Zie de volgende onderwerpen voor meer informatie:

* [Plannings- en bedieningsgids voor Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)
* [Veelgestelde vragen over Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-faq): Raadpleeg de veelgestelde vragen over het gebruik van de service.


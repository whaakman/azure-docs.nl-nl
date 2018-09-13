---
title: Slimme detectie - detectie-Pack beveiliging met Azure Application Insights | Microsoft Docs
description: Controleer de App met Azure Application Insights voor potentiële beveiligingsproblemen met zich mee.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: mbullwin
ms.openlocfilehash: edfe2956da84347abf3e5da7a7d0fd448361c7f5
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35643811"
---
# <a name="application-security-detection-pack-preview"></a>Application security detectie pack (preview)

Application Insights wordt automatisch de telemetriegegevens die zijn gegenereerd door uw toepassing te analyseren en detecteert mogelijke beveiligingsproblemen. Deze mogelijkheid kunt u mogelijke beveiligingsproblemen te identificeren en deze te verwerken door de toepassing op te lossen of door de benodigde veiligheidsmaatregelen nemen.

Deze functie is vereist geen speciale configuratie dan [configureren van uw app om telemetrie te verzenden](https://docs.microsoft.com/azure/application-insights/app-insights-usage-overview).

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Wanneer kan ik dit type melding voor slimme detectie krijgen?
Er zijn drie typen beveiligingsproblemen die worden gedetecteerd:
1. Onbeveiligde toegang tot de URL's: een URL in de toepassing wordt geopend via HTTP en HTTPS. Normaal gesproken moet een URL die HTTPS-aanvragen accepteert geen HTTP-aanvragen accepteren. Dit kan duiden op een bug of beveiliging oplossen in uw toepassing.
2. Onveilige vorm: een formulier (of een andere aanvraag "POST") in de toepassing maakt gebruik van HTTP in plaats van HTTPS. Met behulp van HTTP, kan de gegevens die door het formulier wordt verzonden in gevaar brengen.
3. Verdachte gebruikersactiviteiten: de toepassing wordt geopend meerdere landen door dezelfde gebruiker op ongeveer hetzelfde moment. Bijvoorbeeld, vanuit dezelfde gebruiker de toepassing Spanje en de Verenigde Staten in datzelfde uur. Deze detectie geeft aan dat een poging kwaadwillende gebruikers toegang krijgen tot uw toepassing.

## <a name="does-my-app-definitely-have-a-security-issue"></a>Beschikt over mijn app absoluut een beveiligingsprobleem?
Nee, een melding betekent niet dat uw app beslist een beveiligingsprobleem heeft. Een detectie van een van de bovenstaande scenario's kunt, in veel gevallen een beveiligingsprobleem aangeven. Echter, de detectie mogelijk een natuurlijke zakelijke rechtvaardiging en kan worden genegeerd.

## <a name="how-do-i-fix-the-insecure-url-access-detection"></a>Hoe kan ik de detectie 'Onveilig URL toegang' oplossen?
1. **Sorteren.** De melding bevat het aantal gebruikers die toegang onveilig URL's en de URL die de meeste heeft beïnvloed door onveilige toegang. Hiermee kunt u een prioriteit toewijzen aan het probleem.
2. **De scope.** Welk percentage van de gebruikers toegang krijgen tot onveilig URL's? Het aantal URL's zijn beïnvloed? Deze gegevens kan worden opgehaald van de melding.
3. **Diagnose.** De detectie bevat de lijst van onbeveiligde aanvragen en de lijst met URL's en gebruikers die zijn getroffen, kunt u het probleem nader onderzoek.

## <a name="how-do-i-fix-the-insecure-form-detection"></a>Hoe kan ik de detectie 'Onveilige vorm' oplossen?
1. **Sorteren.** De melding bevat het aantal onveilig formulieren en het aantal gebruikers waarvan de gegevens mogelijk is aangetast. Hiermee kunt u een prioriteit toewijzen aan het probleem.
2. **De scope.** Welke vorm is betrokken bij het grootste aantal overdrachten van niet-beveiligd, en wat de distributie van onbeveiligde verzendingen na verloop van tijd? Deze gegevens kan worden opgehaald van de melding.
3. **Diagnose.** De detectie bevat de lijst van onbeveiligde formulieren en een uitsplitsing van het aantal onveilig verzendingen voor elk formulier, kunt u het probleem nader onderzoek.

## <a name="how-do-i-fix-the-suspicious-user-activity-detection"></a>Hoe kan ik de detectie 'verdachte gebruikersactiviteiten' oplossen?
1. **Sorteren.** De melding geeft het aantal verschillende gebruikers die het verdachte gedrag werd gehost. Hiermee kunt u een prioriteit toewijzen aan het probleem.
2. **De scope.** In welke landen de verdachte aanvragen afkomstig zijn? Welke gebruiker is het meest verdacht? Deze gegevens kan worden opgehaald van de melding.
3. **Diagnose.** De detectie bevat de lijst met verdachte gebruikers en de lijst met landen voor elke gebruiker, kunt u het probleem nader onderzoek.

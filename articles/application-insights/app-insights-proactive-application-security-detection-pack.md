---
title: Detectie - detectie-Pack beveiliging met Azure Application Insights van smartcard | Microsoft Docs
description: Bewaken met Azure Application Insights-toepassing voor mogelijke beveiligingsproblemen.
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: mbullwin
ms.openlocfilehash: 837525d70f28c0710b8a8b86d411a7854402207f
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="application-security-detection-pack-preview"></a>Toepassing beveiliging detectie pack (preview)

Application Insights automatisch analyseert de telemetrie door uw toepassing wordt gegenereerd en detecteert mogelijke beveiligingsproblemen. Deze mogelijkheid kunt u mogelijke beveiligingsproblemen identificeren en deze te verwerken door de toepassing is hersteld of de benodigde veiligheidsmaatregelen te nemen.

Dit onderdeel vereist geen speciale instellingen, anders dan [configureren van uw app om het verzenden van telemetrie](https://docs.microsoft.com/azure/application-insights/app-insights-usage-overview).

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Wanneer dit type Slimme detectie melding krijgt?
Er zijn drie typen beveiligingsproblemen die worden gedetecteerd:
1. Onbeveiligde URL toegang: een URL in de toepassing wordt geopend via HTTP en HTTPS. Normaal gesproken moet een URL die HTTPS-aanvragen accepteert geen HTTP-aanvragen accepteren. Dit kan duiden op een bug of beveiliging probleem in uw toepassing.
2. Onveilige vorm: een formulier (of een andere aanvraag "POST") in de toepassing maakt gebruik van HTTP in plaats van HTTPS. Met behulp van HTTP, kan de gebruikersgegevens die worden verzonden door het formulier in gevaar brengen.
3. Met verdachte gebruikersactiviteit: de toepassing wordt geopend uit meerdere landen door dezelfde gebruiker rond dezelfde tijd. Bijvoorbeeld, vanuit dezelfde gebruiker de toepassing Spanje en de Verenigde Staten binnen hetzelfde uur. Deze detectie duidt dit op een poging kwaadwillende gebruikers toegang krijgen tot uw toepassing.

## <a name="does-my-app-definitely-have-a-security-issue"></a>Beschikt over mijn app zeker een beveiligingsprobleem?
Nee, een melding betekent niet dat uw app zeker een beveiligingsprobleem heeft. Een detectie van een van de bovenstaande scenario's in veel gevallen kan duiden op een beveiligingsprobleem. Echter, de detectie heeft misschien een natuurlijke zakelijke reden en kan worden genegeerd.

## <a name="how-do-i-fix-the-insecure-url-access-detection"></a>Hoe kan ik de detectie 'Onbeveiligde URL toegang' oplossen?
1. **Selectie.** De melding levert het aantal gebruikers die benaderd onbeveiligde URL's en de URL die de meeste is van invloed op een onbeveiligde toegang. Hiermee kunt u een prioriteit toewijzen aan het probleem.
2. **De scope.** Welk percentage van de gebruikers toegankelijk onbeveiligde URL's? Hoeveel URL's is van invloed op? Deze informatie kan worden verkregen van de melding.
3. **Diagnose.** De detectie voorziet in de lijst van onbeveiligde aanvragen en de lijst met URL's en gebruikers die zijn getroffen, kunt u het probleem nader onderzoek.

## <a name="how-do-i-fix-the-insecure-form-detection"></a>Hoe kan ik de detectie "Onveilige vorm" oplossen?
1. **Selectie.** De melding biedt het aantal onbeveiligde formulieren en het aantal gebruikers waarvan de gegevens mogelijk wordt geschonden. Hiermee kunt u een prioriteit toewijzen aan het probleem.
2. **De scope.** Welke vorm is betrokken bij het grootste aantal onbeveiligde gegevensoverdrachten en wat de distributie van onbeveiligde gegevensoverdrachten gedurende een bepaalde periode is? Deze informatie kan worden verkregen van de melding.
3. **Diagnose.** De detectie voorziet in de lijst met onbeveiligde formulieren en een uitsplitsing van het aantal onbeveiligde gegevensoverdrachten voor elk formulier, kunt u het probleem nader onderzoek.

## <a name="how-do-i-fix-the-suspicious-user-activity-detection"></a>Hoe kan ik de detectie 'verdachte gebruikersactiviteit' oplossen?
1. **Selectie.** De melding levert het aantal verschillende gebruikers waarmee de verdacht gedrag optrad. Hiermee kunt u een prioriteit toewijzen aan het probleem.
2. **De scope.** In welke landen is de verdachte aanvragen afkomstig? Welke gebruiker de verdachte is? Deze informatie kan worden verkregen van de melding.
3. **Diagnose.** De detectie voorziet in de lijst met verdachte gebruikers en de lijst met landen voor elke gebruiker, kunt u het probleem nader onderzoek.

---
title: Slimme detectie - mogelijk geheugenlekkage gedetecteerd door Azure Application Insights | Microsoft Docs
description: Toepassingen met Azure Application Insights voor mogelijke geheugenlekken controleren.
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
ms.openlocfilehash: 25d26db3cd11fff7f7e9ba472247a920ecddea33
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47090731"
---
# <a name="memory-leak-detection-preview"></a>Detectie van geheugen-geheugenlek (preview)

Application Insights automatisch analyseert het geheugengebruik van elk proces in uw toepassing, en kunt u gewaarschuwd over mogelijke geheugenlekken of de toegenomen geheugengebruik.

Deze functie is vereist geen speciale configuratie dan [configureren van prestatiemeteritems](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) voor uw app. Deze is actief wanneer uw app voldoende geheugen prestaties tellers telemetrie (bijvoorbeeld, privé-Bytes genereert).

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Wanneer kan ik dit type melding voor slimme detectie krijgen?
Een typische melding volgt een consistente toename in het geheugenverbruik gedurende een lange periode, in een of meer processen en/of een of meer machines die deel van uw toepassing uitmaken. Machine learning-algoritmen worden gebruikt voor het detecteren van toegenomen geheugengebruik die overeenkomt met het patroon van een geheugenlek.

## <a name="does-my-app-really-have-a-problem"></a>Beschikt over mijn app echt een probleem?
Nee, een melding betekent niet dat uw app beslist een probleem is. Hoewel geheugen geheugenlek patronen duiden meestal op een toepassingsprobleem, deze patronen kunnen zijn voor uw specifieke proces, of een natuurlijke zakelijke reden kunnen zijn, en kunnen worden genegeerd.

## <a name="how-do-i-fix-it"></a>Hoe herstel ik deze?
De meldingen betreffen: diagnostische gegevens voor de ondersteuning in het analyseproces diagnostische:
1. **Sorteren.** De melding ziet u de hoeveelheid geheugen (in GB) verhogen en de periode waarin het geheugen is toegenomen. Hiermee kunt u een prioriteit toewijzen aan het probleem.
2. **De scope.** Het aantal machines werd het patroon van geheugen geheugenlek gehost? Het aantal uitzonderingen zijn geactiveerd tijdens de mogelijk geheugenlekkage? Deze gegevens kan worden opgehaald van de melding.
3. **Diagnose.** De detectie bevat het patroon geheugen geheugenlek, geheugengebruik van het proces na verloop van tijd weergeven. U kunt ook de verwante items en rapporten koppelen aan de ondersteunende informatie om u te helpen meer vaststellen van het probleem.

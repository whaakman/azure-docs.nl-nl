---
title: "Detectie - potentiële geheugenlek gedetecteerd door Azure Application Insights van smartcard | Microsoft Docs"
description: "Toepassingen met Azure Application Insights voor potentiële geheugenlekken bewaken."
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
ms.openlocfilehash: e98caaa387418d746905990436b69925a591b260
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="memory-leak-detection-preview"></a>Geheugen-geheugenlek detectie (preview)

Application Insights automatisch analyseert het geheugengebruik van elk proces in uw toepassing en kunt u gewaarschuwd over mogelijke geheugenlekken of geheugenverbruik toegenomen.

Dit onderdeel vereist geen speciale instellingen, anders dan [prestatiemeteritems configureren](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) voor uw app. Het is actief wanneer uw app voldoende geheugen prestaties tellers telemetrie (bijvoorbeeld persoonlijke Bytes genereert).


## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Wanneer dit type Slimme detectie melding krijgt?
Een typische melding volgen een consistente toename in het geheugenverbruik gedurende een lange periode (een paar uur) in een of meer processen en/of een of meer machines die deel van uw toepassing uitmaken.
Machine learning-algoritmen worden gebruikt voor het detecteren van meer geheugen-verbruik dat overeenkomt met een patroon van een geheugenlek, in tegenstelling tot verhoogde geheugenverbruik vanwege natuurlijk toepassingsgebruik verhogen.

## <a name="does-my-app-definitely-have-a-problem"></a>Beschikt over mijn app zeker een probleem?
Nee, een melding betekent dat uw app zeker een probleem heeft niet. Hoewel geheugen-geheugenlek patronen duiden meestal op een toepassingsprobleem, kunnen deze patronen typische uw specifieke wilt verwerken, of een natuurlijke zakelijke reden kunnen zijn, en kunnen worden genegeerd.

## <a name="how-do-i-fix-it"></a>Hoe kan ik oplossen?
De meldingen betreffen: diagnostische gegevens voor de ondersteuning van het proces diagnostische analyse:
1. **Selectie.** De melding ziet u de hoeveelheid geheugen (in GB) te verbeteren en de periode waarin het geheugen is toegenomen. Hiermee kunt u een prioriteit toewijzen aan het probleem.
2. **De scope.** Het aantal machines doordat het patroon van geheugen-geheugenlek? Het aantal uitzonderingen zijn gegenereerd tijdens het potentiële geheugenlek? Deze informatie kan worden verkregen van de melding.
3. **Diagnose.** De detectie bevat het patroon geheugen-geheugenlek, geheugenverbruik van het proces na verloop van tijd wordt weergegeven. U kunt ook de verwante items en rapporten koppelen aan de ondersteunende informatie om u te helpen verder onderzoeken het probleem.

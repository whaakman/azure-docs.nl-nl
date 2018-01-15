---
title: Detectie - laag gebruik van cloudbronnen die zijn gedetecteerd door Azure Application Insights van smartcard | Microsoft Docs
description: Bewaken van toepassingen met Azure Application Insights voor laag gebruik van cloudbronnen.
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
ms.openlocfilehash: 8382f6047ae222a01cc0e8d6ca9dcf5593d0dff6
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="low-utilization-of-cloud-resources-preview"></a>Laag gebruik van cloudbronnen (preview)

Application Insights automatisch analyseert het CPU-verbruik van elke rolinstantie in uw toepassing en detecteert exemplaren met minder CPU-gebruik. Deze detectie kunt u uw Azure-resources te verkleinen en kosten verminderen door het verlagen van het aantal rolinstanties die maakt gebruik van elke rol, of door het verlagen van het aantal rollen.

Dit onderdeel vereist geen speciale instellingen, anders dan [prestatiemeteritems configureren](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) voor uw app. Het is actief wanneer uw app onvoldoende CPU prestaties teller telemetrie (% processortijd genereert).

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Wanneer dit type Slimme detectie melding krijgt?
Een typische melding treedt op wanneer veel van uw Web/Worker rolinstanties lage CPU-gebruik vertonen.

## <a name="does-my-app-definitely-consume-too-many-resources"></a>Mijn app zeker te veel bronnen gebruiken?
Nee, een melding betekent niet dat uw app zeker te veel resources verbruikt. Hoewel deze patronen in het lage CPU-gebruik duiden meestal op dat het brongebruik kan worden verminderd, dit gedrag gebruikelijke voor uw specifieke rol kan worden of een natuurlijke zakelijke reden kan zijn, en kan worden genegeerd. Bijvoorbeeld mogelijk dat meerdere exemplaren nodig zijn voor andere resources, zoals geheugen/netwerk- en niet de CPU.

## <a name="how-do-i-fix-it"></a>Hoe kan ik oplossen?
Diagnostische gegevens voor de ondersteuning van het proces van diagnostische gegevens bevatten, de meldingen:
1. **Selectie.** De melding ziet u de rollen in uw app met minder CPU-gebruik. Hiermee kunt u een prioriteit toewijzen aan het probleem.
2. **De scope.** Hoeveel rollen optrad lage CPU-gebruik en hoeveel exemplaren in elke rol lage CPU gebruiken? Deze informatie kan worden verkregen van de melding.
3. **Diagnose.** De detectie bevat het percentage van CPU benut, CPU-gebruik gedurende een periode van elke instantie weergegeven. U kunt ook de gerelateerde artikelen en rapporten koppelen aan de ondersteunende informatie, zoals percentielen CPU-gebruik, kunt u het probleem nader onderzoek.

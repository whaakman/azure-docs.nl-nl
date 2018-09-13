---
title: Slimme detectie - laag gebruik van cloudbronnen die zijn gedetecteerd door Azure Application Insights | Microsoft Docs
description: Bewaken van toepassingen met Azure Application Insights voor laag gebruik van cloudresources.
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
ms.openlocfilehash: ca4f944f605db96a2cedf2682f3ff4c811007ffb
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35643752"
---
# <a name="low-cpu-utilization-in-cloud-resources-preview"></a>Laag CPU-gebruik van cloudresources (preview)

Application Insights wordt automatisch analyseert de CPU-verbruik van elke rolinstantie in uw toepassing en -exemplaren met minder CPU-gebruik wordt gedetecteerd. Deze detectie kunt u uw Azure-resources te verlagen en lagere kosten, door het verlagen van het aantal rolinstanties die maakt gebruik van elke rol, of door het verlagen van het aantal rollen.

Deze functie is vereist geen speciale configuratie dan [configureren van prestatiemeteritems](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) voor uw app. Deze is actief wanneer uw app onvoldoende CPU prestaties teller telemetrie (% processortijd genereert).

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Wanneer kan ik dit type melding voor slimme detectie krijgen?
Een typische melding treedt op wanneer met de exemplaren van uw Web-/ Werkrol minder CPU-gebruik vertonen.

## <a name="does-my-app-definitely-consume-too-many-resources"></a>Mijn app definitief te veel resources verbruiken?
Nee, een melding betekent niet dat uw app definitief te veel resources verbruikt. Hoewel deze patronen van lage CPU-gebruik geven meestal aan dat het gebruik van resources kan worden verminderd, dit gedrag kan zijn voor uw specifieke rol of een natuurlijke zakelijke reden kan zijn, en kan worden genegeerd. Het kan bijvoorbeeld zijn dat meerdere exemplaren nodig zijn voor andere resources, zoals geheugen/netwerk- en niet CPU.

## <a name="how-do-i-fix-it"></a>Hoe herstel ik deze?
De meldingen betreffen: diagnostische gegevens voor de ondersteuning in het proces van diagnostische gegevens:
1. **Sorteren.** De melding ziet u de rollen in uw app met minder CPU-gebruik. Hiermee kunt u een prioriteit toewijzen aan het probleem.
2. **De scope.** Hoeveel rollen optrad minder CPU-gebruik en het aantal exemplaren in elke rol gebruikmaken van lage CPU? Deze gegevens kan worden opgehaald van de melding.
3. **Diagnose.** De detectie bevat het percentage van CPU gebruikt, CPU-gebruik na verloop van tijd van elke instantie weergegeven. U kunt ook de verwante items en rapporten koppelen aan de ondersteunende informatie, zoals percentielen van de CPU-gebruik, kunt u het probleem nader onderzoek.

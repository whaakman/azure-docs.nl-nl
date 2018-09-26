---
title: 'Slimme detectie: afwijkende toename van uitzonderingen, in Azure Application Insights | Microsoft Docs'
description: Toepassingsuitzonderingen met Azure Application Insights voor ongebruikelijke patronen van uitzonderingen bewaken.
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
ms.date: 12/08/2017
ms.author: mbullwin
ms.openlocfilehash: 898cc0935051f65cb0f2977c7d90e998ec32cdd3
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47093941"
---
# <a name="abnormal-rise-in-exception-volume-preview"></a>Abnormale toename van uitzonderingen (preview)

Application Insights automatisch analyseert de uitzonderingen in uw toepassing, en kunt u gewaarschuwd over ongebruikelijke patronen in uw uitzonderingstelemetrie.

Deze functie is vereist geen speciale configuratie dan [uitzondering rapportage configureren](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-exceptions#set-up-exception-reporting) voor uw app. Deze is actief wanneer uw app zoveel uitzonderingstelemetrie genereert.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Wanneer kan ik dit type melding voor slimme detectie krijgen?
U kunt dit type melding krijgen als uw app een afwijkende toename van het aantal uitzonderingen van een bepaald type gedurende een dag ten opzichte van een basislijn berekend voor de afgelopen zeven dagen vertoont is.
Machine learning-algoritmen worden gebruikt voor het detecteren van de toename van het aantal uitzonderingen, daarbij rekening houdend met een natuurlijke groei van het toepassingsgebruik van uw.

## <a name="does-my-app-definitely-have-a-problem"></a>Beschikt over mijn app absoluut een probleem?
Nee, een melding betekent niet dat uw app beslist een probleem is. Hoewel een uitzonderlijk groot aantal uitzonderingen duidt meestal op een toepassingsprobleem, kunnen deze uitzonderingen goedaardige en verwerkt door uw toepassing correct zijn.

## <a name="how-do-i-fix-it"></a>Hoe herstel ik deze?
De meldingen betreffen: diagnostische gegevens voor de ondersteuning in het proces van diagnostische gegevens:
1. **Sorteren.** De melding ziet u hoeveel gebruikers of het aantal aanvragen worden beïnvloed. Hiermee kunt u een prioriteit toewijzen aan het probleem.
2. **De scope.** Het probleem invloed heeft op al het verkeer of slechts enkele bewerking? Deze gegevens kan worden opgehaald van de melding.
3. **Diagnose.** De detectie bevat informatie over de methode die de uitzondering is opgetreden, evenals het uitzonderingstype. U kunt ook de verwante items en rapporten koppelen aan de ondersteunende informatie om u te helpen meer vaststellen van het probleem.
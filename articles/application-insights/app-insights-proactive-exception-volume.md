---
title: Detectie - ongewone stijging van de uitzondering volume in Azure Application Insights van smartcard | Microsoft Docs
description: Toepassingsuitzonderingen met Azure Application Insights voor ongewone patronen in uitzondering volume bewaken.
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
ms.date: 12/08/2017
ms.author: mbullwin
ms.openlocfilehash: 8030f3331a03170bb265c417a57725544bdc7d3f
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2017
---
# <a name="abnormal-rise-in-exception-volume-preview"></a>Ongewone stijging van de uitzondering volume (preview)

Application Insights automatisch analyseert de uitzonderingen die in uw toepassing en kunt u gewaarschuwd over ongewone patronen in uw uitzonderingstelemetrie.

Dit onderdeel vereist geen speciale instellingen, anders dan [uitzondering rapportage configureren](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-exceptions#set-up-exception-reporting) voor uw app. Het is actief wanneer uw app genoeg uitzonderingstelemetrie genereert.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Wanneer dit type Slimme detectie melding krijgt?
U kunt dit type melding als uw app een abnormale toename in het aantal uitzonderingen van een bepaald type tijdens een dag vertoont, vergeleken met een basislijn berekend gedurende de afgelopen zeven dagen.
Machine learning-algoritmen worden gebruikt voor het detecteren van de toename in aantal uitzondering, daarbij rekening houdend met een natuurlijke groei van uw gebruik van de toepassing.

## <a name="does-my-app-definitely-have-a-problem"></a>Beschikt over mijn app zeker een probleem?
Nee, een melding betekent dat uw app zeker een probleem heeft niet. Hoewel een uitzonderlijk groot aantal uitzonderingen geeft meestal aan een toepassingsprobleem dat, kunnen deze uitzonderingen goedaardige en verwerkt door uw toepassing correct zijn.

## <a name="how-do-i-fix-it"></a>Hoe kan ik oplossen?
Diagnostische gegevens voor de ondersteuning van het proces van diagnostische gegevens bevatten, de meldingen:
1. **Selectie.** De melding ziet u hoeveel gebruikers of hoeveel aanvragen worden beïnvloed. Hiermee kunt u een prioriteit toewijzen aan het probleem.
2. **De scope.** Het probleem invloed op alle verkeer, of een enkele bewerking? Deze informatie kan worden verkregen van de melding.
3. **Diagnose.** De detectie bevat informatie over de methode van waaruit de uitzondering is opgetreden, evenals het uitzonderingstype. U kunt ook de verwante items en rapporten koppelen aan de ondersteunende informatie om u te helpen verder onderzoeken het probleem.
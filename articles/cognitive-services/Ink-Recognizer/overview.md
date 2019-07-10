---
title: Wat is de Ink-herkenning? -Ink herkenning API
titleSuffix: Azure Cognitive Services
description: De Ink-herkenning integreren in uw toepassingen, websites, hulpprogramma's en andere oplossingen om toe te staan ink lijn gegevens kunnen worden geïdentificeerd en gebruikt als invoer.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: overview
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 95121523c510e72894086740ad340bdfd33f3c32
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67721397"
---
# <a name="what-is-the-ink-recognizer-api"></a>Wat is de Ink Recognizer-API?


De Ink herkenning Cognitive Service biedt een REST-API op basis van een cloud te analyseren en te herkennen digitale ink-inhoud. In tegenstelling tot de services die gebruikmaken van optische tekenherkenning (OCR), vereist dat de API digitale inkt lijn gegevens als invoer. Digitale inkt lijnen zijn sets tijd geordende opeenvolgende 2D-punten (X, Y-coördinaten) die staan voor de beweging van de invoer-hulpprogramma's zoals digitale pennen of vingers. Vervolgens herkent de vormen en handgeschreven inhoud uit de invoer en retourneert een JSON-antwoord met alle herkende entiteiten.

![Een stroomdiagram met een beschrijving van ink lijn invoer voor de API verzenden](media/ink-recognizer-pen-graph.png)

## <a name="features"></a>Functies

Met de Ink herkenning API kunt kunt u gemakkelijk herkennen handgeschreven inhoud in uw toepassingen. 

|Functie  |Description  |
|---------|---------|
| Handschriftherkenning | Herkennen handgeschreven inhoud in 63 core [talen en landinstellingen](language-support.md). | 
| Herkenning van lay-out | Structurele informatie over de digitale ink-inhoud ophalen. De inhoud in de regio's, alinea's, regels, woorden, lijsten met opsommingstekens schrijven opsplitsen. Uw toepassingen kunnen vervolgens de lay outinformatie gebruiken om te maken van aanvullende functies zoals automatische lijstopmaak en uitlijning van de vorm. |
| Shape-herkenning | Herkent de meest gebruikte [geometrische vormen](concepts/send-ink-data.md#shapes-recognized-by-the-ink-recognizer-api) bij het maken van notities. |
| Gecombineerde vormen en tekstherkenning | Welke ink lijnen deel uitmaken van shapes of handgeschreven inhoud herkennen en ze afzonderlijk te classificeren.|

## <a name="workflow"></a>Werkstroom

De Ink herkenning-API is een RESTful-web-service, zodat u eenvoudig kunt aanroepen vanuit elke programmeertaal die HTTP-aanvragen te parseren van JSON.

[!INCLUDE [cognitive-services-ink-recognizer-signup-requirements](../../../includes/cognitive-services-ink-recognizer-signup-requirements.md)]

Na het aanmelden:

1. Breng uw gegevens van de lijn ink en [opmaken](concepts/send-ink-data.md#sending-ink-data) in geldige JSON.
1. Een aanvraag verzenden naar de Ink-herkenning-API met uw gegevens.
1. Verwerk de API-reactie door het geretourneerde JSON-bericht te parseren.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg een snelstartgids in de volgende talen om te beginnen met het maken van aanroepen naar de API Ink-herkenning.
* [C#](quickstarts/csharp.md)
* [Java](quickstarts/java.md)
* [JavaScript](quickstarts/csharp.md)

Als u wilt zien hoe de Ink-API in een digitale bij app werkt, bekijk de volgende voorbeeldtoepassingen op GitHub:
* [C# en Universal Windows Platform (UWP)](https://go.microsoft.com/fwlink/?linkid=2089803)  
* [C# en Windows Presentation Foundation (WPF)](https://go.microsoft.com/fwlink/?linkid=2089804)
* [Webbrowser-app (Javascript)](https://go.microsoft.com/fwlink/?linkid=2089908)       
* [Mobiele app (Java en Android)](https://go.microsoft.com/fwlink/?linkid=2089906)
* [Mobiele app (Swift en iOS)](https://go.microsoft.com/fwlink/?linkid=2089805)

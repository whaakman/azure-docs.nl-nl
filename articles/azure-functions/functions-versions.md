---
title: Overzicht van Azure Functions-runtime-versies
description: Azure Functions ondersteunt meerdere versies van de runtime. Meer informatie over de verschillen tussen deze en hoe u kiest die geschikt is voor u.
services: functions
documentationcenter: 
author: ggailey777
manager: cfowler
editor: 
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: glenga
ms.openlocfilehash: 9f916aaa8032ff519709d73a1c1f51195f811686
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="azure-functions-runtime-versions-overview"></a>Overzicht van Azure Functions-runtime-versies

 Er zijn twee primaire versies van de Azure Functions-runtime.: 1.x en 2.x. Dit artikel wordt uitgelegd hoe u kunt kiezen welke primaire versie te gebruiken.

> [!IMPORTANT] 
> Runtime 1.x is de enige versie goedgekeurd voor gebruik in productieomgevingen.

| Runtime | Status |
|---------|---------|
|1.x|In het algemeen beschikbaar (GA)|
|2.x|Preview|

Zie voor meer informatie over het configureren van een functie-app of uw ontwikkelomgeving voor een bepaalde versie [hoe gericht op Azure Functions-runtime-versies](set-runtime-version.md) en [Code en test Azure Functions lokaal](functions-run-local.md).

## <a name="cross-platform-development"></a>Platformoverschrijdende ontwikkeling

Runtime 1.x ondersteunt ontwikkeling en het hosten van alleen in de portal of in Windows. Runtime 2.x wordt uitgevoerd op .NET Core, wat betekent dat deze kan worden uitgevoerd op alle platforms ondersteund door .NET Core, met inbegrip van Mac OS- en Linux. Hiermee kunnen platformoverschrijdende ontwikkeling en hostingscenario's die niet met 1.x mogelijk.

## <a name="languages"></a>Talen

Runtime 2.x maakt gebruik van een nieuw model voor de uitbreidbaarheid van taal. In eerste instantie, profiteert JavaScript en Java van dit nieuwe model. Azure Functions 1.x experimentele talen zijn niet bijgewerkt voor het gebruik van het nieuwe model, zodat ze worden niet ondersteund in 2.x. De volgende tabel geeft aan welke programmeertalen worden ondersteund in elke runtimeversie.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Zie voor meer informatie [ondersteunde talen](supported-languages.md).

## <a name="bindings"></a>Bindingen 

Runtime 2.x maakt gebruik van een nieuwe [uitbreidingsmodel voor binding](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) die de volgende voordelen biedt:

* Ondersteuning voor uitbreidingen van derden binding.
* Ontkoppelen van de runtime en bindingen. Hiermee kunt uitbreidingen van de binding samengestelde en uitgebracht onafhankelijk. U kunt bijvoorbeeld ervoor kiezen om te upgraden naar een versie van een uitbreiding die afhankelijk van een nieuwere versie van een onderliggende SDK is.
* Een lichter uitvoeringsomgeving, waarbij alleen de bindingen gebruikt bekend zijn bij en door de runtime geladen.

Alle ingebouwde Azure Functions-bindingen dit model hebt vastgesteld en niet langer standaard, met uitzondering van de Timer-trigger en de HTTP-trigger zijn opgenomen. Deze uitbreidingen worden automatisch geïnstalleerd wanneer u functies via hulpmiddelen zoals Visual Studio of via de portal maken.

De volgende tabel geeft aan welke bindingen in elke runtimeversie worden ondersteund.

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

## <a name="known-issues-in-2x"></a>Bekende problemen in 2.x

Zie voor meer informatie over ondersteuning voor bindingen en andere functionele hiaten in de 2.x [Runtime 2.0 bekende problemen](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Azure-Functions-runtime-2.0-known-issues).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Doel van de runtime 2.0 in uw lokale ontwikkelingsomgeving](functions-run-local.md)

> [!div class="nextstepaction"]
> [Zie de releaseopmerkingen voor runtime-versies](https://github.com/Azure/azure-webjobs-sdk-script/releases)
---
title: Overzicht van Azure Functions runtime-versies
description: Azure Functions ondersteunt meerdere versies van de runtime. Meer informatie over de verschillen tussen deze en kiezen die geschikt is voor u.
services: functions
documentationcenter: ''
author: ggailey777
manager: cfowler
editor: ''
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: glenga
ms.openlocfilehash: 8ba12f21d76d8deded047f40489c46657c9380b8
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38602114"
---
# <a name="azure-functions-runtime-versions-overview"></a>Overzicht van Azure Functions runtime-versies

 Er zijn twee primaire versies van de Azure Functions-runtime: 1.x en 2.x. Alleen 1.x is goedgekeurd voor gebruik in productieomgevingen. In dit artikel wordt uitgelegd wat is er nieuw in 2.x gebruikt, dat zich in de Preview-versie.

| Runtime | Status |
|---------|---------|
|1.x|In het algemeen beschikbaar is (GA)|
|2.x|Preview|

> [!NOTE] 
> Dit artikel wordt verwezen naar de cloudservice Azure Functions. Zie voor meer informatie over het product waarmee u Azure Functions on-premises uitvoert, de [overzicht van Azure Functions Runtime](functions-runtime-overview.md).

## <a name="cross-platform-development"></a>Platformoverschrijdende ontwikkeling

Runtime 1.x ondersteunt ontwikkeling en die als host fungeert voor alleen in de portal of op Windows. Runtime 2.x wordt uitgevoerd op .NET Core, wat betekent dat het kan worden uitgevoerd op alle platformen worden ondersteund door .NET Core, met inbegrip van macOS en Linux. Hiermee kunnen platformoverschrijdende ontwikkeling en hosting scenario's die niet mogelijk is met 1.x.

## <a name="languages"></a>Talen

Runtime 2.x maakt gebruik van een nieuwe taalmodel voor uitbreidbaarheid. In eerste instantie, profiteren JavaScript en Java van dit nieuwe model. Azure Functions 1.x experimentele talen zijn niet bijgewerkt voor het gebruik van het nieuwe model, zodat ze worden niet ondersteund in 2.x. De volgende tabel geeft aan welke programmeertalen worden ondersteund in elke runtimeversie.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Zie voor meer informatie, [ondersteunde talen](supported-languages.md).

## <a name="bindings"></a>Bindingen 

Runtime 2.x maakt gebruik van een nieuwe [binding uitbreidingsmodel](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) die deze voordelen biedt:

* Ondersteuning voor extensies van derden binding.
* Ontkoppeling van de runtime en -bindingen. Hiermee wordt bindinguitbreidingen worden samengesteld en vrijgegeven onafhankelijk van elkaar. U kunt bijvoorbeeld kiezen om te upgraden naar een versie van een extensie die is gebaseerd op een nieuwere versie van een onderliggende SDK.
* Een lichtere uitvoeringsomgeving, waarbij alleen de bindingen in gebruik zijn bekend en geladen door de runtime.

Alle ingebouwde Azure Functions-bindingen dit model hebben aangenomen en worden niet meer opgenomen in de standaardinstelling, met uitzondering van de Timer-trigger en de HTTP-trigger. Deze extensies worden automatisch geïnstalleerd wanneer u functies met de hulpprogramma's zoals Visual Studio of via de portal maken.

De volgende tabel geeft aan welke veldparameterbindings ondersteund in elke runtimeversie.

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

## <a name="known-issues-in-2x"></a>Bekende problemen in 2.x

Zie voor meer informatie over de ondersteuning van bindingen en andere functionele hiaten in 2.x [Runtime 2.0 bekende problemen](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Azure-Functions-runtime-2.0-known-issues).

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

* [Azure-functies lokaal programmeren en testen](functions-run-local.md)
* [Het doel-Azure Functions runtime-versies](set-runtime-version.md)
* [Releaseopmerkingen](https://github.com/Azure/azure-functions-host/releases)
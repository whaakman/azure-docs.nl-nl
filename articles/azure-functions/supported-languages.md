---
title: Ondersteunde talen in Azure Functions
description: Informatie over welke talen worden ondersteund (GA) en die zijn experimenteel of in de Preview-versie.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.devlang: dotnet
ms.topic: reference
ms.date: 08/02/2018
ms.author: glenga
ms.openlocfilehash: 15d2e40127579fbd278cc3dc18653d782a515caa
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46957309"
---
# <a name="supported-languages-in-azure-functions"></a>Ondersteunde talen in Azure Functions

In dit artikel wordt uitgelegd in dat de niveaus van ondersteuning geboden voor talen die u met Azure Functions gebruiken kunt.

## <a name="levels-of-support"></a>Ondersteuningsniveaus

Er zijn drie niveaus van ondersteuning:

* **Algemeen beschikbaar (GA)** : volledig ondersteund en goedgekeurd voor gebruik in productieomgevingen.
* **Preview-versie** - nog niet ondersteund, maar wordt verwacht dat de status van de algemene beschikbaarheid in de toekomst te bereiken.
* **Experimentele** : niet ondersteund en kan worden afgeschaft in de toekomst; bieden geen garantie voor uiteindelijke voorlopige versie of de status van de algemene beschikbaarheid.

## <a name="languages-in-runtime-1x-and-2x"></a>Talen in runtime 1.x en 2.x

[Twee versies van de Azure Functions-runtime](functions-versions.md) beschikbaar zijn. De volgende tabel ziet u welke talen worden ondersteund in elke runtimeversie.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

### <a name="experimental-languages"></a>Experimentele talen

De experimentele talen in versie 1.x niet goed worden geschaald en bieden geen ondersteuning voor alle bindingen. Bijvoorbeeld, 1.x Python is traag, omdat de Functions-runtime wordt uitgevoerd *python.exe* bij elke functieaanroep. En hoewel Python HTTP-bindingen ondersteunt, heeft geen toegang tot het request-object.

Experimentele ondersteuning voor PowerShell in 1.x is beperkt tot versie 5.1, omdat dat is wat wordt standaard geïnstalleerd op de virtuele machines op welke functie-apps worden uitgevoerd. Als u uitvoeren van PowerShell-scripts wilt, kunt u overwegen [Azure Automation](https://azure.microsoft.com/services/automation/).

Gebruik geen experimentele talen voor alles wat u afhankelijk zijn, omdat er geen officiële ondersteuning voor hen is. Kwesties moeten niet worden geopend voor problemen met experimentele talen. 

De runtime versie 2.x biedt geen ondersteuning voor experimentele talen. Ondersteuning voor nieuwe talen wordt toegevoegd wanneer de taal kan worden ondersteund in de productieomgeving. 

### <a name="language-extensibility"></a>Taal-uitbreidbaarheid

De runtime 2.x is ontworpen om u te bieden [taal uitbreidbaarheid](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Language-Extensibility). De talen van de JavaScript en Java in de runtime 2.x zijn gebouwd met deze uitbreidbaarheid.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over hoe u een van de algemene beschikbaarheid of preview talen in Azure Functions, raadpleegt u de volgende bronnen:

> [!div class="nextstepaction"]
> [C#](functions-reference-csharp.md)

> [!div class="nextstepaction"]
> [F#](functions-reference-fsharp.md)

> [!div class="nextstepaction"]
> [JavaScript](functions-reference-node.md)

> [!div class="nextstepaction"]
> [Java](functions-reference-java.md)

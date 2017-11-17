---
title: Ondersteunde talen in Azure Functions
description: Informatie over welke talen worden ondersteund (GA) en die zijn experimentele of in preview.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: 
tags: 
ms.service: functions
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/07/2017
ms.author: tdykstra
ms.openlocfilehash: 5786a206b258cfe7c48f52ead9b5a4cceb64cd5f
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="supported-languages-in-azure-functions"></a>Ondersteunde talen in Azure Functions

Dit artikel wordt uitgelegd op dat de niveaus van ondersteuning geboden voor talen die u met Azure Functions gebruiken kunt.

## <a name="levels-of-support"></a>Niveaus van ondersteuning

Er zijn drie niveaus van ondersteuning:

* **Algemeen beschikbaar (GA)** - volledig ondersteund en goedgekeurd voor gebruik in productieomgevingen.
* **Preview** - nog niet ondersteund, maar wordt verwacht in de toekomst GA status bereikt.
* **Experimentele** : niet ondersteund en kan worden opgegeven in de toekomst; geen garantie van uiteindelijke preview of GA status.

## <a name="languages-in-runtime-1x-and-2x"></a>Talen in runtime 1.x en 2.x

[Twee versies van de Azure Functions-runtime](functions-versions.md) beschikbaar zijn. De 1.x-runtime is de algemene beschikbaarheid. Het is de enige runtime is goedgekeurd voor productietoepassingen. De 2.x-runtime is momenteel in preview, zodat de talen die wordt ondersteund in preview. De volgende tabel ziet welke talen worden ondersteund in elke runtimeversie.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

### <a name="experimental-languages"></a>Experimentele talen

De experimentele talen in 1.x niet goed worden geschaald en bieden geen ondersteuning voor alle bindingen. Bijvoorbeeld, Python traag is omdat de runtime van Functions wordt uitgevoerd *python.exe* bij elke functieaanroep. En terwijl Python HTTP-bindingen ondersteunt, heeft geen toegang tot het request-object.

Experimentele ondersteuning voor PowerShell is beperkt tot versie 4.0 omdat wat wordt geïnstalleerd op de virtuele machines die functie apps worden uitgevoerd. Als u uitvoeren van PowerShell-scripts wilt, kunt u overwegen [Azure Automation](https://azure.microsoft.com/services/automation/).

De 2.x runtime biedt geen ondersteuning voor experimentele talen. In 2.x, wordt er ondersteuning voor een taal alleen als het goed schalen en ondersteunt geavanceerde triggers toevoegen.

Als u wilt een van de talen die alleen beschikbaar in 1.x zijn te gebruiken, blijven op de 1.x-runtime. Maar niet gebruikmaken van experimentele talen voor alles wat u afhankelijk zijn, aangezien er geen officiële ondersteuning voor deze is. U kunt aanvragen help door [GitHub problemen maken](https://github.com/Azure/azure-webjobs-sdk-script/issues), maar ondersteuningsaanvragen mogen niet worden geopend voor problemen met experimentele talen. 

### <a name="language-extensibility"></a>Taal uitbreidbaarheid

De runtime 2.x is ontworpen om u te bieden [taal uitbreidbaarheid](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Language-Extensibility). Een van de eerste talen worden gebaseerd op deze uitbreidbaarheid is model Java, die zich in het voorbeeld in 2.x.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van een van de talen GA of preview in Azure Functions, de volgende bronnen:

> [!div class="nextstepaction"]
> [C#](functions-reference-csharp.md)

> [!div class="nextstepaction"]
> [F#](functions-reference-fsharp.md)

> [!div class="nextstepaction"]
> [JavaScript](functions-reference-node.md)

> [!div class="nextstepaction"]
> [Java](functions-reference-java.md)
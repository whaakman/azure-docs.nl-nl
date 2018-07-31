---
title: Ondersteunde talen in Azure Functions
description: Informatie over welke talen worden ondersteund (GA) en die zijn experimenteel of in de Preview-versie.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
ms.service: functions
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/07/2017
ms.author: glenga
ms.openlocfilehash: 00f291e903948bf43bc997816b6072186cf1f889
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39343080"
---
# <a name="supported-languages-in-azure-functions"></a>Ondersteunde talen in Azure Functions

In dit artikel wordt uitgelegd in dat de niveaus van ondersteuning geboden voor talen die u met Azure Functions gebruiken kunt.

## <a name="levels-of-support"></a>Ondersteuningsniveaus

Er zijn drie niveaus van ondersteuning:

* **Algemeen beschikbaar (GA)** : volledig ondersteund en goedgekeurd voor gebruik in productieomgevingen.
* **Preview-versie** - nog niet ondersteund, maar wordt verwacht dat de status van de algemene beschikbaarheid in de toekomst te bereiken.
* **Experimentele** : niet ondersteund en kan worden afgeschaft in de toekomst; bieden geen garantie voor uiteindelijke voorlopige versie of de status van de algemene beschikbaarheid.

## <a name="languages-in-runtime-1x-and-2x"></a>Talen in runtime 1.x en 2.x

[Twee versies van de Azure Functions-runtime](functions-versions.md) beschikbaar zijn. De runtime 1.x is algemene beschikbaarheid. Het is de enige runtime die is goedgekeurd voor productie-Apps. De runtime 2.x is momenteel in preview, zodat de talen die ondersteund in preview zijn. De volgende tabel ziet u welke talen worden ondersteund in elke runtimeversie.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

### <a name="experimental-languages"></a>Experimentele talen

De experimentele talen in 1.x niet goed schalen en bieden geen ondersteuning voor alle bindingen. Bijvoorbeeld: Python is traag, omdat de Functions-runtime wordt uitgevoerd *python.exe* bij elke functieaanroep. En hoewel Python HTTP-bindingen ondersteunt, heeft geen toegang tot het request-object.

Experimentele ondersteuning voor PowerShell is beperkt tot versie 4.0 omdat die wordt geïnstalleerd op de virtuele machines die op functie-apps worden uitgevoerd. Als u uitvoeren van PowerShell-scripts wilt, kunt u overwegen [Azure Automation](https://azure.microsoft.com/services/automation/).

De runtime 2.x biedt geen ondersteuning voor experimentele talen. In 2.x gebruikt, zullen we ondersteuning voor een taal alleen als het goed schaalt en ondersteunt geavanceerde triggers toevoegen.

Als u wilt een van de talen die alleen beschikbaar in 1.x zijn te gebruiken, blijven maken van de runtime 1.x. Maar experimentele talen niet worden gebruikt voor alles wat u afhankelijk zijn, omdat er geen officiële ondersteuning voor hen is. U kunt aanvragen kunnen hierbij helpen door [GitHub-problemen maken](https://github.com/Azure/azure-webjobs-sdk-script/issues), maar kwesties mogen niet worden geopend voor problemen met experimentele talen. 

### <a name="language-extensibility"></a>Taal-uitbreidbaarheid

De runtime 2.x is ontworpen om u te bieden [taal uitbreidbaarheid](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Language-Extensibility). Model is tussen de eerste talen gebaseerd zijn op deze uitbreidbaarheid Java, die zich in de Preview-versie in 2.x.

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
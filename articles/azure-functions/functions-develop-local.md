---
title: Ontwikkelen en lokaal uitvoeren van Azure functions | Microsoft Docs
description: Ontdek hoe u de code en testen van Azure functions op uw lokale computer voordat u deze op Azure Functions uitvoeren.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 06/27/2018
ms.author: glenga
ms.openlocfilehash: 89f94be4cf624914183480362ae23c62c363622f
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37088620"
---
# <a name="code-and-test-azure-functions-locally"></a>Code en Azure Functions lokaal testen

Terwijl u kunt ontwikkelen en testen van Azure Functions in de [Azure Portal], veel ontwikkelaars liever een lokale ontwikkeling biedt. Functies kunt eenvoudig uw favoriete code-editor en de ontwikkeling van hulpprogramma's gebruiken voor het maken en testen van functies op uw lokale computer. Uw lokale functies kunnen verbinding maken met live Azure-services en u kunt deze fouten op uw lokale computer met behulp van de volledige functies runtime.

## <a name="local-development-environments"></a>Lokale ontwikkelomgevingen

De manier waarop u functies op uw lokale computer ontwikkelt is afhankelijk van uw [taal](supported-languages.md) en voorkeuren tooling. De omgevingen in de volgende tabel bieden ondersteuning voor lokale ontwikkeling:

|Omgeving                              |Talen         |Beschrijving|
|-----------------------------------------|------------|---|
| [Opdrachtprompt of terminal](functions-run-local.md) | [C# script (.csx)](functions-reference-csharp.md), [JavaScript](functions-reference-node.md) | [Azure Functions kernonderdelen] biedt de core runtime en sjablonen voor het maken van functies, waardoor lokale ontwikkeling. Versie 2.x ondersteuning ontwikkeling op Linux-, Mac OS- en Windows. Alle omgevingen zijn afhankelijk van de Core-hulpprogramma's voor de lokale runtime van Functions.|
|[Visual Studio Code](https://code.visualstudio.com/tutorials/functions-extension/getting-started)| [C# script (.csx)](functions-reference-csharp.md), [JavaScript](functions-reference-node.md) | De [Azure Functions-extensie voor VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) voegt ondersteuning voor functies in VS-Code. Vereist de Core-hulpprogramma's. Ondersteunt de ontwikkeling op Linux-, Mac OS- en Windows, wanneer u versie 2.x van de Core-hulpprogramma's. Zie voor meer informatie, [implementeren naar Azure met Azure Functions](https://code.visualstudio.com/tutorials/functions-extension/getting-started).  |
| [Visual Studio 2017](functions-develop-vs.md) | [C# (class-bibliotheek)](functions-dotnet-class-library.md) | De Azure Functions-hulpprogramma's zijn opgenomen in de **ontwikkelen van Azure** werkbelasting van [Visual Studio 2017 versie 15,5](https://www.visualstudio.com/vs/) en latere versies. U kunt compileren van functies in een class-bibliotheek en het DLL-bestand publiceren naar Azure. Bevat de Core-hulpprogramma's voor het testen van lokale. Zie voor meer informatie, [Azure Functions ontwikkelen met Visual Studio](functions-develop-vs.md). |
| [Maven](functions-create-first-java-maven.md) | [Java](functions-reference-java.md) | Kan worden geïntegreerd met Core hulpmiddelen voor het ontwikkelen van Java-functies inschakelen. Versie 2.x ondersteunt ontwikkeling op Linux-, Mac OS- en Windows. Zie voor meer informatie, [maken van uw eerste functie met Java en Maven](functions-create-first-java-maven.md).|

[!INCLUDE [Don't mix development environments](../../includes/functions-mixed-dev-environments.md)]

Elk van deze lokale ontwikkelomgevingen kunt u de functie app-projecten maken en vooraf gedefinieerde functies sjablonen gebruiken om nieuwe functies te maken. Gebruik de Core-hulpprogramma's zodat u kunt testen en foutopsporing van uw functies tegen de echte runtime van Functions op uw eigen computer net als elke andere app. U kunt ook publiceert u de functie app-project uit een van deze omgevingen naar Azure.  

## <a name="next-steps"></a>Volgende stappen

+ Zie voor meer informatie over het ontwikkelen van gecompileerde C#-functies met behulp van Visual Studio 2017 van lokale, [Azure Functions ontwikkelen met Visual Studio](functions-develop-vs.md).
+ Zie voor meer informatie over lokale ontwikkeling van functies met VS-Code op een Mac, Linux of Windows-computer, de [tegenover Code-documentatie voor Azure Functions](https://code.visualstudio.com/tutorials/functions-extension/getting-started).
+ Zie voor meer informatie over het ontwikkelen van functies van de opdrachtprompt of terminal, [werken met hulpprogramma's voor Azure Functions Core](functions-run-local.md).

<!-- LINKS -->

[Azure Functions kernonderdelen]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure Portal]: https://portal.azure.com 
[Node.js]: https://docs.npmjs.com/getting-started/installing-node#osx-or-windows

---
title: Ontwikkelen en uitvoeren van Azure functions lokaal | Microsoft Docs
description: Informatie over het programmeren en testen van Azure functions op uw lokale computer voordat u ze op Azure Functions uitvoeren.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 0f1d4fdbb9e5d2b05b8ec43c1393951a4c6a87b8
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44094756"
---
# <a name="code-and-test-azure-functions-locally"></a>Code en Azure Functions lokaal testen

Terwijl u bent kunnen ontwikkelen en testen van Azure Functions in de [Azure Portal], veel ontwikkelaars voorkeur geeft aan een lokale ontwikkeling-ervaring. Functies kunt eenvoudig uw favoriete code-editor en ontwikkeling-hulpprogramma's gebruiken om te maken en testen van functies op uw lokale computer. Uw lokale functies kunnen verbinding maken met live Azure-services, en u deze op uw lokale computer met behulp van de volledige Functions-runtime kunt fouten opsporen.

## <a name="local-development-environments"></a>Lokale ontwikkelomgevingen

De manier waarop u functies op uw lokale computer ontwikkelen is afhankelijk van uw [taal](supported-languages.md) en hulpprogramma's van voorkeuren. De omgevingen in de volgende tabel bieden ondersteuning voor lokale ontwikkeling:

|Omgeving                              |Talen         |Beschrijving|
|-----------------------------------------|------------|---|
| [Opdrachtprompt of terminal](functions-run-local.md) | [C# (klassebibliotheek)](functions-dotnet-class-library.md), [C#-script (.csx)](functions-reference-csharp.md), [JavaScript](functions-reference-node.md) | [Azure Functions Core Tools] bevat van de core runtime en sjablonen voor het maken van functies, waarmee lokale ontwikkeling. Versie 2.x ondersteunt ontwikkeling op Linux, MacOS en Windows. Alle omgevingen zijn afhankelijk van de Core-hulpprogramma's voor de lokale Functions-runtime. |
|[Visual Studio Code](functions-create-first-function-vs-code.md)| [C# (klassebibliotheek)](functions-dotnet-class-library.md), [C#-script (.csx)](functions-reference-csharp.md), [JavaScript](functions-reference-node.md) | De [Azure Functions-extensie voor VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) voegt functies aan Visual Studio Code ondersteunen. De essentiële hulpprogramma vereist. Ondersteunt de ontwikkeling van Linux, MacOS en Windows, wanneer u versie 2.x van de Core-hulpprogramma's. Zie voor meer informatie, [uw eerste functie maken met Visual Studio Code](functions-create-first-function-vs-code.md). |
| [Visual Studio 2017](functions-develop-vs.md) | [C# (klassebibliotheek)](functions-dotnet-class-library.md) | De Azure Functions-hulpprogramma's zijn opgenomen in de **Azure-ontwikkeling** werkbelasting van [Visual Studio 2017 versie 15.5](https://www.visualstudio.com/vs/) en latere versies. Hiermee kunt u functies in een klassebibliotheek gecompileerd en het DLL-bestand publiceert naar Azure. Bevat de Core-hulpprogramma's voor het lokale testen. Zie voor meer informatie, [Azure Functions ontwikkelen met Visual Studio](functions-develop-vs.md). |
| [Maven](functions-create-first-java-maven.md) (verschillende) | [Java](functions-reference-java.md) | Kan worden geïntegreerd met Core-hulpprogramma's waarmee de ontwikkeling van Java-functies. Versie 2.x ondersteunt ontwikkeling op Linux, MacOS en Windows. Zie voor meer informatie, [uw eerste functie maken met Java en Maven](functions-create-first-java-maven.md). Biedt ook ondersteuning voor ontwikkeling met [Eclipse](functions-create-maven-eclipse.md) en [IntelliJ IDEA](functions-create-maven-intellij.md) |

[!INCLUDE [Don't mix development environments](../../includes/functions-mixed-dev-environments.md)]

Elk van deze omgevingen lokale ontwikkeling kunt u functie-app-projecten maken en vooraf gedefinieerde Functions-sjablonen gebruiken om nieuwe functies te maken. Gebruik de essentiële hulpprogramma zodat u kunt testen en foutopsporing van uw functies op basis van de echte Functions-runtime op uw computer, net als elke andere app. U kunt ook publiceren u functie-app-project in elk van deze omgevingen naar Azure.  

## <a name="next-steps"></a>Volgende stappen

+ Zie voor meer informatie over lokale ontwikkeling van gecompileerde C#-functies met behulp van Visual Studio 2017, [Azure Functions ontwikkelen met Visual Studio](functions-develop-vs.md).
+ Zie voor meer informatie over lokaal ontwikkelen van functies met VS Code op een Mac, Linux of Windows-computer, de [VS Code-documentatie voor Azure Functions](https://code.visualstudio.com/tutorials/functions-extension/getting-started).
+ Zie voor meer informatie over het ontwikkelen van functies van de opdrachtprompt of terminal, [werken met Azure Functions Core Tools](functions-run-local.md).

<!-- LINKS -->

[Azure Functions Core Tools]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure Portal]: https://portal.azure.com 
[Node.js]: https://docs.npmjs.com/getting-started/installing-node#osx-or-windows

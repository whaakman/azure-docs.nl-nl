---
title: Azure Functions-binding extensies registreren
description: Informatie over het registreren van een Azure Functions-bindingsuitbreiding op basis van uw omgeving.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 02/18/2019
ms.author: cshoe
ms.openlocfilehash: 5534086d5754691f650370e465fa2c63210e0dc7
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56739520"
---
# <a name="register-azure-functions-binding-extensions"></a>Azure Functions-binding extensies registreren

Azure Functions ondersteunt HTTP- en timer buiten het vak. Als u wilt werken met andere services, die u wilt installeren of *registreren* een [binding](./functions-triggers-bindings.md) extensie. Binding-extensies worden geleverd via Azure Core Tools of NuGet-pakketten. 

De volgende tabel geeft aan wanneer en hoe u de bindingen registreren.

| Ontwikkelomgeving |Registratie<br/> in de functies 1.x  |Registratie<br/> in de functies 2.x  |
|-------------------------|------------------------------------|------------------------------------|
|Azure Portal|Automatisch|[Automatisch met prompt](#azure-portal-development)|
|Niet-.NET-talen of lokale Azure Core Tools-ontwikkeling|Automatisch|[Core-hulpprogramma's-CLI-opdrachten gebruiken](#local-development-azure-functions-core-tools)|
|C#-klassenbibliotheek vormt met behulp van Visual Studio 2017|[NuGet-hulpprogramma's gebruiken](#c-class-library-with-visual-studio-2017)|[NuGet-hulpprogramma's gebruiken](#c-class-library-with-visual-studio-2017)|
|C#-klassenbibliotheek vormt met behulp van Visual Studio Code|N/A|[.NET Core CLI gebruiken](#c-class-library-with-visual-studio-code)|

De volgende bindingstypen zijn uitzonderingen waarvoor geen expliciete registratie is vereist omdat ze automatisch worden geregistreerd in alle versies en omgevingen: HTTP- en timer.

> [!IMPORTANT]
> Deze inhoud functies voor de rest van dit artikel is alleen bedoeld voor 2.x. Bindinguitbreidingen expliciet worden niet geregistreerd in de functies 1.x, behalve wanneer [het maken van een C# met behulp van Visual Studio 2017-klassenbibliotheek vormt](#local-csharp).

## <a name="azure-portal-development"></a>Azure portal-ontwikkeling

Wanneer u een functie maken of een binding toevoegt, wordt u gevraagd wanneer de uitbreiding voor de trigger of binding moet worden geregistreerd. Door te klikken op de prompt beantwoorden **installeren** voor het registreren van de extensie. Installatie kan tot tien minuten duren in een verbruiksabonnement.

U moet elke uitbreiding slechts één keer voor een bepaalde functie-app installeren. Voor ondersteunde bindingen die niet beschikbaar in de portal of bij te werken de geïnstalleerde extensie, u kunt ook [handmatig installeren of bijwerken van Azure Functions binding extensies vanuit de portal](install-update-binding-extensions-manual.md).  

## <a name="local-development-azure-functions-core-tools"></a>Azure Functions Core Tools voor lokale ontwikkeling

[!INCLUDE [functions-core-tools-install-extension](../../includes/functions-core-tools-install-extension.md)]

<a name="local-csharp"></a>
## <a name="c-class-library-with-visual-studio-2017"></a>C#-klassenbibliotheek vormt met Visual Studio 2017

In **Visual Studio 2017**, u kunt pakketten installeren vanuit de Package Manager-Console met behulp van de [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package) opdracht, zoals wordt weergegeven in het volgende voorbeeld:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions.ServiceBus -Version <target_version>
```

De naam van het pakket moet worden gebruikt voor een bepaalde binding is opgegeven in het referentieartikel voor die binding. Zie voor een voorbeeld: de [pakketten gedeelte van het Service Bus-binding verwijzing artikel](functions-bindings-service-bus.md#packages---functions-1x).

Vervang `<target_version>` in het voorbeeld met een specifieke versie van het pakket, zoals `3.0.0-beta5`. Geldige versies worden weergegeven op de afzonderlijke pakket's op de [NuGet.org](https://nuget.org). De primaire versies die met Functions-runtime overeenkomen 1.x of 2.x zijn opgegeven in het artikel verwijzing voor de binding.

## <a name="c-class-library-with-visual-studio-code"></a>C#-klassenbibliotheek vormt met Visual Studio Code

In **Visual Studio Code**, u kunt pakketten installeren vanuit de opdrachtprompt met de [dotnet-pakket toevoegen](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) opdracht in de .NET Core CLI, zoals wordt weergegeven in het volgende voorbeeld:

```terminal
dotnet add package Microsoft.Azure.WebJobs.Extensions.ServiceBus --version <target_version>
```

De .NET Core-CLI kan alleen worden gebruikt voor het ontwikkelen van Azure Functions 2.x.

De naam van het pakket moet worden gebruikt voor een bepaalde binding is opgegeven in het referentieartikel voor die binding. Zie voor een voorbeeld: de [pakketten gedeelte van het Service Bus-binding verwijzing artikel](functions-bindings-service-bus.md#packages---functions-1x).

Vervang `<target_version>` in het voorbeeld met een specifieke versie van het pakket, zoals `3.0.0-beta5`. Geldige versies worden weergegeven op de afzonderlijke pakket's op de [NuGet.org](https://nuget.org). De primaire versies die met Functions-runtime overeenkomen 1.x of 2.x zijn opgegeven in het artikel verwijzing voor de binding.

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Azure functie-trigger en binding voorbeeld](./functions-bindings-example.md)


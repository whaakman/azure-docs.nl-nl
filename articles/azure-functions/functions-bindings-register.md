---
title: Azure Functions-binding extensies registreren
description: Informatie over het registreren van een Azure Functions-bindingsuitbreiding op basis van uw omgeving.
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 02/25/2019
ms.author: cshoe
ms.openlocfilehash: 88ffd6ec24ed19dd3b1e57277884c8759cdac1f9
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67480340"
---
# <a name="register-azure-functions-binding-extensions"></a>Azure Functions-binding extensies registreren

In Azure Functions versie 2.x [bindingen](./functions-triggers-bindings.md) zijn beschikbaar als afzonderlijke pakketten in de functions-runtime. Terwijl de .NET-functies via NuGet-pakketten toegang krijgen tot bindingen, extensie bundels andere functies toegang geven tot alle bindingen via een configuratie-instelling.

Houd rekening met de volgende items die betrekking hebben op het binden van extensies:

- Bindinguitbreidingen expliciet worden niet geregistreerd in de functies 1.x, behalve wanneer [het maken van een C# met Visual Studio-klassenbibliotheek vormt](#local-csharp).

- HTTP- en timer triggers standaard worden ondersteund en vereisen een uitbreiding.

De volgende tabel geeft aan wanneer en hoe u de bindingen registreren.

| Ontwikkelomgeving |Registratie<br/> in de functies 1.x  |Registratie<br/> in de functies 2.x  |
|-------------------------|------------------------------------|------------------------------------|
|Azure Portal|Automatisch|Automatisch|
|Niet-.NET-talen of lokale Azure Core Tools-ontwikkeling|Automatisch|[Gebruik Azure Functions Core Tools en extensie bundels](#extension-bundles)|
|C#met behulp van Visual Studio 2019 klassebibliotheek|[NuGet-hulpprogramma's gebruiken](#c-class-library-with-visual-studio-2019)|[NuGet-hulpprogramma's gebruiken](#c-class-library-with-visual-studio-2019)|
|C#-klassenbibliotheek vormt met behulp van Visual Studio Code|N/A|[.NET Core CLI gebruiken](#c-class-library-with-visual-studio-code)|

## <a name="extension-bundles"></a>Extensie-bundels voor lokale ontwikkeling

Bundels van extensie is een technologie voor lokale ontwikkeling voor de runtime versie 2.x, waarmee u een compatibele set functies extensies binden aan uw functie-app-project toevoegen. Deze extensiepakketten worden vervolgens opgenomen in het implementatiepakket wanneer u in Azure implementeert. Bundels maakt alle bindingen die zijn gepubliceerd door Microsoft beschikbaar via een instelling in de *host.json* bestand. Extensiepakketten die zijn gedefinieerd in een bundel zijn compatibel met elkaar worden verbonden, waarmee u het voorkomen van conflicten tussen pakketten. Wanneer het ontwikkelen van lokaal, zorg ervoor dat u de nieuwste versie van [Azure Functions Core Tools](functions-run-local.md#v2).

Extensie-bundels voor alle lokale ontwikkeling met behulp van Azure Functions Core Tools of Visual Studio Code gebruiken.

Als u geen extensie bundels gebruikt, moet u de .NET Core installeren 2.x SDK op uw lokale computer voordat u een binding-extensies installeert. Bundels Hiermee verwijdert u deze vereiste voor lokale ontwikkeling. 

Als u wilt gebruiken extensie bundels, de *host.json* bestand om op te nemen van de volgende vermelding voor `extensionBundle`:

```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```

De volgende eigenschappen zijn beschikbaar in `extensionBundle`:

| Eigenschap | Description |
| -------- | ----------- |
| **`id`** | De naamruimte voor Microsoft Azure Functions-extensie bundels. |
| **`version`** | De versie van de bundel installeren. De Functions-runtime kiest altijd de maximale toegestane versie gedefinieerd door het bereik van de versie of de interval. De waarde voor de bovenstaande versie kunt alle versies van de bundel van 1.0.0 tot, maar niet inclusief 2.0.0. Zie voor meer informatie de [interval notatie voor het opgeven van versiebereiken](https://docs.microsoft.com/nuget/reference/package-versioning#version-ranges-and-wildcards). |

Verhoging van de bundel versies als pakketten in de bundel wijzigen. Primaire versiewijzigingen optreden wanneer pakketten in de bundel verhogen door een primaire versie, die meestal met een wijziging in de belangrijkste versie van de Functions-runtime overeenkomt.  

De huidige set extensies geïnstalleerd door de standaard-bundel worden opgesomd in dit [extensions.json bestand](https://github.com/Azure/azure-functions-extension-bundles/blob/master/src/Microsoft.Azure.Functions.ExtensionBundle/extensions.json).

<a name="local-csharp"></a>

## <a name="c-class-library-with-visual-studio-2019"></a>C\# -klassenbibliotheek vormt met Visual Studio 2019

In **Visual Studio 2019**, u kunt pakketten installeren vanuit de Package Manager-Console met behulp van de [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package) opdracht, zoals wordt weergegeven in het volgende voorbeeld:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions.ServiceBus -Version <TARGET_VERSION>
```

De naam van het pakket wordt gebruikt voor een bepaalde binding is opgegeven in het referentieartikel voor die binding. Zie voor een voorbeeld: de [pakketten gedeelte van het Service Bus-binding verwijzing artikel](functions-bindings-service-bus.md#packages---functions-1x).

Vervang `<TARGET_VERSION>` in het voorbeeld met een specifieke versie van het pakket, zoals `3.0.0-beta5`. Geldige versies worden weergegeven op de afzonderlijke pakket's op de [NuGet.org](https://nuget.org). De primaire versies die met Functions-runtime overeenkomen 1.x of 2.x zijn opgegeven in het artikel verwijzing voor de binding.

## <a name="c-class-library-with-visual-studio-code"></a>C#-klassenbibliotheek vormt met Visual Studio Code

> [!NOTE]
> Wordt u aangeraden [extensie bundels](#extension-bundles) dat functies een compatibele set binding-extensiepakketten automatisch worden geïnstalleerd.

In **Visual Studio Code**, pakketten installeren voor een C# -klassebibliotheekproject vanuit de opdrachtprompt met de [dotnet-pakket toevoegen](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) opdracht in de .NET Core CLI, zoals wordt weergegeven in het volgende voorbeeld:

```terminal
dotnet add package Microsoft.Azure.WebJobs.Extensions.ServiceBus --version <TARGET_VERSION>
```

De .NET Core-CLI kan alleen worden gebruikt voor het ontwikkelen van Azure Functions 2.x.

De naam van het pakket moet worden gebruikt voor een bepaalde binding is opgegeven in het referentieartikel voor die binding. Zie voor een voorbeeld: de [pakketten gedeelte van het Service Bus-binding verwijzing artikel](functions-bindings-service-bus.md#packages---functions-1x).

Vervang `<TARGET_VERSION>` in het voorbeeld met een specifieke versie van het pakket, zoals `3.0.0-beta5`. Geldige versies worden weergegeven op de afzonderlijke pakket's op de [NuGet.org](https://nuget.org). De primaire versies die met Functions-runtime overeenkomen 1.x of 2.x zijn opgegeven in het artikel verwijzing voor de binding.

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Azure functie-trigger en binding voorbeeld](./functions-bindings-example.md)


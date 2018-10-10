---
title: Overzicht van Azure Functions runtime-versies
description: Azure Functions ondersteunt meerdere versies van de runtime. Meer informatie over de verschillen tussen deze en kiezen die geschikt is voor u.
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 10/03/2018
ms.author: glenga
ms.openlocfilehash: f2f1313461fcb58ea48af99aeda2f7005534fe34
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48885184"
---
# <a name="azure-functions-runtime-versions-overview"></a>Overzicht van Azure Functions runtime-versies

 Er zijn twee primaire versies van de Azure Functions-runtime: 1.x en 2.x. De huidige versie, waarbij nieuwe functie werk en verbeteringen worden aangebracht is 2.x gebruikt, hoewel beide worden ondersteund voor productiescenario's.  De volgende details van de verschillen tussen de twee, hoe u elke versie maken en een upgrade van 1.x naar 2.x.

> [!NOTE]
> Dit artikel wordt verwezen naar de cloudservice Azure Functions. Zie voor informatie over de preview-product waarmee u Azure Functions on-premises uitvoert, de [overzicht van Azure Functions Runtime](functions-runtime-overview.md).

## <a name="cross-platform-development"></a>Platformoverschrijdende ontwikkeling

De runtime versie 2.x wordt uitgevoerd op .NET Core 2, waarmee deze wordt uitgevoerd op alle platformen worden ondersteund door .NET Core, met inbegrip van macOS en Linux. Wordt uitgevoerd op .NET Core, kunt u platformoverschrijdende ontwikkeling en hostingscenario's.

Ter vergelijking, ondersteunt de runtime versie 1.x alleen ontwikkelen en hosten in Azure portal of op Windows-computers.

## <a name="languages"></a>Talen

De runtime versie 2.x maakt gebruik van een nieuwe taalmodel voor uitbreidbaarheid. In versie 2.x, alle functies in een functie-app moeten delen dezelfde taal. De taal van de functies in een functie-app wordt gekozen bij het maken van de app.

Azure Functions 1.x experimentele talen wordt niet worden bijgewerkt voor het gebruik van het nieuwe model, zodat ze worden niet ondersteund in 2.x. De volgende tabel geeft aan welke programmeertalen worden momenteel ondersteund in elke runtimeversie.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Zie voor meer informatie, [ondersteunde talen](supported-languages.md).

## <a name="creating-1x-apps"></a>Uitvoeren op versie 1.x

Functie-apps die zijn gemaakt in Azure portal zijn standaard ingesteld op versie 2.x. Indien mogelijk moet u deze runtime-versie, waarbij de nieuwe functie investeringen worden aangebracht. Als u wilt, kunt u nog steeds een functie-app uitvoeren in de versie 1.x-runtime. Nadat u uw functie-app hebt gemaakt, maar voordat u alle functies kunt toevoegen, kunt u alleen de runtimeversie wijzigen. Zie voor informatie over het vastmaken van de runtimeversie 1.x, [weergeven en bijwerken van de huidige runtimeversie](set-runtime-version.md#view-and-update-the-current-runtime-version).

## <a name="migrating-from-1x-to-2x"></a>Migreren van 1.x naar 2.x

U kunt voor het migreren van een bestaande app gebruikmaken van de versie 1.x-runtime voor gebruik in plaats daarvan versie 2.x. De meeste van de wijzigingen die u moet zijn met betrekking tot wijzigingen in de taal, zoals C# API-wijzigingen tussen .NET Framework 4.7 en .NET Core 2. U moet ook voor zorgen dat uw code en bibliotheken zijn compatibel met de taal die u kiest. Tot slot moet u Let op eventuele wijzigingen in de trigger, bindingen en functies die hieronder beschreven. Voor de beste resultaten van de migratie, moet u een nieuwe functie-app voor versie 2.x en poort uw bestaande versie 1.x-functie code in de nieuwe app.  

### <a name="changes-in-triggers-and-bindings"></a>Wijzigingen in de triggers en bindingen

Versie 2.x, moet u voor het installeren van de extensies voor specifieke triggers en bindingen die worden gebruikt door de functies in uw app. De enige uitzondering voor deze HTTP- en timer triggers, die geen extensie is vereist.  Zie voor meer informatie, [registreren en installeer binding extensies](./functions-triggers-bindings.md#register-binding-extensions).

Er zijn ook enkele wijzigingen in de `function.json` of kenmerken van de functie tussen versies. Bijvoorbeeld, de Event Hub `path` eigenschap is nu `eventHubName`. Zie de [bestaande binding tabel](#bindings) voor koppelingen naar documentatie voor elke binding.

### <a name="changes-in-features-and-functionality"></a>Wijzigingen in de functies en functionaliteit

Een aantal functies dat ook is verwijderd, bijgewerkt of vervangen in de nieuwe versie. Deze sectie bevat de wijzigingen die u in versie ziet 2.x na versie hebben gebruikt 1.x.

In versie 2.x, de volgende wijzigingen zijn aangebracht:

* Sleutels voor het aanroepen van HTTP-eindpunten worden altijd versleuteld opgeslagen in Azure Blob-opslag. In versie 1.x, sleutels zijn opgeslagen Azure File storage worden standaard. Wanneer een app upgraden vanaf versie 1.x naar versie 2.x, bestaande geheimen die zich in de opslag van bestanden worden opnieuw ingesteld.

* De runtime versie 2.x bevat geen ingebouwde ondersteuning voor de webhook-providers. Deze wijziging is aangebracht om prestaties te verbeteren. U kunt wel HTTP-triggers gebruiken als eindpunten voor webhooks.

* Het configuratiebestand van de host (host.json) moet leeg zijn of de tekenreeks `"version": "2.0"`.

* Voor het verbeteren van bewaking, het WebJobs-dashboard in de portal, gebruikt de [ `AzureWebJobsDashboard` ](functions-app-settings.md#azurewebjobsdashboard) instelling wordt vervangen door Azure Application Insights, dat gebruikmaakt van de [ `APPINSIGHTS_INSTRUMENTATIONKEY` ](functions-app-settings.md#appinsightsinstrumentationkey) instelling. Zie voor meer informatie, [Monitor Azure Functions](functions-monitoring.md).

* Alle functies in een functie-app moeten delen dezelfde taal. Wanneer u een functie-app maakt, moet u een runtimestack voor de app. De runtimestack is opgegeven door de [ `FUNCTIONS_WORKER_RUNTIME` ](functions-app-settings.md#functionsworkerruntime) waarde in de toepassingsinstellingen. Deze vereiste is toegevoegd aan de footprint en opstarten verbeteren. Wanneer u lokaal ontwikkelt, moet u ook deze instelling in de [bestand local.settings.json](functions-run-local.md#local-settings-file).

* De standaardtime-out voor functies in een App Service-plan is gewijzigd in 30 minuten. U kunt handmatig de time-out op onbeperkt wijzigen met behulp van de [functionTimeout](functions-host-json.md#functiontimeout) instellen in host.json.

* HTTP-gelijktijdigheid vertragingen zijn standaard voor verbruik plan functies, met een standaardwaarde van 100 gelijktijdige aanvragen per exemplaar geïmplementeerd. U kunt dit in wijzigen de [ `maxConcurrentRequests` ](functions-host-json.md#http) instellen in het bestand host.json.

* Vanwege [.NET core beperkingen](https://github.com/Azure/azure-functions-host/issues/3414), ondersteuning voor functies van F #-script (.fsx) is verwijderd. Gecompileerde F #-functies (.fs) worden nog steeds ondersteund.

* De URL-indeling van Event Grid trigger webhooks is gewijzigd in `https://{app}/runtime/webhooks/{triggerName}`.

### <a name="migrating-a-locally-developed-application"></a>Migreren van een lokaal ontwikkelde toepassing

U hebt bestaande projecten voor apps die zijn ontwikkeld lokaal via de runtime versie 1.x werken. Bijwerken naar versie 2.x gebruikt, moet u een lokale functie-app-project met versie 2.x en poort uw bestaande code in de nieuwe app. U kunt handmatig het bestaande project en code, een soort 'in-place' upgrade bijwerken. Er zijn echter een aantal andere verbeteringen tussen versie 1.x en versie 2.x die u mogelijk nog steeds wilt aanbrengen. Bijvoorbeeld, het opsporen van fouten object in C# is gewijzigd van `TraceWriter` naar `ILogger`. Als u een nieuwe versie 2.x-project maakt, beginnen u met bijgewerkte functies op basis van de meest recente versie 2.x-sjablonen.

#### <a name="visual-studio-runtime-versions"></a>Versies van Visual Studio-runtime

In Visual Studio selecteert u de runtimeversie wanneer u een project maakt. Azure Functions-hulpprogramma's voor Visual Studio biedt ondersteuning voor zowel grote runtime-versie. De juiste versie wordt gebruikt wanneer foutopsporing en publiceren op basis van de instellingen voor het project. De versie-instellingen worden gedefinieerd in de `.csproj` bestand in de volgende eigenschappen:

##### <a name="version-1x"></a>Versie 1.x

```xml
<TargetFramework>net461</TargetFramework>
<AzureFunctionsVersion>v1</AzureFunctionsVersion>
```

##### <a name="version-2x"></a>Versie 2.x

```xml
<TargetFramework>netstandard2.0</TargetFramework>
<AzureFunctionsVersion>v2</AzureFunctionsVersion>
```

Wanneer u fouten opspoort of uw project kunt publiceren, worden de juiste versie van de runtime wordt gebruikt.

#### <a name="vs-code-and-azure-functions-core-tools"></a>VS Code en Azure Functions Core Tools

[Azure Functions Core Tools](functions-run-local.md) wordt gebruikt voor het ontwikkelen van de opdrachtregel en ook door de [Azure Functions-extensie](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) voor Visual Studio Code. Om te ontwikkelen met betrekking tot versie 2.x, Installeer versie 2.x van de Core-hulpprogramma's. Versie 1.x ontwikkeling vereist versie 1.x van de essentiële hulpprogramma. Zie voor meer informatie, [Azure Functions Core Tools installeren](functions-run-local.md#install-the-azure-functions-core-tools).

Voor de ontwikkeling van Visual Studio Code, u moet mogelijk ook bijwerken van de gebruikersinstelling voor de `azureFunctions.projectRuntime` overeenkomt met de versie van de hulpprogramma's geïnstalleerd.  Deze instelling werkt ook de sjablonen en talen die worden gebruikt tijdens het maken van de functie-app.

### <a name="changing-version-of-apps-in-azure"></a>Wijzigen van de versie van apps in Azure

De versie van de runtime van de functies die worden gebruikt door de gepubliceerde apps in Azure wordt bepaald door de [ `FUNCTIONS_EXTENSION_VERSION` ](functions-app-settings.md#functionsextensionversion) toepassingsinstelling. Een waarde van `~2` is gericht op de runtime versie 2.x en `~1` is gericht op de versie 1.x-runtime. Deze instelling niet willekeurig niet wijzigen omdat er andere wijzigingen voor app-instelling en de wijzigingen in de code in uw functies waarschijnlijk vereist zijn. Zie voor meer informatie over de aanbevolen manier om uw functie-app migreren naar een andere runtimeversie, [hoe gericht op versies van Azure Functions-runtime](set-runtime-version.md).

## <a name="bindings"></a>Bindingen

De runtime versie 2.x maakt gebruik van een nieuwe [binding uitbreidingsmodel](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) die deze voordelen biedt:

* Ondersteuning voor extensies van derden binding.

* Ontkoppeling van de runtime en -bindingen. Deze wijziging kunt bindinguitbreidingen worden samengesteld en vrijgegeven onafhankelijk van elkaar. U kunt bijvoorbeeld kiezen om te upgraden naar een versie van een extensie die is gebaseerd op een nieuwere versie van een onderliggende SDK.

* Een lichtere uitvoeringsomgeving, waarbij alleen de bindingen in gebruik zijn bekend en geladen door de runtime.

Met uitzondering van de HTTP- en timer wordt geactiveerd, moeten alle bindingen expliciet toegevoegd aan de functie-app-project en geregistreerd in de portal. Zie voor meer informatie, [bindinguitbreidingen registreren](functions-triggers-bindings.md#register-binding-extensions).

De volgende tabel ziet welke bindingen worden ondersteund in elke runtimeversie.

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

* [Azure-functies lokaal programmeren en testen](functions-run-local.md)
* [Het doel-Azure Functions runtime-versies](set-runtime-version.md)
* [Releaseopmerkingen](https://github.com/Azure/azure-functions-host/releases)

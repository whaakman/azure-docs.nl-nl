---
title: Overzicht van Azure Functions runtime-versies
description: Azure Functions ondersteunt meerdere versies van de runtime. Leer over de verschillen tussen deze versies en hoe de juiste te kiezen.
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 10/03/2018
ms.author: glenga
ms.openlocfilehash: 6988fb547b07f81891efea3caad8bf34f4c8a476
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58088414"
---
# <a name="azure-functions-runtime-versions-overview"></a>Overzicht van Azure Functions runtime-versies

 Er zijn twee primaire versies van de Azure Functions-runtime: 1.x en 2.x. De huidige versie, waarbij nieuwe features en verbeteringen worden aangebracht is 2.x. Beide versies worden ondersteund voor productiescenario's.  Hieronder worden de verschillen tussen de twee beschreven, hoe u elke versie kunt toepassen, en de manier waarop u kunt upgraden van 1.x naar 2.x.

> [!NOTE]
> In dit artikel wordt verwezen naar de cloudservice Azure Functions. Zie voor informatie over het preview-product waarmee u Azure Functions on-premises uitvoert, het [overzicht van Azure Functions Runtime](functions-runtime-overview.md).

## <a name="cross-platform-development"></a>Platformoverschrijdende ontwikkeling

De runtime versie 2.x wordt uitgevoerd op .NET Core 2, waarmee deze kan worden uitgevoerd op alle platformen die worden ondersteund door .NET Core, met inbegrip van macOS en Linux. Vanwege de uitvoering op .NET Core kunt u platformoverschrijdende ontwikkeling en hostingscenario's toepassen.

Ter vergelijking, runtime versie 1.x ondersteunt alleen het ontwikkelen en hosten in de Azure portal of op Windows-computers.

## <a name="languages"></a>Talen

De runtime versie 2.x maakt gebruik van een nieuwe taalmodel voor uitbreidbaarheid. In versie 2.x moeten alle functies in een functie-app dezelfde taal delen. De taal van de functies in een functie-app wordt gekozen bij het maken van de app.

Experimentele talen van Azure Functions 1.x worden niet bijgewerkt voor het gebruik in het nieuwe model. Deze worden dus niet ondersteund in 2.x. De volgende tabel geeft aan welke programmeertalen momenteel worden ondersteund in elke runtimeversie.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Zie voor meer informatie [Ondersteunde talen](supported-languages.md).

## <a name="creating-1x-apps"></a>Uitvoeren op versie 1.x

Functie-apps die zijn gemaakt in de Azure portal zijn standaard ingesteld op versie 2.x. Indien mogelijk moet u deze runtime-versie gebruiken, omdat hier investeringen in nieuwe features worden gedaan. Als u wilt, kunt u nog steeds een functie-app uitvoeren in de versie 1.x-runtime. U kunt alleen de runtime-versie wijzigen na het maken van een functie-app wanneer u nog geen functies hebt toegevoegd. Zie voor informatie over het vastmaken van de runtimeversie 1.x, [weergeven en bijwerken van de huidige runtimeversie](set-runtime-version.md#view-and-update-the-current-runtime-version).

## <a name="migrating-from-1x-to-2x"></a>Migreren van 1.x naar 2.x

U kunt ervoor kiezen om een bestaande app die gebruikmaakt van de versie 1.x-runtime te migreren naar gebruik van versie 2.x. De meeste wijzigingen die u moet aanbrengen hebben betrekking op wijzigingen in de taal, zoals C# API-wijzigingen tussen .NET Framework 4.7 en .NET Core 2. U moet er ook voor zorgen dat uw code en bibliotheken compatibel zijn met de taal die u kiest. Tot slot moet u letten op eventuele wijzigingen in de trigger, bindingen en functies die hieronder beschreven staan. Voor de beste resultaten bij een migratie maakt u een nieuwe functie-app voor versie 2.x en poort uw bestaande versie 1.x-functie code naar de nieuwe app.  

### <a name="changes-in-triggers-and-bindings"></a>Wijzigingen in de triggers en bindingen

Versie 2.x vereist dat u de extensies voor specifieke triggers en bindingen die worden gebruikt door de functies in uw app installeert. De enige uitzondering hierop zijn HTTP- en timer-triggers, die geen extensie vereisen.  Zie voor meer informatie, [registreren en installeer binding extensies](./functions-bindings-register.md).

Er zijn tussen de versies ook enkele veranderingen in de `function.json` of kenmerken van de functie. Bijvoorbeeld, de Event Hub `path` eigenschap is nu `eventHubName`. Zie de [bestaande binding tabel](#bindings) voor koppelingen naar documentatie voor elke binding.

### <a name="changes-in-features-and-functionality"></a>Wijzigingen in de functies en functionaliteit

Een aantal functies is ook verwijderd, bijgewerkt of vervangen in de nieuwe versie. Deze sectie bevat de wijzigingen die u in versie 2.x ziet na versie 1.x te hebben gebruikt.

In versie 2.x zijn de volgende wijzigingen  aangebracht:

* Sleutels voor het aanroepen van HTTP-eindpunten worden altijd versleuteld opgeslagen in Azure Blob-opslag. In versie 1.x werden sleutels standaard opgeslagen in Azure File opslag. Bij het upgraden van een app vanaf versie 1.x naar versie 2.x worden bestaande geheimen in de bestandsopslag opnieuw ingesteld.

* De runtime versie 2.x bevat geen ingebouwde ondersteuning voor webhook-providers. Deze wijziging is aangebracht om prestaties te verbeteren. U kunt wel HTTP-triggers gebruiken als eindpunten voor webhooks.

* Het configuratiebestand van de host (host.json) moet leeg zijn of de tekenreeks `"version": "2.0"` bevatten.

* Voor het verbeteren van bewaking wordt het WebJobs-dashboard in de portal dat gebruik maakte van de instelling [ `AzureWebJobsDashboard` ](functions-app-settings.md#azurewebjobsdashboard) vervangen door Azure Application Insights, dat gebruikmaakt van de [ `APPINSIGHTS_INSTRUMENTATIONKEY` ](functions-app-settings.md#appinsights_instrumentationkey) instelling. Zie voor meer informatie, [Monitor Azure Functions](functions-monitoring.md).

* Alle functies in een functie-app moeten dezelfde taal delen. Wanneer u een functie-app maakt, moet u een runtimestack voor de app kiezen. De runtimestack wordt opgegeven door de [ `FUNCTIONS_WORKER_RUNTIME` ](functions-app-settings.md#functions_worker_runtime) waarde in de toepassingsinstellingen. Deze vereiste is toegevoegd om de footprint en de opstarttijd te verbeteren. Wanneer u lokaal ontwikkelt moet u ook deze instelling in het [bestand local.settings.json](functions-run-local.md#local-settings-file) opnemen.

* De standaardtime-out voor functies in een App Service-plan is gewijzigd naar 30 minuten. U kunt handmatig de time-out op onbeperkt instellen met behulp van de [functionTimeout](functions-host-json.md#functiontimeout) in host.json.

* HTTP-concurrency-vertragingen zijn standaard ingesteld voor functies in een verbruiksplan, met een standaardwaarde van 100 gelijktijdige aanvragen per instantie. U kunt dit wijzigen via de [ `maxConcurrentRequests` ](functions-host-json.md#http) instelling in het bestand host.json.

* Vanwege [.NET core beperkingen](https://github.com/Azure/azure-functions-host/issues/3414) is ondersteuning voor functies in F#-script (.fsx) verwijderd. Gecompileerde F#-functies (.fs) worden nog steeds ondersteund.

* De URL-indeling van Event Grid trigger webhooks is gewijzigd in `https://{app}/runtime/webhooks/{triggerName}`.

### <a name="migrating-a-locally-developed-application"></a>Migreren van een lokaal ontwikkelde toepassing

U hebt mogelijk bestaande projecten voor apps die lokaal zijn ontwikkeld voor gebruik van de versie 1.x runtime. Om te upgraden naar versie 2.x maakt u een lokale functie-app-project op basis van versie 2.x en poort uw bestaande code naar de nieuwe app. U kunt handmatig het bestaande project en de code bijwerken als een soort 'in-place' upgrade. Er is echter een aantal andere verbeteringen tussen versie 1.x en versie 2.x die u mogelijk nog steeds moet aanbrengen. Bijvoorbeeld, het object voor het opsporen van fouten in C# is gewijzigd van `TraceWriter` naar `ILogger`. Als u een nieuw versie 2.x-project maakt, begint u met bijgewerkte functies op basis van de meest recente versie 2.x-sjablonen.

#### <a name="visual-studio-runtime-versions"></a>Versies van Visual Studio-runtime

In Visual Studio selecteert u de runtimeversie wanneer u een project maakt. Azure Functions-hulpprogramma's voor Visual Studio bieden ondersteuning voor beide grote runtime-versies. De juiste versie wordt gebruikt voor foutopsporing en publicatie op basis van de instellingen voor het project. De versie-instellingen worden gedefinieerd in het `.csproj` bestand in de volgende eigenschappen:

##### <a name="version-1x"></a>Versie 1.x

```xml
<TargetFramework>net461</TargetFramework>
<AzureFunctionsVersion>v1</AzureFunctionsVersion>
```

##### <a name="version-2x"></a>Versie 2.x

```xml
<TargetFramework>netcoreapp2.2</TargetFramework>
<AzureFunctionsVersion>v2</AzureFunctionsVersion>
```

Wanneer u fouten opspoort of uw project publiceert wordt de juiste versie van de runtime gebruikt.

#### <a name="vs-code-and-azure-functions-core-tools"></a>VS Code en Azure Functions Core Tools

[Azure Functions Core Tools](functions-run-local.md) wordt gebruikt voor het ontwikkelen vanaf de opdrachtregel en ook door de [Azure Functions-extensie](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) voor Visual Studio Code. Installeer versie 2.x van de Core-hulpprogramma's om te ontwikkelen voor versie 2.x. Versie 1.x ontwikkeling vereist versie 1.x van de essentiële hulpprogramma. Zie voor meer informatie, [Azure Functions Core Tools installeren](functions-run-local.md#install-the-azure-functions-core-tools).

Voor ontwikkeling in Visual Studio Code moet u mogelijk ook de gebruikersinstelling voor de `azureFunctions.projectRuntime` bijwerken zodat deze overeenkomt met de versie van de hulpprogramma's die zijn geïnstalleerd.  Deze instelling werkt ook de sjablonen en talen bij die worden gebruikt tijdens het maken van de functie-app.

### <a name="changing-version-of-apps-in-azure"></a>Wijzigen van de versie van apps in Azure

De versie van de runtime van de functies die worden gebruikt door de gepubliceerde apps in Azure wordt bepaald door de [ `FUNCTIONS_EXTENSION_VERSION` ](functions-app-settings.md#functions_extension_version) toepassingsinstelling. Een waarde van `~2` is gericht op de runtime versie 2.x en `~1` is gericht op de versie 1.x-runtime. Deze instelling kunt u niet willekeurig  wijzigen omdat er waarschijnlijk andere wijzigingen voor app-instelling en wijzigingen van de code in uw functies vereist zijn. Zie voor meer informatie over de aanbevolen manier om uw functie-app migreren naar een andere runtimeversie, [hoe te richten op versies van Azure Functions-runtime](set-runtime-version.md).

## <a name="bindings"></a>Bindingen

De runtime versie 2.x maakt gebruik van een nieuw [binding uitbreidingsmodel](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) die deze voordelen biedt:

* Ondersteuning voor bindinguitbreidingen van derden.

* Ontkoppeling van de runtime en bindingen. Door deze wijziging kunnen bindinguitbreidingen onafhankelijk van elkaar worden samengesteld en vrijgegeven. U kunt bijvoorbeeld kiezen om te upgraden naar een versie van een extensie die is gebaseerd op een nieuwere versie van een onderliggende SDK.

* Een lichtere uitvoeringsomgeving, waarbij alleen de bindingen die in gebruik zijn door de runtime bekend worden en geladen.

Met uitzondering van de HTTP- en timer-triggers, moeten alle bindingen expliciet worden toegevoegd aan het functie-app-project en geregistreerd in de portal. Zie voor meer informatie, [bindinguitbreidingen registreren](./functions-bindings-expressions-patterns.md).

In de volgende tabel ziet u welke bindingen worden ondersteund in elke runtimeversie.

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

[!INCLUDE [Timeout Duration section](../../includes/functions-timeout-duration.md)]

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

* [Code en Azure Functions lokaal testen](functions-run-local.md)
* [Het doel-Azure Functions runtime-versies](set-runtime-version.md)
* [Releaseopmerkingen](https://github.com/Azure/azure-functions-host/releases)

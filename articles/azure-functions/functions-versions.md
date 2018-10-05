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
ms.openlocfilehash: 372cf445e518ccdb287ce23ade6a3d92ddc5bc2b
ms.sourcegitcommit: 4edf9354a00bb63082c3b844b979165b64f46286
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48784896"
---
# <a name="azure-functions-runtime-versions-overview"></a>Overzicht van Azure Functions runtime-versies

 Er zijn twee primaire versies van de Azure Functions-runtime: 1.x en 2.x. De huidige versie, waarbij nieuwe functie werk en verbeteringen worden aangebracht is 2.x gebruikt, hoewel beide worden ondersteund voor productiescenario's.  De volgende details van de verschillen tussen de twee, hoe u elke versie maken en een upgrade van 1.x naar 2.x.

> [!NOTE] 
> Dit artikel wordt verwezen naar de cloudservice Azure Functions. Zie voor informatie over de preview-product waarmee u Azure Functions on-premises uitvoert, de [overzicht van Azure Functions Runtime](functions-runtime-overview.md).

## <a name="creating-1x-apps"></a>Het maken van 1.x-apps

Nieuwe apps die zijn gemaakt in de Azure Portal worden standaard ingesteld op 2.x omdat dit de meest recente versie en waar de nieuwe functie investeringen worden aangebracht.  U kunt echter nog steeds v1.x apps maken door het volgende te doen.

1. Een Azure-functie maken vanuit Azure Portal
1. Open de gemaakte app en het leeg is, opent u de **functie-instellingen**
1. Wijzig de versie van ongeveer 2 tot ~ 1.  *Met deze schakelaar wordt uitgeschakeld als u functies in uw app hebt*.
1. Klik op Opslaan en start de app opnieuw.  Alle sjablonen moeten nu maken en uitvoeren in 1.x.

## <a name="cross-platform-development"></a>Platformoverschrijdende ontwikkeling

Runtime 1.x ondersteunt ontwikkeling en die als host fungeert voor alleen in de portal of op Windows. Runtime 2.x wordt uitgevoerd op .NET Core 2, wat betekent dat het kan worden uitgevoerd op alle platformen worden ondersteund door .NET Core, met inbegrip van macOS en Linux. Hiermee kunnen platformoverschrijdende ontwikkeling en hosting van scenario's.

## <a name="languages"></a>Talen

Runtime 2.x maakt gebruik van een nieuwe taalmodel voor uitbreidbaarheid. Bovendien zijn ter verbetering van hulpprogramma's en prestaties van elke app in 2.x is beperkt tot hebben alleen functies in één taal. Ondersteunde talen in 2.x zijn C#, F #, JavaScript en Java. Azure Functions 1.x experimentele talen zijn niet bijgewerkt voor het gebruik van het nieuwe model, zodat ze worden niet ondersteund in 2.x. De volgende tabel geeft aan welke programmeertalen worden ondersteund in elke runtimeversie.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Zie voor meer informatie, [ondersteunde talen](supported-languages.md).

## <a name="migrating-from-1x-to-2x"></a>Migreren van 1.x naar 2.x

U kunt desgewenst een bestaande app, geschreven in 1.x om 2.x verplaatsen.  De meeste van de overwegingen vereist bij het verplaatsen tussen versies zijn gerelateerd aan de language runtime wijzigingen bovenstaande (bijvoorbeeld C# verplaatsen van .NET Framework 4.7 naar .NET Core 2).  Moet u controleren of uw code en bibliotheken zijn compatibel met de taal-runtimes wordt gebruikt.  Ook worden Noteer eventuele wijzigingen in de trigger, bindingen en functies die hieronder beschreven.

### <a name="changes-in-triggers-and-bindings"></a>Wijzigingen in de triggers en bindingen

De meeste van de trigger en binding eigenschappen en de configuraties zijn hetzelfde voor de versies, in 2.x u moet voor het installeren van een trigger of -binding aan de app. De enige uitzondering voor deze is http- en Timer triggers.  Zie [registreren en installeer binding extensies](./functions-triggers-bindings.md#register-binding-extensions).  Let op: Er kan ook worden gewijzigd de `function.json` of kenmerken van de functie tussen versies (bijvoorbeeld CosmosDB `connection` eigenschap is nu `ConnectionStringSetting`).  Naslaginformatie over de [bestaande binding tabel](#bindings) voor koppelingen naar documentatie voor elke binding.

### <a name="changes-in-features-available"></a>Wijzigingen in de functies die beschikbaar zijn

Naast de wijzigingen in talen en bindingen zijn er enkele functies die zijn verwijderd, bijgewerkt, of tussen versies vervangen.  Hieronder vindt u enkele van de belangrijkste overwegingen bij het starten met 2.x nadat u 1.x.  De volgende wijzigingen zijn aangebracht in v2.x:

* Sleutels voor het aanroepen van een functie worden altijd in versleutelde blob-opslag opgeslagen. In 1.x standaard ze zijn in de opslag van bestanden en kunnen worden verplaatst naar de blob als functies zoals sleuven inschakelen.  Als een 1.x-app bijwerken naar 2.x en geheimen in de opslag van bestanden op dit moment kunt u ze worden ingesteld.
* Voor betere prestaties worden triggers van het type "webhook" verwijderd en vervangen door 'HTTP'-triggers.
* De hostconfiguratie (`host.json`) moet ofwel niet leeg zijn of bevatten `version` van `2.0` voor een van de eigenschappen.
* Bewaking en observability, het WebJobs-Dashboard te verbeteren (`AzureWebJobsDashboard`) wordt vervangen door [Azure Application Insights](functions-monitoring.md) (`APPINSIGHTS_INSTRUMENTATIONKEY`)
* Toepassingsinstellingen (`local.settings.json`) is een waarde vereist voor de eigenschap `FUNCTIONS_WORKER_RUNTIME` die wordt toegewezen aan de taal van de app `dotnet | node | java | python`.
    * Ter verbetering van de voetafdruk en opstarten van zijn apps beperkt tot één taal. U kunt meerdere apps als u functies in verschillende talen voor de dezelfde oplossing wilt publiceren.
* Standaardtime-out voor functies in een app service-plan is 30 minuten.  Dit kan nog steeds worden handmatig ingesteld op onbeperkt.
* HTTP-gelijktijdigheid vertragingen in zijn standaard voor verbruik plan functies (100 gelijktijdige aanvragen per exemplaar) geïmplementeerd.  Deze instellingen kunnen worden gewijzigd de `host.json` bestand.
* [Vanwege een .NET core ten aanzien](https://github.com/Azure/azure-functions-host/issues/3414), `.fsx` scripts voor F #-functies zijn verwijderd. Gecompileerde F #-functies worden nog steeds ondersteund.
* De indeling van triggers met webhooks (bijvoorbeeld Event Grid) is gewijzigd in `https://{app}/runtime/webhooks/{triggerName}`

### <a name="upgrading-a-locally-developed-application"></a>Bijwerken van een lokaal ontwikkelde toepassing

Als uw app v1.x lokaal is ontwikkeld, kunt u wijzigingen aanbrengt aan de app of het om compatibel te maken met v2-project.  Het verdient aanbeveling om te maken van een nieuwe app en de poort over de code in de nieuwe app.  Er zijn wijzigingen die aan een bestaande app kunnen worden gemaakt voor het uitvoeren van een in-place upgrade, er zijn een aantal andere verbeteringen tussen v1 en v2 die verouderde code waarschijnlijk niet van profiteren is (bijvoorbeeld in C# de wijziging van `TraceWriter` te `ILogger`) .  

De aanbevolen procedure is beginnen met een van de v2-sjablonen en beweeg over de code in een nieuw project of de app.

#### <a name="visual-studio-runtime-versions"></a>Versies van Visual Studio-runtime

In Visual Studio selecteert u de runtimeversie wanneer u een project maakt.  Visual Studio heeft de bits voor beide primaire versies en dynamisch kan gebruikmaken van de juiste is voor het project.  Deze instellingen zijn afgeleid van de `.csproj` bestand.  Voor de 1.x-apps heeft het project de eigenschappen

```xml
<TargetFramework>net461</TargetFramework>
<AzureFunctionsVersion>v1</AzureFunctionsVersion>
```

In de Projecteigenschappen zijn v2

```xml
<TargetFramework>netstandard2.0</TargetFramework>
<AzureFunctionsVersion>v2</AzureFunctionsVersion>
```

Te klikken op fouten opspoort of publiceert, wordt de juiste versie onjuist ingesteld voor de instellingen van het project.

#### <a name="vs-code-and-azure-functions-core-tools"></a>VS Code en Azure Functions Core Tools

Andere lokale hulpprogramma's, is afhankelijk van de Azure Functions Core Tools.  Deze hulpprogramma's worden geïnstalleerd op de machine en doorgaans slechts één versie is geïnstalleerd op een ontwikkelcomputer in één keer.  Zie [instructies over het installeren van de specifieke versies van de essentiële hulpprogramma](./functions-run-local.md).

Voor VS Code mogelijk moet u ook om bij te werken van de gebruikersinstelling voor de `azureFunctions.projectRuntime` overeenkomt met de versie van de hulpprogramma's geïnstalleerd.  Dit wordt ook de sjablonen en talen die worden opgehaald tijdens het maken van nieuwe apps bijwerken.

### <a name="changing-version-of-apps-in-azure"></a>Wijzigen van de versie van apps in Azure

Gepubliceerde app-versies worden ingesteld via de toepassingsinstelling `FUNCTIONS_EXTENSION_VERSION`.  Deze optie is ingesteld op `~2` voor v2-apps en `~1` voor v1-apps.  Het wordt sterk afgeraden om te wijzigen van de runtimeversie van een app met bestaande functies die zijn gepubliceerd naar het ook de zonder code te wijzigen van deze functies.  De aanbevolen procedure is een nieuwe functie-app maken en ingesteld op de juiste versie, wijzigingen, testen en vervolgens uitschakelen of verwijderen van de vorige app.

## <a name="bindings"></a>Bindingen 

Runtime 2.x maakt gebruik van een nieuwe [binding uitbreidingsmodel](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) die deze voordelen biedt:

* Ondersteuning voor extensies van derden binding.
* Ontkoppeling van de runtime en -bindingen. Hiermee wordt bindinguitbreidingen worden samengesteld en vrijgegeven onafhankelijk van elkaar. U kunt bijvoorbeeld kiezen om te upgraden naar een versie van een extensie die is gebaseerd op een nieuwere versie van een onderliggende SDK.
* Een lichtere uitvoeringsomgeving, waarbij alleen de bindingen in gebruik zijn bekend en geladen door de runtime.

Alle ingebouwde Azure Functions-bindingen dit model hebben aangenomen en worden niet meer opgenomen in de standaardinstelling, met uitzondering van de Timer-trigger en de HTTP-trigger. Deze extensies worden automatisch geïnstalleerd wanneer u functies met de hulpprogramma's zoals Visual Studio of via de portal maken.

De volgende tabel geeft aan welke veldparameterbindings ondersteund in elke runtimeversie.

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

* [Azure-functies lokaal programmeren en testen](functions-run-local.md)
* [Het doel-Azure Functions runtime-versies](set-runtime-version.md)
* [Releaseopmerkingen](https://github.com/Azure/azure-functions-host/releases)

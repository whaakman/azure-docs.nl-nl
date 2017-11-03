---
title: 'Azure Functions F # referentie voor ontwikkelaars | Microsoft Docs'
description: 'Begrijpen hoe Azure-functies met F # ontwikkelen.'
services: functions
documentationcenter: fsharp
author: sylvanc
manager: jbronsk
editor: 
tags: 
keywords: 'Azure functions, functies, gebeurtenisverwerking webhooks, dynamische compute, zonder server architectuur, F #'
ms.assetid: e60226e5-2630-41d7-9e5b-9f9e5acc8e50
ms.service: functions
ms.devlang: fsharp
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/09/2016
ms.author: syclebsc
ms.openlocfilehash: 314f528a1fcef2c7afb0eedba012023f3bc9502b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-f-developer-reference"></a>Naslaginformatie over Azure Functions-F # ontwikkelaars
[!INCLUDE [functions-selector-languages](../../includes/functions-selector-languages.md)]

F # voor Azure Functions is een oplossing voor het eenvoudig uitvoeren van kleine stukjes code of 'functies' in de cloud. Gegevensstromen in de functie F # via functieargumenten. Argumentnamen zijn opgegeven in `function.json`, en er zijn vooraf gedefinieerde namen voor toegang tot de beheerder de functie berichtenlogboek en annulering tokens.

In dit artikel wordt ervan uitgegaan dat u al hebt gelezen de [naslaginformatie voor ontwikkelaars van Azure Functions](functions-reference.md).

## <a name="how-fsx-works"></a>De werking van .fsx
Een `.fsx` bestand is een script F #. Deze kan worden beschouwd als een F #-project dat opgenomen in één bestand. Het bestand bevat zowel de code voor het programma (in dit geval uw Azure-functie) en richtlijnen voor het beheren van afhankelijkheden.

Als u werkt met een `.fsx` voor een Azure-functie vaak vereiste assembly's worden automatisch opgenomen voor u, zodat u zich richten op de functie in plaats van 'standaard'-code.

## <a name="binding-to-arguments"></a>Binding met argumenten
Elke binding ondersteunt sommige set argumenten, zoals beschreven in de [Azure Functions triggers en bindingen naslaginformatie](functions-triggers-bindings.md). Een van de argument bindingen die ondersteuning biedt voor een trigger blob is bijvoorbeeld een POCO die kan worden uitgedrukt met behulp van een record F #. Bijvoorbeeld:

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

Uw Azure-functie F # duurt een of meer argumenten. Wanneer we over Azure Functions argumenten hebben, is *invoer* argumenten en *uitvoer* argumenten. Een invoerargument is precies deze klinkt als: invoer voor uw Azure-functie F #. Een *uitvoer* -argument is veranderlijke gegevens of een `byref<>` argument die fungeert als een manier om gegevens weer te geven *uit* van de functie.

In het bovenstaande voorbeeld `blob` is een invoerargument en `output` is een argument van de uitvoer. U ziet dat we gebruikt `byref<>` voor `output` (is niet nodig om toe te voegen de `[<Out>]` aantekening). Met behulp van een `byref<>` type biedt de mogelijkheid uw functie te wijzigen welke record of het argument naar verwijst object.

Wanneer een record F # wordt gebruikt als een type invoer, de definitie van de record moet worden gemarkeerd met `[<CLIMutable>]` om de Azure Functions-framework de velden op de juiste wijze instellen voordat de record aan de functie doorgegeven. Achter de schermen `[<CLIMutable>]` genereert setters voor de recordeigenschappen. Bijvoorbeeld:

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

Een klasse F # kan ook worden gebruikt voor zowel in- en argumenten. Voor een klasse moet eigenschappen meestal getters en setters. Bijvoorbeeld:

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a>Logboekregistratie
Aanmelden van uitvoer naar uw [streaminglogboeken](../app-service/web-sites-enable-diagnostic-log.md) in F #, de functie moet rekening houden met een argument van het type `TraceWriter`. Voor consistentie, raden we dit argument heet `log`. Bijvoorbeeld:

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a>Asynchrone
De `async` werkstroom kan worden gebruikt, maar het resultaat moet retourneren een `Task`. Dit kan worden gedaan met `Async.StartAsTask`, bijvoorbeeld:

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a>Annulering Token
Als de functie afsluiten probleemloos verwerken moet, u kunt daarvoor een [ `CancellationToken` ](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argument. Dit kan worden gecombineerd met `async`, bijvoorbeeld:

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a>Naamruimten importeren
Naamruimten kan worden geopend in de gebruikelijke manier:

```fsharp
open System.Net
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

Automatisch geopend op de volgende naamruimten:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Verwijzen naar externe assembly 's
Op deze manier kunnen framework-assembly-verwijzingen worden toegevoegd met de `#r "AssemblyName"` richtlijn.

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

De volgende assembly's worden automatisch toegevoegd door de Azure Functions hostomgeving:

* `mscorlib`,
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`.

Bovendien de volgende assembly's zijn speciale geïntegreerd en kan worden verwezen door simplename (bijvoorbeeld `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`.

Als u nodig hebt om te verwijzen naar een persoonlijke assembly, kunt u in het assembly-bestand uploaden een `bin` map ten opzichte van uw functie en de verwijzing naar deze met behulp van het bestand (bijvoorbeeld een naam  `#r "MyAssembly.dll"`). Zie voor meer informatie over de bestanden uploaden naar uw map voor de functie in de volgende sectie op het gebied van pakket.

## <a name="editor-prelude"></a>Editor Prelude
Een editor die ondersteuning biedt voor F # Compiler-Services worden niet op de hoogte van de naamruimten en assembly's met Azure Functions automatisch. Hierdoor kan het handig zijn om op te nemen van een prelude waarmee de editor voor de assembly's die u gebruikt vinden en openen expliciet naamruimten. Bijvoorbeeld:

```fsharp
#if !COMPILED
#I "../../bin/Binaries/WebJobs.Script.Host"
#r "Microsoft.Azure.WebJobs.Host.dll"
#endif

open Sytem
open Microsoft.Azure.WebJobs.Host

let Run(blob: string, output: byref<string>, log: TraceWriter) =
    ...
```

Wanneer uw code wordt uitgevoerd in Azure Functions, verwerking van de bron met `COMPILED` gedefinieerd, zodat de prelude editor worden genegeerd.

<a name="package"></a>

## <a name="package-management"></a>Pakket management
Als u NuGet-pakketten in een functie F #, Voeg een `project.json` van het bestand in de map van de functie in het bestandssysteem van de functie-app. Hier volgt een voorbeeld `project.json` -bestand dat wordt toegevoegd een NuGet-pakket verwijzing naar `Microsoft.ProjectOxford.Face` versie 1.1.0:

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

Alleen .NET Framework 4.6 wordt ondersteund, dus zorg ervoor dat uw `project.json` bestand geeft `net46` zoals hier wordt weergegeven.

Wanneer u uploadt een `project.json` bestand, de runtime opgehaald van de pakketten en verwijzingen naar de pakket-assembly's worden automatisch toegevoegd. U hoeft niet te voegen `#r "AssemblyName"` richtlijnen. Alleen toe te voegen de vereiste `open` instructies voor uw `.fsx` bestand.

U kunt desgewenst automatisch verwijzingen naar assembly's in uw editor prelude, voor het verbeteren van uw editor interactie met F # compileren Services plaatsen.

### <a name="how-to-add-a-projectjson-file-to-your-azure-function"></a>Het toevoegen van een `project.json` bestand naar uw Azure-functie
1. Begin door te controleren of de functie-app wordt uitgevoerd, die u kunt doen door de functie openen in de Azure-portal. Dit biedt ook toegang naar de streaminglogboeken waar pakket installatie uitvoer wordt weergegeven.
2. Voor het uploaden van een `project.json` bestand, gebruikt u een van de methoden van [het bijwerken van de functie app-bestanden](functions-reference.md#fileupdate). Als u [continue implementatie voor Azure Functions](functions-continuous-deployment.md), kunt u toevoegen een `project.json` bestand naar uw vertakking test om te experimenteren voordat u deze toevoegt aan uw vertakking implementatie.
3. Na de `project.json` bestand wordt toegevoegd, ziet u uitvoer die vergelijkbaar is met het volgende voorbeeld in de functie de streaming-logboek:

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189
2016-04-04T19:02:57.189
2016-04-04T19:02:57.455 Packages restored.
```

## <a name="environment-variables"></a>Omgevingsvariabelen
Als u een omgevingsvariabele of een app die waarde instellen, gebruikt `System.Environment.GetEnvironmentVariable`, bijvoorbeeld:

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a>Hergebruik van code .fsx
U kunt de code van andere `.fsx` bestanden met behulp van een `#load` richtlijn. Bijvoorbeeld:

`run.fsx`

```fsharp
#load "logger.fsx"

let Run(timer: TimerInfo, log: TraceWriter) =
    mylog log (sprintf "Timer: %s" DateTime.Now.ToString())
```

`logger.fsx`

```fsharp
let mylog(log: TraceWriter, text: string) =
    log.Verbose(text);
```

Paden biedt aan de `#load` richtlijn zijn ten opzichte van de locatie van uw `.fsx` bestand.

* `#load "logger.fsx"`een bestand in de map van de functie laadt.
* `#load "package\logger.fsx"`laden van een bestand in de `package` map in de map van de functie.
* `#load "..\shared\mylogger.fsx"`laden van een bestand in de `shared` map op hetzelfde niveau als de functie-map, dat wil zeggen, direct onder `wwwroot`.

De `#load` richtlijn werkt alleen met `.fsx` (F # script)-bestanden, en niet met `.fs` bestanden.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie:

* [F # handleiding](/dotnet/articles/fsharp/index)
* [Aanbevolen procedures voor Azure Functions](functions-best-practices.md)
* [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)
* [Azure Functions-triggers en bindingen](functions-triggers-bindings.md)
* [Azure Functions testen](functions-test-a-function.md)
* [Azure Functions schalen](functions-scale.md)


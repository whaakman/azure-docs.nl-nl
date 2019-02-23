---
title: Azure Functions F# referentie voor ontwikkelaars | Microsoft Docs
description: Meer informatie over het ontwikkelen van Azure Functions met behulp van F# script.
services: functions
documentationcenter: fsharp
author: sylvanc
manager: jbronsk
keywords: Azure functions, functies, gebeurtenisverwerking, webhooks, dynamisch berekenen, serverloze architectuurF#
ms.assetid: e60226e5-2630-41d7-9e5b-9f9e5acc8e50
ms.service: azure-functions
ms.devlang: fsharp
ms.topic: reference
ms.date: 10/09/2018
ms.author: syclebsc
ms.openlocfilehash: e7e4e898142d6f9d1a93e91c1f1476ff81fc7d3c
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56734656"
---
# <a name="azure-functions-f-developer-reference"></a>Azure Functions F# referentie voor ontwikkelaars

F#voor Azure Functions is een oplossing voor het eenvoudig uitvoeren van kleine stukjes code, ofwel 'functies' in de cloud. Gegevens worden overgebracht naar uw F# functie via argumenten voor de functie. Argumentnamen zijn opgegeven in `function.json`, en er zijn vooraf gedefinieerde namen voor toegang tot items zoals de functie logger en annulering tokens. 

>[!IMPORTANT]
>F#script (.fsx) wordt alleen ondersteund door [versie 1.x](functions-versions.md#creating-1x-apps) van de Azure Functions-runtime. Als u wilt gebruiken F# met de runtime versie 2.x, moet u een vooraf gecompileerde F# -klassebibliotheekproject (.fs). Maken, beheren en publiceren een F# -klassebibliotheekproject met Visual Studio, net als een [ C# -klassebibliotheekproject](functions-dotnet-class-library.md). Zie voor meer informatie over de versies van functies, [overzicht van de versies van de Azure Functions runtime](functions-versions.md).

In dit artikel wordt ervan uitgegaan dat u al hebt gelezen de [referentie voor ontwikkelaars van Azure Functions](functions-reference.md).

## <a name="how-fsx-works"></a>De werking van .fsx
Een `.fsx` bestand is een F# script. Het kan worden beschouwd als een F# project dat opgenomen in één bestand. Het bestand bevat zowel de code voor uw programma (in dit geval uw Azure-functie) en richtlijnen voor het beheren van afhankelijkheden.

Wanneer u gebruikt een `.fsx` voor een Azure-functie, vaak vereiste assembly's worden automatisch opgenomen voor u, zodat u zich kunt richten op de functie in plaats van 'standaard'-code.

## <a name="folder-structure"></a>mapstructuur

De mapstructuur voor een F# script project ziet er als volgt uit:

```
FunctionsProject
 | - MyFirstFunction
 | | - run.fsx
 | | - function.json
 | | - function.proj
 | - MySecondFunction
 | | - run.fsx
 | | - function.json
 | | - function.proj
 | - host.json
 | - extensions.csproj
 | - bin
```

Er is een gedeelde [host.json](functions-host-json.md) -bestand dat kan worden gebruikt voor het configureren van de functie-app. Elke functie heeft een eigen codebestand (.fsx) en de binding-configuratiebestand (function.json).

De binding-extensies vereist in [versie 2.x](functions-versions.md) van de functies runtime zijn gedefinieerd in de `extensions.csproj` bestand met de werkelijke dll-bestanden in de `bin` map. Als u lokaal ontwikkelt, moet u [bindinguitbreidingen registreren](./functions-bindings-register.md#local-development-azure-functions-core-tools). Bij het ontwikkelen van functies in Azure portal, geldt deze registratie voor u.

## <a name="binding-to-arguments"></a>Binding met argumenten
Elke binding ondersteunt een enkele set argumenten, zoals beschreven in de [Azure Functions-triggers en bindingen naslaginformatie](functions-triggers-bindings.md). Een van de bindingen van het argument biedt ondersteuning voor een blobtrigger is bijvoorbeeld een POCO, die kan worden uitgedrukt met behulp van een F# record. Bijvoorbeeld:

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

Uw F# Azure-functie duurt een of meer argumenten. Wanneer we over Azure Functions argumenten praten, verwijzen we naar *invoer* argumenten en *uitvoer* argumenten. Een invoer voor argument is precies wat je zou denken, zoals: invoer voor uw F# Azure-functie. Een *uitvoer* argument is veranderlijke gegevens of een `byref<>` argument die fungeert als een manier om gegevens weer te geven *uit* van uw functie.

In het bovenstaande voorbeeld `blob` is een invoer-argument en `output` is een uitvoer-argument. U ziet dat we hebben gebruikt `byref<>` voor `output` (Er is niet nodig om toe te voegen de `[<Out>]` aantekening). Met behulp van een `byref<>` type kunt u uw functie te wijzigen welke record of het argument naar verwijst object.

Wanneer een F# record wordt gebruikt als een type gebruikersinvoer, de definitie van de record moet worden gemarkeerd met `[<CLIMutable>]` om de Azure Functions-framework voor de velden op de juiste wijze ingesteld voordat de record wordt doorgegeven aan de functie staat te stellen. Achter de schermen `[<CLIMutable>]` genereert setters voor de recordeigenschappen. Bijvoorbeeld:

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: ILogger) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

Een F# klasse kan ook worden gebruikt voor zowel inkomende/uitgaande argumenten. Voor een klasse moet eigenschappen meestal getters en setters. Bijvoorbeeld:

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a>Logboekregistratie
Om aan te melden uitvoer naar uw [streaminglogboeken](../app-service/troubleshoot-diagnostic-logs.md) in F#, de functie moet rekening houden met een argument van het type [ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger). Voor consistentie, wordt aangeraden dit argument is met de naam `log`. Bijvoorbeeld:

```fsharp
let Run(blob: string, output: byref<string>, log: ILogger) =
    log.LogInformation(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a>Async
De `async` werkstroom kan worden gebruikt, maar het resultaat moet retourneren een `Task`. Dit kan worden gedaan met `Async.StartAsTask`, bijvoorbeeld:

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a>Annulering Token
Als uw functie nodig heeft om af te handelen afsluiten zonder problemen, geeft u deze kunt een [ `CancellationToken` ](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argument. Dit kan worden gecombineerd met `async`, bijvoorbeeld:

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a>Naamruimten importeren
Naamruimten kunnen worden geopend op de gebruikelijke manier:

```fsharp
open System.Net
open System.Threading.Tasks
open Microsoft.Extensions.Logging

let Run(req: HttpRequestMessage, log: ILogger) =
    ...
```

De volgende naamruimten worden automatisch geopend:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Verwijst naar een externe assembly 's
Op deze manier framework-assembly-verwijzingen kunnen worden toegevoegd met de `#r "AssemblyName"` richtlijn.

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks
open Microsoft.Extensions.Logging

let Run(req: HttpRequestMessage, log: ILogger) =
    ...
```

De volgende assembly's worden automatisch toegevoegd door de omgeving voor het hosten van Azure Functions:

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

Bovendien de volgende assembly's zijn speciale indeling en kan worden verwezen door simplename (bijvoorbeeld `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`.

Als u nodig hebt om te verwijzen naar een privé-assembly, uploadt u het assemblybestand in een `bin` map ten opzichte van uw functie en de verwijzing met behulp van het bestand een naam (bijvoorbeeld)  `#r "MyAssembly.dll"`). Zie voor informatie over het uploaden van bestanden naar de map van uw functie in de volgende sectie op Pakketbeheer.

## <a name="editor-prelude"></a>Editor voorproefje
Een editor die ondersteuning biedt voor F# Compiler Services zich niet op de hoogte van de naamruimten en de assembly's met Azure Functions automatisch. Daarom kan het nuttig zijn om op te nemen van een voorproefje op waarmee de editor vinden van de assembly's die u gebruikt, en naamruimten expliciet te openen. Bijvoorbeeld:

```fsharp
#if !COMPILED
#I "../../bin/Binaries/WebJobs.Script.Host"
#r "Microsoft.Azure.WebJobs.Host.dll"
#endif

open System
open Microsoft.Azure.WebJobs.Host
open Microsoft.Extensions.Logging

let Run(blob: string, output: byref<string>, log: ILogger) =
    ...
```

Wanneer uw code wordt uitgevoerd in Azure Functions, verwerkt de bron met `COMPILED` gedefinieerd, zodat de voorproefje editor worden genegeerd.

<a name="package"></a>

## <a name="package-management"></a>Pakketbeheer
Gebruik de NuGet-pakketten in een F# functioneren, toe te voegen een `project.json` -bestand naar de map van de functie in het bestandssysteem van de functie-app. Hier volgt een voorbeeld `project.json` -bestand dat wordt toegevoegd een NuGet-pakketverwijzing naar `Microsoft.ProjectOxford.Face` versie 1.1.0:

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

Alleen de .NET Framework 4.6 wordt ondersteund, dus zorg ervoor dat uw `project.json` bestand `net46` zoals hier wordt weergegeven.

Wanneer u uploadt een `project.json` bestand, de runtime opgehaald van de pakketten en verwijzingen naar de pakket-assembly's worden automatisch toegevoegd. U hoeft niet te voegen `#r "AssemblyName"` richtlijnen. Alleen de vereiste toevoegen `open` instructies toe aan uw `.fsx` bestand.

U kunt desgewenst plaatst automatisch verwijzingen naar assembly's in uw editor voorproefje, voor het verbeteren van de editor interactie met F# Services worden gecompileerd.

### <a name="how-to-add-a-projectjson-file-to-your-azure-function"></a>Het toevoegen van een `project.json` -bestand naar uw Azure-functie
1. Begin door te zorgen dat uw functie-app wordt uitgevoerd, die u kunt doen door de functie openen in Azure portal. Dit geeft ook toegang tot de streaminglogboeken waar pakket installatie uitvoer wordt weergegeven.
2. Het uploaden van een `project.json` bestand, gebruik een van de methoden die worden beschreven in [het bijwerken van de functie app-bestanden](functions-reference.md#fileupdate). Als u [continue implementatie voor Azure Functions](functions-continuous-deployment.md), kunt u toevoegen een `project.json` bestand aan uw staging vertakking om te experimenteren met deze voordat u deze toevoegt aan de vertakking van uw implementatie.
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
Als u een omgevingsvariabele of een app-instelling waarde, gebruikt u `System.Environment.GetEnvironmentVariable`, bijvoorbeeld:

```fsharp
open System.Environment
open Microsoft.Extensions.Logging

let Run(timer: TimerInfo, log: ILogger) =
    log.LogInformation("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.LogInformation("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a>Hergebruik van code .fsx
U kunt de code van andere `.fsx` bestanden met behulp van een `#load` richtlijn. Bijvoorbeeld:

`run.fsx`

```fsharp
#load "logger.fsx"

let Run(timer: TimerInfo, log: ILogger) =
    mylog log (sprintf "Timer: %s" DateTime.Now.ToString())
```

`logger.fsx`

```fsharp
let mylog(log: ILogger, text: string) =
    log.LogInformation(text);
```

Paden biedt tot de `#load` richtlijn zijn ten opzichte van de locatie van uw `.fsx` bestand.

* `#load "logger.fsx"` een bestand in de map van de functie laadt.
* `#load "package\logger.fsx"` laden van een bestand te vinden in de `package` in de functie-map.
* `#load "..\shared\mylogger.fsx"` laden van een bestand te vinden in de `shared` map op hetzelfde niveau als de functie een map, dat wil zeggen rechtstreeks onder `wwwroot`.

De `#load` richtlijn werkt alleen met `.fsx` (F# script)-bestanden, en niet met `.fs` bestanden.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie:

* [F#Handleiding](/dotnet/articles/fsharp/index)
* [Aanbevolen procedures voor Azure Functions](functions-best-practices.md)
* [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)
* [Azure Functions-triggers en bindingen](functions-triggers-bindings.md)
* [Azure Functions testen](functions-test-a-function.md)
* [Azure Functions schalen](functions-scale.md)


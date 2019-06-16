---
title: PowerShell-referentie voor ontwikkelaars voor Azure Functions
description: Lees hoe u voor het ontwikkelen van functies met behulp van PowerShell.
services: functions
documentationcenter: na
author: tylerleonhardt
manager: jeconnoc
ms.service: azure-functions
ms.devlang: powershell
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: tyleonha, glenga
ms.openlocfilehash: fa82725174645a0e5f1d957d8423c97547682542
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67065481"
---
# <a name="azure-functions-powershell-developer-guide"></a>Ontwikkelaarshandleiding voor Azure Functions-PowerShell

In dit artikel bevat informatie over hoe u Azure Functions met behulp van PowerShell schrijven.

[!INCLUDE [functions-powershell-preview-note](../../includes/functions-powershell-preview-note.md)]

Een PowerShell Azure-functie (functie) wordt weergegeven als een PowerShell-script dat wordt uitgevoerd wanneer geactiveerd. Elke functie script heeft een bijbehorende `function.json` -bestand dat hoe de functie zich gedraagt definieert, zoals hoe deze wordt geactiveerd en de bijbehorende invoer- en -parameters. Zie voor meer informatie, de [Triggers en binding artikel](functions-triggers-bindings.md). 

Net als andere soorten functies, functies voor PowerShell-script uitvoeren in de parameters die overeenkomen met de namen van alle invoerbindingen gedefinieerd in de `function.json` bestand. Een `TriggerMetadata` parameter wordt ook doorgegeven die bevat aanvullende informatie over de trigger die de functie gestart.

In dit artikel wordt ervan uitgegaan dat u al hebt gelezen de [referentie voor ontwikkelaars van Azure Functions](functions-reference.md). U moet ook hebben voltooid de [Functions-Snelstartgids voor PowerShell](functions-create-first-function-powershell.md) om uw eerste PowerShell-functie te maken.

## <a name="folder-structure"></a>mapstructuur

De vereiste mapstructuur voor een PowerShell-project ziet er als volgt uit. Deze standaardinstelling kan worden gewijzigd. Zie voor meer informatie de [scriptbestand](#configure-function-scriptfile) onderstaande sectie.

```
PSFunctionApp
 | - MyFirstFunction
 | | - run.ps1
 | | - function.json
 | - MySecondFunction
 | | - run.ps1
 | | - function.json
 | - Modules
 | | - myFirstHelperModule
 | | | - myFirstHelperModule.psd1
 | | | - myFirstHelperModule.psm1
 | | - mySecondHelperModule
 | | | - mySecondHelperModule.psd1
 | | | - mySecondHelperModule.psm1
 | - local.settings.json
 | - host.json
 | - requirements.psd1
 | - profile.ps1
 | - extensions.csproj
 | - bin
```

In de hoofdmap van het project, er is een gedeelde [ `host.json` ](functions-host-json.md) -bestand dat kan worden gebruikt voor het configureren van de functie-app. Elke functie heeft een map met een eigen codebestand (.ps1) en het configuratiebestand van de binding (`function.json`). De naam van de bovenliggende map van de function.json-bestand is altijd de naam van uw functie.

Bepaalde bindingen vereist de aanwezigheid van een `extensions.csproj` bestand. Binding-extensies, vereist in [versie 2.x](functions-versions.md) van de Functions-runtime, zijn gedefinieerd in de `extensions.csproj` bestand met de werkelijke dll-bestanden in de `bin` map. Als u lokaal ontwikkelt, moet u [bindinguitbreidingen registreren](functions-bindings-register.md#local-development-with-azure-functions-core-tools-and-extension-bundles). Bij het ontwikkelen van functies in Azure portal, geldt deze registratie voor u.

In PowerShell-functie-Apps kan eventueel hebt u een `profile.ps1` die wordt uitgevoerd wanneer een functie-app begint te lopen (anders weten als een  *[koude start](#cold-start)* . Zie voor meer informatie, [PowerShell profiel](#powershell-profile).

## <a name="defining-a-powershell-script-as-a-function"></a>Een PowerShell-script definiëren als een functie

Standaard de Functions-runtime zoekt de functie in `run.ps1`, waarbij `run.ps1` deelt dezelfde bovenliggende map als het bijbehorende `function.json`.

Het script wordt een aantal argumenten doorgegeven op worden uitgevoerd. Voor het afhandelen van deze parameters toevoegen een `param` blok aan het begin van het script zoals in het volgende voorbeeld:

```powershell
# $TriggerMetadata is optional here. If you don't need it, you can safely remove it from the param block
param($MyFirstInputBinding, $MySecondInputBinding, $TriggerMetadata)
```

### <a name="triggermetadata-parameter"></a>TriggerMetadata parameter

De `TriggerMetadata` parameter wordt gebruikt voor het leveren van meer informatie over de trigger. Binding binding is afhankelijk van de aanvullende metagegevens, maar ze alle bevatten een `sys` eigenschap met de volgende gegevens:

```powershell
$TriggerMetadata.sys
```

| Eigenschap   | Description                                     | Type     |
|------------|-------------------------------------------------|----------|
| UtcNow     | Wanneer de functie in UTC, is geactiveerd        | DateTime |
| MethodName | De naam van de functie die is geactiveerd     | string   |
| RandGuid   | een unieke guid voor deze uitvoering van de functie | string   |

Elk triggertype heeft een andere set metagegevens. Bijvoorbeeld, de `$TriggerMetadata` voor `QueueTrigger` bevat de `InsertionTime`, `Id`, `DequeueCount`, onder andere. Voor meer informatie over de metagegevens van de wachtrijtrigger, gaat u naar de [officiële documentatie voor queue-triggers](functions-bindings-storage-queue.md#trigger---message-metadata). Raadpleeg de documentatie op de [triggers](functions-triggers-bindings.md) u samenwerkt om te zien wat in de metagegevens van de trigger wordt geleverd.

## <a name="bindings"></a>Bindingen

In PowerShell [bindingen](functions-triggers-bindings.md) zijn geconfigureerd en gedefinieerd in de function.json van een functie. Functies werken met bindingen een aantal manieren.

### <a name="reading-trigger-and-input-data"></a>Trigger voor lezen en invoergegevens

Trigger en invoerbindingen worden gelezen als parameters doorgegeven aan uw functie. Invoerbindingen hebben een `direction` ingesteld op `in` in function.json. De `name` -eigenschap worden gedefinieerd `function.json` is de naam van de parameter in de `param` blokkeren. Omdat PowerShell benoemde parameters voor binding gebruikt, kan de volgorde van de parameters maakt niet uit. Het is echter een aanbevolen procedure te volgen van de volgorde van de bindingen die is gedefinieerd de `function.json`.

```powershell
param($MyFirstInputBinding, $MySecondInputBinding)
```

### <a name="writing-output-data"></a>Uitvoer schrijven van gegevens

In de functies, een Uitvoerbinding heeft een `direction` ingesteld op `out` in de function.json. U kunt schrijven naar een Uitvoerbinding met behulp van de `Push-OutputBinding` cmdlet die beschikbaar is op de Functions-runtime. In alle gevallen moet de `name` eigenschap van de binding zoals gedefinieerd in `function.json` komt overeen met de `Name` parameter van de `Push-OutputBinding` cmdlet.

Hieronder ziet u hoe u aan te roepen `Push-OutputBinding` in uw functie-script:

```powershell
param($MyFirstInputBinding, $MySecondInputBinding)

Push-OutputBinding -Name myQueue -Value $myValue
```

U kunt ook een waarde doorgeven voor een specifieke binding via de pijplijn.

```powershell
param($MyFirstInputBinding, $MySecondInputBinding)

Produce-MyOutputValue | Push-OutputBinding -Name myQueue
```

`Push-OutputBinding` gedraagt zich anders op basis van de opgegeven waarde voor `-Name`:

* Wanneer de opgegeven naam kan niet worden omgezet naar een geldige Uitvoerbinding, is een fout opgetreden.

* Wanneer de Uitvoerbinding een verzameling waarden accepteert, u kunt aanroepen `Push-OutputBinding` herhaaldelijk om meerdere waarden.

* Wanneer de Uitvoerbinding alleen een singleton-waarde accepteert, aanroepen van `Push-OutputBinding` een tweede keer wordt een fout gegenereerd.

#### <a name="push-outputbinding-syntax"></a>`Push-OutputBinding` Syntaxis

Hieronder vindt u geldige parameters op voor het aanroepen `Push-OutputBinding`:

| Name | Type | Positie | Description |
| ---- | ---- |  -------- | ----------- |
| **`-Name`** | String | 1 | De naam van de Uitvoerbinding die u wilt instellen. |
| **`-Value`** | Object | 2 | De waarde van de Uitvoerbinding u wilt instellen, dat wordt geaccepteerd vanuit de pipeline ByValue. |
| **`-Clobber`** | SwitchParameter | met de naam | (Optioneel) Als u opgeeft, zorgt ervoor dat de waarde die moet worden ingesteld voor een opgegeven Uitvoerbinding. | 

De volgende algemene parameters worden ook ondersteund: 
* `Verbose`
* `Debug`
* `ErrorAction`
* `ErrorVariable`
* `WarningAction`
* `WarningVariable`
* `OutBuffer`
* `PipelineVariable`
* `OutVariable` 

Zie voor meer informatie, [over CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216).

#### <a name="push-outputbinding-example-http-responses"></a>Voorbeeld van de push-OutputBinding: HTTP-antwoorden

Een HTTP-trigger retourneert een antwoord met behulp van een Uitvoerbinding die met de naam `response`. In het volgende voorbeeld wordt de Uitvoerbinding van `response` heeft de waarde van "uitvoer #1":

```powershell
PS >Push-OutputBinding -Name response -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "output #1"
})
```

Omdat de uitvoer tot HTTP, die alleen een singleton-waarde accepteert is, wordt er een fout gegenereerd wanneer `Push-OutputBinding` heet een tweede keer.

```powershell
PS >Push-OutputBinding -Name response -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "output #2"
})
```

Voor uitvoer die voor singleton-waarden alleen accepteren, kunt u de `-Clobber` parameter voor de onderdrukking van de oude waarde in plaats van probeert toe te voegen aan een verzameling. Het volgende voorbeeld wordt ervan uitgegaan dat u al een waarde hebt toegevoegd. Met behulp van `-Clobber`, overschrijft de bestaande waarde om terug te keren een waarde van "uitvoer #3" van het antwoord van het volgende voorbeeld:

```powershell
PS >Push-OutputBinding -Name response -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "output #3"
}) -Clobber
```

#### <a name="push-outputbinding-example-queue-output-binding"></a>Voorbeeld van de push-OutputBinding: Queue Storage-Uitvoerbinding

`Push-OutputBinding` wordt gebruikt om gegevens te verzenden naar uitvoerbindingen, zoals een [Azure Queue storage-Uitvoerbinding](functions-bindings-storage-queue.md#output). In het volgende voorbeeld wordt heeft het bericht is geschreven naar de wachtrij de waarde "uitvoer #1":

```powershell
PS >Push-OutputBinding -Name outQueue -Value "output #1"
```

De Uitvoerbinding voor een opslagwachtrij accepteert meerdere uitvoerwaarden. In dit geval wordt het volgende voorbeeld aanroepen nadat de eerste naar de wachtrij een lijst met twee items schrijft: "uitvoer #1" en 'uitvoer #2'.

```powershell
PS >Push-OutputBinding -Name outQueue -Value "output #2"
```

Het volgende voorbeeld, wanneer aangeroepen na de vorige twee voegt twee meer waarden voor de verzameling uitvoer:

```powershell
PS >Push-OutputBinding -Name outQueue -Value @("output #3", "output #4")
```

Wanneer geschreven naar de wachtrij, het bericht bevat deze vier waarden: "uitvoer #1", "uitvoer #2", "uitvoer #3" en 'uitvoer #4'.

#### <a name="get-outputbinding-cmdlet"></a>`Get-OutputBinding` Cmdlet

U kunt de `Get-OutputBinding` cmdlet voor het ophalen van de waarden die momenteel is ingesteld voor uw uitvoerbindingen. Dit smdlet haalt een hash-tabel die de namen van de uitvoerbindingen met de bijbehorende waarden bevat. 

Hier volgt een voorbeeld van het gebruik van `Get-OutputBinding` om huidige binding waarden te retourneren:

```powershell
Get-OutputBinding
```

```Output
Name                           Value
----                           -----
MyQueue                        myData
MyOtherQueue                   myData
```

`Get-OutputBinding` bevat ook een parameter met de naam `-Name`, die kan worden gebruikt voor het filteren van de geretourneerde binding, zoals in het volgende voorbeeld:

```powershell
Get-OutputBinding -Name MyQ*
```

```Output
Name                           Value
----                           -----
MyQueue                        myData
```

Jokertekens (*) worden ondersteund in `Get-OutputBinding`.

## <a name="logging"></a>Logboekregistratie

Logboekregistratie in de PowerShell-functies, zoals logboekregistratie van reguliere PowerShell werkt. U kunt de cmdlets van logboekregistratie van gebruiken om te schrijven naar de uitvoerstroom. Elke cmdlet wordt toegewezen aan een logboek-niveau die worden gebruikt door de functies.

| Functies die het niveau van logboekregistratie | Cmdlet voor logboekregistratie |
| ------------- | -------------- |
| Fout | **`Write-Error`** |
| Waarschuwing | **`Write-Warning`**  | 
| Informatie | **`Write-Information`** <br/> **`Write-Host`** <br /> **`Write-Output`**      | Informatie | Schrijft naar _informatie_ logboekregistratie op. |
| fouten opsporen | **`Write-Debug`** |
| Tracering | **`Write-Progress`** <br /> **`Write-Verbose`** |

Naast deze cmdlets iets geschreven naar de pijplijn wordt omgeleid naar de `Information` log niveau en weergegeven met de standaardopmaak PowerShell.

> [!IMPORTANT]
> Met behulp van de `Write-Verbose` of `Write-Debug` cmdlets is niet genoeg om te zien uitgebreide en niveau van logboekregistratie voor foutopsporing. U moet ook de logboek niveau drempelwaarde, verklaart welk niveau van de logboeken die u daadwerkelijk interessante configureren. Zie voor meer informatie, [configureren van het logboek-niveau van de functie-app](#configure-the-function-app-log-level).

### <a name="configure-the-function-app-log-level"></a>Het logboek-niveau van de functie-app configureren

Azure Functions kunt u bij het definiëren van de drempelwaarde voor het gemakkelijk om te bepalen de manier waarop functies worden geschreven naar de logboeken. Als u wilt dat de drempelwaarde voor alle traceringen geschreven naar de console, gebruikt u de `logging.logLevel.default` eigenschap in de [ `host.json` bestand][naslaginformatie voor host.json]. Deze instelling geldt voor alle functies in uw functie-app.

Het volgende voorbeeld wordt de drempel voor het inschakelen van uitgebreide logboekregistratie in voor alle functies, maar Hiermee stelt u de drempel voor het inschakelen van logboekregistratie voor foutopsporing voor een functie met de naam `MyFunction`:

```json
{
    "logging": {
        "logLevel": {
            "Function.MyFunction": "Debug",
            "default": "Trace"
        }
    }
}  
```

Zie voor meer informatie, [naslaginformatie voor host.JSON].

### <a name="viewing-the-logs"></a>De logboeken bekijken

Als uw functie-App in Azure wordt uitgevoerd, kunt u Application Insights gebruiken om te controleren. Lezen [controlefuncties van Azure](functions-monitoring.md) voor meer informatie over het weergeven en uitvoeren van query's functielogboeken.

Als u uw functie-App voor de ontwikkeling van lokaal uitvoert, registreert standaard naar het bestandssysteem. Als u wilt zien in de logboekbestanden in de console, stelt de `AZURE_FUNCTIONS_ENVIRONMENT` omgevingsvariabele `Development` voordat u begint met de functie-App.

## <a name="triggers-and-bindings-types"></a>Triggers en bindingen typen

Er zijn een aantal triggers en bindingen beschikbaar voor gebruik met uw functie-app. De volledige lijst met triggers en bindingen [vindt u hier](functions-triggers-bindings.md#supported-bindings).

Alle triggers en bindingen worden weergegeven in de code als een paar echte gegevenstypen:

* Hash-tabel
* string
* byte[]
* int
* double
* HttpRequestContext
* HttpResponseContext

De eerste vijf typen in deze lijst zijn standaard .NET-typen. De laatste twee alleen worden gebruikt de [HttpTrigger trigger](#http-triggers-and-bindings).

Elke bindingsparameter in uw functies moet een van de volgende typen.

### <a name="http-triggers-and-bindings"></a>HTTP-triggers en bindingen

HTTP- en webhook-triggers en HTTP-output bindingen met de aanvraag en respons objecten vertegenwoordigen de HTTP-berichten.

#### <a name="request-object"></a>Request-object

De Aanvraagobject dat wordt doorgegeven in het script is van het type `HttpRequestContext`, heeft de volgende eigenschappen:

| Eigenschap  | Description                                                    | Type                      |
|-----------|----------------------------------------------------------------|---------------------------|
| **`Body`**    | Een object dat de hoofdtekst van de aanvraag bevat. `Body` in het beste type op basis van de gegevens is geserialiseerd. Bijvoorbeeld, als de gegevens JSON is, wordt doorgegeven als een hash-tabel. Als de gegevens een tekenreeks is, wordt deze doorgegeven als een tekenreeks. | object |
| **`Headers`** | Een woordenlijst met de aanvraagheaders.                | Dictionary < string, string ><sup>*</sup> |
| **`Method`** | De HTTP-methode van de aanvraag.                                | string                    |
| **`Params`**  | Een object dat de routering parameters van de aanvraag bevat. | Dictionary < string, string ><sup>*</sup> |
| **`Query`** | Een object met de queryparameters.                  | Dictionary < string, string ><sup>*</sup> |
| **`Url`** | De URL van de aanvraag.                                        | string                    |

<sup>*</sup> Alle `Dictionary<string,string>` sleutels zijn niet hoofdlettergevoelig.

#### <a name="response-object"></a>Responsobject

De antwoordobject dat u moet terugsturen is van het type `HttpResponseContext`, heeft de volgende eigenschappen:

| Eigenschap      | Description                                                 | Type                      |
|---------------|-------------------------------------------------------------|---------------------------|
| **`Body`**  | Een object dat de hoofdtekst van het antwoord bevat.           | object                    |
| **`ContentType`** | Een korte hand voor het instellen van het type inhoud voor het antwoord. | string                    |
| **`Headers`** | Een object met de antwoordheaders.               | Woordenlijst of hash-tabel   |
| **`StatusCode`**  | De HTTP-statuscode van het antwoord.                       | tekenreeks of int             |

#### <a name="accessing-the-request-and-response"></a>Toegang tot de aanvraag en respons

Wanneer u met HTTP-triggers werkt, kunt u de HTTP-aanvraag de dezelfde manier als met andere Invoerbinding openen. Het is in de `param` blokkeren.

Gebruik een `HttpResponseContext` object om terug te keren een antwoord, zoals wordt weergegeven in het volgende:

`function.json`

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "anonymous"
    },
    {
      "type": "http",
      "direction": "out"
    }
  ]
}
```

`run.ps1`

```powershell
param($req, $TriggerMetadata)

$name = $req.Query.Name

Push-OutputBinding -Name res -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "Hello $name!"
})
```

Het resultaat van het aanroepen van deze functie zijn:

```
PS > irm http://localhost:5001?Name=Functions
Hello Functions!
```

### <a name="type-casting-for-triggers-and-bindings"></a>Type-casten voor triggers en bindingen

Voor bepaalde bindingen, zoals de blob-binding, kunt u het type van de parameter op te geven.

Bijvoorbeeld, als u gegevens uit Blob storage is opgegeven als een tekenreeks, toevoegen de volgende typen cast-conversie uitvoeren naar mijn `param` blokkeren:

```powershell
param([string] $myBlob)
```

## <a name="powershell-profile"></a>PowerShell-profiel

In PowerShell wordt het concept van een PowerShell-profiel. Als u niet bekend met PowerShell-profielen bent, Zie [over profielen](/powershell/module/microsoft.powershell.core/about/about_profiles).

PowerShell-functies, het profiel script wordt uitgevoerd wanneer de functie-app wordt gestart. Functie-apps starten als eerste geïmplementeerd in de en na wordt inactief sinds ([koude start](#cold-start)).

Wanneer u een functie-app met behulp van hulpprogramma's, zoals Visual Studio Code en Azure Functions Core Tools, een standaard maakt `profile.ps1` wordt voor u gemaakt. Het standaardprofiel wordt bijgehouden [Core Tools GitHub-opslagplaats](https://github.com/Azure/azure-functions-core-tools/blob/dev/src/Azure.Functions.Cli/StaticResources/profile.ps1) en bevat:

* Automatische MSI-verificatie in Azure.
* De mogelijkheid om in te schakelen op de Azure PowerShell `AzureRM` PowerShell aliassen als u wilt.

## <a name="powershell-version"></a>PowerShell-versie

De volgende tabel ziet u de PowerShell-versie die wordt gebruikt door elke primaire versie van de runtime van Functions:

| Versie van de functies | PowerShell-versie                             |
|-------------------|------------------------------------------------|
| 1.x               | Windows PowerShell 5.1 (vergrendeld door de runtime) |
| 2.x               | PowerShell Core 6                              |

U kunt de huidige versie zien door te drukken `$PSVersionTable` van elke functie.

## <a name="dependency-management"></a>Beheer van afhankelijkheden

PowerShell-functies ondersteuning voor het beheren van Azure-modules door de service. Door de host.json wijzigen en de managedDependency ingeschakeld-eigenschap instelt op true, wordt het bestand requirements.psd1 worden verwerkt. De meest recente Azure-modules worden automatisch gedownload en beschikbaar gesteld aan de functie.

host.json
```json
{
    "managedDependency": {
        "enabled": true
    }
}
```

requirements.psd1

```powershell
@{
    Az = '1.*'
}
```

Gebruik te maken van uw eigen aangepaste modules of modules van de [PowerShell Gallery](https://powershellgallery.com) is enigszins anders dan hoe u het normaal zou doen.

Wanneer u de module op uw lokale computer installeert, de oplossing gaat in een van de algemeen beschikbare mappen in uw `$env:PSModulePath`. Omdat de functie wordt uitgevoerd in Azure, u geen toegang heeft tot de modules die op uw computer geïnstalleerd. Dit vereist dat de `$env:PSModulePath` voor een PowerShell-functie app wijkt af van `$env:PSModulePath` in een reguliere PowerShell-script.

In de functies, `PSModulePath` bevat twee paden:

* Een `Modules` map aanwezig is in de hoofdmap van uw functie-App.
* Een pad naar een `Modules` map die zich in de PowerShell-taal werknemer.

### <a name="function-app-level-modules-folder"></a>Op de functie app-niveau `Modules` map

Voor het gebruik van aangepaste modules of PowerShell-modules van de PowerShell Gallery, kunt u plaatsen van modules die uw functies afhankelijk zijn in een `Modules` map. Vanuit deze map worden modules automatisch beschikbaar voor de functions-runtime. Een functie in de functie-app kunnen deze modules worden gebruikt.

Als u wilt profiteren van deze functie, maak een `Modules` map in de hoofdmap van uw functie-app. De modules die u wilt gebruiken in uw functies in deze locatie opslaan.

```powershell
mkdir ./Modules
Save-Module MyGalleryModule -Path ./Modules
```

Gebruik `Save-Module` opslaan van alle modules in uw functies gebruiken of uw eigen aangepaste modules te kopiëren de `Modules` map. Uw functie-app moet de volgende mapstructuur hebben met een map Modules:

```
PSFunctionApp
 | - MyFunction
 | | - run.ps1
 | | - function.json
 | - Modules
 | | - MyGalleryModule
 | | - MyOtherGalleryModule
 | | - MyCustomModule.psm1
 | - local.settings.json
 | - host.json
```

Wanneer u uw functie-app start, de PowerShell-taal werknemer voegt dit `Modules` map die u wilt de `$env:PSModulePath` zodat u vertrouwen op de module autoloading, kunt net zoals u zou in een reguliere PowerShell-script doen.

### <a name="language-worker-level-modules-folder"></a>Taal worker niveau `Modules` map

Meerdere modules worden vaak gebruikt door de werkrol PowerShell-taal. Deze modules zijn gedefinieerd in de laatste positie van `PSModulePath`. 

De huidige lijst met modules is als volgt:

* [Microsoft.PowerShell.Archive](https://www.powershellgallery.com/packages/Microsoft.PowerShell.Archive): module gebruikt voor het werken met archieven, zoals `.zip`, `.nupkg`, enzovoort.
* **ThreadJob**: Een implementatie op basis van een thread van de taak PowerShell API's.

De meest recente versie van deze modules wordt gebruikt door de functies. Voor het gebruik van een specifieke versie van deze modules kunt u de specifieke versie opnemen in de `Modules` map van uw functie-app.

## <a name="environment-variables"></a>Omgevingsvariabelen

In de functies, [app-instellingen](functions-app-settings.md), zoals serviceverbinding tekenreeksen, worden weergegeven als omgevingsvariabelen tijdens de uitvoering. U hebt toegang tot deze instellingen met behulp van `$env:NAME_OF_ENV_VAR`, zoals wordt weergegeven in het volgende voorbeeld:

```powershell
param($myTimer)

Write-Host "PowerShell timer trigger function ran! $(Get-Date)"
Write-Host $env:AzureWebJobsStorage
Write-Host $env:WEBSITE_SITE_NAME
```

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

Bij lokale uitvoering, app-instellingen worden gelezen uit de [local.settings.json](functions-run-local.md#local-settings-file) projectbestand.

## <a name="concurrency"></a>Gelijktijdigheid

De runtime van Functions PowerShell standaard kan alleen één aanroep van een functie tegelijk verwerken. Maar dit niveau gelijktijdigheid van taken mogelijk niet voldoende in de volgende situaties:

* Wanneer u probeert om af te handelen van een groot aantal aanroepen op hetzelfde moment.
* Wanneer u hebt functies die andere functies in dezelfde functie-app aanroepen.

U kunt dit gedrag wijzigen door de volgende omgevingsvariabele in te stellen op een geheel getal:

```
PSWorkerInProcConcurrencyUpperBound
```

U deze omgevingsvariabele instellen in de [app-instellingen](functions-app-settings.md) van uw functie-App.

### <a name="considerations-for-using-concurrency"></a>Overwegingen voor het gebruik van gelijktijdigheid

PowerShell is een _één thread_ scripttaal standaard. Gelijktijdigheid kan echter worden toegevoegd met behulp van meerdere PowerShell runspaces in hetzelfde proces. Deze functie is de werking van de PowerShell voor Azure Functions-runtime.

Er zijn enkele nadelen met deze methode.

#### <a name="concurrency-is-only-as-good-as-the-machine-its-running-on"></a>Gelijktijdigheid is alleen nuttig als de machine waarop het wordt uitgevoerd

Als uw functie-app wordt uitgevoerd op een [App Service-plan](functions-scale.md#app-service-plan) die alleen ondersteuning biedt voor een enkele kern, en vervolgens gelijktijdigheid wordt niet te veel. Dat komt doordat er zijn geen extra cores te verdelen over de belasting. Prestaties kan in dit geval verschillen wanneer één kern heeft met context-switch tussen runspaces genoemd.

De [verbruiksabonnement](functions-scale.md#consumption-plan) wordt uitgevoerd met behulp van slechts één core, zodat u kunt geen gebruikmaken van gelijktijdigheid. Als u profiteren van gelijktijdigheid van taken wilt, in plaats daarvan implementeert uw functies aan een functie-app die wordt uitgevoerd op een toegewezen App Service-plan met voldoende kernen.

#### <a name="azure-powershell-state"></a>De status van Azure PowerShell

Azure PowerShell maakt gebruik van sommige _procesniveau_ context en status om te besparen u overtollige typen. Echter, als u gelijktijdigheid in uw functie-app inschakelen en acties die in een statuswijziging aanroepen, u den racevoorwaarden. Deze racevoorwaarden zijn moeilijk op te lossen omdat één aanroep is afhankelijk van een bepaalde status en het andere aanroepen heeft de status gewijzigd.

Er is gigantische waarde in gelijktijdigheid met Azure PowerShell, omdat bepaalde bewerkingen kunnen een aanzienlijke hoeveelheid tijd duren. U moet echter doorgaan met de waarschuwing. Als u vermoedt dat u een zeldzame situatie ondervindt, stel de waarde voor concurrency terug naar `1` en probeer de aanvraag opnieuw.

## <a name="configure-function-scriptfile"></a>Functie configureren `scriptFile`

Een PowerShell-functie wordt standaard uitgevoerd vanuit `run.ps1`, een bestand met dezelfde bovenliggende map als het bijbehorende gedeelde `function.json`.

De `scriptFile` eigenschap in de `function.json` kan worden gebruikt om op te halen van een mapstructuur die lijkt op het volgende voorbeeld:

```
FunctionApp
 | - host.json
 | - myFunction
 | | - function.json
 | - lib
 | | - PSFunction.ps1
```

In dit geval de `function.json` voor `myFunction` bevat een `scriptFile` eigenschap verwijst naar het bestand met de geëxporteerde functie om uit te voeren.

```json
{
  "scriptFile": "../lib/PSFunction.ps1",
  "bindings": [
    // ...
  ]
}
```

## <a name="use-powershell-modules-by-configuring-an-entrypoint"></a>PowerShell-modules gebruiken bij het configureren van een ingangspunt

In dit artikel heeft laten zien van PowerShell-functies in de standaard `run.ps1` scriptbestand die worden gegenereerd door de sjablonen.
U kunt echter ook uw functies opnemen in de PowerShell-modules. U kunt verwijzen naar de code van uw specifieke functie in de module met behulp van de `scriptFile` en `entryPoint` velden in de function.json' configuratiebestand.

In dit geval `entryPoint` is de naam van een functie of cmdlet in de PowerShell-module waarnaar wordt verwezen in `scriptFile`.

Houd rekening met de volgende mapstructuur:

```
FunctionApp
 | - host.json
 | - myFunction
 | | - function.json
 | - lib
 | | - PSFunction.psm1
```

Waar `PSFunction.psm1` bevat:

```powershell
function Invoke-PSTestFunc {
    param($InputBinding, $TriggerMetadata)

    Push-OutputBinding -Name OutputBinding -Value "output"
}

Export-ModuleMember -Function "Invoke-PSTestFunc"
```

In dit voorbeeld wordt de configuratie voor `myFunction` bevat een `scriptFile` eigenschap die verwijst naar `PSFunction.psm1`, dit is een PowerShell-module in een andere map.  De `entryPoint` eigenschap verwijst naar de `Invoke-PSTestFunc` functie, die het toegangspunt in de module is.

```json
{
  "scriptFile": "../lib/PSFunction.psm1",
  "entryPoint": "Invoke-PSTestFunc",
  "bindings": [
    // ...
  ]
}
```

Met deze configuratie de `Invoke-PSTestFunc` wordt uitgevoerd als een `run.ps1` zou.

## <a name="considerations-for-powershell-functions"></a>Overwegingen voor het PowerShell-functies

Wanneer u met PowerShell-functies werkt, worden op de hoogte van de overwegingen met betrekking tot in de volgende secties.

### <a name="cold-start"></a>Koude Start

Bij het ontwikkelen van Azure Functions in de [serverloze hostingmodel](functions-scale.md#consumption-plan), koude starts worden realiteit. *Koude start* verwijst naar bepaalde tijd het duurt voor uw functie-app wilt uitvoeren om een aanvraag te verwerken. Koude start gebeurt vaak in meer het verbruik van plan bent omdat uw functie-app wordt afgesloten tijdens perioden van inactiviteit.

### <a name="bundle-modules-instead-of-using-install-module"></a>Bundel modules in plaats van `Install-Module`

Het script wordt uitgevoerd op elke aanroep. Vermijd het gebruik van `Install-Module` in uw script. Gebruik in plaats daarvan `Save-Module` vóór publicatie, zodat uw functie niet kwijt aan het downloaden van de module. Als koude starts van invloed zijn op uw functies, kunt u overwegen uw functie-app implementeren een [App Service-plan](functions-scale.md#app-service-plan) ingesteld op *AlwaysOn* of naar een [Premium-abonnement](functions-scale.md#premium-plan).

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

* [Aanbevolen procedures voor Azure Functions](functions-best-practices.md)
* [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)
* [Azure Functions-triggers en bindingen](functions-triggers-bindings.md)

[naslaginformatie voor host.JSON]: functions-host-json.md

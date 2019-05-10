---
title: Zelfstudie voor het gebruik van de functie vlaggen in een .NET Core-app | Microsoft Docs
description: In deze zelfstudie leert u hoe u functie-vlaggen in .NET Core-apps implementeren
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 04/19/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 28ba4397ca5a5fd3c281555238fc7eec8a82943d
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65413674"
---
# <a name="tutorial-use-feature-flags-in-a-net-core-app"></a>Zelfstudie: Functievlaggen gebruiken in een .NET Core-app

De .NET Core functie Management-bibliotheken bieden idiomatisch ondersteuning voor het implementeren van functie-vlaggen in een .NET- of ASP.NET Core-toepassing. Hiermee kunt u om toe te voegen functie vlaggen in uw code meer declaratief zodat u niet hoeft al te schrijven de `if` -instructies voor deze handmatig. Deze functie vlag levenscycli (bijvoorbeeld, vernieuwen en cache vlag statussen, een vlag status onveranderbaar tijdens het aanroepen van aanvraag garanderen) beheren achter de scène. De ASP.NET Core-bibliotheek biedt bovendien out-of-the-box-integraties met inbegrip van MVC-controller acties, weergaven, routes en middleware.

De [functie vlaggen toevoegen aan een ASP.NET Core-app](./quickstart-feature-flag-aspnet-core.md) Quick Start ziet u een aantal manieren om toe te voegen functie vlaggen in een ASP.NET Core-toepassing. Deze zelfstudie wordt uitgelegd in meer informatie. Zie de [documentatie over het beheer van het ASP.NET Core-functie](https://go.microsoft.com/fwlink/?linkid=2091410) voor een volledig overzicht.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Functie-vlaggen in belangrijke onderdelen van uw toepassing voor het beheren van de beschikbaarheid van functies toevoegen.
> * Integreren met App-configuratie bij het gebruik van deze functie vlaggen beheren.

## <a name="setup"></a>Installatie

De manager van de functie .NET Core `IFeatureManager` functie vlaggen opgehaald uit de systeemeigen configuratiesysteem van het framework. Als gevolg hiervan kunt u definiëren van uw toepassing functie vlaggen met behulp van een configuratiebron die .NET Core wordt ondersteund, met inbegrip van de lokale *appsettings.json* bestand of de omgeving variabelen. Functie manager vertrouwt op afhankelijkheidsinjectie .NET Core. U kunt de functie management-services met behulp van standaard conventies registreren.

```csharp
using Microsoft.FeatureManagement;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement();
    }
}
```

De functie manager Hiermee haalt u vlaggen van de functie in het gedeelte 'FeatureManagement' van de .NET Core-configuratiegegevens standaard. Het volgende voorbeeld wordt het lezen van een andere sectie in plaats daarvan 'MyFeatureFlags' genoemd.

```csharp
using Microsoft.FeatureManagement;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement(options =>
        {
                options.UseConfiguration(Configuration.GetSection("MyFeatureFlags"));
        });
    }
}
```

Als u een filter in uw functie-vlaggen gebruiken, moet u omvatten een extra bibliotheek en deze te registreren. Het volgende voorbeeld ziet u hoe u een ingebouwde functie filter met de naam **PercentageFilter "**.

```csharp
using Microsoft.FeatureManagement;
using Microsoft.FeatureManagement.FeatureFilters;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement()
                .AddFeatureFilter<PercentageFilter>();
    }
}
```

Voor het goed functioneren, moet u functie vlaggen buiten de toepassing behouden en ze afzonderlijk beheren. In dat geval kunt u de vlag statussen op elk gewenst moment wijzigen en de wijzigingen die van kracht in de toepassing onmiddellijk. App-configuratie biedt een centrale locatie voor het organiseren en beheren van alle uw functie via een speciale portal UI worden gemarkeerd en levert de vlaggen aan uw toepassing rechtstreeks via de .NET Core-client bibliotheken. De eenvoudigste manier om uw ASP.NET Core-App met App-configuratie verbinding te maken is door de configuratieprovider `Microsoft.Extensions.Configuration.AzureAppConfiguration`. U kunt dit NuGet-pakket in uw code door toe te voegen van de volgende de *Program.cs* bestand:

```csharp
using Microsoft.Extensions.Configuration.AzureAppConfiguration;

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
           .ConfigureAppConfiguration((hostingContext, config) => {
               var settings = config.Build();
               config.AddAzureAppConfiguration(options => {
                   options.Connect(settings["ConnectionStrings:AppConfig"])
                          .UseFeatureFlags();
                });
           })
           .UseStartup<Startup>();
```

Functie vlagwaarden wordt verwacht dat het verloop van tijd veranderen. Standaard worden de functie manager vernieuwd vlagwaarden functie elke 30 seconden. U kunt een andere polling-interval in de `options.UseFeatureFlags()` aanroep hierboven.

```csharp
config.AddAzureAppConfiguration(options => {
    options.Connect(settings["ConnectionStrings:AppConfig"])
           .UseFeatureFlags(featureFlagOptions => {
                featureFlagOptions.PollInterval = TimeSpan.FromSeconds(5);
           });
});
```

## <a name="feature-flag-declaration"></a>Functie vlag declaratie

Elke de functievlag bestaat uit twee delen: een naam en een lijst met een of meer filters die worden gebruikt om te evalueren of de status van een onderdeel is *op* (dat wil zeggen, wanneer de waarde ervan is `True`). Een filter definieert een gebruiksvoorbeeld waarvoor een functie moet worden ingeschakeld. Als een functievlag meerdere filters heeft, wordt de lijst met filters in volgorde doorlopen totdat een van de filters bepaalt dat de functie moet worden ingeschakeld. Op dit moment functievlag wordt beschouwd als *op* en alle overige resultaten van het filter zijn overgeslagen. Als er is geen filter geeft aan dat de functie moet worden ingeschakeld, is het functievlag *uit*.

De functie manager ondersteunt *appsettings.json* als een configuratiebron voor de functie vlaggen. Het volgende voorbeeld ziet hoe u functie-vlaggen in een json-bestand instelt.

```JSON
"FeatureManagement": {
    "FeatureX": true, // Feature flag set to on
    "FeatureY": false, // Feature flag set to off
    "FeatureC": {
        "EnabledFor": [
            {
                "Name": "Percentage",
                "Parameters": {
                    "Value": 50
                }
            }
        ]
    }
}
```

Volgens de conventies wordt de `FeatureManagement` sectie van dit json-document wordt gebruikt voor instellingen voor apparaatfuncties vlag. Het bovenstaande voorbeeld ziet u drie functie vlaggen met hun filters zijn gedefinieerd de *EnabledFor* eigenschap:

* **FeatureA** is *op*.
* **FeatureB** is *uit*.
* **FeatureC** Hiermee geeft u een filter met de naam *Percentage* met een *Parameters* eigenschap. *Percentage* volgt een voorbeeld van een configureerbaar filter instellen en de kans op 50% voor de **FeatureC** vlag *op*.

## <a name="referencing"></a>Verwijst naar

Hoewel dit niet vereist, functie vlaggen moeten worden gedefinieerd als `enum` variabelen zodat ze gemakkelijk in code kunnen worden verwezen.

```csharp
public enum MyFeatureFlags
{
    FeatureA,
    FeatureB,
    FeatureC
}
```

## <a name="feature-flag-check"></a>Functie vlag-controle

Het patroon van het beheer van de functie is om eerst te controleren als een functievlag is ingesteld op *op* en vervolgens de ingesloten acties uitvoeren als dit het geval is.

```csharp
IFeatureManager featureManager;
...
if (featureManager.IsEnabled(nameof(MyFeatureFlags.FeatureA)))
{
    // Run the following code
}
```

## <a name="dependency-injection"></a>Afhankelijkheidsinjectie

In de ASP.NET Core MVC, de functie manager `IFeatureManager` is toegankelijk via afhankelijkheidsinjectie.

```csharp
public class HomeController : Controller
{
    private readonly IFeatureManager _featureManager;

    public HomeController(IFeatureManager featureManager)
    {
        _featureManager = featureManager;
    }
}
```

## <a name="controller-action"></a>Controller-actie

In MVC-controllers, een `Feature` kenmerk kan worden gebruikt om te bepalen of een hele controllerklasse of een specifieke actie is ingeschakeld. De volgende `HomeController` controller vereist *FeatureA* moet *op* voordat een actie die deze bevat kan worden uitgevoerd.

```csharp
[Feature(MyFeatureFlags.FeatureA)]
public class HomeController : Controller
{
    ...
}
```

De volgende `Index` bovenstaande-actie vereist *FeatureA* moet *op* voordat het kan worden uitgevoerd.

```csharp
[Feature(MyFeatureFlags.FeatureA)]
public IActionResult Index()
{
    return View();
}
```

Wanneer een MVC-controller of de actie is geblokkeerd omdat de controle over functievlag *uit*, een geregistreerde `IDisabledFeatureHandler` wordt genoemd. De standaardwaarde `IDisabledFeatureHandler` een 404-statuscode geretourneerd naar de client zonder antwoordtekst.

## <a name="view"></a>Weergeven

In weergaven, MVC, een `<feature>` tag kan worden gebruikt om inhoud op basis van of een functievlag is ingeschakeld of niet weer te geven.

```html
<feature name="FeatureA">
    <p>This can only be seen if 'FeatureA' is enabled.</p>
</feature>
```

## <a name="mvc-filter"></a>MVC-filter

MVC-filters kunnen worden ingesteld dat ze worden geactiveerd op basis van de status van een functievlag. Het volgende voorbeeld wordt een MVC-filter met de naam `SomeMvcFilter`. Dit filter wordt geactiveerd in de pijplijn MVC alleen als *FeatureA* is ingeschakeld.

```csharp
using Microsoft.FeatureManagement.FeatureFilters;

IConfiguration Configuration { get; set;}

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc(options => {
        options.Filters.AddForFeature<SomeMvcFilter>(nameof(MyFeatureFlags.FeatureA));
    });
}
```

## <a name="route"></a>Route

Routes kunnen dynamisch op basis van functie vlaggen worden blootgesteld. Het volgende voorbeeld wordt een route, waarmee wordt ingesteld `Beta` als de standaard-controller, alleen wanneer *FeatureA* is ingeschakeld.

```csharp
app.UseMvc(routes => {
    routes.MapRouteForFeature(nameof(MyFeatureFlags.FeatureA), "betaDefault", "{controller=Beta}/{action=Index}/{id?}");
});
```

## <a name="middleware"></a>Middleware

Vlaggen voor de functie kunnen worden gebruikt om toe te voegen vertakkingen van toepassingen en middleware voorwaardelijk. De volgende invoegingen middleware onderdeel van de aanvraag voor pipeline-alleen wanneer *FeatureA* is ingeschakeld.

```csharp
app.UseMiddlewareForFeature<ThirdPartyMiddleware>(nameof(MyFeatureFlags.FeatureA));
```

U maakt uit de meer algemene mogelijkheden voor de hele toepassing op basis van een functievlag vertakking.

```csharp
app.UseForFeature(featureName, appBuilder => {
    appBuilder.UseMiddleware<T>();
});
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u functie vlaggen in uw ASP.NET Core-toepassing implementeert door gebruik te maken de `Microsoft.FeatureManagement` bibliotheken. Zie de volgende bronnen voor meer informatie over ondersteuning voor het beheer van functies in ASP.NET Core- en App-configuratie.

* [Functievlag voor ASP.NET Core-voorbeeldcode]()
* [Microsoft.FeatureManagement-documentatie]()
* [Functievlaggen beheren](./manage-feature-flags.md)

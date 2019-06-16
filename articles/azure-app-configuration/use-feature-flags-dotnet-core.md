---
title: Zelfstudie voor het gebruik van de functie vlaggen in een .NET Core-app | Microsoft Docs
description: In deze zelfstudie leert u hoe u functie-vlaggen in .NET Core-apps implementeert.
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
ms.openlocfilehash: 577cb55ce381976a6d623b272b920d0d1bf2eeb9
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67144001"
---
# <a name="tutorial-use-feature-flags-in-an-aspnet-core-app"></a>Zelfstudie: Functie vlaggen gebruiken in een ASP.NET Core-app

De .NET Core functie Management-bibliotheken bieden idiomatisch ondersteuning voor het implementeren van functie-vlaggen in een .NET- of ASP.NET Core-toepassing. Deze bibliotheken kunnen u declaratief functie vlaggen toevoegen aan uw code, zodat u hoeft te schrijven alle de `if` -instructies voor deze handmatig.

De functie Management-bibliotheken ook beheren, levenscycli van de functie vlag achter de schermen. Bijvoorbeeld, de bibliotheken vernieuwen vlaggenstaten in de cache en garanderen van een vlag status onveranderbaar tijdens het aanroepen van de aanvraag. De ASP.NET Core-bibliotheek biedt bovendien out-of-the-box-integraties, met inbegrip van MVC-controller acties, weergaven, routes en middleware.

De [functie vlaggen toevoegen aan een ASP.NET Core-app Quickstart](./quickstart-feature-flag-aspnet-core.md) ziet u enkele manieren om toe te voegen functie vlaggen in een ASP.NET Core-toepassing. In deze zelfstudie wordt uitgelegd voor meer informatie over deze methoden. Zie voor een volledig overzicht de [documentatie over het beheer van het ASP.NET Core-functie](https://go.microsoft.com/fwlink/?linkid=2091410).

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Functie-vlaggen in belangrijke onderdelen van uw toepassing voor het beheren van de beschikbaarheid van functies toevoegen.
> * Integreren met App-configuratie wanneer u gebruikt dit voor het beheren van de functie vlaggen.

## <a name="set-up-feature-management"></a>Functie-beheer instellen

De manager van de functie .NET Core `IFeatureManager` functie vlaggen opgehaald uit de systeemeigen configuratiesysteem van het framework. Als gevolg hiervan kunt u van uw toepassing functie vlaggen definiëren met behulp van een configuratiebron die .NET Core wordt ondersteund, met inbegrip van de lokale *appsettings.json* bestand of de omgeving variabelen. `IFeatureManager` is gebaseerd op .NET Core-afhankelijkheidsinjectie. U kunt de functie management-services registreren met behulp van standaard conventies:

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

Standaard haalt de functie manager functie vlaggen uit de `"FeatureManagement"` sectie van de .NET Core-configuratiegegevens. Het volgende voorbeeld wordt de functie manager om te lezen uit een andere sectie met de naam `"MyFeatureFlags"` in plaats daarvan:

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

Als u filters in uw functie-vlaggen gebruiken, moet u een extra bibliotheek opnemen en deze te registreren. Het volgende voorbeeld ziet u hoe u een ingebouwde functie filter met de naam `PercentageFilter`:

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

U wordt aangeraden dat u functie-vlaggen buiten de toepassing behouden en ze afzonderlijk beheren. In dat geval kunt u de vlag statussen op elk gewenst moment wijzigen en de wijzigingen in de toepassing onmiddellijk van kracht. App-configuratie biedt een centrale locatie voor het organiseren en beheren van uw functie-vlaggen via een speciale portal-gebruikersinterface. App-configuratie zorgt ook voor de vlaggen voor uw toepassing rechtstreeks via de .NET Core-client bibliotheken.

De eenvoudigste manier om uw ASP.NET Core-App met App-configuratie verbinding te maken is door de configuratieprovider `Microsoft.Extensions.Configuration.AzureAppConfiguration`. Voor het gebruik van dit NuGet-pakket, voeg de volgende code aan de *Program.cs* bestand:

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

Functie vlagwaarden wordt verwacht dat het verloop van tijd veranderen. De functie manager vernieuwt vlagwaarden functie elke 30 seconden standaard. De volgende code laat zien hoe u het polling-interval wijzigen op 5 seconden in de `options.UseFeatureFlags()` aanroepen:

```csharp
config.AddAzureAppConfiguration(options => {
    options.Connect(settings["ConnectionStrings:AppConfig"])
           .UseFeatureFlags(featureFlagOptions => {
                featureFlagOptions.PollInterval = TimeSpan.FromSeconds(300);
           });
});
```

## <a name="feature-flag-declaration"></a>Functie vlag declaratie

Elke de functievlag bestaat uit twee delen: een naam en een lijst met een of meer filters die worden gebruikt om te evalueren of de status van een onderdeel is *op* (dat wil zeggen, wanneer de waarde ervan is `True`). Een filter definieert een use-case voor wanneer een functie moet worden ingeschakeld.

Wanneer een functievlag meerdere filters heeft, wordt de lijst met filters in volgorde doorlopen totdat een van de filters bepaalt dat de functie moet worden ingeschakeld. Op dat moment de functievlag is *op*, en alle overige resultaten van het filter zijn overgeslagen. Als er is geen filter geeft aan de functie moet worden ingeschakeld dat, is het functievlag *uit*.

De functie manager ondersteunt *appsettings.json* als een configuratiebron voor de functie vlaggen. Het volgende voorbeeld laat zien hoe het instellen van de functie markeringen in een JSON-bestand:

```JSON
"FeatureManagement": {
    "FeatureA": true, // Feature flag set to on
    "FeatureB": false, // Feature flag set to off
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

Volgens de conventies wordt de `FeatureManagement` sectie van dit JSON-document wordt gebruikt voor instellingen voor apparaatfuncties vlag. Het vorige voorbeeld ziet u drie functie vlaggen met hun filters zijn gedefinieerd de `EnabledFor` eigenschap:

* `FeatureA` is *op*.
* `FeatureB` is *uit*.
* `FeatureC` Hiermee geeft u een filter met de naam `Percentage` met een `Parameters` eigenschap. `Percentage` is een configureerbare filter. In dit voorbeeld `Percentage` Hiermee geeft u de kans op 50 procent van de `FeatureC` vlag *op*.

## <a name="feature-flag-references"></a>Functie vlag verwijzingen

Zodat u eenvoudig naar functie markeringen in code verwijst kunt, moet u deze als definiëren `enum` variabelen:

```csharp
public enum MyFeatureFlags
{
    FeatureA,
    FeatureB,
    FeatureC
}
```

## <a name="feature-flag-checks"></a>Functie Vlagcontroles

Het patroon van het beheer van de functie is om eerst te controleren als een functievlag is ingesteld op *op*. Als u dus de functie manager voert de acties die wordt de functie bevat. Bijvoorbeeld:

```csharp
IFeatureManager featureManager;
...
if (featureManager.IsEnabled(nameof(MyFeatureFlags.FeatureA)))
{
    // Run the following code
}
```

## <a name="dependency-injection"></a>Afhankelijkheidsinjectie

In de ASP.NET Core MVC, opent u de functie manager `IFeatureManager` via afhankelijkheidsinjectie:

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

## <a name="controller-actions"></a>Controller-acties

In MVC-controllers, gebruikt u de `Feature` kenmerk om te bepalen of een hele controllerklasse of een specifieke actie is ingeschakeld. De volgende `HomeController` controller vereist `FeatureA` moet *op* voordat een actie die de controllerklasse bevat kan worden uitgevoerd:

```csharp
[Feature(MyFeatureFlags.FeatureA)]
public class HomeController : Controller
{
    ...
}
```

De volgende `Index` -actie vereist `FeatureA` moet *op* voordat het kan worden uitgevoerd:

```csharp
[Feature(MyFeatureFlags.FeatureA)]
public IActionResult Index()
{
    return View();
}
```

Wanneer een MVC-controller of de actie is geblokkeerd omdat de controle over functievlag *uit*, een geregistreerde `IDisabledFeaturesHandler` interface wordt genoemd. De standaardwaarde `IDisabledFeaturesHandler` interface een 404-statuscode geretourneerd naar de client zonder antwoordtekst.

## <a name="mvc-views"></a>MVC-weergaven

In MVC weergaven, kunt u een `<feature>` tag om inhoud weer te geven op basis van het of een functievlag is ingeschakeld:

```html
<feature name="FeatureA">
    <p>This can only be seen if 'FeatureA' is enabled.</p>
</feature>
```

## <a name="mvc-filters"></a>MVC-filters

U kunt MVC filters instellen zodat deze zijn geactiveerd op basis van de status van een functievlag. De volgende code wordt een MVC-filter met de naam toegevoegd `SomeMvcFilter`. Dit filter wordt geactiveerd in de pijplijn MVC alleen als `FeatureA` is ingeschakeld.

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

## <a name="routes"></a>Routes

Vlaggen voor functie kunt u dynamisch routes beschikbaar te maken. De volgende code wordt een route, waarmee wordt ingesteld `Beta` als de standaard-controller alleen wanneer `FeatureA` is ingeschakeld:

```csharp
app.UseMvc(routes => {
    routes.MapRouteForFeature(nameof(MyFeatureFlags.FeatureA), "betaDefault", "{controller=Beta}/{action=Index}/{id?}");
});
```

## <a name="middleware"></a>Middleware

U kunt ook de functie vlaggen gebruiken om toe te voegen voorwaardelijk vertakkingen van toepassingen en middleware worden uitgevoerd. De volgende code voegt een onderdeel middleware in de aanvraag pijplijn alleen wanneer `FeatureA` is ingeschakeld:

```csharp
app.UseMiddlewareForFeature<ThirdPartyMiddleware>(nameof(MyFeatureFlags.FeatureA));
```

Deze code bouwt voort uit de mogelijkheid meer-algemene voor de hele toepassing op basis van een functievlag vertakking:

```csharp
app.UseForFeature(featureName, appBuilder => {
    appBuilder.UseMiddleware<T>();
});
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u functie-vlaggen in uw ASP.NET Core-toepassing implementeren met behulp van de `Microsoft.FeatureManagement` bibliotheken. Zie de volgende bronnen voor meer informatie over ondersteuning voor het beheer van functies in ASP.NET Core- en App-configuratie:

* [Functievlag voor ASP.NET Core-voorbeeldcode](/azure/azure-app-configuration/quickstart-feature-flag-aspnet-core)
* [Microsoft.FeatureManagement-documentatie](https://docs.microsoft.com/dotnet/api/microsoft.featuremanagement)
* [Functievlaggen beheren](./manage-feature-flags.md)

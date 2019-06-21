---
title: Afhankelijkheidsinjectie gebruiken in .NET Azure Functions
description: Informatie over het gebruik van afhankelijkheidsinjectie voor het registreren en gebruiken van services in de .NET-functies
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: Azure functions, functions, serverloze architectuur
ms.service: azure-functions
ms.devlang: dotnet
ms.topic: reference
ms.date: 05/28/2019
ms.author: jehollan, cshoe
ms.openlocfilehash: 9f932bf92cb3871af7f0eb294ac15dec82cdc8ba
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274242"
---
# <a name="use-dependency-injection-in-net-azure-functions"></a>Afhankelijkheidsinjectie gebruiken in .NET Azure Functions

Azure Functions biedt ondersteuning voor de afhankelijkheid injectie (DI) software ontwerppatroon, dit is een techniek te bereiken [tekenomkering van het besturingselement (IoC)](https://docs.microsoft.com/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) tussen klassen en de bijbehorende afhankelijkheden.

Azure Functions voortbouwt op de functies van ASP.NET Core-Afhankelijkheidsinjectie. Wordt op de hoogte van services, levensduur en ontwerppatronen van [ASP.NET Core-afhankelijkheidsinjectie](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) voordat u DI-functies in een Azure-Functions app wordt aanbevolen.

Ondersteuning voor afhankelijkheidsinjectie met Azure Functions begint 2.x.

## <a name="prerequisites"></a>Vereisten

Voordat u afhankelijkheidsinjectie gebruiken kunt, moet u de volgende NuGet-pakketten installeren:

- [Microsoft.Azure.Functions.Extensions](https://www.nuget.org/packages/Microsoft.Azure.Functions.Extensions/)

- [Pakket Microsoft.NET.Sdk.Functions](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions/) versie 1.0.28 of hoger

- Optioneel: [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) alleen voor het registreren van httpclient maakt bij het opstarten is vereist

## <a name="register-services"></a>Services registreren

Voor het registreren van services, kunt u een methode voor het configureren en het toevoegen van onderdelen voor het maken een `IFunctionsHostBuilder` exemplaar.  De host van de Azure Functions maakt een exemplaar van `IFunctionsHostBuilder` en geeft deze rechtstreeks in de methode.

Voor het registreren van de methode, voeg de `FunctionsStartup` assembly-kenmerk met de typenaam gebruikt tijdens het opstarten. Ook code verwijst naar een voorlopige versie van [Microsoft.Azure.Cosmos](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/) op Nuget.

```csharp
using System;
using Microsoft.Azure.Functions.Extensions.DependencyInjection;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Http;
using Microsoft.Extensions.Logging;
using Microsoft.Azure.Cosmos;

[assembly: FunctionsStartup(typeof(MyNamespace.Startup))]

namespace MyNamespace
{
    public class Startup : FunctionsStartup
    {
        public override void Configure(IFunctionsHostBuilder builder)
        {
            builder.Services.AddHttpClient();
            builder.Services.AddSingleton((s) => {
                return new CosmosClient(Environment.GetEnvironmentVariable("COSMOSDB_CONNECTIONSTRING"));
            });
            builder.Services.AddSingleton<ILoggerProvider, MyLoggerProvider>();
        }
    }
}
```

## <a name="use-injected-dependencies"></a>Gebruik van geïnjecteerde afhankelijkheden

ASP.NET Core gebruikt constructor injectie om de afhankelijkheden beschikbaar zijn voor uw functie te maken. Het volgende voorbeeld laat zien hoe de `IMyService` en `HttpClient` afhankelijkheden zijn opgenomen in een HTTP-geactiveerde functie.

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;

namespace MyNamespace
{
    public class HttpTrigger
    {
        private readonly IMyService _service;
        private readonly HttpClient _client;

        public HttpTrigger(IMyService service, IHttpClientFactory httpClientFactory)
        {
            _service = service;
            _client = httpClientFactory.CreateClient();;
        }

        [FunctionName("GetPosts")]
        public async Task<IActionResult> Get(
            [HttpTrigger(AuthorizationLevel.Function, "get", Route = "posts")] HttpRequest req,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            var res = await _client.GetAsync("https://microsoft.com");
            await _service.AddResponse(res);

            return new OkResult();
        }
    }
}
```

Het gebruik van constructor injectie betekent dat u statische functies niet gebruiken te als u wilt profiteren van afhankelijkheidsinjectie.

## <a name="service-lifetimes"></a>Service-levensduur

Azure Functions-apps bieden de levensduur van de dezelfde service als [ASP.NET Afhankelijkheidsinjectie](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes): tijdelijke, binnen het bereik, en singleton.

In een Azure functions-app, de levensduur van een bereik service komt overeen met de levensduur van een functie-uitvoering. Services binnen het bereik worden één keer per uitvoering worden gemaakt. Aanvragen voor die service tijdens de uitvoering opnieuw gebruiken van het bestaande service-exemplaar. De levensduur van een singleton-service komt overeen met de levensduur van de host en wordt hergebruikt voor functie-uitvoeringen op dat exemplaar.

Singleton-levensduur-services worden aanbevolen voor verbindingen en -clients, bijvoorbeeld `SqlConnection`, `CloudBlobClient`, of `HttpClient` exemplaren.

Weergeven of downloaden een [steekproef van de levensduur van de andere service](https://aka.ms/functions/di-sample) op GitHub.

## <a name="logging-services"></a>Van logboekregistratieservices

Als u uw eigen provider voor logboekregistratie, de aanbevolen manier om u te registreren is een `ILoggerProvider` exemplaar. Application Insights wordt automatisch toegevoegd door Azure Functions.

> [!WARNING]
> Voeg geen `AddApplicationInsightsTelemetry()` aan de verzameling services als deze services wordt geregistreerd die strijdig zijn met services die worden geleverd door de omgeving.

## <a name="function-app-provided-services"></a>Functie-app opgegeven services

De functie host wordt veel services geregistreerd. De volgende services zijn veilig om op te nemen als een afhankelijkheid in uw toepassing:

|Servicetype|Levensduur|Description|
|--|--|--|
|`Microsoft.Extensions.Configuration.IConfiguration`|Singleton|Runtime-configuratie|
|`Microsoft.Azure.WebJobs.Host.Executors.IHostIdProvider`|Singleton|Verantwoordelijk voor het leveren van de ID van de instantie van de host|

Als er andere services die u maken van een afhankelijkheid zijn wilt op [een probleem maken en ze op GitHub voorstellen](https://github.com/azure/azure-functions-host).

### <a name="overriding-host-services"></a>Overschrijven hosten van services

Het overschrijven van de services die worden geleverd door de host wordt momenteel niet ondersteund.  Als er services die u onderdrukken zijn wilt, [een probleem maken en ze op GitHub voorstellen](https://github.com/azure/azure-functions-host).

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

- [Het bewaken van uw functie-app](functions-monitoring.md)
- [Aanbevolen procedures voor functions](functions-best-practices.md)

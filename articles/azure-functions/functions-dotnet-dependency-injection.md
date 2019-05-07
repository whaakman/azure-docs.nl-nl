---
title: Afhankelijkheidsinjectie gebruiken in .NET Azure Functions
description: Informatie over het gebruik van afhankelijkheidsinjectie voor het registreren en gebruiken van services in de .NET-functies
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure functions, functions, serverloze architectuur
ms.service: azure-functions
ms.devlang: dotnet
ms.topic: reference
ms.date: 03/22/2019
ms.author: jehollan
ms.openlocfilehash: 1ef688dff65dc11f875f76e2f9127bf89af2f2b9
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074588"
---
# <a name="use-dependency-injection-in-net-azure-functions"></a>Afhankelijkheidsinjectie gebruiken in .NET Azure Functions

Azure Functions biedt ondersteuning voor de afhankelijkheid injectie (DI) software ontwerppatroon, dit is een techniek voor het bereiken van [tekenomkering van het besturingselement (IoC)](https://docs.microsoft.com/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) tussen klassen en de bijbehorende afhankelijkheden.

Azure Functions voortbouwt op de functies van ASP.NET Core-Afhankelijkheidsinjectie.  U moet kennen services, levensduur en ontwerppatronen van [ASP.NET Core-afhankelijkheidsinjectie](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) voordat u ze in de functies.

## <a name="installing-dependency-injection-packages"></a>Afhankelijkheid injectie pakketten installeren

De afhankelijkheid injectie om functies te gebruiken, moet u het NuGet-pakket dat wordt aangegeven dat deze API's opnemen.

```powershell
Install-Package Microsoft.Azure.Functions.Extensions
```

## <a name="registering-services"></a>Registreren van services

Voor het registreren van services, kunt u een methode configureren maken en toevoegen van onderdelen naar een `IFunctionsHostBuilder` exemplaar.  Hiermee maakt u de Azure Functions-host een `IFunctionsHostBuilder` en geeft deze rechtstreeks in uw geconfigureerde methode.

Om u te registreren uw methode configureren, moet u een assembly-kenmerk dat Hiermee geeft u het type voor toevoegen uw configureren met behulp van methode de `FunctionsStartup` kenmerk.

```csharp
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

## <a name="service-lifetimes"></a>Service-levensduur

Azure functie-apps bieden de levensduur van de dezelfde service als [ASP.NET Afhankelijkheidsinjectie](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes), tijdelijke, binnen het bereik, en singleton.

De levensduur van een service binnen het bereik in een functie-app komt overeen met de levensduur van een functie-uitvoering. Services binnen het bereik worden één keer per uitvoering worden gemaakt.  Aanvragen voor die service tijdens de uitvoering opnieuw dat exemplaar gebruiken.  De levensduur van een singleton-service komt overeen met de levensduur van de host en wordt hergebruikt voor functie-uitvoeringen op dat exemplaar.

Singleton-levensduur-services worden aanbevolen voor verbindingen en -clients, bijvoorbeeld een `SqlConnection`, `CloudBlobClient`, of `HttpClient`.

Weergeven of downloaden een [steekproef van de levensduur van de andere service](http://aka.ms/functions/di-sample).

## <a name="logging-services"></a>Van logboekregistratieservices

Als u uw eigen provider voor logboekregistratie, de aanbevolen manier om u te registreren is een `ILoggerProvider`.  Voor Application Insights, functies Application Insights wordt automatisch toegevoegd voor u.  

> [!WARNING]
> Voeg geen `AddApplicationInsightsTelemetry()` tot de services verzamelen als services die een conflict veroorzaken geregistreerd met wat wordt geleverd door de omgeving. 
 
## <a name="function-app-provided-services"></a>Functie-app opgegeven services

De functie host wordt veel services zich registreert.  Hieronder vindt u services die veilig om op te nemen van een afhankelijkheid zijn op.  Andere hostservices worden niet ondersteund voor het registreren of afhankelijk zijn.  Als er andere services zijn die u wilt ondernemen een afhankelijkheid, neem [maken van een probleem en een discussie op GitHub](https://github.com/azure/azure-functions-host).

|Servicetype|Levensduur|Description|
|--|--|--|
|`Microsoft.Extensions.Configuration.IConfiguration`|Singleton|Runtime-configuratie|
|`Microsoft.Azure.WebJobs.Host.Executors.IHostIdProvider`|Singleton|Verantwoordelijk voor het leveren van de ID van de instantie van de host|

### <a name="overriding-host-services"></a>Hostservices te overschrijven

Het overschrijven van de services die worden geleverd door de host wordt momenteel niet ondersteund.  Als er zijn services die u onderdrukken wilt, [maken van een probleem en een discussie op GitHub](https://github.com/azure/azure-functions-host).

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

* [Het bewaken van uw functie-app](functions-monitoring.md)
* [Aanbevolen procedures voor functions](functions-best-practices.md)
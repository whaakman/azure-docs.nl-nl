---
title: ASP.NET Core-apps - Azure App Service configureren | Microsoft Docs
description: Informatie over het configureren van ASP.NET Core-apps werken in Azure App Service
services: app-service
documentationcenter: ''
author: cephalin
manager: jpconnock
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/28/2019
ms.author: cephalin
ms.openlocfilehash: f2781e3cc2433f73ba7ff33e5c452e29de746adf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65956200"
---
# <a name="configure-a-linux-aspnet-core-app-for-azure-app-service"></a>Configureren van een Linux ASP.NET Core-app voor Azure App Service

ASP.NET Core-apps moeten worden geïmplementeerd als gecompileerde binaire bestanden. Het hulpprogramma voor Visual Studio publiceren de oplossing bouwt en implementeert vervolgens de gecompileerde binaire bestanden rechtstreeks, terwijl de App Service-implementatie-engine de codeopslagplaats eerst implementeert en vervolgens de binaire bestanden compileert.

Deze handleiding bevat belangrijke concepten en instructies voor ASP.NET Core-ontwikkelaars die gebruikmaken van een ingebouwde Linux-container in App Service. Als u Azure App Service nog nooit hebt gebruikt, volgt u de [ASP.NET Core-snelstartgids](quickstart-dotnetcore.md) en [ASP.NET Core met SQL Database-zelfstudie](tutorial-dotnetcore-sqldb-app.md) eerste.

## <a name="show-net-core-version"></a>.NET Core-versie weergeven

Om weer te geven van de huidige versie van .NET Core, voert u de volgende opdracht uit de [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Als u wilt weergeven van alle ondersteunde versies van .NET Core, kunt u de volgende opdracht uitvoeren de [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp list-runtimes --linux | grep DOTNETCORE
```

## <a name="set-net-core-version"></a>.NET Core-versie instellen

Voer de volgende opdracht uit de [Cloud Shell](https://shell.azure.com) om in te stellen van de .NET Core-versie naar versie 2.1:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --linux-fx-version "DOTNETCORE|2.1"
```

## <a name="access-environment-variables"></a>Toegang tot omgevingsvariabelen

In App Service, kunt u [app-instellingen](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) buiten de app-code. Vervolgens kunt u ze met behulp van de standaard ASP.NET-patroon openen:

```csharp
include Microsoft.Extensions.Configuration;
// retrieve App Service app setting
System.Configuration.ConfigurationManager.AppSettings["MySetting"]
// retrieve App Service connection string
Configuration.GetConnectionString("MyDbConnection")
```

Als u een app-instelling configureren met dezelfde naam in App Service en in *Web.config*, de waarde van de App heeft voorrang op de waarde van Web.config. De Web.config-waarde kunt u de app lokaal opsporen, maar de waarde van de App kunt werken in product met productie-instellingen van de app. Tekenreeksen voor databaseverbindingen werken op dezelfde manier. Op deze manier kunt u uw toepassingsgeheimen opslaan buiten de codeopslagplaats van uw en toegang tot de juiste waarden zonder uw code te wijzigen.

## <a name="get-detailed-exceptions-page"></a>Pagina gedetailleerde uitzonderingen

Wanneer uw ASP.NET-app een uitzondering in het foutopsporingsprogramma van Visual Studio genereert, een uitzonderingspagina gedetailleerde in de browser weergegeven, maar in App Service die pagina wordt vervangen door een generieke **HTTP 500** fout of **een fout opgetreden verwerken van uw aanvraag.** Bericht. Als de uitzonderingspagina gedetailleerde in App Service weergeven, toevoegen de `ASPNETCORE_ENVIRONMENT` app-instelling op uw app door de volgende opdracht uit te voeren de <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings ASPNETCORE_ENVIRONMENT="Development"
```

## <a name="detect-https-session"></a>HTTPS-sessie detecteren

In App Service vindt [SSL-beëindiging](https://wikipedia.org/wiki/TLS_termination_proxy) plaats in de load balancers voor het netwerk, zodat alle HTTPS-aanvragen uw app bereiken als niet-versleutelde HTTP-aanvragen. Als uw logische app weten moet als de gebruiker zijn versleuteld vraagt of niet, configureert u de Headers doorgestuurd Middleware in *Startup.cs*:

- Configureer de middleware met [ForwardedHeadersOptions](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) voor het doorsturen van de `X-Forwarded-For` en `X-Forwarded-Proto` kopteksten in `Startup.ConfigureServices`.
- Privé IP-adresbereiken toevoegen met de bekende netwerken, zodat de middleware de load balancer van App Service kunt vertrouwen.
- Aanroepen van de [UseForwardedHeaders](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) methode in `Startup.Configure` voordat u andere middlewares aanroept.

Het samenstellen van alle drie elementen, uw code ziet eruit zoals in het volgende voorbeeld:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders =
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
        options.KnownNetworks.Add(new IPNetwork(IPAddress.Parse("::ffff:10.0.0.0"), 104));
        options.KnownNetworks.Add(new IPNetwork(IPAddress.Parse("::ffff:192.168.0.0"), 112));
        options.KnownNetworks.Add(new IPNetwork(IPAddress.Parse("::ffff:172.16.0.0"), 108));
    });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseForwardedHeaders();

    ...

    app.UseMvc();
}
```

Zie voor meer informatie, [ASP.NET Core configureren om te werken met de proxy-servers en load balancers](https://docs.microsoft.com/aspnet/core/host-and-deploy/proxy-load-balancer).

## <a name="deploy-multi-project-solutions"></a>Meerdere project-oplossingen implementeren

Wanneer u een ASP.NET-opslagplaats implementeert voor de implementatie-engine met een *.csproj* bestand in de hoofdmap, de engine implementeert het project. Wanneer u een ASP.NET-opslagplaats met implementeert een *.sln* bestand in de hoofdmap, de engine kiest de eerste website of Web Application-Project als de App Service-app is gevonden. Het is mogelijk voor de engine niet aan het project dat u wilt verzamelen.

Als u wilt een oplossing voor meerdere project implementeert, kunt u het project in App Service op twee verschillende manieren gebruiken:

### <a name="using-deployment-file"></a>Met behulp van bestand .deployment

Voeg een *.deployment* -bestand naar de hoofdmap van de opslagplaats en voeg de volgende code toe:

```
[config]
project = <project-name>/<project-name>.csproj
```

### <a name="using-app-settings"></a>Met behulp van app-instellingen

In de <a target="_blank" href="https://shell.azure.com">Azure Cloud Shell</a>, een app-instelling toevoegen aan uw App Service-app door het uitvoeren van de volgende CLI-opdracht. Vervang  *\<app-naam >* ,  *\<resource-group-name >* , en  *\<project-name >* met de juiste waarden .

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PROJECT="<project-name>/<project-name>.csproj"
```

## <a name="access-diagnostic-logs"></a>Toegang tot diagnostische logboeken

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>SSH-sessie openen in browser

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie: ASP.NET Core-app met SQL Database](tutorial-dotnetcore-sqldb-app.md)

> [!div class="nextstepaction"]
> [App Service Linux Veelgestelde vragen](app-service-linux-faq.md)
---
title: Maak een .NET-toepassing voor Service Fabric | Microsoft Docs
description: Informatie over het maken van een toepassing met een front-ASP.NET Core en een stateful betrouwbare service back-end en de toepassing implementeren naar een cluster.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/17/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: f4b3c766ee46233cd4ec2d195e39d0b68516952f
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="create-and-deploy-an-application-with-an-aspnet-core-web-api-front-end-service-and-a-stateful-back-end-service"></a>Maken en implementeren van een toepassing met een front-ASP.NET Core Web API-service en een stateful back-endservice
Deze zelfstudie maakt deel uit een reeks.  U leert hoe een Azure Service Fabric-toepassing maken met een front-end van ASP.NET Core Web-API en een stateful back-end-service voor het opslaan van uw gegevens. Wanneer u klaar bent, hebt u een stemtoepassing met een ASP.NET Core-web-front-end die stemresultaten opslaat in een stateful back-endservice in het cluster. Als u niet wilt dat u handmatig de stemtoepassing maken, kunt u [download de broncode](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) voor de voltooide toepassing en gaat u verder met [doorlopen van de stemmende voorbeeldtoepassing](#walkthrough_anchor).

![Diagram van de toepassing](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

Deel een van de reeks, leert u hoe:

> [!div class="checklist"]
> * Een ASP.NET Core Web API-service als betrouwbare stateful service maken
> * Een ASP.NET-webtoepassing Core-service als een stateless webservice maken
> * De omgekeerde proxy gebruiken om te communiceren met de stateful service

In deze zelfstudie reeks leert u hoe:
> [!div class="checklist"]
> * Een .NET-Service Fabric-toepassing bouwen
> * [De toepassing naar een externe-cluster implementeren](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [CI/CD met behulp van Visual Studio Team Services configureren](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * [Controle en diagnostische gegevens voor de toepassing instellen](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Vereisten
Voordat u deze zelfstudie begint:
- Als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Installeer Visual Studio 2017](https://www.visualstudio.com/) versie 15.3 of hoger, waarop de **ontwikkelen van Azure** en **ASP.NET en web ontwikkeling** werkbelastingen.
- [De Service Fabric SDK installeren](service-fabric-get-started.md)

## <a name="create-an-aspnet-web-api-service-as-a-reliable-service"></a>Een ASP.NET Web API-service als een betrouwbare service maken
Maak eerst de web-front-van de stemmende toepassing met behulp van ASP.NET Core. ASP.NET Core is een lichtgewicht, platformoverschrijdende ontwikkeling webframework die u gebruiken kunt voor het maken van moderne webgebruikersinterface en web-API's. Als u een compleet begrip van hoe ASP.NET Core met Service Fabric integreert, wordt aangeraden lezen via de [ASP.NET Core in Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) artikel. Op dit moment kunt u deze zelfstudie voor snel aan de slag. Zie voor meer informatie over ASP.NET Core, de [ASP.NET Core documentatie](https://docs.microsoft.com/aspnet/core/).

1. Start Visual Studio als **beheerder**.

2. Maken van een project met **bestand**->**nieuw**->**Project**

3. Kies in het dialoogvenster **Nieuw Project** de optie **Cloud > Service Fabric-toepassing**.

4. Naam van de toepassing **Voting** en druk op **OK**.

   ![Dialoogvenster voor nieuw project in Visual Studio](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog.png)

5. Op de **nieuwe Service Fabric-Service** pagina **staatloze ASP.NET Core**, en de naam van uw service **VotingWeb**.
   
   ![ASP.NET-webservice te kiezen in het dialoogvenster voor nieuwe service](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog-2.png) 

6. De volgende pagina bevat een set van ASP.NET Core projectsjablonen. Voor deze zelfstudie kiest **webtoepassing (Model-View-Controller)**. 
   
   ![ASP.NET-project kiezen](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog.png)

   Visual Studio maakt een toepassing en een serviceproject en geeft deze weer in Solution Explorer.

   ![Solution Explorer na het maken van een toepassing met ASP.NET core Web API-service]( ./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-angularjs-to-the-votingweb-service"></a>AngularJS toevoegen aan de VotingWeb-service
Voeg [AngularJS](http://angularjs.org/) naar uw service met [Bower ondersteuning](/aspnet/core/client-side/bower). Voeg een configuratiebestand Bower eerst toe aan het project.  Klik in Solution Explorer met de rechtermuisknop op **VotingWeb** en selecteer **toevoegen -> Nieuw Item**. Selecteer **Web** en vervolgens **Bower configuratiebestand**.  De *bower.json* -bestand wordt gemaakt.

Open *bower.json* en vermeldingen voor hoeken en hoekvormige bootstrap toevoegen en vervolgens uw wijzigingen niet opslaan.

```json
{
  "name": "asp.net",
  "private": true,
  "dependencies": {
    "bootstrap": "3.3.7",
    "jquery": "3.2.1",
    "jquery-validation": "1.16.0",
    "jquery-validation-unobtrusive": "3.2.6",
    "angular": "v1.6.8",
    "angular-bootstrap": "v1.1.0"
  }
}
```
Bij het opslaan de *bower.json* bestand Angular is geïnstalleerd in uw project *wwwroot/lib* map. Bovendien wordt vermeld in de *afhankelijkheden/Bower* map.

### <a name="update-the-sitejs-file"></a>Het bestand site.js bijwerken
Open de *wwwroot/js/site.js* bestand.  De inhoud ervan vervangen door JavaScript gebruikt door de startpagina weergaven:

```javascript
var app = angular.module('VotingApp', ['ui.bootstrap']);
app.run(function () { });

app.controller('VotingAppController', ['$rootScope', '$scope', '$http', '$timeout', function ($rootScope, $scope, $http, $timeout) {

    $scope.refresh = function () {
        $http.get('api/Votes?c=' + new Date().getTime())
            .then(function (data, status) {
                $scope.votes = data;
            }, function (data, status) {
                $scope.votes = undefined;
            });
    };

    $scope.remove = function (item) {
        $http.delete('api/Votes/' + item)
            .then(function (data, status) {
                $scope.refresh();
            })
    };

    $scope.add = function (item) {
        var fd = new FormData();
        fd.append('item', item);
        $http.put('api/Votes/' + item, fd, {
            transformRequest: angular.identity,
            headers: { 'Content-Type': undefined }
        })
            .then(function (data, status) {
                $scope.refresh();
                $scope.item = undefined;
            })
    };
}]);
```

### <a name="update-the-indexcshtml-file"></a>Het bestand Index.cshtml bijwerken
Open de *Views/Home/Index.cshtml* -bestand, de weergave die specifiek zijn voor de controller startpagina.  Vervang de inhoud door het volgende, sla de wijzigingen vervolgens.

```html
@{
    ViewData["Title"] = "Service Fabric Voting Sample";
}

<div ng-controller="VotingAppController" ng-init="refresh()">
    <div class="container-fluid">
        <div class="row">
            <div class="col-xs-8 col-xs-offset-2 text-center">
                <h2>Service Fabric Voting Sample</h2>
            </div>
        </div>

        <div class="row">
            <div class="col-xs-8 col-xs-offset-2">
                <form class="col-xs-12 center-block">
                    <div class="col-xs-6 form-group">
                        <input id="txtAdd" type="text" class="form-control" placeholder="Add voting option" ng-model="item"/>
                    </div>
                    <button id="btnAdd" class="btn btn-default" ng-click="add(item)">
                        <span class="glyphicon glyphicon-plus" aria-hidden="true"></span>
                        Add
                    </button>
                </form>
            </div>
        </div>

        <hr/>

        <div class="row">
            <div class="col-xs-8 col-xs-offset-2">
                <div class="row">
                    <div class="col-xs-4">
                        Click to vote
                    </div>
                </div>
                <div class="row top-buffer" ng-repeat="vote in votes.data">
                    <div class="col-xs-8">
                        <button class="btn btn-success text-left btn-block" ng-click="add(vote.key)">
                            <span class="pull-left">
                                {{vote.key}}
                            </span>
                            <span class="badge pull-right">
                                {{vote.value}} Votes
                            </span>
                        </button>
                    </div>
                    <div class="col-xs-4">
                        <button class="btn btn-danger pull-right btn-block" ng-click="remove(vote.key)">
                            <span class="glyphicon glyphicon-remove" aria-hidden="true"></span>
                            Remove
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
```

### <a name="update-the-layoutcshtml-file"></a>Het bestand _Layout.cshtml bijwerken
Open de *Views/Shared/_Layout.cshtml* -bestand, de standaardindeling voor de ASP.NET-app.  Vervang de inhoud door het volgende, sla de wijzigingen vervolgens.

```html
<!DOCTYPE html>
<html ng-app="VotingApp" xmlns:ng="http://angularjs.org">
<head>
    <meta charset="utf-8"/>
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>@ViewData["Title"]</title>

    <link href="~/lib/bootstrap/dist/css/bootstrap.min.css" rel="stylesheet"/>
    <link href="~/css/site.css" rel="stylesheet"/>

</head>
<body>
<div class="container body-content">
    @RenderBody()
</div>

<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
<script src="~/lib/angular/angular.js"></script>
<script src="~/lib/angular-bootstrap/ui-bootstrap-tpls.js"></script>
<script src="~/js/site.js"></script>

@RenderSection("Scripts", required: false)
</body>
</html>
```

### <a name="update-the-votingwebcs-file"></a>Het bestand VotingWeb.cs bijwerken
Open de *VotingWeb.cs* bestand, dat wordt gemaakt van de WebHost ASP.NET Core binnen de staatloze service met de webserver WebListener.  

Voeg de `using System.Net.Http;` richtlijn boven aan het bestand.  

Vervang de `CreateServiceInstanceListeners()` werken met het volgende en sla vervolgens uw wijzigingen.

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(
            serviceContext =>
                new KestrelCommunicationListener(
                    serviceContext,
                    "ServiceEndpoint",
                    (url, listener) =>
                    {
                        ServiceEventSource.Current.ServiceMessage(serviceContext, $"Starting Kestrel on {url}");

                        return new WebHostBuilder()
                            .UseKestrel()
                            .ConfigureServices(
                                services => services
                                    .AddSingleton<HttpClient>(new HttpClient())
                                    .AddSingleton<FabricClient>(new FabricClient())
                                    .AddSingleton<StatelessServiceContext>(serviceContext))
                            .UseContentRoot(Directory.GetCurrentDirectory())
                            .UseStartup<Startup>()
                            .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                            .UseUrls(url)
                            .Build();
                    }))
    };
}
```

Voeg ook de `GetVotingDataServiceName` methode, die de naam van de service als gepeild retourneert:

```csharp
internal static Uri GetVotingDataServiceName(ServiceContext context)
{
    return new Uri($"{context.CodePackageActivationContext.ApplicationName}/VotingData");
}
```

### <a name="add-the-votescontrollercs-file"></a>Het bestand VotesController.cs toevoegen
Een controller, waarmee wordt gedefinieerd stemmende acties toevoegen. Met de rechtermuisknop op de **domeincontrollers** map, selecteer vervolgens **toevoegen -> Nieuw item -> klasse**.  Naam van het bestand 'VotesController.cs' en klik op **toevoegen**.  

Inhoud van het bestand vervangen door de volgende, sla de wijzigingen vervolgens.  Verderop in [bijwerken van het bestand VotesController.cs](#updatevotecontroller_anchor), dit bestand is gewijzigd om te lezen en schrijven van stemmende gegevens op de back-endservice.  Op dit moment retourneert de controller statische tekenreeksgegevens naar de weergave.

```csharp
namespace VotingWeb.Controllers
{
    using System;
    using System.Collections.Generic;
    using System.Fabric;
    using System.Fabric.Query;
    using System.Linq;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;
    using Newtonsoft.Json;

    [Produces("application/json")]
    [Route("api/Votes")]
    public class VotesController : Controller
    {
        private readonly HttpClient httpClient;

        public VotesController(HttpClient httpClient)
        {
            this.httpClient = httpClient;
        }

        // GET: api/Votes
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            List<KeyValuePair<string, int>> votes= new List<KeyValuePair<string, int>>();
            votes.Add(new KeyValuePair<string, int>("Pizza", 3));
            votes.Add(new KeyValuePair<string, int>("Ice cream", 4));

            return Json(votes);
        }
     }
}
```

### <a name="configure-the-listening-port"></a>De luisterpoort configureren
Wanneer de front-endservice VotingWeb is gemaakt, zijn Visual Studio selecteert willekeurig een poort voor de service voor luisteren.  De service VotingWeb fungeert als front-end voor deze toepassing en externe verkeer accepteert, dus die service binden aan een vaste en poort ook weten.  De [servicemanifest](service-fabric-application-and-service-manifests.md) declareert u de service-eindpunten. Open in Solution Explorer *VotingWeb/PackageRoot/ServiceManifest.xml*.  Zoeken de **eindpunt** resource in de **Resources** sectie en wijzig de **poort** waarde in op 80 of naar een andere poort. Als u wilt implementeren en de toepassing lokaal uitvoeren, moet de toepassing luisterpoort open en beschikbare op uw computer.

```xml
<Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="80" />
    </Endpoints>
  </Resources>
```

De waarde van de toepassings-URL-eigenschap in het project Voting ook bijwerken zodat een webbrowser op de juiste poort opent wanneer u fouten opspoort, druk op F5 '.  Klik in Solution Explorer, selecteer de **Voting** project en werk de **toepassings-URL** eigenschap.

![De URL van toepassing](./media/service-fabric-tutorial-deploy-app-to-party-cluster/application-url.png)

### <a name="deploy-and-run-the-application-locally"></a>Implementeren en de toepassing lokaal uitvoeren
U kunt nu doorgaan en de toepassing uitvoeren. Druk op `F5` in Visual Studio om de toepassing voor foutopsporing te implementeren. `F5`mislukt als u niet eerder Visual Studio als Open **beheerder**.

> [!NOTE]
> De eerste keer dat u de toepassing lokaal uitvoert en implementeert, wordt door Visual Studio een lokaal cluster voor foutopsporing gemaakt.  Maken van het cluster kan enige tijd duren. De status van het maken van het cluster wordt weergegeven in het Visual Studio-uitvoervenster.

Uw web-app moet er op dit punt wordt als volgt uitzien:

![ASP.NET Core front-](./media/service-fabric-tutorial-create-dotnet-app/debug-front-end.png)

Als u wilt stop de foutopsporing voor de toepassing, gaat u terug naar Visual Studio en druk op **Shift + F5**.

## <a name="add-a-stateful-back-end-service-to-your-application"></a>Een stateful back-endservice toevoegt aan uw toepassing
Een ASP.NET Web API-service wordt uitgevoerd in de toepassing, gaat u verder gaan en een stateful betrouwbare service voor het opslaan van sommige gegevens in de toepassing toevoegen.

Service Fabric kunt u voor het opslaan van uw gegevens rechtstreeks in uw service met behulp van betrouwbare verzamelingen consistent en betrouwbaar. Betrouwbare verzamelingen zijn een set van maximaal beschikbare en betrouwbare verzameling klassen die bekend zijn voor iedereen die is gebruikt C#-verzamelingen.

In deze zelfstudie maakt u een service die een itemwaarde in een betrouwbare verzameling opslaat.

1. Klik in Solution Explorer met de rechtermuisknop op **Services** in de toepassing project en kies **toevoegen > nieuwe Service Fabric-Service**.
    
2. In de **nieuwe Service Fabric-Service** dialoogvenster kiezen **Stateful ASP.NET Core**, en de naam van de service **VotingData** en druk op **OK**.

    ![Dialoogvenster voor nieuwe service in Visual Studio](./media/service-fabric-tutorial-create-dotnet-app/add-stateful-service.png)

    Nadat uw serviceproject is gemaakt, hebt u twee services in uw toepassing. Als u doorgaat met uw toepassing bouwen, kunt u meer services die zich op dezelfde manier kunt toevoegen. Elk zijn onafhankelijk samengestelde en bijgewerkte.

3. De volgende pagina bevat een set van ASP.NET Core projectsjablonen. Voor deze zelfstudie kiest **Web API**.

    ![ASP.NET-project kiezen](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog2.png)

    Visual Studio maakt een serviceproject en geeft deze weer in Solution Explorer.

    ![Solution Explorer](./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-webapi-service.png)

### <a name="add-the-votedatacontrollercs-file"></a>Het bestand VoteDataController.cs toevoegen

In de **VotingData** project met de rechtermuisknop op de **domeincontrollers** map, selecteer vervolgens **toevoegen -> Nieuw item -> klasse**. Naam van het bestand 'VoteDataController.cs' en klik op **toevoegen**. Inhoud van het bestand vervangen door de volgende, sla de wijzigingen vervolgens.

```csharp
// ------------------------------------------------------------
//  Copyright (c) Microsoft Corporation.  All rights reserved.
//  Licensed under the MIT License (MIT). See License.txt in the repo root for license information.
// ------------------------------------------------------------

namespace VotingData.Controllers
{
    using System.Collections.Generic;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.ServiceFabric.Data;
    using Microsoft.ServiceFabric.Data.Collections;

    [Route("api/[controller]")]
    public class VoteDataController : Controller
    {
        private readonly IReliableStateManager stateManager;

        public VoteDataController(IReliableStateManager stateManager)
        {
            this.stateManager = stateManager;
        }

        // GET api/VoteData
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            CancellationToken ct = new CancellationToken();

            IReliableDictionary<string, int> votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                IAsyncEnumerable<KeyValuePair<string, int>> list = await votesDictionary.CreateEnumerableAsync(tx);

                IAsyncEnumerator<KeyValuePair<string, int>> enumerator = list.GetAsyncEnumerator();

                List<KeyValuePair<string, int>> result = new List<KeyValuePair<string, int>>();

                while (await enumerator.MoveNextAsync(ct))
                {
                    result.Add(enumerator.Current);
                }

                return this.Json(result);
            }
        }

        // PUT api/VoteData/name
        [HttpPut("{name}")]
        public async Task<IActionResult> Put(string name)
        {
            IReliableDictionary<string, int> votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                await votesDictionary.AddOrUpdateAsync(tx, name, 1, (key, oldvalue) => oldvalue + 1);
                await tx.CommitAsync();
            }

            return new OkResult();
        }

        // DELETE api/VoteData/name
        [HttpDelete("{name}")]
        public async Task<IActionResult> Delete(string name)
        {
            IReliableDictionary<string, int> votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                if (await votesDictionary.ContainsKeyAsync(tx, name))
                {
                    await votesDictionary.TryRemoveAsync(tx, name);
                    await tx.CommitAsync();
                    return new OkResult();
                }
                else
                {
                    return new NotFoundResult();
                }
            }
        }
    }
}
```


## <a name="connect-the-services"></a>Verbinding maken met de services
In deze stap, verbinding maken met de twee services en breng de front-end-webservers toepassing get en set-informatie van de back-endservice stemmen.

Service Fabric biedt volledige flexibiliteit in hoe u met reliable services communiceren. Binnen één toepassing hebt u mogelijk services die toegankelijk via TCP zijn. Andere services die mogelijk toegankelijk via een HTTP REST-API en nog andere services kunnen toegang worden verkregen via websockets. Zie voor achtergrondinformatie over de beschikbare opties en de wisselwerking betrokken [services communiceert](service-fabric-connect-and-communicate-with-services.md).

In deze zelfstudie gebruikt [ASP.NET Core Web API](service-fabric-reliable-services-communication-aspnetcore.md).

<a id="updatevotecontroller" name="updatevotecontroller_anchor"></a>

### <a name="update-the-votescontrollercs-file"></a>Het bestand VotesController.cs bijwerken
In de **VotingWeb** project, open de *Controllers/VotesController.cs* bestand.  Vervang de `VotesController` definitie inhoud met de volgende klasse en sla vervolgens uw wijzigingen.

```csharp
public class VotesController : Controller
{
    private readonly HttpClient httpClient;
    private readonly FabricClient fabricClient;
    private readonly StatelessServiceContext serviceContext;

    public VotesController(HttpClient httpClient, StatelessServiceContext context, FabricClient fabricClient)
    {
        this.fabricClient = fabricClient;
        this.httpClient = httpClient;
        this.serviceContext = context;
    }

    // GET: api/Votes
    [HttpGet("")]
    public async Task<IActionResult> Get()
    {
        Uri serviceName = VotingWeb.GetVotingDataServiceName(this.serviceContext);
        Uri proxyAddress = this.GetProxyAddress(serviceName);

        ServicePartitionList partitions = await this.fabricClient.QueryManager.GetPartitionListAsync(serviceName);

        List<KeyValuePair<string, int>> result = new List<KeyValuePair<string, int>>();

        foreach (Partition partition in partitions)
        {
            string proxyUrl =
                $"{proxyAddress}/api/VoteData?PartitionKey={((Int64RangePartitionInformation) partition.PartitionInformation).LowKey}&PartitionKind=Int64Range";

            using (HttpResponseMessage response = await this.httpClient.GetAsync(proxyUrl))
            {
                if (response.StatusCode != System.Net.HttpStatusCode.OK)
                {
                    continue;
                }

                result.AddRange(JsonConvert.DeserializeObject<List<KeyValuePair<string, int>>>(await response.Content.ReadAsStringAsync()));
            }
        }

        return this.Json(result);
    }

    // PUT: api/Votes/name
    [HttpPut("{name}")]
    public async Task<IActionResult> Put(string name)
    {
        Uri serviceName = VotingWeb.GetVotingDataServiceName(this.serviceContext);
        Uri proxyAddress = this.GetProxyAddress(serviceName);
        long partitionKey = this.GetPartitionKey(name);
        string proxyUrl = $"{proxyAddress}/api/VoteData/{name}?PartitionKey={partitionKey}&PartitionKind=Int64Range";

        StringContent putContent = new StringContent($"{{ 'name' : '{name}' }}", Encoding.UTF8, "application/json");
        putContent.Headers.ContentType = new MediaTypeHeaderValue("application/json");

        using (HttpResponseMessage response = await this.httpClient.PutAsync(proxyUrl, putContent))
        {
            return new ContentResult()
            {
                StatusCode = (int) response.StatusCode,
                Content = await response.Content.ReadAsStringAsync()
            };
        }
    }

    // DELETE: api/Votes/name
    [HttpDelete("{name}")]
    public async Task<IActionResult> Delete(string name)
    {
        Uri serviceName = VotingWeb.GetVotingDataServiceName(this.serviceContext);
        Uri proxyAddress = this.GetProxyAddress(serviceName);
        long partitionKey = this.GetPartitionKey(name);
        string proxyUrl = $"{proxyAddress}/api/VoteData/{name}?PartitionKey={partitionKey}&PartitionKind=Int64Range";

        using (HttpResponseMessage response = await this.httpClient.DeleteAsync(proxyUrl))
        {
            if (response.StatusCode != System.Net.HttpStatusCode.OK)
            {
                return this.StatusCode((int) response.StatusCode);
            }
        }

        return new OkResult();
    }


    /// <summary>
    /// Constructs a reverse proxy URL for a given service.
    /// Example: http://localhost:19081/VotingApplication/VotingData/
    /// </summary>
    /// <param name="serviceName"></param>
    /// <returns></returns>
    private Uri GetProxyAddress(Uri serviceName)
    {
        return new Uri($"http://localhost:19081{serviceName.AbsolutePath}");
    }

    /// <summary>
    /// Creates a partition key from the given name.
    /// Uses the zero-based numeric position in the alphabet of the first letter of the name (0-25).
    /// </summary>
    /// <param name="name"></param>
    /// <returns></returns>
    private long GetPartitionKey(string name)
    {
        return Char.ToUpper(name.First()) - 'A';
    }
}
```
<a id="walkthrough" name="walkthrough_anchor"></a>

## <a name="walk-through-the-voting-sample-application"></a>Stapsgewijs door de voorbeeldstemtoepassing gaan
De stemtoepassing bestaat uit twee services:
- Web-front-endservice (VotingWeb): een ASP.NET Core web-front-endservice die de webpagina ondersteunt en web-API's beschikbaar stelt om te communiceren met de back-endservice.
- Back-endservice (VotingData): een ASP.NET Core-webservice, die een API beschikbaar stelt om de stemresultaten in een betrouwbare dictionary op schijf op te slaan.

![Diagram van de toepassing](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

Wanneer u in de toepassing stemt, vinden de volgende gebeurtenissen plaats:
1. Een JavaScript verzendt de stemaanvraag als een HTTP PUT-aanvraag naar de web-API in de web-front-endservice.

2. De web-front-endservice gebruikt een proxyserver om een HTTP PUT-aanvraag te vinden en door te sturen naar de back-endservice.

3. De back-endservice neemt de binnenkomende aanvraag aan en slaat het bijgewerkte resultaat op in een betrouwbare dictionary die wordt gerepliceerd naar meerdere knooppunten binnen het cluster en opgeslagen op schijf. Gegevens van de toepassing worden opgeslagen in het cluster, zodat er geen database nodig is.

## <a name="debug-in-visual-studio"></a>Fouten opsporen met Visual Studio
Als u met Visual Studio fouten opspoort in de toepassing, gebruikt u daarvoor een lokaal ontwikkelingscluster van Service Fabric. U hebt de mogelijkheid om uw foutopsporingservaring aan uw specifieke scenario aan te passen. In deze toepassing opgeslagen gegevens in de back-end-service met behulp van een betrouwbare woordenlijst. Visual Studio verwijdert standaard de toepassing wanneer u het foutopsporingsprogramma stopt. Door de toepassing te verwijderen, worden de gegevens in de back-endservice ook verwijderd. Als u de gegevens tussen de foutopsporingssessies wilt kunnen behouden, kunt u de **foutopsporingsmodus van de toepassing** als eigenschap op het **Voting**-project in Visual Studio wijzigen.

Als u wilt zien wat er in de code gebeurt, moet u de volgende stappen uitvoeren:
1. Open de **VotesController.cs** bestands- en stel een onderbrekingspunt in de web-API's **plaatsen** methode (line-63) - u kunt zoeken naar het bestand in Solution Explorer in Visual Studio.

2. Open de **VoteDataController.cs** bestands- en stel een onderbrekingspunt in deze web-API **plaatsen** methode (line 53).

3. Ga terug naar de browser en klik op een stemoptie of voeg een nieuwe stemoptie toe. U komt uit bij het eerste onderbrekingspunt in de API-controller van de web-front-end.
    
    1. Dit is het punt waarop door JavaScript in de browser een aanvraag wordt verzonden naar de web-API-controller in de front-endservice.
    
    ![Front-endservice van Vote toevoegen](./media/service-fabric-tutorial-create-dotnet-app/addvote-frontend.png)

    2. Eerst samenstellen van de URL van de ReverseProxy voor de back-endservice **(1)**.
    3. Verzend het HTTP PUT-aanvraag naar de ReverseProxy **(2)**.
    4. Ten slotte het retourtype het antwoord van de back-end-service op de client **(3)**.

4. Druk op **F5** om door te gaan
    1. U bent nu op het onderbrekingspunt in de back-endservice aanbeland.
    
    ![Back-endservice Vote toevoegen](./media/service-fabric-tutorial-create-dotnet-app/addvote-backend.png)

    2. In de eerste regel in de methode **(1)** gebruiken de `StateManager` ophalen of het toevoegen van een betrouwbare woordenlijst aangeroepen `counts`.
    3. Alle interacties met waarden in een betrouwbare dictionary vereisen een transactie, met deze instructie **(2)** wordt die transactie gemaakt.
    4. Werk de waarde van de relevante sleutel voor de optie in de transactie en voert u de bewerking **(3)**. Zodra de doorvoermethode resultaten retourneert, worden de gegevens bijgewerkt in de dictionary en gerepliceerd naar andere knooppunten in het cluster. De gegevens worden nu veilig opgeslagen in het cluster en de back-endservice kan een failover-overschakeling uitvoeren naar andere knooppunten, waarbij de gegevens beschikbaar blijven.
5. Druk op **F5** om door te gaan

Als u de foutopsporingssessie wilt stoppen, drukt u op **Shift+F5**.


## <a name="next-steps"></a>Volgende stappen
In dit deel van de zelfstudie hebt u geleerd hoe:

> [!div class="checklist"]
> * Een ASP.NET Core Web API-service als betrouwbare stateful service maken
> * Een ASP.NET-webtoepassing Core-service als een stateless webservice maken
> * De omgekeerde proxy gebruiken om te communiceren met de stateful service

Ga naar de volgende zelfstudie:
> [!div class="nextstepaction"]
> [De toepassing implementeren naar Azure](service-fabric-tutorial-deploy-app-to-party-cluster.md)

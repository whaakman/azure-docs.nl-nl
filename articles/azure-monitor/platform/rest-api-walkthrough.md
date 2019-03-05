---
title: Azure Monitoring REST-API-overzicht
description: Over het verifiëren van aanvragen en de Azure Monitor REST API gebruiken om beschikbare metrische definities en metrische waarden te halen.
author: mcollier
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: mcollier
ms.subservice: ''
ms.openlocfilehash: 2ba0ea64aab67221aa1ee3a87ad35ce7d5516167
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57310043"
---
# <a name="azure-monitoring-rest-api-walkthrough"></a>Azure Monitoring REST-API-overzicht

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

In dit artikel laat zien hoe verificatie uitvoeren zodat uw code kunt u de [Microsoft Azure Monitor REST API-verwijzing](https://docs.microsoft.com/rest/api/monitor/).

De Azure Monitor-API maakt het mogelijk is om op te halen via een programma de metrische definities van de beschikbare standaardregels, granulariteit en metrische waarden. De gegevens kunnen worden opgeslagen in een afzonderlijk gegevensarchief zoals Azure SQL Database, Azure Cosmos DB of Azure Data Lake. Van daaruit kan zo nodig extra analyses worden uitgevoerd.

Naast het werken met verschillende metrische gegevenspunten, maakt de API van de Monitor ook het mogelijk aan de lijst met regels voor waarschuwingen, activiteitenlogboeken bekijken en nog veel meer. Zie voor een volledige lijst van beschikbare bewerkingen, de [Microsoft Azure Monitor REST API-verwijzing](https://docs.microsoft.com/rest/api/monitor/).

## <a name="authenticating-azure-monitor-requests"></a>Verificatie van Azure Monitor-aanvragen

De eerste stap is om te verifiëren van de aanvraag.

Alle taken die worden uitgevoerd voor de API van Azure Monitor gebruiken de Azure Resource Manager-verificatiemodel. Daarom moeten alle aanvragen worden geverifieerd bij Azure Active Directory (Azure AD). Eén mogelijke benadering voor het verifiëren van de clienttoepassing is op een Azure AD-service-principal maken en op te halen van het verificatietoken (JWT). Het script in het volgende voorbeeld ziet u een Azure AD-service-principal via PowerShell maken. Voor meer gedetailleerde stapsgewijze instructies, Raadpleeg de documentatie op [met Azure PowerShell om te maken van een service-principal toegang krijgen tot bronnen](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps). Het is ook mogelijk om te [maken van een service-principal via de Azure-portal](../../active-directory/develop/howto-create-service-principal-portal.md).

```PowerShell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"

# Authenticate to a specific Azure subscription.
Connect-AzAccount -SubscriptionId $subscriptionId

# Password for the service principal
$pwd = "{service-principal-password}"
$secureStringPassword = ConvertTo-SecureString -String $pwd -AsPlainText -Force

# Create a new Azure AD application
$azureAdApplication = New-AzADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $secureStringPassword

# Create a new service principal associated with the designated application
New-AzADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role to the newly created service principal
New-AzRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

Om te vragen de API van Azure Monitor, moet de clienttoepassing de eerder gemaakte service-principal gebruiken om te verifiëren. Het volgende voorbeeld PowerShell-script toont een benadering, met behulp van de [Active Directory Authentication Library](../../active-directory/develop/active-directory-authentication-libraries.md) (ADAL) om op te halen van de JWT-token voor verificatie. De JWT-token wordt doorgegeven als onderdeel van een HTTP-autorisatie-parameter in aanvragen aan de Azure Monitor REST API.

```PowerShell
$azureAdApplication = Get-AzADApplication -IdentifierUri "https://localhost/azure-monitor"

$subscription = Get-AzSubscription -SubscriptionId $subscriptionId

$clientId = $azureAdApplication.ApplicationId.Guid
$tenantId = $subscription.TenantId
$authUrl = "https://login.microsoftonline.com/${tenantId}"

$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($clientId, $pwd)

$result = $AuthContext.AcquireTokenAsync("https://management.core.windows.net/", $cred).GetAwaiter().GetResult()

# Build an array of HTTP header values
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

Na verificatie, kunnen vervolgens query's worden uitgevoerd voor de Azure Monitor REST API. Er zijn twee handige query's:

1. Lijst van de metrische definities voor een resource
2. De metrische waarden ophalen

> [!NOTE]
> Raadpleeg voor meer informatie over verificatie met de Azure REST API, de [Azure REST API-verwijzing](https://docs.microsoft.com/rest/api/azure/).
>
>

## <a name="retrieve-metric-definitions-multi-dimensional-api"></a>Metrische definities (Multi-dimensionale API) ophalen

Gebruik de [definities voor metrische gegevens van Azure Monitor REST-API](https://docs.microsoft.com/rest/api/monitor/metricdefinitions) voor toegang tot de lijst met metrische gegevens die beschikbaar voor een service zijn.

**Methode**: GET

**Aanvraag-URI**: https://management.azure.com/subscriptions/ *{subscriptionId}*/resourceGroups/*{resourceGroupName}*/providers/*{resourceProviderNamespace}* / *{resourceType}*/*{resourceName}*/providers/microsoft.insights/metricDefinitions?api-version=*{apiVersion}*

Bijvoorbeeld, als u wilt ophalen van de metrische definities voor een Azure Storage-account, zou de aanvraag er als volgt uitzien:

```PowerShell
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/microsoft.insights/metricDefinitions?api-version=2018-01-01"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -OutFile ".\contosostorage-metricdef-results.json" `
                  -Verbose

```

> [!NOTE]
> Als u wilt ophalen van de metrische definities met behulp van de multi-dimensionale metrische met Azure Monitor REST-API, '2018-01-01' als de API-versie te gebruiken.
>
>

De hoofdtekst van het resulterende JSON-antwoord is vergelijkbaar met het volgende voorbeeld: (Houd er rekening mee dat de tweede metriek dimensies is)

```JSON
{
    "value": [
        {
            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/microsoft.insights/metricdefinitions/UsedCapacity",
            "resourceId": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage",
            "namespace": "Microsoft.Storage/storageAccounts",
            "category": "Capacity",
            "name": {
                "value": "UsedCapacity",
                "localizedValue": "Used capacity"
            },
            "isDimensionRequired": false,
            "unit": "Bytes",
            "primaryAggregationType": "Average",
            "supportedAggregationTypes": [
                "Total",
                "Average",
                "Minimum",
                "Maximum"
            ],
            "metricAvailabilities": [
                {
                    "timeGrain": "PT1H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT6H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT12H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "P1D",
                    "retention": "P93D"
                }
            ]
        },
        {
            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/microsoft.insights/metricdefinitions/Transactions",
            "resourceId": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage",
            "namespace": "Microsoft.Storage/storageAccounts",
            "category": "Transaction",
            "name": {
                "value": "Transactions",
                "localizedValue": "Transactions"
            },
            "isDimensionRequired": false,
            "unit": "Count",
            "primaryAggregationType": "Total",
            "supportedAggregationTypes": [
                "Total"
            ],
            "metricAvailabilities": [
                {
                    "timeGrain": "PT1M",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT5M",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT15M",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT30M",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT1H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT6H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT12H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "P1D",
                    "retention": "P93D"
                }
            ],
            "dimensions": [
                {
                    "value": "ResponseType",
                    "localizedValue": "Response type"
                },
                {
                    "value": "GeoType",
                    "localizedValue": "Geo type"
                },
                {
                    "value": "ApiName",
                    "localizedValue": "API name"
                }
            ]
        },
        ...
    ]
}
```

## <a name="retrieve-dimension-values-multi-dimensional-api"></a>Ophalen van dimensiewaarden (Multi-dimensionale API)

Zodra de beschikbare metrische definities bekend zijn, kunnen er bepaalde metrische gegevens die dimensies hebben. Voordat u query's uitvoeren voor de metrische gegevens die om te ontdekken welke het bereik van waarden kunt u is een dimensie. Op basis van deze dimensiewaarden die u er vervolgens voor kiezen kunt om te filteren of segment de metrische gegevens op basis van dimensiewaarden tijdens het opvragen van metrische gegevens.  Gebruik de [REST-API van Azure Monitor Metrics](https://docs.microsoft.com/rest/api/monitor/metrics) om dit te bereiken.

Gebruik van de metrische gegevens de naam 'value' (niet de ' localizedValue') voor geen filtering aanvragen. Als er geen filters zijn opgegeven, wordt de metrische standaardgegevens geretourneerd. Het gebruik van deze API kan slechts één dimensie hebben een wildcard-filter.

> [!NOTE]
> Als u wilt ophalen met behulp van de Azure Monitor REST API dimensiewaarden, '2018-01-01' als de API-versie te gebruiken.
>
>

**Methode**: GET

**Aanvraag-URI**: https://management.azure.com/subscriptions/ *{abonnement-id}*/resourceGroups/*{resource-group-name}*/providers/*{resource-provider-naamruimte}* / *{resourcetype}*/*{resourcenaam}*/providers/microsoft.insights/metrics?metricnames=*{metriek}*& timespan =*{starttime/endtime}*& $filter =*{filter}*& resultType = metagegevens & api-version =*{apiVersion}*

Bijvoorbeeld, om op te halen van de lijst met dimensiewaarden die voor de 'naam van de API-dimensie' voor de metriek "Transacties" zijn verzonden, waarbij de dimensie GeoType = 'Primaire' tijdens de opgegeven periode, de aanvraag zou als volgt:

```PowerShell
$filter = "APIName eq '*' and GeoType eq 'Primary'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/microsoft.insights/metrics?metricnames=Transactions&timespan=2018-03-01T00:00:00Z/2018-03-02T00:00:00Z&resultType=metadata&`$filter=${filter}&api-version=2018-01-01"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contosostorage-dimension-values.json" `
    -Verbose
```

De hoofdtekst van het resulterende JSON-antwoord is vergelijkbaar met het volgende voorbeeld:

```JSON
{
  "timespan": "2018-03-01T00:00:00Z/2018-03-02T00:00:00Z",
  "value": [
    {
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/Microsoft.Insights/metrics/Transactions",
      "type": "Microsoft.Insights/metrics",
      "name": {
        "value": "Transactions",
        "localizedValue": "Transactions"
      },
      "unit": "Count",
      "timeseries": [
        {
          "metadatavalues": [
            {
              "name": {
                "value": "apiname",
                "localizedValue": "apiname"
              },
              "value": "DeleteBlob"
            }
          ]
        },
        {
          "metadatavalues": [
            {
              "name": {
                "value": "apiname",
                "localizedValue": "apiname"
              },
              "value": "SetBlobProperties"
            }
          ]
        },
        ...
      ]
    }
  ],
  "namespace": "Microsoft.Storage/storageAccounts",
  "resourceregion": "eastus"
}
```

## <a name="retrieve-metric-values-multi-dimensional-api"></a>Ophalen van metrische waarden (Multi-dimensionale API)

Zodra de beschikbare metrische definities en mogelijke dimensiewaarden bekend zijn, is het vervolgens mogelijk om op te halen van de gerelateerde metrische waarden.  Gebruik de [REST-API van Azure Monitor Metrics](https://docs.microsoft.com/rest/api/monitor/metrics) om dit te bereiken.

Gebruik van de metrische gegevens de naam 'value' (niet de ' localizedValue') voor geen filtering aanvragen. Als er geen dimensiefilters zijn opgegeven, wordt de samengevouwen samengevoegde metrische gegevens geretourneerd. Als een query voor metrische gegevens meerdere timeseries retourneert, kunt u de parameters van de query 'Top' en 'OrderBy' gebruiken om een beperkte geordende lijst timeseries te retourneren.

> [!NOTE]
> Als u wilt ophalen van multi-dimensionale metrische waarden met behulp van de Azure Monitor REST API, '2018-01-01' als de API-versie te gebruiken.
>
>

**Methode**: GET

**Aanvraag-URI**: https://management.azure.com/subscriptions/ *{abonnement-id}*/resourceGroups/*{resource-group-name}*/providers/*{resource-provider-naamruimte}* / *{resourcetype}*/*{resourcenaam}*/providers/microsoft.insights/metrics?metricnames=*{metriek}*& timespan =*{starttime/endtime}*& $filter =*{filter}*& interval =*{timeGrain}*& aggregatie =*{ aggreation}*& api-version =*{apiVersion}*

Bijvoorbeeld, om op te halen van de top-3-API's, in aflopende waarde, door het aantal 'Transacties' tijdens een bereik van 5 minuten, waar de GeotType 'Primaire', is de aanvraag zou als volgt:

```PowerShell
$filter = "APIName eq '*' and GeoType eq 'Primary'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/microsoft.insights/metrics?metricnames=Transactions&timespan=2018-03-01T02:00:00Z/2018-03-01T02:05:00Z&`$filter=${filter}&interval=PT1M&aggregation=Total&top=3&orderby=Total desc&api-version=2018-01-01"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contosostorage-metric-values.json" `
    -Verbose
```

De hoofdtekst van het resulterende JSON-antwoord is vergelijkbaar met het volgende voorbeeld:

```JSON
{
  "cost": 0,
  "timespan": "2018-03-01T02:00:00Z/2018-03-01T02:05:00Z",
  "interval": "PT1M",
  "value": [
    {
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/Microsoft.Insights/metrics/Transactions",
      "type": "Microsoft.Insights/metrics",
      "name": {
        "value": "Transactions",
        "localizedValue": "Transactions"
      },
      "unit": "Count",
      "timeseries": [
        {
          "metadatavalues": [
            {
              "name": {
                "value": "apiname",
                "localizedValue": "apiname"
              },
              "value": "GetBlobProperties"
            }
          ],
          "data": [
            {
              "timeStamp": "2017-09-19T02:00:00Z",
              "total": 2
            },
            {
              "timeStamp": "2017-09-19T02:01:00Z",
              "total": 1
            },
            {
              "timeStamp": "2017-09-19T02:02:00Z",
              "total": 3
            },
            {
              "timeStamp": "2017-09-19T02:03:00Z",
              "total": 7
            },
            {
              "timeStamp": "2017-09-19T02:04:00Z",
              "total": 2
            }
          ]
        },
        ...
      ]
    }
  ],
  "namespace": "Microsoft.Storage/storageAccounts",
  "resourceregion": "eastus"
}
```

## <a name="retrieve-metric-definitions"></a>Metrische definities ophalen

Gebruik de [definities voor metrische gegevens van Azure Monitor REST-API](https://msdn.microsoft.com/library/mt743621.aspx) voor toegang tot de lijst met metrische gegevens die beschikbaar voor een service zijn.

**Methode**: GET

**Aanvraag-URI**: https://management.azure.com/subscriptions/ *{subscriptionId}*/resourceGroups/*{resourceGroupName}*/providers/*{resourceProviderNamespace}* / *{resourceType}*/*{resourceName}*/providers/microsoft.insights/metricDefinitions?api-version=*{apiVersion}*

Bijvoorbeeld, als u wilt ophalen van de metrische definities voor een logische App van Azure, zou de aanvraag er als volgt uitzien:

```PowerShell
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metricDefinitions?api-version=2016-03-01"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -OutFile ".\contosotweets-metricdef-results.json" `
                  -Verbose
```

> [!NOTE]
> Als u wilt ophalen van de metrische definities van de Azure Monitor REST API, '2016-03-01' als de API-versie te gebruiken.
>
>

De hoofdtekst van het resulterende JSON-antwoord is vergelijkbaar met het volgende voorbeeld:

```JSON
{
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metricdefinitions",
  "value": [
    {
      "name": {
        "value": "RunsStarted",
        "localizedValue": "Runs Started"
      },
      "category": "AllMetrics",
      "startTime": "0001-01-01T00:00:00Z",
      "endTime": "0001-01-01T00:00:00Z",
      "unit": "Count",
      "primaryAggregationType": "Total",
      "resourceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets",
      "resourceId": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets",
      "metricAvailabilities": [
        {
          "timeGrain": "PT1M",
          "retention": "P30D",
          "location": null,
          "blobLocation": null
        },
        {
          "timeGrain": "PT1H",
          "retention": "P30D",
          "location": null,
          "blobLocation": null
        }
      ],
      "properties": null,
      "dimensions": null,
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metricdefinitions/RunsStarted",
      "supportedAggregationTypes": [ "None", "Average", "Minimum", "Maximum", "Total", "Count" ]
    }
  ]
}
```

Zie voor meer informatie de [lijst van de metrische definities voor een resource in Azure Monitor REST API](https://msdn.microsoft.com/library/azure/mt743621.aspx) documentatie.

## <a name="retrieve-metric-values"></a>Metrische waarden ophalen

Zodra de beschikbare metrische definities bekend zijn, is het vervolgens mogelijk om op te halen van de gerelateerde metrische waarden. Van de metrische gegevens de naam 'value' (niet de ' localizedValue') gebruiken voor alle aanvragen filteren (bijvoorbeeld de metrische gegevens 'CpuTime' en 'Aanvragen' herstelpunten ophalen). Als er geen filters zijn opgegeven, wordt de metrische standaardgegevens geretourneerd.

> [!NOTE]
> Om op te halen metrische waarden met behulp van de Azure Monitor REST API, '2016-09-01' als de API-versie te gebruiken.
>
>

**Methode**: GET

**Aanvraag-URI**: https://management.azure.com/subscriptions/ *{abonnement-id}*/resourceGroups/*{resource-group-name}*/providers/*{resource-provider-naamruimte}* / *{resourcetype}*/*{resourcenaam}*/providers/microsoft.insights/metrics?$filter=*{filter}*& api-version =*{apiVersion}*

Bijvoorbeeld, als u wilt ophalen van de punten van de metrische gegevens RunsSucceeded voor het opgegeven tijdsbereik en een tijdgranulariteit op van 1 uur, zou de aanvraag zijn als volgt:

```PowerShell
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2017-08-18T19:00:00 and endTime eq 2017-08-18T23:00:00 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=2016-09-01"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contosotweets-metrics-results.json" `
    -Verbose
```

De hoofdtekst van het resulterende JSON-antwoord is vergelijkbaar met het volgende voorbeeld:

```JSON
{
  "value": [
    {
      "data": [
        {
          "timeStamp": "2017-08-18T19:00:00Z",
          "total": 0.0
        },
        {
          "timeStamp": "2017-08-18T20:00:00Z",
          "total": 159.0
        },
        {
          "timeStamp": "2017-08-18T21:00:00Z",
          "total": 174.0
        },
        {
          "timeStamp": "2017-08-18T22:00:00Z",
          "total": 97.0
        }
      ],
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/Microsoft.Insights/metrics/RunsSucceeded",
      "name": {
        "value": "RunsSucceeded",
        "localizedValue": "Runs Succeeded"
      },
      "type": "Microsoft.Insights/metrics",
      "unit": "Count"
    }
  ]
}
```

Om op te halen van meerdere punten van gegevens of aggregatie, voegen metrische definitie van namen en aggregatietypen aan het filter, zoals te zien is in het volgende voorbeeld:

```PowerShell
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2017-08-18T21:00:00 and endTime eq 2017-08-18T21:30:00 and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=2016-09-01"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contosotweets-metrics-multiple-results.json" `
    -Verbose
```

De hoofdtekst van het resulterende JSON-antwoord is vergelijkbaar met het volgende voorbeeld:

```JSON
{
  "value": [
    {
      "data": [
        {
          "timeStamp": "2017-08-18T21:03:00Z",
          "total": 5.0,
          "average": 1.0
        },
        {
          "timeStamp": "2017-08-18T21:04:00Z",
          "total": 7.0,
          "average": 1.0
        }
      ],
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/Microsoft.Insights/metrics/ActionsCompleted",
      "name": {
        "value": "ActionsCompleted",
        "localizedValue": "Actions Completed "
      },
      "type": "Microsoft.Insights/metrics",
      "unit": "Count"
    },
    {
      "data": [
        {
          "timeStamp": "2017-08-18T21:03:00Z",
          "total": 5.0,
          "average": 1.0
        },
        {
          "timeStamp": "2017-08-18T21:04:00Z",
          "total": 7.0,
          "average": 1.0
        }
      ],
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/Microsoft.Insights/metrics/RunsSucceeded",
      "name": {
        "value": "RunsSucceeded",
        "localizedValue": "Runs Succeeded"
      },
      "type": "Microsoft.Insights/metrics",
      "unit": "Count"
    }
  ]
}
```

### <a name="use-armclient"></a>Gebruik ARMClient

Een aanvullende methode is het gebruik van [ARMClient](https://github.com/projectkudu/armclient) op uw Windows-computer. ARMClient verwerkt automatisch de Azure AD-verificatie (en de resulterende JWT-token). De volgende stappen beschrijven het gebruik van ARMClient voor het ophalen van metrische gegevens:

1. Installeer [Chocolatey](https://chocolatey.org/) en [ARMClient](https://github.com/projectkudu/armclient).
2. Typ in een terminalvenster *armclient.exe aanmelding*. In dat geval vraagt u zich aanmelden bij Azure.
3. Type *armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01*
4. Type *armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-09-01*

Bijvoorbeeld, om op te halen van de metrische definities voor een specifieke logische App, voert u de volgende opdracht uit:

```
armclient GET /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metricDefinitions?api-version=2016-03-01
```

## <a name="retrieve-the-resource-id"></a>De resource-ID ophalen

Met behulp van de REST-API kunt echt te begrijpen van de beschikbare metrische definities, het granulariteit en de bijbehorende waarden. Informatie is nuttig bij het gebruik van de [Azure Management-bibliotheek](https://msdn.microsoft.com/library/azure/mt417623.aspx).

Voor de bovenstaande code is de resource-ID voor het gebruik van het volledige pad naar de gewenste Azure-resource. Om te vragen op basis van een Azure-Web-App, kan de resource-ID zou zijn:

*/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{site-name}/*

De volgende lijst bevat enkele voorbeelden van resource-ID-notaties voor verschillende Azure-resources:

* **IoT Hub** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Devices/IotHubs/*{iot-hub-name}*
* **Elastische SQL-Pool** -/subscriptions/*{abonnement-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Sql/servers/*{pool-db}*/elasticpools/*{sql-pool-name}*
* **SQL-Database (v12)** -/subscriptions/*{abonnement-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Sql/servers/*{servernaam}*/databases/*{databasenaam}*
* **Service Bus** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.ServiceBus/*{namespace}*/*{servicebus-name}*
* **Virtuele-machineschaalsets** -/subscriptions/*{abonnement-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Compute/virtualMachineScaleSets/ *{vm-naam}*
* **VMs** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Compute/virtualMachines/*{vm-name}*
* **Event Hubs** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.EventHub/namespaces/*{eventhub-namespace}*

Er zijn alternatieve methoden voor het ophalen van de resource-ID, inclusief het gebruik van Azure Resource Explorer, de gewenste resource weergeven in Azure portal en via PowerShell of Azure CLI.

### <a name="azure-resource-explorer"></a>Azure Resource Explorer

U vindt de resource-ID voor een gewenste resource, een handige methode is het gebruik van de [Azure Resource Explorer](https://resources.azure.com) hulpprogramma. Navigeer naar de gewenste resource en zoek vervolgens naar de ID die wordt weergegeven, zoals in de volgende schermafbeelding:

![ALT "Azure Resource Explorer"](./media/rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a>Azure Portal

De resource-ID kan ook worden verkregen vanuit de Azure-portal. Om dit te doen, gaat u naar de gewenste resource en klik op Eigenschappen. De Resource-ID wordt weergegeven in de sectie met eigenschappen, zoals weergegeven in de volgende schermafbeelding:

![ALT "Resource-ID weergegeven op de blade eigenschappen in de Azure-portal'](./media/rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a>Azure PowerShell

De resource-ID kan worden opgehaald met behulp van Azure PowerShell-cmdlets. Bijvoorbeeld, als u de resource-ID voor een logische App van Azure, voert u de cmdlet Get-AzureLogicApp, zoals in het volgende voorbeeld:

```PowerShell
Get-AzLogicApp -ResourceGroupName azmon-rest-api-walkthrough -Name contosotweets
```

Het resultaat moeten vergelijkbaar met het volgende voorbeeld:

```
Id             : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets
Name           : ContosoTweets
Type           : Microsoft.Logic/workflows
Location       : centralus
ChangedTime    : 8/21/2017 6:58:57 PM
CreatedTime    : 8/18/2017 7:54:21 PM
AccessEndpoint : https://prod-08.centralus.logic.azure.com:443/workflows/f3a91b352fcc47e6bff989b85446c5db
State          : Enabled
Definition     : {$schema, contentVersion, parameters, triggers...}
Parameters     : {[$connections, Microsoft.Azure.Management.Logic.Models.WorkflowParameter]}
SkuName        :
AppServicePlan :
PlanType       :
PlanId         :
Version        : 08586982649483762729
```

### <a name="azure-cli"></a>Azure-CLI

Om op te halen van de resource-ID voor een Azure Storage-account met de Azure CLI, voert u de opdracht 'az storage account show' uit zoals wordt weergegeven in het volgende voorbeeld:

```
az storage account show -g azmon-rest-api-walkthrough -n contosotweets2017
```

Het resultaat moeten vergelijkbaar met het volgende voorbeeld:

```JSON
{
  "accessTier": null,
  "creationTime": "2017-08-18T19:58:41.840552+00:00",
  "customDomain": null,
  "enableHttpsTrafficOnly": false,
  "encryption": null,
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/contosotweets2017",
  "identity": null,
  "kind": "Storage",
  "lastGeoFailoverTime": null,
  "location": "centralus",
  "name": "contosotweets2017",
  "networkAcls": null,
  "primaryEndpoints": {
    "blob": "https://contosotweets2017.blob.core.windows.net/",
    "file": "https://contosotweets2017.file.core.windows.net/",
    "queue": "https://contosotweets2017.queue.core.windows.net/",
    "table": "https://contosotweets2017.table.core.windows.net/"
  },
  "primaryLocation": "centralus",
  "provisioningState": "Succeeded",
  "resourceGroup": "azmon-rest-api-walkthrough",
  "secondaryEndpoints": null,
  "secondaryLocation": "eastus2",
  "sku": {
    "name": "Standard_GRS",
    "tier": "Standard"
  },
  "statusOfPrimary": "available",
  "statusOfSecondary": "available",
  "tags": {},
  "type": "Microsoft.Storage/storageAccounts"
}
```

> [!NOTE]
> Met Azure Logic Apps zijn nog niet beschikbaar is via de Azure CLI, dus een Azure Storage-account wordt weergegeven in het voorgaande voorbeeld.
>
>

## <a name="retrieve-activity-log-data"></a>Ophalen van gegevens van een activiteitenlogboek

Naast de metrische definities en bijbehorende waarden is het ook mogelijk met gebruik van de Azure Monitor REST API om op te halen van andere interessante inzichten met betrekking tot Azure-resources. Bijvoorbeeld: het is mogelijk om query [activiteitenlogboek](https://msdn.microsoft.com/library/azure/dn931934.aspx) gegevens. Het volgende voorbeeld ziet u de Azure Monitor REST API om gegevens van een activiteitenlogboek query binnen een bepaald datumbereik te gebruiken voor een Azure-abonnement:

```PowerShell
$apiVersion = "2015-04-01"
$filter = "eventTimestamp ge '2017-08-18' and eventTimestamp le '2017-08-19'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -Verbose
```

## <a name="next-steps"></a>Volgende stappen

* Controleer de [overzicht van de bewaking](../../azure-monitor/overview.md).
* Weergave de [ondersteunde metrische gegevens met Azure Monitor](metrics-supported.md).
* Controleer de [Microsoft Azure REST API-verwijzing bewaken](https://msdn.microsoft.com/library/azure/dn931943.aspx).
* Controleer de [Azure Management-bibliotheek](https://msdn.microsoft.com/library/azure/mt417623.aspx).
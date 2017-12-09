---
title: Overzicht van Azure Monitoring REST-API | Microsoft Docs
description: "Hoe aanvragen verifiëren en de REST-API van Azure Monitor gebruikt voor het ophalen van de beschikbare metrische definities en metrische waarden."
author: mcollier
manager: 
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 565e6a88-3131-4a48-8b82-3effc9a3d5c6
ms.service: monitoring-and-diagnostics
ms.workload: 
ms.tgt_pltfrm: 
ms.devlang: 
ms.search.region: 
ms.search.scope: 
ms.search.validFrom: 
ms.dyn365.ops.version: 
ms.topic: article
ms.date: 09/18/2017
ms.author: mcollier
ms.openlocfilehash: 357a63c65a4f6864dca259aad8a76f83681cd501
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="azure-monitoring-rest-api-walkthrough"></a>Azure Monitoring REST-API-overzicht
In dit artikel wordt beschreven hoe u verificatie uitvoeren zodat uw code kunt u de [Microsoft Azure Monitor REST API-verwijzing](https://msdn.microsoft.com/library/azure/dn931943.aspx).         

De Monitor Azure API stelt u de beschikbare standaardregels metrische definities, granulatie en metrische waarden niet ophalen. De gegevens kunnen worden opgeslagen in een afzonderlijke gegevensarchief, zoals Azure SQL Database, Azure Cosmos DB of Azure Data Lake. Van daaruit kan indien nodig aanvullende analyse worden uitgevoerd.

Naast het werken met verschillende punten van de metrische gegevens, maakt de API Monitor ook het mogelijk voor een lijst met regels voor waarschuwingen, logboeken van de activiteit bekijken en nog veel meer. Zie voor een volledige lijst met beschikbare bewerkingen, de [Microsoft Azure Monitor REST API-verwijzing](https://msdn.microsoft.com/library/azure/dn931943.aspx).

## <a name="authenticating-azure-monitor-requests"></a>Verifiërende Azure Monitor aanvragen
De eerste stap is het verifiëren van de aanvraag.

Alle taken die worden uitgevoerd met de API van Azure-Monitor voor het Azure Resource Manager verificatiemodel te gebruiken. Alle aanvragen moeten daarom worden geverifieerd bij Azure Active Directory (Azure AD). Eén manier om te verifiëren van de clienttoepassing is een Azure AD-service-principal maken en ophalen van het verificatietoken (JWT). Het volgende voorbeeldscript wordt gedemonstreerd maken van een Azure AD-service principal via PowerShell. Voor een meer gedetailleerde procedure, Raadpleeg de documentatie op [Azure PowerShell gebruiken voor het maken van een service-principal voor toegang tot bronnen](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-password). Het is ook mogelijk om te [maken van een service-principal via Azure portal](../azure-resource-manager/resource-group-create-service-principal-portal.md).

```PowerShell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"

# Authenticate to a specific Azure subscription.
Login-AzureRmAccount -SubscriptionId $subscriptionId

# Password for the service principal
$pwd = "{service-principal-password}"

# Create a new Azure AD application
$azureAdApplication = New-AzureRmADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $pwd

# Create a new service principal associated with the designated application
New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role to the newly created service principal
New-AzureRmRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

Om te vragen van de Monitor-API van Azure, moet de clienttoepassing de eerder gemaakte service-principal voor verificatie gebruiken. Het volgende voorbeeld PowerShell-script toont een benadert, met behulp van de [Active Directory Authentication Library](../active-directory/active-directory-authentication-libraries.md) (ADAL) om op te halen van de JWT-token voor verificatie. De JWT-token is doorgegeven als onderdeel van een HTTP-autorisatie-parameter in aanvragen aan de Monitor REST-API van Azure.

```PowerShell
$azureAdApplication = Get-AzureRmADApplication -IdentifierUri "https://localhost/azure-monitor"

$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId

$clientId = $azureAdApplication.ApplicationId.Guid
$tenantId = $subscription.TenantId
$authUrl = "https://login.microsoftonline.com/${tenantId}"

$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($clientId, $pwd)

$result = $AuthContext.AcquireToken("https://management.core.windows.net/", $cred)

# Build an array of HTTP header values
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

Als de verificatie is gelukt, worden query's vervolgens met de REST-API van Azure Monitor uitgevoerd. Er zijn twee handig query's:

1. Lijst van de metrische definities voor een bron
2. Ophalen van de metrische waarden

## <a name="retrieve-metric-definitions-multi-dimensional-api"></a>Metrische definities (Multi-dimensionale API) ophalen

Gebruik de [Azure Monitor metriek definities REST-API](https://docs.microsoft.com/rest/api/monitor/metricdefinitions) voor toegang tot de lijst met metrische gegevens die beschikbaar voor een service zijn.

**Methode**: ophalen

**URI van de aanvraag**: https://management.azure.com/subscriptions/*{subscriptionId}*/resourceGroups/*{resourceGroupName}*/providers/*{ resourceProviderNamespace}*/*{resourceType}*/*{resourceName*/providers/microsoft.insights/metricDefinitions? API-version =*{apiVersion}*

Bijvoorbeeld: voor het ophalen van de metrische definities voor een Azure Storage-account, de aanvraag zou als volgt uitzien:

```PowerShell
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/accounts/ContosoStorage/providers/microsoft.insights/metricDefinitions?api-version=2017-05-01-preview"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -OutFile ".\contosostorage-metricdef-results.json" `
                  -Verbose

```
> [!NOTE]
> Voor het ophalen van metrische definities met behulp van de multi-dimensionale Azure Monitor metrische gegevens REST-API '2017-05-01-preview' als de API-versie te gebruiken.
>
>

De resulterende JSON-antwoordtekst zou zijn vergelijkbaar met het volgende voorbeeld: (Let erop dat de tweede metriek dimensies heeft)

```JSON
{
    "value": [
        {
            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/accounts/ContosoStorage/providers/microsoft.insights/metricdefinitions/UsedCapacity",
            "resourceId": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/accounts/ContosoStorage",
            "category": "Capacity",
            "name": {
                "value": "UsedCapacity",
                "localizedValue": "Used capacity"
            },
            "isDimensionRequired": false,
            "unit": "Bytes",
            "primaryAggregationType": "Average",
            "metricAvailabilities": [
                {
                    "timeGrain": "PT1M",
                    "retention": "P30D"
                },
                {
                    "timeGrain": "PT1H",
                    "retention": "P30D"
                }
            ]
        },
        {
            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/accounts/ContosoStorage/providers/microsoft.insights/metricdefinitions/Transactions",
            "resourceId": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/accounts/ContosoStorage",
            "category": "Transaction",
            "name": {
                "value": "Transactions",
                "localizedValue": "Transactions"
            },
            "isDimensionRequired": false,
            "unit": "Count",
            "primaryAggregationType": "Total",
            "metricAvailabilities": [
                {
                    "timeGrain": "PT1M",
                    "retention": "P30D"
                },
                {
                    "timeGrain": "PT1H",
                    "retention": "P30D"
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
Zodra de beschikbare metrische definities gekend zijn, worden sommige metrische gegevens die dimensies hebben. Voordat u een query uitvoert voor de metrische gegevens die u wilt detecteren welke het bereik van waarden heeft een dimensie. Op basis van deze dimensiewaarden die u er vervolgens voor kiezen kunt om te filteren of segment de metrische gegevens op basis van dimensiewaarden tijdens het opvragen van metrische gegevens. De metriek naam 'value' (niet de ' localizedValue') gebruiken voor elke filtering aanvragen (bijvoorbeeld de punten van de metrische gegevens 'CpuTime' en 'Aanvragen' ophalen). Als er geen filters zijn opgegeven, wordt de metriek standaard geretourneerd.

> [!NOTE]
> Voor het ophalen van waarden van de dimensie met de REST-API van Azure Monitor '2017-05-01-preview' als de API-versie te gebruiken.
>
>

**Methode**: ophalen

**URI van de aanvraag**: https://management.azure.com/subscriptions/*{abonnement-id}*/resourceGroups/*{resource-group-name}*/providers/*{ resource provider naamruimte}*/*{resourcetype}*/*{resourcenaam}*/providers/microsoft.insights/metrics?metric= *{metriek}*& timespan =*{starttime/endtime}*& $filter =*{filter}*& resultType = metagegevens & api-version =*{ apiVersion}*

Bijvoorbeeld: voor het ophalen van de lijst met mogelijke waarden van de dimensie van het API-naam voor de metriek 'Transacties' in een bepaalde periode, de aanvraag zou zijn als volgt:

```PowerShell
$filter = "APIName eq '*'"
$request = "https://management.azure.com/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/accounts/ContosoStorage/providers/microsoft.insights/metrics?metric=Transactions&timespan=2017-09-01T00:00:00Z/2017-09-10T00:00:00Z&resultType=metadata&$filter=${filter}&api-version=2017-05-01-preview"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contosostorage-dimension-values.json" `
    -Verbose
```
De resulterende JSON-antwoordtekst zou zijn vergelijkbaar met het volgende voorbeeld:

```JSON
{
  "timespan": "2017-09-01T00:00:00Z/2017-09-10T00:00:00Z",
  "value": [
    {
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/accounts/ContosoStorage/providers/Microsoft.Insights/metrics/Transactions",
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
        {
          "metadatavalues": [
            {
              "name": {
                "value": "apiname",
                "localizedValue": "apiname"
              },
              "value": "PutPage"
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
              "value": "Unknown"
            }
          ]
        },
        ...
      ]    
    }
  ]
}
```

## <a name="retrieve-metric-values-multi-dimensional-api"></a>Ophalen van metrische waarden (Multi-dimensionale API)
Zodra de beschikbare metrische definities en de mogelijke waarden gekend zijn, is deze mogelijk voor het ophalen van de verwante metrische waarden. De metriek naam 'value' (niet de ' localizedValue') gebruiken voor alle aanvragen filteren. Als er geen dimensiefilters zijn opgegeven, wordt de samengevouwen samengevoegde metrische gegevens geretourneerd.

> [!NOTE]
> Voor het ophalen van multidimensionale metrische waarden met de REST-API van Azure Monitor '2017-05-01-preview' als de API-versie te gebruiken.
>
>

**Methode**: ophalen

**URI van de aanvraag**: https://management.azure.com/subscriptions/*{abonnement-id}*/resourceGroups/*{resource-group-name}*/providers/*{ resource provider naamruimte}*/*{resourcetype}*/*{resourcenaam}*/providers/microsoft.insights/metrics?metric= *{metriek}*& timespan =*{starttime/endtime}*& $filter =*{filter}*& interval =*{timeGrain}* & aggregatie =*{aggreation}*& api-version =*{apiVersion}*

Bijvoorbeeld, om op te halen van de metrische waarden de opslag 'Transacties' metriek tijdens een bereik 5 min voor alle transacties naar de API-naam 'GetBlobProperties', zou de aanvraag zijn als volgt:

```PowerShell
$filter = "APIName eq 'GetBlobProperties'"
$request = "https://management.azure.com/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/accounts/ContosoStorage/providers/microsoft.insights/metrics?metric=Transactions&timespan=2017-09-19T02:00:00Z/2017-09-19T02:05:00Z&$filter=${filter}&interval=PT1M&aggregation=Count&api-version=2017-05-01-preview"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contosostorage-metric-values.json" `
    -Verbose
```
De resulterende JSON-antwoordtekst zou zijn vergelijkbaar met het volgende voorbeeld:

```JSON
{
  "cost": 0,
  "timespan": "2017-09-19T02:00:00Z/2017-09-19T02:05:00Z",
  "interval": "PT1M",
  "value": [
    {
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/accounts/ContosoStorage/providers/Microsoft.Insights/metrics/Transactions",
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
              "count": 2.0
            },
            {
              "timeStamp": "2017-09-19T02:01:00Z",
              "count": 1.0
            },
            {
              "timeStamp": "2017-09-19T02:02:00Z",
              "count": 3.0
            },
            {
              "timeStamp": "2017-09-19T02:03:00Z",
              "count": 7.0
            },
            {
              "timeStamp": "2017-09-19T02:04:00Z",
              "count": 2.0
            }
          ]
        }
      ]
    }
  ]
}
```

## <a name="retrieve-metric-definitions"></a>Metrische definities ophalen
Gebruik de [Azure Monitor metriek definities REST-API](https://msdn.microsoft.com/library/mt743621.aspx) voor toegang tot de lijst met metrische gegevens die beschikbaar voor een service zijn.

**Methode**: ophalen

**URI van de aanvraag**: https://management.azure.com/subscriptions/*{subscriptionId}*/resourceGroups/*{resourceGroupName}*/providers/*{ resourceProviderNamespace}*/*{resourceType}*/*{resourceName}*/providers/microsoft.insights/metricDefinitions? API-version =*{apiVersion}*

Bijvoorbeeld: voor het ophalen van de metrische definities voor Azure Logic App, de aanvraag zou als volgt uitzien:

```PowerShell
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metricDefinitions?api-version=2016-03-01"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -OutFile ".\contostweets-metricdef-results.json" `
                  -Verbose
```
> [!NOTE]
> Voor het ophalen van metrische definities met de REST-API van Azure Monitor '2016-03-01' als de API-versie te gebruiken.
>
>

De resulterende JSON-antwoordtekst zou zijn vergelijkbaar met het volgende voorbeeld:
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

Zie voor meer informatie de [lijst van de metrische definities voor een resource in de REST-API van Azure-Monitor](https://msdn.microsoft.com/library/azure/mt743621.aspx) documentatie.

## <a name="retrieve-metric-values"></a>Metrische waarden ophalen
Zodra de beschikbare metrische definities gekend zijn, is deze mogelijk de verwante metrische waarden ophalen. De metriek naam 'value' (niet de ' localizedValue') gebruiken voor elke filtering aanvragen (bijvoorbeeld de punten van de metrische gegevens 'CpuTime' en 'Aanvragen' ophalen). Als er geen filters zijn opgegeven, wordt de metriek standaard geretourneerd.

> [!NOTE]
> Voor het ophalen van metrische waarden met de REST-API van Azure Monitor '2016-09-01' als de API-versie te gebruiken.
>
>

**Methode**: ophalen

**URI van de aanvraag**: https://management.azure.com/subscriptions/*{abonnement-id}*/resourceGroups/*{resource-group-name}*/providers/*{resource provider naamruimte}*/*{resourcetype}*/*{resourcenaam}*/providers/microsoft.insights/metrics?$filter=*{filter}*& api-version =*{apiVersion}*

Bijvoorbeeld: voor het ophalen van de punten van de metrische gegevens RunsSucceeded voor het opgegeven tijdsbereik en voor een tijdgranulariteit op van 1 uur, de aanvraag zou zijn als volgt:

```PowerShell
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2017-08-18T19:00:00 and endTime eq 2017-08-18T23:00:00 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metrics?$filter=${filter}&api-version=2016-09-01"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contostweets-metrics-results.json" `
    -Verbose
```

De resulterende JSON-antwoordtekst zou zijn vergelijkbaar met het volgende voorbeeld:

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

Voor het ophalen van meerdere gegevens of aggregatie punten toevoegen metrische definitie namen en aggregatietypen aan het filter, zoals in het volgende voorbeeld:

```PowerShell
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2017-08-18T21:00:00 and endTime eq 2017-08-18T21:30:00 and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metrics?$filter=${filter}&api-version=2016-09-01"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contostweets-metrics-multiple-results.json" `
    -Verbose
```
De resulterende JSON-antwoordtekst zou zijn vergelijkbaar met het volgende voorbeeld:

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
Een aanvullende methode is het gebruik van [ARMClient](https://github.com/projectkudu/armclient) op uw Windows-machine. ARMClient verwerkt automatisch de Azure AD-verificatie (en de resulterende JWT-token). De volgende stappen geven een overzicht van het gebruik van ARMClient voor het ophalen van de metrische gegevens:

1. Installeer [Chocolatey](https://chocolatey.org/) en [ARMClient](https://github.com/projectkudu/armclient).
2. Typ in een terminalvenster *armclient.exe aanmelding*. In dat geval vraagt u zich aanmelden bij Azure.
3. Type *armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01*
4. Type *armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-09-01*

Om op te halen van de metrische definities voor een specifieke logische App, verleent u bijvoorbeeld de volgende opdracht:
```
armclient GET /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metricDefinitions?api-version=2016-03-01
```


## <a name="retrieve-the-resource-id"></a>De resource-ID ophalen
Met de REST API kunt zeker dat u begrijpt de beschikbare metrische definities, granulatie en bijbehorende waarden. Informatie is nuttig wanneer u de [Azure Management-bibliotheek](https://msdn.microsoft.com/library/azure/mt417623.aspx).

De resource-ID is voor de bovenstaande code het volledige pad naar de gewenste Azure-resource. Om te doorzoeken op basis van een Azure-Web-App, kan de resource-ID zou zijn:

*/Subscriptions/{Subscription-id}/resourceGroups/{resource-Group-Name}/providers/Microsoft.Web/sites/{site-name}/*

De volgende lijst bevat enkele voorbeelden van resource-ID-notaties voor verschillende Azure-resources:

* **IoT Hub** -/subscriptions/*{abonnement-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Devices/IotHubs/*{iot-hub-name}*
* **Elastische SQL-groep** -/subscriptions/*{abonnement-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Sql/servers/*{pool-db}*/elasticpools/*{sql-pool-name}*
* **SQL-Database (v12)** -/subscriptions/*{abonnement-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Sql/servers/*{servernaam}*/databases/*{databasenaam}*
* **Service Bus** -/subscriptions/*{abonnement-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.ServiceBus/*{naamruimte}*/*{servicebus-name}*
* **Virtuele-machineschaalsets** -/subscriptions/*{abonnement-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Compute/virtualMachineScaleSets/ *{vm-naam}*
* **Virtuele machines** -/subscriptions/*{abonnement-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Compute/virtualMachines/*{vm-naam}*
* **Event Hubs** -/subscriptions/*{abonnement-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.EventHub/namespaces/*{eventhub-naamruimte}*

Er zijn alternatieve methoden voor het ophalen van de resource-ID, inclusief het gebruik van Azure Resource Explorer, de gewenste resource weergeven in de Azure portal en via PowerShell of Azure CLI.

### <a name="azure-resource-explorer"></a>Azure Resource Explorer
De resource-ID voor een gewenste resource vindt een handig aanpak is het gebruik van de [Azure Resource Explorer](https://resources.azure.com) hulpprogramma. Navigeer naar de gewenste resource en zoek vervolgens naar de ID die wordt weergegeven, zoals in de volgende schermafbeelding:

![ALT 'Azure Resource Explorer'](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a>Azure Portal
De resource-ID kan ook worden verkregen via de Azure-portal. Om dit te doen, gaat u naar de gewenste resource en klik op Eigenschappen. De Resource-ID wordt weergegeven in de sectie eigenschappen, zoals in de volgende schermafbeelding:

![ALT "Resource-ID weergegeven in de blade eigenschappen in de Azure portal](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a>Azure PowerShell
De resource-ID kan worden opgehaald met behulp van Azure PowerShell-cmdlets. Bijvoorbeeld: voor de resource-ID voor een Azure Logic App uitvoeren van de cmdlet Get-AzureLogicApp, zoals in het volgende voorbeeld:

```PowerShell
Get-AzureRmLogicApp -ResourceGroupName azmon-rest-api-walkthrough -Name contosotweets
```

Het resultaat moet er ongeveer als volgt:
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


### <a name="azure-cli"></a>Azure CLI
Voor het ophalen van de resource-ID voor een Azure Storage-account met de Azure CLI, geef dan de opdracht 'az storage-account weergeven, zoals weergegeven in het volgende voorbeeld:

```
az storage account show -g azmon-rest-api-walkthrough -n contosotweets2017
```

Het resultaat moet er ongeveer als volgt:
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
> Logische Apps van Azure nog niet beschikbaar is via de Azure CLI, dus een Azure Storage-account wordt weergegeven in het voorgaande voorbeeld.
>
>

## <a name="retrieve-activity-log-data"></a>Ophalen van de logboekgegevens van de activiteit
Naast de metrische definities en het bijbehorende waarden is het ook mogelijk om te gebruiken de REST-API van de Azure-Monitor voor het ophalen van extra interessante inzichten die betrekking hebben op Azure-resources. Als u bijvoorbeeld het is mogelijk om de query [activiteitenlogboek](https://msdn.microsoft.com/library/azure/dn931934.aspx) gegevens. Het volgende voorbeeld wordt gedemonstreerd met behulp van de Monitor REST API van Azure naar de logboekgegevens van query activiteit binnen een bepaald datumbereik voor een Azure-abonnement:

```PowerShell
$apiVersion = "2015-04-01"
$filter = "eventTimestamp ge '2017-08-18' and eventTimestamp le '2017-08-19'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&$filter=${filter}"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -Verbose
```

## <a name="next-steps"></a>Volgende stappen
* Controleer de [overzicht van de bewaking](monitoring-overview.md).
* Weergave de [ondersteund met een Azure-Monitor](monitoring-supported-metrics.md).
* Controleer de [Microsoft Azure bewaken REST API-verwijzing](https://msdn.microsoft.com/library/azure/dn931943.aspx).
* Controleer de [Azure Management-bibliotheek](https://msdn.microsoft.com/library/azure/mt417623.aspx).

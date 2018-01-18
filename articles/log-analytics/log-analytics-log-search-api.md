---
title: Meld u logboekanalyse search REST-API | Microsoft Docs
description: Deze handleiding biedt een eenvoudige zelfstudie beschrijven hoe kunt u de zoekopdracht logboekanalyse REST-API in de Operations Management Suite (OMS) en biedt voorbeelden van hoe u de opdrachten gebruiken.
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: b4e9ebe8-80f0-418e-a855-de7954668df7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: bwren
ms.openlocfilehash: 0ca80408f8e8b2dae7ff35d50b3d2c41ae54d3d3
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="log-analytics-log-search-rest-api"></a>Log Analytics logboek search REST-API

> [!IMPORTANT]
> Als uw werkruimte is bijgewerkt naar de [querytaal van nieuwe logboekanalyse](log-analytics-log-search-upgrade.md), en u naar verwijzen moet de [documentatie voor de nieuwe versie van de zoekopdracht logboek API](https://dev.loganalytics.io/).  Deze oudere API toch werkt met een bijgewerkte werkruimte, maar dit zal binnenkort wel depracated.  U moet eventuele bestaande oplossingen voor het gebruik van de nieuwe API wijzigen.

Deze handleiding biedt een eenvoudige zelfstudie, inclusief voorbeelden van hoe u de Log Analytics Search REST-API kunt. Log Analytics maakt deel uit van de Operations Management Suite (OMS).


## <a name="overview-of-the-log-search-rest-api"></a>Overzicht van de zoekopdracht logboek REST-API
Log Analytics Search REST-API RESTful is en toegankelijk zijn via de API van Azure Resource Manager. Dit artikel vindt u voorbeelden van de toegang tot de API via [ARMClient](https://github.com/projectkudu/ARMClient), een open-source-opdrachtregelprogramma dat vereenvoudigt de Azure Resource Manager-API wordt aangeroepen. Het gebruik van ARMClient is een van de vele opties voor toegang tot de Log Analytics zoeken-API. Een andere optie is het gebruik van de Azure PowerShell-module voor OperationalInsights, waaronder de cmdlets voor toegang tot zoeken. Met deze hulpprogramma's, kunt u gebruikmaken van de Azure Resource Manager-API aanroepen met OMS-werkruimten en uitvoeren van opdrachten in deze zoekopdracht. De API levert zoekresultaten in JSON-indeling, zodat u kunt de lijst met zoekresultaten programmatisch op veel verschillende manieren gebruiken.

De Azure Resource Manager kan worden gebruikt een [-bibliotheek voor .NET](https://msdn.microsoft.com/library/azure/dn910477.aspx) en de [REST-API](https://msdn.microsoft.com/library/azure/mt163658.aspx). Bekijk voor meer informatie, de gekoppelde webpagina's.

> [!NOTE]
> Als u een aggregatie-opdracht zoals gebruiken `|measure count()` of `distinct`, elke aanroep voor de zoekopdracht kan resulteren in een Resourcegroepnaam 500.000 records. Zoekopdrachten dat een aggregatie-opdracht niet opneemt retourneren maximaal 5.000 records.
>
>

## <a name="basic-log-analytics-search-rest-api-tutorial"></a>Basic Log Analytics Search REST-API-zelfstudie
### <a name="to-use-armclient"></a>ARMClient gebruiken
1. Installeer [Chocolatey](https://chocolatey.org/), dit is een Open Source Package Manager voor Windows. Open een opdrachtpromptvenster als administrator en voer vervolgens de volgende opdracht:

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```
2. ARMClient installeren met de volgende opdracht:

    ```
    choco install armclient
    ```

### <a name="to-perform-a-search-using-armclient"></a>Een zoekopdracht met ARMClient uitvoeren
1. Meld u aan met uw Microsoft-account of de account van uw werk of school:

    ```
    armclient login
    ```

    Een geslaagde aanmelding geeft een lijst van alle abonnementen die zijn gekoppeld aan het opgegeven account:

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```
2. Haal de werkruimten Operations Management Suite:

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    Een geslaagde Get-aanvraag zou alle werkruimten die zijn gekoppeld aan het abonnement uitvoer:

    ```
    {
    "value": [
    {
      "properties": {
    "source": "External",
    "customerId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "portalUrl": "https://eus.login.mms.microsoft.com/SignIn.aspx?returnUrl=https%3a%2f%2feus.mms.microsoft.com%2fMain.aspx%3fcid%xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      },
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/{WORKSPACE NAME}",
      "name": "{WORKSPACE NAME}",
      "type": "Microsoft.OperationalInsights/workspaces",
      "location": "East US"
       ]
    }
    ```
3. Uw zoekopdracht-variabele maken:

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. Zoeken met behulp van de variabele van uw nieuwe zoekopdracht:

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a>Meld u voorbeelden van Analytics Search REST-API-verwijzing
De volgende voorbeelden ziet u hoe u kunt de Search-API.

### <a name="search---actionread"></a>Zoeken - actie leestijd
**Voorbeeld-Url:**

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

**Aanvraag:**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```
De volgende tabel beschrijft de eigenschappen die beschikbaar zijn.

| **Eigenschap** | **Beschrijving** |
| --- | --- |
| Boven |Het maximum aantal resultaten te retourneren. |
| markeren |Bevat vóór en na-parameters, meestal gebruikt voor de overeenkomende velden markeren |
| vooraf |De opgegeven tekenreeks voor de overeenkomende velden prefixes. |
| Verzenden |Voegt de opgegeven tekenreeks met de overeenkomende velden. |
| query |De query gebruikt voor het verzamelen en retourneren van resultaten. |
| start |Het begin van de periode die u wilt dat de resultaten uit die u wilt zoeken. |
| einde |Het einde van de periode die u wilt dat de resultaten uit die u wilt zoeken. |

**Antwoord:**

```
    {
      "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "__metadata" : {
        "resultType" : "raw",
        "total" : 1455,
        "top" : 150,
        "StartTime" : "2015-02-11T21:09:07.0345815Z",
        "Status" : "Successful",
        "LastUpdated" : "2015-02-11T21:09:07.331463Z",
        "CoreResponses" : [],
        "sort" : [{
          "name" : "TimeGenerated",
          "order" : "desc"
        }],
        "requestTime" : 450
      },
      "value": [{
        "SourceSystem" : "OpsManager",
        "TimeGenerated" : "2015-02-07T14:07:33Z",
        "Source" : "SideBySide",
        "EventLog" : "Application",
        "Computer" : "BAMBAM",
        "EventCategory" : 0,
        "EventLevel" : 1,
        "EventLevelName" : "Error",
        "UserName" : "N/A",
        "EventID" : 78,
        "MG" : "00000000-0000-0000-0000-000000000001",
        "TimeCollected" : "2015-02-07T14:10:04.69Z",
        "ManagementGroupName" : "AOI-5bf9a37f-e841-462b-80d2-1d19cd97dc40",
        "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "Type" : "Event",
        "__metadata" : {
          "Type" : "Event",
          "TimeGenerated" : "2015-02-07T14:07:33Z",
          "highlighting" : {
          "EventLevelName" : ["{[hl]}Error{[/hl]}"]
        }
      }]
    ],
            "start" : "2015-02-04T21:03:29.231Z",
            "end" : "2015-02-11T21:03:29.231Z"
          }
        }
      }]
    }
```

### <a name="searchid---actionread"></a>Zoek / {-ID} - actie leestijd
**De inhoud van een opgeslagen zoekopdracht aanvragen:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

> [!NOTE]
> Als de zoekopdracht wordt geretourneerd met de status 'In behandeling', kan zal een poll de bijgewerkte resultaten worden gedaan via deze API. Na 6 minimum, het resultaat van de zoekactie wordt verwijderd uit de cache en HTTP-geworden wordt geretourneerd. Als de aanvankelijke zoekaanvraag retourneert onmiddellijk de status 'Mislukt', worden de resultaten niet toegevoegd aan de cache waardoor deze API om terug te keren HTTP geworden als opgevraagd. De inhoud van een HTTP 200-resultaat zijn in dezelfde indeling als de eerste zoekaanvraag alleen met de bijgewerkte waarden.
>
>

### <a name="saved-searches"></a>Opgeslagen zoekopdrachten
**Lijst met opgeslagen zoekopdrachten aanvragen:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

Ondersteunde methoden: GET, PUT verwijderen

Ondersteunde methoden: ophalen

De volgende tabel beschrijft de eigenschappen die beschikbaar zijn.

| Eigenschap | Beschrijving |
| --- | --- |
| Id |De unieke id. |
| ETag |**Vereist voor Patch**. Bijgewerkt met de server bij elke schrijfbewerking. Waarde moet gelijk zijn aan de huidige waarde van de opgeslagen of ' *' om bij te werken. 409 geretourneerd voor waarden van de oude/ongeldig. |
| Properties.query |**Vereist**. De query. |
| properties.displayName |**Vereist**. De gebruiker gedefinieerde weergavenaam van de query. |
| Properties.Category |**Vereist**. De gebruiker gedefinieerde categorie van de query. |

> [!NOTE]
> De logboekanalyse search API retourneert momenteel gebruiker gemaakte opgeslagen zoekopdrachten wanneer gepeild voor opgeslagen zoekopdrachten in een werkruimte. De API retourneert geen opgeslagen zoekopdrachten geleverd door oplossingen.
>
>

### <a name="create-saved-searches"></a>Opgeslagen zoekopdrachten maken
**Aanvraag:**

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisismyid?api-version=2015-03-20 $savedSearchParametersJson
```

> [!NOTE]
> De naam op voor alle opgeslagen zoekopdrachten, schema's en acties die zijn gemaakt met de Log Analytics-API moet in kleine letters.

### <a name="delete-saved-searches"></a>Verwijder opgeslagen zoekopdrachten
**Aanvraag:**

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a>Opgeslagen zoekopdrachten bijwerken
 **Aanvraag:**

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a>Metagegevens - JSON alleen
Hier volgt een manier om de velden voor alle logboekbestanden typen voor de verzamelde gegevens in uw werkruimte te bekijken. Bijvoorbeeld, als u wilt dat u weet of het type gebeurtenis een veld met de naam Computer, klikt u vervolgens deze query is één manier om te controleren.

**Aanvraag voor velden:**

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

**Antwoord:**

```
    {
      "__metadata" : {
        "schema" : {
          "name" : "Example Name",
          "version" : 2
        },
        "resultType" : "schema",
        "requestTime" : 35
      },
      "value" : [{
          "name" : "MG",
          "displayName" : "MG",
          "type" : "Guid",
          "facetable" : true,
          "display" : false,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Capacity_SMBUtilizationByHost", "Capacity_ArrayUtilization", "Capacity_SMBShareUtilization", "SQLAssessmentRecommendation", "Event", "ConfigurationChange", "ConfigurationAlert", "ADAssessmentRecommendation", "ConfigurationObject", "ConfigurationObjectProperty"]
        }, {
          "name" : "ManagementGroupName",
          "displayName" : "ManagementGroupName",
          "type" : "String",
          "facetable" : true,
          "display" : true,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Event", "ConfigurationChange", "ConfigurationAlert", "W3CIISLog", "AlertHistory", "Recommendation", "Alert", "SecurityEvent", "UpdateAgent", "RequiredUpdate", "ConfigurationObject", "ConfigurationObjectProperty"]
        }
      ]
    }
```

De volgende tabel beschrijft de eigenschappen die beschikbaar zijn.

| **Eigenschap** | **Beschrijving** |
| --- | --- |
| naam |Veldnaam. |
| displayName |De weergavenaam van het veld. |
| type |Het Type van de veldwaarde. |
| geschikt voor facetten |De combinatie van huidige 'indexed', ' opgeslagen ' en 'facet' Eigenschappen. |
| Weergavenaam |Huidige 'weergeven'-eigenschap. True als het veld wordt weergegeven in de zoekopdracht. |
| ownerType |Beperkt tot alleen typen die behoren tot de vrijgegeven IP's. |

## <a name="optional-parameters"></a>Optionele parameters
De volgende informatie beschrijft de optionele parameters beschikbaar.

### <a name="highlighting"></a>Markeren
De parameter "Highlight" is een optionele parameter die u kunt gebruiken om aan te vragen van het subsysteem zoeken omvatten een reeks markeringen in zijn reactie.

Deze markeringen geven het begin en eindigen gemarkeerde tekst die overeenkomt met de voorwaarden die is opgegeven in uw query.
U kunt de begin- en markeringen die worden gebruikt door de zoekfunctie inpakken van de gemarkeerde term opgeven.

**Voorbeeld van de zoekopdracht**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```

**Resultaat van de steekproef:**

```
    {
        "TimeGenerated":
        "2015-05-18T23:55:59Z", "Source":
        "EventLog": "Application",
        "Computer": "smokedturkey.net",
        "EventCategory": 0,
        "EventLevel":1,
        "EventLevelName":
        "Error"
        "Manager ", "__metadata":
        {
            "Type": "Event",
            "TimeGenerated": "2015-05-18T23:55:59Z",
            "highlighting": {
                "EventLevelName":
                ["{[hl]}Error{[/hl]}"]
            }
        }
    }
```

U ziet dat het vorige resultaat bevat een foutbericht weergegeven dat is voorafgegaan en toegevoegd.

## <a name="computer-groups"></a>Computergroepen
Computergroepen zijn speciale opgeslagen zoekopdrachten die resulteren in een set computers.  U kunt een computergroep in andere query's gebruiken om de resultaten op de computers in de groep te beperken.  Een computergroep is geïmplementeerd als een zoekopdracht met een code van de groep met een waarde van de Computer.

Hier volgt een voorbeeldantwoord voor een computergroep.

```
    "etag": "W/\"datetime'2016-04-01T13%3A38%3A04.7763203Z'\"",
    "properties": {
        "Category": "My Computer Groups",
        "DisplayName": "My Computer Group",
        "Query": "srv* | Distinct Computer",
        "Tags": [{
            "Name": "Group",
            "Value": "Computer"
          }],
    "Version": 1
    }
```

### <a name="retrieving-computer-groups"></a>Bij het ophalen van computergroepen
Voor het ophalen van een computergroep, gebruikt u de Get-methode met de id van de groep.

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a>Maken of bijwerken van een computergroep
Gebruik voor het maken een computergroep de Put-methode met een opgeslagen zoekopdracht unieke ID. Als u een bestaande computer-ID gebruikt, is dat een gewijzigd. Wanneer u een computergroep in de portal Log Analytics maakt, wordt de ID van de groep en de naam gemaakt.

De query die wordt gebruikt voor de groepsdefinitie van de moet een verzameling van computers voor de groep te laten functioneren retourneren.  Het raadzaam dat u uw query met beëindigen `| Distinct Computer` om te controleren of de juiste gegevens geretourneerd.

De definitie van de opgeslagen zoekopdracht moet een groep met de naam met een waarde van de Computer voor de zoekopdracht worden geclassificeerd als een computergroep code bevatten.

```
    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson
```

### <a name="deleting-computer-groups"></a>Computergroepen verwijderen
Een als computergroep wilt verwijderen, gebruikt u de Delete-methode met de id van de groep.

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) om query's op basis van aangepaste velden voor de criteria samen te stellen.

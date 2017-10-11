---
title: REST-API's van Resource Manager | Microsoft Docs
description: Een overzicht van de REST-API's van Resource Manager-verificatie- en gebruiksgegevens-voorbeelden
services: azure-resource-manager
documentationcenter: na
author: navalev
manager: timlt
editor: 
ms.assetid: e8d7a1d2-1e82-4212-8288-8697341408c5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/13/2017
ms.author: navale;tomfitz;
ms.openlocfilehash: 2f7ba23775545637de865f9ef63680ae22c62164
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="resource-manager-rest-apis"></a>REST API's voor Resource Manager
> [!div class="op_single_selector"]
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [Azure-CLI](xplat-cli-azure-resource-manager.md)
> * [Portal](resource-group-portal.md) 
> * [REST API](resource-manager-rest-api.md)
> 
> 

Achter elke aanroep in Azure Resource Manager achter elke geïmplementeerde sjabloon achter elke geconfigureerde storage-account zijn er een of meer aanroepen naar de Azure Resource Manager-RESTful-API. In dit onderwerp wordt besteed aan de API's en hoe u ze kunt aanroepen zonder een SDK helemaal. Deze methode is handig als u volledig beheer van aanvragen naar Azure wilt, of als de SDK voor uw voorkeurstaal niet beschikbaar is of biedt geen ondersteuning voor de bewerkingen die u nodig.

In dit artikel verder niet via elke API die wordt weergegeven in Azure, maar in plaats daarvan maakt gebruik van bepaalde bewerkingen als voorbeelden van hoe u verbinding mee te maken. Nadat u de basisbeginselen, kunt u lezen de [Azure Resource Manager REST API Reference](https://docs.microsoft.com/rest/api/resources/) voor gedetailleerde informatie over het gebruik van de rest van de API's.

## <a name="authentication"></a>Authentication
Verificatie voor Resource Manager is verwerkt door Azure Active Directory (AD). Als u wilt verbinding maken met API's, moet u eerst om te verifiëren met Azure AD ontvangt geen verificatietoken die u aan elke aanvraag doorgeven kunt. Als we beschrijving van een aanroep van pure rechtstreeks aan de REST API's, gaan we ervan uit dat u niet verifiëren wilt met een gebruikersnaam en wachtwoord wordt gevraagd. We ook wordt ervan uitgegaan dat u geen gebruikmaakt van twee mechanismen voor factor-verificatie. We maken daarom, wat een Azure AD-toepassing en een service principal die worden gebruikt voor het aanmelden wordt aangeroepen. Maar houd er rekening mee dat Azure AD ondersteunen verschillende procedures voor verificatie en deze kan worden gebruikt om op te halen die verificatietoken die we nodig hebben voor de volgende API-aanvragen.
Ga als volgt [maken van Azure AD-toepassing en Service-Principal](resource-group-create-service-principal-portal.md) voor stapsgewijze instructies.

### <a name="generating-an-access-token"></a>Genereren van een toegangstoken
Verificatie met Azure AD wordt uitgevoerd door Azure AD, die zich bevindt op login.microsoftonline.com. Om te verifiëren, moet u de volgende informatie:

* Azure AD-Tenant met ID (de naam van die Azure AD dat u bent aan te melden, vaak hetzelfde zijn als uw bedrijf worden gebruikt, maar niet nodig)
* Toepassings-ID (die zijn uitgevoerd tijdens de stap het maken van Azure AD toepassing)
* Wachtwoord (die u hebt geselecteerd tijdens het maken van de Azure AD-toepassing)

Zorg dat u 'Azure AD-Tenant-ID', 'Toepassings-ID' en 'Password' vervangen door de juiste waarden in de volgende HTTP-aanvraag.

**Algemene HTTP-aanvraag:**

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

... wordt (als de verificatie slaagt) leiden tot een reactie vergelijkbaar met het volgende antwoord:

```json
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1448199959",
  "not_before": "1448196059",
  "resource": "https://management.core.windows.net/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhb...86U3JI_0InPUk_lZqWvKiEWsayA"
}
```
(De access_token in het vorige antwoord zijn ingekort voor een verhoging van de leesbaarheid)

**Genereren met behulp van Bash toegangstoken:**

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net/&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

**Genereren met behulp van PowerShell toegangstoken:**

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

Het antwoord bevat een toegangstoken, informatie over hoe lang dat token geldig is en informatie over welke resource kunt u dat token voor.
Het toegangstoken dat u hebt ontvangen in de vorige aanroep van de HTTP-moet voor alle aanvragen worden doorgegeven aan de Resource Manager-API. Als u doorgeeft als een headerwaarde met de naam "Autorisatie" met de waarde 'Bearer YOUR_ACCESS_TOKEN'. U ziet de ruimte tussen 'Bearer' en uw toegangstoken.

Als u in het bovenstaande HTTP-resultaat zien kunt, is het token is ongeldig voor een bepaalde periode gedurende welke u moet in de cache en dat hetzelfde token opnieuw gebruiken. Zelfs als het is mogelijk om te verifiëren met Azure AD voor elke API-aanroep, zou het zeer inefficiënt zijn.

## <a name="calling-resource-manager-rest-apis"></a>Aanroepen van Resource Manager REST-API 's
In dit onderwerp wordt alleen enkele API's gebruikt om uit te leggen het basisgebruik van de REST-bewerkingen. Zie voor meer informatie over de bewerkingen [Azure Resource Manager REST API's](https://docs.microsoft.com/rest/api/resources/).

### <a name="list-all-subscriptions"></a>Lijst van alle abonnementen
Een van de meest eenvoudige bewerkingen die u kunt doen is voor een lijst met de beschikbare abonnementen waartoe u toegang hebt. In de volgende aanvraag ziet u hoe het toegangstoken is doorgegeven als een header:

(YOUR_ACCESS_TOKEN vervangen door uw werkelijke Access Token).

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

... en daardoor het ophalen van een lijst met abonnementen dat is toegestaan voor toegang tot deze service-principal

(Abonnement-id's zijn ingekort voor leesbaarheid)

```json
{
  "value": [
    {
      "id": "/subscriptions/3a8555...555995",
      "subscriptionId": "3a8555...555995",
      "displayName": "Azure Subscription",
      "state": "Enabled",
      "subscriptionPolicies": {
        "locationPlacementId": "Internal_2014-09-01",
        "quotaId": "Internal_2014-09-01"
      }
    }
  ]
}
```

### <a name="list-all-resource-groups-in-a-specific-subscription"></a>Lijst van alle resourcegroepen in een specifiek abonnement
Alle resources die beschikbaar zijn met de Resource Manager-API's zijn genest binnen een resourcegroep. U kunt een Resource Manager query voor bestaande resourcegroepen in uw abonnement met de volgende HTTP GET-aanvraag. U ziet hoe de abonnements-ID is doorgegeven als onderdeel van de URL van deze tijd.

(YOUR_ACCESS_TOKEN en SUBSCRIPTION_ID vervangen door uw werkelijke access token en de abonnement-ID)

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

Het antwoord dat u afhankelijk van of u gedefinieerd bronnengroepen hebt en zo ja, hoeveel.

(Abonnement-id's zijn ingekort voor leesbaarheid)

```json
{
    "value": [
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/myfirstresourcegroup",
            "name": "myfirstresourcegroup",
            "location": "eastus",
            "properties": {
                "provisioningState": "Succeeded"
            }
        },
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/mysecondresourcegroup",
            "name": "mysecondresourcegroup",
            "location": "northeurope",
            "tags": {
                "tagname1": "My first tag"
            },
            "properties": {
                "provisioningState": "Succeeded"
            }
        }
    ]
}
```

### <a name="create-a-resource-group"></a>Een resourcegroep maken
Tot nu toe hebben we alleen is opvragen van de Resource Manager-API's voor meer informatie. Is het tijd we sommige resources maken en u begint met de eenvoudigste ze allemaal zijn, een resourcegroep. De volgende HTTP-aanvraag maakt een resourcegroep in een regio of de locatie van uw keuze en voegt een label aan.

(YOUR_ACCESS_TOKEN, SUBSCRIPTION_ID, RESOURCE_GROUP_NAME vervangen door uw werkelijke Access Token, abonnements-ID en naam van de resourcegroep die u wilt maken)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  }
}
```

Als dit lukt, kunt u een reactie die vergelijkbaar is met het volgende antwoord krijgen:

```json
{
  "id": "/subscriptions/3a8555...555995/resourceGroups/RESOURCE_GROUP_NAME",
  "name": "RESOURCE_GROUP_NAME",
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  },
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

U hebt een resourcegroep gemaakt in Azure. Gefeliciteerd.

### <a name="deploy-resources-to-a-resource-group-using-a-resource-manager-template"></a>Resources aan een resourcegroep met een Resource Manager-sjabloon implementeren
Met Resource Manager kunt u uw resources met behulp van sjablonen implementeren. Een sjabloon worden gedefinieerd voor verschillende resources en de bijbehorende afhankelijkheden. Voor deze sectie we ervan uitgaan dat u bekend bent met Resource Manager-sjablonen en we alleen beschreven hoe u de API-aanroep implementatie te starten. Zie voor meer informatie over het maken van sjablonen [Azure Resource Manager-sjablonen samenstellen](resource-group-authoring-templates.md).

Implementatie van een sjabloon verschillen niet veel hoe u andere API niet aanroepen. Een belangrijk aspect is dat de implementatie van een sjabloon kan erg lang duren. De API-aanroep slechts retourneert en het is aan u als ontwikkelaar query voor de status van de implementatie nagaan wanneer de implementatie is voltooid. Zie voor meer informatie [bijhouden asynchrone Azure bewerkingen](resource-manager-async-operations.md).

In dit voorbeeld gebruiken we een openbaar blootgestelde sjabloon beschikbaar is op [GitHub](https://github.com/Azure/azure-quickstart-templates). De sjabloon we gebruiken implementeert een Linux-VM voor de regio VS-West. Hoewel dit voorbeeld gebruikt een beschikbare sjabloon in een openbare zoals GitHub-opslagplaats, kunt u in plaats daarvan de volledige sjabloon doorgeven als onderdeel van de aanvraag. Houd er rekening mee dat we parameterwaarden in de aanvraag die worden gebruikt in de geïmplementeerde sjabloon opgeven.

(SUBSCRIPTION_ID, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD en DNS_NAME_FOR_PUBLIC_IP vervangen op waarden die geschikt is voor uw aanvraag)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME/providers/microsoft.resources/deployments/DEPLOYMENT_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "properties": {
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json",
      "contentVersion": "1.0.0.0",
    },
    "mode": "Incremental",
    "parameters": {
        "newStorageAccountName": {
          "value": "GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME"
        },
        "adminUsername": {
          "value": "ADMIN_USER_NAME"
        },
        "adminPassword": {
          "value": "ADMIN_PASSWORD"
        },
        "dnsNameForPublicIP": {
          "value": "DNS_NAME_FOR_PUBLIC_IP"
        },
        "ubuntuOSVersion": {
          "value": "15.04"
        }
    }
  }
}
```

Het lang JSON-antwoord voor deze aanvraag is weggelaten om de leesbaarheid van deze documentatie te verbeteren. Het antwoord bevat informatie over de sjablonen implementatie die u hebt gemaakt.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het verwerken van asynchrone REST-bewerkingen, [bijhouden asynchrone Azure bewerkingen](resource-manager-async-operations.md).

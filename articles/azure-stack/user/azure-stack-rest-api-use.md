---
title: Gebruik de Azure API-Stack | Microsoft Docs
description: Informatie over het ophalen van een verificatie van Azure API-aanvragen in Azure Stack aanbrengen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/14/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: e8a9489a3f487a45303bac45f805381b41427b4b
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
<!--  cblackuk and charliejllewellyn. This is a community contribution by cblackuk-->

# <a name="use-the-azure-stack-api"></a>De Azure API-Stack gebruiken

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt de Azure Stack Interface API (Application Programming) gebruiken om bewerkingen zoals Webfeedindeling marketplace-items te automatiseren.

De API vereist dat de client om te verifiëren voor het eindpunt van Microsoft Azure-aanmelding. Het eindpunt retourneert een token voor gebruik in de koptekst van elke aanvraag verzonden naar de Stack-API van Azure. Oauth 2.0 maakt gebruik van Microsoft Azure.

Dit artikel vindt u voorbeelden die gebruikmaken van de **cURL** hulpprogramma voor het maken van Azure-Stack-aanvragen. De toepassing, cURL, is een opdrachtregelprogramma met een bibliotheek voor het overbrengen van gegevens. Deze voorbeelden is doorlopen van het proces van het ophalen van een token voor toegang tot de Azure-Stack-API. De meeste programmeertalen bieden Oauth 2.0-bibliotheken die robuust token taken voor beheer en de ingang die het vernieuwen van het token.

Bekijk het hele proces van het gebruik van de Stack REST-API van Azure met een algemene REST-client, zoals **cURL**waarmee u inzicht krijgen in de onderliggende aanvragen en ziet u wat u kunt verwachten in een nettolading antwoord ontvangen.

In dit artikel verkennen niet alle opties die beschikbaar zijn voor het ophalen van tokens zoals interactieve aanmelding of specifieke App-id's maken. Zie voor informatie over deze onderwerpen, [Azure REST API-verwijzing](https://docs.microsoft.com/rest/api/).

## <a name="get-a-token-from-azure"></a>Een token verkrijgen van Azure

Maak een aanvraagtekst geformatteerd met het type inhoud x-1-800-www-Dell-form-urlencoded verkrijgen van een toegangstoken. Uw aanvraag om het eindpunt Azure REST Authentication en meld u aan te plaatsen.

### <a name="uri"></a>URI

```bash  
POST https://login.microsoftonline.com/{tenant id}/oauth2/token
```

**Tenant-ID** is:

 - Uw tenant-domein, zoals `fabrikam.onmicrosoft.com`
 - Uw tenant-ID, zoals `8eaed023-2b34-4da1-9baa-8bc8c9d6a491`
 - De standaardwaarde voor tenant-onafhankelijke sleutels: `common`

### <a name="post-body"></a>Post-instantie

```bash  
grant_type=password
&client_id=1950a258-227b-4e31-a9cf-717495945fc2
&resource=https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155
&username=admin@fabrikam.onmicrosoft.com
&password=Password123
&scope=openid
```

Voor elke waarde:

 - **grant_type**  
    Het type verificatieschema u gaat gebruiken. In dit voorbeeld wordt is de waarde `password`

 - **Resource**  
    De resource het token wordt opgevraagd. U vindt de resource een query uitgevoerd op het Azure-Stack management metagegevens-eindpunt. Bekijk de **doelgroepen** sectie

 - **Azure Stack-management-eindpunt**  
    ```
    https://management.{region}.{Azure Stack domain}/metadata/endpoints?api-version=2015-01-01
    ```

  > [!NOTE]  
  > Als u een beheerder toegang wilt krijgen tot de tenant-API moet vervolgens u controleren eindpunt te gebruiken tenant, bijvoorbeeld: `https://adminmanagement.{region}.{Azure Stack domain}/metadata/endpoints?api-version=2015-01-011`  

  Bijvoorbeeld, met de Azure-Stack Development Kit als een eindpunt:

    ```bash
    curl 'https://management.local.azurestack.external/metadata/endpoints?api-version=2015-01-01'
    ```

  Antwoord:

  ```
  {
  "galleryEndpoint":"https://adminportal.local.azurestack.external:30015/",
  "graphEndpoint":"https://graph.windows.net/",
  "portalEndpoint":"https://adminportal.local.azurestack.external/",
  "authentication":{
      "loginEndpoint":"https://login.windows.net/",
      "audiences":["https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155"]
      }
  }
  ```

### <a name="example"></a>Voorbeeld

  ```
  https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155
  ```

  **client_id**

  Deze waarde is vastgelegd op een standaardwaarde:

  ```
  1950a258-227b-4e31-a9cf-717495945fc2
  ```

  Alternatieve opties zijn beschikbaar voor specifieke scenario's:

  
  | Toepassing | ApplicationID |
  | --------------------------------------- |:-------------------------------------------------------------:|
  | LegacyPowerShell | 0a7bdc5c-7b57-40be-9939-d4c5fc7cd417 |
  | PowerShell | 1950a258-227b-4e31-a9cf-717495945fc2 |
  | WindowsAzureActiveDirectory | 00000002-0000-0000-c000-000000000000 |
  | VisualStudio | 872cd9fa-d31f-45e0-9eab-6e460a02d1f1 |
  | AzureCLI | 04b07795-8ddb-461a-bbee-02f9e1bf7b46 |

  **Gebruikersnaam**

  De Azure-Stack AAD-account:

  ```
  azurestackadmin@fabrikam.onmicrosoft.com
  ```

  **Wachtwoord**

  De Azure-Stack AAD Administrator-wachtwoord.

### <a name="example"></a>Voorbeeld

Aanvraag:

```
curl -X "POST" "https://login.windows.net/fabrikam.onmicrosoft.com/oauth2/token" \
-H "Content-Type: application/x-www-form-urlencoded" \
--data-urlencode "client_id=1950a258-227b-4e31-a9cf-717495945fc2" \
--data-urlencode "grant_type=password" \
--data-urlencode "username=admin@fabrikam.onmicrosoft.com" \
--data-urlencode 'password=Password12345' \
--data-urlencode "resource=https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155"
```

Antwoord:

```
{
  "token_type": "Bearer",
  "scope": "user_impersonation",
  "expires_in": "3599",
  "ext_expires_in": "0",
  "expires_on": "1512574780",
  "not_before": "1512570880",
  "resource": "https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155",
  "access_token": "eyJ0eXAiOi...truncated for readability..."
}
```

## <a name="api-queries"></a>API-query 's

Nadat u uw toegangstoken ontvangt, moet u als een koptekst toevoegen aan elk van uw API-aanvragen. Als u wilt doen, moet u een koptekst maken **autorisatie** met waarde: `Bearer <access token>`. Bijvoorbeeld:

Aanvraag:

```bash  
curl -H "Authorization: Bearer eyJ0eXAiOi...truncated for readability..." 'https://adminmanagement.local.azurestack.external/subscriptions?api-version=2016-05-01'
```

Antwoord:

```bash  
offerId : /delegatedProviders/default/offers/92F30E5D-F163-4C58-8F02-F31CFE66C21B
id : /subscriptions/800c4168-3eb1-406b-a4ca-919fe7ee42e8
subscriptionId : 800c4168-3eb1-406b-a4ca-919fe7ee42e8
tenantId : 9fea4606-7c07-4518-9f3f-8de9c52ab628
displayName : Default Provider Subscription
state : Enabled
subscriptionPolicies : @{locationPlacementId=AzureStack}
```

### <a name="url-structure-and-query-syntax"></a>De syntaxis van de structuur en query van de URL

Algemene aanvraag-URI, bestaat uit: {URI-schema} :// {URI-host} / {bronpad}? {queryreeks}

- **URI-schema**:  
De URI geeft aan dat het protocol dat wordt gebruikt om de aanvraag te verzenden. Bijvoorbeeld: `http` of `https`.
- **URI host**:  
De host geeft de domeinnaam of het IP-adres van de server waarop de REST-service-eindpunt wordt gehost, zoals `graph.microsoft.com` of `adminmanagement.local.azurestack.external`.
- **Bronpad**:  
Het pad bevat de resource of resourceverzameling, waaronder verschillende segmenten die worden gebruikt door de service bij het bepalen van de selectie van deze bronnen. Bijvoorbeeld: `beta/applications/00003f25-7e1f-4278-9488-efc7bac53c4a/owners` query uitvoeren op de lijst met eigenaren van een specifieke toepassing binnen de verzameling toepassingen kunnen worden gebruikt.
- **Querytekenreeks**:  
De tekenreeks biedt extra eenvoudige parameters, zoals de API-versie of resource selectiecriteria.

## <a name="azure-stack-request-uri-construct"></a>Azure-Stack aanvraag-URI constructie

```
{URI-scheme} :// {URI-host} / {subscription id} / {resource group} / {provider} / {resource-path} ? {OPTIONAL: filter-expression} {MANDATORY: api-version}
```

### <a name="uri-syntax"></a>URI-syntaxis

```
https://adminmanagement.local.azurestack.external/{subscription id}/resourcegroups/{resource group}/providers/{provider}/{resource-path}?{api-version}
```

### <a name="query-uri-example"></a>Voorbeeld van de query-URI

```
https://adminmanagement.local.azurestack.external/subscriptions/800c4168-3eb1-406b-a4ca-919fe7ee42e8/resourcegroups/system.local/providers/microsoft.infrastructureinsights.admin/regionhealths/local/Alerts?$filter=(Properties/State eq 'Active') and (Properties/Severity eq 'Critical')&$orderby=Properties/CreatedTimestamp desc&api-version=2016-05-01"
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van de RESTful Azure-eindpunten [Azure REST API-verwijzing](https://docs.microsoft.com/rest/api/).

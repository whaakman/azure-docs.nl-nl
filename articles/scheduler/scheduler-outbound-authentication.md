---
title: Uitgaande verificatie - Azure Scheduler
description: Meer informatie over het instellen of verwijderen van uitgaande verificatie voor Azure Scheduler
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: 6707f82b-7e32-401b-a960-02aae7bb59cc
ms.topic: article
ms.date: 08/15/2016
ms.openlocfilehash: 88f2fe0781bad4b652826b6a8d1961dd39b063e1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46993326"
---
# <a name="outbound-authentication-for-azure-scheduler"></a>Uitgaande verificatie voor Azure Scheduler

> [!IMPORTANT]
> [Met Azure Logic Apps](../logic-apps/logic-apps-overview.md) vervangt Azure Scheduler, is buiten gebruik gesteld. Voor het plannen van taken, [Azure Logic Apps in plaats daarvan probeert](../scheduler/migrate-from-scheduler-to-logic-apps.md). 

Azure Scheduler-taken mogelijk services waarvoor verificatie, zoals andere Azure-services, Salesforce.com, Facebook en beveiligde aangepaste websites worden aangeroepen. De naam service kan bepalen of de aangevraagde resources toegang heeft tot de Scheduler-taak. 

Scheduler biedt ondersteuning voor deze verificatiemodellen: 

* *Clientcertificaat* verificatie bij het gebruik van SSL/TLS-clientcertificaten
* *Basic* verificatie
* *Active Directory-OAuth* verificatie

## <a name="add-or-remove-authentication"></a>Toevoegen of verwijderen van verificatie

* Als u wilt toevoegen aan een job Scheduler verificatie bij het maken of bijwerken van de taak, de `authentication` JavaScript Object Notation (JSON) onderliggend element aan de `request` element. 

  Antwoorden retourneren nooit geheimen die worden doorgegeven aan de Scheduler-service via een PUT, PATCH of POST-aanvraag in de `authentication` object. 
  Antwoorden geheime informatie ingesteld op null of een openbare-sleuteltoken die staat voor de geverifieerde entiteit kunnen gebruiken. 

* Voor verificatie van een Scheduler-taak te verwijderen, expliciet een PUT of PATCH-aanvraag worden uitgevoerd op de taak en stel de `authentication` object op null. Het antwoord niet verificatie-eigenschappen worden opgenomen.

## <a name="client-certificate"></a>Clientcertificaat

### <a name="request-body---client-certificate"></a>Hoofdtekst van de aanvraag - clientcertificaat

Bij het toevoegen van verificatie met behulp van de `ClientCertificate` model, geeft u deze aanvullende elementen in de aanvraagtekst.  

| Element | Vereist | Beschrijving |
|---------|----------|-------------|
| **verificatie** (bovenliggend element) | De verificatieobject voor het gebruik van een SSL-clientcertificaat |
| **type** | Ja | Het verificatietype. De waarde voor SSL-clientcertificaten heeft `ClientCertificate`. |
| **pfx** | Ja | De met base64 gecodeerde inhoud van het PFX-bestand |
| **Wachtwoord** | Ja | Het wachtwoord voor toegang tot het PFX-bestand |
||| 

### <a name="response-body---client-certificate"></a>Antwoordtekst - clientcertificaat 

Wanneer een aanvraag wordt verzonden met de verificatie-informatie, bevat het antwoord deze verificatie-elementen.

| Element | Beschrijving | 
|---------|-------------| 
| **verificatie** (bovenliggend element) | De verificatieobject voor het gebruik van een SSL-clientcertificaat |
| **type** | Het verificatietype. De waarde voor SSL-clientcertificaten heeft `ClientCertificate`. |
| **certificateThumbprint** |Vingerafdruk van het certificaat |
| **certificateSubjectName** |Het certificaat onderwerp DN-naam |
| **certificateExpiration** | Vervaldatum van het certificaat |
||| 

### <a name="sample-rest-request---client-certificate"></a>Voorbeeld van REST-aanvraag - clientcertificaat

```json
PUT https://management.azure.com/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "clientcertificate",
          "password": "password",
          "pfx": "pfx key"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled"
  }
}
```

### <a name="sample-rest-response---client-certificate"></a>Antwoord in de voorbeeld-REST - clientcertificaat

```json
HTTP/1.1 200 OKCache-Control: no-cache
Pragma: no-cache
Content-Length: 858
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 56c7b40e-721a-437e-88e6-f68562a73aa8
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 1075219e-e879-4030-bc81-094e54fbabce
x-ms-routing-request-id: WESTUS:20160316T190424Z:1075219e-e879-4030-bc81-094e54fbabce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:04:23 GMT

{
  "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
  "type": "Microsoft.Scheduler/jobCollections/jobs",
  "name": "southeastasiajc/httpjob",
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "certificateThumbprint": "88105CG9DF9ADE75B835711D899296CB217D7055",
          "certificateExpiration": "2021-01-01T07:00:00Z",
          "certificateSubjectName": "CN=Scheduler Mgmt",
          "type": "ClientCertificate"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
    "status": {
      "nextExecutionTime": "2016-03-16T19:05:00Z",
      "executionCount": 0,
      "failureCount": 0,
      "faultedCount": 0
    }
  }
}
```

## <a name="basic"></a>Basic

### <a name="request-body---basic"></a>Aanvraagtekst - Basic

Bij het toevoegen van verificatie met behulp van de `Basic` model, geeft u deze aanvullende elementen in de aanvraagtekst.

| Element | Vereist | Beschrijving |
|---------|----------|-------------|
| **verificatie** (bovenliggend element) | De verificatieobject voor het gebruik van basisverificatie | 
| **type** | Ja | Het verificatietype. De waarde voor basisverificatie wordt gebruikt, heeft `Basic`. | 
| **gebruikersnaam** | Ja | De gebruikersnaam om te verifiëren | 
| **Wachtwoord** | Ja | Het wachtwoord voor verificatie |
|||| 

### <a name="response-body---basic"></a>Antwoordtekst - Basic

Wanneer een aanvraag wordt verzonden met de verificatie-informatie, bevat het antwoord deze verificatie-elementen.

| Element | Beschrijving | 
|---------|-------------|
| **verificatie** (bovenliggend element) | De verificatieobject voor het gebruik van basisverificatie |
| **type** | Het verificatietype. De waarde voor basisverificatie wordt gebruikt, heeft `Basic`. |
| **gebruikersnaam** | De geverifieerde gebruikersnaam |
||| 

### <a name="sample-rest-request---basic"></a>Voorbeeld van REST-aanvraag - Basic

```json
PUT https://management.azure.com/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 562
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "basic",
          "username": "user",
          "password": "password"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled"
  }
}
```

### <a name="sample-rest-response---basic"></a>Voorbeeld van een REST-antwoord: Basic

```json
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 701
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: a2dcb9cd-1aea-4887-8893-d81273a8cf04
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 7816f222-6ea7-468d-b919-e6ddebbd7e95
x-ms-routing-request-id: WESTUS:20160316T190506Z:7816f222-6ea7-468d-b919-e6ddebbd7e95
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:05:06 GMT

{  
   "id":"/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "username":"user1",
               "type":"Basic"
            }
         },
         "type":"Http"
      },
      "recurrence":{  
         "frequency":"Minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"Enabled",
      "status":{  
         "nextExecutionTime":"2016-03-16T19:06:00Z",
         "executionCount":0,
         "failureCount":0,
         "faultedCount":0
      }
   }
}
```

## <a name="active-directory-oauth"></a>Active Directory OAuth

### <a name="request-body---active-directory-oauth"></a>Aanvraagtekst - Active Directory-OAuth 

Bij het toevoegen van verificatie met behulp van de `ActiveDirectoryOAuth` model, geeft u deze aanvullende elementen in de aanvraagtekst.

| Element | Vereist | Beschrijving |
|---------|----------|-------------|
| **verificatie** (bovenliggend element) | Ja | De verificatieobject voor het gebruik van ActiveDirectoryOAuth verificatie |
| **type** | Ja | Het verificatietype. De waarde voor ActiveDirectoryOAuth verificatie, heeft `ActiveDirectoryOAuth`. |
| **tenant** | Ja | De tenant-id voor de Azure AD-tenant. Uitvoeren als u zoekt de tenant-id voor de Azure AD-tenant, `Get-AzureAccount` in Azure PowerShell. |
| **Doelgroep** | Ja | Deze waarde is ingesteld op `https://management.core.windows.net/`. | 
| **ClientId** | Ja | De client-id voor de Azure AD-toepassing | 
| **secret** | Ja | Het geheim voor de client die het token is aangevraagd | 
|||| 

### <a name="response-body---active-directory-oauth"></a>Antwoordtekst - Active Directory-OAuth

Wanneer een aanvraag wordt verzonden met de verificatie-informatie, bevat het antwoord deze verificatie-elementen.

| Element | Beschrijving |
|---------|-------------|
| **verificatie** (bovenliggend element) | De verificatieobject voor het gebruik van ActiveDirectoryOAuth verificatie |
| **type** | Het verificatietype. De waarde voor ActiveDirectoryOAuth verificatie, heeft `ActiveDirectoryOAuth`. | 
| **tenant** | De tenant-id voor de Azure AD-tenant |
| **Doelgroep** | Deze waarde is ingesteld op `https://management.core.windows.net/`. |
| **ClientId** | De client-id voor de Azure AD-toepassing |
||| 

### <a name="sample-rest-request---active-directory-oauth"></a>Voorbeeld van REST-aanvraag - Active Directory-OAuth

```json
PUT https://management.azure.com/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 757
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "tenant":"microsoft.onmicrosoft.com",
          "audience":"https://management.core.windows.net/",
          "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
          "secret": "G6u071r8Gjw4V4KSibnb+VK4+tX399hkHaj7LOyHuj5=",
          "type":"ActiveDirectoryOAuth"
        }
      },
      "type": "Http"
    },
    "recurrence": {
      "frequency": "Minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "Enabled"
  }
}
```

### <a name="sample-rest-response---active-directory-oauth"></a>Voorbeeld van een REST-antwoord: Active Directory-OAuth

```json
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 885
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 86d8e9fd-ac0d-4bed-9420-9baba1af3251
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
x-ms-routing-request-id: WESTUS:20160316T191003Z:5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:10:02 GMT

{
   "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type": "Microsoft.Scheduler/jobCollections/jobs",
   "name": "southeastasiajc/httpjob",
   "properties": {
      "startTime": "2015-05-14T14:10:00Z",
      "action": {  
         "request": {
            "uri": "https://mywebserviceendpoint.com",
            "method": "GET",
            "headers": {  
               "x-ms-version": "2013-03-01"
            },
            "authentication": {  
               "tenant": "microsoft.onmicrosoft.com",
               "audience": "https://management.core.windows.net/",
               "clientId": "dc23e764-9be6-4a33-9b9a-c46e36f0c137",
               "type": "ActiveDirectoryOAuth"
            }
         },
         "type": "Http"
      },
      "recurrence": {  
         "frequency": "minute",
         "endTime": "2016-04-10T08:00:00Z",
         "interval": 1
      },
      "state": "Enabled",
      "status": {  
         "lastExecutionTime": "2016-03-16T19:10:00.3762123Z",
         "nextExecutionTime": "2016-03-16T19:11:00Z",
         "executionCount": 5,
         "failureCount": 5,
         "faultedCount": 1
      }
   }
}
```

## <a name="see-also"></a>Zie ook

* [Wat is Azure Scheduler?](scheduler-intro.md)
* [Azure Scheduler-concepten, -terminologie en -entiteitenhiërarchie](scheduler-concepts-terms.md)
* [Azure Scheduler-limieten, standaardwaarden en foutcodes](scheduler-limits-defaults-errors.md)
* [Azure Scheduler REST API](https://msdn.microsoft.com/library/mt629143)
* [Naslaginformatie over Azure Scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)

---
title: Afhandeling van uitzonderingen & fout logboekregistratie scenario - Azure Logic Apps | Microsoft Docs
description: Hier volgt een echte use-case over geavanceerde afhandeling van uitzonderingen en fout bij het aanmelden in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: hedidin
ms.author: b-hoedid
ms.reviewer: estfan, LADocs
ms.assetid: 63b0b843-f6b0-4d9a-98d0-17500be17385
ms.topic: article
ms.date: 07/29/2016
ms.openlocfilehash: 58e59e4faa135e24124f494d90437b49caa30129
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60599533"
---
# <a name="scenario-exception-handling-and-error-logging-for-logic-apps"></a>Scenario: Afhandeling van uitzonderingen en logboekregistratie van fouten voor logische apps

In dit scenario wordt beschreven hoe u een logische app voor betere ondersteuning bieden voor de afhandeling van uitzonderingen kunt uitbreiden. We hebben een concrete use case gebruikt om de vraag te beantwoorden: "Azure Logic Apps ondersteunt uitzondering en foutafhandeling?"

> [!NOTE]
> Het huidige Azure Logic Apps-schema biedt een standaardsjabloon voor antwoorden op actie. Deze sjabloon bevat zowel interne validatie en foutberichten die zijn geretourneerd door een API-app.

## <a name="scenario-and-use-case-overview"></a>Scenario en gebruik case-overzicht

Dit is de boodschap als de use-case voor dit scenario: 

Een bekende zorginstelling overbrugt heeft ons aan de ontwikkeling van een Azure-oplossing die een patiënt portal maken wilt met behulp van Microsoft Dynamics CRM Online. Ze nodig zijn voor het verzenden van afspraakrecords tussen de patiënt Dynamics CRM Online-portal en Salesforce. We zijn gevraagd om te gebruiken de [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) standaard voor alle patiëntendossiers.

Het project heeft twee belangrijke vereisten:  

* Een methode voor het melden van records die zijn verzonden vanaf de portal voor Dynamics CRM Online
* Een manier om eventuele fouten die zijn opgetreden in de werkstroom weer te geven

> [!TIP]
> Zie voor een op hoog niveau video over dit project, [Integration-gebruikersgroep](http://www.integrationusergroup.com/logic-apps-support-error-handling/ "Integration-gebruikersgroep").

## <a name="how-we-solved-the-problem"></a>Hoe we het probleem opgelost

We hebben gekozen voor [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/ "Azure Cosmos DB") als een opslagplaats voor het logboek- en fout-records (Cosmos DB verwijzen we naar records als documenten). Aangezien Azure Logic Apps een standaardsjabloon voor alle antwoorden is, hebben we dan niet om een aangepaste schema te maken. Er kan een API-app maken **invoegen** en **Query** voor zowel fouten en logbestanden records. We kunnen ook een schema definiëren voor elke binnen de API-app.  

Er is een andere vereiste records opschonen na een bepaalde datum. Cosmos DB is een eigenschap genaamd [Time to Live](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "Time to Live") (TTL), die hadden we om in te stellen een **Time to Live** waarde voor elke record of verzameling. Deze mogelijkheid hoefde handmatig om records te verwijderen in Cosmos DB.

> [!IMPORTANT]
> Voor deze zelfstudie, moet u een Cosmos DB-database en twee verzamelingen (logboekregistratie en fouten) maken.

## <a name="create-the-logic-app"></a>De logische app maken

De eerste stap is het maken van de logische app en de app in Logic App Designer openen. In dit voorbeeld gebruiken we bovenliggende / onderliggende logische apps. Stel dat we het bovenliggende element al hebt gemaakt en gaat maken van een onderliggende logische app.

Omdat we gaan de logboekrecord die afkomstig zijn uit Dynamics CRM Online, laten we beginnen aan de bovenkant. We moet gebruiken een **aanvragen** omdat het bovenliggende logische app wordt geactiveerd voor dit kind activeren.

### <a name="logic-app-trigger"></a>Logische app-trigger

We gebruiken een **aanvragen** activeren zoals wordt weergegeven in het volgende voorbeeld:

``` json
"triggers": {
        "request": {
          "type": "request",
          "kind": "http",
          "inputs": {
            "schema": {
              "properties": {
                "CRMid": {
                  "type": "string"
                },
                "recordType": {
                  "type": "string"
                },
                "salesforceID": {
                  "type": "string"
                },
                "update": {
                  "type": "boolean"
                }
              },
              "required": [
                "CRMid",
                "recordType",
                "salesforceID",
                "update"
              ],
              "type": "object"
            }
          }
        }
      },

```


## <a name="steps"></a>Stappen

We moeten de bron (aanvraag) van de patiënt record zich van de Dynamics CRM Online-portal.

1. Er moet een nieuwe afspraakrecord ophalen uit Dynamics CRM Online.

   De trigger die afkomstig zijn van CRM biedt met ons op via de **CRM PatentId**, **recordtype**, **Record bijgewerkt of nieuw** (nieuw of Booleaanse waarde bijwerken), en  **SalesforceId**. De **SalesforceId** kan niet null zijn omdat deze alleen wordt gebruikt voor een update.
   Krijgen we de CRM-record met behulp van de CRM **PatientID** en de **recordtype**.

2. Vervolgens moeten we onze Azure Cosmos DB SQL API-app toevoegen **InsertLogEntry** bewerking zoals hier wordt weergegeven in Logic App Designer.

   **Invoegen van logboekvermelding**

   ![Invoegen van logboekvermelding](media/logic-apps-scenario-error-and-exception-handling/lognewpatient.png)

   **Fout bij invoegen**

   ![Invoegen van logboekvermelding](media/logic-apps-scenario-error-and-exception-handling/insertlogentry.png)

   **Controleren op record fout bij maken**

   ![Voorwaarde](media/logic-apps-scenario-error-and-exception-handling/condition.png)

## <a name="logic-app-source-code"></a>Broncode van logische app

> [!NOTE]
> De volgende voorbeelden zijn alleen voorbeelden. Omdat deze zelfstudie is gebaseerd op een implementatie nu in productie, de waarde van een **bronknooppunt** mogelijk niet weergegeven voor eigenschappen die zijn gerelateerd aan het plannen van een afspraak. > 

### <a name="logging"></a>Logboekregistratie

Het volgende codevoorbeeld voor logische app laat zien hoe voor het afhandelen van logboekregistratie.

#### <a name="log-entry"></a>Logboekvermelding

Hier volgt de broncode van de logische app voor het invoegen van een logboekvermelding.

``` json
"InsertLogEntry": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "date": "@{outputs('Gets_NewPatientRecord')['headers']['Date']}",
            "operation": "New Patient",
            "patientId": "@{triggerBody()['CRMid']}",
            "providerId": "@{triggerBody()['providerID']}",
            "source": "@{outputs('Gets_NewPatientRecord')['headers']}"
        },
        "method": "post",
        "uri": "https://.../api/Log"
        },
        "runAfter":    {
            "Gets_NewPatientecord": ["Succeeded"]
        }
}
```

#### <a name="log-request"></a>Logboek-aanvraag

Hier wordt het logboek aanvraagbericht in de API-app geplaatst.

``` json
    {
    "uri": "https://.../api/Log",
    "method": "post",
    "body": {
        "date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "operation": "New Patient",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "providerId": "",
        "source": "{/"Pragma/":/"no-cache/",/"x-ms-request-id/":/"e750c9a9-bd48-44c4-bbba-1688b6f8a132/",/"OData-Version/":/"4.0/",/"Cache-Control/":/"no-cache/",/"Date/":/"Fri, 10 Jun 2016 22:31:56 GMT/",/"Set-Cookie/":/"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1/",/"Server/":/"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0/",/"X-AspNet-Version/":/"4.0.30319/",/"X-Powered-By/":/"ASP.NET/",/"Content-Length/":/"1935/",/"Content-Type/":/"application/json; odata.metadata=minimal; odata.streaming=true/",/"Expires/":/"-1/"}"
        }
    }

```


#### <a name="log-response"></a>Logboek-antwoord

Hier volgt het antwoordbericht logboek van de API-app.

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:32:17 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "964",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "ttl": 2592000,
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0_1465597937",
        "_rid": "XngRAOT6IQEHAAAAAAAAAA==",
        "_self": "dbs/XngRAA==/colls/XngRAOT6IQE=/docs/XngRAOT6IQEHAAAAAAAAAA==/",
        "_ts": 1465597936,
        "_etag": "/"0400fc2f-0000-0000-0000-575b3ff00000/"",
        "patientID": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:56Z",
        "source": "{/"Pragma/":/"no-cache/",/"x-ms-request-id/":/"e750c9a9-bd48-44c4-bbba-1688b6f8a132/",/"OData-Version/":/"4.0/",/"Cache-Control/":/"no-cache/",/"Date/":/"Fri, 10 Jun 2016 22:31:56 GMT/",/"Set-Cookie/":/"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1/",/"Server/":/"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0/",/"X-AspNet-Version/":/"4.0.30319/",/"X-Powered-By/":/"ASP.NET/",/"Content-Length/":/"1935/",/"Content-Type/":/"application/json; odata.metadata=minimal; odata.streaming=true/",/"Expires/":/"-1/"}",
        "operation": "New Patient",
        "salesforceId": "",
        "expired": false
    }
}

```

Nu we de stappen voor foutafhandeling bekijken.

### <a name="error-handling"></a>Foutafhandeling

Het volgende codevoorbeeld voor logische app laat zien hoe u foutafhandeling kunt implementeren.

#### <a name="create-error-record"></a>Foutrecord maken

Hier volgt de broncode van de logische app voor het maken van een foutrecord voor een.

``` json
"actions": {
    "CreateErrorRecord": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "action": "New_Patient",
            "isError": true,
            "crmId": "@{triggerBody()['CRMid']}",
            "patientID": "@{triggerBody()['CRMid']}",
            "message": "@{body('Create_NewPatientRecord')['message']}",
            "providerId": "@{triggerBody()['providerId']}",
            "severity": 4,
            "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
            "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
            "salesforceId": "",
            "update": false
        },
        "method": "post",
        "uri": "https://.../api/CrMtoSfError"
        },
        "runAfter":
        {
            "Create_NewPatientRecord": ["Failed" ]
        }
    }
}             
```

#### <a name="insert-error-into-cosmos-db--request"></a>Fout bij invoegen in Cosmos DB - aanvragen

``` json

{
    "uri": "https://.../api/CrMtoSfError",
    "method": "post",
    "body": {
        "action": "New_Patient",
        "isError": true,
        "crmId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "providerId": "",
        "severity": 4,
        "salesforceId": "",
        "update": false,
        "source": "{/"Account_Class_vod__c/":/"PRAC/",/"Account_Status_MED__c/":/"I/",/"CRM_HUB_ID__c/":/"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0/",/"Credentials_vod__c/",/"DTC_ID_MED__c/":/"/",/"Fax/":/"/",/"FirstName/":/"A/",/"Gender_vod__c/":/"/",/"IMS_ID__c/":/"/",/"LastName/":/"BAILEY/",/"MasterID_mp__c/":/"/",/"C_ID_MED__c/":/"851588/",/"Middle_vod__c/":/"/",/"NPI_vod__c/":/"/",/"PDRP_MED__c/":false,/"PersonDoNotCall/":false,/"PersonEmail/":/"/",/"PersonHasOptedOutOfEmail/":false,/"PersonHasOptedOutOfFax/":false,/"PersonMobilePhone/":/"/",/"Phone/":/"/",/"Practicing_Specialty__c/":/"FM - FAMILY MEDICINE/",/"Primary_City__c/":/"/",/"Primary_State__c/":/"/",/"Primary_Street_Line2__c/":/"/",/"Primary_Street__c/":/"/",/"Primary_Zip__c/":/"/",/"RecordTypeId/":/"012U0000000JaPWIA0/",/"Request_Date__c/":/"2016-06-10T22:31:55.9647467Z/",/"ONY_ID__c/":/"/",/"Specialty_1_vod__c/":/"/",/"Suffix_vod__c/":/"/",/"Website/":/"/"}",
        "statusCode": "400"
    }
}
```

#### <a name="insert-error-into-cosmos-db--response"></a>Fout bij invoegen in Cosmos DB - antwoord

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:57 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "1561",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0-1465597917",
        "_rid": "sQx2APhVzAA8AAAAAAAAAA==",
        "_self": "dbs/sQx2AA==/colls/sQx2APhVzAA=/docs/sQx2APhVzAA8AAAAAAAAAA==/",
        "_ts": 1465597912,
        "_etag": "/"0c00eaac-0000-0000-0000-575b3fdc0000/"",
        "prescriberId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:57.3651027Z",
        "action": "New_Patient",
        "salesforceId": "",
        "update": false,
        "body": "CRM failed to complete task: Message: duplicate value found: CRM_HUB_ID__c duplicates value on record with id: 001U000001c83gK",
        "source": "{/"Account_Class_vod__c/":/"PRAC/",/"Account_Status_MED__c/":/"I/",/"CRM_HUB_ID__c/":/"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0/",/"Credentials_vod__c/":/"DO - Degree level is DO/",/"DTC_ID_MED__c/":/"/",/"Fax/":/"/",/"FirstName/":/"A/",/"Gender_vod__c/":/"/",/"IMS_ID__c/":/"/",/"LastName/":/"BAILEY/",/"MterID_mp__c/":/"/",/"Medicis_ID_MED__c/":/"851588/",/"Middle_vod__c/":/"/",/"NPI_vod__c/":/"/",/"PDRP_MED__c/":false,/"PersonDoNotCall/":false,/"PersonEmail/":/"/",/"PersonHasOptedOutOfEmail/":false,/"PersonHasOptedOutOfFax/":false,/"PersonMobilePhone/":/"/",/"Phone/":/"/",/"Practicing_Specialty__c/":/"FM - FAMILY MEDICINE/",/"Primary_City__c/":/"/",/"Primary_State__c/":/"/",/"Primary_Street_Line2__c/":/"/",/"Primary_Street__c/":/"/",/"Primary_Zip__c/":/"/",/"RecordTypeId/":/"012U0000000JaPWIA0/",/"Request_Date__c/":/"2016-06-10T22:31:55.9647467Z/",/"XXXXXXX/":/"/",/"Specialty_1_vod__c/":/"/",/"Suffix_vod__c/":/"/",/"Website/":/"/"}",
        "code": 400,
        "errors": null,
        "isError": true,
        "severity": 4,
        "notes": null,
        "resolved": 0
        }
}
```

#### <a name="salesforce-error-response"></a>SalesForce-foutbericht

``` json
{
    "statusCode": 400,
    "headers": {
        "Pragma": "no-cache",
        "x-ms-request-id": "3e8e4884-288e-4633-972c-8271b2cc912c",
        "X-Content-Type-Options": "nosniff",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "Set-Cookie": "ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1",
        "Server": "Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "205",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "status": 400,
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "source": "Salesforce.Common",
        "errors": []
    }
}

```

### <a name="return-the-response-back-to-parent-logic-app"></a>Retourneert het antwoord terug naar het bovenliggende logische app

Nadat u het antwoord krijgt, kunt u het antwoord terug naar de bovenliggende logische app doorgeven.

#### <a name="return-success-response-to-parent-logic-app"></a>Geslaagd antwoord naar de bovenliggende logische app retourneren

``` json
"SuccessResponse": {
    "runAfter":
        {
            "UpdateNew_CRMPatientResponse": ["Succeeded"]
        },
    "inputs": {
        "body": {
            "status": "Success"
    },
    "headers": {
    "    Content-type": "application/json",
        "x-ms-date": "@utcnow()"
    },
    "statusCode": 200
    },
    "type": "Response"
}
```

#### <a name="return-error-response-to-parent-logic-app"></a>Fout antwoord naar de bovenliggende logische app retourneren

``` json
"ErrorResponse": {
    "runAfter":
        {
            "Create_NewPatientRecord": ["Failed"]
        },
    "inputs": {
        "body": {
            "status": "BadRequest"
        },
        "headers": {
            "Content-type": "application/json",
            "x-ms-date": "@utcnow()"
        },
        "statusCode": 400
    },
    "type": "Response"
}

```


## <a name="cosmos-db-repository-and-portal"></a>Cosmos DB-opslagplaats en -portal

Onze oplossing mogelijkheden met toegevoegde [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db).

### <a name="error-management-portal"></a>Fout-beheerportal

Als u de fouten, kunt u een MVC-web-app om de foutrecords uit Cosmos DB weer te geven. De **lijst**, **Details**, **bewerken**, en **verwijderen** bewerkingen zijn opgenomen in de huidige versie.

> [!NOTE]
> Bewerking bewerken: Cosmos DB vervangt het hele document. De records die wordt weergegeven in de **lijst** en **Detail** weergaven zijn alleen voorbeelden. Ze zijn niet werkelijke patiënt afspraakrecords.

Hier volgen enkele voorbeelden van de gegevens van onze MVC-app gemaakt met de eerder beschreven methode.

#### <a name="error-management-list"></a>Foutenlijst voor beheer
![Foutenlijst](media/logic-apps-scenario-error-and-exception-handling/errorlist.png)

#### <a name="error-management-detail-view"></a>Weergave van details van fout management
![Foutdetails](media/logic-apps-scenario-error-and-exception-handling/errordetails.png)

### <a name="log-management-portal"></a>Logboek-beheerportal

Als u de logboeken, we ook een MVC-web-app gemaakt. Hier volgen enkele voorbeelden van de gegevens van onze MVC-app gemaakt met de eerder beschreven methode.

#### <a name="sample-log-detail-view"></a>Detailweergave van voorbeeld-logboek
![Weergave van details van logboek](media/logic-apps-scenario-error-and-exception-handling/samplelogdetail.png)

### <a name="api-app-details"></a>API-app-details

#### <a name="logic-apps-exception-management-api"></a>Logic Apps-Uitzonderingsbeheer API

Onze open-source Azure Logic Apps uitzondering beheer-API-app biedt functionaliteit, zoals hier wordt beschreven: Er zijn twee domeincontrollers:

* **ErrorController** voegt een foutrecord (document) in een Azure Cosmos DB-verzameling.
* **LogController** voegt een logboekrecord (document) in een Azure Cosmos DB-verzameling.

> [!TIP]
> Beide controllers gebruiken `async Task<dynamic>` werkzaamheden, waardoor bewerkingen op te lossen tijdens runtime, zodat we het schema van de Azure Cosmos DB in de hoofdtekst van de bewerking maken kunt. 
> 

Elk document in Azure Cosmos DB moet een unieke id hebben. We gebruiken `PatientId` en het toevoegen van een tijdstempel die wordt geconverteerd naar een Unix-timestamp-waarde (double). We afkappen de waarde als u wilt verwijderen van de decimale waarde.

U vindt de broncode van de controller fout API van [GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/LogicAppsExceptionManagementApi/Controllers/LogController.cs).

We aanroepen de API vanuit een logische app met behulp van de volgende syntaxis:

``` json
 "actions": {
        "CreateErrorRecord": {
          "metadata": {
            "apiDefinitionUrl": "https://.../swagger/docs/v1",
            "swaggerSource": "website"
          },
          "type": "Http",
          "inputs": {
            "body": {
              "action": "New_Patient",
              "isError": true,
              "crmId": "@{triggerBody()['CRMid']}",
              "prescriberId": "@{triggerBody()['CRMid']}",
              "message": "@{body('Create_NewPatientRecord')['message']}",
              "salesforceId": "@{triggerBody()['salesforceID']}",
              "severity": 4,
              "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
              "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
              "update": false
            },
            "method": "post",
            "uri": "https://.../api/CrMtoSfError"
          },
          "runAfter": {
              "Create_NewPatientRecord": ["Failed"]
            }
        }
 }
```

De expressie in het voorgaande voorbeeld controleert de *Create_NewPatientRecord* status van **mislukt**.

## <a name="summary"></a>Samenvatting

* U kunt eenvoudig logboekregistratie en foutafhandeling in een logische app implementeren.
* U kunt Azure Cosmos DB gebruiken als de opslagplaats voor logboek- en fout-records (documenten).
* MVC kunt u een portal als u wilt weergeven van logboek- en fout-records maken.

### <a name="source-code"></a>Broncode

De broncode voor de Logic Apps uitzondering management-API-toepassing is beschikbaar in deze [GitHub-opslagplaats](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "Logic App uitzondering Management API").

## <a name="next-steps"></a>Volgende stappen

* [Meer voorbeelden van logic Apps en scenario's weergeven](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Meer informatie over het bewaken van logische apps](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Sjablonen voor automatische implementatie voor logische apps maken](../logic-apps/logic-apps-create-deploy-template.md)

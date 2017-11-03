---
title: Schema voor Azure activiteit logboek met gebeurtenis | Microsoft Docs
description: Het schema van de gebeurtenis voor gegevens die in het activiteitenlogboek begrijpen
author: johnkemnetz
manager: robb
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2017
ms.author: johnkem
ms.openlocfilehash: 91129da9ef7791a506292d9e13e386a25ee341a8
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2017
---
# <a name="azure-activity-log-event-schema"></a>Azure Activity Log gebeurtenis schema
De **Azure Activity Log** is een logboek die biedt inzicht in een abonnement op gebeurtenissen die hebben plaatsgevonden in Azure. Dit artikel wordt het schema van de gebeurtenis per categorie van gegevens.

## <a name="administrative"></a>Beheerdersrechten
Deze rubriek bevat de record van alle maken, update, delete en actie bewerkingen uitgevoerd via Resource Manager. Voorbeelden van welke typen gebeurtenissen u in deze categorie ziet zijn 'virtuele machine maken' en 'netwerkbeveiligingsgroep verwijderen' elke actie op die door een gebruiker of toepassing die met Resource Manager is gemodelleerd als een bewerking op een bepaald resourcetype. Als het bewerkingstype schrijven, verwijderen of actie is, worden de records van de begin- en het slagen of mislukken van die bewerking vastgelegd in de beheercategorie. De beheercategorie omvat ook eventuele wijzigingen aan rollen gebaseerd toegangsbeheer in een abonnement.

### <a name="sample-event"></a>Voorbeeld van de gebeurtenis
```json
{
  "authorization": {
    "action": "microsoft.support/supporttickets/write",
    "role": "Subscription Admin",
    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841"
  },
  "caller": "admin@contoso.com",
  "channels": "Operation",
  "claims": {
    "aud": "https://management.core.windows.net/",
    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
    "iat": "1421876371",
    "nbf": "1421876371",
    "exp": "1421880271",
    "ver": "1.0",
    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
    "puid": "20030000801A118C",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
    "name": "John Smith",
    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
    "appidacr": "2",
    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
    "http://schemas.microsoft.com/claims/authnclassreference": "1"
  },
  "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
  "description": "",
  "eventDataId": "44ade6b4-3813-45e6-ae27-7420a95fa2f8",
  "eventName": {
    "value": "EndRequest",
    "localizedValue": "End request"
  },
  "httpRequest": {
    "clientRequestId": "27003b25-91d3-418f-8eb1-29e537dcb249",
    "clientIpAddress": "192.168.35.115",
    "method": "PUT"
  },
  "id": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
  "level": "Informational",
  "resourceGroupName": "MSSupportGroup",
  "resourceProviderName": {
    "value": "microsoft.support",
    "localizedValue": "microsoft.support"
  },
  "resourceUri": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
  "operationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
  "operationName": {
    "value": "microsoft.support/supporttickets/write",
    "localizedValue": "microsoft.support/supporttickets/write"
  },
  "properties": {
    "statusCode": "Created"
  },
  "status": {
    "value": "Succeeded",
    "localizedValue": "Succeeded"
  },
  "subStatus": {
    "value": "Created",
    "localizedValue": "Created (HTTP Status Code: 201)"
  },
  "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
  "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
  "subscriptionId": "s1"
}
```

### <a name="property-descriptions"></a>Eigenschapbeschrijvingen
| Elementnaam | Beschrijving |
| --- | --- |
| Autorisatie |De BLOB van RBAC-eigenschappen van de gebeurtenis. Omvat gewoonlijk het eigenschappen "action", 'rol' en 'bereik'. |
| aanroeper |E-mailadres van de gebruiker die is uitgevoerd. de bewerking, UPN-claim of SPN claim op basis van beschikbaarheid. |
| kanalen |Een van de volgende waarden: 'Admin', 'Operation' |
| Claims |De JWT-token gebruikt door Active Directory voor het verifiëren van de gebruiker of toepassing naar deze bewerking niet uitvoeren in het resourcemanager. |
| correlationId |Meestal een GUID in de indeling van de tekenreeks. Gebeurtenissen die een correlationId share deel uitmaken van dezelfde uber actie. |
| description |De beschrijving van de statische tekst van een gebeurtenis. |
| eventDataId |De unieke id van een gebeurtenis. |
| httpRequest |BLOB met een beschrijving van de Http-aanvraag. Omvat gewoonlijk het 'clientRequestId', "clientIpAddress" en "method" (HTTP-methode. For example, plaatsen). |
| niveau |Niveau van de gebeurtenis. Een van de volgende waarden: 'Kritiek', 'Fout', 'Waarschuwing', 'Ter informatie' en 'Verbose' |
| resourceGroupName |De naam van de resourcegroep voor de betrokken resource. |
| resourceProviderName |Naam van de resourceprovider voor de betrokken resource |
| resourceId |Bron-id van de betrokken resource. |
| operationId |Een GUID die wordt gedeeld door de gebeurtenissen die met één bewerking overeenkomen. |
| operationName |De naam van de bewerking. |
| properties |Een set `<Key, Value>` paren (dat wil zeggen, een woordenlijst) met een beschrijving van de details van de gebeurtenis. |
| status |De tekenreeks met een beschrijving van de status van de bewerking. Sommige algemene waarden zijn: In voortgang, geslaagd, mislukt, actief, opgelost gestart. |
| subStatus |Meestal de HTTP-statuscode van de bijbehorende REST te bellen, maar kan ook andere tekenreeksen met een beschrijving van een substatus, zoals deze algemene waarden bevatten: OK (HTTP-statuscode: 200), gemaakt (HTTP-statuscode: 201), geaccepteerde (HTTP-statuscode: 202), geen inhoud (http-Status Code: 204), onjuiste aanvraag (HTTP-statuscode: 400), niet is gevonden (HTTP-statuscode: 404), Conflict (HTTP-statuscode: 409), interne serverfout (HTTP-statuscode: 500), Service niet beschikbaar (HTTP-statuscode: 503), time-out van Gateway (HTTP-statuscode: 504). |
| eventTimestamp |Tijdstempel wanneer de gebeurtenis is gegenereerd door de Azure-service verwerken van de aanvraag de gebeurtenis overeenkomt. |
| submissionTimestamp |Tijdstempel wanneer de gebeurtenis beschikbaar voor het uitvoeren van query's zijn geworden. |
| subscriptionId |Azure-abonnement-id. |

## <a name="service-health"></a>Servicestatus
Deze rubriek bevat de record van een service health incidenten die hebben plaatsgevonden in Azure. Een voorbeeld van het type gebeurtenis u in deze categorie ziet is "SQL Azure in VS-Oost ondervindt uitvaltijd." Gebeurtenissen van de health service in vijf soorten komen: actie vereist, ondersteunde herstel, Incident, onderhoud, gegevens of beveiliging, en wordt alleen weergegeven als er een resource in het abonnement dat zou worden beïnvloed door de gebeurtenis.

### <a name="sample-event"></a>Voorbeeld van de gebeurtenis
```json
{
  "channels": "Admin",
  "correlationId": "c550176b-8f52-4380-bdc5-36c1b59d3a44",
  "description": "Active: Network Infrastructure - UK South",
  "eventDataId": "c5bc4514-6642-2be3-453e-c6a67841b073",
  "eventName": {
      "value": null
  },
  "category": {
      "value": "ServiceHealth",
      "localizedValue": "Service Health"
  },
  "eventTimestamp": "2017-07-20T23:30:14.8022297Z",
  "id": "/subscriptions/mySubscriptionID/events/c5bc4514-6642-2be3-453e-c6a67841b073/ticks/636361902148022297",
  "level": "Warning",
  "operationName": {
      "value": "Microsoft.ServiceHealth/incident/action",
      "localizedValue": "Microsoft.ServiceHealth/incident/action"
  },
  "resourceProviderName": {
      "value": null
  },
  "resourceType": {
      "value": null,
      "localizedValue": ""
  },
  "resourceId": "/subscriptions/mySubscriptionID",
  "status": {
      "value": "Active",
      "localizedValue": "Active"
  },
  "subStatus": {
      "value": null
  },
  "submissionTimestamp": "2017-07-20T23:30:34.7431946Z",
  "subscriptionId": "mySubscriptionID",
  "properties": {
    "title": "Network Infrastructure - UK South",
    "service": "Service Fabric",
    "region": "UK South",
    "communication": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited to App Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows to mitigate the impact. The next update will be provided in 60 minutes, or as events warrant.",
    "incidentType": "Incident",
    "trackingId": "NA0F-BJG",
    "impactStartTime": "2017-07-20T21:41:00.0000000Z",
    "impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"UK South\"}],\"ServiceName\":\"Service Fabric\"}]",
    "defaultLanguageTitle": "Network Infrastructure - UK South",
    "defaultLanguageContent": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited to App Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows to mitigate the impact. The next update will be provided in 60 minutes, or as events warrant.",
    "stage": "Active",
    "communicationId": "636361902146035247",
    "version": "0.1.1"
  }
}
```

### <a name="property-descriptions"></a>Eigenschapbeschrijvingen
Elementnaam | Beschrijving
-------- | -----------
kanalen | Is een van de volgende waarden: 'Admin', 'Operation'
correlationId | Is meestal een GUID in de indeling van de tekenreeks. Gebeurtenissen met die deel uitmaken van dezelfde uber actie meestal delen de dezelfde correlationId.
description | Beschrijving van de gebeurtenis.
eventDataId | De unieke id van een gebeurtenis.
EventName | De titel van de gebeurtenis.
niveau | Niveau van de gebeurtenis. Een van de volgende waarden: 'Kritiek', 'Fout', 'Waarschuwing', 'Ter informatie' en 'Verbose'
resourceProviderName | De naam van de resourceprovider voor de betrokken resource. Als u niet bekend is, wordt dit niet null zijn.
resourceType| Het type resource van de betrokken resource. Als u niet bekend is, wordt dit niet null zijn.
subStatus | Meestal null voor Service Health-gebeurtenissen.
eventTimestamp | Tijdstempel wanneer het gebeurtenislogboek is gegenereerd en naar het activiteitenlogboek verzonden.
submissionTimestamp |   Tijdstempel wanneer de gebeurtenis is beschikbaar in het activiteitenlogboek geworden.
subscriptionId | De Azure-abonnement in waarmee deze gebeurtenis is vastgelegd.
status | De tekenreeks met een beschrijving van de status van de bewerking. Sommige algemene waarden zijn: actief, opgelost.
operationName | De naam van de bewerking. Meestal Microsoft.ServiceHealth/incident/action.
category | 'ServiceHealth'
resourceId | Bron-id van de betrokken resource, indien bekend. Abonnements-ID is anders opgegeven.
Properties.title | De gelokaliseerde titel voor deze communicatie. Engels is de standaardtaal.
Properties.Communication | De gelokaliseerde details van de communicatie met de HTML-opmaak. Engels is de standaardinstelling.
Properties.incidentType | Mogelijke waarden: AssistedRecovery, ActionRequired, informatie, Incident-, onderhoud, beveiliging
Properties.trackingId | Identificeert het incident dat deze gebeurtenis is gekoppeld. Gebruik deze optie als u de gebeurtenissen die betrekking hebben op een incident met elkaar correleren.
Properties.impactedServices | Een escape-teken JSON-blob die hierin worden de services en regio's die worden beïnvloed door het incident. Een lijst met Services, die allemaal een servicenaam en een lijst met ImpactedRegions, die allemaal een RegionName.
Properties.defaultLanguageTitle | De communicatie in het Engels
Properties.defaultLanguageContent | De communicatie in het Engels als HTML-indeling of tekst zonder opmaak
Properties.Stage | Mogelijke waarden voor AssistedRecovery, ActionRequired, informatie, Incident-, beveiliging: actief, zijn opgelost. Ze zijn voor onderhoud: actief, gepland, InProgress, geannuleerd, Rescheduled, opgelost, voltooid
Properties.communicationId | De communicatie deze gebeurtenis is gekoppeld.

## <a name="alert"></a>Waarschuwing
Deze rubriek bevat de record van alle activeringen van waarschuwingen van Azure. Een voorbeeld van het type gebeurtenis u in deze categorie ziet is "CPU-percentage op myVM is meer dan 80 voor de afgelopen vijf minuten." Een verscheidenheid aan Azure systemen hebben een waarschuwingsmethoden concept--u kunt een regel bepaalde hardwaresleutel definiëren en een melding ontvangen wanneer voorwaarden overeenkomen met die regel. Elke keer dat een ondersteunde Azure Waarschuwingstype 'wordt geactiveerd,' of de voorwaarden wordt voldaan voor het genereren van een melding, een record van de activering is ook naar deze categorie van het activiteitenlogboek gepusht.

### <a name="sample-event"></a>Voorbeeld van de gebeurtenis

```json
{
  "caller": "Microsoft.Insights/alertRules",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/alertRules"
  },
  "correlationId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "description": "'Disk read LessThan 100000 ([Count]) in the last 5 minutes' has been resolved for CloudService: myResourceGroup/Production/Event.BackgroundJobsWorker.razzle (myResourceGroup)",
  "eventDataId": "149d4baf-53dc-4cf4-9e29-17de37405cd9",
  "eventName": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "category": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "id": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle/events/149d4baf-53dc-4cf4-9e29-17de37405cd9/ticks/636362258535221920",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "Microsoft.ClassicCompute",
    "localizedValue": "Microsoft.ClassicCompute"
  },
  "resourceId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle",
  "resourceType": {
    "value": "Microsoft.ClassicCompute/domainNames/slots/roles",
    "localizedValue": "Microsoft.ClassicCompute/domainNames/slots/roles"
  },
  "operationId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "operationName": {
    "value": "Microsoft.Insights/AlertRules/Resolved/Action",
    "localizedValue": "Microsoft.Insights/AlertRules/Resolved/Action"
  },
  "properties": {
    "RuleUri": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert",
    "RuleName": "myalert",
    "RuleDescription": "",
    "Threshold": "100000",
    "WindowSizeInMinutes": "5",
    "Aggregation": "Average",
    "Operator": "LessThan",
    "MetricName": "Disk read",
    "MetricUnit": "Count"
  },
  "status": {
    "value": "Resolved",
    "localizedValue": "Resolved"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T09:24:13.522192Z",
  "submissionTimestamp": "2017-07-21T09:24:15.6578651Z",
  "subscriptionId": "mySubscriptionID"
}
```

### <a name="property-descriptions"></a>Eigenschapbeschrijvingen
| Elementnaam | Beschrijving |
| --- | --- |
| aanroeper | Altijd Microsoft.Insights/alertRules |
| kanalen | Altijd 'Admin, bewerking' |
| Claims | JSON-blob met het type SPN (service principal name) of de bron van de waarschuwings-engine. |
| correlationId | Een GUID in de indeling van de tekenreeks. |
| description |De beschrijving van de statische tekst van de waarschuwing gebeurtenis. |
| eventDataId |De unieke id van de waarschuwing gebeurtenis. |
| niveau |Niveau van de gebeurtenis. Een van de volgende waarden: 'Kritiek', 'Fout', 'Waarschuwing', 'Ter informatie' en 'Verbose' |
| resourceGroupName |De naam van de resourcegroep voor de betrokken resource als deze een metrische waarschuwing. Dit is de naam van de resourcegroep die de waarschuwing zelf bevat voor de overige Waarschuwingstypen. |
| resourceProviderName |De naam van de resourceprovider voor de betrokken resource als deze een metrische waarschuwing. Dit is de naam van de resourceprovider voor de waarschuwing zelf voor andere typen waarschuwingen. |
| resourceId | De naam van de resource-ID voor de betrokken resource als deze een metrische waarschuwing. Dit is de bron-ID van de waarschuwing resource zelf voor andere typen waarschuwingen. |
| operationId |Een GUID die wordt gedeeld door de gebeurtenissen die met één bewerking overeenkomen. |
| operationName |De naam van de bewerking. |
| properties |Een set `<Key, Value>` paren (dat wil zeggen, een woordenlijst) met een beschrijving van de details van de gebeurtenis. |
| status |De tekenreeks met een beschrijving van de status van de bewerking. Sommige algemene waarden zijn: In voortgang, geslaagd, mislukt, actief, opgelost gestart. |
| subStatus | Meestal null voor waarschuwingen. |
| eventTimestamp |Tijdstempel wanneer de gebeurtenis is gegenereerd door de Azure-service verwerken van de aanvraag de gebeurtenis overeenkomt. |
| submissionTimestamp |Tijdstempel wanneer de gebeurtenis beschikbaar voor het uitvoeren van query's zijn geworden. |
| subscriptionId |Azure-abonnement-id. |

### <a name="properties-field-per-alert-type"></a>Van eigenschappenveld per type waarschuwing
Het eigenschappenveld bevat verschillende waarden, afhankelijk van de bron van de waarschuwing gebeurtenis. Twee gebeurtenisproviders voor algemene waarschuwing zijn activiteitenlogboek van waarschuwingen en metrische waarschuwingen.

#### <a name="properties-for-activity-log-alerts"></a>Eigenschappen van activiteitenlogboek van waarschuwingen
| Elementnaam | Beschrijving |
| --- | --- |
| properties.subscriptionId | De abonnements-ID van de activiteit gebeurtenislogboek waardoor deze activiteit logboek-waarschuwingsregel worden geactiveerd. |
| properties.eventDataId | De gebeurtenis-ID van de activiteit gebeurtenislogboek waardoor deze activiteit logboek-waarschuwingsregel worden geactiveerd. |
| properties.resourceGroup | De resourcegroep van de activiteit gebeurtenislogboek waardoor deze activiteit logboek-waarschuwingsregel worden geactiveerd. |
| properties.resourceId | De resource-ID van de activiteit gebeurtenislogboek waardoor deze activiteit logboek-waarschuwingsregel worden geactiveerd. |
| properties.eventTimestamp | De gebeurtenis tijdstempel van de activiteit gebeurtenislogboek waardoor deze activiteit logboek-waarschuwingsregel worden geactiveerd. |
| properties.operationName | De naam van de bewerking van de activiteit gebeurtenislogboek waardoor deze activiteit logboek-waarschuwingsregel worden geactiveerd. |
| Properties.status | De status van de activiteit gebeurtenislogboek waardoor deze activiteit logboek-waarschuwingsregel worden geactiveerd.|

#### <a name="properties-for-metric-alerts"></a>Eigenschappen van metrische waarschuwingen
| Elementnaam | Beschrijving |
| --- | --- |
| Eigenschappen. RuleUri | Bron-ID van de metrische waarschuwingsregel zelf. |
| Eigenschappen. RuleName | De naam van de metrische waarschuwingsregel. |
| Eigenschappen. RuleDescription | De beschrijving van de metrische waarschuwingsregel (zoals gedefinieerd in de waarschuwingsregel). |
| Eigenschappen. Drempelwaarde | De drempelwaarde die wordt gebruikt in de evaluatie van de metrische waarschuwingsregel. |
| Eigenschappen. WindowSizeInMinutes | De grootte die in de evaluatie van de metrische waarschuwingsregel wordt gebruikt. |
| Eigenschappen. Aggregatie | Het samenvoegingstype in de metrische waarschuwingsregel gedefinieerd. |
| Eigenschappen. Operator | De voorwaardelijke operator die wordt gebruikt in de evaluatie van de metrische waarschuwingsregel. |
| Eigenschappen. MetricName | De metrische naam van de metrische gegevens die in de evaluatie van de metrische waarschuwingsregel wordt gebruikt. |
| Eigenschappen. MetricUnit | De metrische eenheid voor de metriek gebruikt in de evaluatie van de metrische waarschuwingsregel. |

## <a name="autoscale"></a>Automatisch schalen
Deze rubriek bevat de record van alle gebeurtenissen met betrekking tot de werking van de engine voor het automatisch schalen op basis van de instellingen voor automatisch schalen die u hebt gedefinieerd in uw abonnement. Een voorbeeld van het type gebeurtenis u in deze categorie ziet is "Automatisch schalen opschaling van de actie is mislukt." Met automatisch schalen, kunt u automatisch geschaald uitbreiden of schalen op basis van tijd van de dag en/of laden (metrische) gegevens met behulp van een instelling voor automatisch schalen van het aantal exemplaren in een ondersteunde brontype. Wanneer de voorwaarden worden voldaan op schaal omhoog of omlaag is gestart en is geslaagd of mislukt gebeurtenissen worden vastgelegd in deze categorie.

### <a name="sample-event"></a>Voorbeeld van de gebeurtenis
```json
{
  "caller": "Microsoft.Insights/autoscaleSettings",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/autoscaleSettings"
  },
  "correlationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "description": "The autoscale engine attempting to scale resource '/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count to 2 instances count.",
  "eventDataId": "a5b92075-1de9-42f1-b52e-6f3e4945a7c7",
  "eventName": {
    "value": "AutoscaleAction",
    "localizedValue": "AutoscaleAction"
  },
  "category": {
    "value": "Autoscale",
    "localizedValue": "Autoscale"
  },
  "id": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup/events/a5b92075-1de9-42f1-b52e-6f3e4945a7c7/ticks/636361956518681572",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "microsoft.insights",
    "localizedValue": "microsoft.insights"
  },
  "resourceId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup",
  "resourceType": {
    "value": "microsoft.insights/autoscalesettings",
    "localizedValue": "microsoft.insights/autoscalesettings"
  },
  "operationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "operationName": {
    "value": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action",
    "localizedValue": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action"
  },
  "properties": {
    "Description": "The autoscale engine attempting to scale resource '/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count to 2 instances count.",
    "ResourceName": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource",
    "OldInstancesCount": "3",
    "NewInstancesCount": "2",
    "LastScaleActionTime": "Fri, 21 Jul 2017 01:00:51 GMT"
  },
  "status": {
    "value": "Succeeded",
    "localizedValue": "Succeeded"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T01:00:51.8681572Z",
  "submissionTimestamp": "2017-07-21T01:00:52.3008754Z",
  "subscriptionId": "mySubscriptionID"
}

```

### <a name="property-descriptions"></a>Eigenschapbeschrijvingen
| Elementnaam | Beschrijving |
| --- | --- |
| aanroeper | Altijd Microsoft.Insights/autoscaleSettings |
| kanalen | Altijd 'Admin, bewerking' |
| Claims | JSON-blob met het type SPN (service principal name) of de bron van de engine voor automatisch schalen. |
| correlationId | Een GUID in de indeling van de tekenreeks. |
| description |De beschrijving van de statische tekst van de gebeurtenis met automatisch schalen. |
| eventDataId |De unieke id van de gebeurtenis met automatisch schalen. |
| niveau |Niveau van de gebeurtenis. Een van de volgende waarden: 'Kritiek', 'Fout', 'Waarschuwing', 'Ter informatie' en 'Verbose' |
| resourceGroupName |Naam van de resourcegroep voor de instelling voor automatisch schalen. |
| resourceProviderName |Naam van de resourceprovider voor de instelling voor automatisch schalen. |
| resourceId |Bron-id van de instelling voor automatisch schalen. |
| operationId |Een GUID die wordt gedeeld door de gebeurtenissen die met één bewerking overeenkomen. |
| operationName |De naam van de bewerking. |
| properties |Een set `<Key, Value>` paren (dat wil zeggen, een woordenlijst) met een beschrijving van de details van de gebeurtenis. |
| Eigenschappen. Beschrijving | Gedetailleerde beschrijving van wat de engine voor het automatisch schalen die werd uitgevoerd. |
| Eigenschappen. ResourceName | Bron-ID van de betrokken resource (de resource waarop de schaalactie werd uitgevoerd) |
| Eigenschappen. OldInstancesCount | Het aantal exemplaren voordat de actie voor automatisch schalen van kracht. |
| Eigenschappen. NewInstancesCount | Het aantal exemplaren nadat de actie voor automatisch schalen van kracht. |
| Eigenschappen. LastScaleActionTime | De tijdstempel van wanneer de actie voor automatisch schalen is opgetreden. |
| status |De tekenreeks met een beschrijving van de status van de bewerking. Sommige algemene waarden zijn: In voortgang, geslaagd, mislukt, actief, opgelost gestart. |
| subStatus | Meestal null voor automatisch schalen. |
| eventTimestamp |Tijdstempel wanneer de gebeurtenis is gegenereerd door de Azure-service verwerken van de aanvraag de gebeurtenis overeenkomt. |
| submissionTimestamp |Tijdstempel wanneer de gebeurtenis beschikbaar voor het uitvoeren van query's zijn geworden. |
| subscriptionId |Azure-abonnement-id. |

## <a name="security"></a>Beveiliging
Deze rubriek bevat de record geen waarschuwingen gegenereerd door Azure Security Center. Een voorbeeld van het type gebeurtenis u in deze categorie ziet is 'dubbele extensie verdachte file wordt uitgevoerd'

### <a name="sample-event"></a>Voorbeeld van de gebeurtenis
```json
{
    "channels": "Operation",
    "correlationId": "965d6c6a-a790-4a7e-8e9a-41771b3fbc38",
    "description": "Suspicious double extension file executed. Machine logs indicate an execution of a process with a suspicious double extension.\r\nThis extension may trick users into thinking files are safe to be opened and might indicate the presence of malware on the system.",
    "eventDataId": "965d6c6a-a790-4a7e-8e9a-41771b3fbc38",
    "eventName": {
        "value": "Suspicious double extension file executed",
        "localizedValue": "Suspicious double extension file executed"
    },
    "category": {
        "value": "Security",
        "localizedValue": "Security"
    },
    "eventTimestamp": "2017-10-18T06:02:18.6179339Z",
    "id": "/subscriptions/d4742bb8-c279-4903-9653-9858b17d0c2e/providers/Microsoft.Security/locations/centralus/alerts/965d6c6a-a790-4a7e-8e9a-41771b3fbc38/events/965d6c6a-a790-4a7e-8e9a-41771b3fbc38/ticks/636439033386179339",
    "level": "Informational",
    "operationId": "965d6c6a-a790-4a7e-8e9a-41771b3fbc38",
    "operationName": {
        "value": "Microsoft.Security/locations/alerts/activate/action",
        "localizedValue": "Microsoft.Security/locations/alerts/activate/action"
    },
    "resourceGroupName": "myResourceGroup",
    "resourceProviderName": {
        "value": "Microsoft.Security",
        "localizedValue": "Microsoft.Security"
    },
    "resourceType": {
        "value": "Microsoft.Security/locations/alerts",
        "localizedValue": "Microsoft.Security/locations/alerts"
    },
    "resourceId": "/subscriptions/d4742bb8-c279-4903-9653-9858b17d0c2e/providers/Microsoft.Security/locations/centralus/alerts/2518939942613820660_a48f8653-3fc6-4166-9f19-914f030a13d3",
    "status": {
        "value": "Active",
        "localizedValue": "Active"
    },
    "subStatus": {
        "value": null
    },
    "submissionTimestamp": "2017-10-18T06:02:52.2176969Z",
    "subscriptionId": "d4742bb8-c279-4903-9653-9858b17d0c2e",
    "properties": {
        "accountLogonId": "0x2r4",
        "commandLine": "c:\\mydirectory\\doubleetension.pdf.exe",
        "domainName": "hpc",
        "parentProcess": "unknown",
        "parentProcess id": "0",
        "processId": "6988",
        "processName": "c:\\mydirectory\\doubleetension.pdf.exe",
        "userName": "myUser",
        "UserSID": "S-3-2-12",
        "ActionTaken": "Detected",
        "Severity": "High"
    },
    "relatedEvents": []
}

```

### <a name="property-descriptions"></a>Eigenschapbeschrijvingen
| Elementnaam | Beschrijving |
| --- | --- |
| kanalen | Altijd 'bewerking' |
| correlationId | Een GUID in de indeling van de tekenreeks. |
| description |De beschrijving van de statische tekst van de beveiligingsgebeurtenis. |
| eventDataId |De unieke id van de beveiligingsgebeurtenis. |
| EventName |Beschrijvende naam van de beveiligingsgebeurtenis. |
| id |Unieke resource-id van de beveiligingsgebeurtenis. |
| niveau |Niveau van de gebeurtenis. Een van de volgende waarden: 'Kritiek', 'Fout', 'Waarschuwing', 'Ter informatie' of 'Uitgebreid' |
| resourceGroupName |De naam van de resourcegroep voor de resource. |
| resourceProviderName |Naam van de resourceprovider voor Azure Security Center. Altijd 'Microsoft.Security'. |
| resourceType |Het type resource dat de beveiligingsgebeurtenis, zoals 'Microsoft.Security/locations/alerts' gegenereerd |
| resourceId |Bron-id van de beveiligingswaarschuwing. |
| operationId |Een GUID die wordt gedeeld door de gebeurtenissen die met één bewerking overeenkomen. |
| operationName |De naam van de bewerking. |
| properties |Een set `<Key, Value>` paren (dat wil zeggen, een woordenlijst) met een beschrijving van de details van de gebeurtenis. Deze eigenschappen varieert afhankelijk van het type beveiligingswaarschuwing. Zie [deze pagina](../security-center/security-center-alerts-type.md) voor een beschrijving van de typen waarschuwingen die afkomstig van Security Center zijn. |
| Eigenschappen. Ernst |De ernst op. Mogelijke waarden zijn 'Hoog', 'Gemiddeld' of 'Laag'. |
| status |De tekenreeks met een beschrijving van de status van de bewerking. Sommige algemene waarden zijn: In voortgang, geslaagd, mislukt, actief, opgelost gestart. |
| subStatus | Meestal null voor beveiligingsgebeurtenissen. |
| eventTimestamp |Tijdstempel wanneer de gebeurtenis is gegenereerd door de Azure-service verwerken van de aanvraag de gebeurtenis overeenkomt. |
| submissionTimestamp |Tijdstempel wanneer de gebeurtenis beschikbaar voor het uitvoeren van query's zijn geworden. |
| subscriptionId |Azure-abonnement-id. |

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over het activiteitenlogboek (voorheen controlelogboeken)](monitoring-overview-activity-logs.md)
* [Stream de Azure Activity Log naar Event Hubs](monitoring-stream-activity-logs-event-hubs.md)

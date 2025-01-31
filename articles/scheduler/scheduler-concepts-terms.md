---
title: Concepten, termen en entiteiten - Azure Scheduler | Microsoft Docs
description: De concepten, terminologie en entiteitenhiërarchie, inclusief jobs en jobverzamelingen, in Azure Scheduler leren
services: scheduler
ms.service: scheduler
ms.suite: infrastructure-services
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: 3ef16fab-d18a-48ba-8e56-3f3e0a1bcb92
ms.topic: conceptual
ms.date: 08/18/2016
ms.openlocfilehash: d701fba39685d781d1a4c2d8a6cf194ca7eb2908
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60530940"
---
# <a name="concepts-terminology-and-entities-in-azure-scheduler"></a>Concepten, terminologie en entiteiten in Azure Scheduler

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) vervangt Azure Scheduler, die buiten gebruik wordt gesteld. [Probeer in plaats daarvan Azure Logic Apps](../scheduler/migrate-from-scheduler-to-logic-apps.md) als u taken wilt plannen. 

## <a name="entity-hierarchy"></a>Entiteitenhiërarchie

De volgende entiteiten of resources worden door de REST API voor Azure Scheduler beschikbaar gemaakt en gebruikt:

| Entiteit | Description |
|--------|-------------|
| **Job** | Definieert één terugkerende actie, met eenvoudige of complexe strategieën, die moet worden uitgevoerd. Acties omvatten mogelijk HTTP-, opslagwachtrij-, Service Bus-wachtrij- of Service Bus-onderwerpaanvragen. | 
| **Jobverzameling** | Bevat een aantal jobs en onderhoudt instellingen, quota en vertragingen die worden gedeeld door jobs binnen de verzameling. Als eigenaar van een Azure-abonnement kunt u jobverzamelingen maken en jobs groeperen op basis van gebruiks- of toepassingsgrenzen. Een jobverzameling heeft de volgende kenmerken: <p>- Beperkt tot één regio. <br>- U kunt er quota mee afdwingen, zodat u het gebruik voor alle jobs in een verzameling kunt beperken. <br>- Quota omvatten MaxJobs en MaxRecurrence. | 
| **Jobgeschiedenis** | Beschrijft de details voor de uitvoering van een job, bijvoorbeeld details over status en antwoorden. |
||| 

## <a name="entity-management"></a>Entiteitsbeheer

Op hoog niveau maakt de REST API van de Scheduler deze bewerkingen voor het beheren van entiteiten beschikbaar.

### <a name="job-management"></a>Jobbeheer

Biedt ondersteuning voor bewerkingen voor het maken en bewerken van jobs. Alle jobs moeten behoren tot een bestaande jobverzameling. Er wordt er dus niet impliciet een gemaakt. Zie [Scheduler REST API - Jobs](https://docs.microsoft.com/rest/api/scheduler/jobs) voor meer informatie. Hier is de URI-adres voor deze bewerkingen:

```
https://management.azure.com/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}
```

### <a name="job-collection-management"></a>Beheer van jobverzameling

Ondersteunt bewerkingen voor het maken en bewerken van jobs en jobverzamelingen, die worden toegewezen aan quota en gedeelde instellingen. Quota specificeren bijvoorbeeld het maximum aantal jobs en het kleinste terugkeerpatroon. Zie [Scheduler REST API - Job Collections](https://docs.microsoft.com/rest/api/scheduler/jobcollections) (Scheduler REST API - Jobverzamelingen) voor meer informatie. Hier is de URI-adres voor deze bewerkingen:

```
https://management.azure.com/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}
```

### <a name="job-history-management"></a>Beheer van jobgeschiedenis

Biedt ondersteuning voor de GET-bewerking voor het ophalen van een geschiedenis van 60 dagen van uitgevoerde jobs, bijvoorbeeld de verstreken tijd van jobs en de resultaten van het uitvoeren van jobs. Omvat ondersteuning voor querytekenreeksparameters om te filteren op basis van toestand en status. Zie [Scheduler REST API - Jobs - List Job History](https://docs.microsoft.com/rest/api/scheduler/jobs/listjobhistory) (Scheduler REST API - Jobs - Lijst met jobgeschiedenissen) voor meer informatie. Hier is de URI-adres voor deze bewerking:

```
https://management.azure.com/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history
```

## <a name="job-types"></a>Jobtypen

Azure Scheduler ondersteunt meerdere jobtypen: 

* HTTP-jobs, inclusief HTTP-jobs die SSL ondersteunen, voor wanneer u het eindpunt voor een bestaande service of workload hebt.
* Opslagwachtrijjobs voor workloads die opslagwachtrijen gebruiken, zoals het posten van berichten naar opslagwachtrijen
* Service Bus-wachtrijjobs voor workloads die gebruikmaken van Service Bus-wachtrijen
* Service Bus-onderwerpjobs voor workloads die gebruikmaken van Service Bus-onderwerpen

## <a name="job-definition"></a>Jobdefinitie

Op hoog niveau bevat een Scheduler-job uit de volgende basisonderdelen:

* De actie die wordt uitgevoerd wanneer de timer van de job wordt gestart
* Optioneel: De tijd voor het uitvoeren van de taak
* Optioneel: Wanneer en hoe vaak de taak herhalen
* Optioneel: Een foutactie die wordt uitgevoerd als de primaire actie mislukt

De job bevat ook door het systeem geleverde gegevens, zoals volgende geplande uitvoeringstijd van de job. De codedefinitie van de job is een object in de indeling JSON (JavaScript Object Notation). Deze bevat drie elementen:

| Element | Vereist | Description | 
|---------|----------|-------------| 
| [**startTime**](#start-time) | Nee | De begintijd voor de taak, met een tijdverschuiving in de [indeling ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) | 
| [**action**](#action) | Ja | De details van de primaire actie, die een **errorAction**-object kan omvatten | 
| [**errorAction**](#error-action) | Nee | De details van de secundaire actie, die wordt uitgevoerd als de eerste actie mislukt |
| [**recurrence**](#recurrence) | Nee | Details als frequentie en interval van een terugkerende job | 
| [**retryPolicy**](#retry-policy) | Nee | De details voor hoe vaak een actie moet worden herhaald | 
| [**state**](#state) | Ja | De details van de huidige status van de job |
| [**status**](#status) | Ja | De details van de huidige status van de job, die onder controle staan van de service |
||||

Hier is een voorbeeld dat een uitgebreide jobdefinitie laat zien voor een HTTP-actie, waarvan meer details van het element in latere secties ter sprake komen: 

```json
"properties": {
   "startTime": "2012-08-04T00:00Z",
   "action": {
      "type": "Http",
      "request": {
         "uri": "http://contoso.com/some-method", 
         "method": "PUT",          
         "body": "Posting from a timer",
         "headers": {
            "Content-Type": "application/json"
         },
         "retryPolicy": { 
             "retryType": "None" 
         },
      },
      "errorAction": {
         "type": "Http",
         "request": {
            "uri": "http://contoso.com/notifyError",
            "method": "POST"
         }
      }
   },
   "recurrence": {
      "frequency": "Week",
      "interval": 1,
      "schedule": {
         "weekDays": ["Monday", "Wednesday", "Friday"],
         "hours": [10, 22]
      },
      "count": 10,
      "endTime": "2012-11-04"
   },
   "state": "Disabled",
   "status": {
      "lastExecutionTime": "2007-03-01T13:00:00Z",
      "nextExecutionTime": "2007-03-01T14:00:00Z ",
      "executionCount": 3,
      "failureCount": 0,
      "faultedCount": 0
   }
}
```

<a name="start-time"></a>

## <a name="starttime"></a>startTime

In het object **startTime** kunt u de begintijd en een tijdverschuiving aangeven in de [indeling ISO 8601](https://en.wikipedia.org/wiki/ISO_8601).

<a name="action"></a>

## <a name="action"></a>action

De Scheduler-job voert een primaire **action** uit op basis van het opgegeven schema. Scheduler biedt ondersteuning voor HTTP-, opslagwachtrij-, Service Bus-onderwerp- en Service Bus-wachtrijacties. Als de primaire **action** mislukt, kan er een secundaire [**errorAction**](#erroraction) worden uitgevoerd die de fout afhandelt. Het object **action** beschrijft de volgende elementen:

* Het servicetype van de actie
* De details van de actie
* Een alternatieve **errorAction**

Het vorige voorbeeld beschrijft een HTTP-actie. Hier volgt een voorbeeld van een opslagwachtrijactie:

```json
"action": {
   "type": "storageQueue",
   "queueMessage": {
      "storageAccount": "myStorageAccount",  
      "queueName": "myqueue",                
      "sasToken": "TOKEN",                   
      "message": "My message body"
    }
}
```

Hier volgt een voorbeeld van een Service Bus-wachtrijactie:

```json
"action": {
   "type": "serviceBusQueue",
   "serviceBusQueueMessage": {
      "queueName": "q1",  
      "namespace": "mySBNamespace",
      "transportType": "netMessaging", // Either netMessaging or AMQP
      "authentication": {  
         "sasKeyName": "QPolicy",
         "type": "sharedAccessKey"
      },
      "message": "Some message",  
      "brokeredMessageProperties": {},
      "customMessageProperties": {
         "appname": "FromScheduler"
      }
   }
},
```

Hier volgt een voorbeeld van een Service Bus-onderwerpactie:

```json
"action": {
   "type": "serviceBusTopic",
   "serviceBusTopicMessage": {
      "topicPath": "t1",  
      "namespace": "mySBNamespace",
      "transportType": "netMessaging", // Either netMessaging or AMQP
      "authentication": {
         "sasKeyName": "QPolicy",
         "type": "sharedAccessKey"
      },
      "message": "Some message",
      "brokeredMessageProperties": {},
      "customMessageProperties": {
         "appname": "FromScheduler"
      }
   }
},
```

Zie [Autoriseren met Shared Access Signatures](../storage/common/storage-dotnet-shared-access-signature-part-1.md) voor meer informatie over SAS-tokens (Shared Access Signature).

<a name="error-action"></a>

## <a name="erroraction"></a>errorAction

Als de primaire **action** van de job mislukt, kan er een **errorAction** worden uitgevoerd die de fout afhandelt. In de primaire **action** kunt u een **errorAction**-object opgeven zodat een foutafhandelingseindpunt kan worden aangeroepen of een melding voor de gebruiker worden verzonden. 

Als er bijvoorbeeld een noodgeval optreedt bij het primaire eindpunt, kunt u **errorAction** gebruiken om een secundair eindpunt aan te roepen of om een foutafhandelingseindpunt te rapporteren. 

Net als de primaire **action** kunt u voor de foutactie eenvoudige of samengestelde logica gebruiken op basis van andere acties. 

<a name="recurrence"></a>

## <a name="recurrence"></a>recurrence

Een job wordt herhaald als de JSON-definitie van de job het object **recurrence** omvat, bijvoorbeeld:

```json
"recurrence": {
   "frequency": "Week",
   "interval": 1,
   "schedule": {
      "hours": [10, 22],
      "minutes": [0, 30],
      "weekDays": ["Monday", "Wednesday", "Friday"]
   },
   "count": 10,
   "endTime": "2012-11-04"
},
```

| Eigenschap | Vereist | Value | Description | 
|----------|----------|-------|-------------| 
| **frequency** | Ja, als **recurrence** wordt gebruikt | Minuut, Uur, Dag, Week, Maand, Jaar | De tijdseenheid tussen de opgetreden gevallen | 
| **interval** | Nee | 1 tot en met 1000 | Een positief geheel getal dat het aantal tijdseenheden tussen de opgetreden gevallen bepaalt op basis van **frequency** | 
| **schedule** | Nee | Varieert | De details voor complexere en geavanceerdere schema's. Zie **hours**, **minutes**, **weekDays**, **months** en **monthDays** | 
| **hours** | Nee | 1 tot 24 | Een matrix met de uuraanduidingen voor wanneer de job moet worden uitgevoerd | 
| **minutes** | Nee | 0-59 | Een matrix met de minuutaanduidingen voor wanneer de job moet worden uitgevoerd | 
| **months** | Nee | 1 tot 12 | Een matrix met de maanden voor wanneer de job moet worden uitgevoerd | 
| **monthDays** | Nee | Varieert | Een matrix met de dagen van de maand voor wanneer de job moet worden uitgevoerd | 
| **weekDays** | Nee | Maandag, Dinsdag, Woensdag, Donderdag, Vrijdag, Zaterdag, Zondag | Een matrix met de dagen van de week voor wanneer de job moet worden uitgevoerd | 
| **count** | Nee | <*geen*> | Het aantal opgetreden gevallen. Het standaardgeval is oneindige herhaling. **count** en **endTime** mogen niet tegelijk worden gebruikt, maar er wordt voldaan aan de regel die het eerst wordt voltooid. | 
| **endTime** | Nee | <*geen*> | De datum en tijd waarop het terugkeerpatroon moet worden gestopt. Het standaardgeval is oneindige herhaling. **count** en **endTime** mogen niet tegelijk worden gebruikt, maar er wordt voldaan aan de regel die het eerst wordt voltooid. | 
||||

Zie [Complexe schema's en geavanceerde terugkeerpatronen bouwen](../scheduler/scheduler-advanced-complexity.md) voor meer informatie over deze elementen.

<a name="retry-policy"></a>

## <a name="retrypolicy"></a>retryPolicy

In het geval dat een Scheduler-job mislukt, kunt u een beleid voor opnieuw proberen instellen. Dit bepaalt of en hoe de actie wordt herhaald. Standaard wordt de job vier maal herhaald met een interval van dertig seconden. U kunt dit beleid meer of minder agressief maken; bijvoorbeeld dat een actie tweemaal per dag wordt herhaald:

```json
"retryPolicy": { 
   "retryType": "Fixed",
   "retryInterval": "PT1D",
   "retryCount": 2
},
```

| Eigenschap | Vereist | Value | Description | 
|----------|----------|-------|-------------| 
| **retryType** | Ja | **Vast**, **Geen** | Bepaalt of u een beleid voor opnieuw proberen opgeeft (**vast**) of niet (**geen**). | 
| **retryInterval** | Nee | PT30S | Geeft het interval en de frequentie op tussen herhaalpogingen in de [indeling ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). De minimumwaarde is 15 seconden; de maximumwaarde is 18 maanden. | 
| **retryCount** | Nee | 4 | Geeft het aantal nieuwe herhaalpogingen op. De maximumwaarde is 20. | 
||||

Zie [High availability and reliability](../scheduler/scheduler-high-availability-reliability.md) (Hoge beschikbaarheid en betrouwbaarheid) voor meer informatie.

<a name="status"></a>

## <a name="state"></a>state

De status van een job is **Ingeschakeld**, **Uitgeschakeld**, **Voltooid** of **Mislukt**, bijvoorbeeld: 

`"state": "Disabled"`

Als u jobs wilt wijzigen in de status **Ingeschakeld** of **Uitgeschakeld**, kunt u de bewerking PUT of PATCH voor die jobs gebruiken.
Als een job echter de status **Voltooid** of **Mislukt** heeft, kunt u de status niet bijwerken, hoewel u een DELETE-bewerking voor de job kunt uitvoeren. Voltooide en mislukte jobs worden na zestig dagen verwijderd. 

<a name="status"></a>

## <a name="status"></a>status

Als een job is gestart, wordt informatie over de status van de job geretourneerd via het object **status**, dat alleen onder controle van Scheduler staat. U kunt het object **status** echter vinden in het object **job**. Hier ziet u de informatie die de status van een job bevat:

* Tijd voor de vorige uitvoering (indien van toepassing)
* Tijd voor de volgende geplande uitvoering voor actieve jobs
* Het aantal uitvoeringen van een job
* Het aantal mislukkingen (indien van toepassing)
* Het aantal fouten (indien van toepassing)

Bijvoorbeeld:

```json
"status": {
   "lastExecutionTime": "2007-03-01T13:00:00Z",
   "nextExecutionTime": "2007-03-01T14:00:00Z ",
   "executionCount": 3,
   "failureCount": 0,
   "faultedCount": 0
}
```

## <a name="see-also"></a>Zie ook

* [Wat is Azure Scheduler?](scheduler-intro.md)
* [Concepten, terminologie en entiteitenhiërarchie](scheduler-concepts-terms.md)
* [Complexe schema's en geavanceerde terugkeerpatronen bouwen](scheduler-advanced-complexity.md)
* [Limieten, quota, standaardwaarden en foutcodes](scheduler-limits-defaults-errors.md)
* [Naslaginformatie over REST API van Azure Scheduler](/rest/api/scheduler)
* [Naslaginformatie over Azure Scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)

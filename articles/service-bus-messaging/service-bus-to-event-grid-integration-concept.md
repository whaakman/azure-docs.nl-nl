---
title: Overzicht integratie Azure Service Bus met Azure Event Grid | Microsoft Docs
description: Beschrijving van de integratie van Service Bus-berichten met Event Grid
services: service-bus-messaging
documentationcenter: .net
author: ChristianWolf42
manager: timlt
editor: 
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 02/15/2018
ms.author: chwolf
ms.openlocfilehash: bf771428505081cb60ca4417f87a4f6c2afbd25d
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2018
---
# <a name="azure-service-bus-to-azure-event-grid-integration-overview"></a>Overzicht integratie Azure Service Bus en Event Grid

Azure Service Bus heeft een nieuwe integratie met Azure Event Grid geïntroduceerd. Het belangrijkste scenario dat deze functie mogelijk maakt, is dat Service Bus-wachtrijen of -abonnementen waarvoor lage volumes berichten voorkomen, geen ontvanger hoeven te hebben die constant navraag doet of er berichten zijn. Service Bus kan nu gebeurtenissen naar Azure Event Grid verzenden als er zich berichten in een wachtrij of abonnement bevinden zonder dat er ontvangers zijn. U kunt Event Grid-abonnementen maken naar uw Service Bus-naamruimten, naar deze gebeurtenissen luisteren en erop reageren door een ontvanger te starten. Met deze functie kan Service Bus in reactieve programmeermodellen worden gebruikt.

Als u deze functie wilt inschakelen, hebt u het volgende nodig:

* Een Azure Service Bus Premium-naamruimte met ten minste één Service Bus-wachtrij of een Service Bus-onderwerp met ten minste één abonnement.
* Inzender-toegang tot de Azure Service Bus-naamruimte.
* Bovendien moet u een abonnement op Azure Event Grid voor de Service Bus-naamruimte hebben. Met het abonnement wordt de melding opgehaald uit Azure Event Grid dat er berichten klaarstaan. Veelvoorkomende abonnees kunnen Logic Apps, Azure Functions of een webhook zijn die contact opnemen met een web-app en die vervolgens de berichten verwerken. 

![19][]

### <a name="verify-that-you-have-contributor-access"></a>Controleren of u toegang op Inzender-niveau hebt

Navigeer naar uw Service Bus-naamruimte en selecteer Toegangsbeheer (IAM) zoals hieronder wordt weergegeven:

![1][]

### <a name="events-and-event-schemas"></a>Gebeurtenissen en gebeurtenisschema's

Azure Service Bus gaat vandaag gebeurtenissen verzenden voor twee scenario's.

* [ActiveMessagesWithNoListenersAvailable](#active-messages-available-event)
* [DeadletterMessagesAvailable](#dead-lettered-messages-available-event)

Bovendien worden de standaard Azure Event Grid-beveiliging en [verificatiemechanismen](https://docs.microsoft.com/en-us/azure/event-grid/security-authentication) gebruikt.

Volg [deze](https://docs.microsoft.com/en-us/azure/event-grid/event-schema) koppeling voor meer informatie over de gebeurtenisschema's van Event Grid.

#### <a name="active-messages-available-event"></a>De gebeurtenis Actieve berichten beschikbaar

Deze gebeurtenis wordt gegenereerd als er actieve berichten in een wachtrij of abonnement zijn, en er geen ontvangers luisteren.

Het schema voor deze gebeurtenis is als volgt:

```JSON
{
  "topic": "/subscriptions/<subscription id>/resourcegroups/DemoGroup/providers/Microsoft.ServiceBus/namespaces/<YOUR SERVICE BUS NAMESPACE WILL SHOW HERE>",
  "subject": "topics/<service bus topic>/subscriptions/<service bus subscription>",
  "eventType": "Microsoft.ServiceBus.ActiveMessagesAvailableWithNoListeners",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://YOUR-SERVICE-BUS-NAMESPACE-WILL-SHOW-HERE.servicebus.windows.net/TOPIC-NAME/subscriptions/SUBSCRIPTIONNAME/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}
```

#### <a name="dead-lettered-messages-available-event"></a>De gebeurtenis Berichten beschikbaar in de wachtrij voor onbestelbare berichten

U krijgt minimaal een gebeurtenis per wachtrij voor onbestelbare berichten, waarin zich berichten bevinden maar waar geen actieve ontvangers zijn.

Het schema voor deze gebeurtenis is als volgt:

```JSON
[{
  "topic": "/subscriptions/<subscription id>/resourcegroups/DemoGroup/providers/Microsoft.ServiceBus/namespaces/<YOUR SERVICE BUS NAMESPACE WILL SHOW HERE>",
  "subject": "topics/<service bus topic>/subscriptions/<service bus subscription>",
  "eventType": "Microsoft.ServiceBus.DeadletterMessagesAvailableWithNoListener",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://YOUR-SERVICE-BUS-NAMESPACE-WILL-SHOW-HERE.servicebus.windows.net/TOPIC-NAME/subscriptions/SUBSCRIPTIONNAME/$deadletterqueue/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="how-often-and-how-many-events-are-emitted"></a>Hoe vaak en hoeveel gebeurtenissen worden er verzonden?

Als u meerdere wachtrijen en onderwerpen/abonnementen in de naamruimte hebt, krijgt u minimaal één gebeurtenis per wachtrij en één per abonnement. De gebeurtenissen worden onmiddellijk verzonden als er geen berichten in de Service Bus-entiteit voorkomen en er een nieuw bericht binnenkomt, of elke twee minuten tenzij Azure Service Bus een actieve ontvanger detecteert. De gebeurtenissen worden niet onderbroken als u door berichten bladert.

Standaard verzendt Azure Service Bus gebeurtenissen voor alle entiteiten in de naamruimte. Als u alleen voor specifieke entiteiten gebeurtenissen wilt ophalen, raadpleegt u de volgende sectie over filteren.

### <a name="filtering-limiting-from-where-you-get-events"></a>Filteren, beperken van waar u wilt dat gebeurtenissen worden opgehaald

Als u gebeurtenissen bijvoorbeeld alleen van één wachtrij of één abonnement binnen uw naamruimte wilt ontvangen, kunt u de filters 'Begint met' of 'Eindigt op' van Azure Event Grid gebruiken. In sommige interfaces worden deze filters 'voorvoegselfilter' en 'achtervoegselfilter' genoemd. Als u gebeurtenissen voor meerdere maar niet alle wachtrijen en abonnementen wilt ontvangen, kunt u meerdere verschillende Azure Event Grid-abonnementen maken en voor elk ervan een filter opgeven.

## <a name="how-to-create-azure-event-grid-subscriptions-for-service-bus-namespaces"></a>Azure Event Grid-abonnementen maken voor Service Bus-naamruimten

Er zijn drie verschillende manieren om Event Grid-abonnementen voor Service Bus-naamruimten te maken.

* [Azure Portal](#portal-instructions)
* [Azure-CLI](#azure-cli-instructions)
* [PowerShell](#powershell-instructions)

## <a name="portal-instructions"></a>Instructies voor portal

Als u een nieuw Azure Event Grid-abonnement wilt maken navigeert u naar uw naamruimte in Azure Portal en selecteert u de blade Event Grid. Klik op '+ Event Grid-abonnement'. hieronder ziet u een naamruimte waarvoor al een aantal Event Grid-abonnementen bestaan.

![20][]

In de volgende schermafbeelding ziet u een voorbeeld van hoe u zich kunt abonneren op een Azure-functie of een webhook, zonder dat er een specifiek filter nodig is:

![21][]

## <a name="azure-cli-instructions"></a>Instructies voor Azure CLI

Controleer eerst of u ten minste Azure CLI versie 2.0 hebt geïnstalleerd. U kunt het installatieprogramma hier downloaden. Druk vervolgens u op 'Windows+X' en open een nieuwe PowerShell-console met beheerdersmachtigingen. U kunt ook een opdrachtshell in Azure Portal gebruiken.

Voer de volgende code uit:

```PowerShell
Az login

Aa account set -s “THE SUBSCRIPTION YOU WANT TO USE”

$namespaceid=(az resource show --namespace Microsoft.ServiceBus --resource-type namespaces --name “<yourNamespace>“--resource-group “<Your Resource Group Name>” --query id --output tsv)

az eventgrid event-subscription create --resource-id $namespaceid --name “<YOUR EVENT GRID SUBSCRIPTION NAME (CAN BE ANY NOT EXISTING)>” --endpoint “<your_function_url>” --subject-ends-with “<YOUR SERVICE BUS SUBSCRIPTION NAME>”
```

## <a name="powershell-instructions"></a>Instructies voor PowerShell

Controleer of u Azure PowerShell hebt geïnstalleerd. U vindt deze hier. Druk vervolgens u op 'Windows+X' en open een nieuwe PowerShell-console met beheerdersmachtigingen. U kunt ook een opdrachtshell in Azure Portal gebruiken.

```PowerShell
Login-AzureRmAccount

Select-AzureRmSubscription -SubscriptionName "<YOUR SUBSCRIPTION NAME>"

# This might be installed already
Install-Module AzureRM.ServiceBus

$NSID = (Get-AzureRmServiceBusNamespace -ResourceGroupName "<YOUR RESOURCE GROUP NAME>" -Na
mespaceName "<YOUR NAMESPACE NAME>").Id 

New-AzureRmEVentGridSubscription -EventSubscriptionName “<YOUR EVENT GRID SUBSCRIPTION NAME (CAN BE ANY NOT EXISTING)>” -ResourceId $NSID -Endpoint "<YOUR FUNCTION URL>” -SubjectEndsWith “<YOUR SERVICE BUS SUBSCRIPTION NAME>”
```

U kunt nu kennismaken met de andere instelopties of [testen of er gebeurtenissen stromen](#test-that-events-are-flowing).

## <a name="next-steps"></a>Volgende stappen

* Service Bus- en Event Grid[voorbeelden](service-bus-to-event-grid-integration-example.md).
* Meer informatie over [Azure Event Grid](https://docs.microsoft.com/en-us/azure/azure-functions/).
* Meer informatie over [Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/).
* Meer informatie over [Azure Logic Apps](https://docs.microsoft.com/en-us/azure/logic-apps/).
* Meer informatie over [Azure Service Bus](https://docs.microsoft.com/en-us/azure/azure-functions/).

[1]: ./media/service-bus-to-event-grid-integration-concept/sbtoeventgrid1.png
[19]: ./media/service-bus-to-event-grid-integration-concept/sbtoeventgriddiagram.png
[8]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid8.png
[9]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid9.png
[20]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal.png
[21]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal2.png
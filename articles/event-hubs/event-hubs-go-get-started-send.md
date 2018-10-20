---
title: Gebeurtenissen verzenden naar Azure Event Hubs met behulp van Go | Microsoft Docs
description: Aan de slag verzenden van gebeurtenissen naar Event Hubs met behulp van Go
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 10/18/2018
ms.author: shvija
ms.openlocfilehash: f5e30a103b09613caee8e9912a89a5bc2d390f65
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49458085"
---
# <a name="send-events-to-event-hubs-using-go"></a>Gebeurtenissen verzenden naar Event Hubs met behulp van Go

Azure Event Hubs is een big data-platform voor het streamen van gegevens en een gebeurtenisopneemservice die miljoenen gebeurtenissen per seconde kan opnemen en verwerken. Event Hubs kan gebeurtenissen, gegevens of telemetrie die wordt geproduceerd door gedistribueerde software en apparaten verwerken en opslaan. Gegevens die naar een Event Hub worden verzonden, kunnen worden omgezet en opgeslagen via een provider voor realtime analytische gegevens of batchverwerking/opslagadapters. Zie voor een gedetailleerd overzicht van Event Hubs [overzicht van Event Hubs](event-hubs-about.md) en [functies van Event Hubs](event-hubs-features.md).

Deze zelfstudie wordt beschreven hoe u gebeurtenissen naar een event hub verzendt vanuit een toepassing die is geschreven in Go. 

> [!NOTE]
> U kunt deze snelstartgids downloaden als een voorbeeld van de [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/eventhubs), Vervang `EventHubConnectionString` en `EventHubName` tekenreeksen door uw event hub-waarden en voer deze uit. U kunt ook kunt u de stappen in deze zelfstudie om te maken van uw eigen volgen.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

* Ga lokaal is geïnstalleerd. Ga als volgt [deze instructies](https://golang.org/doc/install) indien nodig.
* Een bestaande Event Hubs-naamruimte en event hub. U kunt deze entiteiten maken door de instructies in [in dit artikel](event-hubs-create.md).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Een Event Hubs-naamruimte en een Event Hub maken
In de eerste stap gebruikt u [Azure Portal](https://portal.azure.com) om een naamruimte van het type Event Hubs te maken en de beheerreferenties te verkrijgen die de toepassing nodig heeft om met de Event Hub te communiceren. Als u wilt een naamruimte en een event hub maken, volgt u de procedure in [in dit artikel](event-hubs-create.md), gaat u verder met de volgende stappen in deze zelfstudie.

## <a name="install-go-package"></a>Go-pakket installeren

Ophalen van de Go-pakket voor Event Hubs met `go get` of `dep`. Bijvoorbeeld:

```bash
go get -u github.com/Azure/azure-event-hubs-go
go get -u github.com/Azure/azure-amqp-common-go/...

# or

dep ensure -add github.com/Azure/azure-event-hubs-go
dep ensure -add github.com/Azure/azure-amqp-common-go
```

## <a name="import-packages-in-your-code-file"></a>Pakketten in uw codebestand importeren

Gebruik het volgende codevoorbeeld voor het importeren van de Go-pakketten:

```go
import (
    aad "github.com/Azure/azure-amqp-common-go/aad"
    eventhubs "github.com/Azure/azure-event-hubs-go"
)
```

## <a name="create-service-principal"></a>Een service-principal maken

Maak een nieuwe serviceprincipal door de instructies in [een Azure service-principal maken met Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli). De opgegeven referenties opslaan in uw omgeving met de volgende namen. Zowel de Azure SDK voor Go en de Event Hubs-pakketten vooraf zijn geconfigureerd voor het zoeken naar deze namen van variabelen:

```bash
export AZURE_CLIENT_ID=
export AZURE_CLIENT_SECRET=
export AZURE_TENANT_ID=
export AZURE_SUBSCRIPTION_ID= 
```

Maak nu een autorisatieprovider voor uw Event Hubs-client die u deze referenties gebruikt:

```go
tokenProvider, err := aad.NewJWTProvider(aad.JWTProviderWithEnvironmentVars())
if err != nil {
    log.Fatalf("failed to configure AAD JWT provider: %s\n", err)
}
```

## <a name="create-event-hubs-client"></a>Event Hubs-client maken

De volgende code maakt een Event Hubs-client:

```go
hub, err := eventhubs.NewHub("namespaceName", "hubName", tokenProvider)
ctx := context.WithTimeout(context.Background(), 10 * time.Second)
defer hub.Close(ctx)
if err != nil {
    log.Fatalf("failed to get hub %s\n", err)
}
```

## <a name="send-messages"></a>Berichten verzenden

Gebruik (1) voor het verzenden van berichten interactief vanuit een terminal of (2) het verzenden van berichten in uw programma in het volgende codefragment:

```go
// 1. send messages at the terminal
ctx = context.Background()
reader := bufio.NewReader(os.Stdin)
for {
    fmt.Printf("Input a message to send: ")
    text, _ := reader.ReadString('\n')
    hub.Send(ctx, eventhubs.NewEventFromString(text))
}

// 2. send messages within program
ctx = context.Background()
hub.Send(ctx, eventhubs.NewEventFromString("hello Azure!")
```

## <a name="extras"></a>Extra 's

Haal de id's van de partities in uw event hub:

```go
info, err := hub.GetRuntimeInformation(ctx)
if err != nil {
    log.Fatalf("failed to get runtime info: %s\n", err)
}
log.Printf("got partition IDs: %s\n, info.PartitionIDs)
```

Voer de toepassing voor het verzenden van gebeurtenissen naar de event hub. 

Gefeliciteerd! U hebt nu berichten verzonden naar een Event Hub.

## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids hebt u berichten verzonden naar een event hub met behulp van Go. Zie voor meer informatie over gebeurtenissen ontvangen van een event hub met behulp van Go, [ontvangen van gebeurtenissen van event hub - Go](event-hubs-go-get-started-receive-eph.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

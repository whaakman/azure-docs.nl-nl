---
title: Gebeurtenissen verzenden naar Azure Event Hubs met behulp van Go | Microsoft Docs
description: Aan de slag verzenden van gebeurtenissen naar Event Hubs met behulp van Go
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 07/23/2018
ms.author: shvija
ms.openlocfilehash: 40b3aa82c3e9e8ab9a30362c0a41998877655725
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005594"
---
# <a name="send-events-to-event-hubs-using-go"></a>Gebeurtenissen verzenden naar Event Hubs met behulp van Go

Azure Event Hubs is een uiterst schaalbare gebeurtenissen beheersysteem dat miljoenen verwerken kan gebeurtenissen per seconde, zodat toepassingen om te verwerken en analyseren van enorme hoeveelheden gegevens die worden geproduceerd door verbonden apparaten en andere systemen. Zodra de verzameld in een event hub, u kunt ontvangen en verwerken van gebeurtenissen met in-process-handlers of doorsturen naar andere systemen analytics.

Zie voor meer informatie over Event Hubs, de [overzicht van Event Hubs][Event Hubs overview].

Deze zelfstudie wordt beschreven hoe u gebeurtenissen naar een event hub verzendt vanuit een toepassing die is geschreven in Go. Om gebeurtenissen te ontvangen, gebruiken de **eph gaat** (Event Processor Host)-pakket, zoals beschreven in [de betreffende ontvangen artikel](event-hubs-go-get-started-receive-eph.md).

Code in deze zelfstudie is afkomstig uit [deze GitHub-voorbeelden](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/eventhubs), die u kunt controleren om te zien van de volledige werkende toepassing, met inbegrip van importinstructies en variabelendeclaraties.

Andere voorbeelden zijn beschikbaar [in de Event Hubs opslagplaats pakket](https://github.com/Azure/azure-event-hubs-go/tree/master/_examples).

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

* Ga lokaal is geïnstalleerd. Ga als volgt [deze instructies](https://golang.org/doc/install) indien nodig.
* Een actief Azure-account. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account][] aan voordat u begint.
* Een bestaande Event Hubs-naamruimte en event hub. U kunt deze entiteiten maken door de instructies in [in dit artikel](event-hubs-create.md).

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

## <a name="next-steps"></a>Volgende stappen

Ga naar de volgende pagina's voor meer informatie over Event Hubs:

* [Gebeurtenissen ontvangen met EventProcessorHost](event-hubs-go-get-started-receive-eph.md)
* [Event Hubs-overzicht][Event Hubs overview]
* [Een Event Hub maken](event-hubs-create.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[gratis account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

---
title: Verzenden en ontvangen van gebeurtenissen met behulp van Go - Azure Event Hubs | Microsoft Docs
description: Dit artikel bevat een overzicht voor het maken van een Go-App die gebeurtenissen uit Azure Event Hubs verzendt.
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 04/15/2019
ms.author: shvija
ms.openlocfilehash: 823ebc985c77785f8b48d12d5919dbbd1b2b1459
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60821689"
---
# <a name="send-events-to-or-receive-events-from-event-hubs-using-go"></a>Gebeurtenissen te verzenden of ontvangen van gebeurtenissen van Event Hubs met behulp van Go
Azure Event Hubs is een big data-platform voor het streamen van gegevens en een gebeurtenisopneemservice die miljoenen gebeurtenissen per seconde kan opnemen en verwerken. Event Hubs kan gebeurtenissen, gegevens of telemetrie die wordt geproduceerd door gedistribueerde software en apparaten verwerken en opslaan. Gegevens die naar een Event Hub worden verzonden, kunnen worden omgezet en opgeslagen via een provider voor realtime analytische gegevens of batchverwerking/opslagadapters. Zie [Overzicht van Event Hubs](event-hubs-about.md) en [Functies van Event Hubs](event-hubs-features.md) voor een gedetailleerd overzicht van Event Hubs.

In deze zelfstudie wordt beschreven hoe u Go-toepassingen schrijven voor gebeurtenissen te verzenden of ontvangen van gebeurtenissen van een event hub. 

> [!NOTE]
> U kunt deze snelstart als voorbeeld downloaden van de [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/eventhubs), de tekenreeksen `EventHubConnectionString` en `EventHubName` vervangen door uw event hub-waarden en deze uitvoeren. U kunt ook de stappen in deze zelfstudie volgen om uw eigen oplossing te maken.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

- Ga lokaal is geïnstalleerd. Ga als volgt [deze instructies](https://golang.org/doc/install) indien nodig.
- Een actief Azure-account. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account][] aan voordat u begint.
- **Maak een Event Hubs-naamruimte en een event hub**. Gebruik de [Azure-portal](https://portal.azure.com) voor het maken van een naamruimte van het type Event Hubs en de beheerreferenties die de toepassing nodig heeft om te communiceren met de event hub te verkrijgen. Volg de procedure in [dit artikel](event-hubs-create.md) om een naamruimte en een Event Hub te maken.

## <a name="send-events"></a>Gebeurtenissen verzenden
Deze sectie leest u over het maken van een Go-App voor het verzenden van gebeurtenissen naar een event hub. 

### <a name="install-go-package"></a>Go-pakket installeren

Ophalen van de Go-pakket voor Event Hubs met `go get` of `dep`. Bijvoorbeeld:

```bash
go get -u github.com/Azure/azure-event-hubs-go
go get -u github.com/Azure/azure-amqp-common-go/...

# or

dep ensure -add github.com/Azure/azure-event-hubs-go
dep ensure -add github.com/Azure/azure-amqp-common-go
```

### <a name="import-packages-in-your-code-file"></a>Pakketten in uw codebestand importeren

Gebruik het volgende codevoorbeeld voor het importeren van de Go-pakketten:

```go
import (
    aad "github.com/Azure/azure-amqp-common-go/aad"
    eventhubs "github.com/Azure/azure-event-hubs-go"
)
```

### <a name="create-service-principal"></a>Een service-principal maken

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

### <a name="create-event-hubs-client"></a>Event Hubs-client maken

De volgende code maakt een Event Hubs-client:

```go
hub, err := eventhubs.NewHub("namespaceName", "hubName", tokenProvider)
ctx := context.WithTimeout(context.Background(), 10 * time.Second)
defer hub.Close(ctx)
if err != nil {
    log.Fatalf("failed to get hub %s\n", err)
}
```

### <a name="write-code-to-send-messages"></a>Code schrijven om berichten te verzenden

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
hub.Send(ctx, eventhubs.NewEventFromString("hello Azure!"))
```

### <a name="extras"></a>Extra 's

Haal de id's van de partities in uw event hub:

```go
info, err := hub.GetRuntimeInformation(ctx)
if err != nil {
    log.Fatalf("failed to get runtime info: %s\n", err)
}
log.Printf("got partition IDs: %s\n", info.PartitionIDs)
```

Voer de toepassing voor het verzenden van gebeurtenissen naar de event hub. 

Gefeliciteerd! U hebt nu berichten verzonden naar een Event Hub.

## <a name="receive-events"></a>Gebeurtenissen ontvangen

### <a name="create-a-storage-account-and-container"></a>Een Storage-account en een container maken

Status, zoals de leases op partities en de controlepunten in de gebeurtenis stream worden gedeeld tussen ontvangers met behulp van een Azure Storage-container. U kunt een storage-account en een container maken met de Go SDK, maar u kunt ook maken volgt u de instructies in [over Azure storage-accounts](../storage/common/storage-create-storage-account.md).

Voorbeelden voor het maken van opslag-artefacten met de SDK voor Go zijn beschikbaar in de [Go-voorbeelden opslagplaats](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage) en in het voorbeeld dat overeenkomt met deze zelfstudie.

### <a name="go-packages"></a>Ga-pakketten

De Go-pakketten voor Event Hubs met ophalen en voor het ontvangen van berichten, `go get` of `dep`:

```bash
go get -u github.com/Azure/azure-event-hubs-go/...
go get -u github.com/Azure/azure-amqp-common-go/...
go get -u github.com/Azure/go-autorest/...

# or

dep ensure -add github.com/Azure/azure-event-hubs-go
dep ensure -add github.com/Azure/azure-amqp-common-go
dep ensure -add github.com/Azure/go-autorest
```

### <a name="import-packages-in-your-code-file"></a>Pakketten in uw codebestand importeren

Gebruik het volgende codevoorbeeld voor het importeren van de Go-pakketten:

```go
import (
    aad "github.com/Azure/azure-amqp-common-go/aad"
    eventhubs "github.com/Azure/azure-event-hubs-go"
    eph "github.com/Azure/azure-event-hubs-go/eph"
    storageLeaser "github.com/Azure/azure-event-hubs-go/storage"
    azure "github.com/Azure/go-autorest/autorest/azure"
)
```

### <a name="create-service-principal"></a>Een service-principal maken

Maak een nieuwe serviceprincipal door de instructies in [een Azure service-principal maken met Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli). De opgegeven referenties opslaan in uw omgeving met de volgende namen: Zowel Azure SDK voor Go en Event Hubs-pakket vooraf zijn geconfigureerd om te zoeken naar deze namen van variabelen.

```bash
export AZURE_CLIENT_ID=
export AZURE_CLIENT_SECRET=
export AZURE_TENANT_ID=
export AZURE_SUBSCRIPTION_ID= 
```

Maak vervolgens een autorisatieprovider voor uw Event Hubs-client die u deze referenties gebruikt:

```go
tokenProvider, err := aad.NewJWTProvider(aad.JWTProviderWithEnvironmentVars())
if err != nil {
    log.Fatalf("failed to configure AAD JWT provider: %s\n", err)
}
```

### <a name="get-metadata-struct"></a>Ophalen van metagegevens struct

Krijg een struct met metagegevens over uw Azure-omgeving met behulp van de Go SDK van Azure. In de struct hoger operations gebruiken om juiste eindpunten.

```go
azureEnv, err := azure.EnvironmentFromName("AzurePublicCloud")
if err != nil {
    log.Fatalf("could not get azure.Environment struct: %s\n", err)
}
```

### <a name="create-credential-helper"></a>Referentie-helper maken 

Maak een referentie-helper die gebruikmaakt van de vorige Azure Active Directory (AAD)-referenties te maken van een Shared Access Signature (SAS)-referentie voor de opslag. De laatste parameter geeft deze constructor de dezelfde omgevingsvariabelen gebruiken als u eerder hebt gebruikt:

```go
cred, err := storageLeaser.NewAADSASCredential(
    subscriptionID,
    resourceGroupName,
    storageAccountName,
    storageContainerName,
    storageLeaser.AADSASCredentialWithEnvironmentVars())
if err != nil {
    log.Fatalf("could not prepare a storage credential: %s\n", err)
}
```

### <a name="create-a-check-pointer-and-a-leaser"></a>Maken van een selectievakje-verwijzing en een leaser 

Maakt een **leaser**, dat verantwoordelijk is voor het leasen van een partitie op een bepaalde ontvanger en een **aanwijzer controleren**, dat verantwoordelijk is voor het schrijven van controlepunten voor de berichtenstroom zodat andere ontvangers kunnen beginnen met het lezen van de juiste verschuiving.

Op dit moment één **StorageLeaserCheckpointer** is beschikbaar die gebruikmaakt van de dezelfde opslag-container voor het beheren van zowel de leases en de controlepunten. Naast de naam van de account en de container voor opslag, de **StorageLeaserCheckpointer** moet de referentie op die in de vorige stap en de Azure-omgeving struct correct toegang tot de container hebt gemaakt.

```go
leaserCheckpointer, err := storageLeaser.NewStorageLeaserCheckpointer(
    cred,
    storageAccountName,
    storageContainerName,
    azureEnv)
if err != nil {
    log.Fatalf("could not prepare a storage leaserCheckpointer: %s\n", err)
}
```

### <a name="construct-event-processor-host"></a>Event Processor Host maken

U hebt nu de onderdelen die nodig zijn om een EventProcessorHost als volgt samen te stellen. Dezelfde **StorageLeaserCheckpointer** wordt gebruikt als een leaser en de controle-aanwijzer, zoals eerder beschreven:

```go
ctx := context.Background()
p, err := eph.New(
    ctx,
    nsName,
    hubName,
    tokenProvider,
    leaserCheckpointer,
    leaserCheckpointer)
if err != nil {
    log.Fatalf("failed to create EPH: %s\n", err)
}
defer p.Close(context.Background())
```

### <a name="create-handler"></a>Handler maken 

Nu een handler maken en registreren bij de Event Processor Host. Als de host wordt gestart, geldt dit deze en andere opgegeven handlers voor inkomende berichten:

```go
handler := func(ctx context.Context, event *eventhubs.Event) error {
    fmt.Printf("received: %s\n", string(event.Data))
    return nil
}

// register the handler with the EPH
_, err := p.RegisterHandler(ctx, handler)
if err != nil {
    log.Fatalf("failed to register handler: %s\n", err)
}
```

### <a name="write-code-to-receive-messages"></a>Code schrijven om berichten te ontvangen

Met alles wat u instelt, kunt u de Event Processor Host met starten `Start(context)` om het te laten permanent actief of met `StartNonBlocking(context)` om uit te voeren alleen zolang er berichten zijn beschikbaar.

In deze zelfstudie wordt gestart en wordt uitgevoerd als volgt; Zie het GitHub-voorbeeld voor het gebruik van een voorbeeld `StartNonBlocking`:

```go
ctx := context.Background()
err = p.Start()
if err != nil {
    log.Fatalf("failed to start EPH: %s\n", err)
}
```

## <a name="next-steps"></a>Volgende stappen
Lees de volgende artikelen:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Functies en de belangrijkste termen in de Azure Event Hubs](event-hubs-features.md)
- [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)


<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[gratis account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

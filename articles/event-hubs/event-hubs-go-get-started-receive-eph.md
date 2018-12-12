---
title: Ontvangen van gebeurtenissen met behulp van Go - Azure Event Hubs | Microsoft Docs
description: Dit artikel bevat een overzicht voor het maken van een Go-App die gebeurtenissen uit Azure Event Hubs ontvangt.
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: db952b82172928e42e951563d98bb32b275e8af7
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53084988"
---
# <a name="receive-events-from-event-hubs-using-go"></a>Gebeurtenissen ontvangen van Event Hubs met behulp van Go

Azure Event Hubs is een uiterst schaalbare gebeurtenissen beheersysteem dat miljoenen verwerken kan gebeurtenissen per seconde, zodat toepassingen om te verwerken en analyseren van enorme hoeveelheden gegevens die worden geproduceerd door verbonden apparaten en andere systemen. Zodra de verzameld in een event hub, u kunt ontvangen en verwerken van gebeurtenissen met in-process-handlers of doorsturen naar andere systemen analytics.

Zie voor meer informatie over Event Hubs, de [overzicht van Event Hubs][Event Hubs overview].

In deze zelfstudie wordt beschreven hoe u gebeurtenissen ontvangen van een event hub in een Go-App. Voor meer informatie over het verzenden van gebeurtenissen Zie [de betreffende verzenden artikel](event-hubs-go-get-started-send.md).

Code in deze zelfstudie is afkomstig uit [deze GitHub-voorbeelden](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/eventhubs), die u kunt controleren om te zien van de volledige instructies en variabelendeclaraties werken met inbegrip van toepassing importeren.

Andere voorbeelden zijn beschikbaar [in de Event Hubs opslagplaats pakket](https://github.com/Azure/azure-event-hubs-go/tree/master/_examples).

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie moet u de volgende vereisten:

* Ga lokaal is geïnstalleerd. Ga als volgt [deze instructies](https://golang.org/doc/install) indien nodig.
* Een actief Azure-account. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account][] aan voordat u begint.
* Om berichten te ontvangen, moet er berichten in de doel-event hub. Meer informatie over het verzenden van berichten de [verzenden zelfstudie](event-hubs-go-get-started-send.md).
* Een bestaande event hub (Zie de volgende sectie).
* Een bestaand opslagaccount en container (Zie de sectie na de volgende sectie).

### <a name="create-an-event-hub"></a>Een Event Hub maken

Deze zelfstudie begint met een bestaande Event Hubs-naamruimte en event hub. U kunt deze entiteiten maken door de instructies in [in dit artikel](event-hubs-create.md).

### <a name="create-a-storage-account-and-container"></a>Een Storage-account en een container maken

Status, zoals de leases op partities en de controlepunten in de gebeurtenis stream worden gedeeld tussen ontvangers met behulp van een Azure Storage-container. U kunt een storage-account en een container maken met de Go SDK, maar u kunt ook maken volgt u de instructies in [over Azure storage-accounts](../storage/common/storage-create-storage-account.md).

Voorbeelden voor het maken van opslag-artefacten met de SDK voor Go zijn beschikbaar in de [Go-voorbeelden opslagplaats](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage) en in het voorbeeld dat overeenkomt met deze zelfstudie.

## <a name="receive-messages"></a>Berichten ontvangen

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

## <a name="import-packages-in-your-code-file"></a>Pakketten in uw codebestand importeren

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

## <a name="create-service-principal"></a>Een service-principal maken

Maak een nieuwe serviceprincipal door de instructies in [een Azure service-principal maken met Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli). De opgegeven referenties opslaan in uw omgeving met de volgende namen: zowel Azure SDK voor Go en Event Hubs-pakket vooraf zijn geconfigureerd voor het zoeken naar deze namen van variabelen.

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

## <a name="get-metadata-struct"></a>Ophalen van metagegevens struct

Krijg een struct met metagegevens over uw Azure-omgeving met behulp van de Go SDK van Azure. In de struct hoger operations gebruiken om juiste eindpunten.

```go
azureEnv, err := azure.EnvironmentFromName("AzurePublicCloud")
if err != nil {
    log.Fatalf("could not get azure.Environment struct: %s\n", err)
}
```

## <a name="create-credential-helper"></a>Referentie-helper maken 

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

## <a name="create-checkpointer-and-leaser"></a>Checkpointer en Leaser maken 

Maak een **Leaser**, dat verantwoordelijk is voor het leasen van een partitie op een bepaalde ontvanger en een **Checkpointer**, dat verantwoordelijk is voor het schrijven van controlepunten voor de berichtenstroom zodat andere ontvangers kunnen beginnen met het lezen van de juiste verschuiving.

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

## <a name="construct-event-processor-host"></a>Event Processor Host maken

U hebt nu de onderdelen die nodig zijn om een EventProcessorHost als volgt samen te stellen. Dezelfde **StorageLeaserCheckpointer** wordt gebruikt als een Leaser en Checkpointer, zoals eerder beschreven:

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

## <a name="create-handler"></a>Handler maken 

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

## <a name="receive-messages"></a>Berichten ontvangen

Met alles wat u instelt, kunt u de Event Processor Host met starten `Start(context)` om het te laten permanent actief of met `StartNonBlocking(context)` om uit te voeren alleen zolang er berichten zijn beschikbaar.

In deze zelfstudie wordt gestart en wordt uitgevoerd als volgt; Zie het GitHub-voorbeeld voor het gebruik van een voorbeeld `StartNonBlocking`:

```go
ctx := context.Background()
err = p.Start()
if err != nil {
    log.Fatalf("failed to start EPH: %s\n", err)
}
```

## <a name="notes"></a>Opmerkingen

In deze zelfstudie wordt één exemplaar van **EventProcessorHost** gebruikt. Als u wilt verhogen doorvoer en betrouwbaarheid, moet u meerdere exemplaren van **EventProcessorHost** op verschillende systemen. De Leaser system zorgt ervoor dat slechts één ontvanger is gekoppeld aan en ontvangt berichten van een opgegeven partitie op een bepaald tijdstip.

## <a name="next-steps"></a>Volgende stappen
In deze quickstart maakt u een Go-toepassing die berichten van een event hub ontvangen gemaakt. Zie voor meer informatie over het verzenden van gebeurtenissen naar een event hub met behulp van Go, [verzenden van gebeurtenissen van event hub - Go](event-hubs-go-get-started-send.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[gratis account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

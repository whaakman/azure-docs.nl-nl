---
title: Overzicht van Azure Relay standaard .NET API's | Microsoft Docs
description: Overzicht van Azure Standard Relay .NET-API
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: b1da9ac1-811b-4df7-a22c-ccd013405c40
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: sethm
ms.openlocfilehash: aa1863a44f00ae17f63b02c7c247b2c9fd9925f6
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="azure-relay-hybrid-connections-net-standard-api-overview"></a>Overzicht van Azure Relay hybride verbindingen .NET Standard API

In dit artikel ziet u een aantal van de sleutel Azure Relay hybride verbindingen .NET Standard [client-API's](/dotnet/api/microsoft.azure.relay).
  
## <a name="relay-connection-string-builder-class"></a>Relay verbindingsreeksen klasse

De [RelayConnectionStringBuilder] [ RelayConnectionStringBuilder] klasse indelingen verbindingsreeksen die specifiek voor Relay hybride verbindingen zijn. U kunt deze gebruiken om te controleren of de indeling van een verbindingsreeks of een verbindingsreeks vanaf het begin. Zie de volgende code voor een voorbeeld:

```csharp
var endpoint = "[Relay namespace]";
var entityPath = "[Name of the Hybrid Connection]";
var sharedAccessKeyName = "[SAS key name]";
var sharedAccessKey = "[SAS key value]";

var connectionStringBuilder = new RelayConnectionStringBuilder()
{
    Endpoint = endpoint,
    EntityPath = entityPath,
    SharedAccessKeyName = sasKeyName,
    SharedAccessKey = sasKeyValue
};
```

U kunt ook een verbindingsreeks rechtstreeks naar doorgeven de `RelayConnectionStringBuilder` methode. Deze bewerking kunt u controleren of de verbindingsreeks is een ongeldige indeling. Als u een van de parameters zijn ongeldig, de constructor genereert een `ArgumentException`.

```csharp
var myConnectionString = "[RelayConnectionString]";
// Declare the connectionStringBuilder so that it can be used outside of the loop if needed
RelayConnectionStringBuilder connectionStringBuilder;
try
{
    // Create the connectionStringBuilder using the supplied connection string
    connectionStringBuilder = new RelayConnectionStringBuilder(myConnectionString);
}
catch (ArgumentException ae)
{
    // Perform some error handling
}
```

## <a name="hybrid-connection-stream"></a>Stroom van hybride verbinding

De [HybridConnectionStream] [ HCStream] klasse is de primaire object dat wordt gebruikt voor het verzenden en ontvangen van gegevens van een eindpunt Azure Relay of u met werkt een [HybridConnectionClient] [ HCClient], of een [HybridConnectionListener][HCListener].

### <a name="getting-a-hybrid-connection-stream"></a>Ophalen van een stream van hybride verbinding

#### <a name="listener"></a>Listener

Met behulp van een [HybridConnectionListener] [ HCListener] object, kunt u een `HybridConnectionStream` object als volgt:

```csharp
// Use the RelayConnectionStringBuilder to get a valid connection string
var listener = new HybridConnectionListener(csb.ToString());
// Open a connection to the Relay endpoint
await listener.OpenAsync();
// Get a `HybridConnectionStream`
var hybridConnectionStream = await listener.AcceptConnectionAsync();
```

#### <a name="client"></a>Client

Met behulp van een [HybridConnectionClient] [ HCClient] object, kunt u een `HybridConnectionStream` object als volgt:

```csharp
// Use the RelayConnectionStringBuilder to get a valid connection string
var client = new HybridConnectionClient(csb.ToString());
// Open a connection to the Relay endpoint and get a `HybridConnectionStream`
var hybridConnectionStream = await client.CreateConnectionAsync();
```

### <a name="receiving-data"></a>Ontvangen van gegevens

De [HybridConnectionStream] [ HCStream] klasse wederzijdse communicatie mogelijk maakt. In de meeste gevallen ontvangt u continu van de stroom. Als u tekst uit de stroom leest, kunt u ook gebruik van een [StreamReader](https://msdn.microsoft.com/library/system.io.streamreader(v=vs.110).aspx) object, waardoor u gemakkelijker bij het parseren van de gegevens. U kunt bijvoorbeeld gegevens lezen als tekst, in plaats van als `byte[]`.

De volgende code worden afzonderlijke tekstregels gelezen uit de stroom totdat een annulering is aangevraagd:

```csharp
// Create a CancellationToken, so that we can cancel the while loop
var cancellationToken = new CancellationToken();
// Create a StreamReader from the 'hybridConnectionStream`
var streamReader = new StreamReader(hybridConnectionStream);

while (!cancellationToken.IsCancellationRequested)
{
    // Read a line of input until a newline is encountered
    var line = await streamReader.ReadLineAsync();
    if (string.IsNullOrEmpty(line))
    {
        // If there's no input data, we will signal that 
        // we will no longer send data on this connection
        // and then break out of the processing loop.
        await hybridConnectionStream.ShutdownAsync(cancellationToken);
        break;
    }
}
```

### <a name="sending-data"></a>Verzenden van gegevens

Wanneer u een verbinding tot stand gebracht, kunt u een bericht verzenden naar de Relay-eindpunt. Omdat het verbindingsobject neemt over [stroom](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx), verzenden van uw gegevens als een `byte[]`. Het volgende voorbeeld ziet u hoe u dit doet:

```csharp
var data = Encoding.UTF8.GetBytes("hello");
await clientConnection.WriteAsync(data, 0, data.Length);
```

Echter, als u tekst rechtstreeks verzenden wilt zonder de voor het coderen van de tekenreeks telkens wanneer u terugloopt de `hybridConnectionStream` object met een [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) object.

```csharp
// The StreamWriter object only needs to be created once
var textWriter = new StreamWriter(hybridConnectionStream);
await textWriter.WriteLineAsync("hello");
```

## <a name="next-steps"></a>Volgende stappen

Volg deze koppelingen voor meer informatie over Azure Relay:

* [Microsoft.Azure.Relay verwijzing](/dotnet/api/microsoft.azure.relay)
* [Wat is Azure Relay?](relay-what-is-it.md)
* [Beschikbare Relay-API 's](relay-api-overview.md)

[RelayConnectionStringBuilder]: /dotnet/api/microsoft.azure.relay.relayconnectionstringbuilder
[HCStream]: /dotnet/api/microsoft.azure.relay.hybridconnectionstream
[HCClient]: /dotnet/api/microsoft.azure.relay.hybridconnectionclient
[HCListener]: /dotnet/api/microsoft.azure.relay.hybridconnectionlistener
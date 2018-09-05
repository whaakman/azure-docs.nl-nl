---
title: Overzicht van Azure Relay .NET Standard-API's | Microsoft Docs
description: Overzicht van Azure Relay .NET Standard-API
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: b1da9ac1-811b-4df7-a22c-ccd013405c40
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: spelluru
ms.openlocfilehash: a6a1706c8d1e849fd1bb4309c46063dd3f9439c1
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43700137"
---
# <a name="azure-relay-hybrid-connections-net-standard-api-overview"></a>Overzicht van Azure Relay Hybrid Connections .NET Standard API

In dit artikel vindt u een samenvatting van de Azure Relay Hybrid Connections .NET Standard sleutel [client-API's](/dotnet/api/microsoft.azure.relay).
  
## <a name="relay-connection-string-builder-class"></a>Klasse van relay-opbouwfunctie voor verbindingsreeksen

De [RelayConnectionStringBuilder] [ RelayConnectionStringBuilder] klasse indelingen verbindingsreeksen die specifiek voor hybride Relay-verbindingen zijn. U kunt deze gebruiken om te controleren of de indeling van een verbindingsreeks of om een verbindingsreeks helemaal. Zie de volgende code voor een voorbeeld:

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

U kunt ook een verbindingsreeks rechtstreeks naar doorgeven de `RelayConnectionStringBuilder` methode. Met deze bewerking kunt u controleren of de verbindingsreeks is een geldige indeling hebben. Als een van de parameters ongeldig zijn, de constructor genereert een `ArgumentException`.

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

## <a name="hybrid-connection-stream"></a>Hybride verbinding stream

De [HybridConnectionStream] [ HCStream] klasse is de primaire object dat wordt gebruikt voor het verzenden en ontvangen van gegevens van een Azure Relay-eindpunt, of u met werkt nu een [HybridConnectionClient] [ HCClient], of een [HybridConnectionListener][HCListener].

### <a name="getting-a-hybrid-connection-stream"></a>Ophalen van een stream van hybride verbinding

#### <a name="listener"></a>Listener

Met behulp van een [HybridConnectionListener] [ HCListener] -object, kunt u een `HybridConnectionStream` object als volgt:

```csharp
// Use the RelayConnectionStringBuilder to get a valid connection string
var listener = new HybridConnectionListener(csb.ToString());
// Open a connection to the Relay endpoint
await listener.OpenAsync();
// Get a `HybridConnectionStream`
var hybridConnectionStream = await listener.AcceptConnectionAsync();
```

#### <a name="client"></a>Client

Met behulp van een [HybridConnectionClient] [ HCClient] -object, kunt u een `HybridConnectionStream` object als volgt:

```csharp
// Use the RelayConnectionStringBuilder to get a valid connection string
var client = new HybridConnectionClient(csb.ToString());
// Open a connection to the Relay endpoint and get a `HybridConnectionStream`
var hybridConnectionStream = await client.CreateConnectionAsync();
```

### <a name="receiving-data"></a>Gegevens ontvangen

De [HybridConnectionStream] [ HCStream] klasse wederzijdse communicatie mogelijk maakt. In de meeste gevallen ontvangt u continu van de stroom. Als u tekst uit de stroom leest, kunt u ook gebruik van een [StreamReader](https://msdn.microsoft.com/library/system.io.streamreader(v=vs.110).aspx) object, waarmee u gemakkelijker bij het parseren van de gegevens. Bijvoorbeeld, kunt u gegevens lezen als tekst, in plaats van als `byte[]`.

De volgende code weergegeven als afzonderlijke regels tekst van de stroom totdat een annulering wordt aangevraagd:

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

Zodra u een verbinding tot stand gebracht hebt, kunt u een bericht verzenden naar de Relay-eindpunt. Omdat het verbindingsobject neemt over [Stream](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx), verzenden van uw gegevens als een `byte[]`. Het volgende voorbeeld ziet u hoe u dit doet:

```csharp
var data = Encoding.UTF8.GetBytes("hello");
await clientConnection.WriteAsync(data, 0, data.Length);
```

Echter, als u verzenden van de tekst rechtstreeks wilt, zonder te coderen van de tekenreeks telkens wanneer u kunt laten lopen de `hybridConnectionStream` object met een [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) object.

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
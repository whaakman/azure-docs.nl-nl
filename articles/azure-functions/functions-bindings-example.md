---
title: Azure Functions-trigger en binding voorbeeld
description: Meer informatie over het configureren van Azure Functions-bindingen
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 02/18/2019
ms.author: cshoe
ms.openlocfilehash: 6d5f9b171a4efc5e52d281655de143ac9d40d437
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56739512"
---
# <a name="azure-functions-trigger-and-binding-example"></a>Azure Functions-trigger en binding voorbeeld

In dit artikel ziet u hoe u het configureren van een [trigger en bindingen](./functions-triggers-bindings.md) in een Azure-functie.

Stel dat u wilt een nieuwe rij naar Azure-tabelopslag geschreven wanneer een nieuw bericht wordt weergegeven in Azure Queue storage. In dit scenario kan worden geïmplementeerd met behulp van een Azure Queue storage-trigger en een Azure Table storage-Uitvoerbinding. 

Hier volgt een *function.json* -bestand voor dit scenario. 

```json
{
  "bindings": [
    {
      "type": "queueTrigger",
      "direction": "in",
      "name": "order",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "type": "table",
      "direction": "out",
      "name": "$return",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

Het eerste element in de `bindings` matrix is de trigger voor Queue storage. De `type` en `direction` eigenschappen van de trigger identificeren. De `name` eigenschap geeft u de parameter van de functie die de inhoud van het wachtrij-bericht ontvangt. De naam van de wachtrij voor het bewaken van wordt `queueName`, en de verbindingsreeks is in de app-instelling die is geïdentificeerd door `connection`.

Het tweede element in de `bindings` matrix is de Azure Table Storage-Uitvoerbinding. De `type` en `direction` eigenschappen geven de binding. De `name` eigenschap geeft aan hoe de functie biedt de nieuwe rij in de tabel, met behulp van de functie wordt in dit geval waarde retourneren. De naam van de tabel wordt `tableName`, en de verbindingsreeks is in de app-instelling die is geïdentificeerd door `connection`.

Bekijken en bewerken van de inhoud van *function.json* in Azure portal, klikt u op de **geavanceerde editor** kiezen op de **integreren** tabblad van uw functie.

> [!NOTE]
> De waarde van `connection` is de naam van een app-instelling met de verbindingsreeks, niet de connection string zelf. Bindings-verbinding gebruiken tekenreeksen die zijn opgeslagen in de app-instellingen af te dwingen de beste praktijk die *function.json* bevat geen geheimen van de service.

## <a name="c-script-example"></a>Voorbeeld van C#-script

Hier volgt een C#-script-code die geschikt is voor deze trigger en een binding. U ziet dat de naam van de parameter waarmee de inhoud van de wachtrij bericht is `order`; deze naam is vereist omdat de `name` eigenschapswaarde in *function.json* is `order` 

```cs
#r "Newtonsoft.Json"

using Microsoft.Extensions.Logging;
using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write to Table storage
// The method return value creates a new row in Table Storage
public static Person Run(JObject order, ILogger log)
{
    return new Person() { 
            PartitionKey = "Orders", 
            RowKey = Guid.NewGuid().ToString(),  
            Name = order["Name"].ToString(),
            MobileNumber = order["MobileNumber"].ToString() };  
}
 
public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

## <a name="javascript-example"></a>JavaScript-voorbeeld

Dezelfde *function.json* bestand kan worden gebruikt met een JavaScript-functie:

```javascript
// From an incoming queue message that is a JSON object, add fields and write to Table Storage
// The second parameter to context.done is used as the value for the new row
module.exports = function (context, order) {
    order.PartitionKey = "Orders";
    order.RowKey = generateRandomId(); 

    context.done(null, order);
};

function generateRandomId() {
    return Math.random().toString(36).substring(2, 15) +
        Math.random().toString(36).substring(2, 15);
}
```

## <a name="class-library-example"></a>Voorbeeld van de klasse-bibliotheek

In een klassebibliotheek, de dezelfde trigger en informatie over de binding &mdash; wachtrij- en tabelnamen, opslagaccounts, functie parameters voor invoer en uitvoer &mdash; wordt geleverd door de kenmerken in plaats van een bestand function.json. Hier volgt een voorbeeld:

```csharp
public static class QueueTriggerTableOutput
{
    [FunctionName("QueueTriggerTableOutput")]
    [return: Table("outTable", Connection = "MY_TABLE_STORAGE_ACCT_APP_SETTING")]
    public static Person Run(
        [QueueTrigger("myqueue-items", Connection = "MY_STORAGE_ACCT_APP_SETTING")]JObject order,
        ILogger log)
    {
        return new Person() {
                PartitionKey = "Orders",
                RowKey = Guid.NewGuid().ToString(),
                Name = order["Name"].ToString(),
                MobileNumber = order["MobileNumber"].ToString() };
    }
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

U hebt nu een werkende-functie die wordt geactiveerd door Azure Table-opslag waarmee gegevens uit naar een wachtrij voert.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Azure Functions expressiepatronen-binding](./functions-bindings-expressions-patterns.md)
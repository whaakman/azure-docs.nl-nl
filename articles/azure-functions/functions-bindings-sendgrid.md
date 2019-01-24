---
title: Azure Functions SendGrid-bindingen
description: Verwijzen naar een Azure Functions SendGrid-bindingen.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 11/29/2017
ms.author: cshoe
ms.openlocfilehash: 1ddb993a3e4d41647a4afcf5a5daed03a834db9f
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54825991"
---
# <a name="azure-functions-sendgrid-bindings"></a>Azure Functions SendGrid-bindingen

In dit artikel wordt uitgelegd hoe u e-mail verzenden met behulp van [SendGrid](https://sendgrid.com/docs/User_Guide/index.html) bindingen in Azure Functions. Azure Functions ondersteunt een Uitvoerbinding voor SendGrid.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pakketten - functies 1.x

De SendGrid-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs.Extensions.SendGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid) NuGet-pakket versie 2.x. Broncode voor het pakket is in de [azure webjobs-sdk extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.SendGrid/) GitHub-opslagplaats.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Pakketten - functies 2.x

De SendGrid-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs.Extensions.SendGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid) NuGet-pakket versie 3.x. Broncode voor het pakket is in de [azure webjobs-sdk extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/) GitHub-opslagplaats.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="example"></a>Voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#c-example)
* [C# script (.csx)](#c-script-example)
* [JavaScript](#javascript-example)

### <a name="c-example"></a>C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) dat maakt gebruik van een Service Bus-wachtrij activeren en een SendGrid-Uitvoerbinding.

#### <a name="synchronous-c-example"></a>Synchrone C# voorbeeld:

```cs
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] Message email,
    [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] out SendGridMessage message)
{
var emailObject = JsonConvert.DeserializeObject<OutgoingEmail>(Encoding.UTF8.GetString(email.Body));

var message = new SendGridMessage();
message.AddTo(emailObject.To);
message.AddContent("text/html", emailObject.Body);
message.SetFrom(new EmailAddress(emailObject.From));
message.SetSubject(emailObject.Subject);
}

public class OutgoingEmail
{
    public string To { get; set; }
    public string From { get; set; }
    public string Subject { get; set; }
    public string Body { get; set; }
}
```
#### <a name="asynchronous-c-example"></a>Asynchrone C# voorbeeld:

```cs
[FunctionName("SendEmail")]
public static async void Run(
 [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] Message email,
 [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] IAsyncCollector<SendGridMessage> messageCollector)
{
 var emailObject = JsonConvert.DeserializeObject<OutgoingEmail>(Encoding.UTF8.GetString(email.Body));

 var message = new SendGridMessage();
 message.AddTo(emailObject.To);
 message.AddContent("text/html", emailObject.Body);
 message.SetFrom(new EmailAddress(emailObject.From));
 message.SetSubject(emailObject.Subject);
 
 await messageCollector.AddAsync(message);
}

public class OutgoingEmail
{
 public string To { get; set; }
 public string From { get; set; }
 public string Subject { get; set; }
 public string Body { get; set; }
}
```

U kunt weglaten instellen van het kenmerk `ApiKey` eigenschap hebt u uw API-sleutel in een app-instelling met de naam 'AzureWebJobsSendGridApiKey'.

### <a name="c-script-example"></a>Voorbeeld van C#-script

Het volgende voorbeeld ziet u een SendGrid-Uitvoerbinding een *function.json* bestand en een [C#-scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding.

Hier volgt de binding-gegevens de *function.json* bestand:

```json 
{
    "bindings": [
        {
          "type": "queueTrigger",
          "name": "mymsg",
          "queueName": "myqueue",
          "connection": "AzureWebJobsStorage",
          "direction": "in"
        },
        {
          "type": "sendGrid",
          "name": "$return",
          "direction": "out",
          "apiKey": "SendGridAPIKeyAsAppSetting",
          "from": "{FromEmail}",
          "to": "{ToEmail}"
        }
    ]
}
```

De [configuratie](#configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de C#-scriptcode:

```csharp
#r "SendGrid"

using System;
using SendGrid.Helpers.Mail;
using Microsoft.Azure.WebJobs.Host;

public static SendGridMessage Run(Message mymsg, ILogger log)
{
    SendGridMessage message = new SendGridMessage()
    {
        Subject = $"{mymsg.Subject}"
    };
    
    message.AddContent("text/plain", $"{mymsg.Content}");

    return message;
}
public class Message
{
    public string ToEmail { get; set; }
    public string FromEmail { get; set; }
    public string Subject { get; set; }
    public string Content { get; set; }
}
```

### <a name="javascript-example"></a>JavaScript-voorbeeld

Het volgende voorbeeld ziet u een SendGrid-Uitvoerbinding een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding.

Hier volgt de binding-gegevens de *function.json* bestand:

```json 
{
    "bindings": [
        {
            "name": "$return",
            "type": "sendGrid",
            "direction": "out",
            "apiKey" : "MySendGridKey",
            "to": "{ToEmail}",
            "from": "{FromEmail}",
            "subject": "SendGrid output bindings"
        }
    ]
}
```

De [configuratie](#configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de JavaScript-code:

```javascript
module.exports = function (context, input) {    
    var message = {
         "personalizations": [ { "to": [ { "email": "sample@sample.com" } ] } ],
        from: { email: "sender@contoso.com" },        
        subject: "Azure news",
        content: [{
            type: 'text/plain',
            value: input
        }]
    };

    context.done(null, message);
};
```

## <a name="attributes"></a>Kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [SendGrid](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs) kenmerk.

Zie voor meer informatie over de kenmerkeigenschappen die u kunt configureren, [configuratie](#configuration). Hier volgt een `SendGrid` kenmerk voorbeeld in een handtekeningmethode:

```csharp
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] out SendGridMessage message)
{
    ...
}
```

Zie voor een compleet voorbeeld [C#-voorbeeld](#c-example).

## <a name="configuration"></a>Configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `SendGrid` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Description|
|---------|---------|----------------------|
|**type**|| Vereist: moet worden ingesteld op `sendGrid`.|
|**direction**|| Vereist: moet worden ingesteld op `out`.|
|**De naam**|| Vereist: de naam van de variabele die wordt gebruikt in de functiecode voor de aanvraag of de hoofdtekst van de aanvraag. Deze waarde is ```$return``` wanneer er slechts één van de geretourneerde waarde. |
|**apiKey**|**ApiKey**| De naam van een app-instelling met uw API-sleutel. Als niet is ingesteld, de standaardapp-instelling is de naam 'AzureWebJobsSendGridApiKey'.|
|**Aan**|**Aan**| e-mailadres van de ontvanger. |
|**from**|**From**| e-mailadres van de afzender. |
|**subject**|**Onderwerp**| het onderwerp van het e-mailbericht. |
|**text**|**Tekst**| de inhoud van de e-mail. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>instellingen voor host.JSON

In deze sectie beschrijft de globale configuratie-instellingen beschikbaar voor deze binding in versie 2.x. Het onderstaande voorbeeld host.json bestand bevat alleen de versie 2.x-instellingen voor deze binding. Voor meer informatie over globale configuratie-instellingen in versie 2.x, Zie [naslaginformatie over host.json voor Azure Functions versie 2.x](functions-host-json.md).

> [!NOTE]
> Voor een verwijzing van host.json in functies 1.x, Zie [naslaginformatie over host.json voor Azure Functions 1.x](functions-host-json-v1.md).

```json
{
    "version": "2.0",
    "extensions": {
        "sendGrid": {
            "from": "Azure Functions <samples@functions.com>"
        }
    }
}
```  

|Eigenschap  |Standaard | Description |
|---------|---------|---------| 
|uit|N.v.t.|E-mailadres van de afzender voor alle functies.| 


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions-triggers en bindingen](functions-triggers-bindings.md)

---
title: Azure Functions SendGrid-bindingen
description: Azure Functions SendGrid-bindingen naar verwijzen.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/29/2017
ms.author: tdykstra
ms.openlocfilehash: 0cd5730d049749949db13f29499e268a1ebccc18
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34830609"
---
# <a name="azure-functions-sendgrid-bindings"></a>Azure Functions SendGrid-bindingen

Dit artikel wordt uitgelegd hoe u e-mail verzendt met behulp van [SendGrid](https://sendgrid.com/docs/User_Guide/index.html) bindingen in de Azure Functions. Azure Functions ondersteunt een uitvoer-binding voor SendGrid.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pakketten - functies 1.x

De SendGrid-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs.Extensions.SendGrid](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid) NuGet-pakket versie 2.x. De broncode voor het pakket bevindt zich in de [azure webjobs-sdk extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.SendGrid/) GitHub-opslagplaats.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Pakketten - functies 2.x

De SendGrid-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs.Extensions.SendGrid](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid) NuGet-pakket versie 3.x. De broncode voor het pakket bevindt zich in de [azure webjobs-sdk extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/) GitHub-opslagplaats.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="example"></a>Voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#c-example)
* [C# script (.csx)](#c-script-example)
* [JavaScript](#javascript-example)

### <a name="c-example"></a>C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) dat maakt gebruik van een Service Bus-wachtrij activeren en een SendGrid uitvoer binding.

```cs
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] out SendGridMessage message)
{
    message = new SendGridMessage();
    message.AddTo(email.To);
    message.AddContent("text/html", email.Body);
    message.SetFrom(new EmailAddress(email.From));
    message.SetSubject(email.Subject);
}

public class OutgoingEmail
{
    public string To { get; set; }
    public string From { get; set; }
    public string Subject { get; set; }
    public string Body { get; set; }
}
```

Kunt u de instelling van het kenmerk weglaten `ApiKey` eigenschap als u uw API-sleutel in een app-instelling met de naam 'AzureWebJobsSendGridApiKey'.

### <a name="c-script-example"></a>Voorbeeld van C#-script

Het volgende voorbeeld ziet u de uitvoer van een SendGrid binding in een *function.json* bestand en een [C# scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json 
{
    "bindings": [
        {
            "name": "message",
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

De [configuratie](#configuration) sectie wordt uitgelegd deze eigenschappen.

Dit is de C#-scriptcode:

```csharp
#r "SendGrid"
using System;
using SendGrid.Helpers.Mail;

public static void Run(TraceWriter log, string input, out Mail message)
{
     message = new Mail
    {        
        Subject = "Azure news"          
    };

    var personalization = new Personalization();
    personalization.AddTo(new Email("recipient@contoso.com"));   

    Content content = new Content
    {
        Type = "text/plain",
        Value = input
    };
    message.AddContent(content);
    message.AddPersonalization(personalization);
}
```

### <a name="javascript-example"></a>JavaScript-voorbeeld

Het volgende voorbeeld ziet u de uitvoer van een SendGrid binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding.

Dit zijn de bindingsgegevens de *function.json* bestand:

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

De [configuratie](#configuration) sectie wordt uitgelegd deze eigenschappen.

Hier volgt de JavaScript-code:

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

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [SendGrid](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs) kenmerk.

Zie voor meer informatie over de kenmerkeigenschappen die u kunt configureren, [configuratie](#configuration). Hier volgt een `SendGrid` kenmerk voorbeeld in een handtekening voor methode:

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

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `SendGrid` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type**|| Vereist - moet worden ingesteld op `sendGrid`.|
|**direction**|| Vereist - moet worden ingesteld op `out`.|
|**Naam**|| Vereist: de naam van de variabele in functiecode gebruikt voor de aanvraag of de hoofdtekst van de aanvraag. Deze waarde is ```$return``` wanneer er slechts één waarde. |
|**apiKey**|**ApiKey**| De naam van een app-instelling die uw API-sleutel bevat. Als niet is ingesteld, de standaard-app instellen van is de naam 'AzureWebJobsSendGridApiKey'.|
|**Aan**|**Aan**| e-mailadres van de geadresseerde. |
|**Van**|**Van**| e-mailadres van de afzender. |
|**Onderwerp**|**Onderwerp**| het onderwerp van het e-mailbericht. |
|**Tekst**|**Tekst**| de inhoud van e-mail. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions triggers en bindingen](functions-triggers-bindings.md)

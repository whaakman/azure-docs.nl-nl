---
title: Azure Functions SendGrid-bindingen | Microsoft Docs
description: Naslaginformatie over Azure Functions SendGrid bindingen
services: functions
documentationcenter: na
author: rachelappel
manager: cfowler
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/26/2017
ms.author: rachelap
ms.openlocfilehash: 4cdafbe05e29d8b483c6b0e1daf41a36583d7b5e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-sendgrid-bindings"></a>Azure Functions SendGrid-bindingen

In dit artikel wordt uitgelegd hoe configureren en werken met de SendGrid-bindingen in de Azure Functions. Als de sendgrid, kunt u Azure Functions aangepaste om e-mail te verzenden via een programma.

Dit artikel is referentie-informatie voor ontwikkelaars voor Azure Functions. Als u geen ervaring met Azure Functions, start u de volgende bronnen:

[Uw eerste Azure-functie maken](functions-create-first-azure-function.md). 
[C#](functions-reference-csharp.md), [F #](functions-reference-fsharp.md), of [knooppunt](functions-reference-node.md) referenties voor ontwikkelaars.

## <a name="functionjson-for-sendgrid-bindings"></a>Function.JSON voor SendGrid-bindingen

Azure Functions bevat een uitvoer-binding voor SendGrid. De SendGrid-uitvoer binding schakelt u maken en verzenden van e-programmatisch. 

De SendGrid-binding ondersteunt de volgende eigenschappen:

|Eigenschap  |Beschrijving  |
|---------|---------|
|**naam**| Vereist: de naam van de variabele in functiecode gebruikt voor de aanvraag of de hoofdtekst van de aanvraag. Deze waarde is ```$return``` wanneer er slechts één waarde. |
|**type**| Vereist - moet worden ingesteld op `sendGrid`.|
|**richting**| Vereist - moet worden ingesteld op `out`.|
|**apiKey**| Vereist - moet worden ingesteld op de naam van uw API-sleutel die is opgeslagen in de instellingen van de functie App-app. |
|**Aan**| e-mailadres van de geadresseerde. |
|**Van**| e-mailadres van de afzender. |
|**Onderwerp**| het onderwerp van het e-mailbericht. |
|**tekst**| de inhoud van e-mail. |

Voorbeeld van **function.json**:

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

> [!NOTE]
> Azure Functions slaat uw verbindingsgegevens en API-sleutels als de app-instellingen zodat ze niet zijn ingeschakeld in uw resourcebeheerbibliotheek. Deze actie beschermt uw vertrouwelijke gegevens.
>
>

## <a name="c-example-of-the-sendgrid-output-binding"></a>C#-voorbeeld van de SendGrid uitvoer binding

```csharp
#r "SendGrid"
using System;
using SendGrid.Helpers.Mail;

public static Mail Run(TraceWriter log, string input, out Mail message)
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

## <a name="node-example-of-the-sendgrid-output-binding"></a>Voorbeeld van de knooppunten van het SendGrid uitvoer binding

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

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over andere bindingen en triggers voor Azure Functions 
- [Azure Functions triggers en bindingen referentie voor ontwikkelaars](functions-triggers-bindings.md)

- [Aanbevolen procedures voor Azure Functions](functions-best-practices.md) vindt u enkele aanbevolen procedures voor gebruik bij het maken van Azure Functions.

- [Naslaginformatie over Azure Functions developer](functions-reference.md) programmeurs verwijzing voor het coderen van functies en definiëren van triggers en bindingen.

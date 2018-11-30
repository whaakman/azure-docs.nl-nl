---
title: Het gebruik van de e-mailservice van SendGrid (Node.js) | Microsoft Docs
description: Informatie over hoe e-mailbericht met de e-mailservice van SendGrid verzenden op Azure. Codevoorbeelden geschreven met behulp van de Node.js-API.
services: ''
documentationcenter: nodejs
author: erikre
manager: wpickett
editor: ''
ms.assetid: cac444b4-26b0-45ea-9c3d-eca28d57dacb
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 01/05/2016
ms.author: erikre
ms.openlocfilehash: f2d653441598a47986913d525057672eed24b435
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52421280"
---
# <a name="how-to-send-email-using-sendgrid-from-nodejs"></a>Hoe u E-mail verzenden via SendGrid met Node.js

Deze handleiding laat zien hoe u veelvoorkomende programming taken met de e-mailservice van SendGrid uitvoeren op Azure. De voorbeelden zijn geschreven met behulp van de Node.js-API. De behandelde scenario's zijn **maken e**, **verzenden van e-mail**, **bijlagen toevoegen**, **met behulp van filters**, en **bijwerken van eigenschappen**. Zie voor meer informatie over SendGrid en het verzenden van e-mail, het [Vervolgstappen](#next-steps) sectie.

## <a name="what-is-the-sendgrid-email-service"></a>Wat is het e-mailservice van SendGrid?

SendGrid is een [cloud-gebaseerde e-mailservice] die zorgt voor betrouwbare [transactionele e-mail delivery], schaalbaarheid en realtime analyses en flexibele API's waarmee u aangepaste integratie eenvoudig. Veelvoorkomende SendGrid gebruiksscenario's zijn onder andere:

* Automatisch verzenden van meldingen voor klanten
* Beheer van distributie bevat voor het verzenden van klanten maandelijks e-flyers en speciale aanbiedingen
* Realtime metrische gegevens voor zaken zoals geblokkeerde e-mail- en reactietijd van klanten verzamelen
* Genereren van rapporten voor het identificeren van trends
* Doorsturen van vragen van klanten
* E-mailmeldingen van uw toepassing

Zie voor meer informatie, [ https://sendgrid.com ](https://sendgrid.com).

## <a name="create-a-sendgrid-account"></a>Een SendGrid-Account maken

[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-nodejs-module"></a>Verwijzen naar de SendGrid-Module voor Node.js

De SendGrid-module voor Node.js kan worden geïnstalleerd via de node package manager (npm) met behulp van de volgende opdracht uit:

```bash
npm install sendgrid
```

Na de installatie, kunt u de module vereisen in uw toepassing met behulp van de volgende code:

```javascript
var sendgrid = require('sendgrid')(sendgrid_username, sendgrid_password);
```

Hiermee exporteert u de SendGrid-module de **SendGrid** en **e** functies.
**SendGrid** is verantwoordelijk voor het verzenden van e-mail via Web-API, terwijl **e** ingekapseld een e-mailbericht.

## <a name="how-to-create-an-email"></a>Hoe: een e-mailbericht maken

Maken van een e-mailbericht met de SendGrid-module, moet eerst het maken van een e-mailbericht met behulp van de functie e-mailbericht, en vervolgens te verzenden met behulp van de SendGrid-functie. Hier volgt een voorbeeld van het maken van een nieuw bericht met de functie e-mailadres:

```javascript
var email = new sendgrid.Email({
    to: 'john@contoso.com',
    from: 'anna@contoso.com',
    subject: 'test mail',
    text: 'This is a sample email message.'
});
```

U kunt ook een HTML-bericht opgeven voor clients die ondersteuning bieden voor het door de html-eigenschap. Bijvoorbeeld:

```javascript
html: This is a sample <b>HTML<b> email message.
```

Instellen van de eigenschappen van de tekst- en HTML-biedt vensters terugval naar tekst voor clients die niet kunnen ondersteuning voor HTML-berichten bieden.

Zie voor meer informatie over alle eigenschappen die worden ondersteund door de functie e [sendgrid-nodejs][sendgrid-nodejs].

## <a name="how-to-send-an-email"></a>Hoe: een e-mailbericht verzenden

Na het maken van een e-mailbericht met de functie e-mailbericht, kunt u met behulp van de Web-API die is geleverd door SendGrid verzenden. 

### <a name="web-api"></a>Web-API

```javascript
sendgrid.send(email, function(err, json){
    if(err) { return console.error(err); }
    console.log(json);
});
```

> [!NOTE]
> Terwijl de bovenstaande voorbeelden weer te geven in een e-object en de callback-functie, kunt u de functie verzenden ook rechtstreeks aanroepen door e-maileigenschappen direct op te geven. Bijvoorbeeld:  
> 
> ```javascript
> sendgrid.send({
> to: 'john@contoso.com',
> from: 'anna@contoso.com',
> subject: 'test mail',
> text: 'This is a sample email message.'
> });
> ```
>

## <a name="how-to-add-an-attachment"></a>Hoe: een bijlage toevoegen
Bijlagen kunnen worden toegevoegd aan een bericht door op te geven de bestandsnaam of-namen en paden in de **bestanden** eigenschap. Het volgende voorbeeld ziet u een bijlage verzenden:

```javascript
sendgrid.send({
    to: 'john@contoso.com',
    from: 'anna@contoso.com',
    subject: 'test mail',
    text: 'This is a sample email message.',
    files: [
        {
            filename:     '',           // required only if file.content is used.
            contentType:  '',           // optional
            cid:          '',           // optional, used to specify cid for inline content
            path:         '',           //
            url:          '',           // == One of these three options is required
            content:      ('' | Buffer) //
        }
    ],
});
```

> [!NOTE]
> Wanneer u de **bestanden** eigenschap, het bestand moet toegankelijk zijn via [fs.readFile](https://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile). Als het bestand dat u wilt koppelen wordt gehost in Azure Storage, zoals in een Blob-container, moet u eerst het bestand kopiëren naar lokale opslag of naar een Azure-station voordat deze kan worden verzonden als een bijlage met de **bestanden** eigenschap.
> 
> 

## <a name="how-to-use-filters-to-enable-footers-and-tracking"></a>Hoe: Filters gebruiken om te inschakelen voetteksten en bijhouden

SendGrid zorgt voor extra e-mailfunctionaliteit door het gebruik van filters. Dit zijn de instellingen die kunnen worden toegevoegd aan een e-mailbericht om in te schakelen specifieke functionaliteit, zoals het inschakelen van klikken traceren in, Google analytics en abonnement bijhouden. Zie voor een volledige lijst met filters, [filterinstellingen][Filter Settings].

Filters kunnen worden toegepast op een bericht weergegeven met behulp van de **filters** eigenschap.
Elk filter is opgegeven door een hash met filter-specifieke instellingen.
De volgende voorbeelden laten zien hoe de voettekst en klikt u op filters bijhouden:

### <a name="footer"></a>Voettekst

```javascript
var email = new sendgrid.Email({
    to: 'john@contoso.com',
    from: 'anna@contoso.com',
    subject: 'test mail',
    text: 'This is a sample email message.'
});

email.setFilters({
    'footer': {
        'settings': {
            'enable': 1,
            'text/plain': 'This is a text footer.'
        }
    }
});

sendgrid.send(email);
```

### <a name="click-tracking"></a>Bijhouden van kliks

```javascript
var email = new sendgrid.Email({
    to: 'john@contoso.com',
    from: 'anna@contoso.com',
    subject: 'test mail',
    text: 'This is a sample email message.'
});

email.setFilters({
    'clicktrack': {
        'settings': {
            'enable': 1
        }
    }
});

sendgrid.send(email);
```

## <a name="how-to-update-email-properties"></a>Hoe: de eigenschappen van de e-mailadres bijwerken

Sommige eigenschappen van het e-mailadres kunnen worden overschreven met behulp van **setProperty** of toegevoegd met behulp van **addProperty**. Bijvoorbeeld, kunt u extra ontvangers toevoegen met behulp van

```javascript
email.addTo('jeff@contoso.com');
```

of stel het filter met behulp van

```javascript
email.addFilter('footer', 'enable', 1);
email.addFilter('footer', 'text/html', '<strong>boo</strong>');
```

Zie voor meer informatie, [sendgrid-nodejs][sendgrid-nodejs].

## <a name="how-to-use-additional-sendgrid-services"></a>Hoe: extra SendGrid Services gebruiken

SendGrid biedt web-API's die u kunt gebruikmaken van aanvullende SendGrid-functionaliteit van uw Azure-toepassing. Zie voor meer informatie, de [SendGrid-API-documentatie][SendGrid API documentation].

## <a name="next-steps"></a>Volgende stappen

Nu dat u de basisprincipes van de SendGrid-e-mailservice hebt geleerd, volgt u deze koppelingen voor meer informatie.

* Opslagplaats voor SendGrid Node.js-module: [sendgrid-nodejs][sendgrid-nodejs]
* SendGrid-API-documentatie: <https://sendgrid.com/docs>
* SendGrid speciaal aanbod voor Azure-klanten: [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)

[special offer]: https://sendgrid.com/windowsazure.html
[sendgrid-nodejs]: https://github.com/sendgrid/sendgrid-nodejs
[Filter Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
[SendGrid API documentation]: https://sendgrid.com/docs
[cloud-gebaseerde e-mailservice]: https://sendgrid.com/email-solutions
[transactionele e-mail delivery]: https://sendgrid.com/transactional-email

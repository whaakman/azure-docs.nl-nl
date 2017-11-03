---
title: Het gebruik van de SendGrid-e-mailservice (Node.js) | Microsoft Docs
description: Meer informatie over hoe verzenden van e-mailbericht met de SendGrid-e-mailservice op Azure. Codevoorbeelden geschreven met behulp van de Node.js-API.
services: 
documentationcenter: nodejs
author: erikre
manager: wpickett
editor: 
ms.assetid: cac444b4-26b0-45ea-9c3d-eca28d57dacb
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 01/05/2016
ms.author: erikre
ms.openlocfilehash: 327cea3a24cc47a9cc463b37cc2346ebc475ef7f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-send-email-using-sendgrid-from-nodejs"></a>Het verzenden van E-mail via SendGrid met Node.js
Deze handleiding wordt uitgelegd hoe u algemene programmeertaken met de SendGrid-e-mailservice uitvoert op Azure. De voorbeelden zijn geschreven met behulp van de Node.js-API. De scenario's worden behandeld: **construeren e**, **verzenden van e-mail**, **bijlagen toevoegen**, **met filters**, en **eigenschappen bijwerken**. Zie voor meer informatie over SendGrid en verzenden van e-mail, de [Vervolgstappen](#next-steps) sectie.

## <a name="what-is-the-sendgrid-email-service"></a>Wat is de SendGrid-e-mailservice?
SendGrid is een [cloud-gebaseerde e-mailservice] die biedt betrouwbare [levering van de transactionele e], schaalbaarheid en realtime-analyses samen met de flexibele API's die aangepaste integratie te vereenvoudigen. Veelvoorkomende gebruiksscenario's van SendGrid zijn onder andere:

* Automatisch verzenden van meldingen op klanten
* Beheer van distributiepunt bevat voor het verzenden van klanten maandelijkse e-Folders en speciale aanbiedingen
* Realtime metrische gegevens voor items zoals geblokkeerd e- en reactietijd van de klant verzamelen
* Genereren van rapporten om trends te identificeren
* Doorsturen van vragen van klanten
* E-mailmeldingen van uw toepassing

Zie voor meer informatie [https://sendgrid.com](https://sendgrid.com).

## <a name="create-a-sendgrid-account"></a>Een SendGrid-Account maken
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-nodejs-module"></a>Verwijst naar de SendGrid-Module voor Node.js
De SendGrid-module voor Node.js kan worden geïnstalleerd via de knooppunt package manager (npm) met behulp van de volgende opdracht:

    npm install sendgrid

Na de installatie, kunt u de module met behulp van de volgende code in uw toepassing vereisen:

    var sendgrid = require('sendgrid')(sendgrid_username, sendgrid_password);

De module SendGrid exporteert de **SendGrid** en **e** functies.
**SendGrid** is verantwoordelijk voor het verzenden van e-mail via Web-API, terwijl **e** ingekapseld een e-mailbericht.

## <a name="how-to-create-an-email"></a>Procedure: een e-mailbericht maken
Maken van een e-mailbericht met de SendGrid-module, moet eerst een e-mailbericht met de e-functie en vervolgens te verzenden met de SendGrid-functie te maken. Hier volgt een voorbeeld van het maken van een nieuw bericht met de functie e-mailadres:

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

U kunt ook een HTML-bericht opgeven voor clients die door de eigenschap html ondersteunen. Bijvoorbeeld:

    html: This is a sample <b>HTML<b> email message.

Instellen van eigenschappen van de tekst en de HTML-biedt correcte terugvallen op tekstinhoud voor clients die HTML-berichten niet ondersteunt.

Zie voor meer informatie over alle eigenschappen die worden ondersteund door de functie e [sendgrid nodejs][sendgrid-nodejs].

## <a name="how-to-send-an-email"></a>Procedure: een e-mailbericht verzenden
Na het maken van een e-mailbericht met de e-functie kunt u met behulp van de Web-API die is geleverd door SendGrid verzenden. 

### <a name="web-api"></a>Web-API
    sendgrid.send(email, function(err, json){
        if(err) { return console.error(err); }
        console.log(json);
    });

> [!NOTE]
> U kunt ook rechtstreeks de send-functie aanroepen door direct e-eigenschappen op te geven, terwijl in de bovenstaande doorgeven in een e-object en de callback-functie voorbeelden. Bijvoorbeeld:  
> 
> `````
> sendgrid.send({
> to: 'john@contoso.com',
> from: 'anna@contoso.com',
> subject: 'test mail',
> text: 'This is a sample email message.'
> });
> `````
> 
> 

## <a name="how-to-add-an-attachment"></a>Procedure: een bijlage toevoegen
Bijlagen kunnen worden toegevoegd aan een bericht door te geven van de bestandsnaam of-namen en het pad in de **bestanden** eigenschap. Het volgende voorbeeld toont een bijlage verzenden:

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

> [!NOTE]
> Wanneer u de **bestanden** eigenschap, het bestand moet toegankelijk zijn via [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile). Als het bestand dat u wilt koppelen wordt gehost in Azure Storage, zoals in een Blob-container moet u eerst het bestand kopiëren naar de lokale opslag of naar een Azure-station voordat deze kan worden verzonden als een bijlage met behulp de **bestanden** eigenschap.
> 
> 

## <a name="how-to-use-filters-to-enable-footers-and-tracking"></a>How to: Enable voetteksten en bijhouden-Filters gebruiken
SendGrid biedt extra e-mailfunctionaliteit door het gebruik van filters. Dit zijn de instellingen die kunnen worden toegevoegd aan een e-mailbericht om in te schakelen van specifieke functionaliteit, zoals het inschakelen van Klik bijhouden, Google analytics en abonnement bijhouden. Zie voor een volledige lijst met filters [filterinstellingen][Filter Settings].

Filters kunnen worden toegepast op een bericht met behulp van de **filters** eigenschap.
Elk filter wordt opgegeven met een hash met filter-specifieke instellingen.
De volgende voorbeelden tonen de voettekst en klik op filters bijhouden:

### <a name="footer"></a>Voettekst
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

### <a name="click-tracking"></a>Klik op bijhouden
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

## <a name="how-to-update-email-properties"></a>How to: de eigenschappen van het e-mailadres bijwerken
Sommige eigenschappen van de e-mail kunnen worden overschreven met  **ingesteld*eigenschap*** of toegevoegd met behulp van  **toevoegen*eigenschap***. U kunt extra ontvangers bijvoorbeeld toevoegen met behulp van

    email.addTo('jeff@contoso.com');

of een filter instellen met behulp van

    email.addFilter('footer', 'enable', 1);
    email.addFilter('footer', 'text/html', '<strong>boo</strong>');

Zie voor meer informatie [sendgrid nodejs][sendgrid-nodejs].

## <a name="how-to-use-additional-sendgrid-services"></a>How to: extra SendGrid-Services gebruiken
SendGrid biedt web-API's waarmee u kunt gebruikmaken van aanvullende SendGrid-functionaliteit van uw Azure-toepassing. Zie voor meer informatie de [SendGrid-API-documentatie][SendGrid API documentation].

## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes van de SendGrid-e-mailservice hebt geleerd, volgt u deze koppelingen voor meer informatie.

* Module-opslagplaats SendGrid Node.js: [sendgrid nodejs][sendgrid-nodejs]
* SendGrid-API-documentatie: <https://sendgrid.com/docs>
* SendGrid speciale aanbieding voor Azure-klanten: [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)

[special offer]: https://sendgrid.com/windowsazure.html
[sendgrid-nodejs]: https://github.com/sendgrid/sendgrid-nodejs
[Filter Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
[SendGrid API documentation]: https://sendgrid.com/docs
[cloud-gebaseerde e-mailservice]: https://sendgrid.com/email-solutions
[levering van de transactionele e]: https://sendgrid.com/transactional-email

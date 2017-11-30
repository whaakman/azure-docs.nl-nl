---
title: Het gebruik van SendGrid in Azure Functions | Microsoft Docs
description: Laat zien hoe u SendGrid in Azure Functions
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/31/2017
ms.author: glenga
ms.openlocfilehash: 444e06ff24ea7f909a24d482aba26f890040d980
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="how-to-use-sendgrid-in-azure-functions"></a>Het gebruik van SendGrid in Azure Functions

## <a name="sendgrid-overview"></a>SendGrid-overzicht

Azure Functions ondersteunt SendGrid uitvoer bindingen zodat uw functies voor het verzenden van e-mailberichten met een paar regels code en een SendGrid-account.

Als u de SendGrid-API in een Azure-functie, hebt u een [SendGrid account](http://SendGrid.com). Bovendien moet u een SendGrid-API-sleutel hebben. Aanmelden bij uw SendGrid-account en klikt u op **instellingen** vervolgens **API-sleutel** voor het genereren van een API-sleutel. Houd deze sleutel is beschikbaar wanneer u deze in een toekomstige stap gebruiken.

U bent nu klaar om te maken van een Azure-functie-app.

## <a name="create-an-azure-function-app"></a>Een Azure-functie-app maken 

Apps van Azure functie zijn containers voor een of meer Azure functions. Azure functions meer zijn dan - functie. Elke Azure-functie is gekoppeld aan één trigger die een gebeurtenis die ervoor zorgt dat de functie om uit te voeren.
Elke functie kan bevatten een willekeurig aantal invoer of uitvoer bindingen. Bindingen zijn services die u in een functie gebruiken kunt. SendGrid is een uitvoer-binding die u gebruiken kunt om e-mail te verzenden. 

1. Aanmelden bij de Azure-portal en [maken van een Azure-functie-App](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) of open een bestaande functie-app. 
2. Maak een Azure-functie. Kies een handmatige trigger en C# het gemak houden. 

 ![Een Azure-functie maken](./media/functions-how-to-use-sendgrid/functions-new-function-manual-trigger-page.png)

## <a name="configure-sendgrid-for-use-in-an-azure-function-app"></a>SendGrid configureren voor gebruik in een Azure-functie-app

U moet uw SendGrid-API-sleutel opslaan als een app-instelling te gebruiken in een functie. Het veld ApiKey is niet uw werkelijke SendGrid-API-sleutel, maar een instelling die u app definiëren waarmee uw werkelijke API-sleutel. Uw sleutel opslaan op deze manier is voor beveiliging, omdat het gescheiden van alle code of bestanden die kunnen worden gecontroleerd in broncodebeheer.

- Maak een **AppSettings** sleutel in de functie-app **toepassingsinstellingen**.

 ![Een Azure-functie maken](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-api-key-app-settings.png)

## <a name="configure-sendgrid-output-bindings"></a>SendGrid uitvoer bindingen configureren

SendGrid is beschikbaar als een Azure functie uitvoer binding. Als u wilt maken van een SendGrid uitvoer binding:

1. Ga naar de **integreren** tabblad van de functie in de Azure portal.
2. Klik op **nieuwe uitvoer** voor het maken van een SendGrid uitvoer binding.
3. Vul de **API-sleutel** en **bericht parameternaam** eigenschappen. Als u wilt, kunt u nu de overige eigenschappen opgeven of ze in plaats daarvan code. Deze instellingen kunnen worden gebruikt als standaardwaarden.

 ![SendGrid uitvoer bindingen configureren](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-output-bindings.png)

Toevoegen van een binding aan een functie maakt een bestand met de naam **function.json** in de map van de functie. Dit bestand bevat dezelfde informatie die u in de Azure functie ziet **integreren** tabblad, maar in Json-indeling. Instellen van de **ApiKey**, **bericht**, en **van** velden maken de volgende vermeldingen in de **function.json** bestand: 

```json
{
  "bindings": [    
    {
      "type": "sendGrid",
      "name": "message",
      "apiKey": "SendGridKey",
      "direction": "out",
      "from": "azure@contoso.com"
    }
  ],
  "disabled": false
}
```

Als u liever, kunt u deze mogelijk wijzigen zelf rechtstreeks bestand.

Nu dat u hebt gemaakt en de functie-App en de functie geconfigureerd, kunt u de code voor het verzenden van een e-mailbericht schrijven.

## <a name="write-code-that-creates-and-sends-email"></a>Schrijven van code die wordt gemaakt en verzendt e-mailbericht

De SendGrid-API bevat alle opdrachten die u wilt maken en verzenden van een e-mailbericht.  

- Vervang de code in de functie met de volgende code:

```cs
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
    // change to email of recipient
    personalization.AddTo(new Email("MoreEmailPlease@contoso.com"));   

    Content content = new Content
    {
        Type = "text/plain",
        Value = input
    };
    message.AddContent(content);
    message.AddPersonalization(personalization);
}
```

Let op de eerste regel bevat de ```#r``` richtlijn die verwijst naar de SendGrid-assembly. Daarna kunt u een ```using``` instructie om eenvoudig toegang krijgen tot de objecten in die naamruimte. Maak in de code exemplaren van ```Mail```, ```Personalization```, en ```Content``` objecten uit de SendGrid-API die samen het e-mailbericht vormen. Wanneer u het bericht terugkeert, wordt het door SendGrid biedt. 

De handtekening van de functie bevat ook een extra uitvoerparameter van het type ```Mail``` met de naam ```message```. Beide invoer en bindingen express zelf uitvoer als parameters van de functie in de code. 

2. Testen van uw code door te klikken op **Test** en in te voeren van een bericht in de **aanvraagtekst** veld vervolgens te klikken op de **uitvoeren** knop.

 ![Testen van uw code](./media/functions-how-to-use-sendgrid/functions-develop-test-sendgrid.png)

3. Controleer de e-mail om te controleren dat SendGrid het e-mailadres verzonden. Het bestand moet gaat u naar het adres in de code uit stap 1 en bevatten het bericht van de **aanvraagtekst**.

## <a name="next-steps"></a>Volgende stappen
In dit artikel is gedemonstreerd hoe u met de SendGrid-service maken en verzenden van e-mail. Raadpleeg de volgende onderwerpen voor meer informatie over het gebruik van Azure Functions in uw apps: 

- [Aanbevolen procedures voor Azure Functions](functions-best-practices.md) vindt u enkele aanbevolen procedures voor gebruik bij het maken van Azure Functions.

- [Naslaginformatie over Azure Functions developer](functions-reference.md) programmeurs verwijzing voor het coderen van functies en definiëren van triggers en bindingen.

- [Azure Functions testen](functions-test-a-function.md) beschrijving van verschillende hulpprogramma's en technieken voor het testen van uw functies.
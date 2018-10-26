---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 467e09f9bd46df6d888d82f2961c5aed9cca4ab5
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50134153"
---
In dit voorbeeld maakt gebruik van de [Twilio](https://www.twilio.com/) service SMS-berichten te verzenden naar een mobiele telefoon. Azure Functions biedt al ondersteuning voor Twilio via de [Twilio binding](https://docs.microsoft.com/azure/azure-functions/functions-bindings-twilio), en het voorbeeld maakt gebruik van deze functie.

Het eerste wat dat u nodig hebt is een Twilio-account. U kunt een gratis bij maken https://www.twilio.com/try-twilio. Wanneer u een account hebt, toevoegen de volgende drie **app-instellingen** aan uw functie-app.

| Naam van de App-instelling | Waardebeschrijving |
| - | - |
| **TwilioAccountSid**  | De beveiligings-id voor uw Twilio-account |
| **TwilioAuthToken**   | Het verificatietoken voor uw Twilio-account |
| **TwilioPhoneNumber** | Het telefoonnummer dat is gekoppeld aan uw Twilio-account. Dit wordt gebruikt voor het verzenden van SMS-berichten. |
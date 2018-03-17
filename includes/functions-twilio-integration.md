Dit voorbeeld ook met behulp van de [Twilio](https://www.twilio.com/) SMS-berichten verzenden naar een mobiele telefoon-service. Azure Functions heeft al ondersteuning voor Twilio via de [Twilio-binding](https://docs.microsoft.com/azure/azure-functions/functions-bindings-twilio), en het voorbeeld maakt gebruik van deze functie.

U moet eerst is een Twilio-account. U kunt een gratis op maken https://www.twilio.com/try-twilio. Zodra u een account hebt, voeg de volgende drie **appinstellingen** aan uw app functie.

| De naam van de App-instelling | Waardebeschrijving |
| - | - |
| **TwilioAccountSid**  | De SID voor uw Twilio-account |
| **TwilioAuthToken**   | De Auth-token voor uw Twilio-account |
| **TwilioPhoneNumber** | Het telefoonnummer dat is gekoppeld aan uw Twilio-account. Dit wordt gebruikt om SMS-berichten te verzenden. |
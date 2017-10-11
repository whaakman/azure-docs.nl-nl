---
title: Het overdragen van de gebruiker de registratie- en productinformatie abonnement
description: Informatie over het overdragen van de registratie en product gebruikerabonnement aan een derde partij in Azure API Management.
services: api-management
documentationcenter: 
author: antonba
manager: erikre
editor: 
ms.assetid: 8b7ad5ee-a873-4966-a400-7e508bbbe158
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 2637ab6405f2d4ea1da84981295a144874dfa4f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-delegate-user-registration-and-product-subscription"></a>Het overdragen van de gebruiker de registratie- en productinformatie abonnement
Overdracht kunt u uw bestaande website te gebruiken voor het verwerken van ontwikkelaars sign-in/sign-up-to-date en abonnement op producten in plaats van met behulp van de ingebouwde functie in de portal voor ontwikkelaars. Hierdoor kan uw website aan de eigenaar van de gebruikersgegevens en de validatie van de volgende stappen uitvoeren in een aangepaste manier.

## <a name="delegate-signin-up"></a>Overdragen developer aanmelden en registreren
Om te delegeren developer aanmelden en aanmelden met uw bestaande website, moet u een speciale delegering-eindpunt op de site die als het toegangspunt voor het verzoek gestart vanuit de ontwikkelaarsportal API Management fungeert maken.

De laatste werkstroom zijn als volgt:

1. Ontwikkelaars klikken op de koppeling aanmelden of registreren bij API Management-portal voor ontwikkelaars
2. Browser wordt omgeleid naar het eindpunt voor overdracht
3. Eindpunt van de overdracht wordt omgeleid naar in of geeft UI gebruiker wordt gevraagd te aanmelden of registreren
4. Indien geslaagd kunt wordt de gebruiker omgeleid naar de API Management developer portal-pagina die ze vanaf gestart

Om te beginnen, aanvragen laten we eerst set-up API Management te routeren via uw eindpunt overdracht. Klik in de publicatieportal van API Management op **beveiliging** en klik vervolgens op de **delegering** tabblad. Klik op het selectievakje in zodat de gemachtigde-in- en registratie.

![Overdracht pagina][api-management-delegation-signin-up]

* Bepalen wat de URL van uw eindpunt speciale delegering wordt en voer deze in de **delegering eindpunt-URL** veld. 
* Binnen de **delegering verificatiesleutel** veld Voer een geheim dat wordt gebruikt voor het berekenen van een handtekening die u voor verificatie om ervoor te zorgen dat de aanvraag inderdaad afkomstig is van Azure API Management. U kunt klikken op de **genereren** knop API een willekeurig genereren van een sleutel voor u hebben.

Nu moet u maken de **delegering eindpunt**. Er is een aantal acties uitvoeren:

1. Een aanvraag ontvangen in de volgende notatie:
   
   > *http://www.yourwebsite.com/apimdelegation?Operation=Signin&returnUrl= {URL van bronpagina} & salt = {tekenreeks} & sig = {tekenreeks}*
   > 
   > 
   
    De queryparameters voor de aanvraag-in- / registratie:
   
   * **bewerking**: welk type overdracht aanvraag is - deze kan alleen worden identificeert **SignIn** in dit geval
   * **returnUrl**: de URL van de pagina waar de gebruiker hebt geklikt op een koppeling aanmelden of registreren
   * **Salt**: een speciale salt tekenreeks voor een hash beveiliging computing
   * **SIG**: een hash van de berekende beveiliging moet worden gebruikt voor vergelijking van uw eigen berekende hash
2. Controleer of dat de aanvraag afkomstig is van Azure API Management (optioneel, maar sterk aanbevolen voor beveiliging)
   
   * COMPUTE een HMAC SHA512-hash van een tekenreeks op basis van de **returnUrl** en **salt** queryparameters ([voorbeeldcode hieronder]):
     
     > HMAC (**salt** + '\n' + **returnUrl**)
     > 
     > 
   * Vergelijk de bovenstaande berekende hash op de waarde van de **sig** queryparameter. Als de twee hashes overeenkomen, gaat u verder met de volgende stap, anders dat de aanvraag wordt geweigerd.
3. Controleer of u een aanvraag voor sign-in/aanmelding-up worden ontvangen: de **bewerking** queryparameter wordt ingesteld op '**SignIn**'.
4. De gebruiker opleveren UI aanmelden of registreren
5. Als de gebruiker de aanmelding is die u moet een bijbehorende account voor hen maken in API Management. [Maken van een gebruiker] met de REST-API van API Management. Wanneer doet, zorg ervoor dat u de gebruikers-ID ingesteld op hetzelfde is in uw archief van de gebruiker of een id die u kunt van bijhouden.
6. Wanneer de gebruiker is geverifieerd:
   
   * [eenmalige aanmelding (SSO)-token van een aanvraag] via de API Management REST API
   * een queryparameter returnUrl toevoegen aan de SSO-URL die u hebt ontvangen van de API-aanroep hierboven:
     
     > bijvoorbeeld https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url 
     > 
     > 
   * de gebruiker omgeleid naar de URL van de bovenstaande geproduceerd

Naast de **SignIn** bewerking, kunt u ook uitvoeren accountbeheer door de vorige stappen te volgen en het gebruik van een van de volgende bewerkingen.

* **ChangePassword**
* **ChangeProfile**
* **CloseAccount**

U moet de volgende queryparameters voor accountbeheerbewerkingen doorgeven.

* **bewerking**: identificeert welk type overdracht aanvraag (ChangePassword, ChangeProfile of CloseAccount)
* **userId**: de gebruikers-id van het account voor het beheren van
* **Salt**: een speciale salt tekenreeks voor een hash beveiliging computing
* **SIG**: een hash van de berekende beveiliging moet worden gebruikt voor vergelijking van uw eigen berekende hash

## <a name="delegate-product-subscription"></a>Product abonnement overdragen
Product abonnement delegeren werkt op dezelfde manier voor het delegeren van de gebruiker aanmelden /-up. De laatste werkstroom zou als volgt zijn:

1. Ontwikkelaar een product selecteert in de API Management-portal voor ontwikkelaars en klikt op de knop aanmelden
2. Browser wordt omgeleid naar het eindpunt voor overdracht
3. Overdracht eindpunt vereist product abonnement stappen worden uitgevoerd - deze bepaalt zelf en kan leiden tot omleiden naar een andere pagina om aan te vragen factureringsgegevens extra vragen, of het opslaan van de informatie en niet een gebruikersactie vereist

Voor het inschakelen van de functionaliteit op de **delegering** pagina op **delegeren product abonnement**.

Verzeker u ervan dat het eindpunt van de overdracht worden de volgende acties uitgevoerd:

1. Een aanvraag ontvangen in de volgende notatie:
   
   > *{bewerking} http://www.yourwebsite.com/apimdelegation?Operation= & productId = {product abonneren op} & userId = {user aanvraag} & salt = {tekenreeks} & sig = {tekenreeks}*
   > 
   > 
   
    De queryparameters voor het product abonnement geval:
   
   * **bewerking**: identificeert welk type overdracht aanvraag is. Voor het product abonnement zijn aanvragen de geldige opties:
     * 'Abonneren': een verzoek voor een abonnement van de gebruiker voor een bepaald product met opgegeven ID (Zie hieronder)
     * 'Afmelden': een verzoek om te stoppen van een gebruiker vanuit een product
     * 'Vernieuwen': een verzoek om een abonnement (bijvoorbeeld die kan verlopen) te vernieuwen
   * **productId**: de ID van het product dat de gebruiker gevraagd om u te abonneren op
   * **userId**: de ID van de gebruiker voor wie de aanvraag wordt gedaan
   * **Salt**: een speciale salt tekenreeks voor een hash beveiliging computing
   * **SIG**: een hash van de berekende beveiliging moet worden gebruikt voor vergelijking van uw eigen berekende hash
2. Controleer of dat de aanvraag afkomstig is van Azure API Management (optioneel, maar sterk aanbevolen voor beveiliging)
   
   * COMPUTE een HMAC-SHA512 van een tekenreeks op basis van de **productId**, **userId** en **salt** queryparameters:
     
     > HMAC (**salt** + '\n' + **productId** + '\n' + **userId**)
     > 
     > 
   * Vergelijk de bovenstaande berekende hash op de waarde van de **sig** queryparameter. Als de twee hashes overeenkomen, gaat u verder met de volgende stap, anders dat de aanvraag wordt geweigerd.
3. De verwerking van een abonnement die op basis van het type in de aangevraagde bewerking uitvoeren **bewerking** -bijvoorbeeld facturering, meer vragen, enzovoort.
4. Abonneren op het abonnement is de gebruiker op het product aan uw kant, door de gebruiker het API Management-product door [aanroepen van de REST-API voor het product abonnement].

## <a name="delegate-example-code"></a> Voorbeeldcode
Deze codevoorbeelden laten zien hoe nemen de *delegering validatiesleutel*, die in het scherm van de overdracht van de publicatieportal is ingesteld voor het maken van een HMAC die vervolgens wordt gebruikt voor het valideren van de handtekening, de geldigheid van de returnUrl die is doorgegeven aan te tonen. De dezelfde code werkt voor de product-id en gebruikers-id met kleine wijziging.

**C#-code voor het genereren van de hash van returnUrl**

```c#
using System.Security.Cryptography;

string key = "delegation validation key";
string returnUrl = "returnUrl query parameter";
string salt = "salt query parameter";
string signature;
using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
{
    signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
    // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature to sig query parameter
}
```

**Code genereren van de hash van returnUrl NodeJS**

```
var crypto = require('crypto');

var key = 'delegation validation key'; 
var returnUrl = 'returnUrl query parameter';
var salt = 'salt query parameter';

var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
var digest = hmac.update(salt + '\n' + returnUrl).digest();
// change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
// compare signature to sig query parameter

var signature = digest.toString('base64');
```

## <a name="next-steps"></a>Volgende stappen
Zie de volgende video voor meer informatie over delegering.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Delegating-User-Authentication-and-Product-Subscription-to-a-3rd-Party-Site/player]
> 
> 

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[eenmalige aanmelding (SSO)-token van een aanvraag]: http://go.microsoft.com/fwlink/?LinkId=507409
[een gebruiker maken]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser
[aanroepen van de REST-API voor het product abonnement]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO
[Next steps]: #next-steps
[voorbeeldcode hieronder]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 

---
title: Hoe u kunt gebruiker-gebruikersregistratie en productabonnement delegeren
description: Informatie over het overdragen van gebruiker-gebruikersregistratie en productabonnement aan een derde partij in Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.assetid: 8b7ad5ee-a873-4966-a400-7e508bbbe158
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: c15dc83929aeaf6811f4d19bfca462abfacf4014
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57892452"
---
# <a name="how-to-delegate-user-registration-and-product-subscription"></a>Hoe u kunt gebruiker-gebruikersregistratie en productabonnement delegeren
Overdracht kunt u uw bestaande website gebruiken voor het verwerken van ontwikkelaars sign-in/aanmelden-up-to-date en abonnement op producten in plaats van met behulp van de ingebouwde functie in de portal voor ontwikkelaars. Hierdoor kan uw website eigenaar zijn van de gebruikersgegevens en de validatie van de volgende stappen uit in een aangepaste manier uitvoeren.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="delegate-signin-up"> </a>Developer aanmelding/registratie delegeren
Als u wilt delegeren developer aanmelden en meld u aan met uw bestaande website, moet u een speciaal delegatie-eindpunt maken op uw site die als het toegangspunt voor het verzoek afkomstig zijn van de API Management-portal voor ontwikkelaars fungeert.

De laatste werkstroom zijn als volgt:

1. Developer-klikken op de koppeling aanmelding of meld u aan bij de API Management-portal voor ontwikkelaars
2. Browser wordt omgeleid naar de delegatie-eindpunt
3. Delegatie-eindpunt in ruil wordt verwezen, of geeft gebruikersinterface waarin gebruiker wordt gevraagd om te ondertekenen in of meld u aan
4. Indien geslaagd kunt wordt de gebruiker omgeleid naar de API Management developer portal-pagina die ze vanaf gestart

Om te beginnen, aanvragen gaan we eerste configuratie API Management voor het routeren van via de delegatie-eindpunt. Klik in de publicatieportal van API Management, op **Security** en klik vervolgens op de **delegering** tabblad. Klik op het selectievakje in om in te schakelen 'Aanmelding en inschrijving delegeren'.

![De delegatie-pagina][api-management-delegation-signin-up]

* Bepalen wat de URL van uw speciaal delegatie-eindpunt worden en geef deze op in de **delegatie-eindpunt-URL** veld. 
* Voer in de verificatie sleutelveld van overdracht, een geheim dat wordt gebruikt voor het berekenen van een handtekening die u voor verificatie om ervoor te zorgen dat de aanvraag inderdaad afkomstig is van Azure API Management. U kunt klikken op de **genereren** knop API Management willekeurig genereren van een sleutel voor u hebben.

Nu moet u maken de **delegatie-eindpunt**. Het heeft een aantal acties uitvoeren:

1. Een aanvraag ontvangen in de volgende notatie:
   
   > *http:\//www.yourwebsite.com/apimdelegation?operation=SignIn & returnUrl = {URL van de bronpagina} & salt = {tekenreeks} & sig = {tekenreeks}*
   > 
   > 
   
    Queryparameters voor de aanmelding / geval registreren:
   
   * **bewerking**: wordt aangegeven welk type overdracht-aanvraag is - deze mag alleen bestaan uit **SignIn** in dit geval
   * **returnUrl**: de URL van de pagina waar de gebruiker op de koppeling van een aanmelden of registreren klikt
   * **Salt**: een speciale salt tekenreeks die wordt gebruikt voor een berekende hash van beveiliging
   * **SIG**: een hash berekende beveiliging moet worden gebruikt voor vergelijking van uw eigen berekende hash
2. Controleer of dat de aanvraag afkomstig is van Azure API Management (optioneel maar ten zeerste aanbevolen voor beveiliging)
   
   * Berekenen van een HMAC-SHA512-hash van een tekenreeks op basis van de **returnUrl** en **salt** queryparameters ([voorbeeldcode hieronder]):
     
     > HMAC (**salt** + '\n' + **returnUrl**)
     > 
     > 
   * Vergelijk de boven-berekende hash op de waarde van de **sig** queryparameter. Als de twee hashes overeenkomen, gaat u verder met de volgende stap, anders wordt de aanvraag weigeren.
3. Controleer of dat u een aanvraag voor sign-in/aanmelding-up ontvangt: het **bewerking** queryparameter wordt ingesteld op '**aanmelding**'.
4. De gebruiker met de gebruikersinterface presenteren aan aanmelden of registreren
5. Als de gebruiker melden die u moet een bijbehorende account voor hen in API Management maken. [Maken van een gebruiker] met de API Management REST-API. Wanneer in dat geval, zorg ervoor dat u de gebruikers-ID ingesteld op hetzelfde is in het archief van de gebruiker of een id die u kunt bijhouden van.
6. Wanneer de gebruiker is geverifieerd:
   
   * [aanvraag voor een eenmalige aanmelding (SSO)-token] via de API Management REST-API
   * een queryparameter returnUrl toevoegen aan de SSO-URL die u hebt ontvangen van de API-aanroep hierboven:
     
     > Bijvoorbeeld: https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url 
     > 
     > 
   * de gebruiker omgeleid naar de URL van de bovenstaande geproduceerd

Naast de **SignIn** bewerking, u kunt ook uitvoeren accountbeheer door de vorige stappen te volgen en daarbij een van de volgende bewerkingen:

* **ChangePassword**
* **ChangeProfile**
* **CloseAccount**

U moet de volgende queryparameters accountbeheerbewerkingen doorgeven.

* **bewerking**: Hiermee identificeert u welk type overdracht-aanvraag (ChangePassword, ChangeProfile of CloseAccount)
* **gebruikers-id**: de gebruikers-id van het account voor het beheren van
* **Salt**: een speciale salt tekenreeks die wordt gebruikt voor een berekende hash van beveiliging
* **SIG**: een hash berekende beveiliging moet worden gebruikt voor vergelijking van uw eigen berekende hash

## <a name="delegate-product-subscription"> </a>Product-abonnement overdragen
Product-abonnement overdragen werkt op dezelfde manier naar gebruiker aanmelden /-up overdragen. De laatste werkstroom zou als volgt zijn:

1. Ontwikkelaar van een product selecteert in de API Management-portal voor ontwikkelaars en klikt op de knop aanmelden
2. Browser wordt omgeleid naar de delegatie-eindpunt
3. Delegatie-eindpunt vereist product abonnement stappen worden uitgevoerd - deze aan u is en kan leiden tot omleiden naar een andere pagina om aan te vragen van factureringsgegevens, extra vragen, of gewoon de informatie op te slaan en niet een gebruikersactie vereist

Inschakelen van de functionaliteit voor de **delegering** pagina op **productabonnement delegeren**.

Controleer vervolgens of de delegatie-eindpunt voert de volgende handelingen uit:

1. Een aanvraag ontvangen in de volgende notatie:
   
   > *http:\//www.yourwebsite.com/apimdelegation?operation= {operation} & product-id = {product abonneren op} & gebruikers-id = {user aanvraag} & salt = {tekenreeks} & sig = {tekenreeks}*
   > 
   > 
   
    De parameters van de query voor het geval van product-abonnement:
   
   * **bewerking**: wordt aangegeven welk type overdracht-aanvraag is. Voor het product abonnement zijn aanvragen de geldige opties:
     * 'Abonnement': een aanvraag voor een abonnement van de gebruiker voor een bepaald product met opgegeven ID (Zie hieronder)
     * "Afmelden": een aanvraag voor een gebruiker uit het product afmelden
     * 'Vernieuwen': een verzoek om te vernieuwen van een abonnement (bijvoorbeeld, die kan verlopen)
   * **product-id**: de ID van het product dat de gebruiker gevraagd om u te abonneren op
   * **gebruikers-id**: de ID van de gebruiker voor wie de aanvraag wordt gedaan
   * **Salt**: een speciale salt tekenreeks die wordt gebruikt voor een berekende hash van beveiliging
   * **SIG**: een hash berekende beveiliging moet worden gebruikt voor vergelijking van uw eigen berekende hash
2. Controleer of dat de aanvraag afkomstig is van Azure API Management (optioneel maar ten zeerste aanbevolen voor beveiliging)
   
   * Berekenen van een HMAC-SHA512 gebruikt van een tekenreeks op basis van de **productId**, **userId**, en **salt** queryparameters:
     
     > HMAC (**salt** + '\n' + **productId** + '\n' + **userId**)
     > 
     > 
   * Vergelijk de boven-berekende hash op de waarde van de **sig** queryparameter. Als de twee hashes overeenkomen, gaat u verder met de volgende stap, anders wordt de aanvraag weigeren.
3. Een product abonnement verwerken op basis van het type in de aangevraagde bewerking **bewerking** : bijvoorbeeld facturering, nog meer vragen hebt, enzovoort.
4. Abonneren op het abonnement is de gebruiker aan het product zelf, de gebruiker naar de API Management-product door [aanroepen van de REST-API voor het product abonnement].

## <a name="delegate-example-code"> </a> Voorbeeldcode
Deze codevoorbeelden laten zien hoe u duren voordat de *delegatie van validatiesleutel*, die in het scherm van de overdracht van de publicatieportal is ingesteld op het maken van een HMAC, dit vervolgens gebruikt wordt om de handtekening te valideren, de geldigheid van de doorgegeven aan te tonen returnUrl. Dezelfde code werkt voor de gebruikers-id met kleine wijziging en de product-id.

**C#code voor het genereren van de hash van returnUrl**

```csharp
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

**NodeJS-code voor het genereren van de hash van returnUrl**

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
Zie de volgende video voor meer informatie over overdracht:

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Delegating-User-Authentication-and-Product-Subscription-to-a-3rd-Party-Site/player]
> 
> 

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[aanvraag voor een eenmalige aanmelding (SSO)-token]: https://docs.microsoft.com/rest/api/apimanagement/User/GenerateSsoUrl
[Maken van een gebruiker]: https://docs.microsoft.com/rest/api/apimanagement/user/createorupdate
[aanroepen van de REST-API voor het product abonnement]: https://docs.microsoft.com/rest/api/apimanagement/productsubscriptions
[Next steps]: #next-steps
[voorbeeldcode hieronder]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 

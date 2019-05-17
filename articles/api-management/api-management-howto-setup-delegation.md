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
ms.date: 04/04/2019
ms.author: apimpm
ms.openlocfilehash: 4db99f23019b34e7361e3ead4096939b9499320d
ms.sourcegitcommit: 17411cbf03c3fa3602e624e641099196769d718b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2019
ms.locfileid: "65518091"
---
# <a name="how-to-delegate-user-registration-and-product-subscription"></a>Hoe u kunt gebruiker-gebruikersregistratie en productabonnement delegeren

Overdracht kunt u uw bestaande website gebruiken voor het verwerken van de ontwikkelaar zich aanmelden / registreren en een abonnement op producten, in plaats van met behulp van de ingebouwde functie in de portal voor ontwikkelaars. Hierdoor kan uw website eigenaar zijn van de gebruikersgegevens en de validatie van de volgende stappen uit in een aangepaste manier uitvoeren.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="delegate-signin-up"> </a>Delegeren van de ontwikkelaar zich aanmelden en registreren

Voor het delegeren van de ontwikkelaar zich aanmelden en aanmelden met uw bestaande website, moet u een speciaal delegatie-eindpunt maken op uw site. Dit moet fungeren als toegangspunt voor het verzoek afkomstig zijn van de API Management-portal voor ontwikkelaars.

De laatste werkstroom zijn als volgt:

1. Developer-klikken op het teken in of meld u aan de koppeling aan de API Management-portal voor ontwikkelaars
2. Browser wordt omgeleid naar de delegatie-eindpunt
3. Delegatie-eindpunt in ruil wordt verwezen, of geeft gebruikersinterface waarin gebruiker wordt gevraagd u aan te melden of zich aanmelden
4. Indien geslaagd kunt wordt de gebruiker omgeleid naar de API Management developer portal-pagina die ze vanaf gestart

Om te beginnen, aanvragen gaan we eerste configuratie API Management voor het routeren van via de delegatie-eindpunt. Klik in de publicatieportal van API Management, op **Security** en klik vervolgens op de **delegering** tabblad. Klik op het selectievakje in om in te schakelen 'Delegeren aanmelden en registreren'.

![De delegatie-pagina][api-management-delegation-signin-up]

* Bepalen wat de URL van uw speciaal delegatie-eindpunt worden en geef deze op in de **delegatie-eindpunt-URL** veld. 
* Voer in de verificatie sleutelveld van overdracht, een geheim dat wordt gebruikt voor het berekenen van een handtekening die u voor verificatie om ervoor te zorgen dat de aanvraag inderdaad afkomstig is van Azure API Management. U kunt klikken op de **genereren** knop API Management willekeurig genereren van een sleutel voor u hebben.

Nu moet u maken de **delegatie-eindpunt**. Het heeft een aantal acties uitvoeren:

1. Een aanvraag ontvangen in de volgende notatie:
   
   > *http:\//www.yourwebsite.com/apimdelegation?operation=SignIn & returnUrl = {URL van de bronpagina} & salt = {tekenreeks} & sig = {tekenreeks}*
   > 
   > 
   
    Queryparameters voor het aanmelden / registreren geval:
   
   * **bewerking**: wordt aangegeven welk type overdracht-aanvraag is - deze mag alleen bestaan uit **SignIn** in dit geval
   * **returnUrl**: de URL van de pagina waar de gebruiker hebt geklikt op een aanmelden of registreren koppeling
   * **Salt**: een speciale salt tekenreeks die wordt gebruikt voor een berekende hash van beveiliging
   * **SIG**: een hash berekende beveiliging moet worden gebruikt voor vergelijking van uw eigen berekende hash
2. Controleer of dat de aanvraag afkomstig is van Azure API Management (optioneel maar ten zeerste aanbevolen voor beveiliging)
   
   * Berekenen van een HMAC-SHA512-hash van een tekenreeks op basis van de **returnUrl** en **salt** queryparameters ([voorbeeldcode hieronder]):
     
     > HMAC (**salt** + '\n' + **returnUrl**)
     > 
     > 
   * Vergelijk de boven-berekende hash op de waarde van de **sig** queryparameter. Als de twee hashes overeenkomen, gaat u verder met de volgende stap, anders wordt de aanvraag weigeren.
3. Controleer of u ontvangt een aanvraag voor aanmelden / registreren: de **bewerking** queryparameter wordt ingesteld op '**SignIn**'.
4. De gebruiker te presenteren met gebruikersinterface aanmelden of registreren
5. Als de gebruiker melden die u moet een bijbehorende account voor hen in API Management maken. [Maken van een gebruiker] met de API Management REST-API. Wanneer in dat geval, zorg ervoor dat u de gebruikers-ID ingesteld op dezelfde waarde als in uw archief van de gebruiker of een id die u kunt bijhouden van.
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
* **gebruikers-id**: de gebruikers-ID van het account voor het beheren van
* **Salt**: een speciale salt tekenreeks die wordt gebruikt voor een berekende hash van beveiliging
* **SIG**: een hash berekende beveiliging moet worden gebruikt voor vergelijking van uw eigen berekende hash

## <a name="delegate-product-subscription"> </a>Product-abonnement overdragen
Product abonnement overdragen werkt op dezelfde manier naar gebruiker aanmelden /-up overdragen. De laatste werkstroom zou als volgt zijn:

1. Ontwikkelaar een product selecteert in de API Management-portal voor ontwikkelaars en klikt op de knop aanmelden.
2. Browser wordt omgeleid naar de delegatie-eindpunt.
3. Delegatie-eindpunt uitvoert vereist product abonnement stappen. Het is aan u de stappen te ontwerpen. Ze kunnen bevatten omleiden naar een andere pagina om aan te vragen van factureringsgegevens, extra vragen, of gewoon de informatie op te slaan en niet een gebruikersactie vereist.

Inschakelen van de functionaliteit voor de **delegering** pagina op **productabonnement delegeren**.

Controleer vervolgens of dat de delegatie-eindpunt heeft de volgende acties:

1. Een aanvraag ontvangen in de volgende notatie:
   
   > *http:\//www.yourwebsite.com/apimdelegation?operation= {operation} & product-id = {product abonneren op} & gebruikers-id = {user aanvraag} & salt = {tekenreeks} & sig = {tekenreeks}*
   >
   
    De parameters van de query voor het geval van product-abonnement:
   
   * **bewerking**: wordt aangegeven welk type overdracht-aanvraag is. Voor het product abonnement zijn aanvragen de geldige opties:
     * 'Abonnement': een aanvraag voor een abonnement van de gebruiker voor een bepaald product met opgegeven ID (Zie hieronder)
     * "Afmelden": een aanvraag voor een gebruiker uit het product afmelden
     * 'Vernieuwen': een verzoek om te vernieuwen van een abonnement (bijvoorbeeld, die kan verlopen)
   * **product-id**: de ID van het product dat de gebruiker gevraagd om u te abonneren op
   * **subscriptionId**: op *Afmelden* en *vernieuwen* -de ID van de product-abonnement
   * **gebruikers-id**: de ID van de gebruiker de aanvraag wordt gedaan voor
   * **Salt**: een speciale salt tekenreeks die wordt gebruikt voor een berekende hash van beveiliging
   * **SIG**: een hash berekende beveiliging moet worden gebruikt voor vergelijking van uw eigen berekende hash

2. Controleer of dat de aanvraag afkomstig is van Azure API Management (optioneel maar ten zeerste aanbevolen voor beveiliging)
   
   * Berekenen van een HMAC-SHA512 gebruikt van een tekenreeks op basis van de **productId**, **userId**, en **salt** queryparameters:
     
     > HMAC (**salt** + '\n' + **productId** + '\n' + **userId**)
     > 
     > 
   * Vergelijk de boven-berekende hash op de waarde van de **sig** queryparameter. Als de twee hashes overeenkomen, gaat u verder met de volgende stap, anders wordt de aanvraag weigeren.
3. Verwerken van de product-abonnement op basis van het type in de aangevraagde bewerking **bewerking** : bijvoorbeeld facturering, nog meer vragen hebt, enzovoort.
4. Abonneren op het abonnement is de gebruiker aan het product zelf, de gebruiker naar de API Management-product door [aanroepen van de REST-API voor abonnementen].

## <a name="delegate-example-code"> </a> Voorbeeldcode

Deze voorbeelden tonen hoe code op:

* Duren voordat de *delegatie van validatiesleutel*, die is ingesteld in het scherm van de overdracht van de publisher-portal
* Maak een HMAC, dit vervolgens gebruikt wordt om de handtekening te valideren, de geldigheid van de doorgegeven returnUrl aan te tonen.

Dezelfde code werkt voor de gebruikers-id met kleine wijziging en de product-id.

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

[Delegating developer sign in and sign up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[aanvraag voor een eenmalige aanmelding (SSO)-token]: https://docs.microsoft.com/rest/api/apimanagement/User/GenerateSsoUrl
[Maken van een gebruiker]: https://docs.microsoft.com/rest/api/apimanagement/user/createorupdate
[aanroepen van de REST-API voor abonnementen]: https://docs.microsoft.com/rest/api/apimanagement/subscription/createorupdate
[Next steps]: #next-steps
[voorbeeldcode hieronder]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 

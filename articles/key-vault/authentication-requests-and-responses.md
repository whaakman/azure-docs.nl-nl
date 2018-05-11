---
title: Verificatie, aanvragen en antwoorden
description: Verifiëren naar AD voor het gebruik van de Sleutelkluis
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 4c321939-8a5b-42ca-83c4-2f5f647ca13e
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: alleonar
ms.openlocfilehash: 94080fb124478a4b8e196e341c335ca32321ecdf
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="authentication-requests-and-responses"></a>Verificatie, aanvragen en antwoorden

Azure Sleutelkluis biedt ondersteuning voor JSON-indeling aanvragen en antwoorden. Aanvragen voor Azure Sleutelkluis worden omgeleid naar een geldige URL voor Azure Sleutelkluis via dat HTTPS met enkele URL-parameters en JSON-aanvraag en antwoord instanties gecodeerd.

Dit onderwerp worden de details voor de Azure Sleutelkluis-service. Raadpleeg voor algemene informatie over het gebruik van Azure REST-interfaces, met inbegrip van verificatie/autorisatie en het verkrijgen van een toegangstoken [Azure REST API-verwijzing](https://docs.microsoft.com/rest/api/).

## <a name="request-url"></a>Aanvraag-URL  
 Sleutelbeheer bewerkingen gebruik van HTTP DELETE, GET, PATCH, opslag en HTTP POST en cryptografische bewerkingen met bestaande sleutel objecten HTTP POST gebruiken. Clients die niet kunnen specifieke HTTP-termen ondersteunen mogelijk ook gebruiken voor HTTP POST met behulp van de header X-HTTP-aanvraag om op te geven van de beoogde term; aanvragen die normaal gesproken geen hoofdtekst vereisen moeten een lege hoofdtekst opnemen wanneer u HTTP POST, bijvoorbeeld wanneer POST gebruik in plaats van verwijderen.  

 Als u wilt werken met objecten in de Azure Sleutelkluis, zijn de volgende voorbeeld-URL's:  

-   Een TESTKEY aangeroepen in het gebruik van een Sleutelkluis - sleutel te maken `PUT /keys/TESTKEY?api-version=<api_version> HTTP/1.1`  

-   Voor het importeren van een IMPORTEDKEY aangeroepen in het gebruik van een Sleutelkluis - sleutel `POST /keys/IMPORTEDKEY/import?api-version=<api_version> HTTP/1.1`  

-   Een geheim MYSECRET aangeroepen in het gebruik van een Sleutelkluis - ophalen `GET /secrets/MYSECRET?api-version=<api_version> HTTP/1.1`  

-   Als u wilt een digest ONDERTEKENEN TESTKEY met een sleutel aangeroepen in het gebruik van een Sleutelkluis- `POST /keys/TESTKEY/sign?api-version=<api_version> HTTP/1.1`  

 De instantie van een aanvraag aan een Sleutelkluis is altijd als volgt,  `https://{keyvault-name}.vault.azure.net/`  

 Sleutels worden altijd opgeslagen onder het pad /keys, geheimen worden altijd opgeslagen onder het pad /secrets.  

## <a name="api-version"></a>API-versie  
 De Azure Key Vault Service ondersteunt protocol versiebeheer voor compatibiliteit met downlevel-clients, maar niet alle mogelijkheden is beschikbaar op deze clients. Clients moeten gebruiken de `api-version` querytekenreeksparameter om op te geven van de versie van het protocol dat ze ondersteunen als er geen standaardwaarde is.  

 Versies van Azure Sleutelkluis-protocol voert u een schema met behulp van een {jjjj} nummering datum. {MM}. De indeling {DD}.  

## <a name="request-body"></a>Aanvraagtekst  
 Volgens de specificatie HTTP GET-bewerkingen mag geen aanvraagtekst en post- en PUT-bewerkingen moeten een aanvraaginhoud hebben. De hoofdtekst in DELETE-bewerkingen is optioneel in HTTP.  

 Tenzij anders vermeld in de beschrijving van de bewerking, wordt het inhoudstype van de aanvraag hoofdtekst moet application/json en een geserialiseerde conform de JSON-object voor type inhoud moet bevatten.  

 Tenzij anders vermeld in de beschrijving van de bewerking moet het mediatype van application/json bevatten in de aanvraagheader accepteren.  

## <a name="response-body"></a>Antwoordtekst  
 Tenzij anders vermeld in de beschrijving van de bewerking, wordt het antwoord hoofdtekst-inhoudstype van geslaagde en mislukte bewerkingen worden application/json en bevat uitgebreide foutinformatie.  

## <a name="using-http-post"></a>Met behulp van HTTP POST  
 Sommige clients mogelijk niet kunnen gebruiken van bepaalde HTTP-termen, zoals PATCH of verwijderen. Azure Sleutelkluis ondersteunt HTTP POST als alternatief voor deze clients op voorwaarde dat de client ook de koptekst 'X-HTTP-METHOD' tot specifieke het oorspronkelijke HTTP-term bevat. Ondersteuning voor HTTP POST wordt voor elk van de API die in dit document vermeld.  

## <a name="error-responses"></a>Foutberichten  
 Foutafhandeling HTTP-statuscodes gebruikt. Typische resultaten zijn:  

-   2xx – geslaagd: gebruikt voor een normale werking. De hoofdtekst van het antwoord bevat de verwachte resultaat  

-   3xx – omleiding: de 304 'niet gewijzigd' om te voldoen aan een voorwaardelijke GET kan worden geretourneerd. Andere codes 3xx kunnen in de toekomst worden gebruikt om aan te geven van wijzigingen in DNS- en pad.  

-   4XX: fout bij de Client: voor ongeldige aanvragen, ontbrekende sleutels, syntaxisfouten, ongeldige parameters, verificatiefouten gebruikt, enzovoort. Hoofdtekst van de reactie bevat gedetailleerde fout uitleg.  

-   5XX-serverfout: gebruikt voor interne server-fouten. Hoofdtekst van de reactie bevat informatie over de fout samengevatte.  

 Het systeem is ontworpen voor gebruik achter een proxy of firewall. Een client ontvangt daarom andere foutcodes.  

 Azure Sleutelkluis retourneert ook informatie over de fout in de hoofdtekst van antwoord wanneer er een probleem optreedt. De antwoordtekst is JSON-indeling en heeft de vorm:  

```  

{  
  "error":  
  {  
    "code": "BadArgument",  
    "message":  

      "’Foo’ is not a valid argument for ‘type’."  
    }  
  }  
}  

```  

## <a name="authentication"></a>Verificatie  
 Alle aanvragen naar Azure Key Vault moet worden geverifieerd. Azure Sleutelkluis biedt ondersteuning voor Azure Active Directory-toegangstokens die kunnen worden verkregen met op OAuth2 [[RFC6749](http://tools.ietf.org/html/rfc6749)]. 
 
 Zie voor meer informatie over het registreren van uw toepassing en verificatie voor het gebruik van Azure Sleutelkluis [uw clienttoepassing registreren met Azure AD](https://docs.microsoft.com/rest/api/index#register-your-client-application-with-azure-ad).
 
 Toegangstokens moeten worden verzonden naar de service met behulp van de HTTP-autorisatie-header:  

```  
PUT /keys/MYKEY?api-version=<api_version>  HTTP/1.1  
Authorization: Bearer <access_token>  

```  

 Als een toegangstoken niet wordt opgegeven, of wanneer een token wordt niet geaccepteerd door de service, wordt een 401 HTTP-fout wordt geretourneerd naar de client en bevat de WWW-Authenticate-header, bijvoorbeeld:  

```  
401 Not Authorized  
WWW-Authenticate: Bearer authorization="…", resource="…"  

```  

 De parameters op de WWW-Authenticate-header zijn:  

-   autorisatie: het adres van de service voor OAuth2-machtigingen die kan worden gebruikt voor het verkrijgen van een toegangstoken voor de aanvraag.  

-   bron: de naam van de resource moet worden gebruikt in de autorisatieaanvraag.  

## <a name="see-also"></a>Zie ook  
 [Informatie over sleutels, geheimen en certificaten](about-keys-secrets-and-certificates.md)

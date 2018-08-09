---
title: Verificatie, vragen en antwoorden
description: Verifiëren met AD voor het gebruik van Key Vault
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
ms.openlocfilehash: caa2d74ecafe0b0e2508bd97eb4dc21a18e58f51
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39626417"
---
# <a name="authentication-requests-and-responses"></a>Verificatie, vragen en antwoorden

Azure Key Vault biedt ondersteuning voor JSON-indeling aanvragen en antwoorden. Aanvragen voor de Azure Key Vault worden doorgestuurd naar een geldige URL voor Azure Key Vault met behulp van dat HTTPS met enkele URL-parameters en JSON gecodeerd organen van de aanvraag en reactie.

In dit onderwerp bevat informatie over specificaties voor de Azure Key Vault-service. Raadpleeg voor algemene informatie over het gebruik van Azure REST-interfaces, zoals verificatie/autorisatie en hoe u een toegangstoken verkrijgen [Azure REST API-verwijzing](https://docs.microsoft.com/rest/api/azure).

## <a name="request-url"></a>Aanvraag-URL  
 Sleutelbeheerbewerkingen Gebruik HTTP DELETE, GET, PATCH, PUT en HTTP POST en cryptografische bewerkingen op basis van bestaande sleutels objecten gebruiken HTTP POST. Clients die niet kunnen specifieke HTTP-termen ondersteunen kunnen ook gebruiken voor HTTP POST met behulp van de X-HTTP-aanvraagheader om op te geven van de beoogde term; aanvragen die normaal gesproken niet een hoofdtekst hoeven moeten een lege hoofdtekst bevatten bij het gebruik van HTTP POST, bijvoorbeeld bij het gebruik van POST in plaats van te verwijderen.  

 Als u wilt werken met objecten in de Azure Key Vault, zijn het volgende voorbeeld-URL's:  

-   Een TESTKEY genoemd in het gebruik van een Key Vault - sleutel te maken `PUT /keys/TESTKEY?api-version=<api_version> HTTP/1.1`  

-   Om een IMPORTEDKEY genoemd in het gebruik van een Key Vault - sleutel te importeren `POST /keys/IMPORTEDKEY/import?api-version=<api_version> HTTP/1.1`  

-   Een MYSECRET genoemd in het gebruik van een Key Vault - geheim ophalen `GET /secrets/MYSECRET?api-version=<api_version> HTTP/1.1`  

-   Als u wilt een samenvatting te ONDERTEKENEN met behulp van een sleutel met de naam TESTKEY in het gebruik van een Key Vault- `POST /keys/TESTKEY/sign?api-version=<api_version> HTTP/1.1`  

 De instantie voor een aanvraag voor een Key Vault is altijd als volgt te werk,  `https://{keyvault-name}.vault.azure.net/`  

 Sleutels altijd worden opgeslagen onder het pad /keys, geheimen altijd worden opgeslagen onder het pad /secrets.  

## <a name="api-version"></a>API-versie  
 De Azure Key Vault-Service ondersteunt protocol versiebeheer voor compatibiliteit met clients met oudere versies, maar niet alle mogelijkheden beschikbaar voor deze clients is. Clients moeten gebruiken de `api-version` query-tekenreeksparameter om op te geven van de versie van het protocol die worden ondersteund als er geen standaardwaarde is.  

 Azure Key Vault-protocolversies voert u een datum nummering schema met behulp van een {DD}. {MM}. De indeling {yyyy}.  

## <a name="request-body"></a>Aanvraagtekst  
 GET-bewerkingen mag geen aanvraagtekst aan de hand van de HTTP-specificatie en POST en PUT-bewerkingen moeten een aanvraagtekst hebben. De hoofdtekst van de bewerking verwijderen is optioneel in HTTP.  

 Tenzij anders vermeld in de beschrijving van de bewerking, wordt het inhoudstype van de aanvraag hoofdtekst moet application/json en een geserialiseerde JSON-object in overeenstemming inhoudstype moet bevatten.  

 Tenzij anders vermeld in de beschrijving van de bewerking, moet de Accept-header voor aanvraag voor het mediatype application/json bevatten.  

## <a name="response-body"></a>Antwoordtekst  
 Tenzij anders vermeld in de beschrijving van de bewerking, wordt het antwoord hoofdtekst-inhoudstype van geslaagde en mislukte bewerkingen worden application/json en bevat gedetailleerde foutgegevens.  

## <a name="using-http-post"></a>Met behulp van HTTP POST  
 Sommige clients mogelijk niet kunnen gebruiken van bepaalde HTTP-termen, zoals de PATCH- of DELETE. Azure Key Vault ondersteunt HTTP POST als alternatief voor deze clients voorwaarde dat de client ook de header van de "X-HTTP-methode" aan specifieke de oorspronkelijke HTTP-woord bevat. Ondersteuning voor HTTP POST wordt vermeld voor elk van de API die is gedefinieerd in dit document.  

## <a name="error-responses"></a>Foutberichten  
 Foutafhandeling gebruikt HTTP-statuscodes. Typische resultaten zijn:  

-   2xx: geslaagd: gebruikt voor normale werking. De antwoordtekst bevat het verwachte resultaat  

-   3xx – omleiding: de 304 "niet gewijzigd' om te voldoen aan een voorwaardelijke GET kan worden geretourneerd. Andere codes 3xx kunnen in de toekomst worden gebruikt om aan te geven van wijzigingen in DNS en pad.  

-   4XX-Clientfout: gebruikt voor ongeldige aanvragen, ontbrekende sleutels, syntaxisfouten, er zijn ongeldige parameters, -verificatiefouten, enzovoort. De antwoordtekst bevat gedetailleerde fout uitleg.  

-   5XX-serverfout: gebruikt voor interne serverfouten optreden. De antwoordtekst bevat informatie over samengevatte fouten.  

 Het systeem is ontworpen om te werken achter een proxy of firewall. Een client ontvangt daarom andere foutcodes.  

 Azure Key Vault retourneert ook informatie over de fout in de antwoordtekst wanneer er een probleem optreedt. De antwoordtekst JSON-indeling is en heeft de vorm:  

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
 Alle aanvragen naar de Azure Key Vault moet worden geverifieerd. Azure Key Vault ondersteunt Azure Active Directory-toegangstokens die kunnen worden verkregen met behulp van OAuth2 [[RFC6749](http://tools.ietf.org/html/rfc6749)]. 
 
 Zie voor meer informatie over het registreren van uw toepassing en verifiëren voor het gebruik van Azure Key Vault, [uw clienttoepassing registreren bij Azure AD](https://docs.microsoft.com/rest/api/azure/index#register-your-client-application-with-azure-ad).
 
 Toegangstokens moeten worden verzonden naar de service met behulp van de HTTP-autorisatie-header:  

```  
PUT /keys/MYKEY?api-version=<api_version>  HTTP/1.1  
Authorization: Bearer <access_token>  

```  

 Wanneer een toegangstoken is niet opgegeven, of wanneer een token wordt niet geaccepteerd door de service, wordt een 401 HTTP-fout worden geretourneerd naar de client en bevat de WWW-Authenticate-header, bijvoorbeeld:  

```  
401 Not Authorized  
WWW-Authenticate: Bearer authorization="…", resource="…"  

```  

 De parameters op de WWW-Authenticate-header zijn:  

-   autorisatie: het adres van de OAuth2-autorisatie-service die kan worden gebruikt voor het verkrijgen van een toegangstoken voor de aanvraag.  

-   bron: de naam van de resource moet worden gebruikt in de autorisatieaanvraag.  

## <a name="see-also"></a>Zie ook  
 [Informatie over sleutels, geheimen en certificaten](about-keys-secrets-and-certificates.md)

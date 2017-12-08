---
title: Overzicht van Media Services bewerkingen REST-API | Microsoft Docs
description: REST-API voor Media Services-overzicht
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a5f1c5e7-ec52-4e26-9a44-d9ea699f68d9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: eada8f2bcd2488d5ed36b0c24aa3d1b917815517
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="media-services-operations-rest-api-overview"></a>Media Services operations REST-API-overzicht
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

De **Media Services Operations REST** API wordt gebruikt voor het maken van taken, assets, beleidsregels voor toegang en andere bewerkingen voor objecten in een Media Service-account. Zie voor meer informatie [naslaginformatie over REST API van Media Services Operations](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).

Microsoft Azure Media Services is een service die op basis van een OData-HTTP-aanvragen accepteert en terug in de uitgebreide JSON of atom + pub kan reageren. Omdat het Media Services voldoen aan de richtlijnen voor het ontwerpen van Azure, is er een reeks vereiste HTTP-headers die elke client gebruiken moet wanneer verbinding met Media Services, evenals een aantal optionele headers die kunnen worden gebruikt. De volgende secties worden de kopteksten en HTTP-termen die u kunt gebruiken bij maken van aanvragen en antwoorden ontvangen van Media Services.

In dit onderwerp geeft een overzicht van het gebruik van v2 REST met Media Services.

## <a name="considerations"></a>Overwegingen

De volgende overwegingen wanneer u de REST van toepassing.

* Tijdens het opvragen van entiteiten, is er een limiet van 1000 entiteiten in één keer wordt geretourneerd omdat openbare REST v2 queryresultaten tot 1000 resultaten beperkt. U moet gebruiken **overslaan** en **nemen** (.NET) / **boven** (REST) zoals beschreven in [in dit voorbeeld .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) en [deze REST-API voorbeeld](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 
* Wanneer met behulp van JSON en op te geven voor het gebruik van de **__metadata** -sleutelwoord in de aanvraag (bijvoorbeeld naar verwijst naar een gekoppelde object) stelt u de **accepteren** koptekst tot [uitgebreide JSON-indeling](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/) (Zie het volgende voorbeeld). OData niet begrijpt het **__metadata** eigenschap in de aanvraag, tenzij u deze op uitgebreide instellen.  
  
        POST https://media.windows.net/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.11
        Authorization: Bearer <token> 
        Host: media.windows.net
  
        {
            "Name" : "NewTestJob", 
            "InputMediaAssets" : 
                [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aba5356eb-30ff-4dc6-9e5a-41e4223540e7')"}}]
        . . . 

## <a name="standard-http-request-headers-supported-by-media-services"></a>Standaard HTTP-aanvraagheaders ondersteund door Media Services
Er is een reeks vereiste headers die u in uw aanvraag opnemen moet voor elke aanroep die u in Media Services aanbrengt, en ook een set optionele headers u mogelijk wilt opnemen. De volgende tabel bevat de vereiste headers:

| Koptekst | Type | Waarde |
| --- | --- | --- |
| Autorisatie |Bearer |Bearer is de enige toegestane autorisatiemechanismen. De waarde moet het toegangstoken dat door de ACS ook bevatten. |
| x-ms-version |Decimale |2.11 |
| DataServiceVersion |Decimale |3.0 |
| MaxDataServiceVersion |Decimale |3.0 |

> [!NOTE]
> Omdat OData Media Services gebruikt om de onderliggende opslag van de metagegevens asset via REST API's weer te geven, moeten de DataServiceVersion en MaxDataServiceVersion headers worden opgenomen in elk verzoek; echter als dat niet het geval is, klikt u vervolgens op dit moment Media Services wordt ervan uitgegaan dat de bedrijfswaarde DataServiceVersion 3.0.
> 
> 

Hier volgt een set optionele headers:

| Koptekst | Type | Waarde |
| --- | --- | --- |
| Date |RFC 1123 datum |Tijdstempel van de aanvraag |
| Accepteren |Type inhoud |Het aangevraagde type inhoud voor het antwoord zoals het volgende:<p> -application/json; odata = verbose<p> -application/atom + xml<p> Antwoorden mogelijk een ander inhoudstype, zoals een blob-ophalen waarbij een geslaagde reactie bevat de blob-stroom als de nettolading. |
| Accepteer codering |Gzip, deflate |GZIP en DEFLATE codering, indien van toepassing. Opmerking: Voor grote bronnen Media Services kunnen deze header negeren en niet-gecomprimeerde gegevens retourneren. |
| Accepteer taal |'en', 'es', enzovoort. |Hiermee geeft u de gewenste taal voor het antwoord. |
| Accepteer Charset |Type Charset zoals "UTF-8" |Standaardwaarde is UTF-8. |
| X-HTTP-methode |HTTP-methode |Kan de clients of firewalls die HTTP-methoden zoals PUT of verwijderen voor het gebruik van deze methoden, via een tunnel via een GET-aanroep niet ondersteunen. |
| Content-Type |Type inhoud |Type inhoud van de aanvraagtekst in PUT- of POST-aanvragen. |
| client-request-id |Tekenreeks |Een aanroeper gedefinieerde waarde waarmee de aanvraag. Als u opgeeft, wordt deze waarde in het antwoordbericht voor het toewijzen van de aanvraag opgenomen. <p><p>**Belangrijk**<p>Waarden moeten worden beperkt tot 2096b (2k). |

## <a name="standard-http-response-headers-supported-by-media-services"></a>Standaard-HTTP-antwoordheaders ondersteund door Media Services
Hier volgt een set headers die kunnen worden geretourneerd voor u, afhankelijk van de resource die u zijn aangevraagd en de actie die u wilde uitvoeren.

| Koptekst | Type | Waarde |
| --- | --- | --- |
| aanvraag-id |Tekenreeks |Een unieke id voor de huidige bewerking, service gegenereerd. |
| client-request-id |Tekenreeks |Een id die is opgegeven door de aanroeper de oorspronkelijke aanvraag, indien aanwezig. |
| Date |RFC 1123 datum |De datum waarop de aanvraag is verwerkt. |
| Content-Type |Varieert |Het inhoudstype van de antwoordtekst. |
| Codering van inhoud |Varieert |Gzip of verkleinen, indien nodig. |

## <a name="standard-http-verbs-supported-by-media-services"></a>Standaard HTTP-termen die door Media Services worden ondersteund
Hier volgt een volledige lijst met HTTP-termen die kunnen worden gebruikt bij het maken van HTTP-aanvragen:

| Term | Beschrijving |
| --- | --- |
| TOEVOEGEN |Retourneert de huidige waarde van een object. |
| VERZENDEN |Hiermee maakt u een object op basis van de gegevens of een opdracht verzonden. |
| PLAATSEN |Maakt een benoemd object (indien van toepassing) of een object vervangt. |
| VERWIJDEREN |Hiermee verwijdert u een object. |
| SAMENVOEGEN |Een bestaand object met benoemde eigenschapswijzigingen bijgewerkt. |
| HEAD |Retourneert de metagegevens van een object voor een GET-antwoord. |

## <a name="discover-media-services-model"></a>Media Services-model detecteren
Als u Media Services-entiteiten meer kan worden gedetecteerd, kan de bewerking $metadata worden gebruikt. Hiermee kunt u voor het ophalen van alle geldige Entiteitstypen, entiteitseigenschappen, koppelingen, functies, acties, enzovoort. Het volgende voorbeeld laat zien hoe de URI te bouwen: https://media.windows.net/API/$ metagegevens.

U moet toevoegen '? api-version=2.x ' aan het einde van de URI als u wilt weergeven van de metagegevens in een browser of de header x-ms-version niet in uw aanvraag opnemen.

## <a name="connect-to-media-services"></a>Verbinding met Media Services maken

Zie voor meer informatie over de verbinding maken met de AMS API [toegang tot de API van Azure Media Services met Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md). Na het correct verbinding maakt met https://media.windows.net, ontvangt u een 301 omleiding opgeven van een andere URI van de Media Services. U moet de volgende aanroepen naar de nieuwe URI.

## <a name="next-steps"></a>Volgende stappen

Voor toegang tot AMS APIs met REST, Zie [gebruik Azure AD-verificatie voor toegang tot de Azure Media Services-API met REST](media-services-rest-connect-with-aad.md).

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


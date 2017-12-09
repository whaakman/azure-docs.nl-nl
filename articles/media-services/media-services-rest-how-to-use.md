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
ms.date: 12/05/2017
ms.author: juliako;johndeu
ms.openlocfilehash: 066959058576af830103aa98a12f0c36acfdbb14
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="media-services-operations-rest-api-overview"></a>Media Services operations REST-API-overzicht
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

De **Media Services Operations REST** API wordt gebruikt voor het maken van taken, Assets, Livekanalen en andere bronnen in een Media Services-account. Zie voor meer informatie [naslaginformatie over REST API van Media Services Operations](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).

Media Services biedt een REST-API die JSON of atom + XML-indeling pub accepteert. REST-API voor Media Services vereist specifieke HTTP-headers die elke client verzenden moet wanneer u verbinding met Media Services, evenals een aantal optionele headers. De volgende secties worden de kopteksten en HTTP-termen die u kunt gebruiken bij maken van aanvragen en antwoorden ontvangen van Media Services.

Verificatie van de Media Serivces REST-API wordt gedaan door middel van Azure Active Directory-verificatie die wordt beschreven in het artikel [gebruik Azure AD-verificatie voor toegang tot de Azure Media Services-API met REST](media-services-rest-connect-with-aad.md)

## <a name="considerations"></a>Overwegingen

De volgende overwegingen wanneer u de REST van toepassing.

* Tijdens het opvragen van entiteiten, is er een limiet van 1000 entiteiten in één keer wordt geretourneerd omdat openbare REST v2 queryresultaten tot 1000 resultaten beperkt. U moet gebruiken **overslaan** en **nemen** (.NET) / **boven** (REST) zoals beschreven in [in dit voorbeeld .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) en [deze REST-API voorbeeld](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 
* Wanneer met behulp van JSON en op te geven voor het gebruik van de **__metadata** sleutelwoord in de aanvraag (bijvoorbeeld om een verwijzing naar een gekoppeld object) stelt u de **accepteren** koptekst tot [uitgebreide JSON-indeling](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/)(Zie het volgende voorbeeld). OData niet begrijpt het **__metadata** eigenschap in de aanvraag, tenzij u deze op uitgebreide instellen.  
  
        POST https://media.windows.net/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.17
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
| Autorisatie |Bearer |Bearer is de enige toegestane autorisatiemechanismen. De waarde moet ook het toegangstoken dat door Azure Active Directory. |
| x-ms-version |Decimale |2.17 (of meest recente versie)|
| DataServiceVersion |Decimale |3.0 |
| MaxDataServiceVersion |Decimale |3.0 |

> [!NOTE]
> Omdat OData Media Services gebruikt om de REST-API's weer te geven, moeten de DataServiceVersion en MaxDataServiceVersion headers worden opgenomen in alle aanvragen; echter als dat niet het geval is, klikt u vervolgens op dit moment Media Services wordt ervan uitgegaan dat de bedrijfswaarde DataServiceVersion 3.0.
> 
> 

Hier volgt een set optionele headers:

| Koptekst | Type | Waarde |
| --- | --- | --- |
| Date |RFC 1123 datum |Tijdstempel van de aanvraag |
| Accepteren |Inhoudstype |Het aangevraagde type inhoud voor het antwoord zoals het volgende:<p> -application/json; odata = verbose<p> -application/atom + xml<p> Antwoorden mogelijk een ander inhoudstype, zoals een blob-ophalen waarbij een geslaagde reactie bevat de blob-stroom als de nettolading. |
| Accepteer codering |Gzip, deflate |GZIP en DEFLATE codering, indien van toepassing. Opmerking: Voor grote bronnen Media Services kunnen deze header negeren en niet-gecomprimeerde gegevens retourneren. |
| Accepteer taal |'en', 'es', enzovoort. |Hiermee geeft u de gewenste taal voor het antwoord. |
| Accepteer Charset |Type Charset zoals "UTF-8" |Standaardwaarde is UTF-8. |
| X-HTTP-methode |HTTP-methode |Kan de clients of firewalls die HTTP-methoden zoals PUT of verwijderen voor het gebruik van deze methoden, via een tunnel via een GET-aanroep niet ondersteunen. |
| Inhoudstype |Inhoudstype |Type inhoud van de aanvraagtekst in PUT- of POST-aanvragen. |
| client-request-id |Reeks |Een aanroeper gedefinieerde waarde waarmee de aanvraag. Als u opgeeft, wordt deze waarde in het antwoordbericht voor het toewijzen van de aanvraag opgenomen. <p><p>**Belangrijk**<p>Waarden moeten worden beperkt tot 2096b (2k). |

## <a name="standard-http-response-headers-supported-by-media-services"></a>Standaard-HTTP-antwoordheaders ondersteund door Media Services
Hier volgt een set headers die kunnen worden geretourneerd voor u, afhankelijk van de resource die u zijn aangevraagd en de actie die u wilde uitvoeren.

| Koptekst | Type | Waarde |
| --- | --- | --- |
| aanvraag-id |Reeks |Een unieke id voor de huidige bewerking, service gegenereerd. |
| client-request-id |Reeks |Een id die is opgegeven door de aanroeper de oorspronkelijke aanvraag, indien aanwezig. |
| Date |RFC 1123 datum |De datum/tijd die de aanvraag is verwerkt. |
| Inhoudstype |Varieert |Het inhoudstype van de antwoordtekst. |
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

## <a name="discover-and-browse-the-media-services-entity-model"></a>Detecteren en de Media Services-entiteitsmodel bladeren
Als u Media Services-entiteiten meer kan worden gedetecteerd, kan de bewerking $metadata worden gebruikt. Hiermee kunt u voor het ophalen van alle geldige Entiteitstypen, entiteitseigenschappen, koppelingen, functies, acties, enzovoort. De bewerking $metadata toevoegt aan het einde van uw Media Services REST-API-eindpunt, kunt u toegang tot deze discovery-service.

 /API/$ metagegevens.

U moet toevoegen '? api-version=2.x ' aan het einde van de URI als u wilt weergeven van de metagegevens in een browser of de header x-ms-version niet in uw aanvraag opnemen.

## <a name="authenticate-with-media-services-rest-using-azure-active-directory"></a>Verificatie met de REST van Media Services met Azure Active Directory

Verificatie op de REST-API wordt gedaan door Azure Active Directory(AAD).
Zie voor meer informatie over de details van de vereiste verificatie voor uw Media Services-account ophalen vanuit de Azure Portal [toegang tot de API van Azure Media Services met Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).

Zie voor meer informatie over het schrijven van code die is verbonden met de REST-API met Azure AD-verificatie, het artikel [gebruik Azure AD-verificatie voor toegang tot de Azure Media Services-API met REST](media-services-rest-connect-with-aad.md).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het gebruik van Azure AD-verificatie met Media Services REST-API, [gebruik Azure AD-verificatie voor toegang tot de Azure Media Services-API met REST](media-services-rest-connect-with-aad.md).

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


---
title: Overzicht van Media Services operations REST-API | Microsoft Docs
description: Media Services REST API-overzicht
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: a5f1c5e7-ec52-4e26-9a44-d9ea699f68d9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako;johndeu
ms.openlocfilehash: e0011d36ccff7b9d621679f15776bbdb15d0cbe4
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56005451"
---
# <a name="media-services-operations-rest-api-overview"></a>Overzicht van Media Services operations REST-API 
[!INCLUDE [media-services-selector-setup](../../../includes/media-services-selector-setup.md)]

De **Media Services Operations REST** API wordt gebruikt voor het maken van taken, Assets, Live kanalen en andere resources in een Media Services-account. Zie voor meer informatie, [naslaginformatie over REST-API van Media Services Operations](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).

Media Services biedt een REST-API die JSON of atom + XML-indeling pub accepteert. Media Services REST API vereist specifieke HTTP-headers die elke client verzenden moet bij het verbinden met Media Services, evenals een aantal optionele headers. De volgende secties beschrijven de headers en HTTP-termen die u kunt gebruiken bij het maken van aanvragen en antwoorden te ontvangen van Media Services.

Media Services REST API-verificatie vindt plaats via Azure Active Directory-verificatie die wordt beschreven in het artikel [gebruik Azure AD-verificatie voor toegang tot de Azure Media Services API met REST](media-services-rest-connect-with-aad.md)

## <a name="considerations"></a>Overwegingen

De volgende overwegingen zijn van toepassing wanneer u met behulp van REST.

* Bij het opvragen van entiteiten, is er een limiet van 1000 entiteiten in één keer wordt geretourneerd omdat openbare REST v2 queryresultaten tot 1000 resultaten beperkt. U moet gebruiken **overslaan** en **nemen** (.NET) / **boven** (REST) zoals beschreven in [in dit voorbeeld .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) en [deze REST-API voorbeeld](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 
* Wanneer met behulp van JSON en op te geven voor het gebruik van de **__metadata** sleutelwoord in de aanvraag (bijvoorbeeld, om te verwijzen naar een gekoppeld object) stelt u de **accepteren** koptekst [uitgebreide JSON-indeling](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/)(Zie het volgende voorbeeld). OData niet begrijpt de **__metadata** eigenschap in de aanvraag, tenzij u dit op uitgebreide instellen.  
  
        POST https://media.windows.net/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.17
        Authorization: Bearer <ENCODED JWT TOKEN> 
        Host: media.windows.net
  
        {
            "Name" : "NewTestJob", 
            "InputMediaAssets" : 
                [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aba5356eb-30ff-4dc6-9e5a-41e4223540e7')"}}]
        . . . 

## <a name="standard-http-request-headers-supported-by-media-services"></a>Standaard HTTP-aanvraagheaders ondersteund door Media Services
Er is een set van vereiste headers die u in uw aanvraag opnemen moet voor elke aanroep die u in Media Services aanbrengt, en ook een set met optionele koppen u wilt opnemen. De volgende tabel bevat de vereiste headers:

| Header | Type | Value |
| --- | --- | --- |
| Autorisatie |Bearer |Bearer is de enige toegestane autorisatiemechanismen. De waarde moet ook het toegangstoken dat door Azure Active Directory. |
| x-ms-version |Decimal |2.17 (of de meest recente versie)|
| DataServiceVersion |Decimal |3.0 |
| MaxDataServiceVersion |Decimal |3.0 |

> [!NOTE]
> Omdat Media Services maakt gebruik van OData om beschikbaar te stellen van de REST-API's, moeten de DataServiceVersion en MaxDataServiceVersion headers worden opgenomen in alle aanvragen; echter als dat niet het geval is, klikt u vervolgens op dit moment Media Services wordt ervan uitgegaan dat de waarde DataServiceVersion in gebruik is 3.0.
> 
> 

Hier volgt een aantal optionele headers:

| Header | Type | Value |
| --- | --- | --- |
| Date |RFC 1123 datum |Tijdstempel van de aanvraag |
| Accepteren |Inhoudstype |Het aangevraagde type inhoud voor het antwoord, zoals het volgende:<p> -application/json;odata=verbose<p> -application/atom + xml<p> Antwoorden mogelijk een andere type inhoud, zoals een blob ophalen, waarbij een geslaagd antwoord bevat de stroom blob als de nettolading. |
| Accept-Encoding |Gzip, deflate |GZIP en DEFLATE-codering, indien van toepassing. Opmerking: Voor grote resources, Media Services deze header negeren en niet-gecomprimeerde gegevens retourneren. |
| Accept-Language |"en", "es", enzovoort. |Hiermee geeft u de gewenste taal voor het antwoord. |
| Accept-Charset |Tekenset type, zoals "UTF-8" |Standaard wordt UTF-8. |
| X-HTTP-methode |HTTP-methode |Hiermee kunnen clients of firewalls die geen ondersteuning voor HTTP-methoden, zoals opslag of verwijderen voor het gebruik van deze methoden, tunnel via een GET-aanroep. |
| Content-Type |Inhoudstype |Type inhoud van de hoofdtekst van de aanvraag in de PUT- of POST-aanvragen. |
| client-request-id |String |Een aanroeper gedefinieerde waarde waarmee de aanvraag. Als u opgeeft, wordt deze waarde in het antwoordbericht worden opgenomen als een manier om toe te wijzen de aanvraag. <p><p>**Belangrijk**<p>Waarden moeten worden beperkt tot 2096b (2k). |

## <a name="standard-http-response-headers-supported-by-media-services"></a>Standaard-HTTP-antwoordheaders ondersteund door Media Services
Hier volgt een set van headers die kunnen worden geretourneerd aan u, afhankelijk van de resource die u zijn aangevraagd en de actie die u bedoeld om uit te voeren.

| Header | Type | Value |
| --- | --- | --- |
| request-id |String |Een unieke id voor de huidige bewerking, service gegenereerd. |
| client-request-id |String |Een id die is opgegeven door de oproepende functie in de oorspronkelijke aanvraag, indien aanwezig. |
| Date |RFC 1123 datum |De datum/tijd die de aanvraag is verwerkt. |
| Content-Type |Varieert |Het inhoudstype van de antwoordtekst. |
| Content-Encoding |Varieert |Gzip of verkleinen, indien van toepassing. |

## <a name="standard-http-verbs-supported-by-media-services"></a>Standaard HTTP-termen die worden ondersteund door Media Services
Hier volgt een volledige lijst met HTTP-termen die kunnen worden gebruikt bij het maken van HTTP-aanvragen:

| term | Description |
| --- | --- |
| GET |Retourneert de huidige waarde van een object. |
| POST |Maakt een object op basis van de gegevens die is opgegeven of een opdracht verzonden. |
| PUT |Een object vervangt of maakt u een benoemd object (indien van toepassing). |
| DELETE |Hiermee verwijdert u een object. |
| SAMENVOEGEN |Een bestaand object bijgewerkt met wijzigingen in de benoemde eigenschappen. |
| HEAD |Retourneert de metagegevens van een object voor een GET-antwoord. |

## <a name="discover-and-browse-the-media-services-entity-model"></a>Detecteren en door de Media Services-entiteitsmodel bladeren
Als u Media Services-entiteiten meer kan worden gedetecteerd, kan de bewerking $metadata worden gebruikt. Hiermee kunt u om op te halen van alle geldige Entiteitstypen, entiteitseigenschappen, koppelingen, functies, acties, enzovoort. De bewerking $metadata toevoegt aan het einde van uw Media Services REST API-eindpunt, kunt u toegang tot deze service voor de detectie.

 /api/$metadata.

U moet toevoegen '? api-version=2.x ' aan het einde van de URI als u wilt weergeven van de metagegevens in een browser of de header x-ms-version niet in uw aanvraag opnemen.

## <a name="authenticate-with-media-services-rest-using-azure-active-directory"></a>Verifiëren met Media Services REST met behulp van Azure Active Directory

Verificatie op de REST-API wordt gedaan door Azure Active Directory(AAD).
Zie voor meer informatie over het verkrijgen van vereiste verificatiegegevens voor uw Media Services-account vanuit Azure Portal [toegang tot de API van Azure Media Services met Azure AD-verificatie](media-services-use-aad-auth-to-access-ams-api.md).

Zie voor meer informatie over het schrijven van code die verbinding maakt met de REST-API met behulp van Azure AD-verificatie, het artikel [gebruik Azure AD-verificatie voor toegang tot de Azure Media Services API met REST](media-services-rest-connect-with-aad.md).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het gebruik van Azure AD-verificatie met Media Services REST API, [gebruik Azure AD-verificatie voor toegang tot de Azure Media Services API met REST](media-services-rest-connect-with-aad.md).

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]


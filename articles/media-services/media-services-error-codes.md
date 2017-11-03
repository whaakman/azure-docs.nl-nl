---
title: Azure Media Services-foutcodes | Microsoft Docs
description: Het onderwerp overzicht een van Azure Media Services-foutcodes.
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: d3a62a64-7608-4b17-8667-479b26ba0d6c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: 39886a955124429302609dd9d5a7c20ae7f498d9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-media-services-error-codes"></a>Azure Media Services-foutcodes
Wanneer u Microsoft Azure Media Services gebruikt, krijgt u HTTP-foutcodes van de service, afhankelijk van de problemen zoals verificatietokens verloopt naar acties die worden niet ondersteund in Media Services. Hieronder volgt een lijst met **HTTP-foutcodes** die door Media Services en de mogelijke oorzaken voor hen kunnen worden geretourneerd.  

## <a name="400-bad-request"></a>400 onjuiste aanvraag
De aanvraag bevat ongeldige gegevens en is geweigerd vanwege een van de volgende redenen:

* Een niet-ondersteunde API-versie is opgegeven. Zie voor de meest recente versie [Setup voor het ontwikkelen van Media Services REST API](media-services-rest-how-to-use.md).
* De API-versie van Media Services is niet opgegeven. Zie voor meer informatie over het opgeven van de API-versie [Media Services Operations REST API-verwijzing](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).
  
  > [!NOTE]
  > Als u de .NET of Java SDK's voor verbinding met Media Services gebruikt, wordt de API-versie is opgegeven voor u wanneer u probeert en uitvoeren van een bepaalde actie op basis van Media Services.
  > 
  > 
* Een ongedefinieerde eigenschap er is opgegeven. Naam van de eigenschap is in het foutbericht. Alleen de eigenschappen die deel uitmaken van een bepaalde entiteit kunnen worden opgegeven. Zie [Azure Media Services REST API-verwijzing](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference) voor een lijst met entiteiten en hun eigenschappen.
* Een ongeldige waarde er is opgegeven. Naam van de eigenschap is in het foutbericht. Zie de vorige koppeling voor geldige eigenschaptypen en hun waarden.
* De waarde van een eigenschap ontbreekt en is vereist.
* Onderdeel van de opgegeven URL bevat een ongeldige waarde.
* Een poging gedaan een eigenschap WriteOnce bij te werken.
* Een poging gedaan om te maken van een taak met een invoer Asset met een primaire AssetFile die is niet opgegeven of kan niet worden bepaald.
* Bijwerken van een SAS-Locator is een poging gedaan. SAS-locators kunnen alleen worden gemaakt of verwijderd. Streaming-locators kan worden bijgewerkt. Zie voor meer informatie [Locators](https://docs.microsoft.com/rest/api/media/operations/locator).
* Een niet-ondersteunde bewerking of de query is ingediend.

## <a name="401-unauthorized"></a>401-niet toegestaan
De aanvraag kan niet worden geverifieerd (voordat deze kan worden geautoriseerd) om een van de volgende redenen:

* Ontbrekende verificatieheader.
* Ongeldige verificatie header-waarde.
  * Het token is verlopen. 
  * Het token bevat een ongeldige handtekening.

## <a name="403-forbidden"></a>403-verboden
De aanvraag is niet toegestaan vanwege een van de volgende redenen:

* De Media Services-account kan niet worden gevonden of is verwijderd.
* De Media Services-account is uitgeschakeld en het aanvraagtype is geen HTTP GET. Servicebewerkingen wordt ook een 403 antwoord geretourneerd.
* Het verificatietoken bevat geen gegevens van de referentie van de gebruiker: AccountName en/of abonnements-id. U vindt deze informatie in de gebruikersinterface van Media Services-uitbreiding voor uw Media Services-account in de Azure-beheerportal.
* De resource kan niet worden geopend.
  
  * Een poging gedaan een MediaProcessor die niet beschikbaar voor uw Media Services-account gebruiken.
  * Een poging gedaan een taaksjabloon gedefinieerd door Media Services bijwerken.
  * Een poging gedaan om te overschrijven sommige andere Media Services-account van Locator.
  * Een poging gedaan om te overschrijven sommige andere Media Services-account van ContentKey.
* De resource kan niet worden gemaakt als gevolg van een service-quota die voor de Media Services-account is bereikt. Zie voor meer informatie over de servicequota [quota's en beperkingen](media-services-quotas-and-limitations.md).

## <a name="404-not-found"></a>404 – Niet gevonden
De aanvraag is niet toegestaan voor een bron om een van de volgende redenen:

* Een poging gedaan om bij te werken van een entiteit die niet bestaat.
* Een poging gedaan om te verwijderen van een entiteit die niet bestaat.
* Een poging gedaan om te maken van een entiteit die is gekoppeld aan een entiteit die niet bestaat.
* Een poging gedaan om op te HALEN van een entiteit die niet bestaat.
* Een poging gedaan om op te geven van een opslagaccount dat is niet gekoppeld aan het Media Services-account.  

## <a name="409-conflict"></a>409 conflict
De aanvraag is niet toegestaan vanwege een van de volgende redenen:

* Meer dan één AssetFile heeft de opgegeven naam binnen de Asset.
* Een poging gedaan om te maken van een tweede primaire AssetFile binnen de Asset.
* Een poging gedaan een ContentKey maken met de opgegeven Id is al gebruikt.
* Een poging gedaan een Locator te maken met de opgegeven Id is al gebruikt.
* Meer dan één IngestManifestFile heeft de opgegeven naam binnen de IngestManifest.
* Een poging gedaan een tweede opslagversleuteling ContentKey koppelen aan de Asset opslag versleuteld.
* Een poging gedaan de dezelfde ContentKey koppelen aan de Asset.
* Een poging gedaan een locator om een Asset waarvan storage-container ontbreekt of is niet meer gekoppeld aan de Asset te maken.
* Een poging gedaan een locator om een Asset die al in gebruik 5 locators is te maken. (Azure-opslag zorgt ervoor dat de limiet van vijf gedeeld toegangsbeleid op één opslagcontainer.)
* Storage-account van een activum koppelen aan een IngestManifestAsset is niet hetzelfde als het opslagaccount dat wordt gebruikt door de bovenliggende IngestManifest.  

## <a name="500-internal-server-error"></a>Interne serverfout 500
Media Services aangetroffen tijdens het verwerken van de aanvraag, er is een fout die voorkomt de verwerking niet door kan gaan dat. Dit kan een van de volgende oorzaken hebben:

* Maken van een activum of de taak mislukt, omdat de servicegegevens quotum van het Media Services-account tijdelijk niet beschikbaar is.
* Maken van een activum of IngestManifest blob storage-container mislukt, omdat de accountgegevens voor opslag van het account is tijdelijk niet beschikbaar.
* Andere onverwachte fout opgetreden.

## <a name="503-service-unavailable"></a>503 Service niet beschikbaar
De server is momenteel niet mogelijk om aanvragen te ontvangen. Deze fout kan worden veroorzaakt door overmatige aanvragen naar de service. Media Services beperking mechanisme Hiermee beperkt u het gebruik van bronnen voor toepassingen die overmatige naar de service indienen.

> [!NOTE]
> Controleer het foutbericht en fouttekenreeks code om meer gedetailleerde informatie over de reden dat u hebt de 503-fout. Deze fout betekent niet altijd beperking.
> 
> 

Beschrijvingen van de mogelijke status zijn:

* 'De server is bezet. Eerder uitgevoerde van dit soort aanvraag duurde langer dan {0} seconden."
* 'De server is bezet. Meer dan {0} aanvragen per seconde kan worden beperkt."
* 'De server is bezet. Meer dan {0} aanvragen binnen enkele seconden {1} kunnen worden beperkt."

Voor het afhandelen van deze fout, wordt u aangeraden Pogingslogica voor exponentiële back uitschakelen. Dit betekent dat met behulp van geleidelijk langer wacht tussen nieuwe pogingen voor opeenvolgende foutberichten.  Zie voor meer informatie [tijdelijke fout afhandeling van Application Block](https://msdn.microsoft.com/library/hh680905.aspx).

> [!NOTE]
> Als u [Azure Media Services SDK voor .net](https://github.com/Azure/azure-sdk-for-media-services/tree/master), de Pogingslogica voor de 503-fout is geïmplementeerd door de SDK.  
> 
> 

## <a name="see-also"></a>Zie ook
[Media Services-Management-foutcodes](http://msdn.microsoft.com/library/windowsazure/dn167016.aspx)

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


---
title: Azure Media Services-foutcodes | Microsoft Docs
description: Het onderwerp biedt een overzicht van Azure Media Services-foutcodes.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: d3a62a64-7608-4b17-8667-479b26ba0d6c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: d700423ea966cab3b0f546d21d4d2f5ca5208971
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55990510"
---
# <a name="azure-media-services-error-codes"></a>Azure Media Services-foutcodes
Als u Microsoft Azure Media Services gebruikt, krijgt u mogelijk HTTP-foutcodes van de service, afhankelijk van de problemen zoals verificatietokens naar acties die worden niet ondersteund in Media Services is verlopen. Hieronder volgt een lijst met **HTTP-foutcodes** die door Media Services en de mogelijke oorzaken voor deze kunnen worden geretourneerd.  

## <a name="400-bad-request"></a>400-Ongeldige aanvraag
De aanvraag bevat ongeldige gegevens en wordt geweigerd vanwege een van de volgende redenen:

* Een niet-ondersteunde API-versie is opgegeven. Zie voor de meest recente versie [instellen voor het ontwikkelen van Media Services REST API](media-services-rest-how-to-use.md).
* De API-versie van Media Services is niet opgegeven. Zie voor meer informatie over het opgeven van de API-versie [Media Services Operations REST API-verwijzing](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).
  
  > [!NOTE]
  > Als u de .NET of Java-SDK's gebruikt voor verbinding met Media Services, wordt de API-versie is opgegeven voor u wanneer u probeert en een actie op basis van Media Services.
  > 
  > 
* Een niet-gedefinieerde eigenschap er is opgegeven. Naam van de eigenschap is in het foutbericht. Alleen de eigenschappen die lid van een bepaalde entiteit zijn kunnen worden opgegeven. Zie [Azure Media Services REST API-verwijzing](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference) voor een lijst met entiteiten en hun eigenschappen.
* Een ongeldige waarde er is opgegeven. Naam van de eigenschap is in het foutbericht. Zie de vorige koppeling voor geldige eigenschaptypen en hun waarden.
* Waarde van een eigenschap ontbreekt en is vereist.
* Onderdeel van de opgegeven URL bevat een ongeldige waarde.
* Er is geprobeerd een eigenschap WriteOnce bij te werken.
* Er is geprobeerd om te maken van een taak met een invoer Asset met een primaire AssetFile die is niet opgegeven of kan niet worden vastgesteld.
* Er is geprobeerd om bij te werken van een SAS-Locator. SAS-locators kunnen alleen worden gemaakt of verwijderd. Streaming-locators kan worden bijgewerkt. Zie voor meer informatie, [Locators](https://docs.microsoft.com/rest/api/media/operations/locator).
* Een niet-ondersteunde bewerking of de query is ingediend.

## <a name="401-unauthorized"></a>401-niet toegestaan
De aanvraag kan niet worden geverifieerd (voordat deze kan worden geautoriseerd) een van de volgende oorzaken hebben:

* Ontbrekende header voor verificatie.
* Onjuiste verificatie header-waarde.
  * Het token is verlopen. 
  * Het token bevat een ongeldige handtekening.

## <a name="403-forbidden"></a>403-verboden
De aanvraag is niet toegestaan vanwege een van de volgende redenen:

* De Media Services-account kan niet worden gevonden of is verwijderd.
* De Media Services-account is uitgeschakeld en het aanvraagtype is geen HTTP GET. Servicebewerkingen wordt ook een 403 antwoord geretourneerd.
* Het verificatietoken bevat referentie-informatie van de gebruiker: AccountName en/of abonnements-id. U kunt deze informatie vinden in de gebruikersinterface van Media Services-extensie voor uw Media Services-account in de Azure Management Portal.
* De resource kan niet worden geopend.
  
  * Er is geprobeerd een MediaProcessor die is niet beschikbaar voor uw Media Services-account gebruiken.
  * Er is geprobeerd een taaksjabloon gedefinieerd door Media Services bijwerken.
  * Er is geprobeerd om te overschrijven sommige andere Media Services-account van Locator.
  * Er is geprobeerd op sommige andere Media Services-account van ContentKey overschrijven.
* De resource kan niet worden gemaakt vanwege een servicequotum die voor de Media Services-account is bereikt. Zie voor meer informatie over de servicequota [quota en beperkingen](media-services-quotas-and-limitations.md).

## <a name="404-not-found"></a>404 – Niet gevonden
De aanvraag is niet toegestaan voor een resource vanwege een van de volgende redenen:

* Er is geprobeerd om bij te werken van een entiteit die niet bestaat.
* Er is geprobeerd op een entiteit verwijdert die niet bestaat.
* Er is geprobeerd een entiteit maken die is gekoppeld aan een entiteit die niet bestaat.
* Er is geprobeerd om op te HALEN van een entiteit die niet bestaat.
* Er is geprobeerd om op te geven van een opslagaccount dat is niet gekoppeld aan de Media Services-account.  

## <a name="409-conflict"></a>409 conflict
De aanvraag is niet toegestaan vanwege een van de volgende redenen:

* Meer dan één AssetFile heeft de naam van de opgegeven in de Asset.
* Er is geprobeerd om te maken van een tweede primaire AssetFile in de Asset.
* Er is geprobeerd een ContentKey maken met de opgegeven Id al gebruikt.
* Er is geprobeerd een Locator te maken met de opgegeven Id al gebruikt.
* Meer dan één IngestManifestFile heeft de opgegeven naam binnen de IngestManifest.
* Er is geprobeerd een tweede opslagversleuteling ContentKey koppelen aan de Asset opslag versleuteld.
* Er is geprobeerd de dezelfde ContentKey koppelen aan de Asset.
* Er is geprobeerd een locator om een Asset waarvan opslagcontainer ontbreekt of is niet meer gekoppeld aan de Asset te maken.
* Er is geprobeerd een locator om een Asset die al in gebruik 5 locators is te maken. (Azure Storage wordt de limiet van vijf beleid voor gedeelde toegang op een storage-container afgedwongen.)
* Storage-account van een Asset koppelen aan een IngestManifestAsset is niet hetzelfde als het opslagaccount dat wordt gebruikt door de bovenliggende IngestManifest.  

## <a name="500-internal-server-error"></a>500 Interne serverfout
Media Services aangetroffen tijdens de verwerking van de aanvraag een fout waarmee wordt voorkomen de verwerking van u doorgaat dat. Dit kan een van de volgende oorzaken hebben:

* Het maken van een Asset of een taak mislukt, omdat de servicegegevens quotum van het Media Services-account tijdelijk niet beschikbaar is.
* Het maken van een Asset of IngestManifest blob storage-container mislukt, omdat de accountgegevens voor opslag van het account is tijdelijk niet beschikbaar.
* Andere onverwachte fout.

## <a name="503-service-unavailable"></a>503 Service niet beschikbaar
De server is momenteel niet om aanvragen te ontvangen. Deze fout kan worden veroorzaakt door overmatige aanvragen voor de service. Media Services beperking mechanisme Hiermee beperkt u het Resourcegebruik voor toepassingen die overmatige bij de service indienen.

> [!NOTE]
> Controleer het foutbericht en de tekenreeks voor de foutcode voor meer gedetailleerde informatie over de reden je de 503-fout. Deze fout betekent niet altijd beperking.
> 
> 

Beschrijvingen van de mogelijke status zijn:

* "De server is bezet. Vorige uitvoeringen van dit type aanvraag heeft meer dan {0} seconden. "
* "De server is bezet. Meer dan {0} aanvragen per seconde kunnen worden beperkt. "
* "De server is bezet. Meer dan {0} vraagt binnen {1} seconden kunnen worden beperkt. "

Voor het afhandelen van deze fout, wordt u aangeraden Pogingslogica voor exponentieel uitstel. Dit betekent dat met behulp van geleidelijk langere wachttijd tussen nieuwe pogingen voor opeenvolgende foutberichten.  Zie voor meer informatie, [verwerken Toepassingsblok fouten](https://msdn.microsoft.com/library/hh680905.aspx).

> [!NOTE]
> Als u [Azure Media Services SDK voor .net](https://github.com/Azure/azure-sdk-for-media-services/tree/master), de logica voor opnieuw proberen voor de 503-fout is geïmplementeerd door de SDK.  
> 
> 

## <a name="see-also"></a>Zie ook
[Media Services-Management-foutcodes](https://msdn.microsoft.com/library/windowsazure/dn167016.aspx)

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]


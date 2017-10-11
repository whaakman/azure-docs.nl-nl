---
title: Documentatie voor Azure API Management-beleid
description: Meer informatie over het beleid dat beschikbaar is voor het configureren van API Management.
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 9d4ac609-b221-4032-93ae-e27a1254d43d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: adc0c4415e10ddd0b4994cecef17f026546e91a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-api-management-policy-reference"></a>Documentatie voor Azure API Management-beleid
Deze sectie bevat een index voor het beleid in de [documentatie voor API Management-beleid][API Management policy reference]. Zie voor meer informatie over het toevoegen en configureren van beleid [-beleid in API Management][Policies in API Management].

Beleidsexpressies kunnen worden gebruikt als kenmerkwaarden of tekstwaarden in API Management-beleidsregels, tenzij het beleid iets anders aangeeft. Sommige beleidsregels, zoals de [transportbesturing] [ Control flow] en [variabele instellen] [ Set variable] beleid is gebaseerd op beleidsexpressies. Zie voor meer informatie [Geavanceerde beleidsregels] [ Advanced policies] en [beleidsexpressies][Policy expressions]

## <a name="policy-reference-index"></a>Index referentie naar beleid
* [Softwarerestrictiebeleid toegang][Access restriction policies]
  * [Controle van HTTP-header] [ Check HTTP header] -afdwingt bestaan en/of de waarde van een HTTP-Header.
  * [Aanroepfrequentie per abonnement] [ Limit call rate by subscription] -API wordt voorkomen dat gebruik wordt bereikt door het beperken van de aanroepfrequentie per abonnement op basis van een.
  * [Aanroepfrequentie per sleutel](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) -API wordt voorkomen dat gebruik wordt bereikt door het beperken van de aanroepfrequentie per sleutel op basis van een.
  * [Aanroeper IP-adressen beperken] [ Restrict caller IPs] -Filters (kunt/weigert) aanroepen van bepaalde IP-adressen en/of -adresbereiken.
  * [Gebruiksquotum per abonnement instellen] [ Set usage quota by subscription] -Hiermee kunt u een vernieuwd of levensduur aanroep volume en/of bandbreedte quotum, op basis van per abonnement afdwingen.
  * [Gebruiksquotum instellen door sleutel](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) -Hiermee kunt u een vernieuwd of levensduur aanroep volume en/of bandbreedte quotum, op basis van de sleutel per afdwingen.
  * [Valideren van JWT] [ Validate JWT] -afdwingt bestaan en de geldigheid van een JWT opgehaald uit een opgegeven HTTP-koptekst of een opgegeven queryparameter.
* [Geavanceerde beleidsregels][Advanced policies]
  * [Transportbesturing] [ Control flow] - voorwaardelijk is van toepassing op basis van de resultaten van de evaluatie van Booleaanse waarde beleidsverklaringen [expressies][expressions].
  * [Doorsturen van aanvraag] [ Forward request] -stuurt de aanvraag naar de back-endservice.
  * [Logboek naar Event Hub] [ Log to Event Hub] -verzendt berichten in de opgegeven indeling voor een berichtdoel gedefinieerd door een [berichtenlogboek](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) entiteit.
  * [Probeer](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) -uitvoering van de ingesloten beleidsverklaringen pogingen als en pas de voorwaarde wordt voldaan. Uitvoering wordt herhaald op de opgegeven tijdsintervallen en tot het opgegeven aantal nieuwe pogingen.
  * [Antwoord retourneren](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) -pipeline-uitvoering wordt afgebroken en retourneert het opgegeven antwoord rechtstreeks naar de aanroeper.
  * [Eenzijdige aanvraag verzenden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) -verzendt een aanvraag naar de opgegeven URL zonder te wachten op reactie.
  * [Aanvraag verzenden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) -verzendt een aanvraag naar de opgegeven URL.
  * [Stel aanvraagmethode](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) -kunt u de HTTP-methode voor een aanvraag wijzigen.
  * [Status instellen](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) -wijzigingen van de HTTP-statuscode in de opgegeven waarde.
  * [Variabele instellen] [ Set variable] -persistent maken van een waarde in een benoemde [context] [ context] variabele voor latere toegang.
  * [Tracering](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) -voegt een tekenreeks in de [API Inspector](api-management-howto-api-inspector.md) uitvoer.
  * [Wacht](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) -wacht ingesloten verzenden aanvragen, Get-waarde van de cache, of stroom toegangsbeheerbeleid voltooid voordat u doorgaat.
* [Verificatiebeleid][Authentication policies]
  * [Verificatie met Basic] [ Authenticate with Basic] -verificatie met een back-endservice basisverificatie wordt gebruikt.
  * [Verificatie met een clientcertificaat] [ Authenticate with client certificate] -verificatie met een back-endservice met behulp van clientcertificaten.
* [Cachebeleidsregels][Caching policies] 
  * [Ophalen uit de cache] [ Get from cache] -cache uitvoeren opzoeken en retourneren een geldige cacheantwoord indien beschikbaar.
  * [Store aan cache] [ Store to cache] -antwoord volgens de configuratie opgegeven cache opslaat in de cache.
  * [Waarde niet ophalen uit de cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) -ophalen van een item in de cache per sleutel.
  * [Waarde opslaan in cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) -opslaan van een item in de cache per sleutel.
  * [Waarde verwijderen uit de cache](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) -verwijderen van een item in de cache per sleutel.
* [Cross-domeinbeleid][Cross domain policies] 
  * [Aanroepen tussen domeinen toestaan] [ Allow cross-domain calls] -de API van Adobe Flash en Microsoft Silverlight op browser gebaseerde clients toegankelijk maakt.
  * [CORS] [ CORS] -cross-origin-resource delen (CORS) ondersteuning voor een bewerking of een API om toe te staan tussen domeinen aanroepen van clients op basis van een browser wordt toegevoegd.
  * [JSONP] [ JSONP] -JSON wordt toegevoegd met ondersteuning voor een bewerking of een API om toe te staan tussen domeinen aanroepen vanuit JavaScript-browser gebaseerde clients opvulling (JSONP).
* [Claimtransformatiebeleidsinstellingen][Transformation policies] 
  * [JSON converteren naar XML] [ Convert JSON to XML] : zet aanvraag of antwoord body van JSON naar XML.
  * [XML niet converteren naar JSON] [ Convert XML to JSON] : zet aanvraag of antwoord body van XML naar JSON.
  * [Zoeken en vervangen tekenreeks in de hoofdtekst] [ Find and replace string in body] - zoeken naar een subtekenreeks aanvraag of antwoord en wordt vervangen door een andere subtekenreeks.
  * [URL's in de inhoud te maskeren] [ Mask URLs in content] -koppelingen herschrijft (maskers) in het antwoord body zodat deze naar de equivalente koppeling via de gateway verwijzen.
  * [Stel de back-endservice] [ Set backend service] -wijzigingen van de back-endservice voor de binnenkomende aanvraag.
  * [Instantie ingesteld] [ Set body] -Hiermee stelt u de berichttekst voor binnenkomende en uitgaande aanvragen.
  * [Set HTTP-header] [ Set HTTP header] : een waarde wordt toegewezen aan een bestaande antwoord en/of de aanvraag-header of voegt een nieuwe antwoord en/of de aanvraag-header.
  * [Querytekenreeksparameter ingesteld] [ Set query string parameter] - wordt toegevoegd, vervangt de waarde van of verwijderd van de aanvraag queryreeksparameter opgeven.
  * [Herschrijven van URL's] [ Rewrite URL] -converteert van een aanvraag-URL van de openbare vorm aan het formulier dat werd verwacht door de webservice.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende video voor meer informatie over beleidsexpressies.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

[Access restriction policies]: https://msdn.microsoft.com/library/azure/dn894078.aspx
[Check HTTP header]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader
[Limit call rate by subscription]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate
[Restrict caller IPs]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs
[Set usage quota by subscription]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota
[Validate JWT]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx
[context]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables
[Forward request]: https://msdn.microsoft.com/library/azure/dn894085.aspx#ForwardRequest
[Log to Event Hub]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[Authentication policies]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[Authenticate with Basic]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[Authenticate with client certificate]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[Get from cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[Store to cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[Cross domain policies]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[Allow cross-domain calls]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[Transformation policies]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[Convert JSON to XML]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[Convert XML to JSON]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
[Find and replace string in body]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody
[Mask URLs in content]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent
[Set backend service]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService
[Set body]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody
[Set HTTP header]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader
[Set query string parameter]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter
[Rewrite URL]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL



[Policies in API Management]: api-management-howto-policies.md
[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx

[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx



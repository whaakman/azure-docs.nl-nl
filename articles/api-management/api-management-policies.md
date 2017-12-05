---
title: Azure API Management-beleidsregels | Microsoft Docs
description: Meer informatie over het beschikbaar voor gebruik in Azure API Management-beleid.
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: d0d10096c004b50688ad5e6550bf248ceb5ef878
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="api-management-policies"></a>API Management-beleidsregels
Deze sectie bevat een verwijzing voor de volgende API Management-beleidsregels. Zie voor meer informatie over het toevoegen en configureren van beleid [-beleid in API Management](api-management-howto-policies.md).  
  
 Beleidsregels zijn een krachtige mogelijkheid van het systeem waarmee de uitgever het gedrag van de API via configuratie wijzigen. Beleidsregels zijn een verzameling instructies die sequentieel worden uitgevoerd op de aanvraag of antwoord van een API. Populaire instructies omvatten Indelingsconversie van XML in JSON en snelheidsbeperking als u wilt beperken, de hoeveelheid inkomende aanroepen van een ontwikkelaar aanroepen. Veel meer beleidsregels zijn gebruiksklaar beschikbaar.  
  
 Beleidsexpressies kunnen worden gebruikt als kenmerkwaarden of tekstwaarden in API Management-beleidsregels, tenzij het beleid iets anders aangeeft. Sommige beleidsregels, zoals de beleidsregels [Stroom controleren](api-management-advanced-policies.md#choose) en [Variabele instellen](api-management-advanced-policies.md#set-variable), zijn gebaseerd op beleidsexpressies. Zie voor meer informatie [Geavanceerde beleidsregels](api-management-advanced-policies.md#AdvancedPolicies) en [beleidsexpressies](api-management-policy-expressions.md).  
  
##  <a name="ProxyPolicies"></a>Beleid  
  
-   [Beleid voor toegangsbeperking](api-management-access-restriction-policies.md#AccessRestrictionPolicies)  
    -   [Controle van HTTP-header](api-management-access-restriction-policies.md#CheckHTTPHeader) -afdwingt bestaan en/of de waarde van een HTTP-Header.  
    -   [Aanroepfrequentie per abonnement](api-management-access-restriction-policies.md#LimitCallRate) -API wordt voorkomen dat gebruik wordt bereikt door het beperken van de aanroepfrequentie per abonnement op basis van een.  
    -   [Aanroepfrequentie per sleutel](api-management-access-restriction-policies.md#LimitCallRateByKey) -API wordt voorkomen dat gebruik wordt bereikt door het beperken van de aanroepfrequentie per sleutel op basis van een.  
    -   [Aanroeper IP-adressen beperken](api-management-access-restriction-policies.md#RestrictCallerIPs) -Filters (kunt/weigert) aanroepen van bepaalde IP-adressen en/of -adresbereiken.  
    -   [Gebruiksquotum per abonnement instellen](api-management-access-restriction-policies.md#SetUsageQuota) -Hiermee kunt u een vernieuwd of levensduur aanroep volume en/of bandbreedte quotum, op basis van per abonnement afdwingen.  
    -   [Gebruiksquotum instellen door sleutel](api-management-access-restriction-policies.md#SetUsageQuotaByKey) -Hiermee kunt u een vernieuwd of levensduur aanroep volume en/of bandbreedte quotum, op basis van de sleutel per afdwingen.  
    -   [Valideren van JWT](api-management-access-restriction-policies.md#ValidateJWT) -afdwingt bestaan en de geldigheid van een JWT opgehaald uit een opgegeven HTTP-koptekst of een opgegeven queryparameter.  
-   [Geavanceerde beleidsregels](api-management-advanced-policies.md#AdvancedPolicies)  
    -   [Transportbesturing](api-management-advanced-policies.md#choose) - voorwaardelijk beleidsverklaringen op basis van de evaluatie van Booleaanse expressies van toepassing is.  
    -   [Doorsturen van aanvraag](api-management-advanced-policies.md#ForwardRequest) -stuurt de aanvraag naar de back-endservice.  
    -   [Logboek naar Event Hub](api-management-advanced-policies.md#log-to-eventhub) -berichten in de opgegeven indeling voor een berichtdoel gedefinieerd door een entiteit berichtenlogboek verzendt.  
    -   [Probeer](api-management-advanced-policies.md#Retry) -uitvoering van de ingesloten beleidsverklaringen pogingen als en pas de voorwaarde wordt voldaan. Uitvoering wordt herhaald op de opgegeven tijdsintervallen en tot het opgegeven aantal nieuwe pogingen.  
    -   [Antwoord retourneren](api-management-advanced-policies.md#ReturnResponse) -pipeline-uitvoering wordt afgebroken en retourneert het opgegeven antwoord rechtstreeks naar de aanroeper.  
    -   [Eenzijdige aanvraag verzenden](api-management-advanced-policies.md#SendOneWayRequest) -verzendt een aanvraag naar de opgegeven URL zonder te wachten op reactie.  
    -   [Aanvraag verzenden](api-management-advanced-policies.md#SendRequest) -verzendt een aanvraag naar de opgegeven URL.  
    -   [Variabele instellen](api-management-advanced-policies.md#set-variable) -persistent maken van een waarde in een variabele met de naam context voor later toegang.  
    -   [Stel aanvraagmethode](api-management-advanced-policies.md#SetRequestMethod) -kunt u de HTTP-methode voor een aanvraag wijzigen.  
    -   [Statuscode ingesteld](api-management-advanced-policies.md#SetStatus) -wijzigingen van de HTTP-statuscode in de opgegeven waarde.  
    -   [Tracering](api-management-advanced-policies.md#Trace) -voegt een tekenreeks in de [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) uitvoer.  
    -   [Wacht](api-management-advanced-policies.md#Wait) -wacht voor ingesloten [Verzendaanvraag](api-management-advanced-policies.md#SendRequest), [waarde niet ophalen uit de cache](api-management-caching-policies.md#GetFromCacheByKey), of [transportbesturing](api-management-advanced-policies.md#choose) beleid om te voltooien voordat u doorgaat.  
-   [Verificatiebeleid](api-management-authentication-policies.md#AuthenticationPolicies)  
    -   [Verificatie met Basic](api-management-authentication-policies.md#Basic) -verificatie met een back-endservice basisverificatie wordt gebruikt.  
    -   [Verificatie met een clientcertificaat](api-management-authentication-policies.md#ClientCertificate) -verificatie met een back-endservice met behulp van clientcertificaten.  
-   [Cachebeleidsregels](api-management-caching-policies.md#CachingPolicies)  
    -   [Ophalen uit de cache](api-management-caching-policies.md#GetFromCache) -cache uitvoeren opzoeken en retourneren een geldige cacheantwoord indien beschikbaar.  
    -   [Store aan cache](api-management-caching-policies.md#StoreToCache) -antwoord volgens de configuratie opgegeven cache opslaat in de cache.  
    -   [Waarde niet ophalen uit de cache](api-management-caching-policies.md#GetFromCacheByKey) -ophalen van een item in de cache per sleutel.  
    -   [Waarde opslaan in cache](api-management-caching-policies.md#StoreToCacheByKey) -opslaan van een item in de cache per sleutel.  
    -   [Waarde verwijderen uit de cache](api-management-caching-policies.md#RemoveCacheByKey) -verwijderen van een item in de cache per sleutel.  
-   [Beleid voor meerdere domeinen](api-management-cross-domain-policies.md#CrossDomainPolicies)  
    -   [Aanroepen tussen domeinen toestaan](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -de API van Adobe Flash en Microsoft Silverlight op browser gebaseerde clients toegankelijk maakt.  
    -   [CORS](api-management-cross-domain-policies.md#CORS) -cross-origin-resource delen (CORS) ondersteuning voor een bewerking of een API om toe te staan tussen domeinen aanroepen van clients op basis van een browser wordt toegevoegd.  
    -   [JSONP](api-management-cross-domain-policies.md#JSONP) -JSON wordt toegevoegd met ondersteuning voor een bewerking of een API om toe te staan tussen domeinen aanroepen vanuit JavaScript-browser gebaseerde clients opvulling (JSONP).  
-   [Transformatiebeleid](api-management-transformation-policies.md#TransformationPolicies)  
    -   [JSON converteren naar XML](api-management-transformation-policies.md#ConvertJSONtoXML) : zet aanvraag of antwoord body van JSON naar XML.  
    -   [XML niet converteren naar JSON](api-management-transformation-policies.md#ConvertXMLtoJSON) : zet aanvraag of antwoord body van XML naar JSON.  
    -   [Zoeken en vervangen tekenreeks in de hoofdtekst](api-management-transformation-policies.md#Findandreplacestringinbody) - zoeken naar een subtekenreeks aanvraag of antwoord en wordt vervangen door een andere subtekenreeks.  
    -   [URL's in de inhoud te maskeren](api-management-transformation-policies.md#MaskURLSContent) -koppelingen herschrijft (maskers) in het antwoord body zodat deze naar de equivalente koppeling via de gateway verwijzen.  
    -   [Stel de back-endservice](api-management-transformation-policies.md#SetBackendService) -wijzigingen van de back-endservice voor de binnenkomende aanvraag.  
    -   [Instantie ingesteld](api-management-transformation-policies.md#SetBody) -Hiermee stelt u de berichttekst voor binnenkomende en uitgaande aanvragen.  
    -   [Set HTTP-header](api-management-transformation-policies.md#SetHTTPheader) : een waarde wordt toegewezen aan een bestaande antwoord en/of de aanvraag-header of voegt een nieuwe antwoord en/of de aanvraag-header.  
    -   [Querytekenreeksparameter ingesteld](api-management-transformation-policies.md#SetQueryStringParameter) - wordt toegevoegd, vervangt de waarde van of verwijderd van de aanvraag queryreeksparameter opgeven.  
    -   [Herschrijven van URL](api-management-transformation-policies.md#RewriteURL) -converteert van een aanvraag-URL van de openbare vorm aan het formulier dat werd verwacht door de webservice.  
    -   [XML met behulp van een XSLT transformeren](api-management-transformation-policies.md#XSLTransform) -geldt een XSL-transformatie voor XML in de hoofdtekst van de aanvraag of antwoord.  

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie, werken met beleid:

+ [Beleidsregels in API Management](api-management-howto-policies.md)
+ [Transformeren API 's](transform-api.md)
+ [Naslaginformatie over beleid](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en hun instellingen
+ [Voorbeelden van beleid](policy-samples.md)   

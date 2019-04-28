---
title: Azure API Management-beleid | Microsoft Docs
description: Meer informatie over het beschikbare beleid voor gebruik in Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 711f6395959b3718ca0a094a03c07c4381892104
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/24/2019
ms.locfileid: "63762858"
---
# <a name="api-management-policies"></a>API Management-beleidsregels
Deze sectie bevat een verwijzing voor de volgende API Management-beleidsregels. Zie voor meer informatie over het toevoegen en configureren van beleid [beleidsregels in API Management](api-management-howto-policies.md).  
  
 Beleidsregels zijn een krachtige mogelijkheid van het systeem waarmee de uitgever het gedrag van de API via configuratie wijzigen. Beleidsregels zijn een verzameling instructies die sequentieel worden uitgevoerd op de aanvraag of antwoord van een API. Populaire instructies omvatten Indelingsconversie van XML in JSON en aanroepfrequentielimiet-als u wilt beperken van de hoeveelheid inkomende aanroepen van een ontwikkelaar te beperken. Veel meer beleidsregels zijn gebruiksklaar beschikbaar.  
  
 Beleidsexpressies kunnen worden gebruikt als kenmerkwaarden of tekstwaarden in API Management-beleidsregels, tenzij het beleid iets anders aangeeft. Sommige beleidsregels, zoals de beleidsregels [Stroom controleren](api-management-advanced-policies.md#choose) en [Variabele instellen](api-management-advanced-policies.md#set-variable), zijn gebaseerd op beleidsexpressies. Zie [Geavanceerde beleidsregels](api-management-advanced-policies.md#AdvancedPolicies) en [Beleidsexpressies](api-management-policy-expressions.md) voor meer informatie.  
  
##  <a name="ProxyPolicies"></a> Beleid  
  
-   [Beleid voor toegangsbeperking](api-management-access-restriction-policies.md#AccessRestrictionPolicies)  
    -   [Selectievakje HTTP-header](api-management-access-restriction-policies.md#CheckHTTPHeader) -afdwingt bestaan en/of de waarde van een HTTP-Header.  
    -   [Aanroepfrequentie per abonnement beperken](api-management-access-restriction-policies.md#LimitCallRate) -gebruik van de API wordt voorkomen dat de pieken door aanroepfrequentie per abonnement beperken.  
    -   [Aanroepfrequentie beperken door sleutel](api-management-access-restriction-policies.md#LimitCallRateByKey) -gebruik van de API wordt voorkomen dat de pieken door aanroepfrequentie, op basis van per sleutel beperken.  
    -   [Aanroeper IP-adressen beperken](api-management-access-restriction-policies.md#RestrictCallerIPs) -Filters (kunt/geweigerd) aanroepen van specifieke IP-adressen en/of -adresbereiken.  
    -   [Gebruiksquotum per abonnement instellen](api-management-access-restriction-policies.md#SetUsageQuota) -Hiermee kunt u een vernieuwd of levensduur aanroep volume en/of bandbreedte quotum, op basis van abonnement afdwingen.  
    -   [Gebruiksquotum instellen op sleutel](api-management-access-restriction-policies.md#SetUsageQuotaByKey) -Hiermee kunt u een vernieuwd of levensduur aanroep volume en/of bandbreedte quotum, op basis van per sleutel afdwingen.  
    -   [Valideren van JWT](api-management-access-restriction-policies.md#ValidateJWT) -afdwingt bestaan en de geldigheid van een JWT geëxtraheerd uit een opgegeven HTTP-Header of een opgegeven query-parameter.  
-   [Geavanceerde beleidsregels](api-management-advanced-policies.md#AdvancedPolicies)  
    -   [Controlestroom](api-management-advanced-policies.md#choose) - voorwaardelijk beleid overzichten op basis van de evaluatie van Booleaanse expressies is van toepassing.  
    -   [Doorsturen van aanvraag](api-management-advanced-policies.md#ForwardRequest) -stuurt de aanvraag naar de back-endservice.  
    -   [Bij Log to Eventhub](api-management-advanced-policies.md#log-to-eventhub) -berichten in de opgegeven indeling verzendt naar een berichtdoel is gedefinieerd door een Logger-entiteit.  
    -   [Probeer](api-management-advanced-policies.md#Retry) -uitvoering van de ingesloten beleidsverklaringen pogingen als en totdat de voorwaarde wordt voldaan. Uitvoering wordt herhaald op de opgegeven tijdsintervallen en tot het opgegeven aantal nieuwe pogingen.  
    -   [Antwoord retourneren](api-management-advanced-policies.md#ReturnResponse) -pipeline-uitvoering wordt afgebroken en retourneert het opgegeven antwoord rechtstreeks naar de oproepende functie.  
    -   [Verzenden van One-Way-request](api-management-advanced-policies.md#SendOneWayRequest) -zendt een verzoek naar de opgegeven URL zonder te wachten op een reactie.  
    -   [Aanvraag verzenden](api-management-advanced-policies.md#SendRequest) -zendt een verzoek naar de opgegeven URL.  
    -   [Variabele instellen](api-management-advanced-policies.md#set-variable) -een waarde in een contextvariabele met de naam voor later toegang behouden.  
    -   [Aanvraagmethode ingesteld](api-management-advanced-policies.md#SetRequestMethod) -kunt u de HTTP-methode voor een aanvraag wijzigen.  
    -   [Instellen van de statuscode](api-management-advanced-policies.md#SetStatus) -verandert de HTTP-statuscode in de opgegeven waarde.  
    -   [Tracering](api-management-advanced-policies.md#Trace) -voegt een tekenreeks in de [API Inspector](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) uitvoer.  
    -   [Wacht](api-management-advanced-policies.md#Wait) -wacht voor ingesloten [Verzendaanvraag](api-management-advanced-policies.md#SendRequest), [waarde niet ophalen uit de cache](api-management-caching-policies.md#GetFromCacheByKey), of [controlestroom](api-management-advanced-policies.md#choose) beleid om te voltooien voordat u doorgaat.  
-   [Verificatiebeleid](api-management-authentication-policies.md#AuthenticationPolicies)  
    -   [Verifiëren met Basic](api-management-authentication-policies.md#Basic) -verificatie met een back-endservice basisverificatie wordt gebruikt.  
    -   [Verifiëren met clientcertificaat](api-management-authentication-policies.md#ClientCertificate) -verificatie met een back endservice met behulp van clientcertificaten.  
    -   [Verifiëren met beheerde identiteit](api-management-authentication-policies.md#ManagedIdentity) -verificatie met een back-end-service met een [beheerde identiteit](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).  
-   [Cachebeleidsregels](api-management-caching-policies.md#CachingPolicies)  
    -   [Ophalen uit de cache](api-management-caching-policies.md#GetFromCache) -cache uitvoeren opzoeken en retourneren een geldige in de cache opgeslagen reactie indien beschikbaar.  
    -   [Store cache](api-management-caching-policies.md#StoreToCache) -antwoord op basis van de opgegeven cache-control-configuratie in de cache opslaat.  
    -   [Waarde niet ophalen uit de cache](api-management-caching-policies.md#GetFromCacheByKey) -sleutel een item in de cache ophalen.  
    -   [Waarde in de cache Store](api-management-caching-policies.md#StoreToCacheByKey) -Store van een item in de cache op sleutel.  
    -   [Waarde verwijderen uit de cache](api-management-caching-policies.md#RemoveCacheByKey) -verwijderen van een item in de cache op sleutel.  
-   [Beleid voor meerdere domeinen](api-management-cross-domain-policies.md#CrossDomainPolicies)  
    -   [Toestaan van aanroepen tussen domeinen](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -de API van Adobe Flash en Microsoft Silverlight-browser-gebaseerde clients toegankelijk maakt.  
    -   [CORS](api-management-cross-domain-policies.md#CORS) -cross-origin resource sharing (CORS) ondersteuning voor een bewerking of een API voor het toestaan van aanroepen tussen domeinen van clients op basis van een browser wordt toegevoegd.  
    -   [JSONP](api-management-cross-domain-policies.md#JSONP) -JSON met ondersteuning voor de opvulling (JSONP) naar een bewerking of een API voor het toestaan van aanroepen tussen domeinen van JavaScript-clients op basis van een browser wordt toegevoegd.  
-   [Transformatiebeleid](api-management-transformation-policies.md#TransformationPolicies)  
    -   [JSON converteren naar XML](api-management-transformation-policies.md#ConvertJSONtoXML) : zet aanvraag of antwoord body van JSON naar XML.  
    -   [XML converteren naar JSON](api-management-transformation-policies.md#ConvertXMLtoJSON) : zet aanvraag of antwoord body van XML naar JSON.  
    -   [Zoeken en vervangen in de hoofdtekst van de tekenreeks](api-management-transformation-policies.md#Findandreplacestringinbody) - vindt u een aanvraag of antwoord subtekenreeks en vervangt deze door een andere subtekenreeks.  
    -   [URL's in de inhoud te maskeren](api-management-transformation-policies.md#MaskURLSContent) -herschrijft (maskers) koppelingen in het antwoord hoofdtekst zodat ze naar de equivalente koppeling via de gateway verwijzen.  
    -   [Instellen van back-endservice](api-management-transformation-policies.md#SetBackendService) -de back endservice voor een binnenkomende aanvraag wordt gewijzigd.  
    -   [Hoofdtekst instellen](api-management-transformation-policies.md#SetBody) -Hiermee stelt u de berichttekst voor binnenkomende en uitgaande aanvragen.  
    -   [Set HTTP-header](api-management-transformation-policies.md#SetHTTPheader) : een waarde toegewezen aan een bestaande antwoord en/of de aanvraagheader of voegt een nieuwe reactie en/of de aanvraag-header.  
    -   [Instellen van de queryreeks-parameter](api-management-transformation-policies.md#SetQueryStringParameter) - wordt toegevoegd, vervangt de waarde van of aanvraag queryreeks-parameter verwijderd.  
    -   [Herschrijven van URL's](api-management-transformation-policies.md#RewriteURL) -converteert van een aanvraag-URL van de openbare vorm aan het formulier werd verwacht door de webservice.  
    -   [Transformeer XML met behulp van een XSLT](api-management-transformation-policies.md#XSLTransform) -een XSL-transformatie is van toepassing op XML in de hoofdtekst van de aanvraag of antwoord.  



## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie werken met beleidsregels voor:

+ [Beleid in API Management](api-management-howto-policies.md)
+ [API's transformeren](transform-api.md)
+ [Naslaginformatie over beleid voor](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en de bijbehorende instellingen
+ [Voorbeelden van beleid](policy-samples.md)   

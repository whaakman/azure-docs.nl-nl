---
title: Azure API Management-beleidsexpressies | Microsoft Docs
description: Meer informatie over beleidsexpressies in Azure API Management.
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: ea160028-fc04-4782-aa26-4b8329df3448
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: fb50ba3f292a390c45f1afe6259731d2b92cc335
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="api-management-policy-expressions"></a>Expressies voor API Management-beleid
De syntaxis van de beleid-expressies is C# 6.0. Elke expressie toegang heeft tot de opgegeven impliciet [context](api-management-policy-expressions.md#ContextVariables) variabele en een toegestane [subset](api-management-policy-expressions.md#CLRTypes) van .NET Framework-typen.  
  
> [!TIP]
>  Zie voor meer informatie over beleidsexpressies de [Beleidsexpressies](https://azure.microsoft.com/documentation/videos/policy-expressions-in-azure-api-management/) video.  
>   
>  Zie voor demonstraties van het configureren van beleidsregels met behulp van beleidsexpressies [Cloud hebben betrekking op aflevering 177: meer API-beheerfuncties met Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). In deze video bevat de volgende expressie demonstraties voor beleid:  
>   
>  -   10:30 - Zie contextinformatie met uw back endservice op te geven. Gebruik de [querytekenreeksparameter ingesteld](api-management-transformation-policies.md#SetQueryStringParameter) en [ingesteld HTTP-header](api-management-transformation-policies.md#SetHTTPheader) beleid om deze informatie te geven. Bij 12:10 is er een demo voor het aanroepen van een bewerking in de portal voor ontwikkelaars, waar u deze beleidsregels op kantoor kunt zien.  
> -   Informatie over het gebruiken van 13:50 - de [JWT valideren](api-management-access-restriction-policies.md#ValidateJWT) beleid vooraf toegang verlenen aan bewerkingen op basis van claims token. Snel door te sturen naar 15:00 om te zien hoe de beleidsregels zijn geconfigureerd in de beleidseditor. Zie een demonstratie van een bewerking aanroepen vanuit de ontwikkelaarsportal zowel met en zonder de vereiste verificatietoken op 18:50.  
> -   21:00 - gebruik een [API Inspector](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) trace om te zien hoe beleid wordt geëvalueerd en de resultaten van deze evaluatie.  
> -   25:25 - informatie over het gebruiken van expressies met de [ophalen uit de cache](api-management-caching-policies.md#GetFromCache) en [Store aan cache](api-management-caching-policies.md#StoreToCache) beleid om caching van API Management-antwoord te configureren. Stel een duur die overeenkomt met het antwoord in cache plaatsen van de back-endservice zoals opgegeven door de back-service `Cache-Control` richtlijn.  
> -   34:30 - informatie over het uitvoeren van webinhoud te filteren. Gegevenselementen verwijderen uit het antwoord van de back-end via de [transportbesturing](api-management-advanced-policies.md#choose) en [instantie ingesteld](api-management-transformation-policies.md#SetBody) beleid. Start op 31:50 voor een overzicht van [de donker Sky Forecast API](https://developer.forecast.io/) gebruikt voor deze demonstratie.  
> -   Zie voor het downloaden van de beleid-instructies in deze video gebruikt de [api-management-voorbeelden/beleid](https://github.com/Azure/api-management-samples/tree/master/policies) github-opslagplaats.  
  
  
##  <a name="Syntax"></a>Syntaxis  
 Één instructie expressies zijn ingesloten in `@(expression)`, waarbij `expression` is een goed ingedeelde C#-expressie-instructie.  
  
 Meerdere instructies expressies zijn ingesloten in `@{expression}`. Alle codepaden binnen meerdere instructies expressies moeten eindigen met een `return` instructie.  
  
##  <a name="PolicyExpressionsExamples"></a>Voorbeelden  
  
```  
@(true)  
  
@((1+1).ToString())  
  
@("Hi There".Length)  
  
@(Regex.Match(context.Response.Headers.GetValueOrDefault("Cache-Control",""), @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value)  
  
@(context.Variables.ContainsKey("maxAge") ? int.Parse((string)context.Variables["maxAge"]) : 3600)  
  
@{   
  string value;   
  if (context.Request.Headers.TryGetValue("Authorization", out value))   
  {   
    return Encoding.UTF8.GetString(Convert.FromBase64String(value));  
  }   
  else   
  {   
    return null;  
  }  
}  
```  
  
##  <a name="PolicyExpressionsUsage"></a>Gebruik  
 Expressies kunnen worden gebruikt als kenmerkwaarden of tekstwaarden in een API Management [beleid](api-management-policies.md) (tenzij de naslaginformatie over beleid iets anders aangeeft).  
  
> [!IMPORTANT]
>  Wanneer u beleidsexpressies gebruikt, is dit alleen beperkt verificatie van de beleidsexpressies als het beleid is gedefinieerd. Expressies worden uitgevoerd door de gateway tijdens runtime, eventuele uitzonderingen die worden gegenereerd door beleid expressies resulteren in een runtime-fout.  
  
##  <a name="CLRTypes"></a>.NET framework-typen die zijn toegestaan in beleidsexpressies voor  
 De volgende tabel bevat de typen .NET Framework en hun leden die zijn toegestaan in beleidsexpressies voor.  
  
|CLR-type|Ondersteunde methoden|  
|--------------|-----------------------|  
|Newtonsoft.Json.Linq.Extensions|Alle methoden worden ondersteund|  
|Newtonsoft.Json.Linq.JArray|Alle methoden worden ondersteund|  
|Newtonsoft.Json.Linq.JConstructor|Alle methoden worden ondersteund|  
|Newtonsoft.Json.Linq.JContainer|Alle methoden worden ondersteund|  
|Newtonsoft.Json.Linq.JObject|Alle methoden worden ondersteund|  
|Newtonsoft.Json.Linq.JProperty|Alle methoden worden ondersteund|  
|Newtonsoft.Json.Linq.JRaw|Alle methoden worden ondersteund|  
|Newtonsoft.Json.Linq.JToken|Alle methoden worden ondersteund|  
|Newtonsoft.Json.Linq.JTokenType|Alle methoden worden ondersteund|  
|Newtonsoft.Json.Linq.JValue|Alle methoden worden ondersteund|  
|System.Collections.Generic.IReadOnlyCollection<T\>|Alle|  
|System.Collections.Generic.IReadOnlyDictionary<TKey,  TValue>|Alle|  
|System.Collections.Generic.ISet<TKey, TValue>|Alle|  
|System.Collections.Generic.KeyValuePair<TKey,  TValue>|Sleutel, waarde|  
|System.Collections.Generic.List<TKey, TValue>|Alle|  
|System.Collections.Generic.Queue<TKey, TValue>|Alle|  
|System.Collections.Generic.Stack<TKey, TValue>|Alle|  
|System.Convert|Alle|  
|System.DateTime|Alle|  
|System.DateTimeKind|UTC|  
|System.DateTimeOffset|Alle|  
|System.Decimal|Alle|  
|System.Double|Alle|  
|System.Guid|Alle|  
|System.IEnumerable < T\>|Alle|  
|System.IEnumerator<T\>|Alle|  
|System.Int16|Alle|  
|System.Int32|Alle|  
|System.Int64|Alle|  
|System.Linq.Enumerable<T\>|Alle methoden worden ondersteund|  
|System.Math|Alle|  
|System.MidpointRounding|Alle|  
|System.Nullable<T\>|Alle|  
|System.Random|Alle|  
|System.SByte|Alle|  
|System.Security.Cryptography. HMACSHA384|Alle|  
|System.Security.Cryptography. HMACSHA512|Alle|  
|System.Security.Cryptography.HashAlgorithm|Alle|  
|System.Security.Cryptography.HMAC|Alle|  
|System.Security.Cryptography.HMACMD5|Alle|  
|System.Security.Cryptography.HMACSHA1|Alle|  
|System.Security.Cryptography.HMACSHA256|Alle|  
|System.Security.Cryptography.KeyedHashAlgorithm|Alle|  
|System.Security.Cryptography.MD5|Alle|  
|System.Security.Cryptography.RNGCryptoServiceProvider|Alle|  
|System.Security.Cryptography.SHA1|Alle|  
|System.Security.Cryptography.SHA1Managed|Alle|  
|System.Security.Cryptography.SHA256|Alle|  
|System.Security.Cryptography.SHA256Managed|Alle|  
|System.Security.Cryptography.SHA384|Alle|  
|System.Security.Cryptography.SHA384Managed|Alle|  
|System.Security.Cryptography.SHA512|Alle|  
|System.Security.Cryptography.SHA512Managed|Alle|  
|System.Single|Alle|  
|System.String|Alle|  
|System.StringSplitOptions|Alle|  
|System.Text.Encoding|Alle|  
|System.Text.RegularExpressions.Capture|De waarde voor index, lengte,|  
|System.Text.RegularExpressions.CaptureCollection|Count, Item|  
|System.Text.RegularExpressions.Group|Opnamen, geslaagd|  
|System.Text.RegularExpressions.GroupCollection|Count, Item|  
|System.Text.RegularExpressions.Match|Leeg, groepen, resultaat|  
|System.Text.RegularExpressions.Regex|(Constructor) IsMatch, overeen, komt overeen met, vervangen|  
|System.Text.RegularExpressions.RegexOptions|Gecompileerd IgnoreCase, IgnorePatternWhitespace, Multiline, None, RightToLeft, Singleline|  
|System.TimeSpan|Alle|  
|System.Tuple|Alle|  
|System.UInt16|Alle|  
|System.UInt32|Alle|  
|System.UInt64|Alle|  
|System.Uri|Alle|  
|System.Xml.Linq.Extensions|Alle methoden worden ondersteund|  
|System.Xml.Linq.XAttribute|Alle methoden worden ondersteund|  
|System.Xml.Linq.XCData|Alle methoden worden ondersteund|  
|System.Xml.Linq.XComment|Alle methoden worden ondersteund|  
|System.Xml.Linq.XContainer|Alle methoden worden ondersteund|  
|System.Xml.Linq.XDeclaration|Alle methoden worden ondersteund|  
|System.Xml.Linq.XDocument|Alle methoden worden ondersteund|  
|System.Xml.Linq.XDocumentType|Alle methoden worden ondersteund|  
|System.Xml.Linq.XElement|Alle methoden worden ondersteund|  
|System.Xml.Linq.XName|Alle methoden worden ondersteund|  
|System.Xml.Linq.XNamespace|Alle methoden worden ondersteund|  
|System.Xml.Linq.XNode|Alle methoden worden ondersteund|  
|System.Xml.Linq.XNodeDocumentOrderComparer|Alle methoden worden ondersteund|  
|System.Xml.Linq.XNodeEqualityComparer|Alle methoden worden ondersteund|  
|System.Xml.Linq.XObject|Alle methoden worden ondersteund|  
|System.Xml.Linq.XProcessingInstruction|Alle methoden worden ondersteund|  
|System.Xml.Linq.XText|Alle methoden worden ondersteund|  
|System.Xml.XmlNodeType|Alle|  
  
##  <a name="ContextVariables"></a>Context variabele  
 Een variabele met de naam `context` impliciet beschikbaar is in elk beleid [expressie](api-management-policy-expressions.md#Syntax). De leden bevatten informatie die relevant zijn voor de `\request`. Alle van de `context` leden zijn alleen-lezen.  
  
|Context variabele|Methoden, eigenschappen en parameterwaarden toegestaan|  
|----------------------|-------------------------------------------------------|  
|Context|Api: IApi<br /><br /> Implementatie<br /><br /> Verstreken: TimeSpan - tijdsinterval tussen de waarde van het tijdstempel en het huidige tijdstip<br /><br /> LastError<br /><br /> Bewerking<br /><br /> Product<br /><br /> Aanvraag<br /><br /> Aanvraag-id: Guid - unieke aanvraag-id<br /><br /> Antwoord<br /><br /> Abonnement<br /><br /> Tijdstempel: DateTime - punt in tijd waarop de aanvraag is ontvangen<br /><br /> Tracering: Boole - geeft aan of tracering in- of uitschakelen <br /><br /> Gebruiker<br /><br /> Variabelen: IReadOnlyDictionary < tekenreeks, object ><br /><br /> VOID Trace(message: string)|  
|context.Api|ID: tekenreeks<br /><br /> IsRevisionCurrent: bool<br /><br />  Naam: tekenreeks<br /><br /> Pad: tekenreeks<br /><br /> Revisie: tekenreeks<br /><br /> ServiceUrl: IUrl<br /><br /> Versie: tekenreeks |  
|context. Implementatie|Regio: tekenreeks<br /><br /> Servicenaam: tekenreeks<br /><br /> Certificates: IReadOnlyDictionary<string, X509Certificate2>|  
|context.LastError|Bron: tekenreeks<br /><br /> Reden: tekenreeks<br /><br /> Bericht: tekenreeks<br /><br /> Bereik: tekenreeks<br /><br /> Sectie: tekenreeks<br /><br /> Pad: tekenreeks<br /><br /> PolicyId: tekenreeks<br /><br /> Voor meer informatie over de context. LastError, Zie [foutafhandeling](api-management-error-handling-policies.md).|  
|context. Bewerking|ID: tekenreeks<br /><br /> Methode: tekenreeks<br /><br /> Naam: tekenreeks<br /><br /> UrlTemplate: tekenreeks|  
|context.Product|API's: IEnumerable < IApi\><br /><br /> ApprovalRequired: bool<br /><br /> Groepen: IEnumerable < IGroup\><br /><br /> ID: tekenreeks<br /><br /> Naam: tekenreeks<br /><br /> Status: enum ProductState {NotPublished, gepubliceerde}<br /><br /> SubscriptionLimit: int?<br /><br /> SubscriptionRequired: bool|  
|context. Aanvraag|Hoofdtekst: IMessageBody<br /><br /> Certificate: System.Security.Cryptography.X509Certificates.X509Certificate2<br /><br /> Headers: IReadOnlyDictionary < string, string [] ><br /><br /> IP-adres: tekenreeks<br /><br /> MatchedParameters: IReadOnlyDictionary < tekenreeks, tekenreeks ><br /><br /> Methode: tekenreeks<br /><br /> OriginalUrl:IUrl<br /><br /> URL: IUrl|  
|tekenreekscontext. Request.Headers.GetValueOrDefault (HeaderName is opgeslagen: string, defaultValue: tekenreeks)|HeaderName is opgeslagen: tekenreeks<br /><br /> Standaardwaarde: tekenreeks<br /><br /> Retourneert de door komma's gescheiden aanvraag met de headerwaarden of `defaultValue` als de header is niet gevonden.|  
|context. Antwoord|Hoofdtekst: IMessageBody<br /><br /> Headers: IReadOnlyDictionary < string, string [] ><br /><br /> StatusCode: int<br /><br /> StatusReason: tekenreeks|  
|tekenreekscontext. Response.Headers.GetValueOrDefault (HeaderName is opgeslagen: string, defaultValue: tekenreeks)|HeaderName is opgeslagen: tekenreeks<br /><br /> Standaardwaarde: tekenreeks<br /><br /> Door komma's gescheiden antwoord geretourneerd met de headerwaarden of `defaultValue` als de header is niet gevonden.|  
|context.Subscription|CreatedTime: datum/tijd<br /><br /> EndDate: DateTime?<br /><br /> ID: tekenreeks<br /><br /> Sleutel: tekenreeks<br /><br /> Naam: tekenreeks<br /><br /> PrimaryKey: tekenreeks<br /><br /> Secundaire sleutel: tekenreeks<br /><br /> StartDate: DateTime?|  
|context.User|E-mailadres: tekenreeks<br /><br /> Voornaam: tekenreeks<br /><br /> Groepen: IEnumerable < IGroup\><br /><br /> ID: tekenreeks<br /><br /> Identiteiten: IEnumerable < IUserIdentity\><br /><br /> Achternaam: tekenreeks<br /><br /> Opmerking: tekenreeks<br /><br /> RegistrationDate: datum/tijd|  
|IApi|ID: tekenreeks<br /><br /> Naam: tekenreeks<br /><br /> Pad: tekenreeks<br /><br /> Protocollen: IEnumerable < tekenreeks\><br /><br /> ServiceUrl: IUrl<br /><br /> SubscriptionKeyParameterNames: ISubscriptionKeyParameterNames|  
|IGroup|ID: tekenreeks<br /><br /> Naam: tekenreeks|  
|IMessageBody|Als < T\>(preserveContent: bool = false): waar T: tekenreeks, JObject, JToken, JArray, XNode, XElement, XDocument<br /><br /> De `context.Request.Body.As<T>` en `context.Response.Body.As<T>` methoden worden gebruikt voor het lezen van een aanvraag en -antwoord de berichttekst in een bepaald type `T`. Standaard wordt de methode maakt gebruik van de oorspronkelijke instantie berichtenstroom en maakt het niet beschikbaar is nadat deze retourneert. Om te voorkomen dat door de methode op een kopie van de hoofdstroom werkt, stel de `preserveContent` -parameter voor `true`. Ga [hier](api-management-transformation-policies.md#SetBody) voor een voorbeeld.|  
|IUrl|Host: tekenreeks<br /><br /> Pad: tekenreeks<br /><br /> Poort: int<br /><br /> Query: IReadOnlyDictionary < string, string [] ><br /><br /> QueryString: tekenreeks<br /><br /> Schema: tekenreeks|  
|IUserIdentity|ID: tekenreeks<br /><br /> Provider: tekenreeks|  
|ISubscriptionKeyParameterNames|Koptekst: tekenreeks<br /><br /> Query: tekenreeks|  
|tekenreeks IUrl.Query.GetValueOrDefault (queryParameterName: string, defaultValue: tekenreeks)|queryParameterName: tekenreeks<br /><br /> Standaardwaarde: tekenreeks<br /><br /> Retourneert de door komma's gescheiden parameterwaarden of `defaultValue` als de parameter is niet gevonden.|  
|T-context. Variables.GetValueOrDefault < T\>(variabelenaam: string, defaultValue: T)|Variabelenaam: tekenreeks<br /><br /> Standaardwaarde: T<br /><br /> Retourneert de waarde van variabele geconverteerd naar type `T` of `defaultValue` als de variabele is niet gevonden.<br /><br /> Deze methode genereert een uitzondering als het opgegeven type komt niet overeen met het werkelijke type van de geretourneerde variabele.|  
|BasicAuthCredentials AsBasic(input: this string)|invoer: tekenreeks<br /><br /> Als de invoerparameter een geldige HTTP basisverificatie autorisatie aanvraag-header-waarde bevat, de methode retourneert een object van het type `BasicAuthCredentials`; anders is de methode retourneert null.|  
|BOOL TryParseBasic (invoer: deze tekenreeks, resultaat: uit BasicAuthCredentials)|invoer: tekenreeks<br /><br /> resultaat: uit BasicAuthCredentials<br /><br /> Als de invoerparameter een geldige HTTP-basisverificatie autorisatiewaarde in de aanvraagheader bevat de methode retourneert `true` en de resultatenparameter bevat een waarde van het type `BasicAuthCredentials`; anders is de methode retourneert `false`.|  
|BasicAuthCredentials|Wachtwoord: tekenreeks<br /><br /> Gebruikers-id: tekenreeks|  
|Jwt AsJwt(input: this string)|invoer: tekenreeks<br /><br /> Als de invoerparameter geen geldige waarde van de JWT-token bevat, de methode retourneert een object van het type `Jwt`; anders is de methode retourneert `null`.|  
|BOOL TryParseJwt (invoer: deze tekenreeks, resultaat: uit Jwt)|invoer: tekenreeks<br /><br /> resultaat: uit Jwt<br /><br /> De methode retourneert als de invoerparameter een geldige waarde van de JWT-token bevat, `true` en de resultatenparameter bevat een waarde van het type `Jwt`; anders is de methode retourneert `false`.|  
|Jwt|Algoritme: tekenreeks<br /><br /> Doelgroep: IEnumerable < tekenreeks\><br /><br /> Claims: IReadOnlyDictionary < string, string [] ><br /><br /> ExpirationTime: DateTime?<br /><br /> ID: tekenreeks<br /><br /> Uitgever: tekenreeks<br /><br /> NotBefore: DateTime?<br /><br /> Onderwerpnaam: tekenreeks<br /><br /> Type: tekenreeks|  
|tekenreeks Jwt.Claims.GetValueOrDefault (claimName: string, defaultValue: tekenreeks)|claimName: tekenreeks<br /><br /> Standaardwaarde: tekenreeks<br /><br /> Retourneert door komma's gescheiden claimwaarden of `defaultValue` als de header is niet gevonden.|
|byte [] coderen (invoer: deze byte [], alg: string, sleutel: byte [], iv:byte[])|invoer - tekst zonder opmaak worden versleuteld<br /><br />Alg - naam van een symmetrische versleutelingsalgoritme<br /><br />sleutel - versleuteling<br /><br />IV - initialisatievector<br /><br />Retourneert tekst zonder opmaak versleuteld.|
|byte [] coderen (invoer: deze byte [], alg: System.Security.Cryptography.SymmetricAlgorithm)|invoer - tekst zonder opmaak worden versleuteld<br /><br />Alg - versleutelingsalgoritme<br /><br />Retourneert tekst zonder opmaak versleuteld.|
|byte [] coderen (invoer: deze byte [], alg: System.Security.Cryptography.SymmetricAlgorithm, sleutel: byte [], iv:byte[])|invoer - tekst zonder opmaak worden versleuteld<br /><br />Alg - versleutelingsalgoritme<br /><br />sleutel - versleuteling<br /><br />IV - initialisatievector<br /><br />Retourneert tekst zonder opmaak versleuteld.|
|byte [] ontsleutelen (invoer: deze byte [], alg: string, sleutel: byte [], iv:byte[])|invoer - ineens tekst die moet worden gedecodeerd<br /><br />Alg - naam van een symmetrische versleutelingsalgoritme<br /><br />sleutel - versleuteling<br /><br />IV - initialisatievector<br /><br />Retourneert tekst zonder opmaak.|
|byte [] ontsleutelen (invoer: deze byte [], alg: System.Security.Cryptography.SymmetricAlgorithm)|invoer - ineens tekst die moet worden gedecodeerd<br /><br />Alg - versleutelingsalgoritme<br /><br />Retourneert tekst zonder opmaak.|
|byte [] ontsleutelen (invoer: deze byte [], alg: System.Security.Cryptography.SymmetricAlgorithm, sleutel: byte [], iv:byte[])|invoer - invoer - ineens tekst die moet worden gedecodeerd<br /><br />Alg - versleutelingsalgoritme<br /><br />sleutel - versleuteling<br /><br />IV - initialisatievector<br /><br />Retourneert tekst zonder opmaak.|

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player] 
>
## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie, werken met beleid:

+ [Beleidsregels in API Management](api-management-howto-policies.md)
+ [Transformeren API 's](transform-api.md)
+ [Naslaginformatie over beleid](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en hun instellingen
+ [Voorbeelden van beleid](policy-samples.md)   

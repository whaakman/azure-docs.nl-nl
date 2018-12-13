---
title: Azure API Management-beleidsexpressies | Microsoft Docs
description: Meer informatie over beleidsexpressies in Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: ea160028-fc04-4782-aa26-4b8329df3448
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: 8cfaacad4619bb06536d41e72b9da1eb9c160dc2
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53163902"
---
# <a name="api-management-policy-expressions"></a>API Management-beleidsexpressies
Dit artikel worden besproken beleidsexpressies syntaxis is C# 7. Elke expressie heeft toegang tot de opgegeven impliciet [context](api-management-policy-expressions.md#ContextVariables) variabele en een toegestane [subset](api-management-policy-expressions.md#CLRTypes) van .NET Framework-typen.

Voor meer informatie:

- Bekijk hoe u contextgegevens naar uw back-endservice te verstrekken. Gebruik de [queryreeks-parameter ingesteld](api-management-transformation-policies.md#SetQueryStringParameter) en [ingesteld HTTP-header](api-management-transformation-policies.md#SetHTTPheader) beleid voor deze informatie verstrekken.
- Informatie over het gebruik de [JWT valideren](api-management-access-restriction-policies.md#ValidateJWT) beleid voor vooraf machtigen toegang tot bewerkingen op basis van claims token.
- Zie hoe u een [API Inspector](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) tracering om te zien hoe beleidsregels worden geëvalueerd en de resultaten van deze evaluatie.
- Informatie over het gebruik van expressies met de [ophalen uit de cache](api-management-caching-policies.md#GetFromCache) en [Store cache](api-management-caching-policies.md#StoreToCache) beleid voor het configureren van API Management antwoorden in cache opslaan. Stel een duur die overeenkomt met de antwoorden in cache opslaan van de back-endservice zoals opgegeven door de back-ups service `Cache-Control` richtlijn.
- Zie hoe u kunt uitvoeren met het filteren van inhoud. Gegevenselementen verwijderen uit het antwoord heeft ontvangen van de back-end via de [controlestroom](api-management-advanced-policies.md#choose) en [hoofdtekst instellen](api-management-transformation-policies.md#SetBody) beleid.
- Zie voor het downloaden van de instructies voor het [api-beheer-samples/beleid](https://github.com/Azure/api-management-samples/tree/master/policies) GitHub-opslagplaats.
  
  
## <a name="Syntax"></a> Syntaxis
Één instructie-expressies zijn ingesloten in `@(expression)`, waarbij `expression` is een goed ingedeelde C# expressie-instructie.
  
Meerdere instructies expressies zijn ingesloten in `@{expression}`. Alle codepaden binnen meerdere instructies expressies moeten eindigen met een `return` instructie.
  
## <a name="PolicyExpressionsExamples"></a> Voorbeelden
  
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
  
## <a name="PolicyExpressionsUsage"></a>Gebruik
Expressies kunnen worden gebruikt als kenmerkwaarden of tekstwaarden in een API Management [beleid](api-management-policies.md) (tenzij de naslaginformatie over beleid iets anders aangeeft).
  
> [!IMPORTANT]
> Wanneer u beleidsexpressies gebruikt, is er alleen beperkte controle van de beleidsexpressies wanneer het beleid is gedefinieerd. Expressies worden uitgevoerd door de gateway tijdens runtime, eventuele uitzonderingen die worden gegenereerd door beleid expressies resulteren in een runtime-fout.
  
## <a name="CLRTypes"></a> .NET framework-typen die zijn toegestaan in beleidsexpressies voor
De volgende tabel bevat de typen .NET Framework en de leden die zijn toegestaan in beleidsexpressies voor.
  
|CLR-type|Ondersteunde leden|
|--------------|-----------------------|
|Newtonsoft.Json.Linq.Extensions|Alle|
|Newtonsoft.Json.Linq.JArray|Alle|
|Newtonsoft.Json.Linq.JConstructor|Alle|
|Newtonsoft.Json.Linq.JContainer|Alle|
|Newtonsoft.Json.Linq.JObject|Alle|
|Newtonsoft.Json.Linq.JProperty|Alle|
|Newtonsoft.Json.Linq.JRaw|Alle|
|Newtonsoft.Json.Linq.JToken|Alle|
|Newtonsoft.Json.Linq.JTokenType|Alle|
|Newtonsoft.Json.Linq.JValue|Alle|
|System.Collections.Generic.IReadOnlyCollection<T\>|Alle|
|System.Collections.Generic.IReadOnlyDictionary<TKey,  TValue>|Alle|
|System.Collections.Generic.ISet<TKey, TValue>|Alle|
|System.Collections.Generic.KeyValuePair<TKey,  TValue>|Sleutel en waarde|
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
|System.IEnumerator < T\>|Alle|
|System.Int16|Alle|
|System.Int32|Alle|
|System.Int64|Alle|
|System.Linq.Enumerable<T\>|Alle|
|System.Math|Alle|
|System.MidpointRounding|Alle|
|System.Net.WebUtility|Alle|
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
|System.Text.RegularExpressions.Capture|Index, lengte, waarde|
|System.Text.RegularExpressions.CaptureCollection|Aantal, Item|
|System.Text.RegularExpressions.Group|Opnamen, geslaagd|
|System.Text.RegularExpressions.GroupCollection|Aantal, Item|
|System.Text.RegularExpressions.Match|Leeg, groepen, resultaat|
|System.Text.RegularExpressions.Regex|(Constructor), IsMatch, overeenkomst, overeenkomsten, vervangen|
|System.Text.RegularExpressions.RegexOptions|Gecompileerd, IgnoreCase, IgnorePatternWhitespace, Multiline None, RightToLeft, Singleline|
|System.TimeSpan|Alle|
|System.Tuple|Alle|
|System.UInt16|Alle|
|System.UInt32|Alle|
|System.UInt64|Alle|
|System.Uri|Alle|
|System.Xml.Linq.Extensions|Alle|
|System.Xml.Linq.XAttribute|Alle|
|System.Xml.Linq.XCData|Alle|
|System.Xml.Linq.XComment|Alle|
|System.Xml.Linq.XContainer|Alle|
|System.Xml.Linq.XDeclaration|Alle|
|System.Xml.Linq.XDocument|Alle|
|System.Xml.Linq.XDocumentType|Alle|
|System.Xml.Linq.XElement|Alle|
|System.Xml.Linq.XName|Alle|
|System.Xml.Linq.XNamespace|Alle|
|System.Xml.Linq.XNode|Alle|
|System.Xml.Linq.XNodeDocumentOrderComparer|Alle|
|System.Xml.Linq.XNodeEqualityComparer|Alle|
|System.Xml.Linq.XObject|Alle|
|System.Xml.Linq.XProcessingInstruction|Alle|
|System.Xml.Linq.XText|Alle|
|System.Xml.XmlNodeType|Alle|
  
## <a name="ContextVariables"></a> Contextvariabele
Een variabele met de naam `context` is impliciet beschikbaar zijn in elk beleid [expressie](api-management-policy-expressions.md#Syntax). De leden bevatten informatie die relevant zijn voor de `\request`. Alle van de `context` leden zijn alleen-lezen.
  
|Contextvariabele|Toegestane methoden, eigenschappen en parameterwaarden|
|----------------------|-------------------------------------------------------|
|Context|API: IApi<br /><br /> Implementatie<br /><br /> Verstreken: TimeSpan - tijdsinterval tussen de waarde van de tijdstempel en de huidige tijd<br /><br /> LastError<br /><br /> Bewerking<br /><br /> Product<br /><br /> Aanvraag<br /><br /> Aanvraag-id: GUID - unieke aanvraag-id<br /><br /> Antwoord<br /><br /> Abonnement<br /><br /> Timestamp: Datum/tijd - punt in tijd wanneer de aanvraag is ontvangen<br /><br /> Tracering: Boole - geeft aan of tracering in- of uitschakelen <br /><br /> Gebruiker<br /><br /> Variabelen: IReadOnlyDictionary < string, object ><br /><br /> Ongeldig Trace(message: string)|
|context.Api|ID: tekenreeks<br /><br /> IsCurrentRevision: bool<br /><br />  Naam: tekenreeks<br /><br /> Pad: tekenreeks<br /><br /> Revisie: tekenreeks<br /><br /> ServiceUrl: IUrl<br /><br /> Versie: tekenreeks |
|context. Implementatie|Regio: tekenreeks<br /><br /> Servicenaam: tekenreeks<br /><br /> Certificaten: IReadOnlyDictionary < string, X509Certificate2 >|
|context.LastError|Bron: tekenreeks<br /><br /> Reden: tekenreeks<br /><br /> Bericht: tekenreeks<br /><br /> Bereik: tekenreeks<br /><br /> Sectie: tekenreeks<br /><br /> Pad: tekenreeks<br /><br /> PolicyId: tekenreeks<br /><br /> Voor meer informatie over context. LastError, Zie [foutafhandeling](api-management-error-handling-policies.md).|
|context. Bewerking|ID: tekenreeks<br /><br /> Methode: tekenreeks<br /><br /> Naam: tekenreeks<br /><br /> UrlTemplate: tekenreeks|
|context.Product|API's: IEnumerable < IApi\><br /><br /> ApprovalRequired: bool<br /><br /> Groepen: IEnumerable < IGroup\><br /><br /> ID: tekenreeks<br /><br /> Naam: tekenreeks<br /><br /> Status: enum ProductState {NotPublished, gepubliceerd}<br /><br /> SubscriptionLimit: int?<br /><br /> SubscriptionRequired: bool|
|context. Aanvraag|Hoofdtekst: IMessageBody<br /><br /> Certificaat: System.Security.Cryptography.X509Certificates.X509Certificate2<br /><br /> Headers: IReadOnlyDictionary < string, string [] ><br /><br /> IP-adres: tekenreeks<br /><br /> MatchedParameters: IReadOnlyDictionary < string, string ><br /><br /> Methode: tekenreeks<br /><br /> OriginalUrl:IUrl<br /><br /> URL: IUrl|
|tekenreekscontext. Request.Headers.GetValueOrDefault (headernaam: string, defaultValue: string)|headernaam: tekenreeks<br /><br /> Standaardwaarde: tekenreeks<br /><br /> Retourneert de waarden voor aanvraag met door komma's gescheiden header of `defaultValue` als de header is niet gevonden.|
|context. Antwoord|Hoofdtekst: IMessageBody<br /><br /> Headers: IReadOnlyDictionary < string, string [] ><br /><br /> StatusCode: int<br /><br /> StatusReason: tekenreeks|
|tekenreekscontext. Response.Headers.GetValueOrDefault (headernaam: string, defaultValue: string)|headernaam: tekenreeks<br /><br /> Standaardwaarde: tekenreeks<br /><br /> Door komma's gescheiden antwoord retourneert headerwaarden of `defaultValue` als de header is niet gevonden.|
|context. Abonnement|Aanmaaktijd: DateTime<br /><br /> Einddatum: Datum-/?<br /><br /> ID: tekenreeks<br /><br /> Sleutel: tekenreeks<br /><br /> Naam: tekenreeks<br /><br /> PrimaryKey: tekenreeks<br /><br /> Secundaire sleutel: tekenreeks<br /><br /> Begindatum: Datum-/?|
|context. Gebruiker|E-mailadres: tekenreeks<br /><br /> Voornaam: tekenreeks<br /><br /> Groepen: IEnumerable < IGroup\><br /><br /> ID: tekenreeks<br /><br /> Identiteiten: IEnumerable < IUserIdentity\><br /><br /> Achternaam: tekenreeks<br /><br /> Opmerking: tekenreeks<br /><br /> RegistrationDate: DateTime|
|IApi|ID: tekenreeks<br /><br /> Naam: tekenreeks<br /><br /> Pad: tekenreeks<br /><br /> Protocollen: IEnumerable < tekenreeks\><br /><br /> ServiceUrl: IUrl<br /><br /> SubscriptionKeyParameterNames: ISubscriptionKeyParameterNames|
|IGroup|ID: tekenreeks<br /><br /> Naam: tekenreeks|
|IMessageBody|Als < T\>(preserveContent: bool = false): Waar/T: string, jobject is leeggemaakt, JToken, JArray, XNode, XElement, XDocument<br /><br /> De `context.Request.Body.As<T>` en `context.Response.Body.As<T>` methoden worden gebruikt voor het lezen van een aanvraag- en de berichttekst in een bepaald type `T`. Standaard wordt de methode maakt gebruik van de oorspronkelijke instantie berichtenstroom en maakt het niet beschikbaar is nadat het resultaat. Om te voorkomen dat door de methode die worden uitgevoerd op een kopie van de hoofdstroom, stel de `preserveContent` parameter `true`. Ga [hier](api-management-transformation-policies.md#SetBody) om een voorbeeld te zien.|
|IUrl|Host: tekenreeks<br /><br /> Pad: tekenreeks<br /><br /> Poort: int<br /><br /> Query: IReadOnlyDictionary < string, string [] ><br /><br /> QueryString: tekenreeks<br /><br /> Schema: tekenreeks|
|IUserIdentity|ID: tekenreeks<br /><br /> Provider: tekenreeks|
|ISubscriptionKeyParameterNames|Koptekst: tekenreeks<br /><br /> Query: tekenreeks|
|tekenreeks IUrl.Query.GetValueOrDefault (queryParameterName: string, defaultValue: string)|queryParameterName: tekenreeks<br /><br /> Standaardwaarde: tekenreeks<br /><br /> Retourneert de door komma's gescheiden waarden van queryparameter of `defaultValue` als de parameter niet wordt gevonden.|
|T-context. Variables.GetValueOrDefault < T\>(variableName: string, defaultValue: T)|variableName: tekenreeks<br /><br /> Standaardwaarde: T<br /><br /> Retourneert de waarde geconverteerd naar type variabele `T` of `defaultValue` als de variabele is niet gevonden.<br /><br /> Deze methode, wordt er een uitzondering genereert als het opgegeven type komt niet overeen met het werkelijke type van de geretourneerde variabele.|
|BasicAuthCredentials AsBasic(input: this string)|invoer: tekenreeks<br /><br /> Als de invoerparameter een geldige HTTP-basisverificatie autorisatie-aanvraag-header-waarde bevat, de methode retourneert een object van het type `BasicAuthCredentials`; anders wordt de methode wordt null geretourneerd.|
|BOOL TryParseBasic (invoer: deze tekenreeks, resultaat: van BasicAuthCredentials)|invoer: tekenreeks<br /><br /> resultaat: van BasicAuthCredentials<br /><br /> Als de invoerparameter een geldige HTTP-basisverificatie autorisatie-waarde in de aanvraagheader bevat de methode retourneert `true` en de resultatenparameter bevat een waarde van het type `BasicAuthCredentials`; anders wordt de methode retourneert `false`.|
|BasicAuthCredentials|Wachtwoord: tekenreeks<br /><br /> Gebruikers-id: tekenreeks|
|Jwt AsJwt(input: this string)|invoer: tekenreeks<br /><br /> Als de invoerparameter een ongeldige waarde van de JWT-token bevat, de methode retourneert een object van het type `Jwt`; anders wordt de methode retourneert `null`.|
|BOOL TryParseJwt (invoer: deze tekenreeks, resultaat: van Jwt)|invoer: tekenreeks<br /><br /> resultaat: van Jwt<br /><br /> De methode retourneert als de invoerparameter een geldige waarde voor JWT-token bevat, `true` en de resultatenparameter bevat een waarde van het type `Jwt`; anders wordt de methode retourneert `false`.|
|Jwt|Algoritme: tekenreeks<br /><br /> Doelgroep: IEnumerable < tekenreeks\><br /><br /> Claims: IReadOnlyDictionary < string, string [] ><br /><br /> ExpirationTime: Datum-/?<br /><br /> ID: tekenreeks<br /><br /> Uitgever: tekenreeks<br /><br /> NotBefore: Datum-/?<br /><br /> Onderwerp: tekenreeks<br /><br /> Type: tekenreeks|
|tekenreeks Jwt.Claims.GetValueOrDefault (claimName: string, defaultValue: string)|claimName: tekenreeks<br /><br /> Standaardwaarde: tekenreeks<br /><br /> Retourneert met door komma's gescheiden waarden claim of `defaultValue` als de header is niet gevonden.|
|byte [] versleutelen (invoer: deze byte [], alg: string, sleutel: byte [], iv:byte[])|invoer - als tekst zonder opmaak moeten worden versleuteld<br /><br />Alg - naam van een symmetrische versleutelingsalgoritme<br /><br />sleutel - versleuteling<br /><br />IV - initialisatievector<br /><br />Met deze eigenschap wordt versleuteld als tekst zonder opmaak.|
|byte [] versleutelen (invoer: deze byte [], alg: System.Security.Cryptography.SymmetricAlgorithm)|invoer - als tekst zonder opmaak moeten worden versleuteld<br /><br />Alg - versleutelingsalgoritme<br /><br />Met deze eigenschap wordt versleuteld als tekst zonder opmaak.|
|byte [] versleutelen (invoer: deze byte [], alg: System.Security.Cryptography.SymmetricAlgorithm, sleutel: byte [], iv:byte[])|invoer - als tekst zonder opmaak moeten worden versleuteld<br /><br />Alg - versleutelingsalgoritme<br /><br />sleutel - versleuteling<br /><br />IV - initialisatievector<br /><br />Met deze eigenschap wordt versleuteld als tekst zonder opmaak.|
|byte [] decoderen (invoer: deze byte [], alg: string, sleutel: byte [], iv:byte[])|invoer - ineens tekst die moet worden gedecodeerd<br /><br />Alg - naam van een symmetrische versleutelingsalgoritme<br /><br />sleutel - versleuteling<br /><br />IV - initialisatievector<br /><br />Geeft als resultaat als tekst zonder opmaak.|
|byte [] decoderen (invoer: deze byte [], alg: System.Security.Cryptography.SymmetricAlgorithm)|invoer - ineens tekst die moet worden gedecodeerd<br /><br />Alg - versleutelingsalgoritme<br /><br />Geeft als resultaat als tekst zonder opmaak.|
|byte [] decoderen (invoer: deze byte [], alg: System.Security.Cryptography.SymmetricAlgorithm, sleutel: byte [], iv:byte[])|invoer - invoer - ineens tekst die moet worden gedecodeerd<br /><br />Alg - versleutelingsalgoritme<br /><br />sleutel - versleuteling<br /><br />IV - initialisatievector<br /><br />Geeft als resultaat als tekst zonder opmaak.|


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie werken met beleidsregels voor:

+ [Beleid in API Management](api-management-howto-policies.md)
+ [API's transformeren](transform-api.md)
+ [Naslaginformatie over beleid voor](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en de bijbehorende instellingen
+ [Voorbeelden van beleid](policy-samples.md)   

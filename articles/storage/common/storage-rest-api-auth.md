---
title: Het aanroepen van REST-API van Azure Storage Services bewerkingen zoals verificatie | Microsoft Docs
description: Het aanroepen van REST-API van Azure Storage Services bewerkingen zoals verificatie
services: storage
documentationcenter: na
author: robinsh
manager: timlt
ms.assetid: f4704f58-abc6-4f89-8b6d-1b1659746f5a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 11/27/2017
ms.author: robinsh
ms.openlocfilehash: 521487c3ed38f191308e14e4d542358438945556
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="using-the-azure-storage-rest-api"></a>De Azure Storage REST API gebruiken

In dit artikel leest u het gebruik van de Blob Storage-Service REST API's en hoe u verifieert de aanroep naar de service. Deze vanuit het oogpunt van iemand die weet endtoepassing niets over REST en geen idee hoe u een REST-aanroep is geschreven, maar er is een ontwikkelaar. We kijken naar de documentatie bij een REST-aanroep en zien hoe u om te zetten in een werkelijke REST-aanroep – welke velden gaan waar? Na het instellen van een REST-aanroep leren, kunt u gebruikmaken van deze informatie voor het gebruik van een van de andere opslag Service REST API's.

## <a name="prerequisites"></a>Vereisten 

De toepassing bevat de containers in blob-opslag voor een opslagaccount. Als u wilt de code in dit artikel uitproberen, moet u de volgende items: 

* Installeer [Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx) met de volgende werkbelasting:
    - Azure-ontwikkeling

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

* Een algemeen opslagaccount. Als u een opslagaccount nog geen hebt, raadpleegt u [een opslagaccount maken](storage-quickstart-create-account.md).

* Het voorbeeld in dit artikel ziet hoe u de containers in een opslagaccount. Uitvoer, toe te voegen sommige containers naar de blob storage in de storage-account voordat u begint.

## <a name="download-the-sample-application"></a>De voorbeeldtoepassing downloaden

De voorbeeldtoepassing is een geschreven in C#-consoletoepassing.

Gebruik [git](https://git-scm.com/) om een kopie van de toepassing naar uw ontwikkelomgeving te downloaden. 

```bash
git clone https://github.com/Azure-Samples/storage-dotnet-rest-api-with-auth.git
```

Met deze opdracht wordt de opslagplaats naar uw lokale git-map gekloond. Open de Visual Studio-oplossing, zoekt u naar de map storage-dotnet-rest-api-with-auth, openen en dubbelklik op StorageRestApiAuth.sln. 

## <a name="why-do-i-need-to-know-rest"></a>Waarom moet ik te weten REST?

Weten hoe u REST is een nuttig kwalificatie. Het Azure-productteam versies regelmatig nieuwe functies. In veel gevallen de nieuwe functies zijn toegankelijk via de REST-interface, maar nog niet zijn opgehaald via **alle** van de opslag-clientbibliotheken of de gebruikersinterface (zoals de Azure-portal). Als u altijd de nieuwste en beste gebruiken wilt, REST learning is een vereiste. Als u wilt schrijven van uw eigen bibliotheek om te kunnen communiceren met Azure Storage of u wilt toegang krijgen tot Azure Storage met een programmeertaal die geen een clientbibliotheek SDK of de opslag, kunt u de REST-API.

## <a name="what-is-rest"></a>Wat is REST?

REST-betekent *representational state transfer*. Bekijk voor de definitie van een specifieke [Wikipedia](http://en.wikipedia.org/wiki/Representational_state_transfer).

In principe REST is voor een architectuur die u kunt gebruiken bij het aanroepen van API's of het maken van API's beschikbaar moet worden aangeroepen. Het is niet afhankelijk van wat er aan beide zijden gebeurt en welke andere software wordt gebruikt bij het verzenden of ontvangen van de REST-aanroepen. U kunt een toepassing die wordt uitgevoerd op een Mac, Windows, Linux, een Android-telefoon of tablet, iPhone, iPod of website schrijven en de dezelfde REST-API gebruiken voor elk van deze platforms. Gegevens kunnen worden doorgegeven en/of uit wanneer de REST-API wordt aangeroepen. De REST-API belangrijk niet vanaf welk platform wordt genoemd – wat belangrijk is de informatie die wordt doorgegeven in de aanvraag en de gegevens die zijn opgegeven in het antwoord.

## <a name="heres-the-plan"></a>Hier wordt het plan

De voorbeeldproject bevat de containers in een opslagaccount. Wanneer u hoe de informatie in de documentatie voor REST-API heeft betrekking op uw eigen code begrijpt, zijn andere REST-aanroepen gemakkelijker te achterhalen. 

Als u naar kijkt de [REST-API van Blob](/rest/api/storageservices/fileservices/Blob-Service-REST-API), ziet u de bewerkingen die u op de blob-opslag uitvoeren kunt. De clientbibliotheken storage wrappers voor de REST API's zijn – ze eenvoudiger voor u toegang tot opslag zonder gebruik van de REST API's direct. Maar zoals eerder vermeld, soms wilt u de REST-API gebruiken in plaats van een storage-clientbibliotheek.

## <a name="rest-api-reference-list-containers-api"></a>Naslaginformatie over REST-API: Lijst Containers API

Bekijk de pagina in de REST-API-referentiemateriaal voor de [ListContainers](/rest/api/storageservices/fileservices/List-Containers2) bewerking zodat u waar sommige velden afkomstig zijn weet uit in de aanvraag en antwoord in de volgende sectie met de code.

**Verzoekmethode**: ophalen. Deze opdracht wordt de HTTP-methode die u als een eigenschap van het request-object opgeeft. Andere waarden voor deze opdracht zijn HEAD, PUT en DELETE, afhankelijk van de API die u aanroept.

**URI van de aanvraag**: dit wordt gemaakt van het eindpunt van blob storage-account https://myaccount.blob.core.windows.net/?comp=list `http://myaccount.blob.core.windows.net` en de brontekenreeks `/?comp=list`.

[URI-parameters](/rest/api/storageservices/fileservices/List-Containers2#uri-parameters): Er zijn aanvullende queryparameters die u gebruiken kunt bij het aanroepen van ListContainers. Enkele van deze parameters worden *time-out* voor de oproep (in seconden) en *voorvoegsel*, die wordt gebruikt voor het filteren.

Een andere nuttige parameter *maxresults:* als meer containers dan deze waarde beschikbaar zijn, de hoofdtekst van het antwoord bevat een *NextMarker* element waarmee wordt aangegeven van de volgende container om terug te keren op de volgende de aanvraag. Deze functie wilt gebruiken, bieden u de *NextMarker* waarde als de *markering* parameter in de wanneer u de volgende aanvraag-URI. Wanneer u deze functie gebruikt, is dit is vergelijkbaar met het bladeren door de resultaten. 

Aanvullende als parameters wilt gebruiken, moet u deze naar de brontekenreeks met de waarde, zoals in dit voorbeeld toevoegen:

```
/?comp=list&timeout=60&maxresults=100
```

[Aanvraagheaders](/rest/api/storageservices/fileservices/List-Containers2#request-headers)**:** in deze sectie vindt u de vereiste en optionele aanvraagheaders. Drie van de headers zijn vereist: een *autorisatie* header *x-ms-date* (de UTC-tijd voor de aanvraag bevat), en *x-ms-version* (geeft de versie van de REST API te gebruiken). Inclusief *x-ms-client-request-id* is optioneel – u kunt de waarde voor dit veld ingesteld op iets; deze naar de logboeken van storage analytics worden geschreven als logboekregistratie is ingeschakeld in de headers.

[Aanvraagtekst](/rest/api/storageservices/fileservices/List-Containers2#request-body)**:** er is geen aanvraaginhoud voor ListContainers. Hoofdtekst van de aanvraag wordt gebruikt op alle van de PUT-bewerkingen tijdens het uploaden van BLOB's, evenals SetContainerAccessPolicy, zodat u kunt verzenden in een XML-lijst van opgeslagen toegangsbeleid toe te passen. Opgeslagen toegangsbeleid worden besproken in het artikel [met behulp van Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md).

[Statuscode van antwoord](/rest/api/storageservices/fileservices/List-Containers2#status-code)**:** Tells van eventuele statuscodes die u wilt weten. In dit voorbeeld is een HTTP-statuscode 200 ok. Bekijk voor een volledige lijst met HTTP-statuscodes, [definities van statuscodes](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html). Zie voor foutcodes die specifiek zijn voor de opslag REST-API's [algemene REST-API-foutcodes](/rest/api/storageservices/common-rest-api-error-codes)

[Antwoordheaders](/rest/api/storageservices/fileservices/List-Containers2#response-headers)**:** hierbij *inhoudstype*; *x-ms-aanvraag-id* (de aanvraag-id u ingevoerd, indien van toepassing); *x-ms-version* (geeft de versie van de Blob-service gebruikt), en de *datum* (UTC, weet welk tijdstip de aanvraag is ingediend).

[Antwoordtekst](/rest/api/storageservices/fileservices/List-Containers2#response-body): dit veld is een XML-structuur bieden de aangevraagde gegevens. In dit voorbeeld is het antwoord een lijst van containers en hun eigenschappen.

## <a name="creating-the-rest-request"></a>Maken van de REST-aanvraag

Een aantal opmerkingen voordat u begint: beveiliging bij uitvoering in productie, altijd gebruiken HTTPS in plaats van HTTP. Voor het doel van deze oefening, moet u HTTP gebruiken, zodat u de aanvraag en antwoord-gegevens kunt bekijken. De aanvraag en -antwoord om informatie te bekijken in de werkelijke REST-aanroepen, kunt u downloaden [Fiddler](http://www.telerik.com/fiddler) of een vergelijkbare toepassing. In de Visual Studio-oplossing de naam van het opslagaccount en -sleutel zijn vastgelegd in de klasse en de methode ListContainersAsyncREST de opslagaccountnaam en opslagaccountsleutel wordt doorgegeven aan de methoden die worden gebruikt voor het maken van de verschillende onderdelen van de REST-aanvraag . In werkelijkheid toepassing, de naam van het opslagaccount en de sleutel zou bevinden zich in een configuratiebestand, omgevingsvariabelen of worden opgehaald uit een Azure Sleutelkluis.

In onze voorbeeldproject wordt de code voor het maken van de autorisatie-header een afzonderlijke klasse, met het idee dat u kan de gehele klasse nemen en toe te aan uw eigen oplossing voegen en 'als zodanig.' De autorisatie-header-code werkt voor de meeste REST API-aanroepen naar Azure Storage.

Als u wilt maken van de aanvraag, dit een object HttpRequestMessage is, gaat u naar ListContainersAsyncREST in Program.cs. De stappen voor het bouwen van de aanvraag zijn: 

* De URI die moet worden gebruikt voor het aanroepen van de service maken. 
* Het object HttpRequestMessage maakt en stel de nettolading. De nettolading is null voor ListContainersAsyncREST omdat wij iets in niet doorgeven.
* Voeg de headers van de aanvraag voor x-ms-datum- en x-ms-version.
* Ophalen van de autorisatie-header en toe te voegen.

Sommige algemene informatie die u nodig: 

*  Voor ListContainers, de **methode** is `GET`. Deze waarde wordt ingesteld bij het instantiëren van de aanvraag. 
*  De **resource** is het query-gedeelte van de URI die geeft aan welke API wordt aangeroepen, zodat de waarde is `/?comp=list`. Zoals eerder opgemerkt, de resource is op de referentie-documentatiepagina waarin de gegevens over de [ListContainers API](/rest/api/storageservices/fileservices/List-Containers2).
*  De URI is opgesteld door het maken van het eindpunt van de Blob-service voor dat opslagaccount en samenvoegen van de resource. De waarde voor **URI van de aanvraag** achteraf `http://contosorest.blob.core.windows.net/?comp=list`.
*  Voor ListContainers, **requestBody** is null en er zijn geen extra **headers**.

Verschillende API's mogelijk andere parameters om op te geven, zoals *ifMatch*. Een voorbeeld van waar u ifMatch kunt gebruiken is bij het aanroepen van PutBlob. In dat geval u ifMatch instellen op een eTag en wordt alleen de blob bijgewerkt als de eTag die u opgeeft overeenkomt met de huidige eTag op de blob. Als iemand anders de blob bijgewerkt heeft sinds het ophalen van de eTag, kan de wijziging won't worden overschreven. 

Stel eerst de `uri` en de `payload`. 

```csharp
// Construct the URI. This will look like this:
//   https://myaccount.blob.core.windows.net/resource
String uri = string.Format("http://{0}.blob.core.windows.net?comp=list", storageAccountName);

// Set this to whatever payload you desire. Ours is null because 
//   we're not passing anything in.
Byte[] requestPayload = null;
```

Vervolgens exemplaar maken van de aanvraag, als u de methode op `GET` en het geven van de URI.

```csharp 
//Instantiate the request message with a null payload.
using (var httpRequestMessage = new HttpRequestMessage(HttpMethod.Get, uri)
{ Content = (requestPayload == null) ? null : new ByteArrayContent(requestPayload) })
{
```

Voeg de headers van de aanvraag voor x-ms-datum- en x-ms-version. Deze locatie in de code is ook waarin u een extra aanvraagheaders vereist voor de aanroep toevoegen. In dit voorbeeld zijn er geen aanvullende headers. Een voorbeeld van een API die wordt doorgegeven in extra headers is SetContainerACL. Wordt toegevoegd een header genaamd 'x-ms-blob-openbare-toegang' en de waarde voor het toegangsniveau voor Blob storage.

```csharp
    // Add the request headers for x-ms-date and x-ms-version.
    DateTime now = DateTime.UtcNow;
    httpRequestMessage.Headers.Add("x-ms-date", now.ToString("R", CultureInfo.InvariantCulture));
    httpRequestMessage.Headers.Add("x-ms-version", "2017-04-17");
    // If you need any additional headers, add them here before creating
    //   the authorization header. 
```

Roep de methode waarmee de autorisatie-header en toe te voegen aan de aanvraagheaders. Hier ziet u het maken van de autorisatie-header later in dit artikel. De methodenaam is GetAuthorizationHeader die u in dit codefragment zien kunt:

```csharp
    // Get the authorization header and add it.
    httpRequestMessage.Headers.Authorization = AzureStorageAuthenticationHelper.GetAuthorizationHeader(
        storageAccountName, storageAccountKey, now, httpRequestMessage);
```

Op dit moment `httpRequestMessage` bevat de REST de aanvraag is voltooid met de autorisatie-header. 

## <a name="call-the-rest-api-with-the-request"></a>De REST-API aan de aanvraag niet aanroepen

Nu dat u de aanvraag hebt, kunt u SendAsync voor het verzenden van de REST-aanvraag aanroepen. SendAsync roept de API en het antwoord weer opgehaald. Het antwoord StatusCode onderzoeken (200 is OK), klikt u vervolgens het antwoord parseren. In dit geval kunt u een XML-lijst van containers ophalen. We bekijken de code voor het aanroepen van de methode GetRESTRequest voor het maken van de aanvraag, uitvoeren van de aanvraag en vervolgens het antwoord voor de lijst van containers te onderzoeken.

```csharp 
    // Send the request.
    using (HttpResponseMessage httpResponseMessage = 
      await new HttpClient().SendAsync(httpRequestMessage, cancellationToken))
    {
        // If successful (status code = 200), 
        //   parse the XML response for the container names.
        if (httpResponseMessage.StatusCode == HttpStatusCode.OK)
        {
            String xmlString = await httpResponseMessage.Content.ReadAsStringAsync();
            XElement x = XElement.Parse(xmlString);
            foreach (XElement container in x.Element("Containers").Elements("Container"))
            {
                Console.WriteLine("Container name = {0}", container.Element("Name").Value);
            }
        }
    }
}
```

Als u een netwerksniffer zoals uitvoert [Fiddler](https://www.telerik.com/fiddler) bij het maken van de aanroep van SendAsync, ziet u de aanvraag en antwoord-informatie. Laten we. De naam van het opslagaccount is *contosorest*.

**Aanvraag:**

```
GET /?comp=list HTTP/1.1
```

**Headers voor aanvraag:**

```
x-ms-date: Thu, 16 Nov 2017 23:34:04 GMT
x-ms-version: 2014-02-14
Authorization: SharedKey contosorest:1dVlYJWWJAOSHTCPGiwdX1rOS8B4fenYP/VrU0LfzQk=
Host: contosorest.blob.core.windows.net
Connection: Keep-Alive
```

**Status code- en reactieheaders na de uitvoering is geretourneerd:**

```
HTTP/1.1 200 OK
Content-Type: application/xml
Server: Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0
x-ms-request-id: 3e889876-001e-0039-6a3a-5f4396000000
x-ms-version: 04-17
Date: Fri, 17 Nov 2017 00:23:42 GMT
Content-Length: 1511
```

**Antwoordtekst (XML):** voor ListContainers Hiermee wordt de lijst van containers en hun eigenschappen weergegeven.

```xml  
<?xml version="1.0" encoding="utf-8"?>
<EnumerationResults 
  ServiceEndpoint="http://contosorest.blob.core.windows.net/">
  <Containers>
    <Container>
      <Name>container-1</Name>
      <Properties>
        <Last-Modified>Thu, 16 Mar 2017 22:39:48 GMT</Last-Modified>
        <Etag>"0x8D46CBD5A7C301D"</Etag>
        <LeaseStatus>unlocked</LeaseStatus>
        <LeaseState>available</LeaseState>
      </Properties>
    </Container>
    <Container>
      <Name>container-2</Name>
      <Properties>
        <Last-Modified>Thu, 16 Mar 2017 22:40:50 GMT</Last-Modified>
        <Etag>"0x8D46CBD7F49E9BD"</Etag>
        <LeaseStatus>unlocked</LeaseStatus>
        <LeaseState>available</LeaseState>
      </Properties>
    </Container>
    <Container>
      <Name>container-3</Name>
      <Properties>
        <Last-Modified>Thu, 16 Mar 2017 22:41:10 GMT</Last-Modified>
        <Etag>"0x8D46CBD8B243D68"</Etag>
        <LeaseStatus>unlocked</LeaseStatus>
        <LeaseState>available</LeaseState>
      </Properties>
    </Container>
    <Container>
      <Name>container-4</Name>
      <Properties>
        <Last-Modified>Thu, 16 Mar 2017 22:41:25 GMT</Last-Modified>
        <Etag>"0x8D46CBD93FED46F"</Etag>
        <LeaseStatus>unlocked</LeaseStatus>
        <LeaseState>available</LeaseState>
        </Properties>
      </Container>
      <Container>
        <Name>container-5</Name>
        <Properties>
          <Last-Modified>Thu, 16 Mar 2017 22:41:39 GMT</Last-Modified>
          <Etag>"0x8D46CBD9C762815"</Etag>
          <LeaseStatus>unlocked</LeaseStatus>
          <LeaseState>available</LeaseState>
        </Properties>
      </Container>
  </Containers>
  <NextMarker />
</EnumerationResults>
```

Nu u weet hoe u de aanvraag maken, de service aanroepen en parseren van de resultaten, gaan we gaat u naar het maken van de autorisatie-header. Maken van die header is ingewikkeld, maar de goed nieuws is zodra u de code werkt hebt, het werkt voor alle van de Storage-Service REST-API's.

## <a name="creating-the-authorization-header"></a>Maken van de autorisatie-header

Er is een artikel waarin wordt uitgelegd conceptueel (geen code) het uitvoeren van [verificatie voor de Azure Storage-Services](/rest/api/storageservices/fileservices/Authentication-for-the-Azure-Storage-Services).
Laten we dat artikel omlaag naar exact distill is vereist en de code niet weergeven.

Eerste verificatie met een gedeelde sleutel gebruiken. De autorisatie-header-indeling ziet er als volgt:

```  
Authorization="SharedKey <storage account name>:<signature>"  
```

Het handtekeningveld is een Hash-based Message Authentication Code (HMAC) gemaakt op basis van de aanvraag en het algoritme SHA256 gebruikt, is gecodeerd met Base64-codering berekend. Hebt u dat? (Vastlopen daar, zelfs niets het woord hebt gehoord *gecanoniseerd* nog.)

Dit codefragment ziet u de indeling van de gedeelde sleutel handtekening tekenreeks:

```csharp  
StringToSign = VERB + "\n" +  
               Content-Encoding + "\n" +  
               Content-Language + "\n" +  
               Content-Length + "\n" +  
               Content-MD5 + "\n" +  
               Content-Type + "\n" +  
               Date + "\n" +  
               If-Modified-Since + "\n" +  
               If-Match + "\n" +  
               If-None-Match + "\n" +  
               If-Unmodified-Since + "\n" +  
               Range + "\n" +  
               CanonicalizedHeaders +  
               CanonicalizedResource;  
```

De meeste van deze velden worden zelden gebruikt. Voor Blob storage geeft u de term, md5, lengte van de inhoud, Gecanoniseerd Headers en Gecanoniseerd Resource. U kunt de andere leeg laten (maar geplaatst in de `\n` zodat deze ze zijn leeg).

Wat zijn CanonicalizedHeaders en CanonicalizedResource? Goede vraag. Wat doet gecanoniseerd in feite gemiddelde? Microsoft Word niet zelfs herkend als een woord. Dit is wat [Wikipedia vertelt over standaardisatie](http://en.wikipedia.org/wiki/Canonicalization): *In computer wetenschap, standaardisatie (soms standaardisatie of normalisatie) is een proces voor het converteren van gegevens met meer dan één mogelijk weergave in een 'standaard', 'normale' of canonieke vorm.* In normale uitspreken, dit betekent dat moet worden overgenomen van de lijst met items (zoals kopteksten in het geval van Headers Gecanoniseerd) en ze naar een vereiste indeling te standaardiseren. In principe Microsoft besloten op een indeling en u wilt vergelijken.

We beginnen met de twee gestandaardiseerde velden, omdat ze zijn vereist voor het maken van de autorisatie-header.

**Canonieke Headers**

Voor het maken van deze waarde ophalen van de headers die beginnen met 'x - ms-' en sorteren en maak deze naar een tekenreeks van `[key:value\n]` exemplaren, samengevoegd tot één tekenreeks. In dit voorbeeld uitzien de canonieke headers: 

```
x-ms-date:Fri, 17 Nov 2017 00:44:48 GMT\nx-ms-version:2017-04-17\n
```

Dit is de code die wordt gebruikt voor het maken van die uitvoer:

```csharp 
private static string GetCanonicalizedHeaders(HttpRequestMessage httpRequestMessage)
{
    var headers = from kvp in httpRequestMessage.Headers
                    where kvp.Key.StartsWith("x-ms-", StringComparison.OrdinalIgnoreCase)
                    orderby kvp.Key
                    select new { Key = kvp.Key.ToLowerInvariant(), kvp.Value };

    StringBuilder sb = new StringBuilder();

    // Create the string in the right format; this is what makes the headers "canonicalized" --
    //   it means put in a standard format. http://en.wikipedia.org/wiki/Canonicalization
    foreach (var kvp in headers)
    {
        StringBuilder headerBuilder = new StringBuilder(kvp.Key);
        char separator = ':';

        // Get the value for each header, strip out \r\n if found, then append it with the key.
        foreach (string headerValues in kvp.Value)
        {
            string trimmedValue = headerValues.TrimStart().Replace("\r\n", String.Empty);
            headerBuilder.Append(separator).Append(trimmedValue);

            // Set this to a comma; this will only be used 
            //   if there are multiple values for one of the headers.
            separator = ',';
        }
        sb.Append(headerBuilder.ToString()).Append("\n");
    }
    return sb.ToString();
}      
```

**Canonieke Resource**

Dit deel van de handtekening tekenreeks vertegenwoordigt de storage-account waarop de aanvraag is gericht. Houd er rekening mee dat de aanvraag-URI is `<http://contosorest.blob.core.windows.net/?comp=list>`, met de naam van de daadwerkelijke account (`contosorest` in dit geval). In dit voorbeeld is dit het resultaat:

```
/contosorest/\ncomp:list
```

Als u hebt de queryparameters, omvat dit die ook. Hier volgt de code, die ook aanvullende queryparameters en query-parameters met meerdere waarden worden verwerkt. Houd er rekening mee dat u deze code werkt voor alle REST-API's, zodat u opnemen van alle mogelijkheden, wilt zelfs als de methode ListContainers niet al deze hoeft samenstelt.

```csharp 
private static string GetCanonicalizedResource(Uri address, string storageAccountName)
{
    // The absolute path will be "/" because for we're getting a list of containers.
    StringBuilder sb = new StringBuilder("/").Append(storageAccountName).Append(address.AbsolutePath);

    // Address.Query is the resource, such as "?comp=list".
    // This ends up with a NameValueCollection with 1 entry having key=comp, value=list.
    // It will have more entries if you have more query parameters.
    NameValueCollection values = HttpUtility.ParseQueryString(address.Query);

    foreach (var item in values.AllKeys.OrderBy(k => k))
    {
        sb.Append('\n').Append(item).Append(':').Append(values[item]);
    }

    return sb.ToString();
}
```

Nu dat de canonieke tekenreeksen zijn ingesteld, bekijken we de autorisatie-header zelf maken. U begint met het maken van een tekenreeks van de handtekening van het bericht in de indeling van StringToSign eerder in dit artikel wordt weergegeven. Dit concept is het eenvoudiger om uit te leggen met opmerkingen in de code, dus hier is de laatste methode waarmee de autorisatie-Header wordt geretourneerd:

```csharp
internal static AuthenticationHeaderValue GetAuthorizationHeader(
    string storageAccountName, string storageAccountKey, DateTime now,
    HttpRequestMessage httpRequestMessage, string ifMatch = "", string md5 = "")
{
    // This is the raw representation of the message signature.
    HttpMethod method = httpRequestMessage.Method;
    String MessageSignature = String.Format("{0}\n\n\n{1}\n{5}\n\n\n\n{2}\n\n\n\n{3}{4}",
                method.ToString(),
                (method == HttpMethod.Get || method == HttpMethod.Head) ? String.Empty
                  : httpRequestMessage.Content.Headers.ContentLength.ToString(),
                ifMatch,
                GetCanonicalizedHeaders(httpRequestMessage),
                GetCanonicalizedResource(httpRequestMessage.RequestUri, storageAccountName),
                md5);

    // Now turn it into a byte array.
    byte[] SignatureBytes = Encoding.UTF8.GetBytes(MessageSignature);

    // Create the HMACSHA256 version of the storage key.
    HMACSHA256 SHA256 = new HMACSHA256(Convert.FromBase64String(storageAccountKey));

    // Compute the hash of the SignatureBytes and convert it to a base64 string.
    string signature = Convert.ToBase64String(SHA256.ComputeHash(SignatureBytes));

    // This is the actual header that will be added to the list of request headers.
    AuthenticationHeaderValue authHV = new AuthenticationHeaderValue("SharedKey",
        storageAccountName + ":" + Convert.ToBase64String(SHA256.ComputeHash(SignatureBytes)));
    return authHV;
}
```

Wanneer u deze code uitvoert, wordt de resulterende MessageSignature ziet er als volgt:

```
GET\n\n\n\n\n\n\n\n\n\n\n\nx-ms-date:Fri, 17 Nov 2017 01:07:37 GMT\nx-ms-version:2017-04-17\n/contosorest/\ncomp:list
```

Hier wordt de eindwaarde voor AuthorizationHeader:

```
SharedKey contosorest:Ms5sfwkA8nqTRw7Uury4MPHqM6Rj2nfgbYNvUKOa67w=
```

De AuthorizationHeader is de laatste kop voordat u het antwoord in de aanvraagheaders is geplaatst.

Die alles wat die u weten moet, samen met de code samenstellen van een klasse die u gebruiken kunt voor het maken van een verzoek om te worden gebruikt voor het aanroepen van de Storage-Services REST API's worden behandeld.

## <a name="how-about-another-example"></a>Hoe zit een ander voorbeeld? 

Bekijk het wijzigen van de code voor het aanroepen van ListBlobs voor de container *container 1*. Dit is bijna identiek aan de code voor het aanbieden van containers, de enige verschillen, wordt de URI en hoe het parseren van het antwoord. 

Als u de documentatie bij bekijkt [ListBlobs](/rest/api/storageservices/fileservices/List-Blobs), u merkt dat de methode *ophalen* en de RequestURI is:

```
https://myaccount.blob.core.windows.net/container-1?restype=container&comp=list
```

Wijzig in ListContainersAsyncREST, de code die de URI voor de API voor ListBlobs ingesteld. De containernaam van de is **container 1**.

```csharp
String uri = 
    string.Format("http://{0}.blob.core.windows.net/container-1?restype=container&comp=list",
      storageAccountName);

```

Wanneer u het antwoord verwerkt wijzigt u vervolgens de code voor blobs in plaats van de containers.

```csharp
foreach (XElement container in x.Element("Blobs").Elements("Blob"))
{
    Console.WriteLine("Blob name = {0}", container.Element("Name").Value);
}
```

Als u dit voorbeeld uitvoert, krijgt u resultaten als volgt:

**Canonieke Headers:**

```
x-ms-date:Fri, 17 Nov 2017 05:16:48 GMT\nx-ms-version:2017-04-17\n
```

**Canonieke Resource:**

```
/contosorest/container-1\ncomp:list\nrestype:container
```

**MessageSignature:**

```
GET\n\n\n\n\n\n\n\n\n\n\n\nx-ms-date:Fri, 17 Nov 2017 05:16:48 GMT
  \nx-ms-version:2017-04-17\n/contosorest/container-1\ncomp:list\nrestype:container
```

**AuthorizationHeader:**

```
SharedKey contosorest:uzvWZN1WUIv2LYC6e3En10/7EIQJ5X9KtFQqrZkxi6s=
```

De volgende waarden zijn van [Fiddler](http://www.telerik.com/fiddler):

**Aanvraag:**

```
GET http://contosorest.blob.core.windows.net/container-1?restype=container&comp=list HTTP/1.1
```

**Headers voor aanvraag:**

```
x-ms-date: Fri, 17 Nov 2017 05:16:48 GMT
x-ms-version: 2017-04-17
Authorization: SharedKey contosorest:uzvWZN1WUIv2LYC6e3En10/7EIQJ5X9KtFQqrZkxi6s=
Host: contosorest.blob.core.windows.net
Connection: Keep-Alive
```

**Status code- en reactieheaders na de uitvoering is geretourneerd:**

```
HTTP/1.1 200 OK
Content-Type: application/xml
Server: Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0
x-ms-request-id: 7e9316da-001e-0037-4063-5faf9d000000
x-ms-version: 2017-04-17
Date: Fri, 17 Nov 2017 05:20:21 GMT
Content-Length: 1135
```

**Antwoordtekst (XML):** deze XML-antwoord bevat de lijst met blobs en hun eigenschappen. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<EnumerationResults 
    ServiceEndpoint="http://contosorest.blob.core.windows.net/" ContainerName="container-1">
    <Blobs>
        <Blob>
            <Name>DogInCatTree.png</Name>
            <Properties><Last-Modified>Fri, 17 Nov 2017 01:41:14 GMT</Last-Modified>
            <Etag>0x8D52D5C4A4C96B0</Etag>
            <Content-Length>419416</Content-Length>
            <Content-Type>image/png</Content-Type>
            <Content-Encoding />
            <Content-Language />
            <Content-MD5 />
            <Cache-Control />
            <Content-Disposition />
            <BlobType>BlockBlob</BlobType>
            <LeaseStatus>unlocked</LeaseStatus>
            <LeaseState>available</LeaseState>
            <ServerEncrypted>true</ServerEncrypted>
            </Properties>
        </Blob>
        <Blob>
            <Name>GuyEyeingOreos.png</Name>
            <Properties>
                <Last-Modified>Fri, 17 Nov 2017 01:41:14 GMT</Last-Modified>
                <Etag>0x8D52D5C4A25A6F6</Etag>
                <Content-Length>167464</Content-Length>
                <Content-Type>image/png</Content-Type>
                <Content-Encoding />
                <Content-Language />
                <Content-MD5 />
                <Cache-Control />
                <Content-Disposition />
                <BlobType>BlockBlob</BlobType>
                <LeaseStatus>unlocked</LeaseStatus>
                <LeaseState>available</LeaseState>
                <ServerEncrypted>true</ServerEncrypted>
            </Properties>
            </Blob>
        </Blobs>
    <NextMarker />
</EnumerationResults>
```

## <a name="summary"></a>Samenvatting

In dit artikel hebt u geleerd hoe u een aanvraag naar de blob storage REST-API voor het ophalen van een lijst van containers of een lijst met blobs in een container. U hebt ook geleerd hoe de autorisatie-handtekening voor de REST-API-aanroep wilt maken, het gebruik ervan in de REST-aanvraag en onderzoeken van het antwoord.

## <a name="next-steps"></a>Volgende stappen

* [BLOB-Service REST-API](/rest/api/storageservices/blob-service-rest-api)
* [File-Service REST-API](/rest/api/storageservices/file-service-rest-api)
* [REST-API van wachtrij](/rest/api/storageservices/queue-service-rest-api)
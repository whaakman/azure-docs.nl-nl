---
title: Aanroepen van REST-API voor Azure Storage-Services bewerkingen waaronder authentication | Microsoft Docs
description: Aanroepen van REST-API voor Azure Storage-Services bewerkingen, met inbegrip van verificatie
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 05/22/2018
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 08a86e1b2808a0778734edecc9385f4d61779b25
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55476193"
---
# <a name="using-the-azure-storage-rest-api"></a>De Azure Storage REST API gebruiken

In dit artikel ziet u hoe u REST API's voor de Blob Storage-Service en hoe u verifieert de oproep naar de service. Het is geschreven vanuit het oogpunt van iemand die weet niets over de REST en geen idee hoe u een REST-aanroep, maar er is een ontwikkelaar. We kijken naar de referentiedocumentatie voor een REST-aanroep en zien hoe u om te zetten in een werkelijke REST-aanroep: welke velden gaan waar? Nadat u bent gekomen over het instellen van een REST-aanroep, kunt u gebruikmaken van deze kennis om een van de andere opslag Service REST API's te gebruiken.

## <a name="prerequisites"></a>Vereisten 

De toepassing bevat de containers in blob-opslag voor een opslagaccount. Als u wilt uitproberen de code in dit artikel, moet u de volgende items: 

* Installeer [Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx) met de volgende workload:
    - Azure-ontwikkeling

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

* Een algemeen opslagaccount. Als u nog een storage-account hebt, raadpleegt u [een opslagaccount maken](storage-quickstart-create-account.md).

* Het voorbeeld in dit artikel ziet hoe u de containers in een storage-account. Om uitvoer te bekijken, voeg enkele containers BLOB storage in het storage-account voordat u begint.

## <a name="download-the-sample-application"></a>De voorbeeldtoepassing downloaden

De voorbeeldtoepassing is een in C# geschreven consoletoepassing.

Gebruik [git](https://git-scm.com/) om een kopie van de toepassing naar uw ontwikkelomgeving te downloaden. 

```bash
git clone https://github.com/Azure-Samples/storage-dotnet-rest-api-with-auth.git
```

Met deze opdracht wordt de opslagplaats naar uw lokale git-map gekloond. Open de Visual Studio-oplossing, zoekt u de map storage-dotnet-rest-api-with-auth, opent u het en dubbelklik op StorageRestApiAuth.sln. 

## <a name="what-is-rest"></a>Wat is er REST?

REST-middelen *representational state transfer*. Bekijk voor de definitie van een specifieke [Wikipedia](http://en.wikipedia.org/wiki/Representational_state_transfer).

In principe REST is voor een architectuur die u kunt gebruiken bij het aanroepen van API's of voor het doen van API's beschikbaar om te worden aangeroepen. Het is niet afhankelijk van wat op beide kanten gebeurt er en welke andere software wordt gebruikt bij het verzenden of ontvangen van de REST-aanroepen. U kunt een toepassing die wordt uitgevoerd op een Mac, Windows, Linux, een Android-telefoon of tablet, iPhone, iPod of website schrijven en dezelfde REST-API gebruiken voor al deze platforms. Gegevens kunnen worden doorgegeven in en/of uit wanneer de REST-API wordt aangeroepen. De REST-API niet van belang van het platform wordt genoemd – belangrijk is de informatie die wordt doorgegeven in de aanvraag en de gegevens die zijn opgegeven in het antwoord.

Weten over het gebruik van REST is een nuttig kwalificatie. De Azure-productteam verzenden versies regelmatig nieuwe functies. In veel gevallen de nieuwe functies zijn toegankelijk via de REST-interface, maar nog niet zijn opgehaald via de **alle** van de storage-clientbibliotheken of de gebruikersinterface (zoals de Azure portal). Als u altijd wilt gebruik van het nieuwste en beste, leren werken met REST is vereist. Als u wilt schrijven van uw eigen bibliotheek om te communiceren met Azure Storage of u wilt toegang tot Azure Storage met een programmeertaal die geen een SDK of storage-clientbibliotheek, kunt u de REST-API.

## <a name="about-the-sample-application"></a>Over de voorbeeld-App

De voorbeeld-App geeft een lijst van de containers in een storage-account. Als u inzicht in hoe de gegevens in de REST-API-documentatie overeenkomt met uw eigen code, zijn andere REST-aanroepen gemakkelijker te achterhalen. 

Als u naar kijkt de [REST API voor Blob Service](/rest/api/storageservices/Blob-Service-REST-API), ziet u de bewerkingen die u op blob-opslag uitvoeren kunt. De clientbibliotheken zijn dan geen wrappers rondom de REST API's – die het gemakkelijk voor u toegang tot opslag zonder rechtstreeks met behulp van de REST API's. Maar zoals eerder vermeld, soms wilt u de REST-API gebruiken in plaats van een storage-clientbibliotheek.

## <a name="rest-api-reference-list-containers-api"></a>Naslaginformatie over REST-API: Lijst met Containers-API

Laten we kijken naar de pagina in de REST-API-naslaginformatie voor de [ListContainers](/rest/api/storageservices/List-Containers2) bewerking, zodat u waar sommige velden afkomstig zijn weet uit in de aanvraag en respons in de volgende sectie met de code.

**Verzoekmethode**: TOEVOEGEN. Deze bewerking wordt de HTTP-methode die u als een eigenschap van het request-object opgeeft. Andere waarden voor deze bewerking zijn HEAD, PUT en DELETE, afhankelijk van de API die u aanroept.

**Aanvraag-URI**: https://myaccount.blob.core.windows.net/?comp=list  Dit is gemaakt op basis van het eindpunt van blob storage-account `http://myaccount.blob.core.windows.net` en de brontekenreeks `/?comp=list`.

[URI-parameters](/rest/api/storageservices/List-Containers2#uri-parameters): Er zijn aanvullende queryparameters die u gebruiken kunt bij het aanroepen van ListContainers. Enkele van deze parameters zijn *time-out* voor de aanroep (in seconden) en *voorvoegsel*, die wordt gebruikt voor het filteren.

Een andere nuttige parameter *maxresults:* als er meer containers dan deze waarde beschikbaar zijn, de antwoordtekst bevat een *NextMarker* element dat geeft aan dat de volgende container om terug te keren op de volgende de aanvraag. Deze functie wilt gebruiken, bieden u de *NextMarker* waarde als de *markering* parameter in de wanneer u de volgende aanvraag-URI. Wanneer u deze functie gebruikt, is het vergelijkbaar met de resultaten worden er pagina's. 

Voor het gebruik van aanvullende parameters aan de resourcetekenreeks met de waarde, zoals in dit voorbeeld toevoegen:

```
/?comp=list&timeout=60&maxresults=100
```

[Aanvraagheaders](/rest/api/storageservices/List-Containers2#request-headers)**:** Deze sectie vindt u de vereiste en optionele aanvraagheaders. Drie van de headers zijn vereist: een *autorisatie* kop *x-ms-datum* (bevat de UTC-tijd voor de aanvraag), en *x-ms-version* (geeft de versie van de REST API voor het gebruik). Inclusief *x-ms-client-request-id* in de headers is optioneel: u kunt de waarde voor dit veld instellen op Alles; ze worden geschreven naar de logboeken van storage analytics als logboekregistratie is ingeschakeld.

[Aanvraagtekst](/rest/api/storageservices/List-Containers2#request-body)**:** Er is geen aanvraagtekst voor ListContainers. Hoofdtekst van de aanvraag wordt gebruikt op al de PUT-bewerkingen tijdens het uploaden van blobs, evenals SetContainerAccessPolicy, zodat u kunt verzenden in een XML-lijst met opgeslagen toegangsbeleid om toe te passen. Opgeslagen toegangsbeleid worden besproken in het artikel [met behulp van Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md).

[Antwoordstatuscode](/rest/api/storageservices/List-Containers2#status-code)**:** Geeft aan van alle statuscodes die u wilt weten. In dit voorbeeld wordt een HTTP-statuscode 200 ok. Bekijk voor een volledige lijst van HTTP-statuscodes [Status Code Definitions](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html). Foutcodes die specifiek zijn voor de Storage REST-API's Zie [algemene REST-API-foutcodes](/rest/api/storageservices/common-rest-api-error-codes)

[Antwoordheaders](/rest/api/storageservices/List-Containers2#response-headers)**:** Het gaat hierbij *inhoudstype*; *x-ms-request-id* (aanvraag-id u doorgegeven in, indien van toepassing); *x-ms-version* (geeft de versie van de Blob-service die wordt gebruikt), en de *datum* (UTC, vertelt hoe de tijd die de aanvraag is gedaan).

[Antwoordtekst](/rest/api/storageservices/List-Containers2#response-body): Dit veld is een XML-structuur waarmee de aangevraagde gegevens geleverd. In dit voorbeeld wordt is het antwoord een lijst met containers en de bijbehorende eigenschappen.

## <a name="creating-the-rest-request"></a>Het maken van de REST-aanvraag

Een aantal opmerkingen bij de voordat u begint met – voor beveiliging bij het uitvoeren van de productie, en gebruik altijd HTTPS in plaats van HTTP-in. U moet voor deze oefening, HTTP gebruiken, zodat u de aanvraag- en -gegevens kunt bekijken. Als u de aanvraag- en informatie in de werkelijke REST-aanroepen, kunt u downloaden [Fiddler](http://www.telerik.com/fiddler) of een vergelijkbare toepassing. In Visual Studio-oplossing, de naam van het opslagaccount en de sleutel zijn vastgelegd in de klasse en de methode ListContainersAsyncREST de opslagaccountnaam en opslagaccountsleutel wordt doorgegeven aan de methoden die worden gebruikt voor het maken van de verschillende onderdelen van de REST-aanvraag . In een werkelijke toepassing, het storage-accountnaam en de sleutel zou bevinden zich in een configuratiebestand, omgevingsvariabelen, of worden opgehaald uit een Azure Key Vault.

In ons voorbeeldproject is de code voor het maken van de autorisatie-header in een afzonderlijke klasse, met het idee dat u kan de gehele klasse nemen en toe te aan uw eigen oplossing voegen en deze gebruikt 'as is." De autorisatie-header-code werkt voor de meeste REST API-aanroepen naar Azure Storage.

Als u wilt maken van de aanvraag, dit een het HttpRequestMessage-object is, gaat u naar ListContainersAsyncREST in Program.cs. De stappen voor het bouwen van de aanvraag zijn: 

* Maak de URI moet worden gebruikt voor het aanroepen van de service. 
* Het HttpRequestMessage-object maken en instellen van de nettolading. De nettolading is null voor ListContainersAsyncREST omdat we alles in niet doorgegeven.
* Voeg de aanvraagheaders voor x-ms-datum- en x-ms-version.
* De autorisatie-header ophalen en toe te voegen.

Sommige elementaire informatie die u nodig: 

*  Voor ListContainers, de **methode** is `GET`. Deze waarde is ingesteld bij het instantiëren van de aanvraag. 
*  De **resource** is het query-gedeelte van de URI die welke API wordt aangeroepen, aangeeft zodat de waarde is `/?comp=list`. Zoals eerder hebt genoteerd, wordt de resource is op de referentiepagina voor documentatie waarin de informatie over de [ListContainers API](/rest/api/storageservices/List-Containers2).
*  De URI is opgesteld door het maken van het eindpunt van Blob service voor dat opslagaccount en samenvoegen van de resource. De waarde voor **aanvraag-URI** eindigt wordt `http://contosorest.blob.core.windows.net/?comp=list`.
*  Voor ListContainers, **requestBody** null is en er zijn geen extra **headers**.

Verschillende API's mogelijk andere parameters om door te geven, zoals *ifMatch*. Een voorbeeld van waar u ifMatch kunt gebruiken is bij het aanroepen van PutBlob. In dat geval kunt u ifMatch instellen op een eTag en wordt alleen de blob bijgewerkt als de eTag die u opgeeft overeenkomt met de huidige eTag voor de blob. Als iemand anders heeft de blob sinds het ophalen van de ETag die is bijgewerkt, kan de wijziging wordt niet worden overschreven. 

Stel eerst de `uri` en de `payload`. 

```csharp
// Construct the URI. This will look like this:
//   https://myaccount.blob.core.windows.net/resource
String uri = string.Format("http://{0}.blob.core.windows.net?comp=list", storageAccountName);

// Set this to whatever payload you desire. Ours is null because 
//   we're not passing anything in.
Byte[] requestPayload = null;
```

Vervolgens exemplaar maken van de aanvraag, instellen van de methode op `GET` en het geven van de URI.

```csharp 
//Instantiate the request message with a null payload.
using (var httpRequestMessage = new HttpRequestMessage(HttpMethod.Get, uri)
{ Content = (requestPayload == null) ? null : new ByteArrayContent(requestPayload) })
{
```

Voeg de aanvraagheaders voor x-ms-datum- en x-ms-version. Deze locatie in de code is ook waar u alle aanvullende aanvraagheaders vereist voor de aanroep toevoegen. In dit voorbeeld zijn er geen extra kopteksten. Een voorbeeld van een API die wordt doorgegeven in extra kopteksten is SetContainerACL. Voor Blob-opslag, wordt een header met de naam "x-ms-blob-openbare-toegang" en de waarde voor het toegangsniveau toegevoegd.

```csharp
    // Add the request headers for x-ms-date and x-ms-version.
    DateTime now = DateTime.UtcNow;
    httpRequestMessage.Headers.Add("x-ms-date", now.ToString("R", CultureInfo.InvariantCulture));
    httpRequestMessage.Headers.Add("x-ms-version", "2017-07-29");
    // If you need any additional headers, add them here before creating
    //   the authorization header. 
```

Roep de methode die wordt gemaakt van de autorisatie-header en toe te voegen aan de aanvraagheaders. Hier ziet u hoe u de autorisatie-header later in dit artikel maakt. Naam van de methode is GetAuthorizationHeader, die u in dit codefragment kunt zien:

```csharp
    // Get the authorization header and add it.
    httpRequestMessage.Headers.Authorization = AzureStorageAuthenticationHelper.GetAuthorizationHeader(
        storageAccountName, storageAccountKey, now, httpRequestMessage);
```

Op dit moment `httpRequestMessage` bevat de REST de aanvraag is voltooid met de autorisatie-header. 

## <a name="call-the-rest-api-with-the-request"></a>De REST-API met de aanvraag aanroepen

Nu dat u de aanvraag hebt, kunt u SendAsync voor het verzenden van de REST-aanvraag aanroepen. SendAsync de API wordt aangeroepen en wordt in het antwoord. Bekijk de reactie StatusCode (200 is OK), klikt u vervolgens het parseren van het antwoord. In dit geval krijgt u een XML-lijst met containers. We bekijken de code voor het aanroepen van de methode GetRESTRequest voor het maken van de aanvraag, de aanvraag hebt uitgevoerd en vervolgens het antwoord voor de lijst met containers te onderzoeken.

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

Als u een netwerksniffer zoals [Fiddler](https://www.telerik.com/fiddler) bij het maken van de aanroep van SendAsync, ziet u de informatie van de aanvraag en reactie. Laten we eens. De naam van het opslagaccount is *contosorest*.

**Aanvraag:**

```
GET /?comp=list HTTP/1.1
```

**Aanvraagheaders:**

```
x-ms-date: Thu, 16 Nov 2017 23:34:04 GMT
x-ms-version: 2014-02-14
Authorization: SharedKey contosorest:1dVlYJWWJAOSHTCPGiwdX1rOS8B4fenYP/VrU0LfzQk=
Host: contosorest.blob.core.windows.net
Connection: Keep-Alive
```

**Status en antwoord-headers geretourneerd na de uitvoering:**

```
HTTP/1.1 200 OK
Content-Type: application/xml
Server: Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0
x-ms-request-id: 3e889876-001e-0039-6a3a-5f4396000000
x-ms-version: 2017-07-29
Date: Fri, 17 Nov 2017 00:23:42 GMT
Content-Length: 1511
```

**Hoofdtekst van antwoord (XML):** Voor ListContainers bevat deze een lijst van containers en de bijbehorende eigenschappen.

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

Nu dat u hoe u de aanvraag te maken begrijpt, roept de service en parseren van de resultaten, zien we over het maken van de autorisatie-header. Het maken van deze header is ingewikkeld, maar het goede nieuws is dat zodra u de code werkt hebt, het werkt voor alle van de Storage-Service REST API's.

## <a name="creating-the-authorization-header"></a>Het maken van de autorisatie-header

> [!TIP]
> Azure Storage ondersteunt nu de integratie van Azure Active Directory (Azure AD) voor de Blob en Queue-services (Preview). Azure AD biedt een veel eenvoudiger ervaring voor het verlenen van een aanvraag naar Azure Storage. Zie voor meer informatie over het gebruik van Azure AD te autoriseren REST-bewerkingen [verifiëren met Azure Active Directory (Preview)](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory). Zie voor een overzicht van Azure AD-integratie met Azure Storage, [verifiëren van toegang tot Azure Storage met behulp van Azure Active Directory (Preview)](storage-auth-aad.md).

Er is een artikel waarin wordt uitgelegd conceptueel gezien (geen code) uitvoeren [-verificatie voor de Azure Storage-Services](/rest/api/storageservices/Authorization-for-the-Azure-Storage-Services).
Laten we precies distilleer dat artikel omlaag naar nodig is en de code weer te geven.

Gebruik eerst een gedeelde sleutelverificatie. De autorisatie-header-indeling ziet er als volgt:

```  
Authorization="SharedKey <storage account name>:<signature>"  
```

Het handtekeningveld is een Hash-based Message Authentication Code (HMAC) gemaakt op basis van de aanvraag en wordt berekend met behulp van de algoritme SHA256 en vervolgens gecodeerd met Base64-codering. Hebt u dat? (Er vastloopt, u dit nog niet hebt, zelfs op basis van het woord *gecanoniseerd* nog.)

Dit codefragment laat zien dat de indeling van de gedeelde sleutel handtekening-tekenreeks:

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

De meeste van deze velden worden zelden gebruikt. Voor Blob-opslag geeft u de bewerking, md5, lengte van de inhoud, Gecanoniseerd Headers en Gecanoniseerd Resource. U kunt de andere leeg laten (maar plaats in de `\n` zodat deze ze zijn leeg).

Wat zijn CanonicalizedHeaders en CanonicalizedResource? Goede vraag. Wat doet gecanoniseerd in feite gemiddelde? Microsoft Word niet, zelfs herkend als een woord. Hier volgt een uitleg [Wikipedia vertelt over standaardisatie](http://en.wikipedia.org/wiki/Canonicalization): *Standaardisatie (soms standardization of normalisering) is in computerwetenschappen, een proces voor het converteren van gegevens met meer dan een mogelijke weergave in een 'standaard', 'normale' of canonieke vorm.* In normale spreken, dit betekent dat de lijst met items (zoals kopteksten in het geval van Headers Gecanoniseerd) en deze naar een vereiste indeling te standaardiseren. In principe Microsoft besloten een indeling en u wilt vergelijken.

Laten we beginnen met deze twee canonieke velden, omdat ze zijn vereist voor het maken van de autorisatie-header.

**Canonieke kopteksten**

Voor het maken van deze waarde ophalen van de headers die beginnen met 'x - ms-' en deze sorteren, en ze te formatteren in een reeks `[key:value\n]` instanties, samengevoegd tot één tekenreeks. In dit voorbeeld er de canonieke headers als volgt: 

```
x-ms-date:Fri, 17 Nov 2017 00:44:48 GMT\nx-ms-version:2017-07-29\n
```

Dit is de code die wordt gebruikt om te maken die uitvoer:

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

Dit deel van de handtekening-tekenreeks vertegenwoordigt de storage-account dat de aanvraag is gericht. Houd er rekening mee dat de aanvraag-URI is `<http://contosorest.blob.core.windows.net/?comp=list>`, met de naam van het werkelijke (`contosorest` in dit geval). In dit voorbeeld is dit het volgende geretourneerd:

```
/contosorest/\ncomp:list
```

Als u queryparameters hebt, bevat dit die ook. Hier volgt de code, die ook aanvullende queryparameters en query-parameters met meerdere waarden worden verwerkt. Houd er rekening mee dat u deze code werkt voor alle van de REST-API's, zodat u opnemen van alle mogelijkheden, wilt zelfs als de methode ListContainers niet al deze hoeft bouwt.

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

Nu de canonieke tekenreeksen zijn ingesteld, gaan we bekijken over het maken van de autorisatie-header zelf. U begint met het maken van een tekenreeks van de handtekening van het bericht in de indeling van StringToSign eerder in dit artikel worden weergegeven. Dit concept is het eenvoudiger om uit te leggen met behulp van opmerkingen in de code, hier is de laatste methode die wordt geretourneerd van de autorisatie-Header:

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

Wanneer u deze code uitvoert, ziet de resulterende MessageSignature er als volgt uit:

```
GET\n\n\n\n\n\n\n\n\n\n\n\nx-ms-date:Fri, 17 Nov 2017 01:07:37 GMT\nx-ms-version:2017-07-29\n/contosorest/\ncomp:list
```

Dit is de laatste waarde voor AuthorizationHeader:

```
SharedKey contosorest:Ms5sfwkA8nqTRw7Uury4MPHqM6Rj2nfgbYNvUKOa67w=
```

De AuthorizationHeader is de laatste kop voordat u het antwoord in de aanvraagheaders is geplaatst.

Dat omvat alles wat die u weten moet, samen met de code voor het samenstellen van een klasse die u gebruiken kunt om een aanvraag moet worden gebruikt voor het aanroepen van de Storage-Services REST API's te maken.

## <a name="how-about-another-example"></a>Hoe zit een ander voorbeeld? 

Laten we kijken hoe u de code voor het aanroepen van ListBlobs voor de container te wijzigen *container-1*. Dit is bijna identiek aan de code voor het aanbieden van containers, de enige verschillen, wordt de URI en hoe het parseren van het antwoord. 

Als u kijkt naar de referentiedocumentatie voor [ListBlobs](/rest/api/storageservices/List-Blobs), vindt u dat de methode is *ophalen* en de RequestURI is:

```
https://myaccount.blob.core.windows.net/container-1?restype=container&comp=list
```

Wijzig de code die Hiermee stelt u de URI voor de API voor ListBlobs in ListContainersAsyncREST. De containernaam van de is **container-1**.

```csharp
String uri = 
    string.Format("http://{0}.blob.core.windows.net/container-1?restype=container&comp=list",
      storageAccountName);

```

Waar u het antwoord verwerkt Wijzig vervolgens de code om op te zoeken voor blobs in plaats van containers.

```csharp
foreach (XElement container in x.Element("Blobs").Elements("Blob"))
{
    Console.WriteLine("Blob name = {0}", container.Element("Name").Value);
}
```

Wanneer u dit voorbeeld uitvoert, krijgt u resultaten als volgt uit:

**Canonieke Headers:**

```
x-ms-date:Fri, 17 Nov 2017 05:16:48 GMT\nx-ms-version:2017-07-29\n
```

**Canonieke Resource:**

```
/contosorest/container-1\ncomp:list\nrestype:container
```

**MessageSignature:**

```
GET\n\n\n\n\n\n\n\n\n\n\n\nx-ms-date:Fri, 17 Nov 2017 05:16:48 GMT
  \nx-ms-version:2017-07-29\n/contosorest/container-1\ncomp:list\nrestype:container
```

**AuthorizationHeader:**

```
SharedKey contosorest:uzvWZN1WUIv2LYC6e3En10/7EIQJ5X9KtFQqrZkxi6s=
```

De volgende waarden afkomstig zijn van [Fiddler](http://www.telerik.com/fiddler):

**Aanvraag:**

```
GET http://contosorest.blob.core.windows.net/container-1?restype=container&comp=list HTTP/1.1
```

**Aanvraagheaders:**

```
x-ms-date: Fri, 17 Nov 2017 05:16:48 GMT
x-ms-version: 2017-07-29
Authorization: SharedKey contosorest:uzvWZN1WUIv2LYC6e3En10/7EIQJ5X9KtFQqrZkxi6s=
Host: contosorest.blob.core.windows.net
Connection: Keep-Alive
```

**Status en antwoord-headers geretourneerd na de uitvoering:**

```
HTTP/1.1 200 OK
Content-Type: application/xml
Server: Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0
x-ms-request-id: 7e9316da-001e-0037-4063-5faf9d000000
x-ms-version: 2017-07-29
Date: Fri, 17 Nov 2017 05:20:21 GMT
Content-Length: 1135
```

**Hoofdtekst van antwoord (XML):** Deze XML-antwoord bevat de lijst met blobs en de bijbehorende eigenschappen. 

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

In dit artikel hebt u geleerd hoe u een aanvraag in de REST-API voor het ophalen van een lijst met containers of een lijst met blobs in een container van de blob-opslag. U hebt ook geleerd over het maken van de autorisatie-handtekening voor de REST-API-aanroep, het gebruik ervan in de REST-aanvraag en onderzoeken van het antwoord.

## <a name="next-steps"></a>Volgende stappen

* [Blob Service REST API](/rest/api/storageservices/blob-service-rest-api)
* [Bestandsservice REST-API](/rest/api/storageservices/file-service-rest-api)
* [REST-API voor Queue-Service](/rest/api/storageservices/queue-service-rest-api)

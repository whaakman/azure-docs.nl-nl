---
title: Ondersteuning voor cross-Origin Resource Sharing (CORS) | Microsoft Docs
description: Informatie over het inschakelen van CORS-ondersteuning voor de Microsoft Azure Storage-Services.
services: storage
author: cbrooksmsft
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 2/22/2017
ms.author: cbrooks
ms.subservice: common
ms.openlocfilehash: 5e65965678ed042081e4a406d3a207fb7ede299f
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58313648"
---
# <a name="cross-origin-resource-sharing-cors-support-for-the-azure-storage-services"></a>Cross-Origin Resource Sharing (CORS) ondersteuning voor de Azure Storage-Services
Vanaf versie 15-08-2013, ondersteuning de Azure storage-services Cross-Origin Resource Sharing (CORS) voor de services Blob, Table, Queue en bestand. CORS is een HTTP-functie waarmee een webtoepassing die wordt uitgevoerd in een bepaald domein te krijgen tot bronnen in een ander domein. Webbrowsers een beveiligingsbeperking wel geïmplementeerd [beleid voor zelfde oorsprong](https://www.w3.org/Security/wiki/Same_Origin_Policy) die voorkomt dat een webpagina van aanroepen van API's in een ander domein; CORS biedt een veilige manier om toe te staan van een domein (het domein van oorsprong) API's aanroepen in een ander domein. Zie de [CORS-specificatie](https://www.w3.org/TR/cors/) voor meer informatie over CORS.

U kunt CORS-regels instellen voor elk van de storage-services, door het aanroepen van [Blob Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452235.aspx), [Queue-Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452232.aspx), en [tabel-Service-eigenschappeninstellen](https://msdn.microsoft.com/library/hh452240.aspx). Nadat u de CORS-regels voor de service hebt ingesteld, klikt u vervolgens een goed geautoriseerde aanvraag indient voor de service uitgevoerd vanaf een ander domein geëvalueerd om te bepalen of het is toegestaan volgens de regels die u hebt opgegeven.

> [!NOTE]
> Houd er rekening mee dat CORS niet een verificatiemethode is. Elke aanvraag ten opzichte van een opslagresource wanneer CORS is ingeschakeld, moet een juiste verificatie-handtekening, of moet worden gemaakt op basis van een openbare-resource.
> 
> 

## <a name="understanding-cors-requests"></a>Inzicht krijgen in de CORS-aanvragen
Een CORS-aanvraag van een domein van oorsprong kan bestaan uit twee afzonderlijke aanvragen:

* Een voorbereidende aanvraag, die de CORS-beperkingen die zijn opgelegd door de service. De voorbereidende aanvraag is vereist, tenzij de aanvraag is een [eenvoudige methode](https://www.w3.org/TR/cors/), wat betekent dat GET, HEAD of POST.
* De werkelijke aanvraag ten opzichte van de gewenste resource.

### <a name="preflight-request"></a>Voorbereidende aanvraag
De voorbereidende aanvraag query's de CORS-beperkingen die zijn gemaakt voor de storage-service door de eigenaar van het account. De webbrowser (of andere gebruikersagent) stuurt een opties-aanvraag die de aanvraagheaders, methode en de oorsprong domein bevat. De storage-service evalueert de beoogde werking op basis van een vooraf geconfigureerde set CORS-regels die specificeren welke oorspronkelijk domeinen, aanvraagmethoden en aanvraagheaders mag worden opgegeven in een werkelijke aanvraag indient voor een opslagresource.

Als CORS is ingeschakeld voor de service en er is een CORS-regel die overeenkomt met de voorbereidende aanvraag, wordt de service reageert met de statuscode 200 (OK) en de vereiste Access Control-headers in het antwoord bevat.

Als CORS is niet ingeschakeld voor de service of als er geen CORS-regel komt overeen met de voorbereidende aanvraag, wordt de service reageert met de statuscode 403 (verboden).

Als de aanvraag OPTIONS niet de vereiste CORS-headers (de oorsprong en de Access-Control-verzoek-methode-headers) bevat, wordt de service reageert met de statuscode 400 (foute aanvraag).

Houd er rekening mee dat een voorbereidende aanvraag geëvalueerd op basis van de service (Blob, wachtrijen en tabellen) en niet op basis van de aangevraagde resource. Eigenaar van het account moet zijn ingeschakeld als CORS als onderdeel van de eigenschappen van het account in de volgorde voor de aanvraag te voltooien.

### <a name="actual-request"></a>Werkelijke aanvraag
Zodra de voorbereidende aanvraag is geaccepteerd en het antwoord wordt geretourneerd, wordt de browser de werkelijke aanvraag op basis van de storage-resource verzonden. De browser weigert de werkelijke direct aanvragen als de voorbereidende aanvraag wordt afgewezen.

De werkelijke aanvraag wordt behandeld als normale aanvraag indient voor de storage-service. De aanwezigheid van de header van oorsprong geeft aan dat de aanvraag een CORS-aanvraag is en de service de overeenkomende CORS-regels controleert. Als een overeenkomst wordt gevonden, worden de Access Control-headers toegevoegd aan het antwoord en verzonden naar de client. Als een overeenkomst wordt gevonden, wordt de CORS-Access-Control-headers worden niet geretourneerd.

## <a name="enabling-cors-for-the-azure-storage-services"></a>CORS inschakelen voor de Azure Storage-services
CORS-regels zijn ingesteld op het serviceniveau van de, zodat u wilt in- of uitschakelen van CORS voor elke service (Blob, Queue en Table) afzonderlijk. CORS is standaard uitgeschakeld voor elke service. U CORS hebt ingeschakeld, moet u de juiste service-eigenschappen met versie 15-08-2013 instellen of hoger, en voeg de CORS-regels toe aan de service-eigenschappen. Raadpleeg voor meer informatie over het in- of uitschakelen CORS voor een service en hoe u CORS-regels in te stellen [Blob Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452235.aspx), [Queue-Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452232.aspx), en [Table-Service instellen Eigenschappen van](https://msdn.microsoft.com/library/hh452240.aspx).

Hier volgt een voorbeeld van een enkele CORS-regel worden opgegeven via een bewerking voor het Service-eigenschappen instellen:

```xml
<Cors>    
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
        <AllowedMethods>PUT,GET</AllowedMethods>
        <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
        <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
        <MaxAgeInSeconds>200</MaxAgeInSeconds>
    </CorsRule>
<Cors>
```

Elk element dat is opgenomen in de CORS-regel wordt hieronder beschreven:

* **AllowedOrigins**: De oorspronkelijk domeinen die te maken van een aanvraag indient voor de storage-service via CORS worden toegestaan. Het oorspronkelijke domein is het domein waaruit de aanvraag afkomstig is. Houd er rekening mee dat de oorsprong moet exact hoofdlettergevoelig overeenkomt met de oorsprong die de leeftijd van de gebruiker worden verzonden naar de service. U kunt ook het jokerteken ' *' om toe te staan alle brondomeinen via CORS-aanvragen. In het bovenstaande voorbeeld wordt de http domeinen:\//www.contoso.com en http: \/ /www.fabrikam.com aanvragen aan de service met behulp van CORS kunt maken.
* **AllowedMethods**: De methoden (HTTP-aanvraagbewerkingen) die het oorspronkelijke domein voor een CORS-aanvraag gebruiken mogen. In het bovenstaande voorbeeld zijn alleen PUT en GET-aanvragen toegestaan.
* **AllowedHeaders**: De aanvraagheaders die het oorspronkelijke domein voor de CORS-aanvraag specificeert mogelijk. In het bovenstaande voorbeeld worden alle headers van Werkstroommetagegevens beginnen met x-ms-meta-data, x-ms-meta-doel en de x-ms-meta-abc zijn toegestaan. Houd er rekening mee dat het jokerteken ' *' geeft aan dat header die begint met het opgegeven voorvoegsel is toegestaan.
* **ExposedHeaders**: De reactieheaders die kunnen worden in het antwoord op de CORS-aanvraag verzonden en wordt weergegeven door de browser voor de aanvraagverlener. In het bovenstaande voorbeeld wordt de browser geïnstrueerd om beschikbaar te stellen van een koptekst die begint met x-ms-metagegevens.
* **MaxAgeInSeconds**: De maximale hoeveelheid tijd dat in een browser de voorbereidende aanvraag OPTIONS moet de cache.

De Azure storage-services opgeven voorvoegselheaders ondersteuning voor zowel de **AllowedHeaders** en **ExposedHeaders** elementen. Als u wilt toestaan dat een categorie van headers, kunt u een algemene voorvoegsel op in die categorie. Bijvoorbeeld, op te geven *x-ms-meta** als een vooraf ingestelde header maakt een regel die voldoet aan alle koppen die met x-ms-metagegevens beginnen.

De volgende beperkingen zijn van toepassing op de CORS-regels:

* U kunt maximaal vijf CORS-regels per opslagservice (Blob, Table en Queue) opgeven.
* De maximale grootte van alle CORS-regels-instellingen op de aanvraag, met uitzondering van XML-tags, mag niet groter zijn dan 2 KB.
* De lengte van een toegestane koptekst, blootgestelde header of toegestane oorsprong mag niet groter zijn dan 256 tekens.
* Toegestane kopteksten en beschikbaar gemaakte kopteksten kunnen zijn:
  * Letterlijke headers, waarin de exacte header-naam is opgegeven, zoals **x-ms-meta-verwerkte**. Een maximum van 64 letterlijke headers mag worden opgegeven in de aanvraag.
  * Headers, waar een voorvoegsel van de header is opgegeven, zoals het voorvoegsel ** x-ms-meta-data ***. Een voorvoegsel op te geven op deze manier kunt of een header die met het opgegeven voorvoegsel begint wordt aangegeven. Maximaal twee voorvoegselheaders mag worden opgegeven in de aanvraag.
* De methoden (of HTTP-woorden) opgegeven in de **AllowedMethods** element moet voldoen aan de methoden die worden ondersteund door Azure storage-service API's. Ondersteunde methoden zijn DELETE, GET, HEAD, MERGE, POST, opties en PUT.

## <a name="understanding-cors-rule-evaluation-logic"></a>Informatie over evaluatielogica CORS-regel
Wanneer een storage-service een aanvraag voorbereidende hetzij feitelijk ontvangt, evalueert deze die aanvragen op basis van de CORS-regels die u hebt gemaakt voor de service via de juiste Service-eigenschappen instellen-bewerking. CORS-regels worden geëvalueerd in de volgorde waarin ze zijn ingesteld in de hoofdtekst van de aanvraag van de Service-eigenschappen instellen-bewerking.

CORS-regels worden als volgt geëvalueerd:

1. Eerst het oorspronkelijke domein van de aanvraag wordt vergeleken met de domeinen vermeld voor de **AllowedOrigins** element. Als het oorspronkelijke domein is opgenomen in de lijst of alle domeinen zijn toegestaan met het jokerteken ' *', klikt u vervolgens regels voor evaluatie voortgezet. Als het oorspronkelijke domein niet opgenomen is, mislukt de aanvraag.
2. Vervolgens wordt de methode (of HTTP-woord) van de aanvraag wordt vergeleken met de methoden die worden vermeld in de **AllowedMethods** element. Als de methode is opgenomen in de lijst, klikt u vervolgens regels evaluatie wordt voortgezet; Als dat niet het geval is, klikt u vervolgens de aanvraag is mislukt.
3. Als de aanvraag overeenkomt met een regel in het domein van oorsprong en de methode, wordt die regel geselecteerd om te verwerken die de aanvraag en er geen verdere regels worden geëvalueerd. Voordat de aanvraag slagen kan, echter in de aanvraag opgegeven kopteksten worden gecontroleerd op basis van de headers die worden vermeld in de **AllowedHeaders** element. Als de koppen die zijn verzonden, komen niet overeen met de toegestane kopteksten, de aanvraag is mislukt.

Omdat de regels worden verwerkt in de volgorde waarin dat ze zijn aanwezig in de hoofdtekst van de aanvraag, wordt aanbevolen dat u de meest beperkende regels met betrekking tot oorsprongen eerst in de lijst opgeven, zodat deze worden eerst geëvalueerd. Geef regels die minder beperkend zijn – bijvoorbeeld een regel waarmee alle oorsprongen – aan het einde van de lijst.

### <a name="example--cors-rules-evaluation"></a>Voorbeeld: CORS-regels evaluatie
Het volgende voorbeeld ziet een gedeeltelijke aanvraagtekst voor een bewerking voor het instellen van de CORS-regels voor de storage-services. Zie [Blob Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452235.aspx), [Queue-Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452232.aspx), en [tabel-Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452240.aspx) voor meer informatie over het maken van de aanvraag.

```xml
<Cors>
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
        <AllowedMethods>PUT,HEAD</AllowedMethods>
        <MaxAgeInSeconds>5</MaxAgeInSeconds>
        <ExposedHeaders>x-ms-*</ExposedHeaders>
        <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
    </CorsRule>
    <CorsRule>
        <AllowedOrigins>*</AllowedOrigins>
        <AllowedMethods>PUT,GET</AllowedMethods>
        <MaxAgeInSeconds>5</MaxAgeInSeconds>
        <ExposedHeaders>x-ms-*</ExposedHeaders>
        <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
    </CorsRule>
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
        <AllowedMethods>GET</AllowedMethods>
        <MaxAgeInSeconds>5</MaxAgeInSeconds>
        <ExposedHeaders>x-ms-*</ExposedHeaders>
        <AllowedHeaders>x-ms-client-request-id</AllowedHeaders>
    </CorsRule>
</Cors>
```

Vervolgens kunt u overwegen de volgende CORS-aanvragen:

| Aanvraag |  |  | Antwoord |  |
| --- | --- | --- | --- | --- |
| **Methode** |**Oorsprong** |**Aanvraagheaders** |**Regel vergelijken** |**Resultaat** |
| **PUT** |http:\//www.contoso.com |x-ms-blob-content-type |Eerste regel |Geslaagd |
| **GET** |http:\//www.contoso.com |x-ms-blob-content-type |Tweede regel |Geslaagd |
| **GET** |http:\//www.contoso.com |x-ms-client-request-id |Tweede regel |Fout |

De eerste aanvraag overeenkomt met de eerste regel: het oorspronkelijke domein komt overeen met de toegestane oorsprongen, de methode komt overeen met de toegestane methoden en de header komt overeen met de toegestane kopteksten – en dus is geslaagd.

De tweede aanvraag komt niet overeen met de eerste regel, omdat de methode komt niet overeen met de toegestane methoden. Deze, echter overeenkomt met de tweede regel, dus het is gelukt.

De derde aanvraag komt overeen met de tweede regel in het domein van oorsprong en de methode, zodat er geen verdere regels worden geëvalueerd. Echter, de *x-ms-client-request-id-header* is niet toegestaan door de tweede regel, zodat de aanvraag mislukt, ondanks het feit dat de semantiek van de derde regel zouden hebben gemaakt om te slagen.

> [!NOTE]
> Hoewel dit voorbeeld ziet u een minder beperkend regel voordat u een meer beperkend, is in het algemeen de aanbevolen procedure om eerst de meest beperkende regels weer te geven.
> 
> 

## <a name="understanding-how-the-vary-header-is-set"></a>Informatie over hoe de header variëren is ingesteld
De *variëren* -header is een standaard HTTP/1.1-header die bestaan uit een set velden van de aanvraag-headers toe om de agent browser of de gebruiker over de criteria die zijn geselecteerd door de server om de aanvraag te verwerken. De *variëren* koptekst wordt hoofdzakelijk gebruikt voor het opslaan in cache op proxy's, browsers en CDN's, die deze gebruiken om te bepalen hoe het antwoord moet worden opgeslagen in de cache. Zie voor meer informatie, de specificatie voor de [variëren header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

Als de browser of een andere gebruikersagent plaatst het antwoord van een CORS-aanvraag, het oorspronkelijke domein is in de cache opgeslagen als de oorsprong toegestaan. Wanneer een tweede domein dezelfde aanvraag voor een opslagresource, uitgeeft terwijl de cache actief is, haalt de gebruikersagent het domein van oorsprong in de cache. Het tweede domein komt niet overeen met het domein in de cache, zodat de aanvraag is mislukt als deze anders zou slagen. Azure Storage wordt in bepaalde gevallen de variëren-header ingesteld op **oorsprong** om te geven van de gebruikersagent naar de volgende CORS-aanvraag verzenden naar de service wanneer het aanvragende domein wijkt af van de oorsprong in de cache.

Azure Storage-stelt de *variëren* koptekst **oorsprong** voor werkelijke GET/HEAD-aanvragen in de volgende gevallen:

* Wanneer de oorsprong van aanvraag exact overeenkomt met de toegestane oorsprong gedefinieerd door een CORS-regel. Als u een exacte overeenkomst, bevatten de CORS-regel mogelijk niet een jokerteken ' * ' teken.
* Er is geen regel die overeenkomt met de oorsprong van de aanvraag, maar CORS is ingeschakeld voor de storage-service.

In het geval waarbij een CORS-regel waarmee alle oorsprongen in een GET/HEAD-aanvraag overeenkomt met het antwoord geeft aan dat alle oorsprongen zijn toegestaan, kan de cache van de agent de volgende aanvragen van een domein van oorsprong terwijl de cache actief is.

Houd er rekening mee dat met behulp van andere methoden dan GET/HEAD-aanvragen, de header variëren, wordt niet door de storage-services worden ingesteld omdat antwoorden op deze methoden zijn niet in de cache opgeslagen door de Gebruikeragents.

De volgende tabel geeft aan hoe Azure-opslag reageren op GET/HEAD-aanvragen op basis van de eerder genoemde cases:

| Aanvraag | Instellingen van een account en het resultaat van evaluatie van de regel |  |  | Antwoord |  |  |
| --- | --- | --- | --- | --- | --- | --- |
| **Aanwezig is op verzoek-header van oorsprong** |**CORS (s) is opgegeven voor deze service** |**Overeenkomende regel bestaat waarmee alle origins(*)** |**Overeenkomende regel bestaat voor de oorsprong exacte overeenkomst** |**Antwoord bevat variëren-header ingesteld op de oorsprong** |**Antwoord bevat Access-Control-toegestaan-Origin: "*"** |**Antwoord bevat Access-Control-blootgesteld-Headers** |
| Nee |Nee |Nee |Nee |Nee |Nee |Nee |
| Nee |Ja |Nee |Nee |Ja |Nee |Nee |
| Nee |Ja |Ja |Nee |Nee |Ja |Ja |
| Ja |Nee |Nee |Nee |Nee |Nee |Nee |
| Ja |Ja |Nee |Ja |Ja |Nee |Ja |
| Ja |Ja |Nee |Nee |Ja |Nee |Nee |
| Ja |Ja |Ja |Nee |Nee |Ja |Ja |

## <a name="billing-for-cors-requests"></a>Facturering voor CORS-aanvragen
Geslaagde voorbereidend aanvragen worden in rekening gebracht als u CORS voor het gebruik van de storage-services voor uw account hebt ingeschakeld (door het aanroepen van [Blob Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452235.aspx), [Queue-Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452232.aspx), of [Instellen van tabeleigenschappen Service](https://msdn.microsoft.com/library/hh452240.aspx)). Kunt u overwegen om te beperken kosten in rekening gebracht, het **MaxAgeInSeconds** -element in uw eigen CORS-regels op een grote waarde zodat de gebruikersagent de verzoek in de cache opslaat.

Mislukte voorbereidende aanvragen wordt niet in rekening gebracht.

## <a name="next-steps"></a>Volgende stappen
[Blob-Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452235.aspx)

[Eigenschappen van de Queue-Service instellen](https://msdn.microsoft.com/library/hh452232.aspx)

[Tabel-Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452240.aspx)

[W3C Cross-Origin Resource Sharing specificatie](https://www.w3.org/TR/cors/)


---
title: Ondersteuning voor cross-Origin-Resource delen (CORS) | Microsoft Docs
description: Informatie over het inschakelen van CORS-ondersteuning voor de Microsoft Azure Storage-Services.
services: storage
documentationcenter: .net
author: cbrooksmsft
manager: carmonm
editor: tysonn
ms.assetid: a0229595-5b64-4898-b8d6-fa2625ea6887
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 2/22/2017
ms.author: cbrooks
ms.openlocfilehash: 8d189d3ec3e6081dd37b912824f287cd75f39b35
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="cross-origin-resource-sharing-cors-support-for-the-azure-storage-services"></a>Cross-Origin-Resource delen (CORS) ondersteuning voor de Azure Storage-Services
Vanaf versie 2013-08-15, ondersteuning de Azure storage-services Cross-Origin-Resource delen (CORS) voor de services Blob, Table, wachtrijen en -bestand. CORS is een HTTP-functie waarmee een webtoepassing in een domein met toegang tot bronnen in een ander domein. Webbrowsers implementeren een beveiligingsbeperkingen bekend als [dezelfde oorsprong beleid](http://www.w3.org/Security/wiki/Same_Origin_Policy) dat voorkomt dat een webpagina van aanroepen API's in een ander domein; CORS biedt een veilige manier om toe te staan van een domein (het domein van de oorsprong) om aan te roepen API's in een ander domein. Zie de [CORS-specificatie](http://www.w3.org/TR/cors/) voor meer informatie over CORS.

U kunt CORS-regels afzonderlijk instellen voor elk van de storage-services door het aanroepen van [Blob-Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452235.aspx), [Queue-Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452232.aspx), en [tabel-Service-eigenschappeninstellen](https://msdn.microsoft.com/library/hh452240.aspx). Als u de CORS-regels voor de service hebt ingesteld, wordt een correct geverifieerde aanvraag ten opzichte van de service uit een ander domein worden geëvalueerd om te bepalen of het is toegestaan volgens de regels die u hebt opgegeven.

> [!NOTE]
> Houd er rekening mee dat CORS niet een verificatiemethode is. Elk verzoek ten opzichte van een opslagresource wanneer CORS is ingeschakeld moet een handtekening van de juiste verificatiegegevens, of moet worden gemaakt op basis van een openbare resource.
> 
> 

## <a name="understanding-cors-requests"></a>Understanding CORS aanvragen
Een CORS-aanvraag van een brondomein kan bestaan uit twee afzonderlijke aanvragen:

* Een voorbereidende aanvraag, die de CORS-beperkingen die zijn opgelegd door de service. De voorbereidende aanvraag is vereist, tenzij de aanvraagmethode is een [eenvoudige methode](http://www.w3.org/TR/cors/), wat betekent dat GET, HEAD of POST.
* De werkelijke aanvraag ten opzichte van de gewenste resource.

### <a name="preflight-request"></a>Voorbereidende aanvraag
De voorbereidende aanvraag query's de CORS-beperkingen die zijn gemaakt voor de storage-service door de eigenaar van het account. De webbrowser (of andere gebruikersagent) verzendt een aanvraag voor opties die de aanvraagheaders, methode en de oorsprong domein bevat. De storage-service evalueert de bewerking op basis van een vooraf geconfigureerde reeks CORS-regels die opgeeft welke domeinen, aanvraagmethoden en aanvraagheaders kunnen worden opgegeven voor een werkelijke aanvraag tegen een opslagresource.

Als u CORS voor de service is ingeschakeld en er is een CORS-regel die overeenkomt met de voorbereidende aanvraag, wordt de service reageert met de statuscode 200 (OK) en bevat de vereiste Access Control-kopteksten in het antwoord.

Als CORS is niet ingeschakeld voor de service of er geen regel CORS overeenkomt met de voorbereidende aanvraag, wordt de service reageren met de statuscode 403 (verboden).

Als de opties-aanvraag bevat niet de vereiste CORS-headers (de oorsprong en Access Control-aanvraag methode headers), wordt de service reageert met de statuscode 400 (ongeldige aanvraag).

Houd er rekening mee dat een aanvraag voor voorbereidende wordt geëvalueerd tegen de service (Blob, wachtrijen en tabellen) en niet de aangevraagde bron. Eigenaar van het account moet CORS als onderdeel van de eigenschappen van het account voor de aanvraag om te slagen hebt ingeschakeld.

### <a name="actual-request"></a>Werkelijke aanvraag
Zodra de voorbereidende aanvraag is geaccepteerd en de respons wordt geretourneerd, wordt in de browser de werkelijke aanvraag tegen de opslagbronnen verzenden. De browser weigert de werkelijke onmiddellijk aanvragen als de voorbereidende aanvraag wordt afgewezen.

De werkelijke aanvraag wordt beschouwd als normale-aanvraag in voor de storage-service. De aanwezigheid van de header van oorsprong geeft aan dat de aanvraag een aanvraag voor CORS is en de service de overeenkomende CORS-regels controleert. Als een overeenkomst wordt gevonden, wordt de Access Control-headers zijn toegevoegd aan het antwoord, en terug naar de client verzonden. Als een overeenkomst niet gevonden is, worden de CORS-toegangsbeheer headers niet geretourneerd.

## <a name="enabling-cors-for-the-azure-storage-services"></a>Inschakelen van CORS voor de Azure Storage-services
CORS-regels op het serviceniveau van de zijn zo ingesteld dat u wilt in- of uitschakelen van CORS voor elke service (Blob, Queue en tabel) afzonderlijk. CORS is standaard uitgeschakeld voor elke service. Voor het CORS zijn ingeschakeld, moet u de juiste service-eigenschappen met versie 2013-08-15 instellen of hoger, en CORS-regels toevoegen aan de service-eigenschappen. Voor meer informatie over het in- of uitschakelen CORS voor een service en hoe u CORS-regels, raadpleegt u [Blob-Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452235.aspx), [Queue-Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452232.aspx), en [Tabelservice instellen Eigenschappen](https://msdn.microsoft.com/library/hh452240.aspx).

Hier volgt een voorbeeld van een enkele CORS-regel worden opgegeven via de bewerking van een Service-eigenschappen instellen:

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

Elk element opgenomen in de CORS-regel wordt hieronder beschreven:

* **AllowedOrigins**: de domeinen die zijn toegestaan voor het maken van een aanvraag in voor de storage-service via CORS. Het brondomein is het domein waaruit de aanvraag afkomstig is. Houd er rekening mee dat de oorsprong moet exact hoofdlettergevoelig overeenkomt met de oorsprong waarmee de leeftijd van de gebruiker naar de service worden verzonden. U kunt ook het jokerteken ' *' om toe te staan alle domeinen aanvragen via CORS. In het bovenstaande voorbeeld wordt de domeinen [http://www.contoso.com](http://www.contoso.com) en [http://www.fabrikam.com](http://www.fabrikam.com) aanvragen op basis van de service met behulp van CORS kunt maken.
* **AllowedMethods**: de methoden die van het brondomein voor een CORS-aanvraag gebruikmaken mogelijk (HTTP-aanvraag woorden). In het bovenstaande voorbeeld worden alleen PUT en GET-aanvragen toegestaan.
* **AllowedHeaders**: de aanvraagheaders die het brondomein, voor de CORS-aanvraag opgeven kan. In het bovenstaande voorbeeld worden alle metagegevens headers beginnen met x-ms-metagegevens, x-ms-meta-doel en de x-ms-meta-abc toegestaan. Houd er rekening mee dat het jokerteken ' *' geeft aan dat een koptekst die begint met het opgegeven voorvoegsel is toegestaan.
* **ExposedHeaders**: de antwoordheaders die mogelijk in het antwoord op de CORS-aanvraag verzonden en die worden weergegeven door de browser van de uitgever van de aanvraag. In het bovenstaande voorbeeld wordt de browser geïnstrueerd om een koptekst die begint met x-ms-meta weer te geven.
* **MaxAgeInSeconds**: de maximale hoeveelheid tijd dat de voorbereidende opties in een browser moet cache aanvragen.

De Azure storage-services opgeven voorvoegsels headers ondersteuning voor zowel de **AllowedHeaders** en **ExposedHeaders** elementen. Als u wilt toestaan dat een categorie-headers, kunt u een algemene voorvoegsel op in die categorie opgeven. Bijvoorbeeld, geven *x-ms-meta** als een voorvoegsels koptekst bepaalt een regel die overeenkomen met alle headers die met x-ms-metagegevens beginnen.

De volgende beperkingen zijn van toepassing op CORS-regels:

* U kunt maximaal vijf CORS-regels per storage-service (Blob, Table en Queue) opgeven.
* De maximale grootte van alle CORS regelinstellingen op de aanvraag, met uitzondering van XML-labels mag niet langer zijn dan 2 KB.
* De lengte van een toegestane koptekst, blootgestelde koptekst of toegestane oorsprong mag niet langer zijn dan 256 tekens.
* Toegestane headers en blootgestelde headers kunnen zijn:
  * Letterlijke kopteksten, waarbij de exacte header-naam is opgegeven, zoals **x-ms-meta-verwerkte**. Een maximum van 64 letterlijke headers worden opgegeven voor de aanvraag.
  * Headers, waarbij een voorvoegsel van de header is opgegeven, zoals het voorvoegsel **x-ms-meta-data***. Een voorvoegsel geven op deze manier kunt of een header die met het opgegeven voorvoegsel begint beschrijft. Maximaal twee voorvoegsels headers kan worden opgegeven voor de aanvraag.
* De methoden (of HTTP-woorden) opgegeven in de **AllowedMethods** element moet voldoen aan de methoden die wordt ondersteund door Azure storage-service API's. Ondersteunde methoden zijn verwijderd, GET, HEAD, samenvoegen, POST, opties en PUT.

## <a name="understanding-cors-rule-evaluation-logic"></a>Understanding CORS regellogica evaluatie
Wanneer een storage-service een voorbereidende of werkelijke aanvraag ontvangt, resulteert deze aanvragen op basis van de CORS-regels die u hebt vastgesteld voor de service via de juiste bewerking van de Service-eigenschappen instellen. CORS-regels worden geëvalueerd in de volgorde waarin ze zijn ingesteld in de aanvraag van de bewerking van de Service-eigenschappen instellen.

CORS-regels worden als volgt geëvalueerd:

1. Eerst het domein van de oorsprong van de aanvraag wordt vergeleken met de domeinen die worden vermeld voor de **AllowedOrigins** element. Als het brondomein is opgenomen in de lijst of alle domeinen zijn toegestaan met het jokerteken ' *', vervolgens regels evaluatie voortgezet. Als het domein van de oorsprong niet opgenomen is, mislukt de aanvraag.
2. Vervolgens wordt de methode (of HTTP-term) van de aanvraag wordt vergeleken met de methoden die in de **AllowedMethods** element. Als de methode is opgenomen in de lijst, is de evaluatie van regels wordt voortgezet; Als dat niet het geval is, mislukt de aanvraag.
3. Als de aanvraag overeenkomt met een regel in het domein van de oorsprong en de bijbehorende methode, wordt die regel geselecteerd om te verwerken die de aanvraag en er geen verdere regels worden geëvalueerd. Voordat de aanvraag kan worden voltooid, maar eventuele headers die is opgegeven voor de aanvraag worden gecontroleerd tegen de headers die worden vermeld in de **AllowedHeaders** element. Als de headers verzonden komen niet overeen met de toegestane headers, mislukt de aanvraag.

Aangezien de regels worden verwerkt in de volgorde waarin dat ze zijn aanwezig in de aanvraagtekst, wordt aanbevolen dat u de meest beperkende regels met betrekking tot oorsprongen eerst in de lijst opgeven zodat deze worden eerst geëvalueerd. Geef regels die minder beperkend zijn – bijvoorbeeld een regel waarmee alle oorsprongen – aan het einde van de lijst.

### <a name="example--cors-rules-evaluation"></a>Voorbeeld: CORS regels evaluatie
Het volgende voorbeeld ziet een gedeeltelijke aanvraagtekst voor een bewerking CORS-regels voor de storage-services in te stellen. Zie [Blob-Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452235.aspx), [Queue-Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452232.aspx), en [tabel-Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452240.aspx) voor meer informatie over het samenstellen van de aanvraag.

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
| **Methode** |**Oorsprong** |**Aanvraagheaders** |**Regel overeen** |**Resultaat** |
| **PLAATSEN** |http://www.contoso.com |x-ms-blob-content-type |Eerste regel |Geslaagd |
| **TOEVOEGEN** |http://www.contoso.com |x-ms-blob-content-type |Tweede regel |Geslaagd |
| **TOEVOEGEN** |http://www.contoso.com |x-ms-client-request-id |Tweede regel |Fout |

De eerste aanvraag komt overeen met de eerste regel – het brondomein overeenkomt met de toegestane oorsprongen, de methode komt overeen met de toegestane methoden en de header komt overeen met de toegestane headers – en dus slaagt.

De tweede aanvraag komt niet overeen met de eerste regel omdat de methode komt niet overeen met de toegestane methoden. Dit het geval is, echter overeenkomen met de tweede regel zodat deze is voltooid.

De derde aanvraag komt overeen met de tweede regel in het brondomein en methode, zodat er geen verdere regels worden geëvalueerd. Echter, de *header x-ms-client-request-id* is niet toegestaan door de tweede regel op, zodat de aanvraag is mislukt, ondanks het feit dat de semantiek van de derde regel zouden hebben gemaakt om te slagen.

> [!NOTE]
> Hoewel dit voorbeeld ziet u een minder beperkend regel voordat een meer beperkend, is in het algemeen de aanbevolen procedure voor het eerst een lijst met de meest beperkende regels.
> 
> 

## <a name="understanding-how-the-vary-header-is-set"></a>Begrijpen hoe de Vary-kop is ingesteld
De *Vary* -header is een standaard HTTP/1.1-header die bestaan uit een set velden van de aanvraag-header toe om de agent browser of een gebruiker over de criteria die zijn geselecteerd door de server om de aanvraag te verwerken. De *Vary* koptekst wordt hoofdzakelijk gebruikt voor het opslaan van proxy's, browsers en CDN die gebruiken om te bepalen hoe het antwoord moet worden opgeslagen in de cache. Zie voor meer informatie de specificatie voor de [Vary-kop](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

Wanneer de browser of een andere gebruikersagent plaatst het antwoord van een aanvraag CORS, het brondomein in cache wordt opgeslagen als de toegestane oorsprong. Wanneer een tweede domein dezelfde aanvraag voor een opslagresource uitgeeft terwijl de cache actief is, haalt de gebruikersagent het brondomein in de cache. Het tweede domein komt niet overeen met het domein in de cache, zodat de aanvraag mislukt wanneer het anders zou slagen. In bepaalde gevallen, Azure Storage Vary-kop ingesteld op **oorsprong** instrueren de gebruikersagent voor de volgende CORS-aanvraag naar de service te verzenden wanneer het domein van de aanvragende van de oorsprong in de cache verschilt.

Azure Storage stelt de *Vary* koptekst tot **oorsprong** voor werkelijke GET/HEAD-aanvragen in de volgende gevallen:

* Wanneer de aanvraag-oorsprong exact overeenkomt met de toegestane oorsprong gedefinieerd door een regel CORS. Als u een exacte overeenkomst, de CORS-regel mag bevatten een jokerteken ' * ' teken.
* Er is geen regel die overeenkomt met de oorsprong van de aanvraag, maar CORS is ingeschakeld voor de storage-service.

Het antwoord geeft in het geval waarbij een GET/HEAD-aanvraag komt overeen met een CORS-regel waarmee alle oorsprongen aan dat alle oorsprongen zijn toegestaan en de agent gebruikerscache volgende aanvragen van een domein van de oorsprong wordt toestaan terwijl de cache actief is.

Houd er rekening mee dat voor andere methoden dan GET/HEAD-aanvragen, de Vary-kop, wordt niet door de storage-services worden ingesteld omdat de antwoorden op deze methoden zijn niet in de cache opgeslagen door Gebruikeragents.

De volgende tabel geeft aan hoe Azure-opslag reageert op GET/HEAD-aanvragen op basis van de eerder genoemde gevallen:

| Aanvraag | Instellingen van een account en het resultaat van evaluatie van de regels |  |  | Antwoord |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Oorsprong header aanwezig is op verzoek** |**CORS (s) is opgegeven voor deze service** |**Overeenkomende regel bestaat waarmee alle oorsprongen (*)** |**Overeenkomende regel bestaat voor de oorsprong van de exacte overeenkomst** |**Antwoord bevat Vary-kop is ingesteld op de oorsprong** |**Antwoord bevat de Access Control-toegestaan-oorsprong: '*'** |**Antwoord bevat Access-Control-blootgesteld-kopteksten** |
| Nee |Nee |Nee |Nee |Nee |Nee |Nee |
| Nee |Ja |Nee |Nee |Ja |Nee |Nee |
| Nee |Ja |Ja |Nee |Nee |Ja |Ja |
| Ja |Nee |Nee |Nee |Nee |Nee |Nee |
| Ja |Ja |Nee |Ja |Ja |Nee |Ja |
| Ja |Ja |Nee |Nee |Ja |Nee |Nee |
| Ja |Ja |Ja |Nee |Nee |Ja |Ja |

## <a name="billing-for-cors-requests"></a>Facturering voor CORS aanvragen
Geslaagde preflight aanvragen worden gefactureerd als u CORS voor een van de storage-services voor uw account hebt ingeschakeld (door het aanroepen van [Blob-Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452235.aspx), [Queue-Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452232.aspx), of [Tabel-Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452240.aspx)). Kunt u overwegen om te beperken kosten, de **MaxAgeInSeconds** -element in uw CORS regels op een hoge waarde, zodat de gebruikersagent de aanvraag slaat.

Mislukte voorbereidende aanvragen worden niet gefactureerd.

## <a name="next-steps"></a>Volgende stappen
[Eigenschappen van de Blob-Service instellen](https://msdn.microsoft.com/library/hh452235.aspx)

[Wachtrij-Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452232.aspx)

[Tabel-Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452240.aspx)

[W3C Cross-Origin Resource Sharing specificatie](http://www.w3.org/TR/cors/)


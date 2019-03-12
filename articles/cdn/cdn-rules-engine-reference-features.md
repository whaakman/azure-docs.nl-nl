---
title: Azure CDN de regelengine functies | Microsoft Docs
description: Naslagdocumentatie voor Azure CDN regels engine functies.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: magattus
ms.openlocfilehash: d6d898b93af6c03b313ec2340eb076de85877155
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57530991"
---
# <a name="azure-cdn-rules-engine-features"></a>Functies Azure CDN-regelengine
Dit artikel vindt u gedetailleerde beschrijvingen van de beschikbare functies voor Azure Content Delivery Network (CDN) [regels-Engine](cdn-rules-engine.md).

Het derde deel van een regel is de functie. Een functie bepaalt het type actie dat wordt toegepast op het aanvraagtype die wordt geïdentificeerd door een set criteria voor overeenkomst.

## <a name="access-features"></a>Toegang tot functies

Deze functies zijn ontworpen voor het beheren van toegang tot inhoud.

Name | Doel
-----|--------
[Weigeren van toegang (403)](#deny-access-403) | Hiermee bepaalt u of alle aanvragen zijn afgewezen met een 403 verboden-antwoord.
[Auth-token](#token-auth) | Hiermee bepaalt u of op tokens gebaseerde verificatie moet worden toegepast op een aanvraag.
[Weigering van token autorisatiecode](#token-auth-denial-code) | Bepaalt het type van de reactie die aan een gebruiker wordt geretourneerd wanneer een aanvraag wordt geweigerd vanwege de verificatie op basis van tokens.
[Token Authentication URL hoofdlettergevoeligheid negeren](#token-auth-ignore-url-case) | Bepaalt of de URL-vergelijkingen gemaakt door verificatie op basis van tokens hoofdlettergevoelig zijn.
[De Auth Parameter-token](#token-auth-parameter) | Hiermee bepaalt u of de queryreeks-parameter op tokens gebaseerde verificatie moet worden gewijzigd.


## <a name="caching-features"></a>Functies voor opslaan in cache

Deze functies zijn ontworpen om aan te passen wanneer en hoe inhoud wordt opgeslagen in de cache.

Name | Doel
-----|--------
[Bandbreedte-Parameters](#bandwidth-parameters) | Bepaalt of bandbreedte bandbreedtebeperking parameters (bijvoorbeeld ec_rate en ec_prebuf) actief zijn.
[Bandbreedtebeperking](#bandwidth-throttling) | Beperkt de bandbreedte voor de respons van de point-of-presence (POP).
[Bypass Cache](#bypass-cache) | Hiermee bepaalt u of de aanvraag opslaan in cache omzeilen moet.
[Cache-Control Header Treatment](#cache-control-header-treatment) | Hiermee bepaalt u de generatie van `Cache-Control` headers met het pop-server als de functie voor externe Max-Age actief is.
[Cache-sleutel Query-tekenreeks](#cache-key-query-string) | Bepaalt of de cache-sleutel bevat of niet van toepassing op queryreeksparameters die zijn gekoppeld aan een aanvraag.
[Cache-sleutel opnieuw schrijven](#cache-key-rewrite) | Herschrijft de cache-sleutel die is gekoppeld aan een aanvraag.
[Opvulling van de Cache voltooien](#complete-cache-fill) | Hiermee wordt bepaald wat er gebeurt wanneer een aanvraag resulteert in een gedeeltelijke Cachemisser op een pop-server.
[Bestandstypen comprimeren](#compress-file-types) | Definieert de bestandsindelingen voor de bestanden die gecomprimeerd zijn op de server.
[Standaard interne Max-Age](#default-internal-max-age) | Bepaalt de standaardintervaltijd van max-age voor pop-server naar de oorsprong server cache hervalidatie.
[Koptekst behandeling verloopt](#expires-header-treatment) | Hiermee bepaalt u de generatie van `Expires` headers met een pop-server als de functie externe Max-Age actief is.
[Externe Max-Age](#external-max-age) | Bepaalt de max-age-interval voor de browser op hervalidatie van POP-cache.
[Force Internal Max-Age](#force-internal-max-age) | Bepaalt de max-age-interval voor pop-server naar de oorspronkelijke server cache hervalidatie.
[H.264-ondersteuning (http-progressief downloaden)](#h264-support-http-progressive-download) | Hiermee bepaalt u de typen H.264-bestandsindelingen die kunnen worden gebruikt voor het streamen van inhoud.
[Aanvraag voor Honor No-Cache](#honor-no-cache-request) | Bepaalt of een HTTP-client geen cache-aanvragen worden doorgestuurd naar de oorspronkelijke server.
[Negeren van oorsprong No-Cache](#ignore-origin-no-cache) | Hiermee bepaalt u of het CDN bepaalde richtlijnen die worden aangeleverd vanuit een originele server negeert.
[Ongeldig bereiken negeren](#ignore-unsatisfiable-ranges) | Hiermee bepaalt u het antwoord dat wordt geretourneerd naar clients wanneer een aanvraag statuscode 416 gevraagde bereik niet geldig genereert.
[Interne Max-verouderd](#internal-max-stale) | Besturingselementen hoe lang na de verlooptijd van de normale een activum in de cache kan worden geleverd vanuit een pop-server wanneer de pop-server niet kan valideren van de cache asset met de oorspronkelijke server.
[Partial Cache Sharing](#partial-cache-sharing) | Bepaalt of een aanvraag voor gedeeltelijk in de cache inhoud kunt genereren.
[De inhoud in cache prevalidate](#prevalidate-cached-content) | Bepaalt of de inhoud in cache in aanmerking komen voor vroege hervalidatie voordat de TTL verloopt.
[Bestanden opslaan in Cache nul bytes vernieuwen](#refresh-zero-byte-cache-files) | Bepaalt hoe van de client van een HTTP-aanvraag voor een asset 0-byte-cache door de POP's worden verwerkt.
[Statuscodes gecachet kan worden ingesteld](#set-cacheable-status-codes) | Definieert de set statuscodes die tot de inhoud in cache leiden kunnen.
[Verouderde Contentlevering bij fout](#stale-content-delivery-on-error) | Bepaalt of verlopen in de cache inhoud wordt geleverd als een fout optreedt tijdens de hervalidatie van de cache of bij het ophalen van de aangevraagde inhoud op de bronserver van de klant.
[Tijdens het Revalidate verlopen](#stale-while-revalidate) | Verbetert de prestaties doordat de POP's voor het bieden van verouderde client voor de aanvrager terwijl hervalidatie doorgevoerd wordt.

## <a name="comment-feature"></a>Functie voor opmerkingen

Deze functie is ontworpen om aanvullende gegevens in een regel.

Name | Doel
-----|--------
[Opmerking](#comment) | Hiermee kunt een opmerking in een regel worden toegevoegd.
 
## <a name="header-features"></a>Header-functies

Deze functies zijn ontworpen voor het toevoegen, wijzigen of verwijderen van headers van de aanvraag of antwoord.

Name | Doel
-----|--------
[Leeftijd Response-Header](#age-response-header) | Bepaalt of een leeftijd response-header is opgenomen in het antwoord verzonden naar de aanvrager.
[Fouten opsporen in Cache antwoordheaders](#debug-cache-response-headers) | Bepaalt of een antwoord op de response-header X-EG-Debug, die informatie over het cache-beleid voor de aangevraagde asset bevat kan bevatten.
[Client-aanvraagheader wijzigen](#modify-client-request-header) | Overschreven, wordt toegevoegd of verwijderd van een koptekst van een aanvraag.
[Client-Reactieheader wijzigen](#modify-client-response-header) | Overschreven, wordt toegevoegd of verwijderd van een koptekst van een antwoord.
[Instellen van aangepaste IP-Header Client](#set-client-ip-custom-header) | Hiermee kunt u het IP-adres van de aanvragende client moet worden toegevoegd aan de aanvraag als een voor aangepaste aanvraagheader.


## <a name="logging-features"></a>Functies voor logboekregistratie

Deze functies zijn ontworpen om aan te passen de gegevens die zijn opgeslagen in onbewerkte logboekbestanden.

Name | Doel
-----|--------
[Het aangepaste logboekveld 1](#custom-log-field-1) | Bepaalt de indeling en de inhoud die is toegewezen aan het aangepaste logboek-veld in een onbewerkte logboekbestand.
[Logboek-queryreeks](#log-query-string) | Bepaalt of een queryreeks samen met de URL in toegang tot logboeken worden opgeslagen.


<!---
## Optimize

These features determine whether a request will undergo the optimizations provided by Edge Optimizer.

Name | Purpose
-----|--------
Edge Optimizer | Determines whether Edge Optimizer can be applied to a request.
Edge Optimizer – Instantiate Configuration | Instantiates or activates the Edge Optimizer configuration associated with a site.

### Edge Optimizer
**Purpose:** Determines whether Edge Optimizer can be applied to a request.

If this feature has been enabled, then the following criteria must also be met before the request will be processed by Edge Optimizer:

- The requested content must use an edge CNAME URL.
- The edge CNAME referenced in the URL must correspond to a site whose configuration has been activated in a rule.

This feature requires the ADN platform and the Edge Optimizer feature.

Value|Result
-|-
Enabled|Indicates that the request is eligible for Edge Optimizer processing.
Disabled|Restores the default behavior. The default behavior is to deliver content over the ADN platform without any additional processing.

**Default Behavior:** Disabled
 

### Edge Optimizer - Instantiate Configuration
**Purpose:** Instantiates or activates the Edge Optimizer configuration associated with a site.

This feature requires the ADN platform and the Edge Optimizer feature.

Key information:

- Instantiation of a site configuration is required before requests to the corresponding edge CNAME can be processed by Edge Optimizer.
- This instantiation only needs to be performed a single time per site configuration. A site configuration that has been instantiated will remain in that state until the Edge Optimizer – Instantiate Configuration feature that references it is removed from the rule.
- The instantiation of a site configuration does not mean that all requests to the corresponding edge CNAME will automatically be processed by Edge Optimizer. The Edge Optimizer feature determines whether an individual request will be processed.

If the desired site does not appear in the list, then you should edit its configuration and verify that the Active option has been marked.

**Default Behavior:** Site configurations are inactive by default.
--->

## <a name="origin-features"></a>Functies van de oorsprong

Deze functies zijn ontworpen om te bepalen hoe het CDN communiceert met een oorspronkelijke server.

Name | Doel
-----|--------
[Maximum aantal Keep-Alive-aanvragen](#maximum-keep-alive-requests) | Definieert het maximum aantal aanvragen voor een Keep-Alive-verbinding wordt gesloten.
[Speciale proxy-kopteksten](#proxy-special-headers) | Definieert de set met CDN-specifieke aanvraagheaders die vanuit een POP worden doorgestuurd naar een bronserver.


## <a name="specialty-features"></a>Gespecialiseerde functies

Deze functies bieden geavanceerde functionaliteit voor ervaren gebruikers.

Name | Doel
-----|--------
[Gecachet kan worden HTTP-methoden](#cacheable-http-methods) | Bepaalt de reeks aanvullende HTTP-methoden die in de cache kan worden opgeslagen in het netwerk.
[Cache-instantie aanvraaggrootte](#cacheable-request-body-size) | Hiermee definieert u de drempelwaarde voor het bepalen of een POST-antwoord kan worden opgeslagen in de cache.
[Variabele van de gebruiker](#user-variable) | Alleen voor intern gebruik.

 
## <a name="url-features"></a>URL-functies

Deze functies kunnen een aanvraag om te worden omgeleid of herschreven naar een andere URL.

Name | Doel
-----|--------
[Omleidingen volgen](#follow-redirects) | Hiermee bepaalt u of aanvragen kunnen worden omgeleid naar de hostnaam die is gedefinieerd in de locatieheader die is geretourneerd door een klant-bronserver.
[URL-omleiding](#url-redirect) | Leidt aanvragen via de Location-header.
[Herschrijven van URL](#url-rewrite)  | Herschrijft de aanvraag-URL.



## <a name="azure-cdn-rules-engine-features-reference"></a>Functies naslaginformatie over Azure CDN de regelengine

---
### <a name="age-response-header"></a>Leeftijd Response-Header
**Doel**: Bepaalt of een leeftijd response-header is opgenomen in het antwoord verzonden naar de aanvrager.
Value|Resultaat
--|--
Ingeschakeld | De leeftijd response-header is opgenomen in het antwoord verzonden naar de aanvrager.
Uitgeschakeld | De leeftijd response-header is uitgesloten van het antwoord verzonden naar de aanvrager.

**Standaardgedrag**: Uitgeschakeld.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="bandwidth-parameters"></a>Bandbreedte-Parameters
**Doel:** bepaalt of bandbreedte bandbreedtebeperking parameters (bijvoorbeeld ec_rate en ec_prebuf) actief zijn.

Bandbreedtebeperking parameters van bandbreedte te bepalen of de overdrachtssnelheid van gegevens voor een aanvraag van een client zijn beperkt tot een aangepaste tarief.

Value|Resultaat
--|--
Ingeschakeld|Hiermee kunt u de POP's aanvragen voor bandbreedteregeling in acht neemt.
Uitgeschakeld|Zorgt ervoor dat de POP's bandbreedtebeperking parameters negeren. De gevraagde inhoud normaal gesproken wordt geleverd (dat wil zeggen, zonder bandbreedtebeperking).

**Standaardgedrag:** ingeschakeld.
 
[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="bandwidth-throttling"></a>Bandbreedtebeperking
**Doel:** beperkt de bandbreedte voor de respons van de POP's.

Beide van de volgende opties moeten worden gedefinieerd om in te stellen correct bandbreedtebeperking.

Optie|Description
--|--
Kilobytes per seconde|Deze optie instelt op de maximale bandbreedte (in Kb per seconde) die kan worden gebruikt voor het leveren van het antwoord.
Prebuf seconden|Deze optie instelt op het aantal seconden voor de POP's moet worden gewacht tot de bandbreedte wordt beperkt. Het doel van deze periode van onbeperkte bandbreedte is om te voorkomen dat een mediaspeler ondervindt problemen met de haperend of buffering door bandbreedtebeperking.

**Standaardgedrag:** uitgeschakeld.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="bypass-cache"></a>Cache overslaan
**Doel:** bepaalt of de aanvraag opslaan in cache omzeilen moet.

Value|Resultaat
--|--
Ingeschakeld|Zorgt ervoor dat alle aanvragen te vallen door op de oorspronkelijke server, zelfs als de inhoud is eerder in de cache op de POP's.
Uitgeschakeld|Zorgt ervoor dat pop's aan cache activa op basis van de cache-beleid dat is gedefinieerd in de antwoordheaders.

**Standaardgedrag:**

- **Grote HTTP:** uitgeschakeld

<!---
- **ADN:** Enabled

--->

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="cacheable-http-methods"></a>Gecachet kan worden HTTP-methoden
**Doel:** bepaalt de reeks aanvullende HTTP-methoden die in de cache kan worden opgeslagen in het netwerk.

Belangrijke informatie:

- Deze functie wordt ervan uitgegaan dat GET antwoorden altijd worden gecached. Als gevolg hiervan moet de GET HTTP-methode niet worden opgenomen bij het instellen van deze functie.
- Deze functie ondersteunt alleen de HTTP POST-methode. POST antwoord in cache opslaan door deze functie in te stellen `POST`.
- Standaard worden alleen aanvragen waarvan de hoofdtekst kleiner dan 14 Kb is het in cache opgeslagen. De gecachet kan worden aanvraag hoofdtekst van de grootte van functie gebruiken om in te stellen van de grootte van de hoofdtekst van de maximale aanvraag.

**Standaardgedrag:** alleen GET-antwoorden in de cache zijn opgeslagen.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="cacheable-request-body-size"></a>Cache-instantie aanvraaggrootte
**Doel:** definieert de drempelwaarde voor het bepalen of een POST-antwoord kan worden opgeslagen in de cache.

Deze drempelwaarde wordt bepaald door de grootte van een maximale aanvraag hoofdtekst op te geven. Aanvragen die een grotere aanvraagtekst bevatten niet in cache zijn opgeslagen.

Belangrijke informatie:

- Deze functie is alleen van toepassing wanneer het bericht antwoorden komen in aanmerking voor het opslaan in cache. Gebruik de gecachet kan worden HTTP-methoden-functie in te schakelen POST-aanvraag in.
- Hoofdtekst van de aanvraag wordt voor in aanmerking genomen:
    - x-www-form-urlencoded goedkeurt waarden
    - Ervoor te zorgen dat een unieke cache-sleutel
- Definiëren van de hoofdtekst van een groot aantal aanvragen kan gegevens leveren de prestaties beïnvloeden.
    - **Aanbevolen waarde:** 14 Kb
    - **Minimale waarde:** 1 Kb

**Standaardgedrag:** 14 Kb

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="cache-control-header-treatment"></a>Cache-Control Header Treatment
**Doel:** Hiermee bepaalt u de generatie van `Cache-Control` headers met het pop-server als de functie voor extern Max-Age actief is.

De eenvoudigste manier om dit type configuratie is de externe Max-Age en de functies van de Cache-Control-Header behandeling plaatsen in dezelfde instructie.

Value|Resultaat
--|--
Overschrijven|Zorgt ervoor dat de volgende acties uitgevoerd:<br/> -Overschrijft de `Cache-Control` header die worden gegenereerd door de oorspronkelijke server. <br/>-Toegevoegd de `Cache-Control` header die worden geproduceerd door de functie externe Max-Age aan het antwoord.
Pass Through|Zorgt ervoor dat de `Cache-Control` header die worden geproduceerd door de functie externe Max-Age nooit wordt toegevoegd aan het antwoord. <br/> Als de oorspronkelijke server produceert een `Cache-Control` header, waar deze voor de eindgebruiker validatiewerkstroom doorloopt. <br/> Als de oorspronkelijke server produceert geen een `Cache-Control` header, wordt deze optie kan ertoe leiden dat de antwoord-header bevat geen een `Cache-Control` header.
Indien ontbrekend toevoegen|Als een `Cache-Control` header is niet ontvangen van de oorspronkelijke server en vervolgens deze optie voegt de `Cache-Control` header die worden geproduceerd door de functie externe Max-Age. Deze optie is handig om ervoor te zorgen dat alle activa zijn toegewezen een `Cache-Control` header.
Verwijderen| Deze optie zorgt ervoor dat een `Cache-Control` -header is niet opgenomen in de header-reactie. Als een `Cache-Control` header al is toegewezen, en vervolgens wordt deze verwijderd uit het header-antwoord.

**Standaardgedrag:** overschrijven.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="cache-key-query-string"></a>Cache-sleutel Query-tekenreeks
**Doel:** bepaalt of de cache-sleutel bevat of niet van toepassing op queryreeksparameters die zijn gekoppeld aan een aanvraag.

Belangrijke informatie:

- Geef een of meer query tekenreeks parameternamen en elke parameternaam scheiden met een spatie.
- Deze functie bepaalt of queryreeksparameters worden opgenomen of van de cache-sleutel uitgesloten. Aanvullende informatie is opgegeven voor elke optie in de volgende tabel.

Type|Description
--|--
 Opnemen|  Geeft aan dat alle opgegeven parameters moet worden opgenomen in de cache-sleutel. Een unieke cache-sleutel wordt gegenereerd voor elke aanvraag die een unieke waarde voor een queryreeks-parameter gedefinieerd in deze functie bevat. 
 Alle  |Geeft aan dat een unieke cache-sleutel is gemaakt voor elke aanvraag voor een asset die een unieke queryreeks bevat. Dit type configuratie wordt meestal niet aanbevolen omdat dit tot een klein percentage treffers in cache leiden kan. Een klein aantal treffers in cache verhoogt de belasting van de oorspronkelijke server, omdat deze meer aanvragen moet verwerken. Deze configuratie een duplicaat van het cachegedrag bekend als "unieke-cache" op de pagina Query-tekenreeks in cache opslaan. 
 Uitsluiten | Geeft aan dat alleen de opgegeven parameter (s) is uitgesloten van de cache-sleutel. Alle andere queryreeksparameters zijn opgenomen in de cache-sleutel. 
 Sluit alle  |Geeft aan dat alle queryreeksparameters zijn uitgesloten van de cache-sleutel. Deze configuratie een duplicaat van de "standaard-cache" standaard cachegedrag op de pagina Query-tekenreeks in cache opslaan.  

De regelengine kunt u het aanpassen van de manier waarop de query queryreeksen opslaan in cache is geïmplementeerd. U kunt bijvoorbeeld opgeven dat de query queryreeksen opslaan in cache wordt uitgevoerd alleen op bepaalde locaties of bestandstypen.

Als u wilt dupliceren van de "no-cache" queryreeks cachegedrag op de pagina Query-tekenreeks in cache opslaan, maakt u een regel met een URL-Query Wildcard-voorwaarde voor overeenkomst en de functie Cache overslaan. Stel de voorwaarde voor overeenkomst van jokertekens voor URL-Query op een sterretje (*).

>[!IMPORTANT] 
> Als het token autorisatie voor elk pad voor dit account is ingeschakeld, is standard-cachemodus de enige modus die kan worden gebruikt voor het opslaan van de query-tekenreeks. Zie [Cachegedrag in Azure CDN bepalen met queryreeksen](cdn-query-string-premium.md) voor meer informatie.

#### <a name="sample-scenarios"></a>Voorbeeldscenario 's

Het volgende voorbeeld van gebruik voor deze functie biedt een voorbeeld van een aanvraag en de standaard-cache-sleutel:

- **Voorbeeld van een aanvraag:** http://wpc.0001.&lt; domein&gt;/800001/Origin/folder/asset.htm?sessionid=1234 & taal = nl & userid = 01
- **Standaard-cache-sleutel:** /800001/Origin/folder/asset.htm

##### <a name="include"></a>Opnemen

Voorbeeldconfiguratie:

- **Type:** opnemen
- **Parameter (s):** taal

Dit type configuratie genereert de volgende query-tekenreeks parameter cache-sleutel:

    /800001/Origin/folder/asset.htm?language=EN

##### <a name="include-all"></a>Alle

Voorbeeldconfiguratie:

- **Type:** omvat alle

Dit type configuratie genereert de volgende query-tekenreeks parameter cache-sleutel:

    /800001/Origin/folder/asset.htm?sessionid=1234&language=EN&userid=01

##### <a name="exclude"></a>Uitsluiten

Voorbeeldconfiguratie:

- **Type:** uitsluiten
- **Parameter (s):** sessionid gebruikers-id

Dit type configuratie genereert de volgende query-tekenreeks parameter cache-sleutel:

    /800001/Origin/folder/asset.htm?language=EN

##### <a name="exclude-all"></a>Sluit alle

Voorbeeldconfiguratie:

- **Type:** Alles uitsluiten

Dit type configuratie genereert de volgende query-tekenreeks parameter cache-sleutel:

    /800001/Origin/folder/asset.htm

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="cache-key-rewrite"></a>Cache-sleutel opnieuw schrijven
**Doel:** herschrijft de cache-sleutel die is gekoppeld aan een aanvraag.

Een cache-sleutel is het relatieve pad waarmee een asset voor de doeleinden van caching. De servers controleren met andere woorden, de versie van een cache van een asset op basis van het pad zoals gedefinieerd in de cache-sleutel.

Deze functie configureren met het definiëren van de volgende opties:

Optie|Description
--|--
Oorspronkelijke pad| Het relatieve pad op van het type van de aanvragen waarvan Cachesleutel wordt herschreven definiëren. Een relatief pad worden gedefinieerd met een basis oorsprongpad selecteren en vervolgens een reguliere-expressiepatroon te definiëren.
Nieuw pad|Het relatieve pad voor de nieuwe cache-sleutel definiëren. Een relatief pad worden gedefinieerd met een basis oorsprongpad selecteren en vervolgens een reguliere-expressiepatroon te definiëren. Het relatieve pad kan dynamisch worden samengesteld met behulp van [HTTP-variabelen](cdn-http-variables.md).
**Standaardgedrag:** Cachesleutel van een aanvraag wordt bepaald door de aanvraag-URI.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="comment"></a>Opmerking
**Doel:** kunt u een opmerking in een regel worden toegevoegd.

Er wordt één gebruikt voor deze functie bevatten aanvullende informatie over het algemeen gebruik van een regel of waarom een bepaalde overeenkomen met de voorwaarde of functie is toegevoegd aan de regel.

Belangrijke informatie:

- Maximaal 150 tekens kan worden opgegeven.
- Gebruik alleen alfanumerieke tekens.
- Deze functie heeft geen invloed op het gedrag van de regel. Dit is alleen bedoeld om een gebied waarin u voor toekomstig gebruik of die kan helpen opgeven kunt bij het oplossen van de regel.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="complete-cache-fill"></a>Opvulling van de Cache voltooien
**Doel:** bepaalt wat er gebeurt wanneer een aanvraag in een gedeeltelijke Cachemisser op een pop-server resulteert.

Een gedeeltelijke Cachemisser beschrijving van de status van de cache voor een asset die niet volledig is gedownload naar een pop-server. Als een activum wordt slechts gedeeltelijk in de cache op een pop-server, klikt u vervolgens de volgende aanvraag voor de activa doorgestuurd opnieuw naar de oorspronkelijke server.
<!---
This feature is not available for the ADN platform. The typical traffic on this platform consists of relatively small assets. The size of the assets served through these platforms helps mitigate the effects of partial cache misses, since the next request will typically result in the asset being cached on that POP.

--->
Een gedeeltelijke Cachemisser treedt meestal op wanneer een gebruiker een download afgebroken of voor activa die uitsluitend zijn aangevraagd met behulp van HTTP-aanvragen voor bereik. Deze functie is vooral nuttig zijn voor grote activa die normaal gesproken niet worden gedownload van begin tot eind (bijvoorbeeld video's). Als gevolg hiervan, is deze functie standaard ingeschakeld op de grote HTTP-platform. Deze is uitgeschakeld op alle andere platforms.

Houd de standaardconfiguratie voor de grote HTTP-platform, omdat het vermindert de belasting van de oorspronkelijke server van uw klant en verhoogt de snelheid waarmee uw klanten uw inhoud downloaden.

Value|Resultaat
--|--
Ingeschakeld|Hiermee herstelt u het standaardgedrag. Het standaardgedrag is om af te dwingen de pop-server om te starten van een op de achtergrond ophalen van de activa van de oorspronkelijke server. Waarna de asset worden weergegeven in de lokale cache van de pop-server.
Uitgeschakeld|Hiermee voorkomt dat een pop-server uitvoeren van een op de achtergrond ophalen voor de asset. Het resultaat is een pop-server aan te vragen bij de oorspronkelijke server van de klant zorgt ervoor dat de volgende aanvraag voor de activa van die regio.

**Standaardgedrag:** ingeschakeld.

#### <a name="compatibility"></a>Compatibiliteit
Deze functie kan niet worden gekoppeld aan de volgende criteria voor overeenkomst vanwege de manier die in welke cache instellingen worden bijgehouden: 
- AS-nummer
- IP-adres van client
- Cookie-Parameter
- Cookie Parameter reguliere expressie
- Land/regio
- Apparaat
- Cname voor edge
- Verwijzende domein
- Aanvraag-Header letterlijke waarde
- Aanvraag-Header reguliere expressie
- Aanvraag-Header jokertekens
- Aanvraagmethode
- Aanvraag-schema
- URL-Query letterlijke waarde
- URL-Query reguliere expressie
- URL-Query jokertekens
- URL Query Parameter

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="compress-file-types"></a>Bestandstypen comprimeren
**Doel:** worden de bestandsindelingen voor de bestanden die gecomprimeerd zijn gedefinieerd op de server.

Een bestandsindeling kan worden opgegeven met behulp van de Internet-media-type (bijvoorbeeld: Content-Type). Mediatype van Internet zijn platformonafhankelijk metagegevens waarmee de servers voor het identificeren van de bestandsindeling van een bepaalde asset. Hieronder vindt u een lijst met algemene Internet mediatypen.

Mediatype van Internet|Description
--|--
text/plain|Bestanden met tekst zonder opmaak
text/html| HTML-bestanden
text/css|Cascading Style Sheets (CSS)
application/x-javascript|Javascript
application/javascript|Javascript
Belangrijke informatie:

- Geef meerdere mediatypen van Internet door die begrenst elkaar met een spatie. 
- Deze functie worden alleen gecomprimeerd activa waarvan de grootte minder dan 1 MB is. Grotere activa zijn niet door de servers gecomprimeerd.
- Bepaalde typen inhoud, zoals afbeeldingen, video en audio-media-activa (bijvoorbeeld, JPG, MP3, MP4, enzovoort), zijn al gecomprimeerd. Omdat er extra compressie op deze typen van de activa is bestandsgrootte niet aanzienlijk verslechteren, verdient het aanbeveling dat u geen compressie op deze inschakelt.
- Jokertekens bevatten, zoals sterretjes, worden niet ondersteund.
- Voordat u deze functie op een regel toevoegt, zorg ervoor dat u instelt dat de compressie uitgeschakelde optie op de pagina compressie voor het platform waarop deze regel wordt toegepast.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="custom-log-field-1"></a>Het aangepaste logboekveld 1
**Doel:** bepaalt de indeling en de inhoud die wordt toegewezen aan het aangepaste logboek-veld in een onbewerkte logboekbestand.

Deze aangepaste veld kunt u om te bepalen welke aanvraag en antwoord-header-waarden worden opgeslagen in uw logboekbestanden.

Het aangepaste logboekveld heet standaard 'x-ec_custom-1'. De naam van dit veld kan worden aangepast via de pagina instellingen van het onbewerkte.

De indeling voor het opgeven van de aanvraag- en reactieheaders wordt als volgt gedefinieerd:

Header-Type|Indeling|Voorbeelden
-|-|-
Aanvraagheader|`%{[RequestHeader]()}[i]()` | %{Accept-Encoding}i <br/> {Referer}i <br/> %{Authorization}i
Antwoordkoptekst|`%{[ResponseHeader]()}[o]()`| %{Age}o <br/> %{Content-Type}o <br/> %{Cookie}o

Belangrijke informatie:

- Een veld van het aangepaste logboek kan elke combinatie van header-velden en tekst zonder opmaak bevatten.
- Geldige tekens voor dit veld zijn als volgt: alfanumerieke tekens (0-9, a-z, A-Z), streepjes, dubbele punten, puntkomma's, apostroffen, komma's, punten, onderstrepingstekens, gelijktekens, haakjes, haakjes en spaties. Het percentagesymbool en de accolades zijn alleen toegestaan als die wordt gebruikt om op te geven van een headerveld.
- De spelling voor elk veld van de opgegeven header moet overeenkomen met de naam van de gewenste aanvraag/antwoord-header.
- Als u opgeven van meerdere headers wilt, gebruikt u een scheidingsteken om aan te geven van elke header. U kunt bijvoorbeeld een afkorting voor elke header:
    - AE: % {accepteren-Encoding} i A: % {autorisatie} i CT: % {Content-Type}-o 

**Standaardwaarde:** -

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="debug-cache-response-headers"></a>Fouten opsporen in Cache antwoordheaders
**Doel:** bepaalt of een antwoord kan bevatten [X-EG-Debug antwoordheaders](cdn-http-debug-headers.md), waarmee u informatie op het cache-beleid voor de aangevraagde asset.

Fouten opsporen in een cacheantwoord headers worden opgenomen in het antwoord wanneer beide van de volgende waar zijn:

- De functie voor antwoordheaders voor fouten opsporen in Cache is ingeschakeld op de opgegeven aanvraag.
- De opgegeven aanvraag definieert de set foutopsporing cache antwoordheaders die worden opgenomen in het antwoord.

Fouten opsporen in een cacheantwoord headers kunnen worden aangevraagd door de volgende header en de opgegeven richtlijnen op te nemen in de aanvraag:

`X-EC-Debug: _&lt;Directive1&gt;_,_&lt;Directive2&gt;_,_&lt;DirectiveN&gt;_`

**Voorbeeld:**

X-EG-Debug: x-ec-cache,x-ec-check-cacheable,x-ec-cache-key,x-ec-cache-state

Value|Resultaat
-|-
Ingeschakeld|Aanvragen voor foutopsporing cache antwoordheaders wordt een antwoord met de header X-EG-Debug.
Uitgeschakeld|De antwoord-header X-EG-Debug worden, uitgesloten van het antwoord.

**Standaardgedrag:** uitgeschakeld.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="default-internal-max-age"></a>Standaard interne Max-Age
**Doel:** bepaalt de standaardintervaltijd van max-age voor pop-server naar de oorspronkelijke server cache hervalidatie. Met andere woorden, de hoeveelheid tijd dat wordt gewacht voordat een pop-server wordt gecontroleerd of een activum in de cache overeenkomt met de asset die zijn opgeslagen op de oorspronkelijke server.

Belangrijke informatie:

- Deze actie vindt alleen plaats voor antwoorden van een bronserver die niet een indicatie maximale leeftijd in hebt toegewezen de `Cache-Control` of `Expires` header.
- Deze actie vindt niet plaats voor activa die worden niet gecachet kan worden beschouwd.
- Deze actie heeft geen invloed op de POP-cache revalidations browser. Deze typen revalidations worden bepaald door de `Cache-Control` of `Expires` koppen die zijn verzonden naar de browser, die kan worden aangepast met de functie externe Max-Age.
- De resultaten van deze actie nog geen een waarneembare effect op de antwoordheaders en de inhoud die is geretourneerd door de POP's voor uw inhoud, maar deze mogelijk van invloed op de hoeveelheid hervalidatie verkeer vanuit POP's verzonden naar de oorspronkelijke server.
- Deze functie door te configureren:
    - Selecteren van de statuscode waarvoor een interne standaard max-age kan worden toegepast.
    - Een geheel getal op te geven en selecteer vervolgens de gewenste tijdseenheid (bijvoorbeeld seconden, minuten, uren, enz.). Deze waarde bepaalt het standaardinterval voor de interne max-age.

- Instellen van de tijdseenheid op 'Uit', wordt een standaard interne max-age interval van zeven dagen voor aanvragen die niet zijn toegewezen aan een indicatie maximale leeftijd in toegewezen hun `Cache-Control` of `Expires` header.

**Standaardwaarde:** 7 dagen

#### <a name="compatibility"></a>Compatibiliteit
Deze functie kan niet worden gekoppeld aan de volgende criteria voor overeenkomst vanwege de manier die in welke cache instellingen worden bijgehouden: 
- AS-nummer
- IP-adres van client
- Cookie-Parameter
- Cookie Parameter reguliere expressie
- Land/regio
- Apparaat
- Cname voor edge
- Verwijzende domein
- Aanvraag-Header letterlijke waarde
- Aanvraag-Header reguliere expressie
- Aanvraag-Header jokertekens
- Aanvraagmethode
- Aanvraag-schema
- URL-Query letterlijke waarde
- URL-Query reguliere expressie
- URL-Query jokertekens
- URL Query Parameter

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="deny-access-403"></a>Deny Access (403)
**Doel**: Hiermee bepaalt u of alle aanvragen zijn afgewezen met een 403 verboden-antwoord.

Value | Resultaat
------|-------
Ingeschakeld| Zorgt ervoor dat alle aanvragen die voldoen aan de overeenkomende criteria kunnen worden geweigerd met een 403 verboden-antwoord.
Uitgeschakeld| Hiermee herstelt u het standaardgedrag. De standaardinstelling is dat de oorspronkelijke server om te bepalen van het type van de reactie die wordt geretourneerd.

**Standaardgedrag**: Uitgeschakeld

> [!TIP]
   > Een mogelijk kunnen worden gebruikt voor deze functie is om te koppelen aan een voorwaarde voor overeenkomst van Header van toegang tot HTTP-verwijzende sites die van inline koppelingen naar uw inhoud gebruikmaken blokkeren.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="expires-header-treatment"></a>Koptekst behandeling verloopt
**Doel:** Hiermee bepaalt u de generatie van `Expires` headers met een pop-server als de functie externe Max-Age actief is.

De eenvoudigste manier om dit type configuratie is om de externe Max-Age en de onderdelen van de koptekst behandeling verloopt in dezelfde instructie.

Value|Resultaat
--|--
Overschrijven|Zorgt ervoor dat de volgende acties uitgevoerd:<br/>-Overschrijft de `Expires` header die worden gegenereerd door de oorspronkelijke server.<br/>-Toegevoegd de `Expires` header die worden geproduceerd door de functie externe Max-Age aan het antwoord.
Pass Through|Zorgt ervoor dat de `Expires` header die worden geproduceerd door de functie externe Max-Age nooit wordt toegevoegd aan het antwoord. <br/> Als de oorspronkelijke server produceert een `Expires` header, worden doorgegeven aan de eindgebruiker. <br/>Als de oorspronkelijke server produceert geen een `Expires` header, wordt deze optie kan ertoe leiden dat de antwoord-header bevat geen een `Expires` header.
Indien ontbrekend toevoegen| Als een `Expires` header is niet ontvangen van de oorspronkelijke server en vervolgens deze optie voegt de `Expires` header die worden geproduceerd door de functie externe Max-Age. Deze optie is handig om ervoor te zorgen dat alle activa zal worden toegewezen een `Expires` header.
Verwijderen| Zorgt ervoor dat een `Expires` -header is niet opgenomen in de header-reactie. Als een `Expires` header al is toegewezen, en vervolgens wordt deze verwijderd uit het header-antwoord.

**Standaardgedrag:** overschrijven

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="external-max-age"></a>Externe Max-Age
**Doel:** bepaalt de max-age-interval voor de browser op hervalidatie van POP-cache. Met andere woorden, de hoeveelheid tijd dat wordt gewacht voordat een browser op een nieuwe versie van een asset van een pop-server controleren kunt.

Inschakelen van deze functie wordt gegenereerd `Cache-Control: max-age` en `Expires` headers van de POP's en ze verzenden naar de HTTP-client. Standaard wordt deze headers die headers die zijn gemaakt door de oorspronkelijke server overschreven. De Cache-Control-Header-verwerking en de functies die verloopt Header behandeling kunnen echter worden gebruikt om dit gedrag te.

Belangrijke informatie:

- Deze actie heeft geen invloed op pop-server naar de oorspronkelijke server cache revalidations. Deze typen revalidations worden bepaald door de `Cache-Control` en `Expires`  kopteksten van de oorspronkelijke server ontvangen en kunnen worden aangepast met de standaard interne Max-Age en de werking van interne Max-Age-functies.
- Deze functie configureren door een geheel getal opgeven en het selecteren van de gewenste tijdseenheid (bijvoorbeeld seconden, minuten, uren, enz.).
- Als u deze functie op een negatieve waarde zorgt ervoor dat de POP's voor het verzenden van een `Cache-Control: no-cache` en een `Expires` tijd die is ingesteld in het verleden met elk antwoord naar de browser. Hoewel een HTTP-client wordt de reactie niet in cache, deze instelling heeft geen invloed op de POP's kunnen het antwoord van de oorspronkelijke server in de cache.
- Instellen van de tijdseenheid op 'Uit', wordt deze functie uitgeschakeld. De `Cache-Control` en `Expires`  kopteksten in de cache opgeslagen met de reactie van de oorspronkelijke server worden doorgegeven aan de browser.

**Standaardgedrag:** uitschakelen

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="follow-redirects"></a>Omleidingen volgen
**Doel:** bepaalt u of aanvragen kunnen worden omgeleid naar de hostnaam die is gedefinieerd in de locatieheader die is geretourneerd door een klant-bronserver.

Belangrijke informatie:

- Aanvragen kunnen alleen worden omgeleid naar edge CNAME-records die met hetzelfde platform overeenkomen.

Value|Resultaat
-|-
Ingeschakeld|Aanvragen kunnen worden omgeleid.
Uitgeschakeld|Geen worden aanvragen omgeleid.

**Standaardgedrag:** uitgeschakeld.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="force-internal-max-age"></a>Force Internal Max-Age
**Doel:** bepaalt de max-age-interval voor pop-server naar de oorspronkelijke server cache hervalidatie. Met andere woorden, de hoeveelheid tijd dat wordt gewacht voordat een pop-server controleren kunt of een activum in de cache overeenkomt met de asset die zijn opgeslagen op de oorspronkelijke server.

Belangrijke informatie:

- Deze functie overschrijft de max-age-interval dat is gedefinieerd `Cache-Control` of `Expires` headers die zijn gegenereerd op basis van een bronserver.
- Deze functie heeft geen invloed op de POP-cache revalidations browser. Deze typen revalidations worden bepaald door de `Cache-Control` of `Expires` koppen die zijn verzonden naar de browser.
- Deze functie heeft geen waarneembare gevolgen heeft voor het antwoord geleverd door een pop-server voor de aanvrager. Echter, heeft dit van invloed op de hoeveelheid hervalidatie verkeer vanuit de POP's verzonden naar de oorspronkelijke server.
- Deze functie door te configureren:
    - Selecteren van de statuscode waarvoor een interne max-age worden toegepast.
    - Een geheel getal op te geven en te selecteren van de gewenste tijdseenheid (bijvoorbeeld seconden, minuten, uren, enz.). Deze waarde bepaalt van de aanvraag max-age interval.

- Als de tijdseenheid op 'Uit', wordt deze functie uitgeschakeld. Een interne max-age interval wordt niet worden toegewezen aan de aangevraagde activa. Als de oorspronkelijke header geen cache-instructies bevat, de asset zal worden opgeslagen in het cachegeheugen op basis van de actieve instelling in de functie standaard interne Max-Age.

**Standaardgedrag:** uitschakelen

#### <a name="compatibility"></a>Compatibiliteit
Deze functie kan niet worden gekoppeld aan de volgende criteria voor overeenkomst vanwege de manier die in welke cache instellingen worden bijgehouden: 
- AS-nummer
- IP-adres van client
- Cookie-Parameter
- Cookie Parameter reguliere expressie
- Land/regio
- Apparaat
- Cname voor edge
- Verwijzende domein
- Aanvraag-Header letterlijke waarde
- Aanvraag-Header reguliere expressie
- Aanvraag-Header jokertekens
- Aanvraagmethode
- Aanvraag-schema
- URL-Query letterlijke waarde
- URL-Query reguliere expressie
- URL-Query jokertekens
- URL Query Parameter

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="h264-support-http-progressive-download"></a>H.264-ondersteuning (http-progressief downloaden)
**Doel:** bepaalt de soorten H.264-bestandsindelingen die kunnen worden gebruikt voor het streamen van inhoud.

Belangrijke informatie:

- Een door spaties gescheiden set toegestane H.264 bestandsnaamextensies definiëren in de optie bestandsextensies. De optie bestandsextensies overschrijft het standaardgedrag. Ondersteuning voor MP4 en F4V onderhouden door op te nemen die bestandsextensies wanneer deze optie instelt. 
- Zijn een periode wanneer u elke bestandsnaamextensie opgeeft (bijvoorbeeld _.mp4_, _.f4v_).

**Standaardgedrag:** MP4 en F4V media standaard HTTP progressieve Download ondersteunt.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="honor-no-cache-request"></a>Aanvraag voor Honor No-Cache
**Doel:** bepaalt of een HTTP-client geen cache's aanvragen zal worden doorgestuurd naar de oorspronkelijke server.

Een niet-cache-aanvraag treedt op wanneer de HTTP-client verzendt een `Cache-Control: no-cache` en/of `Pragma: no-cache` -header in de HTTP-aanvraag.

Value|Resultaat
--|--
Ingeschakeld|Kan geen van de client van een HTTP-cache-aanvragen worden doorgestuurd naar de oorspronkelijke server en de oorspronkelijke server retourneert de antwoordheaders en de hoofdtekst met het pop-server terug naar de HTTP-client.
Uitgeschakeld|Hiermee herstelt u het standaardgedrag. Het standaardgedrag is om te voorkomen dat niet-cache-aanvragen worden doorgestuurd naar de oorspronkelijke server.

Voor alle productieverkeer, is het raadzaam deze functie in de standaardstaat uitgeschakeld laten staan. Bronservers wordt anders niet worden afgeschermd van eindgebruikers die mogelijk veel niet-cache-aanvragen per ongeluk activeren bij het vernieuwen van webpagina's, of van de vele populaire mediaspelers die zijn gecodeerd voor het verzenden van een niet-cache-header met elke video aanvraag. Deze functie kan echter nuttig toe te passen op bepaalde niet-productie voor fasering of het testen van mappen, zodat u nieuwe inhoud op aanvraag worden opgehaald uit de oorspronkelijke server zijn.

De status van de cache die is gerapporteerd voor een aanvraag die kan worden doorgestuurd naar een oorspronkelijke server vanwege deze functie is `TCP_Client_Refresh_Miss`. Het rapport Statusbepaling van de Cache, die beschikbaar in de module reporting Core is, bevat statistische gegevens door de status van de cache. Dit rapport kunt u voor het bijhouden van het aantal en percentage van de aanvragen die worden doorgestuurd naar een bronserver vanwege deze functie.

**Standaardgedrag:** uitgeschakeld.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="ignore-origin-no-cache"></a>Negeren van oorsprong No-Cache
**Doel:** bepaalt of het CDN wordt genegeerd door de volgende richtlijnen die worden aangeleverd vanuit een originele server:

- `Cache-Control: private`
- `Cache-Control: no-store`
- `Cache-Control: no-cache`
- `Pragma: no-cache`

Belangrijke informatie:

- Deze functie configureren met het definiëren van een door spaties gescheiden lijst van statuscodes waarvoor de bovenstaande richtlijnen worden genegeerd.
- De set met geldige waarden voor deze functie zijn: 200, 203, 300, 301, 302, 305, 307, 400, 401, 402, 403, 404, 405, 406, 407, 408, 409, 410, 411, 412, 413, 414, 415, 416, 417, 500, 501, 502, 503, 504, en 505.
- Deze functie uitschakelen door deze te stellen op een lege waarde.

**Standaardgedrag:** het standaardgedrag is om te voldoen aan de bovenstaande richtlijnen.

#### <a name="compatibility"></a>Compatibiliteit
Deze functie kan niet worden gekoppeld aan de volgende criteria voor overeenkomst vanwege de manier die in welke cache instellingen worden bijgehouden: 
- AS-nummer
- IP-adres van client
- Cookie-Parameter
- Cookie Parameter reguliere expressie
- Land/regio
- Apparaat
- Cname voor edge
- Verwijzende domein
- Aanvraag-Header letterlijke waarde
- Aanvraag-Header reguliere expressie
- Aanvraag-Header jokertekens
- Aanvraagmethode
- Aanvraag-schema
- URL-Query letterlijke waarde
- URL-Query reguliere expressie
- URL-Query jokertekens
- URL Query Parameter

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="ignore-unsatisfiable-ranges"></a>Ongeldig bereiken negeren 
**Doel:** bepaalt de reactie die wordt geretourneerd naar clients wanneer een aanvraag statuscode 416 gevraagde bereik niet geldig genereert.

Standaard wordt deze statuscode geretourneerd wanneer de aanvraag opgegeven bytebereik kan niet worden voldaan door een pop-server en een veld als bereik aanvraagheader is niet opgegeven.

Value|Resultaat
-|-
Ingeschakeld|Hiermee voorkomt dat de POP's reageren op een ongeldige bytebereik-aanvraag met een 416 statuscode gevraagde bereik niet geldig. In plaats daarvan wordt de servers leveren de aangevraagde asset en 200 OK naar de client geretourneerd.
Uitgeschakeld|Hiermee herstelt u het standaardgedrag. Het standaardgedrag is om te voldoen aan de 416 statuscode gevraagde bereik niet geldig.

**Standaardgedrag:** uitgeschakeld.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="internal-max-stale"></a>Interne Max-verouderd
**Doel:** besturingselementen hoe lang na de verlooptijd van de normale een activum in de cache van een pop-server kan worden geleverd wanneer de pop-server niet kan valideren van de cache asset met de oorspronkelijke server.

Normaal gesproken wanneer van een asset max-age tijd is verlopen, wordt de pop-server een validatieaanvraag verzonden naar de oorspronkelijke server. De oorspronkelijke server wordt vervolgens reageren met een van beide een 304 niet worden gewijzigd zodat de pop-server een nieuwe van de lease voor de cache asset, hetzij met 200 OK als u wilt de pop-server voorzien van een bijgewerkte versie van de activa in de cache.

Als de pop-server kan tot stand brengen van een verbinding met de oorspronkelijke server bij dergelijke hervalidatie, en vervolgens deze interne Max-verouderde-functie of en hoe lang bepaalt is, blijven de pop-server voldoen aan de asset nu verouderd.

Houd er rekening mee dat dit tijdsinterval wordt gestart wanneer van de asset max-age is verlopen, niet als de mislukte hervalidatie optreedt. De maximale periode gedurende welke een asset kan worden geleverd zonder geslaagde hervalidatie is daarom de hoeveelheid tijd die is opgegeven door de combinatie van max-age plus max-verouderd. Bijvoorbeeld, als een asset om 9:00 uur met een max-age van 30 minuten en een maximale-verouderde van 15 minuten in cache is opgeslagen, zou klikt u vervolgens een poging is mislukt hervalidatie op 9:44 leiden tot een eindgebruiker de verouderde cache asset ontvangen tijdens een poging tot mislukte hervalidatie om 9:46 tot de en leiden zou d gebruiker ontvangt een 504 time-out van Gateway.

Een waarde die is geconfigureerd voor deze functie is vervangen door een andere `Cache-Control: must-revalidate` of `Cache-Control: proxy-revalidate` headers ontvangen van de oorspronkelijke server. Als een van de headers van de oorspronkelijke server is ontvangen wanneer een asset is in eerste instantie in de cache opgeslagen, klikt u vervolgens in de pop-server niet een verlopen in de cache asset fungeren. Als de pop-server kan niet valideren met de oorsprong als van de asset max-age interval is verlopen, is geeft de pop-server in dat geval een 504 time-out van Gateway-fout.

Belangrijke informatie:

- Deze functie door te configureren:
    - Selecteren van de statuscode waarvoor een max-verouderde worden toegepast.
    - Een geheel getal op te geven en selecteer vervolgens de gewenste tijdseenheid (bijvoorbeeld seconden, minuten, uren, enz.). Deze waarde bepaalt de interne max-verouderde die worden toegepast.

- Instellen van de tijdseenheid op 'Uit', wordt deze functie uitgeschakeld. Een activum in de cache worden niet buiten de normale vervaltijd verwerkt.

**Standaardgedrag:** twee minuten

#### <a name="compatibility"></a>Compatibiliteit
Deze functie kan niet worden gekoppeld aan de volgende criteria voor overeenkomst vanwege de manier die in welke cache instellingen worden bijgehouden: 
- AS-nummer
- IP-adres van client
- Cookie-Parameter
- Cookie Parameter reguliere expressie
- Land/regio
- Apparaat
- Cname voor edge
- Verwijzende domein
- Aanvraag-Header letterlijke waarde
- Aanvraag-Header reguliere expressie
- Aanvraag-Header jokertekens
- Aanvraagmethode
- Aanvraag-schema
- URL-Query letterlijke waarde
- URL-Query reguliere expressie
- URL-Query jokertekens
- URL Query Parameter

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="log-query-string"></a>Logboek-queryreeks
**Doel:** bepaalt of een queryreeks samen met de URL in toegang tot logboeken worden opgeslagen.

Value|Resultaat
-|-
Ingeschakeld|Kan de opslag van queryreeksen bij het vastleggen van URL's in een toegangslogboek. Als een URL geen een queryreeks bevat, klikt u vervolgens deze optie geen effect.
Uitgeschakeld|Hiermee herstelt u het standaardgedrag. Het standaardgedrag is queryreeksen negeren wanneer URL's op te nemen in een toegangslogboek.

**Standaardgedrag:** uitgeschakeld.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="maximum-keep-alive-requests"></a>Maximum aantal Keep-Alive-aanvragen
**Doel:** definieert het maximum aantal aanvragen voor een Keep-Alive-verbinding wordt gesloten.

Als u het maximum aantal aanvragen op een lage waarde wordt afgeraden en kan leiden tot verminderde prestaties.

Belangrijke informatie:

- Deze waarde als een geheel getal opgeven.
- Geen komma's of punten in de opgegeven waarde.

**Standaardwaarde:** 10.000 aanvragen

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="modify-client-request-header"></a>Client-aanvraagheader wijzigen
**Doel:** elke aanvraag bevat een set aanvraagheaders waarin deze worden beschreven. Deze functie kan:

- Toevoegen of de waarde die is toegewezen aan een aanvraagheader overschrijven. Als de opgegeven aanvraagheader niet bestaat, wordt klikt u vervolgens deze functie toegevoegd aan de aanvraag.
- Een aanvraagheader verwijderen uit de aanvraag.

Aanvragen die worden doorgestuurd naar een oorspronkelijke server weer de wijzigingen van deze functie.

Een van de volgende acties kan worden uitgevoerd op een aanvraagheader:

Optie|Description|Voorbeeld
-|-|-
Toevoegen|De opgegeven waarde zal worden toegevoegd aan het einde van de bestaande waarde voor de aanvraag-header.|**Aanvraag-header-waarde (client):**<br/>Value1<br/>**Aanvraag-header-waarde (regelengine):**<br/>Value2 <br/>**Nieuwe aanvraagheader-waarde:** <br/>Value1Value2
Overschrijven|De waarde van de aanvraag-header wordt worden ingesteld op de opgegeven waarde.|**Aanvraag-header-waarde (client):**<br/>Value1<br/>**Aanvraag-header-waarde (regelengine):**<br/>Value2<br/>**Nieuwe aanvraagheader-waarde:**<br/> Value2 <br/>
Verwijderen|Hiermee verwijdert u de koptekst opgegeven aanvraag.|**Aanvraag-header-waarde (client):**<br/>Value1<br/>**Client-aanvraag-header-configuratie wijzigen:**<br/>Verwijder de aanvraagheader in kwestie.<br/>**Resultaat:**<br/>De opgegeven aanvraagheader niet doorgestuurd naar de oorspronkelijke server.

Belangrijke informatie:

- Zorg ervoor dat de waarde die is opgegeven in de optie voor een exacte overeenkomst voor de gewenste aanvraagheader is.
- Aanvraag is niet in aanmerking voor de identificatie van een koptekst genomen. Bijvoorbeeld, een van de volgende variaties van de `Cache-Control` header-naam kan worden gebruikt om te kunnen identificeren:
    - cache-control
    - CACHE-CONTROL
    - cachE-Control
- Wanneer u een headernaam opgeeft, gebruikt u alleen alfanumerieke tekens, streepjes of onderstrepingstekens bevatten.
- Verwijderen van een koptekst wordt voorkomen dat deze wordt doorgestuurd naar een oorspronkelijke server door de POP's.
- De volgende headers zijn gereserveerd en kunnen niet worden gewijzigd door deze functie:
    - doorgestuurd
    - host
    - via
    - waarschuwing
    - x doorgestuurd voor
    - Alle headernamen die met "x-EG beginnen" zijn gereserveerd.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="modify-client-response-header"></a>Client-Reactieheader wijzigen
Elke reactie bevat een set van antwoordheaders waarin deze worden beschreven. Deze functie kan:

- Toevoegen of de waarde die is toegewezen aan een response-header overschreven. Als de opgegeven antwoord-header niet bestaat, wordt klikt u vervolgens deze functie toegevoegd aan het antwoord.
- Verwijdert een antwoordheader van het antwoord.

Standaard worden de antwoord-headerwaarden worden gedefinieerd door een oorspronkelijke-server en de POP's.

Een van de volgende acties kan worden uitgevoerd op een antwoordheader:

Optie|Description|Voorbeeld
-|-|-
Toevoegen|De opgegeven waarde zal worden toegevoegd aan het einde van de waarde van de bestaande antwoord-header.|**Antwoord-header-waarde (client):**<br />Value1<br/>**Antwoord-header-waarde (regelengine):**<br/>Value2<br/>**Nieuwe waarde voor antwoord-header:**<br/>Value1Value2
Overschrijven|De waarde van de antwoord-header wordt worden ingesteld op de opgegeven waarde.|**Antwoord-header-waarde (client):**<br/>Value1<br/>**Antwoord-header-waarde (regelengine):**<br/>Value2 <br/>**Nieuwe waarde voor antwoord-header:**<br/>Value2 <br/>
Verwijderen|Hiermee verwijdert u de opgegeven antwoord-header.|**Antwoord-header-waarde (client):**<br/>Value1<br/>**Client antwoord-header-configuratie wijzigen:**<br/>Verwijder de betreffende response-header.<br/>**Resultaat:**<br/>De opgegeven antwoord-header niet doorgestuurd naar de aanvrager.

Belangrijke informatie:

- Zorg ervoor dat de waarde die is opgegeven in de optie voor een exacte overeenkomst voor de gewenste response-header. 
- Aanvraag is niet in aanmerking voor de identificatie van een koptekst genomen. Bijvoorbeeld, een van de volgende variaties van de `Cache-Control` header-naam kan worden gebruikt om te kunnen identificeren:
    - cache-control
    - CACHE-CONTROL
    - cachE-Control
- Verwijderen van een koptekst voorkomt u dat het wordt doorgestuurd naar de aanvrager.
- De volgende headers zijn gereserveerd en kunnen niet worden gewijzigd door deze functie:
    - accept-encoding
    - Leeftijd
    - verbinding
    - content-encoding
    - content-length
    - content-range
    - date
    - server
    - trailer
    - transfer-encoding
    - upgrade
    - variëren
    - via
    - waarschuwing
    - Alle headernamen die met "x-EG beginnen" zijn gereserveerd.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="partial-cache-sharing"></a>Partial Cache Sharing
**Doel:** bepaalt of een aanvraag voor gedeeltelijk in de cache inhoud kunt genereren.

Deze gedeeltelijke cache kan vervolgens worden gebruikt om te voldoen aan nieuwe aanvragen voor die inhoud totdat de gevraagde inhoud is volledig in de cache opgeslagen.

Value|Resultaat
-|-
Ingeschakeld|Gedeeltelijk in de cache inhoud kunnen worden gegenereerd door aanvragen.
Uitgeschakeld|Aanvragen kunnen alleen genereren voor een volledig in de cache opgeslagen versie van de aangevraagde inhoud.

**Standaardgedrag:** uitgeschakeld.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="prevalidate-cached-content"></a>De inhoud in cache prevalidate
**Doel:** bepaalt of de inhoud in cache in aanmerking voor vroege hervalidatie komen, voordat de TTL verloopt.

De hoeveelheid tijd vóór de vervaldatum van de gevraagde inhoud TTL gedurende welke het komen in aanmerking voor vroege hervalidatie definiëren.

Belangrijke informatie:

- 'Uit' selecteren als de tijdseenheid hervalidatie vereist om te worden toegepast na de inhoud in cache is TTL verlopen. Tijd mag niet worden opgegeven en wordt genegeerd.

**Standaardgedrag:** uitschakelen. Hervalidatie kan alleen plaatsvinden nadat de inhoud in cache-TTL is verlopen.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="proxy-special-headers"></a>Speciale proxy-kopteksten
**Doel:** definieert de set [Verizon-specifieke HTTP-aanvraagheaders](cdn-verizon-http-headers.md) die, van een POP worden doorgestuurd naar een oorspronkelijke server.

Belangrijke informatie:

- Elk CDN-specifieke aanvraagheader gedefinieerd in deze functie wordt doorgestuurd naar een oorspronkelijke server. Uitgesloten headers zijn niet doorgestuurd.
- Als u wilt voorkomen dat een aanvraagheader CDN-specifieke worden doorgestuurd, door deze te verwijderen uit spaties gescheiden lijst in het headerveld-lijst.

De volgende HTTP-headers zijn opgenomen in de lijst met:
- Via
- X doorgestuurd voor
- X-Forwarded-Proto
- X-Host
- X-Midgress
- X-Gateway-List
- X-EC-Name
- Host

**Standaardgedrag:** alle CDN-specifieke aanvraagheaders doorgestuurd naar de oorspronkelijke server.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="refresh-zero-byte-cache-files"></a>Bestanden opslaan in Cache nul bytes vernieuwen
**Doel:** bepaalt hoe van de client van een HTTP-aanvraag voor een asset 0-byte-cache door de POP's worden verwerkt.

Geldige waarden zijn:

Value|Resultaat
--|--
Ingeschakeld|Zorgt ervoor dat de POP opnieuw ophalen van de activa van de oorspronkelijke server.
Uitgeschakeld|Hiermee herstelt u het standaardgedrag. Het standaardgedrag is om te fungeren voor geldige cache activa op aanvraag.
Deze functie is niet vereist voor de juiste caching en levering van inhoud, maar zijn mogelijk nuttig zijn als tijdelijke oplossing. Dynamische inhoud generatoren van bronservers kunnen bijvoorbeeld per ongeluk resulteren in 0-byte-antwoorden aan de POP's worden verzonden. Deze typen antwoorden zijn meestal in cache opgeslagen door de POP's. Als u weet dat een 0-byte-antwoord nooit een geldige reactie is 

voor dergelijke inhoud vervolgens deze functie kunt voorkomen dat deze typen van de activa die worden geleverd aan uw clients.

**Standaardgedrag:** uitgeschakeld.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="set-cacheable-status-codes"></a>Statuscodes gecachet kan worden ingesteld
**Doel:** definieert de set statuscodes die tot de inhoud in cache leiden kunnen.

Standaard is opslaan in cache alleen ingeschakeld voor antwoorden op 200 OK.

Een set door spaties gescheiden van de gewenste statuscodes definiëren.

Belangrijke informatie:

- De functie genegeerd authenticiteit No-Cache inschakelen. Als deze functie niet is ingeschakeld, 200 OK antwoorden kunnen niet worden opgeslagen in het cachegeheugen.
- De set met geldige waarden voor deze functie zijn: 203, 300, 301, 302, 305, 307, 400, 401, 402, 403, 404, 405, 406, 407, 408, 409, 410, 411, 412, 413, 414, 415, 416, 417, 500, 501, 502, 503, 504, en 505.
- Deze functie kan niet worden gebruikt om uit te schakelen in cache opslaan van antwoorden die een 200 OK statuscode genereren.

**Standaardgedrag:** opslaan in cache is alleen ingeschakeld voor antwoorden die een 200 OK statuscode genereren.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="set-client-ip-custom-header"></a>Instellen van aangepaste IP-Header Client
**Doel:** voegt u een aangepaste header die de aanvragende client op IP-adres aan de aanvraag identificeert.

De optie voor Header-naam definieert de naam van de aangepaste aanvraagheader waar het IP-adres van de client wordt opgeslagen.

Met deze functie kunt u een klant bronserver om erachter te komen client-IP-adressen via een aangepaste aanvraagheader. Als de aanvraag is uitgevoerd uit de cache, klikt u vervolgens de oorspronkelijke server wordt niet worden op de hoogte van de IP-adres van de client. Daarom is het aanbevolen dat deze functie worden gebruikt met activa die worden niet in cache opgeslagen.

Zorg ervoor dat de naam van de opgegeven header komt niet overeen met een van de volgende namen:

- Naam van de aanvraag voor Standard-header. Een lijst met de naam van de standard-header kan worden gevonden in [RFC 2616](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).
- Naam van de gereserveerde header:
    - doorgestuurd voor
    - host
    - variëren
    - via
    - waarschuwing
    - x doorgestuurd voor
    - Alle headernamen die met "x-EG beginnen" zijn gereserveerd.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="stale-content-delivery-on-error"></a>Verouderde Contentlevering bij fout
**Doel:** bepaalt of verlopen in de cache-inhoud wanneer er een fout optreedt tijdens de hervalidatie van de cache of bij het ophalen van de aangevraagde inhoud op de bronserver van de klant worden geleverd.

Value|Resultaat
-|-
Ingeschakeld|Verouderde content wordt geleverd aan de aanvrager wanneer een fout tijdens een verbinding met een oorspronkelijke server optreedt.
Uitgeschakeld|Fout bij de oorspronkelijke server wordt doorgestuurd naar de aanvrager.

**Standaardgedrag:** uitgeschakeld

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="stale-while-revalidate"></a>Tijdens het Revalidate verlopen
**Doel:** verbetert de prestaties doordat de POP's voor het bieden van verouderde inhoud voor de aanvrager terwijl hervalidatie doorgevoerd wordt.

Belangrijke informatie:

- Er is het gedrag van deze functie afhankelijk van de geselecteerde tijdseenheid.
    - **Tijdseenheid:** een tijdsduur weer te geven en selecteer een tijdseenheid (bijvoorbeeld seconden, minuten, uren, enzovoort) om toe te staan van verouderde levering van inhoud verlagen. Dit type installatieprogramma kunt het CDN om uit te breiden, de hoeveelheid tijd die dit kan worden bezorgd inhoud voordat vereist validatie op basis van de volgende formule: **TTL** + **tijdens Revalidate tijd verlopen** 
    - **Uitgeschakeld:** Selecteer "uitgeschakeld" aan moet worden gevalideerd voordat een aanvraag voor verouderde inhoud kan worden geleverd.
        - Geef een lengte van de tijd niet omdat deze niet van toepassing is en worden genegeerd.

**Standaardgedrag:** uitschakelen. Hervalidatie moet plaatsvinden voordat de aangevraagde inhoud kan worden weergegeven.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="token-auth"></a>Auth-token
**Doel:** wordt bepaald of op tokens gebaseerde verificatie wordt toegepast op een aanvraag.

Als verificatie op basis van het Token is ingeschakeld, wordt alleen aanvragen die een versleutelde beveiligingstoken bieden en te voldoen aan de vereisten die door dit token opgegeven worden herkend.

De versleutelingssleutel die wordt gebruikt om te coderen en decoderen token waarden wordt bepaald door de primaire sleutel en de sleutel van de back-up-opties op de pagina Token verificatie. Houd er rekening mee dat sleutels voor Documentversleuteling platform-specifiek zijn.

**Standaardgedrag:** uitgeschakeld.

Deze functie heeft voorrang op de meeste functies met uitzondering van de functie voor herschrijven van URL's.

Value | Resultaat
------|---------
Ingeschakeld | Beveiligt de gevraagde inhoud met verificatie op basis van tokens. Alleen aanvragen van clients die Geef een geldig token en voldoen aan de vereisten wordt van kracht. FTP-transacties zijn uitgesloten van verificatie op basis van tokens.
Uitgeschakeld| Hiermee herstelt u het standaardgedrag. Het standaardgedrag is om toe te staan van uw tokens gebaseerde authenticatie-configuratie om te bepalen of een aanvraag wordt beveiligd.

#### <a name="compatibility"></a>Compatibiliteit
Gebruik geen Token verificatie met een voorwaarde altijd. 

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="token-auth-denial-code"></a>Weigering van token autorisatiecode
**Doel:** bepaalt het type reactie die aan een gebruiker wordt geretourneerd wanneer een aanvraag wordt geweigerd vanwege de verificatie op basis van tokens.

De beschikbare responscodes worden weergegeven in de volgende tabel.

Antwoordcode|De naam van de reactie|Description
-------------|-------------|--------
301|Permanent verplaatst|Niet-geautoriseerde gebruikers leidt deze statuscode naar de URL die is opgegeven in de Location-header.
302|Gevonden|Niet-geautoriseerde gebruikers leidt deze statuscode naar de URL die is opgegeven in de Location-header. Deze statuscode is de standaardmethode van het uitvoeren van een omleiding.
307|Tijdelijke omleiding|Niet-geautoriseerde gebruikers leidt deze statuscode naar de URL die is opgegeven in de Location-header.
401|Niet geautoriseerd|Deze statuscode combineren met de reactieheader WWW-verificatie, kunt u een gebruiker voor de verificatie wordt gevraagd.
403|Verboden|Dit bericht is de standaard 403 verboden statusbericht dat een onbevoegde gebruiker ziet wanneer u probeert te krijgen tot beveiligde inhoud.
404|Het bestand is niet gevonden|Deze statuscode geeft aan dat de HTTP-client kan communiceren met de server is, maar de aangevraagde inhoud is niet gevonden.

#### <a name="compatibility"></a>Compatibiliteit
Gebruik geen Token autorisatiecode denial-of met een voorwaarde altijd. Gebruik in plaats daarvan de **aangepaste denial-of verwerking** sectie de **Token Authentication** pagina van de **beheren** portal. Zie voor meer informatie, [beveiligen van Azure CDN-assets met tokenverificatie](cdn-token-auth.md).

#### <a name="url-redirection"></a>URL-omleiding

Deze functie biedt ondersteuning voor URL-omleiding naar een door de gebruiker gedefinieerde URL wanneer deze is geconfigureerd voor een statuscode 3xx retourneren. Deze door de gebruiker gedefinieerde URL kan worden opgegeven met de volgende stappen uit:

1. Selecteer een antwoordcode 3xx voor de Token autorisatiecode denial-of-functie.
2. Selecteer "Locatie" in de naam van de optionele Header-optie.
3. De optionele Header-waarde-optie instellen op de gewenste URL.

Als een URL is niet gedefinieerd voor een statuscode 3xx, wordt de pagina standaardreactie voor een statuscode 3xx worden geretourneerd naar de gebruiker.

URL-omleiding is alleen van toepassing voor 3xx responscodes.

De optie optionele Header-waarde ondersteunt alfanumerieke tekens, aanhalingstekens en spaties.

#### <a name="authentication"></a>Authentication

Deze functie ondersteunt de mogelijkheid om op te nemen van de WWW-Authenticate-header reageren op een niet-gemachtigde aanvraag voor inhoud die is beveiligd door verificatie op basis van tokens. Als de WWW-Authenticate-header is ingesteld op 'basic' in de configuratie, wordt klikt u vervolgens de niet-gemachtigde gebruiker gevraagd referenties.

De bovenstaande configuratie kan worden bereikt door de volgende stappen uit:

1. Selecteer '401' als de responscode voor de Token autorisatiecode denial-of-functie.
2. Selecteer 'WWW ' verifiëren in de naam van de optionele Header-optie.
3. Stel de optie optionele Header-waarde voor 'basic'.

De WWW-Authenticate-header is alleen van toepassing voor 401-responscodes.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="token-auth-ignore-url-case"></a>Token Authentication URL hoofdlettergevoeligheid negeren
**Doel:** bepaalt of de URL-vergelijkingen gemaakt door verificatie op basis van tokens hoofdlettergevoelig zijn.

De parameters die worden beïnvloed door deze functie zijn:

- ec_url_allow
- ec_ref_allow
- ec_ref_deny

Geldige waarden zijn:

Value|Resultaat
---|----
Ingeschakeld|Zorgt ervoor dat de POP hoofdlettergevoeligheid niet bij het vergelijken van URL's voor de parameters voor verificatie op basis van tokens.
Uitgeschakeld|Hiermee herstelt u het standaardgedrag. Het standaardgedrag is voor de URL-vergelijkingen voor tokenverificatie zijn hoofdlettergevoelig.

**Standaardgedrag:** uitgeschakeld.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="token-auth-parameter"></a>De Auth Parameter-token
**Doel:** bepaalt of de queryreeks-parameter op tokens gebaseerde verificatie moet worden gewijzigd.

Belangrijke informatie:

- De optie waarde definieert de parameternaam van de query-tekenreeks waarmee een token kan worden opgegeven.
- De optie waarde kan niet worden ingesteld op "ec_token."
- Zorg ervoor dat de naam die is gedefinieerd in de optie waarde alleen geldige URL-tekens bevat.

Value|Resultaat
----|----
Ingeschakeld|De optie waarde definieert de parameternaam van de query-tekenreeks waarmee de tokens moeten worden gedefinieerd.
Uitgeschakeld|Een token worden opgegeven als een niet-gedefinieerde queryreeks-parameter in de aanvraag-URL.

**Standaardgedrag:** uitgeschakeld. Een token worden opgegeven als een niet-gedefinieerde queryreeks-parameter in de aanvraag-URL.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="url-redirect"></a>URL-omleiding
**Doel:** leidt aanvragen via de Location-header.

De configuratie van deze functie is vereist voor het instellen van de volgende opties:

Optie|Description
-|-
Code|Selecteer de responscode die worden geretourneerd aan de aanvrager.
Bron & patroon| Deze instellingen geven aan een aanvraag-URI-patroon dat het type aanvragen dat kan worden omgeleid. Alleen aanvragen waarvan de URL voldoet aan beide van de volgende criteria worden omgeleid: <br/> <br/> **Bron (of inhoud toegangspunt):** een relatief pad waarmee een bronserver selecteren. Dit pad wordt de _/XXXX/_ sectie en de naam van uw eindpunt. <br/><br/> **Bron (patroon):** een patroon dat aanvragen worden aangeduid met het relatieve pad moet worden gedefinieerd. Dit patroon van gewone expressie moet een pad dat begint direct na de toegang tot de eerder geselecteerde inhoud verwijzen (Zie hierboven) definiëren. <br/> -Zorg ervoor dat de aanvraag-URI (dat wil zeggen, bron & patroon) eerder gedefinieerde criteria niet conflicteert met alle criteria voor overeenkomst gedefinieerd voor deze functie. <br/> -Geef een patroon; Als u een lege waarde als het patroon, worden alle tekenreeksen worden vergeleken.
Doel| Definieer de URL waarnaar de bovenstaande aanvragen worden omgeleid. <br/><br/> Dynamisch bouw deze URL met behulp van: <br/> -Een reguliere-expressiepatroon <br/>- [HTTP-variabelen](cdn-http-variables.md) <br/><br/> Vervang de waarden die zijn vastgelegd in het bron-patroon in de doel-patroon met $_n_ waar _n_ geeft een waarde door de volgorde waarin deze is vastgelegd. $1 vertegenwoordigt bijvoorbeeld de eerste waarde die is vastgelegd in het bron-patroon, terwijl $2 geeft aan dat de tweede waarde. <br/> 
Het is raadzaam een absolute URL gebruiken. Het gebruik van een relatieve URL kan CDN-URL's omleiden naar een ongeldig pad.

**Voorbeeldscenario**

In dit voorbeeld ziet u hoe u een rand CNAME-URL die wordt omgezet aan deze basis CDN-URL omleiden: http:\//marketing.azureedge.net/brochures

In aanmerking komende aanvragen worden omgeleid naar deze basis edge CNAME-URL: http:\//cdn.mydomain.com/resources

Deze URL-omleiding kan worden bereikt via de volgende configuratie: ![URL-omleiding](./media/cdn-rules-engine-reference/cdn-rules-engine-redirect.png)

**Belangrijke punten:**

- De functie voor URL-omleiding definieert de aanvraag URL's die worden omgeleid. Criteria voor aanvullende overeenkomst als gevolg hiervan zijn niet vereist. Hoewel de voorwaarde voor overeenkomst is gedefinieerd als 'Altijd', wordt alleen aanvragen die naar de map 'brochures' op de 'marketing' origine verwijzen worden omgeleid. 
- Alle overeenkomende aanvragen omgeleid naar de rand van het CNAME-URL die is gedefinieerd in de doel-optie. 
    - Voorbeeldscenario voor #1: 
        - Voorbeeld van een aanvraag (CDN URL): http:\//marketing.azureedge.net/brochures/widgets.pdf 
        - Aanvraag-URL (na omleiding): http:\//cdn.mydomain.com/resources/widgets.pdf  
    - Voorbeeldscenario voor #2: 
        - Voorbeeld van een aanvraag (Edge CNAME-URL): http:\//marketing.mydomain.com/brochures/widgets.pdf 
        - Aanvraag-URL (na omleiding): http:\//cdn.mydomain.com/resources/widgets.pdf
    - Voorbeeldscenario voor #3: 
        - Voorbeeld van een aanvraag (Edge CNAME-URL): http:\//brochures.mydomain.com/campaignA/final/productC.ppt 
        - Aanvraag-URL (na omleiding): http:\//cdn.mydomain.com/resources/campaignA/final/productC.ppt  
- De variabele aanvraag schema (% {scheme}) wordt gebruikt in de doel-optie, die zorgt ervoor dat het schema van de aanvraag ongewijzigd na omleiding blijft.
- De URL-segmenten die zijn vastgelegd in de aanvraag worden toegevoegd aan de nieuwe URL via "$1."

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="url-rewrite"></a>URL opnieuw genereren
**Doel:** herschrijft de aanvraag-URL.

Belangrijke informatie:

- De configuratie van deze functie is vereist voor het instellen van de volgende opties:

Optie|Description
-|-
 Bron & patroon | Deze instellingen geven aan een aanvraag-URI-patroon dat het type aanvragen dat kan worden herschreven. Er wordt alleen aanvragen waarvan de URL voldoet aan beide van de volgende criteria worden herschreven: <br/><br/>  - **Bron (of inhoud toegangspunt):** een relatief pad waarmee een bronserver selecteren. Dit pad wordt de _/XXXX/_ sectie en de naam van uw eindpunt. <br/><br/> - **Bron (patroon):** een patroon dat aanvragen worden aangeduid met het relatieve pad moet worden gedefinieerd. Dit patroon van gewone expressie moet een pad dat begint direct na de toegang tot de eerder geselecteerde inhoud verwijzen (Zie hierboven) definiëren. <br/> Controleer of dat de aanvraag-URI (dat wil zeggen, bron & patroon) eerder gedefinieerde criteria niet conflicteert met een van de criteria voor overeenkomst gedefinieerd voor deze functie. Geef een patroon; Als u een lege waarde als het patroon, worden alle tekenreeksen worden vergeleken. 
 Doel  |Definieer de relatieve URL waarnaar de bovenstaande aanvragen worden herschreven door: <br/>    1. Een punt voor toegang tot inhoud waarmee een bronserver selecteren. <br/>    2. Definieert een relatief pad gebruiken: <br/>        -Een reguliere-expressiepatroon <br/>        - [HTTP-variabelen](cdn-http-variables.md) <br/> <br/> Vervang de waarden die zijn vastgelegd in het bron-patroon in de doel-patroon met $_n_ waar _n_ geeft een waarde door de volgorde waarin deze is vastgelegd. $1 vertegenwoordigt bijvoorbeeld de eerste waarde die is vastgelegd in het bron-patroon, terwijl $2 geeft aan dat de tweede waarde. 
 Deze functie kunt de POP's te herschrijven van de URL zonder het uitvoeren van een traditionele omleiding. Dat wil zeggen, ontvangt de aanvrager de dezelfde responscode alsof de rewritten URL heeft aangevraagd.

**Voorbeeldscenario 1**

In dit voorbeeld ziet u hoe u een rand CNAME-URL die wordt omgezet aan deze basis CDN-URL omleiden: http:\//marketing.azureedge.net/brochures/

In aanmerking komende aanvragen worden omgeleid naar deze basis edge CNAME-URL: http:\//MyOrigin.azureedge.net/resources/

Deze URL-omleiding kan worden bereikt via de volgende configuratie: ![URL-omleiding](./media/cdn-rules-engine-reference/cdn-rules-engine-rewrite.png)

**Sample Scenario 2**

In dit voorbeeld ziet u hoe u omleiden van een rand CNAME-URL in hoofdletters naar kleine letters reguliere expressies gebruiken.

Deze URL-omleiding kan worden bereikt via de volgende configuratie: ![URL-omleiding](./media/cdn-rules-engine-reference/cdn-rules-engine-to-lowercase.png)


**Belangrijke punten:**

- De functie voor herschrijven van URL's definieert de aanvraag URL's die zal worden herschreven. Criteria voor aanvullende overeenkomst als gevolg hiervan zijn niet vereist. Hoewel de voorwaarde voor overeenkomst is gedefinieerd als 'Altijd', wordt alleen aanvragen die naar de map 'brochures' op de 'marketing' origine verwijzen worden herschreven.

- De URL-segmenten die zijn vastgelegd in de aanvraag worden toegevoegd aan de nieuwe URL via "$1."

#### <a name="compatibility"></a>Compatibiliteit
Deze functie bevat die overeenkomen met criteria die moeten worden voldaan voordat deze kan worden toegepast op een aanvraag. Deze functie is om te voorkomen dat het instellen van conflicterende overeenkomen met criteria, niet compatibel met de volgende criteria voor overeenkomst:

- AS-nummer
- CDN-oorsprong
- IP-adres van client
- Oorsprong van de klant
- Aanvraag-schema
- URL-Path-map
- URL-pad-extensie
- URL-pad bestandsnaam
- URL-pad letterlijke waarde
- URL-pad reguliere expressie
- URL-pad jokertekens
- URL-Query letterlijke waarde
- URL Query Parameter
- URL-Query reguliere expressie
- URL-Query jokertekens

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

---
### <a name="user-variable"></a>Variabele van de gebruiker
**Doel:** alleen voor intern gebruik.

[Terug naar boven](#azure-cdn-rules-engine-features)

</br>

## <a name="next-steps"></a>Volgende stappen
* [Verwijzing regelengine](cdn-rules-engine-reference.md)
* [Voorwaardelijke expressies regelengine](cdn-rules-engine-reference-conditional-expressions.md)
* [Criteria voor overeenkomst regelengine](cdn-rules-engine-reference-match-conditions.md)
* [Met behulp van de regelengine HTTP-gedrag negeren](cdn-rules-engine.md)
* [Overzicht van Azure CDN](cdn-overview.md)

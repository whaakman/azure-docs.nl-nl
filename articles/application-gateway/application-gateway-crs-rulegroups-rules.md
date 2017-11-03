---
title: Azure Application Gateway web application firewall CRS regel groepen en regels | Microsoft Docs
description: Deze pagina bevat informatie over groepen met web application firewall CRS regels en voorschriften.
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: e5ea5cf9-3b41-4b85-a12c-e758bff7f3ec
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 03/28/2017
ms.author: davidmu
ms.openlocfilehash: 9265be4ac4258115c9302189d84b20e4894d42bb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="list-of-web-application-firewall-crs-rule-groups-and-rules-offered"></a>Lijst met groepen met web application firewall CRS regels en regels die worden aangeboden

Application Gateway web application firewall (WAF) beschermt webtoepassingen tegen veelvoorkomende beveiligingslekken en aanvallen. Dit wordt gedaan door regels die zijn gedefinieerd op basis van de OWASP core regelsets 2.2.9 of 3.0. Deze regels kunnen worden uitgeschakeld op basis van de regel door de regel. Dit artikel bevat de huidige regels en rulesets aangeboden.

De volgende tabellen worden de regelgroepen en regels die beschikbaar zijn bij het gebruik van Application Gateway met web application firewall.  Elke tabel vertegenwoordigt de regels in de regelgroep van een voor een specifieke CRS versie gevonden.

##<a name="owasp30"></a>OWASP_3.0

### <a name="crs910"></a>  <p x-ms-format-detection="none">AANVRAAG-910-IP-REPUTATIE</p>

|ruleId|Beschrijving|
|---|---|
|910011|Regel 910011|
|910012|Regel 910012|
|910000|Aanvragen van bekende schadelijke Client (op basis van de vorige verkeer schendingen).|
|910100|Client IP-adres is van een hoog risico land locatie.|
|910120|Regel 910120|
|910130|Regel 910130|
|910150|HTTP-Blacklist overeenkomen voor engine voor het IP-adres zoeken|
|910160|HTTP-Blacklist overeenkomst voor IP-adres van afzender|
|910170|HTTP-Blacklist overeenkomst voor verdachte IP-adres|
|910180|HTTP-Blacklist overeenkomst voor harvester IP-adres|
|910013|Regel 910013|
|910014|Regel 910014|
|910015|Regel 910015|
|910016|Regel 910016|
|910017|Regel 910017|
|910018|Regel 910018|

### <a name="crs911"></a> <p x-ms-format-detection="none">AANVRAAG 911-METHODE AFDWINGEN</p>

|ruleId|Beschrijving|
|---|---|
|911011|Regel 911011|
|911012|Regel 911012|
|911100|Methode is niet toegestaan door beleid|
|911013|Regel 911013|
|911014|Regel 911014|
|911015|Regel 911015|
|911016|Regel 911016|
|911017|Regel 911017|
|911018|Regel 911018|

### <a name="crs912"></a> <p x-ms-format-detection="none">AANVRAAG 912-DOS-BEVEILIGING</p>

|ruleId|Beschrijving|
|---|---|
|912100|Regel 912100|
|912012|Regel 912012|
|912120|Denial of Service (DoS) aanval geïdentificeerd van % @{tx.real_ip} (% @{tx.dos_block_counter} treffers sinds de laatste melding)|
|912130|Regel 912130|
|912140|Regel 912140|
|912150|Regel 912150|
|912160|Regel 912160|
|912170|Aanval Denial of Service (DoS) van % @{tx.real_ip} - # van aanvragen barst = % @{ip.dos_burst_counter}|
|912013|Regel 912013|
|912014|Regel 912014|
|912019|Regel 912019|
|912171|Aanval Denial of Service (DoS) van % @{tx.real_ip} - # van aanvragen barst = % @{ip.dos_burst_counter}|
|912015|Regel 912015|
|912016|Regel 912016|
|912017|Regel 912017|
|912018|Regel 912018|

### <a name="crs913"></a> <p x-ms-format-detection="none">AANVRAAG 913-SCANNER DETECTIE</p>

|ruleId|Beschrijving|
|---|---|
|913011|Regel 913011|
|913012|Regel 913012|
|913100|User-Agent die is gekoppeld aan beveiligingsscanner gevonden|
|913110|Aanvraag-header-beveiligingsscanner gekoppeld gevonden|
|913120|Aanvraag/argument filename gekoppeld beveiligingsscanner gevonden|
|913013|Regel 913013|
|913014|Regel 913014|
|913101|User-Agent die is gekoppeld aan het uitvoeren van scripts/algemene HTTP-client gevonden|
|913102|User-Agent die is gekoppeld aan web crawler/bot gevonden|
|913015|Regel 913015|
|913016|Regel 913016|
|913017|Regel 913017|
|913018|Regel 913018|

### <a name="crs920"></a> <p x-ms-format-detection="none">AANVRAAG 920-PROTOCOL AFDWINGEN</p>

|ruleId|Beschrijving|
|---|---|
|920011|Regel 920011|
|920012|Regel 920012|
|920100|Ongeldige HTTP-aanvraag regel|
|920130|Parseren van de hoofdtekst van de aanvraag is mislukt.|
|920140|Meerdelige aanvraag hoofdtekst strikte validatie is mislukt = PE %@{REQBODY_PROCESSOR_ERROR} bv %@{MULTIPART_BOUNDARY_QUOTED} BW %@{MULTIPART_BOUNDARY_WHITESPACE} DB %@{MULTIPART_DATA_BEFORE} DA %@{MULTIPART_DATA_AFTER} HF %@{MULTIPART_HEADER_FOLDING} LF % @ {MULTIPART_LF_LINE}     SM %@{MULTIPART_SEMICOLON_MISSING} IQ %@{MULTIPART_INVALID_QUOTING} IH %@{MULTIPART_INVALID_HEADER_FOLDING} FLE %@{MULTIPART_FILE_LIMIT_EXCEEDED}|
|920160|De HTTP Content-Length-header is niet numeriek.|
|920170|GET of HEAD aanvragen met inhoud van de hoofdtekst.|
|920180|Ontbrekende Header Content-Length POST-aanvraag.|
|920190|Bereik = Ongeldige laatste bytewaarde.|
|920210|Meerdere/conflicterende koptekstgegevens verbinding gevonden.|
|920220|URL-codering misbruik aanval poging|
|920240|URL-codering misbruik aanval poging|
|920250|UTF8-Codering misbruik aanval poging|
|920260|Unicode-volledige en halve breedte misbruik aanval poging|
|920270|Ongeldig teken in de aanvraag (null-teken)|
|920280|Aanvraag ontbreekt een Host-Header|
|920290|Lege Host-Header|
|920310|Aanvraag heeft een lege Header accepteren|
|920311|Aanvraag heeft een lege Header accepteren|
|920330|Lege gebruiker Agent-Header|
|920340|Aanvragen van inhoud die bevat maar header Content-Type ontbreekt|
|920350|Host-header is een numerieke IP-adres|
|920380|Te veel argumenten in aanvraag|
|920360|Naam van het argument te lang|
|920370|Argumentwaarde is te lang|
|920390|Totaal aantal argumenten is te groot|
|920400|Geüploade bestand te groot|
|920410|Totaal aantal verzonden bestanden te groot|
|920420|Inhoud van het type is niet toegestaan door beleid|
|920430|HTTP-protocolversie is niet toegestaan door beleid|
|920440|URL-bestandsextensie wordt beperkt door het beleid|
|920450|HTTP-header is beperkt door het beleid (%@{MATCHED_VAR})|
|920013|Regel 920013|
|920014|Regel 920014|
|920200|Bereik = te veel velden (6 of hoger)|
|920201|Bereik = te veel velden voor PDF-aanvraag (35 of hoger)|
|920230|Meerdere URL-codering wordt gedetecteerd|
|920300|Aanvraag ontbreekt een Header accepteren|
|920271|Ongeldig teken in de aanvraag (niet-afdrukbare tekens)|
|920320|Ontbrekende Header van de gebruiker Agent|
|920015|Regel 920015|
|920016|Regel 920016|
|920272|Ongeldig teken in de aanvraag (buiten afdrukbare tekens hieronder ascii 127)|
|920017|Regel 920017|
|920018|Regel 920018|
|920202|Bereik = te veel velden voor PDF-aanvraag (6 of hoger)|
|920273|Ongeldig teken in de aanvraag (buiten zeer strikte set)|
|920274|Ongeldig teken in de aanvraagheaders (buiten zeer strikte set)|
|920460|Regel 920460|

### <a name="crs921"></a> <p x-ms-format-detection="none">AANVRAAG-921-PROTOCOL-AANVAL</p>

|ruleId|Beschrijving|
|---|---|
|921011|Regel 921011|
|921012|Regel 921012|
|921100|HTTP-aanvraag smokkelen aanval.|
|921110|HTTP-aanvraag ' smokkelen '-aanval|
|921120|HTTP-antwoord splitsen aanval|
|921130|HTTP-antwoord splitsen aanval|
|921140|HTTP-Header-injectieaanvallen via headers|
|921150|HTTP-Header-injectieaanvallen via nettolading (CR/LF gedetecteerd)|
|921160|HTTP-Header-injectieaanvallen via nettolading (CR/LF en headernaam gedetecteerd)|
|921013|Regel 921013|
|921014|Regel 921014|
|921151|HTTP-Header-injectieaanvallen via nettolading (CR/LF gedetecteerd)|
|921015|Regel 921015|
|921016|Regel 921016|
|921170|Regel 921170|
|921180|HTTP-Parameter verontreiniging (% @{TX.1})|
|921017|Regel 921017|
|921018|Regel 921018|

### <a name="crs930"></a> <p x-ms-format-detection="none">AANVRAAG-930-TOEPASSING-AANVAL-LFI</p>

|ruleId|Beschrijving|
|---|---|
|930011|Regel 930011|
|930012|Regel 930012|
|930100|Pad verandering aanval (/... /)|
|930110|Pad verandering aanval (/... /)|
|930120|Toegangspoging OS-bestand|
|930130|Beperkte bestand toegangspoging|
|930013|Regel 930013|
|930014|Regel 930014|
|930015|Regel 930015|
|930016|Regel 930016|
|930017|Regel 930017|
|930018|Regel 930018|

### <a name="crs931"></a> <p x-ms-format-detection="none">AANVRAAG-931-TOEPASSING-AANVAL-RFI</p>

|ruleId|Beschrijving|
|---|---|
|931011|Regel 931011|
|931012|Regel 931012|
|931100|Mogelijke externe bestand opnemen (RFI)-aanval = URL-Parameter met IP-adres|
|931110|Mogelijke externe bestand opnemen (RFI)-aanval = algemene RFI kwetsbaar parameternaam gebruikt w URL nettolading|
|931120|Mogelijke extern bestand opnemen (RFI) aanval = URL gebruikt nettolading w/afsluitende vraag markeren teken (?)|
|931013|Regel 931013|
|931014|Regel 931014|
|931130|Mogelijke extern bestand opnemen (RFI)-aanval uit domein/koppeling =|
|931015|Regel 931015|
|931016|Regel 931016|
|931017|Regel 931017|
|931018|Regel 931018|

### <a name="crs932"></a> <p x-ms-format-detection="none">AANVRAAG-932-TOEPASSING-AANVAL-DWINGEN</p>

|ruleId|Beschrijving|
|---|---|
|932011|Regel 932011|
|932012|Regel 932012|
|932120|Externe opdrachten uit te voeren Windows PowerShell-opdracht gevonden =|
|932130|Externe opdrachten uit te voeren = Unix Shell-expressie gevonden|
|932140|Externe opdrachten uit te voeren Windows = voor / als de opdracht gevonden|
|932160|Externe opdrachten uit te voeren Unix Shell-Code gevonden =|
|932170|Externe opdrachten uit te voeren = Shellshock (CVE-2014-6271)|
|932171|Externe opdrachten uit te voeren = Shellshock (CVE-2014-6271)|
|932013|Regel 932013|
|932014|Regel 932014|
|932015|Regel 932015|
|932016|Regel 932016|
|932017|Regel 932017|
|932018|Regel 932018|

### <a name="crs933"></a> <p x-ms-format-detection="none">AANVRAAG-933-TOEPASSING-AANVAL-PHP</p>

|ruleId|Beschrijving|
|---|---|
|933011|Regel 933011|
|933012|Regel 933012|
|933100|PHP-injectieaanvallen openende/sluitende label gevonden =|
|933110|PHP-injectieaanvallen = uploaden PHP Script bestand gevonden|
|933120|PHP-injectieaanvallen configuratie-instructie gevonden =|
|933130|PHP-injectieaanvallen variabelen =|
|933150|PHP-injectieaanvallen = de naam van de functie met een hoog risico PHP gevonden|
|933160|PHP-injectieaanvallen met een hoog risico PHP-functieaanroep gevonden =|
|933180|PHP-injectieaanvallen variabele functieaanroep gevonden =|
|933013|Regel 933013|
|933014|Regel 933014|
|933151|PHP-injectieaanvallen = de naam van de functie gemiddeld risico PHP gevonden|
|933015|Regel 933015|
|933016|Regel 933016|
|933131|PHP-injectieaanvallen variabelen =|
|933161|PHP-injectieaanvallen = functieaanroep PHP lage waarde gevonden|
|933111|PHP-injectieaanvallen = uploaden PHP Script bestand gevonden|
|933017|Regel 933017|
|933018|Regel 933018|

### <a name="crs941"></a> <p x-ms-format-detection="none">AANVRAAG-941-TOEPASSING-AANVAL-XSS</p>

|ruleId|Beschrijving|
|---|---|
|941011|Regel 941011|
|941012|Regel 941012|
|941100|XSS-aanval gedetecteerd via libinjection|
|941110|Filter XSS - categorie 1 = Script Tag Vector|
|941130|Filter XSS - categorie 3 = kenmerk Vector|
|941140|Filter XSS - categorie 4 = Javascript-URI-Vector|
|941150|Filter XSS - categorie 5 = niet-toegestane HTML-kenmerken|
|941180|Knooppunt Validator Blacklist trefwoorden|
|941190|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|941200|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|941210|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|941220|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|941230|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|941240|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|941260|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|941270|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|941280|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|941290|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|941300|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|941310|US-ASCII-ongeldige codering XSS-Filter - aanval gedetecteerd.|
|941350|UTF-7 codering IE XSS - aanval gedetecteerd.|
|941013|Regel 941013|
|941014|Regel 941014|
|941320|Mogelijke XSS-aanval gedetecteerd - HTML-Tag-Handler|
|941015|Regel 941015|
|941016|Regel 941016|
|941017|Regel 941017|
|941018|Regel 941018|

### <a name="crs942"></a> <p x-ms-format-detection="none">AANVRAAG-942-TOEPASSING-AANVAL-SQLI</p>

|ruleId|Beschrijving|
|---|---|
|942011|Regel 942011|
|942012|Regel 942012|
|942100|SQL injectie aanval gedetecteerd via libinjection|
|942140|SQL-injectieaanvallen = algemene DB namen gedetecteerd|
|942160|Blind sqli tests met sleep() of benchmark() detecteert.|
|942170|Detecteert benchmark en slaapstand voor SQL-injectie probeert waaronder voorwaardelijke 's|
|942230|Detecteert voorwaardelijke SQL-injectie pogingen|
|942270|Basic sql-injectie zoekt. Algemene aanval tekenreeks voor oracle mysql en anderen.|
|942290|Basic MongoDB SQL-injectie pogingen vindt|
|942320|Detecteert MySQL en PostgreSQL opgeslagen procedure/functie injectie|
|942350|Detecteert MySQL UDF injectie en andere manipulatie structuur-gegevens probeert|
|942013|Regel 942013|
|942014|Regel 942014|
|942150|SQL-injectieaanvallen|
|942410|SQL-injectieaanvallen|
|942440|Volgorde van SQL-opmerking gedetecteerd.|
|942450|SQL hexadecimale codering geïdentificeerd|
|942015|Regel 942015|
|942016|Regel 942016|
|942251|Detecteert HAVING injectie|
|942460|META teken Afwijkingsdetectie detectie waarschuwing - herhalende niet Word tekens|
|942017|Regel 942017|
|942018|Regel 942018|

### <a name="crs943"></a> <p x-ms-format-detection="none">REQUEST-943-APPLICATION-ATTACK-SESSION-FIXATION</p>

|ruleId|Beschrijving|
|---|---|
|943011|Regel 943011|
|943012|Regel 943012|
|943100|Mogelijke sessie vastlegging aanval = cookiewaarden instellen in HTML-code|
|943110|Mogelijke sessie vastlegging aanval = de naam van de sessie-id-Parameter met uit domein verwijzende site|
|943120|Mogelijke sessie vastlegging aanval = de naam van de sessie-id-Parameter met geen verwijzende site|
|943013|Regel 943013|
|943014|Regel 943014|
|943015|Regel 943015|
|943016|Regel 943016|
|943017|Regel 943017|
|943018|Regel 943018|

##<a name="owasp229"></a>OWASP_2.2.9

### <a name="crs20"></a>crs_20_protocol_violations

|ruleId|Beschrijving|
|---|---|
|960911|Ongeldige HTTP-aanvraag regel|
|981227|Apache fout = Ongeldige URI in de aanvraag.|
|960912|Parseren van de hoofdtekst van de aanvraag is mislukt.|
|960914|Meerdelige aanvraag hoofdtekst strikte validatie is mislukt = PE %@{REQBODY_PROCESSOR_ERROR} bv %@{MULTIPART_BOUNDARY_QUOTED} BW %@{MULTIPART_BOUNDARY_WHITESPACE} DB %@{MULTIPART_DATA_BEFORE} DA %@{MULTIPART_DATA_AFTER} HF %@{MULTIPART_HEADER_FOLDING} LF % @ {MULTIPART_LF_LINE}     SM %@{MULTIPART_SEMICOLON_MISSING} IQ %@{MULTIPART_INVALID_QUOTING} IH %@{MULTIPART_INVALID_HEADER_FOLDING} FLE %@{MULTIPART_FILE_LIMIT_EXCEEDED}|
|960915|Meerdelige parser de grens van een mogelijk niet-overeenkomende gedetecteerd.|
|960016|De HTTP Content-Length-header is niet numeriek.|
|960011|GET of HEAD aanvragen met inhoud van de hoofdtekst.|
|960012|Ontbrekende Header Content-Length POST-aanvraag.|
|960902|Ongeldig gebruik van de identiteit codering.|
|960022|Header is niet toegestaan voor HTTP 1.0 wordt verwacht.|
|960020|Cache-Control-Header vereist pragma Header voor HTTP/1.1-aanvragen.|
|958291|Bereik = veld bestaat en begint met 0.|
|958230|Bereik = Ongeldige laatste bytewaarde.|
|958295|Meerdere/conflicterende koptekstgegevens verbinding gevonden.|
|950107|URL-codering misbruik aanval poging|
|950109|Meerdere URL-codering wordt gedetecteerd|
|950108|URL-codering misbruik aanval poging|
|950801|UTF8-Codering misbruik aanval poging|
|950116|Unicode-volledige en halve breedte misbruik aanval poging|
|960901|Ongeldig teken in de aanvraag|
|960018|Ongeldig teken in de aanvraag|

### <a name="crs21"></a>crs_21_protocol_anomalies

|ruleId|Beschrijving|
|---|---|
|960008|Aanvraag ontbreekt een Host-Header|
|960007|Lege Host-Header|
|960015|Aanvraag ontbreekt een Header accepteren|
|960021|Aanvraag heeft een lege Header accepteren|
|960009|Aanvraag ontbreekt een koptekst van de Agent gebruiker|
|960006|Lege gebruiker Agent-Header|
|960904|Aanvragen van inhoud die bevat maar header Content-Type ontbreekt|
|960017|Host-header is een numerieke IP-adres|

### <a name="crs23"></a>crs_23_request_limits

|ruleId|Beschrijving|
|---|---|
|960209|Naam van het argument te lang|
|960208|Argumentwaarde is te lang|
|960335|Te veel argumenten in aanvraag|
|960341|Totaal aantal argumenten is te groot|
|960342|Geüploade bestand te groot|
|960343|Totaal aantal verzonden bestanden te groot|

### <a name="crs30"></a>crs_30_http_policy

|ruleId|Beschrijving|
|---|---|
|960032|Methode is niet toegestaan door beleid|
|960010|Inhoud van het type is niet toegestaan door beleid|
|960034|HTTP-protocolversie is niet toegestaan door beleid|
|960035|URL-bestandsextensie wordt beperkt door het beleid|
|960038|HTTP-header is beperkt door het beleid|

### <a name="crs35"></a>crs_35_bad_robots

|ruleId|Beschrijving|
|---|---|
|990002|Aanvraag geeft aan dat een beveiligingsscanner gescand de Site|
|990901|Aanvraag geeft aan dat een beveiligingsscanner gescand de Site|
|990902|Aanvraag geeft aan dat een beveiligingsscanner gescand de Site|
|990012|Crawlermetagegevens: Rogue-website|

### <a name="crs40"></a>crs_40_generic_attacks

|ruleId|Beschrijving|
|---|---|
|960024|META teken Afwijkingsdetectie detectie waarschuwing - herhalende niet Word tekens|
|950008|Injectie van niet-gedocumenteerde ColdFusion-Tags|
|950010|LDAP-injectieaanvallen|
|950011|SSI injectie aanval|
|950018|Universele PDF XSS-URL is gedetecteerd.|
|950019|E-injectieaanvallen|
|950012|HTTP-aanvraag smokkelen aanval.|
|950910|HTTP-antwoord splitsen aanval|
|950911|HTTP-antwoord splitsen aanval|
|950117|Extern bestand insluitings-aanval|
|950118|Extern bestand insluitings-aanval|
|950119|Extern bestand insluitings-aanval|
|950120|Mogelijke extern bestand opnemen (RFI)-aanval uit domein/koppeling =|
|981133|Regel 981133|
|981134|Regel 981134|
|950009|Sessie vastlegging aanval|
|950003|Sessie vastlegging|
|950000|Sessie vastlegging|
|950005|Poging van externe toegang|
|950002|Toegang tot het systeem-opdracht|
|950006|Systeem opdracht injectie|
|959151|PHP-injectieaanvallen|
|958976|PHP-injectieaanvallen|
|958977|PHP-injectieaanvallen|

### <a name="crs41sql"></a>crs_41_sql_injection_attacks

|ruleId|Beschrijving|
|---|---|
|981231|Volgorde van SQL-opmerking gedetecteerd.|
|981260|SQL hexadecimale codering geïdentificeerd|
|981320|SQL-injectieaanvallen = algemene DB namen gedetecteerd|
|981300|Regel 981300|
|981301|Regel 981301|
|981302|Regel 981302|
|981303|Regel 981303|
|981304|Regel 981304|
|981305|Regel 981305|
|981306|Regel 981306|
|981307|Regel 981307|
|981308|Regel 981308|
|981309|Regel 981309|
|981310|Regel 981310|
|981311|Regel 981311|
|981312|Regel 981312|
|981313|Regel 981313|
|981314|Regel 981314|
|981315|Regel 981315|
|981316|Regel 981316|
|981317|Waarschuwing voor Afwijkingsdetectie SQL SELECT-instructie|
|950007|Blind SQL-injectieaanvallen|
|950001|SQL-injectieaanvallen|
|950908|SQL-injectieaanvallen.|
|959073|SQL-injectieaanvallen|
|981272|Blind sqli tests met sleep() of benchmark() detecteert.|
|981250|Detecteert benchmark en slaapstand voor SQL-injectie probeert waaronder voorwaardelijke 's|
|981241|Detecteert voorwaardelijke SQL-injectie pogingen|
|981276|Basic sql-injectie zoekt. Algemene aanval tekenreeks voor oracle mysql en anderen.|
|981270|Basic MongoDB SQL-injectie pogingen vindt|
|981253|Detecteert MySQL en PostgreSQL opgeslagen procedure/functie injectie|
|981251|Detecteert MySQL UDF injectie en andere manipulatie structuur-gegevens probeert|

### <a name="crs41xss"></a>crs_41_xss_attacks

|ruleId|Beschrijving|
|---|---|
|973336|Filter XSS - categorie 1 = Script Tag Vector|
|973338|Filter XSS - categorie 3 = Javascript-URI-Vector|
|981136|Regel 981136|
|981018|Regel 981018|
|958016|Aanval cross-site Scripting (XSS)|
|958414|Aanval cross-site Scripting (XSS)|
|958032|Aanval cross-site Scripting (XSS)|
|958026|Aanval cross-site Scripting (XSS)|
|958027|Aanval cross-site Scripting (XSS)|
|958054|Aanval cross-site Scripting (XSS)|
|958418|Aanval cross-site Scripting (XSS)|
|958034|Aanval cross-site Scripting (XSS)|
|958019|Aanval cross-site Scripting (XSS)|
|958013|Aanval cross-site Scripting (XSS)|
|958408|Aanval cross-site Scripting (XSS)|
|958012|Aanval cross-site Scripting (XSS)|
|958423|Aanval cross-site Scripting (XSS)|
|958002|Aanval cross-site Scripting (XSS)|
|958017|Aanval cross-site Scripting (XSS)|
|958007|Aanval cross-site Scripting (XSS)|
|958047|Aanval cross-site Scripting (XSS)|
|958410|Aanval cross-site Scripting (XSS)|
|958415|Aanval cross-site Scripting (XSS)|
|958022|Aanval cross-site Scripting (XSS)|
|958405|Aanval cross-site Scripting (XSS)|
|958419|Aanval cross-site Scripting (XSS)|
|958028|Aanval cross-site Scripting (XSS)|
|958057|Aanval cross-site Scripting (XSS)|
|958031|Aanval cross-site Scripting (XSS)|
|958006|Aanval cross-site Scripting (XSS)|
|958033|Aanval cross-site Scripting (XSS)|
|958038|Aanval cross-site Scripting (XSS)|
|958409|Aanval cross-site Scripting (XSS)|
|958001|Aanval cross-site Scripting (XSS)|
|958005|Aanval cross-site Scripting (XSS)|
|958404|Aanval cross-site Scripting (XSS)|
|958023|Aanval cross-site Scripting (XSS)|
|958010|Aanval cross-site Scripting (XSS)|
|958411|Aanval cross-site Scripting (XSS)|
|958422|Aanval cross-site Scripting (XSS)|
|958036|Aanval cross-site Scripting (XSS)|
|958000|Aanval cross-site Scripting (XSS)|
|958018|Aanval cross-site Scripting (XSS)|
|958406|Aanval cross-site Scripting (XSS)|
|958040|Aanval cross-site Scripting (XSS)|
|958052|Aanval cross-site Scripting (XSS)|
|958037|Aanval cross-site Scripting (XSS)|
|958049|Aanval cross-site Scripting (XSS)|
|958030|Aanval cross-site Scripting (XSS)|
|958041|Aanval cross-site Scripting (XSS)|
|958416|Aanval cross-site Scripting (XSS)|
|958024|Aanval cross-site Scripting (XSS)|
|958059|Aanval cross-site Scripting (XSS)|
|958417|Aanval cross-site Scripting (XSS)|
|958020|Aanval cross-site Scripting (XSS)|
|958045|Aanval cross-site Scripting (XSS)|
|958004|Aanval cross-site Scripting (XSS)|
|958421|Aanval cross-site Scripting (XSS)|
|958009|Aanval cross-site Scripting (XSS)|
|958025|Aanval cross-site Scripting (XSS)|
|958413|Aanval cross-site Scripting (XSS)|
|958051|Aanval cross-site Scripting (XSS)|
|958420|Aanval cross-site Scripting (XSS)|
|958407|Aanval cross-site Scripting (XSS)|
|958056|Aanval cross-site Scripting (XSS)|
|958011|Aanval cross-site Scripting (XSS)|
|958412|Aanval cross-site Scripting (XSS)|
|958008|Aanval cross-site Scripting (XSS)|
|958046|Aanval cross-site Scripting (XSS)|
|958039|Aanval cross-site Scripting (XSS)|
|958003|Aanval cross-site Scripting (XSS)|
|973300|Mogelijke XSS-aanval gedetecteerd - HTML-Tag-Handler|
|973301|XSS-aanval gedetecteerd|
|973302|XSS-aanval gedetecteerd|
|973303|XSS-aanval gedetecteerd|
|973304|XSS-aanval gedetecteerd|
|973305|XSS-aanval gedetecteerd|
|973306|XSS-aanval gedetecteerd|
|973307|XSS-aanval gedetecteerd|
|973308|XSS-aanval gedetecteerd|
|973309|XSS-aanval gedetecteerd|
|973311|XSS-aanval gedetecteerd|
|973313|XSS-aanval gedetecteerd|
|973314|XSS-aanval gedetecteerd|
|973331|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973315|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973330|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973327|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973326|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973346|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973345|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973324|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973323|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973348|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973321|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973320|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973318|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973317|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973329|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973328|Filters in Internet Explorer XSS - aanval gedetecteerd.|

### <a name="crs42"></a>crs_42_tight_security

|ruleId|Beschrijving|
|---|---|
|950103|Pad verandering aanval|

### <a name="crs45"></a>crs_45_trojans

|ruleId|Beschrijving|
|---|---|
|950110|Via een achterdeur toegang|
|950921|Via een achterdeur toegang|
|950922|Via een achterdeur toegang|

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het uitschakelen van WAF regels met bezoeken: [regels WAF aanpassen](application-gateway-customize-waf-rules-portal.md)

[1]: ./media/application-gateway-integration-security-center/figure1.png
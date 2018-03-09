---
title: Azure CDN regels overeen motor | Microsoft Docs
description: Documentatie bij Azure Content Delivery Network regels motor overeen.
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2017
ms.author: rli
ms.openlocfilehash: e4b7113f27e5e15d69dfdd1efd13e255ef4a8ab7
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="azure-cdn-rules-engine-match-conditions"></a>Voldoen aan de engine van Azure CDN-regels 
Dit artikel vindt u gedetailleerde beschrijvingen van de beschikbare overeenkomst voorwaarden voor het Azure Content Delivery Network (CDN) [regelengine](cdn-rules-engine.md).

Het tweede gedeelte van een regel wordt de voorwaarde van de overeenkomst. De voorwaarde van een overeenkomst identificeert specifieke typen aanvragen waarvoor een reeks functies worden uitgevoerd.

U kunt bijvoorbeeld een voorwaarde overeenkomst te gebruiken:
- Aanvragen filteren voor inhoud op een bepaalde locatie.
- Aanvragen filteren is gegenereerd op basis van een bepaald IP-adres of een land.
- Aanvragen filteren door de header-informatie.

## <a name="always-match-condition"></a>Altijd overeenkomen met de voorwaarde

De voorwaarde van de overeenkomst altijd geldt een aantal functies voor alle aanvragen.

Naam | Doel
-----|--------
[Altijd](#always) | Een aantal functies geldt voor alle aanvragen.

## <a name="device-match-condition"></a>Apparaat overeen voorwaarde

De voorwaarde van de overeenkomst apparaat identificeert aanvragen van een mobiel apparaat op basis van de eigenschappen ervan.  

Naam | Doel
-----|--------
[Apparaat](#device) | Aanvragen vanaf een mobiel apparaat op basis van de eigenschappen ervan identificeert.

## <a name="location-match-conditions"></a>Locatie overeenkomst voorwaarden

De voorwaarden van de overeenkomst locatie identificeren aanvragen op basis van de locatie van de aanvrager.

Naam | Doel
-----|--------
[AS-nummer](#as-number) | Aanvragen die afkomstig van een bepaald netwerk zijn identificeert.
[Land](#country) | Aanvragen die afkomstig uit de opgegeven landen zijn identificeert.

## <a name="origin-match-conditions"></a>Oorsprong overeenkomst voorwaarden

De voorwaarden van de overeenkomst oorsprong identificeren aanvragen die naar het Content Delivery Network-opslag- of een klant-bronserver verwijzen.

Naam | Doel
-----|--------
[CDN-oorsprong](#cdn-origin) | Aanvragen voor inhoud die is opgeslagen in de opslag van inhoud Delivery Network identificeert.
[Oorsprong van de klant](#customer-origin) | Aanvragen voor inhoud die is opgeslagen op een specifieke klant oorsprong server identificeert.

## <a name="request-match-conditions"></a>Aanvraag overeenkomst voorwaarden

De voorwaarden van de overeenkomst aanvraag identificeren aanvragen op basis van hun eigenschappen.

Naam | Doel
-----|--------
[IP-clientadres](#client-ip-address) | Aanvragen die afkomstig van een bepaald IP-adres zijn identificeert.
[Cookie Parameter](#cookie-parameter) | Controleert de cookies die zijn gekoppeld aan elke aanvraag naar de opgegeven waarde.
[Cookie Parameter Regex](#cookie-parameter-regex) | Controleert de cookies die zijn gekoppeld aan elke aanvraag voor de opgegeven reguliere expressie.
[Rand Cname](#edge-cname) | Aanvragen die naar een specifieke rand CNAME verwijzen identificeert.
[Verwijzende domein](#referring-domain) | Aanvragen die zijn aangeduid van de opgegeven hostnamen identificeert.
[Aanvraag-Header Literal](#request-header-literal) | Geeft aan dat aanvragen met de opgegeven header is ingesteld op een opgegeven waarde.
[Aanvraag-Header Regex](#request-header-regex) | Geeft aan dat aanvragen met de opgegeven header is ingesteld op een waarde die overeenkomt met de opgegeven reguliere expressie.
[Aanvraag-Header jokertekens](#request-header-wildcard) | Geeft aan dat aanvragen met de opgegeven header is ingesteld op een waarde die overeenkomt met het opgegeven patroon.
[Verzoekmethode](#request-method) | Identificeert aanvragen door de HTTP-methode.
[Aanvraag-schema](#request-scheme) | Identificeert aanvragen door de HTTP-protocol.

## <a name="url-match-conditions"></a>Criteria voor URL-overeenkomst

De voorwaarden van de overeenkomst URL identificeren aanvragen op basis van de URL's.

Naam | Doel
-----|--------
[URL-pad naar map](#url-path-directory) | Identificeert aanvragen door hun relatief pad.
[URL-pad-uitbreiding](#url-path-extension) | Identificeert aanvragen door de bestandsnaamextensie.
[URL-pad bestandsnaam](#url-path-filename) | Identificeert aanvragen door hun naam.
[URL-pad Literal](#url-path-literal) | Vergelijkt de relatieve pad van een aanvraag met de opgegeven waarde.
[URL-pad Regex](#url-path-regex) | Vergelijkt relatieve pad van een aanvraag naar de opgegeven reguliere expressie.
[URL-pad jokerteken](#url-path-wildcard) | Vergelijkt relatieve pad van een aanvraag naar het opgegeven patroon.
[URL-Query Literal](#url-query-literal) | Vergelijkt de queryreeks van een aanvraag met de opgegeven waarde.
[URL-queryparameter](#url-query-parameter) | Geeft aan dat aanvragen met de opgegeven tekenreeks queryparameter ingesteld op een waarde die overeenkomt met een opgegeven patroon.
[URL-Query Regex](#url-query-regex) | Geeft aan dat aanvragen met de opgegeven tekenreeks queryparameter ingesteld op een waarde die overeenkomt met een opgegeven reguliere expressie.
[URL-Query jokertekens](#url-query-wildcard) | De opgegeven waarde op basis van de queryreeks van de aanvraag wordt vergeleken.


## <a name="reference-for-rules-engine-match-conditions"></a>Verwijzing voor de overeenkomst motor regels

---
### <a name="always"></a>Altijd

De voorwaarde van de overeenkomst altijd geldt een aantal functies voor alle aanvragen.

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="as-number"></a>AS-nummer 
Het AS-nummer-netwerk wordt gedefinieerd door de autonoom systeemnummer (ASN). 

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder het AS-nummer overeenkomen met de voorwaarde is voldaan:
- **Komt overeen met**: is vereist dat het ASN van het clientnetwerk overeenkomt met een van de opgegeven ASN's. 
- **Komt niet overeen met**: is vereist dat het ASN van het clientnetwerk komt niet overeen met een van de opgegeven ASN's.

Belangrijke informatie:
- Geef meerdere ASN's door die begrenst elkaar met een spatie. 64514 64515 komt bijvoorbeeld overeen met aanvragen die van de 64514 of 64515 binnenkomen.
- Bepaalde aanvragen mogelijk geen geldige ASN niet geretourneerd. Een vraagteken (?) komt overeen met aanvragen waarvoor een geldige ASN kan niet worden bepaald.
- Geef de hele ASN worden gebruikt voor het gewenste netwerk. Ondersteuning voor gedeeltelijke waarden wordt niet overeen.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Force Internal Max-Age
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="cdn-origin"></a>CDN-oorsprong
De CDN oorsprong overeen-voorwaarde wordt voldaan wanneer beide volgende voorwaarden wordt voldaan:
- Inhoud uit CDN opslag is aangevraagd.
- De aanvraag-URI wordt gebruikt voor het type inhoud toegangspunt (bijvoorbeeld /000001) dat gedefinieerd in deze overeenkomst voorwaarde:
  - CDN-URL: De aanvraag-URI moet het toegangspunt voor het geselecteerde inhoud bevatten.
  - Rand CNAME-URL: De configuratie van de bijbehorende rand CNAME moet verwijzen naar de geselecteerde inhoud access point.
  
Belangrijke informatie:
 - Het toegangspunt inhoud de service identificeert die de gevraagde inhoud fungeren moet.
 - Gebruik een instructie en als niet aan bepaalde voorwaarden overeen combineren. Een voorwaarde CDN oorsprong overeen met de voorwaarde van een klant oorsprong overeen combineren zou bijvoorbeeld een patroon overeen die nooit overeen maken. Om deze reden kunnen niet twee CDN oorsprong overeenkomst voorwaarden via een instructie en als worden gecombineerd.

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="client-ip-address"></a>IP-adres van client
De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder het IP-adres van de Client overeenkomen met de voorwaarde is voldaan:
- **Komt overeen met**: is vereist dat de client-IP-adres overeenkomt met een van de opgegeven IP-adressen. 
- **Komt niet overeen met**: is vereist dat de client-IP-adres komt niet overeen met een van de opgegeven IP-adressen. 

Belangrijke informatie:
- Gebruik de CIDR-notatie.
- Geef meerdere IP-adressen en/of IP-adresblokken door die begrenst elkaar met een spatie. Bijvoorbeeld:
  - **IPv4-voorbeeld**: 1.2.3.4 10.20.30.40 komt overeen met alle aanvragen die van adres 1.2.3.4 of 10.20.30.40 binnenkomen.
  - **IPv6-voorbeeld**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 komt overeen met alle aanvragen die van het adres 1:2:3:4:5:6:7:8 of 10:20:30:40:50:60:70:80 binnenkomen.
- De syntaxis voor een IP-Adresblok is het basisadres van de IP-gevolgd door een slash en de grootte van het voorvoegsel. Bijvoorbeeld:
  - **IPv4-voorbeeld**: 5.5.5.64/26 komt overeen met alle aanvragen die van de adressen 5.5.5.64 via 5.5.5.127 binnenkomen.
  - **IPv6-voorbeeld**: 1:2:3: / 48 komt overeen met alle aanvragen die van de adressen 1:2:3:0:0:0:0:0 via 1:2:3:ffff:ffff:ffff:ffff:ffff binnenkomen.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Force Internal Max-Age
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="cookie-parameter"></a>Cookie Parameter
De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de Cookie-Parameter overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: vereist een aanvraag voor de opgegeven cookie met een waarde die overeenkomt met ten minste een van de waarden die zijn gedefinieerd in deze overeenkomst voorwaarde bevatten.
- **Komt niet overeen met**: is vereist dat de aanvraag voldoet aan een van de volgende criteria:
  - Het bevat niet de opgegeven cookie.
  - De opgegeven cookie bevat, maar de waarde komt niet overeen met een van de waarden die zijn gedefinieerd in deze overeenkomst voorwaarde.
  
Belangrijke informatie:
- Cookienaam: 
  - Omdat de jokerteken-waarden, waaronder sterretjes (*) worden niet ondersteund wanneer u een Cookienaam opgeven, komen alleen exacte cookie naam komt overeen met in aanmerking voor vergelijking.
  - Alleen de naam van een enkele cookie kan per exemplaar van deze overeenkomst voorwaarde worden opgegeven.
  - Cookie naam vergelijkingen zijn niet hoofdlettergevoelig.
- Cookiewaarde: 
  - Meerdere cookiewaarden opgeven door die begrenst elkaar met een spatie.
  - De waarde van een cookie kan profiteren van [jokertekens waarden](cdn-rules-engine-reference.md#wildcard-values). 
  - Als een jokerteken niet opgegeven is, wordt alleen een exacte overeenkomst deze overeenkomst voorwaarde voldoen. Bijvoorbeeld ' waarde ' komt overeen met '' waarde '', maar niet 'Value1' of 'Waarde2'.
  - Gebruik de **negeren geval** optie om te bepalen of een hoofdlettergevoelige vergelijking wordt gemaakt met de waarde van de aanvraag-cookie.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Force Internal Max-Age
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="cookie-parameter-regex"></a>Cookie Parameter Regex
De voorwaarde van de overeenkomst Cookie Parameter Regex definieert een Cookienaam en waarde. U kunt [reguliere expressies](cdn-rules-engine-reference.md#regular-expressions) voor het definiëren van de gewenste cookiewaarde. 

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de Cookie Parameter Regex overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: vereist een aanvraag voor de opgegeven cookie met een waarde die overeenkomt met de opgegeven reguliere expressie bevatten.
- **Komt niet overeen met**: is vereist dat de aanvraag voldoet aan een van de volgende criteria:
  - Het bevat niet de opgegeven cookie.
  - De opgegeven cookie bevat, maar de waarde komt niet overeen met de opgegeven reguliere expressie.
  
Belangrijke informatie:
- Cookienaam: 
  - Omdat reguliere expressies en jokerteken waarden, met inbegrip van sterretjes (*) worden niet ondersteund wanneer u een Cookienaam opgeven, komen alleen exacte cookie naam komt overeen met in aanmerking voor vergelijking.
  - Alleen de naam van een enkele cookie kan per exemplaar van deze overeenkomst voorwaarde worden opgegeven.
  - Cookie naam vergelijkingen zijn niet hoofdlettergevoelig.
- Cookiewaarde: 
  - De waarde van een cookie kunt profiteren van reguliere expressies.
  - Gebruik de **negeren geval** optie om te bepalen of een hoofdlettergevoelige vergelijking wordt gemaakt met de waarde van de aanvraag-cookie.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Force Internal Max-Age
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

--- 
### <a name="country"></a>Land
U kunt een land via de landcode opgeven. 

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder het land overeenkomen met de voorwaarde is voldaan:
- **Komt overeen met**: de aanvraag bevat de waarden voor het opgegeven land vereist. 
- **Komt niet overeen met**: de aanvraag bevat geen waarden voor het opgegeven land vereist.

Belangrijke informatie:
- Geef meerdere landcodes door die begrenst elkaar met een spatie.
- Jokertekens worden niet ondersteund wanneer u bij het opgeven van een landcode.
- De landcodes 'EU' en 'AP' kunnen niet alle IP-adressen in die gebieden omvatten.
- Bepaalde aanvragen mogelijk een geldige landcode niet geretourneerd. Een vraagteken (?) komt overeen met aanvragen waarvoor een geldige landcode kan niet worden bepaald.
- Landcodes zijn hoofdlettergevoelig.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Force Internal Max-Age
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

#### <a name="implementing-country-filtering-by-using-the-rules-engine"></a>Landen filteren implementeren met behulp van de regelengine voor
Deze voorwaarde overeen Hiermee kunt u een groot aantal aanpassingen op basis van de locatie van waaruit een aanvraag afkomstig is. Het gedrag van de functie landen filteren kan bijvoorbeeld worden gerepliceerd door de volgende configuratie:

- URL-pad jokerteken: Stel de [URL pad jokerteken overeenkomen met de voorwaarde](#url-path-wildcard) naar de map die worden beveiligd. 
    Een sterretje aan het einde van het relatieve pad om ervoor te zorgen dat toegang tot alle onderliggende zal worden beperkt door deze regel toevoegen.

- Land overeen: de voorwaarde van de overeenkomst land ingesteld op de gewenste set landen.
   - Het land overeen toestaan: Stel voorwaarde moet **komt niet overeen met** zodat alleen de opgegeven landen toegang hebben tot inhoud die is opgeslagen in de locatie die is gedefinieerd door de voorwaarde van de overeenkomst jokerteken voor URL-pad.
   - Blokkeren: Stel het land overeen voorwaarde **komt overeen met** blokkeren van de opgegeven landen toegang krijgen tot inhoud opgeslagen in de locatie die is gedefinieerd door de voorwaarde van de overeenkomst jokerteken voor URL-pad.

- Functie voor weigeren toegang (403): Inschakelen van de [toegang weigeren (403) functie](cdn-rules-engine-reference-features.md#deny-access-403) voor replicatie van het gedeelte toestaan of blokkeren van de functie landen filteren.

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="customer-origin"></a>Oorsprong van de klant

Belangrijke informatie: 
- De klant oorsprong overeen voorwaarde wordt voldaan ongeacht of de inhoud wordt aangevraagd door middel van een CDN-URL of een edge CNAME-URL die naar de oorsprong van de geselecteerde klant verwijst.
- De configuratie van een klant-oorsprong waarnaar wordt verwezen door een regel kan niet worden verwijderd uit de pagina van de oorsprong van de klant. Voordat u probeert te verwijderen van de configuratie van een klant oorsprong, controleert u dat de volgende configuraties niet verwijzen naar deze:
  - Een klant oorsprong overeen voorwaarde
  - Een edge CNAME-configuratie
- Gebruik een instructie en als niet aan bepaalde voorwaarden overeen combineren. Een voorwaarde klant oorsprong overeen met de voorwaarde van een oorsprong CDN overeen combineren zou bijvoorbeeld een patroon overeen die nooit overeen maken. Om deze reden kunnen niet via een instructie en als worden gecombineerd met twee voorwaarden van de oorsprong van de klant overeen.

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="device"></a>Apparaat

De voorwaarde van de overeenkomst apparaat identificeert aanvragen van een mobiel apparaat op basis van de eigenschappen ervan. Detectie van mobiele apparaten wordt bereikt door [WURFL](http://wurfl.sourceforge.net/). 

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder het apparaat overeenkomen met de voorwaarde is voldaan:
- **Komt overeen met**: de aanvrager apparaat moet overeenkomen met de opgegeven waarde. 
- **Komt niet overeen met**: apparaat van de aanvrager komt niet overeen met de opgegeven waarde is vereist.

Belangrijke informatie:

- Gebruik de **negeren geval** optie om op te geven of de opgegeven waarde hoofdlettergevoelig is.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Force Internal Max-Age
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

#### <a name="string-type"></a>Type String
Een mogelijkheid WURFL accepteert meestal een combinatie van getallen, letters en symbolen. Vanwege de flexibele aard van deze mogelijkheid, moet u kiezen hoe de waarde die is gekoppeld aan deze voorwaarde overeenkomst wordt geïnterpreteerd. De volgende tabel beschrijft de set beschikbare opties:

Type     | Beschrijving
---------|------------
Letterlijke  | Selecteer deze optie om te voorkomen dat de meeste tekens op een speciale betekenis met behulp van hun [letterlijke waarde](cdn-rules-engine-reference.md#literal-values).
Jokertekens | Selecteer deze optie om te profiteren van alle [jokertekens] ([jokertekens waarden](cdn-rules-engine-reference.md#wildcard-values).
Reguliere expressie    | Selecteer deze optie gebruiken [reguliere expressies](cdn-rules-engine-reference.md#regular-expressions). Reguliere expressies zijn handig voor het definiëren van een patroon van tekens.

#### <a name="wurfl-capabilities"></a>WURFL mogelijkheden
Een mogelijkheid WURFL verwijst naar een categorie die mobiele apparaten worden beschreven. De geselecteerde mogelijkheid bepaalt het type van de beschrijving van mobiele apparaten die wordt gebruikt om aanvragen te identificeren.

De volgende tabel bevat WURFL mogelijkheden en hun variabelen voor de regelengine voor.
<br>
> [!NOTE] 
> De volgende variabelen worden ondersteund in de **Client aanvraag-Header wijzigen** en **Client antwoordheader wijzigen** functies.

Mogelijkheid | Variabele | Beschrijving | Voorbeeldwaarden
-----------|----------|-------------|----------------
De naam van het merk | %{wurfl_cap_brand_name} | Een tekenreeks die de naam van het merk van het apparaat geeft. | Samsung
Besturingssysteem van het apparaat | %{wurfl_cap_device_os} | Een tekenreeks die aangeeft van het besturingssysteem op het apparaat geïnstalleerd. | IOS
Versie van apparaatbesturingssysteem | %{wurfl_cap_device_os_version} | Een tekenreeks die het versienummer van het besturingssysteem op het apparaat geïnstalleerd geeft. | 1.0.1
Dual afdrukstand | %{wurfl_cap_dual_orientation} | Een Booleaanse waarde die aangeeft of het apparaat dual afdrukstand ondersteunt. | true
HTML voorkeur DTD | %{wurfl_cap_html_preferred_dtd} | Een tekenreeks die typedefinitie (DTD) voor HTML-inhoud van het mobiele apparaat voorkeur document aangeeft. | geen<br/>xhtml_basic<br/>html5
Afbeelding Inlining | %{wurfl_cap_image_inlining} | Een Boolean die aangeeft of het apparaat ondersteunt met Base64 gecodeerd installatiekopieën. | false
Is Android | %{wurfl_vcap_is_android} | Een Booleaanse waarde die aangeeft of het apparaat Android OS gebruikt. | true
IOS is | %{wurfl_vcap_is_ios} | Een Booleaanse waarde die aangeeft of het apparaat iOS gebruikt. | false
Is Smart TV | %{wurfl_cap_is_smarttv} | Een Booleaanse waarde die aangeeft of het apparaat een intelligente TV is. | false
Smartphone is | %{wurfl_vcap_is_smartphone} | Een Booleaanse waarde die aangeeft of het apparaat een smartphone is. | true
Is Tablet | %{wurfl_cap_is_tablet} | Een Booleaanse waarde die aangeeft of het apparaat een tablet is. Deze beschrijving is onafhankelijk van het besturingssysteem. | true
Draadloze apparaat | %{wurfl_cap_is_wireless_device} | Een Booleaanse waarde die aangeeft of het apparaat wordt beschouwd als een draadloos apparaat. | true
De naam van marketing | %{wurfl_cap_marketing_name} | Een tekenreeks die marketing naam van het apparaat aangeeft. | BlackBerry 8100 Pearl
Mobile Browser | %{wurfl_cap_mobile_browser} | Een tekenreeks die aangeeft van de browser die wordt gebruikt voor het aanvragen van inhoud van het apparaat. | Chrome
Mobiele browserversie | %{wurfl_cap_mobile_browser_version} | Een tekenreeks die de versie van de browser die wordt gebruikt voor het aanvragen van inhoud van het apparaat aangeeft. | 31
Modelnaam | %{wurfl_cap_model_name} | Een tekenreeks die modelnaam van het apparaat aangeeft. | s3
Progressief downloaden | %{wurfl_cap_progressive_download} | Een Booleaanse waarde die aangeeft of het apparaat ondersteuning biedt voor het afspelen van audio en video terwijl het nog steeds wordt gedownload. | true
Releasedatum | %{wurfl_cap_release_date} | Een tekenreeks die het jaar en maand op waarop het apparaat is toegevoegd aan de database WURFL aangeeft.<br/><br/>Indeling: `yyyy_mm` | 2013_december
Hoogte van de oplossing | %{wurfl_cap_resolution_height} | Een geheel getal dat van het apparaat hoogte in pixels aangeeft. | 768
Breedte van de oplossing | %{wurfl_cap_resolution_width} | Een geheel getal dat aangeeft het apparaat de breedte in pixels dat. | 1024

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="edge-cname"></a>Rand Cname
Belangrijke informatie: 
- De lijst met beschikbare rand CNAME-records is beperkt tot deze rand CNAMEs die zijn geconfigureerd op de pagina rand CNAME-records voor het platform waarop de regelengine wordt geconfigureerd.
- Voordat u probeert te verwijderen van een CNAME-rand configuratie, ervoor zorgen dat een overeenkomst rand Cname voorwaarde verwijst niet naar het. Rand CNAME-configuraties die zijn gedefinieerd in een regel kunnen niet worden verwijderd uit de pagina rand CNAME-records. 
- Gebruik een instructie en als niet aan bepaalde voorwaarden overeen combineren. Een voorwaarde rand Cname overeen met de voorwaarde van een klant oorsprong overeen combineren zou bijvoorbeeld een patroon overeen die nooit overeen maken. Om deze reden kunnen niet via een instructie en als worden gecombineerd met twee voorwaarden van de rand Cname overeen.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Force Internal Max-Age
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="referring-domain"></a>Verwijzende domein
De hostnaam die is gekoppeld aan de verwijzende site waarmee inhoud is aangevraagd, wordt bepaald of het domein verwijst-voorwaarde is voldaan. 

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder het domein verwijst overeenkomen met de voorwaarde is voldaan:
- **Komt overeen met**: vereist de verwijzende hostnaam overeenkomen met de opgegeven waarden. 
- **Komt niet overeen met**: de verwijzende hostnaam komt niet overeen met de opgegeven waarde is vereist.

Belangrijke informatie:
- Geef meerdere hostnamen door die begrenst elkaar met een spatie.
- Deze voorwaarde overeen ondersteunt [jokertekens waarden](cdn-rules-engine-reference.md#wildcard-values).
- Als de opgegeven waarde geen een sterretje bevat, moet een exacte overeenkomst voor de verwijzende hostnaam. Bijvoorbeeld 'mijndomein.com' geven niet overeenkomen met 'www.mydomain.com'.
- Gebruik de **negeren geval** optie om te bepalen of een hoofdlettergevoelige vergelijking wordt gemaakt.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Force Internal Max-Age
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---  
### <a name="request-header-literal"></a>Aanvraag-Header Literal
De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de aanvraag-Header letterlijke overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: de aanvraag bevat de opgegeven header is vereist. De waarde moet overeenkomen met die gedefinieerd in deze overeenkomst voorwaarde.
- **Komt niet overeen met**: is vereist dat de aanvraag voldoet aan een van de volgende criteria:
  - Het bevat niet de opgegeven header.
  - Het opgegeven header bevat, maar de waarde komt niet overeen met die gedefinieerd in deze overeenkomst voorwaarde.
  
Belangrijke informatie:
- Header-naam vergelijkingen zijn niet hoofdlettergevoelig. Gebruik de **negeren geval** optie om te bepalen van de hoofdlettergevoeligheid vergelijkingen van header-waarde.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Force Internal Max-Age
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---  
### <a name="request-header-regex"></a>Aanvraag-Header Regex
De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de Regex-Header aanvraag overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: de aanvraag bevat de opgegeven header is vereist. De waarde moet overeenkomen met het patroon dat gedefinieerd in de opgegeven [reguliere expressie](cdn-rules-engine-reference.md#regular-expressions).
- **Komt niet overeen met**: is vereist dat de aanvraag voldoet aan een van de volgende criteria:
  - Het bevat niet de opgegeven header.
  - Het opgegeven header bevat, maar de waarde komt niet overeen met de opgegeven reguliere expressie.

Belangrijke informatie:
- Headernaam: 
  - Header-naam vergelijkingen zijn niet hoofdlettergevoelig.
  - Spaties in de header-naam vervangen door "% 20". 
- Headerwaarde: 
  - De waarde van een koptekst kunt profiteren van reguliere expressies.
  - Gebruik de **negeren geval** optie om te bepalen van de hoofdlettergevoeligheid vergelijkingen van header-waarde.
  - De overeenkomst-voorwaarde wordt voldaan, alleen wanneer de waarde van een header exact overeenkomt met ten minste één van de opgegeven patronen.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Force Internal Max-Age
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd 

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="request-header-wildcard"></a>Aanvraag-Header jokertekens
De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder het jokerteken Header aanvraag overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: de aanvraag bevat de opgegeven header is vereist. De waarde moet overeenkomen met ten minste een van de waarden die zijn gedefinieerd in deze overeenkomst voorwaarde.
- **Komt niet overeen met**: is vereist dat de aanvraag voldoet aan een van de volgende criteria:
  - Het bevat niet de opgegeven header.
  - Het opgegeven header bevat, maar de waarde komt niet overeen met een van de opgegeven waarden.
  
Belangrijke informatie:
- Headernaam: 
  - Header-naam vergelijkingen zijn niet hoofdlettergevoelig.
  - Spaties in de header-naam moeten worden vervangen door "% 20". U kunt deze waarde ook gebruiken om op te geven spaties in een headerwaarde.
- Headerwaarde: 
  - Een headerwaarde kan profiteren van [jokertekens waarden](cdn-rules-engine-reference.md#wildcard-values).
  - Gebruik de **negeren geval** optie om te bepalen van de hoofdlettergevoeligheid vergelijkingen van header-waarde.
  - Deze overeenkomst voorwaarde wordt voldaan aan wanneer een headerwaarde exact overeenkomt met ten minste een van de opgegeven patronen.
  - Meerdere waarden opgeven door die begrenst elkaar met een spatie.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Force Internal Max-Age
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="request-method"></a>Verzoekmethode
De voorwaarde van de overeenkomst aanvraagmethode wordt voldaan, alleen wanneer de activa zijn aangevraagd met de geselecteerde aanvraag-methode. De beschikbare aanvraagmethoden zijn:
- TOEVOEGEN
- HEAD 
- POST 
- OPTIES 
- PUT 
- VERWIJDEREN 
- TRACERING 
- CONNECT 

Belangrijke informatie:
- Standaard kan alleen de methode GET-aanvraag in de cache inhoud op het netwerk genereren. Andere aanvraagmethoden zijn via een proxyserver doorgestuurd via het netwerk.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Force Internal Max-Age
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="request-scheme"></a>Aanvraag-schema
De aanvraag schema overeen-voorwaarde wordt voldaan alleen wanneer de activa zijn aangevraagd door het geselecteerde protocol. De beschikbare protocollen zijn: 
- HTTP
- HTTPS

Belangrijke informatie:
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Force Internal Max-Age
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-path-directory"></a>URL-pad naar map
Identificeert een aanvraag door de relatief pad, met uitsluiting van de bestandsnaam van de aangevraagde asset.

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de URL-pad naar map overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: de aanvraag bevat een relatief URL-pad, met uitzondering van de bestandsnaam, die overeenkomt met de opgegeven URL-patroon is vereist.
- **Komt niet overeen met**: de aanvraag bevat een relatief URL-pad, met uitzondering van bestand waarvan de naam, komt niet overeen met de opgegeven URL-patroon is vereist.

Belangrijke informatie:
- Gebruik de **ten opzichte van** optie om op te geven of de vergelijking URL vóór of na de punt van de toegang tot inhoud begint. Het punt toegang tot inhoud is het gedeelte van het pad tussen de hostnaam van het CDN van Verizon en het relatieve pad naar de aangevraagde asset (bijvoorbeeld /800001/CustomerOrigin). Verwijst naar een locatie op servertype (bijvoorbeeld CDN of klant oorsprong) en de nummer van uw klant.

   De volgende waarden zijn beschikbaar voor de **ten opzichte van** optie:
   - **Hoofdmap**: geeft aan dat het punt van de vergelijking URL direct na de hostnaam van het CDN begint. 

     Bijvoorbeeld: http:\//wpc.0001.&lt; domein&gt;/**myorigin-800001/mijnmap**/index.htm

   - **Oorsprong**: geeft aan dat het punt van de vergelijking URL na de punt van de toegang tot inhoud (bijvoorbeeld /000001 of 800001/myorigin begint). Omdat de \*. azureedge.net CNAME is ten opzichte van de oorsprong-map op de hostnaam van het CDN van Verizon standaard gemaakt, Azure CDN gebruikers moeten gebruiken de **oorsprong** waarde. 

     Bijvoorbeeld: https:\//&lt;eindpunt&gt;.azureedge.net/**mijnmap**/index.htm 

     Deze URL verwijst naar de hostnaam van de volgende Verizon CDN: http:\//wpc.0001.&lt; Domein&gt;/800001/myorigin/**mijnmap**/index.htm

- Een edge CNAME-URL is naar de URL van een CDN voorafgaand aan de vergelijking URL herschreven.

    Bijvoorbeeld, beide van de volgende URL's verwijzen naar dezelfde asset en daarom hebben de dezelfde URL-pad.
    - CDN-URL: http:\//wpc.0001.&lt; Domein&gt;/800001/CustomerOrigin/path/asset.htm
    
    - Rand CNAME-URL: http:\//&lt;eindpunt&gt;.azureedge.net/path/asset.htm

    Extra informatie:
    - Aangepast domein: https:\//my.domain.com/path/asset.htm
    
    - URL-pad (ten opzichte van hoofdmap): / 800001/CustomerOrigin/pad /
    
    - URL-pad (ten opzichte van de oorsprong): /path/

- Het gedeelte van de URL die wordt gebruikt voor het einde van de vergelijking URL net vóór de bestandsnaam van de aangevraagde asset. Een afsluitende slash is het laatste teken in dit type pad.
    
- Geen spaties in het patroon URL-pad vervangen door "% 20".
    
- Elke URL-paden kan een of meer sterretjes (*), waarbij elke sterretje overeenkomt met een reeks van een of meer tekens bevatten.
    
- Geef meerdere paden van de URL in het patroon door die begrenst elkaar met een spatie.

    Bijvoorbeeld: * /sales/ * /marketing/

- Het opgegeven pad van een URL kunt profiteren van [jokertekens waarden](cdn-rules-engine-reference.md#wildcard-values).

- Gebruik de **negeren geval** optie om te bepalen of een hoofdlettergevoelige vergelijking wordt uitgevoerd.

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-path-extension"></a>URL-pad-uitbreiding
Identificeert aanvragen door de bestandsextensie van de aangevraagde asset.

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de URL-pad extensie overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: de URL van de aanvraag bevat een bestandsextensie die exact overeenkomt met het opgegeven patroon is vereist.

   Bijvoorbeeld, als u 'htm' opgeeft, wordt 'htm' activa voldaan, maar niet 'html' activa.  

- **Komt niet overeen met**: de URL-aanvraag bevat een bestandsextensie die komt niet overeen met het opgegeven patroon is vereist.

Belangrijke informatie:
- Geef de bestandsextensies zoekt in de **waarde** vak. Neem geen geen voorlooppunt; bijvoorbeeld, htm gebruiken in plaats van .htm.

- Gebruik de **negeren geval** optie om te bepalen of een hoofdlettergevoelige vergelijking wordt uitgevoerd.

- Geef meerdere bestandsextensies door die begrenst elke uitbreiding met een spatie. 

    Bijvoorbeeld: htm html

- Bijvoorbeeld 'htm' geven komt overeen met 'htm' activa, maar niet 'html' activa.


#### <a name="sample-scenario"></a>Voorbeeldscenario

De voorbeeldconfiguratie van de volgende wordt ervan uitgegaan dat deze overeenkomst aan voorwaarde wordt voldaan wanneer een aanvraag komt overeen met een van de opgegeven extensies.

Waarde van de specificatie: asp aspx-php html

Deze overeenkomst-voorwaarde is voldaan wanneer URL's die op de volgende extensies eindigen:
- .asp
- .aspx
- .php
- .html

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-path-filename"></a>URL-pad bestandsnaam
Identificeert aanvragen door de naam van de aangevraagde asset. Voor het doel van deze overeenkomst voorwaarde bestaat een bestandsnaam op van de naam van de aangevraagde asset, een periode en de bestandsextensie (bijvoorbeeld index.html).

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de URL-pad bestandsnaam overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: de aanvraag bevat een bestandsnaam in de URL-pad dat overeenkomt met het opgegeven patroon is vereist.
- **Komt niet overeen met**: de aanvraag bevat een bestandsnaam in de URL-pad komt niet overeen met het opgegeven patroon is vereist.

Belangrijke informatie:
- Gebruik de **negeren geval** optie om te bepalen of een hoofdlettergevoelige vergelijking wordt uitgevoerd.

- Als u meerdere bestandsextensies, scheidt u elke uitbreiding met een spatie.

    Bijvoorbeeld: index.htm index.html

- Spaties in de waarde voor een bestandsnaam vervangen door "% 20".
    
- Waarde voor een bestandsnaam kunt profiteren van [jokertekens waarden](cdn-rules-engine-reference.md#wildcard-values). Bijvoorbeeld, kan elke gewenste bestandsnaampatroon bestaan uit een of meer sterretjes (*), waarbij elke sterretje overeenkomt met een reeks van een of meer tekens.
    
- Als jokertekens niet opgegeven zijn, wordt alleen een exacte overeenkomst deze overeenkomst voorwaarde voldoen.

    Bijvoorbeeld 'presentation.ppt' geven overeenkomt met een asset met de naam 'presentation.ppt', maar niet één met de naam 'presentatie.pptx."

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-path-literal"></a>URL-pad Literal
Vergelijkt een aanvraag URL-pad, inclusief de bestandsnaam met de opgegeven waarde.

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de URL-pad letterlijke overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: de aanvraag bevat een URL-pad dat overeenkomt met het opgegeven patroon is vereist.
- **Komt niet overeen met**: de aanvraag bevat een URL-pad komt niet overeen met het opgegeven patroon is vereist.

Belangrijke informatie:
- Gebruik de **ten opzichte van** optie om op te geven of het punt van de vergelijking URL vóór of na de punt van de toegang tot inhoud begint. 

    De volgende waarden zijn beschikbaar voor de **ten opzichte van** optie:
     - **Hoofdmap**: geeft aan dat het punt van de vergelijking URL direct na de hostnaam van het CDN begint.

       Bijvoorbeeld: http:\//wpc.0001.&lt; Domein&gt;/**800001/myorigin/myfolder/index.htm**

     - **Oorsprong**: geeft aan dat het punt van de vergelijking URL na de punt van de toegang tot inhoud (bijvoorbeeld /000001 of 800001/myorigin begint). Omdat de \*. azureedge.net CNAME is ten opzichte van de oorsprong-map op de hostnaam van het CDN van Verizon standaard gemaakt, Azure CDN gebruikers moeten gebruiken de **oorsprong** waarde. 

       Bijvoorbeeld: https:\//&lt;eindpunt&gt;.azureedge.net/**myfolder/index.htm**

     Deze URL verwijst naar de hostnaam van de volgende Verizon CDN: http:\//wpc.0001.&lt; Domein&gt;/800001/myorigin/**myfolder/index.htm**

- Een CNAME-URL-rand wordt herschreven naar een CDN-URL voor een vergelijking van de URL.

   Bijvoorbeeld, beide van de volgende URL's verwijzen naar dezelfde asset en daarom hebben de dezelfde URL-pad:
    - CDN-URL: http:\//wpc.0001.&lt; Domein&gt;/800001/CustomerOrigin/path/asset.htm
    - Rand CNAME-URL: http:\//&lt;eindpunt&gt;.azureedge.net/path/asset.htm

   Extra informatie:
    
    - URL-pad (ten opzichte van hoofdmap): /800001/CustomerOrigin/path/asset.htm
   
    - URL-pad (ten opzichte van de oorsprong): /path/asset.htm

- Queryreeksen in de URL worden genegeerd.
- Gebruik de **negeren geval** optie om te bepalen of een hoofdlettergevoelige vergelijking wordt uitgevoerd.
- De opgegeven waarde voor deze voorwaarde overeenkomst zal worden vergeleken met het relatieve pad van de exacte verzoek van de client.

- Zodat deze overeenkomen met alle aanvragen voor een bepaalde map, gebruiken de [URL-pad naar map](#url-path-directory) of de [URL pad jokerteken](#url-path-wildcard) overeenkomen met de voorwaarde.

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-path-regex"></a>URL-pad Regex
Een aanvraag-URL-pad naar het opgegeven vergelijkt [reguliere expressie](cdn-rules-engine-reference.md#regular-expressions).

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de URL-pad Regex overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: de aanvraag bevat een URL-pad dat overeenkomt met de opgegeven reguliere expressie is vereist.
- **Komt niet overeen met**: de aanvraag bevat een URL-pad komt niet overeen met de opgegeven reguliere expressie is vereist.

Belangrijke informatie:
- Een CNAME-URL-rand wordt herschreven naar de URL van een CDN voordat URL vergelijking. 
 
   Bijvoorbeeld: beide URL's verwijzen naar dezelfde asset en daarom hebben de dezelfde URL-pad.

     - CDN-URL: http:\//wpc.0001.&lt; Domein&gt;/800001/CustomerOrigin/path/asset.htm

     - Rand CNAME-URL: http:\//my.domain.com/path/asset.htm

   Extra informatie:
    
     - URL-pad: /800001/CustomerOrigin/path/asset.htm

- Queryreeksen in de URL worden genegeerd.
    
- Gebruik de **negeren geval** optie om te bepalen of een hoofdlettergevoelige vergelijking wordt uitgevoerd.
    
- Spaties in de URL-pad moeten worden vervangen door "% 20".

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-path-wildcard"></a>URL-pad jokerteken
Vergelijkt relatief URL-pad van een aanvraag met het patroon opgegeven jokerteken.

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder het URL-pad jokerteken overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: de aanvraag bevat een URL-pad dat overeenkomt met het jokerteken opgegeven patroon is vereist.
- **Komt niet overeen met**: de aanvraag bevat een URL-pad komt niet overeen met het jokerteken opgegeven patroon is vereist.

Belangrijke informatie:
- **Ten opzichte van** optie: deze optie wordt bepaald of het punt van de vergelijking URL vóór of na de punt van de toegang tot inhoud begint.

   Deze optie kan de volgende waarden hebben:
     - **Hoofdmap**: geeft aan dat het punt van de vergelijking URL direct na de hostnaam van het CDN begint.

       Bijvoorbeeld: http:\//wpc.0001.&lt; Domein&gt;/**800001/myorigin/myfolder/index.htm**

     - **Oorsprong**: geeft aan dat het punt van de vergelijking URL na de punt van de toegang tot inhoud (bijvoorbeeld /000001 of 800001/myorigin begint). Omdat de \*. azureedge.net CNAME is ten opzichte van de oorsprong-map op de hostnaam van het CDN van Verizon standaard gemaakt, Azure CDN gebruikers moeten gebruiken de **oorsprong** waarde. 

       Bijvoorbeeld: https:\//&lt;eindpunt&gt;.azureedge.net/**myfolder/index.htm**

     Deze URL verwijst naar de hostnaam van de volgende Verizon CDN: http:\//wpc.0001.&lt; Domein&gt;/800001/myorigin/**myfolder/index.htm**

- Een CNAME-URL-rand wordt herschreven naar de URL van een CDN voordat URL vergelijking.

   Bijvoorbeeld, beide van de volgende URL's verwijzen naar dezelfde asset en daarom hebben de dezelfde URL-pad:
     - CDN-URL: http://wpc.0001. &lt;Domein&gt;/800001/CustomerOrigin/path/asset.htm
     - Rand CNAME-URL: http:\//&lt;eindpunt&gt;.azureedge.net/path/asset.htm

   Extra informatie:
    
     - URL-pad (ten opzichte van hoofdmap): /800001/CustomerOrigin/path/asset.htm
    
     - URL-pad (ten opzichte van de oorsprong): /path/asset.htm
    
- Meerdere URL-paden opgeven door die begrenst elkaar met een spatie.

   Bijvoorbeeld: /marketing/asset.* /sales/*.htm

- Queryreeksen in de URL worden genegeerd.
    
- Gebruik de **negeren geval** optie om te bepalen of een hoofdlettergevoelige vergelijking wordt uitgevoerd.
    
- Spaties in de URL-pad vervangen door "% 20".
    
- De opgegeven waarde voor een URL-pad van profiteren kan [jokertekens waarden](cdn-rules-engine-reference.md#wildcard-values). Elke URL-paden kan een of meer sterretjes (*), waarbij elke sterretje kan overeenkomen met een reeks van een of meer tekens bevatten.

#### <a name="sample-scenarios"></a>Voorbeeldscenario 's

De voorbeeld-configuraties in de volgende tabel wordt ervan uitgegaan dat deze overeenkomst aan voorwaarde wordt voldaan wanneer een aanvraag komt overeen met de opgegeven URL-patroon:

Waarde                   | Relatieve    | Resultaat 
------------------------|----------------|-------
*/Test.HTML */test.php  | Basis- of oorsprong | Dit patroon is overeenkomt met de aanvragen voor bedrijfsmiddelen die met de naam 'test.html' of 'test.php' in een andere map.
/80ABCD/origin/text/*   | Hoofdmap           | Dit patroon komt overeen wanneer de aangevraagde asset voldoet aan de volgende criteria: <br />-Deze moet zich bevinden op een klant-oorsprong aangeroepen 'origin'. <br />-Het relatieve pad moet beginnen met een map met de naam 'text'. Dat wil zeggen, dat de aangevraagde asset ofwel kan zich in de map 'text' of een van de recursieve submappen ervan.
*/CSS/* *padinfo /js/*          | Basis- of oorsprong | Dit patroon komt overeen met alle CDN of edge CNAME-URL's die een map CSS- of js bevatten.
*.jpg *.gif *.png       | Basis- of oorsprong | Dit patroon komt overeen met alle CDN of edge CNAME-URL's die eindigen met .jpg, .gif of PNG. Een andere manier om op te geven van dit patroon is met de [URL-pad extensie overeenkomen met de voorwaarde](#url-path-extension).
installatiekopieën / * / media / *      | Oorsprong         | Dit patroon is overeenkomt met de CDN of edge CNAME-URL's waarvan het relatieve pad met een map 'afbeeldingen' of 'media begint'. <br />-CDN URL: http:\//wpc.0001.&lt; Domein&gt;/800001/myorigin/images/sales/event1.png<br />- Sample edge CNAME URL: http:\//cdn.mydomain.com/images/sales/event1.png

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-query-literal"></a>URL-Query Literal
Vergelijkt de queryreeks van een aanvraag met de opgegeven waarde.

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de URL-Query letterlijke overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: de aanvraag bevat een URL-query-tekenreeks die overeenkomt met de opgegeven queryreeks vereist.
- **Komt niet overeen met**: de aanvraag bevat een URL-queryreeks die komt niet overeen met de opgegeven queryreeks vereist.

Belangrijke informatie:

- Alleen exacte query tekenreeks komt overeen met voldoen aan de voorwaarde van deze overeenkomst.
    
- Gebruik de **negeren geval** optie om te bepalen van de hoofdlettergevoeligheid van tekenreeksvergelijkingen query.
    
- Een begin-vraagteken (?) niet opnemen in de tekst van de query-tekenreeks.
    
- Bepaalde tekens vereisen URL-codering. Gebruik het percentagesymbool naar URL coderen van de volgende tekens bevatten:

   Teken | URL-codering
   ----------|---------
   Spatiebalk     | %20
   &         | %25

- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
   - Voltooien van de opvulling van de Cache
   - Standaard interne-maximumleeftijd
   - Force Internal Max-Age
   - Negeren oorsprong No-Cache
   - Interne Max-verouderd

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-query-parameter"></a>URL-queryparameter
Geeft aan dat aanvragen met de opgegeven tekenreeks van de queryparameter. Deze parameter is ingesteld op een waarde die overeenkomt met een opgegeven patroon. Tekenreeks queryparameters (bijvoorbeeld parameter = waarde) in het aanvraag-URL bepalen of deze voorwaarde is voldaan. Deze overeenkomst voorwaarde identificeert een queryreeksparameter opgeven met de naam en een of meer waarden voor de waarde van parameter accepteert. 

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de URL-queryparameter overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: vereist een aanvraag voor de opgegeven parameter met een waarde die overeenkomt met ten minste een van de waarden die zijn gedefinieerd in deze overeenkomst voorwaarde bevatten.
- **Komt niet overeen met**: is vereist dat de aanvraag voldoet aan een van de volgende criteria:
  - Het bevat niet de opgegeven parameter.
  - Deze bevat de opgegeven parameter, maar de waarde komt niet overeen met een van de waarden die zijn gedefinieerd in deze overeenkomst voorwaarde.

Deze voorwaarde overeen biedt een eenvoudige manier om op te geven parametercombinaties naam/waarde. Voor meer flexibiliteit als u een queryreeksparameter overeenkomende, kunt u overwegen de [URL Query jokertekens](#url-query-wildcard) overeenkomen met de voorwaarde.

Belangrijke informatie:
- Per exemplaar van deze overeenkomst voorwaarde kan alleen een enkele URL-query parameternaam worden opgegeven.
    
- Omdat jokertekens waarden worden niet ondersteund wanneer een parameternaam is opgegeven, zijn alleen exacte parameter naam komt overeen met in aanmerking komen voor vergelijking.
- Parameterwaarde(n) kunt opnemen [jokertekens waarden](cdn-rules-engine-reference.md#wildcard-values).
   - Elke parameter waarde patroon kan bestaan uit een of meer sterretjes (*), waarbij elke sterretje kan overeenkomen met een reeks van een of meer tekens.
   - Bepaalde tekens vereisen URL-codering. Gebruik het percentagesymbool naar URL coderen van de volgende tekens bevatten:

       Teken | URL-codering
       ----------|---------
       Spatiebalk     | %20
       &         | %25

- Geef meer parameterwaarden voor query-tekenreeks door die begrenst elkaar met een spatie. Deze overeenkomst voorwaarde wordt voldaan aan wanneer een aanvraag een van de opgegeven naam/waarde-combinaties bevat.

   - Voorbeeld 1:

     - Configuratie:

       ValueA ValueB

     - Deze configuratie komt overeen met de volgende parameters voor query-tekenreeks:

       Parameter1=ValueA
    
       Parameter1=ValueB

   - Voorbeeld 2:

     - Configuratie: 

        Value%20A Value%20B

     - Deze configuratie komt overeen met de volgende parameters voor query-tekenreeks:

       Parameter1=Value%20A

       Parameter1=Value%20B

- Deze overeenkomst-voorwaarde is voldaan, alleen wanneer er een exacte overeenkomst voor ten minste één van de opgegeven query-tekenreeks naam/waarde-combinaties.

   Bijvoorbeeld, als u de configuratie in het vorige voorbeeld, de parameter naam/waarde combinatie ' Parameter1 ValueAdd = ' kan niet worden gezien als een overeenkomst. Echter, als u een van de volgende waarden opgeven, deze overeenkomt met die naam/waarde-combinatie:

   - ValueA ValueB ValueAdd
   - ValueA* ValueB

- Gebruik de **negeren geval** optie om te bepalen van de hoofdlettergevoeligheid van tekenreeksvergelijkingen query.
    
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
   - Voltooien van de opvulling van de Cache
   - Standaard interne-maximumleeftijd
   - Force Internal Max-Age
   - Negeren oorsprong No-Cache
   - Interne Max-verouderd

#### <a name="sample-scenarios"></a>Voorbeeldscenario's
Het volgende voorbeeld laat zien hoe deze optie werkt in bepaalde situaties:

Naam      | Waarde |  Resultaat
----------|-------|--------
Gebruiker      | Jan   | Dit patroon komt overeen wanneer de query-tekenreeks voor een aangevraagde URL is '? gebruiker Jan =. "
Gebruiker      | *     | Dit patroon komt overeen wanneer de query-tekenreeks voor een aangevraagde URL een parameter van de gebruiker bevat.
E-Jan | *     | Dit patroon komt overeen wanneer de query-tekenreeks voor een aangevraagde URL bevat een e-parameter die begint met "Jan".

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-query-regex"></a>URL-Query Regex
Geeft aan dat aanvragen met de opgegeven tekenreeks van de queryparameter. Deze parameter is ingesteld op een waarde die overeenkomt met een opgegeven [reguliere expressie](cdn-rules-engine-reference.md#regular-expressions).

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de URL-Query Regex overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: de aanvraag bevat een URL-query-tekenreeks die overeenkomt met de opgegeven reguliere expressie is vereist.
- **Komt niet overeen met**: de aanvraag bevat een URL-queryreeks die komt niet overeen met de opgegeven reguliere expressie is vereist.

Belangrijke informatie:
- Alleen is exact overeenkomen met de opgegeven reguliere expressie voldoen aan de voorwaarde van deze overeenkomst.
    
- Gebruik de **negeren geval** optie om te bepalen van de hoofdlettergevoeligheid van tekenreeksvergelijkingen query.
    
- Voor de toepassing van deze optie wordt begint een query-tekenreeks met het eerste teken na het scheidingsteken vraagteken (?) voor de queryreeks.
    
- Bepaalde tekens vereisen URL-codering. Gebruik het percentagesymbool naar URL coderen van de volgende tekens bevatten:

   Teken | URL-codering | Waarde
   ----------|--------------|------
   Spatiebalk     | %20          | \%20
   &         | %25          | \%25

   Houd er rekening mee dat percentage symbolen moeten worden voorafgegaan.

- Speciale reguliere expressie dubbele escape-tekens (bijvoorbeeld \^$. +) om op te nemen van een backslash in de reguliere expressie.

   Bijvoorbeeld:

   Waarde | Geïnterpreteerd als 
   ------|---------------
   \\+    | +
   \\\+   | \\+

- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
   - Voltooien van de opvulling van de Cache
   - Standaard interne-maximumleeftijd
   - Force Internal Max-Age
   - Negeren oorsprong No-Cache
   - Interne Max-verouderd


[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-query-wildcard"></a>URL-Query jokertekens
De opgegeven waarden worden ingesteld op basis van de queryreeks van de aanvraag wordt vergeleken.

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder het jokerteken van de Query-URL overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: vereist dat de aanvraag voor een URL-query-tekenreeks die overeenkomt met de waarde van de opgegeven jokertekens bevatten.
- **Komt niet overeen met**: de aanvraag bevat een URL-queryreeks die komt niet overeen met het opgegeven jokerteken is vereist.

Belangrijke informatie:
- Voor de toepassing van deze optie wordt begint een query-tekenreeks met het eerste teken na het scheidingsteken vraagteken (?) voor de queryreeks.
- Parameterwaarden kunnen opnemen [jokertekens waarden](cdn-rules-engine-reference.md#wildcard-values):
   - Elke parameter waarde patroon kan bestaan uit een of meer sterretjes (*), waarbij elke sterretje kan overeenkomen met een reeks van een of meer tekens.
   - Bepaalde tekens vereisen URL-codering. Gebruik het percentagesymbool naar URL coderen van de volgende tekens bevatten:

     Teken | URL-codering
     ----------|---------
     Spatiebalk     | %20
     &         | %25

- Meerdere waarden opgeven door die begrenst elkaar met een spatie.

   Bijvoorbeeld: *Parameter1 = ValueA* *ValueB* *Parameter1 = ValueC & Parameter2 ValueD =*

- Alleen deze overeenkomst voorwaarde voldoen aan de exacte overeenkomsten met ten minste één van de opgegeven tekenreeks querypatronen.
    
- Gebruik de **negeren geval** optie om te bepalen van de hoofdlettergevoeligheid van tekenreeksvergelijkingen query.
    
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
   - Voltooien van de opvulling van de Cache
   - Standaard interne-maximumleeftijd
   - Force Internal Max-Age
   - Negeren oorsprong No-Cache
   - Interne Max-verouderd

#### <a name="sample-scenarios"></a>Voorbeeldscenario's
Het volgende voorbeeld laat zien hoe deze optie werkt in bepaalde situaties:

 Naam                 | Beschrijving
 ---------------------|------------
user=joe              | Dit patroon komt overeen wanneer de query-tekenreeks voor een aangevraagde URL is '? gebruiker Jan =. "
\*user=\* \*optout=\* | Dit patroon komt overeen wanneer de CDN-URL-query de gebruiker of een optout-parameter bevat.

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van Azure Content Delivery Network](cdn-overview.md)
* [Regels engine verwijzing](cdn-rules-engine-reference.md)
* [Regels engine voorwaardelijke expressies](cdn-rules-engine-reference-conditional-expressions.md)
* [Regels engine functies](cdn-rules-engine-reference-features.md)
* [Standaardgedrag HTTP met de regelengine van](cdn-rules-engine.md)


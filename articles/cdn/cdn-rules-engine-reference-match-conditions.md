---
title: Voldoen aan bepaalde voorwaarden voor de engine van Azure CDN regels | Microsoft Docs
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
ms.openlocfilehash: 9986e654b076df099e3912f9da628728723b5c3d
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="match-conditions-for-the-azure-cdn-rules-engine"></a>Voldoen aan bepaalde voorwaarden voor de regels-engine van Azure CDN
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
[Cookie-Parameter](#cookie-parameter) | Controleert de cookies die zijn gekoppeld aan elke aanvraag naar de opgegeven waarde.
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
URL-pad naar map | Identificeert aanvragen door hun relatief pad.
URL-pad-uitbreiding | Identificeert aanvragen door de bestandsnaamextensie.
URL-pad bestandsnaam | Identificeert aanvragen door hun naam.
URL-pad Literal | Vergelijkt de relatieve pad van een aanvraag met de opgegeven waarde.
URL-pad Regex | Vergelijkt relatieve pad van een aanvraag naar de opgegeven reguliere expressie.
URL-pad jokerteken | Vergelijkt relatieve pad van een aanvraag naar het opgegeven patroon.
URL-Query Literal | Vergelijkt de queryreeks van een aanvraag met de opgegeven waarde.
URL-queryparameter | Geeft aan dat aanvragen met de opgegeven tekenreeks queryparameter ingesteld op een waarde die overeenkomt met een opgegeven patroon.
URL-Query Regex | Geeft aan dat aanvragen met de opgegeven tekenreeks queryparameter ingesteld op een waarde die overeenkomt met een opgegeven reguliere expressie.
URL-Query jokertekens | De opgegeven waarde op basis van de queryreeks van de aanvraag wordt vergeleken.


## <a name="reference-for-rules-engine-match-conditions"></a>Verwijzing voor de overeenkomst motor regels

---
### <a name="always"></a>Altijd

De voorwaarde van de overeenkomst altijd geldt een aantal functies voor alle aanvragen.

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="as-number"></a>AS-nummer 
Het AS-nummer-netwerk wordt gedefinieerd door de autonoom systeemnummer (ASN). Een optie is om aan te geven of deze voorwaarde wordt voldaan aan wanneer een client netwerk 'Komt overeen met' of 'Komt niet overeen met' aan het opgegeven ASN opgegeven.

Belangrijke informatie:
- Geef meerdere ASN's door die begrenst elkaar met een spatie. 64514 64515 komt bijvoorbeeld overeen met aanvragen die van de 64514 of 64515 binnenkomen.
- Bepaalde aanvragen mogelijk geen geldige ASN niet geretourneerd. Een vraagteken (?) komt overeen met aanvragen waarvoor een geldige ASN kan niet worden bepaald.
- U moet de gehele ASN worden gebruikt voor het gewenste netwerk opgeven. Ondersteuning voor gedeeltelijke waarden wordt niet overeen.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="cdn-origin"></a>CDN-oorsprong
De CDN oorsprong overeen-voorwaarde wordt voldaan wanneer beide volgende voorwaarden wordt voldaan:
- Inhoud uit de Content Delivery Network-opslag is aangevraagd.
- De aanvraag-URI gebruikt het punt toegang tot inhoud (bijvoorbeeld /000001) die gedefinieerd in deze overeenkomst voorwaarde.
  - Inhoud levering netwerk URL: De aanvraag-URI moet het toegangspunt voor het geselecteerde inhoud bevatten.
  - Rand CNAME-URL: De configuratie van de bijbehorende rand CNAME moet verwijzen naar de geselecteerde inhoud access point.
  
Belangrijke informatie:
 - Het toegangspunt inhoud de service identificeert die de gevraagde inhoud fungeren moet.
 - Gebruik een instructie en als niet aan bepaalde voorwaarden overeen combineren. Een voorwaarde CDN oorsprong overeen met de voorwaarde van een klant oorsprong overeen combineren zou bijvoorbeeld een patroon overeen die nooit overeen maken. Om deze reden kunnen niet twee CDN oorsprong overeenkomst voorwaarden via een instructie en als worden gecombineerd.

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="client-ip-address"></a>IP-adres van client
Een optie is om aan te geven of de IP-adres Client voorwaarde worden voldaan wanneer een client-IP-adres 'Komt overeen met' of 'Komt niet overeen met' de opgegeven IP-adressen opgegeven.

Belangrijke informatie:
- Zorg ervoor dat CIDR-notatie wordt gebruikt.
- Geef meerdere IP-adressen en/of IP-adresblokken door die begrenst elkaar met een spatie.
  - **IPv4-voorbeeld**: 1.2.3.4 10.20.30.40 komt overeen met alle aanvragen die van de 1.2.3.4 of 10.20.30.40 binnenkomen.
  - **IPv6-voorbeeld**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 komt overeen met alle aanvragen die van de 1:2:3:4:5:6:7:8 of 10:20:30:40:50:60:70:80 binnenkomen.
- De syntaxis voor een IP-Adresblok is het basisadres van de IP-gevolgd door een slash en de grootte van het voorvoegsel.
  - **IPv4-voorbeeld**: 5.5.5.64/26 komt overeen met alle aanvragen die van de 5.5.5.64 via 5.5.5.127 binnenkomen.
  - **IPv6-voorbeeld**: 1:2:3: / 48 komt overeen met alle aanvragen die van de 1:2:3:0:0:0:0:0 via 1:2:3:ffff:ffff:ffff:ffff:ffff binnenkomen.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="cookie-parameter"></a>Cookie-Parameter
De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de Cookie-Parameter overeenkomen met de voorwaarde wordt voldaan.
- **Komt overeen met**: vereist een aanvraag voor de opgegeven cookie met een waarde die overeenkomt met ten minste een van de waarden die zijn gedefinieerd in deze overeenkomst voorwaarde bevatten.
- **Komt niet overeen met**: is vereist dat de aanvraag voldoen aan een van de volgende criteria:
  - Het bevat niet de opgegeven cookie.
  - De opgegeven cookie bevat, maar de waarde komt niet overeen met een van de waarden die zijn gedefinieerd in deze overeenkomst voorwaarde.
  
Belangrijke informatie:
- Cookienaam: 
  - Speciale tekens, inclusief een sterretje worden niet ondersteund wanneer u een Cookienaam opgeven. Dit betekent dat alleen exacte cookie naam komt overeen met in aanmerking voor de vergelijking komen.
  - Alleen de naam van een enkele cookie kan per exemplaar van deze overeenkomst voorwaarde worden opgegeven.
  - Cookie naam vergelijkingen zijn niet hoofdlettergevoelig.
- Cookiewaarde: 
  - Meerdere cookiewaarden opgeven door die begrenst elkaar met een spatie.
  - De waarde van een cookie kunt profiteren van speciale tekens. 
  - Als een jokerteken niet opgegeven is, wordt alleen een exacte overeenkomst deze overeenkomst voorwaarde voldoen. Bijvoorbeeld ' waarde ' komt overeen met '' waarde '', maar niet 'Value1' of 'Waarde2'.
  - De **negeren geval** optie wordt bepaald of hoofdletters worden uitgevoerd voor de waarde van de aanvraag-cookie.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="cookie-parameter-regex"></a>Cookie Parameter Regex
De voorwaarde van de overeenkomst Cookie Parameter Regex definieert een Cookienaam en waarde. Reguliere expressies kunt u de gewenste cookiewaarde definiëren. 

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden onder waarvoor deze overeenkomen met de voorwaarde voldaan.
- **Komt overeen met**: vereist een aanvraag voor de opgegeven cookie met een waarde die overeenkomt met de opgegeven reguliere expressie bevatten.
- **Komt niet overeen met**: is vereist dat de aanvraag voldoen aan een van de volgende criteria:
  - Het bevat niet de opgegeven cookie.
  - De opgegeven cookie bevat, maar de waarde komt niet overeen met de opgegeven reguliere expressie.
  
Belangrijke informatie:
- Cookienaam: 
  - Reguliere expressies en speciale tekens, inclusief een sterretje worden niet ondersteund wanneer u een Cookienaam opgeven. Dit betekent dat alleen exacte cookie naam komt overeen met in aanmerking voor de vergelijking komen.
  - Alleen de naam van een enkele cookie kan per exemplaar van deze overeenkomst voorwaarde worden opgegeven.
  - Cookie naam vergelijkingen zijn niet hoofdlettergevoelig.
- Cookiewaarde: 
  - De waarde van een cookie kunt profiteren van reguliere expressies.
  - De **negeren geval** optie wordt bepaald of hoofdletters worden uitgevoerd voor de waarde van de aanvraag-cookie.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

--- 
### <a name="country"></a>Land
U kunt een land via de landcode opgeven. Krijgt u een optie om aan te geven of deze voorwaarde wordt voldaan wanneer het land van waaruit een aanvraag afkomstig 'Komt overeen met' of 'Komt niet overeen met' de opgegeven waarden is.

Belangrijke informatie:
- Geef meerdere landcodes door die begrenst elkaar met een spatie.
- Jokertekens worden niet ondersteund wanneer u bij het opgeven van een landcode.
- De landcodes 'EU' en 'AP' kunnen niet alle IP-adressen in die gebieden omvatten.
- Bepaalde aanvragen mogelijk een geldige landcode niet geretourneerd. Een vraagteken (?) komt overeen met aanvragen waarvoor een geldige landcode kan niet worden bepaald.
- Landcodes zijn hoofdlettergevoelig.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="customer-origin"></a>Oorsprong van de klant

Belangrijke informatie: 
- Ongeacht of de inhoud wordt aangevraagd via een netwerk-URL van de inhoud-levering of een CNAME-URL die naar de oorsprong van de geselecteerde klant verwijst-rand wordt aan de voorwaarde van de overeenkomst oorsprong van de klant worden voldaan.
- De configuratie van een klant-oorsprong waarnaar wordt verwezen door een regel kan niet worden verwijderd uit de pagina van de oorsprong van de klant. Voordat u probeert te verwijderen van de configuratie van een klant oorsprong, controleert u dat de volgende configuraties niet verwijzen naar deze:
  - Een klant oorsprong overeen voorwaarde
  - Een edge CNAME-configuratie
- Gebruik een instructie en als niet aan bepaalde voorwaarden overeen combineren. Een voorwaarde klant oorsprong overeen met de voorwaarde van een oorsprong CDN overeen combineren zou bijvoorbeeld een patroon overeen die nooit overeen maken. Om deze reden kunnen niet via een instructie en als worden gecombineerd met twee voorwaarden van de oorsprong van de klant overeen.

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="device"></a>Apparaat

De voorwaarde van de overeenkomst apparaat identificeert aanvragen van een mobiel apparaat op basis van de eigenschappen ervan. Detectie van mobiele apparaten wordt bereikt door [WURFL](http://wurfl.sourceforge.net/). De volgende tabel bevat WURFL mogelijkheden en hun variabelen voor de engine voor het Content Delivery Network regels.
<br>
> [!NOTE] 
> De volgende variabelen worden ondersteund in de **Client aanvraag-Header wijzigen** en **Client antwoordheader wijzigen** functies.

Mogelijkheid | Variabele | Beschrijving | Voorbeeldwaarden
-----------|----------|-------------|----------------
De naam van het merk | % {wurfl_cap_brand_name} | Een tekenreeks die de naam van het merk van het apparaat geeft. | Samsung
Besturingssysteem van het apparaat | % {wurfl_cap_device_os} | Een tekenreeks die aangeeft van het besturingssysteem op het apparaat geïnstalleerd. | IOS
Versie van apparaatbesturingssysteem | % {wurfl_cap_device_os_version} | Een tekenreeks die het versienummer van het besturingssysteem op het apparaat geïnstalleerd geeft. | 1.0.1
Dual afdrukstand | % {wurfl_cap_dual_orientation} | Een Booleaanse waarde die aangeeft of het apparaat dual afdrukstand ondersteunt. | waar
HTML voorkeur DTD | % {wurfl_cap_html_preferred_dtd} | Een tekenreeks die typedefinitie (DTD) voor HTML-inhoud van het mobiele apparaat voorkeur document aangeeft. | geen<br/>xhtml_basic<br/>HTML5
Afbeelding Inlining | % {wurfl_cap_image_inlining} | Een Boolean die aangeeft of het apparaat ondersteunt met Base64 gecodeerd installatiekopieën. | onwaar
Android is | % {wurfl_vcap_is_android} | Een Booleaanse waarde die aangeeft of het apparaat Android OS gebruikt. | waar
IOS is | % {wurfl_vcap_is_ios} | Een Booleaanse waarde die aangeeft of het apparaat iOS gebruikt. | onwaar
Smart TV is | % {wurfl_cap_is_smarttv} | Een Booleaanse waarde die aangeeft of het apparaat een intelligente TV is. | onwaar
Smartphone is | % {wurfl_vcap_is_smartphone} | Een Booleaanse waarde die aangeeft of het apparaat een smartphone is. | waar
Is Tablet | % {wurfl_cap_is_tablet} | Een Booleaanse waarde die aangeeft of het apparaat een tablet is. Dit is een beschrijving onafhankelijk van het besturingssysteem. | waar
Draadloze apparaat | % {wurfl_cap_is_wireless_device} | Een Booleaanse waarde die aangeeft of het apparaat wordt beschouwd als een draadloos apparaat. | waar
De naam van marketing | % {wurfl_cap_marketing_name} | Een tekenreeks die marketing naam van het apparaat aangeeft. | BlackBerry 8100 Pearl
Mobiele Browser | % {wurfl_cap_mobile_browser} | Een tekenreeks die aangeeft van de browser die wordt gebruikt voor het aanvragen van inhoud van het apparaat. | Chrome
Mobiele browserversie | % {wurfl_cap_mobile_browser_version} | Een tekenreeks die de versie van de browser die wordt gebruikt voor het aanvragen van inhoud van het apparaat aangeeft. | 31
Modelnaam | % {wurfl_cap_model_name} | Een tekenreeks die modelnaam van het apparaat aangeeft. | S3
Progressief downloaden | % {wurfl_cap_progressive_download} | Een Booleaanse waarde die aangeeft of het apparaat ondersteuning biedt voor het afspelen van audio en video terwijl het nog steeds wordt gedownload. | waar
Releasedatum | % {wurfl_cap_release_date} | Een tekenreeks die het jaar en maand op waarop het apparaat is toegevoegd aan de database WURFL aangeeft.<br/><br/>Indeling:`yyyy_mm` | 2013_december
Hoogte van de oplossing | % {wurfl_cap_resolution_height} | Een geheel getal dat van het apparaat hoogte in pixels aangeeft. | 768
Breedte van de oplossing | % {wurfl_cap_resolution_width} | Een geheel getal dat aangeeft het apparaat de breedte in pixels dat. | 1024

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="edge-cname"></a>Rand Cname
Belangrijke informatie: 
- De lijst met beschikbare rand CNAME-records is beperkt tot apparaten die zijn geconfigureerd op de pagina rand CNAME-records die overeenkomt met het platform waarop HTTP regels-Engine wordt geconfigureerd.
- Voordat u probeert te verwijderen van een CNAME-rand configuratie, ervoor zorgen dat een overeenkomst rand Cname voorwaarde verwijst niet naar het. Rand CNAME-configuraties die zijn gedefinieerd in een regel kunnen niet worden verwijderd uit de pagina rand CNAME-records. 
- Gebruik een instructie en als niet aan bepaalde voorwaarden overeen combineren. Een voorwaarde rand Cname overeen met de voorwaarde van een klant oorsprong overeen combineren zou bijvoorbeeld een patroon overeen die nooit overeen maken. Om deze reden kunnen niet via een instructie en als worden gecombineerd met twee voorwaarden van de rand Cname overeen.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="referring-domain"></a>Verwijzende domein
De hostnaam die is gekoppeld aan de verwijzende site waarmee inhoud is aangevraagd, wordt bepaald of het domein verwijst-voorwaarde is voldaan. Een optie wordt om aan te geven of deze voorwaarde wordt voldaan aan wanneer de verwijzende host-naam 'Komt overeen met' of 'Komt niet overeen met' de opgegeven waarden verstrekt.

Belangrijke informatie:
- Geef meerdere hostnamen door die begrenst elkaar met een spatie.
- Deze voorwaarde overeen ondersteunt speciale tekens.
- Als de opgegeven waarde geen een sterretje bevat, moet een exacte overeenkomst voor de verwijzende hostnaam. Bijvoorbeeld 'mijndomein.com' geven niet overeenkomen met 'www.mydomain.com'.
- De **negeren geval** optie wordt bepaald of hoofdletters worden uitgevoerd.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---  
### <a name="request-header-literal"></a>Aanvraag-Header Literal
De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden onder waarvoor deze overeenkomen met de voorwaarde voldaan.
- **Komt overeen met**: de aanvraag bevat de opgegeven header is vereist. De waarde moet overeenkomen met die gedefinieerd in deze overeenkomst voorwaarde.
- **Komt niet overeen met**: is vereist dat de aanvraag voldoen aan een van de volgende criteria:
  - Het bevat niet de opgegeven header.
  - Het opgegeven header bevat, maar de waarde komt niet overeen met die gedefinieerd in deze overeenkomst voorwaarde.
  
Belangrijke informatie:
- Header-naam vergelijkingen zijn niet hoofdlettergevoelig. De **negeren geval** optie bepaalt de hoofdlettergevoeligheid vergelijkingen van header-waarde.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---  
### <a name="request-header-regex"></a>Aanvraag-Header Regex
De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de Regex-Header aanvraag overeenkomen met de voorwaarde wordt voldaan.
- **Komt overeen met**: de aanvraag bevat de opgegeven header is vereist. De waarde moet overeenkomen met het patroon dat gedefinieerd in de opgegeven reguliere expressie.
- **Komt niet overeen met**: is vereist dat de aanvraag voldoen aan een van de volgende criteria:
  - Het bevat niet de opgegeven header.
  - Het opgegeven header bevat, maar de waarde komt niet overeen met de opgegeven reguliere expressie.

Belangrijke informatie:
- Headernaam: 
  - Header-naam vergelijkingen zijn niet hoofdlettergevoelig.
  - Spaties in de header-naam moeten worden vervangen door "% 20". 
- Headerwaarde: 
  - De waarde van een koptekst kunt profiteren van reguliere expressies.
  - De **negeren geval** optie bepaalt de hoofdlettergevoeligheid vergelijkingen van header-waarde.
  - Alleen exacte header-waarde komt overeen met ten minste een van de opgegeven patronen voldoen aan deze voorwaarde.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd 

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="request-header-wildcard"></a>Aanvraag-Header jokertekens
De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder het jokerteken Header aanvraag overeenkomen met de voorwaarde wordt voldaan.
- **Komt overeen met**: de aanvraag bevat de opgegeven header is vereist. De waarde moet overeenkomen met ten minste een van de waarden die zijn gedefinieerd in deze overeenkomst voorwaarde.
- **Komt niet overeen met**: is vereist dat de aanvraag voldoen aan een van de volgende criteria:
  - Het bevat niet de opgegeven header.
  - Het opgegeven header bevat, maar de waarde komt niet overeen met een van de opgegeven waarden.
  
Belangrijke informatie:
- Headernaam: 
  - Header-naam vergelijkingen zijn niet hoofdlettergevoelig.
  - Spaties in de header-naam moeten worden vervangen door "% 20". U kunt deze waarde ook gebruiken om op te geven spaties in een headerwaarde.
- Headerwaarde: 
  - De waarde van een koptekst kunt profiteren van speciale tekens.
  - De **negeren geval** optie bepaalt de hoofdlettergevoeligheid vergelijkingen van header-waarde.
  - Alleen exacte header-waarde komt overeen met ten minste een van de opgegeven patronen voldoen aan deze voorwaarde.
  - Meerdere waarden opgeven door die begrenst elkaar met een spatie.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="request-method"></a>Verzoekmethode
Alleen elementen die worden aangevraagd met de methode van de geselecteerde aanvraag voldoen aan de voorwaarde aanvraagmethode. De beschikbare aanvraagmethoden zijn:
- GET
- HEAD 
- POST 
- OPTIES 
- PUT 
- DELETE 
- TRACERING 
- VERBINDING MAKEN 

Belangrijke informatie:
- Standaard kan alleen de methode GET-aanvraag in de cache inhoud op het netwerk genereren. Andere aanvraagmethoden zijn via een proxyserver doorgestuurd via het netwerk.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="request-scheme"></a>Aanvraag-schema
Alleen elementen die zijn aangevraagd door het geselecteerde protocol voldoen aan de voorwaarde schema aanvragen. De beschikbare protocollen zijn HTTP en HTTPS.

Belangrijke informatie:
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van Azure Content Delivery Network](cdn-overview.md)
* [Regels engine verwijzing](cdn-rules-engine-reference.md)
* [Regels engine voorwaardelijke expressies](cdn-rules-engine-reference-conditional-expressions.md)
* [Regels engine functies](cdn-rules-engine-reference-features.md)
* [Standaardgedrag HTTP met de regelengine van](cdn-rules-engine.md)


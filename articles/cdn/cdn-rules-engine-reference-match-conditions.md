---
title: Azure CDN regels overeen motor | Microsoft Docs
description: Documentatie bij Azure CDN regels overeen motor en onderdelen.
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
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: 0abbcf8508cb95d0fb8a9c8e5243426752efe590
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="azure-cdn-rules-engine-match-conditions"></a>Voldoen aan de engine van Azure CDN-regels
Dit onderwerp vindt u gedetailleerde beschrijvingen van de beschikbare overeenkomst voorwaarden voor Azure Content Delivery Network (CDN) [regelengine](cdn-rules-engine.md).

Het tweede gedeelte van een regel wordt de voorwaarde van de overeenkomst. De voorwaarde van een overeenkomst identificeert specifieke typen aanvragen waarvoor een reeks functies worden uitgevoerd.

Het kan bijvoorbeeld worden gebruikt voor het filteren van aanvragen voor inhoud op een bepaalde locatie aanvragen die worden gegenereerd vanuit een bepaalde IP-adres of een land of als header-informatie.

## <a name="always"></a>Altijd

De voorwaarde van de overeenkomst altijd is ontworpen voor een aantal functies toepassen op alle aanvragen.

## <a name="device"></a>Apparaat

De voorwaarde van de overeenkomst apparaat identificeert aanvragen van een mobiel apparaat op basis van de eigenschappen ervan.  Detectie van mobiele apparaten wordt bereikt door [WURFL](http://wurfl.sourceforge.net/).  WURFL mogelijkheden en hun variabelen CDN regelengine worden hieronder vermeld.
<br>
> [!NOTE] 
> De variabelen die hieronder worden ondersteund in de **Client aanvraag-Header wijzigen** en **Client antwoordheader wijzigen** functies.

Mogelijkheid | Variabele | Beschrijving | Voorbeeld waarde(n)
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
Mobiele Browser | % {wurfl_cap_mobile_browser} | Een tekenreeks die aangeeft van de browser gebruikt voor het aanvragen van inhoud van het apparaat. | Chrome
Mobiele browserversie | % {wurfl_cap_mobile_browser_version} | Een tekenreeks die de versie van de browser gebruikt voor het aanvragen van inhoud van het apparaat aangeeft. | 31
Modelnaam | % {wurfl_cap_model_name} | Een tekenreeks die modelnaam van het apparaat aangeeft. | S3
Progressief downloaden | % {wurfl_cap_progressive_download} | Een Booleaanse waarde die aangeeft of het apparaat het afspelen van audio/video ondersteunt terwijl het nog steeds wordt gedownload. | waar
Releasedatum | % {wurfl_cap_release_date} | Een tekenreeks die het jaar en maand op waarop het apparaat is toegevoegd aan de database WURFL aangeeft.<br/><br/>Indeling:`yyyy_mm` | 2013_december
Hoogte van de oplossing | % {wurfl_cap_resolution_height} | Een geheel getal dat van het apparaat hoogte in pixels aangeeft. | 768
Breedte van de oplossing | % {wurfl_cap_resolution_width} | Een geheel getal dat aangeeft het apparaat de breedte in pixels dat. | 1024


## <a name="location"></a>Locatie

Deze overeenkomst voorwaarden zijn ontworpen om aanvragen op basis van de locatie van de aanvrager te identificeren.

Naam | Doel
-----|--------
AS-nummer | Aanvragen die afkomstig van een bepaald netwerk zijn identificeert.
Land | Aanvragen die afkomstig uit de opgegeven landen zijn identificeert.

### <a name="as-number"></a>AS-nummer 
Dit netwerk is gedefinieerd door de Autonomous System Number (ASN). Een optie is om aan te geven of deze voorwaarde wordt voldaan aan wanneer een client netwerk 'Komt overeen met' of 'Komt niet overeen met' het opgegeven getal opgegeven.

**Belangrijke informatie**
- Meerdere AS-nummers opgeven door die begrenst elkaar met een spatie. 64514 64515 komt bijvoorbeeld overeen met aanvragen die binnenkomen vanuit 64514 of 64515.
- Bepaalde aanvragen kunnen niet een geldig getal retourneren. Een vraagteken (dat wil zeggen,?) komt overeen met aanvragen waarvoor een geldig getal kan niet worden bepaald.
- Het volledige nummer voor het gewenste netwerk moet worden opgegeven. Ondersteuning voor gedeeltelijke waarden wordt niet overeen.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

### <a name="country"></a>Land
Een land kan worden opgegeven via de landcode. Krijgt u een optie om aan te geven of deze voorwaarde wordt voldaan wanneer het land van waaruit een aanvraag afkomstig 'Komt overeen met' of 'Komt niet overeen met' de opgegeven waarden worden ingesteld is.


**Belangrijke informatie**
- Geef meerdere landcodes door die begrenst elkaar met een spatie.
- Jokertekens worden niet ondersteund bij het opgeven van een landcode.
- De landcodes 'EU' en 'AP' kunnen niet alle IP-adressen in die gebieden omvatten.
- Bepaalde aanvragen kunnen niet een geldige landcode geretourneerd. Een vraagteken (dat wil zeggen,?) komt overeen met aanvragen waarvoor een geldige landcode kan niet worden bepaald.
- Landcodes zijn hoofdlettergevoelig.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

## <a name="origin"></a>Oorsprong

Deze overeenkomst voorwaarden zijn ontworpen om u te identificeren aanvraagt dat punt naar de CDN-opslag of een klant-bronserver.

Naam | Doel
-----|--------
CDN-oorsprong | Aanvragen voor inhoud die is opgeslagen op opslag CDN identificeert.
Oorsprong van de klant | Aanvragen voor inhoud die is opgeslagen op een specifieke klant oorsprong server identificeert.

### <a name="cdn-origin"></a>CDN-oorsprong
Deze overeenkomst-voorwaarde is voldaan wanneer beide volgende voorwaarden wordt voldaan:
- Inhoud uit CDN opslag is aangevraagd.
- De aanvraag-URI maakt gebruik van de toegang tot inhoud punt (bijvoorbeeld /000001) in deze overeenkomst voorwaarde gedefinieerd.
  - CDN-URL: De aanvraag-URI moet het toegangspunt voor het geselecteerde inhoud bevatten.
  - Rand CNAME-URL: De configuratie van de bijbehorende rand CNAME moet verwijzen naar de geselecteerde inhoud access point.
  
*Opmerkingen:*
 - Het toegangspunt inhoud de service identificeert die de gevraagde inhoud fungeren moet.
 - Een instructie en mag niet worden gebruikt aan bepaalde voorwaarden overeen combineren. Een voorwaarde CDN oorsprong overeen met de voorwaarde van een klant oorsprong overeen combineren zou bijvoorbeeld een patroon overeen die nooit overeen maken. Dezelfde daarom worden niet via een instructie en als gecombineerd met twee CDN oorsprong overeenkomst voorwaarden.
 
### <a name="customer-origin"></a>Oorsprong van de klant

**Belangrijke informatie** 
- Deze voorwaarde overeenkomst wordt ongeacht of inhoud wordt aangevraagd met behulp van een CDN of rand CNAME-URL die naar de oorsprong van de geselecteerde klant verwijst worden voldaan.
- De configuratie van een klant-oorsprong waarnaar wordt verwezen door een regel kan niet worden verwijderd uit de pagina van de oorsprong van de klant. Voordat u probeert te verwijderen van de configuratie van een klant oorsprong, zorg dat de volgende configuraties niet verwijzen naar deze:
  - Klant oorsprong overeen voorwaarde
  - Een edge CNAME-configuratie.
- Een instructie en mag niet worden gebruikt aan bepaalde voorwaarden overeen combineren. Een voorwaarde klant oorsprong overeen met de voorwaarde van een oorsprong CDN overeen combineren zou bijvoorbeeld een patroon overeen die nooit overeen maken. Dezelfde daarom worden niet via een instructie en als gecombineerd met twee voorwaarden van de oorsprong van de klant overeen.

## <a name="request"></a>Aanvraag

Deze voorwaarden zijn ontworpen om u te identificeren aanvragen op basis van de eigenschappen overeen.

Naam | Doel
-----|--------
IP-adres van client | Aanvragen die afkomstig van een bepaald IP-adres zijn identificeert.
Cookie-Parameter | Controleert de cookies die zijn gekoppeld aan elke aanvraag naar de opgegeven waarde.
Cookie Parameter Regex | Controleert de cookies die zijn gekoppeld aan elke aanvraag voor de opgegeven reguliere expressie.
Rand Cname | Aanvragen die naar een specifieke rand CNAME verwijzen identificeert.
Verwijzende domein | Aanvragen die zijn aangeduid van de opgegeven hostname(s) identificeert.
Aanvraag-Header Literal | Geeft aan dat aanvragen met de opgegeven header is ingesteld op een opgegeven waarde(n).
Aanvraag-Header Regex | Geeft aan dat aanvragen met de opgegeven header is ingesteld op een waarde die overeenkomt met de opgegeven reguliere expressie.
Aanvraag-Header jokertekens | Geeft aan dat aanvragen met de opgegeven header is ingesteld op een waarde die overeenkomt met het opgegeven patroon.
Verzoekmethode | Identificeert aanvragen door de HTTP-methode.
Aanvraag-schema | Identificeert aanvragen door de HTTP-protocol.

### <a name="client-ip-address"></a>IP-adres van client
Een optie om aan te geven of deze voorwaarde wordt voldaan aan wanneer een client IP's wordt geleverd 'Komt overeen met' of 'Komt niet overeen met' adres de IP-adressen die zijn opgegeven.

**Belangrijke informatie:**
- Zorg ervoor dat CIDR-notatie wordt gebruikt.
- Geef meerdere IP-adressen en/of IP-adresblokken door die begrenst elkaar met een spatie.
  - **IPv4-voorbeeld:** 1.2.3.4 10.20.30.40 overeenkomt met de verzoeken van 1.2.3.4 of 10.20.30.40 die binnenkomen.
  - **IPv6-voorbeeld:** 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 overeenkomt met de verzoeken van 1:2:3:4:5:6:7:8 of 10:20:30:40:50:60:70:80 die binnenkomen.
- De syntaxis voor een IP-Adresblok is het basisadres van de IP-gevolgd door een slash en de grootte van het voorvoegsel.
  - **IPv4-voorbeeld:** 5.5.5.64/26 overeenkomt met de verzoeken van 5.5.5.64 via 5.5.5.127 die binnenkomen.
  - **IPv6-voorbeeld:** 1:2:3: / 48 komt overeen met de verzoeken van 1:2:3:0:0:0:0:0 via 1:2:3:ffff:ffff:ffff:ffff:ffff die binnenkomen.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

### <a name="cookie-parameter"></a>Cookie-Parameter
De **overeenkomt met / komt niet overeen met** optie bepaalt u de voorwaarden onder waarvoor deze overeenkomen met de voorwaarde voldaan.
- **Overeenkomsten:** vereist dat een aanvraag voor de opgegeven cookie met een waarde die overeenkomt met ten minste een van de waarden die zijn gedefinieerd in deze overeenkomst voorwaarde bevatten.
- **Komt niet overeen met:** vereist dat de aanvraag voldoen aan een van de volgende criteria:
  - Het bevat niet de opgegeven cookie.
  - De opgegeven cookie bevat, maar de waarde komt niet overeen met een van de waarden in deze overeenkomst voorwaarde gedefinieerd.
  
**Belangrijke informatie:**
- **Cookienaam:** 
  - Speciale tekens, inclusief een sterretje worden niet ondersteund bij het opgeven van een cookienaam. Dit betekent dat alleen exacte cookie naam komt overeen met in aanmerking voor de vergelijking komen.
  - Alleen de naam van een enkele cookie mag per exemplaar van deze overeenkomst voorwaarde worden opgegeven.
  - Cookie naam vergelijkingen zijn niet hoofdlettergevoelig.
- **Cookiewaarde:** 
  - Meerdere cookiewaarden opgeven door die begrenst elkaar met een spatie.
  - De waarde van een cookie kunt profiteren van speciale tekens. 
  - Als een jokerteken niet opgegeven is, wordt alleen een exacte overeenkomst deze overeenkomst voorwaarde voldoen. 
   - **Voorbeeld:** geven 'Waarde' komt overeen met '' waarde '', maar niet 'Value1' of 'Waarde2'.
  - De **negeren geval** optie wordt bepaald of hoofdletters worden uitgevoerd voor de waarde van de aanvraag-cookie.
  - Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
   - Voltooien van de opvulling van de Cache
   - Standaard interne-maximumleeftijd
   - Interne maximumleeftijd forceren
   - Negeren oorsprong No-Cache
   - Interne Max-verouderd

### <a name="cookie-parameter-regex"></a>Cookie Parameter Regex
Deze voorwaarde overeen definieert een Cookienaam en waarde. Reguliere expressies kunnen worden gebruikt voor het definiëren van de gewenste cookiewaarde. 

De **overeenkomt met / komt niet overeen met** optie bepaalt u de voorwaarden waaronder deze overeenkomst voorwaarde wordt voldaan.
- **Overeenkomsten:** vereist dat een aanvraag voor de opgegeven cookie met een waarde die overeenkomt met de opgegeven reguliere expressie bevatten.
- **Komt niet overeen met:** vereist dat de aanvraag voldoen aan een van de volgende criteria:
  - Het bevat niet de opgegeven cookie.
  - De opgegeven cookie bevat, maar de waarde komt niet overeen met de opgegeven reguliere expressie.
  
**Belangrijke informatie:**
- **Cookienaam:** 
  - Reguliere expressies en speciale tekens, inclusief een sterretje worden niet ondersteund bij het opgeven van een cookienaam. Dit betekent dat alleen exacte cookie naam komt overeen met in aanmerking voor de vergelijking komen.
  - Alleen de naam van een enkele cookie mag per exemplaar van deze overeenkomst voorwaarde worden opgegeven.
  - Cookie naam vergelijkingen zijn niet hoofdlettergevoelig.
- **Cookiewaarde:** 
  - De waarde van een cookie kunt profiteren van reguliere expressies.
  - De **negeren geval** optie wordt bepaald of hoofdletters worden uitgevoerd voor de waarde van de aanvraag-cookie.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

### <a name="edge-cname"></a>Rand Cname
**Belangrijke informatie** 
- De lijst met beschikbare rand CNAME-records is beperkt tot apparaten die zijn geconfigureerd op de pagina van de rand CNAMEs overeenkomt met het platform waarop HTTP regels-Engine wordt geconfigureerd.
- Voordat u probeert te verwijderen van een CNAME-configuratie van de rand, controleert u of een Edge-Cname overeenkomen met de voorwaarde verwijst niet naar het. Rand CNAME-configuraties die zijn gedefinieerd in een regel kunnen niet worden verwijderd uit de pagina rand CNAME-records. 
- Een instructie en mag niet worden gebruikt aan bepaalde voorwaarden overeen combineren. Een voorwaarde rand Cname overeen met de voorwaarde van een klant oorsprong overeen combineren zou bijvoorbeeld een patroon overeen die nooit overeen maken. Dezelfde daarom worden niet twee voorwaarden van de rand Cname overeen via een instructie en als gecombineerd.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

### <a name="referring-domain"></a>Verwijzende domein
De hostnaam die is gekoppeld aan de verwijzende site waarmee inhoud is aangevraagd, bepaalt u of deze voorwaarde is voldaan. Een optie om aan te geven of deze voorwaarde worden voldaan als de hostnaam van de verwijzende 'Komt overeen met' of 'Komt niet overeen met' is opgegeven de opgegeven waarden worden ingesteld.
**Belangrijke informatie:**
- Geef meerdere hostnamen door die begrenst elkaar met een spatie.
- Deze voorwaarde overeen ondersteunt speciale tekens.
- Als de opgegeven waarde geen een sterretje bevat, moet deze een exacte overeenkomst voor de verwijzende hostnaam. Bijvoorbeeld 'mijndomein.com' geven niet overeenkomen met 'www.mydomain.com'.
- De optie negeren geval bepaalt of hoofdletters worden uitgevoerd.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd
  
 ### <a name="request-header-literal"></a>Aanvraag-Header Literal
De **overeenkomt met / komt niet overeen met** optie bepaalt u de voorwaarden onder waarvoor deze overeenkomen met de voorwaarde voldaan.
- **Overeenkomsten:** vereist dat de aanvraag voor het opgegeven header en de waarde moeten overeenkomen met in deze overeenkomst voorwaarde gedefinieerd.
- **Komt niet overeen met:** vereist dat de aanvraag voldoen aan een van de volgende criteria:
  - Het bevat niet de opgegeven header.
  - Het opgegeven header bevat, maar de waarde komt niet overeen met de in deze overeenkomst voorwaarde gedefinieerd.
  
**Belangrijke informatie:**
- Header-naam vergelijkingen zijn niet hoofdlettergevoelig. De hoofdlettergevoeligheid vergelijkingen van header-waarde wordt bepaald door de optie geval negeren.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd
  
### <a name="request-header-regex"></a>Aanvraag-Header Regex
**Opmerking:** deze mogelijkheid is regels-Engine - geavanceerde regels die moeten worden afzonderlijk aangeschaft. Neem contact op met uw accountmanager CDN om deze te activeren. 

De **overeenkomt met / komt niet overeen met** optie bepaalt u de voorwaarden onder waarvoor deze overeenkomen met de voorwaarde voldaan.
- **Overeenkomsten:** vereist dat de aanvraag voor het opgegeven header en de waarde moeten overeenkomen met het patroon dat is gedefinieerd in de opgegeven reguliere expressie.
- **Komt niet overeen met:** vereist dat de aanvraag voldoen aan een van de volgende criteria:
  - Het bevat niet de opgegeven header.
  - Het opgegeven header bevat, maar de waarde komt niet overeen met de opgegeven reguliere expressie.

**Belangrijke informatie:**
- Headernaam: 
  - Header-naam vergelijkingen zijn niet hoofdlettergevoelig.
  - Spaties in de header-naam moeten worden vervangen door "% 20". 
- Headerwaarde: 
  - Een headerwaarde kan profiteren van reguliere expressies.
  - De hoofdlettergevoeligheid vergelijkingen van header-waarde wordt bepaald door de optie geval negeren.
  - Alleen exacte header-waarde komt overeen met ten minste een van de opgegeven patronen voldoen aan deze voorwaarde.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd 

### <a name="request-header-wildcard"></a>Aanvraag-Header jokertekens
De **overeenkomt met / komt niet overeen met** optie bepaalt u de voorwaarden onder waarvoor deze overeenkomen met de voorwaarde voldaan.
- **Overeenkomsten:** vereist dat de aanvraag voor het opgegeven header en de waarde moeten overeenkomen met ten minste een van de waarden die zijn gedefinieerd in deze overeenkomst voorwaarde.
- **Komt niet overeen met:** vereist dat de aanvraag voldoen aan een van de volgende criteria:
  - Het bevat niet de opgegeven header.
  - Het opgegeven header bevat, maar de waarde komt niet overeen met een van de opgegeven waarden.
  
**Belangrijke informatie:**
- Headernaam: 
  - Header-naam vergelijkingen zijn niet hoofdlettergevoelig.
  - Spaties in de header-naam moeten worden vervangen door "% 20". Deze waarde kan ook worden gebruikt om op te geven spaties in een headerwaarde.
- Headerwaarde: 
  - De waarde van een koptekst kunt profiteren van speciale tekens.
  - De hoofdlettergevoeligheid vergelijkingen van header-waarde wordt bepaald door de optie geval negeren.
  - Alleen exacte header-waarde komt overeen met ten minste een van de opgegeven patronen voldoen aan deze voorwaarde.
  - Meerdere waarden opgeven door die begrenst elkaar met een spatie.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

### <a name="request-method"></a>Verzoekmethode
Alleen elementen die worden aangevraagd met behulp van de geselecteerde aanvraagmethode voldoen aan deze voorwaarde. De beschikbare aanvraagmethoden zijn:
- TOEVOEGEN
- HEAD 
- VERZENDEN 
- OPTIES 
- PLAATSEN 
- VERWIJDEREN 
- TRACERING 
- VERBINDING MAKEN 

**Belangrijke informatie:**
- Standaard kan alleen de methode GET-aanvraag in de cache inhoud op ons netwerk genereren. Andere aanvraagmethoden zijn gewoon via een proxyserver doorgestuurd via het netwerk.
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

### <a name="request-scheme"></a>Aanvraag-schema
Alleen elementen die worden aangevraagd met behulp van het geselecteerde protocol voldoen aan deze voorwaarde. De beschikbare protocollen zijn HTTP en HTTPS.

**Belangrijke informatie:**
- Vanwege de manier die in cache van welke instellingen worden bijgehouden, moet aan deze voorwaarde overeenkomst is niet compatibel met de volgende functies:
  - Voltooien van de opvulling van de Cache
  - Standaard interne-maximumleeftijd
  - Interne maximumleeftijd forceren
  - Negeren oorsprong No-Cache
  - Interne Max-verouderd

## <a name="url"></a>URL

Deze overeenkomst voorwaarden zijn ontworpen om u te identificeren aanvragen op basis van de URL's.

Naam | Doel
-----|--------
URL-pad naar map | Identificeert aanvragen door hun relatief pad.
URL-pad-uitbreiding | Identificeert aanvragen door hun bestandsnaamextensie.
URL-pad bestandsnaam | Identificeert aanvragen door de bestandsnaam.
URL-pad Literal | Vergelijkt de relatieve pad van een aanvraag met de opgegeven waarde.
URL-pad Regex | Vergelijkt relatieve pad van een aanvraag naar de opgegeven reguliere expressie.
URL-pad jokerteken | Vergelijkt relatieve pad van een aanvraag naar het opgegeven patroon.
URL-Query Literal | Vergelijkt de queryreeks van een aanvraag met de opgegeven waarde.
URL-queryparameter | Geeft aan dat aanvragen met de opgegeven tekenreeks queryparameter ingesteld op een waarde die overeenkomt met een opgegeven patroon.
URL-Query Regex | Geeft aan dat aanvragen met de opgegeven tekenreeks queryparameter ingesteld op een waarde die overeenkomt met een opgegeven reguliere expressie.
URL-Query jokertekens | De opgegeven waarden worden ingesteld op basis van de queryreeks van de aanvraag wordt vergeleken.


## <a name="next-steps"></a>Volgende stappen
* [Overzicht van Azure CDN](cdn-overview.md)
* [Regels Engine verwijzing](cdn-rules-engine-reference.md)
* [Regels Engine voorwaardelijke expressies](cdn-rules-engine-reference-conditional-expressions.md)
* [Regels Engine functies](cdn-rules-engine-reference-features.md)
* [Standaardgedrag HTTP met de regelengine van](cdn-rules-engine.md)


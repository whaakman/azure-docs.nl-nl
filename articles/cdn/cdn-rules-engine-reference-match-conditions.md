---
title: Azure CDN regels criteria voor overeenkomst regelengine | Microsoft Docs
description: Naslagdocumentatie voor Azure Content Delivery Network-criteria voor overeenkomst regelengine voorschriften.
services: cdn
documentationcenter: ''
author: Lichard
manager: akucer
editor: ''
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2017
ms.author: rli
ms.openlocfilehash: 44182d686548fa5b6363a87be0ce7851829e20ab
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55820553"
---
# <a name="azure-cdn-rules-engine-match-conditions"></a>Voldoen aan de Azure CDN-regelengine 
Dit artikel vindt u gedetailleerde beschrijvingen van de criteria voor het Azure Content Delivery Network (CDN) voor de beschikbare overeenkomst [regels-engine](cdn-rules-engine.md).

Het tweede gedeelte van een regel wordt de voorwaarde voor overeenkomst. Een voorwaarde voor overeenkomst identificeert specifieke typen aanvragen die een set functies worden uitgevoerd.

U kunt bijvoorbeeld een voorwaarde voor overeenkomst te gebruiken:
- Aanvragen filteren voor inhoud op een bepaalde locatie.
- Aanvragen filteren is gegenereerd op basis van een bepaald IP-adres of land/regio.
- Aanvragen filteren door de header-informatie.

## <a name="always-match-condition"></a>Altijd overeenkomen met de voorwaarde

De voorwaarde altijd geldt een set functies voor alle aanvragen.

Name | Doel
-----|--------
[Altijd](#always) | Een set functies geldt voor alle aanvragen.

## <a name="device-match-condition"></a>Voorwaarde voor overeenkomst van apparaat

De apparaat-voorwaarde voor overeenkomst identificeert aanvragen van een mobiel apparaat op basis van de eigenschappen ervan.  

Name | Doel
-----|--------
[apparaat](#device) | Hiermee geeft u aanvragen van een mobiel apparaat op basis van de eigenschappen ervan.

## <a name="location-match-conditions"></a>Locatie-criteria voor overeenkomst

De locatie-criteria voor overeenkomst identificeren aanvragen op basis van de locatie van de aanvrager.

Name | Doel
-----|--------
[AS-nummer](#as-number) | Aanvragen die afkomstig van een bepaald netwerk zijn identificeert.
[Land](#country) | Hiermee geeft u aanvragen die afkomstig uit de opgegeven landen zijn.

## <a name="origin-match-conditions"></a>Criteria voor overeenkomst van oorsprong

De criteria voor overeenkomst van oorsprong identificeren aanvragen die naar de opslag van Content Delivery Network of een klant-bronserver verwijzen.

Name | Doel
-----|--------
[CDN-oorsprong](#cdn-origin) | Hiermee geeft u aanvragen voor inhoud die is opgeslagen in de opslag van Content Delivery Network.
[Oorsprong van de klant](#customer-origin) | Hiermee geeft u aanvragen voor inhoud die is opgeslagen op een specifieke klant origin-server.

## <a name="request-match-conditions"></a>Criteria voor overeenkomst aanvragen

De aanvraag-criteria voor overeenkomst identificeren op basis van hun eigenschappen aanvragen.

Name | Doel
-----|--------
[IP-adres van client](#client-ip-address) | Aanvragen die afkomstig van een bepaald IP-adres zijn identificeert.
[Cookie Parameter](#cookie-parameter) | Controleert of de cookies die zijn gekoppeld aan elke aanvraag voor de opgegeven waarde.
[Cookie Parameter reguliere expressie](#cookie-parameter-regex) | Controleert of de cookies die zijn gekoppeld aan elke aanvraag voor de opgegeven reguliere expressie.
[Cname voor edge](#edge-cname) | Aanvragen die naar een specifieke rand CNAME verwijzen identificeert.
[Verwijzende domein](#referring-domain) | Aanvragen die zijn verwezen in de namen van de opgegeven host identificeert.
[Aanvraag-Header letterlijke waarde](#request-header-literal) | Hiermee geeft u aanvragen met de opgegeven header ingesteld op een opgegeven waarde.
[Aanvraag-Header reguliere expressie](#request-header-regex) | Hiermee geeft u aanvragen met de opgegeven header ingesteld op een waarde die overeenkomt met de opgegeven reguliere expressie.
[Aanvraag-Header jokertekens](#request-header-wildcard) | Hiermee geeft u aanvragen met de opgegeven header ingesteld op een waarde die overeenkomt met het opgegeven patroon.
[Aanvraagmethode](#request-method) | Hiermee geeft u aanvragen door de HTTP-methode.
[Aanvraag-schema](#request-scheme) | Hiermee geeft u aanvragen door de HTTP-protocol.

## <a name="url-match-conditions"></a>URL-criteria voor overeenkomst

De URL-criteria voor overeenkomst identificeren op basis van de URL's aanvragen.

Name | Doel
-----|--------
[URL-Path-map](#url-path-directory) | Hiermee geeft u aanvragen door hun relatieve pad.
[URL-pad-extensie](#url-path-extension) | Hiermee geeft u aanvragen door de bestandsnaamextensie.
[URL-pad bestandsnaam](#url-path-filename) | Hiermee geeft u aanvragen door de bestandsnaam.
[URL-pad letterlijke waarde](#url-path-literal) | Vergelijkt de relatieve pad van een aanvraag met de opgegeven waarde.
[URL-pad reguliere expressie](#url-path-regex) | Vergelijkt de relatieve pad van een aanvraag naar de opgegeven reguliere expressie.
[URL-pad jokertekens](#url-path-wildcard) | Vergelijkt de relatieve pad van een aanvraag naar het opgegeven patroon.
[URL-Query letterlijke waarde](#url-query-literal) | Vergelijkt de querytekenreeks van een aanvraag met de opgegeven waarde.
[URL-queryparameter](#url-query-parameter) | Hiermee geeft u aanvragen met de opgegeven query-tekenreeksparameter ingesteld op een waarde die overeenkomt met een opgegeven patroon.
[URL-Query reguliere expressie](#url-query-regex) | Hiermee geeft u aanvragen met de opgegeven query-tekenreeksparameter ingesteld op een waarde die overeenkomt met een opgegeven reguliere expressie.
[URL-Query jokertekens](#url-query-wildcard) | De opgegeven waarde op basis van de aanvraag-queryreeks wordt vergeleken.


## <a name="reference-for-rules-engine-match-conditions"></a>Naslaginformatie voor de criteria voor overeenkomst regelengine

---
### <a name="always"></a>Altijd

De voorwaarde altijd geldt een set functies voor alle aanvragen.

[Terug naar boven](#main)

</br>

---
### <a name="as-number"></a>AS-nummer 
Het netwerk AS-nummer wordt gedefinieerd door de autonoom systeemnummer (ASN). 

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder het AS-nummer overeenkomen met de voorwaarde is voldaan:
- **Komt overeen met**: Vereist dat de ASN van het clientnetwerk overeenkomt met een van de opgegeven ASN's. 
- **Komt niet overeen met**: Vereist dat de ASN van het clientnetwerk komt niet overeen met een van de opgegeven ASN's.

Belangrijke informatie:
- Geef meerdere ASN's door die begrenst elkaar met een spatie. Bijvoorbeeld, 64514 64515 komt overeen met aanvragen die binnenkomen vanaf 64514 of 64515.
- Bepaalde aanvragen mogelijk een geldige ASN niet retourneren. Een vraagteken (?) komt overeen met aanvragen waarvoor een geldige ASN kan niet worden vastgesteld.
- Geef de volledige ASN worden gebruikt voor het gewenste netwerk. Ondersteuning voor gedeeltelijke waarden wordt niet overeen.
- Vanwege de manier in welke cache instellingen worden bijgehouden, is deze voorwaarde voor overeenkomst niet compatibel met de volgende functies:
  - Opvulling van de Cache voltooien
  - Standaard interne Max-Age
  - Force Internal Max-Age
  - Negeren van oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#main)

</br>

---
### <a name="cdn-origin"></a>CDN-oorsprong
De CDN-oorsprong voorwaarde wordt voldaan wanneer beide volgende voorwaarden wordt voldaan:
- Inhoud uit CDN-opslag is aangevraagd.
- De aanvraag-URI maakt gebruik van het type inhoud toegangspunt (bijvoorbeeld /000001) die gedefinieerd in deze voorwaarde voor overeenkomst:
  - CDN-URL: De aanvraag-URI moet het toegangspunt voor het geselecteerde inhoud bevatten.
  - Edge CNAME URL: De configuratie van de bijbehorende edge CNAME moet verwijzen naar de geselecteerde inhoud toegangspunt.
  
Belangrijke informatie:
 - Het punt toegang tot de inhoud identificeert de service die de gevraagde inhoud dienen moet.
 - Gebruik niet een instructie en als aan bepaalde criteria voor overeenkomst combineren. Bijvoorbeeld, maakt een voorwaarde voor CDN-oorsprong overeenkomst met een voorwaarde voor overeenkomst origine combineren een match-patroon die nooit kan worden afgestemd. Om deze reden kunnen niet via een instructie en als twee CDN Origin-criteria voor overeenkomst worden gecombineerd.

[Terug naar boven](#main)

</br>

---
### <a name="client-ip-address"></a>IP-adres van client
De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder het IP-adres van Client overeenkomen met de voorwaarde is voldaan:
- **Komt overeen met**: Vereist dat de IP-adres van de client overeenkomt met een van de opgegeven IP-adressen. 
- **Komt niet overeen met**: Vereist dat de IP-adres van de client komt niet overeen met een van de opgegeven IP-adressen. 

Belangrijke informatie:
- Gebruik de CIDR-notatie.
- Meerdere IP-adressen en/of IP-adresblokken opgeven die begrenst elkaar met een spatie. Bijvoorbeeld:
  - **Voorbeeld van de IPv4-**: 1.2.3.4 10.20.30.40 komt overeen met alle aanvragen die vanaf mailadres 1.2.3.4 of 10.20.30.40 binnenkomen.
  - **Voorbeeld van de IPv6-**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 komt overeen met alle aanvragen die vanaf adres 1:2:3:4:5:6:7:8 of 10:20:30:40:50:60:70:80 binnenkomen.
- De syntaxis voor een IP-Adresblok is het basisadres van de IP-gevolgd door een slash het bestemmingsvoorvoegsel en vaste grootte. Bijvoorbeeld:
  - **Voorbeeld van de IPv4-**: 5.5.5.64/26 komt overeen met alle aanvragen die vanaf 5.5.5.64 via 5.5.5.127-adressen binnenkomen.
  - **Voorbeeld van de IPv6-**: 1:2:3: / 48 komt overeen met alle aanvragen die van de adressen 1:2:3:0:0:0:0:0 via 1:2:3:ffff:ffff:ffff:ffff:ffff binnenkomen.
- Vanwege de manier in welke cache instellingen worden bijgehouden, is deze voorwaarde voor overeenkomst niet compatibel met de volgende functies:
  - Opvulling van de Cache voltooien
  - Standaard interne Max-Age
  - Force Internal Max-Age
  - Negeren van oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#main)

</br>

---
### <a name="cookie-parameter"></a>Cookie-Parameter
De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de Cookie-Parameter overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: Vereist een aanvraag voor de opgegeven cookie met een waarde die overeenkomt met ten minste een van de waarden die zijn gedefinieerd in deze voorwaarde voor overeenkomst bevatten.
- **Komt niet overeen met**: Vereist dat de aanvraag voldoet aan een van de volgende criteria:
  - De opgegeven cookie bevat niet.
  - De opgegeven cookie bevat, maar de waarde komt niet overeen met een van de waarden die zijn gedefinieerd in deze voorwaarde voor overeenkomst.
  
Belangrijke informatie:
- Cookienaam: 
  - Omdat jokertekens waarden, met inbegrip van sterretjes (*) worden niet ondersteund wanneer u een Cookienaam opgeven, zijn alleen exacte cookie naam komt overeen met die in aanmerking komen voor de vergelijking.
  - Alleen de naam van een enkele cookie kan per exemplaar van deze voorwaarde worden opgegeven.
  - Cookie naam vergelijkingen zijn niet hoofdlettergevoelig.
- Cookiewaarde: 
  - Meerdere cookiewaarden opgeven door die begrenst elkaar met een spatie.
  - De waarde van een cookie kan profiteren van [jokertekens waarden](cdn-rules-engine-reference.md#wildcard-values). 
  - Als een jokerteken-waarde niet is opgegeven, wordt alleen een exacte overeenkomst deze voorwaarde voldoen. Bijvoorbeeld, "waarde" komt overeen met "Waarde", maar niet 'Value1' of 'Value2'.
  - Gebruik de **negeren geval** optie om te bepalen of een hoofdlettergevoelige vergelijking wordt gemaakt op basis van de waarde van de cookie van de aanvraag.
- Vanwege de manier in welke cache instellingen worden bijgehouden, is deze voorwaarde voor overeenkomst niet compatibel met de volgende functies:
  - Opvulling van de Cache voltooien
  - Standaard interne Max-Age
  - Force Internal Max-Age
  - Negeren van oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#main)

</br>

---
### <a name="cookie-parameter-regex"></a>Cookie Parameter reguliere expressie
De Cookie Parameter Regex-voorwaarde voor overeenkomst definieert een Cookienaam en een waarde. U kunt [reguliere expressies](cdn-rules-engine-reference.md#regular-expressions) voor het definiëren van de gewenste cookiewaarde. 

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de Cookie Parameter reguliere expressie overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: Vereist een aanvraag voor de opgegeven cookie met een waarde die overeenkomt met de opgegeven reguliere expressie bevatten.
- **Komt niet overeen met**: Vereist dat de aanvraag voldoet aan een van de volgende criteria:
  - De opgegeven cookie bevat niet.
  - De opgegeven cookie bevat, maar de waarde komt niet overeen met de opgegeven reguliere expressie.
  
Belangrijke informatie:
- Cookienaam: 
  - Omdat reguliere expressies en jokertekens waarden, met inbegrip van sterretjes (*) worden niet ondersteund wanneer u een Cookienaam opgeven, zijn alleen exacte cookie naam komt overeen met die in aanmerking komen voor de vergelijking.
  - Alleen de naam van een enkele cookie kan per exemplaar van deze voorwaarde worden opgegeven.
  - Cookie naam vergelijkingen zijn niet hoofdlettergevoelig.
- Cookiewaarde: 
  - De waarde van een cookie kan profiteren van reguliere expressies.
  - Gebruik de **negeren geval** optie om te bepalen of een hoofdlettergevoelige vergelijking wordt gemaakt op basis van de waarde van de cookie van de aanvraag.
- Vanwege de manier in welke cache instellingen worden bijgehouden, is deze voorwaarde voor overeenkomst niet compatibel met de volgende functies:
  - Opvulling van de Cache voltooien
  - Standaard interne Max-Age
  - Force Internal Max-Age
  - Negeren van oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#main)

</br>

--- 
### <a name="country"></a>Land/regio
U kunt een land/regio via de landcode. 

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder het land overeenkomen met de voorwaarde is voldaan:
- **Komt overeen met**: De aanvraag bevat de waarden voor het opgegeven land/regio vereist. 
- **Komt niet overeen met**: Vereist dat de aanvraag bevat niet de waarden voor het opgegeven land.

Belangrijke informatie:
- Geef meerdere landcodes door die begrenst elkaar met een spatie.
- Jokertekens worden niet ondersteund wanneer u een landcode opgeven.
- De landcodes "EU" en 'Azië en Stille Oceaan' kunnen niet alle IP-adressen in die regio's omvatten.
- Bepaalde aanvragen mogelijk een geldige landcode niet retourneren. Een vraagteken (?) komt overeen met aanvragen waarvoor een geldige landcode kan niet worden vastgesteld.
- Landcodes zijn hoofdlettergevoelig.
- Vanwege de manier in welke cache instellingen worden bijgehouden, is deze voorwaarde voor overeenkomst niet compatibel met de volgende functies:
  - Opvulling van de Cache voltooien
  - Standaard interne Max-Age
  - Force Internal Max-Age
  - Negeren van oorsprong No-Cache
  - Interne Max-verouderd

#### <a name="implementing-country-filtering-by-using-the-rules-engine"></a>Landen filteren implementeren met behulp van de regelengine
Deze voorwaarde voor overeenkomst kunt u uitvoeren van een groot aantal aanpassingen op basis van de locatie van waaruit een aanvraag afkomstig is. Bijvoorbeeld, kan het gedrag van de functie voor het filteren van land/regio worden gerepliceerd via de volgende configuratie:

- Jokerteken van URL-pad: Stel de [URL-pad jokertekens overeenkomen met de voorwaarde](#url-path-wildcard) naar de map die wordt beveiligd. 
    Een sterretje aan het einde van het relatieve pad om ervoor te zorgen dat toegang tot alle onderliggende items worden beperkt door deze regel toevoegen.

- Land/regio overeenkomst: De land-voorwaarde voor overeenkomst ingesteld op de gewenste set landen.
   - Toestaan: De land-voorwaarde voor overeenkomst ingesteld op **komt niet overeen met** om toe te staan alleen de opgegeven landen toegang tot inhoud die is opgeslagen op de locatie die is gedefinieerd door de voorwaarde voor overeenkomst van jokertekens voor URL-pad.
   - Blokkeren: De land-voorwaarde voor overeenkomst ingesteld op **komt overeen met** moet worden geblokkeerd dat de opgegeven landen toegang krijgen tot inhoud die is opgeslagen op de locatie die is gedefinieerd door de voorwaarde voor overeenkomst van jokertekens voor URL-pad.

- Functie-toegang (403) weigeren: Schakel de [toegang weigeren (403) functie](cdn-rules-engine-reference-features.md#deny-access-403) voor het repliceren van het gedeelte toestaan of blokkeren van de functie landen filteren.

[Terug naar boven](#main)

</br>

---
### <a name="customer-origin"></a>Oorsprong van de klant

Belangrijke informatie: 
- De origine overeenkomst voorwaarde wordt voldaan, ongeacht of de inhoud wordt aangevraagd door een CDN-URL of een rand CNAME-URL die naar de oorsprong van de geselecteerde klant verwijst.
- De configuratie van een klant-oorsprong waarnaar wordt verwezen door een regel kan niet worden verwijderd uit de oorsprong van de klant-pagina. Voordat u probeert te verwijderen van de configuratie van een klant oorsprong, zorg ervoor dat de volgende configuraties bevatten geen verwijzing naar het:
  - Een voorwaarde voor overeenkomst origine
  - Een edge-CNAME-configuratie
- Gebruik niet een instructie en als aan bepaalde criteria voor overeenkomst combineren. Bijvoorbeeld, maakt een voorwaarde voor origine overeenkomst met een CDN-oorsprong-voorwaarde voor overeenkomst combineren een match-patroon die nooit kan worden afgestemd. Daarom kunnen niet via een instructie en als twee origine-criteria voor overeenkomst worden gecombineerd.

[Terug naar boven](#main)

</br>

---
### <a name="device"></a>Apparaat

De apparaat-voorwaarde voor overeenkomst identificeert aanvragen van een mobiel apparaat op basis van de eigenschappen ervan. Detectie van mobiele apparaten wordt bereikt door [WURFL](http://wurfl.sourceforge.net/). 

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder het apparaat overeenkomen met de voorwaarde is voldaan:
- **Komt overeen met**: Vereist van de aanvrager apparaat zodat deze overeenkomen met de opgegeven waarde. 
- **Komt niet overeen met**: Vereist van de aanvrager apparaat komt niet overeen met de opgegeven waarde.

Belangrijke informatie:

- Gebruik de **negeren geval** optie om op te geven of de opgegeven waarde hoofdlettergevoelig is.
- Vanwege de manier in welke cache instellingen worden bijgehouden, is deze voorwaarde voor overeenkomst niet compatibel met de volgende functies:
  - Opvulling van de Cache voltooien
  - Standaard interne Max-Age
  - Force Internal Max-Age
  - Negeren van oorsprong No-Cache
  - Interne Max-verouderd

#### <a name="string-type"></a>Tekenreekstype
Een functie WURFL accepteert meestal een combinatie van cijfers, letters en symbolen. Door de flexibele aard van deze mogelijkheid, moet u kiezen hoe de waarde die is gekoppeld aan deze voorwaarde wordt geïnterpreteerd. De volgende tabel beschrijft de set beschikbare opties:

Type     | Description
---------|------------
Letterlijke  | Selecteer deze optie om te voorkomen dat de meeste tekens op speciale betekenis met behulp van hun [letterlijke waarde](cdn-rules-engine-reference.md#literal-values).
Jokertekens | Selecteer deze optie om te profiteren van alle [jokertekens] ([jokertekens waarden](cdn-rules-engine-reference.md#wildcard-values).
Regex    | Selecteer deze optie om het gebruik van [reguliere expressies](cdn-rules-engine-reference.md#regular-expressions). Reguliere expressies zijn nuttig voor het definiëren van een patroon van tekens.

#### <a name="wurfl-capabilities"></a>WURFL mogelijkheden
Een functie WURFL verwijst naar een categorie die mobiele apparaten worden beschreven. De geselecteerde functie bepaalt het type van de beschrijving van het mobiele apparaat dat wordt gebruikt voor het identificeren van aanvragen.

De volgende tabel bevat WURFL mogelijkheden en hun variabelen voor de regelengine.
<br>
> [!NOTE] 
> De volgende variabelen worden ondersteund in de **wijzigen Client Header van** en **Client Reactieheader wijzigen** functies.

Mogelijkheid | Variabele | Description | Voorbeeldwaarden
-----------|----------|-------------|----------------
De naam van het merk | %{wurfl_cap_brand_name} | Een tekenreeks die de merknaam van het apparaat aangeeft. | Samsung
Besturingssysteem van het apparaat | %{wurfl_cap_device_os} | Een tekenreeks die aangeeft van het besturingssysteem op het apparaat geïnstalleerd. | IOS
Versie van apparaatbesturingssysteem | %{wurfl_cap_device_os_version} | Een tekenreeks die aangeeft van het versienummer van het besturingssysteem op het apparaat geïnstalleerd. | 1.0.1
Dubbele afdrukstand | %{wurfl_cap_dual_orientation} | Een Booleaanse waarde die aangeeft of het apparaat twee richting ondersteunt. | true
HTML voorkeur DTD | %{wurfl_cap_html_preferred_dtd} | Een tekenreeks die van het mobiele apparaat voorkeur document typedefinitie (DTD) voor HTML-inhoud aangeeft. | geen<br/>xhtml_basic<br/>html5
Afbeelding Inlining | %{wurfl_cap_image_inlining} | Een Booleaanse waarde die aangeeft of het apparaat met Base64 ondersteunt gecodeerd installatiekopieën. | false
Is Android | %{wurfl_vcap_is_android} | Een Booleaanse waarde die aangeeft of het apparaat gebruikmaakt van het Android-besturingssysteem. | true
IOS is | %{wurfl_vcap_is_ios} | Een Booleaanse waarde die aangeeft of het apparaat iOS gebruikt. | false
Is Smart TV | %{wurfl_cap_is_smarttv} | Een Booleaanse waarde die aangeeft of het apparaat een smart-tv's. | false
Smartphone is | %{wurfl_vcap_is_smartphone} | Een Booleaanse waarde die aangeeft of het apparaat een smartphone. | true
Is Tablet | %{wurfl_cap_is_tablet} | Een Booleaanse waarde die aangeeft of het apparaat een tablet. Deze beschrijving is onafhankelijk van het besturingssysteem. | true
Draadloze apparaat | %{wurfl_cap_is_wireless_device} | Een Booleaanse waarde die aangeeft of het apparaat wordt beschouwd als een draadloze-apparaat. | true
De naam van marketing | %{wurfl_cap_marketing_name} | Een tekenreeks die marketing naam van het apparaat aangeeft. | BlackBerry 8100 Pearl
Mobiele Browser | %{wurfl_cap_mobile_browser} | Een tekenreeks die de browser die wordt gebruikt voor het aanvragen van inhoud van het apparaat aangeeft. | Chrome
Mobiele Browser-versie | %{wurfl_cap_mobile_browser_version} | Een tekenreeks die de versie van de browser die wordt gebruikt voor het aanvragen van inhoud van het apparaat aangeeft. | 31
Modelnaam | %{wurfl_cap_model_name} | Een tekenreeks die de modelnaam van het apparaat aangeeft. | s3
Progressief downloaden | %{wurfl_cap_progressive_download} | Een Booleaanse waarde die aangeeft of het apparaat ondersteuning biedt voor het afspelen van audio en video terwijl het nog steeds wordt gedownload. | true
Releasedatum | %{wurfl_cap_release_date} | Een tekenreeks die aangeeft van het jaar en maand op waarop het apparaat is toegevoegd aan de WURFL-database.<br/><br/>Indeling: `yyyy_mm` | 2013_december
Resolutie hoogte | %{wurfl_cap_resolution_height} | Een geheel getal dat van het apparaat hoogte in pixels aangeeft. | 768
Breedte van de oplossing | %{wurfl_cap_resolution_width} | Een geheel getal dat aangeeft de breedte van het apparaat in pixels dat. | 1024

[Terug naar boven](#main)

</br>

---
### <a name="edge-cname"></a>Cname voor edge
Belangrijke informatie: 
- De lijst met beschikbare edge CNAME's is beperkt tot die edge CNAME's die zijn geconfigureerd op de pagina Edge CNAME-records voor het platform waarop de regelengine wordt geconfigureerd.
- Voordat u probeert te verwijderen van een rand CNAME-configuratie, ervoor zorgen dat een CNAME-Edge-voorwaarde voor overeenkomst verwijst niet naar deze. Edge CNAME-configuraties die zijn gedefinieerd in een regel kunnen niet worden verwijderd van de pagina Edge CNAME-records. 
- Gebruik niet een instructie en als aan bepaalde criteria voor overeenkomst combineren. Bijvoorbeeld, maakt een Edge-Cname-voorwaarde voor overeenkomst met een voorwaarde voor overeenkomst origine combineren een match-patroon die nooit kan worden afgestemd. Om deze reden kunnen niet via een instructie en als twee Edge Cname-criteria voor overeenkomst worden gecombineerd.
- Vanwege de manier in welke cache instellingen worden bijgehouden, is deze voorwaarde voor overeenkomst niet compatibel met de volgende functies:
  - Opvulling van de Cache voltooien
  - Standaard interne Max-Age
  - Force Internal Max-Age
  - Negeren van oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#main)

</br>

---
### <a name="referring-domain"></a>Verwijzende domein
De hostnaam die is gekoppeld aan de verwijzende site waarmee inhoud is aangevraagd, wordt bepaald of de verwijzende domein-voorwaarde wordt voldaan. 

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder het domein verwijst overeenkomen met de voorwaarde is voldaan:
- **Komt overeen met**: De naam van de verwijzende zodat deze overeenkomen met de opgegeven waarden vereist. 
- **Komt niet overeen met**: Vereist de verwijzende hostnaam komt niet overeen met de opgegeven waarde.

Belangrijke informatie:
- Namen van meerdere hosts opgeven die begrenst elkaar met een spatie.
- Biedt ondersteuning voor deze voorwaarde voor overeenkomst [jokertekens waarden](cdn-rules-engine-reference.md#wildcard-values).
- Als de opgegeven waarde geen een sterretje bevat, moet dit een exacte overeenkomst voor de hostnaam van de verwijzende site. Bijvoorbeeld, op te geven "mijndomein.com" zou niet overeen met "www.mydomain.com."
- Gebruik de **negeren geval** optie om te bepalen of een hoofdlettergevoelige vergelijking wordt gemaakt.
- Vanwege de manier in welke cache instellingen worden bijgehouden, is deze voorwaarde voor overeenkomst niet compatibel met de volgende functies:
  - Opvulling van de Cache voltooien
  - Standaard interne Max-Age
  - Force Internal Max-Age
  - Negeren van oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#main)

</br>

---  
### <a name="request-header-literal"></a>Aanvraag-Header letterlijke waarde
De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de aanvraag-Header letterlijke overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: Vereist dat de aanvraag voor de opgegeven header bevatten. De waarde moet overeenkomen met de optie die gedefinieerd in deze voorwaarde voor overeenkomst.
- **Komt niet overeen met**: Vereist dat de aanvraag voldoet aan een van de volgende criteria:
  - De opgegeven header bevat niet.
  - Deze bevat de opgegeven header, maar de waarde komt niet overeen met de optie die gedefinieerd in deze voorwaarde voor overeenkomst.
  
Belangrijke informatie:
- Header-naam vergelijkingen zijn altijd niet hoofdlettergevoelig. Gebruik de **negeren geval** optie voor het beheren van de hoofdlettergevoeligheid van vergelijkingen van header-waarde.
- Vanwege de manier in welke cache instellingen worden bijgehouden, is deze voorwaarde voor overeenkomst niet compatibel met de volgende functies:
  - Opvulling van de Cache voltooien
  - Standaard interne Max-Age
  - Force Internal Max-Age
  - Negeren van oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#main)

</br>

---  
### <a name="request-header-regex"></a>Aanvraag-Header reguliere expressie
De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de Regex-Header aanvraag overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: Vereist dat de aanvraag voor de opgegeven header bevatten. De waarde moet overeenkomen met het patroon dat gedefinieerd in de opgegeven [reguliere expressie](cdn-rules-engine-reference.md#regular-expressions).
- **Komt niet overeen met**: Vereist dat de aanvraag voldoet aan een van de volgende criteria:
  - De opgegeven header bevat niet.
  - Deze bevat de opgegeven header, maar de waarde komt niet overeen met de opgegeven reguliere expressie.

Belangrijke informatie:
- De headernaam: 
  - Header-naam vergelijkingen zijn niet hoofdlettergevoelig.
  - Spaties in de header-naam vervangen door "% 20." 
- Headerwaarde: 
  - De waarde van een header kan profiteren van reguliere expressies.
  - Gebruik de **negeren geval** optie voor het beheren van de hoofdlettergevoeligheid van vergelijkingen van header-waarde.
  - De voorwaarde is voldaan alleen als een headerwaarde exact overeenkomt met ten minste een van de opgegeven patronen.
- Vanwege de manier in welke cache instellingen worden bijgehouden, is deze voorwaarde voor overeenkomst niet compatibel met de volgende functies:
  - Opvulling van de Cache voltooien
  - Standaard interne Max-Age
  - Force Internal Max-Age
  - Negeren van oorsprong No-Cache
  - Interne Max-verouderd 

[Terug naar boven](#main)

</br>

---
### <a name="request-header-wildcard"></a>Aanvraag-Header jokertekens
De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder het jokerteken Header aanvraag overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: Vereist dat de aanvraag voor de opgegeven header bevatten. De waarde moet overeenkomen met ten minste een van de waarden die zijn gedefinieerd in deze voorwaarde voor overeenkomst.
- **Komt niet overeen met**: Vereist dat de aanvraag voldoet aan een van de volgende criteria:
  - De opgegeven header bevat niet.
  - Deze bevat de opgegeven header, maar de waarde komt niet overeen met een van de opgegeven waarden.
  
Belangrijke informatie:
- De headernaam: 
  - Header-naam vergelijkingen zijn niet hoofdlettergevoelig.
  - Spaties in de header-naam moeten worden vervangen door "% 20." U kunt deze waarde ook gebruiken om op te geven van spaties in een headerwaarde.
- Headerwaarde: 
  - De waarde van een header kan profiteren van [jokertekens waarden](cdn-rules-engine-reference.md#wildcard-values).
  - Gebruik de **negeren geval** optie voor het beheren van de hoofdlettergevoeligheid van vergelijkingen van header-waarde.
  - Deze voorwaarde wordt voldaan aan als een headerwaarde exact overeenkomt met ten minste een van de opgegeven patronen.
  - Meerdere waarden opgeven door die begrenst elkaar met een spatie.
- Vanwege de manier in welke cache instellingen worden bijgehouden, is deze voorwaarde voor overeenkomst niet compatibel met de volgende functies:
  - Opvulling van de Cache voltooien
  - Standaard interne Max-Age
  - Force Internal Max-Age
  - Negeren van oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#main)

</br>

---
### <a name="request-method"></a>Aanvraagmethode
De aanvraagmethode overeenkomst-voorwaarde is voldaan alleen als assets worden aangevraagd door de geselecteerde aanvraag-methode. De voor beschikbare aanvraagmethoden zijn:
- GET
- HEAD 
- POST 
- OPTIES 
- PUT 
- DELETE 
- TRACE 
- CONNECT 

Belangrijke informatie:
- Standaard kan alleen de GET-aanvraag-methode in de cache inhoud op het netwerk genereren. Alle andere aanvraagmethoden voor zijn via proxy via het netwerk.
- Vanwege de manier in welke cache instellingen worden bijgehouden, is deze voorwaarde voor overeenkomst niet compatibel met de volgende functies:
  - Opvulling van de Cache voltooien
  - Standaard interne Max-Age
  - Force Internal Max-Age
  - Negeren van oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#main)

</br>

---
### <a name="request-scheme"></a>Aanvraag-schema
De aanvraag schema overeenkomst-voorwaarde wordt voldaan alleen als assets worden aangevraagd door het geselecteerde protocol. De beschikbare protocollen zijn: 
- HTTP
- HTTPS

Belangrijke informatie:
- Vanwege de manier in welke cache instellingen worden bijgehouden, is deze voorwaarde voor overeenkomst niet compatibel met de volgende functies:
  - Opvulling van de Cache voltooien
  - Standaard interne Max-Age
  - Force Internal Max-Age
  - Negeren van oorsprong No-Cache
  - Interne Max-verouderd

[Terug naar boven](#main)

</br>

---
### <a name="url-path-directory"></a>URL-Path-map
Identificeert een aanvraag door de relatief pad, met uitsluiting van de bestandsnaam van de aangevraagde asset.

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de URL-pad-Directory overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: Vereist dat de aanvraag voor een relatieve URL-pad, met uitzondering van de bestandsnaam, die overeenkomt met de opgegeven URL-patroon bevatten.
- **Komt niet overeen met**: Vereist dat de aanvraag voor een relatieve URL-pad, met uitzondering van de bestandsnaam, komt niet overeen met de opgegeven URL-patroon bevatten.

Belangrijke informatie:
- Gebruik de **ten opzichte van** optie om op te geven of de URL-vergelijking vóór of na de punt van de toegang tot inhoud begint. De toegang tot inhoud-punt is het gedeelte van het pad dat wordt weergegeven tussen de Verizon CDN-hostnaam en het relatieve pad naar de aangevraagde asset (bijvoorbeeld /800001/CustomerOrigin). Verwijst naar een locatie op servertype (bijvoorbeeld een CDN of klant oorsprong) en het nummer van uw klant-account.

   De volgende waarden zijn beschikbaar voor de **ten opzichte van** optie:
   - **Hoofdmap**: Geeft aan dat het punt van de vergelijking van de URL direct na de CDN-hostnaam begint. 

     Bijvoorbeeld: http:\//wpc.0001.&lt; domein&gt;/**800001/myorigin/mijnmap**/index.htm

   - **Oorsprong**: Geeft aan dat het punt van de vergelijking van de URL na de punt van de toegang tot inhoud (bijvoorbeeld /000001 of/800001/myorigin begint). Omdat de \*. azureedge.net CNAME is ten opzichte van de map van de oorsprong van de hostnaam Verizon CDN die standaard worden gemaakt, moeten worden gebruikt door Azure CDN-gebruikers de **oorsprong** waarde. 

     Bijvoorbeeld: https:\//&lt;eindpunt&gt;.azureedge.net/**mijnmap**/index.htm 

     Deze URL verwijst naar de volgende Verizon CDN-hostnaam: http:\//wpc.0001.&lt; domein&gt;/800001/myorigin/**mijnmap**/index.htm

- Een edge CNAME-URL wordt herschreven naar de URL van een CDN voorafgaand aan de URL-vergelijking.

    Bijvoorbeeld, beide van de volgende URL's verwijzen naar dezelfde asset en daarom hebben de dezelfde URL-pad.
    - CDN-URL: http:\//wpc.0001.&lt; domein&gt;/800001/CustomerOrigin/path/asset.htm
    
    - Edge CNAME-URL: http:\//&lt;eindpunt&gt;.azureedge.net/path/asset.htm
    
    Aanvullende informatie:
    - Aangepast domein: https:\//my.domain.com/path/asset.htm
    
    - URL-pad (relatief aan hoofdmap van): / 800001/CustomerOrigin/path /
    
    - URL-pad (ten opzichte van oorsprong): /path/

- Het gedeelte van de URL die wordt gebruikt voor het einde van de vergelijking URL net vóór de bestandsnaam van de aangevraagde asset. Een afsluitende slash is het laatste teken in dit type pad.
    
- Geen spaties in het URL-pad-patroon vervangen door "% 20."
    
- Elk patroon URL-pad kan een of meer sterretjes (*), waarbij elke sterretje overeenkomt met een reeks van een of meer tekens bevatten.
    
- Geef meerdere URL-paden in het patroon door die begrenst elkaar met een spatie.

    Bijvoorbeeld: * /sales/ * /marketing/

- Een URL-pad-specificatie kunt profiteren van [jokertekens waarden](cdn-rules-engine-reference.md#wildcard-values).

- Gebruik de **negeren geval** optie om te bepalen of een hoofdlettergevoelige vergelijking wordt uitgevoerd.

[Terug naar boven](#main)

</br>

---
### <a name="url-path-extension"></a>URL-pad-extensie
Hiermee geeft u aanvragen door de bestandsextensie van de aangevraagde asset.

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de URL-pad-extensie overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: Vereist de URL van de aanvraag bevat een bestandsextensie die exact overeenkomt met het opgegeven patroon.

   Bijvoorbeeld, als u 'htm' opgeeft, wordt "htm" activa voldaan, maar niet 'html' activa.  

- **Komt niet overeen met**: Vereist dat de URL-aanvraag bevat een bestandsextensie die komt niet overeen met het opgegeven patroon.

Belangrijke informatie:
- Geef de bestandsextensies zoekt in de **waarde** vak. Maak daarbij dan geen geen voorlooppunt; bijvoorbeeld, htm gebruiken in plaats van htm.

- Gebruik de **negeren geval** optie om te bepalen of een hoofdlettergevoelige vergelijking wordt uitgevoerd.

- Meerdere bestandsextensies opgeven die door die begrenst elke uitbreiding met een spatie. 

    Bijvoorbeeld: htm html

- Bijvoorbeeld, op te geven "htm" komt overeen met "htm" activa, maar niet 'html' activa.


#### <a name="sample-scenario"></a>Voorbeeldscenario

De voorbeeldconfiguratie van de volgende wordt ervan uitgegaan dat deze voorwaarde wordt voldaan wanneer een aanvraag overeenkomt met een van de opgegeven extensies.

Specificatie-waarde: ASP-aspx-php html

Deze voorwaarde wordt voldaan wanneer URL's die met de volgende extensies eindigen:
- .asp
- .aspx
- .php
- .html

[Terug naar boven](#main)

</br>

---
### <a name="url-path-filename"></a>URL-pad bestandsnaam
Hiermee geeft u aanvragen door de bestandsnaam van de aangevraagde asset. Voor de toepassing van deze voorwaarde voor overeenkomst bestaat een bestandsnaam op van de naam van de aangevraagde asset, een punt en de bestandsextensie (bijvoorbeeld index.html).

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de URL-pad bestandsnaam overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: Vereist dat de aanvraag bevat een bestandsnaam in de URL-pad dat overeenkomt met het opgegeven patroon.
- **Komt niet overeen met**: Vereist dat de aanvraag bevat een naam in de URL-pad komt niet overeen met het opgegeven patroon.

Belangrijke informatie:
- Gebruik de **negeren geval** optie om te bepalen of een hoofdlettergevoelige vergelijking wordt uitgevoerd.

- Als u meerdere bestandsextensies, scheidt u elke uitbreiding met een spatie.

    Bijvoorbeeld: index.htm index.html

- Spaties in een waarde van de naam van bestand vervangen door "% 20."
    
- Waarde voor een bestandsnaam kunt profiteren van [jokertekens waarden](cdn-rules-engine-reference.md#wildcard-values). Bijvoorbeeld, kan elke bestandsnaampatroon bestaan uit een of meer sterretjes (*), waarbij elke sterretje overeenkomt met een reeks van een of meer tekens.
    
- Als jokertekens niet opgegeven zijn, wordt alleen een exacte overeenkomst deze voorwaarde voldoen.

    Bijvoorbeeld, op te geven "presentation.ppt" overeenkomt met een asset met de naam 'presentation.ppt', maar niet één met de naam 'presentatie.pptx."

[Terug naar boven](#main)

</br>

---
### <a name="url-path-literal"></a>URL-pad letterlijke waarde
Vergelijkt van een aanvraag URL-pad, inclusief de bestandsnaam, met de opgegeven waarde.

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de URL-pad letterlijke overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: Vereist dat de aanvraag bevat een URL-pad dat overeenkomt met het opgegeven patroon.
- **Komt niet overeen met**: Vereist dat de aanvraag bevat een URL-pad komt niet overeen met het opgegeven patroon.

Belangrijke informatie:
- Gebruik de **ten opzichte van** optie om op te geven of het punt van de vergelijking van de URL vóór of na de punt van de toegang tot inhoud begint. 

    De volgende waarden zijn beschikbaar voor de **ten opzichte van** optie:
     - **Hoofdmap**: Geeft aan dat het punt van de vergelijking van de URL direct na de CDN-hostnaam begint.

       Bijvoorbeeld: http:\//wpc.0001.&lt; domein&gt;/**800001/myorigin/myfolder/index.htm**

     - **Oorsprong**: Geeft aan dat het punt van de vergelijking van de URL na de punt van de toegang tot inhoud (bijvoorbeeld /000001 of/800001/myorigin begint). Omdat de \*. azureedge.net CNAME is ten opzichte van de map van de oorsprong van de hostnaam Verizon CDN die standaard worden gemaakt, moeten worden gebruikt door Azure CDN-gebruikers de **oorsprong** waarde. 

       Bijvoorbeeld: https:\//&lt;eindpunt&gt;.azureedge.net/**myfolder/index.htm**

     Deze URL verwijst naar de volgende Verizon CDN-hostnaam: http:\//wpc.0001.&lt; domein&gt;/800001/myorigin/**myfolder/index.htm**

- Een edge CNAME-URL wordt herschreven naar een CDN-URL voordat u een URL-vergelijking.

    Bijvoorbeeld, beide van de volgende URL's verwijzen naar dezelfde asset en daarom hebben de dezelfde URL-pad:
    - CDN-URL: http:\//wpc.0001.&lt; domein&gt;/800001/CustomerOrigin/path/asset.htm
    - Edge CNAME-URL: http:\//&lt;eindpunt&gt;.azureedge.net/path/asset.htm
    
    Aanvullende informatie:
    
    - URL-pad (relatief aan hoofdmap van): /800001/CustomerOrigin/path/asset.htm
   
    - URL-pad (ten opzichte van oorsprong): /path/asset.htm

- Queryreeksen in de URL worden genegeerd.
- Gebruik de **negeren geval** optie om te bepalen of een hoofdlettergevoelige vergelijking wordt uitgevoerd.
- De opgegeven waarde voor deze voorwaarde voor overeenkomst vergeleken met het relatieve pad van de exacte verzoek van de client.

- Gebruiken om alle aanvragen bij een bepaalde map, de [URL-pad naar map](#url-path-directory) of de [URL-pad jokertekens](#url-path-wildcard) overeenkomen met de voorwaarde.

[Terug naar boven](#main)

</br>

---
### <a name="url-path-regex"></a>URL-pad reguliere expressie
Vergelijking van de aanvraag-URL-pad naar het opgegeven [reguliere expressie](cdn-rules-engine-reference.md#regular-expressions).

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de URL-pad reguliere expressie overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: De aanvraag bevat een URL-pad dat overeenkomt met de opgegeven reguliere expressie vereist.
- **Komt niet overeen met**: De aanvraag bevat een URL-pad komt niet overeen met de opgegeven reguliere expressie vereist.

Belangrijke informatie:
- Een edge CNAME-URL wordt herschreven naar een CDN-URL voor vergelijking van de URL. 
 
    Bijvoorbeeld, beide URL's verwijzen naar dezelfde asset en daarom hebben de dezelfde URL-pad.

     - CDN-URL: http:\//wpc.0001.&lt; domein&gt;/800001/CustomerOrigin/path/asset.htm

     - Edge CNAME-URL: http:\//my.domain.com/path/asset.htm
    
    Aanvullende informatie:
    
     - URL-pad: /800001/CustomerOrigin/path/asset.htm

- Queryreeksen in de URL worden genegeerd.
    
- Gebruik de **negeren geval** optie om te bepalen of een hoofdlettergevoelige vergelijking wordt uitgevoerd.
    
- Opslagruimten in het URL-pad moeten worden vervangen door "% 20."

[Terug naar boven](#main)

</br>

---
### <a name="url-path-wildcard"></a>URL-pad jokertekens
Relatieve URL-pad van een aanvraag voor het patroon opgegeven jokertekens vergelijkt.

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de URL-pad jokerteken overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: Vereist dat de aanvraag voor een URL-pad dat overeenkomt met het patroon opgegeven jokertekens bevatten.
- **Komt niet overeen met**: Vereist dat de aanvraag voor een URL-pad komt niet overeen met het patroon opgegeven jokertekens bevatten.

Belangrijke informatie:
- **Ten opzichte van** optie: Deze optie bepaalt u of het punt van de vergelijking van de URL vóór of na de punt van de toegang tot inhoud begint.

   Deze optie kan de volgende waarden hebben:
     - **Hoofdmap**: Geeft aan dat het punt van de vergelijking van de URL direct na de CDN-hostnaam begint.

       Bijvoorbeeld: http:\//wpc.0001.&lt; domein&gt;/**800001/myorigin/myfolder/index.htm**

     - **Oorsprong**: Geeft aan dat het punt van de vergelijking van de URL na de punt van de toegang tot inhoud (bijvoorbeeld /000001 of/800001/myorigin begint). Omdat de \*. azureedge.net CNAME is ten opzichte van de map van de oorsprong van de hostnaam Verizon CDN die standaard worden gemaakt, moeten worden gebruikt door Azure CDN-gebruikers de **oorsprong** waarde. 

       Bijvoorbeeld: https:\//&lt;eindpunt&gt;.azureedge.net/**myfolder/index.htm**

     Deze URL verwijst naar de volgende Verizon CDN-hostnaam: http:\//wpc.0001.&lt; domein&gt;/800001/myorigin/**myfolder/index.htm**

- Een edge CNAME-URL wordt herschreven naar een CDN-URL voor vergelijking van de URL.

    Bijvoorbeeld, beide van de volgende URL's verwijzen naar dezelfde asset en daarom hebben de dezelfde URL-pad:
     - De URL van CDN: http://wpc.0001.&lt; domein&gt;/800001/CustomerOrigin/path/asset.htm
     - Edge CNAME-URL: http:\//&lt;eindpunt&gt;.azureedge.net/path/asset.htm
    
    Aanvullende informatie:
    
     - URL-pad (relatief aan hoofdmap van): /800001/CustomerOrigin/path/asset.htm
    
     - URL-pad (ten opzichte van oorsprong): /path/asset.htm
    
- Geef meerdere URL-paden door die begrenst elkaar met een spatie.

   Bijvoorbeeld: /marketing/asset.* /sales/*.htm

- Queryreeksen in de URL worden genegeerd.
    
- Gebruik de **negeren geval** optie om te bepalen of een hoofdlettergevoelige vergelijking wordt uitgevoerd.
    
- Opslagruimten in het URL-pad vervangen door "% 20."
    
- De opgegeven waarde voor een URL-pad van profiteren kan [jokertekens waarden](cdn-rules-engine-reference.md#wildcard-values). Elk patroon URL-pad kan een of meer sterretjes (*), waarbij elke sterretje overeenkomen met een reeks van een of meer tekens bevatten.

#### <a name="sample-scenarios"></a>Voorbeeldscenario 's

De voorbeeld-configuraties in de volgende tabel wordt ervan uitgegaan dat aan deze voorwaarde is voldaan wanneer een aanvraag overeenkomt met de opgegeven URL-patroon:

Value                   | Ten opzichte van    | Resultaat 
------------------------|----------------|-------
*/test.html */test.php  | Basis- of oorsprong | Dit patroon is afgestemd op aanvragen voor activa met de naam 'test.html' of 'test.php' in een map.
/ 80ABCD/oorsprong/tekst / *   | Basis           | Dit patroon wordt voldaan aan wanneer de aangevraagde asset voldoet aan de volgende criteria: <br />-Deze moet zich bevinden op de oorsprong van een klant met de naam 'origin'. <br />-Het relatieve pad moet beginnen met een map met de naam 'tekst'. De aangevraagde asset kan dat wil zeggen, ofwel zich bevinden in de map 'tekst' of een van de recursieve submappen.
*/CSS/* */js/*          | Basis- of oorsprong | Dit patroon wordt voldaan aan alle CDN of edge CNAME-URL's die een CSS- of js-map bevatten.
*.jpg *.gif *.png       | Basis- of oorsprong | Dit patroon is afgestemd op alle CDN- of edge CNAME-URL's .jpg, .gif of .png eindigt. Een alternatieve manier om op te geven van dit patroon is met de [URL-pad extensie overeenkomen met de voorwaarde](#url-path-extension).
/ afbeeldingen / * / media / *      | Oorsprong         | Dit patroon wordt voldaan aan door de CDN- of edge CNAME-URL's waarvan het relatieve pad met een 'installatiekopieën' of 'media'-map begint. <br />-CDN URL: http:\//wpc.0001.&lt; domein&gt;/800001/myorigin/images/sales/event1.png<br />-Voorbeeld edge CNAME-URL: http:\//cdn.mydomain.com/images/sales/event1.png

[Terug naar boven](#main)

</br>

---
### <a name="url-query-literal"></a>URL-Query letterlijke waarde
Vergelijkt de querytekenreeks van een aanvraag met de opgegeven waarde.

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de URL-Query letterlijke overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: Vereist dat de aanvraag voor een URL-query-tekenreeks die overeenkomt met de opgegeven queryreeks bevatten.
- **Komt niet overeen met**: Vereist dat de aanvraag voor een URL-querytekenreeks die komt niet overeen met de opgegeven queryreeks bevatten.

Belangrijke informatie:

- Alleen exacte query tekenreeks komt overeen met voldoen aan deze voorwaarde voor overeenkomst.
    
- Gebruik de **negeren geval** optie voor het beheren van de hoofdlettergevoeligheid van query-tekenreeksvergelijkingen.
    
- Moet u een toonaangevende vraagteken (?) niet opnemen in de tekst van de query-tekenreeks-waarde.
    
- Bepaalde tekens vereist URL-codering. Gebruik het percentagesymbool naar URL coderen de volgende tekens bevatten:

   Teken | URL Encoding
   ----------|---------
   Ruimte     | %20
   &         | %25

- Vanwege de manier in welke cache instellingen worden bijgehouden, is deze voorwaarde voor overeenkomst niet compatibel met de volgende functies:
   - Opvulling van de Cache voltooien
   - Standaard interne Max-Age
   - Force Internal Max-Age
   - Negeren van oorsprong No-Cache
   - Interne Max-verouderd

[Terug naar boven](#main)

</br>

---
### <a name="url-query-parameter"></a>URL Query Parameter
Hiermee geeft u aanvragen met de opgegeven queryreeks-parameter. Deze parameter is ingesteld op een waarde die overeenkomt met een opgegeven patroon. Queryparameters tekenreeks (bijvoorbeeld parameter = value) in de aanvraag-URL te bepalen of deze voorwaarde wordt voldaan. Deze voorwaarde identificeert een queryreeks-parameter met de naam en een of meer waarden voor de waarde van parameter accepteert. 

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de queryparameter URL overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: Vereist een aanvraag voor de opgegeven parameter met een waarde die overeenkomt met ten minste een van de waarden die zijn gedefinieerd in deze voorwaarde voor overeenkomst bevatten.
- **Komt niet overeen met**: Vereist dat de aanvraag voldoet aan een van de volgende criteria:
  - Het bevat niet de opgegeven parameter.
  - Deze bevat de opgegeven parameter, maar de waarde komt niet overeen met een van de waarden die zijn gedefinieerd in deze voorwaarde voor overeenkomst.

Deze voorwaarde voor overeenkomst biedt een eenvoudige manier om op te geven van de naam/waarde-combinaties van parameters. Voor meer flexibiliteit als u een queryreeks-parameter overeenkomende, kunt u overwegen de [URL Query jokertekens](#url-query-wildcard) overeenkomen met de voorwaarde.

Belangrijke informatie:
- Per exemplaar van deze voorwaarde voor overeenkomst kan alleen een enkele URL-query parameternaam worden opgegeven.
    
- Omdat jokertekens waarden worden niet ondersteund wanneer een parameternaam is opgegeven, zijn alleen exacte parameter naam komt overeen met die in aanmerking komen voor de vergelijking.
- Parameterwaarde(n) kunt opnemen [jokertekens waarden](cdn-rules-engine-reference.md#wildcard-values).
   - Elke parameter waarde patroon kan bestaan uit een of meer sterretjes (*), waarbij elke sterretje overeenkomen met een reeks van een of meer tekens.
   - Bepaalde tekens vereist URL-codering. Gebruik het percentagesymbool naar URL coderen de volgende tekens bevatten:

       Teken | URL Encoding
       ----------|---------
       Ruimte     | %20
       &         | %25

- Geef meerdere parameterwaarden van de query-tekenreeks door die begrenst elkaar met een spatie. Deze voorwaarde wordt voldaan aan wanneer een aanvraag een van de opgegeven naam/waarde-combinaties bevat.

   - Voorbeeld 1:

     - Configuratie:

       ValueA ValueB

     - Deze configuratie komt overeen met de volgende parameters van de query-tekenreeks:

       Parameter1=ValueA
    
       Parameter1=ValueB

   - Voorbeeld 2:

     - Configuratie: 

        % 20A waarde % 20 ter waarde

     - Deze configuratie komt overeen met de volgende parameters van de query-tekenreeks:

       Parameter1=Value%20A

       Parameter1=Value%20B

- Deze voorwaarde is voldaan alleen als er een exacte overeenkomst voor ten minste één van de opgegeven query-tekenreeks naam/waarde-combinaties.

   Bijvoorbeeld als u de configuratie in het vorige voorbeeld, de naam/waarde van parameter combinatie "Parameter1 ValueAdd =" kan niet worden gezien als een overeenkomst. Echter, als u een van de volgende waarden opgeven, dit komt overeen met deze combinatie van naam/waarde:

   - ValueA waardeb is ValueAdd
   - ValueA * waardeb is

- Gebruik de **negeren geval** optie voor het beheren van de hoofdlettergevoeligheid van query-tekenreeksvergelijkingen.
    
- Vanwege de manier in welke cache instellingen worden bijgehouden, is deze voorwaarde voor overeenkomst niet compatibel met de volgende functies:
   - Opvulling van de Cache voltooien
   - Standaard interne Max-Age
   - Force Internal Max-Age
   - Negeren van oorsprong No-Cache
   - Interne Max-verouderd

#### <a name="sample-scenarios"></a>Voorbeeldscenario's
Het volgende voorbeeld ziet u hoe deze optie werkt in bepaalde situaties:

Name  | Value |  Resultaat
------|-------|--------
Gebruiker  | Jaap   | Dit patroon wordt voldaan aan wanneer de query-tekenreeks voor een opgegeven URL is "? gebruiker = joe."
Gebruiker  | *     | Dit patroon wordt gekoppeld wanneer de query-tekenreeks voor een opgegeven URL een parameter van de gebruiker bevat.
Email | Jaap\* | Dit patroon wordt voldaan aan wanneer de query-tekenreeks voor een opgegeven URL bevat een e-parameter die begint met "Jan".

[Terug naar boven](#main)

</br>

---
### <a name="url-query-regex"></a>URL-Query reguliere expressie
Hiermee geeft u aanvragen met de opgegeven queryreeks-parameter. Deze parameter is ingesteld op een waarde die overeenkomt met een opgegeven [reguliere expressie](cdn-rules-engine-reference.md#regular-expressions).

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder de URL-Query reguliere expressie overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: De aanvraag bevat een URL-query-tekenreeks die overeenkomt met de opgegeven reguliere expressie vereist.
- **Komt niet overeen met**: De aanvraag bevat een URL-querytekenreeks die komt niet overeen met de opgegeven reguliere expressie vereist.

Belangrijke informatie:
- Alleen voldoen exacte overeenkomsten met de opgegeven reguliere expressie deze voorwaarde voor overeenkomst.
    
- Gebruik de **negeren geval** optie voor het beheren van de hoofdlettergevoeligheid van query-tekenreeksvergelijkingen.
    
- Voor de toepassing van deze optie wordt begint een query-tekenreeks met het eerste teken na het scheidingsteken vraagteken (?) voor de query-tekenreeks.
    
- Bepaalde tekens vereist URL-codering. Gebruik het percentagesymbool naar URL coderen de volgende tekens bevatten:

   Teken | URL Encoding | Value
   ----------|--------------|------
   Ruimte     | %20          | \%20
   &         | %25          | \%25

   Houd er rekening mee dat de symbolen percentage moeten worden weergegeven.

- Speciale reguliere expressie dubbele escape-tekens (bijvoorbeeld \^$. +) om op te nemen van een backslash in de reguliere expressie.

   Bijvoorbeeld:

   Value | Geïnterpreteerd als 
   ------|---------------
   \\+    | +
   \\\\+   | \\+

- Vanwege de manier in welke cache instellingen worden bijgehouden, is deze voorwaarde voor overeenkomst niet compatibel met de volgende functies:
   - Opvulling van de Cache voltooien
   - Standaard interne Max-Age
   - Force Internal Max-Age
   - Negeren van oorsprong No-Cache
   - Interne Max-verouderd


[Terug naar boven](#main)

</br>

---
### <a name="url-query-wildcard"></a>URL-Query jokertekens
De opgegeven waarde(n) op basis van de aanvraag-queryreeks vergelijkt.

De **komt overeen met**/**komt niet overeen met** optie bepaalt u de voorwaarden waaronder het jokerteken van de Query-URL overeenkomen met de voorwaarde is voldaan.
- **Komt overeen met**: Vereist dat de aanvraag voor een URL-query-tekenreeks die overeenkomt met de waarde van de opgegeven jokertekens bevatten.
- **Komt niet overeen met**: De aanvraag bevat een URL-querytekenreeks die komt niet overeen met de opgegeven jokerteken-waarde vereist.

Belangrijke informatie:
- Voor de toepassing van deze optie wordt begint een query-tekenreeks met het eerste teken na het scheidingsteken vraagteken (?) voor de query-tekenreeks.
- Parameterwaarden kunnen opnemen [jokertekens waarden](cdn-rules-engine-reference.md#wildcard-values):
   - Elke parameter waarde patroon kan bestaan uit een of meer sterretjes (*), waarbij elke sterretje overeenkomen met een reeks van een of meer tekens.
   - Bepaalde tekens vereist URL-codering. Gebruik het percentagesymbool naar URL coderen de volgende tekens bevatten:

     Teken | URL Encoding
     ----------|---------
     Ruimte     | %20
     &         | %25

- Meerdere waarden opgeven door die begrenst elkaar met een spatie.

   Bijvoorbeeld: *Parameter1=ValueA* *ValueB* *Parameter1=ValueC&Parameter2=ValueD*

- Alleen voldoen exacte overeenkomsten met ten minste één van de opgegeven tekenreeks querypatronen deze voorwaarde voor overeenkomst.
    
- Gebruik de **negeren geval** optie voor het beheren van de hoofdlettergevoeligheid van query-tekenreeksvergelijkingen.
    
- Vanwege de manier in welke cache instellingen worden bijgehouden, is deze voorwaarde voor overeenkomst niet compatibel met de volgende functies:
   - Opvulling van de Cache voltooien
   - Standaard interne Max-Age
   - Force Internal Max-Age
   - Negeren van oorsprong No-Cache
   - Interne Max-verouderd

#### <a name="sample-scenarios"></a>Voorbeeldscenario's
Het volgende voorbeeld ziet u hoe deze optie werkt in bepaalde situaties:

 Name                 | Description
 ---------------------|------------
user=joe              | Dit patroon wordt voldaan aan wanneer de query-tekenreeks voor een opgegeven URL is "? gebruiker = joe."
\*gebruiker =\* \*optout =\* | Dit patroon wordt gekoppeld wanneer de CDN-URL-query de gebruikers- of optout-parameter bevat.

[Terug naar boven](#main)

</br>

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van Azure Content Delivery Network](cdn-overview.md)
* [Verwijzing regelengine](cdn-rules-engine-reference.md)
* [Voorwaardelijke expressies regelengine](cdn-rules-engine-reference-conditional-expressions.md)
* [Functies regelengine](cdn-rules-engine-reference-features.md)
* [Standaardgedrag HTTP met behulp van de regelengine](cdn-rules-engine.md)


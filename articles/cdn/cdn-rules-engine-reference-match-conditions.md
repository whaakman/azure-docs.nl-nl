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
ms.openlocfilehash: f4886b1d78dfa87cf25737fb46c12b5963034f27
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cdn-rules-engine-match-conditions"></a>Voldoen aan de engine van Azure CDN-regels
Dit onderwerp vindt u gedetailleerde beschrijvingen van de beschikbare overeenkomst voorwaarden voor Azure Content Delivery Network (CDN) [regelengine](cdn-rules-engine.md).

Het tweede gedeelte van een regel wordt de voorwaarde van de overeenkomst. De voorwaarde van een overeenkomst identificeert specifieke typen aanvragen waarvoor een reeks functies worden uitgevoerd.

Het kan bijvoorbeeld worden gebruikt voor het filteren van aanvragen voor inhoud op een bepaalde locatie aanvragen die worden gegenereerd vanuit een bepaalde IP-adres of een land of als header-informatie.

## <a name="always"></a>Altijd

De voorwaarde van de overeenkomst altijd is ontworpen voor een aantal functies toepassen op alle aanvragen.

## <a name="device"></a>Apparaat

De voorwaarde van de overeenkomst apparaat identificeert aanvragen van een mobiel apparaat op basis van de eigenschappen ervan.  Detectie van mobiele apparaten wordt bereikt door [WURFL](http://wurfl.sourceforge.net/).  WURFL mogelijkheden en hun variabelen CDN regelengine worden hieronder vermeld.

> [!NOTE] 
> De variabelen die hieronder worden ondersteund in de **Client aanvraag-Header wijzigen** en **Client antwoordheader wijzigen** functies.

Mogelijkheid | Variabele | Beschrijving | Voorbeeld waarde(n)
-----------|----------|-------------|----------------
De naam van het merk | % {wurfl_cap_brand_name} | Een tekenreeks die de naam van het merk van het apparaat geeft. | Samsung
Besturingssysteem van het apparaat | % {wurfl_cap_device_os} | Een tekenreeks die aangeeft van het besturingssysteem op het apparaat geïnstalleerd. | IOS
Versie besturingssysteem apparaat | % {wurfl_cap_device_os_version} | Een tekenreeks die het versienummer van het besturingssysteem op het apparaat geïnstalleerd geeft. | 1.0.1
Dual afdrukstand | % {wurfl_cap_dual_orientation} | Een Booleaanse waarde die aangeeft of het apparaat dual afdrukstand ondersteunt. | De waarde True
HTML voorkeur DTD | % {wurfl_cap_html_preferred_dtd} | Een tekenreeks die typedefinitie (DTD) voor HTML-inhoud van het mobiele apparaat voorkeur document aangeeft. | Geen<br/>xhtml_basic<br/>HTML5
Afbeelding Inlining | % {wurfl_cap_image_inlining} | Een Boolean die aangeeft of het apparaat ondersteunt met Base64 gecodeerd installatiekopieën. | ONWAAR
Android is | % {wurfl_vcap_is_android} | Een Booleaanse waarde die aangeeft of het apparaat Android OS gebruikt. | De waarde True
IOS is | % {wurfl_vcap_is_ios} | Een Booleaanse waarde die aangeeft of het apparaat iOS gebruikt. | ONWAAR
Smart TV is | % {wurfl_cap_is_smarttv} | Een Booleaanse waarde die aangeeft of het apparaat een intelligente TV is. | ONWAAR
Smartphone is | % {wurfl_vcap_is_smartphone} | Een Booleaanse waarde die aangeeft of het apparaat een smartphone is. | De waarde True
Is Tablet | % {wurfl_cap_is_tablet} | Een Booleaanse waarde die aangeeft of het apparaat een tablet is. Dit is een beschrijving onafhankelijk van het besturingssysteem. | De waarde True
Draadloze apparaat | % {wurfl_cap_is_wireless_device} | Een Booleaanse waarde die aangeeft of het apparaat wordt beschouwd als een draadloos apparaat. | De waarde True
De naam van marketing | % {wurfl_cap_marketing_name} | Een tekenreeks die marketing naam van het apparaat aangeeft. | BlackBerry 8100 Pearl
Mobiele Browser | % {wurfl_cap_mobile_browser} | Een tekenreeks die aangeeft van de browser gebruikt voor het aanvragen van inhoud van het apparaat. | Chrome
Mobiele browserversie | % {wurfl_cap_mobile_browser_version} | Een tekenreeks die de versie van de browser gebruikt voor het aanvragen van inhoud van het apparaat aangeeft. | 31
Modelnaam | % {wurfl_cap_model_name} | Een tekenreeks die modelnaam van het apparaat aangeeft. | S3
Progressief downloaden | % {wurfl_cap_progressive_download} | Een Booleaanse waarde die aangeeft of het apparaat het afspelen van audio/video ondersteunt terwijl het nog steeds wordt gedownload. | De waarde True
Releasedatum | % {wurfl_cap_release_date} | Een tekenreeks die het jaar en maand op waarop het apparaat is toegevoegd aan de database WURFL aangeeft.<br/><br/>Indeling:`yyyy_mm` | 2013_december
Hoogte van de oplossing | % {wurfl_cap_resolution_height} | Een geheel getal dat van het apparaat hoogte in pixels aangeeft. | 768
Breedte van de oplossing | % {wurfl_cap_resolution_width} | Een geheel getal dat aangeeft het apparaat de breedte in pixels dat. | 1024


## <a name="location"></a>Locatie

Deze overeenkomst voorwaarden zijn ontworpen om aanvragen op basis van de locatie van de aanvrager te identificeren.

Naam | Doel
-----|--------
AS-nummer | Aanvragen die afkomstig van een bepaald netwerk zijn identificeert.
Land | Aanvragen die afkomstig uit de opgegeven landen zijn identificeert.


## <a name="origin"></a>Oorsprong

Deze overeenkomst voorwaarden zijn ontworpen om u te identificeren aanvraagt dat punt naar de CDN-opslag of een klant-bronserver.

Naam | Doel
-----|--------
CDN-oorsprong | Aanvragen voor inhoud die is opgeslagen op opslag CDN identificeert.
Oorsprong van de klant | Aanvragen voor inhoud die is opgeslagen op een specifieke klant oorsprong server identificeert.


## <a name="request"></a>Aanvraag

Deze voorwaarden zijn ontworpen om u te identificeren aanvragen op basis van de eigenschappen overeen.

Naam | Doel
-----|--------
IP-clientadres | Aanvragen die afkomstig van een bepaald IP-adres zijn identificeert.
Cookie-Parameter | Controleert de cookies die zijn gekoppeld aan elke aanvraag naar de opgegeven waarde.
Cookie Parameter Regex | Controleert de cookies die zijn gekoppeld aan elke aanvraag voor de opgegeven reguliere expressie.
Rand Cname | Aanvragen die naar een specifieke rand CNAME verwijzen identificeert.
Verwijzende domein | Aanvragen die zijn aangeduid van de opgegeven hostname(s) identificeert.
Aanvraag-Header Literal | Geeft aan dat aanvragen met de opgegeven header is ingesteld op een opgegeven waarde(n).
Aanvraag-Header Regex | Geeft aan dat aanvragen met de opgegeven header is ingesteld op een waarde die overeenkomt met de opgegeven reguliere expressie.
Aanvraag-Header jokertekens | Geeft aan dat aanvragen met de opgegeven header is ingesteld op een waarde die overeenkomt met het opgegeven patroon.
Verzoekmethode | Identificeert aanvragen door de HTTP-methode.
Aanvraag-schema | Identificeert aanvragen door de HTTP-protocol.

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


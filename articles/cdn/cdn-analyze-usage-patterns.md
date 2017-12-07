---
title: Rapporten van Verizon Core | Microsoft Docs
description: 'U kunt gebruikspatronen voor uw CDN bekijken met behulp van de volgende rapporten: bandbreedte, de hoeveelheid overgedragen gegevens, treffers, Cache statussen, Cache Hit Ratio, IPV4/IPV6-gegevens overgebracht.'
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 5a0d9018-8bdb-48ff-84df-23648ebcf763
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: fa828bfa736d677fb4881e5cc2628c0e03eb8749
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="core-reports-from-verizon"></a>Rapporten van Verizon Core

[!INCLUDE[cdn-verizon-only](../../includes/cdn-verizon-only.md)]

U kunt met behulp van Verizon Core rapporten via de portal beheren voor Verizon profielen, gebruikspatronen bekijken voor uw CDN met de volgende rapporten:

* Bandbreedte
* Gegevens die overgedragen
* Treffers
* Cache-statussen
* Verhouding tussen treffers in de cache
* IPv4/IPV6-gegevens die zijn overgedragen

## <a name="accessing-verizon-core-reports"></a>Toegang tot rapporten van Verizon Core
1. Klik in de blade CDN-profiel op de **beheren** knop.
   
    ![Knop voor CDN-profiel beheren](./media/cdn-reports/cdn-manage-btn.png)
   
    Hiermee opent u de CDN-beheerportal.
2. Beweeg de muisaanwijzer over de **Analytics** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **Core rapporten** doel. Klik op een rapport in het menu.
   
    ![CDN-beheerportal - menu Core rapporten](./media/cdn-reports/cdn-core-reports.png)

3. Voor elk rapport, selecteer een datumbereik van de **datumbereik** lijst. U kunt een vooraf gedefinieerde datumbereik, zoals selecteren **vandaag** of **deze Week**, of u kunt selecteren **aangepaste** handmatig te typen en een datumbereik door te klikken op de kalenderpictogrammen. 

4. Nadat u een datumbereik hebt geselecteerd, klikt u op **gaat** om het rapport te genereren. 

4. Als u de gegevens in Excel-indeling exporteren wilt, klikt u op het bovenstaande Excel-pictogram de **gaat** knop.

## <a name="bandwidth"></a>Bandbreedte
Het rapport bandbreedte bestaat uit een grafiek en een gegevenstabel die het bandbreedtegebruik CDN voor HTTP en HTTPS gedurende een bepaalde periode in Mbps aangeeft. U kunt het bandbreedtegebruik weergeven via alle POP's of voor een bepaalde POP. Dit rapport kunt u het verkeer pieken en distribueren voor POP's weergeven.

Van de **Edge-knooppunten** selecteert **alle knooppunten van de rand** Zie verkeer van alle knooppunten of Selecteer een specifieke regio.

Het rapport wordt bijgewerkt om de vijf minuten.

![Bandbreedte-rapport](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Gegevens die overgedragen
Dit rapport bevat een grafiek en een gegevenstabel die het gebruik van CDN-verkeer voor HTTP en HTTPS gedurende een bepaalde periode in GB aangeeft. U kunt het gebruik van het verkeer tussen alle POP's of voor een bepaalde POP weergeven. Dit rapport kunt u het verkeer pieken en distribueren via POP's weergeven.

Van de **Edge-knooppunten** selecteert **alle knooppunten van de rand** Zie verkeer van alle knooppunten of Selecteer een specifieke regio.

Het rapport wordt bijgewerkt om de vijf minuten.

![Gegevensoverdracht-rapport](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>Treffers (statuscodes)
Dit rapport beschrijft de distributie van aanvraag statuscodes voor uw inhoud. Elke aanvraag voor inhoud genereert een HTTP-statuscode. De statuscode wordt beschreven hoe rand POP's verwerkt de aanvraag. Bijvoorbeeld, een statuscode 2xx geeft aan dat de aanvraag met succes is uitgevoerd naar een client, terwijl een statuscode 4xx geeft aan dat er een fout opgetreden. Zie voor meer informatie over HTTP-statuscodes [lijst van HTTP-statuscodes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

![Treffers rapport](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Statusbepaling van de cache
Dit rapport wordt de distributie van treffers in cache en Cachemissers voor clientaanvragen beschreven. Omdat de snelste prestaties uit treffers in cache voortvloeit, kunt u gegevens levering snelheden optimaliseren door te minimaliseren Cachemissers en treffers in cache verlopen. 

Als u Cachemissers, oorsprong server om te beperken van het gebruik van de volgende opties te configureren: 
 * `no-cache`antwoordheaders
 * Query-tekenreeks caching, tenzij strikt noodzakelijk is  
 * Niet-caching geschikte reactiecodes

Als u treffers in cache verlopen, instellen van een activum `max-age` op een lange periode het aantal aanvragen met de oorspronkelijke server te minimaliseren.

![Cache statussen rapport](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Hoofdcache statuswaarden zijn onder andere:
* TCP_HIT: Aangeboden via edge-server. Het object in de cache is en niet de maximumleeftijd overschreden.
* TCP_MISS: Geleverd van de bronserver. Het object is niet in de cache en het antwoord terug naar de oorsprong is.
* TCP_EXPIRED _MISS: geleverd vanuit de oorspronkelijke server na het opnieuw te worden gevalideerd met oorsprong. Het object is in de cache, maar de maximumleeftijd had overschreden. Een hervalidatie met oorsprong resulteert in het cacheobject wordt vervangen door een nieuw antwoord van de oorsprong.
* TCP_EXPIRED _HIT: aangeboden via Edge na hervalidatie met oorsprong. Het object is in de cache, maar de maximumleeftijd had overschreden. Een opnieuw te worden gevalideerd met de oorspronkelijke server resulteert in het cacheobject wordt niet gewijzigd.

### <a name="full-list-of-cache-statuses"></a>Volledige lijst van cache statussen
* TCP_HIT - deze status wordt vermeld als een aanvraag naar de client rechtstreeks vanuit de pop-server is uitgevoerd. Een actief wordt onmiddellijk van een POP-geleverd wanneer het is in de cache op de pop-server die het dichtst bij de client en een geldige time-to-live (TTL) heeft. TTL wordt bepaald door de volgende antwoordheaders:
  
  * Cache-Control: s-maxage
  * Cache-Control: maximumleeftijd
  * Verloopt op
* TCP_MISS: Deze status geeft aan dat de versie van de aangevraagde asset een cache niet op de pop-server die het dichtst bij de client gevonden is. De asset is aangevraagd vanaf een bronserver of een schild bronserver. Als de bronserver of de oorsprong shield-server een asset retourneert, is geleverd aan de client en in de cache op zowel de client als de edge-server. Anders wordt een niet-200-statuscode (bijvoorbeeld 403 verboden of 404 niet gevonden) wordt geretourneerd.
* TCP_EXPIRED_HIT: Deze status wordt vermeld als een aanvraag die gericht is op een activum met een verlopen TTL rechtstreeks vanuit de pop-server naar de client is uitgevoerd. Bijvoorbeeld, is wanneer de activa de maximale leeftijd verlopen. 
  
   Een verlopen aanvraag resulteert gewoonlijk in een validatieaanvraag met de oorspronkelijke server. Voor de TCP_EXPIRED_HIT status om te worden uitgevoerd, moet de oorspronkelijke server aangeven dat een nieuwere versie van de asset niet bestaat. Deze situatie resulteert gewoonlijk in een update van de asset Cache-Control en Expires-koppen.
* TCP_EXPIRED_MISS: Deze status wordt vermeld als een nieuwere versie van een verlopen in de cache asset van de pop-server naar de client wordt geleverd. Deze status wordt weergegeven wanneer de TTL voor een asset in de cache is verlopen (bijvoorbeeld verlopen maximumleeftijd) en de oorspronkelijke server retourneert een nieuwere versie van dat actief. Deze nieuwe versie van de asset is geleverd aan de client in plaats van de versie van de cache. Bovendien is het in cache op de edge-server en de client.
* CONFIG_NOCACHE: Deze status geeft aan dat de configuratie van een klantspecifieke rand POP voorkomen dat de asset worden opgeslagen.
* GEEN: deze status geeft aan dat een cache-inhoud nieuwheid-controle is niet uitgevoerd.
* TCP_CLIENT_REFRESH_MISS: Deze status wordt vermeld als een HTTP-client, zoals een browser, zorgt ervoor een edge pop-server dat voor het ophalen van een nieuwe versie van een verouderde actief op de bronserver. Standaard de servers te voorkomen dat een HTTP-client de edge-servers voor het ophalen van een nieuwe versie van de asset vanaf de oorspronkelijke server forceren.
* TCP_PARTIAL_HIT: Deze status wordt vermeld als een aanvraag voor het bereik van byte in een treffer voor een asset gedeeltelijk in cache resulteert. Het aangevraagde bytebereik is onmiddellijk door de pop-server aan de client geleverd.
* UNCACHEABLE: Deze status wordt vermeld als een asset `Cache-Control` en `Expires` headers aangeven dat deze mag niet worden opgeslagen op een pop-server of door de HTTP-client. Dergelijke aanvragen worden geleverd op de bronserver.

## <a name="cache-hit-ratio"></a>Verhouding tussen treffers in de cache
Dit rapport geeft het percentage van in cache aanvragen die zijn geleverd rechtstreeks vanuit de cache.

Het rapport bevat de volgende details:

* De gevraagde inhoud in cache is opgeslagen op de pop-server die het dichtst bij de aanvrager.
* De aanvraag is uitgevoerd rechtstreeks vanaf de rand van het netwerk.
* De aanvraag is niet moet worden gevalideerd met de bronserver.

Het rapport bevatten niet:

* Aanvragen die zijn geweigerd vanwege land opties voor het filteren.
* Aanvragen voor activa waarvan headers aangeven dat ze mag niet worden opgeslagen. Bijvoorbeeld: `Cache-Control: private`, `Cache-Control: no-cache`, of `Pragma: no-cache` headers te voorkomen dat een actief worden opgeslagen.
* Byte-bereikaanvragen voor deels in cache opgeslagen inhoud.

De formule is: (TCP_ HIT / (TCP_ HIT + TCP_MISS)) * 100

![Rapport van de verhouding tussen treffers in de cache](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>IPv4/IPV6-gegevens die zijn overgedragen
Dit rapport toont de distributie van de informatie over het gebruik verkeer in IPV4 en IPV6.

![IPv4/IPV6-gegevens die zijn overgedragen](./media/cdn-reports/cdn-ipv4-ipv6.png)

## <a name="considerations"></a>Overwegingen
Rapporten kunnen alleen worden gegenereerd in de afgelopen 18 maanden.


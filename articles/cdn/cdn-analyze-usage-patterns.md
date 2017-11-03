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
ms.openlocfilehash: e9ee0041061296e313b3372dce13b5b86b2369c8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
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
   
    ![Knop blade CDN-profiel beheren](./media/cdn-reports/cdn-manage-btn.png)
   
    Hiermee opent u de CDN-beheerportal.
2. Beweeg de muisaanwijzer over de **Analytics** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **Core rapporten** doel.  Klik op het gewenste rapport in het menu.
   
    ![CDN-beheerportal - menu Core rapporten](./media/cdn-reports/cdn-core-reports.png)

## <a name="bandwidth"></a>Bandbreedte
Het rapport bandbreedte bestaat uit een grafiek en een gegevenstabel die aangeeft van het bandbreedtegebruik voor HTTP en HTTPS gedurende een bepaalde periode. U kunt het bandbreedtegebruik weergeven via alle CDN POP's of een specifieke CDN pop-locatie. Dit rapport kunt u het verkeer pieken en distribueren via CDN POP's in Mbps weergeven.

* Selecteer alle knooppunten van de rand Zie verkeer van alle knooppunten of kies een specifieke regio/knooppunt in de vervolgkeuzelijst.
* Selecteer het datumbereik wilt weergeven van gegevens voor deze vandaag/week/deze maand, enzovoort of Voer de aangepaste datums en klik op **gaat** om ervoor te zorgen dat uw selectie wordt bijgewerkt.
* U kunt exporteren en downloaden van de gegevens door te klikken op het excel-blad pictogram naast **gaat**.

Het rapport wordt bijgewerkt om de vijf minuten.

![Bandbreedte-rapport](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Gegevens die overgedragen
Dit rapport bevat een grafiek en een gegevenstabel die het gebruik van verkeer voor HTTP en HTTPS gedurende een bepaalde periode aangegeven. U kunt het gebruik van het verkeer tussen alle CDN POP's of een bepaalde POP weergeven. Dit rapport kunt u het verkeer pieken en distribueren via CDN POP's in GB weergeven.

* Selecteer alle knooppunten van de rand verkeer van alle notities zien of een specifieke regio/knooppunt kiezen uit de vervolgkeuzelijst.
* Selecteer het datumbereik wilt weergeven van gegevens voor deze vandaag/week/deze maand, enzovoort of Voer de aangepaste datums en klik op **gaat** om ervoor te zorgen dat uw selectie wordt bijgewerkt.
* U kunt exporteren en downloaden van de gegevens door te klikken op het excel-blad pictogram naast **gaat**.

Het rapport wordt bijgewerkt om de vijf minuten.

![Gegevensoverdracht-rapport](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>Treffers (statuscodes)
Dit rapport beschrijft de distributie van aanvraag statuscodes voor uw inhoud. Elke aanvraag voor inhoud genereert een HTTP-statuscode. De statuscode wordt beschreven hoe rand POP's verwerkt de aanvraag. Bijvoorbeeld, een statuscode 2xx geeft aan dat de aanvraag met succes is uitgevoerd naar een client, terwijl een statuscode 4xx geeft aan dat er een fout opgetreden. Zie voor meer informatie over HTTP-statuscodes [statuscodes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

* Selecteer het datumbereik wilt weergeven van gegevens voor deze vandaag/week/deze maand, enzovoort of Voer de aangepaste datums en klik op **gaat** om ervoor te zorgen dat uw selectie wordt bijgewerkt.
* U kunt exporteren en downloaden van de gegevens door te klikken op de excel-werkblad naast zich **gaat**.

![Treffers rapport](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Statusbepaling van de cache
Dit rapport wordt de distributie van treffers in cache en Cachemissers voor clientaanvraag beschreven. Omdat de snelste prestaties afkomstig zijn van treffers in cache, kunt u gegevens levering snelheden door Minimalisatie van het aantal Cachemissers en treffers in cache verlopen optimaliseren. U kunt Cachemissers verminderen door het configureren van de bronserver om te voorkomen dat 'no-cache' antwoordheaders toewijzen, door te vermijden queryreeks in cache opslaan, behalve indien strikt noodzakelijk is en door te vermijden niet caching geschikte responscodes. Treffers in cache verlopen kunt u voorkomen door het maken van een asset-maximumleeftijd zo lang mogelijk het aantal aanvragen met de oorspronkelijke server te minimaliseren.

![Cache statussen rapport](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Hoofdcache statuswaarden zijn onder andere:
* TCP_HIT: Geleverd vanuit Edge. Het object in de cache is en niet de maximumleeftijd overschreden.
* TCP_MISS: Geleverd vanuit de oorsprong. Het object is niet in de cache en het antwoord terug naar de oorsprong is.
* TCP_EXPIRED _MISS: geleverd vanuit de oorsprong na hervalidatie met oorsprong. Het object is in de cache, maar de maximumleeftijd had overschreden. Een hervalidatie met oorsprong resulteert in het cacheobject wordt vervangen door een nieuw antwoord van de oorsprong.
* TCP_EXPIRED _HIT: aangeboden via Edge na hervalidatie met oorsprong. Het object is in de cache, maar de maximumleeftijd had overschreden. Een opnieuw te worden gevalideerd met de oorspronkelijke server resulteert in het cacheobject wordt niet gewijzigd.
* Selecteer het datumbereik wilt weergeven van gegevens voor deze vandaag/week/deze maand, enzovoort of Voer de aangepaste datums en klik op **gaat** om ervoor te zorgen dat uw selectie wordt bijgewerkt.
* U kunt exporteren en downloaden van de gegevens door te klikken op het excel-blad pictogram naast **gaat**.

### <a name="full-list-of-cache-statuses"></a>Volledige lijst van cache statussen
* TCP_HIT - deze status wordt vermeld als een aanvraag naar de client rechtstreeks vanuit de pop-server is uitgevoerd. Een actief wordt onmiddellijk van een POP-geleverd wanneer het is in de cache op de pop-server die het dichtst bij de client en een geldige time-to-live (TTL) heeft. TTL wordt bepaald door de volgende antwoordheaders:
  
  * Cache-Control: s-maxage
  * Cache-Control: maximumleeftijd
  * Verloopt
* TCP_MISS - deze status geeft aan dat de versie van de aangevraagde asset een cache niet op de pop-server die het dichtst bij de client gevonden is. De asset is aangevraagd vanaf een bronserver of een schild bronserver. Als de bronserver of de oorsprong shield-server een asset retourneert, is geleverd aan de client en in de cache op zowel de client als de edge-server. Anders wordt een niet-200-statuscode (bijvoorbeeld 403 verboden of 404 niet gevonden) wordt geretourneerd.
* TCP_EXPIRED _HIT - deze status wordt vermeld als een aanvraag die gericht is op een activum met een verlopen TTL rechtstreeks vanuit de pop-server naar de client is uitgevoerd. Bijvoorbeeld, is wanneer de activa de maximale leeftijd verlopen. 
  
    Een verlopen aanvraag resulteert gewoonlijk in een validatieaanvraag met de oorspronkelijke server. Voor een _HIT TCP_EXPIRED om te worden uitgevoerd, moet de oorspronkelijke server aangeven dat een nieuwere versie van de asset niet bestaat. Deze situatie resulteert gewoonlijk in een update van de asset Cache-Control en Expires-koppen.
* TCP_EXPIRED _MISS - deze status wordt vermeld als een nieuwere versie van een verlopen in de cache asset van de pop-server naar de client wordt geleverd. Deze status wordt weergegeven wanneer de TTL voor een asset in de cache is verlopen (bijvoorbeeld verlopen maximumleeftijd) en de oorspronkelijke server retourneert een nieuwere versie van dat actief. Deze nieuwe versie van de asset is geleverd aan de client in plaats van de versie van de cache. Bovendien is het in cache op de edge-server en de client.
* CONFIG_NOCACHE - deze status geeft aan dat een configuratie klantspecifieke op onze rand POP voorkomen dat de asset worden opgeslagen.
* GEEN: deze status geeft aan dat een cache-inhoud nieuwheid-controle is niet uitgevoerd.
* TCP_ CLIENT_REFRESH _MISS - deze status wordt vermeld als een HTTP-client, zoals een browser, zorgt ervoor een edge pop-server dat voor het ophalen van een nieuwe versie van een verouderde actief op de bronserver.
  
    Standaard onze servers te voorkomen dat een HTTP-client afdwingen van onze randservers voor het ophalen van een nieuwe versie van de activa op de bronserver.
* TCP_ PARTIAL_HIT - deze status wordt vermeld als een aanvraag voor het bereik van byte in een treffer voor een asset gedeeltelijk in cache resulteert. Het aangevraagde bytebereik is onmiddellijk door de pop-server aan de client geleverd.
* UNCACHEABLE - deze status wordt vermeld als een asset Cache-Control en Expires-koppen aangeven dat deze mag niet worden opgeslagen op een pop-server of door de HTTP-client. Dergelijke aanvragen worden geleverd op de bronserver.

## <a name="cache-hit-ratio"></a>Verhouding tussen treffers in de cache
Dit rapport geeft het percentage van in cache aanvragen die zijn geleverd rechtstreeks vanuit de cache.

Het rapport bevat de volgende details:

* De gevraagde inhoud in cache is opgeslagen op de pop-server die het dichtst bij de aanvrager.
* De aanvraag is uitgevoerd rechtstreeks vanaf de rand van het netwerk.
* De aanvraag is niet moet worden gevalideerd met de bronserver.

Het rapport bevatten niet:

* Aanvragen die zijn geweigerd vanwege land opties voor het filteren.
* Aanvragen voor activa waarvan headers aangeven dat ze mag niet worden opgeslagen. Bijvoorbeeld, Cache-Control: persoonlijke, Cache-Control: Nee-cache of Pragma: Nee-cache-headers te voorkomen dat een actief worden opgeslagen.
* Byte-bereikaanvragen voor deels in cache opgeslagen inhoud.

De formule is: (TCP_ HIT / (TCP_ HIT + TCP_MISS)) * 100

* Selecteer het datumbereik wilt weergeven van gegevens voor deze vandaag/week/deze maand, enzovoort of Voer de aangepaste datums en klik op **gaat** om ervoor te zorgen dat uw selectie wordt bijgewerkt.
* U kunt exporteren en downloaden van de gegevens door te klikken op het excel-blad pictogram naast **gaat**.

![Rapport van de verhouding tussen treffers in de cache](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>IPv4/IPV6-gegevens die zijn overgedragen
Dit rapport toont de distributie van de informatie over het gebruik verkeer in IPV4 en IPV6.

![IPv4/IPV6-gegevens die zijn overgedragen](./media/cdn-reports/cdn-ipv4-ipv6.png)

* Selecteer het datumbereik weergeven van gegevens voor deze vandaag/week/deze maand, enzovoort of aangepaste datums invoeren.
* Klik vervolgens op **gaat** om ervoor te zorgen dat uw selectie wordt bijgewerkt.

## <a name="considerations"></a>Overwegingen
Rapporten kunnen alleen worden gegenereerd in de afgelopen 18 maanden.


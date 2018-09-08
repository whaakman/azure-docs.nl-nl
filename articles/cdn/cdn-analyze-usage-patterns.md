---
title: Verizon-rapporten Core | Microsoft Docs
description: 'U kunt gebruikspatronen voor uw CDN weergeven met behulp van de volgende rapporten: bandbreedte, gegevensoverdracht, treffers, Cachestatussen, Cache Hit Ratio, IPV4/IPV6-gegevens overgebracht.'
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: 5a0d9018-8bdb-48ff-84df-23648ebcf763
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: d10a40d03f0f76676e70afdec94e9adfaa0dd09f
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162067"
---
# <a name="core-reports-from-verizon"></a>Kernrapporten

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

U kunt met behulp van Verizon Core rapporten via de portal beheren voor Verizon-profielen, gebruikspatronen bekijken voor uw CDN met de volgende rapporten:

* Bandbreedte
* Overdracht van gegevens
* Treffers
* Statusbepaling van de cache
* Aantal cachetreffers
* IPv4/IPV6 gegevens die worden overgedragen

## <a name="accessing-verizon-core-reports"></a>Toegang tot Core Verizon-rapporten
1. Klik in de blade CDN-profiel op de **beheren** knop.
   
    ![Knop voor CDN-profiel beheren](./media/cdn-reports/cdn-manage-btn.png)
   
    De CDN-beheerportal wordt geopend.
2. Beweeg de muisaanwijzer over de **Analytics** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **Core rapporten** flyout. Klik op een rapport in het menu.
   
    ![CDN-beheerportal - Core Rapportmenu](./media/cdn-reports/cdn-core-reports.png)

3. Selecteer een datumbereik uit voor elk rapport de **datumbereik** lijst. U kunt bijvoorbeeld een vooraf gedefinieerde datumbereik selecteren **vandaag** of **deze Week**, of u kunt selecteren **aangepaste** handmatig te typen en een datumbereik door te klikken op de pictogrammen van de agenda. 

4. Nadat u een datumbereik hebt geselecteerd, klikt u op **gaat** om het rapport te genereren. 

4. Als u de gegevens in Excel-indeling exporteren wilt, klikt u op het Excel-pictogram boven de **gaat** knop.

## <a name="bandwidth"></a>Bandbreedte
De bandbreedte-rapport bestaat uit een grafiek en een gegevenstabel die aangeeft van het CDN-bandbreedtegebruik voor HTTP en HTTPS gedurende een bepaalde periode in Mbps. U kunt het bandbreedtegebruik weergeven over alle POP's of voor een bepaalde POP. Dit rapport kunt u om de pieken in het verkeer en de distributie voor POP's weer te geven.

Uit de **Edge-knooppunten** in de lijst met **alle Edge-knooppunten** verkeer van alle knooppunten weergeven of Selecteer een specifieke regio.

Het rapport wordt elke vijf minuten bijgewerkt.

![Bandbreedte-rapport](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Overdracht van gegevens
Dit rapport bestaat uit een grafiek en een gegevenstabel die aangeeft van het gebruik van CDN-verkeer voor HTTP en HTTPS gedurende een bepaalde periode in GB. U kunt het gebruik van verkeer tussen alle POP's of voor een bepaalde POP weergeven. Dit rapport kunt u om de pieken in het verkeer en de distributie in de POP's weer te geven.

Uit de **Edge-knooppunten** in de lijst met **alle Edge-knooppunten** verkeer van alle knooppunten weergeven of Selecteer een specifieke regio.

Het rapport wordt elke vijf minuten bijgewerkt.

![Overgedragen gegevens rapport](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>Treffers (statuscodes)
Dit rapport wordt de verdeling van de aanvraag-statuscodes voor uw inhoud beschreven. Elke aanvraag voor inhoud genereert een HTTP-statuscode. De statuscode wordt beschreven hoe edge POP's verwerkt de aanvraag. Een statuscode 2xx geeft bijvoorbeeld aan dat de aanvraag met succes is uitgevoerd naar een client, terwijl een statuscode 4xx geeft aan dat er een fout opgetreden. Zie voor meer informatie over HTTP-statuscodes [lijst van de HTTP-statuscodes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

![Treffers in rapport](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Statusbepaling van de cache
Dit rapport wordt de verdeling van de cachetreffers en missers in cache voor clientaanvragen beschreven. Omdat het resultaat is de snelste prestaties van treffers in cache, kunt u gegevens leveren snelheden optimaliseren door het minimaliseren van missers in cache en verlopen cachetreffers. 

Configureer uw oorspronkelijke server om te beperken van het gebruik van de volgende Cachemissers, verminderen: 
 * `no-cache` Antwoordheaders
 * Query-tekenreeks caching, tenzij strikt noodzakelijk is  
 * Niet gecachet kan worden responscodes

Als u wilt verkleinen verlopen cachetreffers, instellen van een asset `max-age` op een lange periode te minimaliseren van het aantal aanvragen naar de oorspronkelijke server.

![Cache statussen van rapport](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Belangrijkste cachestatussen zijn onder andere:
* TCP_HIT: Worden aangeleverd vanuit edge-server. Het object is in de cache en de max-age niet is overschreden.
* TCP_MISS: Worden aangeleverd vanuit de oorspronkelijke server. Het object is niet in de cache en het antwoord terug naar de oorsprong is.
* TCP_EXPIRED _MISS: aangeleverd vanuit de oorspronkelijke server na het opnieuw te worden gevalideerd met de oorsprong. Het object is in de cache, maar de max-age had overschreden. Een hervalidatie met oorsprong heeft geresulteerd in de cache-object wordt vervangen door een nieuw antwoord van de oorsprong.
* TCP_EXPIRED _HIT: na het opnieuw te worden gevalideerd met oorsprong worden aangeleverd vanuit Microsoft Edge. Het object is in de cache, maar de max-age had overschreden. Een hervalidatie met de oorspronkelijke server heeft geresulteerd in de cache-object wordt niet gewijzigd.

### <a name="full-list-of-cache-statuses"></a>Volledige lijst met cachestatussen van de
* TCP_HIT - deze status wordt gerapporteerd bij een aanvraag naar de client rechtstreeks vanuit de pop-server wordt uitgevoerd. Een asset is onmiddellijk worden aangeleverd vanuit een pop-server wanneer deze is in de cache op de pop-server die het dichtst bij de client en een geldige time-to-live (TTL). TTL-waarde wordt bepaald door de volgende antwoordheaders:
  
  * Cache-Control: s-maxage
  * Cache-Control: max-age
  * Verloopt op
* TCP_MISS: Deze status geeft aan dat de versie van een cache van de aangevraagde asset niet op de pop-server die het dichtst bij de client gevonden is. De asset is aangevraagd bij de oorspronkelijke server of in een oorspronkelijke shield-server. Als de oorspronkelijke server of de oorspronkelijke shield-server een asset retourneert, is geleverd aan de client en in de cache op zowel de client en de edge-server. Anders wordt een niet-200-statuscode (bijvoorbeeld: 403 verboden of 404 niet gevonden) geretourneerd.
* TCP_EXPIRED_HIT: Deze status wordt gerapporteerd bij een aanvraag die gericht is op een asset met een verlopen TTL rechtstreeks vanuit de pop-server naar de client is uitgevoerd. Bijvoorbeeld, is wanneer de activa de max-age verlopen. 
  
   Een verlopen aanvraag resulteert gewoonlijk in een validatieaanvraag naar de oorspronkelijke server. Voor de status van een TCP_EXPIRED_HIT optreden, moet de oorspronkelijke server aangeven dat een nieuwere versie van de asset niet bestaat. Deze situatie leidt meestal tot bijwerken van de Cache-Control en -Expires-koppen van de asset.
* TCP_EXPIRED_MISS: Deze status wordt gerapporteerd als een nieuwere versie van een verlopen in de cache asset is aangeleverd vanuit de pop-server naar de client. Deze status wordt weergegeven wanneer de TTL voor een asset in de cache is verlopen (bijvoorbeeld verlopen max-age) en de oorspronkelijke server retourneert een nieuwere versie van dat actief. Deze nieuwe versie van de asset is geleverd aan de client in plaats van de versie van de cache. Bovendien is het in cache opgeslagen op de edge-server en de client.
* CONFIG_NOCACHE: Deze status geeft aan dat een klant-specifieke configuratie rand van het pop-server niet meer aanmelden bij de asset worden opgeslagen.
* GEEN: deze status geeft aan dat een cache-inhoud webdocumenten controle niet is uitgevoerd.
* TCP_CLIENT_REFRESH_MISS: Deze status wordt gerapporteerd als een HTTP-client, zoals een browser, zorgt ervoor dat een rand pop-server een nieuwe versie van een verouderde asset ophalen uit de oorspronkelijke server. Standaard de servers te voorkomen dat een HTTP-client forceren van het edge-servers om op te halen van een nieuwe versie van de activa van de oorspronkelijke server.
* TCP_PARTIAL_HIT: Deze status wordt gerapporteerd bij een aanvraag voor het bereik van bytes in een kiest voor een asset gedeeltelijk in de cache resulteert. Het aangevraagde bytebereik is onmiddellijk worden aangeleverd vanuit de pop-server naar de client.
* UNCACHEABLE: Deze status wordt gerapporteerd bij van een asset `Cache-Control` en `Expires` headers aangeven dat deze moet niet worden opgeslagen in de cache op een pop-server of door de HTTP-client. Dergelijke aanvragen worden aangeleverd vanuit de oorspronkelijke server.

## <a name="cache-hit-ratio"></a>Aantal cachetreffers
Dit rapport geeft het percentage in de cache-aanvragen die zijn aangeleverd rechtstreeks vanuit de cache.

Het rapport bevat de volgende details:

* De gevraagde inhoud in cache is opgeslagen op de pop-server die het dichtst bij de aanvrager.
* De aanvraag is uitgevoerd rechtstreeks vanaf de rand van ons netwerk.
* De aanvraag is niet opnieuw moet worden gevalideerd met de oorspronkelijke server.

Het rapport bevat geen:

* Aanvragen die zijn geweigerd vanwege land opties voor het filteren.
* Aanvragen voor activa waarvan headers aangeven dat ze moeten niet worden opgeslagen in de cache. Bijvoorbeeld, `Cache-Control: private`, `Cache-Control: no-cache`, of `Pragma: no-cache` headers te voorkomen dat een actief in de cache worden opgeslagen.
* Byte-bereikaanvragen voor gedeeltelijk in de cache-inhoud.

De formule is: (TCP_ HIT / (TCP_ TREFFERS + TCP_MISS)) * 100

![Rapport van de verhouding tussen treffers in cache](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>IPv4/IPV6-gegevensoverdracht
Dit rapport geeft de distributie van verkeer gebruik in IPV4 en IPV6.

![IPv4/IPV6-gegevensoverdracht](./media/cdn-reports/cdn-ipv4-ipv6.png)

## <a name="considerations"></a>Overwegingen
Rapporten kunnen alleen worden gegenereerd in de afgelopen 18 maanden.


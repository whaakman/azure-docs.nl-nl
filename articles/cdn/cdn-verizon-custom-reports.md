---
title: Aangepaste rapporten van Verizon | Microsoft Docs
description: 'U kunt gebruikspatronen voor uw CDN bekijken met behulp van de volgende rapporten: bandbreedte, de hoeveelheid overgedragen gegevens, treffers, Cache statussen, Cache Hit Ratio, IPV4/IPV6-gegevens overgebracht.'
services: cdn
documentationcenter: 
author: dksimpson
manager: 
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: v-deasim
ms.openlocfilehash: f09195dc07a96ebcca7f7a9e4bcf521fae13630c
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="custom-reports-from-verizon"></a>Aangepaste rapporten van Verizon

[!INCLUDE[cdn-verizon-only](../../includes/cdn-verizon-only.md)]

U kunt met behulp van Verizon aangepaste rapporten via de portal beheren voor Verizon profielen, het type gegevens moeten worden verzameld voor rand CNAMEs rapporten definiëren.


## <a name="accessing-verizon-custom-reports"></a>Openen van Verizon aangepaste rapporten
1. Klik in de blade CDN-profiel op de **beheren** knop.
   
    ![Knop voor CDN-profiel beheren](./media/cdn-reports/cdn-manage-btn.png)
   
    Hiermee opent u de CDN-beheerportal.
2. Beweeg de muisaanwijzer over de **Analytics** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **aangepaste rapporten** doel. Klik op **CNAME-records van de rand**.
   
    ![CDN-beheerportal - aangepaste rapporten menu](./media/cdn-reports/cdn-custom-reports.png)

## <a name="edge-cnames-custom-report"></a>Rand CNAMES aangepast rapport
Het aangepaste CNAMES Edge-rapport biedt treffers en statistieken overgedragen gegevens voor rand CNAMEs waarop aangepast rapport logboekregistratie is ingeschakeld. Rand CNAME-records bestaan uit Azure CDN-eindpunt hostnamen en eventuele bijbehorende aangepast domein hostnamen. 

Aangepast rapport gegevensregistratie begint één uur nadat u een edge-CNAME aangepaste rapportagemogelijkheid inschakelen. U kunt rapportgegevens weergeven door het genereren van een rapport rand CNAME-records voor een specifiek platform of voor alle platforms. Het bereik voor dit rapport is beperkt tot de rand CNAMEs waarvoor aangepast rapportgegevens zijn verzameld tijdens de opgegeven periode. De rand CNAMEs rapport bestaat uit een grafiek en een gegevenstabel voor de 10 bovenrand CNAMEs volgens de waarde die is gedefinieerd in de optie metrische gegevens. 

Een aangepast rapport genereren met het definiëren van de volgende opties:

- Metrische gegevens: De volgende opties worden ondersteund:

   - Treffers: Geeft het totale aantal aanvragen die worden doorgestuurd naar een edge-CNAME waarop de aangepaste rapportagemogelijkheid is ingeschakeld. Met deze metriek omvat geen de statuscode geretourneerd naar de client.

   - De overgedragen gegevens: Geeft de totale hoeveelheid gegevens overgedragen van de edge-servers aan de HTTP-clients (bijvoorbeeld webbrowsers) voor aanvragen die worden doorgestuurd naar een edge-CNAME waarop de aangepaste rapportagemogelijkheid is ingeschakeld. De hoeveelheid gegevens die overgedragen wordt berekend door HTTP-antwoordheaders toe te voegen aan de hoofdtekst van het antwoord. Als gevolg hiervan is de hoeveelheid gegevens overgedragen voor elk actief groter is dan de werkelijke bestandsgrootte.

- Groeperingen: Bepaalt het type statistieken die worden weergegeven onder het staafdiagram. De volgende opties worden ondersteund:

   - Codes voor HTTP-antwoord: Statistieken ordent door HTTP-antwoordcode (bijvoorbeeld, 200, 403, etc.) geretourneerd naar de client. 

   - Status van de cache: Ordent statistieken door de status van de cache.


Als u wilt het datumbereik voor het rapport instellen, u kunt ook selecteren een vooraf gedefinieerde datumbereik, zoals **vandaag** of **deze Week**, uit de vervolgkeuzelijst of u kunt selecteren **aangepaste** en Voer handmatig een datumbereik door te klikken op de kalenderpictogrammen. 

Nadat u het datumbereik hebt geselecteerd, klikt u op **gaat** om het rapport te genereren.

U kunt de gegevens in Excel-indeling exporteren door te klikken op het symbool Excel rechts van de **gaat** knop.

![CNAME-records rapport](./media/cdn-reports/cdn-cnames-report.png)

## <a name="edge-cnames-custom-report-fields"></a>Rand CNAMES aangepast rapportvelden

| Veld                     | Beschrijving   |
|---------------------------|---------------|
| 2xx                       | Geeft het totale aantal aanvragen of overgedragen gegevens (MB) voor de rand CNAME die in een 2xx HTTP-statuscode resulteert (bijvoorbeeld 200 OK). |
| 3xx                       | Geeft het totale aantal aanvragen of overgedragen gegevens (MB) voor de rand CNAME die in een 3xx HTTP-statuscode resulteert (bijvoorbeeld 302 gevonden of 304 niet gewijzigd. |
| 4XX                       | Geeft het totale aantal aanvragen of overgedragen gegevens (MB) voor de rand CNAME die in een 4xx HTTP-statuscode resulteert (bijvoorbeeld: 400 onjuiste aanvraag, 403 verboden of 404 niet gevonden). |
| 5XX                       | Geeft het totale aantal aanvragen of gegevens die overgedragen (MB) voor de rand CNAME die in een 5xx HTTP-statuscode (bijvoorbeeld 500 Interne serverfout of 502 Ongeldige Gateway resulteert). |
| Cache Hit %               | Geeft het percentage van caching geschikte aanvragen die aan de aanvrager rechtstreeks uit de cache zijn geleverd. |
| Cachetreffers                | Geeft het totale aantal aanvragen of overgedragen gegevens (MB) voor de rand CNAME die in een cachetreffer (bijvoorbeeld TCP_EXPIRED_HIT, TCP_HIT of TCP_PARTIAL_HIT resulteert). Een cachetreffer treedt op wanneer de versie van een cache van de aangevraagde inhoud is gevonden. |
| Gegevensoverdracht (MB)     | Geeft de totale hoeveelheid gegevens overgedragen (MB) van de randservers naar HTTP-clients (webbrowsers) voor de CNAME van de rand aan. De hoeveelheid gegevens die overgedragen wordt berekend door de HTTP-antwoordheaders toe te voegen aan de hoofdtekst van het antwoord. Als gevolg hiervan is de hoeveelheid gegevens overgedragen voor elk actief groter is dan de werkelijke bestandsgrootte. |
| Beschrijving               | Identificeert een edge CNAME door de hostnaam |
| Treffers                      | Geeft het totale aantal aanvragen naar de rand CNAME |
| Missers                    | Geeft het totale aantal aanvragen of overgedragen gegevens (MB) voor de rand CNAME die in een cache ontbreekt, (bijvoorbeeld TCP_CLIENT_REFRESH_MISS, TCP_EXPIRED_MISS of TCP_MISS resulteert). Een cache ontbreekt treedt op wanneer de aangevraagde inhoud is niet in de cache opgeslagen op de edge-server die de aanvraag gehonoreerd. | 
| Geen Cache                  | Geeft het totale aantal aanvragen of overgedragen gegevens (MB) voor de rand CNAME die in een statuscode CONFIG_NOCACHE-cache resulteert.  |
| Overige                     | Geeft het totale aantal aanvragen of gegevens die worden verzonden (MB) voor de rand CNAME aangegeven die resulteert in een HTTP-statuscode die buiten de 2xx - 5xx-bereik valt. |
| Platform                  | Hiermee geeft u het platform dat verkeer van de edge-CNAME verwerkt. |
| Niet-toegewezen               | Geeft het totale aantal aanvragen of gegevens die overgedragen (MB) voor de edge-CNAME voor welke statuscode van de cache of HTTP-statuscode informatie niet is geregistreerd.  |
| Uncacheable               | Geeft het totale aantal aanvragen of overgedragen gegevens (MB) voor de rand CNAME die in een statuscode UNCACHEABLE cache resulteert.  |


## <a name="considerations"></a>Overwegingen
Rapporten kunnen alleen binnen de laatste 18 maanden worden gegenereerd.


---
title: Aangepaste Verizon-rapporten | Microsoft Docs
description: 'U kunt gebruikspatronen voor uw CDN weergeven met behulp van de volgende rapporten: bandbreedte, gegevensoverdracht, treffers, Cachestatussen, Cache Hit Ratio, IPV4/IPV6-gegevens overgebracht.'
services: cdn
documentationcenter: ''
author: dksimpson
manager: ''
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: v-deasim
ms.openlocfilehash: f18b6edb900640e48e5ca87639f71cec9287af38
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44159738"
---
# <a name="custom-reports-from-verizon"></a>Aangepaste Verizon-rapporten

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Met behulp van Verizon aangepaste rapporten via de portal beheren voor Verizon-profielen, kunt u het type gegevens moeten worden verzameld voor edge CNAME-records rapporten definiëren.


## <a name="accessing-verizon-custom-reports"></a>Aangepaste Verizon-rapporten openen
1. Klik in de blade CDN-profiel op de **beheren** knop.
   
    ![Knop voor CDN-profiel beheren](./media/cdn-reports/cdn-manage-btn.png)
   
    De CDN-beheerportal wordt geopend.
2. Beweeg de muisaanwijzer over de **Analytics** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **aangepaste rapporten** flyout. Klik op **CNAME-records van de rand**.
   
    ![CDN-beheerportal - aangepaste rapporten menu](./media/cdn-reports/cdn-custom-reports.png)

## <a name="edge-cnames-custom-report"></a>Edge CNAME-records aangepast rapport
Het aangepaste rapport Edge CNAME's biedt treffers en overgedragen gegevens statistieken voor edge CNAME-records die aangepaste rapport logboekregistratie is ingeschakeld. Edge CNAME's bestaan uit Azure CDN-eindpunt hostnamen en geen hostnaam gekoppeld aangepast domein. 

Aangepast rapport gegevensregistratie begint één uur nadat u de aangepaste rapportagefunctionaliteit een edge-CNAME hebt ingeschakeld. U kunt rapportgegevens weergeven door het genereren van een rapport Edge CNAME-records voor een specifiek platform of voor alle platforms. De dekking voor dit rapport is beperkt tot het edge-CNAME's waarvoor aangepaste rapportgegevens zijn verzameld tijdens de opgegeven periode. Rand van het CNAME-records rapport bestaat uit een grafiek en een gegevenstabel voor de 10 bovenrand CNAME-records op basis van de waarde die is gedefinieerd in de optie voor metrische gegevens. 

Een aangepast rapport genereren met het definiëren van de volgende opties:

- Metrische gegevens: De volgende opties worden ondersteund:

   - Treffers: Geeft het totale aantal aanvragen die worden doorgestuurd naar een edge-CNAME waarop de aangepaste rapportagefunctionaliteit is ingeschakeld. Met deze metriek omvat niet de statuscode geretourneerd naar de client.

   - De overgedragen gegevens: Geeft de totale hoeveelheid gegevens die van de edge-servers worden overgedragen naar de HTTP-clients (bijvoorbeeld webbrowsers) voor aanvragen die worden doorgestuurd naar een edge-CNAME waarop de aangepaste rapportagefunctionaliteit is ingeschakeld. De hoeveelheid overgedragen gegevens wordt berekend door het HTTP-antwoordheaders toe te voegen aan de antwoordtekst. Als gevolg hiervan is de hoeveelheid gegevens die worden overgedragen voor elke asset groter is dan de werkelijke bestandsgrootte.

- Groeperingen: Bepaalt het type van de statistieken die worden weergegeven onder het staafdiagram. De volgende opties worden ondersteund:

   - HTTP-responscodes: Organiseert statistieken op HTTP-responscode (bijvoorbeeld 200, 403, etc.) geretourneerd naar de client. 

   - Status van de cache: Organiseert statistieken met de status van de cache.


Om in te stellen het datumbereik voor het rapport, u kunt selecteren een vooraf gedefinieerde datumbereik, zoals **vandaag** of **deze Week**, uit de vervolgkeuzelijst of u kunt selecteren **aangepaste** en Voer handmatig een datumbereik door te klikken op de pictogrammen van de agenda. 

Nadat u het datumbereik hebt geselecteerd, klikt u op **gaat** om het rapport te genereren.

U kunt de gegevens in Excel-indeling exporteren door te klikken op het Excel-symbool aan de rechterkant van de **gaat** knop.

![Rapport van CNAME-records](./media/cdn-reports/cdn-cnames-report.png)

## <a name="edge-cnames-custom-report-fields"></a>Edge CNAME-records aangepaste rapportvelden

| Veld                     | Beschrijving   |
|---------------------------|---------------|
| 2xx                       | Geeft het totale aantal aanvragen of overgedragen gegevens (MB) voor de rand CNAME die in een 2xx HTTP-statuscode resulteert (bijvoorbeeld 200 OK). |
| 3xx                       | Geeft het totale aantal aanvragen of overgedragen gegevens (MB) voor de rand CNAME die in een 3xx HTTP-statuscode resulteert (bijvoorbeeld 302 gevonden of 304 niet gewijzigd. |
| 4XX                       | Geeft het totale aantal aanvragen of overgedragen gegevens (MB) voor de rand CNAME die in een HTTP-statuscode 4xx resulteert (bijvoorbeeld 400 Ongeldige aanvraag, 403 verboden of 404 niet gevonden). |
| 5XX                       | Geeft het totale aantal aanvragen of overgedragen gegevens (MB) voor de rand CNAME die in een 5xx HTTP-statuscode (bijvoorbeeld 500 Interne serverfout of 502 Ongeldige Gateway resulteert). |
| Percentage treffers in cache               | Geeft het percentage van gecachet kan worden aanvragen die zijn behandeld rechtstreeks uit de cache aan de aanvrager. |
| Cachetreffers                | Geeft het totale aantal aanvragen of overgedragen gegevens (MB) voor de rand CNAME die in een cache-hits (bijvoorbeeld TCP_EXPIRED_HIT, TCP_HIT of TCP_PARTIAL_HIT resulteert). Een cache-hits treedt op wanneer de versie van een cache van de aangevraagde inhoud is gevonden. |
| Overgedragen gegevens (MB)     | Geeft de totale hoeveelheid gegevens overgedragen (MB) van het edge-servers naar HTTP-clients (webbrowsers) voor de CNAME van de rand aan. De hoeveelheid overgedragen gegevens wordt berekend door de HTTP-antwoordheaders toe te voegen aan de antwoordtekst. Als gevolg hiervan is de hoeveelheid gegevens die worden overgedragen voor elke asset groter is dan de werkelijke bestandsgrootte. |
| Beschrijving               | Identificeert een rand CNAME door de hostnaam |
| Treffers                      | Geeft het totale aantal aanvragen naar de rand CNAME |
| Missers                    | Geeft het totale aantal aanvragen of overgedragen gegevens (MB) voor de rand CNAME die in een cache ontbreekt (bijvoorbeeld TCP_CLIENT_REFRESH_MISS, TCP_EXPIRED_MISS of TCP_MISS resulteert). Een cache ontbreekt treedt op wanneer de aangevraagde inhoud is niet in de cache opgeslagen op het edge-server die de aanvraag gehonoreerd. | 
| Geen Cache                  | Geeft het totale aantal aanvragen of overgedragen gegevens (MB) voor de rand CNAME die in een CONFIG_NOCACHE cache-statuscode resulteert.  |
| Overige                     | Geeft het totale aantal aanvragen of gegevens die worden overgedragen (MB) voor de rand CNAME aangegeven die resulteert in een HTTP-statuscode die buiten het 2xx - bereik 5xx valt. |
| Platform                  | Geeft aan dat het platform die verantwoordelijk is voor de edge-CNAME-verkeer. |
| Niet-toegewezen               | Geeft het totale aantal aanvragen of overgedragen gegevens (MB) voor de edge-CNAME voor welke cache statuscode of HTTP-statuscode informatie niet is geregistreerd.  |
| Uncacheable               | Geeft het totale aantal aanvragen of overgedragen gegevens (MB) voor de rand CNAME die in een statuscode UNCACHEABLE cache resulteert.  |


## <a name="considerations"></a>Overwegingen
Rapporten kunnen alleen in de afgelopen 18 maanden worden gegenereerd.


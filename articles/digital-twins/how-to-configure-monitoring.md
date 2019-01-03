---
title: Bewaking in digitale dubbels voor Azure configureren | Microsoft Docs
description: Hoe u bewaking in Azure, digitale dubbels configureren.
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/26/2018
ms.author: adgera
ms.custom: seodec18
ms.openlocfilehash: 2749a5c6c4e6003c51523d83c46b48d3b55b3d45
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/28/2018
ms.locfileid: "53807581"
---
# <a name="how-to-configure-monitoring-in-azure-digital-twins"></a>Bewaking in Azure, digitale dubbels configureren

Azure van digitale dubbels biedt ondersteuning voor robuuste logboekregistratie, controle en analyse. Oplossingen voor ontwikkelaars kunnen Azure Log Analytics, diagnostische logboeken, activiteit logboekregistratie en andere services gebruiken voor de ondersteuning van de behoeften van complexe bewaking van een IoT-app. Opties voor logboekregistratie kunnen worden gecombineerd om opvragen of records weergeven binnen diverse services en om gedetailleerde logboekregistratie dekking voor veel services.

In dit artikel bevat een overzicht van logboekregistratie en bewaking opslagopties en informatie over om ze te combineren in manieren van specifieke digitale dubbels van Azure.

## <a name="review-activity-logs"></a>Activiteitenlogboeken bekijken

Azure [activiteitenlogboeken](../azure-monitor/platform/activity-logs-overview.md) bieden snel inzicht krijgen in de geschiedenis gebeurtenis en de bewerking op abonnementsniveau voor elke Azure-service-exemplaar.

Gebeurtenissen op abonnementsniveau zijn onder andere:

* Het maken van resources
* Resource verwijderen
* Het maken van appgeheimen
* Integreren met andere services

Activiteit logboekregistratie voor digitale dubbels van Azure is standaard ingeschakeld en kunt u vinden via de Azure-portal door:

1. Uw digitale dubbels Azure-exemplaar selecteren.
1. Kiezen **activiteitenlogboek** om het deelvenster weergeven:

    ![Activiteitenlogboek][1]

Voor geavanceerde logboekregistratie voor activiteit:

1. Selecteer de **logboeken** optie om weer te geven de **overzicht voor analyse van activiteit**:

    ![Selectie][2]

1. De **overzicht voor analyse van activiteit** bevat een overzicht van gegevens van een essentiële activiteitenlogboek:

    ![Overzicht voor analyse van activiteit][3]

>[!TIP]
>Gebruik **activiteitenlogboeken** snel inzicht krijgen in gebeurtenissen op abonnementsniveau.

## <a name="enable-customer-diagnostic-logs"></a>Klant logboeken met diagnostische gegevens inschakelen

Azure [diagnostische instellingen](../azure-monitor/platform/diagnostic-logs-overview.md) kan worden ingesteld voor elke Azure-instantie om te voorzien in activiteitenregistratie. Activiteitenlogboeken hebben betrekking op gebeurtenissen op abonnementsniveau, biedt registratie in diagnoselogboek inzicht in de operationele geschiedenis van de resources zelf.

Voorbeelden van het vastleggen van diagnostische gegevens zijn:

* De uitvoeringstijd voor een gebruiker gedefinieerde functie
* De responscode van de status van een geslaagd API-aanvraag
* Het ophalen van een app-sleutel van een kluis

Diagnostische logboeken voor een exemplaar inschakelen:

1. Open de resource in Azure portal.
1. Klik op **diagnostische instellingen**:

    ![Diagnostische instellingen][4]

1. Klik op **diagnostische gegevens inschakelen** voor het verzamelen van gegevens (indien niet eerder ingeschakeld).
1. Vul de vereiste velden in en selecteer hoe en waar gegevens worden opgeslagen:

    ![Diagnostische instellingen twee][5]

    Diagnostische logboeken zijn vaak opgeslagen met [Azure File Storage](../storage/files/storage-files-deployment-guide.md) en gedeeld met [Azure Log Analytics](../azure-monitor/log-query/get-started-portal.md). Beide opties kunnen worden geselecteerd.

>[!TIP]
>Gebruik **diagnostische logboeken** voor inzicht in bewerkingen van resources.

## <a name="azure-monitor-and-log-analytics"></a>Azure monitor en log analytics

IoT-toepassingen aandragen voor de verschillende bronnen, apparaten, locaties en gegevens in één. Uiterst gedetailleerde logboekregistratie biedt gedetailleerde informatie over elke specifiek, een service of een onderdeel van de algehele architectuur, maar een uniform overzicht is vaak vereist voor onderhoud en foutopsporing.

Azure Monitor omvat de krachtige Log Analytics-service, waardoor bronnen logboekregistratie worden bekeken en geanalyseerd op één locatie. Azure Monitor is dus zeer nuttig voor het analyseren van Logboeken in geavanceerde IoT-apps.

Voorbeelden van het gebruik zijn:

* Uitvoeren van query's meerdere geschiedenis van diagnostische logboeken
* Logboeken voor verschillende door de gebruiker gedefinieerde functies bekijken
* Logboeken voor twee of meer services weergeven binnen een bepaalde periode

Volledige logboek uitvoeren van query's wordt geboden via [Azure Log Analytics](../azure-monitor/log-query/log-query-overview.md). Om in te stellen deze krachtige functies:

1. Zoeken naar **Log Analytics** in Azure portal.
1. U ziet uw beschikbare **Log Analytics** exemplaren. Kies een en selecteer **logboeken** query:

    ![Log Analytics][6]

1. Als u nog geen een **Log Analytics** exemplaar, kunt u een werkruimte maken door te klikken op de **toevoegen** knop:

    ![OMS maken][7]

Zodra uw **Log Analytics** exemplaar is ingericht, mag u krachtige query's naar items zoeken in Logboeken veelvouden of zoeken met behulp van specifieke criteria, met behulp van **Log Management**:

   ![Logboekbeheer][8]

Zie voor meer informatie over krachtige querybewerkingen [aan de slag met query's](../azure-monitor/log-query/get-started-queries.md).

> [!NOTE]
> Een vertraging van 5 minuten kunnen optreden bij het verzenden van gebeurtenissen naar **Log Analytics** voor de eerste keer.

Azure Log Analytics biedt ook krachtige fout en waarschuwingsmeldingen services, dat kunnen worden weergegeven door te klikken op **vaststellen en oplossen van problemen met**:

   ![Waarschuwing en fout-meldingen][9]

>[!TIP]
>Gebruik **Log Analytics** naar query log geschiedenisgegevens voor meerdere app-functies, abonnementen of -services.

## <a name="other-options"></a>Andere opties

Azure van digitale dubbels biedt ook ondersteuning voor toepassingsspecifieke logboekregistratie en beveiligingscontrole. Zie voor een uitgebreide overzicht van alle opties voor logboekregistratie van Azure beschikbaar is voor uw Azure digitale Twins-exemplaar, de [Azure log audit](../security/azure-log-audit.md) artikel.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over Azure [activiteitenlogboeken](../azure-monitor/platform/activity-logs-overview.md).

- Duik dieper in Azure diagnostische instellingen lezen een [overzicht van diagnostische logboeken](../azure-monitor/platform/diagnostic-logs-overview.md).

- Meer informatie over [Azure Log Analytics](../azure-monitor/log-query/get-started-portal.md).

<!-- Images -->
[1]: media/how-to-configure-monitoring/activity-log.png
[2]: media/how-to-configure-monitoring/activity-log-select.png
[3]: media/how-to-configure-monitoring/log-analytics-overview.png
[4]: media/how-to-configure-monitoring/diagnostic-settings-one.png
[5]: media/how-to-configure-monitoring/diagnostic-settings-two.png
[6]: media/how-to-configure-monitoring/log-analytics.png
[7]: media/how-to-configure-monitoring/log-analytics-oms.png
[8]: media/how-to-configure-monitoring/log-analytics-management.png
[9]: media/how-to-configure-monitoring/log-analytics-notifications.png

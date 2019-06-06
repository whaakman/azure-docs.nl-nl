---
title: Bewaking in digitale dubbels voor Azure configureren | Microsoft Docs
description: Hoe u bewaking in Azure, digitale dubbels configureren.
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 06/03/2018
ms.author: v-adgera
ms.custom: seodec18
ms.openlocfilehash: 9765af13a22a0ae9db8a45c396e7d5a32b71b79c
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66730323"
---
# <a name="how-to-configure-monitoring-in-azure-digital-twins"></a>Bewaking in Azure, digitale dubbels configureren

Azure van digitale dubbels biedt ondersteuning voor robuuste logboekregistratie, controle en analyse. Oplossingen voor ontwikkelaars kunnen Azure Monitor-Logboeken, diagnostische logboeken, activiteit logboekregistratie en andere services gebruiken voor de ondersteuning van de behoeften van complexe bewaking van een IoT-app. Opties voor logboekregistratie kunnen worden gecombineerd om opvragen of records weergeven binnen diverse services en om gedetailleerde logboekregistratie dekking voor veel services.

In dit artikel bevat een overzicht van logboekregistratie en bewaking opslagopties en informatie over om ze te combineren in manieren van specifieke digitale dubbels van Azure.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

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

    [![Activiteitenlogboek](media/how-to-configure-monitoring/activity-log.png)](media/how-to-configure-monitoring/activity-log.png#lightbox)

Voor geavanceerde logboekregistratie voor activiteit:

1. Selecteer de **logboeken** optie om weer te geven de **overzicht voor analyse van activiteit**:

    [![Selectie](media/how-to-configure-monitoring/activity-log-select.png)](media/how-to-configure-monitoring/activity-log-select.png#lightbox)

1. De **overzicht voor analyse van activiteit** bevat een overzicht van gegevens van een essentiële activiteitenlogboek:

    [![Overzicht voor analyse van activiteit]( media/how-to-configure-monitoring/log-analytics-overview.png)]( media/how-to-configure-monitoring/log-analytics-overview.png#lightbox)

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

    [![Diagnostische instellingen](media/how-to-configure-monitoring/diagnostic-settings-one.png)](media/how-to-configure-monitoring/diagnostic-settings-one.png#lightbox)

1. Klik op **diagnostische gegevens inschakelen** voor het verzamelen van gegevens (indien niet eerder ingeschakeld).
1. Vul de vereiste velden in en selecteer hoe en waar gegevens worden opgeslagen:

    [![Diagnostische instellingen twee](media/how-to-configure-monitoring/diagnostic-settings-two.png)](media/how-to-configure-monitoring/diagnostic-settings-two.png#lightbox)

    Diagnostische logboeken zijn vaak opgeslagen met [Azure File Storage](../storage/files/storage-files-deployment-guide.md) en gedeeld met [logboeken van Azure Monitor](../azure-monitor/log-query/get-started-portal.md). Beide opties kunnen worden geselecteerd.

>[!TIP]
>Gebruik **diagnostische logboeken** voor inzicht in bewerkingen van resources.

## <a name="azure-monitor-and-log-analytics"></a>Azure monitor en log analytics

IoT-toepassingen aandragen voor de verschillende bronnen, apparaten, locaties en gegevens in één. Uiterst gedetailleerde logboekregistratie biedt gedetailleerde informatie over elke specifiek, een service of een onderdeel van de algehele architectuur, maar een uniform overzicht is vaak vereist voor onderhoud en foutopsporing.

Azure Monitor omvat de krachtige log analytics-service, waarmee de logboekregistratie van bronnen om te worden bekeken en geanalyseerd op één locatie. Azure Monitor is dus zeer nuttig voor het analyseren van Logboeken in geavanceerde IoT-apps.

Voorbeelden van het gebruik zijn:

* Uitvoeren van query's meerdere geschiedenis van diagnostische logboeken
* Logboeken voor verschillende door de gebruiker gedefinieerde functies bekijken
* Logboeken voor twee of meer services weergeven binnen een bepaalde periode

Volledige logboek uitvoeren van query's wordt geboden via [logboeken van Azure Monitor](../azure-monitor/log-query/log-query-overview.md). Om in te stellen deze krachtige functies:

1. Zoeken naar **Log Analytics** in Azure portal.
1. U ziet uw beschikbare **Log Analytics-werkruimte** exemplaren. Kies een en selecteer **logboeken** query:

    [![Log analytics](media/how-to-configure-monitoring/log-analytics.png)](media/how-to-configure-monitoring/log-analytics.png#lightbox)

1. Als u nog geen een **Log Analytics-werkruimte** exemplaar, kunt u een werkruimte maken door te klikken op de **toevoegen** knop:

    [![OMS maken](media/how-to-configure-monitoring/log-analytics-oms.png)](media/how-to-configure-monitoring/log-analytics-oms.png#lightbox)

Zodra uw **Log Analytics-werkruimte** exemplaar is ingericht, mag u krachtige query's naar items zoeken in Logboeken veelvouden of zoeken met behulp van specifieke criteria, met behulp van **Log Management**:

   [![Logboekbeheer](media/how-to-configure-monitoring/log-analytics-management.png)](media/how-to-configure-monitoring/log-analytics-management.png#lightbox)

Zie voor meer informatie over krachtige querybewerkingen [aan de slag met query's](../azure-monitor/log-query/get-started-queries.md).

> [!NOTE]
> Een vertraging van 5 minuten kunnen optreden bij het verzenden van gebeurtenissen naar **Log Analytics-werkruimte** voor de eerste keer.

Azure Monitor-logboeken bieden ook krachtige fout en waarschuwingsmeldingen services, dat kunnen worden weergegeven door te klikken op **vaststellen en oplossen van problemen met**:

   [![Waarschuwing en fout-meldingen](media/how-to-configure-monitoring/log-analytics-notifications.png)](media/how-to-configure-monitoring/log-analytics-notifications.png#lightbox)

>[!TIP]
>Gebruik **Log Analytics-werkruimte** naar query log geschiedenisgegevens voor meerdere app-functies, abonnementen of -services.

## <a name="other-options"></a>Andere opties

Azure van digitale dubbels biedt ook ondersteuning voor toepassingsspecifieke logboekregistratie en beveiligingscontrole. Zie voor een uitgebreide overzicht van alle opties voor logboekregistratie van Azure beschikbaar is voor uw Azure digitale Twins-exemplaar, de [Azure log audit](../security/azure-log-audit.md) artikel.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over Azure [activiteitenlogboeken](../azure-monitor/platform/activity-logs-overview.md).

- Duik dieper in Azure diagnostische instellingen lezen een [overzicht van diagnostische logboeken](../azure-monitor/platform/diagnostic-logs-overview.md).

- Meer informatie over [logboeken van Azure Monitor](../azure-monitor/log-query/get-started-portal.md).

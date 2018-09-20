---
title: Beveiliging configureren om te openen en beheren van Azure Time Series Insights | Microsoft Docs
description: In dit artikel wordt beschreven hoe u beveiliging en machtigingen configureren als toegang tot de beleidsregels en data access-beleid voor het beveiligen van Azure Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/15/2017
ms.openlocfilehash: 7cb5dc5b170103f98d56abc920f36dd85f855961
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364808"
---
# <a name="grant-data-access-to-a-time-series-insights-environment-using-azure-portal"></a>Gegevenstoegang verlenen tot een Time Series Insights-omgeving met Azure Portal

Time Series Insights-omgevingen hebben twee onafhankelijke typen toegangsbeleid:

* Beleid voor beheertoegang
* Beleid voor gegevenstoegang

Beide soorten beleid verlenen Azure Active Directory-principals (gebruikers en apps) verschillende machtigingen voor een specifieke omgeving. De principals (gebruikers en apps) moeten behoren tot de active directory (ook wel de Azure-tenant) die zijn gekoppeld aan het abonnement met de omgeving.

Beleid voor beheertoegang verleent machtigingen die betrekking hebben op de configuratie van de omgeving, zoals
*   maken en verwijderen van de omgeving, gebeurtenisbronnen, referentiegegevenssets, en
*   beheer van het beleid voor gegevenstoegang.

Beleid voor gegevenstoegang verleent machtigingen voor het uitvoeren van gegevensquery's, het bewerken van referentiegegevens in de omgeving en het delen van opgeslagen query's en perspectieven die aan de omgeving zijn gekoppeld.

De twee typen beleid maken een duidelijke scheiding mogelijk tussen toegang tot het beheer van de omgeving en toegang tot de gegevens in de omgeving. Het is bijvoorbeeld mogelijk het instellen van een omgeving zodanig dat de eigenaar/maker van de omgeving is verwijderd uit de toegang tot gegevens. Bovendien gebruikers en services die zijn toegestaan voor het lezen van gegevens van de omgeving mogelijk geen toegang worden verleend aan de configuratie van de omgeving.

[!INCLUDE [iot-tsi-data-access](../../includes/iot-tsi-data-access.md)]

## <a name="next-steps"></a>Volgende stappen

* Informatie over [een Event Hub-gebeurtenisbron toevoegen aan uw Azure Time Series Insights-omgeving](time-series-insights-how-to-add-an-event-source-eventhub.md).
* [Gebeurtenissen verzenden](time-series-insights-send-events.md) naar de gebeurtenisbron.
* Uw omgeving bekijken in [Verkenner van Time Series Insights](https://insights.timeseries.azure.com).

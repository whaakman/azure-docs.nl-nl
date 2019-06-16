---
title: Servicestatusmeldingen bekijken met de Azure-portal
description: Servicestatusmeldingen kunnen u berichten van health-service gepubliceerd door Microsoft Azure weergeven.
author: dkamstra
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 4/12/2018
ms.author: dukek
ms.subservice: logs
ms.openlocfilehash: 8cf8c3eb86f55b5595ef9a1af83ea50580bf638b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67069356"
---
# <a name="view-service-health-notifications-by-using-the-azure-portal"></a>Servicestatusmeldingen bekijken met de Azure-portal

Servicestatusmeldingen worden gepubliceerd door de Azure-infrastructuur. Ze bevatten informatie over de resources in uw abonnement. Deze meldingen zijn een subklasse van de activiteit van gebeurtenissen, en dus ook te vinden in het activiteitenlogboek. Servicestatusmeldingen zijn informatieve en bruikbare, afhankelijk van de klasse.

Zie voor meer informatie over de verschillende soorten servicestatusmeldingen [Service-eigenschappen van de gezondheid van meldingen](../../service-health/service-health-notifications-properties.md).

## <a name="view-your-service-health-notifications-in-the-azure-portal"></a>Uw servicestatusmeldingen bekijken in Azure portal

1. In de [Azure-portal](https://portal.azure.com), selecteer **Monitor**.

    ![Schermafbeelding van de Azure portal in het menu met Monitor geselecteerd](./media/service-notifications/home-monitor.png)

    Azure Monitor combineert alle controle-instellingen en gegevens in één geconsolideerde weergave. Als eerste wordt de sectie **Activiteitenlogboek** geopend.

1. Selecteer **waarschuwingen**.

    ![Schermafbeelding van de Monitor Activity-log, met waarschuwingen die zijn geselecteerd](./media/service-notifications/service-health-summary.png)

1. Selecteer **+ waarschuwing voor activiteitenlogboek toevoegen**, en een waarschuwing instellen om te controleren of u ontvangt een melding voor toekomstige servicemeldingen. Zie voor meer informatie, [waarschuwingen voor activiteitenlogboek maken voor servicemeldingen](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).

## <a name="next-steps"></a>Volgende stappen

* Ontvangen [meldingen wanneer er een melding van de health service](../../azure-monitor/platform/alerts-activity-log-service-notifications.md) wordt geplaatst.  
* Meer informatie over [waarschuwingen voor activiteitenlogboeken](../../azure-monitor/platform/activity-log-alerts.md).

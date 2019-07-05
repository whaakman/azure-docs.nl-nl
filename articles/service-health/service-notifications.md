---
title: Servicestatusmeldingen bekijken met de Azure-portal
description: Servicestatusmeldingen kunnen u berichten van health-service gepubliceerd door Microsoft Azure weergeven.
author: stephbaron
ms.author: stbaron
services: monitoring
ms.service: service-health
ms.topic: conceptual
ms.date: 6/27/2019
ms.subservice: ''
ms.openlocfilehash: d2e18ae28e79590cb04ee0045341ea817be4a3bc
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538352"
---
# <a name="view-service-health-notifications-by-using-the-azure-portal"></a>Servicestatusmeldingen bekijken met de Azure-portal

Servicestatusmeldingen zijn gepubliceerd door de Azure-infrastructuur in de [Azure-activiteitenlogboek](../azure-monitor/platform/activity-logs-overview.md).  De meldingen bevatten informatie over de resources in uw abonnement. Gezien de mogelijk grote hoeveelheid gegevens die zijn opgeslagen in het activiteitenlogboek, bestaat een afzonderlijke gebruikersinterface zodat u gemakkelijk weergeven en waarschuwingen voor servicemeldingen status instellen. 

Servicestatusmeldingen zijn informatieve en bruikbare, afhankelijk van de klasse.

Zie voor meer informatie over de verschillende soorten servicestatusmeldingen [Service-eigenschappen van de gezondheid van meldingen](service-health-notifications-properties.md).

## <a name="view-your-service-health-notifications-in-the-azure-portal"></a>Uw servicestatusmeldingen bekijken in Azure portal

1. In de [Azure-portal](https://portal.azure.com), selecteer **Monitor**.

    ![Schermafbeelding van de Azure portal in het menu met Monitor geselecteerd](./media/service-notifications/home-monitor.png)

    Azure Monitor combineert alle controle-instellingen en gegevens in één geconsolideerde weergave. Als eerste wordt de sectie **Activiteitenlogboek** geopend.

1. Selecteer **waarschuwingen**.

    ![Schermafbeelding van de Monitor Activity-log, met waarschuwingen die zijn geselecteerd](./media/service-notifications/service-health-summary.png)

1. Selecteer **+ waarschuwing voor activiteitenlogboek toevoegen**, en een waarschuwing instellen om te controleren of u ontvangt een melding voor toekomstige servicemeldingen. Zie voor meer informatie, [waarschuwingen voor activiteitenlogboek maken voor servicemeldingen](../azure-monitor/platform/alerts-activity-log-service-notifications.md).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [waarschuwingen voor activiteitenlogboeken](../azure-monitor/platform/activity-log-alerts.md).

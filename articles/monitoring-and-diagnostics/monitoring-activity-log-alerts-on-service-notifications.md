---
title: Waarschuwingen voor activiteitenlogboeken van Azure-service-meldingen ontvangen
description: Ontvang een melding via SMS, e-mail of webhook als Azure-service optreedt.
author: shawntabrizi
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/09/2018
ms.author: shtabriz
ms.component: alerts
ms.openlocfilehash: 6e1a72c428425c73ff0446fc0d41b1b18333c3e3
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39423885"
---
# <a name="create-activity-log-alerts-on-service-notifications"></a>Waarschuwingen voor activiteitenlogboek maken voor servicemeldingen
## <a name="overview"></a>Overzicht
In dit artikel leest u hoe u waarschuwingen voor activiteitenlogboeken voor servicestatusmeldingen instelt met behulp van de Azure-portal.  

U kunt een waarschuwing ontvangt als Azure service health meldingen naar uw Azure-abonnement verzendt. U kunt de waarschuwing op basis van configureren:

- De klasse van de melding van de health service (serviceproblemen, gepland onderhoud, statusadvies).
- Het abonnement dat is beïnvloed.
- De (s) dat is beïnvloed.
- De regio('s) beïnvloed.

> [!NOTE]
> Servicestatusmeldingen verzendt een waarschuwing met betrekking tot resource health-gebeurtenissen.

Ook kunt u configureren die de waarschuwing moet worden verzonden naar:

- Selecteer een bestaande actiegroep.
- Maak een nieuwe actiegroep (die kan worden gebruikt voor toekomstige waarschuwingen).

Raadpleeg [Actiegroepen maken en beheren](monitoring-action-groups.md) voor meer informatie over actiegroepen.

Zie voor meer informatie over het configureren van de statusmeldingen notification service met behulp van Azure Resource Manager-sjablonen [Resource Manager-sjablonen](monitoring-create-activity-log-alerts-with-resource-manager-template.md).

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-the-azure-portal"></a>Een waarschuwing op een melding van de health service voor een nieuwe actiegroep maken met behulp van Azure portal
1. In de [portal](https://portal.azure.com), selecteer **servicestatus**.

    ![De "Service" statusservice](./media/monitoring-activity-log-alerts-on-service-notifications/home-servicehealth.png)

1. In de **waarschuwingen** sectie, selecteer **statusmeldingen**.

    ![Het tabblad 'Waarschuwingen voor Health'](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades-sh.png)

1. Selecteer **statuswaarschuwing voor de service maken** en vul de velden in.

    ![De opdracht 'Service-statusmelding maken'](./media/monitoring-activity-log-alerts-on-service-notifications/service-health-alert.png)

1. Selecteer de **abonnement**, **Services**, en **regio's** u wilt worden gewaarschuwd voor.

    ![In het dialoogvenster 'Waarschuwing voor activiteitenlogboek toevoegen'](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-new-ux.png)

> [!NOTE]
> Dit abonnement wordt gebruikt voor het opslaan van de waarschuwing voor activiteitenlogboek. De waarschuwing resource wordt geïmplementeerd voor dit abonnement en gebeurtenissen in het activiteitenlogboek voor bewaakt.

1. Kies de **gebeurtenistypen** u wilt worden gewaarschuwd voor: *Service probleem*, *gepland onderhoud*, en *statusadvies* 

1. De Waarschuwingsdetails definiëren door te voeren een **naam waarschuwingsregel** en **beschrijving**.

1. Selecteer de **resourcegroep** waar u de waarschuwing moet worden opgeslagen.

1. Een nieuwe actiegroep maken door te selecteren **nieuwe actiegroep**. Voer een naam in de **naam van de actiegroep** in en voert u een naam in de **afkorting** vak. De korte naam wordt verwezen in de meldingen die worden verzonden wanneer deze waarschuwing wordt geactiveerd.

    ![Een nieuwe actiegroep maken](./media/monitoring-activity-log-alerts-on-service-notifications/action-group-creation.png)

1. Een lijst met ontvangers definiëren door te geven van de ontvanger:

    a. **Naam**: Voer de naam, alias of id van de ontvanger.

    b. **Actietype**: Selecteer SMS, e-mail, webhook, Azure-app en meer.

    c. **Details**: op basis van het actietype is gekozen, voer een telefoonnummer, e-mailadres, webhook URI, enzovoort.

1. Selecteer **OK** de actiegroep maken en vervolgens **waarschuwingsregel maken** om uit te voeren van de waarschuwing.

Binnen een paar minuten, de waarschuwing is actief en begint met activeren op basis van de voorwaarden die u hebt opgegeven tijdens het maken van.

Meer informatie over het [configureren van de webhook-meldingen voor bestaande beheersystemen voor probleem](../service-health/service-health-alert-webhook-guide.md). Zie voor meer informatie over de webhook-schema voor waarschuwingen voor activiteitenlogboeken [Webhooks voor Azure-activiteit waarschuwingen voor activiteitenlogboeken](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>De actiegroep die is gedefinieerd in deze stappen is herbruikbare als een bestaande actiegroep voor alle toekomstige definities van de waarschuwing.
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-by-using-the-azure-portal"></a>Een waarschuwing op een melding van de health service voor een bestaande actiegroep maken met behulp van Azure portal

1. Volg de stappen 1 tot en met 7 in de vorige sectie om te maken van de melding van de health service. 

1. Onder **actiegroep definiëren**, klikt u op de **actie selecteren groep** knop. Selecteer de juiste actie.

1. Selecteer **toevoegen** om toe te voegen van de actiegroep en vervolgens **waarschuwingsregel maken** om uit te voeren van de waarschuwing.

Binnen een paar minuten, de waarschuwing is actief en begint met activeren op basis van de voorwaarden die u hebt opgegeven tijdens het maken van.

## <a name="manage-your-alerts"></a>Uw waarschuwingen beheren

Nadat u een waarschuwing gemaakt, wordt het weergegeven in de **waarschuwingen** sectie van **Monitor**. Selecteer de waarschuwing die u wilt beheren:

* Deze bewerken.
* Het verwijderen.
* - Of uitschakelen, als u wilt tijdelijk stoppen of doorgaan met het ontvangen van meldingen voor de waarschuwing.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het [configureren van de webhook-meldingen voor bestaande beheersystemen voor probleem](../service-health/service-health-alert-webhook-guide.md).
- Meer informatie over [health servicemeldingen](monitoring-service-notifications.md).
- Meer informatie over [melding gelden enkele beperkingen](monitoring-alerts-rate-limiting.md).
- Controleer de [activiteit log waarschuwing webhook-schema](monitoring-activity-log-alerts-webhook.md).
- Krijgen een [overzicht van waarschuwingen voor activiteitenlogboeken](monitoring-overview-alerts.md), en leer hoe u waarschuwingen ontvangt. 
- Meer informatie over [actiegroepen](monitoring-action-groups.md).

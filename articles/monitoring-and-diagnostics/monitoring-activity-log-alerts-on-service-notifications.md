---
title: Activiteit logboek meldingen ontvangen over Azure-service-meldingen
description: Blijf op de hoogte via SMS of e-mail webhook wanneer Azure service optreedt.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/09/2018
ms.author: johnkem
ms.component: alerts
ms.openlocfilehash: 01dc3a3c6489b694af26c78ae3b4756f3e8f00b7
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263113"
---
# <a name="create-activity-log-alerts-on-service-notifications"></a>Het maken van de activiteit logboek waarschuwingen op servicemeldingen
## <a name="overview"></a>Overzicht
In dit artikel leest u hoe activiteit logboek waarschuwingen voor servicestatusmeldingen instellen met behulp van de Azure-portal.  

U kunt een waarschuwing ontvangt als Azure servicestatusmeldingen naar uw Azure-abonnement verzendt. U kunt de waarschuwing op basis van configureren:

- De klasse van de melding van de health service (problemen met de Service, gepland onderhoud, Health aanbevelingen).
- Het abonnement is van invloed op.
- De (s) dat is beïnvloed.
- De regio('s) beïnvloed.

Ook kunt u configureren die de waarschuwing moet worden verzonden naar:

- Selecteer een bestaande actiegroep.
- Maak een nieuwe actiegroep (die kan worden gebruikt voor toekomstige waarschuwingen).

Zie voor meer informatie over actiegroepen, [maken en beheren van actiegroepen](monitoring-action-groups.md).

Zie voor meer informatie over de service health meldingen om waarschuwingen te configureren met behulp van Azure Resource Manager-sjablonen [Resource Manager-sjablonen](monitoring-create-activity-log-alerts-with-resource-manager-template.md).

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-the-azure-portal"></a>Een waarschuwing op een melding van de health service voor een nieuwe actiegroep maken met behulp van de Azure-portal
1. In de [portal](https://portal.azure.com), selecteer **servicestatus**.

    ![De "Service" statusservice](./media/monitoring-activity-log-alerts-on-service-notifications/home-servicehealth.png)

2. In de **waarschuwingen** sectie **statuswaarschuwingen**.

    ![Het tabblad 'Health waarschuwingen'](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades-sh.png)

3. Selecteer **maken service health waarschuwing** en vul de velden in.

    ![De opdracht 'Service-statuswaarschuwing maken'](./media/monitoring-activity-log-alerts-on-service-notifications/service-health-alert.png)

4. Selecteer de **abonnement**, **Services**, en **regio's** u wilt worden gewaarschuwd voor.

    ![Het dialoogvenster 'Activiteit logboek waarschuwing toevoegen' in](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-new-ux.png)

> [!NOTE]
> Dit abonnement wordt gebruikt voor het opslaan van de activiteit logboek-waarschuwing. De waarschuwing resource wordt geïmplementeerd voor dit abonnement en gebeurtenissen in het gebeurtenissenlogboek voor bewaakt.

5. Kies de **gebeurtenistypen** u wilt worden gewaarschuwd voor: *Service probleem*, *gepland onderhoud*, en *Health aanbevelingen* 

6. Definieer de details van de waarschuwing door te geven een **waarschuwingsregel naam** en **beschrijving**.

7. Selecteer de **resourcegroep** waar u de waarschuwing op te slaan.

8. Een nieuwe actiegroep maken door te selecteren **nieuwe actiegroep**. Voer een naam in de **actie groepsnaam** vak en voer een naam in de **afkorting** vak. De korte naam wordt verwezen in de meldingen die worden verzonden wanneer deze waarschuwing wordt geactiveerd.

    ![Maak een nieuwe actiegroep](./media/monitoring-activity-log-alerts-on-service-notifications/action-group-creation.png)

9. Een lijst met ontvangers door te geven van de ontvanger definiëren:

    a. **Naam**: Voer de naam, de alias of de id van de ontvanger.

    b. **Actietype**: Selecteer SMS, e, webhook, Apps van Azure en meer.

    c. **Details**: op basis van het actietype is gekozen, voer een telefoonnummer, e-mailadres, webhook URI, enzovoort.

10. Selecteer **OK** om het actiegroep te maken en vervolgens **waarschuwingsregel maken** voltooien van de waarschuwing.

Binnen een paar minuten de waarschuwing actief is en begint te activeren op basis van de voorwaarden die u hebt opgegeven tijdens het maken van.

Meer informatie over hoe [webhook meldingen configureren voor bestaande beheersystemen voor probleem](../service-health/service-health-alert-webhook-guide.md). Zie voor meer informatie over het schema van de webhook voor activiteit logboek waarschuwingen [Webhooks voor Azure activiteit waarschuwingen melden](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>De actiegroep gedefinieerd in deze stappen is herbruikbare als een bestaande actiegroep voor alle toekomstige definities van de waarschuwing.
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-by-using-the-azure-portal"></a>Een waarschuwing op een melding van de health service voor een bestaande actiegroep maken met behulp van de Azure-portal

1. Volg de stappen 1 tot en met 7 in de vorige sectie voor het maken van de melding van de health service. 

2. Onder **definiëren actiegroep**, klikt u op de **Selecteer actiegroep** knop. Selecteer de juiste actie-groep.

3. Selecteer **toevoegen** de actiegroep toe te voegen en vervolgens **waarschuwingsregel maken** voltooien van de waarschuwing.

Binnen een paar minuten de waarschuwing actief is en begint te activeren op basis van de voorwaarden die u hebt opgegeven tijdens het maken van.

## <a name="manage-your-alerts"></a>Waarschuwingen beheren

Nadat u een waarschuwing gemaakt, is zichtbaar in de **waarschuwingen** sectie van **Monitor**. Selecteer de waarschuwing die u wilt beheren:

* Bewerken.
* Verwijder de groep.
* - Of uitschakelen, als u wilt tijdelijk stoppen of te hervatten ontvangen van meldingen voor de waarschuwing.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over hoe [webhook meldingen configureren voor bestaande beheersystemen voor probleem](../service-health/service-health-alert-webhook-guide.md).
- Meer informatie over [service health meldingen](monitoring-service-notifications.md).
- Meer informatie over [melding snelheidsbeperking](monitoring-alerts-rate-limiting.md).
- Controleer de [activiteit logboek waarschuwing webhook schema](monitoring-activity-log-alerts-webhook.md).
- Ophalen van een [overzicht van de activiteit logboek waarschuwingen](monitoring-overview-alerts.md), en meer informatie over het om waarschuwingen te ontvangen. 
- Meer informatie over [actiegroepen](monitoring-action-groups.md).

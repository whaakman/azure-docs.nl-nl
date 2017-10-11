---
title: Activiteit logboek meldingen ontvangen over servicemeldingen | Microsoft Docs
description: Blijf op de hoogte via SMS of e-mail webhook wanneer Azure service optreedt.
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: johnkem
ms.openlocfilehash: bf6a98fd7e7e11764bef174f9efd0635fa7efe9a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="create-activity-log-alerts-on-service-notifications"></a>Het maken van de activiteit logboek waarschuwingen op servicemeldingen
## <a name="overview"></a>Overzicht
In dit artikel leest u hoe activiteit logboek waarschuwingen voor servicestatusmeldingen instellen met behulp van de Azure-portal.  

U kunt een waarschuwing ontvangt als Azure servicestatusmeldingen naar uw Azure-abonnement verzendt. U kunt de waarschuwing op basis van configureren:

- De klasse van service health melding (incident, onderhoud, gegevens, enzovoort).
- De (s) dat is beïnvloed.
- De regio('s) beïnvloed.
- De status van de melding (actieve versus opgelost).
- Het niveau van de meldingen (informatief, waarschuwing, fout).

Ook kunt u configureren die de waarschuwing moet worden verzonden naar:

- Selecteer een bestaande actiegroep.
- Maak een nieuwe actiegroep (die kan worden gebruikt voor toekomstige waarschuwingen).

Zie voor meer informatie over actiegroepen, [maken en beheren van actiegroepen](monitoring-action-groups.md).

Zie voor meer informatie over de service health meldingen om waarschuwingen te configureren met behulp van Azure Resource Manager-sjablonen [Resource Manager-sjablonen](monitoring-create-activity-log-alerts-with-resource-manager-template.md).

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-the-azure-portal"></a>Een waarschuwing op een melding van de health service voor een nieuwe actiegroep maken met behulp van de Azure-portal
1. In de [portal](https://portal.azure.com), selecteer **Monitor**.

    ![De 'Monitor'-service](./media/monitoring-activity-log-alerts-on-service-notifications/home-monitor.png)

2. In de **activiteitenlogboek** sectie **waarschuwingen**.

    ![Het tabblad 'Waarschuwingen'](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades.png)

3. Selecteer **toevoegen activiteit logboek waarschuwing**, en vul de velden in.

    ![De opdracht 'Melding toevoegen activiteit log'](./media/monitoring-activity-log-alerts-on-service-notifications/add-activity-log-alert.png)

4. Voer een naam in de **waarschuwing logboeknaam activiteit** vak en geef een **beschrijving**.

    ![Het dialoogvenster 'Activiteit logboek waarschuwing toevoegen' in](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-service-notification-new-action-group.png)

5. De **abonnement** vak autofills met uw huidige abonnement. Dit abonnement wordt gebruikt voor het opslaan van de activiteit logboek-waarschuwing. De waarschuwing resource wordt geïmplementeerd voor dit abonnement en gebeurtenissen in het gebeurtenissenlogboek voor bewaakt.

6. Selecteer de **resourcegroep** waarin de waarschuwing resource is gemaakt. Dit is de resourcegroep die wordt bewaakt door de waarschuwing niet. Het is in plaats daarvan de resourcegroep waar de waarschuwing bron zich bevindt.

7. In de **gebeurteniscategorie** de optie **servicestatus**. Selecteer desgewenst de **Service**, **regio**, **Type**, **Status**, en **niveau** van servicestatus meldingen die u wilt ontvangen.

8. Onder **waarschuwing via**, selecteer de **nieuw** actieknop groep. Voer een naam in de **actie groepsnaam** vak en voer een naam in de **afkorting** vak. De korte naam wordt verwezen in de meldingen die worden verzonden wanneer deze waarschuwing wordt geactiveerd.

9. Een lijst met ontvangers door te geven van de ontvanger definiëren:

    a. **Naam**: Voer de naam, de alias of de id van de ontvanger.

    b. **Actietype**: Selecteer SMS, e-mail of webhook.

    c. **Details**: op basis van het actietype is gekozen, voer een telefoonnummer, e-mailadres of webhook URI.

10. Selecteer **OK** de waarschuwing wilt maken.

Binnen een paar minuten de waarschuwing actief is en begint te activeren op basis van de voorwaarden die u hebt opgegeven tijdens het maken van.

Zie voor meer informatie over het schema van de webhook voor activiteit logboek waarschuwingen [Webhooks voor Azure activiteit waarschuwingen melden](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>De actiegroep gedefinieerd in deze stappen is herbruikbare als een bestaande actiegroep voor alle toekomstige definities van de waarschuwing.
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-by-using-the-azure-portal"></a>Een waarschuwing op een melding van de health service voor een bestaande actiegroep maken met behulp van de Azure-portal

1. Volg de stappen 1 tot en met 7 in de vorige sectie voor het maken van de melding van de health service. 

2. Onder **waarschuwing via**, selecteer de **bestaande** actieknop groep. Selecteer de juiste actie-groep.

3. Selecteer **OK** de waarschuwing wilt maken.

Binnen een paar minuten de waarschuwing actief is en begint te activeren op basis van de voorwaarden die u hebt opgegeven tijdens het maken van.

## <a name="manage-your-alerts"></a>Waarschuwingen beheren

Nadat u een waarschuwing gemaakt, is zichtbaar in de **waarschuwingen** sectie van de **Monitor** blade. Selecteer de waarschuwing die u wilt beheren:

* Bewerken.
* Verwijder de groep.
* - Of uitschakelen, als u wilt tijdelijk stoppen of te hervatten ontvangen van meldingen voor de waarschuwing.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [service health meldingen](monitoring-service-notifications.md).
- Meer informatie over [melding snelheidsbeperking](monitoring-alerts-rate-limiting.md).
- Controleer de [activiteit logboek waarschuwing webhook schema](monitoring-activity-log-alerts-webhook.md).
- Ophalen van een [overzicht van de activiteit logboek waarschuwingen](monitoring-overview-alerts.md), en meer informatie over het om waarschuwingen te ontvangen. 
- Meer informatie over [actiegroepen](monitoring-action-groups.md).

---
title: Actiegroepen in de Azure portal maken en beheren | Microsoft Docs
description: Informatie over het actiegroepen in de Azure portal maken en beheren.
author: anirudhcavale
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
ms.date: 05/15/2017
ms.author: ancav
ms.openlocfilehash: 05775415e210333cf63565e7b5b554d014f6ba23
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Actiegroepen in de Azure portal maken en beheren
## <a name="overview"></a>Overzicht ##
In dit artikel laat zien hoe actiegroepen in de Azure portal maken en beheren.

U kunt een lijst van acties met actiegroepen configureren. Deze groepen kunnen vervolgens worden gebruikt wanneer u activiteitswaarschuwingen logboekbestand definieert. Deze groepen kunnen vervolgens worden hergebruikt door elke activiteit logboek waarschuwing die u hebt gedefinieerd, waarbij u ervoor zorgt dat elke keer dat de activiteit logboek waarschuwing is geactiveerd door dezelfde acties worden genomen.

Een actiegroep kan maximaal 10 van elk actietype hebben. Elke actie bestaat uit de volgende eigenschappen:

* **Naam**: een unieke id binnen de groep in te grijpen.  
* **Actietype**: een SMS-bericht verzenden, e-mailbericht verzenden, een webhook aanroepen, gegevens verzenden naar een hulpprogramma ITSM, aanroepen van een Azure-app of een Automation-runbook uitvoeren.
* **Details**: de bijbehorende phone getal, e-mailadres, webhook URI of ITSM verbindingsdetails.

Zie voor meer informatie over het gebruik van Azure Resource Manager-sjablonen voor het configureren van de actiegroepen [actie groep Resource Manager-sjablonen](monitoring-create-action-group-with-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Een actiegroep maken met behulp van de Azure-portal ##
1. In de [portal](https://portal.azure.com), selecteer **Monitor**. De **Monitor** blade consolideert alle controle-instellingen en gegevens in één weergave.

    ![De 'Monitor'-service](./media/monitoring-action-groups/home-monitor.png)
2. In de **instellingen** sectie **actiegroepen**.

    ![Het tabblad 'actiegroepen'](./media/monitoring-action-groups/action-groups-blade.png)
3. Selecteer **actie-groep toevoegen**, en vul de velden in.

    ![De opdracht 'Groep toevoegen actie'](./media/monitoring-action-groups/add-action-group.png)
4. Voer een naam in de **actie groepsnaam** vak en voer een naam in de **afkorting** vak. De korte naam wordt gebruikt in plaats van een volledige groep actienaam wanneer meldingen worden verzonden met behulp van deze groep.

      ![In het dialoogvenster van de groep toevoegen acties'](./media/monitoring-action-groups/action-group-define.png)

5. De **abonnement** vak autofills met uw huidige abonnement. Dit abonnement is de waarin de actiegroep wordt opgeslagen.

6. Selecteer de **resourcegroep** in de actiegroep wordt opgeslagen.

7. Een lijst van acties door te geven van de actie definiëren:

    a. **Naam**: Voer een unieke id voor deze actie.

    b. **Actietype**: Selecteer SMS, e, webhook, Apps van Azure, ITSM of Automation-Runbook.

    c. **Details**: op basis van het actietype, voer een telefoonnummer, e-mailadres, webhook URI, Apps van Azure, ITSM verbinding of Automation-runbook. Voor ITSM actie bovendien opgeven **werkitem** en andere velden uw ITSM hulpprogramma vereist. 

   > [!NOTE]
   > ITSM-actie vereist een ITSM-verbinding. Meer informatie over het maken van een [ITSM verbinding](../log-analytics/log-analytics-itsmc-overview.md). Actie ITSM werkt momenteel alleen voor activiteit logboek waarschuwingen. Deze actie is momenteel geen voor andere typen waarschuwingen.

8. Selecteer **OK** om het actiegroep te maken.

## <a name="manage-your-action-groups"></a>Actiegroepen beheren ##
Nadat u een actiegroep maakt, is zichtbaar in de **actiegroepen** sectie van de **Monitor** blade. Selecteer de actiegroep die u wilt beheren:

* Toevoegen, bewerken of verwijderen van acties.
* De actiegroep verwijderen.

## <a name="next-steps"></a>Volgende stappen ##
* Meer informatie over [SMS waarschuwing gedrag](monitoring-sms-alert-behavior.md).  
* Krijgen een [inzicht in het schema activiteit logboek waarschuwing webhook](monitoring-activity-log-alerts-webhook.md).  
* Meer informatie over [ITSM-Connector](../log-analytics/log-analytics-itsmc-overview.md)
* Meer informatie over [snelheidsbeperking](monitoring-alerts-rate-limiting.md) van waarschuwingen. 
* Ophalen van een [overzicht van de activiteit logboek waarschuwingen](monitoring-overview-alerts.md), en meer informatie over het om waarschuwingen te ontvangen.  
* Meer informatie over hoe [waarschuwingen configureren wanneer een melding van de health service wordt geboekt](monitoring-activity-log-alerts-on-service-notifications.md).

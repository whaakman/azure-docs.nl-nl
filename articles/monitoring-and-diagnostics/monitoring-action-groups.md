---
title: Actiegroepen in de Azure portal maken en beheren | Microsoft Docs
description: Informatie over het actiegroepen in de Azure portal maken en beheren.
author: dkamstra
manager: chrad
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/1/2018
ms.author: dukek
ms.openlocfilehash: fd6817e63eea622294bbf766b66beb8bc49bc30c
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34726054"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Actiegroepen in de Azure portal maken en beheren
## <a name="overview"></a>Overzicht ##
Een actiegroep is een verzameling van voorkeuren gedefinieerd door de gebruiker. Waarschuwingen van Azure bewaken en servicestatus zijn geconfigureerd voor gebruik van een specifieke actiegroep wanneer de waarschuwing wordt geactiveerd. Diverse waarschuwingen kunnen gebruikmaken van de groep met dezelfde acties of andere actiegroepen, afhankelijk van de vereisten van de gebruiker.

In dit artikel laat zien hoe actiegroepen in de Azure portal maken en beheren.

Elke actie bestaat uit de volgende eigenschappen:

* **Naam**: een unieke id binnen de groep in te grijpen.  
* **Actietype**: een telefoongesprek of SMS-bericht verzenden, e-mailbericht verzenden, een webhook aanroepen, gegevens verzenden naar een hulpprogramma ITSM, aanroepen van een logische App, een push-melding verzenden naar de Azure app of een Automation-runbook uitvoeren.
* **Details**: de bijbehorende phone getal, e-mailadres, webhook URI of ITSM verbindingsdetails.

Zie voor meer informatie over het gebruik van Azure Resource Manager-sjablonen voor het configureren van de actiegroepen [actie groep Resource Manager-sjablonen](monitoring-create-action-group-with-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Een actiegroep maken met behulp van de Azure-portal ##
1. In de [portal](https://portal.azure.com), selecteer **Monitor**. De **Monitor** blade consolideert alle controle-instellingen en gegevens in één weergave.

    ![De 'Monitor'-service](./media/monitoring-action-groups/home-monitor.png)
2. In de **instellingen** sectie **actiegroepen**.

    ![Het tabblad 'actiegroepen'](./media/monitoring-action-groups/action-groups-blade.png)
3. Selecteer **actie-groep toevoegen**, en vul de velden in.

    ![De opdracht 'Groep toevoegen actie'](./media/monitoring-action-groups/add-action-group.png)
4. Voer een naam in de **actie groepsnaam** vak en voer een naam in de **afkorting** vak. De korte naam wordt gebruikt in plaats van een volledige naam van de actiegroep als er meldingen via deze groep worden verzonden.

      ![In het dialoogvenster van de groep toevoegen acties'](./media/monitoring-action-groups/action-group-define.png)

5. De **abonnement** vak autofills met uw huidige abonnement. Dit abonnement is de waarin de actiegroep wordt opgeslagen.

6. Selecteer de **resourcegroep** in de actiegroep wordt opgeslagen.

7. Een lijst van acties door te geven van de actie definiëren:

    a. **Naam**: Voer een unieke id voor deze actie.

    b. **Actietype**: e-mailadres/SMS/Push/spraak, logische App, Webhook, ITSM of Automation-Runbook selecteren.

    c. **Details**: op basis van het actietype, voer een telefoonnummer, e-mailadres, webhook URI, Apps van Azure, ITSM verbinding of Automation-runbook. Voor ITSM actie bovendien opgeven **werkitem** en andere velden uw ITSM hulpprogramma vereist.

8. Selecteer **OK** om het actiegroep te maken.

## <a name="action-specific-information"></a>Actie-specifieke informatie
<dl>
<dt>Apps van Azure-Push</dt>
<dd>Mogelijk hebt u maximaal 10 Apps van Azure-acties in een groep in te grijpen.</dd>
<dd>Op dit moment ondersteunt de Azure-app-actie alleen ServiceHealth waarschuwingen. Andere waarschuwing tijd zal worden genegeerd. Zie [waarschuwingen configureren wanneer een melding van de health service wordt geboekt](monitoring-activity-log-alerts-on-service-notifications.md).</dd>

<dt>E-mail</dt>
<dd>De volgende e-mailadressen moeten e-mailberichten worden verzonden. Controleer of uw e-mail filteren op de juiste wijze is geconfigureerd

    - azure-noreply@microsoft.com
    - azureemail-noreply@microsoft.com
    - alerts-noreply@mail.windowsazure.com
    
</dd>
<dd>Mogelijk hebt u maximaal 1000 e-acties in de groep van een actie</dd>
<dd>Zie de [snelheidsbeperking informatie](./monitoring-alerts-rate-limiting.md) artikel</dd>

<dt>ITSM</dt>
<dd>Mogelijk hebt u maximaal 10 ITSM acties in de groep van een actie</dd>
<dd>ITSM-actie vereist een ITSM-verbinding. Meer informatie over het maken van een [ITSM verbinding](../log-analytics/log-analytics-itsmc-overview.md).</dd>

<dt>Logische App</dt>
<dd>Mogelijk hebt u maximaal 10 logische App-acties in de groep van een actie</dd>

<dt>runbook</dt>
<dd>Mogelijk hebt u maximaal 10 Runbook-acties in de groep van een actie</dd>

<dt>SMS</dt>
<dd>Mogelijk hebt u maximaal 10 SMS-acties in de groep van een actie</dd>
<dd>Zie de [snelheidsbeperking informatie](./monitoring-alerts-rate-limiting.md) artikel</dd>
<dd>Zie de [SMS waarschuwing gedrag](monitoring-sms-alert-behavior.md) artikel</dd>

<dt>Voice-over</dt>
<dd>Mogelijk hebt u maximaal 10 Voice-acties in de groep van een actie</dd>
<dd>Zie de [snelheidsbeperking informatie](./monitoring-alerts-rate-limiting.md) artikel</dd>

<dt>Webhook</dt>
<dd>Mogelijk hebt u maximaal 10 webhookacties in de groep van een actie
<dd>Pogingslogica - de time-outperiode voor een antwoord 10 seconden is. De aanroep van de webhook is geprobeerd een maximum van 2 maal wanneer de volgende HTTP-statuscodes worden geretourneerd: 408, 429, 503, 504 of het HTTP-eindpunt reageert niet. De eerste poging gebeurt na 10 seconden. De tweede en de laatste poging gebeurt na 100 seconden.</dd>
</dl>

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

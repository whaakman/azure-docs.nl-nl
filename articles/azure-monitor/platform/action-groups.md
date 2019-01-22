---
title: Actiegroepen in Azure portal maken en beheren
description: Informatie over het maken en beheren van actiegroepen in Azure portal.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: 38cccf17980f5a6a2cf162cdecdc6aad40d4f38e
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54432554"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Actiegroepen in Azure portal maken en beheren
## <a name="overview"></a>Overzicht ##
Een actiegroep is een verzameling van voorkeuren voor meldingen gedefinieerd door de eigenaar van een Azure-abonnement. Waarschuwingen van Azure Monitor en de servicestatus actiegroepen gebruiken om gebruikers te waarschuwen dat een waarschuwing is geactiveerd. Verschillende waarschuwingen kunnen gebruiken voor de actiegroep dezelfde of verschillende actiegroepen, afhankelijk van de vereisten van de gebruiker.

Wanneer een actie is geconfigureerd voor het verwittigen van een persoon via e-mail of SMS de persoon ontvangt een bevestiging die aangeeft dat hij / zij is toegevoegd aan de actiegroep.

In dit artikel wordt beschreven hoe u maken en beheren van actiegroepen in Azure portal.

Elke actie bestaat uit de volgende eigenschappen:

* **Naam**: Een unieke id binnen de actiegroep.  
* **Actietype**: De actie om uit te voeren. Voorbeelden zijn onder meer het verzenden van een stem oproep, SMS, e-mail; of verschillende soorten acties die automatisch wordt geactiveerd. Zie typen verderop in dit artikel. 
* **Details**: De bijbehorende gegevens die per variëren *actietype*. 

Zie voor meer informatie over het gebruik van Azure Resource Manager-sjablonen configureren actiegroepen [actie groep Resource Manager-sjablonen](../../azure-monitor/platform/action-groups-create-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Een actiegroep maken met behulp van de Azure-portal ##
1. In de [portal](https://portal.azure.com), selecteer **Monitor**. De **Monitor** blade consolideert alle controle-instellingen en gegevens in één weergave.

    ![De service 'Controleren'](./media/action-groups/home-monitor.png)
1. Selecteer **waarschuwingen** Selecteer **actiegroepen beheren**.

    ![Actiegroepen knop beheren](./media/action-groups/manage-action-groups.png)
1. Selecteer **actiegroep toevoegen**, en vul de velden in.

    ![De opdracht 'Actiegroep toevoegen'](./media/action-groups/add-action-group.png)
1. Voer een naam in de **naam van de actiegroep** vak en voer een naam in de **afkorting** vak. De korte naam wordt gebruikt in plaats van een volledige naam van de actiegroep als er meldingen via deze groep worden verzonden.

      ![In het dialoogvenster van de actiegroep toevoegen'](./media/action-groups/action-group-define.png)

1. De **abonnement** vak autofills met uw huidige abonnement. Dit abonnement is de waarin de actiegroep is opgeslagen.

1. Selecteer de **resourcegroep** in de actiegroep is opgeslagen.

1. Een lijst met acties definiëren door te geven van de actie:

    a. **Naam**: Voer een unieke id voor deze actie.

    b. **Actietype**: Selecteer de e-mailadres/SMS/Push/Voice, logische App, Webhook, ITSM of Automation-Runbook.

    c. **Details**: Op basis van het actietype, voer een telefoonnummer, e-mailadres, webhook URI, Azure-app, ITSM-verbinding of Automation-runbook. Voor ITSM-actie, Daarnaast Geef **werkitem** en andere velden uw ITSM-hulpprogramma is vereist.

1. Selecteer **OK** te maken van de actiegroep.

## <a name="manage-your-action-groups"></a>Beheren van uw actiegroepen ##
Nadat u een actiegroep die u hebt gemaakt, wordt het weergegeven in de **actiegroepen** sectie van de **Monitor** blade. Selecteer de actiegroep die u wilt beheren:

* Toevoegen, bewerken of verwijderen van acties.
* De actiegroep verwijderen.

## <a name="action-specific-information"></a>Actie-specifieke informatie
**Azure-app Pushmeldingen** -mogelijk hebt u maximaal 10 Apps van Azure-acties in een actiegroep. De Azure-app-actie ondersteunt op dit moment alleen ServiceHealth waarschuwingen. Andere waarschuwingen tijd worden genegeerd. Zie [waarschuwingen configureren wanneer er een melding van de health service wordt geplaatst](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).

**E-mailbericht** -e-mailberichten ontvangt van de volgende e-mailadressen. Zorg ervoor dat uw e-mail filteren op de juiste wijze is geconfigureerd
   - azure-noreply@microsoft.com
   - azureemail-noreply@microsoft.com
   - alerts-noreply@mail.windowsazure.com

Mogelijk hebt u maximaal 1000 e-mailacties in een actiegroep. Zie de [snelheidsbeperking informatie](./../../azure-monitor/platform/alerts-rate-limiting.md) artikel

**ITSM** -mogelijk hebt u maximaal 10 ITSM-acties in een actie groep ITSM-actie vereist een ITSM-verbinding. Meer informatie over het maken van een [ITSM-verbinding](../../azure-monitor/platform/itsmc-overview.md).

**Logische App** -mogelijk hebt u maximaal 10 acties van logische App in een actiegroep

**Functie-App** -de functietoetsen voor functie-Apps die zijn geconfigureerd als acties worden gelezen via de API-functies, die momenteel v2 functie-apps de app vereist-instelling 'AzureWebJobsSecretStorageType' naar 'bestanden' configureren, Zie [ Wijzigingen in de Key Management in functies V2]( https://aka.ms/funcsecrets) voor meer informatie.

**Runbook** -mogelijk hebt u maximaal 10 Runbook-acties in een actie groep verwijst naar de [Servicelimieten van Azure-abonnement](../../azure-subscription-service-limits.md) voor beperkingen met betrekking tot de Runbook-nettoladingen

**SMS** -mogelijk hebt u maximaal 10 SMS-acties in een actie groep zien de [snelheidsbeperking informatie](./../../azure-monitor/platform/alerts-rate-limiting.md) Zie artikel de [SMS waarschuwen gedrag](../../azure-monitor/platform/alerts-sms-behavior.md) artikel

**Stem** -mogelijk hebt u maximaal 10 Voice-acties in een actiegroep</dd>
Zie de [snelheidsbeperking informatie](./../../azure-monitor/platform/alerts-rate-limiting.md) artikel</dd>

**Webhook** -mogelijk hebt u maximaal 10 Webhook-acties in een actiegroep. Logica voor opnieuw proberen - de time-outperiode voor een antwoord 10 seconden is. De webhook-aanroep is geprobeerd een maximum van 2 tijden wanneer de volgende HTTP-statuscodes worden weergegeven: 408, 429, 503, 504 of het HTTP-eindpunt reageert niet. De eerste poging gebeurt na tien seconden. De tweede en laatste poging gebeurt na 100 seconden.

Bron-IP-adresbereiken
    - 13.106.57.181
    - 13.106.54.3
    - 13.106.54.19
    - 13.106.38.142
    - 13.106.38.148
    - 13.106.57.196

Voor het ontvangen van updates over wijzigingen in deze IP-adressen aangeraden dat u configureert een [statuswaarschuwing voor de Service](./../../azure-monitor/platform/service-notifications.md) die voor informatieve meldingen over de service actiegroepen bewaakt.


## <a name="next-steps"></a>Volgende stappen ##
* Meer informatie over [SMS waarschuwen gedrag](../../azure-monitor/platform/alerts-sms-behavior.md).  
* Krijg een [begrip van de activiteit log waarschuwing webhook-schema](../../azure-monitor/platform/activity-log-alerts-webhook.md).  
* Meer informatie over [ITSM-Connector](../../azure-monitor/platform/itsmc-overview.md)
* Meer informatie over [gelden enkele beperkingen](../../azure-monitor/platform/alerts-rate-limiting.md) op waarschuwingen.
* Krijgen een [overzicht van waarschuwingen voor activiteitenlogboeken](../../azure-monitor/platform/alerts-overview.md), en leer hoe u waarschuwingen ontvangt.  
* Meer informatie over het [waarschuwingen configureren wanneer er een melding van de health service wordt geplaatst](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).


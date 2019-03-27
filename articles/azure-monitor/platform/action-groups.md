---
title: Actiegroepen in Azure portal maken en beheren
description: Informatie over het maken en beheren van actiegroepen in Azure portal.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 3/26/2019
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: 695a2ff827fc5514c3a32364026bc9d47c8a2121
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58500305"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Actiegroepen in Azure portal maken en beheren
## <a name="overview"></a>Overzicht ##
Een actiegroep is een verzameling van voorkeuren voor meldingen gedefinieerd door de eigenaar van een Azure-abonnement. Waarschuwingen van Azure Monitor en de servicestatus actiegroepen gebruiken om gebruikers te waarschuwen dat een waarschuwing is geactiveerd. Verschillende waarschuwingen kunnen gebruiken voor de actiegroep dezelfde of verschillende actiegroepen, afhankelijk van de vereisten van de gebruiker. U kunt maximaal 2.000 actiegroepen configureren in een abonnement.

U configureren een actie om de hoogte van een persoon via e-mail of SMS, ontvangen ze een bevestiging die aangeeft dat ze zijn toegevoegd aan de actiegroep te.

In dit artikel wordt beschreven hoe u maken en beheren van actiegroepen in Azure portal.

Elke actie bestaat uit de volgende eigenschappen:

* **Naam**: Een unieke id binnen de actiegroep.  
* **Actietype**: De actie uitgevoerd. Voorbeelden zijn onder meer het verzenden van een stem oproep, SMS, e-mail; of verschillende soorten acties die automatisch wordt geactiveerd. Zie typen verderop in dit artikel. 
* **Details**: De bijbehorende details die per variëren *actietype*. 

Zie voor meer informatie over het gebruik van Azure Resource Manager-sjablonen configureren actiegroepen [actie groep Resource Manager-sjablonen](../../azure-monitor/platform/action-groups-create-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Een actiegroep maken met behulp van de Azure-portal ##
1. In de [portal](https://portal.azure.com), selecteer **Monitor**. De **Monitor** deelvenster consolideert alle controle-instellingen en gegevens in één weergave.

    ![De service 'Controleren'](./media/action-groups/home-monitor.png)
1. Selecteer **waarschuwingen** Selecteer **actiegroepen beheren**.

    ![Actiegroepen knop beheren](./media/action-groups/manage-action-groups.png)
1. Selecteer **actiegroep toevoegen**, en vul de velden in.

    ![De opdracht 'Actiegroep toevoegen'](./media/action-groups/add-action-group.png)
1. Voer een naam in de **naam van de actiegroep** vak en voer een naam in de **afkorting** vak. De korte naam wordt gebruikt in plaats van een volledige naam van de actiegroep als er meldingen via deze groep worden verzonden.

      ![In het dialoogvenster van de actiegroep toevoegen'](./media/action-groups/action-group-define.png)

1. De **abonnement** vak autofills met uw huidige abonnement. Dit abonnement is de waarin de actiegroep is opgeslagen.

1. Selecteer de **resourcegroep** in de actiegroep is opgeslagen.

1. Een lijst met acties definiëren. Hiermee geeft u de volgende voor elke actie:

    a. **Naam**: Voer een unieke id voor deze actie.

    b. **Actietype**: Selecteer de e-mailadres/SMS/Push/Voice, logische App, Webhook, ITSM of Automation-Runbook.

    c. **Details**: Op basis van het actietype, voer een telefoonnummer, e-mailadres, webhook URI, Azure-app, ITSM-verbinding of Automation-runbook. Voor ITSM-actie, Daarnaast Geef **werkitem** en andere velden uw ITSM-hulpprogramma is vereist.

1. Selecteer **OK** te maken van de actiegroep.

## <a name="manage-your-action-groups"></a>Beheren van uw actiegroepen ##
Nadat u een actiegroep die u hebt gemaakt, wordt het weergegeven in de **actiegroepen** sectie van de **Monitor** deelvenster. Selecteer de actiegroep die u wilt beheren:

* Toevoegen, bewerken of verwijderen van acties.
* De actiegroep verwijderen.

## <a name="action-specific-information"></a>Actie-specifieke informatie
> [!NOTE]
> Zie [abonnement Servicelimieten voor bewaking](https://docs.microsoft.com/azure/azure-subscription-service-limits#monitor-limits) voor numerieke limieten op elk van de volgende items.  

**Azure-app Pushmeldingen** -u kunt een beperkt aantal Azure-app-acties hebben in een actiegroep. De Azure-app-actie ondersteunt op dit moment alleen ServiceHealth waarschuwingen. Andere Waarschuwingstype worden genegeerd. Zie [waarschuwingen configureren wanneer er een melding van de health service wordt geplaatst](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).

**E-mailbericht** -e-mailberichten ontvangt van de volgende e-mailadressen. Zorg ervoor dat uw e-mail filteren op de juiste wijze is geconfigureerd
- azure-noreply@microsoft.com
- azureemail-noreply@microsoft.com
- alerts-noreply@mail.windowsazure.com

U mogelijk een beperkt aantal e-mailacties in een actiegroep. Zie de [snelheidsbeperking informatie](./../../azure-monitor/platform/alerts-rate-limiting.md) artikel

**ITSM** -u mogelijk een beperkt aantal beperkt aantal ITSM-acties in een actiegroep. ITSM-actie vereist een ITSM-verbinding. Meer informatie over het maken van een [ITSM-verbinding](../../azure-monitor/platform/itsmc-overview.md).

**Logische App** -u mogelijk een beperkt aantal acties van logische apps in een actiegroep.

**Functie-App** -de functie sleutels voor functie-Apps die zijn geconfigureerd als acties worden gelezen via de API-functies, die momenteel v2 functie-apps vereist configureren van de app-instelling 'AzureWebJobsSecretStorageType' naar 'files'. Zie voor meer informatie, [wijzigingen in de Key Management in functies V2]( https://aka.ms/funcsecrets).

**Runbook** -u mogelijk een beperkt aantal Runbook-acties in een actiegroep. Raadpleeg de [Servicelimieten van Azure-abonnement](../../azure-subscription-service-limits.md) voor beperkingen met betrekking tot de Runbook-nettoladingen.

**SMS** -u mogelijk een beperkt aantal SMS-acties in een actiegroep. Zie ook de [snelheidsbeperking informatie](./../../azure-monitor/platform/alerts-rate-limiting.md) en [SMS waarschuwen gedrag](../../azure-monitor/platform/alerts-sms-behavior.md) voor belangrijke aanvullende informatie. 

**Stem** -u mogelijk een beperkt aantal Voice-acties in een actiegroep. Zie de [snelheidsbeperking informatie](./../../azure-monitor/platform/alerts-rate-limiting.md) artikel.

**Webhook** -u mogelijk een beperkt aantal webhookacties in een actiegroep. Webhooks worden opnieuw uitgevoerd met behulp van de volgende regels. De webhook-aanroep opnieuw is geprobeerd een maximum van 2 tijden wanneer de volgende HTTP-statuscodes worden weergegeven: 408, 429, 503, 504 of het HTTP-eindpunt reageert niet. De eerste poging vindt plaats na 10 seconden. De tweede nieuwe poging gebeurt na 100 seconden. Na twee fouten zal geen actiegroep het eindpunt aanroepen gedurende 30 minuten. 

Bron-IP-adresbereiken
 - 13.72.19.232
 - 13.106.57.181
 - 13.106.54.3
 - 13.106.54.19
 - 13.106.38.142
 - 13.106.38.148
 - 13.106.57.196
 - 52.244.68.117
 - 51.4.138.199
 - 51.5.148.86

Voor het ontvangen van updates over wijzigingen in deze IP-adressen we u raden een [servicestatus waarschuwing configureren, waarmee wordt gecontroleerd voor informatieve meldingen over de actiegroepen-service.


## <a name="next-steps"></a>Volgende stappen ##

* Meer informatie over [SMS waarschuwen gedrag](../../azure-monitor/platform/alerts-sms-behavior.md).  
* Krijg een [begrip van de activiteit log waarschuwing webhook-schema](../../azure-monitor/platform/activity-log-alerts-webhook.md).  
* Meer informatie over [ITSM-Connector](../../azure-monitor/platform/itsmc-overview.md)
* Meer informatie over [gelden enkele beperkingen](../../azure-monitor/platform/alerts-rate-limiting.md) op waarschuwingen.
* Krijgen een [overzicht van waarschuwingen voor activiteitenlogboeken](../../azure-monitor/platform/alerts-overview.md), en leer hoe u waarschuwingen ontvangt.  
* Meer informatie over het [waarschuwingen configureren wanneer er een melding van de health service wordt geplaatst](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).


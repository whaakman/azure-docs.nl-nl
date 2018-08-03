---
title: Waarschuwingen voor klassieke activiteitenlogboek maken
description: Bericht via SMS, webhook en e-mailbericht wanneer bepaalde in het activiteitenlogboek gebeurtenissen.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/18/2017
ms.author: johnkem
ms.component: alerts
ms.openlocfilehash: 120fd3552ad36b3d19179f39ca95ce2b3ee2c2e6
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426615"
---
# <a name="create-activity-log-alerts-classic"></a>Waarschuwingen (klassiek) voor activiteitenlogboek maken

## <a name="overview"></a>Overzicht
Waarschuwingen voor activiteitenlogboeken zijn waarschuwingen die activeren wanneer een nieuwe activiteit log-gebeurtenis optreedt die overeenkomt met de voorwaarden die zijn opgegeven in de waarschuwing. Ze zijn Azure-resources, zodat ze kunnen worden gemaakt met behulp van een Azure Resource Manager-sjabloon. Ze kunnen ook worden gemaakt, bijgewerkt of verwijderd in Azure portal. In dit artikel bevat de concepten achter waarschuwingen voor activiteitenlogboeken. Hier vervolgens ziet u hoe u Azure portal gebruiken voor het instellen van een waarschuwing op gebeurtenissen in het activiteitenlogboek.

> [!NOTE]

>  De nieuwe [waarschuwingen](monitoring-overview-unified-alerts.md) ervaring is vervangen door deze procedure. In dit artikel is bedoeld als referentie voor de eerdere ervaring. [Meer informatie](monitoring-activity-log-alerts-new-experience.md).

Normaal gesproken u waarschuwingen voor activiteitenlogboek maken om meldingen te ontvangen wanneer:

* Bepaalde wijzigingen optreden voor resources in uw Azure-abonnement, vaak binnen het bereik van bepaalde resourcegroepen of resources. Bijvoorbeeld, als u wilt worden gewaarschuwd wanneer een virtuele machine in myProductionResourceGroup wordt verwijderd. Of u wilt worden gewaarschuwd als nieuwe rollen zijn toegewezen aan een gebruiker in uw abonnement.
* Er treedt een gebeurtenis van de health service op. Service health-gebeurtenissen opnemen van incidenten en onderhoudsgebeurtenissen die betrekking hebben op de resources in uw abonnement.

In beide gevallen wordt een waarschuwing voor activiteitenlogboek controleert alleen voor gebeurtenissen in het abonnement waarin de waarschuwing is gemaakt.

U kunt een waarschuwing voor activiteitenlogboek op basis van een eigenschap op het hoogste niveau in de JSON-object voor het activiteitenlogboek kunt configureren. De portal ziet u echter de meest voorkomende opties:

- **Categorie**: met beheerdersrechten, Service-status, automatisch schalen en aanbeveling. Zie voor meer informatie, [overzicht van de Azure-activiteitenlogboek](./monitoring-overview-activity-logs.md#categories-in-the-activity-log). Zie voor meer informatie over service health-gebeurtenissen, [ontvangen van waarschuwingen voor activiteitenlogboeken voor servicemeldingen](./monitoring-activity-log-alerts-on-service-notifications.md).
- **Resourcegroep**
- **Resource**
- **Resourcetype**
- **De naam van bewerking**: naam van de bewerking The Resource Manager Role-Based Access Control.
- **Niveau**: de ernst van de gebeurtenis (uitgebreid, informatief, waarschuwing, fout of kritiek).
- **Status**: de status van de gebeurtenis, meestal aan de slag, mislukt of geslaagd.
- **Gebeurtenis gestart door**: ook wel bekend als de "oproepende functie." De e-mailadres of Azure Active Directory-id van de gebruiker die de bewerking heeft uitgevoerd.

> [!NOTE]
> Wanneer de categorie 'administratieve' is, moet u ten minste één van de bovenstaande criteria opgeven in de waarschuwing. U kunt een waarschuwing die wordt geactiveerd wanneer een gebeurtenis wordt gemaakt in de activiteitenlogboeken niet maken.

Wanneer een waarschuwing voor activiteitenlogboek wordt geactiveerd, wordt een actiegroep die u gebruikt om acties of meldingen te genereren. Een actiegroep is een herbruikbare set van ontvangers van meldingen, zoals e-mailadressen, telefoonnummers webhook-URL's of SMS. De ontvangers kunnen worden verwezen vanuit meerdere waarschuwingen en de meldingskanalen groep centraliseren. Wanneer u de waarschuwing voor activiteitenlogboeken hebt gedefinieerd, hebt u twee opties. U kunt:

* Gebruik een bestaande actiegroep die u in de waarschuwing voor activiteitenlogboeken.
* Een nieuwe actiegroep maken.

Zie voor meer informatie over actiegroepen [maken en beheren van actiegroepen in Azure portal](monitoring-action-groups.md).

Zie voor meer informatie over servicestatusmeldingen [ontvangen van waarschuwingen voor activiteitenlogboeken op servicestatusmeldingen](monitoring-activity-log-alerts-on-service-notifications.md).

## <a name="create-an-alert-classic-on-an-activity-log-event-with-a-new-action-group-by-using-the-azure-portal"></a>Een waarschuwing maken (klassiek) van een activiteitenlogboek met een nieuwe actiegroep met behulp van Azure portal
1. In de [portal](https://portal.azure.com), selecteer **Monitor**.

    ![De service 'Controleren'](./media/monitoring-activity-log-alerts/home-monitor.png)
1. In de **activiteitenlogboek** sectie, selecteer **waarschuwingen (klassiek)**.

    ![Het tabblad 'Waarschuwingen'](./media/monitoring-activity-log-alerts/alerts-blades.png)
1. Selecteer **waarschuwing voor activiteitenlogboek toevoegen**, en vul de velden in.

1. Voer een naam in de **naam voor waarschuwing voor activiteitenlogboek** vak en selecteer een **beschrijving**.

    ![De opdracht 'Waarschuwing voor activiteitenlogboek toevoegen'](./media/monitoring-activity-log-alerts/add-activity-log-alert.png)

1. De **abonnement** vak autofills met uw huidige abonnement. Dit abonnement is de waarin de actiegroep is opgeslagen. De waarschuwing resource wordt geïmplementeerd voor dit abonnement en gebeurtenissen in het activiteitenlogboek uit bewaakt.

    ![In het dialoogvenster 'Waarschuwing voor activiteitenlogboek toevoegen'](./media/monitoring-activity-log-alerts/activity-log-alert-new-action-group.png)

1. Selecteer de **resourcegroep** waarin de waarschuwing resource is gemaakt. Dit is niet de resourcegroep die wordt bewaakt door de waarschuwing. Het is in plaats daarvan de resourcegroep waar de waarschuwing resource zich bevindt.

1. Selecteer desgewenst een **gebeurteniscategorie** te wijzigen van de aanvullende filters die worden weergegeven. Voor gebeurtenissen met beheerdersrechten, de filters bevatten **resourcegroep**, **Resource**, **resourcetype**, **bewerkingsnaam**, **Niveau**, **Status**, en **gebeurtenis gestart door**. Deze waarden identificeren welke gebeurtenissen die deze waarschuwing moet worden gecontroleerd.

    >[!NOTE]
    >U moet ten minste één van de bovenstaande criteria opgeven in de waarschuwing. U kunt een waarschuwing die wordt geactiveerd wanneer een gebeurtenis wordt gemaakt in de activiteitenlogboeken niet maken.
    >
    >

1. Voer een naam in de **naam van de actiegroep** vak en voer een naam in de **afkorting** vak. De korte naam wordt gebruikt in plaats van een volledige naam van de actiegroep als er meldingen via deze groep worden verzonden.

1.  Een lijst met acties definiëren door te geven van de actie:

    a. **Naam**: Voer de naam, alias of id van de actie.

    b. **Actietype**: Selecteer SMS, e-mail of webhook.

    c. **Details**: op basis van het actietype, voer een telefoonnummer, e-mailadres of webhook URI.

1.  Selecteer **OK** om de waarschuwing te maken.

De waarschuwing duurt een paar minuten volledig doorgegeven en vervolgens worden actief. Deze wordt geactiveerd wanneer er nieuwe gebeurtenissen die overeenkomen met de criteria van de waarschuwing.

Zie voor meer informatie, [begrijpen van de webhook-schema gebruikt in waarschuwingen voor activiteitenlogboeken](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>De actiegroep die is gedefinieerd in deze stappen is herbruikbare als een bestaande actiegroep voor alle toekomstige definities van de waarschuwing.
>
>

## <a name="create-an-alert-on-an-activity-log-event-for-an-existing-action-group-by-using-the-azure-portal"></a>Een waarschuwing in het activiteitenlogboek voor een bestaande actiegroep maken met de Azure-portal
1. Volg de stappen 1 tot en met 7 in de vorige sectie om te maken van de waarschuwing voor activiteitenlogboeken.

1. Onder **hierover te informeren via**, selecteer de **bestaande** groep actieknop. Selecteer een bestaande actiegroep die u in de lijst.

1. Selecteer **OK** om de waarschuwing te maken.

De waarschuwing duurt een paar minuten volledig doorgegeven en vervolgens worden actief. Deze wordt geactiveerd wanneer er nieuwe gebeurtenissen die overeenkomen met de criteria van de waarschuwing.

## <a name="manage-your-alerts"></a>Uw waarschuwingen beheren

Nadat u een waarschuwing gemaakt, is deze zichtbaar in de sectie waarschuwingen van de blade Monitor. Selecteer de waarschuwing die u wilt beheren:

* Deze bewerken.
* Het verwijderen.
* - Of uitschakelen, als u wilt tijdelijk stoppen of doorgaan met het ontvangen van meldingen voor de waarschuwing.

## <a name="next-steps"></a>Volgende stappen
- Krijgen een [overzicht van waarschuwingen](monitoring-overview-alerts.md).
- Meer informatie over [melding gelden enkele beperkingen](monitoring-alerts-rate-limiting.md).
- Controleer de [activiteit log waarschuwing webhook-schema](monitoring-activity-log-alerts-webhook.md).
- Meer informatie over [actiegroepen](monitoring-action-groups.md).  
- Meer informatie over [health servicemeldingen](monitoring-service-notifications.md).
- Maak een [waarschuwing voor activiteitenlogboek voor het bewaken van alle voor automatisch schalen-engine-bewerkingen op uw abonnement](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).
- Maak een [waarschuwing voor activiteitenlogboek voor het bewaken van alle mislukte voor automatisch schalen schaal-in/scale-out-bewerkingen op uw abonnement](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).

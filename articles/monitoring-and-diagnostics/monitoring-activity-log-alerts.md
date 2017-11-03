---
title: Activiteit logboek waarschuwingen maken | Microsoft Docs
description: Een melding via SMS, webhook en e-mailbericht wanneer bepaalde in het gebeurtenissenlogboek gebeurtenissen.
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
ms.date: 08/03/2017
ms.author: johnkem
ms.openlocfilehash: 3885469ec0e1fcc31386dd0ad7fe6cb5d03ab28e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-activity-log-alerts"></a>Logboek waarschuwingen voor de activiteit maken

## <a name="overview"></a>Overzicht
Activiteit logboek waarschuwingen zijn waarschuwingen die worden geactiveerd wanneer er een nieuwe activiteit gebeurtenislogboek optreedt die overeenkomt met de voorwaarden die is opgegeven in de waarschuwing. Ze zijn Azure-resources, zodat ze kunnen worden gemaakt met behulp van een Azure Resource Manager-sjabloon. Ze kunnen ook worden gemaakt, bijgewerkt of verwijderd in de Azure portal. Dit artikel bevat de concepten achter activiteit logboek waarschuwingen. Deze vervolgens ziet u hoe de Azure portal gebruiken voor het instellen van een waarschuwing op activiteit logboekgebeurtenissen.

Meestal maakt u activiteit logboek waarschuwingen om meldingen te ontvangen wanneer:

* Specifieke wijzigingen optreden voor bronnen in uw Azure-abonnement, vaak binnen het bereik van bepaalde resourcegroepen of resources. U wilt worden gewaarschuwd wanneer een virtuele machine in myProductionResourceGroup wordt verwijderd. Of u mogelijk wilt worden gewaarschuwd als er geen nieuwe rollen zijn toegewezen aan een gebruiker in uw abonnement.
* Er treedt een gebeurtenis van de health service op. Gebeurtenissen van de health service omvatten melding van incidenten en het onderhoud van gebeurtenissen die betrekking hebben op de resources in uw abonnement.

In beide gevallen controleert de activiteit logboek waarschuwing alleen voor gebeurtenissen in het abonnement waarmee de waarschuwing is gemaakt.

U kunt een activiteit logboek waarschuwing op basis van een eigenschap op het hoogste niveau in de JSON-object voor een activiteit van het gebeurtenislogboek kunt configureren. Echter, de portal ziet u de meest voorkomende opties:

- **Categorie**: beheer-, status, automatisch schalen en aanbeveling. Zie voor meer informatie [overzicht van het Azure activiteitenlogboek](./monitoring-overview-activity-logs.md#categories-in-the-activity-log). Zie voor meer informatie over gebeurtenissen van de health service, [activiteit logboek meldingen ontvangen over servicemeldingen](./monitoring-activity-log-alerts-on-service-notifications.md).
- **Resourcegroep**
- **Resource**
- **Brontype**
- **Naam van de bewerking**: de naam van de The Resource Manager Role-Based Access Control-bewerking.
- **Niveau**: de ernst van de gebeurtenis (uitgebreid, ter Info, waarschuwing, fout of kritiek).
- **Status**: de status van de gebeurtenis, doorgaans gestart, is mislukt of geslaagd.
- **De gebeurtenis wordt gestart door**: ook wel bekend als de "aanroeper." De e-mailadres of Azure Active Directory-id van de gebruiker die de bewerking uitgevoerd.

>[!NOTE]
>U moet ten minste twee van de bovenstaande criteria opgeven in de waarschuwing met één voor de categorie. U kunt een waarschuwing die wordt geactiveerd telkens wanneer een gebeurtenis wordt gemaakt in de activiteitenlogboeken van de niet maken.
>
>

Wanneer een activiteit logboek waarschuwing is geactiveerd, wordt een actiegroep gebruikt om acties of meldingen te genereren. Een actiegroep is een herbruikbare reeks melding ontvangers, zoals e-mailadressen, telefoonnummers webhook-URL's of SMS-bericht. De ontvangers kunnen worden verwezen vanuit meerdere waarschuwingen en de meldingskanalen groep centraliseren. Wanneer u uw logboek activiteit waarschuwing definieert, hebt u twee opties. U kunt:

* Gebruik een bestaande actiegroep in de activiteit logboek-waarschuwing. 
* Maak een nieuwe actiegroep. 

Zie voor meer informatie over actiegroepen, [maken en beheren van actiegroepen in de Azure portal](monitoring-action-groups.md).

Zie voor meer informatie over servicestatusmeldingen, [activiteit logboek meldingen ontvangen op servicestatusmeldingen](monitoring-activity-log-alerts-on-service-notifications.md).

## <a name="create-an-alert-on-an-activity-log-event-with-a-new-action-group-by-using-the-azure-portal"></a>Een waarschuwing op het gebeurtenislogboek van een activiteit met een nieuwe actiegroep maken met behulp van de Azure-portal
1. In de [portal](https://portal.azure.com), selecteer **Monitor**.

    ![De 'Monitor'-service](./media/monitoring-activity-log-alerts/home-monitor.png)
2. In de **activiteitenlogboek** sectie **waarschuwingen**.

    ![Het tabblad 'Waarschuwingen'](./media/monitoring-activity-log-alerts/alerts-blades.png)
3. Selecteer **toevoegen activiteit logboek waarschuwing**, en vul de velden in.

4. Voer een naam in de **waarschuwing logboeknaam activiteit** vak en selecteer een **beschrijving**.

    ![De opdracht 'Melding toevoegen activiteit log'](./media/monitoring-activity-log-alerts/add-activity-log-alert.png)

5. De **abonnement** vak autofills met uw huidige abonnement. Dit abonnement is de waarin de actiegroep wordt opgeslagen. De waarschuwing resource wordt geïmplementeerd voor dit abonnement en activiteit logboekgebeurtenissen hieruit bewaakt.

    ![Het dialoogvenster 'Activiteit logboek waarschuwing toevoegen' in](./media/monitoring-activity-log-alerts/activity-log-alert-new-action-group.png)

6. Selecteer de **resourcegroep** waarin de waarschuwing resource is gemaakt. Dit is niet de resourcegroep die wordt bewaakt door de waarschuwing. Het is in plaats daarvan de resourcegroep waar de waarschuwing bron zich bevindt.

7. Selecteer desgewenst een **gebeurteniscategorie** te wijzigen van de aanvullende filters die worden weergegeven. Administratieve gebeurtenissen, de filters omvatten **resourcegroep**, **Resource**, **brontype**, **bewerkingsnaam**, **niveau**, **Status**, en **gebeurtenis wordt gestart door**. Deze waarden bepalen welke gebeurtenissen moet worden gecontroleerd door deze waarschuwing.

    >[!NOTE]
    >U moet ten minste één van de bovenstaande criteria opgeven in de waarschuwing. U kunt een waarschuwing die wordt geactiveerd telkens wanneer een gebeurtenis wordt gemaakt in de activiteitenlogboeken van de niet maken.
    >
    >

8. Voer een naam in de **actie groepsnaam** vak en voer een naam in de **afkorting** vak. De korte naam wordt gebruikt in plaats van een volledige groep actienaam wanneer meldingen worden verzonden met behulp van deze groep.

9.  Een lijst van acties door te geven van de actie definiëren:

    a. **Naam**: Voer de naam, de alias of de id van de actie.

    b. **Actietype**: Selecteer SMS, e-mail of webhook.

    c. **Details**: op basis van het actietype, voer een telefoonnummer, e-mailadres of webhook URI.

10. Selecteer **OK** de waarschuwing wilt maken.

De waarschuwing duurt een paar minuten volledig doorgegeven en vervolgens worden actieve. Deze wordt geactiveerd wanneer nieuwe gebeurtenissen aan de waarschuwing criteria voldoen.

Zie voor meer informatie [begrijpen van de webhook-schema gebruikt in een logboek activiteitswaarschuwingen](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>De actiegroep gedefinieerd in deze stappen is herbruikbare als een bestaande actiegroep voor alle toekomstige definities van de waarschuwing.
>
>

## <a name="create-an-alert-on-an-activity-log-event-for-an-existing-action-group-by-using-the-azure-portal"></a>Maken van een waarschuwing op het gebeurtenislogboek van een activiteit voor een bestaande actiegroep met behulp van de Azure-portal
1. Volg de stappen 1 tot en met 7 in de vorige sectie uw logboek activiteit waarschuwing wilt maken.

2. Onder **melden**, selecteer de **bestaande** actieknop groep. Selecteer een bestaande actiegroep in de lijst.

3. Selecteer **OK** de waarschuwing wilt maken.

De waarschuwing duurt een paar minuten volledig doorgegeven en vervolgens worden actieve. Deze wordt geactiveerd wanneer nieuwe gebeurtenissen aan de waarschuwing criteria voldoen.

## <a name="manage-your-alerts"></a>Waarschuwingen beheren

Nadat u een waarschuwing gemaakt, is deze zichtbaar in de sectie waarschuwingen van de Monitor-blade. Selecteer de waarschuwing die u wilt beheren:

* Bewerken.
* Verwijder de groep.
* - Of uitschakelen, als u wilt tijdelijk stoppen of te hervatten ontvangen van meldingen voor de waarschuwing.

## <a name="next-steps"></a>Volgende stappen
- Ophalen van een [overzicht van waarschuwingen](monitoring-overview-alerts.md).
- Meer informatie over [melding snelheidsbeperking](monitoring-alerts-rate-limiting.md).
- Controleer de [activiteit logboek waarschuwing webhook schema](monitoring-activity-log-alerts-webhook.md).
- Meer informatie over [actiegroepen](monitoring-action-groups.md).  
- Meer informatie over [service health meldingen](monitoring-service-notifications.md).
- Maak een [activiteit logboek waarschuwing voor het bewaken van alle automatisch schalen engine bewerkingen voor uw abonnement](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).
- Maak een [activiteit logboek waarschuwing voor het bewaken van alle mislukte automatisch schalen scale-in/scale-out-bewerkingen op uw abonnement](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).

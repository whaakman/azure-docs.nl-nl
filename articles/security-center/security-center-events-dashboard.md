---
title: Controle en het verwerken van beveiligingsgebeurtenissen in Azure Security Center | Microsoft Docs
description: Lees hoe u gebeurtenissendashboard van Security Center kunt gebruiken om beveiligingsgebeurtenissen van uw Azure-VM's en niet-Azure-computers te bekijken.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: rkarlin
ms.openlocfilehash: 2b9426043619887d99003677fd6e8488cb56cce2
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57241065"
---
# <a name="monitoring-and-processing-security-events-in-azure-security-center"></a>Controle en het verwerken van beveiligingsgebeurtenissen in Azure Security Center
Het dashboard gebeurtenissen bevat een overzicht van het aantal beveiligingsgebeurtenissen verzameld voor tijd en een lijst van gebeurtenissen die aandacht vereisen die uw aandacht nodig hebben.  

> [!NOTE]
> Deze functie wilt gebruiken, moet uw werkruimte Log Analytics versie 2 actief zijn en moet op Security Center Standard-laag. Zie het Security Center [pagina met prijzen](security-center-pricing.md) voor meer informatie over de Standard-laag.
>
>

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="what-is-a-security-event"></a>Wat is een beveiligingsgebeurtenis?
Security Center maakt gebruik van de Microsoft Monitoring Agent voor het verzamelen van verschillende aan beveiliging gerelateerde configuraties en gebeurtenissen van uw machines en slaat deze gebeurtenissen in uw werkruimte(n). Voorbeelden van dergelijke gegevens zijn: besturingssysteemlogboeken (Windows-gebeurtenislogboeken), uitvoeren van processen en gebeurtenissen van beveiligingsoplossingen die zijn geïntegreerd met Security Center. De Microsoft Monitoring Agent kopieert ook crashdumpbestanden naar uw werkruimte(n).

## <a name="events-processed-dashboard"></a>Verwerkte gebeurtenissen-dashboard
U hebt tot de **gebeurtenissen** dashboard van de in het hoofdmenu van Security Center of de Security Center **overzicht** blade.  

![Verwerkte gebeurtenissen-dashboard][1]

De **gebeurtenissen** tegel onder **Security Center** geeft het aantal gebeurtenissen die worden doorgestuurd naar Security Center van uw Azure-VM's en niet-Azure-computers.

De **gebeurtenissendashboard** biedt een overzicht van het aantal verwerkte gebeurtenissen-overuren en een lijst van gebeurtenissen.

 ![Dashboard][2]

 Het bovenste gedeelte van het dashboard trends alle gebeurtenissen verwerkt in de afgelopen week. De onderste helft van het dashboard geeft een lijst van gebeurtenissen die aandacht vereisen en alle gebeurtenissen per type:

 - **Gebeurtenissen die aandacht vereisen** event-query's die Security Center biedt en gebeurtenis-query's die u maakt en toevoegt. Het dashboard biedt ook snel inzicht krijgt in de telling van elke gebeurtenis die aandacht vereist.
 - **Alle gebeurtenissen per type** bevat de typen gebeurtenissen die worden ontvangen en een telling voor elk type. Voorbeelden van het gebeurtenistype zijn SecurityEvent, CommonSecurityLog, WindowsFirewall en W3CIISLog.

> [!NOTE]
> Gebeurtenissen die aandacht vereisen opnemen [web baseline assessment](https://docs.microsoft.com/azure/operations-management-suite/oms-security-web-baseline-assessment). Het doel van de evaluatie van de webbasislijn is om mogelijk kwetsbare webserverinstellingen te vinden.

## <a name="view-processed-event-details"></a>Details van verwerkte gebeurtenissen weergeven
1. Onder de **Security Center** hoofdmenu, selecteer **gebeurtenissen**.
2. De **gebeurtenissendashboard** werkruimteselector kan worden geopend. Als u slechts één werkruimte hebt, wordt deze werkruimteselector niet weergegeven. Als u meer dan één werkruimte hebt, moet u Selecteer een werkruimte om de details van de verwerkte gebeurtenis weer te geven. Selecteer een werkruimte in de lijst hebt u meer dan één werkruimte.

  ![Lijst met werkruimten][3]

3. De **gebeurtenissendashboard** wordt geopend met de details van de gebeurtenis voor de geselecteerde werkruimte. U kunt de gebeurtenissen die aandacht vereisen en alle gebeurtenissen per type weergeven.  In dit voorbeeld wordt geselecteerd **gebeurtenissen die aandacht vereisen**.

  ![Gebeurtenis die aandacht vereist][4]

4. U kunt een query voor meer gegevens onder de werkruimte door een gebeurtenistype te selecteren. In dit voorbeeld wordt geselecteerd **SecurityEvent**.

  ![Een gebeurtenistype selecteren][5]

5. **Zoeken in logboeken** wordt geopend met aanvullende informatie over het gebeurtenistype.

  ![Zoekopdrachten in logboeken][6]

## <a name="add-a-notable-event"></a>Een gebeurtenis die aandacht vereist toevoegen
Security Center biedt gebeurtenissen die aandacht vereisen voor out-of-the-box. U kunt gebeurtenissen die aandacht vereisen op basis van uw eigen query met toevoegen de [Kusto-querytaal](../log-analytics/log-analytics-search-reference.md). Wordt geretourneerd en wordt naar de **gebeurtenissendashboard** om toe te voegen een gebeurtenis die aandacht vereist.

1. Selecteer **gebeurtenis die aandacht vereist toevoegen**.

  ![Een gebeurtenis die aandacht vereist toevoegen][7]

2. **Aangepaste gebeurtenis die aandacht vereist toevoegen** wordt geopend.  Onder **weergavenaam**, voer een naam voor de gebeurtenis die aandacht vereist. Onder **zoekquery**, voer de query voor de gebeurtenis.

  ![Voer uw query][8]

4. Selecteer **OK**.

## <a name="update-your-workspace-for-events-processing"></a>Bijwerken van uw werkruimte voor verwerking van gebeurtenissen
Uw werkruimte moet worden uitgevoerd van versie 2 van Log Analytics en moet op de standaardlaag van Security Center gebruik van de verwerking van gebeurtenissen in Security Center. De **gebeurtenissendashboard** werkruimteselector identificeert werkruimten die niet voldoen aan deze vereisten.

![Werkruimte voldoet niet aan vereisten][9]

Als de rij van de werkruimte:

- Bevat **vereist UPDATE** -u moet uw werkruimte bijwerken naar Log Analytics versie 2
- Bevat **UPGRADE abonnement** : u moet uw werkruimte upgraden naar Security Center Standard-laag
- Is blanco - voldoet aan uw werkruimte en een werkruimte selecteren, gaat u naar het dashboard

> [!NOTE]
> Onder **gebeurtenissendashboard**, wordt de **gebeurtenissen** kolom wordt aangegeven hoeveel gebeurtenissen in elke werkruimte.  Deze kolom is leeg voor een aantal werkruimten, omdat de gratis laag van Security Center wordt toegepast op deze werkruimte. Onder de gratis laag, Security Center verzamelt gebeurtenissen, maar de gebeurtenissen worden niet opgeslagen in Azure Monitor-logboeken en zijn niet beschikbaar in het dashboard.
>
>

## <a name="update-workspace-to-log-analytics-version-2"></a>Werkruimte bijwerken naar Log Analytics versie 2
1. Selecteer een werkruimte die **moet worden bijgewerkt**.
2. **Upgrade zoeken** wordt geopend. Selecteer **nu een upgrade uitvoeren**.

  ![Nu upgraden][10]

## <a name="upgrade-to-security-centers-standard-tier"></a>Een upgrade uitvoert naar Standard-laag van Security Center
1. Selecteer een werkruimte met **UPGRADE abonnement**.
2. **Gebeurtenissendashboard** wordt geopend. Selecteer **gebeurtenissendashboard proberen**.

  ![Probeer het dashboard][11]

3. Onder **Onboarding naar geavanceerde beveiliging**, selecteer de werkruimte die u wilt bijwerken.
4. Onder **prijzen**, selecteer **Standard**.
5. Selecteer **Opslaan**.

  ![Een upgrade uitvoert naar Standard-laag][12]

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u Event-dashboard van Security Center bevinden. Zie voor meer informatie over de werking van het dashboard en het schrijven van uw eigen query's van gebeurtenis:

- [Wat is Azure Monitor Logboeken?](../log-analytics/log-analytics-overview.md) – Overzicht van Azure Monitor-Logboeken
- [Understanding zoekopdrachten in Logboeken in Kusto](../log-analytics/log-analytics-log-search-new.md) : hierin wordt beschreven hoe zoekopdrachten in Logboeken van Azure Monitor worden gebruikt en concepten die moet worden geïnterpreteerd voor het maken van een zoeken in Logboeken
- [Verwijzing naar de Kusto](../log-analytics/log-analytics-search-reference.md) : informatie over het schrijven van uw eigen event-query's met behulp van de querytaal in logboek

Zie de volgende onderwerpen voor meer informatie over Azure Security Center:

- [Overzicht van Security Center](security-center-intro.md) – beschrijft Security Center de belangrijkste mogelijkheden

<!--Image references-->
[1]: ./media/security-center-events-dashboard/events-processed.png
[2]: ./media/security-center-events-dashboard/dashboard.png
[3]: ./media/security-center-events-dashboard/view-processed-event.png
[4]: ./media/security-center-events-dashboard/notable-event.png
[5]: ./media/security-center-events-dashboard/events-by-type.png
[6]: ./media/security-center-events-dashboard/log-search-detail.png
[7]: ./media/security-center-events-dashboard/add-notable-event.png
[8]: ./media/security-center-events-dashboard/create-query.png
[9]: ./media/security-center-events-dashboard/requires-update.png
[10]: ./media/security-center-events-dashboard/search-upgrade.png
[11]: ./media/security-center-events-dashboard/try-dashboard.png
[12]: ./media/security-center-events-dashboard/onboard-workspace.png

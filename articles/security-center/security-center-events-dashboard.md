---
title: Bewaking en het verwerken van beveiligingsgebeurtenissen in Azure Security Center | Microsoft Docs
description: Meer informatie over hoe u Security Center van gebeurtenissen-dashboard kunt gebruiken om beveiligingsgebeurtenissen van uw Azure VM's en niet-Azure-computers te bekijken.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: terrylan
ms.openlocfilehash: 367067874b167268bd690a9e0b55412e92e08122
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="monitoring-and-processing-security-events-in-azure-security-center"></a>Bewaking en het verwerken van beveiligingsgebeurtenissen in Azure Security Center
Het dashboard gebeurtenissen biedt een overzicht van het aantal beveiligingsgebeurtenissen gedurende een bepaalde tijd een lijst met belangrijke gebeurtenissen die uw aandacht nodig hebben verzameld.  

> [!NOTE]
> Deze functie wilt gebruiken, moet uw werkruimte logboekanalyse versie 2 actief zijn en moet op de standaardcategorie van Security Center. Zie het Beveiligingscentrum [pagina met prijzen](security-center-pricing.md) voor meer informatie over de prijscategorie Standard.
>
>

## <a name="what-is-a-security-event"></a>Wat is er een beveiligingsgebeurtenis?
Security Center maakt gebruik van Microsoft Monitoring Agent voor het verzamelen van verschillende beveiliging gerelateerde configuraties en gebeurtenissen van uw computers en slaat u deze gebeurtenissen in uw workspace(s). Voorbeelden van dergelijke gegevens zijn: systeemlogboeken (Windows-gebeurtenislogboeken), met besturingssysteem verwerkt, en gebeurtenissen van beveiligingsoplossingen geïntegreerd met Security Center. De Microsoft Monitoring Agent kopieert ook crashdumpbestanden naar uw werkruimte(n).

## <a name="events-processed-dashboard"></a>Verwerkte gebeurtenissen dashboard
U toegang tot de **gebeurtenissen** dashboard vanuit het hoofdmenu Security Center of Security Center **overzicht** blade.  

![Verwerkte gebeurtenissen dashboard][1]

De **gebeurtenissen** tegel onder **Security Center** geeft het aantal gebeurtenissen die binnenkomen in Security Center van uw Azure VM's en niet-Azure-computers.

De **gebeurtenissen dashboard** biedt een overzicht van het aantal verwerkte gebeurtenissen overuren en een lijst van gebeurtenissen.

 ![Dashboard][2]

 Het bovenste gedeelte van het dashboard trends alle gebeurtenissen verwerkt in de afgelopen week. De onderste helft van het dashboard vindt u belangrijke gebeurtenissen en alle gebeurtenissen die door het type:

 - **Opmerkelijke gebeurtenissen** omvatten event-query's die door Security Center biedt en gebeurtenis-query's die u maakt en toevoegt. Het dashboard biedt ook snel inzicht in de telling van elke gebeurtenis die aandacht vereisen.
 - **Alle gebeurtenissen per type** ziet u de typen gebeurtenissen die worden ontvangen en een telling voor elk type. Voorbeelden van het gebeurtenistype zijn SecurityEvent, CommonSecurityLog, WindowsFirewall en W3CIISLog.

> [!NOTE]
> Opmerkelijke gebeurtenissen omvatten [web-basislijn assessment](https://docs.microsoft.com/azure/operations-management-suite/oms-security-web-baseline-assessment). Het doel van de evaluatie van de webbasislijn is om mogelijk kwetsbare webserverinstellingen te vinden.

## <a name="view-processed-event-details"></a>Details van verwerkte gebeurtenissen weergeven
1. Onder de **Security Center** hoofdmenu, selecteer **gebeurtenissen**.
2. De **gebeurtenissen dashboard** werkruimte selector kan worden geopend. Als u slechts één werkruimte hebt, wordt deze werkruimte selector niet weergegeven. Als u meer dan één werkruimte hebt, moet u Selecteer een werkruimte om de verwerkte gebeurtenisdetails te bekijken. Selecteer een werkruimte in de lijst hebt u meer dan één werkruimte.

  ![Werkruimtelijst][3]

3. De **gebeurtenissen dashboard** geopend met de details van gebeurtenis voor de geselecteerde werkruimte. U kunt de opmerkelijke gebeurtenissen en alle gebeurtenissen per type weergeven.  In dit voorbeeld wordt geselecteerd **opmerkelijke gebeurtenissen**.

  ![Opmerkelijke gebeurtenis][4]

4. U kunt een query voor meer gegevens onder de werkruimte door het selecteren van een gebeurtenistype. In dit voorbeeld wordt geselecteerd **SecurityEvent**.

  ![Een gebeurtenistype selecteren][5]

5. **Meld u zoekopdracht** wordt geopend met aanvullende informatie over het gebeurtenistype.

  ![Zoekopdrachten in logboeken][6]

## <a name="add-a-notable-event"></a>Een belangrijke gebeurtenis toevoegen
Security Center biedt opmerkelijke out-of-the-box-gebeurtenissen. U kunt toevoegen opmerkelijke gebeurtenissen op basis van uw eigen query met de [querytaal van logboekanalyse](../log-analytics/log-analytics-search-reference.md). We keert terug naar de **gebeurtenissen dashboard** toevoegen van een gebeurtenis aandacht vereisen.

1. Selecteer **opmerkelijke gebeurtenis toevoegen**.

  ![Een belangrijke gebeurtenis toevoegen][7]

2. **Toevoegen van aangepaste opmerkelijke gebeurtenis** wordt geopend.  Onder **weergavenaam**, voer een naam voor uw aandacht vereisen gebeurtenis. Onder **zoekquery**, voer de query voor de gebeurtenis.

  ![Voer uw query][8]

4. Selecteer **OK**.

## <a name="update-your-workspace-for-events-processing"></a>Bijwerken van de werkruimte voor verwerking van gebeurtenissen
Uw werkruimte moet logboekanalyse versie 2 actief zijn en moet op Security Center de Standard-laag voor het gebruik van de verwerking van gebeurtenissen in Security Center. De **gebeurtenissen dashboard** werkruimte selector identificeert werkruimten die niet voldoen aan deze vereisten.

![Werkruimte voldoet niet aan vereisten][9]

Als de rij werkruimte:

- Bevat **vereist bijwerken** -moet u het bijwerken van uw werkruimte voor logboekanalyse versie 2
- Bevat **UPGRADE plannen** – u moet uw werkruimte bijwerken naar Standard-laag van Security Center
- Leeg - is uw werkruimte voldoet aan de vereisten en het selecteren van een werkruimte gaat u naar het dashboard

> [!NOTE]
> Onder **gebeurtenissen dashboard**, wordt de **gebeurtenissen** kolom wordt aangegeven hoeveelheid gebeurtenissen in elke werkruimte.  In deze kolom is leeg voor sommige werkruimtes omdat Security Center van gratis laag wordt toegepast op deze werkruimte. Onder de laag gratis Security Center verzamelt gebeurtenissen, maar de gebeurtenissen worden niet opgeslagen in Log Analytics en zijn niet beschikbaar in het dashboard.
>
>

## <a name="update-workspace-to-log-analytics-version-2"></a>Werkruimte voor logboekanalyse versie 2 bijwerken
1. Selecteer een werkruimte die **moet worden bijgewerkt**.
2. **Upgrade zoeken** wordt geopend. Selecteer **nu bijwerken**.

  ![Nu upgraden][10]

## <a name="upgrade-to-security-centers-standard-tier"></a>Een upgrade uitvoert naar de Standard-laag van Security Center
1. Selecteer een werkruimte met **UPGRADE plannen**.
2. **Gebeurtenissen dashboard** wordt geopend. Selecteer **probeer gebeurtenissen dashboard**.

  ![Probeer het dashboard][11]

3. Onder **voorbereiding voor geavanceerde beveiliging**, selecteer de werkruimte die u wilt upgraden.
4. Onder **prijzen**, selecteer **standaard**.
5. Selecteer **Opslaan**.

  ![Een upgrade uitvoert naar de Standard-laag][12]

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe Security Center van gebeurtenis-dashboard gebruiken. Zie voor meer informatie over de werking van het dashboard en uw eigen event-query's schrijven:

- [Wat is Log Analytics?](../log-analytics/log-analytics-overview.md) – Overzicht op Log Analytics
- [Understanding logboek zoekt in logboekanalyse](../log-analytics/log-analytics-log-search-new.md) - beschrijving van hoe logboek zoekopdrachten worden gebruikt in Log Analytics en concepten die worden begrepen voordat het maken van een zoekopdracht logboek
- [Log Analytics zoeken verwijzing](../log-analytics/log-analytics-search-reference.md) – informatie over het schrijven van uw eigen event-query's met behulp van de query language in logboek

Zie voor meer informatie over Security Center:

- [Security Center overzicht](security-center-intro.md) – Describes Security Center-belangrijkste mogelijkheden

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

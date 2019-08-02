---
title: Beveiligings gebeurtenissen bewaken en verwerken in Azure Security Center | Microsoft Docs
description: Meer informatie over hoe u het dash board gebeurtenissen van Security Center kunt gebruiken om beveiligings gebeurtenissen van uw Azure-Vm's en niet-Azure-computers te bekijken.
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
ms.openlocfilehash: 389aaee621251890cd3f75744a94b9c9b29c5695
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68662400"
---
# <a name="monitoring-and-processing-security-events-in-azure-security-center"></a>Beveiligings gebeurtenissen in Azure Security Center bewaken en verwerken
Het Events-dash board biedt een overzicht van het aantal beveiligings gebeurtenissen dat gedurende een bepaalde periode is verzameld en een lijst met belang rijke gebeurtenissen die uw aandacht kunnen vereisen.  

> [!NOTE]
> Het dash board voor beveiligings gebeurtenissen is op 31 juli 2019 buiten gebruik gesteld. Zie voor meer informatie en alternatieve Services de [buiten gebruiks telling van Security Center-functies (2019 juli)](security-center-features-retirement-july2019.md#menu_events).

## <a name="what-is-a-security-event"></a>Wat is een beveiligings gebeurtenis?
Security Center gebruikt de micro soft Monitoring Agent voor het verzamelen van verschillende aan beveiliging gerelateerde configuraties en gebeurtenissen van uw computers en slaat deze gebeurtenissen op in uw werk ruimte (n). Voor beelden van dergelijke gegevens zijn: Logboeken van het besturings systeem (Windows-gebeurtenis Logboeken), actieve processen en gebeurtenissen van beveiligings oplossingen die zijn geïntegreerd met Security Center. De Microsoft Monitoring Agent kopieert ook crashdumpbestanden naar uw werkruimte(n).

## <a name="requirements"></a>Vereisten
Als u deze functie wilt gebruiken, moet op uw werk ruimte Log Analytics versie 2 worden uitgevoerd en de standaardlaag van Security Center zijn. Zie de [pagina met prijzen](security-center-pricing.md) voor Security Center voor meer informatie over de Standard-laag.

## <a name="events-processed-dashboard"></a>Verwerkte gebeurtenissen in het dash board
U opent het dash board **gebeurtenissen** vanuit het hoofd menu van Security Center of de Blade **overzicht** Security Center.  

![Verwerkte gebeurtenissen in het dash board][1]

De tegel **gebeurtenissen** onder **Security Center** geeft het aantal gebeurtenissen weer dat in Security Center van uw Azure-Vm's en niet-Azure-computers loopt.

Het **Events-dash board** biedt een overzicht van het aantal verwerkte gebeurtenissen en een lijst met gebeurtenissen.

 ![Dashboard][2]

 In het bovenste gedeelte van het dash board worden alle gebeurtenissen die in de afgelopen week zijn verwerkt, trends doorgevoerd. In de onderste helft van het dash board vindt u een overzicht van belang rijke gebeurtenissen en alle gebeurtenissen op type:

 - Belang rijke **gebeurtenissen** zijn onder andere gebeurtenis query's die Security Center levert en gebeurtenis query's die u maakt en toevoegt. Het dash board biedt ook een snel overzicht van de telling van elke belang rijke gebeurtenis.
 - **Alle gebeurtenissen op type** toont de gebeurtenis typen die worden ontvangen en een telling voor elk type. Voor beelden van gebeurtenis typen zijn SecurityEvent, CommonSecurityLog, WindowsFirewall en W3CIISLog.

> [!NOTE]
> Belang rijke gebeurtenissen zijn onder andere [evaluatie](https://docs.microsoft.com/azure/operations-management-suite/oms-security-web-baseline-assessment)van de webbasislijn. Het doel van de evaluatie van de webbasislijn is om mogelijk kwetsbare webserverinstellingen te vinden.

## <a name="view-processed-event-details"></a>Details van verwerkte gebeurtenis weer geven
1. Selecteer in het hoofd menu van Security Center **gebeurtenissen**.
2. De **dash board** -werk ruimte van de gebeurtenissen kan worden geopend. Als u slechts één werk ruimte hebt, wordt deze werkruimte kiezer niet weer gegeven. Als u meer dan één werk ruimte hebt, moet u een werk ruimte selecteren om de verwerkte gebeurtenis details weer te geven. Selecteer een werk ruimte in de lijst als u meer dan één werk ruimte hebt.

   ![Werkruimte lijst][3]

3. Het **dash board gebeurtenissen** wordt geopend met de details van de gebeurtenis voor de geselecteerde werk ruimte. U kunt de belangrijkste gebeurtenissen en alle gebeurtenissen per type weer geven.  In dit voor beeld hebben we een opvallende **gebeurtenis**geselecteerd.

   ![Gebeurtenis die aandacht vereist][4]

4. U kunt een query uitvoeren op meer gegevens in de werk ruimte door een gebeurtenis type te selecteren. In dit voor beeld hebben we **SecurityEvent**geselecteerd.

   ![Een gebeurtenis type selecteren][5]

5. **Zoeken** in Logboeken wordt geopend met aanvullende details over het gebeurtenis type.

   ![Zoekopdrachten in logboeken][6]

## <a name="add-a-notable-event"></a>Een belang rijke gebeurtenis toevoegen
Security Center voorziet in out-of-the-box belang rijke gebeurtenissen. U kunt belang rijke gebeurtenissen toevoegen op basis van uw eigen query met behulp van de [Kusto-query taal](../log-analytics/log-analytics-search-reference.md). We gaan terug naar het **Events-dash board** om een belang rijke gebeurtenis toe te voegen.

1. Selecteer een opvallende **gebeurtenis toevoegen**.

   ![Een belang rijke gebeurtenis toevoegen][7]

2. **Aangepaste gebeurtenis toevoegen** wordt geopend.  Voer onder **weergave naam**een naam in voor uw belang rijke gebeurtenis. Voer onder **Zoek query**uw query in voor de gebeurtenis.

   ![Voer uw query in][8]

4. Selecteer **OK**.

## <a name="update-your-workspace-for-events-processing"></a>Uw werk ruimte bijwerken voor het verwerken van gebeurtenissen
Voor uw werk ruimte moet Log Analytics versie 2 worden uitgevoerd en moet de laag standaard van Security Center zijn voor het gebruik van gebeurtenis verwerking in Security Center. De **gebeurtenissen dashboard** werkruimte selectie identificeert werk ruimten die niet aan deze vereisten voldoen.

![De werk ruimte voldoet niet aan de vereisten][9]

Als de rij met de werk ruimte:

- Bevat **updates vereist** : u moet uw werk ruimte bijwerken naar log Analytics versie 2
- Bevat een **upgrade plan** : u moet uw werk ruimte upgraden naar de Standard-laag van Security Center
- Is leeg: uw werk ruimte voldoet aan de vereisten en door een werk ruimte te selecteren gaat u naar het dash board

> [!NOTE]
> Onder **gebeurtenissen-dash board**geeft de kolom **gebeurtenissen** de hoeveelheid gebeurtenissen in elke werk ruimte aan.  Deze kolom is leeg voor sommige werk ruimten omdat de gratis laag van Security Center wordt toegepast op die werk ruimte. Onder de laag gratis verzamelt Security Center gebeurtenissen, maar de gebeurtenissen worden niet opgeslagen in Azure Monitor logboeken en zijn niet beschikbaar in het dash board.
>
>

## <a name="update-workspace-to-log-analytics-version-2"></a>Werk ruimte bijwerken naar Log Analytics versie 2
1. Selecteer een werk ruimte die **moet worden bijgewerkt**.
2. **Zoek upgrade** wordt geopend. Selecteer **Nu bijwerken**.

   ![Nu upgraden][10]

## <a name="upgrade-to-security-centers-standard-tier"></a>Upgrade uitvoeren naar de Standard-laag van Security Center
1. Selecteer een werk ruimte met een **upgrade plan**.
2. Het **dash board gebeurtenissen** wordt geopend. Selecteer **dash board gebeurtenissen try**.

   ![Dash board uitproberen][11]

3. Onder **onboarding naar geavanceerde beveiliging**selecteert u de werk ruimte die u wilt bijwerken.
4. Selecteer onder **prijzen**de optie **standaard**.
5. Selecteer **Opslaan**.

   ![Upgrade uitvoeren naar Standard-laag][12]

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u het gebeurtenis Dashboard van Security Center kunt gebruiken. Zie voor meer informatie over de werking van het dash board en het schrijven van uw eigen gebeurtenis query's:

- [Wat is Azure Monitor logboeken?](../log-analytics/log-analytics-overview.md) – Overzicht van Azure Monitor-logboeken
- [Zoek opdrachten in Logboeken in Kusto](../log-analytics/log-analytics-log-search-new.md) : beschrijft hoe logboek zoekopdrachten in azure monitor logboeken worden gebruikt en biedt concepten die moeten worden begrepen voordat een zoek opdracht in het logboek wordt gemaakt.
- [Naslag](../log-analytics/log-analytics-search-reference.md) informatie voor Kusto: Leer hoe u uw eigen gebeurtenis query's schrijft met behulp van de query taal in het logboek

Zie de volgende onderwerpen voor meer informatie over Azure Security Center:

- [Overzicht van Security Center](security-center-intro.md) : beschrijft de belangrijkste mogelijkheden van Security Center

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

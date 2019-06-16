---
title: Beveiligingsoplossingen integreren in Azure Security Center | Microsoft Docs
description: Leer hoe Azure Security Center kan worden geïntegreerd met partners om de algehele beveiliging van uw Azure-resources te verbeteren.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/20/2019
ms.author: rkarlin
ms.openlocfilehash: d94567800a9fd020784c9cb07b2c6824cd032509
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67064275"
---
# <a name="integrate-security-solutions-in-azure-security-center"></a>Beveiligingsoplossingen integreren in Azure Security Center
Dit document helpt u bij het beheren van beveiligingsoplossingen die al zijn gekoppeld aan Azure Security Center en bij het toevoegen van nieuwe oplossingen.

> [!NOTE]
> Een subset van beveiligingsoplossingen stopgezet op 31 juli 2019. Zie voor meer informatie en andere services, [buiten gebruik stellen van Security Center-functies (juli 2019)](security-center-features-retirement-july2019.md#menu_solutions).

## <a name="integrated-azure-security-solutions"></a>Geïntegreerde Azure-beveiligingsoplossingen
Met Security Center kunt u gemakkelijk geïntegreerde beveiligingsoplossingen in Azure inschakelen. Voordelen zijn:

- **Eenvoudige implementatie**: Security Center biedt gestroomlijnd inrichten van geïntegreerde partneroplossingen. Met Security Center kan op virtuele machines de benodigde agent worden ingericht voor oplossingen zoals het beoordelen van antimalware en beveiligingsproblemen. Voor firewallapparaten kan in Security Center een groot gedeelte van de vereiste netwerkconfiguratie worden uitgevoerd.
- **Geïntegreerde detectie**: Beveiligingsgebeurtenissen van partneroplossingen worden automatisch verzameld, samengevoegd en weergegeven als meldingen en incidenten in Security Center. Deze gebeurtenissen worden ook gekoppeld aan detecties van andere bronnen voor geavanceerde detectie van bedreigingen.
- **Geïntegreerde statuscontrole en beheer**: Klanten kunnen geïntegreerde statusgebeurtenissen gebruiken om in één oogopslag alle partneroplossingen te controleren. Er zijn basisbeheermogelijkheden beschikbaar, met eenvoudige toegang tot geavanceerde installatie met behulp van de partneroplossing.

Op dit moment geïntegreerde beveiligingsoplossingen omvatten voor evaluatie van beveiligingsproblemen door [Qualys](https://www.qualys.com/public-clouds/microsoft-azure/) en [Rapid7](https://www.rapid7.com/products/insightvm/) en Microsoft Application Gateway Web application firewall.

> [!NOTE]
> Security Center installeert de Microsoft Monitoring Agent niet op virtuele apparaten van partners omdat de meeste leveranciers van beveiligingsoplossingen het niet toestaan dat externe agents worden uitgevoerd op hun apparaat.
>
>

## <a name="how-security-solutions-are-integrated"></a>Beveiligingsoplossingen integreren
Azure-beveiligingsoplossingen die zijn geïmplementeerd vanuit Security Center, zijn automatisch verbonden. U kunt ook verbinding maken met andere gegevensbronnen voor beveiliging, inclusief computers waarop on-premises of in andere clouds.

![Integratie van partneroplossingen](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

## <a name="manage-integrated-azure-security-solutions-and-other-data-sources"></a>Geïntegreerde Azure-beveiligingsoplossingen en andere gegevensbronnen beheren

1. Meld u aan bij [Azure Portal](https://azure.microsoft.com/features/azure-portal/).

2. Ga naar het menu van **Microsoft Azure** en selecteer **Security Center**. **Security Center - Overzicht** wordt geopend.

3. Selecteer in het menu van Security Center de optie **Beveiligingsoplossingen**.

   ![Overzicht van Security Center](./media/security-center-partner-integration/overview.png)

Bij **beveiligingsoplossingen** kunt u informatie bekijken over de status van de geïntegreerde Azure-beveiligingsoplossingen en algemene beheertaken uitvoeren. U kunt ook andere typen gegevensbronnen voor beveiliging verbinden, zoals Azure Active Directory Identity Protection-waarschuwingen en firewall-logboeken in CEF (Common Event Format).

### <a name="connected-solutions"></a>Verbonden oplossingen

Het gedeelte **Verbonden oplossingen** bevat beveiligingsoplossingen die momenteel zijn verbonden met Security Center en informatie over de status van elke oplossing.  

![Verbonden oplossingen](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

De status van een partneroplossing kan zijn:

* In orde (groen) - er is geen probleem met de status.
* Niet in orde (rood): er is een probleem met de status waarvoor onmiddellijke aandacht is vereist.
* Statusproblemen (oranje): de oplossing is gestopt melden van de status.
* Niet gerapporteerd (grijs): de oplossing heeft alles wat niet gerapporteerd nog, de status van een oplossing mogelijk ondersteuningsteams als deze onlangs is verbonden en is nog steeds de implementatie, of geen statusgegevens beschikbaar is.

> [!NOTE]
> Als de gegevens van status niet beschikbaar is, ziet u Security Center de datum en tijd van de laatste gebeurtenis ontvangen om aan te geven of de oplossing al dan niet rapporteert. Als er geen statusgegevens beschikbaar is en er zijn geen waarschuwingen worden ontvangen binnen de afgelopen 14 dagen, Security Center geeft aan dat de oplossing beschadigd of niet rapporteren is.
>
>

1. Selecteer **weergave** voor meer informatie en opties, inclusief:

   - **Oplossingenconsole**. Hiermee opent u de ervaring voor deze oplossing.
   - **VM koppelen**. Hiermee opent u de blade toepassingen koppelen. Hier kunt u resources verbinden met de partneroplossing.
   - **Oplossing verwijderen**.
   - **Configureer**.

   ![Details van partneroplossingen](./media/security-center-partner-solutions/partner-solutions-detail.png)

### <a name="discovered-solutions"></a>Gedetecteerde oplossingen

Security Center detecteert automatisch beveiligingsoplossingen die worden uitgevoerd in Azure maar niet zijn verbonden met Security Center, en geeft deze oplossingen weer in het gedeelte **Gedetecteerde oplossingen**. Hierbij gaat het zowel om Azure-oplossingen, zoals [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection), als om partneroplossingen.

> [!NOTE]
> De Standard-laag van Security Center is op abonnementsniveau vereist voor de functie Gedetecteerde oplossingen. Ga naar [deze pagina](security-center-pricing.md) voor meer informatie over de prijzen van Security Center.
>
>

Selecteer **CONNECT** onder een oplossing om met Security Center te integreren en meldingen over beveiligingswaarschuwingen te ontvangen.

![Gedetecteerde oplossingen](./media/security-center-partner-integration/security-center-partner-integration-fig5.png)

Security Center detecteert ook oplossingen die worden uitgevoerd in het abonnement en die Common Event Format-logboeken (CEF) kunnen doorsturen. Informatie over hoe u [een beveiligingsoplossing met Security Center verbindt](quick-security-solutions.md) die CEF-logboeken gebruikt.

### <a name="add-data-sources"></a>Gegevensbronnen toevoegen

Het gedeelte **Gegevensbronnen toevoegen** bevat andere beschikbare gegevensbronnen die kunnen worden verbonden. Klik op **TOEVOEGEN** voor instructies over het toevoegen van gegevens uit een van deze bronnen.

![Gegevensbronnen](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)

## <a name="exporting-data-to-a-siem"></a>Exporteren van gegevens naar een SIEM

Verwerkte gebeurtenissen die worden geproduceerd door Azure Security Center worden gepubliceerd naar de Azure [activiteitenlogboek](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), een van het logboek voor bestandstypen die beschikbaar zijn via Azure Monitor. Azure Monitor biedt een geconsolideerde pijplijn voor de routering van uw bewakingsgegevens naar een SIEM-hulpprogramma. Dit wordt gedaan door het streamen van die gegevens naar een Event Hub waar deze kan vervolgens worden opgehaald naar een partner-hulpprogramma.

Maakt gebruik van deze pipe de [één pijplijn Azure Monitoring](../azure-monitor/platform/stream-monitoring-data-event-hubs.md) voor het verkrijgen van toegang aan de bewakingsgegevens uit uw Azure-omgeving. Hiermee kunt u eenvoudig instellen van siem's en controleprogramma's gebruiken voor de gegevens.

De volgende secties wordt beschreven hoe u gegevens kunnen worden gestreamd naar een event hub kunt configureren. De stappen wordt ervan uitgegaan dat u al geconfigureerd in uw Azure-abonnement van Azure Security Center hebt.

Overzicht

![Overzicht op hoog niveau](media/security-center-export-data-to-siem/overview.png)

### <a name="what-is-the-azure-security-data-exposed-to-siem"></a>Wat zijn de gegevens van de Azure-beveiliging blootgesteld aan SIEM?

In deze versie stellen we de [beveiligingswaarschuwingen.](../security-center/security-center-managing-and-responding-alerts.md) In toekomstige versies zullen we de gegevensset met aanbevelingen voor beveiliging verrijken.

### <a name="how-to-setup-the-pipeline"></a>Het instellen van de pijplijn

#### <a name="create-an-event-hub"></a>Een Event Hub maken

Voordat u begint, moet u [maken van een Event Hubs-naamruimte](../event-hubs/event-hubs-create.md). Deze naamruimte en Event Hub is de bestemming voor al uw bewakingsgegevens.

#### <a name="stream-the-azure-activity-log-to-event-hubs"></a>Stream het Azure-activiteitenlogboek naar Eventhubs

Raadpleeg het volgende artikel [activiteitenlogboek streamen naar Event Hubs](../azure-monitor/platform/activity-logs-stream-event-hubs.md)

#### <a name="install-a-partner-siem-connector"></a>Een partner SIEM-connector installeren 

Routering van uw bewakingsgegevens naar een Event Hub met Azure Monitor kunt u eenvoudig kunt integreren met SIEM-partner- en controlehulpprogramma's.

Raadpleeg de volgende koppeling om te zien van de lijst met [ondersteunde siem's](../azure-monitor/platform/stream-monitoring-data-event-hubs.md#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub)

### <a name="example-for-querying-data"></a>Voorbeeld voor het opvragen van gegevens 

Hier volgt een aantal Splunk-query's die u kunt gebruiken voor het ophalen van waarschuwingsgegevens:

| **Beschrijving van Query** | **Query** |
|----|----|
| Alle waarschuwingen| index=main Microsoft.Security/locations/alerts|
| Aantal bewerkingen met hun naam samenvatten| index = belangrijkste sourcetype = "amal: security" \| tabel operationName \| statistieken operationName tellen|
| Waarschuwingen ophalen: Tijd, naam, status, -ID en -abonnement | index=main Microsoft.Security/locations/alerts \| table \_time, properties.eventName, State, properties.operationId, am_subscriptionId |


## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u kunnen lezen hoe u partneroplossingen integreert in Security Center. Zie de volgende artikelen voor meer informatie over Security Center:

* [Beveiligingsstatus controleren in Security Center](security-center-monitoring.md). Meer informatie over het controleren van de status van uw Azure-resources.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md). Krijg antwoorden op veelgestelde vragen over het gebruik van Security Center.
* [Azure-beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/). Raadpleeg de blogberichten over beveiliging en naleving in Azure.

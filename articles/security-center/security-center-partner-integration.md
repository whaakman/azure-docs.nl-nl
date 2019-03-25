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
ms.openlocfilehash: e756a0a7af9ad89e3aad8b0dbe27a870a3f855c1
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58400938"
---
# <a name="integrate-security-solutions-in-azure-security-center"></a>Beveiligingsoplossingen integreren in Azure Security Center
Dit document helpt u bij het beheren van beveiligingsoplossingen die al zijn gekoppeld aan Azure Security Center en bij het toevoegen van nieuwe oplossingen.

## <a name="integrated-azure-security-solutions"></a>Geïntegreerde Azure-beveiligingsoplossingen
Met Security Center kunt u gemakkelijk geïntegreerde beveiligingsoplossingen in Azure inschakelen. Voordelen zijn:

- **Eenvoudige implementatie**: Security Center biedt gestroomlijnd inrichten van geïntegreerde partneroplossingen. Met Security Center kan op virtuele machines de benodigde agent worden ingericht voor oplossingen zoals het beoordelen van antimalware en beveiligingsproblemen. Voor firewallapparaten kan in Security Center een groot gedeelte van de vereiste netwerkconfiguratie worden uitgevoerd.
- **Geïntegreerde detectie**: Beveiligingsgebeurtenissen van partneroplossingen worden automatisch verzameld, samengevoegd en weergegeven als meldingen en incidenten in Security Center. Deze gebeurtenissen worden ook gekoppeld aan detecties van andere bronnen voor geavanceerde detectie van bedreigingen.
- **Geïntegreerde statuscontrole en beheer**: Klanten kunnen geïntegreerde statusgebeurtenissen gebruiken om in één oogopslag alle partneroplossingen te controleren. Er zijn basisbeheermogelijkheden beschikbaar, met eenvoudige toegang tot geavanceerde installatie met behulp van de partneroplossing.

Geïntegreerde beveiligingsoplossingen omvatten momenteel:

- Firewall voor webtoepassingen ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/products.html) en [Azure Application Gateway](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/))
- Firewall van de volgende generatie ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](https://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2), [Cisco](https://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html) en [Palo Alto Networks](https://www.paloaltonetworks.com/products))
- Evaluatie van beveiligingsproblemen ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/) en [Rapid7](https://www.rapid7.com/products/insightvm/))

> [!NOTE]
> Security Center installeert de Microsoft Monitoring Agent niet op virtuele apparaten van partners omdat de meeste leveranciers van beveiligingsoplossingen het niet toestaan dat externe agents worden uitgevoerd op hun apparaat.
>
>

## <a name="how-security-solutions-are-integrated"></a>Beveiligingsoplossingen integreren
Azure-beveiligingsoplossingen die zijn geïmplementeerd vanuit Security Center, zijn automatisch verbonden. U kunt ook verbinding maken met andere gegevensbronnen voor beveiliging, waaronder:

- Azure AD-identiteitsbeveiliging
- Computers die on-premises of in andere clouds worden uitgevoerd
- Een beveiligingsoplossing die ondersteuning biedt voor CEF (Common Event Format)
- Microsoft Advanced Threat Analytics

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

### <a name="connect-external-solutions"></a>Verbinding maken met externe oplossingen

U kunt niet alleen beveiligingsgegevens verzamelen van uw computers, maar u kunt ook beveiligingsgegevens integreren van tal van andere beveiligingsoplossingen, waaronder alle oplossingen die CEF (Common Event Format) ondersteunen. CEF is een standaardindeling in de branche voor Syslog-berichten. De indeling wordt door veel leveranciers van beveiligingsproducten gebruikt om gebeurtenisintegratie tussen verschillende platforms mogelijk te maken.

In deze snelstartgids leert u het volgende:
- Een beveiligingsoplossing met Security Center verbinden met behulp van CEF-logboeken
- De verbinding met de beveiligingsoplossing valideren

#### <a name="prerequisites"></a>Vereisten
U moet over een abonnement op Microsoft Azure beschikken om met Security Center aan de slag te gaan. Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/).

Om deze zelfstartgids te doorlopen, moet u zich in de Standard-prijscategorie van Security Center bevinden. U kunt Security Center Standard kosteloos proberen. In de snelstartgids [Onboard your Azure subscription to Security Center Standard](security-center-get-started.md) (Uw Azure-abonnement registreren voor Security Center Standard) wordt u begeleid bij het upgraden naar Standard. Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie.

U hebt ook een [Linux-machine](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-linux) nodig, waarop de Syslog-service al is verbonden met uw Security Center.

#### <a name="connect-solution-using-cef"></a>Oplossing verbinden met behulp van CEF

1. Meld u aan bij de [Azure Portal](https://azure.microsoft.com/features/azure-portal/).
2. Ga naar het **Microsoft Azure**-menu en selecteer **Security Center**. **Security Center - Overzicht** wordt geopend.

    ![Security Center selecteren](./media/quick-security-solutions/quick-security-solutions-fig1.png)  

3. Selecteer in het hoofdmenu van Security Center de optie **Beveiligingsoplossingen**.
4. Op de pagina Beveiligingsoplossingen gaat u naar **Gegevensbronnen toevoegen (3)** en klikt u onder **Common Event Format** op **Toevoegen**.

    ![Gegevensbron toevoegen](./media/quick-security-solutions/quick-security-solutions-fig2.png)

5. Vouw op de pagina Common Event Format-logboeken de tweede stap, **Syslog doorsturen configureren om de vereiste logboeken te zenden naar de agent op UDP-poort 25226**, uit en volg de onderstaande instructies op uw Linux-computer:

    ![Syslog configureren](./media/quick-security-solutions/quick-security-solutions-fig3.png)

6. Vouw de derde stap, **Het configuratiebestand van de agent op de agentcomputer plaatsen**, uit en volg de onderstaande instructies op uw Linux-computer:

    ![Agent configuration](./media/quick-security-solutions/quick-security-solutions-fig4.png)

7. Vouw de derde stap, **De syslog-daemon en de agent opnieuw starten**, uit en volg de onderstaande instructies op uw Linux-computer:

    ![De syslog opnieuw starten](./media/quick-security-solutions/quick-security-solutions-fig5.png)


#### <a name="validate-the-connection"></a>De verbinding valideren

Voordat u verder gaat met onderstaande stappen, moet u wachten tot de syslog is gestart met de rapportage aan Security Center. Dit kan enige tijd duren, afhankelijk van de grootte van de omgeving.

1.  Klik in het linkerdeelvenster van het dashboard van Security Center op **Zoeken**.
2.  Selecteer de werkruimte waarmee de Syslog (Linux-computer) is verbonden.
3.  Typ *CommonSecurityLog* en klik op de knop **Zoeken**.

Het volgende voorbeeld toont het resultaat van deze stappen: ![CommonSecurityLog](./media/quick-security-solutions/common-sec-log.png)

#### <a name="clean-up-resources"></a>Resources opschonen
Andere snelstartgidsen en zelfstudies in deze verzameling zijn gebaseerd op deze snelstartgids. Als u de volgende snelstartgidsen en zelfstudies ook wilt doornemen, blijf dan de prijscategorie Standard gebruiken en houd automatische inrichting ingeschakeld. Als u niet wilt doorgaan of wilt terugkeren naar de laag gratis:

1. Ga terug naar het hoofdmenu van Security Center en selecteer **Beveiligingsbeleid**.
2. Selecteer het abonnement of het beleid dat u wilt terugzetten op Gratis. **Beveiligingsbeleid** wordt geopend.
3. Selecteer onder **BELEIDSONDERDELEN** de optie **Prijscategorie**.
4. Selecteer **Gratis** om het abonnement te wijzigen van de Standard-laag in de Gratis laag.
5. Selecteer **Opslaan**.

Als u automatisch inrichten wilt uitschakelen:

1. Ga terug naar het hoofdmenu van Security Center en selecteer **Beveiligingsbeleid**.
2. Selecteer het abonnement waarvoor u automatisch inrichten wilt uitschakelen.
3. Ga naar **Beveiligingsbeleid – Gegevensverzameling** en selecteer onder **Onboarding** de optie **Uit** om automatisch inrichten uit te schakelen.
4. Selecteer **Opslaan**.

>[!NOTE]
> Wanneer u automatische inrichting uitschakelt, wordt MMA niet verwijderd van Azure-VM's waarop de agent is ingericht. Door automatische inrichting uit te schakelen, wordt de beveiligingsbewaking voor uw resources beperkt.
>

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
* [Partneroplossingen controleren met Security Center](security-center-partner-solutions.md). Meer informatie over het bewaken van de status van uw partneroplossingen.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md). Krijg antwoorden op veelgestelde vragen over het gebruik van Security Center.
* [Azure-beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/). Raadpleeg de blogberichten over beveiliging en naleving in Azure.

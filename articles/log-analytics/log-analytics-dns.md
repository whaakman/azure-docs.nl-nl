---
title: DNS-Analytics-oplossing in Azure Log Analytics | Microsoft Docs
description: Instellen en gebruiken van de DNS-Analytics-oplossing in logboekanalyse voor het verzamelen van inzicht in de DNS-infrastructuur op beveiliging, prestaties en bewerkingen.
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f44a40c4-820a-406e-8c40-70bd8dc67ae7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: banders
ms.openlocfilehash: 0e8fc0ffb8e0d0bdf00bea46594fe050c00b6c8e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="gather-insights-about-your-dns-infrastructure-with-the-dns-analytics-preview-solution"></a>Inzicht in uw DNS-infrastructuur met de DNS-Analytics Preview oplossing verzamelen

![DNS-Analytics symbool](./media/log-analytics-dns/dns-analytics-symbol.png)

In dit artikel wordt beschreven hoe instellen en gebruiken van de Azure DNS-Analytics-oplossing in Azure-logboekanalyse voor het verzamelen van inzicht in de DNS-infrastructuur op beveiliging, prestaties en bewerkingen.

DNS-Analytics helpt u bij:

- Clients identificeren die op te lossen schadelijke domeinnamen.
- Identificeer bronrecords.
- Vaak worden aangevraagd domeinnamen en interactie DNS-clients te identificeren.
- Weergave werklast op DNS-servers.
- Dynamische DNS-registratiefouten weergeven.

De oplossing verzamelt, analyseert en correleert Windows DNS-analyse en controlelogboeken en andere gerelateerde gegevens uit uw DNS-servers.

## <a name="connected-sources"></a>Verbonden bronnen

De volgende tabel beschrijft de verbonden bronnen die worden ondersteund door deze oplossing:

| **Verbonden bron** | **Ondersteuning** | **Beschrijving** |
| --- | --- | --- |
| [Windows-agents](log-analytics-windows-agents.md) | Ja | De oplossing verzamelt DNS-informatie van Windows-agents. |
| [Linux-agents](log-analytics-linux-agents.md) | Nee | De oplossing verzamelt geen DNS-informatie van directe Linux-agents. |
| [System Center Operations Manager-beheergroep](log-analytics-om-agents.md) | Ja | De oplossing verzamelt DNS-gegevens van agents in een verbonden beheergroep van Operations Manager. Directe verbinding met de Operations Management Suite van Operations Manager-agent is niet vereist. Gegevens uit de beheergroep doorgestuurd naar de Operations Management Suite-opslagplaats. |
| [Azure Storage-account](log-analytics-azure-storage.md) | Nee | Azure-opslag wordt niet gebruikt door de oplossing. |

### <a name="data-collection-details"></a>Details van de verzameling gegevens

De oplossing verzamelt inventaris van de DNS- en DNS-gebeurtenissen gerelateerde gegevens uit de DNS-servers waarop een Log Analytics-agent is geïnstalleerd. Deze gegevens vervolgens geüpload naar Log Analytics en weergegeven in het dashboard van oplossing. Inventarisatie-gerelateerde gegevens, zoals het aantal DNS-servers, zones en bronrecords, is verzameld door het uitvoeren van de DNS-PowerShell-cmdlets. De gegevens worden bijgewerkt om de twee dagen. De gebeurtenis-gerelateerde gegevens worden verzameld bijna realtime uit de [analytische en controlelogboeken](https://technet.microsoft.com/library/dn800669.aspx#enhanc) geleverd door verbeterde DNS-logboekregistratie en diagnostische gegevens in Windows Server 2012 R2.

## <a name="configuration"></a>Configuratie

Gebruik de volgende informatie voor het configureren van de oplossing:

- U moet hebben een [Windows](log-analytics-windows-agents.md) of [Operations Manager](log-analytics-om-agents.md) -agent op elke DNS-server die u wilt bewaken.
- U kunt de DNS-Analytics-oplossing toevoegen aan uw Operations Management Suite-werkruimte van de [Azure Marketplace](https://aka.ms/dnsanalyticsazuremarketplace). U kunt ook de procedure beschreven in [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md).

De oplossing wordt het verzamelen van gegevens zonder de noodzaak van verdere configuratie gestart. Echter, u kunt de volgende configuratie gegevensverzameling aanpassen.

### <a name="configure-the-solution"></a>De oplossing configureren

Klik op het dashboard van de oplossing **configuratie** om de configuratie van de DNS-pagina te openen. Er zijn wijzigingen in de configuratie die u kunt twee typen:

- **Goedgekeurde lijst van domeinnamen**. De oplossing verwerkt geen alle lookup-query's. Een whitelist domeinnaamachtervoegsels houdt. De lookup-query's die worden omgezet in de domeinnamen die overeenkomen met domeinnaamachtervoegsels in deze lijst met geaccepteerde niet zijn verwerkt door de oplossing. Niet verwerken van domeinnamen wilt plaatsen helpt bij het optimaliseren van de gegevens die worden verzonden met logboekanalyse. De standaard goedgekeurde bevat populaire openbare domein-namen, zoals www.google.com en www.facebook.com. U kunt de volledige lijst met weergeven door te schuiven.

 U kunt de lijst om toe te voegen, het achtervoegsel van een naam die u wilt weergeven van lookup insights voor wijzigen. U kunt ook het achtervoegsel van een naam die u niet wilt weergeven van lookup insights voor verwijderen.

- **Interactie Client drempelwaarde**. DNS-clients die groter is dan de drempelwaarde voor het aantal lookup-aanvragen zijn gemarkeerd in de **DNS-Clients** blade. De drempelwaarde is standaard 1000. U kunt de drempelwaarde bewerken.

    ![Goedgekeurde lijst domeinnamen](./media/log-analytics-dns/dns-config.png)

## <a name="management-packs"></a>Management packs

Als u van Microsoft Monitoring Agent gebruikmaakt verbinding maken met uw Operations Management Suite-werkruimte, worden de volgende management pack is geïnstalleerd:

- Microsoft DNS-gegevens Collector Intelligence Pack (Microsft.IntelligencePacks.Dns)

Als uw Operations Manager-beheergroep is verbonden met uw Operations Management Suite-werkruimte, worden de volgende management packs in Operations Manager geïnstalleerd wanneer u deze oplossing toevoegt. Er is geen vereiste configuratie of onderhoud van deze management packs:

- Microsoft DNS-gegevens Collector Intelligence Pack (Microsft.IntelligencePacks.Dns)
- Microsoft System Center Advisor DNS-configuratie (Microsoft.IntelligencePack.Dns.Configuration)

Zie [Operations Manager koppelen aan Log Analytics](log-analytics-om-agents.md) voor meer informatie over de manier waarop uw management packs voor oplossingen worden bijgewerkt.

## <a name="use-the-dns-analytics-solution"></a>De DNS-Analytics-oplossing gebruiken

Deze sectie wordt uitgelegd dat het dashboard functies en hoe ze te gebruiken.

Nadat u de oplossing aan uw werkruimte hebt toegevoegd, biedt de tegel oplossing op de pagina overzicht van Operations Management Suite een snel overzicht van de DNS-infrastructuur. Dit omvat het aantal DNS-servers waar de gegevens worden verzameld. Dit omvat ook het aantal aanvragen door clients voor het oplossen van schadelijke domeinen in de afgelopen 24 uur. Wanneer u op de tegel klikt, wordt het dashboard van de oplossing wordt geopend.

![DNS-Analytics tegel](./media/log-analytics-dns/dns-tile.png)

### <a name="solution-dashboard"></a>Dashboard van de oplossing

Dashboard van de oplossing bevat een samenvatting van de gegevens voor de verschillende functies van de oplossing. Dit omvat ook koppelingen naar de gedetailleerde weergave voor forensische analyse en diagnose. Standaard worden de gegevens weergegeven voor de afgelopen zeven dagen. U kunt het bereik van de datum en tijd wijzigen met behulp van de **van datum / tijd-Selectiebesturingselement**, zoals wordt weergegeven in de volgende afbeelding:

![Besturingselement van de selectie tijd](./media/log-analytics-dns/dns-time.png)

Dashboard van de oplossing ziet u de volgende blades:

**DNS-beveiliging**. De DNS-clients die probeert te communiceren met schadelijke domeinen rapporten. Met behulp van Microsoft threat intelligence feeds kunt Analytics DNS client-IP-adressen die u probeert toegang te krijgen van schadelijke domeinen detecteren. In veel gevallen bellen malware geïnfecteerde apparaten' ' naar het midden 'opdracht en controle' van het domein schadelijke door de malware-domeinnaam op te lossen.

![Blade van DNS-beveiliging](./media/log-analytics-dns/dns-security-blade.png)

Wanneer u een client IP-adres in de lijst klikt, wordt logboek zoekopdracht wordt geopend en de lookup worden details weergegeven van de respectieve query. In het volgende voorbeeld, DNS-Analytics gedetecteerd dat de communicatie is klaar met een [IRCbot](https://www.microsoft.com/security/portal/threat/encyclopedia/entry.aspx?Name=Win32/IRCbot):

![Logboek zoekresultaten ircbot weergeven](./media/log-analytics-dns/ircbot.png)

De informatie helpt u bij het identificeren van de:

- Client-IP dat de communicatie wordt gestart.
- De domeinnaam die wordt omgezet in het schadelijke IP-adres.
- IP-adressen die de domeinnaam wordt omgezet in.
- Schadelijke IP-adres.
- Ernst van het probleem.
- De reden voor het beleid van het schadelijke IP-adres.
- Tijd van de detectie.

**Domeinen die zijn opgevraagd**. Biedt de meest voorkomende domeinnamen die door de DNS-clients in uw omgeving wordt opgevraagd. U kunt de lijst met alle domeinnamen opgevraagd weergeven. U kunt ook inzoomen op de details van de aanvraag opzoeken van een specifieke domeinnaam in het logboek zoeken.

![Domeinen opgevraagde blade](./media/log-analytics-dns/domains-queried-blade.png)

**DNS-Clients**. Rapporten van de clients *inbreuk op de drempelwaarde* voor het aantal query's in de opgegeven tijdsperiode. U kunt de lijst van alle DNS-clients en de details van de query's die door deze in het logboek zoeken weergeven.

![Blade van DNS-Clients](./media/log-analytics-dns/dns-clients-blade.png)

**Dynamische DNS-registraties**. Rapportnaam registratiefouten. Alle registratiefouten voor adres [bronrecords](https://en.wikipedia.org/wiki/List_of_DNS_record_types) (Type A en AAAA) samen met de client-IP-adressen die de van registratieaanvragen worden gemarkeerd. U kunt deze informatie vervolgens gebruiken om te vinden van de hoofdoorzaak van het mislukken van de registratie met de volgende stappen:

1. Zoek de zone die is gemachtigd voor de naam die de client wilt bijwerken.

2. De oplossing gebruiken om te controleren van de informatie over de inventaris van deze zone.

3. Controleer of de dynamische update voor de zone is ingeschakeld.

4. Controleer of de zone voor beveiligde dynamische updates is geconfigureerd of niet.

    ![Blade voor dynamische DNS-registratie](./media/log-analytics-dns/dynamic-dns-reg-blade.png)

**Naam van de van registratieaanvragen**. De bovenste tegel ziet u een trendlijn van geslaagde en mislukte DNS-aanvragen voor dynamische updates. De onderste tegel vermeldt de top 10-clients die mislukte DNS-updates aanvragen naar de DNS-servers verzendt, gesorteerd op basis van het aantal fouten.

![De naam van registratie van aanvragen-blade ](./media/log-analytics-dns/name-reg-req-blade.png)

**Steekproef DDI analysequery's**. Bevat een lijst van de meest algemene zoekquery's die rechtstreeks onbewerkte analytische gegevens ophalen.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Voorbeeldquery 's](./media/log-analytics-dns/queries.png)

U kunt deze query's als uitgangspunt gebruiken voor het maken van uw eigen query's voor aangepaste rapportage. De query's koppelen aan de DNS-Analytics logboek zoekpagina waar de resultaten worden weergegeven:

- **Lijst met DNS-Servers**. Geeft een lijst weer van alle DNS-servers en hun bijbehorende FQDN-naam, domeinnaam, naam van het forest en server IP-adressen.
- **Lijst met DNS-Zones**. Geeft een lijst weer van alle DNS-zones met de bijbehorende zonenaam, de status van de dynamische update, naamservers en DNSSEC-ondertekeningsstatus.
- **Ongebruikte bronrecords**. Bevat een overzicht van alle de ongebruikte/verouderd bronrecords. Deze lijst bevat de naam van de resource record, bronrecordtype, de bijbehorende DNS-server, record genereren en zonenaam. U kunt deze lijst gebruiken voor het identificeren van de DNS-bronrecords die niet langer in gebruik. Op basis van deze informatie, kunt u vervolgens deze vermeldingen verwijderen uit de DNS-servers.
- **DNS-Servers opvragen Load**. Geeft informatie weer, zodat u kunt een perspectief van de DNS-belasting op de DNS-servers ophalen. Deze informatie kunt u bij het plannen van de capaciteit voor de servers. Gaat u naar de **metrische gegevens** tabblad in de weergave te wijzigen naar een visualisatie in grafische. Deze weergave helpt u begrijpen hoe de DNS-belasting is verdeeld over de DNS-servers. Deze DNS-query snelheid laat trends zien voor elke server.

    ![Zoekresultaten voor DNS-servers query-logboek](./media/log-analytics-dns/dns-servers-query-load.png)

- **DNS-Zones Query Load**. Geeft de statistieken van de DNS-zone query per seconde van alle zones op de DNS-servers worden beheerd door de oplossing. Klik op de **metrische gegevens** tab om te wijzigen van de weergave van gedetailleerde records naar een grafische visualisatie van de resultaten.
- **Configuratiegebeurtenissen**. Bevat alle DNS-configuratie wijzigen-gebeurtenissen en bijbehorende berichten. Vervolgens kunt u deze gebeurtenissen op basis van tijd van de server van de DNS-gebeurtenis met gebeurtenis-ID filteren of taakcategorie. De gegevens kunt u wijzigingen in specifieke DNS-servers op specifieke tijdstippen controleren.
- **DNS-analytische logboek**. Geeft alle analytische gebeurtenissen op alle DNS-servers die worden beheerd door de oplossing. Vervolgens kunt u filteren deze gebeurtenissen op basis van tijd van de gebeurtenis met gebeurtenis-ID, DNS-server, client-IP-die de lookup-query en query type taakcategorie. DNS-server analysegebeurtenissen inschakelen activiteit op de DNS-server bijhouden. Een analytisch gebeurtenis wordt geregistreerd telkens wanneer de server worden verzonden of ontvangen van de DNS-informatie.

### <a name="search-by-using-dns-analytics-log-search"></a>Zoeken met behulp van DNS-Analytics logboek zoeken

U kunt een query maken op de pagina logboek zoeken. U kunt uw zoekresultaten filteren met behulp van besturingselementen facet. U kunt ook een geavanceerde query's op transformatie en filter het rapport maken op uw resultaten. Start met behulp van de volgende query's:

1. In de **query zoekvak**, type `Type=DnsEvents` om alle DNS-gebeurtenissen die worden gegenereerd door de DNS-servers worden beheerd door de oplossing weer te geven. De lijst met resultaten de logboekgegevens voor alle gebeurtenissen met betrekking tot lookup-query's, dynamische registraties en configuratiewijzigingen.

    ![DnsEvents logboek zoeken](./media/log-analytics-dns/log-search-dnsevents.png)  

    a. Als u wilt de logboekgegevens voor lookup-query's weergeven, selecteert u **LookUpQuery** als de **Subtype** filter uit het besturingselement facet aan de linkerkant. Een tabel waarin de gebeurtenissen voor de query lookup voor de geselecteerde periode wordt weergegeven.

    b. Als u wilt de logboekgegevens voor dynamische registraties weergeven, selecteert u **DynamicRegistration** als de **Subtype** filter uit het besturingselement facet aan de linkerkant. Een tabel waarin alle dynamische registratie-gebeurtenissen voor de geselecteerde periode wordt weergegeven.

    c. Als u wilt weergeven van de logboekgegevens voor wijzigingen in de configuratie, selecteer **ConfigurationChange** als de **Subtype** filter uit het besturingselement facet aan de linkerkant. Een tabel waarin alle wijzigingsgebeurtenissen voor de configuratie voor de geselecteerde periode wordt weergegeven.

2. In de **query zoekvak**, type `Type=DnsInventory` om alle DNS-inventarisatie-gerelateerde gegevens voor de DNS-servers worden beheerd door de oplossing weer te geven. De lijst met resultaten de logboekgegevens voor DNS-servers, DNS-zones en bronrecords.

    ![DnsInventory logboek zoeken](./media/log-analytics-dns/log-search-dnsinventory.png)

## <a name="feedback"></a>Feedback

Er zijn twee manieren waarop u feedback kunt geven:

- **UserVoice**. Na de ideeën voor DNS-Analytics functies werken. Ga naar de [Operations Management Suite UserVoice-pagina](https://aka.ms/dnsanalyticsuservoice).
- **Deelnemen aan onze cohort**. We altijd geïnteresseerd in nieuwe klanten deelnemen aan onze cohorten om vroege toegang tot de nieuwe functies en helpen bij het verbeteren van DNS-Analytics hebben. Als u geïnteresseerd bent in onze cohorten toevoegen, vult u [deze snelle enquête](https://aka.ms/dnsanalyticssurvey).

## <a name="next-steps"></a>Volgende stappen

[Zoeken in een logboek](log-analytics-log-searches.md) om gedetailleerde DNS-records in het logboek weer te geven.

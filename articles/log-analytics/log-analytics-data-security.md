---
title: Meld u Analytics gegevensbeveiliging | Microsoft Docs
description: Meer informatie over hoe logboekanalyse beschermt u uw privacy en uw gegevens beveiligt.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: a33bb05d-b310-4f2c-8f76-f627e600c8e7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: magoedte
ms.openlocfilehash: f14b96b88a96f4bef24602bb9338a77352fbf375
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="log-analytics-data-security"></a>Meld u Analytics-gegevensbeveiliging
Dit document is bedoeld om specifieke informatie van de Azure-logboekanalyse als aanvulling op de informatie op te geven [Azure Vertrouwenscentrum](../security/security-microsoft-trust-center.md).  

Dit artikel wordt uitgelegd hoe gegevens verzameld, verwerkt en beveiligd door logboekanalyse. Agents kunt u verbinding maken met de webservice, System Center Operations Manager gebruiken voor het verzamelen van operationele gegevens of gegevens ophalen uit Azure diagnostics voor gebruik door logboekanalyse. 

De Log Analytics-service beheert uw cloud-gebaseerde gegevens veilig met behulp van de volgende methoden:

* gegevensscheiding
* Bewaartijd van gegevens
* Fysieke beveiliging
* Incidentbeheer
* Naleving
* standaarden veiligheidscertificaten

Neem contact met ons met eventuele vragen, suggesties of problemen met betrekking tot een van de volgende informatie, met inbegrip van ons beveiligingsbeleid op [Azure ondersteuningsopties](http://azure.microsoft.com/support/options/).

## <a name="data-segregation"></a>gegevensscheiding
Nadat uw gegevens wordt ingenomen door de Log Analytics-service, is de gegevens geïsoleerd logisch op elk onderdeel in de service. Alle gegevens per werkruimte is gecodeerd. Deze markering blijft aanwezig gedurende de levenscyclus van de gegevens en deze wordt afgedwongen op elke laag van de service. Uw gegevens worden opgeslagen in een specifieke database in het cluster in de regio die u hebt geselecteerd.

## <a name="data-retention"></a>Bewaartijd van gegevens
Geïndexeerde logboekgegevens zoeken is opgeslagen en behouden volgens uw plan prijscategorie. Zie voor meer informatie [Log Analytics prijzen](https://azure.microsoft.com/pricing/details/log-analytics/).

Als onderdeel van uw [abonnementsovereenkomst](https://azure.microsoft.com/support/legal/subscription-agreement/), Microsoft uw gegevens conform de overeenkomst, behouden.  Wanneer de gegevens wordt verwijderd, verwijderen we ook het Azure-Opslagaccount waarin de gegevens zich bevindt.  Wanneer gegevens van de klant wordt verwijderd, worden er geen fysieke schijven vernietigd.  

De volgende tabel vindt u enkele van de beschikbare oplossingen en vindt u voorbeelden van het type van de gegevens die zij verzamelen.

| **Oplossing** | **Gegevenstypen** |
| --- | --- |
| Capaciteit en prestaties |Prestatiegegevens en metagegevens |
| Malware-evaluatie |Configuratiegegevens en metagegevens |
| Updatebeheer |Metagegevens en de statusgegevens |
| Logboekbeheer |Gebruiker gedefinieerde gebeurtenislogboeken, Windows-gebeurtenislogboeken en/of de IIS-logboeken |
| Tracering wijzigen |Software-inventaris, Windows-service en Linux-daemon metagegevens en de metagegevens van de Windows-/ Linux-bestand |
| SQL- en beoordeling van de Active Directory |WMI-gegevens, registergegevens, prestatiegegevens en SQL Server dynamische Beheerweergave resultaten weergeven |

De volgende tabel ziet u voorbeelden van gegevenstypen:

| **Gegevenstype** | **Velden** |
| --- | --- |
| Waarschuwing |Waarschuwing naam, beschrijving van de waarschuwing, BaseManagedEntityId, probleem-ID, IsMonitorAlert, RuleId, oplossingsstatus, prioriteit, ernst, categorie, eigenaar, ResolvedBy, TimeRaised, TimeAdded, LastModified, LastModifiedBy, LastModifiedExceptRepeatCount, TimeResolved, TimeResolutionStateLastModified, TimeResolutionStateLastModifiedInDB, RepeatCount |
| Configuratie |CustomerID, AgentID, id van de entiteit, ManagedTypeID, ManagedTypePropertyID, CurrentValue, ChangeDate |
| Gebeurtenis |Gebeurtenis-id, EventOriginalID, BaseManagedEntityInternalId, RuleId, PublisherId, PublisherName, FullNumber, getal, categorie, ChannelLevel, LoggingComputer, EventData, EventParameters, TimeGenerated, TimeAdded <br>**Opmerking:** wanneer u gebeurtenissen met aangepaste velden in het Windows-gebeurtenislogboek schrijft, OMS ze verzamelt. |
| Metagegevens |BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, IP-adres, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-adres, NetbiosDomainName, LogicalProcessors, DNSName, DisplayName, DomainDnsName, ActiveDirectorySite, primaire naam, OffsetInMinuteFromGreenwichTime |
| Prestaties |Objectnaam, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded |
| Status |StateChangeEventId, StateId, NewHealthState, OldHealthState, Context, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified |

## <a name="physical-security"></a>Fysieke beveiliging
De Log Analytics-service wordt beheerd door medewerkers van Microsoft en alle activiteiten die worden geregistreerd en kunnen worden gecontroleerd. Log Analytics wordt beheerd als een Azure-Service en voldoet aan alle vereisten voor Azure-compatibiliteit en beveiliging. U kunt meer informatie over de fysieke beveiliging van Azure activa weergeven op pagina 18 van de [beveiligingsoverzicht van Microsoft Azure](http://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf). Fysieke toegangsrechten voor het beveiligen van gebieden worden gewijzigd binnen één werkdag voor iedereen die verantwoordelijk is voor de OMS-service, inclusief overdracht en beëindiging beschikt niet langer over. U kunt meer informatie over globale fysieke infrastructuur gebruiken we op [Microsoft-Datacenters](https://www.microsoft.com/server-cloud/cloud-os/global-datacenters.aspx).

## <a name="incident-management"></a>Incidentbeheer
OMS heeft een incidentbeheerproces die voor alle Microsoft-services worden aangehouden. Samengevat: we:

* Een gedeelde verantwoordelijkheid model waarin een deel van de verantwoordelijkheid van de beveiliging van Microsoft behoort tot en een deel aan de klant gebruiken
* Azure beveiligingsincidenten beheren:
  * Start een onderzoek na detectie van een incident
  * Evalueer de impact en urgentie van een incident door een teamlid voor respons op incidenten op oproep. Op basis van bewijs, de beoordeling kan of kan niet resulteren in meer escalatie naar het security response team.
  * Een incident door beveiliging antwoord deskundigen aan het technische of forensisch onderzoek verrichten, identificeren containment risicobeperking en tijdelijke oplossing strategieën onderzoeken. Als het beveiligingsteam gelooft dat gegevens van de klant kan hebben worden blootgesteld aan een persoon onrechtmatig of niet-geautoriseerde, begint de parallelle uitvoering van het proces van de klant Incident melding parallel.  
  * Regulering en het herstellen van het incident. Het team respons op incidenten wordt gemaakt van een herstelplan bij om het probleem te verhelpen. Crisis containment stappen zoals betrokken systemen in quarantaine plaatsen kunnen optreden onmiddellijk en parallel met diagnose. Langer term oplossingen kunnen worden gepland die zich voordoen nadat de onmiddellijke risico is verstreken.  
  * Sluit het incident en een post-keuring uitvoeren. Het team respons op incidenten maakt een post-keuring die geeft een overzicht van de details van het incident, met de bedoeling te herzien beleidsregels, procedures en processen om te voorkomen dat een herhaling van de gebeurtenis.
* Gebruikers informeren over beveiligingsincidenten:
  * Bepalen van het bereik van de betrokken klanten en geef iedereen die als een aankondiging mogelijk gedetailleerde wordt beïnvloed
  * Maak een aankondiging voor klanten met gedetailleerde voldoende informatie zodat ze kunnen een onderzoek op hun einde uitvoeren en voldoen aan eventuele wanneer die ze in hun eindgebruikers hebt aangebracht, terwijl niet ten onrechte meldingen vertragen.
  * Bevestig en declareren van het incident, indien nodig.
  * Klanten met een incident melding zonder onredelijk vertraging en in overeenstemming met alle juridische of contractueel streven informeren. Meldingen van beveiligingsincidenten worden geleverd op een of meer van de klant beheerders door middel van die Microsoft selecteert, inclusief via e-mail.
* Team gereedheid voor en training uitvoeren:
  * Medewerkers van Microsoft zijn vereist voor het voltooien van de beveiligings- en awareness training, waardoor ze om te bepalen en mogelijke beveiligingsproblemen rapporteren.  
  * Operators werken op de Microsoft Azure-service hebben toevoeging training verplichtingen omringende toegang krijgen tot gevoelige systemen die als host fungeert voor gegevens van de klant.
  * Medewerkers van Microsoft security response ontvangen speciale training voor hun rollen

Als het verlies van enige klantgegevens optreedt, wordt elke klant melden binnen één dag. Klant-gegevensverlies is echter nooit opgetreden met de service. 

Zie voor meer informatie over hoe Microsoft moet op beveiligingsincidenten reageren [reactie van Microsoft Azure-beveiliging in de Cloud](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/1/Microsoft Azure Security Response in the cloud.pdf).

## <a name="compliance"></a>Naleving
Log Analytics software ontwikkelings- en service van het team informatie beveiliging en beheeracties programma ondersteunt de zakelijke vereisten en voldoet aan regelgeving zoals beschreven op [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) en [ Microsoft Trust Center naleving](https://www.microsoft.com/TrustCenter/Compliance/default.aspx). Hoe logboekanalyse beveiligingsvereisten vaststelt, identificeert beveiligingsmechanismen beheert en bewaakt de risico's worden er ook beschreven. Jaarlijks, wij controleren beleidsregels, standaarden, procedures en richtlijnen.

Elk teamlid ontwikkeling ontvangt beveiligingstraining formele toepassing. We gebruiken een versiebeheersysteem intern voor softwareontwikkeling. Elk software-project wordt beveiligd door het versiebeheersysteem.

Microsoft heeft een beveiliging en naleving team dat verantwoordelijk en alle services in Microsoft evalueert. De vaststelling van informatie van het team maken en ze niet zijn gekoppeld aan de technische afdelingen die logboekanalyse ontwikkelt. De beveiliging verantwoordelijken hebben hun eigen Managementketen en uitvoering van een onafhankelijke beoordelingen van producten en services om te controleren of de beveiliging en naleving.

Raad van bestuur van Microsoft is een melding via een jaarlijkse rapport over alle informatie beveiligingsprogramma's bij Microsoft.

Het team logboekanalyse software ontwikkelings- en -service werkt actief samen met de teams Microsoft Legal en naleving en andere zakenpartners verschillende certificaten verkrijgen.

## <a name="certifications-and-attestations"></a>Certificaten en verklaringen
Azure Log Analytics voldoet aan de volgende vereisten:

* [ISO/IEC 27001](http://www.iso.org/iso/home/standards/management-standards/iso27001.htm)
* [ISO/IEC 27018:2014](http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498)
* [ISO 22301](https://azure.microsoft.com/blog/iso22301/)
* [Payment Card Industry (PCI compatibele) gegevens beveiligingsstandaard (PCI DSS)](https://www.microsoft.com/en-us/TrustCenter/Compliance/PCI) door de PCI Security Standards Raad.
* [Type service organisatie besturingselementen (SOC) 1 1 en SOC 2 Type 1](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2) compatibel
* [HIPAA en HITECH](https://www.microsoft.com/TrustCenter/Compliance/HIPAA) voor bedrijven die een HIPAA-overeenkomst voor het koppelen van bedrijven hebben
* Windows gemeenschappelijke Engineering Criteria
* Microsoft Trustworthy Computing
* Als een Azure-service voldoen de onderdelen die gebruikmaakt van logboekanalyse aan nalevingsvereisten van Azure. U vindt meer op [Microsoft Trust Center naleving](https://www.microsoft.com/TrustCenter/Compliance/default.aspx).

> [!NOTE]
> In sommige certificeringen/verklaringen Log Analytics wordt vermeld onder de oude naam van *Operational Insights*.
>
>

## <a name="cloud-computing-security-data-flow"></a>Cloudcomputing beveiliging gegevensstroom
Het volgende diagram toont een cloud-beveiligingsarchitectuur als de stroom van gegevens van uw bedrijf en hoe deze wordt beveiligd, zoals is verplaatst naar de Log Analytics-service, uiteindelijk bekijken door u in de Azure-portal of de klassieke OMS-portal. Meer informatie over elke stap volgt in het diagram.

![Afbeelding van logboekanalyse gegevensverzameling en beveiliging](./media/log-analytics-data-security/log-analytics-data-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. Aanmelden voor logboekanalyse en gegevens verzamelen
Voor uw organisatie gegevens verzenden naar Log Analytics, moet u een Windows- of Linux-agent uitgevoerd op virtuele machines in Azure of op virtuele of fysieke computers in uw omgeving of andere cloudprovider configureren.  Als u Operations Manager gebruikt, uit de beheergroep u Operations Manager-agent. Gebruikers (dit is mogelijk u afzonderlijke gebruikers of een groep personen) een of meer Log Analytics-werkruimten maken en registreren van agents met een van de volgende accounts:

* [Organisatie-ID](../active-directory/sign-up-organization.md)
* [Microsoft-Account - Outlook, Office Live, MSN](http://www.microsoft.com/account/default.aspx)

Een werkruimte voor logboekanalyse is de gegevens wordt verzameld, samengevoegd, geanalyseerd en gepresenteerd. Een werkruimte wordt voornamelijk gebruikt om partitiegegevens te en elke werkruimte uniek is. U wilt bijvoorbeeld uw productiegegevens die worden beheerd door een werkruimte en uw testgegevens die worden beheerd door een andere werkruimte. Een beheerder gebruikerstoegang helpen werkruimten ook bij de gegevens. Elke werkruimte kan meerdere gebruikersaccounts die zijn gekoppeld, en elke gebruikersaccount hebben toegang tot meerdere Log Analytics-werkruimten. U maakt op basis van regio datacenter werkruimten. Elke werkruimte wordt gerepliceerd naar andere datacentra in de regio, hoofdzakelijk voor beschikbaarheid van Log Analytics-service.

Voor Operations Manager, de beheergroep van Operations Manager een verbinding maakt met de Log Analytics-service. U configureert welke agent beheerde systemen in de beheergroep zijn toegestaan voor het verzamelen en verzenden van gegevens naar de service. Gegevens van deze oplossingen zijn afhankelijk van de oplossing die u hebt ingeschakeld, hetzij rechtstreeks vanuit een Operations Manager-beheerserver wordt verzonden naar de Log Analytics-service, of vanwege de hoeveelheid gegevens die worden verzameld door het systeem door agents beheerde rechtstreeks vanuit worden verzonden de agent met de service. Voor systemen die niet worden bewaakt door Operations Manager, verbindt elk veilig naar de Log Analytics-service rechtstreeks.

Alle communicatie tussen verbonden systemen en de Log Analytics-service is versleuteld.  Het TLS (HTTPS)-protocol wordt gebruikt voor versleuteling.  Het Microsoft SDL-proces wordt om ervoor te zorgen dat logboekanalyse is bijgewerkt door de meest recente ontwikkelingen in cryptografieprotocollen gevolgd.

Elk type agent verzamelt gegevens voor logboekanalyse. Het type van de gegevens die worden verzameld is afhankelijk van de typen van oplossingen die worden gebruikt. U ziet een overzicht van het verzamelen van gegevens op [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md). Meer gedetailleerde informatie van de verzameling is daarnaast beschikbaar voor de meeste oplossingen. Een oplossing is een bundel van vooraf gedefinieerde weergaven, logboek zoekquery's, regels voor het verzamelen van gegevens en logica voor verwerking. Alleen beheerders kunnen Log Analytics gebruiken voor het importeren van een oplossing. Nadat de oplossing is geïmporteerd, wordt deze verplaatst naar de Operations Manager-beheerservers (indien gebruikt), en vervolgens naar alle agents die u hebt gekozen. Daarna wordt verzamelen de agents de gegevens.

## <a name="2-send-data-from-agents"></a>2. Verzenden van gegevens van agents
U alle typen van de agent met een sleutel inschrijving registreren en een beveiligde verbinding tot stand is gebracht tussen de agent en de Log Analytics-service met behulp van certificaten gebaseerde verificatie en SSL met poort 443. Log Analytics maakt gebruik van een geheime store te genereren en te onderhouden van sleutels. Persoonlijke sleutels om de 90 dagen worden gedraaid en worden opgeslagen in Azure en worden beheerd door de Azure-bewerkingen die strikt regelgeving en naleving procedures te volgen.

Met Operations Manager stelt u de beheergroep die is geregistreerd bij een werkruimte voor logboekanalyse een beveiligde HTTPS-verbinding met een Operations Manager-beheerserver.

Voor Windows of Linux-agents worden uitgevoerd op virtuele machines in Azure, wordt een alleen-lezen opslagsleutel gebruikt om te lezen van diagnostische gebeurtenissen in de Azure-tabellen.  

Met een agent die rapporteert aan een beheergroep van Operations Manager die is geïntegreerd met Log Analytics, als de beheerserver kan niet communiceren met is de service voor een of andere reden de verzamelde gegevens lokaal opgeslagen in een tijdelijke cache op de management -Server.   Ze proberen te verzenden de gegevens van elke acht minuten gedurende 2 uur.  Voor gegevens die de beheerserver omzeilt en rechtstreeks naar het Log Analytics is verzonden, is het gedrag consistent met de Windows-agent.  

De Windows- of management server-agent in de cache opgeslagen gegevens wordt beveiligd door de referentie-archief van het besturingssysteem. Als de service de gegevens niet na twee uur verwerken kan, wordt de gegevens van de agents de verzendwachtrij. Als de wachtrij vol is, start de agent verwijderen van gegevenstypen, beginnen met de prestatiegegevens. De wachtrijlimiet agent is een registersleutel zodat u wijzigen kunt, indien nodig. Verzamelde gegevens wordt gecomprimeerd en verzonden naar de service, het omzeilen van de databases van Operations Manager management groep, zodat voegt geen belasting toe. Nadat de verzamelde gegevens worden verzonden, wordt deze verwijderd uit de cache.

Zoals hierboven wordt beschreven, wordt gegevens uit de beheerserver of rechtstreeks aangesloten agents via SSL naar Microsoft Azure-datacenters verzonden. Desgewenst kunt u ExpressRoute voor aanvullende beveiliging voor de gegevens. ExpressRoute is een manier om rechtstreeks verbinding maken met Azure vanaf uw eigen WAN-netwerk, zoals een multiprotocol label switching (MPLS) VPN, geleverd door een netwerkserviceprovider. Zie voor meer informatie [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

## <a name="3-the-log-analytics-service-receives-and-processes-data"></a>3. De Log Analytics-service ontvangt en verwerkt gegevens
De service logboekanalyse zorgt ervoor dat binnenkomende gegevens van een vertrouwde bron door het valideren van certificaten en de integriteit van gegevens met Azure-verificatie. De niet-verwerkte onbewerkte gegevens wordt vervolgens opgeslagen in een Azure Event Hub in de regio die de gegevens in rust uiteindelijk worden opgeslagen. Het type gegevens dat is opgeslagen, is afhankelijk van de typen van oplossingen die zijn geïmporteerd en gebruikt om gegevens te verzamelen. Vervolgens wordt de logboekanalyse service processen de onbewerkte gegevens en het opgenomen in de database.

De bewaarperiode van verzamelde gegevens die zijn opgeslagen in de database, is afhankelijk van de geselecteerde prijsstelling. Voor de *vrije* laag, verzamelde gegevens zijn beschikbaar voor 7 dagen. Voor de *betaalde* laag, verzamelde gegevens is beschikbaar voor 31 dagen standaard, maar kan worden uitgebreid naar 720 dagen. Gegevens worden versleuteld in rust in de Azure-opslag, om te controleren of de vertrouwelijkheid van gegevens opgeslagen. De afgelopen twee weken van gegevens worden ook opgeslagen in cache op basis van SSD en deze cache is momenteel niet versleuteld.  We zullen deze codering in de hoger helft van 2018 ondersteunen.  

## <a name="4-use-log-analytics-to-access-the-data"></a>4. Log Analytics gebruiken voor toegang tot de gegevens
Voor toegang tot uw werkruimte voor logboekanalyse moet je je aanmelden bij de Azure-portal met behulp van de organisatie-account of de Microsoft-account dat u eerder hebt ingesteld. Al het verkeer tussen de portal en Log Analytics-service is verzonden via een beveiligd kanaal voor HTTPS. Wanneer u de portal gebruikt, een sessie-ID is gegenereerd op de client gebruiker (webbrowser) en gegevens worden opgeslagen in een lokale cache totdat de sessie wordt beëindigd. Wanneer is beëindigd, wordt de cache verwijderd. Client-side cookies, die geen persoonsgegevens bevatten, worden niet automatisch verwijderd. Sessiecookies HTTPOnly zijn gemarkeerd en zijn beveiligd. Na een vooraf bepaalde niet-actieve periode, is de Azure portal sessie beëindigd.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het verzamelen van gegevens met Log Analytics voor uw Azure VM's na de [Quick Start de virtuele machine van Azure](log-analytics-quick-collect-azurevm.md).  

*  Als u zoeken wilt voor het verzamelen van gegevens van fysieke of virtuele Windows- of Linux-computers in uw omgeving, raadpleegt u de [Quick Start voor Linux-computers](log-analytics-quick-collect-linux-computer.md) of [Quick Start voor Windows-computers](log-analytics-quick-collect-windows-computer.md)


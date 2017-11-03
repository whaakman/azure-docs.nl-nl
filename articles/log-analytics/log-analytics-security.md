---
title: Meld u Analytics gegevensbeveiliging | Microsoft Docs
description: Meer informatie over hoe logboekanalyse beschermt u uw privacy en uw gegevens beveiligt.
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: a33bb05d-b310-4f2c-8f76-f627e600c8e7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: magoedte
ms.openlocfilehash: 91af774560860b35913e57b49fb7a1dd59f5640f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="log-analytics-data-security"></a>Meld u Analytics-gegevensbeveiliging
Microsoft hecht groot belang om uw privacy te beschermen en beveiliging van uw gegevens tijdens de levering van software en services die u helpen de IT-infrastructuur van uw organisatie beheren. We erkennen dat wanneer u uw gegevens naar anderen belasten, vertrouwensrelatie strengere beveiliging vereist. Microsoft voldoet aan strikte nalevings- en beveiligingsrichtlijnen - van het schrijven van code tot de uitvoering van een service.

Beveiliging en bescherming van gegevens is een topprioriteit bij Microsoft. Neem contact met ons met eventuele vragen, suggesties of problemen met betrekking tot een van de volgende informatie, met inbegrip van ons beveiligingsbeleid op [Azure ondersteuningsopties](http://azure.microsoft.com/support/options/).

Dit artikel wordt uitgelegd hoe gegevens die worden verzameld, verwerkt en beveiligd door logboekanalyse Operations Management Suite (OMS). Agents kunt u verbinding maken met de webservice, System Center Operations Manager gebruiken voor het verzamelen van operationele gegevens of gegevens ophalen uit Azure diagnostics voor gebruik door logboekanalyse. De verzamelde gegevens wordt verzonden via Internet met behulp van certificaten gebaseerde verificatie en SSL-3 naar het Log Analytics-service, die wordt gehost in Microsoft Azure. Gegevens worden gecomprimeerd door de agent voordat deze wordt verzonden.

De Log Analytics-service beheert uw cloud-gebaseerde gegevens veilig met behulp van de volgende methoden:

* gegevensscheiding
* bewaren van gegevens
* Fysieke beveiliging
* Incidentbeheer
* Naleving
* standaarden veiligheidscertificaten

## <a name="data-segregation"></a>gegevensscheiding
Gegevens van de klant is geïsoleerd logisch op elk onderdeel in de OMS-service. Alle gegevens worden gemarkeerd per organisatie. Deze markering blijft aanwezig gedurende de levenscyclus van de gegevens en deze wordt afgedwongen op elke laag van de service. Elke klant heeft een specifieke Azure blob met de gegevens op lange termijn

## <a name="data-retention"></a>Bewaartijd van gegevens
Geïndexeerde logboekgegevens zoeken is opgeslagen en behouden volgens uw plan prijscategorie. Zie voor meer informatie [Log Analytics prijzen](https://azure.microsoft.com/pricing/details/log-analytics/).

Microsoft verwijdert klantgegevens 30 dagen na de OMS-werkruimte is gesloten. Microsoft worden ook verwijderd van de Azure Storage-Account waarin de gegevens zich bevindt. Wanneer gegevens van de klant wordt verwijderd, worden er geen fysieke schijven vernietigd.

De volgende tabel vindt u enkele van de beschikbare oplossingen in OMS en voorbeelden de soorten gegevens ze verzamelen.

| **Oplossing** | **Gegevenstypen** |
| --- | --- |
| Configuratie-evaluatie |Configuratiegegevens, metagegevens en gegevens van de gebruikersstatus |
| Capaciteitsplanning |Prestatiegegevens en metagegevens |
| Antimalware |Configuratiegegevens en metagegevens |
| Evaluatie voor systeemupdate |Metagegevens en de statusgegevens |
| Logboekbeheer |Gebruiker gedefinieerde gebeurtenislogboeken, Windows-gebeurtenislogboeken en/of de IIS-logboeken |
| Tracering wijzigen |Software-inventaris en metagegevens van de Windows-Service |
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
De logboekanalyse in OMS-service is bemand door medewerkers van Microsoft en alle activiteiten die worden geregistreerd en kunnen worden gecontroleerd. De service volledig in Azure wordt uitgevoerd en voldoet aan de Azure algemene engineering criteria. U kunt meer informatie over de fysieke beveiliging van Azure activa weergeven op pagina 18 van de [beveiligingsoverzicht van Microsoft Azure](http://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf). Fysieke toegangsrechten voor het beveiligen van gebieden worden gewijzigd binnen één werkdag voor iedereen die verantwoordelijk is voor de OMS-service, inclusief overdracht en beëindiging beschikt niet langer over. U kunt meer informatie over globale fysieke infrastructuur gebruiken we op [Microsoft-Datacenters](https://www.microsoft.com/en-us/server-cloud/cloud-os/global-datacenters.aspx).

## <a name="incident-management"></a>Incidentbeheer
OMS heeft een incidentbeheerproces die voor alle Microsoft-services worden aangehouden. Samengevat: we:

* Een gedeelde verantwoordelijkheid model waarin een deel van de verantwoordelijkheid van de beveiliging van Microsoft behoort tot en een deel aan de klant gebruiken
* Azure-beveiligingsincidenten beheren
  * Start een onderzoek na detectie van een incident
  * Evalueer de impact en urgentie van een incident door een teamlid voor respons op incidenten op oproep. Op basis van bewijs, de beoordeling kan of kan niet resulteren in meer escalatie naar het security response team.
  * Een incident door beveiliging antwoord deskundigen aan het technische of forensisch onderzoek verrichten, identificeren containment risicobeperking en tijdelijke oplossing strategieën onderzoeken. Als het beveiligingsteam gelooft dat gegevens van de klant kan hebben worden blootgesteld aan een persoon onrechtmatig of niet-geautoriseerde, begint de parallelle uitvoering van het proces van de klant Incident melding parallel.  
  * Regulering en het herstellen van het incident. Het team respons op incidenten wordt gemaakt van een herstelplan bij om het probleem te verhelpen. Crisis containment stappen zoals betrokken systemen in quarantaine plaatsen kunnen optreden onmiddellijk en parallel met diagnose. Langer term oplossingen kunnen worden gepland die zich voordoen nadat de onmiddellijke risico is verstreken.  
  * Sluit het incident en een post-keuring uitvoeren. Het team respons op incidenten maakt een post-keuring die geeft een overzicht van de details van het incident, met de bedoeling te herzien beleidsregels, procedures en processen om te voorkomen dat een herhaling van de gebeurtenis.
* Gebruikers informeren over beveiligingsincidenten
  * Bepalen van het bereik van de betrokken klanten en geef iedereen die als een aankondiging mogelijk gedetailleerde wordt beïnvloed
  * Maak een aankondiging voor klanten met gedetailleerde voldoende informatie zodat ze kunnen een onderzoek op hun einde uitvoeren en voldoen aan eventuele wanneer die ze in hun eindgebruikers hebt aangebracht, terwijl niet ten onrechte meldingen vertragen.
  * Bevestig en declareren van het incident, indien nodig.
  * Klanten met een incident melding zonder onredelijk vertraging en in overeenstemming met alle juridische of contractueel streven informeren. Meldingen van beveiligingsincidenten worden geleverd op een of meer van de klant beheerders door middel van die Microsoft selecteert, inclusief via e-mail.
* Gedragscode team gereedheid voor en training
  * Medewerkers van Microsoft zijn vereist voor het voltooien van de beveiligings- en awareness training, waardoor ze om te bepalen en mogelijke beveiligingsproblemen rapporteren.  
  * Operators werken op de Microsoft Azure-service hebben toevoeging training verplichtingen omringende toegang krijgen tot gevoelige systemen die als host fungeert voor gegevens van de klant.
  * Medewerkers van Microsoft security response ontvangen speciale training voor hun rollen

Als het verlies van enige klantgegevens optreedt, wordt elke klant melden binnen één dag. Klant-gegevensverlies opgetreden echter nooit met OMS. Bovendien we kopieën van gegevens die zijn gemaakt en geografisch wordt gedistribueerd.

Zie voor meer informatie over hoe Microsoft moet op beveiligingsincidenten reageren [reactie van Microsoft Azure-beveiliging in de Cloud](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/1/Microsoft Azure Security Response in the cloud.pdf).

## <a name="compliance"></a>Naleving
OMS software ontwikkelings- en service van het team informatie beveiliging en beheeracties programma ondersteunt de zakelijke vereisten en voldoet aan regelgeving zoals beschreven op [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) en [Microsoft Trust Center naleving](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx). Hoe OMS beveiligingsvereisten vaststelt, identificeert beveiligingsmechanismen beheert en bewaakt de risico's worden er ook beschreven. Jaarlijks, wij controleren beleidsregels, standaarden, procedures en richtlijnen.

Elk teamlid OMS-ontwikkeling ontvangt beveiligingstraining formele toepassing. We gebruiken een versiebeheersysteem intern voor softwareontwikkeling. Elk software-project wordt beveiligd door het versiebeheersysteem.

Microsoft heeft een beveiliging en naleving team dat verantwoordelijk en alle services in Microsoft evalueert. De vaststelling van informatie van het team maken en ze niet zijn gekoppeld aan de technische afdelingen die OMS ontwikkelen. De beveiliging verantwoordelijken hebben hun eigen Managementketen en uitvoering van een onafhankelijke beoordelingen van producten en services om te controleren of de beveiliging en naleving.

Raad van bestuur van Microsoft is een melding via een jaarlijkse rapport over alle informatie beveiligingsprogramma's bij Microsoft.

Het team OMS software development en -service werkt actief samen met de teams Microsoft Legal en naleving en andere zakenpartners verschillende certificaten verkrijgen.

## <a name="certifications-and-attestations"></a>Certificaten en verklaringen
OMS Log Analytics voldoet aan de volgende vereisten:

* [ISO/IEC 27001](http://www.iso.org/iso/home/standards/management-standards/iso27001.htm)
* [ISO/IEC 27018:2014](http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498)
* [ISO 22301](https://azure.microsoft.com/en-us/blog/iso22301/)
* [Payment Card Industry (PCI compatibele) gegevens beveiligingsstandaard (PCI DSS)](https://www.microsoft.com/en-us/TrustCenter/Compliance/PCI) door de PCI Security Standards Raad.
* [Type service organisatie besturingselementen (SOC) 1 1 en SOC 2 Type 1](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2) compatibel
* [HIPAA en HITECH](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA) voor bedrijven die een HIPAA-overeenkomst voor het koppelen van bedrijven hebben
* Windows gemeenschappelijke Engineering Criteria
* Microsoft Trustworthy Computing
* Als een Azure-service voldoen de onderdelen die gebruikmaakt van OMS aan nalevingsvereisten van Azure. U vindt meer op [Microsoft Trust Center naleving](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx).

> [!NOTE]
> In sommige certificeringen/verklaringen Log Analytics wordt vermeld onder de oude naam van *Operational Insights*.
>
>


## <a name="cloud-computing-security-data-flow"></a>Cloudcomputing beveiliging gegevensstroom
Het volgende diagram toont een cloud-beveiligingsarchitectuur als de stroom van gegevens van uw bedrijf en hoe deze wordt beveiligd, zoals is verplaatst naar de Log Analytics-service, uiteindelijk gezien door u in de OMS-portal. Meer informatie over elke stap volgt in het diagram.

![Afbeelding van OMS-gegevensverzameling en -beveiliging](./media/log-analytics-security/log-analytics-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. Aanmelden voor logboekanalyse en gegevens verzamelen
Voor uw organisatie gegevens verzenden naar Log Analytics, moet u Windows-agents, agents die worden uitgevoerd op Azure virtual machines of OMS-Agents voor Linux configureren. Als u Operations Manager-agents gebruikt, vervolgens gebruikt u wizard een configuratie in de Operations-console configureren ervan. Gebruikers (dit is mogelijk u afzonderlijke gebruikers of een groep personen) een of meer OMS-accounts (OMS-werkruimten) maken en registreren van agents met een van de volgende accounts:

* [Organisatie-ID](../active-directory/sign-up-organization.md)
* [Microsoft-Account - Outlook, Office Live, MSN](http://www.microsoft.com/account/default.aspx)

Een OMS-werkruimte is de gegevens wordt verzameld, samengevoegd, geanalyseerd en gepresenteerd. Een werkruimte wordt voornamelijk gebruikt om partitiegegevens te en elke werkruimte uniek is. U wilt bijvoorbeeld uw productiegegevens die worden beheerd door een OMS-werkruimte en uw testgegevens die worden beheerd door een andere werkruimte. Een beheerder gebruikerstoegang helpen werkruimten ook bij de gegevens. Elke werkruimte kan meerdere gebruikersaccounts die zijn gekoppeld, en elke gebruikersaccount hebben toegang tot meerdere OMS werkruimten. U maakt op basis van regio datacenter werkruimten. Elke werkruimte wordt gerepliceerd naar andere datacentra in de regio, hoofdzakelijk voor de beschikbaarheid van de OMS-service.

Wanneer de configuratiewizard is voltooid, maakt elke Operations Manager-beheergroep voor Operations Manager verbinding met de Log Analytics-service. U kunt vervolgens de Wizard Computers toevoegen gebruiken om te kiezen welke computers in de beheergroep mogen gegevens verzenden naar de service. Voor andere typen agent verbindt elk veilig met de OMS-service.

Alle communicatie tussen verbonden systemen en de Log Analytics-service is versleuteld.  Het TLS (HTTPS)-protocol wordt gebruikt voor versleuteling.  Het Microsoft SDL-proces wordt om ervoor te zorgen dat logboekanalyse is bijgewerkt door de meest recente ontwikkelingen in cryptografieprotocollen gevolgd.

Elk type agent verzamelt gegevens voor logboekanalyse. Het type van de gegevens die worden verzameld is afhankelijk van de typen van oplossingen die worden gebruikt. U ziet een overzicht van het verzamelen van gegevens op [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md). Meer gedetailleerde informatie van de verzameling is daarnaast beschikbaar voor de meeste oplossingen. Een oplossing is een bundel van vooraf gedefinieerde weergaven, logboek zoekquery's, regels voor het verzamelen van gegevens en logica voor verwerking. Alleen beheerders kunnen Log Analytics gebruiken voor het importeren van een oplossing. Nadat de oplossing is geïmporteerd, wordt deze verplaatst naar de Operations Manager-beheerservers (indien gebruikt), en vervolgens naar alle agents die u hebt gekozen. Daarna wordt verzamelen de agents de gegevens.

## <a name="2-send-data-from-agents"></a>2. Verzenden van gegevens van agents
U alle typen van de agent met een sleutel inschrijving registreren en een beveiligde verbinding tot stand is gebracht tussen de agent en de Log Analytics-service met behulp van certificaten gebaseerde verificatie en SSL met poort 443. Een geheim archief OMS gebruikt voor het genereren en onderhouden van sleutels. Persoonlijke sleutels om de 90 dagen worden gedraaid en worden opgeslagen in Azure en worden beheerd door de Azure-bewerkingen die strikt regelgeving en naleving procedures te volgen.

Met Operations Manager, registreert u een werkruimte met de Log Analytics-service en een beveiligde HTTPS-verbinding tot stand is gebracht tussen de Operations Manager-beheerserver.

Voor Windows-agents uitgevoerd op virtuele machines in Azure, wordt een alleen-lezen opslagsleutel gebruikt om te lezen van diagnostische gebeurtenissen in de Azure-tabellen.

Als een agent kan niet communiceren met de service voor een of andere reden is, wordt de verzamelde gegevens lokaal wordt opgeslagen in een tijdelijke cache en de beheerserver probeert te verzenden van de gegevens elke acht minuten gedurende 2 uur. Gegevens in de cache van de agent wordt beveiligd door de referentie-archief van het besturingssysteem. Als de service de gegevens niet na twee uur verwerken kan, wordt de gegevens van de agents de verzendwachtrij. Als de wachtrij vol is, start OMS gegevenstypen, beginnend met prestatiegegevens neer te zetten. De wachtrijlimiet agent is een registersleutel zodat u wijzigen kunt, indien nodig. Verzamelde gegevens wordt gecomprimeerd en verzonden naar de service, het omzeilen van lokale databases, dus voegt geen belasting toe. Nadat de verzamelde gegevens worden verzonden, wordt deze verwijderd uit de cache.

Zoals hierboven wordt beschreven, worden de gegevens van uw agents verzonden via SSL aan Microsoft Azure-datacenters. Desgewenst kunt u ExpressRoute voor aanvullende beveiliging voor de gegevens. ExpressRoute is een manier om rechtstreeks verbinding maken met Azure vanaf uw eigen WAN-netwerk, zoals een multiprotocol label switching (MPLS) VPN, geleverd door een netwerkserviceprovider. Zie voor meer informatie [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

## <a name="3-the-log-analytics-service-receives-and-processes-data"></a>3. De Log Analytics-service ontvangt en verwerkt gegevens
De service logboekanalyse zorgt ervoor dat binnenkomende gegevens van een vertrouwde bron door het valideren van certificaten en de integriteit van gegevens met Azure-verificatie. De niet-verwerkte onbewerkte gegevens vervolgens wordt opgeslagen als een blob in [Microsoft Azure Storage](../storage/common/storage-introduction.md) en is niet versleuteld. Elke Azure-opslag-blob heeft echter een set unieke set van sleutels, die alleen toegankelijk is voor die gebruiker. Het type gegevens dat is opgeslagen, is afhankelijk van de typen van oplossingen die zijn geïmporteerd en gebruikt om gegevens te verzamelen. De Log Analytics-service verwerkt vervolgens de onbewerkte gegevens voor de Azure-opslag-blob.

## <a name="4-use-log-analytics-to-access-the-data"></a>4. Log Analytics gebruiken voor toegang tot de gegevens
U kunt aanmelden met logboekanalyse in de OMS-portal met behulp van de organisatie-account of de Microsoft-account dat u eerder hebt ingesteld. Al het verkeer tussen de OMS-portal en Log Analytics in OMS wordt verzonden via een beveiligd kanaal voor HTTPS. Wanneer u de OMS-portal, een sessie-ID is gegenereerd op de client gebruiker (webbrowser) en gegevens worden opgeslagen in een lokale cache totdat de sessie wordt beëindigd. Wanneer is beëindigd, wordt de cache verwijderd. Client-side cookies, die geen persoonsgegevens bevatten, worden niet automatisch verwijderd. Sessiecookies HTTPOnly zijn gemarkeerd en zijn beveiligd. Na een vooraf bepaalde niet-actieve periode wordt de OMS-portal sessie beëindigd.

Met de OMS-portal kunt u gegevens exporteren naar een CSV-bestand en u toegang hebt tot gegevens via zoeken-API's. CSV-export is beperkt tot 50.000 rijen per exporteren en API-gegevens wordt beperkt tot 5000 rijen per zoeken.

## <a name="next-steps"></a>Volgende stappen
* [Aan de slag met logboekanalyse](log-analytics-get-started.md) voor meer informatie over Log Analytics en ophalen van actief in minuten.

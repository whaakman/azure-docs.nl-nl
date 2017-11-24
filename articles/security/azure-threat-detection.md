---
title: Azure geavanceerde detectie van dreigingen | Microsoft Docs
description: Meer informatie over Identity Protection en de mogelijkheden ervan.
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: d2fab26d8ff9f006cfed82685a738b791d0b0624
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="azure-advanced-threat-detection"></a>Azure Advanced Threat detectie
## <a name="introduction"></a>Inleiding

### <a name="overview"></a>Overzicht

Microsoft heeft ontwikkeld een reeks whitepapers, overzichten van de beveiliging, aanbevolen procedures en controlelijsten om te helpen over de verschillende betrekking hebben op beveiliging mogelijkheden beschikbaar in Azure-klanten en rond de Azure-Platform. De onderwerpen in termen van breedte en diepte liggen en regelmatig worden bijgewerkt. Dit document is onderdeel van de reeks zoals samengevat in de volgende sectie abstract.

### <a name="azure-platform"></a>Azure-Platform

Azure is een openbare cloud service-platform die ondersteuning biedt voor de breedste selectie van besturingssystemen, programmeertalen, frameworks, hulpprogramma's, databases, en apparaten.
Ondersteunt de volgende programmeertalen:
-   Linux-containers worden uitgevoerd met Docker-integratie.
-   Met JavaScript, Python, .NET, PHP, Java en Node.js-apps bouwen
-   Build back-ends voor iOS, Android en Windows apparaten.

Openbare Azure-cloud-services ondersteunen dezelfde technologieën miljoenen ontwikkelaars en IT-professionals die al zijn afhankelijk van en vertrouwt.

Wanneer u migreert naar een openbare cloud met een organisatie of organisatie die verantwoordelijk zijn voor uw gegevens beschermen en bieden beveiliging en beheeracties rond het systeem is.

De infrastructuur van Azure is zo ontworpen dat toepassingen miljoenen klanten tegelijkertijd kunnen hosten en er daarnaast een betrouwbare basis wordt geboden waarop bedrijven kunnen voldoen aan hun beveiligingsbehoeften. Azure biedt een breed scala aan opties voor het configureren en aanpassen van beveiliging om te voldoen aan de vereisten van uw app-implementaties. Dit document helpt u aan deze vereisten voldoet.

### <a name="abstract"></a>Abstracte

Microsoft Azure-aanbiedingen die zijn ingebouwd in de functie voor de detectie geavanceerde threat via services, zoals Azure Active Directory, Azure Operations Management Suite (OMS) en Azure Security Center. Deze verzameling beveiligingsservices en -mogelijkheden biedt een eenvoudige en snelle manier om te begrijpen wat gebeurt er in uw Azure-implementaties.

In dit artikel begeleidt u de "Microsoft Azure nadert' naar threat beveiligingslek diagnostische en analyse van de risico's die zijn gekoppeld aan de schadelijke activiteiten die zijn gericht op basis van servers en andere Azure-resources. Dit helpt u bij het identificeren van de methoden voor het beheer van identificatie- en beveiligingsproblemen met geoptimaliseerde oplossingen door de Azure-platform en klantgerichte beveiligingsservices en -technologieën.

Dit document richt zich op de technologie van Azure-platform en klantgerichte besturingselementen en probeert niet te adres Sla's, prijzen modellen en DevOps practice overwegingen.

## <a name="azure-active-directory-identity-protection"></a>Azure Active Directory Identity Protection

![Azure Active Directory Identity Protection](./media/azure-threat-detection/azure-threat-detection-fig1.png)


[Azure Active Directory: Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) is een functie van de [Azure AD Premium-P2](https://docs.microsoft.com/azure/active-directory/active-directory-editions) edition waarmee u een overzicht van de risicogebeurtenissen en mogelijke beveiligingsproblemen die invloed hebben op de identiteiten van uw organisatie. Microsoft heeft identiteiten voor een cloud-gebaseerde is beveiligen via een tien jaar en met Azure AD Identity Protection Microsoft is deze dezelfde beveiliging systemen beschikbaar maken voor enterprise-klanten. Identity Protection gebruikmaakt van bestaande Azure AD afwijkingsdetectie detectiemogelijkheden beschikbaar via [Azure AD-rapporten voor afwijkende activiteiten](https://docs.microsoft.com/azure/active-directory/active-directory-view-access-usage-reports#anomalous-activity-reports), en introduceert nieuwe risico gebeurtenistypen dat realtime afwijkingen kunnen detecteren.

Identity Protection maakt gebruik van geavanceerde machine learning-algoritmen en methodiek voor het opsporen van afwijkingen en gebeurtenissen die aangeven mogelijk dat een identiteit is ingebroken risico. Met deze gegevens Identity Protection genereert rapporten en waarschuwingen die zodat u kunt deze risicogebeurtenissen onderzoeken en de juiste doorvoeren of risicobeperking actie ondernemen.

Maar Azure Active Directory: Identity Protection is meer dan een hulpprogramma voor bewaking en rapportage. Op basis van de risico's, berekent Identity Protection het risiconiveau van een gebruiker voor elke gebruiker, zodat u kunt beleid op basis van een risico voor het automatisch beveiligen van de identiteit van uw organisatie configureren.

Deze beleidsregels risico gebaseerde naast andere [voorwaardelijk toegangsbeheer](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) verstrekt door Azure Active Directory en [EMS](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access), kunnen automatisch blokkeren of bieden adaptieve herstelacties met wachtwoord opnieuw instellen en afdwingen van multi-factor authentication-server.

### <a name="identity-protections-capabilities"></a>De mogelijkheden van Identity Protection

Azure Active Directory: Identity Protection is meer dan een controle en rapportage. Ter bescherming van uw organisatie identiteiten, kunt u risico-beleidsregels die automatisch op gedetecteerde problemen reageren als een opgegeven risiconiveau is bereikt. Deze beleidsregels, naast de andere besturingselementen voorwaardelijke toegang is verstrekt door Azure Active Directory en EMS, kunnen automatisch blokkeren of initiëren adaptieve herstelacties met inbegrip van wachtwoorden opnieuw instellen en afdwingen van multi-factor authentication-server.

Enkele voorbeelden van een van de manieren waarop Azure Identity Protection kunt beveiligen van uw accounts en identiteiten omvatten:

[Risico's en riskant accounts te detecteren:](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection#detection)
-   Zes risico gebeurtenistypen met machine learning en heuristische regels detecteren
-   Gebruiker risiconiveaus berekenen
-   Aangepaste aanbevelingen voor de algehele beveiligingsstatus markeert beveiligingslekken bieden

[Bezig met het onderzoeken van risico's:](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection#investigation)
-   Verzenden van meldingen voor risico 's
-   Het onderzoeken van risico's met behulp van de relevante en contextuele informatie
-   Biedt eenvoudige werkstromen voor het bijhouden van onderzoeken
-   Eenvoudige toegang tot herstelacties zoals het wachtwoord opnieuw instellen

[Beleid voor voorwaardelijke toegang op basis van risico's:](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection#risky-sign-ins)
-   Beleid voor riskante aanmeldingen beperken door aanmeldingen blokkeren of uitdagingen voor meervoudige verificatie vereisen.
-   Beleid voor het blokkeren of beveiligde riskant gebruikersaccounts
-   Beleid voor gebruikers moeten zich registreren voor meervoudige verificatie

### <a name="azure-ad-privileged-identity-management-pim"></a>Azure AD Privileged Identity Management (PIM)

Met [Azure Active Directory (AD) Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure),

![Azure AD Privileged Identity Management](./media/azure-threat-detection/azure-threat-detection-fig2.png)

u kunt beheren, bepalen en controleren van toegang binnen uw organisatie. Dit is inclusief toegang tot resources in Azure AD en andere Microsoft-onlineservices zoals Office 365 en Microsoft Intune.

Azure AD Privileged Identity Management kunt u:

-   Een waarschuwing en rapporteren over Azure AD-beheerders en 'just in time' beheerderstoegang toewijzen voor Microsoft Online Services zoals Office 365 en Intune

-   Rapporten over beheerder toegang tot geschiedenis en wijzigingen in beheerderstoewijzingen ophalen

-   Ontvang waarschuwingen over de toegang tot een bevoorrechte rol

## <a name="microsoft-operations-management-suite-oms"></a>Microsoft Operations Management Suite (OMS)

[Microsoft Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) van Microsoft cloud-gebaseerde IT beheeroplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloud-infrastructuur. Aangezien OMS wordt geïmplementeerd als een cloudservice, kunt u er al snel mee aan de slag, en dat met minimale investeringen in infrastructuurservices. Nieuwe beveiligingsfuncties automatisch worden geleverd voor het opslaan van uw voortdurend onderhoud en kosten te upgraden.

Naast het bieden van waardevolle services op een eigen, OMS kan worden geïntegreerd met System Center-onderdelen, zoals [System Center Operations Manager](https://blogs.technet.microsoft.com/cbernier/2013/10/23/monitoring-windows-azure-with-system-center-operations-manager-2012-get-me-started/) uit te breiden de beveiliging van uw bestaande investeringen management in de cloud. Door de samenwerking van System Center en OMS krijgt u de beschikking over een volledige hybride beheeroplossing.

### <a name="holistic-security-and-compliance-posture"></a>Holistische beveiliging en naleving houding

De [OMS beveiligings- en Audit dashboard](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) biedt een uitgebreid overzicht van uw organisatie IT beveiligingspostuur met ingebouwde zoekquery's voor problemen die aandacht vereisen die uw aandacht vereisen. Het dashboard beveiligings- en Audit is het startscherm voor alles met betrekking tot beveiliging in OMS. Het zorgt voor hoogwaardig inzicht in de beveiligingsstatus van uw computers. Ook kunnen op het startscherm alle gebeurtenissen van de afgelopen 24 uur, 7 dagen of een ander tijdsbestek worden weergegeven.

OMS dashboards waarmee u snel en eenvoudig inzicht in de algehele beveiligingsstatus van de omgeving, allemaal binnen de context van de IT-bewerkingen, inclusief: software-update-evaluatie, antimalware beoordeling en configuratiebasislijnen. Bovendien is de beveiliging logboekgegevens gemakkelijk toegankelijk zijn voor het stroomlijnen van de beveiliging en naleving audit-processen.

Het OMS-dashboard Beveiliging en controle is ingedeeld in vier hoofdcategorieën:

![OMS-dashboard Beveiliging en controle](./media/azure-threat-detection/azure-threat-detection-fig3.jpg)

-   **Beveiligingsdomeinen:** op dit gebied kunt u zich verder verkennen beveiligingsrecord gedurende een bepaalde periode, toegang tot evaluatie van schadelijke software, bijwerken assessment, netwerkbeveiliging, informatie over identiteit en toegang, computers met beveiligingsgebeurtenissen en snel toegang hebben tot Azure Security Center-dashboard.

-   **Problemen die aandacht vereisen:** deze optie kunt u het aantal actieve problemen en de ernst van deze problemen snel kan identificeren.

-   **Detecties (Preview):** kunt u aanvalspatronen identificeren door te visualiseren beveiligingswaarschuwingen als zij op basis van uw resources plaatsvinden.

-   **Dreigingen:** aanvalspatronen identificeren door het totale aantal servers met uitgaand schadelijk IP-verkeer, het type schadelijke threat en die laat waar deze IP-adressen afkomstig zijn zien van een-toewijzing te visualiseren.

-   **Algemene beveiliging query's:** deze optie geeft u een lijst van de meest voorkomende beveiliging query's die u gebruiken kunt voor het bewaken van uw omgeving. Wanneer u op een van de query's klikt, wordt de Search-blade geopend met de resultaten voor deze query.

### <a name="insight-and-analytics"></a>Inzicht en analyses
In het midden van [logboekanalyse](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) is de OMS-opslagplaats die wordt gehost in de Azure-cloud.

![Inzicht en analyses](./media/azure-threat-detection/azure-threat-detection-fig4.png)

Gegevens worden vanuit verbonden bronnen verzameld in de opslagplaats door gegevensbronnen te configureren en oplossingen toe te voegen aan uw abonnement.

![abonnement](./media/azure-threat-detection/azure-threat-detection-fig5.png)

Gegevensbronnen en oplossingen creëren elk verschillende recordtypen die hun eigen set eigenschappen hebben, maar nog steeds samen kunnen worden geanalyseerd in query's in de opslagplaats. Hiermee kunt u dezelfde hulpprogramma's en methoden gebruiken om te werken met verschillende soorten gegevens die door verschillende bronnen zijn verzameld.


De meeste van uw interactie met logboekanalyse is via de OMS-portal, dat wordt uitgevoerd in een browser en toegang tot configuratie-instellingen en meerdere hulpprogramma's te analyseren en fungeren van verzamelde gegevens. Vanuit de portal kunt u [Meld zoekopdrachten](https://docs.microsoft.com/azure/log-analytics/log-analytics-log-searches) wanneer u query's om verzamelde gegevens te analyseren samenstellen [dashboards](https://docs.microsoft.com/azure/log-analytics/log-analytics-dashboards), die u kunt aanpassen met grafische weergave van uw waardevolste zoekopdrachten en [oplossingen](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions), die aanvullende functionaliteit en analyse hulpprogramma's bieden.

![analyseprogramma 's](./media/azure-threat-detection/azure-threat-detection-fig6.png)

Oplossingen voegen functionaliteit toe aan Log Analytics. Deze worden voornamelijk uitgevoerd in de cloud en analyseren gegevens die in de OMS-opslagplaats zijn verzameld. Ze kunnen ook nieuwe recordtypen definiëren om te verzamelen die kunnen worden geanalyseerd met zoekopdrachten in logboeken of door een aanvullende gebruikersinterface geleverd door de oplossing in het OMS-dashboard.
De beveiligings- en Audit is een voorbeeld van deze typen oplossingen.



### <a name="automation--control-alert-on-security-configuration-drifts"></a>Automation en Control: waarschuwing voor beveiligingsconfiguratie drifts

Azure Automation automatiseert administratieve processen met runbooks die zijn gebaseerd op PowerShell en worden uitgevoerd in de Azure-cloud. Runbooks kunnen ook worden uitgevoerd op een server in uw lokale datacentrum om lokale resources te beheren. Azure Automation biedt Configuratiebeheer met PowerShell DSC (Desired State Configuration).

![Azure Automation](./media/azure-threat-detection/azure-threat-detection-fig7.png)

U kunt maken en beheren van DSC-resources gehost in Azure en ze naar de cloud en on-premises systemen voor het definiëren en automatisch afdwingen van de configuratie of haal rapporten op afwijking om te verzekeren dat beveiligingsconfiguraties binnen beleid blijven toepassen.

## <a name="azure-security-center"></a>Azure Security Center

Azure Security Center beschermt u uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen. In de service u zich kunt definiëren niet alleen op basis van uw Azure-abonnementen, maar ook het beleid op basis van [resourcegroepen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal), zodat u gedetailleerder worden uitgevoerd kunt.

![Azure Security Center](./media/azure-threat-detection/azure-threat-detection-fig8.png)

Beveiligingsonderzoekers van Microsoft zijn voortdurend op zoek naar bedreigingen. Ze hebben toegang tot een uitgebreide set telemetrie, afkomstig van de wereldwijde aanwezigheid van Microsoft in de cloud en on-premises. Deze verrijkende en uitgebreide verzameling gegevenssets stelt Microsoft in staat om nieuwe aanvalspatronen en -trends te ontdekken in on-premises producten voor consumenten en ondernemingen, evenals in haar online services.

Security Center kunt dus snel de detectiealgoritmen bijwerken als aanvallers release nieuwe en steeds meer geavanceerde aanvallen. Deze aanpak kunt u blijven met een bedreiging snel bewegende omgeving.

![Security Center](./media/azure-threat-detection/azure-threat-detection-fig9.jpg)

Het detecteren van bedreigingen van Security Center werkt volgens het automatisch verzamelen van beveiligingsgegevens van uw Azure-resources, het netwerk en verbonden partneroplossingen.  Deze informatie kunt correleren van gegevens uit meerdere bronnen, bedreigingen identificeren wordt geanalyseerd.
Beveiligingswaarschuwingen krijgen in Security Center een prioriteit, evenals aanbevelingen voor het oplossen van de bedreiging.

Security Center maakt gebruik van geavanceerde beveiligingsanalyses die veel verder gaan dan op handtekeningen gebaseerde benaderingen. Doorbraken in big data en [machine learning](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) technologieën worden gebruikt voor het evalueren van gebeurtenissen in de volledige in de cloud-infrastructuurresources – bedreigingen die niet mogelijk om u te identificeren met behulp van handmatige benaderingen en het voorspellen van de evolutie van aanvallen te detecteren. Deze beveiligingsanalyse omvat het volgende.

### <a name="threat-intelligence"></a>Bedreigingsinformatie

Microsoft heeft een gigantische hoeveelheid informatie over wereldwijde bedreigingen.
Telemetrie stroomt in uit meerdere bronnen, zoals Azure, Office 365, Microsoft CRM online, Microsoft Dynamics AX, outlook.com, MSN.com, Microsoft Digital Crimes Unit (DCU) en Microsoft Security Response Center (MSRC).

![Bedreigingsinformatie](./media/azure-threat-detection/azure-threat-detection-fig10.jpg)

Onderzoekers ontvangen ook informatie over bedreigingen die wordt gedeeld tussen de primaire cloudserviceproviders en abonnementen op feeds met informatie over bedreigingen van derden. Azure Security Center kan deze informatie gebruiken om u te waarschuwen over bedreigingen van bekende beveiligingsrisico’s. Voorbeelden zijn:

-   **De stroom van Machine Learning - harnessing** Azure Security Center toegang heeft tot een enorme hoeveelheid gegevens over de netwerkactiviteit cloud, die kan worden gebruikt voor het detecteren van bedreigingen die gericht is op uw Azure-implementaties. Bijvoorbeeld:

-   **Brute Force detecties -** Machine learning wordt gebruikt om een historische patroon pogingen voor externe toegang, zodat ze kunnen detecteren beveiligingsaanvallen op SSH, RDP en SQL-poorten te maken.

-   **Uitgaande DDoS Botnet en de detectie en** -een algemene doelstelling van aanvallen die gericht is op cloud-bronnen is met de rekencapaciteit van deze bronnen uit te voeren van andere aanvallen.

-   **Nieuwe gebruikersgedrag Analytics-Servers en virtuele machines -** wanneer een server of de virtuele machine is geknoeid, aanvallers gebruikmaken van een groot aantal technieken schadelijke code uitvoeren op dat systeem tijdens detectie vermijden, gezorgd persistentie en beveiligingsmechanismen en vermindert.

-   **Azure SQL Database-Bedreigingsdetectie -** detectie van dreigingen voor Azure SQL Database, waarmee wordt aangegeven afwijkende databaseactiviteiten die aangeeft ongebruikelijke en potentieel schadelijke probeert te krijgen of misbruik van databases.

### <a name="behavioral-analytics"></a>Gedragsanalyse

Gedragsanalyse is een techniek waarbij gegevens worden geanalyseerd en vergeleken met een verzameling bekende patronen. Deze patronen zijn echter geen eenvoudige handtekeningen. Ze worden vastgesteld aan de hand van complexe machine learning-algoritmen die worden toegepast op grote gegevenssets.

![Gedragsanalyse](./media/azure-threat-detection/azure-threat-detection-fig11.jpg)


Ze worden ook vastgesteld via de zorgvuldige analyse van schadelijk gedrag door deskundige analisten. Azure Security Center kunt gedragsanalyse gebruiken om te identificeren waarmee is geknoeid bronnen op basis van de analyse van de logboeken van de virtuele machine, apparaatlogboeken virtueel netwerk, infrastructuur Logboeken, crashdumps en andere bronnen.

Er wordt ook gekeken naar andere signalen om te controleren op ondersteunend bewijs van een wijdverbreide aanval. Aan de hand van dit verband kan beter worden vastgesteld welke gebeurtenissen samenhangen met de opgestelde indicatoren van inbreuk.

Voorbeelden zijn:
-   **Uitvoering van verdachte:** aanvallers gebruiken verschillende technieken schadelijke software zonder detectie uitvoeren. Bijvoorbeeld een aanvaller mogelijk geven malware dezelfde naam als legitieme systeembestanden, maar deze bestanden in een andere locatie te plaatsen, gebruik een naam die lijkt erg een onschadelijk bestand of maskeren true bestandsextensie. Security Center maakt gedragsmodellen en controleert de uitvoering van processen om uitschieters als deze te detecteren.

-   **Verborgen kwaadaardige software en misbruik pogingen:** geavanceerde malware traditionele antimalwareproducten door nooit schrijven naar schijf of software-onderdelen die zijn opgeslagen op schijf versleutelen kunt omzeilen. Echter kan dergelijke schadelijke software worden gedetecteerd met behulp van de geheugenanalyse, zoals de malware traceringen in het geheugen naar de functie laat moet. Wanneer software vastloopt, bevat een crashdump een gedeelte van het geheugen ten tijde van de crash. Door het geheugen in de crashdump wordt geanalyseerd, kan Azure Security Center technieken die worden gebruikt via beveiligingslekken, toegang krijgen tot vertrouwelijke gegevens en ongemerkt binnen een verdachte computer blijven behouden zonder enige impact op de prestaties van uw computer detecteren.

-   **Lateral movement en interne reconnaissance:** om te blijven behouden in een waarmee is geknoeid netwerk- en zoeken/oogst waardevolle gegevens, aanvallers proberen vaak verplaatsen lateraal van de computer waarop inbreuk is gepleegd naar anderen binnen hetzelfde netwerk. Security Center bewaakt proces en meld u aan activiteiten voor het detecteren van aanvallen uit te breiden een aanvaller voet achter de deur binnen het netwerk, zoals externe opdrachten uit te voeren, het netwerk scannen en accountinventarisatie.

-   **Schadelijke PowerShell-Scripts:** PowerShell kan worden gebruikt door aanvallers schadelijke code uitvoeren op de doel-virtuele machines voor een verschillende doeleinden. Security Center inspecteert PowerShell-activiteit op tekenen van verdachte activiteiten.

-   **Uitgaande aanvallen:** aanvallers vaak cloudresources met het doel van het gebruik van deze bronnen voor aanvullende aanvallen koppelen als doel. Geïnfecteerde virtuele machines kan bijvoorbeeld worden gebruikt voor beveiligingsaanvallen op andere virtuele machines starten, verzenden van SPAM of scannen open poorten en andere apparaten op Internet. Door machine learning toe te passen op netwerkverkeer kan Security Center het detecteren wanneer uitgaande netwerkcommunicatie groter is dan de norm. Als SPAM, ongebruikelijke e verkeer Security Center ook correleert met intelligence vanuit Office 365 om te bepalen of het e-mailbericht waarschijnlijk slechte of het resultaat van een geldig e-campagne.

### <a name="anomaly-detection"></a>Afwijkingsdetectie

Azure Security Center maakt ook gebruik van afwijkingsdetectie om bedreigingen te identificeren. In tegenstelling tot gedragsanalyses (die afhankelijk zijn van bekende patronen die zijn afgeleid van grote gegevenssets) is afwijkingsdetectie meer "gepersonaliseerd" en richt het zich op basislijnen die specifiek voor uw implementaties zijn. Machine learning wordt toegepast om de normale activiteit voor uw implementaties te bepalen en vervolgens worden regels gegenereerd om afwijkende omstandigheden te definiëren die een veiligheidsrisico zouden kunnen vormen. Hier volgt een voorbeeld:

-   **Binnenkomend RDP/SSH beveiligingsaanvallen:** uw implementaties mogelijk bezet virtuele machines met veel aanmeldingen elke dag en andere virtuele machines hebt die weinig of aanmeldingen hebben. Azure Security Center kunt bepalen basislijn Aanmeldingsactiviteit voor deze virtuele machines en machine learning-definiëren om de normale aanmelding activiteiten gebruiken. Als er een discrepantie met de basislijn gedefinieerd voor aanmelding kenmerken verwante, en vervolgens een waarschuwing worden gegenereerd. Ook hier weer wordt door machine learning bepaald wat een aanzienlijk verschil is.

### <a name="continuous-threat-intelligence-monitoring"></a>Continue dreigingen bewaking

Azure Security Center werkt met beveiliging onderzoeks- en wetenschappelijke teams in de hele wereld die voortdurend op wijzigingen in de mate van gevaar controleren. Dit omvat de volgende initiatieven:

-   **Threat intelligence bewaking:** Threat intelligence omvat mechanismen, indicators gevolgen en bruikbare advies over bestaande of nieuwe bedreigingen. Deze informatie wordt gedeeld in de beveiligingscommunity en Microsoft volgt continu feeds met informatie over bedreigingen uit interne en externe bronnen.

-   **Delen van signaal:** inzichten voor teams van de beveiliging in brede portfolio van Microsoft cloud en on-premises services, servers en client eindpunt apparaten worden gedeeld en geanalyseerd.

-   **Microsoft security-specialisten:** actieve betrokkenheid bij via Microsoft-teams dat werkt in de velden van gespecialiseerde beveiliging, zoals forensische en web-aanvalsdetectie.

-   **Detectie afstemmen:** algoritmen voor bestaande klanten gegevenssets worden uitgevoerd en beveiligingsonderzoekers werken met klanten voor het valideren van de resultaten. Echte en fout-positieven worden gebruikt voor het verfijnen van machine learning-algoritmen.

Deze gecombineerde inspanningen moeten resulteren in nieuwe en verbeterde detecties waarvan u onmiddellijk kunt profiteren: u hoeft helemaal geen actie te ondernemen.

## <a name="advanced-threat-detection-features---other-azure-services"></a>Advanced Threat detectie onderdelen - andere Azure-Services

### <a name="virtual-machine-microsoft-antimalware"></a>Virtuele Machine: Microsoft Antimalware

[Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) voor Azure een oplossing voor één agent voor toepassingen en tenant-omgevingen is, die is ontworpen om uit te voeren op de achtergrond, zonder menselijke tussenkomst. U kunt beveiliging op basis van de behoeften van uw toepassing werkbelastingen, met een eenvoudige secure--standaard of aangepaste configuratie, met inbegrip van antimalware monitoring Geavanceerd implementeren. Azure antimalware is een beveiligingsoptie voor Azure Virtual Machines en wordt automatisch geïnstalleerd op alle Azure PaaS virtuele machines.

**Functies van Azure te implementeren en Microsoft Antimalware voor uw toepassingen inschakelen**

#### <a name="microsoft-antimalware-core-features"></a>Microsoft Antimalware-kernfuncties

-   **Real-timebeveiliging -** controleert de activiteit in de Cloud Services en op virtuele Machines om te detecteren en blokkeren van schadelijke software worden uitgevoerd.

-   **Geplande scan -** periodiek voert gerichte scannen om te detecteren van schadelijke software, inclusief actieve programma's.

-   **Oplossen van malware -** neemt automatisch actie op de gedetecteerde malware, zoals het verwijderen of schadelijke bestanden in quarantaine plaatsen en schadelijke registervermeldingen opruimen.

-   **Updates van de handtekening -** installeert automatisch de meest recente definities (virusdefinities) om ervoor te zorgen beveiliging op de hoogte van een vooraf bepaald frequentie is.

-   **Antimalware-Engine-updates -** automatisch bijgewerkt met de Microsoft Antimalware-engine.

-   **Updates voor anti-malware-Platform-** automatisch bijgewerkt met de Microsoft Antimalware-platform.

-   **Actieve beveiliging -** telemetrie metagegevens over gedetecteerde bedreigingen en verdachte bronnen rapporten naar Microsoft Azure om ervoor te zorgen snelle reactie op de zich ontwikkelende bedreigingen liggend en het inschakelen van realtime synchrone handtekening levering via de Microsoft Active Protection System (MAPS).

-   **Voorbeelden van de reporting -** biedt en voorbeelden rapporten naar Microsoft Antimalware-service voor het verfijnen van de service en het oplossen van problemen inschakelen.

-   **Uitsluitingen –** kunnen toepassing en servicebeheerders voor het configureren van bepaalde bestanden, processen, en tot ze uitsluiten van beveiliging en het scannen voor prestaties en/of andere redenen.

-   **Gebeurtenissen verzamelen van Antimalware -** registreert de status van antimalware-service, verdachte activiteiten en herstelacties genomen in het gebeurtenislogboek van het besturingssysteem en verzamelt u ze in Azure Storage-account van de klant.

### <a name="azure-sql-database-threat-detection"></a>Detectie van dreigingen voor Azure SQL Database

[Azure SQL Database met detectie van dreigingen](https://azure.microsoft.com/blog/azure-sql-database-threat-detection-your-built-in-security-expert/) is een nieuwe beveiligingsfunctie intelligence ingebouwd in de Azure SQL Database-service. Werkt het servicepakket voor meer informatie, profiel en afwijkende databaseactiviteiten gedetecteerd, identificeert Azure SQL Database met detectie van dreigingen potentiële bedreigingen voor de database.

-Afdelingen of andere aangewezen beheerders kunnen een onmiddellijke melding over verdachte databaseactiviteiten krijgen wanneer deze zich voordoen. Elke melding vindt u informatie over de verdachte activiteit en raadt aan om verder onderzoek en risico.

Op dit moment detecteert Azure SQL Database met detectie van dreigingen mogelijke beveiligingsproblemen en SQL-injectieaanvallen en database afwijkende toegangspatronen.

Bij ontvangst threat detectie e-mailmeldingen, kunnen gebruikers zich om te navigeren en de relevante controlerecords met behulp van de dieptekoppeling naar in het e-mailbericht dat wordt geopend een audit viewer en/of de vooraf geconfigureerde controle Excel sjabloon waarin de relevante controlerecords rond de tijd van de verdachte gebeurtenis volgens het volgende bekijken:
-   Opslag voor de databaseserver met de afwijkende databaseactiviteiten controleren

-   Relevante audit opslag tabel dat op het moment van de gebeurtenis is gebruikt voor het controlelogboek schrijven

-   Records van het volgende uur controleren omdat de gebeurtenis zich voordoet.

-   Controlerecords met vergelijkbare gebeurtenis-ID op het moment van de gebeurtenis (optioneel voor sommige detectoren)

SQL Database Threat detectoren gebruiken een van de volgende methoden voor detectie:

-   **Deterministische detectie –** detecteert verdachte patronen (regels gebaseerd) in de SQL client-query's die overeenkomen met bekende aanvallen. Deze methode heeft de detectie van hoge en lage fout-positief dekking echter beperkt, omdat deze binnen de categorie met '-atomic detecties valt'.

-   **Gedrag detectie –** gebreken van de afwijkende activiteit, die is abnormaal gedrag voor de database die niet tijdens de afgelopen 30 dagen gevonden is.  Een voorbeeld van de SQL client afwijkende activiteit mag een piek van mislukte aanmeldingen query's, hoog volume aan gegevens uitgepakt, ongebruikelijke canonieke query's en onbekende IP-adressen die worden gebruikt voor toegang tot de database

### <a name="application-gateway-web-application-firewall"></a>Application Gateway Web Application Firewall

[Web Application Firewall](../app-service/environment/app-service-app-service-environment-web-application-firewall.md) is een functie van [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-webapplicationfirewall-overview) die biedt beveiliging voor webtoepassingen die gebruikmaken van toepassingsgateway voor standaard [levering Toepassingsbeheer](https://kemptechnologies.com/in/application-delivery-controllers) functies. Web application firewall doet dit door deze te beschermen tegen de meeste van de [OWASP top 10 veelvoorkomende web beveiligingslekken](https://www.owasp.org/index.php/Top_10_2010-Main)

![Application Gateway-webtoepassing Firewally](./media/azure-threat-detection/azure-threat-detection-fig13.png)

-   Beveiliging tegen SQL-injecties

-   Beveiliging tegen scripting op meerdere sites

-   Beveiliging tegen veelvoorkomende aanvallen via internet, zoals opdrachtinjectie, het smokkelen van HTTP-aanvragen, het uitsplitsen van HTTP-antwoorden en aanvallen waarbij een extern bestand wordt ingesloten

-   Beveiliging tegen schendingen van het HTTP-protocol

-   Beveiliging tegen afwijkingen van het HTTP-protocol, zoals een gebruikersagent voor de host en Accept-headers die ontbreken

-   Beveiliging tegen bots, crawlers en scanners

-   Detectie van veelvoorkomende onvolkomenheden in toepassing (dat wil zeggen, Apache, IIS, enz.)

WAF bij toepassingsgateway configureren, geeft u het volgende voordeel voor u:

-   U beveiligt de webtoepassing tegen internetkwetsbaarheden en aanvallen zonder dat u de code voor de back-ends hoeft aan te passen.

-   U beveiligt gelijktijdig meerdere webtoepassingen achter de toepassingsgateway. Application Gateway ondersteunt het hosten van maximaal 20 websites achter één gateway, die alle kunnen worden beveiligd tegen aanvallen via internet.

-   U controleert de webtoepassing tegen aanvallen met behulp van een realtime rapport dat wordt gegenereerd door de WAF-logboeken in Application Gateway.

-   Voor bepaald nalevingsbeheer moeten alle eindpunten die zijn verbonden met internet worden beveiligd door een WAF-oplossing. Wanneer u Application Gateway met de WAF-functionaliteit gebruikt, kunt u aan deze nalevingsvereisten voldoen.

### <a name="anomaly-detection--an-api-built-with-azure-machine-learning"></a>Afwijkingsdetectie – een gebouwd met Azure Machine Learning API

Afwijkingsdetectie is een API die is gebouwd met Azure Machine Learning die nuttig is voor het detecteren van verschillende soorten afwijkende patronen in de tijd reeksgegevens. De API een score afwijkingsdetectie toegewezen aan elk gegevenspunt in de tijdreeks die kan worden gebruikt voor het genereren van waarschuwingen, bewaking via dashboards of verbinding te maken met uw ticketsysteem systemen.

De [Afwijkingsdetectie detectie API](https://docs.microsoft.com/azure/machine-learning/machine-learning-apps-anomaly-detection-api) kan de volgende typen afwijkingen op tijd reeksgegevens detecteren:

-   **Bereikt en daalt:** bijvoorbeeld bij de bewaking van het aantal mislukte aanmeldingen met een service of het nummer van de opdrachten om uitchecken in een e-commercesite ongebruikelijke pieken of dips kunnen wijzen op beveiligingsaanvallen of service-onderbrekingen.

-   **Positieve en negatieve trends:** wanneer het geheugengebruik in cloudcomputing bewaking, bijvoorbeeld verkleinen van de grootte van beschikbaar geheugen is indicatief voor een mogelijke Geheugenlek; bij de bewaking van de service-wachtrijlengte een voortdurende opwaartse trend kan duiden op een onderliggende softwareprobleem.

-   **Wijzigingen en wijzigingen in dynamische waardenbereik op niveau:** bijvoorbeeld niveau wijzigingen in de latenties van een service nadat een service bijwerken of lagere niveaus van uitzonderingen na de upgrade kan interessante om te controleren.

API waarmee op basis van de machine learning:

-   **Flexibele en robuuste detectie:** de modellen afwijkingsdetectie-detectie toestaan dat gebruikers gevoeligheid instellingen configureren en afwijkingen voor seizoensgebonden en niet-seizoensgebonden gegevenssets detecteren. Gebruikers kunnen het model van de detectie afwijkingsdetectie zodat de API-detectie meer of minder gevoelig naargelang hun behoeften aanpassen. Dit betekent dat de meer of minder zichtbaar afwijkingen in de gegevens met en zonder seizoensgebonden patronen te detecteren.

-   **Schaalbare en tijdige detectie:** de traditionele manier van bewaking met aanwezig drempels, opgegeven door deskundigen domein kennis zijn kostbaar en geen schaalbare naar miljoenen dynamisch veranderende gegevenssets. De afwijkingsdetectie detectie modellen in deze API worden geleerd en modellen automatisch zijn afgestemd van historische en realtime-gegevens.

-   **Detectie van proactieve en actie worden uitgevoerd:** trage trend en detectie van de wijzigingen kunnen worden toegepast voor vroege afwijkingsdetectie. De vroege abnormale signalen gedetecteerd kunnen worden gebruikt om mensen te onderzoeken en reageren op de probleemgebieden leiden.  Bovendien hoofdoorzaak analysis modellen en waarschuwen hulpprogramma's boven op dit afwijkingsdetectie API-service kunnen worden ontwikkeld.

De afwijkingsdetectie API is een oplossing effectiever en efficiënter voor een breed scala aan scenario's zoals de servicestatus & KPI bewaking, IoT, bewaking van toepassingsprestaties en controle van netwerkverkeer. Hier volgen enkele populaire scenario's waar deze API kan nuttig zijn:
- IT-afdelingen hebben middelen nodig voor gebeurtenissen bijhouden, foutcode logboek voor informatie over het gebruik en prestaties (CPU, geheugen, enzovoort) tijdig.

-   Online commercewebsites wilt bijhouden klantactiviteiten, paginaweergaven en klikt op.

-   Hulpprogramma voor bedrijven wilt bijhouden verbruik van water, gas, elektriciteit en andere bronnen.

-   Faciliteit/gebouw beheerservices wilt temperatuur, vocht en verkeer controleren.

-   IoT/fabrikanten wilt sensorgegevens in tijdreeks werkstroom monitor en kwaliteit gebruiken.

-   Service-providers, zoals aanroep centers moeten trend in de service-aanvraag, volume aan incidenten controleren, wacht wachtrijlengte enzovoort.

-   Analytics bedrijfsgroepen wilt bewaken van zakelijke KPI's (zoals sales volume klant patronen, prijzen) abnormale gegevensverplaatsing in realtime.

### <a name="cloud-app-security"></a>Cloud App Security

[Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) is een essentieel onderdeel van de Microsoft Cloud Security-stack. Er is een uitgebreide oplossing die uw organisatie helpen kan naar volledig te profiteren van de belofte van cloud-toepassingen, maar zorgen ervoor dat u controle, dankzij verbeterde zichtbaarheid in de activiteit. Het helpt tevens de beveiliging van essentiële gegevens in cloud-toepassingen te verhogen.

Met hulpprogramma's waarmee u shadow IT onthullen, risico's te beoordelen, beleid af te dwingen, activiteiten te onderzoeken en bedreigingen te stoppen, kunt uw organisatie veiliger verplaatsen naar de cloud terwijl controle van essentiële gegevens wordt behouden.

<table style="width:100%">
 <tr>
   <td>Ontdekken</td>
   <td>Onthullen shadow IT-medewerkers met Cloud App Security. Inzicht krijgen door apps, activiteiten, gebruikers, gegevens en bestanden in uw cloudomgeving te detecteren. Apps van derden die zijn verbonden met uw cloud detecteren.</td>
 </tr>
 <tr>
   <td>Onderzoeken</td>
   <td>Onderzoek uw cloud-apps met behulp van cloud forensische hulpprogramma's voor dieper in riskante apps, specifieke gebruikers en bestanden in uw netwerk. Zoek patronen in de gegevens die worden verzameld uit de cloud. Rapporten voor het bewaken van uw cloud genereren.</td>

 </tr>
 <tr>
   <td>Besturingselement</td>
   <td>Beperk risico's door het instellen van beleidsregels en waarschuwingen om maximale controle over het netwerkverkeer van de cloud. Cloud App Security gebruiken uw gebruikers migreren naar alternatieven voor veilige, erkende cloud-app.</td>

 </tr>
 <tr>
   <td>Beschermen</td>
   <td>Met Cloud App Security erkennen of verbieden toepassingen, afgedwongen preventie van gegevensverlies, machtigingen voor beheer en te delen en aangepaste rapporten en waarschuwingen te genereren.</td>

 </tr>
 <tr>
   <td>Besturingselement</td>
   <td>Beperk risico's door het instellen van beleidsregels en waarschuwingen om maximale controle over het netwerkverkeer van de cloud. Cloud App Security gebruiken uw gebruikers migreren naar alternatieven voor veilige, erkende cloud-app.</td>

 </tr>
</table>


![Cloud App Security](./media/azure-threat-detection/azure-threat-detection-fig14.png)

Cloud App Security integreert zichtbaarheid met uw cloud door

-   Met behulp van Cloud Discovery toewijzen en identificeren van uw cloudomgeving en de cloud-apps maakt gebruik van uw organisatie.


-   Verbieden van apps in uw cloud en de.



-   Met behulp van eenvoudig te implementeren app-connectors die gebruikmaken van de provider-API's, voor zichtbaarheid en beheer van apps waarmee u verbinding maakt.

-   Helpt u doorlopende controle door de instelling en voortdurend aan te passen, beleid.

Cloud App Security wordt op het verzamelen van gegevens van deze bronnen, geavanceerde analyses uitgevoerd op de gegevens. Meteen wordt u gewezen op afwijkende activiteiten en biedt u grondige zichtbaarheid in uw cloudomgeving. U kunt een beleid in de Cloud App Security configureren en gebruiken om te beveiligen alles in uw cloudomgeving.

## <a name="third-party-atd-capabilities-through-azure-marketplace"></a>Mogelijkheden van de derde partij ATD via Azure Marketplace

### <a name="web-application-firewall"></a>Web Application Firewall

Web Application Firewall inspecteert web binnenkomend verkeer en -blokken SQL-injectie, Cross-Site Scripting, malware uploads & DDoS-toepassing en andere aanvallen die gericht is op uw webtoepassingen. Deze ook inspecteert de antwoorden van de back-end-webservers gegevens gegevensverlies te voorkomen (DLP). De engine voor het besturingselement geïntegreerde toegang kan beheerders gedetailleerde toegangscontrolebeleid maken voor verificatie, autorisatie en Accounting (AAA), waarmee organisaties sterke verificatie en Gebruikersbeheer.

**Licht:**
-   Detecteert en blokkeert SQL injectie, Cross-Site Scripting, malware uploads, DDoS-toepassing of andere aanvallen op basis van uw toepassing.

-   Verificatie en toegangsbeheer.

-   Uitgaand verkeer voor het detecteren van gevoelige gegevens en kunt maskeren of blokkeren van de informatie uitlekken uit scant.

-   De bezorging van web application inhoud, met mogelijkheden, zoals caching, compressie en andere optimalisaties verkeer wordt versneld.

Voorbeeld van Web Application firewalls beschikbaar in de Azure-marktplaats volgen:

[Barracuda Web Application Firewall, Brocade virtuele Web Application Firewall (Brocade vWAF), Imperva SecureSphere en de ThreatSTOP IP-Firewall.](https://azure.microsoft.com/marketplace/partners/brocade_communications/brocade-virtual-web-application-firewall-templatevtmcluster/)

## <a name="next-steps"></a>Volgende stappen

- [Detectiemogelijkheden van Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities)

Mogelijkheden van Azure Security Center geavanceerde detectie kan worden geïdentificeerd actieve bedreigingen die gericht is op uw Microsoft Azure-resources en biedt u de inzichten die nodig zijn voor het snel reageren.

- [Detectie van dreigingen voor Azure SQL Database](https://azure.microsoft.com/blog/azure-sql-database-threat-detection-your-built-in-security-expert/)

Azure SQL Database met detectie van dreigingen geholpen adres hun opmerkingen over potentiële bedreigingen voor de database.

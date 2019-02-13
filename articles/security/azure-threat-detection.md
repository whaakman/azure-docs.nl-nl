---
title: Azure advanced threat detection | Microsoft Docs
description: Meer informatie over Azure AD Identity Protection en de mogelijkheden ervan.
services: security
documentationcenter: na
author: UnifyCloud
manager: barbkess
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: 32e705c5c338d9bd7c16514b2dc2ab081a9caf6a
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56115552"
---
# <a name="azure-advanced-threat-detection"></a>Geavanceerde bedreigingsdetectie van Azure

Azure biedt ingebouwde functie voor de detectie geavanceerde bedreigingen via services zoals Azure Active Directory (Azure AD), Azure Log Analytics en Azure Security Center. Deze verzameling van beveiligingsservices en -mogelijkheden biedt een eenvoudige en snelle manier om te begrijpen wat er gebeurt in uw Azure-implementaties.

Azure biedt een breed scala aan opties voor het configureren en aanpassen van beveiliging om te voldoen aan de vereisten van uw app-implementaties. In dit artikel wordt beschreven hoe u aan deze vereisten voldoen.

## <a name="azure-active-directory-identity-protection"></a>Azure Active Directory Identity Protection

[Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) is een [Azure Active Directory Premium P2](https://docs.microsoft.com/azure/active-directory/active-directory-editions) edition-functie waarmee een overzicht van de risicogebeurtenissen en mogelijke beveiligingsproblemen die invloed kunnen zijn op de identiteiten van uw organisatie. Identity Protection maakt gebruik van bestaande Azure AD-anomaliedetectie-mogelijkheden die beschikbaar via zijn [Azure AD-afwijkende activiteitenrapporten](https://docs.microsoft.com/azure/active-directory/active-directory-view-access-usage-reports), en introduceert nieuwe typen risicogebeurtenissen die realtime afwijkingen kunnen detecteren.

![Azure AD Identity Protection-diagram](./media/azure-threat-detection/azure-threat-detection-fig1.png)

Identity Protection maakt gebruik van geavanceerde machine learning-algoritmen en methodiek voor het opsporen van afwijkingen en risicogebeurtenissen die aangeven mogelijk dat een identiteit is aangetast. Met behulp van deze gegevens genereert Identity Protection rapporten en waarschuwingen zodat u kunt deze risicogebeurtenissen onderzoeken en juiste herstel of risicobeperking actie.

Azure Active Directory Identity Protection is meer dan een controle- en rapportagetool. Op basis van risico's, berekend Identity Protection een risiconiveau van de gebruiker voor elke gebruiker zodat u risico's gebaseerd beleid voor het automatisch beveiligen van de identiteiten van uw organisatie kunt configureren.

Deze risico's gebaseerd beleid, naast andere [besturingselementen voor voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) die worden geleverd door Azure Active Directory en [EMS](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access), kunnen automatisch blokkeren of adaptieve herstelacties bieden die wachtwoord opnieuw instellen van wachtwoorden en het afdwingen van multi-factor authentication bevatten.

### <a name="identity-protection-capabilities"></a>Mogelijkheden voor identiteits-beveiliging

Azure Active Directory Identity Protection is meer dan een controle- en rapportagetool. Ter bescherming van identiteiten in uw organisatie, kunt u risico's gebaseerd beleid die automatisch op gedetecteerde problemen reageren wanneer een opgegeven risiconiveau is bereikt. Deze beleidsregels, naast andere besturingselementen voor voorwaardelijke toegang is geleverd door Azure Active Directory en EMS, kunnen automatisch blokkeren of adaptieve herstelacties met inbegrip van wachtwoorden en meervoudige verificatie afdwingen initiëren.

Enkele voorbeelden van een van de manieren waarop Azure Identity Protection kan helpen beveiligen uw accounts en id's omvatten:

[Detecteren van risico's en riskante accounts](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection#detection)
-   Zes risicogebeurtenistypen met machine learning en heuristische regels detecteren.
-   Bereken de risiconiveaus van de gebruiker.
-   Geef aangepaste aanbevelingen voor het totale beveiligingspostuur verbeteren door het markeren van beveiligingsproblemen.

[Risicogebeurtenissen onderzoeken](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection#investigation)
-   Meldingen voor risicogebeurtenissen verzenden.
-   Risicogebeurtenissen die met behulp van relevante en contextuele gegevens onderzoeken.
-   Bieden eenvoudige werkstromen voor het volgen van onderzoeken.
-   Eenvoudig toegang bieden tot herstelacties, zoals wachtwoord opnieuw instellen.

[Beleid voor risico's gebaseerde, voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)
-   Riskante aanmeldingen beperken door het blokkeren van aanmeldingen of uitdagingen voor meervoudige verificatie vereisen.
-   Blokkeren of riskante gebruikersaccounts te beveiligen.
-   Vereisen dat gebruikers zich registreren voor meervoudige verificatie.

### <a name="azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management

Met [Azure Active Directory Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure), u kunt beheren, beheren en te controleren, toegang tot binnen uw organisatie. Deze functie omvat toegang tot resources in Azure AD en andere Microsoft online services, zoals Office 365 of Microsoft Intune.

![Azure AD Privileged Identity Management-diagram](./media/azure-threat-detection/azure-threat-detection-fig2.png)

PIM kunt u:

-   Ontvang waarschuwingen en rapporten over Azure AD-beheerders en just-in-time (JIT) met beheerdersrechten toegang tot Microsoft online services, zoals Office 365 en Intune.

-   Rapporten over beheerder beheerderstoeganggeschiedenis en wijzigingen in toewijzingen van beheerder ophalen.

-   Ontvang waarschuwingen over de toegang tot een bevoorrechte rol.

## <a name="azure-log-analytics"></a>Azure Log Analytics

[Log Analytics](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) is een Microsoft cloud-gebaseerde IT-beheeroplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloudinfrastructuur. Omdat de Log Analytics wordt geïmplementeerd als een cloud-gebaseerde service, kunt u laten actief en werkend snel met minimale investeringen in infrastructuurservices. Nieuwe beveiligingsfuncties worden automatisch geleverd, voortdurende onderhoud opslaan en kosten voor een upgrade uitvoert.

Naast het leveren van waardevolle services op een eigen, Log Analytics kan worden geïntegreerd met System Center-onderdelen, zoals [System Center Operations Manager](https://blogs.technet.microsoft.com/cbernier/2013/10/23/monitoring-windows-azure-with-system-center-operations-manager-2012-get-me-started/), uit te breiden de beveiliging van uw bestaande investeringen in beheerfuncties in de cloud. System Center en Log Analytics kunnen samenwerken voor een volledige hybride beheer van ondervinden.

### <a name="holistic-security-and-compliance-posture"></a>Holistische beveiliging en naleving verbeteren

De [dashboard Log Analytics-beveiliging en controle](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) biedt een uitgebreid overzicht van uw organisatie beveiligingspostuur van IT, met ingebouwde zoekquery's voor belangrijke problemen die uw aandacht nodig hebben. Het dashboard beveiliging en controle is het startscherm voor alles met betrekking tot beveiliging in Log Analytics. Het zorgt voor hoogwaardig inzicht in de beveiligingsstatus van uw computers. U kunt ook alle gebeurtenissen van de afgelopen 24 uur, 7 dagen of andere aangepaste periode bekijken.

Log Analytics kunt u snel en eenvoudig inzicht in de algehele beveiligingsstatus van elke omgeving, allemaal binnen de context van IT-activiteiten, met inbegrip van software-evaluatie, antimalware-evaluatie en configuratiebasislijnen. Logboekgegevens van de beveiliging is gemakkelijk toegankelijk zijn voor het stroomlijnen van de beveiliging en naleving audit-processen.

![Het dashboard Log Analytics-beveiliging en controle](./media/azure-threat-detection/azure-threat-detection-fig3.jpg)

De Log Analytics-beveiliging en controle-dashboard is onderverdeeld in vier hoofdcategorieën:

-   **Beveiligingsdomeinen**: Hiermee kunt u nader bestuderen beveiligingsrecords in de loop van de tijd; toegang tot malware-evaluaties; Update-evaluaties; netwerkbeveiliging, identiteit en toegang krijgen tot informatie; weergeven computers met beveiligingsgebeurtenissen; weergeven en krijg snel toegang tot het Azure Security Center-dashboard.

-   **Problemen die aandacht vereisen**: Hiermee kunt u snel het aantal actieve problemen en de ernst van de problemen kunt identificeren.

-   **Detecties (Preview)**: Hiermee kunt u aanvalspatronen identificeren door beveiligingswaarschuwingen weergeven wanneer deze zich op basis van uw resources voordoen.

-   **Bedreigingsinformatie**: Hiermee kunt u aanvalspatronen identificeren door het totale aantal servers met schadelijk uitgaand IP-verkeer, het type schadelijke dreigingen en een overzicht van de IP-adressen locaties weer te geven.

-   **Algemene Beveiligingsquery's**: Geeft een lijst van de meest voorkomende Beveiligingsquery's die u gebruiken kunt voor het bewaken van uw omgeving. Wanneer u een query selecteert, wordt het zoekvenster wordt geopend en de resultaten voor deze query worden weergegeven.

### <a name="insight-and-analytics"></a>Inzicht en analyse
In het centrum van [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) is de opslagplaats, die wordt gehost door Azure.

![Inzicht en analyse-diagram](./media/azure-threat-detection/azure-threat-detection-fig4.png)

U verzamelen gegevens in de opslagplaats van verbonden bronnen door te configureren van gegevensbronnen en oplossingen toe te voegen aan uw abonnement.

![De Log Analytics-dashboard ](./media/azure-threat-detection/azure-threat-detection-fig5.png)

Gegevensbronnen en oplossingen voor elke afzonderlijke recordtypen maken met hun eigen set eigenschappen, maar u kunt nog steeds analyseren ze samen in query's naar de opslagplaats. U kunt dezelfde hulpprogramma's en methoden gebruiken om te werken met een verscheidenheid aan gegevens die worden verzameld door verschillende bronnen.


Het merendeel van uw interactie met Log Analytics is via de Azure-portal, die wordt uitgevoerd in een browser en biedt u toegang tot configuratie-instellingen en verschillende hulpprogramma's om te analyseren en benutten van verzamelde gegevens. Vanuit de portal, kunt u het volgende gebruiken:
* [Zoekopdrachten](https://docs.microsoft.com/azure/log-analytics/log-analytics-log-searches) waar het samenstellen van query's om verzamelde gegevens te analyseren.
* [Dashboards](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-logs-dashboards), die u kunt aanpassen met grafische views van uw waardevolste zoekopdrachten.
* [Oplossingen](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions), die aanvullende functionaliteit en analysehulpprogramma's bieden.

![Analyseprogramma 's](./media/azure-threat-detection/azure-threat-detection-fig6.png)

Oplossingen voegen functionaliteit toe aan Log Analytics. Deze voornamelijk uitgevoerd in de cloud en analyseren gegevens die worden verzameld in de Log Analytics-opslagplaats. Oplossingen mogelijk ook nieuwe recordtypen moeten worden verzameld die kunnen worden geanalyseerd met zoekopdrachten in Logboeken of met behulp van een aanvullende gebruikersinterface die de oplossing in de Log Analytics-dashboard biedt definiëren.

Het dashboard beveiliging en controle is een voorbeeld van deze typen oplossingen.

### <a name="automation-and-control-alert-on-security-configuration-drifts"></a>Automation en beheer: Waarschuwing bij security configuration groepen

Azure Automation automatiseert u administratieve processen met runbooks die zijn gebaseerd op PowerShell en voer in de cloud. Runbooks kunnen ook worden uitgevoerd op een server in uw lokale datacentrum om lokale resources te beheren. Azure Automation biedt Configuratiebeheer met PowerShell Desired State Configuration (DSC).

![Azure Automation diagram](./media/azure-threat-detection/azure-threat-detection-fig7.png)

U kunt maken en beheren van DSC-resources die worden gehost in Azure en deze toepassen op de cloud en on-premises systemen. Door te voeren, zodat u kunt definiëren en automatisch afdwingen van de configuratie of haal rapporten op over drift om ervoor te zorgen dat beveiliging met blijven configuraties binnen beleid.

## <a name="azure-security-center"></a>Azure Security Center

Azure Security Center helpt te beschermen van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen. In de service, kunt u het beleid op basis van zowel uw Azure-abonnementen en [resourcegroepen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal) voor grotere mate van granulatie.

![Azure Security Center-diagram](./media/azure-threat-detection/azure-threat-detection-fig8.png)

Beveiligingsonderzoekers van Microsoft zijn voortdurend op zoek naar bedreigingen. Ze hebben toegang tot een uitgebreide set telemetrie, afkomstig van de wereldwijde aanwezigheid van Microsoft in de cloud en on-premises. Deze verrijkende en uitgebreide verzameling gegevenssets stelt Microsoft in staat om nieuwe aanvalspatronen en -trends te ontdekken in on-premises producten voor consumenten en ondernemingen, evenals in haar online services.

Daarom kunt Security Center de detectie-algoritmen snel bijwerken wanneer aanvallers met nieuwe en steeds geavanceerder wordende aanvallen. Deze aanpak helpt u houdt gelijke tred met een dreigingsomgeving ontdekt.

![Detectie van bedreigingen van Security Center](./media/azure-threat-detection/azure-threat-detection-fig9.jpg)

Het detecteren van bedreigingen van Security Center werkt volgens het automatisch verzamelen van beveiligingsgegevens van uw Azure-resources, het netwerk en verbonden partneroplossingen. Deze informatie, correleren van gegevens uit meerdere bronnen, om te identificeren van bedreigingen wordt geanalyseerd.

Beveiligingswaarschuwingen krijgen in Security Center een prioriteit, evenals aanbevelingen voor het oplossen van de bedreiging.

Security Center maakt gebruik van geavanceerde beveiligingsanalyses die veel verder gaan dan op handtekeningen gebaseerde benaderingen. Doorbraken in big data en [machine learning](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) worden gebruikt voor het evalueren van gebeurtenissen in de hele cloudinfrastructuur. Geavanceerde analyse kan bedreigingen detecteren die onmogelijk te identificeren door middel van handmatige benaderingen en het voorspellen van de ontwikkeling van aanvallen. Deze beveiligingstypen analytics worden in de volgende secties besproken.

### <a name="threat-intelligence"></a>Informatie over bedreigingen

Microsoft heeft toegang tot een gigantische hoeveelheid informatie over wereldwijde dreigingen.

Telemetrie in loopt van meerdere bronnen, zoals Azure, Office 365, Microsoft CRM online, Microsoft Dynamics AX, outlook.com, MSN.com, de Microsoft Digital Crimes Unit (DCU) en Microsoft Security Response Center (MSRC).

![Threat intelligence bevindingen](./media/azure-threat-detection/azure-threat-detection-fig10.jpg)

Onderzoekers ontvangen ook informatie over bedreigingen die wordt gedeeld door de primaire cloudserviceproviders en ze zich abonneren op feeds met informatie over bedreigingen van derden. Azure Security Center kan deze informatie gebruiken om u te waarschuwen over bedreigingen van bekende beveiligingsrisico’s. Voorbeelden zijn:

-   **Benutten de kracht van machine learning**: Azure Security Center heeft toegang tot een grote hoeveelheid gegevens over de cloudnetwerkactiviteit, die kan worden gebruikt voor het detecteren van bedreigingen die gericht is op uw Azure-implementaties.

-   **Detectie van beveiligingsaanvallen**: Machine learning wordt gebruikt om een historische patroon van pogingen tot externe toegang, zodat er voor het detecteren van beveiligingsaanvallen op poorten voor Secure Shell (SSH), Remote Desktop Protocol (RDP) en SQL kan worden te maken.

-   **Uitgaande DDoS en botnet detectie**: Een algemene doelstelling van aanvallen die zijn gericht op cloud-bronnen is het gebruik van de compute-kracht van deze resources het uitvoeren van andere aanvallen.

-   **Nieuwe gedragsanalyses servers en virtuele machines**: Nadat een server of virtuele machine is geknoeid, aanvallers maken gebruik van een groot aantal technieken om schadelijke code uitvoeren op dat systeem tijdens het vermijden van detectie, ervoor te zorgen dat de persistentie en besturingselementen voor de beveiliging en vermindert.

-   **Azure SQL Database Threat Detection**: Detectie van bedreigingen voor Azure SQL Database, die identificeert afwijkende database-activiteiten die ongebruikelijke en potentieel schadelijke aangeven probeert te openen of misbruik te maken van databases.

### <a name="behavioral-analytics"></a>Gedragsanalyse

Gedragsanalyse is een techniek waarbij gegevens worden geanalyseerd en vergeleken met een verzameling bekende patronen. Deze patronen zijn echter geen eenvoudige handtekeningen. Ze worden vastgesteld aan de hand van complexe machine learning-algoritmen die worden toegepast op grote gegevenssets.

![Gedragsanalyse bevindingen](./media/azure-threat-detection/azure-threat-detection-fig11.jpg)

De patronen worden ook vastgesteld via de zorgvuldige analyse van schadelijk gedrag door deskundige analisten. Azure Security Center kan gebruikmaken van gedragsanalyses om verdachte resources op basis van de analyse van Logboeken van de virtuele machine, virtuele netwerken, infrastructuurlogboeken, crashdumps en andere bronnen te identificeren.

Bovendien worden patronen gecorreleerd met andere signalen om te controleren op ondersteunend bewijs van een wijdverbreide aanval. Aan de hand van dit verband kan beter worden vastgesteld welke gebeurtenissen samenhangen met de opgestelde indicatoren van inbreuk.

Voorbeelden zijn:
-   **Uitvoering van verdachte processen**: Aanvallers maken gebruik van verschillende technieken voor het uitvoeren van schadelijke software zonder te worden gedetecteerd. Bijvoorbeeld, kan een aanvaller schadelijke software de dezelfde namen geven als legitieme systeembestanden, maar plaatst deze bestanden op een andere locatie, gebruik een naam die is vergelijkbaar met die van een goedaardig bestand of de echte bestandsextensie maskeren. Security Center modellen verwerken gedrag en uitvoeringen door een proces voor het detecteren van uitschieters als deze monitor.

-   **Verborgen schadelijke software en pogingen tot misbruik**: Geavanceerde schadelijke software kunt omzeilen traditionele antimalwareproducten door nooit naar schijf schrijven of softwareonderdelen die zijn opgeslagen op schijf. Dergelijke schadelijke software kan echter worden gedetecteerd met behulp van de analyse van geheugengebruik, omdat de malware traceringen in het geheugen van functie achterlaten moet. Wanneer software vastloopt, bevat een crashdump een gedeelte van het geheugen ten tijde van de crash. Door het analyseren van het geheugen in de crashdump, Azure Security Center technieken die worden gebruikt voor het misbruik van zwakke plekken in software, toegang krijgen tot vertrouwelijke gegevens en ongemerkt aanwezig te blijven in een aangetaste machine zonder gevolgen voor de prestaties van detecteren uw machine.

-   **Laterale verplaatsing en interne verkenning**: Als u wilt behouden in een aangetast netwerk en zoeken en waardevolle gegevens verzamelt, proberen aanvallers vaak zich lateraal van het aangetaste apparaat om naar te verplaatsen anderen binnen hetzelfde netwerk. Security Center controleert processen en aanmeldingsactiviteiten voor het detecteren van pogingen om uit te breiden een kwaadwillende persoon voetafdruk binnen het netwerk, zoals externe opdrachten uit te voeren, netwerk scannen en inventarisatie van accounts.

-   **Schadelijke PowerShell-scripts**: PowerShell kan worden gebruikt door aanvallers om schadelijke code uitvoeren op de virtuele doelmachines voor verschillende doeleinden. Security Center inspecteert PowerShell-activiteit op tekenen van verdachte activiteiten.

-   **Uitgaande aanvallen**: Aanvallers richten vaak op cloudresources met het doel van het gebruik van deze resources voor het uitvoeren van extra aanvallen. Aangetaste virtuele machines kunnen kan bijvoorbeeld worden gebruikt om te starten beveiligingsaanvallen op andere virtuele machines, ongewenste e-mail te verzenden of scannen van poorten openen en andere apparaten op internet. Door machine learning toe te passen op netwerkverkeer kan Security Center het detecteren wanneer uitgaande netwerkcommunicatie groter is dan de norm. Wanneer spam wordt gedetecteerd, Security Center ook een koppeling tussen ongebruikelijk e met informatie uit Office 365 om te bepalen of het e-mailbericht waarschijnlijk kwaadwillend of het resultaat van een geldige e-campagne.

### <a name="anomaly-detection"></a>Afwijkingsdetectie

Azure Security Center maakt ook gebruik van afwijkingsdetectie om bedreigingen te identificeren. In tegenstelling tot gedragsanalyses (die afhankelijk zijn van bekende patronen die zijn afgeleid van grote gegevenssets) is afwijkingsdetectie meer "gepersonaliseerd" en richt het zich op basislijnen die specifiek voor uw implementaties zijn. Machine learning wordt toegepast om te bepalen van de normale activiteit voor uw implementaties en vervolgens worden regels gegenereerd om uitschieter voorwaarden die kunnen staan voor een beveiligingsgebeurtenis te definiëren. Hier volgt een voorbeeld:

-   **Inkomende RDP/SSH-beveiligingsaanvallen**: Uw implementaties mogelijk drukke virtuele machines met een groot aantal aanmeldingen per dag en andere virtuele machines met enkele, indien van toepassing, aanmeldingen. Azure Security Center kunt Aanmeldingsactiviteit voor deze virtuele machines vaststellen en gebruik van machine learning om te definiëren om de normale aanmeldactiviteiten. Als er een verschil met de basislijn die is gedefinieerd voor aanmelding gerelateerde kenmerken, kan een waarschuwing worden gegenereerd. Ook hier weer wordt door machine learning bepaald wat een aanzienlijk verschil is.

### <a name="continuous-threat-intelligence-monitoring"></a>Doorlopende controle van informatie over bedreigingen

Azure Security Center werkt met onderzoeks- en data science beveiligingsteams overal ter wereld die continu op veranderingen in bedreigingen controleren. Dit omvat de volgende initiatieven:

-   **Controle van informatie over bedreigingen**: Informatie over bedreigingen omvat mechanismen, indicatoren, implicaties en advies over bestaande of nieuwe bedreigingen. Deze informatie wordt gedeeld in de beveiligingscommunity, en Microsoft volgt continu feeds met informatie over bedreigingen uit interne en externe bronnen.

-   **Signalen delen**: Inzichten op basis van beveiliging teams in de brede Microsoft portfolio van cloud en on-premises services, servers en client-endpointapparaten worden gedeeld en geanalyseerd.

-   **Microsoft-beveiligingsspecialisten**: Continue inzet van teams overal bij Microsoft die op gespecialiseerde beveiligingsgebieden, zoals forensisch onderzoek doen en web aanvalsdetectie werken.

-   **Detectieafstemming**: Algoritmen worden uitgevoerd op echte gegevenssets en beveiligingsonderzoekers werken samen met klanten om de resultaten te valideren. Echte en fout-positieven worden gebruikt voor het verfijnen van machine learning-algoritmen.

Deze gecombineerde inspanningen resulteren in nieuwe en verbeterde detecties waarvan u onmiddellijk kunt profiteren. Er is geen actie te ondernemen.

## <a name="advanced-threat-detection-features-other-azure-services"></a>Geavanceerde threat detection functies: Andere Azure-services

### <a name="virtual-machines-microsoft-antimalware"></a>Virtuele machines: Microsoft antimalware

[Microsoft antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) voor Azure een één-agent-oplossing voor toepassingen en tenant-omgevingen is, ontworpen om uit te voeren op de achtergrond zonder menselijke tussenkomst. U kunt beveiliging op basis van de behoeften van uw werkbelastingen van toepassingen, met een standaard secure standaard of aangepaste configuratie, met inbegrip van antimalware monitoring geavanceerde implementeren. Azure anti-malware is een beveiligingsoptie voor virtuele machines van Azure die automatisch wordt geïnstalleerd op alle virtuele machines van Azure PaaS.

#### <a name="microsoft-antimalware-core-features"></a>Belangrijkste functies van Microsoft antimalware

Hier vindt u de functies van Azure die implementeren en Microsoft antimalware voor uw toepassingen inschakelen:

-   **Real-timebeveiliging**: Controleert de activiteit in cloudservices en op virtuele machines te detecteren en blokkeren van schadelijke software worden uitgevoerd.

-   **Geplande scan**: Tijd tot tijd voert gerichte scannen voor het detecteren van malware, met inbegrip van programma's actief wordt uitgevoerd.

-   **Oplossen van malware**: Automatisch omgaat met gedetecteerde schadelijke software, zoals het verwijderen of schadelijke bestanden in quarantaine plaatsen en opschonen van schadelijke registervermeldingen.

-   **Handtekeningupdates**: Installeert automatisch de meest recente protection handtekeningen (virusdefinities) om ervoor te zorgen dat de beveiliging bijgewerkt op een vooraf bepaald frequentie is.

-   **Antimalware-Engine-updates**: Werkt automatisch de Microsoft Antimalware-Engine.

-   **Updates voor anti-malware-platform**: Automatisch bijgewerkt met het anti-malwareplatform van Microsoft.

-   **Actieve beveiliging**: Telemetrie-metagegevens rapporten over gedetecteerde bedreigingen en verdachte resources naar Microsoft Azure om snel kun reageren op de zich ontwikkelende bedreigingen, realtime synchrone handtekening levering via de Microsoft active protection-systeem inschakelen.

-   **Voorbeelden van reporting**: Biedt en voorbeelden van rapporten naar de Microsoft antimalware-service om u te helpen bij het verfijnen van de service en het oplossen van inschakelen.

-   **Uitsluitingen**: Kunnen toepassingen en servicebeheerders om bepaalde bestanden, processen en stations voor uitsluiting van beveiliging en het scannen van prestaties en andere redenen te configureren.

-   **Antimalware-gebeurtenisverzameling**: Registreert de antimalware-servicestatus, verdachte activiteiten en herstelacties die zijn uitgevoerd in het gebeurtenislogboek van het besturingssysteem en verzamelt deze naar Azure storage-account van de klant.

### <a name="azure-sql-database-threat-detection"></a>Azure SQL Database Threat Detection

[Azure SQL Database Threat Detection](https://azure.microsoft.com/blog/azure-sql-database-threat-detection-your-built-in-security-expert/) is een nieuwe beveiligings-intelligence functie die is ingebouwd in de Azure SQL Database-service. Het ononderbroken werkt als u wilt leren, profileren en afwijkende databaseactiviteiten worden gedetecteerd, identificeert Azure SQL Database Threat Detection potentiële bedreigingen voor de database.

Security officers of andere aangewezen administrators krijgt een onmiddellijke melding over verdachte databaseactiviteiten wanneer deze zich voordoen. Elke melding vindt u informatie over de verdachte activiteit en raadt aan om het verder te onderzoeken en tegenhouden.

Op dit moment detecteert Azure SQL Database Threat Detection mogelijke beveiligingsproblemen en SQL-injectieaanvallen en afwijkende patronen voor databasetoegang.

Na ontvangst van een e-mailmelding voor detectie van bedreigingen, kunnen gebruikers zich om te navigeren en de relevante controlerecords via een dieptekoppeling weergeven in het e-mailbericht. De koppeling opent u een audit-viewer of een vooraf geconfigureerde controle Excel sjabloon waarin de relevante controlerecords rond de tijd van de verdachte activiteit, op basis van het volgende:

-   Opslag voor de databaseserver met de afwijkende activiteiten controleren.

-   De opslagtabel relevante controle die op het moment van de gebeurtenis is gebruikt voor het schrijven van het auditlogboek.

-   Controlerecords van het uur direct na het optreden van de gebeurtenis.

-   Controlerecords met een soortgelijke gebeurtenis-ID op het moment van de gebeurtenis (optioneel voor sommige detectoren).

SQL Database threat detectoren gebruiken een van de volgende methoden voor detectie:

-   **Deterministische detectie**: Detecteert verdachte patronen (regels op basis van) in de SQL-client-query's die overeenkomen met bekende aanvallen. Deze methodologie biedt detectie van hoge en lage ONWAAR positief, maar beperkte dekking, omdat deze valt binnen de categorie "atomic detecties."

-   **Detectie van gebruikersgedrag**: Detecteert afwijkende activiteiten, wat abnormaal gedrag in de database die niet tijdens de laatste 30 dagen gevonden is. Voorbeelden van SQL-client afwijkende activiteit is een piek van de mislukte aanmeldingen of query's, een grote hoeveelheid gegevens die zijn uitgepakt, ongebruikelijke canonieke query's of niet bekend IP-adressen die worden gebruikt voor toegang tot de database.

### <a name="application-gateway-web-application-firewall"></a>Application Gateway Web Application Firewall

[Web Application Firewall (WAF)](../app-service/environment/app-service-app-service-environment-web-application-firewall.md) is een functie van [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-webapplicationfirewall-overview) die beveiliging biedt voor webtoepassingen die gebruikmaken van een application gateway voor standaard [application delivery control](https://kemptechnologies.com/in/application-delivery-controllers) functies. Web Application Firewall doet dit door deze te beschermen tegen de meeste van de [Open Web Application Security Project (OWASP) top 10 veelvoorkomende web beveiligingslekken](https://www.owasp.org/index.php/Top_10_2010-Main).

![Diagram van Application Gateway Web Application Firewall](./media/azure-threat-detection/azure-threat-detection-fig13.png)

Beveiligingen zijn onder andere:

-   Beveiliging van SQL-injectie.

-   Cross-site scripting beveiliging.

-   Algemene Web aanvallen beveiliging, zoals opdracht injectie, HTTP-aanvraag smokkelen, HTTP-antwoorden en aanvallen van extern bestand opgenomen.

-   Beveiliging tegen schendingen van de HTTP-protocol.

-   Beveiliging tegen afwijkingen van HTTP-protocol, zoals ontbrekende gebruikersagent host en accept-headers.

-   Preventie tegen bots, crawlers en scanners.

-   Detectie van veelvoorkomende onjuiste configuraties van toepassingen (dat wil zeggen, Apache, IIS, enzovoort).

WAF configureert in uw application gateway biedt de volgende voordelen:

-   Beschermt uw webtoepassing tegen internetkwetsbaarheden en aanvallen zonder wijziging van de back-end-code.

-   Meerdere webtoepassingen beschermt op hetzelfde moment achter een application gateway. Een application gateway ondersteunt het hosten van maximaal 20 websites.

-   Monitors webtoepassingen tegen aanvallen met behulp van realtime rapporten die worden gegenereerd door application gateway WAF-Logboeken.

-   Kunnen voldoen aan nalevingsvereisten. Bepaald nalevingsbeheer moeten alle internetgerichte eindpunten om te worden beveiligd met een WAF-oplossing.

### <a name="anomaly-detection-api-built-with-azure-machine-learning"></a>Anomaliedetectie-API: Gebouwd met Azure Machine Learning

De API voor Afwijkingsdetectie is een API die nuttig is voor het detecteren van tal van afwijkende patronen in uw time series-gegevens. De API een score van afwijkingen toegewezen aan elk gegevenspunt in de tijdreeks, die kunnen worden gebruikt voor het genereren van waarschuwingen, bewaking via dashboards of verbinding te maken met uw ticketsystemen.

De [API voor Afwijkingsdetectie](https://docs.microsoft.com/azure/machine-learning/machine-learning-apps-anomaly-detection-api) kan de volgende typen anomalieën op tijdreeksgegevensgegevens detecteren:

-   **Pieken en dalen**: Wanneer u het aantal mislukte aanmeldpogingen bij naar een service of het aantal kassabetalingen in een e-commerce-site, ongebruikelijke pieken controleert of Spanningsdips kunnen duiden op beveiligingsaanvallen of serviceonderbrekingen.

-   **Positieve en negatieve trends**: Wanneer u uw bewakingsgegevens geheugengebruik in computing, vrije-geheugengrootte verkleinen mogelijk geheugenlekkage aangegeven. Voor service-wachtrijlengte voor bewaking, kan een aanhoudende stijgende wijzen op een onderliggend softwareprobleem.

-   **Niveauwijzigingen en veranderingen in dynamisch bereik van waarden**: Niveau wijzigingen in de latenties van een service na een service-upgrade of lagere niveaus van uitzonderingen na de upgrade kan interessant om te controleren.

De machine learning gebaseerde API kunt:

-   **Flexibele en krachtige detectie**: De detectie van afwijkingen modellen kunnen gebruikers voor het configureren van instellingen voor hoofdlettergevoeligheid en afwijkingen voor seizoensgebonden als niet-seizoensgebonden gegevenssets. Gebruikers kunnen het model van de detectie van afwijkingen zodat de detection-API meer of minder gevoelige gegevens op basis van hun behoeften aanpassen. Dit betekent dat het detecteren van de meer of minder zichtbaar afwijkingen in de gegevens met en zonder seizoensgebonden patronen.

-   **Schaalbare en tijdige detectie**: De traditionele manier van bewaking met aanwezig drempelwaarden zijn ingesteld door de kennis van de experts kostbare en niet schaalbaar tot miljoenen dynamisch wijzigen van de gegevenssets. De modellen van de detectie van afwijkingen in deze API wordt geleerd en modellen automatisch zijn afgestemd op de basis van zowel historische als realtime-gegevens.

-   **Detectie van proactieve en bruikbare**: Trage trend en detectie van de wijzigingen kunnen worden toegepast voor vroege afwijkingsdetectie. De vroege abnormaal signalen die zijn gedetecteerd kunnen worden gebruikt om mensen te onderzoeken en reageren op de probleemgebieden regelen. Bovendien hoofdoorzaak analysis-modellen en hulpprogramma's voor waarschuwingen kunnen worden ontwikkeld op deze anomaliedetectie API-service.

De anomaliedetectie-API is een oplossing effectief en efficiënt voor een breed scala aan scenario's, zoals de servicestatus en KPI's controleren, IoT, bewaking van toepassingsprestaties en controle van netwerkverkeer. Hier volgen enkele populaire scenario's waarin deze API kan nuttig zijn:

- IT-afdelingen hebben middelen nodig voor gebeurtenissen bijhouden, foutcode log gebruik en prestaties (CPU, geheugen, enzovoort) tijdig.

-   Online commercewebsites wilt bijhouden klantactiviteiten, paginaweergaven en klikken.

-   Hulpprogramma voor bedrijven wilt bijhouden van gebruik van water, gas, elektriciteit en andere bronnen.

-   Faciliteit of gebouw management-services wilt gebruiken voor het bewaken van temperatuur, vochtigheid en verkeer.

-   IoT/fabrikanten wilt gebruiken van sensorgegevens in tijdreeks werkstroom monitor, kwaliteit, enzovoort.

-   Service-providers, zoals callcenters, moeten voor het bewaken van de trend in de service-aanvraag, volume aan incidenten, lengte van wachtrij, enzovoort.

-   Business analytics-groepen wilt bewaken van zakelijke KPI's (zoals omzet, klant sentimenten of prijzen) abnormaal verkeer in realtime.

### <a name="cloud-app-security"></a>Cloud App Security

[Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) is een essentieel onderdeel van de Microsoft Cloud Security-stack. Het is een uitgebreide oplossing die uw organisatie helpen kan terwijl u zich verplaatst om te profiteren van de belofte van cloud-toepassingen. Het blijft u controle dankzij verbeterde zichtbaarheid in de activiteit. Het verhoogt ook de beveiliging van essentiële gegevens in cloudtoepassingen.

Met hulpprogramma's om schaduw-IT te onthullen, risico's te beoordelen, beleid af te dwingen, activiteiten te onderzoeken en bedreigingen te stoppen, kan uw organisatie veiliger overstappen naar de cloud terwijl de controle over essentiële gegevens wordt behouden.

| | |
|---|---|
| Ontdekken | Onthul shadow IT met Cloud App Security. Krijg meer inzicht door apps, activiteiten, gebruikers, gegevens en bestanden in uw cloudomgeving te detecteren. Detecteer apps van derden die zijn verbonden met uw cloud.|
|Onderzoeken | Onderzoek uw cloud-apps met behulp van forensische hulpprogramma's voor cloud om riskante apps, specifieke gebruikers en bestanden in uw netwerk. Het vinden van patronen in de gegevens die worden verzameld van uw cloud. Maak rapporten om uw cloud te bewaken. |
| Beheer | Beperk risico's door beleidsregels en waarschuwingen om maximale controle over het netwerkverkeer van de cloud te stellen. Gebruik Cloud App Security voor uw gebruikers migreren naar alternatieven voor veilige, goedgekeurde cloud-app. |
| Beschermen | Cloud App Security kunt goedkeuren of toepassingen verbieden, af te dwingen van preventie van gegevensverlies, machtigingen en delen en aangepaste rapporten en waarschuwingen te genereren. |
| Beheer | Beperk risico's door beleidsregels en waarschuwingen om maximale controle over het netwerkverkeer van de cloud te stellen. Gebruik Cloud App Security voor uw gebruikers migreren naar alternatieven voor veilige, goedgekeurde cloud-app. |
| | |


![Cloud App Security-diagram](./media/azure-threat-detection/azure-threat-detection-fig14.png)

Cloud App Security wordt zichtbaarheid geïntegreerd met uw cloud door:

-   met behulp van Cloud Discovery toewijzen en identificeren van uw cloudomgeving en de cloud-apps maakt gebruik van uw organisatie.

-   Goedkeuren en verbieden van apps in de cloud.

-   met behulp van eenvoudig te implementeren app-connectors die gebruikmaken van de provider-API's, voor zichtbaarheid en beheer van apps waarmee u verbinding maken.

-   helpt u doorlopend controle te instelling en aanpassen, beleid hebben.

Op het verzamelen van gegevens van deze bronnen, voert Cloud App Security geavanceerde analyses erop. Onmiddellijk waarschuwt voor afwijkende activiteiten en u krijgt meer inzicht in uw cloudomgeving. U kunt een beleid configureren in Cloud App Security en het gebruiken om alles in uw cloudomgeving te beveiligen.

## <a name="third-party-advanced-threat-detection-capabilities-through-the-azure-marketplace"></a>Geavanceerde detectie van bedreigingen-mogelijkheden van derden via Azure Marketplace

### <a name="web-application-firewall"></a>Web Application Firewall

Web Application Firewall inspecteert het inkomend webverkeer en blokkeert SQL-injecties, cross-site scripting, malware-uploads, application DDoS-aanvallen en andere aanvallen die gericht is op uw webtoepassingen. Ook inspecteert deze de antwoorden van de back-end-webservers voor preventie van gegevensverlies (DLP). De geïntegreerde access control-engine kan beheerders maken gedetailleerde access controlebeleid voor verificatie, autorisatie en accounting (AAA), waardoor organisaties sterke verificatie en -beheer.

Web Application Firewall biedt de volgende voordelen:

-   Detecteert en blokkeert SQL-injecties, Cross-Site Scripting, malware-uploads, DDoS-toepassing of andere aanvallen tegen uw toepassing.

-   Verificatie en toegangsbeheer.

-   Uitgaand verkeer voor het detecteren van gevoelige informatie en kunt maskeren of blokkeren van de informatie uitlekken uit scant.

-   Versnelt de levering van inhoud voor web-toepassing, met behulp van mogelijkheden zoals caching, compressie en andere optimalisaties verkeer.

Zie voor meer voorbeelden van web application firewalls die beschikbaar in de Azure Marketplace zijn [Barracuda WAF, Brocade virtuele web application firewall (vWAF), Imperva SecureSphere en de ThreatSTOP IP-firewall](https://azuremarketplace.microsoft.com/marketplace/apps/barracudanetworks.waf).

## <a name="next-steps"></a>Volgende stappen

- [Detectiemogelijkheden van Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities): Actieve bedreigingen die gericht op uw Azure-resources en biedt de inzichten die u wilt snel reageren wordt geïdentificeerd.

- [Azure SQL Database Threat Detection](https://azure.microsoft.com/blog/azure-sql-database-threat-detection-your-built-in-security-expert/): Helpt oplossen uw opmerkingen over potentiële bedreigingen voor uw databases.

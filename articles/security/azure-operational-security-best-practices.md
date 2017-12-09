---
title: Azure operationeel aanbevolen beveiligingsprocedures | Microsoft Docs
description: Dit artikel bevat een set met aanbevolen procedures voor operationele Azure-beveiliging.
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: tomsh
ms.openlocfilehash: db9840817d92b4f2f63009d30dbd554666d53dbe
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="azure-operational-security-best-practices"></a>Aanbevolen procedures voor Azure operationele beveiliging
Azure bedrijfsbeveiliging verwijst naar de services, besturingselementen en functies die beschikbaar zijn voor gebruikers voor het beveiligen van hun gegevens, toepassingen en andere elementen in Microsoft Azure. Azure operationele beveiliging is gebaseerd op een framework dat de kennis die is opgedaan met verschillende mogelijkheden die uniek voor Microsoft zijn, waaronder Microsoft Security Development Lifecycle (SDL), de Microsoft Security Response Center-programma en grondige kennis van de threat cybersecurity liggend opgenomen.

In dit artikel bespreken we een verzameling aanbevolen beveiligingsprocedures voor Azure-database. Deze aanbevolen procedures zijn afgeleid van onze ervaring met beveiliging van de Azure-database en de ervaringen van klanten, zoals zelf.

Voor elke beste kunnen worden uitgelegd:
-   Wat de beste handelswijze is
-   Waarom u wilt dat het beste inschakelen
-   Wat zijn het resultaat als u niet de aanbevolen procedure inschakelen
- Hoe u kunt informatie over het inschakelen van de aanbevolen procedure

In dit artikel Azure operationele Best Practices voor beveiliging is gebaseerd op een advies consensus en mogelijkheden van Azure-platform en functiesets, als deze bestaan in de tijd die in dit artikel is geschreven. Adviezen en technologieën op den duur veranderen en dit artikel wordt regelmatig gecontroleerd op basis van deze wijzigingen worden bijgewerkt.

Azure operationeel aanbevolen beveiligingsprocedures besproken in dit artikel zijn onder andere:

-   Bewaken, beheren en cloudinfrastructuur beveiligen
-   Identiteiten beheren en implementeren van eenmalige aanmelding (SSO)
-   Trace-aanvragen, trends in gebruik analyseren en problemen identificeren
-   Bewaking van services met een oplossing voor gecentraliseerde controle
-   Detecteren, voorkomen van en reageren op bedreigingen
-   End-to-end scenario's gebaseerde netwerkbewaking
-   Veilig implementeren met de beproefde DevOps-hulpprogramma 's

## <a name="monitor-manage-and-protect-cloud-infrastructure"></a>Bewaken, beheren en cloudinfrastructuur beveiligen
IT Operations is verantwoordelijk voor het beheren van datacenter-infrastructuur, toepassingen en gegevens, inclusief de stabiliteit en beveiliging van deze systemen. Security insights krijgen over de vaak complexe IT-omgevingen verhogen vereist echter organisaties cobble samen gegevens uit meerdere systemen voor beveiliging en beheer.

[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) van Microsoft cloud-gebaseerde IT beheeroplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloud-infrastructuur.

[OMS beveiligings- en Audit oplossing](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources) kunnen IT-actief bewaken alle resources die kunnen helpen de impact van beveiligingsincidenten minimaliseren. OMS beveiligings- en Audit hebben security-domeinen die kunnen worden gebruikt voor het bewaken van de resources.

Lees het artikel [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx) voor meer informatie over OMS.

Om te detecteren, voorkomen van en reageren op bedreigingen, [Operations Management Suite (OMS) beveiligings- en Audit oplossing](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) verzamelt en verwerkt gegevens over uw resources, waaronder:

-   Beveiligingslogboek
-   ETW-gebeurtenissen (Event Tracing for Windows)
-   Controlegebeurtenissen AppLocker
-   Windows Firewall-logboek
-   Advanced Threat Analytics-gebeurtenissen
-   Resultaten van de evaluatie van de basislijn
-   Resultaten van de antimalware-evaluatie
-   Resultaten van de evaluatie van updates/patching
-   Syslog-stromen die expliciet zijn ingeschakeld op de agent


## <a name="manage-identity-and-implement-single-sign-on"></a>Identiteiten beheren en implementeren van eenmalige aanmelding
[Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) is Microsoft van meerdere tenants in de cloud-map en identity management-service.

[Azure AD](https://azure.microsoft.com/services/active-directory/) bevat ook een volledige reeks [identiteitsbeheer](https://docs.microsoft.com/azure/security/security-identity-management-overview) mogelijkheden, waaronder [multi-factorauthenticatie](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication), apparaatregistratie, selfservice voor wachtwoordherstel management, self-service groepsbeheer, bevoegd accountmanagement, op rollen gebaseerde toegangsbeheer, toepassingsgebruik bewaking, uitgebreide controle en beveiligingsbewaking en waarschuwingen.

De volgende mogelijkheden kunnen cloud-gebaseerde toepassingen, IT-processen te stroomlijnen, kosten knippen en ervoor te zorgen dat de doelstellingen van de naleving wordt voldaan:

-   Identiteits- en toegangsbeheer voor de cloud
-   Overal eenvoudiger gebruikerstoegang tot alle cloudtoepassingen
-   Vertrouwelijke gegevens en toepassingen beveiligen
-   Self-service voor uw werknemers mogelijk maken
-   Integreren met Azure Active Directory

### <a name="identity-and-access-management-for-the-cloud"></a>Identiteits- en toegangsbeheer voor de cloud
Azure Active Directory (Azure AD) is een uitgebreide [identiteits- en toegangsbeheer management-cloudoplossing](https://www.microsoft.com/cloud-platform/identity-management), hebt u een set krachtige mogelijkheden voor het beheer van gebruikers en groepen. Het helpt veilige toegang tot on-premises en cloudtoepassingen, met inbegrip van Microsoft web-services zoals Office 365 en veel niet-Microsoft-software als een dienst (SaaS)-toepassingen.
Zie voor meer informatie het inschakelen van identiteitsbeveiliging in Azure AD, [inschakelen van Azure Active Directory: Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-enable).

### <a name="simplify-user-access-to-any-cloud-app"></a>Overal eenvoudiger gebruikerstoegang tot alle cloudtoepassingen
[Eenmalige aanmelding inschakelen](https://docs.microsoft.com/azure/active-directory/active-directory-sso-integrate-saas-apps) om gebruikerstoegang te vereenvoudigen met duizenden cloud-toepassingen van Windows, Mac, Android en iOS-apparaten. Gebruikers kunnen toepassingen starten vanuit een persoonlijke webtoegang Configuratiescherm of mobiele app met zijn bedrijfsreferenties. Gebruik de module voor Azure AD-toepassingsproxy voorbij de SaaS-toepassingen en publiceren van on-premises webtoepassingen om maximaal veilige externe toegang en eenmalige aanmelding te bieden.

### <a name="protect-sensitive-data-and-applications"></a>Vertrouwelijke gegevens en toepassingen beveiligen
Schakel [Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) om te voorkomen dat onbevoegde toegang tot on-premises en cloudtoepassingen door te geven van een extra verificatieniveau. Bescherm uw bedrijf met beveiligingsbewaking, meldingen en rapporten op basis van Machine Learning die inconsistente toegangspatronen identificeren om zo potentiële bedreigingen te helpen voorkomen.

### <a name="enable-self-service-for-your-employees"></a>Self-service voor uw werknemers mogelijk maken
Delegeer belangrijke taken, zoals het opnieuw instellen van wachtwoorden en het maken en beheren van groepen, aan uw medewerkers. [Wijziging van de selfservice voor wachtwoordherstel inschakelen](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password)resetten en Self-service groepsbeheer met Azure AD.

### <a name="integrate-with-azure-active-directory"></a>Integreren met Azure Active Directory
Uitbreiden [Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-to-integrate) en andere on-premises mappen naar Azure AD voor eenmalige aanmelding voor alle cloud-gebaseerde toepassingen inschakelen. Gebruikerskenmerken kunnen automatisch worden gesynchroniseerd naar uw clouddirectory vanuit allerlei soorten on-premises directory's.

Lees het artikel voor meer informatie over de integratie van Azure Active Directory en hoe deze in te schakelen, [uw on-premises adreslijsten integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

## <a name="trace-requests-analyze-usage-trends-and-diagnose-issues"></a>Trace-aanvragen, trends in gebruik analyseren en problemen identificeren
[Azure Storage Analytics](https://docs.microsoft.com/azure/storage/storage-analytics) voert logboekregistratie en voorziet in metrische gegevens voor een opslagaccount. Deze gegevens kunt u gebruiken om aanvragen te traceren, gebruikstrends te analyseren en een diagnose uit te voeren voor problemen met uw opslagaccount.

Metrische gegevens Storage Analytics zijn standaard ingeschakeld voor nieuwe storage-accounts. U kunt logboekregistratie inschakelen en configureren van de metrische gegevens en logboekregistratie in de Azure portal; Zie voor meer informatie [een opslagaccount in de Azure-portal bewaken](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). U kunt ook Opslaganalyse programmatisch via de REST-API of de clientbibliotheek inschakelen. De bewerking Service-eigenschappen instellen voor het inschakelen van Storage Analytics afzonderlijk voor elke service gebruiken.

Zie voor een gedetailleerde uitleg over het gebruik van Storage Analytics en andere hulpprogramma's om te bepalen, onderzoeken en oplossen van problemen met Azure Storage met [Monitor, vaststellen en oplossen van Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/storage-monitoring-diagnosing-troubleshooting).

Lees het artikel voor meer informatie over de integratie van Azure Active Directory en hoe deze in te schakelen, [inschakelen en configureren van opslag Analytics](https://docs.microsoft.com/rest/api/storageservices/Enabling-and-Configuring-Storage-Analytics?redirectedfrom=MSDN).

## <a name="monitoring-services"></a>Bewaking van services
Cloud-toepassingen zijn complexe met veel bewegende onderdelen. Monitoring biedt gegevens om ervoor te zorgen dat uw toepassing up blijft en wordt uitgevoerd in een foutloze toestand bevindt. Ook kunt u potentiële problemen voorkomen of oplossen uit het verleden zijn. Bovendien kunt u bewakingsgegevens grondige om inzicht te krijgen over uw toepassing. Deze kennis kan u helpen bij het verbeteren van de prestaties van toepassingen of onderhoud of acties die anders worden handmatige interventie moeten automatiseren.

### <a name="monitor-azure-resources"></a>Azure-resources bewaken
[Monitor voor Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started) de platform-service die voorziet in één bron voor het bewaken van de Azure-resources. Met Azure Monitor kunt u visualiseren, query's uitvoeren, routeren, archiveren en actie ondernemen op basis van de metrische gegevens en logboeken van resources in Azure. U kunt werken met deze gegevens via de blade van de Monitor-portal, [Monitor PowerShell-cmdlets](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-powershell-samples), [platformoverschrijdende CLI](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-cli-samples) of [Azure Monitor REST API's](https://msdn.microsoft.com/library/dn931943.aspx).

### <a name="enable-autoscale-with-azure-monitor"></a>Automatisch schalen met monitor voor Azure inschakelen
Schakel [Azure Monitor automatisch schalen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-autoscale-get-started) geldt alleen voor virtuele-machineschaalsets (VMSS), cloudservices, app-serviceabonnementen en app service-omgevingen.

### <a name="manage-roles-permissions-and-security"></a>Rollen beheren machtigingen en beveiliging
Veel teams moeten strikt [toegang tot aan de bewaking](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-roles-permissions-security) gegevens en instellingen. Als er teamleden die werken alleen op de bewaking (ondersteuningsmedewerkers, devops engineers) of als u een provider van beheerde services gebruikt, u kunt ze om toegang te verlenen tot alleen bewakingsgegevens terwijl de beperkingen voor de mogelijkheid om te maken, wijzigen of verwijderen van resources.

Dit laat zien hoe snel een ingebouwde bewaking RBAC-rol toepassen op een gebruiker in Azure of uw eigen aangepaste rol voor een gebruiker aan wie beperkte machtigingen voor controle moet bouwen. Vervolgens wordt de beveiligingsoverwegingen voor uw Azure-Monitor-gerelateerde resources en hoe u toegang tot de gegevens die ze bevatten alleen besproken.

## <a name="prevent-detect-and-respond-to-threats"></a>Detecteren, voorkomen van en reageren op bedreigingen
Detectie van dreigingen Security Center werkt door het automatisch verzamelen van gegevens van de beveiliging van Azure-resources, het netwerk en partneroplossingen verbonden. Deze analyseert deze gegevens vaak correleren van gegevens uit meerdere bronnen, bedreigingen identificeren. Beveiligingswaarschuwingen krijgen in Security Center een prioriteit, evenals aanbevelingen voor het oplossen van de bedreiging.

-   [Een beveiligingsbeleid configureren](https://docs.microsoft.com/azure/security-center/security-center-policies) voor uw Azure-abonnement.
-   Gebruik de [aanbevelingen in Security Center](https://docs.microsoft.com/azure/security-center/security-center-recommendations) voor hulp bij het beveiligen van uw Azure-resources.
-   Bekijken en beheren van uw huidige [beveiligingswaarschuwingen](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).

[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) helpt u bij het detecteren, voorkomen van en reageren op bedreigingen dankzij een verhoogde zichtbaarheid van en controle over de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen te detecteren die anders onopgemerkt zouden blijven, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

Security Center biedt eenvoudige en effectieve preventie en detectie van bedreigingen en reactiemogelijkheden die zijn ingebouwd in Azure. De belangrijkste mogelijkheden zijn:

-   Beveiligingsstatus van de cloud begrijpen
-   Neem de controle over cloudbeveiliging
-   Eenvoudig geïntegreerde cloudbeveiligingsoplossingen implementeren
-   Snel bedreigingen detecteren en erop reageren

### <a name="understand-cloud-security-state"></a>Beveiligingsstatus van de cloud begrijpen
Gebruik het Azure Security Center voor een centraal overzicht van de beveiligingsstatus van al uw Azure-resources. Controleer of de juiste beveiligingscontroles zijn geïnstalleerd en correct geconfigureerd in een oogopslag en snel identificeren alle resources die aandacht vereisen.

### <a name="take-control-of-cloud-security"></a>Neem de controle over cloudbeveiliging
Definieer [beveiligingsbeleid](https://docs.microsoft.com/azure/security-center/security-center-policies) voor uw Azure-abonnementen volgens cloud security-behoeften van uw bedrijf, die is afgestemd op het type toepassingen of de vertrouwelijkheid van de gegevens in elk abonnement. Gebruik op beleid gebaseerde aanbevelingen om eigenaren van resources op weg te helpen met het implementeren van de vereiste controlesystemen, waarmee u nattevingerwerk binnen cloudbeveiliging voorkomt.

### <a name="easily-deploy-integrated-cloud-security-solutions"></a>Eenvoudig geïntegreerde cloudbeveiligingsoplossingen implementeren
[Inschakelen van beveiligingsoplossingen](https://docs.microsoft.com/azure/security-center/security-center-partner-integration) van Microsoft en haar partners, met inbegrip van toonaangevende firewalls en anti-malware. Maak gebruik van gestroomlijnd inrichten om beveiligingsoplossingen te implementeren; zelfs netwerkveranderingen worden voor u geconfigureerd. De beveiligingsgebeurtenissen van partneroplossingen worden automatisch verzameld voor analyse en waarschuwingsdoeleinden.

### <a name="detect-threats-and-respond-fast"></a>Snel bedreigingen detecteren en erop reageren
Blijf actuele en toekomstige cloudbedreigingen voor met een geïntegreerde, op analyses gebaseerde aanpak. Door een combinatie van Microsoft globale [dreiging intelligence](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities) en expertise met inzichten in cloud beveiligingsgebeurtenissen in uw Azure-implementaties, Security Center helpt u vroeg werkelijke bedreigingen detecteren en fout-positieven te verminderen. Beveiligingswaarschuwingen cloud bieden u inzicht in de campagne aanval, met inbegrip van gerelateerde gebeurtenissen en betrokken resources en stellen manieren voor het oplossen van problemen en snel herstellen.

## <a name="end-to-end-scenario-based-network-monitoring"></a>End-to-end scenario's gebaseerde netwerkbewaking
Een end-to-end-netwerk in Azure opbouwen klanten door te organiseren en samenstellen van verschillende afzonderlijke netwerkbronnen zoals VNet, ExpressRoute, Application Gateway, Load balancers en meer. Bewaking is beschikbaar op elk van de netwerkbronnen.

[Netwerk-Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) is een regionale service waarmee u kunt bewaken en onderzoeken van de voorwaarden op een netwerk scenario niveau, en naar Azure. Netwerkcontrole en visualisatie hulpprogramma's beschikbaar met de netwerk-Watcher kunnen u begrijpen, diagnoses stellen en Verkrijg inzicht in uw netwerk in Azure.

### <a name="automate-remote-network-monitoring-with-packet-capture"></a>Externe netwerkbewaking automatiseren met packetopname
Met Network Watcher bewaakt u uw netwerk en stelt u problemen vast, zonder u aan te hoeven melden op uw virtuele machines. Trigger [pakketopname](https://docs.microsoft.com/azure/network-watcher/network-watcher-alert-triggered-packet-capture) door waarschuwingen instellen en toegang krijgen tot informatie over realtime prestaties op het pakketniveau van het. Wanneer er een probleem wordt vastgesteld, kunt u dat uitgebreid onderzoeken voor een gedetailleerde diagnose.

### <a name="gain-insight-into-your-network-traffic-using-flow-logs"></a>Inzicht in uw netwerkverkeer met stroomlogboeken
Maken van een beter begrip van uw netwerkverkeer patroon met [Netwerkbeveiligingsgroep stroom logboeken](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview). Informatie die door de stroom logboeken helpt u gegevens op naleving controleren en bewaken van uw netwerk beveiligingsprofiel verzamelen.

### <a name="diagnose-vpn-connectivity-issues"></a>VPN-verbindingsproblemen vaststellen
Netwerk-Watcher biedt u de mogelijkheid om [vaststellen van de meest voorkomende problemen met VPN-Gateway en verbindingen](https://docs.microsoft.com/azure/network-watcher/network-watcher-diagnose-on-premises-connectivity). Zodat u niet alleen voor het vaststellen van het probleem, maar ook de gedetailleerde logboeken gemaakt om verder onderzoek te gebruiken.

Lees het artikel voor meer informatie over het configureren van netwerk-watcher en hoe deze in te schakelen, [configureren van netwerk-watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="secure-deployment-using-proven-devops-tools"></a>Veilig implementeren met de beproefde DevOps-hulpprogramma 's
Dit zijn enkele van de lijst van het Azure DevOps procedures in deze ruimte Microsoft Cloud, waardoor de bedrijven en teams productief en efficiënt.

-   **Infrastructuur als de Code (IaC):** infrastructuur als de Code is een verzameling technieken en procedures waarmee IT-professionals de werkbelasting die is gekoppeld aan de dagelijkse build en het beheer van modulaire infrastructuur verwijderen. Hiermee kunt de IT-professionals bouwen en onderhouden van hun serveromgeving moderne op een manier die lijkt op hoe Softwareontwikkelaars bouwen en onderhouden van toepassingscode. Voor Azure, hebben we [Azure Resource Manager]( https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/) kunt u uw toepassingen met behulp van een declaratief sjabloon inrichten. U kunt in één enkele sjabloon meerdere services plus de bijbehorende afhankelijkheden implementeren. U gebruikt dezelfde sjabloon om uw toepassing herhaaldelijk te implementeren in elke fase van de levenscyclus van de toepassing.
-   **Continue integratie en implementatie:** kunt u uw Visual Studio Online teamprojecten naar [automatisch bouwen en implementeren](https://www.visualstudio.com/docs/build/overview) naar Azure-web-apps of cloudservices. De binaire bestanden implementeert VSO automatisch hierna moet u een build naar Azure na elke code incheckt. Het pakket buildproces hier beschreven komt overeen met de opdracht pakket in Visual Studio en de publicatie stappen gelijk zijn aan de opdracht Publish in Visual Studio.
-   **Releasebeheer:** Visual Studio [Release Management](https://msdn.microsoft.com/library/vs/alm/release/overview) is een ideale oplossing voor de implementatie van meerdere fasen automatiseren en beheren van het proces release. Beheerde continue implementatie pijplijnen snel, eenvoudig en vaak vrij te maken. Met de Release Management we onze release-proces veel kunt automatiseren en we kunnen beschikken over vooraf gedefinieerde werkstromen voor goedkeuring. On-premises implementeren naar de cloud uitbreiden en aanpassen zoals vereist.
-   **Bewaking van App-prestaties:** problemen detecteren, oplossen van problemen en breng voortdurend verbeteringen aan uw toepassingen. Analyseer snel problemen in uw live-toepassing. Krijg inzicht in wat gebruikers met uw toepassing doen. Configuratie is eenvoudig kwestie van JS code en een webconfig-vermelding toe te voegen en ziet u resultaten binnen enkele minuten in de portal met de details. [App insights](https://azure.microsoft.com/documentation/articles/app-insights-start-monitoring-app-health-usage/) helpt bedrijven voor een snellere detectie van problemen en herstel.
-   **Laden getest & automatisch schalen:** We prestatieproblemen kunt vinden in de app om de kwaliteit implementatie te verbeteren en om ervoor te zorgen dat de app is altijd up-to-date of beschikbaar om te voorzien in de zakelijke behoeften. Zorg ervoor dat uw app voor uw volgende starten of marketing campagne verkeer kan verwerken. Cloud-gebaseerde uitgevoerd [tests laden](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing) in bijna geen tijd met Visual Studio Online.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Azure bedrijfsbeveiliging](https://docs.microsoft.com/azure/security/azure-operational-security).
- Voor meer informatie [Operations Management Suite | Beveiliging en naleving](https://www.microsoft.com/cloud-platform/security-and-compliance).
- [Aan de slag met beveiliging van Operations Management Suite-oplossing van de Audit en](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started).

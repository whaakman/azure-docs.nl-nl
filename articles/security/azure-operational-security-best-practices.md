---
title: Azure best practices voor operationele beveiliging | Microsoft Docs
description: Dit artikel bevat een set van aanbevolen procedures voor operationele beveiliging van Azure.
services: security
documentationcenter: na
author: unifycloud
manager: mbaldwin
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: tomsh
ms.openlocfilehash: d8f6ad48c62ff2021c91e593cee44dd6f5551247
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297905"
---
# <a name="azure-operational-security-best-practices"></a>Azure best practices voor operationele beveiliging
Azure operationele beveiliging verwijst naar de services, besturingselementen en functies die beschikbaar zijn voor gebruikers voor het beveiligen van hun gegevens, toepassingen en andere items in Microsoft Azure. Operationele beveiliging in Azure is gebouwd op een framework waarin de kennis opgedaan uit verschillende mogelijkheden die uniek voor Microsoft zijn, met inbegrip van de Microsoft Security Development Lifecycle (SDL), het programma Microsoft Security Response Center en een diep besef van het landschap van cyberveiligheidsbedreigingen.

In dit artikel wordt besproken hoe een verzameling van aanbevolen beveiligingsprocedures voor Azure-database. Deze aanbevolen procedures zijn afgeleid van onze ervaring met beveiliging van de Azure database en de ervaringen van klanten zoals zelf.

Voor elke best practice wordt we het volgende uitgelegd:
-   Wat de beste manier is
-   Waarom u deze wilt inschakelen, die best practices
-   Wat kan het resultaat zijn als u een failover naar de aanbevolen procedure inschakelen
- Hoe u meer informatie kunt krijgen om in te schakelen van de aanbevolen procedure

In dit artikel Azure operationele Best Practices voor beveiliging is gebaseerd op een advies consensus en mogelijkheden van Azure-platform en functiesets, zoals ze bestaan op het moment dat dit artikel is geschreven. Adviezen en -technologieën na verloop van tijd worden gewijzigd en regelmatig op basis van deze wijzigingen in dit artikel wordt bijgewerkt.

Aanbevolen procedures voor Azure operationele beveiliging die in dit artikel worden besproken omvatten:

-   Controleren, beheren en cloudinfrastructuur beveiligen
-   Identiteit beheren en implementeren van eenmalige aanmelding (SSO)
-   Aanvragen traceren, gebruikstrends te analyseren en problemen diagnosticeren
-   Bewaken van services met een gecentraliseerde oplossing voor bewaking
-   Voorkomen, detecteren en direct reageren op bedreigingen
-   End-to-end scenario's gebaseerde netwerkbewaking
-   Implementatie met behulp van bewezen DevOps-hulpprogramma's beveiligen

## <a name="monitor-manage-and-protect-cloud-infrastructure"></a>Controleren, beheren en cloudinfrastructuur beveiligen
IT Operations is verantwoordelijk voor het beheren van datacenter-infrastructuur, toepassingen en gegevens, inclusief de stabiliteit en beveiliging van deze systemen. Verkrijgen van beveiligingsinzichten op in complexe IT-omgevingen vaak verhogen vereist echter organisaties cobble samen gegevens van meerdere systemen voor beveiliging en beheer.

[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) is van Microsoft cloud-gebaseerde IT-beheeroplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloudinfrastructuur.

[Oplossing OMS beveiliging en controle](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources) Hierdoor kunnen IT-actief Controleer alle resources, die kunnen helpen het effect van beveiligingsincidenten minimaliseren. OMS beveiliging en controle hebt beveiligingsdomeinen die kunnen worden gebruikt voor het bewaken van resources.

Lees het artikel [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx) voor meer informatie over OMS.

Om te voorkomen, detecteren en direct reageren op bedreigingen, [Operations Management Suite (OMS) beveiligings- en controleoplossing](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) verzamelt en verwerkt gegevens over uw resources, waaronder:

-   Beveiligingslogboek
-   ETW-gebeurtenissen (Event Tracing for Windows)
-   Controlegebeurtenissen AppLocker
-   Windows Firewall-logboek
-   Advanced Threat Analytics-gebeurtenissen
-   Resultaten van de evaluatie van de basislijn
-   Resultaten van de antimalware-evaluatie
-   Resultaten van de evaluatie van updates/patching
-   Syslog-streams die expliciet zijn ingeschakeld op de agent


## <a name="manage-identity-and-implement-single-sign-on"></a>Identiteit beheren en implementeren van eenmalige aanmelding
[Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) is van Microsoft met meerdere tenants cloudgebaseerde directory en identity management-service.

[Azure AD](https://azure.microsoft.com/services/active-directory/) bevat ook een volledige reeks [identiteitsbeheer](https://docs.microsoft.com/azure/security/security-identity-management-overview) mogelijkheden, waaronder [multi-factor authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication), apparaatregistratie, eigen wachtwoordbeheer, Selfservice groepsbeheer, bevoegd accountmanagement, op rollen gebaseerd toegangsbeheer, bewaking, uitgebreide controle en beveiliging voor bewaking en waarschuwingen gebruik van de toepassing.

De volgende mogelijkheden kunnen helpen veilige cloud-gebaseerde toepassingen, IT-processen te stroomlijnen, kosten en ervoor te zorgen dat zakelijke nalevingsvereisten wordt voldaan:

-   Identiteits- en toegangsbeheer voor de cloud
-   Overal eenvoudiger gebruikerstoegang tot alle cloudtoepassingen
-   Vertrouwelijke gegevens en toepassingen beveiligen
-   Self-service voor uw werknemers mogelijk maken
-   Integreren met Azure Active Directory

### <a name="identity-and-access-management-for-the-cloud"></a>Identiteits- en toegangsbeheer voor de cloud
Azure Active Directory (Azure AD) is een complete [cloudoplossing voor identiteits- en toegangsbeheer](https://www.microsoft.com/cloud-platform/identity-management), waarmee u een robuuste set mogelijkheden voor het beheren van gebruikers en groepen. Het helpt de toegang tot on-premises en cloudtoepassingen, met inbegrip van Microsoft-webservices, zoals Office 365 en nog veel niet-Microsoft-software als een service (SaaS)-toepassingen.
Zie voor meer informatie over het inschakelen van de identiteitsbeveiliging in Azure AD, [inschakelen van Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-enable).

### <a name="simplify-user-access-to-any-cloud-app"></a>Overal eenvoudiger gebruikerstoegang tot alle cloudtoepassingen
[Eenmalige aanmelding inschakelen](https://docs.microsoft.com/azure/active-directory/active-directory-sso-integrate-saas-apps) voor het vereenvoudigen van gebruikerstoegang tot duizenden cloudtoepassingen van Windows, Mac-, Android en iOS-apparaten. Gebruikers kunnen een aangepaste web gebaseerde Toegangsvenster of mobiele app met hun bedrijfsreferenties toepassingen starten. Gebruik de module Azure AD-toepassingsproxy verder dan SaaS-toepassingen en publiceren van on-premises webtoepassingen om uitermate veilige externe toegang en eenmalige aanmelding te bieden.

### <a name="protect-sensitive-data-and-applications"></a>Vertrouwelijke gegevens en toepassingen beveiligen
Schakel [Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) te voorkomen dat onbevoegde toegang tot on-premises en cloudtoepassingen beveiligt een extra verificatieniveau. Bescherm uw bedrijf met beveiligingsbewaking, meldingen en rapporten op basis van Machine Learning die inconsistente toegangspatronen identificeren om zo potentiële bedreigingen te helpen voorkomen.

### <a name="enable-self-service-for-your-employees"></a>Self-service voor uw werknemers mogelijk maken
Delegeer belangrijke taken, zoals het opnieuw instellen van wachtwoorden en het maken en beheren van groepen, aan uw medewerkers. [Inschakelen van self-service voor wachtwoord wijzigen](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password)opnieuw instellen en selfservice groepsbeheer met Azure AD.

### <a name="integrate-with-azure-active-directory"></a>Integreren met Azure Active Directory
Uitbreiden [Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-to-integrate) en andere on-premises directory's uit naar Azure AD om eenmalige aanmelding voor alle cloud-gebaseerde toepassingen. Gebruikerskenmerken kunnen automatisch worden gesynchroniseerd naar uw clouddirectory vanuit allerlei soorten on-premises directory's.

Lees het artikel voor meer informatie over de integratie van Azure Active Directory en het inschakelen ervan [uw on-premises directory's integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

## <a name="trace-requests-analyze-usage-trends-and-diagnose-issues"></a>Aanvragen traceren, gebruikstrends te analyseren en problemen diagnosticeren
[Azure Storage Analytics](https://docs.microsoft.com/azure/storage/storage-analytics) logboekregistratie uitvoert en metrische gegevens biedt voor een opslagaccount. Deze gegevens kunt u gebruiken om aanvragen te traceren, gebruikstrends te analyseren en een diagnose uit te voeren voor problemen met uw opslagaccount.

Storage Analytics metrics zijn standaard ingeschakeld voor nieuwe storage-accounts. U kunt logboekregistratie inschakelen en configureren van metrische gegevens en logboekregistratie in de Azure-portal; Zie voor meer informatie, [een opslagaccount in Azure portal controleren](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). U kunt ook Storage Analytics via een programma via de REST-API of de clientbibliotheek inschakelen. Gebruik de bewerking van de Service-eigenschappen instellen voor het inschakelen van Storage Analytics afzonderlijk voor elke service.

Zie voor een uitgebreide handleiding over het gebruik van Storage Analytics en andere hulpprogramma's om te identificeren, onderzoeken en oplossen van problemen met betrekking tot Azure Storage, [bewaken, problemen vaststellen en oplossen van Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/storage-monitoring-diagnosing-troubleshooting).

Lees het artikel voor meer informatie over de integratie van Azure Active Directory en het inschakelen ervan [inschakelen en configureren van Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/Enabling-and-Configuring-Storage-Analytics?redirectedfrom=MSDN).

## <a name="monitoring-services"></a>Bewaken van services
Cloud-Apps zijn complexe met veel bewegende onderdelen bevatten. Monitoring biedt gegevens om ervoor te zorgen dat uw toepassing actief en wordt uitgevoerd in een foutloze toestand bevindt. Ook kunt u potentiële problemen voorkomen of oplossen van het verleden zijn. Bovendien kunt u bewakingsgegevens diep om inzicht te krijgen over uw toepassing. Deze kennis kan u helpen te verbeteren van de prestaties van de toepassing of onderhoud, of Automatiseer acties die anders handmatig worden opgelost moeten zouden.

### <a name="monitor-azure-resources"></a>Azure-resources controleren
[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started) is de platformservice die één bron biedt voor het bewaken van Azure-resources. Met Azure Monitor kunt u visualiseren, query's uitvoeren, routeren, archiveren en actie ondernemen op basis van de metrische gegevens en logboeken van resources in Azure. U kunt werken met deze gegevens via de blade van de Monitor-portal, [Monitor PowerShell-cmdlets](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-powershell-samples), [platformoverschrijdende CLI](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-cli-samples) of [Azure Monitor REST API's](https://msdn.microsoft.com/library/dn931943.aspx).

### <a name="enable-autoscale-with-azure-monitor"></a>Automatisch schalen met Azure monitor inschakelen
Schakel [voor automatisch schalen van Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-autoscale-get-started) geldt alleen voor virtuele-machineschaalsets (VMSS), cloudservices, app service-abonnementen en app service-omgevingen.

### <a name="manage-roles-permissions-and-security"></a>Rollen beheren machtigingen en beveiliging
Veel teams moeten strikt [regelen de toegang tot de bewaking](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-roles-permissions-security) gegevens en instellingen. Bijvoorbeeld, als u de teamleden die uitsluitend over het bewaken van (ondersteuningstechnici, devops-technici) werken hebt of als u een provider van beheerde services gebruikt, kunt u ze om toegang te verlenen tot alleen bewakingsgegevens tijdens het beperken van de mogelijkheid om te maken, wijzigen, of resources verwijderen.

Dit laat zien hoe snel een ingebouwde bewaking RBAC-rol van toepassing op een gebruiker in Azure of bouw uw eigen aangepaste rol voor een gebruiker bent en beperkte machtigingen voor bewaking. Hierin worden vervolgens beveiligingsoverwegingen voor uw resources met betrekking tot Azure Monitor en het beperken van toegang tot de gegevens die ze bevatten.

## <a name="prevent-detect-and-respond-to-threats"></a>Voorkomen, detecteren en direct reageren op bedreigingen
Detectie van bedreigingen van Security Center werkt volgens het automatisch verzamelen van gegevens van de beveiliging van Azure-resources, het netwerk en verbonden partneroplossingen. Deze analyseert deze gegevens, vaak correleren van gegevens uit meerdere bronnen, om te identificeren van bedreigingen. Beveiligingswaarschuwingen krijgen in Security Center een prioriteit, evenals aanbevelingen voor het oplossen van de bedreiging.

-   [Een beveiligingsbeleid configureren](https://docs.microsoft.com/azure/security-center/security-center-policies) voor uw Azure-abonnement.
-   Gebruik de [aanbevelingen in Security Center](https://docs.microsoft.com/azure/security-center/security-center-recommendations) om u te helpen beschermen van uw Azure-resources.
-   Bekijken en beheren van uw huidige [beveiligingswaarschuwingen](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).

[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) helpt u bij het detecteren, voorkomen van en reageren op bedreigingen dankzij een verhoogde zichtbaarheid van en controle over de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen te detecteren die anders onopgemerkt zouden blijven, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

Security Center biedt eenvoudige en effectieve preventie en detectie van bedreigingen en reactiemogelijkheden die zijn ingebouwd in Azure. De belangrijkste mogelijkheden zijn:

-   Krijg inzicht in cloudbeveiligingsstatus
-   Neem de controle over cloudbeveiliging
-   Eenvoudig geïntegreerde cloudbeveiligingsoplossingen implementeren
-   Snel bedreigingen detecteren en erop reageren

### <a name="understand-cloud-security-state"></a>Krijg inzicht in cloudbeveiligingsstatus
Gebruik het Azure Security Center voor een centraal overzicht van de beveiligingsstatus van al uw Azure-resources. Controleer of de juiste beveiligingscontroles zijn geïnstalleerd en correct geconfigureerd in een oogopslag en snel zien of er resources, die uw aandacht vereisen.

### <a name="take-control-of-cloud-security"></a>Neem de controle over cloudbeveiliging
Definieer [beveiligingsbeleid](https://docs.microsoft.com/azure/security-center/security-center-policies) voor uw Azure-abonnementen op basis van de behoeften van uw bedrijf cloud, die zijn afgestemd op het type toepassingen of de gevoeligheid van de gegevens in elk abonnement. Gebruik op beleid gebaseerde aanbevelingen om eigenaren van resources op weg te helpen met het implementeren van de vereiste controlesystemen, waarmee u nattevingerwerk binnen cloudbeveiliging voorkomt.

### <a name="easily-deploy-integrated-cloud-security-solutions"></a>Eenvoudig geïntegreerde cloudbeveiligingsoplossingen implementeren
[Inschakelen van beveiligingsoplossingen](https://docs.microsoft.com/azure/security-center/security-center-partner-integration) van Microsoft en haar partners, inclusief toonaangevende firewalls en antimalware. Maak gebruik van gestroomlijnd inrichten om beveiligingsoplossingen te implementeren; zelfs netwerkveranderingen worden voor u geconfigureerd. De beveiligingsgebeurtenissen van partneroplossingen worden automatisch verzameld voor analyse en waarschuwingsdoeleinden.

### <a name="detect-threats-and-respond-fast"></a>Snel bedreigingen detecteren en erop reageren
Blijf actuele en toekomstige cloudbedreigingen voor met een geïntegreerde, op analyses gebaseerde aanpak. Door een combinatie van Microsoft global [bedreigingsinformatie](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities) en expertise met inzichten in cloud-veiligheidsgerelateerde gebeurtenissen binnen uw Azure-implementaties, Security Center helpt u vroeg actuele bedreigingen detecteren en fout-positieven. Door cloudbeveiligingswaarschuwingen krijgt u inzicht in de aanvalsactie, inclusief gerelateerde gebeurtenissen en getroffen resources, en worden manieren voor probleemoplossing en snel herstel voorgesteld.

## <a name="end-to-end-scenario-based-network-monitoring"></a>End-to-end scenario's gebaseerde netwerkbewaking
Klanten bouwen een end-to-end-netwerk in Azure door te organiseren en het samenstellen van diverse afzonderlijke netwerkbronnen zoals VNet, ExpressRoute, Application Gateway, Load balancers en meer. Bewaking is beschikbaar op elk van de netwerkbronnen.

[Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) is een regionale service waarmee u kunt bewaken en diagnosticeren op een scenario voor het netwerk niveau in, en naar Azure. Diagnose en visualisatie hulpprogramma's die beschikbaar zijn met Network Watcher kunnen u begrijpen, diagnosticeren en inzicht verkrijgen in uw netwerk in Azure.

### <a name="automate-remote-network-monitoring-with-packet-capture"></a>Externe netwerkbewaking automatiseren met packetopname
Met Network Watcher bewaakt u uw netwerk en stelt u problemen vast, zonder u aan te hoeven melden op uw virtuele machines. Trigger [pakketopname](https://docs.microsoft.com/azure/network-watcher/network-watcher-alert-triggered-packet-capture) door waarschuwingen instellen en toegang krijgen tot informatie over realtime prestaties op packetniveau. Wanneer er een probleem wordt vastgesteld, kunt u dat uitgebreid onderzoeken voor een gedetailleerde diagnose.

### <a name="gain-insight-into-your-network-traffic-using-flow-logs"></a>Inzicht in uw netwerkverkeer met stroomlogboeken
Bouw een gedetailleerd beeld van uw netwerkverkeer patroon met [stroomlogboeken van Netwerkbeveiligingsgroep](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview). Informatie geleverd door de stroomlogboeken helpt u gegevens verzamelen voor naleving, controleren en bewaken van het beveiligingsprofiel van uw netwerk.

### <a name="diagnose-vpn-connectivity-issues"></a>VPN-verbindingsproblemen vaststellen
Network Watcher biedt u de mogelijkheid om [vaststellen van de meest voorkomende problemen met VPN-Gateway en verbindingen](https://docs.microsoft.com/azure/network-watcher/network-watcher-diagnose-on-premises-connectivity). Zodat u kunt niet alleen bepalen wat het probleem, maar ook gebruik van gedetailleerde logboeken om te helpen verder kunnen onderzoeken.

Lees het artikel voor meer informatie over het configureren van Network watcher en het inschakelen ervan, [Network watcher configureren](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="secure-deployment-using-proven-devops-tools"></a>Implementatie met behulp van bewezen DevOps-hulpprogramma's beveiligen
Dit zijn enkele van de lijst van Azure DevOps-procedures in deze ruimte Microsoft Cloud, waardoor bedrijven en teams productief en efficiënt.

-   **Infrastructuur als Code (IaC):** Infrastructure as Code is een reeks technieken en procedures waarmee IT-professionals de werkbelasting die is gekoppeld aan de dagelijkse build en het beheer van modulaire infrastructuur verwijderen. Hiermee kunnen IT-professionals om te bouwen en onderhouden van hun moderne serveromgeving op een manier die lijkt op hoe software-ontwikkelaars ontwikkelen en onderhouden van de toepassingscode. Voor Azure, hebben we [Azure Resource Manager]( https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/) kunt u voor het inrichten van uw toepassingen met behulp van een declaratieve sjabloon. U kunt in één enkele sjabloon meerdere services plus de bijbehorende afhankelijkheden implementeren. U gebruikt dezelfde sjabloon om uw toepassing herhaaldelijk te implementeren in elke fase van de levenscyclus van de toepassing.
-   **Continue integratie en implementatie:** kunt u uw Azure DevOps-projecten te configureren [automatisch bouwen en implementeren van](https://www.visualstudio.com/docs/build/overview) op Azure-web-apps of cloudservices. Azure DevOps implementeert automatisch de binaire bestanden hierna een build naar Azure na elke check code in. De build-proces van het pakket hier beschreven komt overeen met de opdracht pakket in Visual Studio en de publicatie stappen zijn gelijk aan de opdracht Publish in Visual Studio.
-   **Release Management:** Visual Studio [Release Management](https://msdn.microsoft.com/library/vs/alm/release/overview) is een uitstekende oplossing voor implementatie van meerdere fasen automatiseren en beheren van de release-proces. Beheerde continue implementatie pijplijnen om snel, eenvoudig en vaak vrij te maken. Met Release Management we onze uitgifteproces veel kunt automatiseren en we kunnen beschikken over vooraf gedefinieerde goedkeuringswerkstromen. Implementeer on-premises en naar de cloud uitbreiden en aanpassen zoals vereist.
-   **Bewaking van App-prestaties:** problemen detecteren, problemen oplossen en uw toepassingen continu te verbeteren. Analyseer snel problemen in uw live-toepassing. Krijg inzicht in wat gebruikers met uw toepassing doen. Configuratie is eenvoudig amper JS-code en een webconfig-vermelding toe te voegen en u resultaten worden binnen minuten in de portal voor alle details weergegeven. [App insights](https://azure.microsoft.com/documentation/articles/app-insights-start-monitoring-app-health-usage/) helpt bedrijven voor snellere detectie van problemen en herstel.
-   **Load Testing & voor automatisch schalen:** prestatieproblemen te vinden in onze app om implementatie kwaliteit te verbeteren en om ervoor te zorgen dat onze app is altijd omhoog of beschikbaar die geschikt zijn voor het bedrijf nodig heeft. Zorg ervoor dat uw app voor uw volgende starten of marketing campagne verkeer kan verwerken. Beginnen met het uitvoeren van cloud-gebaseerde [belastingstests](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing) in vrijwel geen tijd met Azure DevOps.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Azure operationele beveiliging](https://docs.microsoft.com/azure/security/azure-operational-security).
- Voor meer informatie [Operations Management Suite | Security & Compliance](https://www.microsoft.com/cloud-platform/security-and-compliance).
- [Aan de slag met Operations Management Suite Security- en controleoplossing](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started).

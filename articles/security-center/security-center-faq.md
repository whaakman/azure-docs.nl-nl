---
title: Azure Security Center Veelgestelde vragen (FAQ) | Microsoft Docs
description: Deze Veelgestelde vragen vindt u antwoorden op vragen over Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2018
ms.author: rkarlin
ms.openlocfilehash: 382f85c268b2e21a780756057f4bf78c41c791c2
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39283500"
---
# <a name="azure-security-center-frequently-asked-questions-faq"></a>Veelgestelde vragen over Azure Security Center
Deze Veelgestelde vragen vindt u antwoorden op vragen over Azure Security Center, een service die u bij het voorkomen helpt, detecteren en direct reageren op bedreigingen met verbeterde zichtbaarheid en controle over de beveiliging van uw Microsoft Azure-resources.

> [!NOTE]
> Vanaf begin juni 2017 zal Security Center de Microsoft Monitoring Agent gebruiken voor het verzamelen en opslaan van gegevens. Zie voor meer informatie, [migratie van Azure Security Center-Platform](security-center-platform-migration.md). De informatie in dit artikel beschrijft functionaliteit van Security Center na de overstap naar de Microsoft Monitoring Agent.
>
>

## <a name="general-questions"></a>Algemene vragen
### <a name="what-is-azure-security-center"></a>Wat is Azure Security Center?
Azure Security Center helpt u bij het detecteren, voorkomen van en reageren op bedreigingen dankzij een verhoogde zichtbaarheid van en controle over de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw abonnementen, helpt bedreigingen te detecteren die anders onopgemerkt zouden blijven, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

### <a name="how-do-i-get-azure-security-center"></a>Hoe krijg ik Azure Security Center?
Azure Security Center is ingeschakeld met uw Microsoft Azure-abonnement en is toegankelijk vanuit de [Azure-portal](https://azure.microsoft.com/features/azure-portal/). ([Aanmelden bij de portal](https://portal.azure.com), selecteer **Bladeren**, en schuif naar **Security Center**).  

## <a name="billing"></a>Billing
### <a name="how-does-billing-work-for-azure-security-center"></a>Hoe werkt facturering voor Azure Security Center?
Security Center wordt aangeboden in twee lagen:

De **gratis laag** biedt meer inzicht in de beveiligingsstatus van uw Azure-resources, beleid van de basisvereisten voor beveiliging, aanbevelingen voor beveiliging en integratie met beveiligingsproducten en -diensten van partners.

De **Standard-laag** geavanceerde threat detectiemogelijkheden, waaronder threat intelligence, gedragsanalyse, anomaliedetectie, beveiligingsincidenten en attribution rapporten van bedreigingen wordt toegevoegd. De prijscategorie Standard is gedurende de eerste 60 dagen gratis. Moet u ervoor kiest om door te gaan om de service langer dan 60 dagen te gebruiken, wordt automatisch gestart kosten in rekening gebracht voor de service.  Als u wilt bijwerken, selecteert u [prijscategorie](https://docs.microsoft.com/azure/security-center/security-center-pricing) in het beveiligingsbeleid.

## <a name="permissions"></a>Machtigingen
Azure Security Center maakt gebruik van [Role-Based Access Control (RBAC)](../role-based-access-control/role-assignments-portal.md), waarmee u [ingebouwde rollen](../role-based-access-control/built-in-roles.md) die kunnen worden toegewezen aan gebruikers, groepen en services in Azure.

Security Center beoordeelt de configuratie van uw resources om beveiligingsproblemen met zich mee en beveiligingsproblemen te identificeren. In Security Center ziet u alleen informatie met betrekking tot een bron wanneer u de rol van eigenaar, bijdrager of lezer voor het abonnement of de resourcegroep die deel uitmaakt van een resource om te worden toegewezen.

Zie [machtigingen in Azure Security Center](security-center-permissions.md) voor meer informatie over de functies en toegestane acties in Security Center.

## <a name="data-collection"></a>Gegevensverzameling
Security Center verzamelt gegevens van uw virtuele Azure-machines (VM's) en niet-Azure-computers om te controleren op beveiligingsproblemen en bedreigingen. De gegevens worden verzameld met behulp van de MMA, die verschillende configuraties en gebeurtenislogboeken met betrekking tot beveiliging van de machine leest en de gegevens kopieert naar uw werkruimte voor analyse.

### <a name="how-do-i-disable-data-collection"></a>Hoe kan ik het verzamelen van gegevens uitschakelen?
Automatische inrichting is standaard uitgeschakeld. Automatische inrichting van resources op elk gewenst moment door het uitschakelen van deze instelling in het beveiligingsbeleid, kunt u uitschakelen. Automatische inrichting wordt sterk aanbevolen om op te halen beveiligingswaarschuwingen en aanbevelingen over systeemupdates, beveiligingsproblemen en endpoint protection.

Om uit te schakelen van gegevens te verzamelen, [aanmelden bij de Azure-portal](https://portal.azure.com), selecteer **Bladeren**, selecteer **Security Center**, en selecteer **Selecteer beleid**. Selecteer het abonnement waarvoor u automatisch inrichten wilt uitschakelen. Wanneer u een abonnement selecteren **beveiligingsbeleid - het verzamelen van gegevens** wordt geopend. Onder **automatische inrichting**, selecteer **uit**.

### <a name="how-do-i-enable-data-collection"></a>Hoe kan ik het verzamelen van gegevens inschakelen?
U kunt het verzamelen van gegevens inschakelen voor uw Azure-abonnement in het beveiligingsbeleid. Om gegevens te verzamelen. [Meld u aan bij Azure portal](https://portal.azure.com), selecteer **Bladeren**, selecteer **Security Center**, en selecteer **beveiligingsbeleid**. Selecteer het abonnement dat u wilt inschakelen automatische inrichting. Wanneer u een abonnement selecteren **beveiligingsbeleid - het verzamelen van gegevens** wordt geopend. Onder **automatische inrichting**, selecteer **op**.

### <a name="what-happens-when-data-collection-is-enabled"></a>Wat gebeurt er wanneer gegevensverzameling is ingeschakeld?
Als automatisch inrichten is ingeschakeld, ondersteund levert Security Center de Microsoft Monitoring Agent op alle Azure-VM's en een nieuwe VM's die worden gemaakt. Automatische inrichting wordt sterk aanbevolen, maar handmatige agentinstallatie is ook beschikbaar. [Informatie over het installeren van de Microsoft Monitoring Agent-extensie](../log-analytics/log-analytics-quick-collect-azurevm.md#enable-the-log-analytics-vm-extension). 

De agent kan het proces maken gebeurtenis 4688 en de *CommandLine* veld binnen gebeurtenis 4688. Nieuwe processen die zijn gemaakt op de virtuele machine zijn vastgelegd door EventLog en bewaakt door Security Center de detectie van services. Zie voor informatie over de details voor elk nieuw proces vastgelegd [beschrijving velden in 4688](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4688#fields). De agent verzamelt de 4688 gebeurtenissen die zijn gemaakt op de virtuele machine ook en slaat ze op in het zoekvak.

De agent ook kunt u gegevens verzamelen voor [besturingselementen voor adaptieve toepassingen](security-center-adaptive-application.md), Security Center configureert u een lokale AppLocker-beleid in de controlemodus om toe te staan alle toepassingen. Dit zorgt ervoor dat AppLocker voor het genereren van gebeurtenissen die worden verzameld en gebruikt door Security Center. Het is belangrijk te weten dat dit beleid niet worden geconfigureerd op alle computers waarop er al een geconfigureerde AppLocker-beleid is. 

Wanneer Security Center verdachte activiteiten op de virtuele machine detecteert, wordt de klant per e-mail verwittigd als [beveiliging contactgegevens](security-center-provide-security-contact-details.md) is opgegeven. Een waarschuwing wordt ook weergegeven in het dashboard van Security Center security-waarschuwingen.



### <a name="does-the-monitoring-agent-impact-the-performance-of-my-servers"></a>De Monitoring Agent invloed op de prestaties van mijn servers?
De agent een nominaal bedrag van systeembronnen verbruikt en moet weinig invloed hebben op de prestaties. Zie voor meer informatie over de invloed op de prestaties en de agent en de extensie, de [planning- en bedieningsgids voor](security-center-planning-and-operations-guide.md#data-collection-and-storage).

### <a name="where-is-my-data-stored"></a>Waar worden mijn gegevens opgeslagen?
Gegevens die worden verzameld van deze agent worden opgeslagen in een bestaande Log Analytics-werkruimte die is gekoppeld aan uw abonnement of een nieuwe werkruimte. Zie voor meer informatie, [gegevensbeveiliging](security-center-data-security.md).

## <a name="using-azure-security-center"></a>Met behulp van Azure Security Center
### <a name="what-is-a-security-policy"></a>Wat is er een beveiligingsbeleid?
Een beveiligingsbeleid bepaalt welke set besturingselementen die worden aanbevolen voor resources binnen het opgegeven abonnement. In Azure Security Center definieert u beleid voor uw Azure-abonnementen op basis van de beveiligingsvereisten van uw bedrijf en het type toepassingen of de vertrouwelijkheid van de gegevens in elk abonnement.

Het beveiligingsbeleid ingeschakeld in Azure Security Center station beveiligingsaanbevelingen en bewaking. Zie voor meer informatie over het beveiligingsbeleid, [beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md).

### <a name="who-can-modify-a-security-policy"></a>Wie kan een beveiligingsbeleid alleen wijzigen?
Als u wilt een beveiligingsbeleid alleen wijzigen, moet u een beveiligingsbeheerder of een eigenaar of bijdrager van het abonnement zijn.

Zie voor informatie over het configureren van een beveiligingsbeleid, [beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md).

### <a name="what-is-a-security-recommendation"></a>Wat is een beveiligingsaanbeveling?
Azure Security Center analyseert de beveiligingsstatus van uw Azure-resources. Wanneer de potentiële beveiligingsproblemen worden geïdentificeerd, worden aanbevelingen worden gemaakt. De aanbevelingen begeleiden u bij het proces van het configureren van het vereiste besturingselement. Voorbeelden zijn:

* Inrichting van anti-malware om te identificeren en verwijderen van schadelijke software
* [Netwerkbeveiligingsgroepen](../virtual-network/security-overview.md) en regels voor het verkeer naar virtuele machines beheren
* Het inrichten van een web application firewall om te beschermen tegen aanvallen die gericht is op uw web-apps
* Implementatie van ontbrekende systeemupdates
* Aanpassing van besturingssysteemconfiguraties die niet overeenkomen met de aanbevolen basislijnen

Alleen de aanbevelingen die in de beleidsregels voor veiligheid zijn ingeschakeld, worden hier weergegeven.

### <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Hoe kan ik de huidige beveiligingsstatus van mijn Azure-resources bekijken?
De **Security Center-overzicht** blade ziet u de algehele beveiligingsstatus van uw omgeving onderverdeeld op basis van Compute, netwerken, opslag en gegevens en toepassingen. Elk resourcetype heeft een indicator die laat zien als alle mogelijke beveiligingsproblemen zijn geïdentificeerd. Te klikken op elke tegel geeft een lijst van beveiligingsproblemen die zijn geïdentificeerd door Security Center, samen met een overzicht van de resources in uw abonnement.

### <a name="what-triggers-a-security-alert"></a>Wat wordt een beveiligingswaarschuwing geactiveerd?
Azure Security Center automatisch verzamelt, analyseert en worden logboekgegevens van uw Azure-resources, het netwerk en partneroplossingen zoals antimalware en firewalls. Wanneer er dreigingen worden gedetecteerd, wordt een beveiligingswaarschuwing gemaakt. Voorbeelden zijn detectie van:

* Geïnfecteerde virtuele machines die communiceren met bekende schadelijke IP-adressen
* Geavanceerde malware is gedetecteerd met behulp van Windows Foutrapportage
* Beveiligingsaanvallen op virtuele machines
* Beveiligingswaarschuwingen van geïntegreerde partneroplossingen beveiliging zoals Anti-Malware of Web Application Firewalls

### <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Wat is het verschil tussen bedreigingen gedetecteerd en waarschuwingen over door Microsoft Security Response Center ten opzichte van Azure Security Center?
De Microsoft Security Response Center (MSRC) wordt uitgevoerd selecteert u beveiligingsbewaking van de Azure-netwerk en de infrastructuur en threat intelligence en misbruik klachten ontvangt van een derde partij. MSRC wordt wanneer op de hoogte dat klantgegevens zijn geopend door een partij onwettig of niet-geautoriseerde of dat gebruik van Azure van de klant niet aan de voorwaarden voor aanvaardbaar gebruik voldoet, een incident beveiligingsbeheer informeert de klant. Melding treedt meestal op door een e-mailbericht verzenden naar het security contactpersonen in Azure Security Center of de eigenaar van de Azure-abonnement moet worden opgegeven als een contactpersoon voor beveiliging is niet opgegeven.

Security Center is een Azure-service waarmee continu gecontroleerd van de klant Azure-omgeving en analytics voor het automatisch detecteren van een groot aantal mogelijk schadelijke activiteit is van toepassing. Deze detecties worden opgehaald als beveiligingswaarschuwingen in Security Center-dashboard.

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Welke Azure-resources worden bewaakt door Azure Security Center?
De volgende Azure-resources worden bewaakt door Azure Security Center:

* Virtuele machines (VM's) (met inbegrip van [Cloudservices](../cloud-services/cloud-services-choose-me.md))
* Virtuele netwerken van Azure
* Azure SQL-service
* Azure Storage-account
* Azure-Web-Apps (in [App Service-omgeving](../app-service/environment/intro.md))
* Oplossingen van partners die zijn geïntegreerd met uw Azure-abonnement, zoals een firewall voor webtoepassingen op virtuele machines en op App Service-omgeving

## <a name="virtual-machines"></a>Virtuele machines
### <a name="what-types-of-virtual-machines-are-supported"></a>Welke typen virtuele machines worden ondersteund?
Bewaking en aanbevelingen zijn beschikbaar voor virtuele machines (VM's) die zijn gemaakt met zowel de [klassieke en Resource Manager-implementatiemodel](../azure-classic-rm.md).

Zie [ondersteunde platforms in Azure Security Center](security-center-os-coverage.md) voor een lijst van ondersteunde platforms.

### <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Waarom niet Azure Security Center wordt herkend door de antimalwareoplossing die draait op mijn virtuele machine van Azure?
Azure Security Center heeft inzicht in antimalware geïnstalleerd via de Azure-extensies. Security Center is bijvoorbeeld niet kan detecteren, anti-malware die vooraf worden geïnstalleerd op een installatiekopie die u hebt opgegeven of als u anti-malware op uw virtuele machines met uw eigen processen (zoals systemen voor Configuratiebeheer) geïnstalleerd is.

### <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>Waarom krijg ik het bericht 'Ontbreekt scant gegevens' voor mijn VM?
Dit bericht wordt weergegeven wanneer er geen Scangegevens voor een virtuele machine zijn. Het kan even (minder dan een uur) voor Scangegevens voor het vullen van nadat het verzamelen van gegevens in Azure Security Center is ingeschakeld. Na de initiële populatie van Scangegevens, kan dit bericht wordt weergegeven omdat er helemaal geen Scangegevens zijn of er geen recente Scangegevens zijn. Scans vullen voor een virtuele machine in een status ' gestopt '. Dit bericht kan ook verschijnen als Scangegevens niet is ingevuld onlangs (in overeenstemming met het bewaarbeleid voor de Windows-agent, die een standaardwaarde van 30 dagen heeft).

### <a name="how-often-does-security-center-scan-for-operating-system-vulnerabilities-system-updates-and-endpoint-protection-issues"></a>Hoe vaak wordt door Security Center gescand op beveiligingsproblemen van besturingssystemen, systeemupdates en problemen met endpoint protection?
De latentie in Security Center gescand op beveiligingsproblemen, updates, en problemen is:

- Besturingssysteem-beveiligingsconfiguraties-gegevens wordt bijgewerkt binnen 48 uur
- Systeemupdates-gegevens wordt bijgewerkt binnen 24 uur
- Problemen met Endpoint Protection: gegevens worden bijgewerkt binnen de 8 uur

Security Center scant doorgaans voor nieuwe gegevens per uur. De latentie van bovenstaande waarden zijn een slechtste scenario waarbij er is een recente scan of een scan is mislukt.

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>Waarom krijg ik het bericht 'VM-Agent is ontbrekende?'
De VM-Agent moet worden geïnstalleerd op VM's om gegevens te verzamelen. De VM-agent wordt standaard geïnstalleerd op VM's die zijn geïmplementeerd vanuit Azure Marketplace. Zie voor meer informatie over het installeren van de VM-Agent op andere virtuele machines, het blogbericht [VM-Agent en -extensies](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).

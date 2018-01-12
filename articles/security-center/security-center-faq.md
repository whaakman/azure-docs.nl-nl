---
title: Azure Security Center Veelgestelde vragen (FAQ) | Microsoft Docs
description: Deze Veelgestelde vragen over de antwoorden op vragen over Azure Security Center.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/11/2018
ms.author: terrylan
ms.openlocfilehash: 2bbd0a8be891bd472cdc631a1f8dc79471d66a77
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="azure-security-center-frequently-asked-questions-faq"></a>Veelgestelde vragen over Azure Security Center
Deze Veelgestelde vragen over de antwoorden op vragen over Azure Security Center, een service waarmee u detecteren, voorkomen van en reageren op bedreigingen dankzij een verhoogde zichtbaarheid van en controle over de beveiliging van uw Microsoft Azure-resources.

> [!NOTE]
> Vanaf begin juni 2017 zal Security Center de Microsoft Monitoring Agent gebruiken voor het verzamelen en opslaan van gegevens. Zie voor meer informatie, [Azure Security Center-Platform migratie](security-center-platform-migration.md). De informatie in dit artikel beschrijft functionaliteit van Security Center na de overstap naar de Microsoft Monitoring Agent.
>
>

## <a name="general-questions"></a>Algemene vragen
### <a name="what-is-azure-security-center"></a>Wat is Azure Security Center?
Azure Security Center helpt u bij het detecteren, voorkomen van en reageren op bedreigingen dankzij een verhoogde zichtbaarheid van en controle over de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw abonnementen, helpt bedreigingen te detecteren die anders onopgemerkt zouden blijven, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

### <a name="how-do-i-get-azure-security-center"></a>Hoe krijg ik Azure Security Center?
Azure Security Center is met uw Microsoft Azure-abonnement ingeschakeld en is toegankelijk vanuit de [Azure-portal](https://azure.microsoft.com/features/azure-portal/). ([Aanmelden bij de portal](https://portal.azure.com), selecteer **Bladeren**, en schuif naar **Security Center**).  

## <a name="billing"></a>Facturering
### <a name="how-does-billing-work-for-azure-security-center"></a>Hoe werkt facturering voor Azure Security Center?
Security Center wordt aangeboden in twee lagen:

De **gratis laag** biedt inzicht in de beveiligingsstatus van uw Azure-resources, basic beveiligingsbeleid, aanbevelingen voor beveiliging en integratie met beveiligingsproducten en services van partners.

De **standaardcategorie** geavanceerde threat detectiemogelijkheden, met inbegrip van dreiging intelligence, gedragsanalyse, afwijkingsdetectie, beveiligingsincidenten en daarmee rapporten dreiging wordt toegevoegd. De Standard-laag is gratis gedurende de eerste 60 dagen. Moet u kiezen om te blijven gebruiken van de service na 60 dagen, wordt automatisch gestart kosten in rekening gebracht voor de service.  Als u wilt bijwerken, selecteert u [prijscategorie](https://docs.microsoft.com/azure/security-center/security-center-pricing) in het beveiligingsbeleid.

## <a name="permissions"></a>Machtigingen
Het Azure Beveiligingscentrum gebruikt [op rollen gebaseerd toegangsbeheer (RBAC)](../active-directory/role-based-access-control-configure.md), dat [ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md) biedt die kunnen worden toegewezen aan gebruikers, groepen en services in Azure.

Security Center beoordeelt de configuratie van uw resources om beveiligingsproblemen en beveiligingslekken te identificeren. In Security Center ziet u alleen informatie met betrekking tot een bron wanneer u de rol van eigenaar, bijdrager of lezer voor het abonnement of resourcegroep die deel uitmaakt van een resource te worden toegewezen.

Zie [machtigingen in Azure Security Center](security-center-permissions.md) voor meer informatie over de functies en de toegestane acties in Security Center.

## <a name="data-collection"></a>Gegevensverzameling
Security Center verzamelt gegevens van uw virtuele machines naar hun beveiligingsstatus beoordelen, aanbevelingen voor beveiliging bieden en een melding op bedreigingen. Wanneer u Security Center voor het eerst opent, worden gegevensverzameling is ingeschakeld op alle virtuele machines in uw abonnement. U kunt ook het verzamelen van gegevens in het beleid Security Center inschakelen.

### <a name="how-do-i-disable-data-collection"></a>Hoe kan ik gegevens verzamelen uitschakelen?
Als u van het Azure Security Center gratis laag gebruikmaakt, kunt u het verzamelen van gegevens van virtuele machines op elk gewenst moment uitschakelen. Verzamelen van gegevens is vereist voor abonnementen op de prijscategorie Standard. U kunt het verzamelen van gegevens voor een abonnement in het beveiligingsbeleid uitschakelen. ([Aanmelden bij de Azure-portal](https://portal.azure.com), selecteer **Bladeren**, selecteer **Security Center**, en selecteer **beleid**.)  Wanneer u een abonnement selecteren, een nieuwe blade geopend en biedt u de mogelijkheid om uit te schakelen **gegevensverzameling**.

### <a name="how-do-i-enable-data-collection"></a>Hoe kan ik gegevensverzameling inschakelen?
U kunt de gegevensverzameling inschakelen voor uw Azure-abonnement in het beveiligingsbeleid. Gegevensverzameling inschakelen. [Meld u aan bij de Azure portal](https://portal.azure.com), selecteer **Bladeren**, selecteer **Security Center**, en selecteer **beleid**. Stel **gegevensverzameling** naar **op**.

### <a name="what-happens-when-data-collection-is-enabled"></a>Wat gebeurt er wanneer gegevensverzameling is ingeschakeld?
Wanneer gegevensverzameling is ingeschakeld, wordt Microsoft Monitoring Agent automatisch geconfigureerd zijn op alle bestaande en nieuw ondersteunde virtuele machines die zijn geïmplementeerd in het abonnement.

De agent kan de gebeurtenis proces maken 4688 en de *CommandLine* veld binnen 4688-gebeurtenis. Nieuwe processen die zijn gemaakt op de virtuele machine zijn vastgelegd door EventLog en bewaakt door Security Center van detectie van services. Zie voor informatie over de details voor elke nieuwe proces vastgelegd [beschrijving velden in 4688](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4688#fields). De agent verzamelt de 4688 gebeurtenissen die zijn gemaakt op de virtuele machine ook en slaat ze op in de zoekopdracht.

Als Security Center verdachte activiteiten op de virtuele machine detecteert, wordt de klant per e-mail verwittigd als [beveiliging contactgegevens](security-center-provide-security-contact-details.md) is opgegeven. Een waarschuwing wordt ook weergegeven in Security Center de beveiliging waarschuwingen-dashboard.

### <a name="does-the-monitoring-agent-impact-the-performance-of-my-servers"></a>De Monitoring Agent heeft invloed op de prestaties van mijn servers?
De agent een nominaal hoeveelheid systeembronnen verbruikt en moet weinig invloed hebben op de prestaties. Zie voor meer informatie over de gevolgen voor de prestaties en de agent en de extensie, de [plannen en de operations guide](security-center-planning-and-operations-guide.md#data-collection-and-storage).

### <a name="where-is-my-data-stored"></a>Waar worden mijn gegevens opgeslagen?
Gegevens die worden verzameld van deze agent wordt opgeslagen in een bestaande werkruimte voor logboekanalyse gekoppeld aan uw abonnement of een nieuwe werkruimte. Zie voor meer informatie [gegevensbeveiliging](security-center-data-security.md).

## <a name="using-azure-security-center"></a>Met behulp van Azure Security Center
### <a name="what-is-a-security-policy"></a>Wat is beveiligingsbeleid?
Een beveiligingsbeleid bepaalt welke set besturingselementen die worden aanbevolen voor resources binnen het opgegeven abonnement. In Azure Security Center definieert u beleid voor uw Azure-abonnementen volgens de beveiligingsvereisten van uw bedrijf en het type toepassingen of de vertrouwelijkheid van de gegevens in elk abonnement.

Het beveiligingsbeleid in Azure Security Center station beveiligingsaanbevelingen en bewaking ingeschakeld. Zie voor meer informatie over beveiligingsbeleid, [beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md).

### <a name="who-can-modify-a-security-policy"></a>Wie kan een beveiligingsbeleid wijzigen?
Als u wilt een beveiligingsbeleid wijzigen, moet u een beveiligingsbeheerder of een eigenaar of bijdrager van het abonnement.

Zie voor meer informatie over het configureren van een beveiligingsbeleid, [beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md).

### <a name="what-is-a-security-recommendation"></a>Wat is een beveiligingsaanbeveling?
Azure Security Center analyseert de beveiligingsstatus van uw Azure-resources. Als mogelijke beveiligingsproblemen worden geïdentificeerd, worden aanbevelingen gemaakt. De aanbevelingen leiden u door het configuratieproces van het vereiste besturingselement. Voorbeelden zijn:

* Inrichting van antimalware om te identificeren en verwijderen van schadelijke software
* Configureren van [Netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-nsg.md) en regels voor het verkeer voor beheer voor virtuele machines
* Het inrichten van web application firewall om te beschermen tegen aanvallen die gericht is op uw webtoepassingen
* Implementatie van ontbrekende systeemupdates
* Aanpassing van besturingssysteemconfiguraties die niet overeenkomen met de aanbevolen basislijnen

Alleen de aanbevelingen die zijn ingeschakeld in het beveiligingsbeleid worden hier weergegeven.

### <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Hoe kan ik de huidige beveiligingsstatus van mijn Azure-resources zien?
De **Security Center Overview** blade ziet u de algehele beveiligingsstatus van uw omgeving uitgesplitst Compute, netwerken, opslag en gegevens en toepassingen. Elk resourcetype heeft een indicator weergeeft als alle mogelijke beveiligingsproblemen zijn geïdentificeerd. Elke tegel te klikken, geeft een lijst van beveiligingsproblemen die zijn geïdentificeerd door Security Center, samen met een inventaris van de resources in uw abonnement.

### <a name="what-triggers-a-security-alert"></a>Wat wordt een beveiligingswaarschuwing geactiveerd?
Azure Security Center automatisch verzamelt, analyseert en worden de logboekgegevens van uw Azure-resources, het netwerk en partneroplossingen zoals antimalware en firewalls. Wanneer er dreigingen worden gedetecteerd, wordt een beveiligingswaarschuwing gemaakt. Voorbeelden zijn detectie van:

* Geïnfecteerde virtuele machines die communiceren met bekende schadelijke IP-adressen
* Geavanceerde malware is gedetecteerd met Windows Foutrapportage
* Beveiligingsaanvallen op virtuele machines
* Beveiligingswaarschuwingen van geïntegreerde partneroplossingen beveiliging zoals Anti-Malware of Web Application Firewalls

### <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Wat is het verschil tussen bedreigingen gedetecteerd en een melding op door de Microsoft Security Response Center versus Azure Security Center?
Microsoft Security Response Center (MSRC) voert selecteert u beveiligingsbewaking van de Azure-netwerk en de infrastructuur en threat intelligence en misbruik klachten ontvangt van een derde partij. Wanneer MSRC realiseert dat gegevens van de klant is geopend door een partij onrechtmatig of niet-geautoriseerde of dat de klant gebruik van Azure niet aan de voorwaarden voor acceptabel gebruik voldoet, een incident beveiligingsbeheer informeert de klant. Melding treedt meestal op door een e-mailbericht te sturen naar de contactpersonen beveiliging is opgegeven in Azure Security Center of de eigenaar van de Azure-abonnement als een contactpersoon beveiliging is niet opgegeven.

Security Center is een Azure-service die continu bewaakt van de klant Azure-omgeving en analytics voor automatische detectie van een groot aantal mogelijk schadelijke activiteiten toepast. Deze detecties worden opgehaald als beveiligingswaarschuwingen in Security Center-dashboard.

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Welke Azure-resources worden bewaakt door Azure Security Center?
Azure Security Center bewaakt de volgende Azure-resources:

* Virtuele machines (VM's) (met inbegrip van [Cloudservices](../cloud-services/cloud-services-choose-me.md))
* Virtuele netwerken van Azure
* Azure SQL-service
* Azure-opslagaccount
* Azure-Web-Apps (in [App Service-omgeving](../app-service/environment/intro.md))
* Partneroplossingen die zijn geïntegreerd met uw Azure-abonnement zoals web application firewall op virtuele machines en op App Service-omgeving

## <a name="virtual-machines"></a>Virtuele machines
### <a name="what-types-of-virtual-machines-are-supported"></a>Welke typen virtuele machines worden ondersteund?
Bewaking en aanbevelingen zijn beschikbaar voor virtuele machines (VM's) die zijn gemaakt met zowel de [klassieke en Resource Manager-implementatiemodel](../azure-classic-rm.md).

Zie [ondersteunde platforms in Azure Security Center](security-center-os-coverage.md) voor een lijst met ondersteunde platforms.

### <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Waarom niet Azure Security Center wordt herkend door de antimalwareoplossing uitgevoerd op mijn Azure VM?
Azure Security Center heeft zichtbaarheid van antimalware geïnstalleerd via de Azure-extensies. Security Center is bijvoorbeeld niet kan detecteren antimalware die vooraf geïnstalleerd op een installatiekopie die u hebt opgegeven of is als u anti-malware op uw virtuele machines met uw eigen processen (zoals systemen voor Configuratiebeheer) hebt geïnstalleerd.

### <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>Waarom krijg ik het bericht 'Missing scannen Data' voor mijn VM?
Dit bericht wordt weergegeven wanneer er geen Scangegevens voor een virtuele machine zijn. Het kan even duren (minder dan een uur) om Scangegevens te vullen nadat gegevensverzameling is ingeschakeld in Azure Security Center. Na de initiële gegevenspopulatie scan, kan dit bericht wordt weergegeven omdat er helemaal geen Scangegevens zijn of er geen recente Scangegevens zijn. Scans vullen voor een virtuele machine gestopt. Dit bericht kan ook optreden als Scangegevens heeft onlangs (in overeenstemming met het bewaarbeleid voor de Windows-agent, die een standaardwaarde van 30 dagen heeft) niet is ingevuld.

### <a name="how-often-does-security-center-scan-for-operating-system-vulnerabilities-system-updates-and-endpoint-protection-issues"></a>Hoe vaak Security Center scannen voor besturingssysteem beveiligingsproblemen, systeemupdates en endpoint protection-problemen?
De latentie in Security Center scant op beveiligingsproblemen, updates, en problemen is:

- Besturingssysteem beveiligingsconfiguraties – gegevens wordt bijgewerkt binnen 48 uur
- Systeemupdates – gegevens wordt bijgewerkt binnen 24 uur
- Endpoint Protection-problemen: gegevens worden bijgewerkt binnen de 8 uur

Security Center scant doorgaans naar nieuwe gegevens elk uur. De latentie-waarden die hierboven zijn een slechtste scenario waarbij er niet een recente scan of een controle mislukt.

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>Waarom krijg ik het bericht 'VM-Agent is ontbreekt?'
De VM-Agent moet worden geïnstalleerd op VM's gegevensverzameling inschakelen. De VM-agent wordt standaard geïnstalleerd op VM's die zijn geïmplementeerd vanuit Azure Marketplace. Zie voor informatie over het installeren van de VM-Agent op andere virtuele machines, het blogbericht [VM-Agent en -extensies](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).

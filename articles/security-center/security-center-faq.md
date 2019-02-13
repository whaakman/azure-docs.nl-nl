---
title: Azure Security Center Veelgestelde vragen (FAQ) | Microsoft Docs
description: Deze Veelgestelde vragen vindt u antwoorden op vragen over Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/31/2018
ms.author: rkarlin
ms.openlocfilehash: 61dac6bea6878630c05339778c717f7818c1c662
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56106644"
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

De **Standard-laag** geavanceerde threat detectiemogelijkheden, waaronder threat intelligence, gedragsanalyse, anomaliedetectie, beveiligingsincidenten en attribution rapporten van bedreigingen wordt toegevoegd. U kunt een standaard tierfree proefversie starten. Als u wilt bijwerken, selecteert u [prijscategorie](https://docs.microsoft.com/azure/security-center/security-center-pricing) in het beveiligingsbeleid. Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie.

## <a name="permissions"></a>Machtigingen
Het Azure Beveiligingscentrum gebruikt [op rollen gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/role-assignments-portal.md), dat [ingebouwde rollen](../role-based-access-control/built-in-roles.md) biedt die kunnen worden toegewezen aan gebruikers, groepen en services in Azure.

Security Center beoordeelt de configuratie van uw resources om beveiligingsproblemen met zich mee en beveiligingsproblemen te identificeren. In Security Center ziet u alleen informatie met betrekking tot een bron wanneer u de rol van eigenaar, bijdrager of lezer voor het abonnement of de resourcegroep die deel uitmaakt van een resource om te worden toegewezen.

Zie [machtigingen in Azure Security Center](security-center-permissions.md) voor meer informatie over de functies en toegestane acties in Security Center.

## <a name="data-collection-agents-and-workspaces"></a>Het verzamelen van gegevens, agents en werkruimten
Security Center verzamelt gegevens van uw virtuele Azure-machines (VM's) en niet-Azure-computers om te controleren op beveiligingsproblemen en bedreigingen. De gegevens worden verzameld met behulp van de MMA, die verschillende configuraties en gebeurtenislogboeken met betrekking tot beveiliging van de machine leest en de gegevens kopieert naar uw werkruimte voor analyse.

### <a name="am-i-billed-for-log-analytics-on-the-workspaces-created-by-security-center"></a>Word ik gefactureerd voor Log Analytics op de werkruimten die zijn gemaakt door Security Center?
Nee. Werkruimten die zijn gemaakt door Security Center, terwijl voor Log Analytics geconfigureerd per knooppunt facturering, doen niet Log Analytics kosten in rekening gebracht. Security Center-facturering is altijd gebaseerd op het Security Center-beveiligingsbeleid en de oplossingen die zijn geïnstalleerd op een werkruimte:

- **Gratis laag** – Security Center schakelt u de oplossing 'SecurityCenterFree' in de standaardwerkruimte. U bent niet in rekening gebracht voor de gratis laag.
- **Standard-laag** – Security Center schakelt u de oplossing 'Beveiliging' in de standaardwerkruimte.

Zie voor meer informatie over prijzen [prijzen van Security Center](https://azure.microsoft.com/pricing/details/security-center/). De pagina met prijzen adressen wijzigingen in de opslag van beveiligingsgegevens en facturering naar rato vanaf juni 2017.

> [!NOTE]
> De prijscategorie van werkruimten die zijn gemaakt door Security Center met Log Analytics heeft geen invloed op de facturering van Security Center.
>
>

### <a name="what-qualifies-a-vm-for-automatic-provisioning-of-the-microsoft-monitoring-agent-installation"></a>Wat is een virtuele machine de definitie voor automatische inrichting van de Microsoft Monitoring Agent-installatie?
Windows- of Linux IaaS-VM's in aanmerking komt als:

- De Microsoft Monitoring Agent-extensie is momenteel niet geïnstalleerd op de virtuele machine.
- De virtuele machine wordt uitgevoerd.
- De Windows- of Linux VM-Agent is geïnstalleerd.
- De virtuele machine wordt niet gebruikt als een apparaat, zoals de web application firewall of de firewall van volgende generatie.

### <a name="can-i-delete-the-default-workspaces-created-by-security-center"></a>Kan ik de Standaardwerkruimten die zijn gemaakt door Security Center wilt verwijderen?
**Verwijderen van de standaardwerkruimte wordt niet aanbevolen.** Security Center maakt gebruik van de Standaardwerkruimten voor het opslaan van beveiligingsgegevens van uw virtuele machines.  Als u een werkruimte verwijdert, kan Security Center voor het verzamelen van deze gegevens en enkele aanbevelingen voor beveiliging en waarschuwingen zijn niet beschikbaar.

Als u wilt herstellen, verwijdert u de Microsoft Monitoring Agent op de virtuele machines die zijn verbonden met de verwijderde werkruimte. Security Center de agent opnieuw installeren en nieuwe Standaardwerkruimten maken.

### <a name="how-can-i-use-my-existing-log-analytics-workspace"></a>Hoe kan ik mijn bestaande Log Analytics-werkruimte gebruiken?

U kunt een bestaande werkruimte voor logboekanalyse voor het opslaan van gegevens die zijn verzameld door Security Center kunt selecteren. Gebruik uw bestaande Log Analytics-werkruimte:

- De werkruimte moet worden gekoppeld aan uw geselecteerde Azure-abonnement.
- Ten minste, moet u machtigingen voor toegang tot de werkruimte hebt gelezen.

Selecteer een bestaande Log Analytics-werkruimte:

1. Onder **beveiligingsbeleid – gegevensverzameling**, selecteer **gebruik een andere werkruimte**.

   ![Een andere werkruimte gebruiken][5]

2. Selecteer een werkruimte voor het opslaan van verzamelde gegevens in de vervolgkeuzelijst.

   > [!NOTE]
   > In de vervolgkeuzelijst menu, worden alleen werkruimten die u hebt toegang tot en zijn in uw Azure-abonnement weergegeven.
   >
   >

3. Selecteer **Opslaan**.
4. Na het selecteren van **opslaan**, wordt u gevraagd als u reconfigure bewaakte VM's wilt.

   - Selecteer **Nee** als u wilt dat de nieuwe werkruimte instellingen **toepassen op nieuwe virtuele machines**. De nieuwe werkruimte-instellingen zijn alleen van toepassing op nieuwe agentinstallaties; nieuwe gedetecteerde virtuele machines die geen van de Microsoft Monitoring Agent is geïnstalleerd.
   - Selecteer **Ja** als u wilt dat de nieuwe werkruimte instellingen **toepassen op alle virtuele machines**. Bovendien wordt elke virtuele machine verbonden met een Security Center-werkruimte gemaakt verbonden aan de nieuwe doelwerkruimte.

   > [!NOTE]
   > Als u Ja selecteert, moet u de werkruimten die zijn gemaakt door Security Center totdat alle virtuele machines hebt opnieuw is verbonden met de nieuwe doelwerkruimte niet verwijderen. Met deze bewerking mislukt als een werkruimte te vroeg is verwijderd.
   >
   >

   - Selecteer **annuleren** om de bewerking te annuleren.

### <a name="what-if-the-microsoft-monitoring-agent-was-already-installed-as-an-extension-on-the-vm"></a>Wat gebeurt er als dat de Microsoft Monitoring Agent al is geïnstalleerd als een uitbreiding op de virtuele machine?
Security Center wordt de bestaande verbindingen met werkruimten van de gebruiker niet overschreven. Security Center worden beveiligingsgegevens van de virtuele machine opgeslagen in de werkruimte is al verbonden. Security Center werkt de versie van de extensie om op te nemen van de Azure-resource-ID van de virtuele machine voor de ondersteuning van Security Center gebruik.

### <a name="what-if-i-had-a-microsoft-monitoring-agent-installed-on-the-machine-but-not-as-an-extension"></a>Wat gebeurt er als ik heb een Microsoft Monitoring Agent geïnstalleerd op de computer, maar niet als een uitbreiding?
Als de Microsoft Monitoring Agent rechtstreeks op de virtuele machine (en niet als een Azure-extensie) is geïnstalleerd, de Microsoft Monitoring Agent wordt niet geïnstalleerd door Security Center en beveiligingsbewaking is beperkt.

Zie voor meer informatie de volgende sectie [wat gebeurt er als een SCOM of OMS directe agent al is geïnstalleerd op mijn VM?](#scomomsinstalled)

### Wat gebeurt er als een SCOM of OMS agent aansturen op mijn virtuele machine is al geïnstalleerd?<a name="scomomsinstalled"></a>
Security Center kan niet van tevoren identificeren dat een agent is geïnstalleerd.  Security Center probeert te installeren van de Microsoft Monitoring Agent-extensie en mislukt als gevolg van de bestaande geïnstalleerde agent.  Deze fout wordt voorkomen dat de verbindingsinstellingen van de agent voor de werkruimte te overschrijven en voorkomt het maken van multihoming.

> [!NOTE]
> De agent-versie is bijgewerkt naar de meest recente versie van de OMS-agent.  Dit geldt ook voor SCOM-gebruikers.
>
>

### <a name="what-is-the-impact-of-removing-these-extensions"></a>Wat zijn de gevolgen van het verwijderen van deze extensies?
Als u de extensie voor Microsoft Monitoring verwijdert, Security Center kan geen beveiligingsgegevens verzamelen van de virtuele machine en enkele aanbevelingen voor beveiliging en waarschuwingen zijn niet beschikbaar. Security Center bepaalt binnen 24 uur dat de virtuele machine in de uitbreiding ontbreekt en de extensie opnieuw geïnstalleerd.

### <a name="how-do-i-stop-the-automatic-agent-installation-and-workspace-creation"></a>Hoe voorkom ik automatische agent-installatie en werkruimte maken
U kunt uitschakelen automatische inrichting van uw abonnementen in het beveiligingsbeleid, maar dit wordt niet aanbevolen. Het uitschakelen van automatische inrichting limieten Security Center aanbevelingen en waarschuwingen. Uitschakelen van automatische inrichting:

1. Als uw abonnement is geconfigureerd voor de laag standaard, opent u het beveiligingsbeleid voor dat abonnement en selecteer de **gratis** laag.

   ![Prijscategorie][1]

2. Vervolgens uitschakelen door het selecteren van een automatische inrichting **uit** op de **beveiligingsbeleid – gegevensverzameling** blade.
   ![Gegevensverzameling][2]

### <a name="should-i-opt-out-of-the-automatic-agent-installation-and-workspace-creation"></a>Moet ik opt-out voor de automatische installatie van agent en werkruimte maken?

> [!NOTE]
> Lees secties [wat zijn de gevolgen van het uitschrijft?](#what-are-the-implications-of-opting-out-of-automatic-provisioning) en [aanbevolen stappen wanneer uitschrijft](#what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning) als u wilt afmelden voor automatische inrichting.
>
>

U wilt afmelden voor automatische inrichting als voor u het volgende geldt:

- Automatische installatie van agent door Security Center is van toepassing op het hele abonnement.  U kunt geen automatische installatie toepassen op een subset van virtuele machines. Als er kritieke VM's die met de Microsoft Monitoring Agent kunnen niet worden geïnstalleerd, moet u ervoor kiezen automatische inrichting van buiten.
- Installatie van de Microsoft Monitoring Agent-extensie van de agent-versie-updates. Dit geldt voor een directe agent en een SCOM-agent. Als de geïnstalleerde SCOM-agent versie 2012 is en wordt bijgewerkt, kunnen beheerbaarheid mogelijkheden worden verbroken wanneer de SCOM-server ook versie 2012. U moet overwegen wanneer u geen gebruik automatische inrichting als de geïnstalleerde SCOM-agent versie 2012.
- Als u een aangepaste werkruimte buiten het abonnement (een gecentraliseerde werkruimte) hebt moet u ervoor kiezen automatische inrichting van buiten. U kunt handmatig installeren van de Microsoft Monitoring Agent-extensie en verbindt dit uw werkruimte zonder Security Center voor het overschrijven van de verbinding.
- Als u wilt voorkomen dat het maken van meerdere werkruimten per abonnement en u uw eigen aangepaste werkruimte binnen het abonnement hebt, hebt u twee opties:

   1. U kunt afmelden voor automatische inrichting. Na de migratie, stel de standaardwaarde in werkruimte-instellingen zoals beschreven in [hoe kan ik mijn bestaande Log Analytics-werkruimte gebruiken?](#how-can-i-use-my-existing-log-analytics-workspace)
   2. Of u kunt de migratie is voltooid, de Microsoft Monitoring Agent worden geïnstalleerd op de virtuele machines, en de virtuele machines die zijn verbonden met de gemaakte werkruimte. Selecteer vervolgens uw eigen aangepaste werkruimte door in te stellen de standaardinstelling van de werkruimte met inschrijving voor het opnieuw configureren van de reeds geïnstalleerde agents. Zie voor meer informatie, [hoe kan ik mijn bestaande Log Analytics-werkruimte gebruiken?](#how-can-i-use-my-existing-log-analytics-workspace)

### <a name="what-are-the-implications-of-opting-out-of-automatic-provisioning"></a>Wat zijn de gevolgen van het geen automatische inrichting?
Zodra de migratie is voltooid, Security Center kan geen beveiligingsgegevens verzamelen van de virtuele machine en enkele aanbevelingen voor beveiliging en waarschuwingen zijn niet beschikbaar. Als u zich afmelden, moet u de Microsoft Monitoring Agent handmatig installeren. Zie [aanbevolen stappen wanneer uitschrijft](#what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning).

### <a name="what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning"></a>Wat zijn de aanbevolen stappen wanneer u geen automatische inrichting?

U moet de Microsoft Monitoring Agent-extensie handmatig installeren, zodat Security Center kunt verzamelen van beveiligingsgegevens van uw virtuele machines en aanbevelingen en waarschuwingen bieden. Zie [installatie van de agent voor Windows-VM](../virtual-machines/extensions/oms-windows.md) of [installatie van de agent voor Linux VM](../virtual-machines/extensions/oms-linux.md) voor hulp bij de installatie.

U kunt de agent koppelen aan een bestaande aangepaste werkruimte of Security Center-werkruimte gemaakt. Als u een aangepaste werkruimte heeft geen van de 'Beveiliging' of 'SecurityCenterFree' oplossingen die zijn ingeschakeld, moet u een oplossing van toepassing. Als u wilt toepassen, selecteert u de aangepaste werkruimte of het abonnement en toepassen van een prijscategorie via de **beveiligingsbeleid – prijscategorie** blade.

   ![Prijscategorie][1]

Security Center kunnen de juiste oplossing is in de werkruimte op basis van de geselecteerde prijscategorie.

### Hoe verwijder ik OMS-extensies geïnstalleerd door Security Center?<a name="remove-oms"></a>
U kunt de Microsoft Monitoring Agent handmatig verwijderen. Dit wordt niet aanbevolen omdat deze wordt beperkt door Security Center aanbevelingen en waarschuwingen.

> [!NOTE]
> Als gegevensverzameling is ingeschakeld, kan Security Center de agent wordt opnieuw nadat u deze verwijderen.  U moet verzamelen van gegevens voordat u de agent handmatig verwijdert uitschakelen. Zie hoe stop ik het automatische agent-installatie en werkruimte maken? voor instructies over het verzamelen van gegevens uitschakelen.
>
>

De agent handmatig te verwijderen:

1.  Open in de portal **Log Analytics**.
2.  Op de blade Log Analytics een werkruimte te selecteren:
3.  Selecteer elke VM die u niet wilt bewaken en selecteer **verbinding verbreken**.

   ![De agent verwijderen][3]

> [!NOTE]
> Als een Linux-VM al een OMS-agent zonder extensie heeft, wordt de extensie verwijdert, worden ook de agent en de klant heeft deze opnieuw installeren.
>
>
### <a name="how-do-i-disable-data-collection"></a>Hoe kan ik het verzamelen van gegevens uitschakelen?
Automatische inrichting is standaard uitgeschakeld. Automatische inrichting van resources op elk gewenst moment door het uitschakelen van deze instelling in het beveiligingsbeleid, kunt u uitschakelen. Automatische inrichting wordt sterk aanbevolen om op te halen beveiligingswaarschuwingen en aanbevelingen over systeemupdates, beveiligingsproblemen en endpoint protection.

Om uit te schakelen van gegevens te verzamelen, [aanmelden bij de Azure-portal](https://portal.azure.com), selecteer **Bladeren**, selecteer **Security Center**, en selecteer **Selecteer beleid**. Selecteer het abonnement waarvoor u automatisch inrichten wilt uitschakelen. Wanneer u een abonnement selecteren **beveiligingsbeleid - het verzamelen van gegevens** wordt geopend. Onder **automatische inrichting**, selecteer **uit**.

### <a name="how-do-i-enable-data-collection"></a>Hoe kan ik het verzamelen van gegevens inschakelen?
U kunt het verzamelen van gegevens inschakelen voor uw Azure-abonnement in het beveiligingsbeleid. Om gegevens te verzamelen. [Meld u aan bij Azure portal](https://portal.azure.com), selecteer **Bladeren**, selecteer **Security Center**, en selecteer **beveiligingsbeleid**. Selecteer het abonnement dat u wilt inschakelen automatische inrichting. Wanneer u een abonnement selecteren **beveiligingsbeleid - het verzamelen van gegevens** wordt geopend. Onder **automatische inrichting**, selecteer **op**.

### <a name="what-happens-when-data-collection-is-enabled"></a>Wat gebeurt er wanneer gegevensverzameling is ingeschakeld?
Als automatisch inrichten is ingeschakeld, ondersteund levert Security Center de Microsoft Monitoring Agent op alle Azure-VM's en een nieuwe VM's die worden gemaakt. Automatische inrichting wordt sterk aanbevolen, maar handmatige agentinstallatie is ook beschikbaar. [Informatie over het installeren van de Microsoft Monitoring Agent-extensie](../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension). 

De agent kan het proces maken gebeurtenis 4688 en de *CommandLine* veld binnen gebeurtenis 4688. Nieuwe processen die zijn gemaakt op de virtuele machine zijn vastgelegd door EventLog en bewaakt door Security Center de detectie van services. Zie voor informatie over de details voor elk nieuw proces vastgelegd [beschrijving velden in 4688](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4688#fields). De agent verzamelt de 4688 gebeurtenissen die zijn gemaakt op de virtuele machine ook en slaat ze op in het zoekvak.

De agent ook kunt u gegevens verzamelen voor [besturingselementen voor adaptieve toepassingen](security-center-adaptive-application.md), Security Center configureert u een lokale AppLocker-beleid in de controlemodus om toe te staan alle toepassingen. Dit zorgt ervoor dat AppLocker voor het genereren van gebeurtenissen die worden verzameld en gebruikt door Security Center. Het is belangrijk te weten dat dit beleid niet worden geconfigureerd op alle computers waarop er al een geconfigureerde AppLocker-beleid is. 

Wanneer Security Center verdachte activiteiten op de virtuele machine detecteert, wordt de klant per e-mail verwittigd als [beveiliging contactgegevens](security-center-provide-security-contact-details.md) is opgegeven. Een waarschuwing wordt ook weergegeven in het dashboard van Security Center security-waarschuwingen.



### <a name="does-the-monitoring-agent-impact-the-performance-of-my-servers"></a>De Monitoring Agent invloed op de prestaties van mijn servers?
De agent een nominaal bedrag van systeembronnen verbruikt en moet weinig invloed hebben op de prestaties. Zie voor meer informatie over de invloed op de prestaties en de agent en de extensie, de [planning- en bedieningsgids voor](security-center-planning-and-operations-guide.md#data-collection-and-storage).

### <a name="where-is-my-data-stored"></a>Waar worden mijn gegevens opgeslagen?
Gegevens die worden verzameld van deze agent worden opgeslagen in een bestaande Log Analytics-werkruimte die is gekoppeld aan uw abonnement of een nieuwe werkruimte. Zie voor meer informatie, [gegevensbeveiliging](security-center-data-security.md).

## Bestaande klanten van Log Analytics<a name="existingloganalyticscust"></a>

### <a name="does-security-center-override-any-existing-connections-between-vms-and-workspaces"></a>Security Center heeft voorrang op bestaande verbindingen tussen virtuele machines en werkruimten?
Als een virtuele machine al de Microsoft Monitoring Agent is geïnstalleerd als een uitbreiding van Azure is, wordt de bestaande verbinding in de werkruimte niet overschreven door Security Center. Security Center gebruikt in plaats daarvan de bestaande werkruimte.

Een oplossing voor Security Center is geïnstalleerd op de werkruimte als dat niet al aanwezig, en de oplossing wordt alleen toegepast op de relevante virtuele machines. Wanneer u een oplossing toevoegt, wordt deze automatisch geïmplementeerd standaard voor alle Windows en Linux-agents die zijn verbonden met uw Log Analytics-werkruimte. [Oplossing doelitems](../operations-management-suite/operations-management-suite-solution-targeting.md) kunt u een bereik toepassen op uw oplossingen.

Als de Microsoft Monitoring Agent rechtstreeks op de virtuele machine (en niet als een Azure-extensie) is geïnstalleerd, de Microsoft Monitoring Agent wordt niet geïnstalleerd door Security Center en beveiligingsbewaking is beperkt.

### <a name="does-security-center-install-solutions-on-my-existing-log-analytics-workspaces-what-are-the-billing-implications"></a>Security Center oplossingen installeren op mijn bestaande Log Analytics-werkruimten Wat zijn de gevolgen voor de facturering?
Als Security Center identificeert dat een virtuele machine al is verbonden met een werkruimte die u hebt gemaakt, kunt u Security Center-oplossingen voor deze werkruimte op basis van uw prijscategorie. De oplossingen worden alleen toegepast op de relevante Azure-virtuele machines, via [oplossingstargeting](../operations-management-suite/operations-management-suite-solution-targeting.md), zodat de facturering hetzelfde blijft.

- **Gratis laag** – installeert Security Center de oplossing 'SecurityCenterFree' in de werkruimte. U bent niet in rekening gebracht voor de gratis laag.
- **Standard-laag** – installeert Security Center de beveiligingsoplossing in de werkruimte.

   ![Oplossingen op standaardwerkruimte][4]

### <a name="i-already-have-workspaces-in-my-environment-can-i-use-them-to-collect-security-data"></a>Ik heb al werkruimten in Mijn omgeving, kan ik deze niet gebruiken voor het verzamelen van beveiligingsgegevens?
Als een virtuele machine al de Microsoft Monitoring Agent is geïnstalleerd als een uitbreiding van Azure is, wordt in Security Center maakt gebruik van de bestaande gekoppelde werkruimte. Een oplossing voor Security Center is geïnstalleerd op de werkruimte als dit niet al aanwezig, en de oplossing wordt alleen toegepast op de relevante virtuele machines via [oplossingstargeting](../operations-management-suite/operations-management-suite-solution-targeting.md).

Wanneer Security Center de Microsoft Monitoring Agent is geïnstalleerd op virtuele machines, gebruikt de standaard werkruimten die zijn gemaakt door Security Center.

### <a name="i-already-have-security-solution-on-my-workspaces-what-are-the-billing-implications"></a>Ik heb al beveiligingsoplossing op mijn werkruimten. Wat zijn de gevolgen voor de facturering?
De oplossing beveiliging en Audit wordt Security Center Standard-laag-functies inschakelen voor virtuele Azure-machines gebruikt. Als de oplossing beveiliging en Audit is al geïnstalleerd op een werkruimte, wordt in Security Center maakt gebruik van de bestaande oplossing. Er is geen wijziging in facturering.

## <a name="using-azure-security-center"></a>Met behulp van Azure Security Center
### <a name="what-is-a-security-policy"></a>Wat is er een beveiligingsbeleid?
Een beveiligingsbeleid bepaalt welke set besturingselementen die worden aanbevolen voor resources binnen het opgegeven abonnement. In Azure Security Center definieert u beleid voor uw Azure-abonnementen op basis van de beveiligingsvereisten van uw bedrijf en het type toepassingen of de vertrouwelijkheid van de gegevens in elk abonnement.

Het beveiligingsbeleid ingeschakeld in Azure Security Center station beveiligingsaanbevelingen en bewaking. Zie voor meer informatie over het beveiligingsbeleid, [beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md).

### <a name="who-can-modify-a-security-policy"></a>Wie kan een beveiligingsbeleid alleen wijzigen?
Als u wilt een beveiligingsbeleid alleen wijzigen, moet u een beveiligingsbeheerder of een eigenaar of bijdrager van het abonnement zijn.

Zie voor informatie over het configureren van een beveiligingsbeleid, [beveiligingsbeleid instellen in Azure Security Center](tutorial-security-policy.md).

### <a name="what-is-a-security-recommendation"></a>Wat is een beveiligingsaanbeveling?
Azure Security Center analyseert de beveiligingsstatus van uw Azure-resources. Wanneer de potentiële beveiligingsproblemen worden geïdentificeerd, worden aanbevelingen worden gemaakt. De aanbevelingen begeleiden u bij het proces van het configureren van het vereiste besturingselement. Een aantal voorbeelden:

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

Security Center doorgaans scant op nieuwe gegevens elk uur, en Hiermee vernieuwt u de aanbevelingen dienovereenkomstig. 

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>Waarom krijg ik het bericht 'VM-Agent is ontbrekende?'
De VM-Agent moet worden geïnstalleerd op VM's om gegevens te verzamelen. De VM-agent wordt standaard geïnstalleerd op VM's die zijn geïmplementeerd vanuit Azure Marketplace. Zie voor meer informatie over het installeren van de VM-Agent op andere virtuele machines, het blogbericht [VM-Agent en -extensies](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).


<!--Image references-->
[1]: ./media/security-center-platform-migration-faq/pricing-tier.png
[2]: ./media/security-center-platform-migration-faq/data-collection.png
[3]: ./media/security-center-platform-migration-faq/remove-the-agent.png
[4]: ./media/security-center-platform-migration-faq/solutions.png
[5]: ./media/security-center-platform-migration-faq/use-another-workspace.png

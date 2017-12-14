---
title: Migratie van de platforms Security Center FAQ | Microsoft Docs
description: Deze Veelgestelde vragen over de antwoorden op vragen over de migratie van Azure Security Center-platform.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 4d1364cd-7847-425a-bb3a-722cb0779f78
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2017
ms.author: terrylan
ms.openlocfilehash: 6ccf104ea09dc1fbce1dd34a06168205d6f5fac8
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="security-center-platform-migration-faq"></a>Security Center-platform migratie Veelgestelde vragen
In eerdere juni 2017 begonnen Azure Security Center met behulp van Microsoft Monitoring Agent voor het verzamelen en opslaan van gegevens. Zie voor meer informatie, [Azure Security Center-Platform migratie](security-center-platform-migration.md). Deze Veelgestelde vragen over de antwoorden op vragen over de migratie van het platform.

## <a name="data-collection-agents-and-workspaces"></a>Verzamelen van gegevens, agents en werkruimten

### <a name="how-is-data-collected"></a>Hoe worden gegevens verzameld
Microsoft Monitoring Agent Security Center gebruikt voor het verzamelen van beveiligingsgegevens van uw virtuele machines. De beveiligingsgegevens vindt u informatie over:

- beveiligingsconfiguraties - gebruikt om beveiligingsproblemen te identificeren
- beveiligingsgebeurtenissen - gebruikt voor het detecteren van bedreigingen

Gegevens die worden verzameld door de agent wordt opgeslagen in een bestaande werkruimte voor logboekanalyse is verbonden met de virtuele machine of een nieuwe werkruimte gemaakt door Security Center. Wanneer Security Center een nieuwe werkruimte maakt, wordt de geolocatie van de virtuele machine in aanmerking genomen.

> [!NOTE]
> Microsoft Monitoring Agent is dezelfde agent die wordt gebruikt door de Operations Management Suite (OMS), Log Analytics-service en System Center Operations Manager (SCOM).
>
>

Wanneer automatische inrichting (eerder logboekverzameling genoemd) is ingeschakeld of wanneer uw abonnementen worden gemigreerd, controleert Security Center als Microsoft Monitoring Agent al is geïnstalleerd als een Azure-extensie op elk van uw virtuele machines. Als Microsoft Monitoring Agent is niet geïnstalleerd, klikt u vervolgens standaard Security Center wordt uitgevoerd:

- De uitbreiding voor de Microsoft Monitoring Agent installeren op de virtuele machine.

   - Als een werkruimte gemaakt door Security Center al in de dezelfde geolocatie als de virtuele machine bestaat, wordt de agent is verbonden met deze werkruimte.
   - Als een werkruimte niet bestaat, Security Center maakt een nieuwe resource en het standaard werkruimte in die geolocatie en wordt de agent verbonden met deze werkruimte. De naamconventie voor de werkruimte en resource-groep is:

       Werkruimte: DefaultWorkspace-[abonnement-ID]-[geo]

       Resourcegroep: DefaultResouceGroup-[geo]

- Schakel een oplossing Security Center in de werkruimte per VM gekoppeld prijscategorie in Security Center. Zie voor meer informatie over prijzen [Security Center prijzen](https://azure.microsoft.com/pricing/details/security-center/).
- Security Center voor gemigreerde abonnementen alleen wordt ook de vorige Azure Monitoring Agent verwijderd.

> [!NOTE]
> U kunt de automatische installatie van Microsoft Monitoring Agent overschrijven en uw eigen werkruimte gebruiken.  Zie [het beëindigen van de installatie en werkruimte maken van de automatische agent](#how-do-i-stop-the-automatic-agent-installation-and-workspace-creation) en [het gebruik van uw bestaande werkruimte](#how-can-i-use-my-existing-log-analytics-workspace).
>
>

De locatie van de werkruimte is gebaseerd op de locatie van de virtuele machine. Zie voor meer informatie, [gegevensbeveiliging](security-center-data-security.md). Als een abonnement virtuele machines van meerdere geolocations bevat, maakt u met Security Center meerdere werkruimten. Meerdere werkruimten worden gemaakt voor het onderhouden van regels voor privacy van gegevens.

> [!NOTE]
> Security Center verzameld beveiligingsgegevens van uw virtuele machines met behulp van de Azure Monitoring Agent vóór de migratie van platform, en gegevens zijn opgeslagen in uw opslagaccount. Na de migratie platform Security Center maakt gebruik van de Microsoft Monitoring Agent en de werkruimte voor het verzamelen en opslaan van de dezelfde gegevens. Het opslagaccount kan worden verwijderd na de migratie.  Security Center verwijdert u eerder geïnstalleerde Azure Monitoring-Agents ook na de migratie van platform.
>
>

### <a name="am-i-billed-for-log-analytics-or-oms-on-the-workspaces-created-by-security-center"></a>Ben ik gefactureerd voor logboekanalyse of OMS op de werkruimten die zijn gemaakt door Security Center?
Nee. Werkruimten die zijn gemaakt door Security Center terwijl geconfigureerd voor OMS per knooppunt financieel medewerkers, komen niet OMS worden kosten in rekening. Security Center facturering is altijd op basis van het beveiligingsbeleid van uw Security Center en de oplossingen die zijn geïnstalleerd op een werkruimte:

- **Gratis laag** – Security Center kunt u de oplossing 'SecurityCenterFree' in de standaardwerkruimte voor. U wordt niet gefactureerd voor de laag gratis.
- **Standard-laag** – Security Center kunt u de oplossing 'Security' in de standaardwerkruimte voor.

Zie voor meer informatie over prijzen [Security Center prijzen](https://azure.microsoft.com/pricing/details/security-center/). Wijzigingen in de gegevensopslag van de beveiliging en naar rato facturering vanaf juni 2017 heeft betrekking op de pagina met prijzen.

> [!NOTE]
> De prijscategorie van de werkruimten die zijn gemaakt door Security Center OMS heeft geen invloed op de facturering Security Center.
>
>

### <a name="what-qualifies-a-vm-for-automatic-provisioning-of-the-microsoft-monitoring-agent-installation"></a>Wat kwalificeren al een virtuele machine voor het automatisch inrichten van de installatie van Microsoft Monitoring Agent?
Windows- of Linux IaaS VM's in aanmerking als:

- De uitbreiding voor Microsoft Monitoring Agent is momenteel niet geïnstalleerd op de virtuele machine.
- De virtuele machine wordt uitgevoerd.
- De Windows- of Linux VM-Agent is geïnstalleerd.
- De virtuele machine niet wordt gebruikt als een apparaat, zoals web application firewall of de volgende generatie firewall.

### <a name="can-i-delete-the-default-workspaces-created-by-security-center"></a>Kan ik de Standaardwerkruimten die zijn gemaakt door Security Center verwijderen?
**Verwijderen van de standaardwerkruimte wordt niet aanbevolen.** De Standaardwerkruimten Security Center gebruikt voor het opslaan van beveiligingsgegevens van uw virtuele machines.  Als u een werkruimte verwijdert, Security Center is kan niet voor het verzamelen van deze gegevens en enkele aanbevelingen voor beveiliging en waarschuwingen zijn niet beschikbaar.

Als u wilt herstellen, verwijder Microsoft Monitoring Agent op de virtuele machines verbonden met de verwijderde werkruimte. Security Center de agent opnieuw installeren en nieuwe Standaardwerkruimten maken.

### <a name="how-can-i-use-my-existing-log-analytics-workspace"></a>Hoe kan ik mijn bestaande werkruimte voor logboekanalyse gebruiken?

U kunt een bestaande werkruimte voor logboekanalyse voor het opslaan van gegevens die worden verzameld door Security Center selecteren. Uw bestaande werkruimte voor logboekanalyse gebruiken:

- De werkruimte moet worden gekoppeld aan uw geselecteerde Azure-abonnement.
- Ten minste moet u leesmachtigingen hebben voor toegang tot de werkruimte.

Selecteer een bestaande werkruimte voor logboekanalyse:

1. Onder **beveiligingsbeleid – gegevensverzameling**, selecteer **gebruik een andere werkruimte**.

   ![Gebruik een andere werkruimte][5]

2. Selecteer een werkruimte voor het opslaan van verzamelde gegevens in de vervolgkeuzelijst.

   > [!NOTE]
   > In de vervolgkeuzelijst menu, worden alleen de werkruimten die u hebt toegang tot en in uw Azure-abonnement weergegeven.
   >
   >

3. Selecteer **Opslaan**.
4. Na het selecteren van **opslaan**, wordt u gevraagd als u reconfigure bewaakt virtuele machines wilt.

   - Selecteer **Nee** als u wilt dat de nieuwe werkruimte instellingen **toepassen op nieuwe virtuele machines**. De nieuwe werkruimte-instellingen zijn alleen van toepassing op nieuwe agentinstallaties; nieuwe gedetecteerde virtuele machines die u geen Microsoft Monitoring Agent geïnstalleerd hebt.
   - Selecteer **Ja** als u wilt dat de nieuwe werkruimte instellingen **toepassen op alle virtuele machines**. Bovendien wordt elke VM die is verbonden met een werkruimte gemaakt Beveiligingscentrum verbonden aan de nieuwe doelwerkruimte.

   > [!NOTE]
   > Als u Ja selecteert, moet u de workspace(s) gemaakt door Security Center totdat alle virtuele machines hebt opnieuw is verbonden met de nieuwe doelwerkruimte niet verwijderen. Deze bewerking mislukt als een werkruimte te vroeg is verwijderd.
   >
   >

   - Selecteer **annuleren** om de bewerking te annuleren.

      ![Bewaakte VM's configureren][6]

### <a name="what-if-the-microsoft-monitoring-agent-was-already-installed-as-an-extension-on-the-vm"></a>Wat gebeurt er als dat Microsoft Monitoring Agent al is geïnstalleerd als een uitbreiding op de virtuele machine?
Security Center wordt de bestaande verbindingen met werkruimten van de gebruiker niet overschreven. Security Center slaat beveiligingsgegevens van de virtuele machine in de werkruimte al verbonden. Security Center werkt de uitbreidingsversie voor de Azure-resource-id van de virtuele machine voor de ondersteuning van Security Center-gebruik.

### <a name="what-if-i-had-a-microsoft-monitoring-agent-installed-on-the-machine-but-not-as-an-extension"></a>Wat gebeurt er als ik een Microsoft Monitoring Agent geïnstalleerd op de computer, maar niet als een uitbreiding hebt gehad?
Als u Microsoft Monitoring Agent rechtstreeks op de VM (niet als een Azure-extensie) is geïnstalleerd, wordt Microsoft Monitoring Agent wordt niet geïnstalleerd door Security Center en beveiligingsbewaking is beperkt.

Zie voor meer informatie de volgende sectie [wat er gebeurt als een SCOM of OMS directe agent al is geïnstalleerd op mijn VM?](security-center-platform-migration-faq.md#what-happens-if-a-scom-or-oms-direct-agent-is-already-installed-on-my-vm)

### <a name="what-happens-if-a-scom-or-oms-direct-agent-is-already-installed-on-my-vm"></a>Wat gebeurt er als een SCOM of OMS agent aansturen op mijn VM is al geïnstalleerd?
Security Center kan niet van tevoren identificeren dat een agent is geïnstalleerd.  Security Center probeert de uitbreiding voor Microsoft Monitoring Agent installeren en mislukt als gevolg van de bestaande agent is geïnstalleerd.  Deze fout wordt voorkomen dat de verbindingsinstellingen van de agent naar de werkruimte te overschrijven en vermijdt multihoming maken.

> [!NOTE]
> De agent-versie is bijgewerkt naar de meest recente versie van de OMS-agent.  Dit geldt ook voor SCOM-gebruikers.
>
>

### <a name="what-is-the-impact-of-removing-these-extensions"></a>Wat zijn de gevolgen van het verwijderen van deze uitbreidingen?
Als u de extensie van Microsoft Monitoring verwijdert, kan geen Security Center voor het verzamelen van beveiligingsgegevens van de virtuele machine en een aantal aanbevelingen voor beveiliging en waarschuwingen zijn niet beschikbaar. Security Center bepaalt binnen 24 uur, dat de virtuele machine de extensie ontbreekt is. vervolgens wordt de extensie.

### <a name="how-do-i-stop-the-automatic-agent-installation-and-workspace-creation"></a>Hoe voorkom ik het automatische agent-installatie en werkruimte maken
U kunt uitschakelen automatische inrichting van uw abonnementen in het beveiligingsbeleid, maar dit wordt niet aanbevolen. Het uitschakelen van automatische inrichting limieten Security Center aanbevelingen en waarschuwingen. Automatische inrichting is vereist voor abonnementen op de Standard-prijscategorie. Voor het automatisch inrichten uitschakelen:

1. Als uw abonnement is geconfigureerd voor de prijscategorie Standard, opent u het beveiligingsbeleid voor dat abonnement en selecteer de **vrije** laag.

   ![Prijscategorie][1]

2. Vervolgens uitschakelen door het selecteren van een automatische inrichting **uit** op de **beveiligingsbeleid – gegevensverzameling** blade.
   ![Gegevensverzameling][2]

### <a name="should-i-opt-out-of-the-automatic-agent-installation-and-workspace-creation"></a>Moet ik kiezen uit de automatische agentinstallatie en -werkruimte maken?

> [!NOTE]
> Lees de secties [wat zijn de gevolgen van het uitschrijft?](#what-are-the-implications-of-opting-out-of-automatic-provisioning) en [aanbevolen stappen bij uitschrijft](#what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning) als u wilt afmelden automatische inrichting.
>
>

U wilt afmelden automatische inrichting als voor u het volgende geldt:

- Automatische agentinstallatie door Security Center geldt voor het hele abonnement.  U kunt automatische installatie niet toepassen op een subset van virtuele machines. Als er kritieke virtuele machines die met Microsoft Monitoring Agent kunnen niet worden geïnstalleerd, moet u ervoor kiezen niet automatisch inrichten.
- Installatie van de uitbreiding voor Microsoft Monitoring Agent-updates van de agent-versie. Dit geldt voor een direct-agent en een SCOM-agents. Als de agent is geïnstalleerd SCOM is versie 2012 is bijgewerkt, kunnen deze mogelijkheden voor beheerbaarheid zijn verloren wanneer de SCOM-server ook versie 2012 is. Automatische inrichting als de agent is geïnstalleerd SCOM 2012 versie van buiten-uitschakelen, moet u overwegen.
- Als u een aangepaste werkruimte buiten het abonnement (een gecentraliseerde werkruimte hebt) moet u ervoor kiezen automatische inrichting van buiten. U kunt handmatig de uitbreiding voor Microsoft Monitoring Agent te installeren en verbind deze uw werkruimte zonder Security Center de verbinding te overschrijven.
- Als u wilt voorkomen dat het maken van meerdere werkruimten per abonnement en u uw eigen aangepaste werkruimte binnen het abonnement hebt, hebt u twee opties:

   1. U kunt afmelden automatische inrichting. Na de migratie, de standaardwaarde instellen werkruimte-instellingen zoals beschreven in [hoe kan ik mijn bestaande werkruimte voor logboekanalyse gebruiken?](#how-can-i-use-my-existing-log-analytics-workspace)
   2. Of, kunt u de migratie is voltooid, wordt Microsoft Monitoring Agent worden geïnstalleerd op de virtuele machines, en de virtuele machines verbonden met de werkruimte te openen. Selecteer vervolgens uw eigen aangepaste werkruimte door in te stellen van de standaardinstelling van de werkruimte met inschrijving voor het opnieuw configureren van de reeds geïnstalleerde agents. Zie voor meer informatie [hoe kan ik mijn bestaande werkruimte voor logboekanalyse gebruiken?](#how-can-i-use-my-existing-log-analytics-workspace)

### <a name="what-are-the-implications-of-opting-out-of-automatic-provisioning"></a>Wat zijn de gevolgen van het automatische inrichting van buiten-uitschakelen?
Zodra de migratie is voltooid, kan geen Security Center voor het verzamelen van beveiligingsgegevens van de virtuele machine en een aantal aanbevelingen voor beveiliging en waarschuwingen zijn niet beschikbaar. Als u dit doet, moet u Microsoft Monitoring Agent handmatig installeren. Zie [aanbevolen stappen bij uitschrijft](#what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning).

### <a name="what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning"></a>Wat zijn de aanbevolen stappen buiten het automatisch inrichten uitschakelen?
U moet Microsoft Monitoring Agent handmatig installeren zodat Security Center kunt verzamelen van beveiligingsgegevens van uw VM's en aanbevelingen waarschuwingen en. Zie [verbinding maken met Windows-computers naar de Log Analytics-service in Azure](../log-analytics/log-analytics-windows-agent.md) voor hulp bij de installatie.

U kunt de agent verbinden met een bestaande aangepaste werkruimte of Security Center werkruimte hebt gemaakt. Als een aangepaste werkruimte beschikt niet over de 'Security' of 'SecurityCenterFree' oplossingen die zijn ingeschakeld, moet u een oplossing toepassen. Als u wilt toepassen, selecteert u de aangepaste werkruimte of het abonnement en toepassen van een prijscategorie via de **beveiligingsbeleid – prijscategorie** blade.

   ![Prijscategorie][1]

Security Center kunnen de juiste oplossing is in de werkruimte op basis van de geselecteerde prijscategorie.

### <a name="how-do-i-remove-oms-extensions-installed-by-security-center"></a>Hoe verwijder ik OMS-serverextensies geïnstalleerd door Security Center?
U kunt Microsoft Monitoring Agent handmatig verwijderen. Dit wordt niet aanbevolen omdat deze wordt beperkt door Security Center aanbevelingen en waarschuwingen.

> [!NOTE]
> Als gegevensverzameling is ingeschakeld, wordt Security Center de agent opnieuw nadat u deze verwijderen.  U moet uitschakelen van gegevensverzameling voordat de agent handmatig te verwijderen. Zie [hoe stopt het automatische agent-installatie en werkruimte maken?](#how-do-i-stop-the-automatic-agent-installation-and-workspace-creation?) voor instructies over het verzamelen van gegevens uitschakelen.
>
>

De agent handmatig verwijderen:

1.  Open in de portal **logboekanalyse**.
2.  Selecteer op de blade Log Analytics een werkruimte:
3.  Selecteer elke VM die u niet wilt bewaken en selecteer **Disconnect**.

   ![De agent verwijderen][3]

> [!NOTE]
> Als een Linux-VM is al een niet-extensie OMS-agent, evenals de agent verwijderen van de extensie verwijdert en de klant heeft opnieuw te installeren.
>
>

## <a name="existing-oms-customers"></a>Bestaande OMS-klanten

### <a name="does-security-center-override-any-existing-connections-between-vms-and-workspaces"></a>Wordt de bestaande verbindingen tussen virtuele machines en werkruimten overschreven door Security Center?
Als een virtuele machine al Microsoft Monitoring Agent geïnstalleerd als een Azure-extensie, wordt de bestaande verbinding in de werkruimte niet overschreven door Security Center. Security Center gebruikt in plaats daarvan de bestaande werkruimte.

Een oplossing Security Center is geïnstalleerd op de werkruimte als dat niet al aanwezig, en de oplossing wordt alleen toegepast op de relevante virtuele machines. Wanneer u een oplossing toevoegt, wordt het automatisch naar alle Windows- en Linux-agents verbonden met uw werkruimte voor logboekanalyse standaard geïmplementeerd. [Oplossing Targeting](../operations-management-suite/operations-management-suite-solution-targeting.md), namelijk een OMS-functie kunt u een bereik van toepassing op uw oplossingen.

Als u Microsoft Monitoring Agent rechtstreeks op de VM (niet als een Azure-extensie) is geïnstalleerd, wordt Microsoft Monitoring Agent wordt niet geïnstalleerd door Security Center en beveiligingsbewaking is beperkt.

### <a name="what-should-i-do-if-i-suspect-that-the-data-platform-migration-broke-the-connection-between-one-of-my-vms-and-my-workspace"></a>Wat moet ik doen als ik vermoedt dat de gegevensmigratie platform de verbinding tussen een van mijn VM's en mijn werkruimte heeft?
Dit vindt niet plaats. Als deze, klikt u vervolgens voordoet [maken van een aanvraag voor de ondersteuning van Azure](../azure-supportability/how-to-create-azure-support-request.md) en bevatten de volgende details:

- De Azure-resource-ID van de betrokken virtuele machine
- De Azure-resource-ID van de werkruimte op de extensie geconfigureerd voordat de verbinding verbroken is
- De agent en de versie die eerder is geïnstalleerd

### <a name="does-security-center-install-solutions-on-my-existing-oms-workspaces-what-are-the-billing-implications"></a>Security Center wordt geïnstalleerd oplossingen op mijn bestaande OMS-werkruimten? Wat zijn de gevolgen voor de facturering?
Als Beveiligingscentrum identificeert dat een virtuele machine al is verbonden met een werkruimte die u hebt gemaakt, kunt Security Center-oplossingen voor deze werkruimte volgens uw prijscategorie. De oplossingen worden alleen toegepast op de relevante Azure VM's [oplossing targeting](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solution-targeting), zodat de facturering hetzelfde is gebleven.

- **Gratis laag** – Security Center installeert de oplossing 'SecurityCenterFree' in de werkruimte. U wordt niet gefactureerd voor de laag gratis.
- **Standard-laag** – Security Center de beveiligingsoplossing installeert op de werkruimte.

   ![Oplossingen op standaardwerkruimte][4]

> [!NOTE]
> De oplossing 'Security' in logboekanalyse is de beveiliging en Audit oplossing in OMS.
>
>

### <a name="i-already-have-workspaces-in-my-environment-can-i-use-them-to-collect-security-data"></a>Ik heb al werkruimten in de omgeving, kan ik ermee voor het verzamelen van beveiligingsgegevens?
Als een virtuele machine al Microsoft Monitoring Agent geïnstalleerd als een Azure-extensie, wordt in Security Center maakt gebruik van de bestaande verbonden werkruimte. Een oplossing Security Center is geïnstalleerd op de werkruimte als dat niet al aanwezig, en de oplossing wordt alleen toegepast op de relevante virtuele machines via [oplossing doelen](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solution-targeting).

Security Center wordt Microsoft Monitoring Agent geïnstalleerd op virtuele machines, wordt de standaard workspace(s) gemaakt door Security Center gebruikt. Klanten wordt binnenkort configureren welke workspace(s) worden gebruikt.

### <a name="i-already-have-security-solution-on-my-workspaces-what-are-the-billing-implications"></a>Ik heb al beveiligingsoplossing op mijn werkruimten. Wat zijn de gevolgen voor de facturering?
De beveiliging en Audit-oplossing wordt gebruikt om Security Center Standard-laag functies inschakelen voor Azure Virtual machines. Als de beveiliging en Audit-oplossing is al geïnstalleerd op een werkruimte, maakt de bestaande oplossing voor het gebruik van Security Center. Er is geen wijziging in de facturering.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de migratie Security Center-platform.

- [Migratie van Azure Security Center-Platform](security-center-platform-migration.md)
- [Handleiding voor probleemoplossing voor Azure Security Center](security-center-troubleshooting-guide.md)

<!--Image references-->
[1]: ./media/security-center-platform-migration-faq/pricing-tier.png
[2]: ./media/security-center-platform-migration-faq/data-collection.png
[3]: ./media/security-center-platform-migration-faq/remove-the-agent.png
[4]: ./media/security-center-platform-migration-faq/solutions.png
[5]: ./media/security-center-platform-migration-faq/use-another-workspace.png
[6]: ./media/security-center-platform-migration-faq/reconfigure-monitored-vm.png

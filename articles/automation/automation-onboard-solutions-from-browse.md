---
title: Meer informatie over het onboarden updatebeheer, wijzigingen bijhouden en inventaris oplossingen voor meerdere virtuele machines in Azure Automation
description: Informatie over hoe zorgen voor onboarding een virtueel Azure-machine met updatebeheer, wijzigingen bijhouden en inventaris oplossingen die deel uitmaken van Azure Automation
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: article
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 83a768b82172b8736ea06bfed012309ac92734b7
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/20/2018
ms.locfileid: "46497957"
---
# <a name="enable-update-management-change-tracking-and-inventory-solutions-on-multiple-vms"></a>Inschakelen van updatebeheer, wijzigingen bijhouden en inventaris-oplossingen op meerdere virtuele machines

Azure Automation biedt oplossingen voor het beheren van beveiligingsupdates, het bijhouden van wijzigingen en inventaris wat er op uw computers is geïnstalleerd. Er zijn meerdere manieren om Onboarding van machines, u kunt Onboarding van de oplossing [van een virtuele machine](automation-onboard-solutions-from-vm.md), van uw [Automation-account](automation-onboard-solutions-from-automation-account.md), bij het bladeren door virtuele machines of door [runbook](automation-onboard-solutions.md). In dit artikel bevat informatie over onboarding deze oplossingen bij het bladeren door virtuele machines in Azure.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Aanmelden bij Azure op https://portal.azure.com

## <a name="enable-solutions"></a>Oplossingen inschakelen

In de Azure-portal, gaat u naar **virtuele machines**.

Met behulp van de selectievakjes uit, selecteer de virtuele machines die u vrijgeven met wijzigingen bijhouden en inventaris of updatebeheer wilt. Onboarding is beschikbaar voor maximaal drie verschillende resourcegroepen bevinden op een tijdstip.

![Lijst met virtuele machines](media/automation-onboard-solutions-from-browse/vmlist.png)
> [!TIP]
> Gebruik de besturingselementen voor filteren aan de lijst met virtuele machines wijzigen en klik vervolgens op het bovenste selectievakje in om te selecteren van alle virtuele machines in de lijst.

Klik in de opdrachtbalk op **Services** en selecteert u **bijhouden**, **voorraad**, of **updatebeheer**.

> [!NOTE]
> **Bijhouden van wijzigingen** en **voorraad** gebruikt het dezelfde oplossing als een is ingeschakeld de andere ook is ingeschakeld.

De volgende afbeelding is voor updatebeheer. Wijzigingen bijhouden en inventaris hebben dezelfde lay-out en gedrag.

De lijst met virtuele machines wordt gefilterd om alleen de virtuele machines die zich in hetzelfde abonnement en locatie weer te geven. Als uw virtuele machines zich in meer dan drie resourcegroepen, wordt de eerste drie resourcegroepen zijn geselecteerd.

### <a name="resource-group-limit"></a> Onboarding-beperkingen

Het aantal resourcegroepen die u voor onboarding gebruiken kunt wordt beperkt door de [limieten voor Resource Manager-implementatie](../azure-resource-manager/resource-manager-cross-resource-group-deployment.md). Resource Manager-implementaties, niet te verwarren met Update-implementaties zijn beperkt tot 5 resourcegroepen per implementatie. 2 van deze resourcegroepen zijn om te controleren of de integriteit van onboarding, gereserveerd voor het configureren van de Log Analytics-werkruimte, het Automation-account en de gerelateerde resources. U verlaat dit met 3 resourcegroepen selecteren voor implementatie.

Gebruik de besturingselementen voor filteren om te selecteren van virtuele machines uit verschillende abonnementen, locaties en resourcegroepen.

![Onboarden voor updatebeheer](media/automation-onboard-solutions-from-browse/onboardsolutions.png)

Controleer de keuzes voor de Log analytics-werkruimte en het Automation-account. Een bestaande werkruimte en een Automation-Account zijn standaard geselecteerd. Als u gebruiken een andere Log Analytics-werkruimte en een Automation-Account wilt, klikt u op **aangepaste** te selecteren uit de **aangepaste configuratie** pagina. Wanneer u ervoor een Log Analytics-werkruimte kiest, wordt een controle uitgevoerd om te bepalen als deze is gekoppeld aan een Automation-Account. Als een gekoppelde Automation-Account wordt gevonden, ziet u het volgende scherm. Wanneer u klaar bent, klikt u op **OK**.

![Selecteer een werkruimte en account](media/automation-onboard-solutions-from-browse/selectworkspaceandaccount.png)

Als de geselecteerde werkruimte is niet gekoppeld aan een Automation-Account, ziet u het volgende scherm. Selecteer een Automation-Account en klik op **OK** wanneer u klaar bent.

![Er is geen werkruimte](media/automation-onboard-solutions-from-browse/no-workspace.png)

Schakel het selectievakje naast een virtuele machine die u niet wilt inschakelen. Virtuele machines die kan niet worden ingeschakeld, zijn al uitgeschakeld.

Klik op **inschakelen** de oplossing in te schakelen. Het duurt maximaal 15 minuten om de oplossing in te schakelen.

## <a name="troubleshooting"></a>Problemen oplossen

Wanneer onboarding meerdere machines, kunnen er machines die als **kan niet worden ingeschakeld**. Er zijn verschillende redenen waarom sommige computers niet kunnen worden ingeschakeld. De volgende secties tonen mogelijke redenen voor de **kan niet worden ingeschakeld** staat op een virtuele machine wanneer u probeert te onboarden.

### <a name="vm-reports-to-a-different-workspace-workspacename--change-configuration-to-use-it-for-enabling"></a>Virtuele machine rapporteert aan een andere werkruimte: '\<workspaceName\>'.  De configuratie wijzigen om deze te gebruiken voor het inschakelen van

**Oorzaak**: deze fout ziet u dat de virtuele machine die u vrijgeven rapporten aan een andere werkruimte wilt.

**Oplossing**: klik op **gebruiken als de configuratie** te wijzigen van de betreffende Automation-Account en de Log Analytics-werkruimte.

### <a name="vm-reports-to-a-workspace-that-is-not-available-in-this-subscription"></a>Virtuele machine rapporteert aan een werkruimte die is niet beschikbaar in dit abonnement

**Oorzaak**: de werkruimte die de virtuele machine rapporteert aan:

* Is in een ander abonnement of
* Niet meer bestaat, of
* Is in een resourcegroep die u geen toegangsmachtigingen voor hebt

**Oplossing**: het automation-account gekoppeld aan de werkruimte waarin de virtuele machine en vrijgeven de virtuele machine gevonden door het veranderen van de scopeconfiguratie.

### <a name="vm-operating-system-version-or-distribution-is-not-supported"></a>De versie van de VM-besturingssysteem of distributie wordt niet ondersteund

**Oorzaak:** de oplossing wordt niet ondersteund voor alle Linux-distributies of alle versies van Windows.

**Oplossing:** verwijzen naar de [lijst van ondersteunde clients](automation-update-management.md#clients) voor de oplossing.

### <a name="classic-vms-cannot-be-enabled"></a>Klassieke virtuele machines kunnen niet worden ingeschakeld

**Oorzaak**: virtuele machines die gebruikmaken van het klassieke implementatiemodel worden niet ondersteund.

**Oplossing**: de virtuele machine migreren naar het resource manager-implementatiemodel. Zie voor meer informatie hoe u dit doet, [classic deployment model resources migreren](../virtual-machines/windows/migration-classic-resource-manager-overview.md).

### <a name="vm-is-stopped-deallocated"></a>Virtuele machine is gestopt. (toewijzing opgeheven)

**Oorzaak**: de virtuele machine in niet in een **met** staat.

**Oplossing**: om onboarding een virtuele machine naar een oplossing voor de virtuele machine moet worden uitgevoerd. Klik op de **VM starten** inlinekoppeling naar de virtuele machine starten zonder de pagina wordt genavigeerd.

## <a name="next-steps"></a>Volgende stappen

Nu dat de oplossing is ingeschakeld voor uw virtuele machines, gaat u naar het artikel van het overzicht updatebeheer voor informatie over het weergeven van de systeemupdate-evaluatie voor uw virtuele machines.

> [!div class="nextstepaction"]
> [Updates beheren - update-evaluatie bekijken](./automation-update-management.md#viewing-update-assessments)

Zelfstudies voor toevoeging van de oplossingen en het gebruik ervan:

* [Zelfstudie - Updates voor uw virtuele machine beheren](automation-tutorial-update-management.md)

* [Zelfstudie - software op een virtuele machine identificeren](automation-tutorial-installed-software.md)

* [Zelfstudie: problemen met wijzigingen op een virtuele machine oplossen](automation-tutorial-troubleshoot-changes.md)
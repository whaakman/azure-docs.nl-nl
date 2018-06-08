---
title: Meer informatie over hoe u ingebouwde updatebeheer, bijhouden en voorraad oplossingen voor meerdere virtuele machines in Azure Automation
description: Meer informatie over hoe om vrij te geven een Azure-virtuele machine met oplossingen voor beheer van updates, bijhouden en voorraad die deel uitmaken van een Azure Automation
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: article
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 6d0109b9043b48fbbbeeaccea6c798eaa547a056
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34839859"
---
# <a name="enable-update-management-change-tracking-and-inventory-solutions-on-multiple-vms"></a>Updatebeheer, bijhouden en oplossingen voor inventarisatie op meerdere virtuele machines inschakelen

Azure Automation biedt oplossingen voor het besturingssysteem beveiligingsupdates beheren, het bijhouden van wijzigingen en wat wordt geïnstalleerd op computers in uw inventaris. Er zijn meerdere manieren om vrijgeven machines, kunt u vrijgeven de oplossing [van een virtuele machine](automation-onboard-solutions-from-vm.md), van uw [Automation-account](automation-onboard-solutions-from-automation-account.md), bij het bladeren door virtuele machines of door de [runbook](automation-onboard-solutions.md). In dit artikel bevat informatie over de voorbereiding deze oplossingen bij het bladeren door virtuele machines in Azure.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Aanmelden bij Azure op https://portal.azure.com

## <a name="enable-solutions"></a>Oplossingen inschakelen

Navigeer in de Azure-portal naar **virtuele machines**.

Met behulp van de selectievakjes uit, selecteer de virtuele machines die u vrijgeven met bijhouden en inventarisatie of updatebeheer wilt. Onboarding is beschikbaar voor maximaal drie verschillende resourcegroepen tegelijk.

![Lijst met VM 's](media/automation-onboard-solutions-from-browse/vmlist.png)
> [!TIP]
> Gebruik de filterbesturingselementen de lijst met virtuele machines wijzigen en klik vervolgens op het bovenste selectievakje om te selecteren van alle virtuele machines in de lijst.

Klik in de opdrachtbalk op **Services** en selecteert u **bijhouden**, **inventaris**, of **updatebeheer**.

> [!NOTE]
> **Bijhouden van wijzigingen** en **inventaris** dezelfde oplossing gebruiken wanneer een is ingeschakeld de andere ook is ingeschakeld.

De volgende afbeelding is om updates te beheren. Bijhouden van wijzigingen en inventaris hebben dezelfde indeling en gedrag.

De lijst met virtuele machines is gefilterd zodat alleen de virtuele machines die zich in hetzelfde abonnement en locatie. Als uw virtuele machines zich in meer dan drie resourcegroepen, worden de eerste drie bronnengroepen geselecteerd.

Gebruik de filterbesturingselementen aan virtuele machines selecteren die uit verschillende abonnementen behoren, locaties en resourcegroepen.

![Vrijgeven voor updatebeheer](media/automation-onboard-solutions-from-browse/onboardsolutions.png)

Controleer de keuzes voor de Log analytics-werkruimte en de Automation-account. Een nieuwe werkruimte en de Automation-Account zijn standaard geselecteerd. Als u een bestaande werkruimte voor logboekanalyse en de Automation-Account hebt u wilt gebruiken, klikt u op **wijzigen** te selecteren van de **configuratie** pagina. Klik op **Opslaan** als u klaar bent.

![Werkruimte en account selecteren](media/automation-onboard-solutions-from-browse/selectworkspaceandaccount.png)

Deselecteer het selectievakje naast elke virtuele machine die u niet wilt inschakelen. Virtuele machines die kan niet worden ingeschakeld, zijn al uitgeschakeld.

Klik op **inschakelen** zodat de oplossing. Het duurt maximaal 15 minuten om de oplossing in te schakelen.

## <a name="troubleshooting"></a>Problemen oplossen

Als voorbereiding op meerdere machines mogelijk zijn er machines die worden weergegeven als **kan niet worden ingeschakeld**. Er zijn verschillende redenen waarom sommige computers niet kunnen worden ingeschakeld. De volgende secties worden enkele mogelijke redenen voor de **kan niet worden ingeschakeld** status op een virtuele machine bij een poging om vrij te geven.

### <a name="vm-reports-to-a-different-workspace-workspacename--change-configuration-to-use-it-for-enabling"></a>VM-rapporten naar een andere werkruimte: '\<workspaceName\>'.  Configuratie gebruiken voor het inschakelen van wijzigen

**Oorzaak**: deze fout ziet u dat de virtuele machine die u vrijgeven rapporten naar een andere werkruimte wilt.

**Oplossing**: Wijzig de werkruimte die u ontwikkelt voor door te klikken in de inline-koppeling of klik op **wijzigen**. U kunt ook de volgende scripts gebruiken om te wijzigen van de werkruimte in de bedoeld is voor elke computer.

### <a name="vm-reports-to-a-workspace-that-is-not-available-in-this-subscription"></a>Rapporten van de virtuele machine naar een werkruimte die niet beschikbaar in dit abonnement

**Oorzaak**: de werkruimte die wordt doorgegeven aan de virtuele machine:

* Bevindt zich in een ander abonnement of
* Niet meer bestaat, of
* Bevindt zich in een resourcegroep die u geen toegangsmachtigingen voor hebt

**Oplossing**: het automation-account gekoppeld aan de werkruimte die wordt doorgegeven aan de virtuele machine en vrijgeven de virtuele machine gevonden door het wijzigen van de configuratie van het bereik.

### <a name="vm-operating-system-version-or-distribution-is-not-supported"></a>De versie van de VM-besturingssysteem of distributie wordt niet ondersteund.

**Oorzaak:** de oplossing wordt niet ondersteund voor alle Linux-distributies of alle versies van Windows.

**Oplossing:** verwijzen naar de [lijst van ondersteunde clients](automation-update-management.md#clients) voor de oplossing.

### <a name="classic-vms-cannot-be-enabled"></a>Klassieke virtuele machines kunnen niet worden ingeschakeld.

**Oorzaak**: virtuele machines die gebruikmaken van het klassieke implementatiemodel worden niet ondersteund.

**Oplossing**: Migreer de virtuele machine naar het resource manager-implementatiemodel. Zie voor meer informatie over hoe u dit doet, [classic deployment model bronnen migreren](../virtual-machines/windows/migration-classic-resource-manager-overview.md).

### <a name="vm-is-stopped-deallocated"></a>Virtuele machine is gestopt. (toewijzing opgeheven)

**Oorzaak**: de virtuele machine in niet in een **met** status.

**Oplossing**: om vrijgeven een virtuele machine naar een oplossing voor de virtuele machine moet worden uitgevoerd. Klik op de **VM starten** inline-koppeling naar het starten van de virtuele machine zonder verlaat de pagina.

## <a name="next-steps"></a>Volgende stappen

Nu dat de oplossing voor uw virtuele machines is ingeschakeld, gaat u naar het artikel bijwerken Management overzicht voor informatie over het weergeven van de beoordeling van de update voor uw machines.

> [!div class="nextstepaction"]
> [Updatebeheer - weergave update-evaluatie](./automation-update-management.md#viewing-update-assessments)

Toevoeging zelfstudies over de oplossingen en hoe ze te gebruiken:

* [Zelfstudie - Updates voor uw virtuele machine beheren](automation-tutorial-update-management.md)

* [Zelfstudie - software op een virtuele machine identificeren](automation-tutorial-installed-software.md)

* [Zelfstudie - wijzigingen op een virtuele machine oplossen](automation-tutorial-troubleshoot-changes.md)
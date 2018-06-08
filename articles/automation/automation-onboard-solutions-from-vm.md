---
title: Meer informatie over hoe u ingebouwde updatebeheer, bijhouden en voorraad oplossingen van een virtuele Machine van Azure
description: Meer informatie over hoe om vrij te geven een Azure-virtuele machine met oplossingen voor beheer van updates, bijhouden en voorraad die deel uitmaken van een Azure Automation
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: conceptual
ms.service: automation
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 675aebf35a6bee6e4cc4fd884204edb5bae4b848
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34830558"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions-from-an-azure-virtual-machine"></a>Ingebouwde updatebeheer, bijhouden en voorraad oplossingen van een virtuele Machine van Azure

Azure Automation biedt oplossingen voor het besturingssysteem beveiligingsupdates beheren, het bijhouden van wijzigingen en wat wordt geïnstalleerd op computers in uw inventaris. Er zijn meerdere manieren om vrijgeven machines, kunt u vrijgeven de oplossing van een virtuele machine, [van uw Automation-account](automation-onboard-solutions-from-automation-account.md), [van meerdere machines Bladeren](automation-onboard-solutions-from-browse.md), of door [runbook](automation-onboard-solutions.md). In dit artikel bevat informatie over de voorbereiding deze oplossingen van een virtuele Machine van Azure.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Aanmelden bij Azure op https://portal.azure.com

## <a name="enable-the-solutions"></a>De oplossingen inschakelen

Navigeer naar een bestaande virtuele machine en selecteer **updatebeheer**, **inventaris**, of **bijhouden** onder **OPERATIONS**.

Om in te schakelen de oplossing voor de virtuele machine alleen Zorg ervoor dat de **inschakelen voor deze virtuele machine** keuzerondje is ingeschakeld, vrijgeven meerdere machines aan de oplossing selecteren **inschakelen voor virtuele machines in dit abonnement** en klik op **Machines inschakelen Schakel**. Zie, [vrijgeven updatebeheer, bijhouden en voorraad oplossingen](automation-onboard-solutions-from-automation-account.md) om weer te geven van de stappen voor het vrijgeven meerdere machines tegelijk.

Kies de Log Analytics-werkruimte en het Automation-account en klik op **Inschakelen** om de oplossing in te schakelen. Het duurt maximaal 15 minuten om de oplossing in te schakelen.

![Onboarding van update-oplossing uitvoeren](media/automation-onboard-solutions-from-vm/onboard-solution.png)

Navigeer naar de andere oplossingen en klikt u op **inschakelen**, wordt de logboekanalyse en vervolgkeuzelijsten zijn uitgeschakeld, omdat ze dezelfde werkruimte en de automation-account als de eerder ingeschakelde oplossing gebruiken Automation-account.

> [!NOTE]
> **Bijhouden van wijzigingen** en **inventaris** dezelfde oplossing gebruiken wanneer een is ingeschakeld de andere ook is ingeschakeld.

## <a name="scope-configuration"></a>Scopeconfiguratie

Elke oplossing gebruikt de configuratie van een Scope in de werkruimte toe te passen van de computers die de oplossing. De configuratie van het is een groep van een of meer opgeslagen zoekopdrachten die wordt gebruikt voor het bereik van de oplossing op bepaalde computers beperken. Voor toegang tot de Scope-configuraties in uw Automation-account onder **verwante RESOURCES**, selecteer **werkruimte** vervolgens in de werkruimte **WERKRUIMTE GEGEVENSBRONNEN**, Selecteer **bereik configuraties**.

Als de geselecteerde werkruimte beschikt niet over de oplossingen voor beheer van updates of wijzigingen bijhouden nog, de volgende configuraties voor de scope gemaakt:

* **MicrosoftDefaultScopeConfig ChangeTracking**

* **MicrosoftDefaultScopeConfig-Updates**

Als het al is de oplossing voor de geselecteerde werkruimte. De oplossing niet opnieuw is geïmplementeerd en de configuratie van het niet is toegevoegd.

Klik op de weglatingstekens (...) op een van de configuraties en selecteer **bewerken**. Op de **bewerken scopeconfiguratie** pagina **computergroepen Selecteer** openen de **computergroepen** pagina. Deze pagina bevat de opgeslagen zoekopdrachten die worden gebruikt voor het maken van de configuratie van de Scope.

## <a name="saved-searches"></a>Opgeslagen zoekopdrachten

Wanneer een computer wordt toegevoegd aan het Update-beheer of de oplossingen bijhouden en inventaris, worden ze toegevoegd aan een van twee opgeslagen zoekopdrachten in uw werkruimte. Deze opgeslagen zoekopdrachten zijn query's die de computers die bedoeld zijn voor deze oplossingen bevatten.

Navigeer naar uw werkruimte en selecteer **opgeslagen zoekopdrachten** onder **algemene**. In de volgende tabel ziet u de twee opgeslagen zoekopdrachten die wordt gebruikt door deze oplossingen:

|Naam     |Category  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Updates        | Updates__MicrosoftDefaultComputerGroup         |

Selecteer een opgeslagen zoekopdracht om de query die wordt gebruikt voor het vullen van de groep weer te geven. De volgende afbeelding ziet u de query en de resultaten.

![Opgeslagen zoekopdrachten](media/automation-onboard-solutions-from-vm/logsearch.png)

## <a name="next-steps"></a>Volgende stappen

Blijven de zelfstudies over de oplossingen voor meer informatie over het gebruik ervan.

* [Zelfstudie - Updates voor uw virtuele machine beheren](automation-tutorial-update-management.md)

* [Zelfstudie - software op een virtuele machine identificeren](automation-tutorial-installed-software.md)

* [Zelfstudie - wijzigingen op een virtuele machine oplossen](automation-tutorial-troubleshoot-changes.md)

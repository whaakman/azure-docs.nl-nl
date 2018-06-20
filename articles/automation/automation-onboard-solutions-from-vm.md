---
title: Ingebouwde updatebeheer, bijhouden en voorraad oplossingen van een Azure VM
description: Meer informatie over hoe naar vrijgeven een virtuele machine van Azure met oplossingen voor beheer van updates, bijhouden en voorraad die deel uitmaken van Azure Automation.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: conceptual
ms.service: automation
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: f270b2ccea51e83bc6475051b8667bf73d7fd717
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36221509"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions-from-an-azure-virtual-machine"></a>Vrijgeven updatebeheer, bijhouden en voorraad oplossingen van een virtuele machine van Azure

Azure Automation biedt oplossingen zodat u beveiligingsupdates besturingssysteem beheren, het bijhouden van wijzigingen en wat wordt geïnstalleerd op computers in uw inventaris. Er zijn meerdere manieren om vrijgeven machines. U kunt vrijgeven de oplossing van een virtuele machine, [van uw Automation-account](automation-onboard-solutions-from-automation-account.md), [van meerdere machines Bladeren](automation-onboard-solutions-from-browse.md), of met behulp van een [runbook](automation-onboard-solutions.md). In dit artikel bevat informatie over de voorbereiding deze oplossingen van een virtuele machine van Azure.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="enable-the-solutions"></a>De oplossingen inschakelen

Ga naar een bestaande virtuele machine. Onder **OPERATIONS**, selecteer **updatebeheer**, **inventaris**, of **bijhouden van wijzigingen**.

Zorg ervoor dat zodat de oplossing voor de virtuele machine alleen **inschakelen voor deze virtuele machine** is geselecteerd. Vrijgeven meerdere machines aan de oplossing, selecteert u **inschakelen voor virtuele machines in dit abonnement**, en selecteer vervolgens **machines inschakelen Schakel**. Voor meer informatie over hoe vrijgeven meerdere machines tegelijk, Zie [vrijgeven updatebeheer, bijhouden en voorraad oplossingen](automation-onboard-solutions-from-automation-account.md).

Selecteer de werkruimte voor logboekanalyse voor Azure en de Automation-account en selecteer vervolgens **inschakelen** zodat de oplossing. Het duurt maximaal 15 minuten om de oplossing in te schakelen.

![Ingebouwde het beheersysteem voor Update](media/automation-onboard-solutions-from-vm/onboard-solution.png)

Ga naar de andere oplossingen en selecteer vervolgens **inschakelen**. De Log Analytics en Automation-account vervolgkeuzelijsten zijn uitgeschakeld omdat deze oplossingen dezelfde werkruimte en Automation-account als de eerder ingeschakelde oplossing gebruiken.

> [!NOTE]
> **Bijhouden van wijzigingen** en **inventaris** dezelfde oplossing gebruiken. Wanneer een van deze oplossingen is ingeschakeld, is ook de andere ingeschakeld.

## <a name="scope-configuration"></a>Configuratie van de scope

Elke oplossing maakt gebruik van de configuratie van een scope in de werkruimte toe te passen van de computers die de oplossing. De configuratie van het is een groep van een of meer opgeslagen zoekopdrachten die worden gebruikt voor het bereik van de oplossing op bepaalde computers beperken. Toegang krijgen tot de scope-configuraties in uw Automation-account onder **verwante RESOURCES**, selecteer **werkruimte**. In de werkruimte onder **WERKRUIMTE GEGEVENSBRONNEN**, selecteer **bereik configuraties**.

Als de geselecteerde werkruimte nog niet over de oplossingen voor beheer van updates of wijzigingen bijhouden, worden de volgende configuraties voor de scope gemaakt:

* **MicrosoftDefaultScopeConfig ChangeTracking**

* **MicrosoftDefaultScopeConfig-Updates**

Als de geselecteerde werkruimte al de oplossing is, de oplossing is niet geïmplementeerd en de configuratie van het is niet toegevoegd.

Selecteer de weglatingstekens (**...** ) op een van de configuraties en selecteer vervolgens **bewerken**. In de **bewerken scopeconfiguratie** deelvenster **computergroepen Selecteer**. De **computergroepen** deelvenster ziet u de opgeslagen zoekopdrachten die worden gebruikt voor het maken van de configuratie van de scope.

## <a name="saved-searches"></a>Opgeslagen zoekopdrachten

Wanneer een computer wordt toegevoegd aan de Update Management, bijhouden of inventaris oplossingen, wordt de computer toegevoegd aan een van twee opgeslagen zoekopdrachten in uw werkruimte. Opgeslagen zoekopdrachten zijn query's die de computers die bedoeld zijn voor deze oplossingen bevatten.

Ga naar uw werkruimte. Onder **algemene**, selecteer **opgeslagen zoekopdrachten**. De twee opgeslagen zoekopdrachten die worden gebruikt door deze oplossingen worden weergegeven in de volgende tabel:

|Naam     |Category  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Updates        | Updates__MicrosoftDefaultComputerGroup         |

Selecteer een van de opgeslagen zoekopdrachten om de query die wordt gebruikt voor het vullen van de groep weer te geven. De volgende afbeelding ziet u de query en de resultaten:

![Opgeslagen zoekopdrachten](media/automation-onboard-solutions-from-vm/logsearch.png)

## <a name="next-steps"></a>Volgende stappen

Blijven de zelfstudies voor de oplossingen voor meer informatie over het gebruik ervan:

* [Zelfstudie - updates voor uw virtuele machine beheren](automation-tutorial-update-management.md)
* [Zelfstudie - software op een virtuele machine identificeren](automation-tutorial-installed-software.md)
* [Zelfstudie - wijzigingen op een virtuele machine oplossen](automation-tutorial-troubleshoot-changes.md)

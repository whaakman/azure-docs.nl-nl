---
title: Meer informatie over hoe u ingebouwde updatebeheer, bijhouden en voorraad oplossingen in Azure Automation
description: Meer informatie over hoe om vrij te geven een Azure-virtuele machine met oplossingen voor beheer van updates, bijhouden en voorraad die deel uitmaken van een Azure Automation
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 2f5d664b660d43e61dba46d13aff1ced796de884
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions"></a>Ingebouwde updatebeheer, bijhouden en voorraad oplossingen

Azure Automation biedt oplossingen voor het besturingssysteem beveiligingsupdates beheren, het bijhouden van wijzigingen en wat wordt geïnstalleerd op computers in uw inventaris. Er zijn meerdere manieren om vrijgeven machines, kunt u vrijgeven de oplossing [van een virtuele machine](automation-onboard-solutions-from-vm.md)van uw Automation-account of door [runbook](automation-onboard-solutions.md). In dit artikel bevat informatie over de voorbereiding deze oplossingen van uw Automation-account.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Aanmelden bij Azure op https://portal.azure.com

## <a name="enable-solutions"></a>Oplossingen inschakelen

Navigeer naar uw Automation-account en selecteer **inventaris** of **bijhouden** onder **CONFIGURATIEBEHEER**.

Kies de Log analytics-werkruimte en de Automation-account en klikt u op **inschakelen** zodat de oplossing. Het duurt maximaal 15 minuten om de oplossing in te schakelen.

![Ingebouwde inventaris-oplossing](media/automation-onboard-solutions-from-automation-account/onboardsolutions.png)

De oplossing Wijzigingen bijhouden en Inventaris biedt de mogelijkheid om [Wijzigingen bijhouden](automation-vm-change-tracking.md) en [Inventaris](automation-vm-inventory.md) uit te voeren op uw virtuele machines. In deze stap schakelt u de oplossing in op een virtuele machine.

Wanneer wordt gemeld dat de onboarding van de oplossing Wijzigingen bijhouden en Inventarisatie is voltooid, klikt u op **Updatebeheer** onder **CONFIGURATIEBEHEER**.

Met de oplossing Updatebeheer kunt u updates en patches voor uw Azure-VM's beheren. U kunt de status van beschikbare updates beoordelen, de installatie van vereiste updates plannen en de implementatieresultaten bekijken om te controleren of updates correct zijn toegepast op de VM. Deze actie wordt de oplossing voor uw virtuele machine ingeschakeld.

Selecteer **updatebeheer** onder **UPDATEBEHEER**. De geselecteerde Log analytics-werkruimte is dezelfde werkruimte als in de vorige stap. Klik op **Inschakelen** om de onboarding van de oplossing Updatebeheer uit te voeren. Het duurt maximaal 15 minuten om de oplossing in te schakelen.

![Ingebouwde update-oplossing](media/automation-onboard-solutions-from-automation-account/onboardsolutions2.png)

## <a name="scope-configuration"></a>Scopeconfiguratie

Elke oplossing gebruikt de configuratie van een Scope in de werkruimte toe te passen van de computers die de oplossing. De configuratie van het is een groep van een of meer opgeslagen zoekopdrachten die wordt gebruikt voor het bereik van de oplossing op bepaalde computers beperken. Voor toegang tot de Scope-configuraties in uw Automation-account onder **verwante RESOURCES**, selecteer **werkruimte**. Klik in de werkruimte **WERKRUIMTE GEGEVENSBRONNEN**, selecteer **bereik configuraties**.

Als de geselecteerde werkruimte beschikt niet over de oplossingen voor beheer van updates of wijzigingen bijhouden nog, de volgende configuraties voor de scope gemaakt:

* **MicrosoftDefaultScopeConfig ChangeTracking**

* **MicrosoftDefaultScopeConfig-Updates**

Als het al is de oplossing voor de geselecteerde werkruimte. De oplossing niet opnieuw is geïmplementeerd en de configuratie van het niet is toegevoegd.

## <a name="saved-searches"></a>Opgeslagen zoekopdrachten

Wanneer een computer wordt toegevoegd aan het Update-beheer of de oplossingen bijhouden en inventaris, worden ze toegevoegd aan een van twee opgeslagen zoekopdrachten in uw werkruimte. Deze opgeslagen zoekopdrachten zijn query's die de computers die bedoeld zijn voor deze oplossingen bevatten.

Navigeer naar uw Automation-account en selecteer **opgeslagen zoekopdrachten** onder **algemene**. In de volgende tabel ziet u de twee opgeslagen zoekopdrachten die wordt gebruikt door deze oplossingen:

|Naam     |Category  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Updates        | Updates__MicrosoftDefaultComputerGroup         |

Selecteer een opgeslagen zoekopdracht om de query die wordt gebruikt voor het vullen van de groep weer te geven. De volgende afbeelding ziet u de query en de resultaten:

![Opgeslagen zoekopdrachten](media/automation-onboard-solutions-from-automation-account/savedsearch.png)

## <a name="onboard-an-azure-machine"></a>Ingebouwde een machine van Azure

Van uw Automation-account selecteren **inventaris** of **bijhouden** onder **CONFIGURATIEBEHEER**, of **updatebeheer** onder **UPDATEBEHEER**.

Klik op **+ Azure VM toevoegen**, selecteert u een virtuele machine uit de lijst. Op de **updatebeheer** pagina, klikt u op **inschakelen**. Hiermee wordt de huidige virtuele machine toegevoegd aan de computergroep opgeslagen zoekactie voor de oplossing.

## <a name="onboard-a-non-azure-machine"></a>Onboarding van een niet-Azure-machine

Van uw Automation-account selecteren **inventaris** of **bijhouden** onder **CONFIGURATIEBEHEER**, of **updatebeheer** onder **UPDATEBEHEER**.

Klik op **toevoegen dan de Azure-machine**. Hiermee opent u een nieuw browservenster met de instructies voor het installeren en configureren van Microsoft Monitoring Agent op de machine, zodat de machine kunt beginnen met rapporteren aan de oplossing. Als u zich voorbereiden op een machine die momenteel wordt beheerd door System Center Operations Manager, een nieuwe agent is niet vereist, de werkruimtegegevens is ingevoerd in de bestaande agent.

## <a name="onboard-machines-in-the-workspace"></a>Ingebouwde machines in de werkruimte

Van uw Automation-account selecteren **inventaris** of **bijhouden** onder **CONFIGURATIEBEHEER**, of **updatebeheer** onder **UPDATEBEHEER**.

Selecteer **machines beheren**. Hiermee opent u de **Machines beheren** pagina. Deze pagina kunt u de oplossing op een bepaalde set van alle beschikbare machines machines inschakelen of inschakelen van de oplossing voor alle huidige machines en inschakelen op alle toekomstige machines.

![Opgeslagen zoekopdrachten](media/automation-onboard-solutions-from-automation-account/managemachines.png)

### <a name="selected-machines"></a>Geselecteerde machines

Als u de oplossing voor een of meer machines selecteert **inschakelen op de geselecteerde computers** en klik op **toevoegen** naast elke machine die u wilt toevoegen aan de oplossing. Deze taak worden de namen van de geselecteerde machine toegevoegd aan de computergroep opgeslagen zoekquery's voor de oplossing.

### <a name="all-available-machines"></a>Alle beschikbare machines

Als u de oplossing voor alle beschikbare machines selecteert **inschakelen op alle beschikbare machines**. Hiermee wordt het besturingselement om toe te voegen machines afzonderlijk uitgeschakeld. Deze taak wordt de namen van de computers die rapporteren aan de werkruimte aan de computergroep opgeslagen zoekquery's toegevoegd.

### <a name="all-available-and-future-machines"></a>Alle beschikbare en toekomstige machines

Als u de oplossing voor alle beschikbare machines en alle toekomstige machines selecteert **inschakelen op alle beschikbare en toekomstige machines**. Deze optie worden de opgeslagen zoekopdrachten en Scope-configuraties verwijderd uit de werkruimte. Hiermee opent u de oplossing voor alle Azure en niet-Azure-machines die aan de werkruimte rapporteren.

## <a name="next-steps"></a>Volgende stappen

Blijven de zelfstudies over de oplossingen voor meer informatie over het gebruik ervan.

* [Zelfstudie - Updates voor uw virtuele machine beheren](automation-tutorial-update-management.md)

* [Zelfstudie - software op een virtuele machine identificeren](automation-tutorial-installed-software.md)

* [Zelfstudie - wijzigingen op een virtuele machine oplossen](automation-tutorial-troubleshoot-changes.md)
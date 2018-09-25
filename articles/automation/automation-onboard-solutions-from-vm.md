---
title: Onboarding updatebeheer, wijzigingen bijhouden en inventaris oplossingen van een Azure-VM
description: Informatie over hoe om te onboarden een Azure-machine met updatebeheer, wijzigingen bijhouden en inventaris oplossingen die deel uitmaken van Azure Automation.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: conceptual
ms.service: automation
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 039e2d3c70493868ca2f79e89fc82d8970ec6865
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47032387"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions-from-an-azure-virtual-machine"></a>Onboarding updatebeheer, wijzigingen bijhouden en inventaris oplossingen van een virtuele machine van Azure

Azure Automation biedt oplossingen om u te helpen beveiligingsupdates beheren, het bijhouden van wijzigingen en inventaris wat op uw computers geïnstalleerd. Er zijn meerdere manieren om Onboarding van machines. U kunt Onboarding van de oplossing van een virtuele machine, [via uw Automation-account](automation-onboard-solutions-from-automation-account.md), [van meerdere machines Bladeren](automation-onboard-solutions-from-browse.md), of met behulp van een [runbook](automation-onboard-solutions.md). In dit artikel bevat informatie over onboarding via een virtuele machine van Azure.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="enable-the-solutions"></a>De oplossingen inschakelen

Ga naar een bestaande virtuele machine. Onder **OPERATIONS**, selecteer **updatebeheer**, **voorraad**, of **wijzigingen bijhouden**.

Zorg ervoor dat zodat de oplossing voor de virtuele machine alleen **inschakelen voor deze virtuele machine** is geselecteerd. Onboarding meerdere virtuele machines aan de oplossing selecteert **inschakelen voor virtuele machines in dit abonnement**, en selecteer vervolgens **Selecteer machines om in te schakelen**. Voor meer informatie over hoe onboarding meerdere virtuele machines in één keer, Zie [onboarding updatebeheer, wijzigingen bijhouden en inventaris oplossingen](automation-onboard-solutions-from-automation-account.md).

Selecteer de Azure Log Analytics-werkruimte en het Automation-account en selecteer vervolgens **inschakelen** de oplossing in te schakelen. Het duurt maximaal 15 minuten om de oplossing in te schakelen.

![Onboard de oplossing Update Management](media/automation-onboard-solutions-from-vm/onboard-solution.png)

Ga naar de andere oplossingen, en selecteer vervolgens **inschakelen**. De Log Analytics en Automation-account vervolgkeuzelijsten zijn uitgeschakeld omdat deze oplossingen gebruikt u de dezelfde werkruimte en het Automation-account als de eerder ingeschakelde oplossing.

> [!NOTE]
> **Bijhouden van wijzigingen** en **voorraad** dezelfde oplossing gebruiken. Wanneer een van deze oplossingen is ingeschakeld, wordt ook de andere ingeschakeld.

## <a name="scope-configuration"></a>Scopeconfiguratie

Elke oplossing maakt gebruik van de configuratie van een bereik in de werkruimte om u te richten op de computers die aan de oplossing. De scopeconfiguratie is een groep van een of meer opgeslagen zoekopdrachten die worden gebruikt voor het bereik van de oplossing op specifieke computers beperken. Toegang krijgen tot de bereik-configuraties, in uw Automation-account onder **gerelateerde RESOURCES**, selecteer **werkruimte**. In de werkruimte onder **GEGEVENSBRONNEN voor WERKRUIMTE**, selecteer **Scopeconfiguraties**.

Als de geselecteerde werkruimte niet al de oplossingen voor beheer van updates of wijzigingen bijhouden, worden de volgende scopeconfiguraties gemaakt:

* **MicrosoftDefaultScopeConfig-voor wijzigingen bijhouden**

* **MicrosoftDefaultScopeConfig-Updates**

Als de geselecteerde werkruimte is al de oplossing, de oplossing niet is geïmplementeerd en configuratie van het bereik is niet toegevoegd.

Selecteer het beletselteken (**...** ) op een van de configuraties en selecteer vervolgens **bewerken**. In de **scopeconfiguratie bewerken** venster **computergroepen selecteren**. De **computergroepen** deelvenster ziet u de opgeslagen zoekopdrachten die worden gebruikt voor het maken van de configuratie van het bereik.

## <a name="saved-searches"></a>Opgeslagen zoekopdrachten

Wanneer een computer wordt toegevoegd aan het updatebeheer, wijzigingen bijhouden of inventaris oplossingen, wordt de computer toegevoegd aan een van twee opgeslagen zoekopdrachten in uw werkruimte. De opgeslagen zoekopdrachten zijn query's die de computers die zijn gericht op deze oplossingen bevatten.

Ga naar uw werkruimte. Onder **algemene**, selecteer **opgeslagen zoekopdrachten**. De twee opgeslagen zoekopdrachten die worden gebruikt door deze oplossingen worden weergegeven in de volgende tabel:

|Naam     |Categorie  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  Voor wijzigingen bijhouden       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Updates        | Updates__MicrosoftDefaultComputerGroup         |

Selecteer een van de opgeslagen zoekopdrachten om de query die wordt gebruikt voor het vullen van de groep weer te geven. De volgende afbeelding ziet u de query en de resultaten:

![Opgeslagen zoekopdrachten](media/automation-onboard-solutions-from-vm/logsearch.png)

## <a name="unlink-workspace"></a>Werkruimte ontkoppelen

De volgende oplossingen zijn afhankelijk van een Log Analytics-werkruimte:

* [Updatebeheer](automation-update-management.md)
* [Tracering wijzigen](automation-change-tracking.md)
* [VM's starten/stoppen buiten kantooruren](automation-solution-vm-management.md)

Als u besluit dat u niet meer wilt integreren van uw Automation-account met Log Analytics, kunt u uw account rechtstreeks vanuit de Azure portal kunt ontkoppelen.  Voordat u doorgaat, moet u eerst te verwijderen van de oplossingen die eerder vermeld, anders wordt dit proces zal worden voorkomen dat u doorgaat. Lees het artikel voor de desbetreffende oplossing die u hebt geïmporteerd voor meer informatie over de stappen die nodig zijn om deze te verwijderen.

Nadat u deze oplossingen hebt verwijderd, kunt u de volgende stappen uit als u wilt loskoppelen van uw Automation-account kunt uitvoeren.

> [!NOTE]
> Sommige oplossingen met inbegrip van eerdere versies van de Azure SQL-oplossing voor controle mogelijk gemaakt automatiseringsactiva en moet mogelijk ook worden verwijderd voordat het ontkoppelen van de werkruimte.

1. Open uw Automation-account vanuit Azure portal, en op de Automation-account selecteren pagina **gekoppelde werkruimte** onder de sectie **gerelateerde Resources** aan de linkerkant.

1. Klik op de pagina van de werkruimte ontkoppelen **werkruimte ontkoppelen**.

   ![Pagina voor werkruimte ontkoppelen](media/automation-onboard-solutions-from-vm/automation-unlink-workspace-blade.png).

   U ontvangt een prompt waarin u wordt gevraagd of u wilt doorgaan.

1. Terwijl Azure Automation probeert te ontkoppelen van het account uw Log Analytics-werkruimte, u kunt de voortgang volgen onder **meldingen** in het menu.

Als u de oplossing Update Management gebruikt, kunt indien gewenst u verwijderen van de volgende items die niet meer nodig zijn nadat u de oplossing te verwijderen.

* Plant u update - elk hebben namen die overeenkomen met de update-implementaties die u hebt gemaakt)

* Hybrid worker-groepen gemaakt voor de oplossing - elke naam op dezelfde manier naar machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8).

Als u de VM's starten/stoppen buiten kantooruren oplossing gebruikt, kunt indien gewenst u verwijderen van de volgende items die niet meer nodig zijn nadat u de oplossing te verwijderen.

* Starten en stoppen van VM runbook schema 's
* VM-runbooks starten en stoppen
* Variabelen

## <a name="next-steps"></a>Volgende stappen

Verder met de zelfstudies voor de oplossingen voor meer informatie over het gebruik ervan:

* [Zelfstudie - updates voor uw virtuele machine beheren](automation-tutorial-update-management.md)
* [Zelfstudie - software op een virtuele machine identificeren](automation-tutorial-installed-software.md)
* [Zelfstudie: problemen met wijzigingen op een virtuele machine oplossen](automation-tutorial-troubleshoot-changes.md)

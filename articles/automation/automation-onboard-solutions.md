---
title: Ingebouwde update en bijhouden van oplossingen voor Azure Automation | Microsoft Docs
description: Meer informatie over het vrijgeven update en het bijhouden van oplossingen voor Azure Automation.
services: automation
documentationcenter: 
author: eamonoreilly
manager: 
editor: 
ms.assetid: edae1156-2dc7-4dab-9e5c-bf253d3971d0
ms.service: automation
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/11/2017
ms.author: eamono
ms.custom: mvc
ms.openlocfilehash: e277aa44dfe625780d72a78010f0638c73a6b182
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="onboard-update-and-change-tracking-solutions-to-azure-automation"></a>Ingebouwde update en bijhouden van oplossingen voor Azure Automation

In deze zelfstudie leert u hoe u automatisch vrijgeven Update, bijhouden en voorraad oplossingen voor virtuele machines voor Azure Automation:

> [!div class="checklist"]
> * Ingebouwde een Azure VM
> * Oplossingen inschakelen
> * Installeren en bijwerken van modules
> * Importeer het runbook voorbereiden
> * Het runbook starten

## <a name="prerequisites"></a>Vereisten

Het volgende is vereist voor het voltooien van deze zelfstudie:

* Azure-abonnement. Als u nog geen abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of u aanmelden voor een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Automation-account](automation-offering-get-started.md) voor het beheren van computers.
* Een [virtuele machine](../virtual-machines/windows/quick-create-portal.md) voor de onboarding.

## <a name="onboard-an-azure-vm"></a>Ingebouwde een Azure VM

Naar vrijgeven Azure Virtual Machines automatisch, moet een bestaande virtuele machine worden vrijgegeven met het bijhouden van wijzigingen of Update-beheeroplossing. In deze stap maakt u met het voorbereiden een virtuele machine met het beheer van updates en wijzigingen bijhouden.

### <a name="enable-change-tracking-and-inventory"></a>Bijhouden van wijzigingen en inventarisatie inschakelen

De oplossing voor wijzigingen bijhouden en inventaris biedt de mogelijkheid om [wijzigingen bijhouden](automation-vm-change-tracking.md) en [inventaris](automation-vm-inventory.md) op uw virtuele machines. In deze stap schakelt u de oplossing op een virtuele machine.

1. Selecteer in het menu links **Automation-Accounts**, en selecteer vervolgens uw automation-account in de lijst.
1. Selecteer **inventaris** onder **CONFIGURATIEBEHEER**.
1. Selecteer een bestaande werkruimte voor logboekanalyse of nieuwe maken. Klik op de **inschakelen** knop.

![Ingebouwde Update-oplossing](media/automation-onboard-solutions/inventory-onboard.png)

Wanneer tracering en inventarisatie oplossing onboarding Wijzigingsmelding is voltooid, klikt u op **updatebeheer** onder **CONFIGURATIEBEHEER**.

### <a name="enable-update-management"></a>Updatebeheer inschakelen

De oplossing voor beheer van de Update kunt u updates en patches voor uw Windows Azure-VM's beheren. U kunt de status van de beschikbare updates, installatie van de planning van vereiste updates kunt beoordelen en Implementatieresultaten bekijken om te controleren of updates met succes zijn toegepast op de virtuele machine. In deze stap schakelt u de oplossing voor uw virtuele machine.

1. Selecteer in uw Automation-Account **updatebeheer** onder **UPDATEBEHEER**.
1. De Log analytics-werkruimte geselecteerd is dezelfde werkruimte gebruikt in de vorige stap. Klik op **inschakelen** voorbereiden het beheersysteem voor Update.

![Ingebouwde Update-oplossing](media/automation-onboard-solutions/update-onboard.png)

Terwijl het beheersysteem voor de Update wordt geïnstalleerd, wordt een banner blauw weergegeven. Wanneer de oplossing is ingeschakeld kiezen **bijhouden** onder **CONFIGURATIEBEHEER** naar de volgende stap.

### <a name="select-azure-vm-to-be-managed"></a>Selecteer de virtuele machine in Azure worden beheerd

Nu dat de oplossingen zijn ingeschakeld, kunt u een virtuele machine van Azure voor Onboarding van deze oplossingen kunt toevoegen.

1. Van uw Automation-Account op de **bijhouden** pagina **+ Azure VM toevoegen** uw virtuele machine toe te voegen.

1. Selecteer uw virtuele machine uit de lijst en selecteer **inschakelen**. Deze actie wordt de wijziging tacking en de inventaris-oplossing voor de virtuele machine.

   ![Update-oplossing voor de virtuele machine inschakelen](media/automation-onboard-solutions/enable-change-tracking.png)

1. Wanneer de melding van de VM-voorbereiding is voltooid, van uw Automation-Account selecteert **updatebeheer** onder **UPDATEBEHEER**.

1. Selecteer **+ Azure VM toevoegen** uw virtuele machine toe te voegen.

1. Selecteer uw virtuele machine uit de lijst en selecteer **inschakelen**. Hierdoor is het beheersysteem voor de Update voor de virtuele machine.

   ![Update-oplossing voor de virtuele machine inschakelen](media/automation-onboard-solutions/enable-update.png)

> [!NOTE]
> Als u niet tot de andere oplossing wachten te voltooien, bij het inschakelen van de volgende oplossing ontvangt u een bericht weergegeven: *installatie van een andere oplossing wordt uitgevoerd op deze of een andere virtuele machine. Wanneer u met de installatie is voltooid de knop inschakelen is ingeschakeld en u de installatie van de oplossing op deze virtuele machine kunt aanvragen.*

## <a name="install-and-update-modules"></a>Installeren en bijwerken van modules

Dit is vereist voor het bijwerken naar de nieuwste Azure-modules en importeren `AzureRM.OperationalInsights` oplossing onboarding is te automatiseren.

## <a name="update-azure-modules"></a>Azure-Modules bijwerken

Selecteer in uw Automation-Account **Modules** onder **gedeelde bronnen**. Selecteer **bijwerken Azure Modules** voor het bijwerken van de Azure-modules naar de nieuwste versie. Selecteer **Ja** op de prompt voor het bijwerken van alle bestaande Azure modules naar de nieuwste versie.

![Update-modules](media/automation-onboard-solutions/update-modules.png)

### <a name="install-azurermoperationalinsights-module"></a>AzureRM.OperationalInsights-module installeren

Van de **Modules** pagina **bladeren galerie** om de Modulegalerie te openen. AzureRM.OperationalInsights zoeken en deze module importeren in het Automation-account.

![OperationalInsights module importeren](media/automation-onboard-solutions/import-operational-insights-module.png)

## <a name="import-the-onboarding-runbook"></a>Importeer het runbook voorbereiden

1. Selecteer in uw Automation-Account op **Runbooks** onder de **PROCESAUTOMATISERING**.
1. Selecteer **bladeren galerie**.
1. Zoeken naar **bijwerken en het bijhouden van**het runbook op en selecteer **importeren** op de **bron weergeven** pagina. Selecteer **OK** importeren van het runbook in het Automation-account.

  ![Onboarding runbook importeren](media/automation-onboard-solutions/import-from-gallery.png)

1. Op de **Runbook** pagina **bewerken**, selecteer daarna **publiceren**. Op de **Runbook publiceren** dialoogvenster Selecteer **Ja** om het runbook te publiceren.

## <a name="start-the-runbook"></a>Het runbook starten

U moet hebben vrijgegeven bijhouden van wijzigingen of oplossingen bijwerken naar een Azure-virtuele machine om te beginnen met dit runbook. Hiervoor een bestaande virtuele machine en de resourcegroep met de oplossing vrijgegeven voor parameters.

1. Open het runbook inschakelen MultipleSolution.

   ![Meerdere runbooks van de oplossing](media/automation-onboard-solutions/runbook-overview.png)

1. Klik op start en voer de volgende waarden voor parameters.

   * **VMNAME** -leeg laten. De naam van een bestaande virtuele machine om vrij te geven voor update of het bijhouden van de oplossing. Deze waarde leeg laat, zijn alle virtuele machines in de resourcegroep vrijgegeven.
   * **VMRESOURCEGROUP** -de naam van de resourcegroep voor de virtuele machines worden vrijgegeven.
   * **SUBSCRIPTIONID** -leeg laten. De abonnement-ID van de nieuwe virtuele machine worden vrijgegeven. Als dit leeg laat, wordt het abonnement van de werkruimte gebruikt. Wanneer een andere abonnements-ID is opgegeven, moet de RunAs-account voor dit automation-account worden toegevoegd als medewerker voor dit abonnement ook.
   * **ALREADYONBOARDEDVM** -de naam van de virtuele machine die handmatig vrijgegeven voor de Updates of ChangeTracking is oplossing.
   * **ALREADYONBOARDEDVMRESOURCEGROUP** -de naam van de resourcegroep die de virtuele machine lid van is.
   * **SOLUTIONTYPE** -Voer **Updates** of **ChangeTracking**

   ![Runbookparameters inschakelen MultipleSolution](media/automation-onboard-solutions/runbook-parameters.png)

1. Selecteer **OK** de runbooktaak starten.
1. Bewaakt de voortgang en eventuele fouten op de pagina van de taak runbook.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Ingebouwde Azure een virtuele machine handmatig.
> * Installeren en bijwerken van de vereiste modules voor Azure.
> * Een runbook dat onboards Azure VM's importeren.
> * Het runbook dat onboards Azure VM's wordt automatisch gestart.

Volg deze koppeling voor meer informatie over het plannen van runbooks.

> [!div class="nextstepaction"]
> [Planning van runbooks](automation-schedules.md).

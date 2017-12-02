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
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2017
ms.author: eamono
ms.openlocfilehash: 9ae5e9ebcbcf3af61318eac98defc6b249417f41
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/02/2017
---
# <a name="onboard-update-and-change-tracking-solutions-to-azure-automation"></a>Ingebouwde update en bijhouden van oplossingen voor Azure Automation

In deze zelfstudie leert u hoe u automatisch vrijgeven Update, bijhouden en voorraad oplossingen voor virtuele machines voor Azure Automation:

> [!div class="checklist"]
> * Ingebouwde Azure een virtuele machine handmatig.
> * Installeren en bijwerken van de vereiste modules voor Azure.
> * Een runbook dat onboards Azure VM's importeren.
> * Het runbook dat onboards Azure VM's wordt automatisch gestart.

## <a name="prerequisites"></a>Vereisten

Het volgende is vereist voor het voltooien van deze zelfstudie.
+ Azure-abonnement. Als u nog geen abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of u aanmelden voor een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
+ [Automation-account](automation-offering-get-started.md) voor het beheren van computers.

## <a name="onboard-an-azure-virtual-machine-manually"></a>Ingebouwde Azure een virtuele machine handmatig

1.  Open het Automation-account.
2.  Klik op de pagina inventaris.
![Ingebouwde inventaris-oplossing](media/automation-onboard-solutions/inventory-onboard.png)
3.  Selecteer een bestaande werkruimte voor logboekanalyse of nieuwe maken. Klik op inschakelen.
4.  Wanneer tracering en inventarisatie oplossing onboarding Wijzigingsmelding is voltooid, klikt u op de pagina updatebeheer.
![Ingebouwde Update-oplossing](media/automation-onboard-solutions/update-onboard.png)
4.  Klik op inschakelen dat onboards de update-oplossing.
5.  Wanneer het updatebericht dat oplossing voorbereiding is voltooid, klikt u op de pagina bijhouden.
![Vrijgeven bijhouden](media/automation-onboard-solutions/change-tracking-onboard-vm.png)
6.  Klik op de knop toevoegen Azure VM.
![Selecteer VM voor het bijhouden](media/automation-onboard-solutions/enable-change-tracking.png)
7.  Selecteer een virtuele machine van Azure en klik op de knop inschakelen voor Onboarding van de oplossing voor tracering en inventarisatie.
8.  Wanneer de melding van de VM-voorbereiding is voltooid, klikt u op de pagina updatebeheer.
![Ingebouwde VM voor updatebeheer](media/automation-onboard-solutions/update-onboard-vm.png)
9.  Klik op de knop toevoegen Azure VM.
![Selecteer VM voor updatebeheer](media/automation-onboard-solutions/enable-update.png)
10.  Selecteer een virtuele machine van Azure en klik op de knop inschakelen voor Onboarding van het beheersysteem voor update.

## <a name="install-and-update-required-azure-modules"></a>Installeren en bijwerken van de vereiste modules voor Azure

Het is vereist voor het bijwerken naar de nieuwste Azure-modules en AzureRM.OperationalInsights voor het automatiseren van oplossing onboarding is geïmporteerd.
1.  Klik op de pagina Modules.
![Update-modules](media/automation-onboard-solutions/update-modules.png)
2.  Klik op de knop Update Azure Modules die worden bijgewerkt naar de nieuwste versie. Wacht totdat de updates te voltooien.
3.  Klik op de knop Bladeren galerie om de Modulegalerie te openen.
![OperationalInsights module importeren](media/automation-onboard-solutions/import-operational-insights-module.png)
4.  AzureRM.OperationalInsights zoeken en deze module importeren in het Automation-account.

## <a name="import-a-runbook-that-onboards-solutions-to-azure-vms"></a>Een runbook importeren die oplossingen onboards virtuele Azure-machines

1.  Klik op de pagina Runbooks onder de categorie 'PROCESAUTOMATISERING'.
2.  Klik op de knop 'Bladeren galerie'.
3.  Zoek naar 'bijwerken en het bijhouden van' en het runbook importeren in het Automation-account.
![Onboarding runbook importeren](media/automation-onboard-solutions/import-from-gallery.png)
4.  Klik op "Edit" weergeven van de Runbook-bron en klik op de knop 'Publiceren'.
![Onboarding runbook importeren](media/automation-onboard-solutions/publish-runbook.png)

## <a name="start-the-runbook-that-onboards-azure-vms-automatically"></a>Het runbook automatisch die onboards virtuele Azure-machines starten

U moet hebben vrijgegeven bijhouden van wijzigingen of oplossingen bijwerken naar een Azure-virtuele machine om te beginnen met dit runbook. Hiervoor een bestaande virtuele machine en de resourcegroep met de oplossing vrijgegeven voor parameters.
1.  Open het runbook inschakelen MultipleSolution.
![Meerdere runbooks van de oplossing](media/automation-onboard-solutions/runbook-overview.png)
2.  Klik op start en voer de volgende waarden voor parameters.
    *   VMNAME. De naam van een bestaande virtuele machine om vrij te geven voor update of het bijhouden van de oplossing. Laat leeg en alle virtuele machines in de resourcegroep zijn vrijgegeven.
    *   VMRESOURCEGROUP. De naam van de resourcegroep die de virtuele machine lid van is.
    *   ABONNEMENT-ID. De abonnements-ID voor de nieuwe virtuele machine om vrij te geven bevindt. Laat leeg en het abonnement van de werkruimte wordt gebruikt. Wanneer een andere abonnements-ID is opgegeven, moet de RunAs-account voor dit automation-account worden toegevoegd als medewerker voor dit abonnement ook.
    *   ALREADYONBOARDEDVM. De naam van de virtuele machine die handmatig vrijgegeven voor de Updates of ChangeTracking is oplossing.
    *   ALREADYONBOARDEDVMRESOURCEGROUP. De naam van de resourcegroep die de virtuele machine lid van is.
    *   SOLUTIONTYPE. Voer 'Updates' of 'ChangeTracking'
    
    ![Meerdere oplossing runbook-parameters](media/automation-onboard-solutions/runbook-parameters.png)
3.  Klik op OK om de runbooktaak.
4.  Bewaakt de voortgang en eventuele fouten op de pagina van de taak runbook.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie [runbooks planning](automation-schedules.md).
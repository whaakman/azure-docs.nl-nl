---
title: Azure Automation integreren met gebeurtenis raster | Microsoft Docs
description: Informatie over het automatisch toevoegen van een label wanneer een nieuwe virtuele machine wordt gemaakt en een melding verzenden naar Microsoft-Teams.
keywords: Automation, runbook, teams, gebeurtenis raster, virtuele machine, VM
services: automation
documentationcenter: 
author: eamonoreilly
manager: 
editor: 
ms.assetid: 0dd95270-761f-448e-af48-c8b1e82cd821
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2017
ms.author: eamono
ms.openlocfilehash: 6798f98755ad1d70d316b074643700f7b3e25ee7
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/02/2017
---
# <a name="integrating-azure-automation-with-event-grid-and-microsoft-teams"></a>Azure Automation integreren met gebeurtenis raster en Microsoft-Teams

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Gebeurtenis raster voorbeeldrunbook importeren.
> * Een optionele Teams-webhook maken.
> * Maak een webhook voor het runbook.
> * Een raster gebeurtenisabonnement maken.
> * Virtuele machine die runbook activeert maken.

## <a name="prerequisites"></a>Vereisten

Het volgende is vereist voor het voltooien van deze zelfstudie.
+ [Automation-account](../automation/automation-offering-get-started.md) voor het opslaan van het runbook dat uit het abonnement van de rasterlijnen gebeurtenis wordt geactiveerd.

## <a name="import-event-grid-sample-runbook"></a>Gebeurtenis raster voorbeeldrunbook importeren
1.  Open de Automation-account en klik op de pagina Runbooks.
2.  Klik op de knop 'Bladeren galerie'.
![Runbook-lijst van de gebruikersinterface](media/ensure-tags-exists-on-new-virtual-machines/event-grid-runbook-list.png)
3.  Zoek naar 'Gebeurtenis raster' en het runbook importeren in het Automation-account.
![Galerie runbook importeren](media/ensure-tags-exists-on-new-virtual-machines/gallery-event-grid.png)
4.  Klik op "Edit" weergeven van de Runbook-bron en klik op de knop 'Publiceren'.
![Runbook uit de gebruikersinterface van publiceren](media/ensure-tags-exists-on-new-virtual-machines/publish-runbook.png)

## <a name="create-an-optional-teams-webhook"></a>Een optionele Teams-webhook maken
1.  In Microsoft-Teams, kies meer opties (...) naast de naam van het kanaal en kies connectors.
![Teams verbindingen](media/ensure-tags-exists-on-new-virtual-machines/teams-webhook.png)
2.  Blader door de lijst met connectors binnenkomende Webhook en klik op toevoegen.
![Webhook-verbinding teams](media/ensure-tags-exists-on-new-virtual-machines/select-teams-webhook.png)
3.  Voer AzureAutomationIntegration voor de naam en klik op maken.
![Webhook teams](media/ensure-tags-exists-on-new-virtual-machines/configure-teams-webhook.png)
4.  De webhook naar het Klembord kopiëren en opslaan. De webhook-URL wordt gebruikt voor het verzenden van gegevens naar Microsoft-Teams.
5.  Selecteer voor het opslaan van de webhook is gedaan.

## <a name="create-a-webhook-for-the-runbook"></a>Maken van een webhook voor het runbook
1.  Open het runbook controle VMWrite.
2.  Klik op Webhooks en de knop toevoegen webhook ![webhook maken](media/ensure-tags-exists-on-new-virtual-machines/add-webhook.png)
2.  Voer 'WatchVMEventGrid' voor de naam en de URL naar het Klembord kopiëren en opslaan.
![De webhooknaam configureren](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-name.png)
3.  Selecteer parameters en voer de Microsoft-Teams webhook-URL en de WEBHOOKDATA leeg laten.
![Webhookparameters voor configureren](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-parameters.png)
4.  Selecteer OK om het Automation-runbook-webhook maken.

## <a name="create-an-event-grid-subscription"></a>Een raster gebeurtenisabonnement maken
1.  Klik op de pagina gebeurtenis raster uit het overzicht van Automation-Account.
![Lijst met gebeurtenissen raster](media/ensure-tags-exists-on-new-virtual-machines/event-grid-list.png)
2.  Klik op de knop Nieuw gebeurtenis abonnement.
3.  Het abonnement te configureren met de volgende informatie:
    *   AzureAutomation voor de naam invoeren. 
    *   Kies in het onderwerp-Type Azure-abonnementen.
    *   Schakel het selectievakje 'Abonneren op alle gebeurtenistypen'
    *   Kies in gebeurtenistypen, Resource schrijven geslaagd.
    *   Voer de Webhook-URL voor het runbook controle VMWrite in abonnee-eindpunt.
    *   Voer in het voorvoegsel Filter abonnement en resourcegroep die u wilt zoeken naar de nieuwe virtuele machines die zijn gemaakt. Het moet eruitzien als /subscriptions/124aa551-849d-46e4-a6dc-0bc4895422aB/resourcegroups/ContosoResourceGroup/providers/Microsoft.Compute/virtualMachines ![gebeurtenis raster lijst](media/ensure-tags-exists-on-new-virtual-machines/configure-event-grid-subscription.png)
6.  Klik op 'Maken' voor het opslaan van het raster voor een gebeurtenisabonnement.

## <a name="create-vm-that-triggers-runbook"></a>Virtuele machine die runbook activeert maken
1.  Maak een nieuwe virtuele machine in de resourcegroep die u hebt opgegeven in het geval van raster abonnement voorvoegsel filter.
2.  Het controle-VMWrite runbook moet worden aangeroepen en een nieuwe Tag toegevoegd aan de virtuele machine.
![VMTag](media/ensure-tags-exists-on-new-virtual-machines/vm-tag.png)
3.  Een nieuw bericht is verzonden naar het kanaal Teams.

![Teams melding](media/ensure-tags-exists-on-new-virtual-machines/teams-vm-message.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie maakt instellen u integratie tussen gebeurtenis raster en automatisering. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Gebeurtenis raster voorbeeldrunbook importeren.
> * Een optionele Teams-webhook maken.
> * Maak een webhook voor het runbook.
> * Een raster gebeurtenisabonnement maken.
> * Virtuele machine die runbook activeert maken.

> [!div class="nextstepaction"]
> [Maken en aangepaste gebeurtenissen met de gebeurtenis raster routeren](../event-grid/custom-event-quickstart.md)

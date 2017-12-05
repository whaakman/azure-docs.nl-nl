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
ms.openlocfilehash: 8b698659ed91782b80dbefbfea02aa036c09210d
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="integrate-azure-automation-with-event-grid-and-microsoft-teams"></a>Azure Automation integreren met gebeurtenis raster en Microsoft-Teams

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een voorbeeldrunbook gebeurtenis raster importeren.
> * Maak een optionele webhook van de Microsoft-Teams.
> * Maak een webhook voor het runbook.
> * Een gebeurtenis raster-abonnement maken.
> * Maak een VM die het runbook wordt geactiveerd.

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie een [Azure Automation-account](../automation/automation-offering-get-started.md) nodig is voor het runbook dat uit het abonnement raster voor Azure-gebeurtenis wordt geactiveerd.

## <a name="import-an-event-grid-sample-runbook"></a>Een voorbeeldrunbook gebeurtenis raster importeren
1. Selecteer **Automation-Accounts**, en selecteer de **Runbooks** pagina.

2. Selecteer de **bladeren galerie** knop.

    ![Runbook-lijst van de gebruikersinterface](media/ensure-tags-exists-on-new-virtual-machines/event-grid-runbook-list.png)

3. Zoeken naar **gebeurtenis raster**, en het runbook importeren in het Automation-account.

    ![Galerie runbook importeren](media/ensure-tags-exists-on-new-virtual-machines/gallery-event-grid.png)

4. Selecteer **bewerken** de runbookbron weergeeft en selecteer de **publiceren** knop.

    ![Runbook uit de gebruikersinterface van publiceren](media/ensure-tags-exists-on-new-virtual-machines/publish-runbook.png)

## <a name="create-an-optional-microsoft-teams-webhook"></a>Een optionele Microsoft-Teams-webhook maken
1. Selecteer in de Microsoft-Teams **meer opties** volgende op de naam van het kanaal en selecteert u vervolgens **Connectors**.

    ![Microsoft-Teams verbindingen](media/ensure-tags-exists-on-new-virtual-machines/teams-webhook.png)

2. Blader door de lijst met connectors **binnenkomende Webhook**, en selecteer **toevoegen**.

    ![Microsoft-Teams webhook verbinding](media/ensure-tags-exists-on-new-virtual-machines/select-teams-webhook.png)

3. Voer **AzureAutomationIntegration** voor de naam en selecteer **maken**.

    ![Microsoft-Teams webhook](media/ensure-tags-exists-on-new-virtual-machines/configure-teams-webhook.png)

4. De webhook naar het Klembord kopiëren en opslaan. De webhook-URL wordt gebruikt om informatie te verzenden naar Microsoft-Teams.

5. Selecteer **gedaan** om op te slaan van de webhook.

## <a name="create-a-webhook-for-the-runbook"></a>Maken van een webhook voor het runbook
1. Open het runbook controle VMWrite.

2. Selecteer **Webhooks**, en selecteer de **Webhook toevoegen** knop.

    ![Webhook maken](media/ensure-tags-exists-on-new-virtual-machines/add-webhook.png)

3. Voer **WatchVMEventGrid** voor de naam. De URL naar het Klembord kopiëren en opslaan.

    ![De webhooknaam configureren](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-name.png)

4. Selecteer **Parameters en uitvoerinstellingen**, en voer de Microsoft-Teams webhook-URL. Laat **WEBHOOKDATA** leeg.

    ![Webhookparameters voor configureren](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-parameters.png)

5. Selecteer **OK** de Automation-runbook-webhook maken.


## <a name="create-an-event-grid-subscription"></a>Een gebeurtenis raster-abonnement maken
1. Op de **Automation-Account** overzichtspagina, selecteer **gebeurtenis raster**.

    ![Lijst met gebeurtenissen raster](media/ensure-tags-exists-on-new-virtual-machines/event-grid-list.png)

2. Selecteer de **gebeurtenisabonnement** knop.

3. Het abonnement te configureren met de volgende informatie:

    *   Voer **AzureAutomation** voor de naam. 
    *   In **Onderwerptype**, selecteer **Azure-abonnementen**.
    *   Schakel de **abonneren op alle gebeurtenistypen** selectievakje.
    *   In **gebeurtenistypen**, selecteer **Resource schrijven geslaagd**.
    *   In **volledige eindpunt-URL**, voert u de webhook-URL voor het runbook controle VMWrite.
    *   In **Filter voorvoegsel**, voert u het abonnement en resourcegroep waar u wilt zoeken naar de nieuwe virtuele machines worden gemaakt. Het moet eruitzien als /subscriptions/124aa551-849d-46e4-a6dc-0bc4895422aB/resourcegroups/ContosoResourceGroup/providers/Microsoft.Compute/virtualMachines

    ![Lijst met gebeurtenissen raster](media/ensure-tags-exists-on-new-virtual-machines/configure-event-grid-subscription.png)

4. Selecteer **maken** om op te slaan van het abonnement gebeurtenis raster.

## <a name="create-a-vm-that-triggers-the-runbook"></a>Maak een VM die het runbook wordt geactiveerd
1. Maak een nieuwe virtuele machine in de resourcegroep die u hebt opgegeven in het geval van raster abonnement voorvoegsel filter.

2. Het controle-VMWrite runbook moet worden aangeroepen en een nieuwe tag toegevoegd aan de virtuele machine.

    ![VM-tag](media/ensure-tags-exists-on-new-virtual-machines/vm-tag.png)

3. Een nieuw bericht is verzonden naar het Microsoft-Teams-kanaal.

    ![Melding van de Microsoft-Teams](media/ensure-tags-exists-on-new-virtual-machines/teams-vm-message.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie maakt instellen u integratie tussen gebeurtenis raster en automatisering. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een voorbeeldrunbook gebeurtenis raster importeren.
> * Maak een optionele webhook van de Microsoft-Teams.
> * Maak een webhook voor het runbook.
> * Een gebeurtenis raster-abonnement maken.
> * Maak een VM die het runbook wordt geactiveerd.

> [!div class="nextstepaction"]
> [Maken en aangepaste gebeurtenissen met de gebeurtenis raster routeren](../event-grid/custom-event-quickstart.md)

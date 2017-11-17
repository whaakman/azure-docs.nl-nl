---
title: Een watcher-taak in de Azure Automation-account maken | Microsoft Docs
description: Informatie over het maken van een watcher in Azure Automation-account moeten worden gecontroleerd of er nieuwe bestanden die in een map wordt gemaakt.
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
ms.date: 01/15/2017
ms.author: eamono
ms.openlocfilehash: 55971908caca4526f7a3b102e90c554a533a7fee
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="azure-automation-watcher-tasks-enable-you-to-respond-to-events-happening-in-your-local-datacenter"></a>Azure Automation-watcher-taken kunnen u reageer op gebeurtenissen die plaatsvinden in uw lokale datacentrum

In deze zelfstudie leert u hoe een nieuwe taak watcher te maken:

> [!div class="checklist"]
> * Een watcher-runbook die er ongeveer voor nieuwe bestanden in een map uitziet maken.
> * Een Automation-variabele te houden van de laatste keer dat een bestand is verwerkt door de watcher van maken.
> * Een runbook actie die wordt aangeroepen wanneer het runbook watcher vindt een nieuw bestand maken.
> * Maak een watcher-taak die u het watcher-runbook en de actie runbook selecteert.
> * Een watcher activeren door een nieuw bestand toe te voegen aan een map.
> * Inspecteer de uitvoer van het runbook actie die wordt informatie weergegeven over het nieuwe bestand.  

## <a name="prerequisites"></a>Vereisten

Het volgende is vereist voor het voltooien van deze zelfstudie.
+ Azure-abonnement. Als u nog geen abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of u aanmelden voor een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
+ [Automation-account](automation-offering-get-started.md) voor het opslaan van de runbooks watcher en de actie en de Watcher-taak.
+ Een [hybride runbook worker](automation-hybrid-runbook-worker.md) waarin de watcher-taak wordt uitgevoerd.

## <a name="create-a-watcher-runbook-that-looks-for-new-files"></a>Een Watcher-runbook die er ongeveer voor nieuwe bestanden uitziet maken
1.  Open de Automation-account en klik op de pagina Runbooks.
2.  Klik op de knop 'Bladeren galerie'.
![Runbook-lijst van de gebruikersinterface](media/automation-watchers-tutorial/WatcherTasksRunbookList.png)
3.  Zoek naar 'Controle NewFile' en het runbook importeren in het Automation-account.
![Runbook uit de gebruikersinterface van publiceren](media/automation-watchers-tutorial/Watch-NewFileRunbook.png)
4.  Klik op "Edit" weergeven van de Runbook-bron en klik op de knop 'Publiceren'.

## <a name="create-an-automation-variable-to-keep-the-last-time-a-file-was-processed-by-the-watcher"></a>Maak een automatiseringsvariabele te houden van de laatste keer dat een bestand is verwerkt door de watcher van de
1.  Open de pagina variabelen onder gedeelde bronnen en klik op 'Variabele toevoegen' ![weer te geven door de gebruikersinterface](media/automation-watchers-tutorial/WatcherVariableList.png)
2.  Voer 'Controle NewFileTimestamp' voor de naam
3.  Type selecteren als datum/tijd en klik vervolgens op de knop 'Maken'.
![Watermerk variabele maken door de gebruikersinterface](media/automation-watchers-tutorial/WatcherWatermarkVariable.png)

## <a name="create-an-action-runbook-that-is-called-when-the-watcher-runbook-finds-a-new-file"></a>Een runbook actie die wordt aangeroepen wanneer het runbook watcher vindt een nieuw bestand maken
1.  Klik op de pagina Runbooks onder de categorie 'PROCESAUTOMATISERING'.
2.  Klik op de knop 'Bladeren galerie'.
3.  Zoek naar 'Proces NewFile' en het runbook importeren in het Automation-account.
4.  Klik op "Edit" weergeven van de Runbook-bron en klik op de knop 'Publiceren'.
![Watcher proces door de gebruikersinterface](media/automation-watchers-tutorial/Watch-ProcessNewFile.png)


## <a name="create-a-watcher-task-that-selects-the-watcher-runbook-and-action-runbook"></a>Een watcher-taak die u het watcher-runbook en de actie runbook selecteert maken
1.  Open de pagina van de Watcher-taken en klikt u op de knop 'Toevoegen van een watcher-taak'.
![Lijst van de Watcher van de gebruikersinterface](media/automation-watchers-tutorial/WatchersList.png)
2.  Voer 'Nieuwe bestanden bekijken' als naam.
3.  Selecteer 'Watcher configureren' en selecteer het runbook 'Controle NewFile'.
![Configureren van de watcher van de gebruikersinterface](media/automation-watchers-tutorial/ConfigureWatcher.png)
4.  Voer de volgende waarden voor parameters:
    *   FOLDERPATH. Een map op de hybride worker waarbij nieuwe bestanden gemaakt
    *   DE EXTENSIE. Laat leeg alle bestandsextensies verwerken.
    *   RECURSE. Laat de standaardwaarde.
    *   INSTELLINGEN UITVOEREN. Kies de hybride worker.
5.  Klik op OK en selecteer vervolgens het terug naar de pagina watcher.
6.  Selecteer 'Action configureren' en selecteer 'Proces NewFile' runbook.
![Configureer watcher actie door de gebruikersinterface](media/automation-watchers-tutorial/ConfigureAction.png)
7.  Voer de volgende waarden voor parameters:
    *   EVENTDATA. Leeg laten. Gegevens wordt van het runbook watcher doorgegeven.
    *   Instellingen uitvoeren. Behoud Azure als dit runbook wordt uitgevoerd in de Automation-service.
8.  Klik op OK en selecteer vervolgens het terug naar de pagina watcher.
9.  Klik op OK om het watcher-taak te maken.

## <a name="trigger-a-watcher-by-adding-a-new-file-to-a-directory"></a>Een watcher activeren door een nieuw bestand toe te voegen aan een map
1.  De afstand in de hybride worker
2.  Een nieuw tekstbestand toevoegen aan de map die door de taak watcher wordt bewaakt.

## <a name="inspect-the-output-from-the-action-runbook-that-shows-information-on-the-new-file"></a>Inspecteer de uitvoer van het runbook actie die wordt informatie weergegeven over het nieuwe bestand
1.  Klik op de taak watcher voor 'Nieuwe bestanden bekijken'
2.  Klik op de 'weergave watcher stromen' om te zien dat u de watcher het nieuwe bestand vinden en het actie-runbook gestart.
3.  Klik op de 'watcher actietaken weergeven' om te zien van de runbooktaak in te grijpen.
![De actietaken Watcher van de gebruikersinterface](media/automation-watchers-tutorial/WatcherActionJobs.png)


## <a name="next-steps"></a>Volgende stappen:

Zie voor meer informatie [Mijn eerste PowerShell-runbook](automation-first-runbook-textual-powershell.md).









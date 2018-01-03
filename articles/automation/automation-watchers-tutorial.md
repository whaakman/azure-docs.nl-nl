---
title: Een watcher-taak in het Azure Automation-account maken | Microsoft Docs
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
ms.date: 12/11/2017
ms.author: eamono
ms.openlocfilehash: 294faa48f9840919b087594835706bad8048d45b
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="create-an-azure-automation-watcher-tasks-to-track-file-changes-on-a-local-machine"></a>Een Azure Automation-watcher-taken voor het bijhouden van wijzigingen in bestanden op een lokale computer maken

Azure Automation maakt gebruik van watcher-taken om te kijken naar gebeurtenissen en acties activeren. Deze zelfstudie leert u een taak watcher om te controleren wanneer een nieuw bestand wordt toegevoegd aan een map maken.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een watcher-runbook importeren
> * Een automatiseringsvariabele maken
> * Een actie-runbook maken
> * Een watcher-taak maken
> * Een watcher activeren
> * Inspecteer de uitvoer

## <a name="prerequisites"></a>Vereisten

Het volgende is vereist voor het voltooien van deze zelfstudie:

* Azure-abonnement. Als u nog geen abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of u aanmelden voor een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Automation-account](automation-offering-get-started.md) voor het opslaan van de runbooks watcher en de actie en de Watcher-taak.
* Een [hybride runbook worker](automation-hybrid-runbook-worker.md) waarin de watcher-taak wordt uitgevoerd.

## <a name="import-a-watcher-runbook"></a>Een watcher-runbook importeren

Deze zelfstudie wordt gebruikgemaakt van een watcher runbook aangeroepen **controle NewFile** wilt zoeken naar nieuwe bestanden in een map. Het watcher-runbook haalt het laatst bekende schrijven naar de bestanden in een map en alle bestanden die nieuwer is dan het watermerk wordt bekeken. In deze stap kunt u dit runbook importeren in uw automation-account.

1. Open uw Automation-account en klik op de **Runbooks** pagina.
1. Klik op de **bladeren galerie** knop.
1. Zoek naar 'Watcher runbook', selecteer **Watcher runbook waarmee wordt gezocht naar nieuwe bestanden in een map** en selecteer **importeren**.
  ![Automation-runbook importeren door de gebruikersinterface](media/automation-watchers-tutorial/importsourcewatcher.png)
1. Geef het runbook een naam en beschrijving en selecteer **OK** importeren van het runbook in uw Automation-account.
1. Selecteer **bewerken** en klik vervolgens op **publiceren**. Wanneer u daarom wordt gevraagd selecteren **Ja** om het runbook te publiceren.

## <a name="create-an-automation-variable"></a>Een automatiseringsvariabele maken

Een [automation-variabele](automation-variables.md) wordt gebruikt voor het opslaan van de tijdstempels die de voorgaande runbook leest en van elk bestand worden opgeslagen. 

1. Selecteer **variabelen** onder **gedeelde bronnen** en selecteer **+ toevoegen van een variabele**.
1. Voer 'Controle NewFileTimestamp' voor de naam
1. Selecteer de datum/tijd voor het Type.
1. Klik op de **maken** knop. Hiermee maakt u de automatiseringsvariabele.

## <a name="create-an-action-runbook"></a>Een actie-runbook maken

Een runbook actie wordt gebruikt in een taak watcher om te fungeren voor de gegevens vanuit een runbook watcher doorgegeven. In deze stap maakt u importeren bijwerken een vooraf gedefinieerde actie runbook 'Proces NewFile' genoemd.

1. Navigeer naar uw automation-account en selecteer **Runbooks** onder de **PROCESAUTOMATISERING** categorie.
1. Klik op de **bladeren galerie** knop.
1. "action Watcher" Zoek en selecteer **Watcher-actie die verwerkt de gebeurtenissen die worden geactiveerd door een watcher-runbook** en selecteer **importeren**.
  ![Actie runbook importeren door de gebruikersinterface](media/automation-watchers-tutorial/importsourceaction.png)
1. Geef het runbook een naam en beschrijving en selecteer **OK** importeren van het runbook in uw Automation-account.
1. Selecteer **bewerken** en klik vervolgens op **publiceren**. Wanneer u daarom wordt gevraagd selecteren **Ja** om het runbook te publiceren.

## <a name="create-a-watcher-task"></a>Een watcher-taak maken

De watcher-taak bestaat uit twee delen. De watcher en de actie. De watcher wordt uitgevoerd op een interval dat is gedefinieerd in de watcher-taak. Gegevens van het runbook watcher wordt doorgegeven naar het runbook in te grijpen. In deze stap configureert u de watcher-taak die verwijzen naar de watcher en de actie runbooks die zijn gedefinieerd in de voorgaande stappen hebt uitgevoerd.

1. Navigeer naar uw automation-account en selecteer **Watcher-taken** onder de **PROCESAUTOMATISERING** categorie.
1. Selecteer de pagina van de Watcher-taken en klik op **+ toevoegen van een taak watcher** knop.
1. Voer 'WatchMyFolder' als naam.

1. Selecteer **configureren watcher** en selecteer de **controle NewFile** runbook.

1. Voer de volgende waarden voor de parameters:

   * **FOLDERPATH** -een map op de hybride worker waarbij nieuwe bestanden gemaakt. d:\examplefiles
   * **EXTENSIE** -verwerken alle bestandsextensies leeg laten.
   * **RECURSE** -laat deze waarde als standaardwaarde.
   * **INSTELLINGEN uitvoeren** -Kies de hybride worker.

1. Klik op OK en selecteer vervolgens het terug naar de pagina watcher.
1. Selecteer **Configureer actie** en selecteer 'Proces NewFile' runbook.
1. Voer de volgende waarden voor parameters:

   *    **EVENTDATA** -leeg laten. Gegevens wordt van het runbook watcher doorgegeven.  
   *    **Instellingen uitvoeren** -behoud Azure als dit runbook wordt uitgevoerd in de Automation-service.

1. Klik op **OK**, en selecteer vervolgens het terug naar de pagina watcher.
1. Klik op **OK** de watcher-taak maken.

![Configureer watcher actie door de gebruikersinterface](media/automation-watchers-tutorial/watchertaskcreation.png)

## <a name="trigger-a-watcher"></a>Een watcher activeren

Als u wilt testen van de watcher werkt zoals verwacht, moet u een testbestand te maken.

De afstand in de hybride worker. Open **PowerShell** en maakt u een testbestand in de map.
  
   ```PowerShell-interactive
   New-Item -Name ExampleFile1.txt
   ```

Het volgende voorbeeld toont de verwachte uitvoer.

```
    Directory: D:\examplefiles


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----       12/11/2017   9:05 PM              0 ExampleFile1.txt
```

## <a name="inspect-the-output"></a>Inspecteer de uitvoer

1. Navigeer naar uw automation-account en selecteer **Watcher-taken** onder de **PROCESAUTOMATISERING** categorie.
1. Selecteer de watcher-taak 'WatchMyFolder'.
1. Klik op **watcher streams weergeven** onder **Streams** om te zien dat u de watcher het nieuwe bestand vinden en het actie-runbook gestart.
1. De actie runbooktaken, klik op de **watcher actietaken weergeven**. Elke taak kan worden de details van de taak voor de weergave selecteert.

   ![De actietaken Watcher van de gebruikersinterface](media/automation-watchers-tutorial/WatcherActionJobs.png)

In het volgende voorbeeld ziet u de verwachte uitvoer wanneer het nieuwe bestand is gevonden:

```
Message is Process new file...



Passed in data is @{FileName=D:\examplefiles\ExampleFile1.txt; Length=0}
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een watcher-runbook importeren
> * Een automatiseringsvariabele maken
> * Een actie-runbook maken
> * Een watcher-taak maken
> * Een watcher activeren
> * Inspecteer de uitvoer

Volg deze koppeling voor meer informatie over het ontwerpen van uw eigen runbook.

> [!div class="nextstepaction"]
> [Mijn eerste PowerShell-runbook](automation-first-runbook-textual-powershell.md).

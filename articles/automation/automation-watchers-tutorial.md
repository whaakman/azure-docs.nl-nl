---
title: Een watcher-taak maken in de Azure Automation-account
description: Informatie over het maken van een watcher-taak in de Azure Automation-account om te bekijken voor nieuwe bestanden die zijn gemaakt in een map.
services: automation
ms.service: automation
ms.component: process-automation
author: eamonoreilly
ms.author: eamono
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: 4068ce42babb786ca33e1b0d41fdfefc7d3f78be
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52282769"
---
# <a name="create-an-azure-automation-watcher-tasks-to-track-file-changes-on-a-local-machine"></a>Maak een Azure Automation-watcher-taken voor het bijhouden van wijzigingen in bestanden op een lokale computer

Azure Automation maakt gebruik van watcher-taken om te kijken naar gebeurtenissen en acties met PowerShell-runbooks activeren. In deze zelfstudie helpt u bij het maken van een watcher-taak om te controleren wanneer een nieuw bestand wordt toegevoegd aan een map.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een watcher-runbook importeren
> * Een Automation-variabele maken
> * Een actie-runbook maken
> * Een watcher-taak maken
> * Een watcher activeren
> * De uitvoer kunt inspecteren

## <a name="prerequisites"></a>Vereisten

Hieronder wordt aangegeven wat de vereisten zijn om deze zelfstudie te voltooien:

* Azure-abonnement. Als u nog geen abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of u aanmelden voor een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Automation-account](automation-offering-get-started.md) voor het opslaan van de watcher- en actie-runbooks en de Watcher-taak.
* Een [hybrid runbook worker](automation-hybrid-runbook-worker.md) waarop de watcher-taak wordt uitgevoerd.

## <a name="import-a-watcher-runbook"></a>Een watcher-runbook importeren

Deze zelfstudie wordt gebruikgemaakt van een watcher-runbook met de naam **Watch NewFile** om te zoeken naar nieuwe bestanden in een map. De watcher-runbook opgehaald van de laatst bekende schrijven naar de bestanden in een map en kijkt naar alle bestanden die nieuwer is dan watermerk. In deze stap maakt importeren u dit runbook in uw automation-account.

1. Open uw Automation-account en klik op de **Runbooks** pagina.
2. Klik op de **bladeren in galerie** knop.
3. Zoek naar 'Watcher-runbook", selecteer **Watcher-runbook dat zoekt naar nieuwe bestanden in een map** en selecteer **importeren**.
  ![Automation-runbook importeren vanuit de gebruikersinterface](media/automation-watchers-tutorial/importsourcewatcher.png)
1. Geef het runbook een naam en beschrijving en selecteer **OK** het runbook importeren in uw Automation-account.
1. Selecteer **bewerken** en klik vervolgens op **publiceren**. Wanneer u hierom wordt gevraagd Selecteer **Ja** om het runbook te publiceren.

## <a name="create-an-automation-variable"></a>Een Automation-variabele maken

Een [automation-variabele](automation-variables.md) wordt gebruikt voor het opslaan van de tijdstempels die de voorgaande runbook leest en van elk bestand opslaat.

1. Selecteer **variabelen** onder **gedeelde bronnen** en selecteer **+ toevoegen van een variabele**.
1. 'Controle-NewFileTimestamp' voor de naam invoeren
1. Selecteer de datum/tijd voor het Type.
1. Klik op de **maken** knop. Hiermee maakt u het automation-variabele.

## <a name="create-an-action-runbook"></a>Een actie-runbook maken

Een runbook action wordt in een watcher-taak gebruikt om te reageren op de gegevens doorgegeven aan deze van een watcher-runbook. PowerShell Workflow-runbooks worden niet ondersteund door watcher-taken, moet u PowerShell-runbooks. In deze stap maakt u importeren bijwerken een vooraf gedefinieerde actie runbook met de naam 'Proces-NewFile'.

1. Navigeer naar uw automation-account en selecteer **Runbooks** onder de **PROCESAUTOMATISERING** categorie.
1. Klik op de **bladeren in galerie** knop.
1. Zoek naar 'Watcher-actie' en selecteer **Watcher-actie die gebeurtenissen die worden geactiveerd door een watcher-runbook verwerkt** en selecteer **importeren**.
  ![Actie runbook importeren vanuit de gebruikersinterface](media/automation-watchers-tutorial/importsourceaction.png)
1. Geef het runbook een naam en beschrijving en selecteer **OK** het runbook importeren in uw Automation-account.
1. Selecteer **bewerken** en klik vervolgens op **publiceren**. Wanneer u hierom wordt gevraagd Selecteer **Ja** om het runbook te publiceren.

## <a name="create-a-watcher-task"></a>Een watcher-taak maken

De watcher-taak bestaat uit twee delen. De watcher- en de actie. De watcher wordt uitgevoerd op een interval dat is gedefinieerd in de watcher-taak. Gegevens van de watcher-runbook is doorgegeven aan het actie-runbook. In deze stap configureert u de watcher-taak die verwijst naar de watcher- en actie-runbooks die zijn gedefinieerd in de voorgaande stappen.

1. Navigeer naar uw automation-account en selecteer **Watcher-taken** onder de **PROCESAUTOMATISERING** categorie.
1. Selecteer de pagina van de Watcher-taken en klik op **+ toevoegen van een watcher-taak** knop.
1. Voer 'WatchMyFolder' als de naam.

1. Selecteer **configureren watcher** en selecteer de **Watch NewFile** runbook.

1. Voer de volgende waarden voor de parameters:

   * **FOLDERPATH** -een map op de hybrid worker waar nieuwe bestanden gemaakt. d:\examplefiles
   * **EXTENSIE** -laat leeg voor het verwerken van alle bestandsextensies.
   * **RECURSE** -laat deze waarde als standaardwaarde.
   * **INSTELLINGEN uitvoeren** -kiezen van de hybrid worker.

1. Klik op OK en selecteer vervolgens om terug te keren naar de watcher-pagina.
1. Selecteer **actie configureren** en selecteer "Proces-NewFile" runbook.
1. Voer de volgende waarden voor parameters:

   ***EVENTDATA** -leeg laten. Gegevens worden doorgegeven van het watcher-runbook.  
   ***Uitvoeringsinstellingen** -laat Azure terwijl dit runbook wordt uitgevoerd in de Automation-service.

1. Klik op **OK**, en selecteer vervolgens om terug te keren naar de watcher-pagina.
1. Klik op **OK** te maken van de watcher-taak.

![Configureren van de watcher-actie van de gebruikersinterface](media/automation-watchers-tutorial/watchertaskcreation.png)

## <a name="trigger-a-watcher"></a>Een watcher activeren

Als u wilt testen van de watcher werkt zoals verwacht, moet u een testbestand te maken.

Extern verbinding maken met de hybrid worker. Open **PowerShell** en een testbestand in de map te maken.
  
```azurepowerShell-interactive
New-Item -Name ExampleFile1.txt
```

Het volgende voorbeeld wordt de verwachte uitvoer weergegeven.

```output
    Directory: D:\examplefiles


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----       12/11/2017   9:05 PM              0 ExampleFile1.txt
```

## <a name="inspect-the-output"></a>De uitvoer kunt inspecteren

1. Navigeer naar uw automation-account en selecteer **Watcher-taken** onder de **PROCESAUTOMATISERING** categorie.
1. Selecteer de watcher-taak 'WatchMyFolder'.
1. Klik op **watcher-streams bekijken** onder **Streams** om te zien dat de watcher gevonden van het nieuwe bestand en het runbook action is gestart.
1. Als u wilt zien van de runbooktaken voor de actie, klikt u op de **watcher-actie-taken weergeven**. Elke taak kan worden de details van de taak voor de weergave geselecteerd.

   ![Taken voor Watcher-actie van UI](media/automation-watchers-tutorial/WatcherActionJobs.png)

De verwachte uitvoer wanneer het nieuwe bestand wordt gevonden in het volgende voorbeeld weergegeven:

```output
Message is Process new file...



Passed in data is @{FileName=D:\examplefiles\ExampleFile1.txt; Length=0}
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een watcher-runbook importeren
> * Een Automation-variabele maken
> * Een actie-runbook maken
> * Een watcher-taak maken
> * Een watcher activeren
> * De uitvoer kunt inspecteren

Volg deze koppeling voor meer informatie over het ontwerpen van uw eigen runbook.

> [!div class="nextstepaction"]
> [Mijn eerste PowerShell-runbook](automation-first-runbook-textual-powershell.md).

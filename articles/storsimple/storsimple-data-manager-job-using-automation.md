---
title: Gebruik Azure Automation starten van een taak in StorSimple Data Manager | Microsoft Docs
description: Informatie over het gebruik van Azure Automation voor activering van StorSimple Data Manager-taken
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/16/2018
ms.author: alkohli
ms.openlocfilehash: 1e5fcbee664271058ac1c7fa80bb285e09b8579a
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="use-azure-automation-to-trigger-a-job"></a>Gebruik Azure Automation om een taak te activeren

Dit artikel wordt uitgelegd hoe u de functie voor transformatie van gegevens in de StorSimple Data Manager-service kunt gebruiken voor het transformeren van gegevens van de StorSimple-apparaat. U kunt een transformatie-taak op twee manieren starten: 

 - De .NET SDK gebruiken
 - Gebruik Azure Automation-runbook
 
Dit artikel wordt uitgelegd hoe u een Azure Automation-runbook maken en vervolgens worden gebruikt voor het initiëren van een transformatie-taak. Voor meer informatie over het initiëren van gegevenstransformatie via .NET SDK, gaat u naar [gebruik .NET SDK trigger data transformation baan](storsimple-data-manager-dotnet-jobs.md).

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u hebt voordat u begint:

*   Azure PowerShell is geïnstalleerd op de clientcomputer. [Download de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).
*   De taakdefinitie van een juist geconfigureerde in een StorSimple Data Manager-service in de resourcegroep.
*   Download [ `DataTransformationApp.zip` ](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/raw/master/Azure%20Automation%20For%20Data%20Manager/DataTransformationApp.zip) bestand van de GitHub-opslagplaats. 
*   Download [ `Trigger-DataTransformation-Job.ps1` ](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Trigger-DataTransformation-Job.ps1) script vanaf de GitHub-opslagplaats.

## <a name="step-by-step-procedure"></a>Stapsgewijze instructies

### <a name="set-up-the-automation-account"></a>Het Automation-account instellen

1. Een automation Azure uitvoeren als-account maken in de Azure-portal. Om dit te doen, gaat u naar **Azure marketplace > Alles** en zoek vervolgens naar **Automation**. Selecteer **Automation-accounts**.

    ![Maak een uitvoeren als de automation-account](./media/storsimple-data-manager-job-using-automation/search-automation-account1.png)

2. Als u wilt een nieuw automatiseringsaccount toevoegen, klikt u op **+ toevoegen**.

    ![Maak een uitvoeren als de automation-account](./media/storsimple-data-manager-job-using-automation/add-automation-account1.png)

3. In de **Automation toevoegen**:

    1. Geef de **naam** van uw automation-account.
    2. Selecteer de **abonnement** gekoppeld aan uw StorSimple Data Manager-service.
    3. Een nieuwe resourcegroep maken of selecteren van een bestaande resourcegroep.
    4. Selecteer een **locatie**.
    5. Laat de standaardwaarde **uitvoeren als-account maken** optie is geselecteerd.
    6. Als u een koppeling voor snelle toegang op het dashboard, Controleer **vastmaken aan dashboard**. Klik op **Create**.

    ![Maak een uitvoeren als de automation-account](./media/storsimple-data-manager-job-using-automation/create-automation-run-as-account.png)
    
    Nadat het automation-account is gemaakt, wordt u gewaarschuwd.
    
    ![Melding voor de implementatie van automation-account](./media/storsimple-data-manager-job-using-automation/deployment-automation-account-notification1.png)

    Ga voor meer informatie naar [een Run As-account maken](../automation/automation-create-runas-account.md).

3. In de zojuist gemaakte account, gaat u naar **gedeelde bronnen > Modules** en klik op **+ toevoegen module**.

    ![Module 1 importeren](./media/storsimple-data-manager-job-using-automation/import-module-1.png)

4. Blader naar de locatie van `DataTransformationApp.zip` bestand vanaf uw lokale computer en selecteer en open de module. Klik op **OK** de module te importeren.

    ![2-module importeren](./media/storsimple-data-manager-job-using-automation/import-module-2.png)

   Wanneer u een module Azure Automation importeert in uw account, pakt deze metagegevens over de module. Deze bewerking kan enkele minuten duren.

   ![4-module importeren](./media/storsimple-data-manager-job-using-automation/import-module-4.png)

5. U ontvangt een melding dat de module wordt geïmplementeerd en nog een melding wanneer het proces voltooid is.  De status in **Modules** wijzigingen in **beschikbaar**.

    ![5-module importeren](./media/storsimple-data-manager-job-using-automation/import-module-5.png)

### <a name="import-publish-and-run-automation-runbook"></a>Importeren, publiceren en Automation-runbook uitvoeren

De volgende stappen uitvoeren om te importeren, publiceren en uitvoeren van het runbook zodat de taakdefinitie.

1. Open uw Automation-account in Azure Portal. Ga naar **procesautomatisering > Runbooks** en klik op **+ een runbook toevoegen**.

    ![Toevoegen van runbook 1](./media/storsimple-data-manager-job-using-automation/add-runbook-1.png)

2. In **runbook toevoegen**, klikt u op **een bestaand runbook importeren**.

3. Wijs de Azure PowerShell-scriptbestand `Trigger-DataTransformation-Job.ps1` voor de **Runbook bestand**. Het runbooktype wordt automatisch geselecteerd. Geef een naam en een optionele beschrijving voor het runbook. Klik op **Create**.

    ![Runbook 2 toevoegen](./media/storsimple-data-manager-job-using-automation/add-runbook-2.png)

4. Het nieuwe runbook wordt weergegeven in de lijst met runbooks voor het Automation-account. Selecteer en klikt u op dit runbook.

    ![Runbook 3 toevoegen](./media/storsimple-data-manager-job-using-automation/add-runbook-3.png)

5. Het runbook bewerken en klik op **Test** deelvenster.

    ![Runbook 4 toevoegen](./media/storsimple-data-manager-job-using-automation/add-runbook-4.png)

6. Geef de parameters zoals de naam van uw StorSimple Data Manager-service, de groep gekoppeld resourcetypen en naam van de taakdefinitie. **Start** de test. Het rapport wordt gegenereerd wanneer de uitvoering voltooid is. Voor meer informatie gaat u naar het [een runbook test](../automation/automation-first-runbook-textual-powershell.md#step-3---test-the-runbook).

    ![Runbook 8 toevoegen](./media/storsimple-data-manager-job-using-automation/add-runbook-8.png)    

7. Inspecteer de uitvoer van het runbook in het deelvenster. Als voldaan, sluit u het deelvenster. Klik op **publiceren** en wanneer u wordt gevraagd om bevestiging, bevestigen en het runbook gepubliceerd.

    ![Runbook 6 toevoegen](./media/storsimple-data-manager-job-using-automation/add-runbook-6.png)

8. Ga terug naar **Runbooks** en selecteer de zojuist gemaakte runbook.

    ![Runbook 7 toevoegen](./media/storsimple-data-manager-job-using-automation/add-runbook-7.png)

9. **Start** het runbook. In **runbook starten**, geef de parameters. Klik op **OK** indienen en start de taak van de transformatie van gegevens.

10. Voor het bewaken van de voortgang van de taak in Azure-portal, gaat u naar **taken** in uw StorSimple Data Manager-service. Selecteer en klik op de taak om de taakgegevens weer te geven.

    ![Runbook 10 toevoegen](./media/storsimple-data-manager-job-using-automation/add-runbook-10.png)

## <a name="next-steps"></a>Volgende stappen

[Gebruik StorSimple Data Manager UI om uw gegevens te transformeren](storsimple-data-manager-ui.md).
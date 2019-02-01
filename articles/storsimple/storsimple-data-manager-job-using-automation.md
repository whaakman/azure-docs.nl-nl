---
title: Azure Automation gebruiken om te starten van een taak in de StorSimple-gegevensbeheer | Microsoft Docs
description: Meer informatie over het gebruik van Azure Automation voor het activeren van StorSimple Data Manager-taken
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/16/2018
ms.author: alkohli
ms.openlocfilehash: 6e62e923b6e983dfff8eb72c9526708a3b360930
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55494745"
---
# <a name="use-azure-automation-to-trigger-a-job"></a>Azure Automation gebruiken om een taak te activeren

In dit artikel wordt uitgelegd hoe u de functie voor transformatie van gegevens in de StorSimple Data Manager-service kunt gebruiken om gegevens van StorSimple-apparaat te transformeren. U kunt een taak voor gegevenstransformatie op twee manieren starten: 

 - De .NET SDK gebruiken
 - Gebruik Azure Automation-runbook
 
Dit artikel wordt uitgelegd hoe u een Azure Automation-runbook maken en vervolgens worden gebruikt om te starten van een taak voor gegevenstransformatie. Voor meer informatie over het starten van de gegevenstransformatie van via .NET SDK, gaat u naar [.NET SDK gebruiken om taken voor gegevenstransformatie trigger te](storsimple-data-manager-dotnet-jobs.md).

## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorg ervoor dat u hebt:

*   Azure PowerShell is geïnstalleerd op de clientcomputer. [Azure PowerShell downloaden](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps).
*   Een juist geconfigureerde taakdefinitie in een StorSimple Data Manager-service binnen een resourcegroep.
*   Download [ `DataTransformationApp.zip` ](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/raw/master/Azure%20Automation%20For%20Data%20Manager/DataTransformationApp.zip) -bestand van de GitHub-opslagplaats. 
*   Download [ `Trigger-DataTransformation-Job.ps1` ](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Trigger-DataTransformation-Job.ps1) script uit de GitHub-opslagplaats.

## <a name="step-by-step-procedure"></a>Stapsgewijze procedure

### <a name="set-up-the-automation-account"></a>Instellen van het Automation-account

1. Een automation uitvoeren als-account maken in Azure portal. Om dit te doen, gaat u naar **Azure marketplace > Alles** en zoek vervolgens **Automation**. Selecteer **Automation-accounts**.

    ![Uitvoeren als-automation-account maken](./media/storsimple-data-manager-job-using-automation/search-automation-account1.png)

2. Als u wilt een nieuw automation-account toevoegen, klikt u op **+ toevoegen**.

    ![Uitvoeren als-automation-account maken](./media/storsimple-data-manager-job-using-automation/add-automation-account1.png)

3. In de **Automation toevoegen**:

    1. Geef de **naam** van uw automation-account.
    2. Selecteer de **abonnement** gekoppeld aan uw StorSimple Data Manager-service.
    3. Maak een nieuwe resourcegroep of een bestaande resourcegroep selecteren.
    4. Selecteer een **locatie**.
    5. Laat de standaardwaarde **uitvoeren als-account maken** optie is geselecteerd.
    6. Ga voor een koppeling voor snelle toegang op het dashboard **vastmaken aan dashboard**. Klik op **Create**.

    ![Uitvoeren als-automation-account maken](./media/storsimple-data-manager-job-using-automation/create-automation-run-as-account.png)
    
    Nadat het automation-account is gemaakt, wordt u gewaarschuwd.
    
    ![Melding voor de implementatie van automation-account](./media/storsimple-data-manager-job-using-automation/deployment-automation-account-notification1.png)

    Ga voor meer informatie naar [uitvoeren als-account maken](../automation/automation-create-runas-account.md).

3. In het nieuwe account, gaat u naar **gedeelde Resources > Modules** en klikt u op **+ toevoegen module**.

    ![1-module importeren](./media/storsimple-data-manager-job-using-automation/import-module-1.png)

4. Blader naar de locatie van `DataTransformationApp.zip` vanaf uw lokale computer, het bestand en selecteer en open de module. Klik op **OK** om de module te importeren.

    ![2-module importeren](./media/storsimple-data-manager-job-using-automation/import-module-2.png)

   Wanneer Azure Automation een module in uw account importeert, pakt deze metagegevens over de module. Deze bewerking kan enkele minuten duren.

   ![4-module importeren](./media/storsimple-data-manager-job-using-automation/import-module-4.png)

5. U ontvangt een melding dat de module wordt geïmplementeerd en nog een melding wanneer het proces voltooid is.  De status in **Modules** wordt gewijzigd in **beschikbaar**.

    ![5-module importeren](./media/storsimple-data-manager-job-using-automation/import-module-5.png)

### <a name="import-publish-and-run-automation-runbook"></a>Importeren, publiceren en Automation-runbook uitvoeren

Voer de volgende stappen uit als u wilt importeren, publiceren en uitvoeren van het runbook voor het activeren van de taakdefinitie.

1. Open uw Automation-account in Azure Portal. Ga naar **procesautomatisering > Runbooks** en klikt u op **+ toevoegen van een runbook**.

    ![Runbook 1 toevoegen](./media/storsimple-data-manager-job-using-automation/add-runbook-1.png)

2. In **runbook toevoegen**, klikt u op **een bestaand runbook importeren**.

3. Verwijzen naar de Azure PowerShell-scriptbestand `Trigger-DataTransformation-Job.ps1` voor de **Runbook-bestand**. Het runbooktype wordt automatisch geselecteerd. Geef een naam en een optionele beschrijving voor het runbook. Klik op **Create**.

    ![Runbook 2 toevoegen](./media/storsimple-data-manager-job-using-automation/add-runbook-2.png)

4. Het nieuwe runbook wordt weergegeven in de lijst van runbooks voor het Automation-account. Selecteer en klikt u op dit runbook.

    ![Runbook 3 toevoegen](./media/storsimple-data-manager-job-using-automation/add-runbook-3.png)

5. Bewerk het runbook en klik op **Test** deelvenster.

    ![Runbook 4 toevoegen](./media/storsimple-data-manager-job-using-automation/add-runbook-4.png)

6. Geef de parameters op, zoals de naam van uw StorSimple Data Manager-service, de bijbehorende resourcegroep en de naam van de taak. **Start** de test. Het rapport wordt gegenereerd wanneer de uitvoering voltooid is. Voor meer informatie gaat u naar het [een runbook testen](../automation/automation-first-runbook-textual-powershell.md#step-3---test-the-runbook).

    ![Runbook 8 toevoegen](./media/storsimple-data-manager-job-using-automation/add-runbook-8.png)    

7. Inspecteer de uitvoer van het runbook in het deelvenster. Als voldaan, sluit u het deelvenster. Klik op **publiceren** en wanneer u hierom wordt gevraagd om bevestiging, bevestigen en het runbook publiceren.

    ![Runbook 6 toevoegen](./media/storsimple-data-manager-job-using-automation/add-runbook-6.png)

8. Ga terug naar **Runbooks** en selecteer het zojuist gemaakte runbook.

    ![Runbook 7 toevoegen](./media/storsimple-data-manager-job-using-automation/add-runbook-7.png)

9. **Start** het runbook. In **runbook starten**, alle parameters opgeven. Klik op **OK** indienen en start de taak voor gegevenstransformatie.

10. Voor het controleren van de voortgang van de taak in Azure portal, gaat u naar **taken** in uw StorSimple Data Manager-service. Selecteer en klik op de taak om de taakdetails weer te geven.

    ![Runbook 10 toevoegen](./media/storsimple-data-manager-job-using-automation/add-runbook-10.png)

## <a name="next-steps"></a>Volgende stappen

[Gebruik StorSimple Data Manager UI om uw gegevens te transformeren](storsimple-data-manager-ui.md).
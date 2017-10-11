---
title: Gebruik Azure Automation om een taak te activeren | Microsoft Docs
description: Informatie over het gebruik van Azure Automation voor activering van StorSimple Data Manager-taken (afgeschermd voorbeeld)
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/16/2017
ms.author: vidarmsft
ms.openlocfilehash: 9691408bcd80afb6eba534f26749b76dd3bfe315
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-automation-to-trigger-a-job-private-preview"></a>Gebruik Azure Automation voor het activeren van een taak (afgeschermd voorbeeld)

Dit artikel wordt beschreven hoe u Azure Automation gebruikt om een StorSimple Data Manager-taak te activeren.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u hebt voordat u begint:

*   Azure Powershell is geïnstalleerd. [Download de Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
*   Configuratie-instellingen voor het initialiseren van de taak gegevenstransformatie (instructies voor het verkrijgen van deze instellingen zijn opgenomen hier).
*   De taakdefinitie van een die correct is geconfigureerd in een hybride Gegevensresource binnen een resourcegroep.
*   Download `DataTransformationApp.zip` [zip](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/raw/master/Azure%20Automation%20For%20Data%20Manager/DataTransformationApp.zip) bestand van de github-opslagplaats.
*   Download `Get-ConfigurationParams.ps1` [script](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Get-ConfigurationParams.ps1) uit de github-opslagplaats.
*   Download `Trigger-DataTransformation-Job.ps1` [script](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Trigger-DataTransformation-Job.ps1) uit de github-opslagplaats.

## <a name="step-by-step"></a>Stapsgewijs

### <a name="get-azure-active-directory-permissions-for-the-automation-job-to-run-the-job-definition"></a>Azure Active Directory-machtigingen voor het uitvoeren van de taakdefinitie automation-taak ophalen

1. Voor het ophalen van de configuratieparameters voor Active Directory, voer de volgende stappen uit:

    1. Open Windows PowerShell in uw lokale computer. Zorg ervoor dat [Azure PowerShell](https://azure.microsoft.com/downloads/) is geïnstalleerd.
    1. Voer de `Get-ConfigurationParams.ps1` script (in de map die u hebt gedownload hierboven). Typ de volgende opdracht in het venster PowerShell:

        ```
        .\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```

        De ActiveDirectoryKey is een wachtwoord dat u later gebruiken. Voer een wachtwoord van uw keuze. AppName kan een willekeurige tekenreeks zijn.

2. Dit script levert de volgende waarden die moeten worden gebruikt tijdens de activering van de automation-runbook. Noteer deze waarden.

    - Client-ID
    - Tenant-id
    - Active Directory-sleutel (zelfde als een dat hierboven is opgegeven)
    - Abonnements-id

### <a name="set-up-the-automation-account"></a>Het Automation-Account instellen

1. Meld u aan bij Azure en open uw Automation-account.
2. Klik op **activa** tegel om de lijst van assets te openen.
3. Klik op **Modules** tegel te openen van de lijst met modules.
4. Klik op **+ toevoegen van een module** knop en de blade van de module toevoegen wordt gestart.

    ![Instellingen voor Automation-account](./media/storsimple-data-manager-job-using-automation/add-module1m.png)

5. Nadat u hebt geselecteerd de `DataTransformationApp.zip` bestand vanaf uw lokale computer, klikt u op **OK** de module te importeren.

   Wanneer u een module Azure Automation importeert in uw account, pakt deze metagegevens over de module. Deze bewerking kan enkele minuten duren.

   ![Instellingen voor Automation-account](./media/storsimple-data-manager-job-using-automation/add-module2m.png)

   

6. U ontvangt een melding dat de module wordt geïmplementeerd en nog een melding wanneer het proces voltooid is.  U kunt ook de status controleren **Modules** tegel.

### <a name="to-import-the-runbook-that-triggers-the-job-definition"></a>Voor het importeren van het runbook waarmee de taakdefinitie wordt geactiveerd

1. Open uw Automation-account in Azure Portal.
2. Klik op **Runbooks** tegel om de lijst van runbooks te openen.
3. Klik op **+ een runbook toevoegen** en vervolgens **een bestaand runbook importeren**.

   ![Een bestaand runbook importeren](./media/storsimple-data-manager-job-using-automation/import-a-runbook.png)

4. Klik op **Runbook bestand** en selecteer de te importeren bestand `Trigger-DataTransformation-Job.ps1`.
5. Klik op **maken** voor het importeren van het runbook. Het nieuwe runbook wordt weergegeven in de lijst met runbooks voor het Automation-account.
7. Klik op **Trigger-DataTransformation-Job** runbook en klik vervolgens op **bewerken**.
8. Klik op **publiceren** en vervolgens **Ja** wanneer u om bevestiging wordt gevraagd.


### <a name="to-run-the-runbook"></a>Het runbook uitvoeren:
1. Open uw Automation-account in Azure Portal.
2. Klik op de tegel **Runbooks** om de lijst met runbooks te openen.
3. Klik op **Trigger-DataTransformation-Job**.
4. Klik op **Starten** om het runbook te starten.

   ![Runbook starten](./media/storsimple-data-manager-job-using-automation/run-runbook1m.png)

5. In de **runbook starten** blade alle parameters opgeven. Klik op **OK** de taak Gegevenstransformatie verzenden.

   ![Runbook starten](./media/storsimple-data-manager-job-using-automation/run-runbook2m.png)


## <a name="next-steps"></a>Volgende stappen

[Gebruik StorSimple Data Manager UI om uw gegevens te transformeren](storsimple-data-manager-ui.md).
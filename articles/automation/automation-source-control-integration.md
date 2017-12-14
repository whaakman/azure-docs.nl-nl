---
title: Bron van de integratie van broncodebeheer in Azure Automation | Microsoft Docs
description: Integratie van broncodebeheer met GitHub in Azure Automation beschreven.
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 224d7375-9887-44dd-b137-06ffe396a4b4
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/29/2017
ms.author: magoedte;sngun
ms.openlocfilehash: bb1ce4ceaa3d0c9aea014fc810ea269641dec14c
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="source-control-integration-in-azure-automation"></a>Integratie van broncodebeheer in Azure Automation
Integratie van broncodebeheer kunt u runbooks in uw Automation-account aan een GitHub-resourcebeheerbibliotheek koppelen. Broncodebeheer kunt u eenvoudig samenwerken met uw team, het bijhouden van wijzigingen en terugkeren naar eerdere versies van uw runbooks. Broncodebeheer kunt u verschillende filialen in besturingselement voor de ontwikkeling, testen of productie Automation-accounts, zodat u eenvoudig ter bevordering van de code die in uw ontwikkelomgeving, zodat uw productie Automation-account is getest synchroniseren.

Broncodebeheer kunt u code pushen van Azure Automation met resourcebeheer of uw runbooks vanuit resourcebeheer voor Azure Automation pull. In dit artikel wordt beschreven hoe resourcebeheer in uw Azure Automation-omgeving instellen. Begin met het configureren van Azure Automation voor toegang tot uw GitHub-opslagplaats en doorloop verschillende bewerkingen die kunnen worden uitgevoerd met behulp van de integratie van broncodebeheer. 

> [!NOTE]
> Biedt ondersteuning voor broncodebeheer ophalen en opslaan [PowerShell Workflow-runbooks](automation-runbook-types.md#powershell-workflow-runbooks) , evenals [PowerShell-runbooks](automation-runbook-types.md#powershell-runbooks). [Grafische runbooks](automation-runbook-types.md#graphical-runbooks) worden nog niet ondersteund.<br><br>
> 
> 

Er zijn twee eenvoudige stappen die nodig zijn voor uw Automation-account en slechts één verbinding met broncodebeheer configureren als u al een GitHub-account. Ze zijn:

## <a name="step-1--create-a-github-repository"></a>Stap 1: Maak een GitHub-opslagplaats
Als u al een GitHub-account en een locatie die u wilt koppelen aan Azure Automation, meldt u zich bij uw bestaande account en beginnen bij stap 2 hieronder. Anders gaat u naar [GitHub](https://github.com/), zich aanmelden voor een nieuw account en [maken van een nieuwe opslagplaats](https://help.github.com/articles/create-a-repo/).

## <a name="step-2--set-up-source-control-in-azure-automation"></a>Stap 2: broncodebeheer in Azure Automation instellen
1. Klik op de pagina Automation-Account in de Azure portal **broncodebeheer instellen.** 
   
    ![Resourcebeheer instellen](media/automation-source-control-integration/automation_01_SetUpSourceControl.png)
2. De **broncodebeheer** pagina wordt geopend, waar u de gegevens van uw GitHub-account kunt configureren. Hieronder volgt de lijst met te configureren parameters:  
   
   | **Parameter** | **Beschrijving** |
   |:--- |:--- |
   | Bron kiezen |Selecteer de gegevensbron. Op dit moment alleen **GitHub** wordt ondersteund. |
   | Autorisatie |Klik op de **autoriseren** knop Azure Automation toegang geven tot uw GitHub-opslagplaats. Als u bent al aangemeld bij uw GitHub-account in een ander venster, wordt de referenties van het account gebruikt. Wanneer autorisatie voltooid is, ziet de pagina uw GitHub-gebruikersnaam onder **autorisatie eigenschap**. |
   | Kies de opslagplaats |Selecteer een GitHub-opslagplaats in de lijst met beschikbare opslagplaatsen. |
   | Kies vertakking |Selecteer een vertakking uit de lijst met beschikbare vertakkingen. Alleen de **master** vertakking wordt weergegeven als u eventuele vertakkingen dat nog niet hebt gemaakt. |
   | Pad naar de Runbookmap |Het pad naar de runbookmap geeft het pad in de GitHub-opslagplaats van waaruit u wilt push of pull uw code. Dit moet worden ingevoerd in de indeling **/mapnaam/submapnaam**. Alleen runbooks in het pad naar de runbookmap worden met uw Automation-account gesynchroniseerd. Runbooks in submappen van het pad naar de runbookmap wordt **niet** worden gesynchroniseerd. Gebruik  **/**  om alle runbooks in de opslagplaats te synchroniseren. |
3. Bijvoorbeeld, als er een opslagplaats met de naam **PowerShellScripts** die een map met de naam bevat **RootFolder**, die een map met de naam bevat **submap**. U kunt de volgende tekenreeksen elke mapniveau synchroniseren:
   
   1. Voor synchronisatie runbooks van **opslagplaats**, pad naar de runbookmap is*/*
   2. Voor synchronisatie runbooks van **RootFolder**, pad naar de runbookmap is */RootFolder*
   3. Voor synchronisatie runbooks van **submap**, pad naar de runbookmap is */RootFolder/submap*.
4. Nadat u de parameters hebt geconfigureerd, worden weergegeven op de **broncodebeheer instellen** pagina.  
   
    ![Besturingselement bronpagina configureren](media/automation-source-control-integration/automation_02_SourceControlConfigure.png)
5. Nadat u op **OK**, integratie van broncodebeheer is nu geconfigureerd voor uw Automation-account en moet worden bijgewerkt met uw GitHub-gegevens. U kunt nu op dit gedeelte om alle uw Taakgeschiedenis bron besturingselement synchronisatie weer te klikken.  
   
    ![Waarden van de opslagplaats](media/automation-source-control-integration/automation_03_RepoValues.png)
6. Nadat u verbinding met broncodebeheer hebt ingesteld, wordt de volgende Automation-resources in uw Automation-account worden gemaakt:  
   Twee [variabele assets](automation-variables.md) worden gemaakt.  
   
   * De variabele **Microsoft.Azure.Automation.SourceControl.Connection** bevat de waarden van de verbindingsreeks, zoals hieronder wordt weergegeven.  
     
     | **Parameter** | **Waarde** |
     |:--- |:--- |
     | Naam |Microsoft.Azure.Automation.SourceControl.Connection |
     | Type |Reeks |
     | Waarde |{{'Vertakking':\<*de naam van uw vertakking*>, 'RunbookFolderPath':\<*pad naar de Runbookmap*>, 'ProviderType':\<*heeft de waarde 1 voor GitHub*>, 'Opslagplaats':\<*naam van uw opslagplaats*>, 'Gebruikersnaam':\<*uw GitHub-gebruikersnaam*>} |

    * De variabele **Microsoft.Azure.Automation.SourceControl.OAuthToken**, bevat de veilig versleutelde waarde van uw OAuthToken.  

    |**Parameter**            |**Waarde** |
    |:---|:---|
    | Naam  | Microsoft.Azure.Automation.SourceControl.OAuthToken |
    | Type | Unknown(Encrypted) |
    | Waarde | <*Versleutelde OAuthToken*> |  

    ![Variabelen](media/automation-source-control-integration/automation_04_Variables.png)  

    * **Automation-bronbeheer** wordt toegevoegd als een geautoriseerde toepassing naar uw GitHub-account. Om de toepassing weer te geven: vanaf uw GitHub-startpagina, gaat u naar uw **profiel** > **instellingen** > **toepassingen**. Deze toepassing kunnen Azure Automation om te synchroniseren van uw GitHub-opslagplaats voor een Automation-account.  

    ![GIT-toepassing](media/automation-source-control-integration/automation_05_GitApplication.png)


## <a name="using-source-control-in-automation"></a>Met behulp van broncodebeheer in Automation
### <a name="check-in-a-runbook-from-azure-automation-to-source-control"></a>Controleren in een runbook uit Azure Automation voor bronbeheer
Runbook inchecken kunt u de wijzigingen die u hebt aangebracht in een runbook in Azure Automation in uw resourcebeheerbibliotheek push. Hieronder volgen de stappen om te controleren in een runbook:

1. Van uw Automation-Account [maken van nieuwe tekstueel runbook](automation-first-runbook-textual.md), of [bewerken van een bestaande, tekstgegevens runbook](automation-edit-textual-runbook.md). Dit runbook is een PowerShell-werkstroom of een PowerShell-script runbook.  
2. Nadat u uw runbook hebt bewerkt, opslaan en klik op **inchecken** van de **bewerken** pagina.  
   
    ![Knop inchecken](media/automation-source-control-integration/automation_06_CheckinButton.png)

     > [!NOTE] 
     > Inchecken van Azure Automation wordt overschreven door de code die momenteel aanwezig is in uw broncodebeheer. De Git equivalente opdrachtregelprogramma instructie voor controle is **git toevoegen + git-doorvoer + git push**  

1. Wanneer u klikt op **inchecken**, wordt u gevraagd een bevestigingsbericht, klik op **Ja** om door te gaan.  
   
    ![Inchecken bericht](media/automation-source-control-integration/automation_07_CheckinMessage.png)
2. Controle begint het besturingselement bron runbook: **Sync MicrosoftAzureAutomationAccountToGitHubV1**. Dit runbook maakt verbinding met GitHub en wijzigingen van Azure Automation verstuurd naar uw opslagplaats. Als u wilt de ingeschakelde in Taakgeschiedenis weergeven, gaat u terug naar de **integratie van broncodebeheer** tabblad en klik op om de synchronisatie van de opslagplaats pagina te openen. Deze pagina geeft alle uw resourcebeheertaken.  Selecteer de taak die u wilt weergeven en klikt u op om de details weer te geven.  
   
    ![Inchecken Runbook](media/automation-source-control-integration/automation_08_CheckinRunbook.png)
   
   > [!NOTE]
   > Bron besturingselement runbooks zijn speciale Automation-runbooks die u kunt weergeven of bewerken. Terwijl ze niet in de lijst met runbook weergegeven worden, ziet u synchronisatietaken weergegeven in de takenlijst met.
   > 
   > 
3. De naam van het gewijzigde runbook wordt verzonden als invoerparameter voor het geselecteerde in runbook. U kunt [de taakdetails weergeven](automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) door het uitbreiden van runbook in **opslagplaats synchronisatie** pagina.  
   
    ![Inchecken invoer](media/automation-source-control-integration/automation_09_CheckinInput.png)
4. Vernieuw uw GitHub-opslagplaats zodra de taak is voltooid om de wijzigingen weer te geven.  Er moet een doorvoeren in de opslagplaats met een commit-bericht: **bijgewerkt *Runbooknaam* in Azure Automation.**  

### <a name="sync-runbooks-from-source-control-to-azure-automation"></a>Runbooks synchroniseren vanuit resourcebeheer voor Azure Automation
De knop synchronisatie op de pagina opslagplaats synchronisatie kunt u voor het ophalen van alle runbooks in het pad naar de runbookmap van uw opslagplaats voor uw Automation-account. Dezelfde opslagplaats kan worden gesynchroniseerd met meer dan een Automation-account. Hieronder vindt u de stappen voor het synchroniseren van een runbook:

1. Open in het Automation-account waarmee u verbinding met broncodebeheer instellen, de **bron besturingselement integratie/opslagplaats synchronisatie** pagina en klik op **Sync**.  U wordt gevraagd met een bevestigingsbericht wordt weergegeven, klikt u op **Ja** om door te gaan.  
   
    ![Knop synchronisatie](media/automation-source-control-integration/automation_10_SyncButtonwithMessage.png)
2. Het runbook door synchronisatie wordt gestart: **Sync MicrosoftAzureAutomationAccountFromGitHubV1**. Dit runbook maakt verbinding met GitHub en haalt de wijzigingen van uw opslagplaats voor Azure Automation. U ziet een nieuwe taak op de **opslagplaats synchronisatie** pagina voor deze actie. Gedetailleerde informatie over de sync-taak, klik op om de pagina met details van de taak.  
   
    ![Runbook synchroniseren](media/automation-source-control-integration/automation_11_SyncRunbook.png)

    > [!NOTE] 
    > Een synchroniseren vanuit resourcebeheer overschrijft de conceptversie van de runbooks die momenteel beschikbaar zijn in uw Automation-account voor **alle** besturingselement voor runbooks die momenteel in de gegevensbron. De Git equivalente opdrachtregelprogramma instructie voor het synchroniseren is **git, pull**


## <a name="troubleshooting-source-control-problems"></a>Het oplossen van problemen van bron-besturingselement
Als er fouten met een vinkje in of sync-taak, de status van de taak moet worden onderbroken en vindt u meer informatie over de fout op de pagina van de taak.  De **alle logboeken** deel ziet u alle PowerShell-gegevensstromen die zijn gekoppeld met deze taak. Dit biedt u de details om u te helpen u bij het oplossen van problemen met uw inchecken of synchronisatie. Ook ziet u de volgorde van de acties die zijn opgetreden tijdens het synchroniseren of een runbook incheckt.  

![Afbeelding van AllLogs](media/automation-source-control-integration/automation_13_AllLogs.png)

## <a name="disconnecting-source-control"></a>Verbinding met broncodebeheer verbreken
Als u wilt loskoppelen van uw GitHub-account, de opslagplaats synchronisatie openen en klik op **Disconnect**. Wanneer u verbinding met broncodebeheer verbreken, runbooks die eerder zijn gesynchroniseerd, nog steeds in uw Automation-account, maar de pagina opslagplaats synchronisatie wordt niet ingeschakeld.  

  ![Knop verbinding verbreken](media/automation-source-control-integration/automation_12_Disconnect.png)

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie over de integratie van broncodebeheer:  

* [Azure Automation: Integratie van broncodebeheer in Azure Automation](https://azure.microsoft.com/blog/azure-automation-source-control-13/)  
* [Stem voor uw favoriete bronbeheersysteem](https://www.surveymonkey.com/r/?sm=2dVjdcrCPFdT0dFFI8nUdQ%3d%3d)  
* [Azure Automation: Integratie van broncodebeheer Runbook met behulp van Visual Studio Team Services](https://azure.microsoft.com/blog/azure-automation-integrating-runbook-source-control-using-visual-studio-online/)  


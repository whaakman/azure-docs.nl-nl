---
title: Integratie van broncodebeheer in Azure Automation - verouderd
description: Dit artikel wordt de integratie van broncodebeheer met GitHub in Azure Automation beschreven.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/25/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: edc38296374538fd708a74f575d5b0c77770c005
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57901539"
---
# <a name="source-control-integration-in-azure-automation---legacy"></a>Integratie van broncodebeheer in Azure Automation - verouderd

> [!NOTE]
> Er is een nieuwe ervaring voor broncodebeheer. Zie voor meer informatie over de nieuwe ervaring, [broncodebeheer (Preview)](source-control-integration.md).

Integratie van broncodebeheer kunt u runbooks in uw Automation-account naar een opslagplaats voor bronbeheer GitHub koppelen. Broncodebeheer kunt u eenvoudig samenwerken met uw team, het bijhouden van wijzigingen en Ga terug naar de eerdere versies van uw runbooks. Bijvoorbeeld, kunt broncodebeheer u verschillende branches in broncodebeheer aan uw ontwikkelings-, test- of productieomgeving Automation-accounts, zodat u eenvoudig kunt ter bevordering van code die in uw ontwikkelomgeving, zodat uw productie-automatisering is getest synchroniseren -account.

Broncodebeheer kunt u code in Azure Automation naar broncodebeheer pushen of pull uw runbooks vanuit bronbeheer naar Azure Automation. Dit artikel wordt beschreven hoe u voor het instellen van broncodebeheer in uw Azure Automation-omgeving. We beginnen door het configureren van Azure Automation om toegang tot uw GitHub-opslagplaats en doorloopt u verschillende bewerkingen die kunnen worden uitgevoerd met behulp van de integratie van broncodebeheer. 

> [!NOTE]
> Broncodebeheer ondersteunt pullen en pushen [PowerShell Workflow-runbooks](automation-runbook-types.md#powershell-workflow-runbooks) , evenals [PowerShell-runbooks](automation-runbook-types.md#powershell-runbooks). [Grafische runbooks](automation-runbook-types.md#graphical-runbooks) worden nog niet ondersteund.<br><br>
> 
> 

Er zijn twee eenvoudige stappen die nodig zijn voor het configureren van broncodebeheer voor uw Automation-account en maximaal één adapter als u al een GitHub-account hebt. Dit zijn:

## <a name="step-1--create-a-github-repository"></a>Stap 1: een GitHub-opslagplaats maken
Als u al hebt een GitHub-account en een opslagplaats die u wilt koppelen aan Azure Automation, meldt u zich bij uw bestaande account en beginnen bij stap 2 hieronder. Anders gaat u naar [GitHub](https://github.com/), zich aanmelden voor een nieuw account en [maken van een nieuwe opslagplaats](https://help.github.com/articles/create-a-repo/).

## <a name="step-2--set-up-source-control-in-azure-automation"></a>Stap 2 – instellen van broncodebeheer in Azure Automation
1. Van het Automation-Account pagina in de Azure-portal onder **Accountinstellingen**, klikt u op **broncodebeheer.** 
   
1. De **broncodebeheer** pagina wordt geopend, waarin u de details van uw GitHub-account kunt configureren. Hieronder volgt de lijst met te configureren parameters:  
   
   | **Parameter** | **Beschrijving** |
   |:--- |:--- |
   | Bron kiezen |Selecteer de gegevensbron. Op dit moment alleen **GitHub** wordt ondersteund. |
   | Autorisatie |Klik op de **autoriseren** knop Azure Automation toegang verlenen tot uw GitHub-opslagplaats. Als u al bent aangemeld bij uw GitHub-account in een ander venster, klikt u vervolgens de referenties van dat account gebruikt. Wanneer autorisatie voltooid is, de pagina ziet u uw GitHub-gebruikersnaam onder **autorisatie-eigenschap**. |
   | Bibliotheek kiezen |Selecteer een GitHub-opslagplaats in de lijst met beschikbare opslagplaatsen. |
   | Kies vertakking |Selecteer een vertakking in de lijst met beschikbare vertakkingen wilt bekijken. Alleen de **master** vertakking wordt weergegeven als u vertakkingen nog niet hebt gemaakt. |
   | Pad naar de Runbookmap |Het pad naar de runbookmap geeft het pad in de GitHub-opslagplaats van waaruit u wilt push of pull uw code. Dit moet worden ingevoerd in de indeling **/mapnaam/submapnaam**. Alleen runbooks in het pad naar de runbookmap zal worden gesynchroniseerd met uw Automation-account. Runbooks in submappen van het pad naar de runbookmap wordt, **niet** worden gesynchroniseerd. Gebruik **/** om te synchroniseren van alle runbooks in de opslagplaats. |
3. Bijvoorbeeld, hebt u een opslagplaats met de naam **PowerShellScripts** die een map met de naam bevat **RootFolder**, die een map met de naam bevat **submap**. U kunt de volgende tekenreeksen gebruiken om te synchroniseren van elk niveau van de map:
   
   1. Voor synchronisatie runbooks uit **opslagplaats**, pad naar de runbookmap is */*
   2. Voor synchronisatie runbooks uit **RootFolder**, pad naar de runbookmap is */RootFolder*
   3. Voor synchronisatie runbooks uit **submap**, pad naar de runbookmap is */RootFolder/submap*.
4. Nadat u de parameters hebt geconfigureerd, worden ze weergegeven op de **broncodebeheer instellen** pagina.  
   
    ![Bron beheren pagina configureren](media/automation-source-control-integration-legacy/automation_02_SourceControlConfigure.png)
5. Nadat u op **OK**, integratie van bronbeheer is nu geconfigureerd voor uw Automation-account en moet worden bijgewerkt met uw GitHub-gegevens. U kunt nu op dit gedeelte om al uw Taakgeschiedenis source control sync weer te klikken.  
   
    ![Waarden van de opslagplaats](media/automation-source-control-integration-legacy/automation_03_RepoValues.png)
6. Nadat u bronnenbeheer instelt, wordt de volgende Automation-resources worden gemaakt in uw Automation-account:  
   Twee [variabele assets](automation-variables.md) worden gemaakt.  
   
   * De variabele **Microsoft.Azure.Automation.SourceControl.Connection** bevat de waarden van de verbindingsreeks, zoals hieronder wordt weergegeven.  
     
     | **Parameter** | **Waarde** |
     |:--- |:--- |
     | Name |Microsoft.Azure.Automation.SourceControl.Connection |
     | Type |String |
     | Value |{'Vertakking':\<*de naam van uw vertakking*>, "RunbookFolderPath":\<*pad naar de Runbookmap*>, "ProviderType":\<*heeft een waarde 1 voor GitHub*>, 'Opslagplaats':\<*naam van uw opslagplaats*>, 'Gebruikersnaam':\<*uw GitHub-gebruikersnaam*>} |

     * De variabele **Microsoft.Azure.Automation.SourceControl.OAuthToken**, bevat de veilige versleutelde waarde van uw OAuthToken.  

     |**Parameter**            |**Waarde** |
     |:---|:---|
     | Name  | Microsoft.Azure.Automation.SourceControl.OAuthToken |
     | Type | Unknown(Encrypted) |
     | Value | <*Versleutelde OAuthToken*> |  

     ![Variabelen](media/automation-source-control-integration-legacy/automation_04_Variables.png)  

     * **Automation-broncodebeheer** wordt toegevoegd als een gemachtigde toepassing naar uw GitHub-account. De toepassing weergeven: Vanuit uw GitHub-startpagina, navigeert u naar uw **profiel** > **instellingen** > **toepassingen**. Deze toepassing kunt Azure Automation om te synchroniseren van uw GitHub-opslagplaats aan een Automation-account.  

     ![GIT-toepassing](media/automation-source-control-integration-legacy/automation_05_GitApplication.png)


## <a name="using-source-control-in-automation"></a>Met behulp van broncodebeheer in Automation
### <a name="check-in-a-runbook-from-azure-automation-to-source-control"></a>Controleren in een runbook in Azure Automation voor broncodebeheer
Runbook inchecken kunt u de wijzigingen die u hebt aangebracht in een runbook in Azure Automation in uw resourcebeheerbibliotheek. Hieronder volgen de stappen om te controleren in een runbook:

1. In uw Automation-Account [maken van een nieuw runbook tekstuele](automation-first-runbook-textual.md), of [bewerken van een bestaande, tekstuele runbook](automation-edit-textual-runbook.md). Dit runbook is een PowerShell-werkstroom of een PowerShell-script-runbook.  
2. Nadat u uw runbook bewerken, opslaan en klikt u op **inchecken** uit de **bewerken** pagina.  
   
    ![Knop inchecken](media/automation-source-control-integration-legacy/automation_06_CheckinButton.png)

     > [!NOTE] 
     > Inchecken van Azure Automation overschrijft de code die momenteel aanwezig is in uw broncodebeheer. De equivalente Git-opdrachtregelinstructie inchecken is **git toevoegen + git-doorvoer + git push**  

1. Wanneer u klikt op **inchecken**, wordt u gevraagd met een bevestigingsbericht wordt weergegeven, klikt u op **Ja** om door te gaan.  
   
    ![Check-inbericht](media/automation-source-control-integration-legacy/automation_07_CheckinMessage.png)
2. Start het besturingselement bron runbook inchecken: **Sync-MicrosoftAzureAutomationAccountToGitHubV1**. Dit runbook maakt verbinding met GitHub en wijzigingen in Azure Automation gepusht naar uw opslagplaats. Als u wilt de ingeschakeld in de taakgeschiedenis bekijken, gaat u terug naar de **integratie van broncodebeheer** tabblad en klik op om de synchronisatie van de opslagplaats-pagina te openen. Deze pagina bevat alle uw resourcebeheertaken.  Selecteer de taak die u wilt weergeven en klikt u op om de details weer te geven.  
   
    ![Runbook inchecken](media/automation-source-control-integration-legacy/automation_08_CheckinRunbook.png)
   
   > [!NOTE]
   > Bron besturingselement runbooks zijn speciale Automation-runbooks die u niet kunt weergeven of bewerken. Terwijl ze niet in de runbooklijst met weergegeven worden, ziet u synchronisatietaken weergegeven in de takenlijst met.
   > 
   > 
3. De naam van het gewijzigde runbook wordt verzonden als invoerparameter voor de geselecteerde in runbook. U kunt [de taakdetails weergeven](automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) door het uitbreiden van runbook in **Bibliotheeksynchronisatie** pagina.  
   
    ![Inchecken invoer](media/automation-source-control-integration-legacy/automation_09_CheckinInput.png)
4. Uw GitHub-opslagplaats vernieuwen nadat de taak is voltooid om de wijzigingen weer te geven.  Er moet een wijziging in uw opslagplaats met een commit-bericht: **Bijgewerkt *Runbooknaam* in Azure Automation.**  

### <a name="sync-runbooks-from-source-control-to-azure-automation"></a>Runbooks synchroniseren vanuit bronbeheer naar Azure Automation
De knop synchroniseren op de pagina Bibliotheeksynchronisatie kunt u voor het ophalen van alle runbooks in het pad naar de runbookmap van uw opslagplaats naar uw Automation-account. Dezelfde opslagplaats kan worden gesynchroniseerd met meer dan één Automation-account. Hieronder vindt u de stappen voor het synchroniseren van een runbook:

1. Open in het Automation-account waar u bronnenbeheer instelt, de **Source Control integratie/Bibliotheeksynchronisatie** pagina en klik op **synchronisatie**.  U wordt gevraagd met een bevestigingsbericht wordt weergegeven, klikt u op **Ja** om door te gaan.  
   
    ![Knop synchroniseren](media/automation-source-control-integration-legacy/automation_10_SyncButtonwithMessage.png)
2. Synchronisatie van begint het runbook: **Sync-MicrosoftAzureAutomationAccountFromGitHubV1**. Dit runbook maakt verbinding met GitHub en haalt de wijzigingen uit uw opslagplaats voor Azure Automation. U ziet een nieuwe taak op de **Bibliotheeksynchronisatie** pagina voor deze actie. Als u wilt meer informatie over de sync-taak weergeven, klikt u op om te openen van de pagina met details van de taak.  
   
    ![Synchronisatie-Runbook](media/automation-source-control-integration-legacy/automation_11_SyncRunbook.png)

    > [!NOTE] 
    > Een synchronisatie vanuit broncodebeheer overschrijft de conceptversie van de runbooks die momenteel aanwezig zijn in uw Automation-account voor **alle** besturingselement voor runbooks die momenteel in de gegevensbron. De Git equivalente opdrachtregelprogramma instructies voor het synchroniseren is **git pull**

![Afbeelding van AllLogs](media/automation-source-control-integration-legacy/automation_13_AllLogs.png)

## <a name="disconnecting-source-control"></a>Verbinding met broncodebeheer verbreken
Als u wilt loskoppelen van uw GitHub-account, open de pagina Bibliotheeksynchronisatie en klikt u op **verbinding verbreken**. Nadat u de verbinding met broncodebeheer verbreken, runbooks die eerder zijn gesynchroniseerd blijven de boekjaargegevens in uw Automation-account, maar de synchronisatie van de opslagplaats-pagina wordt niet ingeschakeld.  

  ![Knop verbinding verbreken](media/automation-source-control-integration-legacy/automation_12_Disconnect.png)

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de integratie van broncodebeheer, de volgende bronnen:  

* [Azure Automation: Integratie van broncodebeheer in Azure Automation](https://azure.microsoft.com/blog/azure-automation-source-control-13/)  
* [Stem voor uw favoriete bronbeheersysteem](https://www.surveymonkey.com/r/?sm=2dVjdcrCPFdT0dFFI8nUdQ%3d%3d)  
* [Azure Automation: Integratie van broncodebeheer van Runbook met behulp van Azure DevOps](https://azure.microsoft.com/blog/azure-automation-integrating-runbook-source-control-using-visual-studio-online/)  



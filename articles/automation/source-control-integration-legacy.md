---
title: Integratie van broncodebeheer in Azure Automation - verouderd
description: Dit artikel wordt de integratie van broncodebeheer met GitHub in Azure Automation beschreven.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/01/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: c95af40c3fa3f9dad2bfb5ea4a1b9f585c636928
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58806893"
---
# <a name="source-control-integration-in-azure-automation---legacy"></a>Integratie van broncodebeheer in Azure Automation - verouderd

> [!NOTE]
> Er is een nieuwe ervaring voor broncodebeheer. Zie voor meer informatie over de nieuwe ervaring, [broncodebeheer (Preview)](source-control-integration.md).

Integratie van broncodebeheer kunt u runbooks in uw Automation-account naar een opslagplaats voor bronbeheer GitHub koppelen. Broncodebeheer kunt u eenvoudig samenwerken met uw team, het bijhouden van wijzigingen en Ga terug naar de eerdere versies van uw runbooks. Bijvoorbeeld, kunt broncodebeheer u verschillende branches in broncodebeheer aan uw ontwikkelings-, test- of productieomgeving Automation-accounts, zodat u eenvoudig kunt ter bevordering van code die in uw ontwikkelomgeving, zodat uw productie-automatisering is getest synchroniseren -account.

Broncodebeheer kunt u code in Azure Automation naar broncodebeheer pushen of pull uw runbooks vanuit bronbeheer naar Azure Automation. Dit artikel wordt beschreven hoe u voor het instellen van broncodebeheer in uw Azure Automation-omgeving. We beginnen door het configureren van Azure Automation om toegang tot uw GitHub-opslagplaats en doorloopt u verschillende bewerkingen die kunnen worden uitgevoerd met behulp van de integratie van broncodebeheer. 

> [!NOTE]
> Broncodebeheer ondersteunt pullen en pushen [PowerShell Workflow-runbooks](automation-runbook-types.md#powershell-workflow-runbooks) , evenals [PowerShell-runbooks](automation-runbook-types.md#powershell-runbooks). [Grafische runbooks](automation-runbook-types.md#graphical-runbooks) worden nog niet ondersteund.

Er zijn twee eenvoudige stappen die nodig zijn voor het configureren van broncodebeheer voor uw Automation-account en maximaal één adapter als u al een GitHub-account hebt. Dit zijn:

## <a name="step-1--create-a-github-repository"></a>Stap 1: een GitHub-opslagplaats maken

Als u al hebt een GitHub-account en een opslagplaats die u wilt koppelen aan Azure Automation, meldt u zich bij uw bestaande account en beginnen bij stap 2 hieronder. Anders gaat u naar [GitHub](https://github.com/), zich aanmelden voor een nieuw account en [maken van een nieuwe opslagplaats](https://help.github.com/articles/create-a-repo/).

## <a name="step-2--set-up-source-control-in-azure-automation"></a>Stap 2 – instellen van broncodebeheer in Azure Automation

1. Van het Automation-Account pagina in de Azure-portal onder **Accountinstellingen**, klikt u op **broncodebeheer.**

2. De **broncodebeheer** pagina wordt geopend, waarin u de details van uw GitHub-account kunt configureren. Hieronder volgt de lijst met te configureren parameters:  

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

    ![De pagina voor het beheer van bron met de instellingen](media/source-control-integration-legacy/automation-SourceControlConfigure.png)
5. Nadat u op **OK**, integratie van bronbeheer is nu geconfigureerd voor uw Automation-account en moet worden bijgewerkt met uw GitHub-gegevens. U kunt nu op dit gedeelte om al uw Taakgeschiedenis source control sync weer te klikken.  

    ![Waarden voor de huidige geconfigureerde bronbeheerconfiguratie](media/source-control-integration-legacy/automation-RepoValues.png)
6. Na het instellen van broncodebeheer, twee [variabele assets](automation-variables.md) worden gemaakt in uw Automation-account. Bovendien wordt een gemachtigde toepassing toegevoegd aan uw GitHub-account.

   * De variabele **Microsoft.Azure.Automation.SourceControl.Connection** bevat de waarden van de verbindingsreeks, zoals hieronder wordt weergegeven.  

     | **Parameter** | **Waarde** |
     |:--- |:--- |
     | `Name`  |Microsoft.Azure.Automation.SourceControl.Connection |
     | `Type`  |String |
     | `Value` |{'Vertakking':\<*de naam van uw vertakking*>, "RunbookFolderPath":\<*pad naar de Runbookmap*>, "ProviderType":\<*heeft een waarde 1 voor GitHub*>, 'Opslagplaats':\<*naam van uw opslagplaats*>, 'Gebruikersnaam':\<*uw GitHub-gebruikersnaam*>} |

   * De variabele **Microsoft.Azure.Automation.SourceControl.OAuthToken**, bevat de veilige versleutelde waarde van uw OAuthToken.  

     |**Parameter**            |**Waarde** |
     |:---|:---|
     | `Name`  | Microsoft.Azure.Automation.SourceControl.OAuthToken |
     | `Type`  | Unknown(Encrypted) |
     | `Value` | <*Versleutelde OAuthToken*> |  

     ![Een venster weergegeven waarin bron besturingselement variabelen](media/source-control-integration-legacy/automation-Variables.png)  

   * **Automation-broncodebeheer** wordt toegevoegd als een gemachtigde toepassing naar uw GitHub-account. De toepassing weergeven: Vanuit uw GitHub-startpagina, navigeert u naar uw **profiel** > **instellingen** > **toepassingen**. Deze toepassing kunt Azure Automation om te synchroniseren van uw GitHub-opslagplaats aan een Automation-account.  

     ![Toepassingsinstellingen in GitHub](media/source-control-integration-legacy/automation-GitApplication.png)

## <a name="using-source-control-in-automation"></a>Met behulp van broncodebeheer in Automation

### <a name="check-in-a-runbook-from-azure-automation-to-source-control"></a>Controleren in een runbook in Azure Automation voor broncodebeheer

Runbook inchecken kunt u de wijzigingen die u hebt aangebracht in een runbook in Azure Automation in uw resourcebeheerbibliotheek. Hieronder volgen de stappen om te controleren in een runbook:

1. In uw Automation-Account [maken van een nieuw runbook tekstuele](automation-first-runbook-textual.md), of [bewerken van een bestaande, tekstuele runbook](automation-edit-textual-runbook.md). Dit runbook is een PowerShell-werkstroom of een PowerShell-script-runbook.  
2. Nadat u uw runbook bewerken, opslaan en klikt u op **inchecken** uit de **bewerken** pagina.  

    ![Een venster weergegeven waarin het inchecken op GitHub-knop](media/source-control-integration-legacy/automation-CheckinButton.png)

     > [!NOTE] 
     > Inchecken van Azure Automation overschrijft de code die momenteel aanwezig is in uw broncodebeheer. De equivalente Git-opdrachtregelinstructie inchecken is **git toevoegen + git-doorvoer + git push**  

3. Wanneer u klikt op **inchecken**, wordt u gevraagd met een bevestigingsbericht wordt weergegeven, klikt u op **Ja** om door te gaan.  

    ![Een dialoogvenster voor bevestiging van het inchecken aan bronbeheer](media/source-control-integration-legacy/automation-CheckinMessage.png)
4. Start het besturingselement bron runbook inchecken: **Sync-MicrosoftAzureAutomationAccountToGitHubV1**. Dit runbook maakt verbinding met GitHub en wijzigingen in Azure Automation gepusht naar uw opslagplaats. Als u wilt de ingeschakeld in de taakgeschiedenis bekijken, gaat u terug naar de **integratie van broncodebeheer** tabblad en klik op om de synchronisatie van de opslagplaats-pagina te openen. Deze pagina bevat alle uw resourcebeheertaken.  Selecteer de taak die u wilt weergeven en klikt u op om de details weer te geven.  

    ![Een venster weergegeven waarin de resultaten van een sync-taak](media/source-control-integration-legacy/automation-CheckinRunbook.png)

   > [!NOTE]
   > Bron besturingselement runbooks zijn speciale Automation-runbooks die u niet kunt weergeven of bewerken. Terwijl ze niet in de runbooklijst met weergegeven worden, ziet u synchronisatietaken weergegeven in de takenlijst met.

5. De naam van het gewijzigde runbook wordt verzonden als invoerparameter voor de geselecteerde in runbook. U kunt [de taakdetails weergeven](automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) door het uitbreiden van runbook in **Bibliotheeksynchronisatie** pagina.  

    ![Een venster weergegeven waarin de invoer voor een sync-taak](media/source-control-integration-legacy/automation-CheckinInput.png)
6. Uw GitHub-opslagplaats vernieuwen nadat de taak is voltooid om de wijzigingen weer te geven.  Er moet een wijziging in uw opslagplaats met een commit-bericht: **Bijgewerkt *Runbooknaam* in Azure Automation.**  

### <a name="sync-runbooks-from-source-control-to-azure-automation"></a>Runbooks synchroniseren vanuit bronbeheer naar Azure Automation

De knop synchroniseren op de pagina Bibliotheeksynchronisatie kunt u voor het ophalen van alle runbooks in het pad naar de runbookmap van uw opslagplaats naar uw Automation-account. Dezelfde opslagplaats kan worden gesynchroniseerd met meer dan één Automation-account. Hieronder vindt u de stappen voor het synchroniseren van een runbook:

1. Open in het Automation-account waar u bronnenbeheer instelt, de **Source Control integratie/Bibliotheeksynchronisatie** pagina en klik op **synchronisatie**.  U wordt gevraagd met een bevestigingsbericht wordt weergegeven, klikt u op **Ja** om door te gaan.  

    ![De knop synchroniseren met een bevestigingsbericht weergegeven dat alle runbooks wordt gesynchroniseerd](media/source-control-integration-legacy/automation-SyncButtonwithMessage.png)

2. Synchronisatie van begint het runbook: **Sync-MicrosoftAzureAutomationAccountFromGitHubV1**. Dit runbook maakt verbinding met GitHub en haalt de wijzigingen uit uw opslagplaats voor Azure Automation. U ziet een nieuwe taak op de **Bibliotheeksynchronisatie** pagina voor deze actie. Als u wilt meer informatie over de sync-taak weergeven, klikt u op om te openen van de pagina met details van de taak.  

    ![Een venster weergegeven waarin de Synchronisatieresultaten van een sync-taak op een GitHub-opslagplaats](media/source-control-integration-legacy/automation-SyncRunbook.png)

    > [!NOTE]
    > Een synchronisatie vanuit broncodebeheer overschrijft de conceptversie van de runbooks die momenteel aanwezig zijn in uw Automation-account voor **alle** besturingselement voor runbooks die momenteel in de gegevensbron. De Git equivalente opdrachtregelprogramma instructies voor het synchroniseren is **git pull**

![Een venster weergegeven waarin alle logboeken van een onderbroken source control sync-taak](media/source-control-integration-legacy/automation-AllLogs.png)

## <a name="disconnecting-source-control"></a>Verbinding met broncodebeheer verbreken

Als u wilt loskoppelen van uw GitHub-account, open de pagina Bibliotheeksynchronisatie en klikt u op **verbinding verbreken**. Nadat u de verbinding met broncodebeheer verbreken, runbooks die eerder zijn gesynchroniseerd blijven de boekjaargegevens in uw Automation-account, maar de synchronisatie van de opslagplaats-pagina wordt niet ingeschakeld.  

  ![Een venster met de knop verbinding verbreken de verbinding met broncodebeheer verbreken](media/source-control-integration-legacy/automation-Disconnect.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de integratie van broncodebeheer, de volgende bronnen:  

* [Azure Automation: Integratie van broncodebeheer in Azure Automation](https://azure.microsoft.com/blog/azure-automation-source-control-13/)  
* [Stem voor uw favoriete bronbeheersysteem](https://www.surveymonkey.com/r/?sm=2dVjdcrCPFdT0dFFI8nUdQ%3d%3d)  
* [Azure Automation: Integratie van broncodebeheer van Runbook met behulp van Azure DevOps](https://azure.microsoft.com/blog/azure-automation-integrating-runbook-source-control-using-visual-studio-online/)  

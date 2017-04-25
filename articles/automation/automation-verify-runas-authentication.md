---
title: Een Azure Automation-account valideren | Microsoft Docs
description: In dit artikel wordt beschreven hoe u kunt controleren of uw Automation-account juist is geconfigureerd.
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/14/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 5bed2616e15a2e5f52e79d0c28159b568e7a1de3
ms.lasthandoff: 04/15/2017

---

# <a name="test-azure-automation-run-as-account-authentication"></a>De verificatie van een Azure Automation Uitvoeren als-account testen
Nadat een Automation-account is gemaakt, kunt u een eenvoudige test uitvoeren om te controleren of u zich met het zojuist gemaakte of bijgewerkte Automation Uitvoeren als-account kunt verifiëren in Azure Resource Manager of de klassieke Azure-implementatie.    

## <a name="automation-run-as-authentication"></a>Automation Uitvoeren als-verificatie

1. Open in Azure Portal het Automation-account dat u eerder hebt gemaakt.  
2. Klik op de tegel **Runbooks** om de lijst met runbooks te openen.
3. Selecteer het runbook **AzureAutomationTutorialScript** en klik op **Start** om het runbook te starten.  U ontvangt een prompt waarin u wordt gevraagd of u het runbook wilt starten.
4. Er wordt een [runbooktaak](automation-runbook-execution.md) gemaakt, de blade Taak wordt weergegeven en de taakstatus wordt weergegeven in de tegel **Taaksamenvatting**.  
5. In eerste instantie is de taakstatus *In de wachtrij geplaatst*. Hiermee wordt aangegeven dat er wordt gewacht tot in de cloud een runbook-werkrol beschikbaar is. De taakstatus verandert daarna in *Starten* wanneer een werkrol de taak claimt en daarna in *Wordt uitgevoerd* wanneer het runbook daadwerkelijk wordt uitgevoerd.  
6. Wanneer de runbooktaak is voltooid, is de status **Voltooid**.<br> ![Runbooktest beveiligingsprincipal](media/automation-verify-runas-authentication/job-summary-automationtutorialscript.png)<br>
7. Klik op de tegel **Uitvoer** als u de gedetailleerde resultaten van het runbook wilt bekijken.
8. Op de blade **Uitvoer** ziet u dat de verificatie is geslaagd. Er wordt ook een lijst met alle beschikbare resources in de resourcegroep weergegeven.
9. Sluit de blade **Uitvoer** om terug te keren naar de blade **Taaksamenvatting**.
10. Sluit de blade **Taaksamenvatting** en het bijbehorende runbook **AzureAutomationTutorialScript**.

## <a name="classic-run-as-authentication"></a>Klassieke Uitvoeren als-verificatie
Als u resources gaat beheren in het klassieke implementatiemodel, voert u de volgende stappen uit om te controleren of u zich met het nieuwe klassieke Uitvoeren als-account kunt verifiëren.     

1. Open in Azure Portal het Automation-account dat u eerder hebt gemaakt.  
2. Klik op de tegel **Runbooks** om de lijst met runbooks te openen.
3. Selecteer het runbook **AzureClassicAutomationTutorialScript** en klik op **Start** om het runbook te starten.  U ontvangt een prompt waarin u wordt gevraagd of u het runbook wilt starten.
4. Er wordt een [runbooktaak](automation-runbook-execution.md) gemaakt, de blade Taak wordt weergegeven en de taakstatus wordt weergegeven in de tegel **Taaksamenvatting**.  
5. In eerste instantie is de taakstatus *In de wachtrij geplaatst*. Hiermee wordt aangegeven dat er wordt gewacht tot in de cloud een runbook-werkrol beschikbaar is. De taakstatus verandert daarna in *Starten* wanneer een werkrol de taak claimt en daarna in *Wordt uitgevoerd* wanneer het runbook daadwerkelijk wordt uitgevoerd.  
6. Wanneer de runbooktaak is voltooid, is de status **Voltooid**.<br><br> ![Runbooktest beveiligingsprincipal](media/automation-verify-runas-authentication/job-summary-automationclassictutorialscript.png)<br>  
7. Klik op de tegel **Uitvoer** als u de gedetailleerde resultaten van het runbook wilt bekijken.
8. Op de blade **Uitvoer** ziet u dat de verificatie is geslaagd. Er wordt ook een lijst met alle klassieke virtuele machines in het abonnement weergegeven.
9. Sluit de blade **Uitvoer** om terug te keren naar de blade **Taaksamenvatting**.
10. Sluit de blade **Taaksamenvatting** en de bijbehorende runbookblade **AzureClassicAutomationTutorialScript**.

## <a name="next-steps"></a>Volgende stappen
* Zie [Mijn eerste PowerShell-runbook](automation-first-runbook-textual-powershell.md) om aan de slag te gaan met PowerShell-runbooks.
* Zie voor meer informatie over grafisch ontwerpen [Grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md).


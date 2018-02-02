---
title: Azure Automation Hybrid Runbook Workers verwijderen | Microsoft Docs
description: "In dit artikel bevat informatie over het beheren van geïmplementeerde Azure Automation Hybrid Runbook Workers kunt u runbooks worden uitgevoerd op computers in uw lokale datacentrum of cloud-omgeving."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/07/2017
ms.author: magoedte
ms.openlocfilehash: 602b868073da6bd1f64099c0f344c9b492abaff0
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="remove-azure-automation-hybrid-runbook-workers"></a>Hybrid Runbook Worker voor Azure Automation verwijderen

De Hybrid Runbook Worker-functie van Azure Automation kunt u direct op de computer die als host fungeert voor de rol en op basis van bronnen in de omgeving voor het beheren van de lokale bronnen runbooks worden uitgevoerd. In dit artikel leert u hoe een hybride worker verwijderen uit een on-premises computer.

## <a name="removing-hybrid-runbook-worker"></a>Hybride Runbook Worker verwijderen

U kunt een of meer Hybrid Runbook Workers verwijderen uit een groep of kunt u de groep, afhankelijk van uw vereisten.  Als u wilt een hybride Runbook Worker verwijderen uit een on-premises computer, moet u de volgende stappen uitvoeren.

1. Ga in de Azure-portal naar uw Automation-account.  
2. Van de **instellingen** blade Selecteer **sleutels** en noteer de waarden voor veld **URL** en **primaire toegangssleutel**.  Deze informatie moet u voor de volgende stap.
3. Open een PowerShell-sessie in de beheerdersmodus en voer de volgende opdracht - `Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey>`.  Gebruik de **-uitgebreide** overschakelen voor een gedetailleerd logboek van het verwijderingsproces.

> [!NOTE]
> Microsoft Monitoring Agent wordt niet verwijderd van de computer, alleen de functionaliteit en de configuratie van de Hybrid Runbook Worker-rol.  

## <a name="remove-hybrid-worker-groups"></a>Hybrid Worker-groepen verwijderen

Voor een groep verwijdert, moet u eerst de hybride Runbook Worker verwijderen uit elke computer die lid is van de groep met de bovenstaande procedure en voer de volgende stappen uit om de groep te verwijderen.  

1. Open het Automation-account in de Azure portal.
1. Onder **procesautomatisering** Selecteer **Hybrid Worker-groepen**. Selecteer de groep die u wilt verwijderen.  Na het selecteren van de specifieke groep de **Hybrid worker-groep** eigenschappenblade wordt weergegeven.<br> ![Blade Hybrid Runbook Worker-groep](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)   
2. Klik op de eigenschappenblade voor de geselecteerde groep **verwijderen**.  Selecteer een bericht weergegeven waarin u deze actie te bevestigen **Ja** als u zeker dat u wilt doorgaan.<br> ![Dialoogvenster voor bevestiging groep verwijderen](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)<br> Dit proces kan enkele seconden duren en u kunt de voortgang bijhouden onder **Meldingen** in het menu. 

## <a name="next-steps"></a>Volgende stappen

* Bekijk [runbooks worden uitgevoerd op een hybride Runbook Worker](automation-hrw-run-runbooks.md) voor informatie over het configureren van uw runbooks voor het automatiseren van processen in uw on-premises datacentrum of andere cloudomgeving.
* Zie voor meer informatie over het installeren van Windows Hybrid Runbook Workers [Azure Automation Windows hybride Runbook Worker](automation-windows-hrw-install.md).
* Zie voor meer informatie over het installeren van Linux Hybrid Runbook Workers [Azure Automation Linux hybride Runbook Worker](automation-linux-hrw-install.md).
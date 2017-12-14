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
ms.openlocfilehash: a897a97b9a1e259f5994a27b974eb5183fd1194b
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="remove-azure-automation-hybrid-runbook-workers"></a>Azure Automation Hybrid Runbook Workers verwijderen

U kunt 

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
1. Selecteer de **Hybrid Worker-groepen** tegel en in de **Hybrid Worker-groepen** blade, selecteer de groep die u wilt verwijderen.  Na het selecteren van de specifieke groep de **Hybrid worker-groep** eigenschappenblade wordt weergegeven.<br> ![Blade Hybrid Runbook Worker-groep](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)   
1. Klik op de eigenschappenblade voor de geselecteerde groep **verwijderen**.  Selecteer een bericht weergegeven waarin u deze actie te bevestigen **Ja** als u zeker dat u wilt doorgaan.<br> ![Dialoogvenster voor bevestiging groep verwijderen](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)<br> Dit proces kan enkele seconden duren en u kunt de voortgang bijhouden onder **Meldingen** in het menu. 

## <a name="next-steps"></a>Volgende stappen

* Bekijk [runbooks worden uitgevoerd op een hybride Runbook Worker](automation-hrw-run-runbooks.md) voor informatie over het configureren van uw runbooks voor het automatiseren van processen in uw on-premises datacentrum of andere cloudomgeving.
* Zie voor meer informatie over het installeren van Windows Hybrid Runbook Workers [Azure Automation Windows hybride Runbook Worker](automation-windows-hrw-install.md).
* Zie voor meer informatie over het installeren van Linux Hybrid Runbook Workers [Azure Automation Linux hybride Runbook Worker](automation-linux-hrw-install.md).
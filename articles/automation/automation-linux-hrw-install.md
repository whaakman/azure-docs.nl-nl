---
title: Hybrid Runbook Worker voor Azure Automation in Linux
description: In dit artikel bevat informatie over het installeren van een Azure Automation Hybrid Runbook Worker die u kunt het uitvoeren van runbooks op Linux-computers in uw lokale datacentrum of cloudomgeving.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 95f34e5d4fd966c41a30cc68c005237ae5405592
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="how-to-deploy-a-linux-hybrid-runbook-worker"></a>Het implementeren van een Linux hybride Runbook Worker

De Hybrid Runbook Worker-functie van Azure Automation kunt u direct op de computer die als host fungeert voor de rol en op basis van bronnen in de omgeving voor het beheren van de lokale bronnen runbooks worden uitgevoerd. Linux Hybrid Runbook Worker runbooks worden uitgevoerd als een speciale gebruiker die kan worden uitgebreid voor het uitvoeren van opdrachten die uitbreiding van bevoegdheden moeten. Runbooks zijn opgeslagen en beheerd in Azure Automation en vervolgens aan een of meer specifieke computers geleverd. Dit artikel decribes de hybride Runbook Worker installeren op een Linux-machine.

## <a name="supported-linux-operating-systems"></a>Ondersteunde Linux-besturingssystemen

Hier volgt een lijst met Linux-distributies die worden ondersteund:

* Amazon Linux 2012.09 2015.09 (x86/x64)-->
* CentOS Linux 5, 6 en 7 (x86/x64)
* Oracle Linux 5, 6 en 7 (x86/x64)
* Red Hat Enterprise Linux Server 5,6 en 7 (x86/x64)
* Debian GNU/Linux 6, 7 en 8 (x86/x64)
* Ubuntu 12.04 TNS, 14.04 TNS, 16.04 LTS (x86/x64)
* SUSE Linux Enteprise Server 11 en 12 (x86/x64)

## <a name="installing-linux-hybrid-runbook-worker"></a>Linux hybride Runbook Worker installeren

Als u wilt installeren en configureren van een hybride Runbook Worker op uw Linux-computer, moet u een rechte doorsturen-proces voor het handmatig installeren en configureren van de rol volgen. Het inschakelen van vereist de **Automation Hybrid Worker** oplossing in de werkruimte voor logboekanalyse en voert u een reeks opdrachten voor het registreren van de computer als een werknemer en toe te voegen aan een nieuwe of bestaande groep.

Voordat u doorgaat, moet u de werkruimte voor logboekanalyse die uw Automation-account is gekoppeld aan, en ook de primaire sleutel voor uw Automation-account. U kunt zowel vanuit de portal vinden door uw Automation-account selecteren en het selecteren van **werkruimte** voor de werkruimte-ID en het selecteren van **sleutels** voor de primaire sleutel. Zie voor informatie over de poorten en adressen die nodig zijn voor de hybride Runbook Worker, [configureren van uw netwerk](automation-hybrid-runbook-worker.md#network-planning).

1. Schakel de oplossing 'Automation Hybrid Worker' in Azure. Dit kan worden gedaan door:

   1. Toevoegen de **Automation Hybrid Worker** oplossing aan uw abonnement met de procedure op [beheeroplossingen voor logboekanalyse toevoegen aan uw werkruimte](../log-analytics/log-analytics-add-solutions.md).
   1. Voer de volgende cmdlet uit:

        ```azurepowershell-interactive
         Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName  <ResourceGroupName> -WorkspaceName <WorkspaceName> -IntelligencePackName  "AzureAutomation" -Enabled $true
        ```

1. De OMS-agent voor Linux installeren met de volgende opdracht vervangen \<WorkspaceID\> en \<WorkspaceKey\> met de juiste waarden uit uw werkruimte.

   ```bash
   wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
   ```

1. Voer de volgende opdracht, het wijzigen van de waarden voor parameters *-w*, *-k*, *-g*, en *-e*. Voor de *-g* parameter, vervang de waarde met de naam van de Hybrid Runbook Worker-groep die moet worden toegevoegd aan de nieuwe Linux hybride Runbook Worker. Als de naam niet al in uw Automation-account bestaat, wordt een nieuwe hybride Runbook Worker-groep bestaat met die naam.

   ```bash
   sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/onboarding.py --register -w <LogAnalyticsworkspaceId> -k <AutomationSharedKey> -g <hybridgroupname> -e <automationendpoint>
   ```

1. Nadat de opdracht voltooid is, de Hybrid Worker-groepen in de Azure portal ziet u de nieuwe groep en het aantal leden of als een bestaande groep, het aantal leden wordt verhoogd. U kunt de groep in de lijst selecteren op de **Hybrid Worker-groepen** pagina en selecteer de **Hybrid Workers** tegel. Op de **Hybrid Workers** pagina ziet u elk lid van de groep die wordt vermeld.

## <a name="turning-off-signature-validation"></a>Het uitschakelen van validatie van handtekening

Standaard is Linux Hybrid Runbook Workers handtekeningvalidatie nodig. Als u een niet-ondertekende runbook op basis van een werknemer uitvoeren, ziet u een fout met "Handtekeningvalidatie is mislukt". Als wilt uitschakelen handtekeningvalidatie, voer de volgende opdracht, de tweede parameter vervangen door uw Log Analytics-werkruimte-ID:

 ```bash
 sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --false <LogAnalyticsworkspaceId>
 ```

## <a name="supported-runbook-types"></a>Ondersteunde runbooktypen

Linux Hybrid Runbook Workers bieden geen ondersteuning voor de volledige set van runbooktypen die zijn gevonden binnen Azure automation.

De volgende runbooktypen werken op een Linux hybride Worker:

* Python 2
* PowerShell

De volgende runbooktypen werken niet op een Linux hybride Worker:

* PowerShell-werkstroom
* Grafische
* Grafische PowerShell Workflow

## <a name="next-steps"></a>Volgende stappen

* Bekijk [runbooks worden uitgevoerd op een hybride Runbook Worker](automation-hrw-run-runbooks.md) voor informatie over het configureren van uw runbooks voor het automatiseren van processen in uw on-premises datacentrum of andere cloudomgeving.
* Zie voor instructies over het verwijderen van Hybrid Runbook Workers [verwijderen Azure Automation Hybrid Runbook Workers](automation-hybrid-runbook-worker.md#removing-hybrid-runbook-worker)

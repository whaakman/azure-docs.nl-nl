---
title: Hybrid Runbook Worker voor Azure Automation in Linux
description: In dit artikel bevat informatie over het installeren van een Azure Automation Hybrid Runbook Worker, zodat u runbooks op basis van Linux-computers in uw lokale datacenter of cloudomgeving uitvoeren kunt.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 06/28/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: cc07aa9c1b2c540c33949a8c591bd98f91b04666
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58225447"
---
# <a name="deploy-a-linux-hybrid-runbook-worker"></a>Een Linux Hybrid Runbook Worker implementeren

U kunt de functie Hybrid Runbook Worker van Azure Automation gebruiken voor het uitvoeren van runbooks rechtstreeks op de computer die als host voor de rol fungeert en op basis van bronnen in de omgeving voor het beheren van deze lokale resources. De Hybrid Runbook Worker in Linux wordt runbooks uitgevoerd als een speciale gebruiker die kan worden uitgebreid voor het uitvoeren van opdrachten die bevoegdheden moeten. Runbooks worden opgeslagen en beheerd in Azure Automation en vervolgens geleverd aan een of meer opgegeven computers.

In dit artikel wordt beschreven hoe u de Hybrid Runbook Worker installeren op een Linux-machine.

## <a name="supported-linux-operating-systems"></a>Ondersteunde Linux-besturingssystemen

De functie Hybrid Runbook Worker ondersteunt de volgende distributies:

* Amazon Linux 2012.09-2015.09 (x86/x64)
* CentOS Linux 5, 6 en 7 (x86/x64)
* Oracle Linux 5, 6 en 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 en 7 (x86/x64)
* Debian GNU/Linux 6, 7 en 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS en 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 en 12 (x86/x64)

## <a name="installing-a-linux-hybrid-runbook-worker"></a>Installeren van een Hybrid Runbook Worker in Linux

Als u wilt installeren en configureren van een Hybrid Runbook Worker op uw Linux-computer, moet u een eenvoudig proces voor het handmatig installeren en configureren van de rol volgen. Hiervoor inschakelen van de **Automation Hybrid Worker** oplossing in uw Azure Log Analytics-werkruimte en een set opdrachten voor het registreren van de computer als een werknemer en toe te voegen aan een groep uit te voeren.

De minimale vereisten voor een Hybrid Runbook Worker in Linux zijn:

* Twee cores
* 4 GB aan RAM-geheugen
* Poort 443 (uitgaand)

### <a name="package-requirements"></a>Pakket-vereisten

| **Vereist pakket** | **Beschrijving** | **Minimale versie**|
|--------------------- | --------------------- | -------------------|
|Glibc |GNU C-bibliotheek| 2.5-12 |
|openssl| OpenSSL-bibliotheken | 1.0 (TLS 1.1 en TLS 1.2 worden ondersteund|
|Curl | cURL webclient | 7.15.5|
|Python-ctypes | |
|PAM | Pluggable Authentication Modules|
| **Optionele pakket** | **Beschrijving** | **Minimale versie**|
| PowerShell Core | Om uit te voeren PowerShell-runbooks, moet PowerShell zijn geïnstalleerd, Zie [PowerShell Core installeren in Linux](/powershell/scripting/setup/installing-powershell-core-on-linux) voor meer informatie over hoe u deze installeert.  | 6.0.0 |

### <a name="installation"></a>Installatie

Voordat u doorgaat, houd er rekening mee de Log Analytics-werkruimte die aan uw Automation-account is gekoppeld. Let ook op de primaire sleutel voor uw Automation-account. U kunt vinden in de Azure portal door uw Automation-account te selecteren **werkruimte** voor de werkruimte-ID en het selecteren van **sleutels** voor de primaire sleutel. Zie voor meer informatie over poorten en -adressen die u nodig hebt voor de Hybrid Runbook Worker [configureren van uw netwerk](automation-hybrid-runbook-worker.md#network-planning).

1. Schakel de **Automation Hybrid Worker** oplossing in Azure met behulp van een van de volgende methoden:

   * Toevoegen de **Automation Hybrid Worker** oplossing aan uw abonnement met behulp van de procedure op [toevoegen Azure Monitor registreert oplossingen aan uw werkruimte](../log-analytics/log-analytics-add-solutions.md).
   * Voer de volgende cmdlet uit:

        ```azurepowershell-interactive
         Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName  <ResourceGroupName> -WorkspaceName <WorkspaceName> -IntelligencePackName  "AzureAutomation" -Enabled $true
        ```

1. De Log Analytics-agent voor Linux installeren met de volgende opdracht. Vervang \<WorkspaceID\> en \<WorkspaceKey\> met de juiste waarden uit uw werkruimte.

   [!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)] 

   ```bash
   wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
   ```

1. Voer de volgende opdracht, de waarden wijzigt voor de parameters *-w*, *-k*, *-g*, en *-e*. Voor de *-g* parameter, de waarde vervangt door de naam van de Hybrid Runbook Worker-groep waaraan u de nieuwe Linux Hybrid Runbook Worker wilt toevoegen. Als de naam niet in uw Automation-account bestaat, wordt een nieuwe Hybrid Runbook Worker-groep bestaat met die naam.

   ```bash
   sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/onboarding.py --register -w <LogAnalyticsworkspaceId> -k <AutomationSharedKey> -g <hybridgroupname> -e <automationendpoint>
   ```

1. Nadat de opdracht is voltooid, de **Hybrid Worker-groepen** pagina in de Azure portal ziet u de nieuwe groep en het aantal leden. Als dit een bestaande groep, wordt het aantal leden wordt verhoogd. U kunt de groep in de lijst selecteren op de **Hybrid Worker-groepen** pagina en selecteer de **Hybrid Workers** tegel. Op de **Hybrid Workers** pagina, ziet u elk lid van de groep die wordt vermeld.

> [!NOTE]
> Als u de extensie van de Azure Monitor-virtuele machine voor Linux gebruikt voor een Azure-VM wordt aangeraden instelling `autoUpgradeMinorVersion` op ONWAAR als automatisch versies upgraden kan leiden tot problemen met de Hybrid Runbook Worker. Zie voor informatie over het handmatig bijwerken van de extensie, [implementatie van Azure CLI ](../virtual-machines/extensions/oms-linux.md#azure-cli-deployment).

## <a name="turning-off-signature-validation"></a>Het uitschakelen van handtekeningvalidatie

Standaard is Linux Hybrid Runbook Workers handtekeningvalidatie nodig. Als u een niet-ondertekende runbook in een werkrol uitvoert, ziet u de foutmelding "handtekeningvalidatie is mislukt." Als u wilt uitschakelen handtekeningvalidatie, moet u de volgende opdracht uitvoeren. De tweede parameter vervangen door uw log analytics-werkruimte-ID.

 ```bash
 sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --false <LogAnalyticsworkspaceId>
 ```

## <a name="supported-runbook-types"></a>Ondersteunde runbooktypen

Linux Hybrid Runbook Workers bieden geen ondersteuning voor de volledige set runbooktypen in Azure Automation.

De volgende runbooktypen werken aan een Hybrid Worker in Linux:

* Python 2
* PowerShell

  > [!NOTE]
  > PowerShell-runbooks vereist PowerShell Core moet worden geïnstalleerd op de Linux-machine. Zie [PowerShell Core installeren in Linux](/powershell/scripting/setup/installing-powershell-core-on-linux) voor meer informatie over hoe u deze installeert.

De volgende runbooktypen werken niet op een Hybrid Worker in Linux:

* PowerShell-werkstroom
* Grafische
* Grafische PowerShell-werkstroom

## <a name="next-steps"></a>Volgende stappen

* Zie voor informatie over het configureren van uw runbooks om processen in uw on-premises datacenter of andere cloudomgeving te automatiseren, [runbooks uitvoeren op een Hybrid Runbook Worker](automation-hrw-run-runbooks.md).
* Zie voor instructies over het verwijderen van Hybrid Runbook Workers [verwijderen Azure Automation Hybrid Runbook Workers](automation-hybrid-runbook-worker.md#remove-a-hybrid-runbook-worker).
* Zie voor informatie over het oplossen van de Hybrid Runbook Workers, [probleemoplossing Linux Hybrid Runbook Workers](troubleshoot/hybrid-runbook-worker.md#linux)
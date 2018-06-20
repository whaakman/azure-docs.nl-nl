---
title: Hybrid Runbook Worker voor Azure Automation in Linux
description: In dit artikel bevat informatie over het installeren van een Azure Automation Hybrid Runbook Worker zodat u runbooks op Linux-computers in uw lokale datacentrum of cloudomgeving uitvoeren kunt.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 8507cf99ea22b24aa3026565cb7c4139e4c3742d
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268120"
---
# <a name="deploy-a-linux-hybrid-runbook-worker"></a>Een hybride Runbook Worker van Linux implementeren

U kunt de hybride Runbook Worker-functie van Azure Automation voor het uitvoeren van runbooks rechtstreeks op de computer die als host voor de rol fungeert en op basis van bronnen in de omgeving voor het beheren van de lokale bronnen gebruiken. Linux Hybrid Runbook Worker runbooks worden uitgevoerd als een speciale gebruiker die kan worden uitgebreid voor het uitvoeren van opdrachten die uitbreiding van bevoegdheden moeten. Runbooks zijn opgeslagen en beheerd in Azure Automation en vervolgens aan een of meer specifieke computers geleverd.

Dit artikel wordt beschreven hoe u de hybride Runbook Worker installeert op een Linux-machine.

## <a name="supported-linux-operating-systems"></a>Ondersteunde Linux-besturingssystemen

De Hybrid Runbook Worker-functie ondersteunt de volgende distributies:

* Amazon Linux 2012.09-2015.09 (x86/x64)
* CentOS Linux 5, 6 en 7 (x86/x64)
* Oracle Linux 5, 6 en 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 en 7 (x86/x64)
* Debian GNU/Linux 6, 7 en 8 (x86/x64)
* Ubuntu 12.04 TNS, 14.04 TNS en 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 en 12 (x86/x64)

## <a name="installing-a-linux-hybrid-runbook-worker"></a>Een Linux hybride Runbook Worker installeren

Als u wilt installeren en configureren van een hybride Runbook Worker op uw Linux-computer, moet u een eenvoudig proces voor het handmatig installeren en configureren van de rol volgen. Het inschakelen van vereist de **Automation Hybrid Worker** oplossing in uw Azure-logboekanalyse-werkruimte en voert u een reeks opdrachten voor het registreren van de computer als een werknemer en toe te voegen aan een groep.

De minimale vereisten voor een Linux hybride Runbook Worker zijn:

* Twee cores
* 4 GB RAM-geheugen
* Poort 443 (uitgaand)

### <a name="package-requirements"></a>Pakketvereisten

| **Vereist pakket** | **Beschrijving** | **Minimale versie**|
|--------------------- | --------------------- | -------------------|
|Glibc |GNU C-bibliotheek| 2.5-12 |
|Openssl| OpenSSL-bibliotheken | 0.9.8e of 1.0|
|CURL | WebClient cURL | 7.15.5|
|Python-ctypes | |
|PAM | Pluggable Authentication Modules|
| **Optioneel pakket** | **Beschrijving** | **Minimale versie**|
| PowerShell Core | Voor het uitvoeren van PowerShell-runbooks, PowerShell moet zijn geïnstalleerd, Zie [PowerShell Core installeren op Linux](/powershell/scripting/setup/installing-powershell-core-on-linux) voor meer informatie over hoe u deze installeert.  | 6.0.0 |

### <a name="installation"></a>Installatie

Let op de werkruimte voor logboekanalyse dat uw Automation-account is gekoppeld aan voordat u doorgaat. Let ook op de primaire sleutel voor uw Automation-account. U kunt vinden in de Azure-portal door te selecteren van uw Automation-account te selecteren **werkruimte** voor de werkruimte-ID en het selecteren van **sleutels** voor de primaire sleutel. Zie voor informatie over de poorten en adressen die u nodig hebt voor de hybride Runbook Worker, [configureren van uw netwerk](automation-hybrid-runbook-worker.md#network-planning).

1. Schakel de **Automation Hybrid Worker** oplossing in Azure met behulp van een van de volgende methoden:

   * Toevoegen de **Automation Hybrid Worker** oplossing voor uw abonnement via de procedure op [beheeroplossingen voor logboekanalyse toevoegen aan uw werkruimte](../log-analytics/log-analytics-add-solutions.md).
   * Voer de volgende cmdlet uit:

        ```azurepowershell-interactive
         Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName  <ResourceGroupName> -WorkspaceName <WorkspaceName> -IntelligencePackName  "AzureAutomation" -Enabled $true
        ```

1. Installeer de OMS-Agent voor Linux door de volgende opdracht uit te voeren. Vervang \<WorkspaceID\> en \<WorkspaceKey\> met de juiste waarden uit uw werkruimte.

   ```bash
   wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
   ```

1. Voer de volgende opdracht, de waarden wijzigt voor de parameters *-w*, *-k*, *-g*, en *-e*. Voor de *-g* parameter, vervang de waarde met de naam van de Hybrid Runbook Worker-groep die moet worden toegevoegd aan de nieuwe Linux hybride Runbook Worker. Als de naam niet in uw Automation-account bestaat, wordt een nieuwe hybride Runbook Worker-groep bestaat met die naam.

   ```bash
   sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/onboarding.py --register -w <LogAnalyticsworkspaceId> -k <AutomationSharedKey> -g <hybridgroupname> -e <automationendpoint>
   ```

1. Nadat de opdracht is voltooid, de **Hybrid Worker-groepen** pagina in de Azure portal wordt de nieuwe groep en het aantal leden. Als dit een bestaande groep, wordt het aantal leden wordt verhoogd. U kunt de groep in de lijst selecteren op de **Hybrid Worker-groepen** pagina en selecteer de **Hybrid Workers** tegel. Op de **Hybrid Workers** pagina ziet u elk lid van de groep die wordt vermeld.

## <a name="turning-off-signature-validation"></a>Het uitschakelen van validatie van handtekening

Standaard is Linux Hybrid Runbook Workers handtekeningvalidatie nodig. Als u een niet-ondertekende runbook op basis van een werknemer uitvoeren, ziet u een foutbericht 'handtekeningvalidatie is mislukt.' Voer de volgende opdracht om uit te schakelen handtekeningvalidatie. De tweede parameter vervangen door uw Log Analytics-werkruimte-ID.

 ```bash
 sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --false <LogAnalyticsworkspaceId>
 ```

## <a name="supported-runbook-types"></a>Ondersteunde runbooktypen

Linux Hybrid Runbook Workers ondersteunen niet de volledige set runbooktypen in Azure Automation.

De volgende runbooktypen werken op een Linux hybride Worker:

* Python 2
* PowerShell

  > [!NOTE]
  > PowerShell-runbooks vereisen PowerShell Core moet worden geïnstalleerd op de Linux-machine. Zie [PowerShell Core installeren op Linux](/powershell/scripting/setup/installing-powershell-core-on-linux) voor meer informatie over hoe u deze installeert.

De volgende runbooktypen werken op een Linux Hybrid Worker niet:

* PowerShell-werkstroom
* Grafische
* Grafische PowerShell Workflow

## <a name="troubleshooting"></a>Problemen oplossen

De Linux hybride Runbook Worker is afhankelijk van de OMS-Agent voor Linux om te communiceren met uw Automation-account voor het registreren van de werknemer, ontvangen van runbooktaken en status rapporteren. Als de registratie van de werknemer is mislukt, volgen hier enkele mogelijke oorzaken voor de fout.

### <a name="the-oms-agent-for-linux-isnt-running"></a>De OMS-Agent voor Linux wordt niet uitgevoerd

Als de OMS-Agent voor Linux wordt uitgevoerd, wordt de Linux hybride Runbook Worker kan niet communiceren met Azure Automation. Controleren of de agent wordt uitgevoerd met de opdracht `ps -ef | grep python`. 

Ziet u uitvoer die vergelijkbaar zijn met de volgende (de Python verwerkt met de **nxautomation** gebruikersaccount). Als de Update Management of Azure Automation-oplossing niet is ingeschakeld, wordt geen van de volgende processen worden uitgevoerd.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

De volgende processen, worden gestart voor een Linux hybride Runbook Worker. Ze zijn alle zich op de `/var/opt/microsoft/omsagent/state/automationworker/` directory.

* **OMS.conf**: dit is het werkproces manager. Deze rechtstreeks van Desired State Configuration (DSC) gestart.

* **worker.conf**: dit is het automatisch geregistreerd Hybrid Worker-proces. Het wordt gestart door de manager van de werknemer. Dit proces wordt gebruikt door updatebeheer en is transparant voor de gebruiker. Dit proces is alleen aanwezig als de oplossing voor beheer van de Update is ingeschakeld op de machine.

* **diy/worker.conf**: dit is het werkproces ZELFOPLOSSING hybride. Het werkproces ZELFOPLOSSING hybride wordt gebruikt voor het uitvoeren van runbooks van de gebruiker op de hybride Runbook Worker. Deze verschilt van het werkproces automatisch geregistreerd hybride alleen in dat gebruikmaakt van een andere configuratie. Dit proces is aanwezig als de Azure Automation-oplossing is ingeschakeld en de ZELFOPLOSSING Linux hybride Worker is geregistreerd.

Als de OMS-Agent voor Linux wordt niet uitgevoerd, de volgende opdracht uitvoeren om de service te starten: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

### <a name="the-specified-class-doesnt-exist"></a>De opgegeven klasse bestaat niet

Als u de fout 'de opgegeven klasse bestaat niet"in `/var/opt/microsoft/omsconfig/omsconfig.log`, de OMS-Agent voor Linux moet worden bijgewerkt. Voer de volgende opdracht om de OMS-Agent opnieuw installeren:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

Zie voor aanvullende stappen over het oplossen van problemen met updatebeheer [updatebeheer: probleemoplossing](automation-update-management.md#troubleshooting).

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het configureren van uw runbooks voor het automatiseren van processen in uw on-premises datacentrum of andere cloudomgeving, [runbooks worden uitgevoerd op een hybride Runbook Worker](automation-hrw-run-runbooks.md).
* Zie voor instructies over het verwijderen van Hybrid Runbook Workers [verwijderen Azure Automation Hybrid Runbook Workers](automation-hybrid-runbook-worker.md#remove-a-hybrid-runbook-worker).

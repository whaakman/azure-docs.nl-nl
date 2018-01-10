---
title: Problemen met Azure Log Analytics VM-extensie | Microsoft Docs
description: Beschrijf de symptomen, de oorzaken en de oplossing voor de meest voorkomende problemen met de Log Analytics VM-extensie voor Windows en Linux Azure Virtual machines.
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/08/2018
ms.author: magoedte
ms.openlocfilehash: 4e43c7a7cea903a2e94e60a519f6ead1e6f932e3
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="troubleshooting-the-log-analytics-vm-extension"></a>Het oplossen van de Log Analytics VM-extensie
Dit artikel bevat help het oplossen van problemen die u mogelijk ondervindt met de Log Analytics VM-extensie voor Windows en Linux virtuele machines die worden uitgevoerd op Microsoft Azure en mogelijke oplossingen om op te lossen ze voorgesteld.

Om te controleren of de status van de extensie, moet u de volgende stappen uitvoeren vanuit de Azure-portal.

1. Meld u aan bij de [Azure Portal](http://portal.azure.com).
2. Klik in Azure Portal op **Meer services** in de linkerbenedenhoek. Typ in de lijst met resources **virtuele machines**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **virtuele machines**.
3. Zoek en selecteer deze in de lijst met virtuele machines.
3. Klik op de virtuele machine, **extensies**.
4. Controleer in de lijst als de Log Analytics-extensie is ingeschakeld.  Voor Linux, de agent wordt vermeld als **OMSAgentforLinux** en voor Windows, de agent wordt vermeld als **MicrosoftMonitoringAgent**.

   ![VM-extensie weergeven](./media/log-analytics-azure-vmext-troubleshoot/log-analytics-vmview-extensions.png)

4. Klik op de extensie om details te bekijken. 

   ![Details van de VM-extensie](./media/log-analytics-azure-vmext-troubleshoot/log-analytics-vmview-extensiondetails.png)

## <a name="troubleshooting-azure-windows-vm-extension"></a>Het oplossen van de virtuele machine van Windows Azure-extensie

Als de *Microsoft Monitoring Agent* VM-extensie is niet geïnstalleerd of rapportage, kunt u de volgende stappen uit om het probleem oplossen uitvoeren.

1. Controleer of de Azure VM-agent is geïnstalleerd en werken goed met behulp van de stappen in [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).
   * U kunt ook het logboekbestand van de VM-agent bekijken`C:\WindowsAzure\logs\WaAppAgent.log`
   * Als het logboek niet bestaat, wordt de VM-agent is niet geïnstalleerd.
   * [De Azure VM-Agent installeren](log-analytics-quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)
2. Bevestig op dat de Microsoft Monitoring Agent-extensie heartbeat-taak wordt uitgevoerd met behulp van de volgende stappen uit:
   * Aanmelden bij de virtuele machine
   * Open Taakplanner en zoek de `update_azureoperationalinsight_agent_heartbeat` taak
   * Controleer de taak is ingeschakeld en wordt uitgevoerd elke één minuut
   * Controleer het logboekbestand heartbeat in`C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`
3. Raadpleeg de logboekbestanden van de Microsoft Monitoring Agent VM-extensie in`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
4. Zorg ervoor dat de virtuele machine kunt PowerShell-scripts uitvoeren
5. Zorg ervoor dat de machtigingen op C:\Windows\temp nog niet zijn gewijzigd.
6. De status van de Microsoft Monitoring Agent weergeven door het volgende te typen in een PowerShell-venster met verhoogde bevoegdheid op de virtuele machine`  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
7. Raadpleeg de installatielogboekbestanden in Microsoft Monitoring Agent in`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

Zie voor meer informatie [probleemoplossing van Windows-uitbreidingen](../virtual-machines/windows/extensions-oms.md).

## <a name="troubleshooting-linux-vm-extension"></a>Het oplossen van Linux VM-extensie
Als de *OMS-Agent voor Linux* VM-extensie is niet geïnstalleerd of rapportage, kunt u de volgende stappen uit om het probleem oplossen uitvoeren.

1. Als de status van de extensie *onbekende* Controleer of de Azure VM-agent is geïnstalleerd en correct werkt aan de hand van het logboekbestand van de VM-agent`/var/log/waagent.log`
   * Als het logboek niet bestaat, wordt de VM-agent is niet geïnstalleerd.
   * [De Azure VM-Agent installeren op virtuele Linux-machines](log-analytics-quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)
2. Bekijk de OMS-Agent voor andere slechte status voor Linux VM-extensie de logboekbestanden worden opgeslagen `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` en`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Als de status van extensie in orde is, maar de gegevens niet worden er geüpload controleren voor Linux-logboekbestanden in de OMS-Agent`/var/opt/microsoft/omsagent/log/omsagent.log`

Zie voor meer informatie [probleemoplossing Linux extensies](../virtual-machines/linux/extensions-oms.md).

## <a name="next-steps"></a>Volgende stappen

Zie voor aanvullende probleemoplossing richtlijnen die betrekking hebben op de OMS-Agent voor Linux op computers buiten Azure worden gehost, [oplossen Azure Log Analytics Linux Agent](log-analytics-agent-linux-support.md).  

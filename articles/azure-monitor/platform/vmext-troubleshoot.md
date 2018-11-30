---
title: Problemen met Azure Log Analytics VM-extensie oplossen | Microsoft Docs
description: Beschrijf de symptomen, de oorzaken en de oplossing voor de meest voorkomende problemen met de Log Analytics VM-extensie voor Windows en Linux Azure-VM's.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/08/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 9907983a901062e5adf622fc6620f5f8432f6a87
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52642345"
---
# <a name="troubleshooting-the-log-analytics-vm-extension"></a>Het oplossen van de Log Analytics VM-extensie
Dit artikel bevat informatie over het oplossen van fouten die u mogelijk ondervindt met de Log Analytics VM-extensie voor Windows en Linux virtuele machines die worden uitgevoerd op Microsoft Azure en duidt op mogelijke oplossingen om op te lossen.

Als u wilt controleren of de status van de extensie, moet u de volgende stappen uitvoeren vanuit de Azure-portal.

1. Meld u aan bij de [Azure Portal](http://portal.azure.com).
2. Klik in Azure Portal op **Alle services**. Typ in de lijst met resources **virtuele machines**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **virtuele machines**.
3. In de lijst met virtuele machines, zoek en selecteer deze.
3. Klik op de virtuele machine, **extensies**.
4. Controleer of de extensie van Log Analytics is ingeschakeld of niet in de lijst.  Voor Linux, de agent wordt vermeld als **OMSAgentforLinux** en voor Windows, de agent wordt vermeld als **MicrosoftMonitoringAgent**.

   ![Weergave van VM-extensie](./media/vmext-troubleshoot/log-analytics-vmview-extensions.png)

4. Klik op de extensie voor meer informatie. 

   ![Details van de VM-extensie](./media/vmext-troubleshoot/log-analytics-vmview-extensiondetails.png)

## <a name="troubleshooting-azure-windows-vm-extension"></a>Oplossen van problemen met Azure Windows VM-extensie

Als de *Microsoft Monitoring Agent* VM-extensie is niet geïnstalleerd of rapportage, kunt u de volgende stappen uit om het probleem oplossen uitvoeren.

1. Controleer of de Azure VM-agent is geïnstalleerd en werkt goed met behulp van de stappen in [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).
   * U kunt ook het logboekbestand van de VM-agent bekijken `C:\WindowsAzure\logs\WaAppAgent.log`
   * Als het logboek niet bestaat, wordt de VM-agent is niet geïnstalleerd.
   * [De Azure VM-Agent installeren](../../log-analytics/log-analytics-quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)
2. Controleer of dat de Microsoft Monitoring Agent-extensie heartbeat-taak wordt uitgevoerd met behulp van de volgende stappen uit:
   * Meld u aan bij de virtuele machine
   * Open de Taakplanner en zoek de `update_azureoperationalinsight_agent_heartbeat` taak
   * Controleer of de taak is ingeschakeld en wordt elke minuut uitgevoerd
   * Controleer het logboekbestand heartbeat in `C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`
3. Bekijk de logboekbestanden van de Microsoft Monitoring Agent-VM-extensie in `C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
4. Zorg ervoor dat de virtuele machine kunt u PowerShell-scripts uitvoeren
5. Controleer of de machtigingen op C:\Windows\temp nog niet is gewijzigd
6. De status van de Microsoft Monitoring Agent weergeven door het volgende te typen in een verhoogde PowerShell-venster op de virtuele machine `  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
7. Bekijk de logboekbestanden voor setup van Microsoft Monitoring Agent in `C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

Zie voor meer informatie, [Windows-extensies oplossen](../../virtual-machines/extensions/oms-windows.md).

## <a name="troubleshooting-linux-vm-extension"></a>Linux VM-extensie oplossen
[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 
Als de *Log Analytics-agent voor Linux* VM-extensie is niet geïnstalleerd of rapportage, kunt u de volgende stappen uit om het probleem oplossen uitvoeren.

1. Als de status van de extensie *onbekende* Controleer of de Azure VM-agent is geïnstalleerd en correct werkt aan de hand van het logboekbestand van de VM-agent `/var/log/waagent.log`
   * Als het logboek niet bestaat, wordt de VM-agent is niet geïnstalleerd.
   * [De Azure VM-Agent installeren op virtuele Linux-machines](../../log-analytics/log-analytics-quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)
2. Raadpleeg voor andere slechte status, de Log Analytics-agent voor Linux VM-extensie de logboekbestanden worden opgeslagen `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` en `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Als de status van de extensie in orde is, maar gegevens is niet geüpload. Controleer de Log Analytics-agent voor Linux-logboekbestanden in `/var/opt/microsoft/omsagent/log/omsagent.log`

Zie voor meer informatie, [Linux-extensies oplossen](../../virtual-machines/extensions/oms-linux.md).

## <a name="next-steps"></a>Volgende stappen

Aanvullende richtlijnen voor probleemoplossing met betrekking tot de Log Analytics-agent voor Linux die op computers buiten Azure worden gehost, Zie [oplossen Azure Log Analytics Linux Agent](agent-linux-troubleshoot.md).  

---
title: Azure Monitor-virtuele machine-extensie voor Linux | Microsoft Docs
description: De Log Analytics-agent op Linux-machine met behulp van de extensie van een virtuele machine implementeren.
services: virtual-machines-linux
documentationcenter: ''
author: roiyz-msft
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: c7bbf210-7d71-4a37-ba47-9c74567a9ea6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/01/2019
ms.author: roiyz
ms.openlocfilehash: 7d8192a3b6ff732481a4d48f6e188b4bb3989cda
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705938"
---
# <a name="azure-monitor-virtual-machine-extension-for-linux"></a>Azure Monitor-virtuele machine-extensie voor Linux

## <a name="overview"></a>Overzicht

Logboeken in Azure Monitor biedt mogelijkheden voor bewaking, waarschuwingen en meldingen herstel via cloud en on-premises assets. De extensie voor de Log Analytics-Agent van een virtuele machine voor Linux is gepubliceerd en ondersteund door Microsoft. De extensie voor de Log Analytics-agent geïnstalleerd op virtuele Azure-machines en virtuele machines voor een bestaande Log Analytics-werkruimte worden ingeschreven. In dit document worden de ondersteunde platforms, configuraties en implementatie-opties voor de extensie van de Azure Monitor-virtuele machine voor Linux.

>[!NOTE]
>Als onderdeel van de lopende overgang van Microsoft Operations Management Suite (OMS) naar Azure Monitor, wordt de OMS-Agent voor Windows of Linux worden aangeduid als de Log Analytics-agent voor Windows en de Log Analytics-agent voor Linux.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem

De Log Analytics-Agent-extensie kan voor deze Linux-distributies worden uitgevoerd.

| Distributie | Versie |
|---|---|
| CentOS Linux | 6 (x86/x64) en 7 (x 64) |
| Amazon-Linux | 2017.09 (x64) | 
| Oracle Linux | 6 en 7 (x86/x64) |
| Red Hat Enterprise Linux Server | 6 (x86/x64) en 7 (x 64) |
| Debian GNU/Linux | 8 en 9 (x86/x64) |
| Ubuntu | 14.04 LTS (x86/x64), 16.04 LTS (x86/x64) en 18.04 LTS (x64) |
| SUSE Linux Enterprise Server | 12 (x 64) en 15 (x 64) |

>[!NOTE]
>OpenSSL lager is dan versie 1.x wordt niet ondersteund op elk platform en versie 1.10 wordt alleen ondersteund op x86_64 platforms (64-bits).  
>

### <a name="agent-prerequisites"></a>Vereisten voor clientagents

De volgende tabel ziet u de pakketten zijn vereist voor de ondersteunde Linux-distributies die de agent worden geïnstalleerd op.

|Vereist pakket |Description |Minimale versie |
|-----------------|------------|----------------|
|Glibc |    GNU C-bibliotheek | 2.5-12 
|Openssl    | OpenSSL-bibliotheken | 1.0.x of 1.1.x |
|Curl | cURL webclient | 7.15.5 |
|Python-ctypes | | 
|PAM | Pluggable Authentication Modules | | 

>[!NOTE]
>Rsyslog of syslog-ng het volgende zijn vereist voor het verzamelen van syslog-berichten. De standaard syslog-daemon op versie 5 van Red Hat Enterprise Linux, CentOS en Oracle Linux-versie (sysklog) wordt niet ondersteund voor de verzameling van syslog. Voor het verzamelen van syslog-gegevens in deze versie van deze distributies, moet de rsyslog-daemon worden geïnstalleerd en geconfigureerd ter vervanging van sysklog.

### <a name="agent-and-vm-extension-version"></a>Versie agent en VM-extensie
De volgende tabel bevat een toewijzing van de versie van de Azure Monitor VM-extensie en bundel van Log Analytics-agent voor elke versie. Een koppeling naar de opmerkingen bij de release voor de versie van Log Analytics-agent-bundel is opgenomen. Opmerkingen bij de release bevatten informatie over oplossingen voor problemen en nieuwe functies die beschikbaar zijn voor een opgegeven agent-release.  

| Versie van de extensie Azure Monitor virtuele Linux-machine | Log Analytics-Agent bundelversie krijgt | 
|--------------------------------|--------------------------|
| 1.11.9 | [1.11.0-7](https://github.com/microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.11.0-7) |
| 1.10.0 | [1.10.0-1](https://github.com/microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.10.0-1) |
| 1.9.1 | [1.9.0-0](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.9.0-0) |
| 1.8.11 | [1.8.1-256](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.8.1.256)| 
| 1.8.0 | [1.8.0-256](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/1.8.0-256)| 
| 1.7.9 | [1.6.1-3](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.6.1.3)| 
| 1.6.42.0 | [1.6.0-42](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.6.0-42)| 
| 1.4.60.2 | [1.4.4-210](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.4-210)| 
| 1.4.59.1 | [1.4.3-174](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.3-174)|
| 1.4.58.7 | [14.2-125](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.2-125)|
| 1.4.56.5 | [1.4.2-124](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.2-124)|
| 1.4.55.4 | [1.4.1-123](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.1-123)|
| 1.4.45.3 | [1.4.1-45](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.1-45)|
| 1.4.45.2 | [1.4.0-45](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.0-45)|
| 1.3.127.5 | [1.3.5-127](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent-201705-v1.3.5-127)| 
| 1.3.127.7 | [1.3.5-127](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent-201705-v1.3.5-127)|
| 1.3.18.7 | [1.3.4-15](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent-201704-v1.3.4-15)|  

### <a name="azure-security-center"></a>Azure Security Center

Azure Security Center richt de Log Analytics-agent automatisch en verbindt deze met een standaard Log Analytics-werkruimte gemaakt door ASC in uw Azure-abonnement. Als u van Azure Security Center gebruikmaakt, niet uitgevoerd door de stappen in dit document. In dat geval wordt de geconfigureerde werkruimte overschreven en de verbinding met Azure Security Center.

### <a name="internet-connectivity"></a>Internetconnectiviteit

De Log Analytics-Agent-extensie voor Linux is vereist dat de virtuele doelmachine is verbonden met internet. 

## <a name="extension-schema"></a>Extensieschema

De volgende JSON ziet u het schema voor de Log Analytics-Agent-extensie. De extensie is vereist voor de werkruimte-ID en werkruimtesleutel van de doel-Log Analytics-werkruimte. Deze waarden kunnen worden [gevonden in uw Log Analytics-werkruimte](../../azure-monitor/learn/quick-collect-linux-computer.md#obtain-workspace-id-and-key) in Azure portal. Omdat de sleutel van de werkruimte moet worden behandeld als gevoelige gegevens, moet deze worden opgeslagen in de instellingsconfiguratie van een beveiligde. Azure-VM-extensie beveiligde instellingsgegevens versleuteld en alleen op de virtuele doelmachine worden ontsleuteld. Houd er rekening mee dat **workspaceId** en **workspaceKey** zijn hoofdlettergevoelig.

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "OMSExtension",
  "apiVersion": "2018-06-01",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.7",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

>[!NOTE]
>Het bovenstaande schema wordt ervan uitgegaan dat er op het hoogste niveau van de sjabloon worden geplaatst. Als u deze in de bron van de virtuele machine in de sjabloon, plaatst de `type` en `name` eigenschappen moeten worden gewijzigd, zoals wordt beschreven [verder omlaag](#template-deployment).
>

### <a name="property-values"></a>Waarden van eigenschappen

| Name | Waarde / voorbeeld |
| ---- | ---- |
| apiVersion | 2018-06-01 |
| publisher | Microsoft.EnterpriseCloud.Monitoring |
| type | OmsAgentForLinux |
| typeHandlerVersion | 1.7 |
| werkruimte-id (bijvoorbeeld) | 6f680a37-00c6-41c7-a93f-1437e3462574 |
| workspaceKey (bijvoorbeeld) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ== |


## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. Sjablonen zijn ideaal bij het implementeren van een of meer virtuele machines waarvoor de post-implementatieconfiguratie zoals onboarding van Azure Monitor-Logboeken. Een voorbeeld van Resource Manager-sjabloon met de Log Analytics-Agent-VM-extensie kunt u vinden op de [Azure Quick Start-galerie](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm). 

De JSON-configuratie voor een VM-extensie worden genest in de bron van de virtuele machine of geplaatst op de hoofdmap of het hoogste niveau van een Resource Manager JSON-sjabloon. De plaatsing van de JSON-configuratie is van invloed op de waarde van de resourcenaam en het type. Zie voor meer informatie, [naam en type voor de onderliggende resources instellen](../../azure-resource-manager/resource-group-authoring-templates.md#child-resources). 

Het volgende voorbeeld wordt ervan uitgegaan dat de VM-extensie is genest in de bron van de virtuele machine. Wanneer het nesten van de extensie-resource, de JSON wordt geplaatst in de `"resources": []` object van de virtuele machine.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2018-06-01",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.7",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

Bij het plaatsen van de JSON-extensie in de hoofdmap van de sjabloon, naam van de resource bevat een verwijzing naar de bovenliggende virtuele machine en het type weerspiegelt de geneste configuratie.  

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "<parentVmResource>/OMSExtension",
  "apiVersion": "2018-06-01",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.7",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

## <a name="azure-cli-deployment"></a>Azure CLI-implementatie

De Azure CLI kan worden gebruikt voor het implementeren van de Log Analytics-Agent-VM-extensie op een bestaande virtuele machine. Vervang de *workspaceId* en *workspaceKey* met die van uw Log Analytics-werkruimte. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.7 --protected-settings '{"workspaceKey": "omskey"}' \
  --settings '{"workspaceId": "omsid"}'
```

## <a name="troubleshoot-and-support"></a>Problemen oplossen en ondersteuning

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure-portal en met behulp van de Azure CLI. Als wilt zien de implementatiestatus van extensies voor een bepaalde virtuele machine, voert u de volgende opdracht uit met de Azure CLI.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Extensie uitvoering uitvoer wordt vastgelegd in het volgende bestand:

```
/opt/microsoft/omsagent/bin/stdout
```

### <a name="error-codes-and-their-meanings"></a>Foutcodes en hun betekenis

| Foutcode | Betekenis | Mogelijke actie |
| :---: | --- | --- |
| 9 | Met de naam voortijdig inschakelen | [De Azure Linux Agent bijwerken](https://docs.microsoft.com/azure/virtual-machines/linux/update-agent) naar de meest recente beschikbare versie. |
| 10 | Virtuele machine is al verbonden met een Log Analytics-werkruimte | StopOnMultipleConnections ingesteld op false in de openbare-instellingen voor de virtuele machine verbinding met de werkruimte die is opgegeven in het extensieschema, of verwijder deze eigenschap. Deze virtuele machine in rekening gebracht wanneer er voor elke werkruimte is verbonden met. |
| 11 | Ongeldige configuratie opgegeven voor de extensie | Volg de voorgaande voorbeelden om in te stellen alle eigenschapswaarden die nodig zijn voor implementatie. |
| 17 | Meld u Analytics-pakket-installatiefout | 
| 19 | Fout tijdens de installatie van de OMI-pakket | 
| 20 | Fout tijdens de installatie van de SCX-pakket |
| 51 | Deze extensie wordt niet ondersteund voor besturingssysteem van de virtuele machine | |
| 55 | Kan geen verbinding maken met de Azure Monitor-service of de vereiste pakketten ontbreken of dpkg Pakketbeheer is vergrendeld| Controleer of het systeem heeft toegang tot Internet of dat een geldige HTTP-proxy is opgegeven. Bovendien Controleer de juistheid van de werkruimte-ID en controleer of curl en tar-hulpprogramma's zijn geïnstalleerd. |

Als u meer informatie over probleemoplossing vindt u op de [Log Analytics-Agent voor Linux Troubleshooting Guide](../../azure-monitor/platform/vmext-troubleshoot.md).

### <a name="support"></a>Ondersteuning

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer Get-ondersteuning. Voor meer informatie over het gebruik van ondersteuning voor Azure, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).

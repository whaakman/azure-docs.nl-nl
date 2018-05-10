---
title: De extensie OMS Azure virtuele machine voor Linux | Microsoft Docs
description: Implementeer de OMS-agent op Linux virtuele machine met de extensie van een virtuele machine.
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: c7bbf210-7d71-4a37-ba47-9c74567a9ea6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/07/2018
ms.author: danis
ms.openlocfilehash: 9f5d3754d97e44d9944cd09f1ec082e9c04ad206
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="oms-virtual-machine-extension-for-linux"></a>Extensie van de virtuele machine OMS voor Linux

## <a name="overview"></a>Overzicht

Log Analytics biedt mogelijkheden voor bewaking, waarschuwingen en waarschuwing herstel tussen cloud en on-premises activa. De extensie van de virtuele machine OMS-Agent voor Linux is gepubliceerd en ondersteund door Microsoft. De extensie wordt de OMS-agent geïnstalleerd op virtuele machines in Azure, en virtuele machines in een bestaande werkruimte voor logboekanalyse schrijft. In dit document worden de ondersteunde platforms, configuraties en implementatie-opties voor de extensie van de OMS-virtuele machine voor Linux.

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem

De extensie OMS-Agent kan worden uitgevoerd op basis van deze Linux-distributies.

| Distributie | Versie |
|---|---|
| CentOS Linux | 5, 6 en 7 |
| Oracle Linux | 5, 6 en 7 |
| Red Hat Enterprise Linux Server | 5, 6 en 7 |
| Debian GNU/Linux | 6, 7 en 8 |
| Ubuntu | 12.04 TNS, 14.04 TNS, 15.04, 15.10, 16.04 TNS |
| SUSE Linux Enterprise Server | 11 en 12 |

### <a name="agent-and-vm-extension-version"></a>Agent en de VM-extensie-versie
De volgende tabel bevat een toewijzing van de versie van de VM OMS-uitbreiding en de bundel OMS-Agent voor elke versie. Een koppeling naar de release-opmerkingen voor de bundel-versie van OMS-agent is opgenomen. Release-opmerkingen bevatten gegevens over oplossingen voor problemen en nieuwe functies beschikbaar zijn voor een bepaalde agent-release.  

| Versie van de virtuele Linux-machine OMS-uitbreiding | Versie van de bundel OMS-Agent | 
|--------------------------------|--------------------------|
| 1.6.42.0 | [1.6.42.0](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.6.0-42)| 
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

Azure Security Center wordt automatisch voorziet in de OMS-agent en is verbonden met een standaard Log Analytics-werkruimte gemaakt door ASC in uw Azure-abonnement. Als u van Azure Security Center gebruikmaakt, niet uitgevoerd door de stappen in dit document. In dat geval worden de geconfigureerde werkruimte overschreven en Hiermee verbreekt u de verbinding met Azure Security Center.

### <a name="internet-connectivity"></a>Internetconnectiviteit

De extensie OMS-Agent voor Linux vereist dat de virtuele doelmachine is verbonden met internet. 

## <a name="extension-schema"></a>Extensieschema

De volgende JSON ziet u het schema voor de uitbreiding OMS-Agent. De extensie moet u de werkruimte-ID en werkruimtesleutel uit de doel-Log Analytics-werkruimte; Deze waarden kunnen worden [gevonden in de werkruimte voor logboekanalyse](../../log-analytics/log-analytics-quick-collect-linux-computer.md#obtain-workspace-id-and-key) in de Azure portal. Omdat de werkruimtesleutel moet worden behandeld als gevoelige gegevens, moet deze worden opgeslagen in de configuratie van een beveiligde instelling. Azure VM-extensie beveiligde instellingsgegevens is versleuteld en alleen op de virtuele doelmachine worden ontsleuteld. Houd er rekening mee dat **workspaceId** en **workspaceKey** zijn hoofdlettergevoelig.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

### <a name="property-values"></a>Eigenschapswaarden

| Naam | Waarde / voorbeeld |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Uitgever | Microsoft.EnterpriseCloud.Monitoring |
| type | OmsAgentForLinux |
| typeHandlerVersion | 1.4 |
| workspaceId (bijvoorbeeld) | 6f680a37-00c6-41c7-a93f-1437e3462574 |
| workspaceKey (bijvoorbeeld) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ== |


## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. Sjablonen zijn ideaal bij het implementeren van een of meer virtuele machines waarvoor de implementatieconfiguratie post zoals voorbereiding voor logboekanalyse. Een voorbeeld Resource Manager-sjabloon met de OMS-Agent VM-extensie vindt u op de [galerie van Azure Quick Start](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm). 

De JSON-configuratie voor de extensie van een virtuele machine worden genest in de bron van de virtuele machine, of aan de basis- of bovenste niveau van een Resource Manager JSON-sjabloon geplaatst. De plaatsing van de JSON-configuratie is van invloed op de waarde van de resourcenaam en het type. Zie voor meer informatie [naam en type voor de onderliggende resources instellen](../../azure-resource-manager/resource-manager-templates-resources.md#child-resources). 

Het volgende voorbeeld wordt ervan uitgegaan dat de VM-extensie is genest binnen de bron van de virtuele machine. Wanneer het nesten van de extensie-resource, de JSON wordt geplaatst in de `"resources": []` object van de virtuele machine.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

Bij het plaatsen van de JSON-extensie in de hoofdmap van de sjabloon, de naam van de bron bevat een verwijzing naar de bovenliggende virtuele machine en het type reflecteert de geneste configuratie.  

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "<parentVmResource>/OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
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

De Azure CLI kan worden gebruikt voor het implementeren van de OMS-Agent VM-extensie op een bestaande virtuele machine. Vervang de *workspaceId* en *workspaceKey* met die van de werkruimte voor logboekanalyse. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.4 --protected-settings '{"workspaceKey": "omskey"}' \
  --settings '{"workspaceId": "omsid"}'
```

## <a name="troubleshoot-and-support"></a>Oplossen van problemen en ondersteunen

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure portal en met behulp van de Azure CLI. Overzicht van de implementatiestatus van uitbreidingen voor een bepaalde virtuele machine, voer de volgende opdracht met de Azure CLI.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

De uitvoer van de extensie-uitvoering wordt vastgelegd in het volgende bestand:

```
/opt/microsoft/omsagent/bin/stdout
```

### <a name="error-codes-and-their-meanings"></a>Foutcodes en hun betekenis

| Foutcode | Betekenis | Mogelijke actie |
| :---: | --- | --- |
| 9 | Voortijdig aangeroepen inschakelen | [Bijwerken van de Azure Linux Agent](https://docs.microsoft.com/azure/virtual-machines/linux/update-agent) naar de meest recente versie. |
| 10 | Virtuele machine is al verbonden met een werkruimte voor logboekanalyse | Voor de virtuele machine verbinding met de werkruimte die is opgegeven in het Uitbreidingsschema, stopOnMultipleConnections ingesteld op false in instellingen voor openbare of verwijdert u deze eigenschap. Deze virtuele machine opgehaald in rekening gebracht zodra voor elke werkruimte is verbonden met. |
| 11 | Ongeldige configuratie opgegeven voor de extensie | Volg de voorgaande voorbeelden voor het instellen van alle eigenschapswaarden nodig is voor implementatie. |
| 12 | De Pakketbeheer dpkg is vergrendeld | Zorg ervoor dat alle dpkg update-bewerkingen op de computer hebt opgegeven en probeer het opnieuw. |
| 19 | OMI-installatiefout | 
| 20 | Installatiefout SCX-pakket |
| 51 | Deze extensie wordt niet ondersteund op de VM-besturingssysteem | |
| 55 | Kan geen verbinding maken met de Microsoft Operations Management Suite-service | Controleer of het systeem toegang heeft toegang tot Internet of een geldige HTTP-proxy is opgegeven. Controleer daarnaast de juistheid van de werkruimte-ID. |

Extra informatie over probleemoplossing vindt u op de [OMS-Agent voor Linux Troubleshooting Guide](../../log-analytics/log-analytics-azure-vmext-troubleshoot.md).

### <a name="support"></a>Ondersteuning

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op de [MSDN Azure en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook een incident voor ondersteuning van Azure indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer de Get-ondersteuning. Voor meer informatie over het gebruik van Azure ondersteuning voor de [ondersteuning van Microsoft Azure Veelgestelde vragen over](https://azure.microsoft.com/support/faq/).

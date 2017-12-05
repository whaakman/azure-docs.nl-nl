---
title: De extensie OMS Azure virtuele machine voor Linux | Microsoft Docs
description: Implementeer de OMS-agent op Linux virtuele machine met de extensie van een virtuele machine.
services: virtual-machines-linux
documentationcenter: 
author: danielsollondon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7bbf210-7d71-4a37-ba47-9c74567a9ea6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: danis
ms.openlocfilehash: dcb7a777c66200c5046a6ad34dc4ff5d346f13e0
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="oms-virtual-machine-extension-for-linux"></a>Extensie van de virtuele machine OMS voor Linux

## <a name="overview"></a>Overzicht

Operations Management Suite (OMS) biedt mogelijkheden voor bewaking, waarschuwingen en waarschuwing herstel tussen cloud en on-premises activa. De extensie van de virtuele machine OMS-Agent voor Linux is gepubliceerd en ondersteund door Microsoft. De extensie wordt de OMS-agent geïnstalleerd op virtuele machines in Azure, en virtuele machines in een bestaande OMS-werkruimte schrijft. In dit document worden de ondersteunde platforms, configuraties en implementatie-opties voor de extensie van de OMS-virtuele machine voor Linux.

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

### <a name="azure-security-center"></a>Azure Security Center

Azure Security Center wordt automatisch voorziet in de OMS-agent en is verbonden met de standaard log analytics-werkruimte van de Azure-abonnement. Als u van Azure Security Center gebruikmaakt, niet uitgevoerd door de stappen in dit document. In dat geval worden de geconfigureerde werkruimte en einde van de verbinding met Azure Security Center overschreven.

### <a name="internet-connectivity"></a>Internetconnectiviteit

De extensie OMS-Agent voor Linux vereist dat de virtuele doelmachine is verbonden met internet. 

## <a name="extension-schema"></a>Uitbreidingsschema

De volgende JSON ziet u het schema voor de uitbreiding OMS-Agent. De extensie moet u de werkruimte-ID en werkruimtesleutel uit de doel-OMS-werkruimte; u vindt deze waarden in de OMS-portal. Omdat de werkruimtesleutel moet worden behandeld als gevoelige gegevens, moet deze worden opgeslagen in de configuratie van een beveiligde instelling. Azure VM-extensie beveiligde instellingsgegevens is versleuteld en alleen op de virtuele doelmachine worden ontsleuteld. Houd er rekening mee dat **workspaceId** en **workspaceKey** zijn hoofdlettergevoelig.

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
| workspaceKey (bijvoorbeeld) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI + rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ == |


## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. Sjablonen zijn ideaal bij het implementeren van een of meer virtuele machines waarvoor de implementatieconfiguratie post zoals voorbereiding voor OMS. Een voorbeeld Resource Manager-sjabloon met de OMS-Agent VM-extensie vindt u op de [galerie van Azure Quick Start](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm). 

De JSON-configuratie voor de extensie van een virtuele machine worden genest in de bron van de virtuele machine, of aan de basis- of bovenste niveau van een Resource Manager JSON-sjabloon geplaatst. De plaatsing van de JSON-configuratie is van invloed op de waarde van de resourcenaam en het type. Zie voor meer informatie [naam en type voor de onderliggende resources instellen](../../azure-resource-manager/resource-manager-template-child-resource.md). 

Het volgende voorbeeld wordt ervan uitgegaan dat de OMS-extensie is genest binnen de bron van de virtuele machine. Wanneer het nesten van de extensie-resource, de JSON wordt geplaatst in de `"resources": []` object van de virtuele machine.

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

De Azure CLI kan worden gebruikt voor het implementeren van de OMS-Agent VM-extensie op een bestaande virtuele machine. Vervangen door de OMS-sleutel en de OMS-ID die uit de OMS-werkruimte. 

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
| 10 | Virtuele machine is al verbonden met een OMS-werkruimte | Voor de virtuele machine verbinding met de werkruimte die is opgegeven in het Uitbreidingsschema, stopOnMultipleConnections ingesteld op false in instellingen voor openbare of verwijdert u deze eigenschap. Deze virtuele machine opgehaald in rekening gebracht zodra voor elke werkruimte is verbonden met. |
| 11 | Ongeldige configuratie opgegeven voor de extensie | Volg de voorgaande voorbeelden voor het instellen van alle eigenschapswaarden nodig is voor implementatie. |
| 12 | De Pakketbeheer dpkg is vergrendeld | Zorg ervoor dat alle dpkg update-bewerkingen op de computer hebt opgegeven en probeer het opnieuw. |
| 20 | Voortijdig aangeroepen inschakelen | [Bijwerken van de Azure Linux Agent](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) naar de meest recente versie. |
| 51 | Deze extensie wordt niet ondersteund op de VM-besturingssysteem | |
| 55 | Kan geen verbinding maken met de Microsoft Operations Management Suite-service | Controleer of het systeem toegang heeft toegang tot Internet of een geldige HTTP-proxy is opgegeven. Controleer daarnaast de juistheid van de werkruimte-ID. |

Extra informatie over probleemoplossing vindt u op de [OMS-Agent voor Linux Troubleshooting Guide](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md#).

### <a name="support"></a>Ondersteuning

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op de [MSDN Azure en Stack Overflow-forums](https://azure.microsoft.com/en-us/support/forums/). U kunt ook een incident voor ondersteuning van Azure indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/en-us/support/options/) en selecteer de Get-ondersteuning. Voor meer informatie over het gebruik van Azure ondersteuning voor de [ondersteuning van Microsoft Azure Veelgestelde vragen over](https://azure.microsoft.com/en-us/support/faq/).

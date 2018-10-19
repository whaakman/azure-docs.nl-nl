---
title: Azure Diagnostics-extensie voor Windows | Microsoft Docs
description: Azure Windows VM's controleren met de Azure Diagnostics-extensie
services: virtual-machines-windows
documentationcenter: ''
author: johnkemnetz
manager: ashwink
editor: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/06/2018
ms.author: johnkem
ms.openlocfilehash: db4208c8fef27d2e2085e63ba3a986456d0544bf
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49429113"
---
# <a name="azure-diagnostics-extension-for-windows-vms"></a>Azure Diagnostics-extensie voor Windows-VM 's

## <a name="overview"></a>Overzicht

De virtuele machine van Azure Diagnostics-extensie kunt u het verzamelen van gegevens, zoals prestatiemeteritems en gebeurtenislogboeken, van uw Windows-VM. Granulair kunt u opgeven welke gegevens u wenst te verzamelen en waar u de gegevens om te gaan, zoals een Azure Storage-account of een Azure Event Hub. U kunt deze gegevens ook gebruiken om te bouwen grafieken in Azure portal of metrische waarschuwingen maken.

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem

De Azure Diagnostics-extensie kunnen voor Windows 10-clients, Windows Server 2008 R2, 2012, 2012 R2 en 2016 worden uitgevoerd.

### <a name="internet-connectivity"></a>Internetconnectiviteit

De Azure Diagnostics-extensie is vereist dat de virtuele doelmachine is verbonden met internet. 

## <a name="extension-schema"></a>Extensieschema

[De Azure Diagnostics-extensie schema en de eigenschap waarden worden beschreven in dit document.](../../monitoring-and-diagnostics/azure-diagnostics-schema-1dot3-and-later.md)

## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. De JSON-schema in de vorige sectie beschreven kan worden gebruikt in een Azure Resource Manager-sjabloon om uit te voeren van de Azure Diagnostics-extensie tijdens de sjabloonimplementatie van een Azure Resource Manager. Zie [Gebruik controle en diagnostische gegevens met een Windows-VM en Azure Resource Manager-sjablonen](extensions-diagnostics-template.md).

## <a name="azure-cli-deployment"></a>Azure CLI-implementatie

De Azure CLI kan worden gebruikt om de Azure Diagnostics-extensie om een bestaande virtuele machine te implementeren. Vervang de beveiligde instellingen en eigenschappen voor door geldige JSON uit de bovenstaande extensieschema. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name IaaSDiagnostics \
  --publisher Microsoft.Azure.Diagnostics \
  --version 1.9.0.0 --protected-settings protected-settings.json \
  --settings public-settings.json 
```

## <a name="powershell-deployment"></a>PowerShell-implementatie

De `Set-AzureRmVMDiagnosticsExtension` opdracht kan worden gebruikt om de Azure Diagnostics-extensie toevoegen aan een bestaande virtuele machine. Zie ook [PowerShell gebruiken voor het inschakelen van Azure Diagnostics in een virtuele machine met Windows](ps-extensions-diagnostics.md).
```powershell
$vm_resourcegroup = "myvmresourcegroup"
$vm_name = "myvm"
$diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup `
  -VMName $vm_name `
  -DiagnosticsConfigurationPath $diagnosticsconfig_path
```

## <a name="troubleshoot-and-support"></a>Problemen oplossen en ondersteuning

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure-portal en met behulp van de Azure CLI. Als wilt zien de implementatiestatus van extensies voor een bepaalde virtuele machine, voert u de volgende opdracht uit met de Azure CLI.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

[Raadpleeg dit artikel](../../monitoring-and-diagnostics/azure-diagnostics-troubleshooting.md) voor een uitgebreidere gids voor probleemoplossing voor de Azure Diagnostics-extensie.

### <a name="support"></a>Ondersteuning

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer Get-ondersteuning. Voor meer informatie over het gebruik van ondersteuning voor Azure, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over de Azure Diagnostics-extensie](../../monitoring-and-diagnostics/azure-diagnostics.md)
* [Raadpleeg de extensieschema en versies](../../monitoring-and-diagnostics/azure-diagnostics-schema.md)

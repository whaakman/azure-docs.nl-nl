---
title: VM-momentopname Linux-extensie voor Azure Backup | Microsoft Docs
description: Nemen toepassing consistente back-up van de virtuele machine van Azure Backup met behulp van de VM-momentopname-extensie
services: backup, virtual-machines-linux
documentationcenter: ''
author: trinadhk
manager: jeconnoc
editor: ''
ms.assetid: 57759670-0baa-44db-ae14-8cdc00d3a906
ms.service: backup, virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 3/26/2018
ms.author: trinadhk
ms.openlocfilehash: bed5716b6d4ea6d81214a95d0f2360f359048893
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33942688"
---
# <a name="vm-snapshot-linux-extension-for-azure-backup"></a>VM-momentopname Linux-extensie voor Azure Backup

## <a name="overview"></a>Overzicht

Azure Backup biedt ondersteuning voor back-ups van werkbelastingen van on-premises naar cloud- en back-ups van cloud-bronnen naar een Recovery Services-kluis. Azure Backup maakt gebruik van VM-momentopname-extensie te laten de virtuele machine een toepassing consistente back-up van de virtuele machine van Azure zonder de noodzaak om af te sluiten. VM-momentopname Linux-uitbreiding is gepubliceerd en ondersteund door Microsoft als onderdeel van Azure Backup-service. De extensie installeert Azure Backup als onderdeel van de eerste geplande back-geactiveerde post back-up inschakelen. In dit document worden de ondersteunde platforms, configuraties en implementatie-opties voor de VM-momentopname-extensie.

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem
Voor een lijst met ondersteunde besturingssystemen, raadpleegt u [besturingssystemen wordt ondersteund door Azure Backup](../../backup/backup-azure-arm-vms-prepare.md#supported-operating-systems-for-backup)

### <a name="internet-connectivity"></a>Internetconnectiviteit

VM-momentopname-uitbreiding vereist dat de virtuele doelmachine is verbonden met internet wanneer we een back-up van virtuele machine.

## <a name="extension-schema"></a>Extensieschema

De volgende JSON ziet u het schema voor de VM-momentopname-extensie. De uitbreiding vereist de taak-ID: Hiermee wordt de back-uptaak die geactiveerd op de virtuele machine, een momentopname geïdentificeerd status blob-uri - waarin de status van de momentopname-bewerking is geschreven, geplande begintijd van de momentopname, logboeken blob-uri - waar logboeken overeenkomt met een momentopname van taak worden geschreven, objstr-weergave van VM-schijven en metagegevens.  Omdat deze instellingen moeten worden behandeld als gevoelige gegevens, moet deze worden opgeslagen in de configuratie van een beveiligde instelling. Azure VM-extensie beveiligde instellingsgegevens is versleuteld en alleen op de virtuele doelmachine worden ontsleuteld. Houd er rekening mee dat deze instellingen worden aanbevolen om uitsluitend als onderdeel van de back-uptaak van Azure Backup-service worden doorgegeven.

```json
{
  "type": "extensions",
  "name": "VMSnapshot",
  "location":"<myLocation>",
  "properties": {
    "publisher": "Microsoft.RecoveryServices",
    "type": "VMSnapshot",
    "typeHandlerVersion": "1.9",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "locale":"<location>",
      "taskId":"<taskId used by Azure Backup service to communicate with extension>",
      "commandToExecute": "snapshot",
      "commandStartTimeUTCTicks": "<scheduled start time of the snapshot task>",
      "vmType": "microsoft.compute/virtualmachines"
    },
    "protectedSettings": {
      "objectStr": "<blob SAS uri represenattion of VM sent by Azure Backup service to extension>",
      "logsBlobUri": "<blob uri where logs of command execution by extension are written to>",
      "statusBlobUri": "<blob uri where status of the command executed by extension is written>"
    }
  }
}
```

### <a name="property-values"></a>Eigenschapswaarden

| Naam | Waarde / voorbeeld | Gegevenstype |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | datum |
| taskId | e07354cf-041e-4370-929f-25a319ce8933_1 | tekenreeks |
| commandStartTimeUTCTicks | 6.36458E + 17 | tekenreeks |
| Landinstellingen | nl-NL | tekenreeks |
| objectStr | Codering van sas uri matrix-'blobSASUri': [' https:\/\/sopattna5365.blob.core.windows.net\/VHD's\/vmubuntu1404ltsc201652903941.vhd? AVP 2014-02-14 = & sr = b & sig = TywkROXL1zvhXcLujtCut8g3jTpgbE6JpSWRLZxAdtA % 3D- & st = 2017-11-09T14% 3A23% 3A28Z & se 2017-11-09T17% 3A38% 3A28Z = & sp = rw ', ' https:\/\/sopattna8461.blob.core.windows.net\/VHD's\/vmubuntu1404ltsc-20160629-122418.vhd? AVP 2014-02-14 = & sr = b & sig = 5S0A6YDWvVwqPAkzWXVy % 2BS % 2FqMwzFMbamT5upwx05v8Q % 3D- & st = 2017-11-09T14% 3A23% 3A28Z & se 2017-11-09T17% 3A38% 3A28Z = & sp = rw ', ' https:\/ \/ sopattna8461.BLOB.Core.Windows.NET\/bootdiagnostics-vmubuntu1-deb58392-ed5e-48be-9228-ff681b0cd3ee\/vmubuntu1404ltsc-20160629-122541.vhd? AVP 2014-02-14 = & sr = b & sig = X0Me2djByksBBMVXMGIUrcycvhQSfjYvqKLeRA7nBD4% 3D- & st = 2017-11-09T14% 3A23% 3A28Z & se 2017-11-09T17% 3A38% 3A28Z = & sp = rw ', ' https:\/\/sopattna5365.blob.core.windows.net\/VHD's\/vmubuntu1404ltsc-20160701-163922.vhd? AVP 2014-02-14 = & sr = b & sig = oXvtK2IXCNqWv7fpjc7TAzFDpc1GoXtT7r % 2BC % 2BNIAork % 3D- & st = 2017-11-09T14% 3A23% 3A28Z & se 2017-11-09T17% 3A38% 3A28Z = & sp = rw ', ' https:\/ \/ sopattna5365.BLOB.Core.Windows.NET\/VHD's\/vmubuntu1404ltsc-20170705-124311.vhd? AVP 2014-02-14 = & sr = b & sig = ZUM9d28Mvvm % 2FfrhJ71TFZh0Ni90m38bBs3zMl % 2FQ9rs0% 3D- & st = 2017-11-09T14% 3A23% 3A28Z & se 2017-11-09T17% 3A38% 3A28Z = & sp rw = "] | tekenreeks |
| logsBlobUri | https://seapod01coord1exsapk732.blob.core.windows.net/bcdrextensionlogs-d45d8a1c-281e-4bc8-9d30-3b25176f68ea/sopattna-vmubuntu1404ltsc.v2.Logs.txt?sv=2014-02-14&sr=b&sig=DbwYhwfeAC5YJzISgxoKk%2FEWQq2AO1vS1E0rDW%2FlsBw%3D&st=2017-11-09T14%3A33%3A29Z&se=2017-11-09T17%3A38%3A29Z&sp=rw | tekenreeks |
| statusBlobUri | https://seapod01coord1exsapk732.blob.core.windows.net/bcdrextensionlogs-d45d8a1c-281e-4bc8-9d30-3b25176f68ea/sopattna-vmubuntu1404ltsc.v2.Status.txt?sv=2014-02-14&sr=b&sig=96RZBpTKCjmV7QFeXm5IduB%2FILktwGbLwbWg6Ih96Ao%3D&st=2017-11-09T14%3A33%3A29Z&se=2017-11-09T17%3A38%3A29Z&sp=rw | tekenreeks |



## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. De aanbevolen manier van een VM-momentopname-extensie toevoegen aan een virtuele machine is echter door in te schakelen van back-up op de virtuele machine. Dit kan worden bereikt via een Resource Manager-sjabloon.  Een voorbeeld Resource Manager-sjabloon waarmee back-up op een virtuele machine kunt u vinden op de [galerie van Azure Quick Start](https://azure.microsoft.com/resources/templates/101-recovery-services-backup-vms/).


## <a name="azure-cli-deployment"></a>Azure CLI-implementatie

De Azure CLI kan worden gebruikt voor back-up op een virtuele machine. Post back-up, eerste geplande back-uptaak installeert de Vm-momentopname-extensie op de virtuele machine.

```azurecli
az backup protection enable-for-vm \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --vm myVM \
    --policy-name DefaultPolicy
```

## <a name="troubleshoot-and-support"></a>Oplossen van problemen en ondersteunen

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure portal en met behulp van de Azure CLI. Overzicht van de implementatiestatus van uitbreidingen voor een bepaalde virtuele machine, voer de volgende opdracht met de Azure CLI.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

De uitvoer van de extensie-uitvoering wordt vastgelegd in het volgende bestand:

```
/var/log/waagent.log
```

### <a name="error-codes-and-their-meanings"></a>Foutcodes en hun betekenis

Informatie over probleemoplossing vindt u op de [Azure VM-back-up probleemoplossingsgids](../../backup/backup-azure-vms-troubleshoot.md).

### <a name="support"></a>Ondersteuning

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op de [MSDN Azure en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook een incident voor ondersteuning van Azure indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer de Get-ondersteuning. Voor meer informatie over het gebruik van Azure ondersteuning voor de [ondersteuning van Microsoft Azure Veelgestelde vragen over](https://azure.microsoft.com/support/faq/).

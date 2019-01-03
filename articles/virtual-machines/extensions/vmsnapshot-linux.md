---
title: VM-momentopname Linux-extensie voor Azure Backup | Microsoft Docs
description: Toepassing consistente back-up van de virtuele machine van Azure Backup met behulp van de momentopname van de VM-extensie nemen
services: backup, virtual-machines-linux
documentationcenter: ''
author: trinadhk
manager: jeconnoc
ms.service: backup, virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.date: 12/17/2018
ms.author: trinadhk
ms.openlocfilehash: 1bcf326bde3ac5c97734393ee162b1f98d1553ce
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53634198"
---
# <a name="vm-snapshot-linux-extension-for-azure-backup"></a>VM-momentopname Linux-extensie voor Azure Backup



Azure Backup biedt ondersteuning voor back-ups van workloads van on-premises naar cloud en Recovery Services-kluis back-ups van cloudresources. Azure Backup maakt gebruik van VM-momentopname-extensie te nemen van een toepassing consistente back-up van de virtuele machine van Azure zonder de noodzaak om af te sluiten de virtuele machine. VM-momentopname Linux-extensie is gepubliceerd en ondersteund door Microsoft als onderdeel van Azure Backup-service. Azure Backup wordt de extensie installeren als onderdeel van de eerste geplande back-up geactiveerde bericht back-up inschakelen. In dit document worden de ondersteunde platforms, configuraties en implementatie-opties voor de VM-momentopname-extensie.

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem
Voor een lijst met ondersteunde besturingssystemen, raadpleegt u [besturingssystemen die worden ondersteund door Azure Backup](../../backup/backup-azure-arm-vms-prepare.md#before-you-start)

### <a name="internet-connectivity"></a>Internetconnectiviteit

VM-momentopname-extensie is vereist dat de virtuele doelmachine is verbonden met internet wanneer we een back-up van virtuele machine.

## <a name="extension-schema"></a>Extensieschema

De volgende JSON ziet u het schema voor de VM-momentopname-extensie. De extensie is vereist voor de taak-ID: Hiermee wordt aangegeven met de back-uptaak dit geactiveerd momentopname op de virtuele machine, status van blob-uri - waarnaar de status van de momentopnamebewerking is geschreven, de geplande begintijd van de momentopname, logboeken voor blob-uri - waar logboeken overeenkomt met een momentopname van taak worden geschreven, objstr-representatie van VM-schijven en metagegevens.  Omdat deze instellingen moeten worden behandeld als gevoelige gegevens, moet deze worden opgeslagen in de instellingsconfiguratie van een beveiligde. Azure-VM-extensie beveiligde instellingsgegevens versleuteld en alleen op de virtuele doelmachine worden ontsleuteld. Houd er rekening mee dat deze instellingen worden aanbevolen om alleen als onderdeel van de back-uptaak van Azure Backup-service worden doorgegeven.

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

### <a name="property-values"></a>Waarden van eigenschappen

| Name | Waarde / voorbeeld | Gegevenstype |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| taak-id | e07354cf-041e-4370-929f-25a319ce8933_1 | string |
| commandStartTimeUTCTicks | 6.36458E + 17 | string |
| landinstelling | nl-NL | string |
| objectStr | Codering van sas uri matrix-"blobSASUri": [' https:\/\/sopattna5365.blob.core.windows.net\/VHD's\/vmubuntu1404ltsc201652903941.vhd? sv 2014-02-14 = & sr = b & sig = TywkROXL1zvhXcLujtCut8g3jTpgbE6JpSWRLZxAdtA % 3D- & st = 2017-11-09T14% 3A23% 3A28Z & se = 2017-11-09T17% 3A38% 3A28Z & sp = rw ', ' https:\/\/sopattna8461.blob.core.windows.net\/VHD's\/vmubuntu1404ltsc-20160629-122418.vhd? sv 2014-02-14 = & sr = b & sig 5S0A6YDWvVwqPAkzWXVy % 2BS % 2FqMwzFMbamT5upwx05v8Q % = 3D- & st = 2017-11-09T14% 3A23% 3A28Z & se = 2017-11-09T17% 3A38% 3A28Z & sp = rw ', ' https:\/ \/ sopattna8461.BLOB.Core.Windows.NET\/bootdiagnostics-vmubuntu1-deb58392-ed5e-48be-9228-ff681b0cd3ee\/vmubuntu1404ltsc-20160629-122541.vhd? sv 2014-02-14 = & sr = b & sig = X0Me2djByksBBMVXMGIUrcycvhQSfjYvqKLeRA7nBD4% 3D- & st = 2017-11-09T14% 3A23% 3A28Z & se = 2017-11-09T17% 3A38% 3A28Z & sp = rw ', ' https:\/\/sopattna5365.blob.core.windows.net\/VHD's\/vmubuntu1404ltsc-20160701-163922.vhd? sv 2014-02-14 = & sr = b & sig oXvtK2IXCNqWv7fpjc7TAzFDpc1GoXtT7r % 2BC % 2BNIAork % = 3D- & st = 2017-11-09T14% 3A23% 3A28Z & se = 2017-11-09T17% 3A38% 3A28Z & sp = rw ', ' https:\/ \/ sopattna5365.BLOB.Core.Windows.NET\/VHD's\/vmubuntu1404ltsc-20170705-124311.vhd? sv 2014-02-14 = & sr = b & sig ZUM9d28Mvvm % 2FfrhJ71TFZh0Ni90m38bBs3zMl % 2FQ9rs0% = 3D- & st = 2017-11-09T14% 3A23% 3A28Z & se = 2017-11-09T17% 3A38% 3A28Z & sp = rw "] | string |
| logsBlobUri | https://seapod01coord1exsapk732.blob.core.windows.net/bcdrextensionlogs-d45d8a1c-281e-4bc8-9d30-3b25176f68ea/sopattna-vmubuntu1404ltsc.v2.Logs.txt?sv=2014-02-14&sr=b&sig=DbwYhwfeAC5YJzISgxoKk%2FEWQq2AO1vS1E0rDW%2FlsBw%3D&st=2017-11-09T14%3A33%3A29Z&se=2017-11-09T17%3A38%3A29Z&sp=rw | string |
| statusBlobUri | https://seapod01coord1exsapk732.blob.core.windows.net/bcdrextensionlogs-d45d8a1c-281e-4bc8-9d30-3b25176f68ea/sopattna-vmubuntu1404ltsc.v2.Status.txt?sv=2014-02-14&sr=b&sig=96RZBpTKCjmV7QFeXm5IduB%2FILktwGbLwbWg6Ih96Ao%3D&st=2017-11-09T14%3A33%3A29Z&se=2017-11-09T17%3A38%3A29Z&sp=rw | string |



## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. De aanbevolen manier om een VM-momentopname-extensie toe te voegen aan een virtuele machine is echter door in te schakelen van back-up op de virtuele machine. Dit kan worden bereikt via Resource Manager-sjabloon.  Een voorbeeld van Resource Manager-sjabloon waarmee back-up op een virtuele machine kunt u vinden op de [Azure Quick Start-galerie](https://azure.microsoft.com/resources/templates/101-recovery-services-backup-vms/).


## <a name="azure-cli-deployment"></a>Azure CLI-implementatie

De Azure CLI kan worden gebruikt om in te schakelen van back-up op een virtuele machine. Back-up post inschakelen, eerste geplande back-uptaak wordt de Vm-momentopname-extensie installeren op de virtuele machine.

```azurecli
az backup protection enable-for-vm \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --vm myVM \
    --policy-name DefaultPolicy
```

## <a name="troubleshoot-and-support"></a>Problemen oplossen en ondersteuning

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure-portal en met behulp van de Azure CLI. Als wilt zien de implementatiestatus van extensies voor een bepaalde virtuele machine, voert u de volgende opdracht uit met de Azure CLI.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Extensie uitvoering uitvoer wordt vastgelegd in het volgende bestand:

```
/var/log/waagent.log
```

### <a name="error-codes-and-their-meanings"></a>Foutcodes en hun betekenis

Informatie over probleemoplossing vindt u op de [Azure VM backup-problemen oplossen met](../../backup/backup-azure-vms-troubleshoot.md).

### <a name="support"></a>Ondersteuning

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer Get-ondersteuning. Voor meer informatie over het gebruik van ondersteuning voor Azure, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).

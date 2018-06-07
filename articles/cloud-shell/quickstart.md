---
title: In de Azure-Cloud Shell snel Bash | Microsoft Docs
description: Snelstartgids voor Bash in de Cloud-Shell
services: ''
documentationcenter: ''
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: juluk
ms.openlocfilehash: 4b7e4302bba2efed12e19043da1f592bed12a2fd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34608879"
---
# <a name="quickstart-for-bash-in-azure-cloud-shell"></a>Snelstartgids voor Bash in de Azure-Cloud-Shell

In dit document worden Bash gebruiken in Azure Cloud-Shell op de [Azure-portal](https://ms.portal.azure.com/).

> [!NOTE]
> Een [PowerShell in Azure Cloud-Shell](quickstart-powershell.md) Quick Start is ook beschikbaar.

## <a name="start-cloud-shell"></a>Cloud-Shell starten
1. Start **Cloud Shell** van de bovenste navigatiebalk van de Azure portal. <br>
![](media/quickstart/shell-icon.png)

2. Selecteer een abonnement om een opslagaccount te maken en delen van Microsoft Azure-bestanden.
3. Selecteer 'Opslag maken'

> [!TIP]
> U bent automatisch geverifieerd voor Azure CLI 2.0 in elke sessie.

### <a name="select-the-bash-environment"></a>Selecteer de Bash-omgeving
Controleer of de omgeving vervolgkeuzelijst vanaf de linkerkant van het venster shell zegt `Bash`. <br>
![](media/quickstart/env-selector.png)

### <a name="set-your-subscription"></a>Stel uw abonnement
1. Lijst met abonnementen u toegang tot hebt.
```azurecli-interactive
az account list
```

2. Stel uw voorkeur abonnement: <br>
```azurecli-interactive
az account set --subscription my-subscription-name`
```

> [!TIP]
> Uw abonnement worden opgeslagen voor toekomstige sessies met behulp van `/home/<user>/.azure/azureProfile.json`.

### <a name="create-a-resource-group"></a>Een resourcegroep maken
Maak een nieuwe resourcegroep in WestUS met de naam 'MyRG'.
```azurecli-interactive
az group create --location westus --name MyRG
```

### <a name="create-a-linux-vm"></a>Een Linux-VM maken
Maak een VM Ubuntu in uw nieuwe resourcegroep. De Azure CLI 2.0 maakt SSH-sleutels en de virtuele machine met hen instellen. <br>

```azurecli-interactive
az vm create -n myVM -g MyRG --image UbuntuLTS --generate-ssh-keys
```

> [!NOTE]
> Met behulp van `--generate-ssh-keys` Hiermee geeft u Azure CLI 2.0 maken en instellen van openbare en persoonlijke sleutels in uw virtuele machine en `$Home` directory. Standaard sleutels worden geplaatst in de Cloud-Shell op `/home/<user>/.ssh/id_rsa` en `/home/<user>/.ssh/id_rsa.pub`. Uw `.ssh` map blijft behouden in uw gekoppelde bestandsshare 5 GB-installatiekopie die wordt gebruikt om vast te leggen `$Home`.

Uw gebruikersnaam op deze virtuele machine worden uw gebruikersnaam die wordt gebruikt in de Cloud-Shell ($User@Azure:).

### <a name="ssh-into-your-linux-vm"></a>SSH in uw virtuele Linux-machine
1. Zoek de naam van uw virtuele machine in de Azure portal zoekbalk.
2. Klik op 'Connect' om uw VM-naam en het openbare IP-adres. <br>
![](media/quickstart/sshcmd-copy.png)

3. SSH in uw virtuele machine met de `ssh` cmd.
```
ssh username@ipaddress
```

Bij het tot stand brengen van de SSH-verbinding, ziet u de Welkom Ubuntu-prompt. <br>
![](media/quickstart/ubuntu-welcome.png)

## <a name="cleaning-up"></a>Opschonen 
1. Sluit uw ssh sessie.
```azurecli-interactive
exit
```

2. De resourcegroep en alle resources daarin verwijderd.
```azurecli-interactive
az group delete -n MyRG
```

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over persistent maken van bestanden voor Bash in de Cloud-Shell](persisting-shell-storage.md) <br>
[Meer informatie over Azure CLI 2.0](https://docs.microsoft.com/cli/azure/) <br>
[Meer informatie over Azure File storage](../storage/files/storage-files-introduction.md) <br>

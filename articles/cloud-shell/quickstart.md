---
title: Bash in de Snelstartgids voor Azure Cloud Shell | Microsoft Docs
description: Snelstartgids voor Bash in Cloudshell
services: ''
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: damaerte
ms.openlocfilehash: 56a4645c5f86e9f95e82533b7f5f81b7bca51e7b
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57245315"
---
# <a name="quickstart-for-bash-in-azure-cloud-shell"></a>Snelstartgids voor Bash in Azure Cloudshell

Dit document wordt uitgelegd hoe het gebruik van Bash in Azure Cloud Shell in de [Azure-portal](https://ms.portal.azure.com/).

> [!NOTE]
> Een [PowerShell in Azure Cloud Shell](quickstart-powershell.md) Quick Start is ook beschikbaar.

## <a name="start-cloud-shell"></a>Cloudshell starten
1. Start **Cloud Shell** via de bovenste navigatiebalk van de Azure-portal. <br>
![](media/quickstart/shell-icon.png)

2. Selecteer een abonnement om een opslagaccount te maken en delen van Microsoft Azure-bestanden.
3. Selecteer 'Opslag maken'

> [!TIP]
> U bent automatisch geverifieerd voor Azure CLI in elke sessie.

### <a name="select-the-bash-environment"></a>Selecteer de Bash-omgeving
Controleer of de omgeving vervolgkeuzelijst aan de linkerkant van de shell-venster zegt `Bash`. <br>
![](media/quickstart/env-selector.png)

### <a name="set-your-subscription"></a>Uw abonnement instellen
1. Lijst met abonnementen u toegang tot hebt.
```azurecli-interactive
az account list
```

2. Stel het gewenste abonnement: <br>
```azurecli-interactive
az account set --subscription my-subscription-name`
```

> [!TIP]
> Uw abonnement worden opgeslagen voor toekomstige sessies met behulp van `/home/<user>/.azure/azureProfile.json`.

### <a name="create-a-resource-group"></a>Een resourcegroep maken
Maak een nieuwe resourcegroep in VS West met de naam 'MyRG'.
```azurecli-interactive
az group create --location westus --name MyRG
```

### <a name="create-a-linux-vm"></a>Een Linux-VM maken
Een Ubuntu-VM maken in uw nieuwe resourcegroep. De Azure CLI wordt SSH-sleutels maken en instellen van de virtuele machine met hen. <br>

```azurecli-interactive
az vm create -n myVM -g MyRG --image UbuntuLTS --generate-ssh-keys
```

> [!NOTE]
> Met behulp van `--generate-ssh-keys` Hiermee geeft u Azure CLI voor het maken en instellen van openbare en persoonlijke sleutels in uw virtuele machine en `$Home` directory. Sleutels worden standaard geplaatst in de Cloud Shell op `/home/<user>/.ssh/id_rsa` en `/home/<user>/.ssh/id_rsa.pub`. Uw `.ssh` map blijft behouden in de gekoppelde bestandsshare 5 GB-installatiekopie die wordt gebruikt om vast te leggen `$Home`.

Uw gebruikersnaam op deze virtuele machine is de gebruikersnaam die wordt gebruikt in de Cloud Shell ($User@Azure:).

### <a name="ssh-into-your-linux-vm"></a>SSH in uw virtuele Linux-machine
1. Zoeken naar de naam van uw virtuele machine in de Azure portal zoekbalk.
2. Klik op 'Connect' als u uw VM-naam en het openbare IP-adres. <br>
![](media/quickstart/sshcmd-copy.png)

3. SSH in uw virtuele machine met de `ssh` cmd.
```
ssh username@ipaddress
```

Bij het maken van de SSH-verbinding, ziet u de Welkom Ubuntu-prompt. <br>
![](media/quickstart/ubuntu-welcome.png)

## <a name="cleaning-up"></a>Opschonen 
1. Sluit uw ssh sessie.
```azurecli-interactive
exit
```

2. Verwijder de resourcegroep en alle resources daarin.
```azurecli-interactive
az group delete -n MyRG
```

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over het behoud van bestanden voor Bash in Cloud Shell](persisting-shell-storage.md) <br>
[Meer informatie over Azure CLI](https://docs.microsoft.com/cli/azure/) <br>
[Meer informatie over Azure File storage](../storage/files/storage-files-introduction.md) <br>

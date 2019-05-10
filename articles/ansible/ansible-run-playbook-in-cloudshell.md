---
title: Snelstartgids - Ansible-playbooks uitvoeren via Bash in Azure Cloud Shell | Microsoft Docs
description: In deze snelstartgids leert u hoe u verschillende Ansible-taken met Bash in Azure Cloud Shell uit te voeren
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
ms.topic: quickstart
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: 083d10de94c39ab134b8e475b66ebf2df30088bc
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65407657"
---
# <a name="quickstart-run-ansible-playbooks-via-bash-in-azure-cloud-shell"></a>Quickstart: Ansible-playbooks uitvoeren via Bash in Azure Cloud Shell

Azure Cloud Shell is een interactieve, browser toegankelijke shell voor het beheren van Azure-resources. Cloudshell biedt maakt u een Bash- of Powershell-opdrachtregel te gebruiken. In dit artikel gebruikt u Bash in Azure Cloud Shell een Ansible-playbook uitvoeren.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
- **Configureren van Azure Cloud Shell** : als u geen ervaring met Azure Cloud Shell, Zie [Snelstartgids voor Bash in Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart).
[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Automatische configuratie van referenties

Ansible wordt geverifieerd bij Azure wanneer u zich aanmeldt bij Cloud Shell om de infrastructuur te beheren zonder extra configuratie. 

Als u werkt met meerdere abonnementen, geef het abonnement op Ansible wordt gebruikt door het exporteren van de `AZURE_SUBSCRIPTION_ID` omgevingsvariabele. 

Voer de volgende opdracht uit om al uw Azure-abonnementen weer te geven:

```azurecli-interactive
az account list
```

Met uw Azure-abonnement-ID, stelt u de `AZURE_SUBSCRIPTION_ID` als volgt:

```azurecli-interactive
export AZURE_SUBSCRIPTION_ID=<your-subscription-id>
```

## <a name="verify-the-configuration"></a>Controleer de configuratie
Om te controleren of de juiste configuratie, Ansible te gebruiken om een Azure-resourcegroep te maken.

[!INCLUDE [create-resource-group-with-ansible.md](../../includes/ansible-snippet-create-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"] 
> [Snelstart: Virtuele machine in Azure met behulp van Ansible configureren](/azure/virtual-machines/linux/ansible-create-vm)
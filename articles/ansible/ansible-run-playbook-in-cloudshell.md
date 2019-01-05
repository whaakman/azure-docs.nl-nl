---
title: Ansible uitvoeren met Bash in Azure Cloudshell
description: Leer hoe u verschillende Ansible-taken met Bash uitvoert in Azure Cloud Shell
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 08/07/2018
ms.topic: article
ms.openlocfilehash: 6bfac47e4afa41b4c75a8d33b4eea1ff5103296d
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54050891"
---
# <a name="run-ansible-with-bash-in-azure-cloud-shell"></a>Ansible uitvoeren met Bash in Azure Cloudshell

In deze zelfstudie leert u hoe u Bash in Cloud Shell gebruiken voor het configureren van een Azure-abonnement als uw werkruimte Ansible. 

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement**: als u nog geen Azure-abonnement hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan.

- **Azure Cloud Shell configureren**: als u niet eerder hebt gewerkt met Azure Cloud Shell, leest u in het artikel [Snelstartgids voor Bash in Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) hoe u Cloud Shell start en configureert. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Automatische configuratie van referenties

Wanneer u bent aangemeld bij de Cloud Shell, verifieert Ansible met Azure voor het beheren van infrastructuur zonder extra configuratie. Als u meer dan één abonnement hebt, kunt u kiezen welk abonnement u Ansible door te exporteren met werkt de `AZURE_SUBSCRIPTION_ID` omgevingsvariabele. Als u al uw Azure-abonnementen, moet u de volgende opdracht uitvoeren:

```azurecli-interactive
az account list
```

Met behulp van de **id** instellen van het abonnement waarmee u wilt werken, de **AZURE_SUBSCRIPTION_ID** als volgt:

```azurecli-interactive
export AZURE_SUBSCRIPTION_ID=<your-subscription-id>
```

## <a name="verify-the-configuration"></a>De configuratie controleren
Als u wilt controleren of de juiste configuratie, Ansible te gebruiken om een resourcegroep te maken.

[!INCLUDE [create-resource-group-with-ansible.md](../../includes/ansible-create-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"] 
> [Een eenvoudige virtuele machine maken in Azure met Ansible](/azure/virtual-machines/linux/ansible-create-vm)
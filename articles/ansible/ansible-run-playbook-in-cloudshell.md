---
title: Ansible uitvoeren met Bash in Azure Cloud Shell
description: Leer hoe u verschillende Ansible-taken met Bash uitvoert in Azure Cloud Shell
ms.service: azure
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 08/07/2018
ms.topic: quickstart
ms.openlocfilehash: 37426f1cdc51734c28b2c9739e26ad35f08e606f
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57791389"
---
# <a name="run-ansible-with-bash-in-azure-cloud-shell"></a>Ansible uitvoeren met Bash in Azure Cloud Shell

In deze zelfstudie leert u hoe u Bash in Cloud Shell gebruikt om een Azure-abonnement als uw Ansible-werkruimte te configureren. 

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement**: als u nog geen Azure-abonnement hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan.

- **Azure Cloud Shell configureren**: als u niet eerder hebt gewerkt met Azure Cloud Shell, leest u in het artikel [Snelstartgids voor Bash in Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) hoe u Cloud Shell start en configureert. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Automatische configuratie van referenties

Ansible wordt geverifieerd bij Azure wanneer u zich aanmeldt bij Cloud Shell om de infrastructuur te beheren zonder extra configuratie. Als u meer dan één abonnement hebt, kunt u kiezen met welk abonnement Ansible moet werken door de omgevingsvariabele `AZURE_SUBSCRIPTION_ID` te exporteren. Voer de volgende opdracht uit om al uw Azure-abonnementen weer te geven:

```azurecli-interactive
az account list
```

Stel met behulp van de **id** van het abonnement waarmee u wilt werken de **AZURE_SUBSCRIPTION_ID** als volgt in:

```azurecli-interactive
export AZURE_SUBSCRIPTION_ID=<your-subscription-id>
```

## <a name="verify-the-configuration"></a>De configuratie controleren
Als u wilt controleren of het configureren is geslaagd, gebruikt u Ansible om een resourcegroep te maken.

[!INCLUDE [create-resource-group-with-ansible.md](../../includes/ansible-create-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"] 
> [Een eenvoudige virtuele machine maken in Azure met Ansible](/azure/virtual-machines/linux/ansible-create-vm)
---
title: Ansible uitvoeren met Bash in Azure Cloudshell
description: Leer hoe u verschillende Ansible-taken met Bash uitvoert in Azure Cloud Shell
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 08/07/2018
ms.topic: article
ms.openlocfilehash: 9928f646905dd0da4b15166ec55e5d8a183cb210
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "42059424"
---
# <a name="run-ansible-with-bash-in-azure-cloud-shell"></a>Ansible uitvoeren met Bash in Azure Cloudshell

In deze zelfstudie leert u hoe u Bash in Cloud Shell gebruiken voor het configureren van een Azure-abonnement als uw werkruimte Ansible. 

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement** : als u geen Azure-abonnement, maak een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

- **Configureren van Azure Cloud Shell** : als u niet bekend bent met Azure Cloud Shell, het artikel [Snelstartgids voor Bash in Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart), ziet u hoe u starten en configureren van Cloud Shell. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>De configuratie van automatische referenties

Wanneer u bent aangemeld bij de Cloud Shell, verifieert Ansible met Azure voor het beheren van infrastructuur zonder extra configuratie. Als u meer dan één abonnement hebt, kunt u kiezen welk abonnement u Ansible door te exporteren met werkt de `AZURE_SUBSCRIPTION_ID` omgevingsvariabele. Als u al uw Azure-abonnementen, moet u de volgende opdracht uitvoeren:

```azurecli-interactive
az account list
```

Met behulp van de **id** instellen van het abonnement waarmee u wilt werken, de **AZURE_SUBSCRIPTION_ID** als volgt:

```azurecli-interactive
export AZURE_SUBSCRIPTION_ID=<your-subscription-id>
```

## <a name="verify-the-configuration"></a>Controleer de configuratie
Als u wilt controleren of de juiste configuratie, Ansible te gebruiken om een resourcegroep te maken.

[!INCLUDE [create-resource-group-with-ansible.md](../../includes/ansible-create-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"] 
> [Een eenvoudige virtuele machine maken in Azure met Ansible](/azure/virtual-machines/linux/ansible-create-vm)
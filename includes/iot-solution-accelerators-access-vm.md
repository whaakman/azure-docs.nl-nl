---
title: bestand opnemen
description: bestand opnemen
services: iot-accelerators
author: dominicbetts
ms.service: iot-accelerators
ms.topic: include
ms.date: 08/16/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 8d1f0f6ae3ffc123f1ae8318a6fbce4a9a3024fa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66814884"
---
## <a name="access-the-virtual-machine"></a>Toegang tot de virtuele machine

De volgende stappen uit de Azure CLI gebruiken in Azure Cloud Shell. Als u liever, kunt u [Azure CLI installeren](/cli/azure/install-azure-cli) op de ontwikkeling van uw computer en de opdrachten lokaal uitvoeren.

De volgende stappen laten zien hoe het configureren van de virtuele machine van Azure waarmee u **SSH** toegang. De weergegeven stappen wordt ervan uitgegaan dat de naam die u hebt gekozen voor de solution accelerator is **contoso-simulatie** --Vervang deze waarde door de naam van uw implementatie:

1. Overzicht van de inhoud van de resourcegroep die de solution accelerator resources bevat:

    ```azurecli-interactive
    az resource list -g contoso-simulation -o table
    ```

    Noteer de naam van de virtuele machine, het openbare IP-adres en de netwerkbeveiligingsgroep: u hebt deze waarden later nodig.

1. De netwerkbeveiligingsgroep voor SSH-toegang bijwerken. De volgende opdracht wordt ervan uitgegaan dat de naam van de netwerkbeveiligingsgroep is **contoso-simulatie-nsg** --Vervang deze waarde door de naam van uw netwerkbeveiligingsgroep:

    ```azurecli-interactive
    az network nsg rule update --name SSH --nsg-name contoso-simulation-nsg -g contoso-simulation --access Allow -o table
    ```

    Alleen SSH-toegang inschakelen tijdens het testen en ontwikkeling. Als u SSH schakelt, [moet u dit opnieuw zo snel mogelijk uitschakelen](https://docs.microsoft.com/azure/security/azure-security-network-security-best-practices#disable-rdpssh-access-to-virtual-machines).

1. Bijwerken van het wachtwoord voor de **azureuser** account op de virtuele machine naar een wachtwoord die u kent. Kies uw eigen wachtwoord wanneer u de volgende opdracht uitvoeren:

    ```azurecli-interactive
    az vm user update --name vm-vikxv --username azureuser --password YOURSECRETPASSWORD  -g contoso-simulation
    ```

1. Het openbare IP-adres van uw virtuele machine vinden. De volgende opdracht wordt ervan uitgegaan dat de naam van de virtuele machine is **vm-vikxv** --Vervang deze waarde door de naam van de virtuele machine die u eerder een notitie van gemaakt:

    ```azurecli-interactive
    az vm list-ip-addresses --name vm-vikxv -g contoso-simulation -o table
    ```

    Noteer het openbare IP-adres van uw virtuele machine.

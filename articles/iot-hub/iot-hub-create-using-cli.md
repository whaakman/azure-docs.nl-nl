---
title: Een IoT-Hub met behulp van Azure CLI maken | Microsoft Docs
description: Het maken van een Azure IoT-hub met behulp van Azure CLI.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/23/2018
ms.author: robinsh
ms.openlocfilehash: 90830c4e27e90af6c9d77509844696f64e4909f0
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46994765"
---
# <a name="create-an-iot-hub-using-the-azure-cli"></a>Maken van een IoT-hub met behulp van de Azure CLI

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Dit artikel laat u het maken van een IoT-hub met behulp van Azure CLI.

## <a name="prerequisites"></a>Vereisten

Als u wilt deze procedures hebt voltooid, moet u een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="sign-in-and-set-your-azure-account"></a>Meld u aan en stel uw Azure-account

Als u Azure CLI lokaal in plaats van met behulp van Cloud Shell uitvoert, moet u zich aanmeldt bij uw Azure-account.

Voer bij de opdrachtprompt de [aanmeldingsopdracht](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli):

    ```azurecli
    az login
    ```

Volg de instructies om te verifiëren met de code en meld u aan bij uw Azure-account via een webbrowser.

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

Azure CLI gebruiken voor het maken van een resourcegroep en voegt u een IoT-hub.

1. Wanneer u een IoT-hub maakt, moet u deze in een resourcegroep maken. Gebruik een bestaande resourcegroep of Voer de volgende [opdracht voor het maken van een resourcegroep](https://docs.microsoft.com/cli/azure/resource):
    
   ```azurecli
   az group create --name {your resource group name} --location westus
   ```

   > [!TIP]
   > In het voorbeeld wordt de resourcegroep gemaakt in de locatie US - west. U kunt een lijst met beschikbare locaties weergeven door het uitvoeren van deze opdracht: 
   >
   >``` bash
   >az account list-locations -o table
   >```
   >

2. Voer de volgende [opdracht voor het maken van een IoT-hub](https://docs.microsoft.com/cli/azure/iot/hub#az-iot-hub-create) in de resourcegroep, met behulp van een wereldwijd unieke naam voor uw IoT-hub:
    
   ```azurecli
   az iot hub create --name {your iot hub name} \
      --resource-group {your resource group name} --sku S1
   ```

   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


De vorige opdracht maakt u een IoT-hub in de S1-prijscategorie die in rekening worden gebracht. Zie voor meer informatie, [prijzen van Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/).

## <a name="remove-an-iot-hub"></a>Een IoT-Hub verwijderen

U kunt Azure CLI voor [een afzonderlijke resource verwijderen](https://docs.microsoft.com/cli/azure/resource), zoals een IoT-hub of verwijder een resourcegroep en alle bijbehorende resources, met inbegrip van een IoT-hubs.

Naar [verwijderen van een IoT-hub](https://docs.microsoft.com/cli/azure/iot/hub#az-iot-hub-delete), voer de volgende opdracht uit:

```azurecli
az iot hub delete --name {your iot hub name} -\
  -resource-group {your resource group name}
```

Naar [verwijderen van een resourcegroep](https://docs.microsoft.com/cli/azure/group#az-group-delete) en alle bijbehorende resources, voer de volgende opdracht uit:

```azurecli
az group delete --name {your resource group name}
```

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over het gebruik van een IoT-hub:

* [Ontwikkelaarshandleiding voor IoT Hub](iot-hub-devguide.md)
* [Met behulp van de Azure-portal voor het beheren van IoT Hub](iot-hub-create-through-portal.md)

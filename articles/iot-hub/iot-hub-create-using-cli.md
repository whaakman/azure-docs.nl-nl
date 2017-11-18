---
title: Maken van een IoT Hub met Azure CLI (az.py) | Microsoft Docs
description: Het maken van een Azure-IoT-hub met behulp van de platformoverschrijdende Azure CLI 2.0 (az.py).
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-hub
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.openlocfilehash: 161089159999a4a63a39b059e69a08b7a9297445
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="create-an-iot-hub-using-the-azure-cli-20"></a>Een iothub met behulp van de Azure CLI 2.0 maken

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Inleiding

U kunt Azure CLI 2.0 (az.py) maken en beheren van Azure IoT hubs programmatisch gebruiken. In dit artikel laat zien hoe de Azure CLI 2.0 (az.py) gebruiken voor het maken van een IoT-hub.

U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

* [Azure CLI (azure.js)](iot-hub-create-using-cli-nodejs.md) – de CLI voor het klassieke en resource management-implementatiemodel.
* Azure CLI 2.0 (az.py) - de volgende generatie CLI voor de resource management-implementatiemodel zoals beschreven in dit artikel.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Een actief Azure-account. Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.
* [Azure CLI 2.0][lnk-CLI-install].

## <a name="sign-in-and-set-your-azure-account"></a>Aanmelden en uw Azure-account instellen

Aanmelden bij uw Azure-account en uw abonnement te selecteren.

1. Voer bij de opdrachtprompt de [aanmelding opdracht][lnk-login-command]:
    
    ```azurecli
    az login
    ```

    Volg de instructies om te verifiëren met de code en meld u aan bij uw Azure-account via een webbrowser.

2. Als u meerdere Azure-abonnementen hebt, aanmelden bij Azure u toegang verleent tot de Azure accounts die zijn gekoppeld aan uw referenties. Gebruik de volgende [opdracht om een lijst van de Azure-accounts] [ lnk-az-account-command] beschikbaar moet worden gebruikt:
    
    ```azurecli
    az account list 
    ```

    Gebruik de volgende opdracht om abonnement die u gebruiken wilt voor het uitvoeren van de opdrachten voor het maken van uw IoT-hub te selecteren. U kunt de naam van abonnement of de ID van de uitvoer van de vorige opdracht gebruiken:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

De Azure CLI gebruiken voor het maken van een resourcegroep en voegt u een IoT-hub.

1. Wanneer u een IoT-hub maakt, moet u deze in een resourcegroep maken. Gebruik een bestaande resourcegroep of Voer de volgende [opdracht voor het maken van een resourcegroep][lnk-az-resource-command]:
    
    ```azurecli
     az group create --name {your resource group name} --location westus
    ```

    > [!TIP]
    > Het vorige voorbeeld maakt de resourcegroep in de locatie VS-West. U kunt een lijst met beschikbare locaties weergeven met de opdracht `az account list-locations -o table`.
    >
    >

2. Voer de volgende [opdracht voor het maken van een IoT-hub] [ lnk-az-iot-command] met behulp van een globaal unieke naam voor uw IoT-hub in de resourcegroep:
    
    ```azurecli
    az iot hub create --name {your iot hub name} --resource-group {your resource group name} --sku S1
    ```

   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


> [!NOTE]
> De vorige opdracht maakt u een IoT-hub in de S1 prijscategorie waarvoor u wordt gefactureerd. Zie voor meer informatie [prijzen van Azure IoT Hub][lnk-iot-pricing].
>
>

## <a name="remove-an-iot-hub"></a>Verwijderen van een IoT-Hub

U kunt de Azure CLI voor [verwijderen van een afzonderlijke resource][lnk-az-resource-command], zoals een IoT-hub of verwijder een resourcegroep en alle bijbehorende bronnen, met inbegrip van een IoT-hubs.

Voer de volgende opdracht voor het verwijderen van een IoT-hub:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

Voer de volgende opdracht voor het verwijderen van een resourcegroep en alle bijbehorende resources:

```azurecli
az group delete --name {your resource group name}
```

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over het ontwikkelen voor IoT-Hub:

* [Ontwikkelaarshandleiding voor IoT Hub][lnk-devguide]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [IoT Hub beheren met behulp van de Azure-portal][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-CLI-install]: https://docs.microsoft.com/cli/azure/install-az-cli2
[lnk-login-command]: https://docs.microsoft.com/cli/azure/get-started-with-az-cli2
[lnk-az-account-command]: https://docs.microsoft.com/cli/azure/account
[lnk-az-register-command]: https://docs.microsoft.com/cli/azure/provider
[lnk-az-addcomponent-command]: https://docs.microsoft.com/cli/azure/component
[lnk-az-resource-command]: https://docs.microsoft.com/cli/azure/resource
[lnk-az-iot-command]: https://docs.microsoft.com/cli/azure/iot
[lnk-iot-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-devguide]: iot-hub-devguide.md
[lnk-portal]: iot-hub-create-through-portal.md 

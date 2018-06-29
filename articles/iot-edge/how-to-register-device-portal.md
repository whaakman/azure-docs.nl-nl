---
title: Registreren van een nieuwe Azure IoT randapparaat (portal) | Microsoft Docs
description: Gebruik de Azure-portal voor het registreren van een nieuwe IoT-randapparaat
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/05/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: b61594469df33e11c23c9cbe0b9542da374fefa3
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035889"
---
# <a name="register-a-new-azure-iot-edge-device-from-the-azure-portal"></a>Registreren van een nieuwe Azure IoT Edge-apparaat via de Azure portal

Voordat u uw IoT-apparaten met Azure IoT rand gebruiken kunt, moet u deze registreren met uw IoT-hub. Wanneer u een apparaat registreert, krijgt u een verbindingsreeks die kan worden gebruikt voor het instellen van uw apparaat voor werkbelastingen van de rand. 

In dit artikel laat zien hoe u een nieuwe IoT Edge-apparaat met de Azure portal registreren.

## <a name="prerequisites"></a>Vereisten

* Een [IoT-hub](../iot-hub/iot-hub-create-through-portal.md) in uw Azure-abonnement. 

## <a name="create-a-device"></a>Een apparaat maken

In de Azure portal IoT randapparaten gemaakt en afzonderlijk beheerd vanaf apparaten die verbinding maken met uw IoT-hub, maar niet rand ingeschakeld. 

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en navigeer naar uw IoT-hub. 
2. Selecteer **IoT rand** in het menu.
3. Selecteer **IoT randapparaat toevoegen**. 
4. Geef een beschrijvende apparaat-ID. 
5. Selecteer **Opslaan**. 

## <a name="view-all-devices"></a>Alle apparaten weergeven

Alle de edge-apparaten die verbinding met uw IoT-hub maken worden vermeld op de **IoT rand** pagina. 

## <a name="retrieve-the-connection-string"></a>De verbindingsreeks ophalen

Wanneer u klaar bent voor het instellen van uw apparaat, moet u de verbindingsreeks die is gekoppeld aan uw fysieke apparaat met de identiteit van de IoT-hub.

1. Van de **IoT rand** pagina in de portal, klikt u op het apparaat-ID uit de lijst met de randapparaten van. 
2. Kopieer de waarde van een van beide **verbindingsreeks: primaire sleutel** of **verbindingsreeks: secundaire sleutel**. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [modules implementeert op een apparaat met de Azure-portal](how-to-deploy-modules-portal.md)
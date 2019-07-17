---
title: bestand opnemen
description: bestand opnemen
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 04/19/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 367a0b1d17f8d5ebe4f46835ace963b00e75354e
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "68229315"
---
Een IoT-hub maken met behulp van Azure Portal:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer **Een resource maken** > **Internet of Things** > **IoT Hub**.

    ![Selecteren om IoT Hub te installeren](media/iot-hub-tutorials-create-free-hub/selectiothub.png)

1. Gebruik de waarden in de volgende tabellen om uw IoT-hub in de gratis laag te maken:

    | Instelling | Waarde |
    | ------- | ----- |
    | Subscription | Selecteer uw Azure-abonnement in de vervolgkeuzelijst. |
    | Resource group | Nieuwe maken. In deze zelfstudie wordt de naam **tutorials-iot-hub-rg** gebruikt. |
    | Regio | In deze zelfstudie wordt gebruikgemaakt van **US - west**. U kunt de regio kiezen die zich het meest in uw buurt bevindt. |
    | Name | In de volgende schermafbeelding wordt de naam **tutorials-iot-hub** gebruikt. U moet uw eigen unieke naam kiezen wanneer u uw hub maakt. |

    ![Hub-instellingen 1](media/iot-hub-tutorials-create-free-hub/hubdefinition-1.png)

    | Instelling | Value |
    | ------- | ----- |
    | Prijs- en schaalniveau | F1 Gratis. U kunt maar één hub in de gratis laag in een abonnement hebben. |
    | IoT Hub-eenheden | 1 |

    ![Hub-instellingen 2](media/iot-hub-tutorials-create-free-hub/hubdefinition-2.png)

1. Klik op **Create**. Het kan enkele minuten duren voordat de hub is gemaakt.

    ![Hub-instellingen 3](media/iot-hub-tutorials-create-free-hub/hubdefinition-3.png)

1. Noteer de naam van de IoT-hub die u hebt gekozen. U gebruikt deze waarde verderop in de zelfstudie.

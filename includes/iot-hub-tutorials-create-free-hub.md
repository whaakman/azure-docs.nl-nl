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
ms.openlocfilehash: e3110e4018e214e7e7aa591b811246369c029ecd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34667182"
---
Een IoT-hub maken met behulp van Azure Portal:

1. Meld u aan bij [Azure Portal](http://portal.azure.com).

1. Selecteer **Een resource maken** > **Internet of Things** > **IoT Hub**.

    ![Selecteren om IoT Hub te installeren](media/iot-hub-tutorials-create-free-hub/selectiothub.png)

1. Gebruik de waarden in de volgende tabellen om uw IoT-hub in de gratis laag te maken:

    | Instelling | Waarde |
    | ------- | ----- |
    | Abonnement | Selecteer uw Azure-abonnement in de vervolgkeuzelijst. |
    | Resourcegroep | Nieuwe maken. In deze zelfstudie wordt de naam **tutorials-iot-hub-rg** gebruikt. |
    | Regio | In deze zelfstudie wordt gebruikgemaakt van **VS-West**. U kunt de regio kiezen die zich het meest in uw buurt bevindt. |
    | Naam | In de volgende schermafbeelding wordt de naam **tutorials-iot-hub** gebruikt. U moet uw eigen unieke naam kiezen wanneer u uw hub maakt. |

    ![Hub-instellingen 1](media/iot-hub-tutorials-create-free-hub/hubdefinition-1.png)

    | Instelling | Waarde |
    | ------- | ----- |
    | Prijs- en schaalniveau | F1 Gratis. U kunt maar één hub in de gratis laag in een abonnement hebben. |
    | IoT Hub-eenheden | 1 |

    ![Hub-instellingen 2](media/iot-hub-tutorials-create-free-hub/hubdefinition-2.png)

1. Klik op **Create**. Het kan enkele minuten duren voordat de hub is gemaakt.

    ![Hub-instellingen 3](media/iot-hub-tutorials-create-free-hub/hubdefinition-3.png)

1. Noteer de naam van de IoT-hub die u hebt gekozen. U gebruikt deze waarde verderop in de zelfstudie.
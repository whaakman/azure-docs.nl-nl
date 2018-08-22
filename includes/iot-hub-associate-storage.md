---
title: bestand opnemen
description: bestand opnemen
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 08/20/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 892dd050829dce242035e2b875ea43ed13910d4a
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/21/2018
ms.locfileid: "40246071"
---
## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Een Azure Storage-account naar IoT Hub koppelen

Omdat het gesimuleerde apparaat-app wordt een bestand geüpload naar een blob, hebt u een [Azure Storage](../articles/storage/common/storage-quickstart-create-account.md) account dat is gekoppeld aan IoT-Hub. Wanneer u een Azure Storage-account aan een IoT-hub koppelt, genereert de IoT-hub een SAS-URI. Een apparaat kunt deze SAS-URI gebruiken om een bestand veilig te uploaden naar een blob-container. De IoT Hub-service en de apparaat-SDK's samen om het proces dat de SAS-URI genereert en maakt ze beschikbaar voor een apparaat te gebruiken om een bestand te uploaden.

Volg de instructies in [configureren-bestand wordt geüpload met behulp van de Azure-portal](../articles/iot-hub/iot-hub-configure-file-upload.md) om te koppelen van een Azure Storage-account naar uw IoT hub. Zorg ervoor dat er een blob-container gekoppeld aan uw IoT-hub is en dat bestandsmeldingen zijn ingeschakeld.

![Schakel meldingen in bestand in de portal](media/iot-hub-associate-storage/enable-file-notifications.png)
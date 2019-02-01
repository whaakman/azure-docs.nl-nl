---
title: bestand opnemen
description: bestand opnemen
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 01/22/2019
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 8693c48905155ed757bb727e42f4180f36c015f1
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55513989"
---
## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Een Azure Storage-account naar IoT Hub koppelen

Omdat het gesimuleerde apparaat-app wordt een bestand geüpload naar een blob, hebt u een [Azure Storage](../articles/storage/common/storage-quickstart-create-account.md) dat is gekoppeld aan uw IoT-hub. Wanneer u een Azure Storage-account aan een IoT-hub koppelt, genereert de IoT-hub een SAS-URI. Een apparaat kunt deze SAS-URI gebruiken om een bestand veilig te uploaden naar een blob-container. De IoT Hub-service en de apparaat-SDK's samen om het proces dat de SAS-URI genereert en maakt ze beschikbaar voor een apparaat te gebruiken om een bestand te uploaden.

Volg de instructies in [configureren-bestand wordt geüpload met behulp van de Azure-portal](../articles/iot-hub/iot-hub-configure-file-upload.md). Zorg ervoor dat er een blob-container gekoppeld aan uw IoT-hub is en dat bestandsmeldingen zijn ingeschakeld.

![Schakel meldingen in bestand in de portal](./media/iot-hub-associate-storage/enable-file-notifications.png)
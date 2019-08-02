---
title: bestand opnemen
description: bestand opnemen
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 07/24/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 103ad020b8d9f50ffe690f502b7cac08dab9237a
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68640450"
---
* Een actief Azure-account. (Als u geen account hebt, kunt u in slechts een paar minuten een [gratis account](https://azure.microsoft.com/pricing/free-trial/) maken.)

* **Windows**

    * [Python 2. x of 3. x](https://www.python.org/downloads/). Zorg ervoor dat u de 32-bits of 64-bits installatie gebruikt, zoals vereist door uw configuratie. Zorg ervoor dat u Python toevoegt aan uw platformspecifieke omgevingsvariabele als u hierom wordt gevraagd tijdens de installatie. Als u Python 2.x gebruikt, moet u mogelijk [pip *installeren of upgraden*, het Python-pakketbeheersysteem](https://pip.pypa.io/en/stable/installing/).

    * Als u Windows-besturings systeem gebruikt, zorg er dan voor dat u de juiste versie van het [ C++ Visual Redistributable-pakket](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) hebt om het gebruik van systeem eigen dll's van python mogelijk te maken. U wordt aangeraden de meest recente versie te gebruiken.

    * Installeer, indien nodig, het [Azure-iothub-Device-client-](https://pypi.org/project/azure-iothub-device-client/) pakket met behulp van de opdracht`pip install azure-iothub-device-client`

    * Installeer, indien nodig, het [Azure-iothub-service-client](https://pypi.org/project/azure-iothub-service-client/) pakket met behulp van de opdracht`pip install azure-iothub-service-client`

* **Mac OS**

    Voor Mac OS hebt u python 3.7.0 (of 2,7) + libboost-1,67 + krul 7.61.1 (alle geïnstalleerd via homebrew) nodig. Elke andere distributie/elk ander besturings systeem bevat waarschijnlijk verschillende versies van Boost & afhankelijkheden die niet werken, en resulteert in een ImportError tijdens runtime.

    De *pip*-pakketten voor `azure-iothub-service-client` en `azure-iothub-device-client` zijn momenteel alleen beschikbaar voor het Windows-besturingssysteem. Voor Linux/Mac OS raadpleegt u de sectie met Linux-en Mac OS-specifieke secties in de [ontwikkel omgeving voorbereiden voor python](https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md) post.

> [!NOTE]
> Er zijn verschillende fouten rapporten bij het importeren van iothub_client in een voor beeld. Zie [omgaan met ImportError-problemen](https://github.com/Azure/azure-iot-sdk-python#important-installation-notes---dealing-with-importerror-issues)voor meer informatie over het oplossen van problemen met **ImportError** .

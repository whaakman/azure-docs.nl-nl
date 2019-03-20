---
title: Vereisten voor Azure IoT Edge-module | Microsoft Docs
description: Vereisten voor het publiceren van een IoT Edge-module.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: pbutlerm
ms.openlocfilehash: a4f1023bdf8a49fccbbda1fd0dc537f83a3acee1
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58013812"
---
# <a name="iot-edge-module-publishing-prerequisites"></a>IoT Edge module publishing vereisten

Dit artikel beschrijft de vereisten voor het publiceren van een aanbieding van IoT Edge-module.  Als u hebt nog niet gedaan, raadpleegt u de [IoT Edge-modules publicatiehandleiding voor](../..//iot-edge-module.md).


## <a name="publishing-prerequisites"></a>Vereisten voor publiceren

U hebt voor het publiceren van een IoT Edge-module in de Azure Marketplace, om te voldoen aan de volgende vereisten:

<!-- P2: It would be great to point to the terms of use of CPP here. This can often be a blocker for big companies and these terms of use are not anonymously visible yet.-->
- Toegang tot de [Cloud Partner-Portal](https://cloudpartner.azure.com/). Zie voor meer informatie, [Azure Marketplace en AppSource publicatiehandleiding voor](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).
- Overeenkomst voor de [voorwaarden voor Azure Marketplace](https://azure.microsoft.com/support/legal/marketplace-terms/)
- Host uw technische asset van IoT Edge-module in een Azure Container Registry.  Zie voor meer informatie, [het voorbereiden van uw IoT Edge module technische asset](./cpp-create-technical-assets.md)
- Uw IoT Edge module metagegevens die klaar voor gebruik hebben. Bijvoorbeeld, het voorbereiden van de volgende elementen:
    - Een titel
    - Een beschrijving (in HTML-indeling)
    - Een installatiekopie van het logo (PNG-indeling en vaste grootte zoals 40x40px, 90x90px, 115x115px, 255x115px)
    - Een termijn van en het privacybeleid
    - Een standaardconfiguratie van de module die bevat: routes, twin gewenste eigenschappen, createOptions en omgevingsvariabelen.
    - Module-documentatie
    - Contactpersonen voor ondersteuning


## <a name="next-steps"></a>Volgende stappen

Als u eenmaal hebt [bereid uw IoT Edge module technische asset](./cpp-create-technical-assets.md), kunt u zich kunt [maken van uw IoT Edge-module-aanbieding](./cpp-create-offer.md). 

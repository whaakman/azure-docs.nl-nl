---
title: Vereisten voor Azure IoT Edge-module | Microsoft Docs
description: Vereisten voor het publiceren van een IoT Edge-module.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: pbutlerm
ms.openlocfilehash: 7271a97dc7ab9de1840d809ded0ba1c2940ed83f
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49431169"
---
# <a name="iot-edge-module-publishing-prerequisites"></a>IoT Edge module publishing vereisten

Dit artikel beschrijft de vereisten voor het publiceren van een aanbieding van IoT Edge-module.

Zie voor meer informatie over IoT Edge-modules en de voordelen van de publicatie van een module in de Azure Marketplace, de [IoT Edge-modules publicatiehandleiding voor](https://docs.microsoft.com/azure/marketplace/iot-edge-module).

## <a name="publishing-prerequisites"></a>Vereisten voor publiceren

U hebt voor het publiceren van een IoT Edge-module in de Azure Marketplace, om te voldoen aan de volgende vereisten:

<!-- P2: It would be great to point to the terms of use of CPP here. This can often be a blocker for big companies and these terms of use are not anonymously visible yet.-->
- Toegang tot de [Cloud Partner-Portal](https://cloudpartner.azure.com/). Zie voor meer informatie, [Azure Marketplace en AppSource publicatiehandleiding voor](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).
- Overeenkomst voor de [voorwaarden voor Azure Marketplace](https://azure.microsoft.com/support/legal/marketplace-terms/)
- Host uw technische asset van IoT Edge-module in een Azure Container Registry.  Zie voor meer informatie, [het voorbereiden van uw IoT Edge module technische asset](./cpp-create-technical-assets.md)
- Uw IoT Edge module metagegevens die klaar voor gebruik hebben. Bijvoorbeeld: (niet-uitputtende lijst):
    - Een titel
    - Een beschrijving (in HTML-indeling)
    - Een installatiekopie van het logo (PNG-indeling en vaste grootte zoals 40x40px, 90x90px, 115x115px, 255x115px)
    - Een termijn van en het privacybeleid
    - Een standaardconfiguratie van de module die bevat: routes, twin gewenste eigenschappen, createOptions en omgevingsvariabelen.
    - Documentatie
    - Contactpersonen voor ondersteuning

## <a name="next-steps"></a>Volgende stappen

- [Uw IoT Edge module technische middel voorbereiden](./cpp-create-technical-assets.md)
- [Uw IoT Edge-module-aanbieding maken](./cpp-create-offer.md)

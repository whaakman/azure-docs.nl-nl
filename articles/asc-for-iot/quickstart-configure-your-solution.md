---
title: Uw Azure Security Center configureren voor IoT-oplossing | Microsoft Docs
description: Meer informatie over het configureren van uw end-to-end IoT-oplossing met behulp van Azure Security Center voor IoT.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: ae2207e8-ac5b-4793-8efc-0517f4661222
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 15c8aabbcb19d009bb202d111dc7f80da4cebff1
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68597026"
---
# <a name="quickstart-configure-your-iot-solution"></a>Quickstart: Uw IoT-oplossing configureren

Dit artikel bevat een uitleg over het uitvoeren van de eerste configuratie van uw IoT-beveiligings oplossing met behulp van Azure Security Center voor IoT. 

## <a name="azure-security-center-for-iot"></a>Azure Security Center voor IoT

Azure Security Center voor IoT biedt uitgebreide end-to-end beveiliging voor IoT-oplossingen op basis van Azure.

Met Azure Security Center voor IoT kunt u uw volledige IoT-oplossing in één dash board bewaken, halen u al uw IoT-apparaten, IoT-platformen en back-end-resources in Azure.

Zodra de IoT Hub is ingeschakeld, identificeert Azure Security Center voor IoT automatisch andere Azure-Services, die ook zijn verbonden met uw IoT-hub en verband houden met uw IoT-oplossing.

Naast automatische relatie detectie kunt u ook kiezen welke andere Azure-resource groepen u wilt labelen als onderdeel van uw IoT-oplossing.

Met uw selecties kunt u volledige abonnementen, resource groepen of afzonderlijke resources toevoegen.

Nadat u alle resource relaties hebt gedefinieerd, Azure Security Center voor IoT maakt gebruik van Azure Security Center om u te voorzien in beveiligings aanbevelingen en waarschuwingen voor deze resources.

## <a name="add-azure-resources-to-your-iot-solution"></a>Azure-resources toevoegen aan uw IoT-oplossing

Ga als volgt te werk om een nieuwe resource toe te voegen aan uw IoT-oplossing: 

1. Open uw **IOT hub** in azure Portal. 
2. Selecteer en open **resources** onder **beveiliging** in het menu links. 
3. Selecteer **bewerken** en kies de resource groepen die deel uitmaken van uw IOT-oplossing.
5. Klik op **Toevoegen**. 

Gefeliciteerd! U hebt een nieuwe resource groep toegevoegd aan uw IoT-oplossing.

Azure Security Center voor IoT bewaakt u nu nieuwe resource groepen en Opper vlakken van relevante beveiligings aanbevelingen en-waarschuwingen als onderdeel van uw IoT-oplossing.

## <a name="next-steps"></a>Volgende stappen

Ga naar het volgende artikel voor meer informatie over het maken van beveiligings modules...

> [!div class="nextstepaction"]
> [Beveiligings modules maken](quickstart-create-security-twin.md)
---
title: Configureren van uw Azure Security Center voor IoT-oplossing Preview | Microsoft Docs
description: Informatie over het configureren van uw end-to-end-IoT-oplossing met behulp van Azure Security Center voor IoT.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: ae2207e8-ac5b-4793-8efc-0517f4661222
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 64a04861b14e48eaa14a369546dd9d54bf59f019
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58861918"
---
# <a name="quickstart-configure-your-iot-solution"></a>Quickstart: Configureren van uw IoT-oplossing

> [!IMPORTANT]
> Azure Security Center voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

In dit artikel bevat een uitleg van het uitvoeren van de oorspronkelijke configuratie van uw IoT-beveiliging-oplossing met behulp van ASC voor IoT. 

## <a name="azure-security-center-asc-for-iot"></a>Azure Security Center (ASC) voor IoT

ASC voor IoT biedt volledige end-to-end beveiliging voor op basis van Azure IoT-oplossingen.

Met ASC voor IoT, kunt u uw volledige IoT-oplossing in één dashboard, bewaken zichtbaar te maken van al uw IoT-apparaten, IoT-platformen en back-endresources in Azure.

Eenmaal is ingeschakeld op uw IoT-Hub, identificeert ASC voor IoT automatisch andere Azure-services ook verbonden met uw IoT-hub en die betrekking hebben op uw IoT-oplossing.

Naast de automatische relatiedetectie, kunt u ook kiezen en kiezen welke andere Azure-resources op tag als onderdeel van uw IoT-oplossing.
Uw selecties kunnen u volledige abonnementen, resourcegroepen of afzonderlijke resources toe te voegen.

Na het definiëren van alle van de resource-relaties, ASC voor IoT maakt gebruik van Azure Security Center zodat u aanbevelingen voor beveiliging en waarschuwingen voor deze resources.

## <a name="add-azure-resources-to-your-iot-solution"></a>Azure-resources toevoegen aan uw IoT-oplossing

Nieuwe resource toevoegen aan uw IoT-oplossing, het volgende doen: 

1. Open uw **IoT-Hub** in Azure portal. 
2. Selecteer en open **Resources** onder **Security** in het menu links. 
3. Selecteer **resources toevoegen**.
4. Kies de resources die deel uitmaken van uw IoT-oplossing.
5. Klik op **Add**. 

Gefeliciteerd! U hebt een nieuwe resource toegevoegd aan uw IoT-oplossing.

ASC voor IoT nu monitors die u zojuist hebt toegevoegd resources, en de relevante beveiligingsaanbevelingen oppervlakken en waarschuwingen als onderdeel van uw IoT-oplossing.

## <a name="next-steps"></a>Volgende stappen

Ga naar het volgende artikel voor meer informatie over het maken van de beveiligingsmodules...

> [!div class="nextstepaction"]
> [Beveiligingsmodules maken](quickstart-create-security-twin.md)
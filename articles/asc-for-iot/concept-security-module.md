---
title: Meer informatie over ASC voor IoT security moduledubbels Preview | Microsoft Docs
description: Meer informatie over het concept van moduledubbels security en hoe ze worden gebruikt in ASC voor IoT.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: a5c25cba-59a4-488b-abbe-c37ff9b151f9
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 63d21cc5027145ab87030bd2561bc5087298f16c
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541840"
---
# <a name="security-module"></a>Beveiligingsmodule

> [!IMPORTANT]
> ASC voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

In dit artikel wordt uitgelegd hoe ASC voor IoT gebruikt apparaatdubbels en modules. 

## <a name="device-twins"></a>Apparaatdubbels

Apparaatdubbels spelen voor IoT-oplossingen die zijn gemaakt in Azure, een belangrijke rol in Apparaatbeheer en automatisering van bedrijfsprocessen.  

ASC voor IoT biedt volledige integratie met uw bestaande IoT platform voor Apparaatbeheer, zodat u kunt de beveiligingsstatus van uw apparaat te beheren en maken gebruik van bestaande mogelijkheden voor toegangsbeheer van apparaat. Integratie wordt bereikt met behulp van de IoT-Hub twin mechanisme.  

Meer informatie over het concept van [apparaat](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins) twins in Azure IoT Hub. 

## <a name="security-module-twins"></a>Beveiliging moduledubbels

ASC voor IoT onderhoudt een moduledubbel beveiliging voor elk apparaat in de service. De moduledubbel beveiliging bevat alle informatie die relevant zijn voor de beveiliging voor elk apparaat voor specifieke apparaten in uw oplossing. Beveiliging van apparaateigenschappen worden bijgehouden in een speciale moduledubbel voor veiliger communicatie en voor het inschakelen van updates en onderhoud waarvoor minder bronnen.  

Zie [maken security moduledubbel](quickstart-create-security-twin.md) en [security-agents configureren](concept-agent-configuration.md) voor informatie over het maken, aanpassen en het dubbele configureren. Zie [informatie over moduledubbels](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-module-twins) voor meer informatie over het concept van moduledubbels in IoT Hub. 
 

## <a name="see-also"></a>Zie ook
- [ASC voor IoT-Preview](overview.md)
- [Beveiliging-agents implementeren](select-deploy-agent.md)
- [Security agent verificatiemethoden](concept-security-agent-authentication-methods.md)
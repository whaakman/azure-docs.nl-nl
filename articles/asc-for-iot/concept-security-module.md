---
title: Inzicht in Azure Security Center voor IoT-beveiligingsmodule dubbels Preview | Microsoft Docs
description: Meer informatie over het concept van moduledubbels security en hoe ze worden gebruikt in Azure Security Center voor IoT.
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
ms.openlocfilehash: 56bc93973bd2c080b13b97f1344cc165f742e757
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758408"
---
# <a name="security-module"></a>Beveiligingsmodule

> [!IMPORTANT]
> Azure Security Center voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

In dit artikel wordt uitgelegd hoe Azure Security Center (ASC) voor IoT gebruikt apparaatdubbels en modules. 

## <a name="device-twins"></a>Apparaatdubbels

Apparaatdubbels spelen voor IoT-oplossingen die zijn gemaakt in Azure, een belangrijke rol in Apparaatbeheer en automatisering van bedrijfsprocessen.  

ASC voor IoT biedt volledige integratie met uw bestaande IoT platform voor Apparaatbeheer, zodat u kunt de beveiligingsstatus van uw apparaat te beheren en maken gebruik van bestaande mogelijkheden voor toegangsbeheer van apparaat. Integratie wordt bereikt met behulp van de IoT-Hub twin mechanisme.  

Meer informatie over het concept van [apparaatdubbels](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins) in Azure IoT Hub. 

## <a name="security-module-twins"></a>Beveiliging moduledubbels

ASC voor IoT onderhoudt een moduledubbel beveiliging voor elk apparaat in de service.
De moduledubbel beveiliging bevat alle informatie die relevant zijn voor de beveiliging voor elk apparaat voor specifieke apparaten in uw oplossing.
Beveiliging van apparaateigenschappen worden bijgehouden in een speciale moduledubbel voor veiliger communicatie en voor het inschakelen van updates en onderhoud waarvoor minder bronnen.  

Zie [maken security moduledubbel](quickstart-create-security-twin.md) en [security-agents configureren](how-to-agent-configuration.md) voor informatie over het maken, aanpassen en het dubbele configureren. Zie [informatie over moduledubbels](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-module-twins) voor meer informatie over het concept van moduledubbels in IoT Hub. 
 

## <a name="see-also"></a>Zie ook
- [ASC voor IoT-Preview](overview.md)
- [Beveiliging-agents implementeren](how-to-deploy-agent.md)
- [Security agent verificatiemethoden](concept-security-agent-authentication-methods.md)
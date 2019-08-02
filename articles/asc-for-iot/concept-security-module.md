---
title: Meer informatie over Azure Security Center voor IoT Security module apparaatdubbels | Microsoft Docs
description: Meer informatie over het concept van de Security module apparaatdubbels en hoe deze worden gebruikt in Azure Security Center voor IoT.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: a5c25cba-59a4-488b-abbe-c37ff9b151f9
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2019
ms.author: mlottner
ms.openlocfilehash: ab3b6e740e644a1ed1495eb776045888be448047
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68596496"
---
# <a name="security-module"></a>Beveiligingsmodule


In dit artikel wordt uitgelegd hoe Azure Security Center voor IoT gebruikmaakt van apparaatdubbels en modules van het apparaat. 

## <a name="device-twins"></a>Apparaat apparaatdubbels

Voor IoT-oplossingen die zijn ingebouwd in azure, speelt apparaatdubbels een belang rijke rol in Apparaatbeheer en proces automatisering.  

Azure Security Center voor IoT biedt volledige integratie met uw bestaande IoT Device Management-platform, zodat u de beveiligings status van uw apparaat kunt beheren en ook de mogelijkheden van bestaande apparaten beheert. Integratie wordt bereikt door gebruik te maken van de IoT Hub dubbele methode.  

Meer informatie over het concept van [device apparaatdubbels](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins) in azure IOT hub. 

## <a name="security-module-twins"></a>Beveiligings module apparaatdubbels

Azure Security Center voor IoT onderhoudt een beveiligings module voor elk apparaat in de service.
De beveiligings module bevat alle informatie die relevant is voor de beveiliging van apparaten voor elk specifiek apparaat in uw oplossing.
Beveiligings eigenschappen van apparaten worden onderhouden in een speciale beveiligings module, met als gevolg dat er een veiliger communicatie is en voor het inschakelen van updates en onderhoud waarvoor minder resources nodig zijn.  

Zie [Security-module](quickstart-create-security-twin.md) configureren, en [Configureer beveiligings agenten](how-to-agent-configuration.md) voor meer informatie over het maken, aanpassen en configureren van de dubbele. Zie [Wat is module apparaatdubbels](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-module-twins) ? voor meer informatie over het concept van module apparaatdubbels in IOT hub. 
 

## <a name="see-also"></a>Zie ook
- [Overzicht van Azure Security Center voor IoT](overview.md)
- [Beveiligings agenten implementeren](how-to-deploy-agent.md)
- [Verificatie methoden voor beveiligings agenten](concept-security-agent-authentication-methods.md)
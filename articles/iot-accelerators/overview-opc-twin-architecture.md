---
title: Architectuur van de OPC-Twin - Azure | Microsoft Docs
description: OPC-Twin-architectuur
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 002e2e06201688638df98e16c45282187f593a3a
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444293"
---
# <a name="opc-twin-architecture"></a>OPC-Twin-architectuur

De volgende diagrammen ziet u de OPC-Twin-architectuur.

## <a name="discover-and-activate"></a>Detecteren en te activeren

1. De operator Hiermee netwerk scannen op de module of maakt een eenmalige detectie met behulp van een detectie-URL. De gedetecteerde eindpunten en informatie over toepassingen worden verzonden via telemetrie naar de onboarding-agent voor verwerking.  De OPC UA-apparaat onboarding-agent verwerkt OPC UA-server discovery-gebeurtenissen die door de OPC dubbele IoT Edge-module in de modus detectie of scan worden verzonden. Het resultaat van de gebeurtenissen detectie in de registratie van toepassingen en updates in het register van OPC UA-apparaat.

   ![De werking van de OPC-Twin](media/overview-opc-twin-architecture/opc-twin1.png)

1. De operator het certificaat van het eindpunt van de gedetecteerde inspecteert en activeert u het dubbele geregistreerde eindpunt voor toegang. 

   ![De werking van de OPC-Twin](media/overview-opc-twin-architecture/opc-twin2.png)

## <a name="browse-and-monitor"></a>Bladeren door en monitor

1. Wanneer geactiveerd, kunt de operator de dubbele service REST-API gebruiken om te bladeren of het servermodel informatie controleren, objectvariabelen voor lezen/schrijven en aanroepen van methoden.  De gebruiker maakt gebruik van een vereenvoudigde OPC UA-API volledig, uitgedrukt in HTTP en JSON.

   ![De werking van de OPC-Twin](media/overview-opc-twin-architecture/opc-twin3.png)

1. De REST-interface van dubbele service kan ook worden gebruikt om te maken van bewaakte items en -abonnementen in de OPC-uitgever. De OPC-uitgever kunt telemetrie naar IoT Hub worden verzonden vanuit OPC UA-serversystemen. Zie voor meer informatie over OPC Publisher [wat OPC Publisher is](overview-opc-publisher.md).

   ![De werking van de OPC-Twin](media/overview-opc-twin-architecture/opc-twin4.png)

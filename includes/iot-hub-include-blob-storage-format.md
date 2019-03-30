---
title: bestand opnemen
description: bestand opnemen
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/15/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: c779147e464a592d45da8a9a2d8e812320dc23e8
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58630938"
---
<!-- This is the note explaining about the avro and json formats when routing to blob storage. -->
> [!NOTE]
> De gegevens kunnen worden geschreven naar de blob storage in ofwel de [Apache Avro](https://avro.apache.org/) -indeling, dit de standaard- of JSON (preview is). 
>    
> De mogelijkheid voor het coderen van JSON-indeling is in preview in alle regio's in deze IoT Hub beschikbaar is, met uitzondering van VS-Oost, VS-West en West-Europa. De coderingsindeling kan alleen worden ingesteld op het moment dat het eindpunt van blob storage is geconfigureerd. De indeling kan niet worden gewijzigd voor een eindpunt dat al is ingesteld. Wanneer u de JSON-codering, moet u de contentType instellen in JSON en beperking de contentEncoding naar UTF-8 in de eigenschappen van het bericht. 
>
> Voor meer informatie gedetailleerde over het gebruik van een eindpunt van blob storage, Raadpleeg [richtlijnen voor de routering naar blob-opslag](../articles/iot-hub/iot-hub-devguide-messages-d2c.md#azure-blob-storage).
>
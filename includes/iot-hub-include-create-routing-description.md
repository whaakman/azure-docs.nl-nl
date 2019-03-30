---
title: bestand opnemen
description: bestand opnemen
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/05/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: b640a3cb9382ad72bb48e06c6a7074c96409e2e4
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58630883"
---
<!-- description of message routing used in the Azure CLI, PowerShell, and RM routing articles.-->

U gaat berichten naar andere resources doorsturen op basis van eigenschappen die door het gesimuleerde apparaat aan het bericht zijn gekoppeld. Berichten die niet aangepast zijn doorgestuurd, worden verzonden naar het standaardeindpunt (berichten/gebeurtenissen). In de volgende zelfstudie berichten verzenden naar de IoT-Hub en deze doorgestuurd naar de andere bestemmingen te zien.

|waarde |Resultaat|
|------|------|
|niveau = 'opslag' |Schrijf naar Azure Storage.|
|niveau = 'kritiek' |Schrijf naar een Service Bus-wachtrij. Een logische app haalt het bericht op uit de wachtrij en gebruikt Office 365 om het bericht via e-mail te versturen.|
|standaardinstelling |Geef deze gegevens weer met Power BI.|

De eerste stap is het instellen van het eindpunt waarop de gegevens worden gerouteerd. De tweede stap is het instellen van de bericht-route die gebruikmaakt van dit eindpunt. Na het instellen van de routering, kunt u de eindpunten en het berichtroutes weergeven in de portal.
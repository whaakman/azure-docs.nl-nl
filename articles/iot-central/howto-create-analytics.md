---
title: Maken van aangepaste analytics voor uw Azure IoT centrale toepassing | Microsoft Docs
description: Als operator aangepaste analytics voor uw Azure IoT centrale toepassing maken.
services: iot-central
author: tanmaybhagwat
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 2f19998429daab9000e35b520673c417b4bc828a
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="how-to-use-analytics-to-analyze-your-device-data"></a>Analytics gebruiken om de apparaatgegevens van uw te analyseren

Microsoft Azure IoT Central biedt uitgebreide analytics mogelijkheden voor het zinvol zijn van grote hoeveelheden gegevens van uw apparaten. U kunt analytics weergeven en analyseren van gegevens voor een [apparaatset](howto-use-device-sets.md) in uw toepassing. Een apparaatset is een door de gebruiker gedefinieerde groepslid van uw apparaten. U kunt uw analyse op een klein aantal apparaten of op één apparaat afbakenen.

Als operator kiezen **Analytics** in het navigatiemenu links, kiest u een apparaat en kies vervolgens de waarden op de grafiek moet worden weergegeven. Hier volgen enkele hulpprogramma's waarmee die u kunt de gegevens verder segmenteren:

* **Metingen:** kiest u de metingen weergeven zoals temperatuur en vochtigheid.
* **Aggregatie:** kiest u de statistische functie voor de metingen. Bijvoorbeeld: **Minimum** of **gemiddelde**.
* **Gesplitste door:** inzoomen door onder te verdelen van de gegevens door de eigenschappen van een apparaat of de naam van apparaat. Bijvoorbeeld, splitsing door de locatie van het apparaat:

     ![Hoofdpagina voor Analytics](media\howto-create-analytics\analytics-main.png)

* **Tijdsbereik:** kunt u een van de vooraf gedefinieerde tijdsbereik tijdsbereik kiezen of maken van een aangepaste periode: ![Analytics tijdsbereik](media\howto-create-analytics\analytics-time-range.png)

## <a name="change-the-visualizations"></a>De visualisaties wijzigen

U kunt wijzigen in de grafieken om te voldoen aan de vereisten van uw visualisatie door een van de drie beschikbare modi te kiezen:

* **Gestapelde:** een grafiek voor elke meting wordt gestapelde en elk van de grafieken hebben hun eigen y-as. Gestapelde grafieken zijn nuttig wanneer u meerdere metingen geselecteerd hebt en wilt u afzonderlijke weergave van deze metingen.
* **Niet gestapelde:** een grafiek voor elke meting wordt uitgezet tegen één y-as, maar de waarden voor de y-as worden gewijzigd op basis van de geselecteerde meting. Niet gestapelde grafieken zijn handig als u wilt meerdere metingen overlay en wilt zien van patronen in deze maatregelen voor dezelfde periode.
* **Gedeelde y-as:** alle grafieken delen de dezelfde y-as en de waarden voor de as niet wijzigen. Gedeelde grafieken van de y-as zijn handig als u zoeken op een enkele meting wilt bij het segmenteren van de gegevens met split-by.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u aangepaste analytics voor uw Azure IoT centrale toepassing maakt, is hier de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Bereid en verbinding maken met een Node.js-toepassing](howto-connect-nodejs.md)
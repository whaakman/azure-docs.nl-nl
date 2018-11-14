---
title: De Azure IoT Central apparaat brug bouwen | Microsoft Docs
description: Bouw de IoT Central apparaat bridge verbinding maken met andere IoT-clouds (Sigfox, Particle, de dingen netwerk enz.) aan uw app IoT Central.
services: iot-central
ms.service: iot-central
author: viv-liu
ms.author: viviali
ms.date: 11/8/2018
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: 83f053a8815f31803f536920497fdc42e72d2a2d
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51629013"
---
# <a name="build-the-iot-central-device-bridge-to-connect-other-iot-clouds-to-iot-central"></a>Bouw de IoT Central apparaat bridge andere clouds IoT verbinden met IoT Central

*In dit onderwerp is bedoeld voor beheerders.*

De apparaat-brug IoT Central is een open-source-oplossing die uw Sigfox, het onderdeel, de dingen netwerk en andere clouds met uw IoT Central-app verbindt. Of u gebruikmaakt van bijhouden van apparaten die zijn verbonden met de Sigfox laag-Power-Wide Area Network Assets of luchtkwaliteit het controleren van apparaten op het apparaat Particle Cloud, of met behulp van de grond vocht het controleren van apparaten op TTN gebruikt, u rechtstreeks kunt gebruikmaken van de kracht van IoT Centrale met behulp van de brug IoT Central-apparaat. De apparaat-brug verbindt andere IoT-clouds met IoT Central door het doorsturen van de gegevens die uw apparaten naar de andere clouds via aan uw app IoT Central verzenden. In uw app IoT Central, kunt u regels maken en analyses uitvoeren op die gegevens, werkstromen maken in Microsoft Flow en Azure Logic apps, die gegevens en nog veel meer exporteren. Krijgen de [IoT Central apparaat bridge](https://aka.ms/iotcentralgithubdevicebridge) vanuit Github

## <a name="what-is-it-and-how-does-it-work"></a>Wat is het en hoe werkt het?
De apparaat-brug IoT Central is een open-source-oplossing in Github. Het is klaar om te beginnen met een knop 'Implementeren naar Azure' die een aangepaste Azure Resource Manager-sjabloon met verschillende Azure-resources in uw Azure-abonnement implementeert. De resources zijn onder andere:
-   Azure Functions-app
-   Azure Storage-account
-   App Service-Plan (S1-laag)
-   Azure Key Vault de functie-app is het cruciaal onderdeel van de brug apparaat. Wordt een HTTP POST-aanvragen ontvangen van andere IoT-platformen of alle aangepaste platforms via een eenvoudige webhook-integratie. Er zijn voorbeelden die laten zien hoe u verbinding maken met Sigfox, Particle en TTN clouds beschikbaar. Deze oplossing voor het verbinding maken met uw aangepaste IoT-cloud als uw platform HTTP POST-aanvragen naar uw functie-app verzenden kunt kunt u gemakkelijk uitbreiden.
De functie-app de gegevens worden getransformeerd in een indeling die door IoT Central geaccepteerd en stuurt het langs via DPS-API's.

![Schermopname van Azure functions](media/howto-build-iotc-device-bridge/azfunctions.png)

Als uw IoT Central-app de ID van het apparaat door apparaat in het doorgestuurde bericht herkent, wordt een nieuwe meting wordt weergegeven voor dat apparaat. Als de apparaat-ID heeft nooit is zichtbaar voor uw IoT Central-app, uw functie-app wordt geprobeerd een nieuw apparaat registreren bij deze apparaat-ID en wordt deze weergegeven als een 'niet-gekoppelde apparaat' in uw IoT Central-app. 

## <a name="how-do-i-set-it-up"></a>Hoe kan ik deze instellen?
De instructies worden in de Leesmij-bestand in de Github-repo in detail weergegeven. 

## <a name="pricing"></a>Prijzen
Dit wordt gehost in uw Azure-abonnement. Het merendeel van de geschatte kosten van de ingerichte bronnen afkomstig is van de [prijs van een standaard App Service-Plan]( https://azure.microsoft.com/en-us/pricing/details/app-service/windows/). U kunt meer informatie over deze en mogelijke manieren om dit in het Leesmij-bestand te verminderen.

## <a name="next-steps"></a>Volgende stappen

Nu dat u over het bouwen van de IoT Central apparaat bridge hebt geleerd, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Uw apparaten beheren](howto-manage-devices.md)
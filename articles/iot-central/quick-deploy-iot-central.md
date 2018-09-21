---
title: Een Azure IoT Central-toepassing maken | Microsoft Docs
description: Maak een nieuwe Azure IoT Central-toepassing om gekoelde verkoopautomaten te beheren. Bekijk de telemetriegegevens die worden gegenereerd door uw gesimuleerde apparaten.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/15/2018
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: af06766d89804b2f3d0aaf061494fb836f6ec262
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46465602"
---
# <a name="create-an-azure-iot-central-application"></a>Een Azure IoT Central-toepassing maken

Als _maker_ gebruikt u de gebruikersinterface van Azure IoT Central om uw Microsoft Azure IoT Central-toepassing te definiëren. In deze snelstartgids leert u het volgende:

- Een Azure IoT Central-toepassing maken die een voorbeeld-_apparaatsjabloon_ en gesimuleerde _apparaten_ bevat.
- De kenmerken van de apparaatsjabloon **Gekoelde verkoopautomaat** bekijken in uw toepassing.
- De telemetrie en analyse van uw gesimuleerde **Koelapparaten** bekijken.

In deze snelstart bekijkt u een gesimuleerd **Koelapparaat** van een apparaatsjabloon. Het gesimuleerde apparaat:

* Verzendt telemetrie, zoals temperatuur en druk, naar uw toepassing.
* Geeft apparaateigenschapswaarden, zoals een bewegingswaarschuwing, door aan uw toepassing.
* Heeft apparaatinstellingen, zoals ventilatorsnelheid, die u kunt instellen in de toepassing.

Wanneer u een gesimuleerd apparaat maakt op basis van een apparaatsjabloon in een Azure IoT Central-toepassing, kunt u met het gesimuleerde apparaat uw toepassing testen voordat u een echt apparaat aansluit.

## <a name="create-the-application"></a>De toepassing maken

Voor het voltooien van deze snelstart moet u een Azure IoT Central toepassing maken op basis van de toepassingssjabloon **Sample Contoso**.

Navigeer naar de pagina [​​Toepassingsbeheer ](https://aka.ms/iotcentral) van Azure IoT Central. Voer vervolgens het e-mailadres en wachtwoord in dat u gebruikt om toegang te krijgen tot uw Azure-abonnement:

![Voer uw organisatieaccount in](media/quick-deploy-iot-central/sign-in.png)

Als u wilt beginnen met het maken van een nieuwe Azure IoT Central-toepassing, kiest u **Nieuwe toepassing** :

![Pagina Toepassingsbeheer van Azure IoT Central](media/quick-deploy-iot-central/iotcentralhome.png)

Een nieuwe Azure IoT Central-toepassing maken:

1. Kies het betalingsplan **Gratis proeftoepassing**.
1. Kies een beschrijvende toepassingsnaam, zoals **Contoso IoT**. Azure IoT Central genereert een uniek URL-voorvoegsel voor u. U kunt dit URL-voorvoegsel wijzigen in iets dat gemakkelijker te onthouden is.
1. Kies de toepassingssjabloon **Sample Contoso**.
1. Kies **Maken**.

![Pagina Toepassing maken van Azure IoT Central](media/quick-deploy-iot-central/iotcentralcreate.png)

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een vooraf ingevulde Azure IoT Central-toepassing gemaakt die een apparaatsjabloon voor een **Gekoelde Verkoopautomaat**  en gesimuleerde apparaten bevat. Zie [Een nieuwe apparaatsjabloon in uw toepassing definiëren](tutorial-define-device-type.md) voor meer informatie over het definiëren van uw eigen apparaatsjablonen als bouwer.

---
title: Overzicht van de Azure IoT Hub X.509-CA-beveiliging | Microsoft Docs
description: Overzicht - hoe u verificatie van apparaten met IoT Hub met X.509-CA's.
author: eustacea
manager: arjmands
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/18/2017
ms.author: eustacea
ms.openlocfilehash: b5028010953b9dbe2386c30e6fa05cc4a94cb971
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185489"
---
# <a name="device-authentication-using-x509-ca-certificates"></a>Apparaatverificatie met behulp van x.509-CA-certificaten

Dit artikel wordt beschreven hoe u het gebruik van x.509-certificeringsinstantie (CA)-certificaten voor verificatie van apparaten die verbinding maken van IoT Hub.  In dit artikel leert u het volgende:

* Over het verkrijgen van een X.509-CA-certificaat
* Informatie over het registreren van het x.509-CA-certificaat naar IoT Hub
* Het ondertekenen van apparaten met behulp van X.509-CA-certificaten
* Hoe de apparaten die zijn ondertekend met een X.509-CA worden geverifieerd

## <a name="overview"></a>Overzicht

De X.509-CA-functie kunt verificatie van apparaten naar IoT Hub met behulp van een certificeringsinstantie (CA). Aanzienlijk eenvoudiger het registratieproces van eerste apparaat en supply chain logistiek tijdens de productie van apparaat. [Meer informatie in dit scenarioartikel over de waarde van het gebruik van x.509-CA-certificaten](iot-hub-x509ca-concept.md) voor verificatie van apparaten.  We raden u aan dit scenarioartikel voordat u doorgaat lezen omdat hierin wordt uitgelegd waarom de volgende stappen bestaat.

## <a name="prerequisite"></a>Vereiste

Met de functie voor X.509-CA, moet u een IoT Hub-account hebben.  [Informatie over het maken van een IoT Hub-instantie](quickstart-send-telemetry-dotnet.md) als u er nog geen hebt.

## <a name="how-to-get-an-x509-ca-certificate"></a>Over het verkrijgen van een X.509-CA-certificaat

Het X.509-CA-certificaat is aan de bovenkant van de keten van certificaten voor elk van uw apparaten.  U kunt aanschaffen of maakt u er een afhankelijk van hoe u van plan bent om deze te gebruiken.

Voor productie-omgeving, wordt u aangeraden dat u een X.509-CA-certificaat uit een openbare basiscertificeringsinstantie hebt gekocht. Het aanschaffen van een CA-certificaat heeft het voordeel van de basis-CA fungeert als een vertrouwde derde partij garant staat voor de geldigheid van uw apparaten. Houd rekening met deze optie als u van plan uw apparaten bent moet deel uitmaken van een open IoT-netwerk waarin ze worden verwacht om te communiceren met producten van derden of services.

U kunt ook een zelfondertekend X.509-CA voor experimenten of voor gebruik in gesloten IoT-netwerken maken.

Ongeacht hoe u uw X.509-CA-certificaat verkrijgen, zorg ervoor dat u de bijbehorende persoonlijke sleutel geheim te houden en beveiligd te allen tijde.  Dit is nodig voor de vertrouwensrelatie van de opbouw van vertrouwen in de X.509-CA-verificatie. 

Meer informatie over het [maken van een zelf-ondertekende CA-certificaat](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md), die u kunt gebruiken om te experimenten in de beschrijving van deze functie.

## <a name="sign-devices-into-the-certificate-chain-of-trust"></a>Meld u apparaten in de certificaatketen van vertrouwensrelatie

De eigenaar van een X.509-CA-certificaat kunt cryptografisch een tussenliggende CA die een andere tussenliggende CA op zijn beurt kan zich aanmelden en enzovoort, totdat de laatste tussenliggende CA wordt beëindigd dit proces door een apparaat te ondertekenen. Het resultaat is een trapsgewijze keten van certificaten bekend als een certificaatketen van de vertrouwensrelatie. In de praktijk speelt dit uit als de overdracht van de vertrouwensrelatie voor de ondertekening van apparaten. Deze overdracht is belangrijk omdat het een cryptografisch variabele bewakingsketen vaststelt en delen van ondertekeningssleutels voorkomt.

![IMG-Generic-CERT-Chain-of-Trust](./media/generic-cert-chain-of-trust.png)

Lees hier hoe u [maken van een certificaatketen](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) zoals bij het aanmelden van apparaten wordt uitgevoerd.

## <a name="how-to-register-the-x509-ca-certificate-to-iot-hub"></a>Informatie over het registreren van het x.509-CA-certificaat naar IoT Hub

Registreer uw X.509-CA-certificaat naar IoT Hub waar deze worden gebruikt voor verificatie van uw apparaten tijdens de registratie en verbinding.  Registratie van het X.509-CA-certificaat is een proces dat bestaat uit de certificaat-bestand uploaden en bewijs van eigendom.

Het uploadproces houdt in dat u uploadt een bestand met uw certificaat.  Dit bestand moet nooit bevat geen persoonlijke sleutels.

Het bewijs van bezit stap bestaat uit een cryptografische vraag en antwoord-proces tussen u en uw IoT-Hub.  Gezien het feit dat de inhoud van digitale certificaten zijn openbaar en daarom gevoelig is voor het niet kan worden afgeluisterd, graag IoT Hub om na te gaan dat u echt eigenaar van het CA-certificaat.  Het moet dit doen door het genereren van een willekeurige uitdaging die u zich met de bijbehorende persoonlijke sleutel van het CA-certificaat aanmelden moet.  Als de privésleutel geheim en beveiligde net als eerst op de hoogte, en vervolgens alleen over de kennis voor het voltooien van deze stap beschikken. Geheimhouding van persoonlijke sleutels is de bron van vertrouwen in deze methode.  Na de ondertekening van de uitdaging voltooien van deze stap door een bestand met de resultaten te uploaden.

Lees hier hoe u [registreren van uw CA-certificaat](iot-hub-security-x509-get-started.md#registercerts).

## <a name="how-to-create-a-device-on-iot-hub"></a>Over het maken van een apparaat in IoT Hub

Als u wilt dat apparaat imitatie, moet IoT Hub u aan zodat deze informatie over welke apparaten die u kunt verwachten.  U doen dit door het maken van een vermelding in de IoT-Hub-apparaatregister.  Dit proces is geautomatiseerd bij het gebruik van IoT-Hub [Device Provisioning Service](https://azure.microsoft.com/blog/azure-iot-hub-device-provisioning-service-preview-automates-device-connection-configuration/) (DPS). 

Lees hier hoe u [handmatig maken van een apparaat in IoT Hub](iot-hub-security-x509-get-started.md#createdevice).

## <a name="authenticating-devices-signed-with-x509-ca-certificates"></a>Verificatie van apparaten die zijn ondertekend met x.509-CA-certificaten

Met X.509-CA-certificaat is geregistreerd en apparaten die zijn aangemeld bij een certificaatvertrouwensketen, wat resteert, is verificatie van apparaten wanneer het apparaat verbinding, zelfs voor de eerste keer maakt.  Wanneer een X.509-CA ondertekend apparaat verbinding maakt, heeft de certificaatketen voor de validatie uploadt. De keten bevat alle tussenliggende CA- en apparaatcertificaten.  Met deze informatie verifieert IoT-Hub het apparaat in een proces in twee stappen.  IoT Hub cryptografisch valideert de certificaatketen voor de interne consistentie en vervolgens een bewijs van eigendom uitdaging op het apparaat.  IoT Hub declareert het apparaat authentiek op een geslaagde respons bewijs van eigendom van het apparaat.  Deze verklaring wordt ervan uitgegaan dat de persoonlijke sleutel van het apparaat wordt beveiligd en dat alleen het apparaat is voor dit probleem kan reageren.  U wordt aangeraden gebruik van beveiligde chips, zoals Hardware beveiligde Modules (HSM) in apparaten ter bescherming van persoonlijke sleutels.

Een apparaatverbinding met IoT Hub het verificatieproces is voltooid en is ook voor een juiste installatie.

Lees hier hoe u [voltooien van deze stap van de verbinding apparaat](iot-hub-security-x509-get-started.md#authenticatedevice).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [de waarde van X.509-CA-verificatie](iot-hub-x509ca-concept.md) in IoT.

Aan de slag met IoT Hub [Device Provisioning Service](https://docs.microsoft.com/azure/iot-dps/).

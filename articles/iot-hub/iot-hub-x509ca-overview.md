---
title: Overzicht van Azure IoT Hub x.509-CA-beveiliging | Microsoft Docs
description: "Overzicht - hoe om apparaten met IoT Hub met X.509-certificeringsinstanties te verifiëren."
services: iot-hub
documentationcenter: .net
author: eustacea
manager: arjmands
editor: 
ms.assetid: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2017
ms.author: eustacea
ms.openlocfilehash: 7497753cc27867b1fdb9635ba6613ac75b449090
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="device-authentication-using-x509-ca-certificates"></a>Verificatie van apparaten met behulp van x.509-CA-certificaten

In dit artikel wordt beschreven hoe x.509-certificeringsinstantie (CA) om certificaten te gebruiken voor verificatie van apparaten die verbinding maken met IoT Hub.  In dit artikel leert u het:

* Het ophalen van een x.509-CA-certificaat
* Het registreren van het x.509-CA-certificaat naar IoT Hub
* Het ondertekenen van apparaten met behulp van de CA X.509-certificaten
* Hoe de apparaten die zijn ondertekend met een x.509-CA worden geverifieerd

## <a name="overview"></a>Overzicht

Het x.509-CA-functie kunt verificatie van apparaten met IoT Hub met behulp van een certificeringsinstantie (CA). Sterk vereenvoudigd het inschrijvingsproces van eerste apparaat en geef keten logistiek tijdens de productie van apparaat. [Meer informatie in dit scenario artikel over de waarde van het gebruik van x.509-CA-certificaten](iot-hub-x509ca-concept.md) voor verificatie van apparaten.  We raden u aan dit artikel scenario voordat u doorgaat lezen, zoals wordt uitgelegd waarom de stappen volgen bestaan.

## <a name="prerequisite"></a>Vereiste

Met de x.509-CA-functie is vereist dat u een account met IoT Hub hebt.  [Informatie over het maken van een IoT Hub-instantie](iot-hub-csharp-csharp-getstarted.md) als u er nog geen hebt.

## <a name="how-to-get-an-x509-ca-certificate"></a>Het ophalen van een x.509-CA-certificaat

Het x.509-CA-certificaat is aan de bovenkant van de keten van certificaten voor elk van uw apparaten.  U kunt kopen of maken van een afhankelijk van hoe u van plan bent om deze te gebruiken.

U wordt aangeraden dat u een x.509-CA-certificaat aanschaffen bij een openbare basiscertificeringsinstantie voor productie-omgeving. Het aanschaffen van een CA-certificaat heeft het voordeel van de basis-CA fungeert als een vertrouwde derde partij aan garant staat voor de geldigheid van uw apparaten. Houd rekening met deze optie als u van plan bent uw apparaten deel uitmaken van een open IoT netwerk waar ze worden geacht om te communiceren met producten van derden of services.

U kunt ook een zelf-ondertekend X.509-CA voor experimenteren of voor gebruik in gesloten IoT-netwerken maken.

Ongeacht hoe u uw CA X.509-certificaat verkrijgen, zorg ervoor dat de bijbehorende persoonlijke sleutel geheim te houden en beveiligd te allen tijde.  Dit is nodig voor de vertrouwensrelatie van de opbouw van vertrouwen in de x.509-CA-verificatie. 

Meer informatie over hoe [een zelf-ondertekende CA-certificaat maken](iot-hub-security-x509-create-certificates.md#createcerts), die u kunt gebruiken om te experimenten in de beschrijving van deze functie.

## <a name="sign-devices-into-the-certificate-chain-of-trust"></a>Meld u apparaten in de certificaatketen van vertrouwensrelatie

De eigenaar van een x.509-CA-certificaat cryptografisch Meld u aan een tussenliggende CA die op zijn beurt zich op een andere tussenliggende CA kan aanmelden en enzovoort, totdat de laatste tussenliggende CA beëindigt dit proces door een apparaat te ondertekenen. Het resultaat is een trapsgewijze keten van certificaten die een certificaatvertrouwensketen genoemd. In de praktijk speelt dit uit als de overdracht van de vertrouwensrelatie voor de ondertekening van apparaten. Deze overdracht is belangrijk, omdat deze een cryptografisch variabele keten bewakingsketen vaststelt en delen van ondertekeningssleutels voorkomt.

![IMG-Generic-CERT-Chain-of-Trust](./media/generic-cert-chain-of-trust.png)

Hier vindt u informatie hoe [maken van een certificaatketen](iot-hub-security-x509-create-certificates.md#createcertchain) zoals bij het ondertekenen van apparaten wordt uitgevoerd.

## <a name="how-to-register-the-x509-ca-certificate-to-iot-hub"></a>Het registreren van het x.509-CA-certificaat naar IoT Hub

Registreer uw x.509-CA-certificaat naar IoT Hub waarop zal worden gebruikt voor verificatie van uw apparaten tijdens de registratie en verbinding.  Registreren van het x.509-CA-certificaat is een proces dat bestaat uit het uploaden van het certificaat en bewijs van eigendom.

Het uploaden van houdt een bestand met uw certificaat uploaden.  Dit bestand mag nooit persoonlijke sleutels bevatten.

Het bewijs van bezit stap omvat een cryptografische vraag en antwoord-proces tussen u en IoT Hub.  Gezien het feit dat de inhoud van digitale certificaten zijn openbaar en daarom vatbaar voor inbreuk, wilt IoT-Hub gaan dat u echt eigenaar van het CA-certificaat.  Hiervoor wordt het genereren van een willekeurige uitdaging die u zich met de bijbehorende persoonlijke sleutel van het CA-certificaat aanmelden moet.  Als u de persoonlijke sleutel geheime en beveiligde als eerder blijven aangeraden, en vervolgens alleen over de kennis voor het voltooien van deze stap beschikken. Secrecy van persoonlijke sleutels is de bron van vertrouwen in deze methode.  Na de ondertekening van de uitdaging waar deze stap hebt voltooid door het uploaden van een bestand met de resultaten.

Hier vindt u informatie hoe [uw CA-certificaat registreren](iot-hub-security-x509-get-started.md#registercerts).

## <a name="how-to-create-a-device-on-iot-hub"></a>Het maken van een apparaat op IoT-Hub

Voor dat apparaat imitatie, moet IoT Hub u laten weten welke apparaten ze kunnen verwachten.  U doen dit door het maken van een apparaatvermelding in het register van de IoT-Hub-apparaat.  Dit proces wordt automatisch uitgevoerd wanneer het gebruik van IoT Hub [apparaat inrichtingsservice](https://azure.microsoft.com/en-us/blog/azure-iot-hub-device-provisioning-service-preview-automates-device-connection-configuration/) (DP's). 

Hier vindt u informatie hoe [handmatig maken van een apparaat in IoT Hub](iot-hub-security-x509-get-started.md#createdevice).

## <a name="authenticating-devices-signed-with-x509-ca-certificates"></a>Verificatie van apparaten die zijn ondertekend met x.509-CA-certificaten

Met CA x.509-certificaat dat is geregistreerd en apparaten die zijn aangemeld bij een certificaatvertrouwensketen, is wat er nog apparaatauthenticatie wanneer het apparaat verbinding, zelfs voor de eerste keer maakt.  Wanneer een X.509-CA ondertekend apparaat verbinding maakt, uploadt de certificaatketen voor validatie. De keten omvat alle tussenliggende CA- en apparaatcertificaten.  Met deze informatie kunt verifieert IoT Hub het apparaat in een proces in twee stappen.  IoT Hub cryptografisch valideert de certificaatketen voor interne consistentiefout en vervolgens een bewijs van bezit uitdaging op het apparaat.  IoT Hub declareert het apparaat authentiek op een geslaagde reactie bewijs van bezit van het apparaat.  Deze verklaring wordt ervan uitgegaan dat de persoonlijke sleutel van het apparaat wordt beveiligd en dat alleen het apparaat is voor deze uitdaging kan reageren.  U wordt aangeraden gebruik van beveiligde chips zoals Hardware beveiligde Modules (HSM) in inrichtingen ter bescherming van persoonlijke sleutels.

Een geslaagde apparaatverbinding met IoT Hub het verificatieproces afrondt, en is ook indicatief voor een juiste instelling.

Hier vindt u informatie hoe [voltooien van deze stap van de verbinding apparaat](iot-hub-security-x509-get-started.md#authenticatedevice).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [de waarde van het x.509-CA verificatie](iot-hub-x509ca-concept.md) in IoT.

Aan de slag met IoT Hub [apparaat inrichtingsservice](https://docs.microsoft.com/azure/iot-dps/).
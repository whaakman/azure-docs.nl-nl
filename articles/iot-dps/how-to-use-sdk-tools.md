---
title: Hulpprogramma's in de Azure IoT Hub apparaat inrichten Service SDK's gebruiken voor het vereenvoudigen van ontwikkeling
description: Dit document worden de hulpprogramma's in Azure IoT Hub apparaat inrichten Service SDK's voor ontwikkeling
services: iot-dps
keywords: 
author: yzhong94
ms.author: yizhon
ms.date: 01/18/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 76c6f64dea202f661691fafaa78a6d77b4a40f14
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="how-to-use-tools-provided-in-the-sdks-to-simplify-development-for-provisioning"></a>Het gebruik van hulpprogramma's in de SDK's voor het vereenvoudigen van ontwikkeling voor inrichting
De IoT Hub apparaat inrichtingsservice vereenvoudigt het inrichtingsproces bij zonder tussenkomst, just in time inrichting op een veilige en schaalbare manier.  Beveiliging attestation in de vorm van het X.509-certificaat of Trusted Platform Module (TPM) is vereist.  Microsoft is ook samenwerking met [andere hardwarepartners beveiliging](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) te vertrouwen bij het beveiligen van IoT-implementatie te verbeteren. Informatie over de hardwarevereisten voor de beveiliging kan erg lastig voor ontwikkelaars zijn gemaakt. Een set van Azure IoT inrichting Service-SDK's zijn bedoeld om ontwikkelaars kunnen gebruikmaken van een laag gemak voor schrijven-clients die met de inrichting service communiceren. De SDK's bieden ook voorbeelden voor algemene scenario's, evenals een aantal hulpprogramma's voor het vereenvoudigen van beveiliging attestation in ontwikkeling.

## <a name="trusted-platform-module-tpm-simulator"></a>Vertrouwde Platform Module (TPM) simulator
[TPM](https://docs.microsoft.com/azure/iot-dps/concepts-security#trusted-platform-module-tpm) kunnen verwijzen naar een standaard voor het veilig opslaan van sleutels voor de authenticatie van het platform of het kan verwijzen naar de i/o-interface gebruikt om te communiceren met de implementatie van de standaard-modules. TPM's kunnen bestaan als discrete hardware, geïntegreerde hardware, firmware gebaseerde of op basis van software.  In productie, TPM bevindt zich op het apparaat als discrete hardware, geïntegreerde hardware of firmware is gebaseerd. In de testfase een TPM op basis van software simulator biedt voor ontwikkelaars.  Deze simulator is alleen beschikbaar voor het ontwikkelen van op Windows-platform voor nu.

Stappen voor het gebruik van de TPM-simulator zijn:
1. [De ontwikkelomgeving voorbereiden](https://docs.microsoft.com/azure/iot-dps/quick-enroll-device-x509-java#prepare-the-development-environment) en de GitHub-opslagplaats klonen:
```
git clone https://github.com/Azure/azure-iot-sdk-java.git
```
2. Navigeer naar de map van de simulator TPM onder ```azure-iot-sdk-java/provisioning/provisioning-tool/tpm-simulator/```.
3. Simulator.exe voorafgaand aan elke clienttoepassing voor inrichting apparaat worden uitgevoerd.
4. Laat de simulator uitvoeren op de achtergrond tijdens het inrichtingsproces om registratie-ID en goedkeuringssleutel te verkrijgen.  Beide waarden zijn alleen geldig voor één exemplaar van de uitvoering.

## <a name="x509-certificate-generator"></a>X.509-certificaat generator
[X.509-certificaten](https://docs.microsoft.com/azure/iot-dps/concepts-security#x509-certificates) kan worden gebruikt als een mechanisme voor attestation productie schalen en eenvoudig mobiele apparaten inrichten.  Er zijn [op verschillende manieren](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview#how-to-get-an-x509-ca-certificate) verkrijgen van een X.509-certificaat:
* Voor de productie-omgeving, wordt u aangeraden het aanschaffen van een x.509-CA-certificaat van een openbare basiscertificeringsinstantie.
* U kunt een X.509-basiscertificaat of het gebruik van x.509-certificaatketen genereren voor het testen van de omgeving:
    * OpenSSL: dit [stapsgewijze handleiding voor](https://docs.microsoft.com/azure/iot-hub/iot-hub-security-x509-create-certificates) doorloopt voorbeeld PowerShell-scripts die gebruikmaken van [OpenSSL](https://www.openssl.org/) maken en meld u aan de X.509-certificaten.  U kunt bovendien ook script gebruiken in andere talen voor het genereren van certificaat:
        * [Node.js](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/tools)
        * [PowerShell](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md)
        
    * Emulator van Device-Identity samenstelling Engine (OPDELEN): OPDELEN kan worden gebruikt voor cryptografische apparaat-id en attestation op basis van TLS-protocol en x.509-certificaten.  [Meer informatie over](https://www.microsoft.com/research/publication/device-identity-dice-riot-keys-certificates/) meer informatie over apparaat-id met OPDELEN.

### <a name="using-x509-certificate-generator-with-dice-emulator"></a>Gebruik van x.509-certificaat generator met OPDELEN emulator
De SDK's bieden de generator van een x.509-certificaat met de emulator OPDELEN, zich in de [Java SDK](https://github.com/Azure/azure-iot-sdk-java/tree/master/provisioning/provisioning-tools/provisioning-x509-cert-generator).  Deze generator werkt cross-platform.  Het gegenereerde certificaat kan worden gebruikt voor ontwikkeling in andere talen.

Op dit moment tijdens de Emulator OPDELEN levert een basiscertificaat, een tussenliggende certificaat, een certificaat en de bijbehorende persoonlijke sleutel.  Echter, het basiscertificaat of tussenliggende certificaat kan niet worden gebruikt voor het ondertekenen van een afzonderlijke leaf-certificaat.  Als u van plan bent om te testen inschrijvingsscenario groep waar een handtekeningcertificaat wordt gebruikt voor het ondertekenen van de leaf-certificaten van meerdere apparaten, kunt u OpenSSL gebruiken voor het produceren van een keten van certificaten.

Genereren met behulp van deze generator X.509-certificaat:
1. [De ontwikkelomgeving voorbereiden](https://docs.microsoft.com/azure/iot-dps/quick-enroll-device-x509-java#prepare-the-development-environment) en de GitHub-opslagplaats klonen:
```
git clone https://github.com/Azure/azure-iot-sdk-java.git
```
2. Wijzig de hoofdmap in azure-iot-sdk-java.
3. Voer ```mvn install -DskipTests=true``` voor het downloaden van alle vereiste pakketten en compileer de SDK
4. Ga naar de hoofdmap voor x.509-certificaat Generator in ```azure-iot-sdk-java/provisioning/provisioning-tools/provisioning-x509-cert-generator```.
5. Bij het maken van```mvn clean install```
6. Voer het hulpprogramma uit met behulp van de volgende opdrachten:
```
cd target
java -jar ./provisioning-x509-cert-generator-{version}-with-deps.jar
```
7. Wanneer u hierom wordt gevraagd, kunt u ook een _algemene naam_ invoeren voor de certificaten.
8. Het hulpprogramma lokaal genereert een **Client Cert**, wordt de **Client Cert persoonlijke sleutel**, **tussenliggende Cert**, en de **hoofdmap Cert**.

**Client Cert** is het leaf-certificaat op het apparaat.  **Client Cert** en de bijbehorende **Client Cert persoonlijke sleutel** in apparaatclient-nodig zijn. Afhankelijk van welke taal die u kiest, kan het mechanisme om dit voor de clienttoepassing afwijken.  Zie voor meer informatie de [snelstartgidsen](https://docs.microsoft.com/azure/iot-dps/quick-create-simulated-device-x509) bij het maken van gesimuleerd apparaat met X.509 voor meer informatie.

Het basiscertificaat of tussenliggende kan worden gebruikt voor het maken van een inschrijvingsgroep of de afzonderlijke inschrijving [programmatisch](https://docs.microsoft.com/azure/iot-dps/how-to-manage-enrollments-sdks) of met behulp van de [portal](https://docs.microsoft.com/azure/iot-dps/how-to-manage-enrollments).

## <a name="next-steps"></a>Volgende stappen
* Ontwikkelen met behulp van de [Azure IoT SDK]( https://github.com/Azure/azure-iot-sdks) voor Azure IoT Hub en Azure IoT Hub apparaat inrichten van Service
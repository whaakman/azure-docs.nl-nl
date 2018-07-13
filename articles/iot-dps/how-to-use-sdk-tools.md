---
title: Hulpprogramma's in de Azure IoT Hub Device Provisioning Service-SDK's gebruiken om ontwikkeling te vereenvoudigen
description: Dit document beoordeelt de hulpprogramma's in Azure IoT Hub Device Provisioning Service-SDK's voor ontwikkeling
author: yzhong94
ms.author: yizhon
ms.date: 04/09/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: arjmands
ms.openlocfilehash: 647f54d8252c594a280f81d661a3de6270bf692b
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39001344"
---
# <a name="how-to-use-tools-provided-in-the-sdks-to-simplify-development-for-provisioning"></a>Het gebruik van hulpprogramma's in de SDK's voor het vereenvoudigen van ontwikkeling voor het inrichten van
De IoT Hub Device Provisioning Service vereenvoudigt het proces van inrichting zero-touch, just-in-time [automatische inrichting](concepts-auto-provisioning.md) op een veilige en schaalbare manier.  Beveiliging attestation in de vorm van X.509-certificaat of Trusted Platform Module (TPM) is vereist.  Microsoft is ook samenwerking met [andere hardware security partners](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) te vertrouwen bij het beveiligen van IoT-implementatie te verbeteren. Informatie over de hardwarevereisten voor de beveiliging kan lastig zijn behoorlijk voor ontwikkelaars. Een set met Azure IoT Provisioning Service-SDK's zijn bedoeld om ontwikkelaars een laag gemak kunnen gebruiken voor het schrijven-clients die met de provisioning-service communiceren. De SDK's bieden ook voorbeelden voor algemene scenario's, evenals een set hulpprogramma's voor het vereenvoudigen van beveiliging attestation in ontwikkeling.

## <a name="trusted-platform-module-tpm-simulator"></a>Vertrouwd Platform Module (TPM) simulator
[TPM](https://docs.microsoft.com/azure/iot-dps/concepts-security#trusted-platform-module-tpm) kunnen verwijzen naar een standaard voor het veilig opslaan van sleutels voor verificatie van het platform of het kan verwijzen naar de i/o-interface gebruikt om te communiceren met de implementatie van de standaard-modules. TPM's kunnen bestaan als discrete hardware, geïntegreerde hardware, op basis van firmware of op basis van software.  In productie, TPM bevindt zich op het apparaat als discrete hardware, geïntegreerde hardware, of op basis van firmware. In de testfase krijgt u een door de software op basis van TPM-simulator voor ontwikkelaars.  Simulator is alleen beschikbaar voor het ontwikkelen van nu op Windows-platform.

Stappen voor het gebruik van de TPM-simulator zijn:
1. [De ontwikkelomgeving voorbereiden](https://docs.microsoft.com/azure/iot-dps/quick-enroll-device-x509-java#prepare-the-development-environment) en kloon de GitHub-opslagplaats:
```
git clone https://github.com/Azure/azure-iot-sdk-java.git
```
2. Navigeer naar de map voor de TPM-simulator onder ```azure-iot-sdk-java/provisioning/provisioning-tool/tpm-simulator/```.
3. Simulator.exe vóór het uitvoeren van elke clienttoepassing voor inrichting apparaat worden uitgevoerd.
4. Laat de simulator op de achtergrond actief tijdens het inrichtingsproces om registratie-ID en goedkeuringssleutel te verkrijgen.  Beide waarden zijn alleen geldig voor één exemplaar van de uitvoering.

## <a name="x509-certificate-generator"></a>X.509-certificaatgenerator
[X.509-certificaten](https://docs.microsoft.com/azure/iot-dps/concepts-security#x509-certificates) kan worden gebruikt als een attestation-mechanisme productie schalen en vereenvoudigen apparaten inrichten.  Er zijn [op verschillende manieren](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview#how-to-get-an-x509-ca-certificate) om een X.509-certificaat te verkrijgen:
* Voor productie-omgeving, wordt u aangeraden het aanschaffen van een X.509-CA-certificaat van een openbaar basiscertificaat van de certificeringsinstantie.
* Voor het testen van omgeving, kunt u een X.509-basiscertificaat X.509- of keten met genereren:
    * OpenSSL: U kunt scripts gebruiken voor het genereren van certificaat:
        * [Node.js](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/tools)
        * [PowerShell- of Bash](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md)
        
    * Device Identity Composition Engine (DICE) Emulator: DICE kan worden gebruikt voor cryptografische apparaat-id en attestation op basis van TLS-protocol en x.509-certificaten.  [Informatie over](https://www.microsoft.com/research/publication/device-identity-dice-riot-keys-certificates/) meer informatie over apparaat-id met OPDELEN.

### <a name="using-x509-certificate-generator-with-dice-emulator"></a>Met behulp van X.509-certificaatgenerator met DICE-emulator
De SDK's bieden een X.509-certificaatgenerator met DICE-emulator, zich in de [Java SDK](https://github.com/Azure/azure-iot-sdk-java/tree/master/provisioning/provisioning-tools/provisioning-x509-cert-generator).  Deze generator werkt cross-platform.  Het gegenereerde certificaat kan worden gebruikt voor ontwikkeling in andere talen.

Op dit moment tijdens de DICE-Emulator levert een basiscertificaat, een tussencertificaat, een leaf-certificaat en de bijbehorende persoonlijke sleutel.  Echter kan niet het basiscertificaat of tussenliggende certificaat worden gebruikt op een afzonderlijke leafcertificaat ondertekenen.  Als u van plan bent voor het testen van de inschrijving van het scenario waarbij een certificaat voor ondertekening wordt gebruikt voor het ondertekenen van de leaf-certificaten van meerdere apparaten, kunt u OpenSSL gebruiken voor het produceren van een keten van certificaten.

Voor het genereren van X.509-certificaat met behulp van deze generator:
1. [De ontwikkelomgeving voorbereiden](https://docs.microsoft.com/azure/iot-dps/quick-enroll-device-x509-java#prepare-the-development-environment) en kloon de GitHub-opslagplaats:
```
git clone https://github.com/Azure/azure-iot-sdk-java.git
```
2. Azure-iot-sdk-java wijzigen in de hoofdmap.
3. Voer ```mvn install -DskipTests=true``` aan alle vereiste pakketten te downloaden en compileer de SDK
4. Ga naar de hoofdmap voor X.509-Certificaatgenerator in ```azure-iot-sdk-java/provisioning/provisioning-tools/provisioning-x509-cert-generator```.
5. Bouwen met ```mvn clean install```
6. Voer het hulpprogramma uit met behulp van de volgende opdrachten:
```
cd target
java -jar ./provisioning-x509-cert-generator-{version}-with-deps.jar
```
7. Wanneer u hierom wordt gevraagd, kunt u ook een _algemene naam_ invoeren voor de certificaten.
8. Het hulpprogramma genereert lokaal een **clientcertificaat**, wordt de **persoonlijke sleutel van clientcertificaat**, **tussenliggende certificaat**, en de **basiscertificaat**.

**Clientcertificaat** is van het leaf-certificaat op het apparaat.  **Clientcertificaat** en de bijbehorende **persoonlijke sleutel van clientcertificaat** in apparaatclient nodig zijn. Afhankelijk van welke taal u kiest, kan het mechanisme om dit in de clienttoepassing afwijken.  Zie voor meer informatie de [snelstartgidsen](https://docs.microsoft.com/azure/iot-dps/quick-create-simulated-device-x509) bij het maken van gesimuleerde apparaat met behulp van X.509 voor meer informatie.

Het basiscertificaat of tussenliggende kan worden gebruikt om het maken van een registratiegroep zit of een afzonderlijke inschrijving [programmatisch](https://docs.microsoft.com/azure/iot-dps/how-to-manage-enrollments-sdks) of met behulp van de [portal](https://docs.microsoft.com/azure/iot-dps/how-to-manage-enrollments).

## <a name="next-steps"></a>Volgende stappen
* Ontwikkelen met behulp van de [Azure IoT SDK]( https://github.com/Azure/azure-iot-sdks) voor Azure IoT Hub en Azure IoT Hub Device Provisioning Service
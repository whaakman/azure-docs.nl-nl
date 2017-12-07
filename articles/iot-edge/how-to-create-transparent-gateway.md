---
title: Maken van een transparante gateway-apparaat met Azure IoT rand | Microsoft Docs
description: Azure IoT Edge gebruiken voor het maken van een transparante gateway-apparaat waarmee informatie voor meerdere apparaten kan worden verwerkt.
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 12/04/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 25f4cea1908a0f9bdf387ddfed5f29e6d19bdd20
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="create-an-iot-edge-device-that-acts-as-a-transparent-gateway---preview"></a>Maken van een IoT-randapparaat die als een transparante gateway fungeert-preview

In dit artikel vindt u gedetailleerde instructies voor het gebruik van een IoT-randapparaat als een transparante gateway. Voor de rest van dit artikel wordt de term *gateway aan de rand van de IoT* verwijst naar een IoT-randapparaat als een transparante gateway gebruikt. Voor meer informatie gedetailleerde, Zie [hoe een Edge van IoT-apparaat kan worden gebruikt als een gateway][lnk-edge-as-gateway], waardoor een conceptueel overzicht. 

>[!NOTE]
>Op dit moment:
> * Als de gateway niet met IoT Hub verbonden is, worden aangesloten apparaten niet verifiëren met de gateway.
> * IoT Edge-apparaten kunnen geen verbinding naar IoT-gateways.

## <a name="understand-the-azure-iot-device-sdk"></a>Het apparaat met Azure IoT SDK begrijpen


De Edge-hub die is geïnstalleerd in alle IoT randapparaten toont de volgende primitieven op downstream-apparaten:

* apparaat-naar-cloud- en cloud-naar-apparaat-berichten
* Rechtstreekse methoden
* bewerkingen voor apparaat-twin

Downstream apparaten zijn op dit moment niet uploaden bestand gebruiken om verbinding te maken via een gateway IoT rand.

Als u apparaten op een gateway aan de rand van de IoT aansluit-apparaat gebruikt de Azure IoT SDK, moet u naar:

* Instellen van de downstream-apparaat met een verbindingsreeks verwijst naar de hostnaam van de gateway-apparaat; en
* Zorg ervoor dat het certificaat te accepteren van de verbinding van het gateway-apparaat wordt vertrouwd door de downstream-apparaat.

Wanneer u de Azure IoT rand runtime met het script besturingselement installeert, een certificaat voor de Edge-hub is gemaakt, net als bij de zelfstudie [IoT rand installeren op een gesimuleerd apparaat op Windows] [ lnk-tutorial1-win] en [Linux][lnk-tutorial1-lin]. Dit certificaat wordt gebruikt door de hub rand om binnenkomende TLS-verbindingen te accepteren en moet worden vertrouwd door de downstream-apparaat om verbinding te maken met het gatewayapparaat.

U kunt certificaatinfrastructuur waarmee de vertrouwensrelatie die vereist zijn voor de topologie van uw apparaat-gateway maken. In dit artikel gaan we ervan uit dezelfde certificaat instellingen die u gebruiken wilt om in te schakelen [x.509-CA-beveiliging] [ lnk-iothub-x509] in IoT-Hub die een x.509-CA-certificaat dat is gekoppeld aan een specifieke IoT-hub omvat het (de *IoT hub eigenaar CA*), en een reeks certificaten die zijn ondertekend met deze CA, geïnstalleerd op de rand van de IoT-apparaten.

>[!IMPORTANT]
>Op dit moment IoT randapparaten en downstream-apparaten alleen kunt [SAS-tokens] [ lnk-iothub-tokens] voor verificatie met IoT Hub. De certificaten wordt alleen voor het valideren van de TLS-verbinding tussen het leaf en gateway-apparaat gebruikt.

Maakt gebruik van onze configuratie **IoT hub eigenaar CA** als beide:
* Een handtekeningcertificaat voor het instellen van de rand van de IoT-runtime op alle apparaten van de rand van de IoT; en
* een openbare-sleutelcertificaat in downstream apparaten geïnstalleerd.

Dit resulteert in een oplossing waarmee alle apparaten een IoT-randapparaat als een gateway gebruiken als ze zijn verbonden met dezelfde iothub.

## <a name="create-the-certificates-for-test-scenarios"></a>Maken van de certificaten voor Testscenario 's

U kunt het Powershell-voorbeeld en Bash-scripts die worden beschreven [voorbeeld van het beheren van CA-certificaat] [ lnk-ca-scripts] voor het genereren van een zelfondertekend **IoT hub eigenaar CA** en certificaten voor apparaten ondertekend met het.

>[!IMPORTANT]
>Dit voorbeeld is alleen voor testdoeleinden bedoeld. Raadpleeg voor productiescenario's [beveiligen van uw IoT-omgeving] [ lnk-iothub-secure-deployment] voor de Azure IoT-richtlijnen over het beveiligen van uw IoT-oplossing en uw certificaat dienovereenkomstig in te richten.


1. [Microsoft Azure IoT SDK's en -bibliotheken voor C] klonen vanuit GitHub:

   ```
   git clone -b modules-preview https://github.com/Azure/azure-iot-sdk-c.git 
   ```

2. Volg de instructies in voor het installeren van de certificaat-scripts **stap 1: initiële installatie** van [voorbeeld van het beheren van CA-certificaat][lnk-ca-scripts]. 
3. Voor het genereren van de **IoT hub eigenaar CA**, volg de instructies in **stap 2: maken van de certificaatketen**. Dit bestand wordt gebruikt door de downstream-apparaten voor het valideren van de verbinding.
4. Gebruik de Bash of de PowerShell-instructies voor het genereren van een certificaat voor uw gatewayapparaat:

### <a name="bash"></a>Bash

Maak het nieuwe apparaatcertificaat:

   ```
   ./certGen.sh create_edge_device_certificate myGateway
   ```

Nieuwe bestanden worden gemaakt:.\certs\new-edge-device.* bevat de openbare sleutel en het pfx-bestand en.\private\new-edge-device.key.pem bevat de persoonlijke sleutel van het apparaat.
 
In de `certs` map en voer de volgende opdracht om op te halen van de volledige keten van de openbare sleutel van apparaat:

   ```
   cat ./new-edge-device.cert.pem ./azure-iot-test-only.intermediate.cert.pem ./azure-iot-test-only.root.ca.cert.pem > ./new-edge-device-full-chain.cert.pem
   ```

### <a name="powershell"></a>PowerShell

Maak het nieuwe apparaatcertificaat: 
   ```
   New-CACertsEdgeDevice myGateway
   ```

Nieuwe myEdgeDevice * bestanden worden gemaakt, die de openbare sleutel, de persoonlijke sleutel en het PFX van dit certificaat bevat. 

Als u wordt gevraagd een wachtwoord moeten invoeren tijdens het ondertekeningsproces, voer '1234'.

## <a name="configure-a-gateway-device"></a>Een gateway-apparaat configureren

Om uw IoT-randapparaat configureren als een gateway u alleen hoeft te configureren voor het gebruik van het certificaat voor apparaten in de vorige sectie hebt gemaakt.

We ervan uitgaan dat de volgende bestandsnamen van de bovenstaande voorbeeldscripts:

| Uitvoer | Bash-scripts | PowerShell |
| ------ | ----------- | ---------- |
| Certificaat voor apparaten | `certs/new-edge-device.cert.pem` | `certs/new-edge-device.cert.pem` |
| De persoonlijke apparaatsleutel | `private/new-edge-device.cert.pem` | `private/new-edge-device.cert.pem` |
| Certificaatketen apparaat | `certs/new-edge-device-full-chain.cert.pem` | `certs/new-edge-device-full-chain.cert.pem` |
| IoT hub eigenaar CA | `certs/azure-iot-test-only.root.ca.cert.pem` | `RootCA.pem` |

Geef de gegevens van het apparaat en het certificaat aan de rand van de IoT-runtime. 
 
In Linux, met behulp van de Bash-uitvoer:

   ```
   sudo iotedgectl setup --connection-string {device connection string}
        --edge-hostname {gateway hostname, e.g. mygateway.contoso.com}
        --device-ca-cert-file {full path}/certs/new-edge-device.cert.pem
        --device-ca-chain-cert-file {full path}/certs/new-edge-device-full-chain.cert.pem
        --device-ca-private-key-file {full path}/private/new-edge-device.key.pem
        --owner-ca-cert-file {full path}/certs/azure-iot-test-only.root.ca.cert.pem
   ```

In Windows met behulp van de PowerShell-uitvoer:

   ```
   iotedgectl setup --connection-string {device connection string}
        --edge-hostname {gateway hostname, e.g. mygateway.contoso.com}
        --device-ca-cert-file {full path}/certs/new-edge-device.cert.pem
        --device-ca-chain-cert-file {full path}/certs/new-edge-device-full-chain.cert.pem
        --device-ca-private-key-file {full path}/private/new-edge-device.key.pem
        --owner-ca-cert-file {full path}/RootCA.pem
   ```

Standaard Stel de voorbeeldscripts een wachtwoordzin voor de persoonlijke sleutel van het apparaat. Als u een wachtwoordzin instelt, moet u de volgende parameter toevoegen:

   ```
   --device-ca-passphrase {passphrase}
   ```

Het script vraagt u een wachtwoordzin voor het certificaat van de Agent van de rand instellen. Start opnieuw op de rand van de IoT-runtime na deze opdracht:

   ```
   iotedgectl restart
   ```

## <a name="configure-a-downstream-device"></a>Een downstream-apparaat configureren

Een downstream-apparaat kan worden alle toepassingen met behulp van de [Azure IoT-device SDK][lnk-devicesdk], zoals een eenvoudige beschreven in [uw apparaat aansluit op uw IoT-hub met .NET] [ lnk-iothub-getstarted].

Eerst een downstream-device-toepassing te vertrouwen heeft de **IoT hub eigenaar CA** certificaat om te valideren van de TLS-verbindingen met de gatewayapparaten. Deze stap kan doorgaans worden uitgevoerd op twee manieren: op het niveau van het besturingssysteem of (voor bepaalde talen) op het toepassingsniveau van de.

Voor .NET-toepassingen, kunt u bijvoorbeeld het volgende fragment voor vertrouwen in een certificaat in PEM-indeling opgeslagen in pad toevoegen `certPath`. Afhankelijk van welke versie van het script dat u hebt gebruikt, het pad verwijst naar een `certs/azure-iot-test-only.root.ca.cert.pem` (Bash) of `RootCA.pem` (Powershell).

   ```
   using System.Security.Cryptography.X509Certificates;
   
   ...

   X509Store store = new X509Store(StoreName.Root, StoreLocation.CurrentUser);
   store.Open(OpenFlags.ReadWrite);
   store.Add(new X509Certificate2(X509Certificate2.CreateFromCertFile(certPath)));
   store.Close();
   ```

Uitvoeren van deze stap op het niveau van het besturingssysteem verschilt tussen Windows en Linux-distributies.

De tweede stap is het initialiseren van de IoT Hub apparaat-sdk met een verbindingsreeks verwijst naar de hostnaam van het gateway-apparaat.
Dit wordt gedaan door toe te voegen de `GatewayHostName` eigenschap in op uw apparaat-verbindingsreeks. Hier is bijvoorbeeld een apparaat-verbindingsreeks voor het voorbeeld voor een apparaat, waaraan wij toegevoegd de `GatewayHostName` eigenschap:

   ```
   HostName=yourHub.azure-devices-int.net;DeviceId=yourDevice;SharedAccessKey=2BUaYca45uBS/O1AsawsuQslH4GX+SPkrytydWNdFxc=;GatewayHostName=mygateway.contoso.com
   ```

Deze twee stappen inschakelen voor uw apparaattoepassing verbinding maken met het gatewayapparaat.

## <a name="next-steps"></a>Volgende stappen
[Overzicht van de vereisten en hulpmiddelen voor het ontwikkelen van IoT rand modules][lnk-module-dev].

[lnk-devicesdk]: ../iot-hub/iot-hub-devguide-sdks.md
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
[lnk-edge-as-gateway]: ./iot-edge-as-gateway.md
[lnk-module-dev]: module-development.md
[lnk-iothub-getstarted]: ../iot-hub/iot-hub-csharp-csharp-getstarted.md
[lnk-iothub-x509]: ../iot-hub/iot-hub-x509ca-overview.md
[lnk-iothub-secure-deployment]: ../iot-hub/iot-hub-security-deployment.md
[lnk-iothub-tokens]: ../iot-hub/iot-hub-devguide-security.md#security-tokens 
[lnk-iothub-throttles-quotas]: ../iot-hub/iot-hub-devguide-quotas-throttling.md
[lnk-iothub-devicetwins]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-iothub-c2d]: ../iot-hub/iot-hub-devguide-messages-c2d.md
[lnk-ca-scripts]: https://github.com/Azure/azure-iot-sdk-c/blob/modules-preview/tools/CACertificates/CACertificateOverview.md
[lnk-modbus-module]: https://github.com/Azure/iot-edge-modbus
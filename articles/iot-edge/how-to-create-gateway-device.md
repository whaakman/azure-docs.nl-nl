---
title: Maken van een gateway-apparaat met Azure IoT rand | Microsoft Docs
description: Azure IoT Edge gebruiken voor het maken van een gateway-apparaat waarmee informatie voor meerdere apparaten kan worden verwerkt.
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: e1337ddf5ed84a06a62e2faa198f3e8fb49bc3bd
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="create-an-iot-edge-gateway-device-to-process-data-from-other-iot-devices---preview"></a>Een IoT-gateway randapparaat maken om gegevens te verwerken van andere apparaten IoT - voorbeeld

Er zijn twee manieren IoT Edge-apparaten als gateways gebruiken:

* Verbinding maken met downstream-apparaten die gebruikmaken van [Azure IoT-device SDK][lnk-devicesdk];
* Het verbinden van apparaten die geen van de IoT Hub-protocol gebruikmaken.

Als u een IoT-randapparaat als een gateway gebruiken, krijgt u de volgende voordelen:

* **Verbinding multiplex**. Alle apparaten die verbinding maken met IoT Hub via een Edge van de IoT-apparaat gebruikt dezelfde onderliggende verbinding.
* **Verkeer vloeiend**. Het apparaat aan de rand van de IoT implementeert automatisch exponentieel uitstel in geval van een IoT Hub beperking, terwijl de berichten lokaal persistent maken. Hierdoor werkt uw oplossing robuuste naar pieken in het verkeer.
* **Beperkte ondersteuning voor offline**. Het gateway-apparaat wordt lokaal opgeslagen berichten die naar IoT Hub kunnen niet worden bezorgd.

In dit artikel noemen we *gateway aan de rand van de IoT* een IoT-randapparaat gebruikt als een gateway.

>[!NOTE]
>Op dit moment:
> * Downstream-apparaten kunnen niet verifiëren met de gateway als de gateway is losgekoppeld van de IoT Hub; en
> * IoT Edge-apparaten kunnen geen verbinding naar IoT-gateways.

## <a name="use-the-azure-iot-device-sdk"></a>Het apparaat met Azure IoT SDK gebruiken

De Edge-hub die is geïnstalleerd in alle IoT randapparaten toont de volgende primitieven op downstream-apparaten:

* apparaat-naar-cloud- en cloud-naar-apparaat-berichten;
* directe methoden; en
* apparaat twin bewerkingen.

>[!NOTE]
>Downstream apparaten zijn op dit moment niet uploaden bestand gebruiken om verbinding te maken via een gateway IoT rand.

Als u apparaten op een gateway aan de rand van de IoT aansluit-apparaat gebruikt de Azure IoT SDK, moet u naar:

* Instellen van de downstream-apparaat met een verbindingsreeks verwijst naar de hostnaam van de gateway-apparaat; en
* Zorg ervoor dat het certificaat te accepteren van de verbinding van het gateway-apparaat wordt vertrouwd door de downstream-apparaat.

Wanneer u de Azure IoT rand runtime met het script besturingselement installeert, een certificaat voor de hub rand wordt gemaakt, zoals in de zelfstudie installeren IoT-Edge van een gesimuleerd apparaat op [Windows] [ lnk-tutorial1-win] en [Linux][lnk-tutorial1-lin]. Dit certificaat wordt gebruikt door de hub rand om binnenkomende TLS-verbindingen te accepteren en moet worden vertrouwd door de downstream-apparaat om verbinding te maken met het gatewayapparaat.

U kunt certificaatinfrastructuur waarmee de vertrouwensrelatie die vereist zijn voor de topologie van uw apparaat-gateway maken. In dit artikel gaan we ervan uit dezelfde certificaat instellingen die u gebruiken wilt om in te schakelen [x.509-CA-beveiliging] [ lnk-iothub-x509] in IoT-Hub die een x.509-CA-certificaat dat is gekoppeld aan een specifieke IoT-hub omvat het (de  *IoT hub eigenaar CA*), en een reeks certificaten die zijn ondertekend met deze CA, geïnstalleerd op de rand van de IoT-apparaten.

>[!IMPORTANT]
>Op dit moment IoT randapparaten en downstream-apparaten alleen kunt [SAS-tokens] [ lnk-iothub-tokens] voor verificatie met IoT Hub. De certificaten wordt alleen voor het valideren van de TLS-verbinding tussen het leaf en gateway-apparaat gebruikt.

Maakt gebruik van onze configuratie **IoT hub eigenaar CA** als:
* een handtekeningcertificaat voor het instellen van de rand van de IoT-runtime op alle IoT randapparaten en als
* een openbare-sleutelcertificaat in downstream apparaten geïnstalleerd.

Dit leidt tot een oplossing waarmee alle apparaten een IoT-randapparaat als een gateway gebruiken als ze zijn verbonden met dezelfde iothub.

### <a name="create-the-certificates-for-test-scenarios"></a>Maken van de certificaten voor Testscenario 's

U kunt het Powershell-voorbeeld en Bash-scripts die worden beschreven [voorbeeld van het beheren van CA-certificaat] [ lnk-ca-scripts] voor het genereren van een zelfondertekend **IoT hub eigenaar CA** en certificaten voor apparaten ondertekend met het.

1. Ga als volgt stap 1 van [voorbeeld van het beheren van CA-certificaat] [ lnk-ca-scripts] voor het installeren van de scripts.
2. Voer stap 2 voor het genereren van de **IoT hub eigenaar CA**, dit bestand wordt gebruikt door de downstream-apparaten voor het valideren van de verbinding.

Gebruik de volgende instructies voor het genereren van een certificaat voor uw gatewayapparaat.

#### <a name="bash"></a>Bash

* Voer `./certGen.sh create_edge_device_certificate myGateway` voor het maken van het nieuwe apparaatcertificaat.  
  Hiermee maakt u de bestanden.\certs\new-edge-device.* met de openbare sleutel en de PFX en het.\private\new-edge-device.key.pem dat de persoonlijke sleutel van het apparaat bevat.  
* `cat new-edge-device.cert.pem azure-iot-test-only.intermediate.cert.pem azure-iot-test-only.root.ca.cert.pem > new-edge-device-full-chain.cert.pem`ophalen van de openbare sleutel.
* `./private/new-edge-device.cert.pem`bevat de persoonlijke sleutel van het apparaat.

#### <a name="powershell"></a>PowerShell

* Voer `New-CACertsEdgeDevice myGateway` voor het maken van het nieuwe apparaatcertificaat.
  Hiermee maakt u bestanden myEdgeDevice * met de openbare sleutel, de persoonlijke sleutel en het PFX van dit certificaat.  Als u wordt gevraagd een wachtwoord moeten invoeren tijdens het ondertekeningsproces, voer '1234'.


>[!IMPORTANT]
>Dit voorbeeld is alleen voor testdoeleinden bedoeld. Raadpleeg voor productiescenario's [beveiligen van uw IoT-omgeving] [ lnk-iothub-secure-deployment] voor de Azure IoT-richtlijnen over het beveiligen van uw IoT-oplossing en uw certificaat dienovereenkomstig in te richten.

### <a name="configure-an-iot-edge-device-as-a-gateway"></a>Een IoT-randapparaat als een gateway configureren

Om uw IoT-randapparaat configureren als een gateway u alleen hoeft te configureren voor het gebruik van het certificaat voor apparaten in de vorige sectie hebt gemaakt.

We ervan uitgaan dat de volgende bestandsnamen van de bovenstaande voorbeeldscripts:

| Uitvoer | Bash-scripts | PowerShell |
| ------ | ----------- | ---------- |
| Certificaat voor apparaten | `certs/new-edge-device.cert.pem` | `certs/new-edge-device.cert.pem` |
| De persoonlijke apparaatsleutel | `private/new-edge-device.cert.pem` | `private/new-edge-device.cert.pem` |
| Certificaatketen apparaat | `certs/new-edge-device-full-chain.cert.pem` | `certs/new-edge-device-full-chain.cert.pem` |
| IoT hub eigenaar CA | `certs/azure-iot-test-only.root.ca.cert.pem` | `RootCA.pem` |

 Analogously op de installatie wordt beschreven in de Azure IoT-rand implementeren op een gesimuleerd apparaat in [Windows] [ lnk-tutorial1-win] of [Linux][lnk-tutorial1-lin], u Geef de bovenstaande gegevens naar de rand van de IoT-runtime. 
 
 In Linux:

        sudo iotedgectl setup --connection-string {device connection string}
            --edge-hostname {gateway hostname, e.g. mygateway.contoso.com}
            --device-ca-cert-file {full path}/certs/new-edge-device.cert.pem
            --device-ca-chain-cert-file {full path}/certs/new-edge-device-full-chain.cert.pem
            --device-ca-private-key-file {full path}/private/new-edge-device.key.pem
            --owner-ca-cert-file {full path}/certs/azure-iot-test-only.root.ca.cert.pem

In Windows:

        iotedgectl setup --connection-string {device connection string}
            --edge-hostname {gateway hostname, e.g. mygateway.contoso.com}
            --device-ca-cert-file {full path}/certs/new-edge-device.cert.pem
            --device-ca-chain-cert-file {full path}/certs/new-edge-device-full-chain.cert.pem
            --device-ca-private-key-file {full path}/private/new-edge-device.key.pem
            --owner-ca-cert-file {full path}/RootCA.pem

Standaard Stel de voorbeeldscripts een wachtwoordzin voor de persoonlijke sleutel van het apparaat. Als u een wachtwoordzin instelt, moet u de volgende parameter toevoegen:

        --device-ca-passphrase {passphrase}

Het script wordt gevraagd een wachtwoordzin voor het certificaat van de Agent van de rand instellen.
U moet opnieuw opstarten van de rand van de IoT-runtime na deze opdracht.

### <a name="configure-an-iot-hub-device-application-as-a-downstream-device"></a>Een IoT Hub apparaat-toepassing configureren als een downstream-apparaat

Een downstream-apparaat kan worden alle toepassingen met behulp van de [Azure IoT-device SDK][lnk-devicesdk], zoals een eenvoudige beschreven in [uw apparaat aansluit op uw IoT-hub met .NET] [ lnk-iothub-getstarted].

Eerst een downstream-device-toepassing te vertrouwen heeft de **IoT hub eigenaar CA** certificaat om te valideren van de TLS-verbindingen met de gatewayapparaten. Deze stap kan doorgaans worden uitgevoerd op twee manieren: op het niveau van het besturingssysteem of (voor bepaalde talen) op het toepassingsniveau van de.

Voor .NET-toepassingen, kunt u bijvoorbeeld het volgende fragment voor vertrouwen in een certificaat in PEM-indeling opgeslagen in pad toevoegen `certPath`.

        using System.Security.Cryptography.X509Certificates;
        
        ...

        X509Store store = new X509Store(StoreName.Root, StoreLocation.CurrentUser);
        store.Open(OpenFlags.ReadWrite);
        store.Add(new X509Certificate2(X509Certificate2.CreateFromCertFile(certPath)));
        store.Close();

Opmerking dat de hierboven vermelde voorbeeldscripts wordt gegenereerd voor de openbare sleutel in het bestand `certs/azure-iot-test-only.root.ca.cert.pem` (Bash) of `RootCA.pem` (Powershell).

Uitvoeren van deze stap op het niveau van het besturingssysteem verschilt tussen Windows en Linux-distributies.

De tweede stap is het initialiseren van de IoT Hub apparaat-sdk met een verbindingsreeks verwijst naar de hostnaam van het gateway-apparaat.
Dit wordt gedaan door toe te voegen de `GatewayHostName` eigenschap in op uw apparaat-verbindingsreeks. Hier is bijvoorbeeld een apparaat-verbindingsreeks voor het voorbeeld voor een apparaat, waaraan wij toegevoegd de `GatewayHostName` eigenschap:

        HostName=yourHub.azure-devices-int.net;DeviceId=yourDevice;SharedAccessKey=2BUaYca45uBS/O1AsawsuQslH4GX+SPkrytydWNdFxc=;GatewayHostName=mygateway.contoso.com

Deze twee stappen kunnen uw apparaattoepassing verbinding maken met het gatewayapparaat.

## <a name="use-other-protocols"></a>Gebruik van andere protocollen

Een van de belangrijkste functies van een gateway-apparaat is interpreteren protocollen van downstream-apparaten. Voor apparaten die geen van de IoT Hub-protocol gebruikmaken verbinding, kunt u een IoT-Edge-module die deze vertaling uitvoert en verbinding maakt namens de downstream-apparaat met IoT Hub ontwikkelen.

U kunt maken van een *transparante* of *ondoorzichtige* gateway:

| &nbsp; | Transparante gateway | Ondoorzichtige gateway|
|--------|-------------|--------|
| Identiteiten die zijn opgeslagen in de id-register van IoT Hub | Identiteit van alle verbonden apparaten | Alleen de identiteit van het gateway-apparaat |
| Dubbel apparaat | Elk aangesloten apparaat heeft een eigen apparaat-twin | Alleen de gateway heeft een horende apparaten en -module |
| Directe methoden en cloud-naar-apparaat-berichten | De cloud kunt elk aangesloten apparaat afzonderlijk oplossen | De cloud kan zich alleen richten op het gateway-apparaat |
| [IoT Hub, vertragingen en quota 's][lnk-iothub-throttles-quotas] | Van toepassing op elk apparaat | Van toepassing op het gateway-apparaat |

Wanneer u een patroon ondoorzichtige gateway gebruikt, delen alle apparaten die verbinding maken via de gateway die dezelfde cloud-naar-apparaat wachtrijexemplaar, dit kan maximaal 50 berichten bevatten. Betekent dit dat het patroon ondoorzichtige gateway moet worden gebruikt wanneer heel weinig apparaten verbinding via de veldgateway voor elk maken en hun cloud-naar-apparaat-verkeer laag is.

Bij het implementeren van een ondoorzichtige gateway gebruikt uw module protocol de vertaling van de module-verbindingsreeks.

Bij het implementeren van een transparante gateway, maakt de module meerdere exemplaren van de IoT Hub apparaat-client met behulp van de verbindingsreeksen voor de downstream-apparaten.

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van de vereisten en hulpmiddelen voor het ontwikkelen van IoT rand modules][lnk-module-dev].

[lnk-devicesdk]: ../iot-hub/iot-hub-devguide-sdks.md
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
[lnk-module-dev]: module-development.md
[lnk-iothub-getstarted]: ../iot-hub/iot-hub-csharp-csharp-getstarted.md
[lnk-iothub-x509]: ../iot-hub/iot-hub-x509ca-overview.md
[lnk-iothub-secure-deployment]: ../iot-hub/iot-hub-security-deployment.md
[lnk-iothub-tokens]: ../iot-hub/iot-hub-devguide-security.md#security-tokens 
[lnk-iothub-throttles-quotas]: ../iot-hub/iot-hub-devguide-quotas-throttling.md
[lnk-iothub-devicetwins]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-iothub-c2d]: ../iot-hub/iot-hub-devguide-messages-c2d.md
[lnk-ca-scripts]: https://github.com/Azure/azure-iot-sdk-c/blob/CACertToolEdge/tools/CACertificates/CACertificateOverview.md
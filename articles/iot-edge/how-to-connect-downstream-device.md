---
title: 'Verbinding maken met downstream-apparaten: Azure IoT Edge | Microsoft Docs'
description: Het configureren van downstream of leaf-apparaten verbinding maken via Azure IoT Edge-gateway-apparaten.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/01/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 0797952641efb9fe876d72f2dce0418ff5771d40
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53083331"
---
# <a name="connect-a-downstream-device-to-an-azure-iot-edge-gateway"></a>Een downstream-apparaat verbinden met Azure IoT Edge-gateway

Azure IoT Edge kunt transparante gateway-scenario's waarin een of meer apparaten hun berichten via een enkele gateway-apparaat dat de verbinding met IoT Hub onderhoudt kunnen doorgeven. Zodra u het gateway-apparaat dat is geconfigureerd hebt, moet u weten hoe u veilig verbinding maken met de downstream-apparaten. 

In dit artikel identificeert bekende problemen met downstream apparaatverbindingen en helpt u bij het instellen van uw downstream-apparaten door: 

* Uitleg over transport layer security (TLS) en grondbeginselen van het certificaat. 
* Uitleg over de werking van TLS-bibliotheken voor verschillende besturingssystemen en hoe elk besturingssysteem omgaat met certificaten.
* Azure IoT hek voorbeelden in verschillende talen om u te helpen aan de slag. 

In dit artikel, de voorwaarden *gateway* en *IoT Edge-gateway* verwijzen naar een IoT Edge-apparaat dat is geconfigureerd als een transparante gateway. 

## <a name="prerequisites"></a>Vereisten

Voordat u de stappen in dit artikel te volgen, hebt u twee apparaten klaar om te gebruiken:

1. Een IoT Edge-apparaat instellen als een transparante gateway. 
    [Een IoT Edge-apparaat om te fungeren als een transparante gateway configureren](how-to-create-transparent-gateway.md)

    Zodra u uw gateway-apparaat dat is geconfigureerd hebt, Kopieer de **azure-iot-test-only.root.ca.cert.pem** certificaat van de gateway en is deze beschikbaar overal op uw downstream-apparaat. 

2. Een downstream-apparaat met een apparaat-id van IoT Hub. 
    U kunt een IoT Edge-apparaat niet gebruiken als de downstream-apparaat. Gebruik in plaats daarvan een apparaat is geregistreerd als een normale IoT-apparaat in IoT Hub. In de portal, registreert u een nieuwe apparaat in de **IoT-apparaten** sectie. Of u kunt de Azure CLI voor [Registreer een apparaat](../iot-hub/quickstart-send-telemetry-c.md#register-a-device). Kopieer de verbindingsreeks en deze beschikbaar voor gebruik in latere secties. 

    Op dit moment kunnen alleen downstream apparaten met verificatie met een symmetrische sleutel verbinding maken via IoT Edge-gateways. X.509-CA's en zelf-ondertekend X.509-certificaten worden momenteel niet ondersteund. 

## <a name="prepare-a-downstream-device"></a>Een downstream apparaat voorbereiden

Een downstream-apparaat mag een toepassing of het platform waarvoor een identiteit die is gemaakt met de [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub) cloudservice. In veel gevallen gebruikt u deze toepassingen de [Azure IoT device-SDK](../iot-hub/iot-hub-devguide-sdks.md). Praktische overwegingen, een downstream apparaat mogelijk ook een toepassing die wordt uitgevoerd op het IoT Edge-gateway-apparaat zelf. 

Voor een downstream-apparaat verbinding met een IoT Edge-gateway, moet u twee dingen:

1. Een apparaat of een toepassing die geconfigureerd met een IoT Hub apparaat-verbindingsreeks toegevoegd met informatie om te verbinden met de gateway. 

    De verbindingsreeks heeft een indeling zoals: `HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;`. Toevoeg-de **GatewayHostName** eigenschap met de hostnaam van de gateway-apparaat aan het einde van de verbindingsreeks. De waarde van **GatewayHostName** moet overeenkomen met de waarde van **hostnaam** in config.yaml-bestand van het gatewayapparaat. 

    Lijkt op de laatste tekenreeks: `HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;GatewayHostName=mygateway.contoso.com`.

2. Het apparaat of de toepassing is op het vertrouwen van de gateway **basis-CA** of **eigenaar CA** om te valideren van de TLS-verbindingen met de gateway-apparaat. 

    Deze stap meer gecompliceerde wordt in detail uitgelegd in de rest van dit artikel. Deze stap kan worden uitgevoerd een van twee manieren: door het installeren van de CA-certificaat in het certificaatarchief van het besturingssysteem of (voor bepaalde talen) verwijst naar het certificaat in toepassingen met behulp van de Azure IoT SDK's.

## <a name="tls-and-certificate-fundamentals"></a>Basisinformatie over TLS en certificaat

De uitdaging van downstream apparaten veilig verbinding te maken met IoT Edge is net als elke andere beveiligde client/server-communicatie die wordt uitgevoerd via internet. Een client en server veilig communiceren via internet met [Transport layer security (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security). TLS is gebouwd met behulp van standaard [openbare-sleutelinfrastructuur (PKI)](https://en.wikipedia.org/wiki/Public_key_infrastructure) constructies certificaten genoemd. TLS is een tamelijk betrokken specificatie en adressen van een groot aantal onderwerpen met betrekking tot het beveiligen van eindpunten, maar het volgende gedeelte bevat ook wat u nodig hebt apparaten veilig verbinding te maken met een IoT Edge-gateway.

Wanneer een client verbinding met een server maakt, de server een keten van certificaten, met de naam geeft de *server certificaatketen*. Een certificaatketen bestaat gewoonlijk uit een basiscertificaat van de certificaat-certificeringsinstantie (CA), een of meer tussenliggende CA-certificaten en ten slotte certificaat van de server zelf. Een client brengt een vertrouwensrelatie met een server tot stand door cryptografisch te controleren of de certificaatketen voor de hele server. Deze clientvalidatie van certificaatketen voor de server heet *serververificatie*. Een client moet voor het valideren van de certificaatketen van een server, een kopie van het basis-CA-certificaat dat is gebruikt voor het certificaat van de server maken (of uitgeven). Normaal gesproken bij het verbinden met websites, een browser wordt al geconfigureerd geleverd met veelgebruikte CA-certificaten, zodat de client een naadloze proces heeft. 

Wanneer een apparaat verbinding met Azure IoT Hub maakt, het apparaat is de client en de IoT Hub-cloudservice is de server. De IoT Hub-cloudservice wordt ondersteund door een basis-CA-certificaat met de naam **Baltimore CyberTrust Root**, is openbaar beschikbare en gebruikte. Omdat de IoT Hub CA-certificaat is al geïnstalleerd op de meeste apparaten, wordt deze door veel implementaties van TLS (OpenSSL, Schannel, LibreSSL) automatisch gebruikt tijdens de validatie van het servercertificaat. Een apparaat dat is mogelijk verbinding met IoT Hub mogelijk problemen met het verbinden met IoT Edge-gateway.

Wanneer een apparaat verbinding met IoT Edge-gateway maakt, de downstream apparaat is de client en de gateway-apparaat is de server. Azure IoT Edge kunt operators (of gebruikers) gateway certificaatketens bouwen, maar ze naar eigen inzicht. De operator besluiten te gebruiken een openbare CA-certificaat, zoals Baltimore, of een zelf-ondertekend (of interne) basis-CA-certificaat gebruiken. Openbare CA-certificaten zijn kosten verbonden aan ze vaak, dus worden meestal gebruikt in productiescenario's. Zelf-ondertekende CA-certificaten worden verkozen voor ontwikkelen en testen. De transparante gateway setup-artikelen die worden vermeld in de sectie vereisten zelf-ondertekend basis-CA-certificaten gebruiken. 

Wanneer u een zelfondertekend basiscertificaat CA-certificaat voor een IoT Edge-gateway gebruikt, moet worden geïnstalleerd op of die de downstream apparaten probeert verbinding maken met de gateway. 

Zie voor meer informatie over IoT Edge-certificaten en de gevolgen van sommige productie [informatie over het IoT Edge certificaat](iot-edge-certs.md).

## <a name="install-certificates-using-the-os"></a>Certificaten met behulp van het besturingssysteem installeren

In dit artikel wordt gebruikgemaakt van *eigenaar CA* om te verwijzen naar de basis-CA-certificaat, omdat dat de term die wordt gebruikt door de scripts in het artikel vereiste gateway. 

In het algemeen de eigenaar van CA-certificaat in het certificaatarchief van het besturingssysteem te installeren, kunt de meeste toepassingen kunnen gebruikmaken van de eigenaar van de CA-certificaat. Er zijn enkele uitzonderingen, zoals NodeJS-toepassing die geen gebruik van het certificaatarchief van het besturingssysteem, maar in plaats daarvan gebruik van de interne certificaatarchief van de runtime van het knooppunt. Als u het certificaat op het niveau van het besturingssysteem niet installeren, raadpleegt u de taalspecifieke voorbeelden verderop in dit artikel voor het gebruik van het certificaat met de Azure IoT SDK in toepassingen. 

### <a name="ubuntu"></a>Ubuntu

De volgende opdrachten zijn een voorbeeld van een CA-certificaat installeren op een Ubuntu-host. In dit voorbeeld wordt ervan uitgegaan dat u gebruikt de **azure-iot-test-only.root.ca.cert.pem** certificaat van de artikelen vereisten, en dat u het certificaat hebt gekopieerd naar een locatie op de downstream-apparaat.  

```bash
sudo cp <path>/azure-iot-test-only.root.ca.cert.pem /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
sudo update-ca-certificates
```

U ziet een bericht waarin wordt gemeld, "Updating certificaten in /etc/ssl/certs... 1 heeft toegevoegd, 0 verwijderd; gedaan."

### <a name="windows"></a>Windows

De volgende stappen zijn een voorbeeld van een CA-certificaat installeren op een Windows-host. In dit voorbeeld wordt ervan uitgegaan dat u gebruikt de **azure-iot-test-only.root.ca.cert.pem** certificaat van de artikelen vereisten, en dat u het certificaat hebt gekopieerd naar een locatie op de downstream-apparaat.  

1. Zoek in het menu Start en selecteer **computercertificaten beheren**. Een hulpprogramma **certlm** wordt geopend.
2. Navigeer naar **certificaten - lokale Computer** > **Trusted Root Certification Authorities**.
3. Met de rechtermuisknop op **certificaten** en selecteer **alle taken** > **importeren**. De wizard Certificaat importeren te starten. 
4. Volg de stappen te gaan en certificaatbestand importeren `<path>/azure-iot-test-only.root.ca.cert.pem`. Wanneer dit is voltooid, kunt u een "Geïmporteerd" bericht moet zien. 

U kunt ook certificaten programmatisch met behulp van .NET-API's, installeren, zoals weergegeven in de .NET-voorbeeld verderop in dit artikel. 

Toepassingen gebruiken normaal gesproken op de Windows voorwaarde TLS-stack met de naam [Schannel](https://docs.microsoft.com/windows/desktop/com/schannel) veilig verbinding maken via TLS. Schannel *vereist* dat alle certificaten in het certificaatarchief van Windows worden geïnstalleerd voordat u probeert een TLS-verbinding tot stand brengen.

## <a name="use-certificates-with-azure-iot-sdks"></a>Certificaten gebruiken met Azure IoT SDK 's

In dit artikel verwijst naar het basis-CA-certificaat als de *eigenaar CA* omdat dat de term die wordt gebruikt door de scripts die de zelf-ondertekend certificaat in de artikelen vereisten genereren. 

Deze sectie wordt beschreven hoe de Azure IoT SDK's verbinding maken met een IoT Edge-apparaat met behulp van eenvoudige voorbeeldtoepassingen. Het doel van alle voorbeelden is verbinding maken met de apparaatclient en berichten over telemetrie verzenden naar de gateway en Verbreek de verbinding en afsluiten. 

### <a name="common-concepts-across-all-azure-iot-sdks"></a>Algemene concepten voor alle Azure IoT SDK 's

Twee dingen die gereed is voordat u de voorbeelden op toepassingsniveau hebben:

1. Uw downstream apparaat IoT Hub-verbindingsreeks gewijzigd om te verwijzen naar het gateway-apparaat.

    De verbindingsreeks heeft een indeling zoals: `HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;`. Toevoeg-de **GatewayHostName** eigenschap met de hostnaam van de gateway-apparaat aan het einde van de verbindingsreeks. De waarde van **GatewayHostName** moet overeenkomen met de waarde van **hostnaam** in config.yaml-bestand van het gatewayapparaat. 

    Lijkt op de laatste tekenreeks: `HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;GatewayHostName=mygateway.contoso.com`.

2. Het volledige pad naar het basis-CA-certificaat dat u hebt gekopieerd en opgeslagen ergens op uw downstream-apparaat.

    Bijvoorbeeld `<path>/azure-iot-test-only.root.ca.cert.pem`. 

### <a name="nodejs"></a>Node.js

Deze sectie bevat een voorbeeld van toepassing op een client voor Azure IoT-NodeJS-apparaten verbinden met IoT Edge-gateway. Voor Linux en Windows-hosts, moet u het basis-CA-certificaat op het toepassingsniveau van de zoals hieronder, zoals NodeJS toepassingen geen gebruik van het systeem-certificaatarchief. 

1. Ophalen van het voorbeeld voor **edge_downstream_device.js** uit de [Azure IoT device-SDK voor Node.js-voorbeelden opslagplaats](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples). 
2. Zorg ervoor dat u alle vereisten hebt voor het uitvoeren van het voorbeeld aan de hand van de **readme.md** bestand. 
3. In het bestand edge_downstream_device.js bijwerken de **connectionString** en **edge_ca_cert_path** variabelen. 
4. Raadpleeg de SDK-documentatie voor instructies over het uitvoeren van het voorbeeld op uw apparaat. 

Voor inzicht in het voorbeeld dat u uitvoert, wordt het volgende codefragment weergegeven hoe de client-SDK leest het certificaatbestand en gebruikt voor het maken van een beveiligde TLS-verbinding: 

```nodejs
// Provide the Azure IoT device client via setOptions with the X509
// Edge root CA certificate that was used to setup the Edge runtime
var options = {
    ca : fs.readFileSync(edge_ca_cert_path, 'utf-8'),
};
```

### <a name="net"></a>.NET

In deze sectie wordt een voorbeeld van toepassing op de client voor een Azure IoT .NET-apparaten verbinden met IoT Edge-gateway. .NET-toepassingen kunnen echter automatisch voor het gebruik van alle geïnstalleerde certificaten in het certificaatarchief van het systeem op zowel Windows als Linux-hosts.

1. Ophalen van het voorbeeld voor **EdgeDownstreamDevice** uit de [IoT Edge .NET-voorbeelden map](https://github.com/Azure/iotedge/tree/master/samples/dotnet/EdgeDownstreamDevice). 
2. Zorg ervoor dat u alle vereisten hebt voor het uitvoeren van het voorbeeld aan de hand van de **readme.md** bestand. 
3. In de **eigenschappen / launchSettings.json** bestand de **DEVICE_CONNECTION_STRING** en **CA_CERTIFICATE_PATH** variabelen. Als u gebruikmaken van het certificaat geïnstalleerd in het vertrouwde certificaatarchief op het hostsysteem wilt, laat u deze variabele leeg. 
4. Raadpleeg de SDK-documentatie voor instructies over het uitvoeren van het voorbeeld op uw apparaat. 

Als u wilt installeren via een programma een vertrouwd certificaat in het certificaatarchief via een .NET-toepassing, verwijzen naar de **InstallCACert()** werken in de **EdgeDownstreamDevice / Program.cs** bestand. Deze bewerking is idempotent zijn, zodat meerdere keren met dezelfde waarden zonder extra effect kunnen worden uitgevoerd. 

### <a name="c"></a>C

In deze sectie wordt een voorbeeld van toepassing op de client van een Azure IoT C-apparaten verbinden met IoT Edge-gateway. De C-SDK kunnen werken met veel TLS-bibliotheken, waaronder OpenSSL, WolfSSL en Schannel. Zie voor meer informatie de [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c). 

1. Krijgen de **iotedge_downstream_device_sample** toepassing uit de [Azure IoT device-SDK voor C-voorbeelden](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples). 
2. Zorg ervoor dat u alle vereisten hebt voor het uitvoeren van het voorbeeld aan de hand van de **readme.md** bestand. 
3. In het bestand iotedge_downstream_device_sample.c bijwerken de **connectionString** en **edge_ca_cert_path** variabelen. 
4. Raadpleeg de SDK-documentatie voor instructies over het uitvoeren van het voorbeeld op uw apparaat. 

De Azure IoT device-SDK voor C biedt een optie voor het registreren van een CA-certificaat bij het instellen van de client. Met deze bewerking niet overal in het certificaat wordt geïnstalleerd, maar in plaats daarvan maakt gebruik van een tekenreeksindeling van het certificaat in het geheugen. Het opgeslagen certificaat is verstrekt aan de onderliggende TLS-stack wanneer een verbinding tot stand brengen. 

```C
(void)IoTHubDeviceClient_SetOption(device_handle, OPTION_TRUSTED_CERT, cert_string);
```

Op Windows-hosts, als u niet OpenSSL of een andere TLS-bibliotheek, de SDK standaard gebruikmaken van Schannel. Voor Schannel om te werken, moet het IoT Edge-basis-CA-certificaat worden geïnstalleerd in het Windows-certificaatarchief, niet is ingesteld met behulp van de `IoTHubDeviceClient_SetOption` bewerking. 

### <a name="java"></a>Java

In deze sectie wordt een voorbeeld van toepassing op de client van een Azure IoT-Java-apparaten verbinden met IoT Edge-gateway. 

1. Ophalen van het voorbeeld voor **verzenden-gebeurtenis** uit de [Azure IoT device-SDK voor Java-voorbeelden](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples). 
2. Zorg ervoor dat u alle vereisten hebt voor het uitvoeren van het voorbeeld aan de hand van de **readme.md** bestand. 
3. Raadpleeg de SDK-documentatie voor instructies over het uitvoeren van het voorbeeld op uw apparaat.

### <a name="python"></a>Python

In deze sectie wordt een voorbeeld van toepassing op de client van een Azure IoT Python-apparaten verbinden met IoT Edge-gateway. 

1. Ophalen van het voorbeeld voor **edge_downstream_client** uit de [Azure IoT device-SDK voor Python-voorbeelden](https://github.com/Azure/azure-iot-sdk-python/tree/master/device/samples). 
2. Zorg ervoor dat u alle vereisten hebt voor het uitvoeren van het voorbeeld aan de hand van de **readme.md** bestand. 
3. In het bestand edge_downstream_client.py bijwerken de **CONNECTION_STRING** en **TRUSTED_ROOT_CA_CERTIFICATE_PATH** variabelen. 
4. Raadpleeg de SDK-documentatie voor instructies over het uitvoeren van het voorbeeld op uw apparaat. 


## <a name="test-the-gateway-connection"></a>De gatewayverbinding testen

Dit is een voorbeeldopdracht instellen voor welke tests dat alles is correct. U ziet een bericht 'OK geverifieerd'.

```cmd/sh
openssl s_client -connect mygateway.contoso.com:8883 -CAfile <CERTDIR>/certs/azure-iot-test-only.root.ca.cert.pem -showcerts
```

## <a name="next-steps"></a>Volgende stappen

Informatie over hoe IoT Edge kunt uitbreiden [offlinemogelijkheden](offline-capabilities.md) op downstream-apparaten. 
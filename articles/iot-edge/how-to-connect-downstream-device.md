---
title: 'Verbinding maken met downstream-apparaten: Azure IoT Edge | Microsoft Docs'
description: Het configureren van downstream of leaf-apparaten verbinding maken met Azure IoT Edge-gateway-apparaten.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/07/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 7a66355ca1a0c9c2c144f04cd944efe22467d3ae
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67058509"
---
# <a name="connect-a-downstream-device-to-an-azure-iot-edge-gateway"></a>Een downstream-apparaat verbinden met Azure IoT Edge-gateway

In dit artikel bevat instructies voor het maken van een betrouwbare verbinding tussen downstream-apparaten en transparante IoT Edge-gateways. In een transparante gateway-scenario, kunnen een of meer apparaten hun berichten doorgeven via een enkele gateway-apparaat dat de verbinding met IoT Hub onderhoudt. Een downstream-apparaat mag een toepassing of het platform waarvoor een identiteit die is gemaakt met de [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub) cloudservice. In veel gevallen gebruikt u deze toepassingen de [Azure IoT device-SDK](../iot-hub/iot-hub-devguide-sdks.md). Een downstream-apparaat kan ook worden voor een toepassing die wordt uitgevoerd op het IoT Edge-gateway-apparaat zelf. 

Er zijn drie algemene stappen voor het instellen van een geslaagde transparante gateway-verbinding. In dit artikel bevat informatie over de derde stap:

1. Het gateway-apparaat moet veilig verbinding maken met downstream apparaten, ontvangen een bericht van downstream-apparaten en het routeren van berichten naar de juiste bestemming. Zie voor meer informatie, [configureren van een IoT Edge-apparaat om te fungeren als een transparante gateway](how-to-create-transparent-gateway.md).
2. De downstream-apparaat moet een apparaat-id kunnen verifiëren met IoT Hub en weten om te communiceren via de gateway-apparaat. Zie voor meer informatie, [verifiëren van een downstream-apparaat met Azure IoT Hub](how-to-authenticate-downstream-device.md).
3. **De downstream-apparaat moet kunnen veilig verbinding maken met de gateway-apparaat.**

In dit artikel identificeert bekende problemen met downstream apparaatverbindingen en helpt u bij het instellen van uw downstream-apparaten door: 

* Uitleg over transport layer security (TLS) en grondbeginselen van het certificaat. 
* Uitleg over de werking van TLS-bibliotheken voor verschillende besturingssystemen en hoe elk besturingssysteem omgaat met certificaten.
* Azure IoT hek voorbeelden in verschillende talen om u te helpen aan de slag. 

In dit artikel, de voorwaarden *gateway* en *IoT Edge-gateway* verwijzen naar een IoT Edge-apparaat dat is geconfigureerd als een transparante gateway. 

## <a name="prepare-a-downstream-device"></a>Een downstream apparaat voorbereiden

Een downstream-apparaat mag een toepassing of het platform waarvoor een identiteit die is gemaakt met de [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub) cloudservice. In veel gevallen gebruikt u deze toepassingen de [Azure IoT device-SDK](../iot-hub/iot-hub-devguide-sdks.md). Een downstream-apparaat kan ook worden voor een toepassing die wordt uitgevoerd op het IoT Edge-gateway-apparaat zelf. 

Voor een downstream-apparaat verbinding met een IoT Edge-gateway, moet u twee dingen:

* Een apparaat of een toepassing die geconfigureerd met een IoT Hub apparaat-verbindingsreeks toegevoegd met informatie om te verbinden met de gateway. 

    Deze stap wordt uitgelegd in [verifiëren van een downstream-apparaat met Azure IoT Hub](how-to-authenticate-downstream-device.md).

* Het apparaat of de toepassing is op het vertrouwen van de gateway **basis-CA** om te valideren van de TLS-verbindingen met de gateway-apparaat. 

    Deze stap wordt in detail uitgelegd in de rest van dit artikel. Deze stap kan worden uitgevoerd een van twee manieren: door het installeren van de CA-certificaat in het certificaatarchief van het besturingssysteem of (voor bepaalde talen) door te verwijzen naar het certificaat in toepassingen met behulp van de Azure IoT SDK's.

## <a name="tls-and-certificate-fundamentals"></a>Basisinformatie over TLS en certificaat

De uitdaging van downstream apparaten veilig verbinding te maken met IoT Edge is net als elke andere beveiligde client/server-communicatie die wordt uitgevoerd via internet. Een client en server veilig communiceren via internet met [Transport layer security (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security). TLS is gebouwd met behulp van standaard [openbare-sleutelinfrastructuur (PKI)](https://en.wikipedia.org/wiki/Public_key_infrastructure) constructies certificaten genoemd. TLS is een tamelijk betrokken specificatie en adressen van een groot aantal onderwerpen met betrekking tot beveiliging van de twee eindpunten. In deze sectie bevat een overzicht van de concepten die relevant zijn voor u apparaten veilig verbinding te maken met een IoT Edge-gateway.

Wanneer een client verbinding met een server maakt, de server een keten van certificaten, met de naam geeft de *server certificaatketen*. Een certificaatketen bestaat gewoonlijk uit een basiscertificaat van de certificaat-certificeringsinstantie (CA), een of meer tussenliggende CA-certificaten en ten slotte certificaat van de server zelf. Een client brengt een vertrouwensrelatie met een server tot stand door cryptografisch te controleren of de certificaatketen voor de hele server. Deze clientvalidatie van certificaatketen voor de server heet *validatie van certificaatketen*. De client de service om te bewijzen dat bezit is van de persoonlijke sleutel die is gekoppeld aan het certificaat van de server in een proces genaamd cryptografisch uitdagingen *bewijs van bezit*. De combinatie van validatie van certificaatketen en bewijs van bezit heet *serververificatie*. Een client moet voor het valideren van de certificaatketen van een server, een kopie van het basis-CA-certificaat dat is gebruikt voor het certificaat van de server maken (of uitgeven). Normaal gesproken bij het verbinden met websites, een browser wordt al geconfigureerd geleverd met veelgebruikte CA-certificaten, zodat de client een naadloze proces heeft. 

Wanneer een apparaat verbinding met Azure IoT Hub maakt, het apparaat is de client en de IoT Hub-cloudservice is de server. De IoT Hub-cloudservice wordt ondersteund door een basis-CA-certificaat met de naam **Baltimore CyberTrust Root**, is openbaar beschikbare en gebruikte. Omdat de IoT Hub CA-certificaat is al geïnstalleerd op de meeste apparaten, wordt deze door veel implementaties van TLS (OpenSSL, Schannel, LibreSSL) automatisch gebruikt tijdens de validatie van het servercertificaat. Een apparaat dat is mogelijk verbinding met IoT Hub mogelijk problemen met het verbinden met IoT Edge-gateway.

Wanneer een apparaat verbinding met IoT Edge-gateway maakt, de downstream apparaat is de client en de gateway-apparaat is de server. Azure IoT Edge kunt operators (of gebruikers) gateway certificaatketens bouwen, maar ze naar eigen inzicht. De operator besluiten te gebruiken een openbare CA-certificaat, zoals Baltimore, of een zelf-ondertekend (of interne) basis-CA-certificaat gebruiken. Openbare CA-certificaten zijn kosten verbonden aan ze vaak, dus worden meestal gebruikt in productiescenario's. Zelf-ondertekende CA-certificaten worden verkozen voor ontwikkelen en testen. De transparante gateway setup-artikelen die worden vermeld in de inleiding zelf-ondertekend basis-CA-certificaten gebruiken. 

Wanneer u een zelfondertekend basiscertificaat CA-certificaat voor een IoT Edge-gateway gebruikt, moet worden geïnstalleerd op of die de downstream apparaten probeert verbinding maken met de gateway. 

![Certificaat-gatewayupgrade](./media/how-to-create-transparent-gateway/gateway-setup.png)

Zie voor meer informatie over IoT Edge-certificaten en de gevolgen van sommige productie [informatie over het IoT Edge certificaat](iot-edge-certs.md).

## <a name="provide-the-root-ca-certificate"></a>Geef het basis-CA-certificaat

Als u wilt controleren of de certificaten van het gatewayapparaat, moet de downstream een eigen kopie van de CA-basiscertificaat. Als u de scripts die zijn opgegeven in de git-opslagplaats voor IoT Edge gebruikt voor het maken van testcertificaten, wordt de basis-CA-certificaat wordt aangeroepen **azure-iot-test-only.root.ca.cert.pem**. Als u niet hebt gedaan als onderdeel van de andere downstream apparaat voorbereidende stappen, verplaatst u dit certificaatbestand in een willekeurige map in uw downstream-apparaat. U kunt een service, zoals [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) of een functie, zoals [beveiligde kopie protocol](https://www.ssh.com/ssh/scp/) te verplaatsen van het certificaatbestand.

## <a name="install-certificates-in-the-os"></a>Certificaten in het besturingssysteem installeren

Installeren van de CA-basiscertificaat in het certificaatarchief van het besturingssysteem in het algemeen kan de meeste toepassingen gebruiken het basis-CA-certificaat. Er zijn enkele uitzonderingen, zoals NodeJS toepassingen die gebruikmaken van de OS-certificaat niet opslaan, maar in plaats daarvan gebruik van de interne certificaatarchief van de runtime van het knooppunt. Als u het certificaat op het niveau van het besturingssysteem installeren kunt, gaat u verder met [-certificaten gebruiken met Azure IoT SDK's](#use-certificates-with-azure-iot-sdks). 

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

Deze sectie wordt beschreven hoe de Azure IoT SDK's verbinding maken met een IoT Edge-apparaat met behulp van eenvoudige voorbeeldtoepassingen. Het doel van alle voorbeelden is verbinding maken met de apparaatclient en berichten over telemetrie verzenden naar de gateway en Verbreek de verbinding en afsluiten. 

Twee dingen die gereed is voordat u de voorbeelden op toepassingsniveau hebben:

* IoT Hub-verbindingsreeks van uw downstream apparaat gewijzigd om te verwijzen naar het gateway-apparaat en eventuele certificaten vereist voor het verifiëren van uw downstream-apparaat met IoT Hub. Zie voor meer informatie, [verifiëren van een downstream-apparaat met Azure IoT Hub](how-to-authenticate-downstream-device.md).

* Het volledige pad naar het basis-CA-certificaat dat u hebt gekopieerd en opgeslagen ergens op uw downstream-apparaat.

    Bijvoorbeeld `<path>/azure-iot-test-only.root.ca.cert.pem`. 

### <a name="nodejs"></a>Node.js

Deze sectie bevat een voorbeeld van toepassing op een client voor Azure IoT-NodeJS-apparaten verbinden met IoT Edge-gateway. Voor NodeJS-toepassingen, moet u het basis-CA-certificaat op het toepassingsniveau van de zoals hier wordt weergegeven. NodeJS-toepassingen gebruik geen van het systeem-certificaatarchief. 

1. Ophalen van het voorbeeld voor **edge_downstream_device.js** uit de [Azure IoT device-SDK voor Node.js-voorbeelden opslagplaats](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples). 
2. Zorg ervoor dat u alle vereisten hebt voor het uitvoeren van het voorbeeld aan de hand van de **readme.md** bestand. 
3. In het bestand edge_downstream_device.js bijwerken de **connectionString** en **edge_ca_cert_path** variabelen. 
4. Raadpleeg de SDK-documentatie voor instructies over het uitvoeren van het voorbeeld op uw apparaat. 

Voor inzicht in het voorbeeld dat u uitvoert, wordt het volgende codefragment weergegeven hoe de client-SDK leest het certificaatbestand en gebruikt voor het maken van een beveiligde TLS-verbinding: 

```javascript
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

Dit is een voorbeeld van een opdracht waarmee wordt getest dat alles is is juist ingesteld. U ziet een bericht 'OK geverifieerd'.

```cmd/sh
openssl s_client -connect mygateway.contoso.com:8883 -CAfile <CERTDIR>/certs/azure-iot-test-only.root.ca.cert.pem -showcerts
```

## <a name="troubleshoot-the-gateway-connection"></a>Problemen met de gatewayverbinding oplossen

Als het leaf-apparaat heeft een onregelmatige verbinding met de gateway-apparaat, probeert u de volgende stappen uit voor het omzetten van. 

1. De hostnaam van de gateway in de verbindingsreeks is hetzelfde als de waarde van de hostnaam in het IoT Edge config.yaml-bestand op de gateway-apparaat?
2. Is de hostnaam van de gateway worden opgelost als een IP-adres? U kunt onregelmatige verbindingen oplossen met behulp van DNS of door een hostbestandvermelding toe te voegen op het leaf-apparaat.
3. Communicatiepoorten openen in uw firewall zijn? Communicatie op basis van het protocol dat wordt gebruikt (MQTTS:8883 / AMQPS:5671 / HTTPS:433) moet mogelijk tussen downstream-apparaat en het transparant IoT Edge.

## <a name="next-steps"></a>Volgende stappen

Informatie over hoe IoT Edge kunt uitbreiden [offlinemogelijkheden](offline-capabilities.md) op downstream-apparaten. 

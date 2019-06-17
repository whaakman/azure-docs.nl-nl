---
title: Verificatie van apparaten downstream - Azure IoT Edge | Microsoft Docs
description: Het verifiëren van downstream apparaten of leaf-apparaten bij IoT Hub en hun verbindingen routeren via Azure IoT Edge-gateway-apparaten.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/07/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 5785b0260474bd0eb861236a0bd78066475baacd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082389"
---
# <a name="authenticate-a-downstream-device-to-azure-iot-hub"></a>Verifiëren met Azure IoT Hub een downstream-apparaat

In een scenario met transparante gateway moeten downstream apparaten (ook wel leaf-apparaten of apparaten van de onderliggende) identiteiten van IoT-Hub, zoals elk ander apparaat. In dit artikel helpt bij de opties voor het verifiëren van een downstream-apparaat naar IoT Hub, en vervolgens ziet u hoe u de gatewayverbinding declareren.

Er zijn drie algemene stappen voor het instellen van een geslaagde transparante gateway-verbinding. In dit artikel bevat informatie over de tweede stap:

1. Het gateway-apparaat moet kunnen veilig verbinding maken met downstream-apparaten, ontvangen een bericht van downstream-apparaten en het routeren van berichten naar de juiste bestemming. Zie voor meer informatie, [configureren van een IoT Edge-apparaat om te fungeren als een transparante gateway](how-to-create-transparent-gateway.md).
2. **De downstream moet een apparaat-id te kunnen verifiëren met IoT Hub en weten om te communiceren via de gateway-apparaat.**
3. De downstream-apparaat moet kunnen veilig verbinding maken met de gateway-apparaat. Zie voor meer informatie, [een downstream-apparaat verbinden met Azure IoT Edge-gateway](how-to-connect-downstream-device.md).

Downstream-apparaten kunnen worden geverifieerd met IoT Hub met behulp van een van drie methoden: symmetrische sleutels (soms aangeduid als gedeelde toegangssleutels), zelf-ondertekend X.509-certificaten of certificeringsinstantie (CA) certificaten ondertekend X.509-certificaat. De verificatie-stappen zijn vergelijkbaar met de stappen voor het instellen van elk IoT Edge-apparaat met IoT Hub, met kleine verschillen om aan te geven van de gateway-relatie.

De stappen in dit artikel laten zien handmatige apparaatinrichting, niet automatisch inrichten met de Azure IoT Hub Device Provisioning Service. 

## <a name="symmetric-key-authentication"></a>Verificatie met een symmetrische sleutel

Symmetrische sleutel verificatie of verificatie van de sleutel van de gedeelde toegang, is de eenvoudigste manier om te verifiëren met IoT Hub. Met verificatie via symmetrische sleutel is een met base64-sleutel gekoppeld aan uw IoT-apparaat-ID van IoT-Hub. U opnemen deze sleutel in uw IoT-toepassingen, zodat het apparaat presenteren kunt bij het verbinden met IoT Hub. 

### <a name="create-the-device-identity"></a>De apparaat-id maken 

Voeg een nieuwe IoT-apparaat in uw IoT-hub met behulp van de Azure-portal, Azure CLI of de IoT-extensie voor Visual Studio Code. Houd er rekening mee dat downstream apparaten moeten worden geïdentificeerd in IoT Hub als normale IoT-apparaat, niet op IoT Edge-apparaten. 

Wanneer u het nieuwe apparaat-id maakt, moet u de volgende informatie leveren: 

* Maak een ID voor uw apparaat.

* Selecteer **symmetrische sleutel** als verificatietype. 

* Optioneel, **instellen van een apparaat bovenliggende** en selecteert u het IoT Edge-gatewayapparaat dat via deze downstream apparaat verbinding. Deze stap is optioneel voor verificatie met een symmetrische sleutel, maar het wordt aanbevolen omdat het instellen van een bovenliggende-apparaat kunt [offlinemogelijkheden](offline-capabilities.md) voor uw downstream-apparaat. U kunt altijd de details ervan toevoegen of wijzigen van de bovenliggende later bijwerken. 

   ![Apparaat-ID maken met de symmetrische sleutel auth in portal](./media/how-to-authenticate-downstream-device/symmetric-key-portal.png)

U kunt de [IoT-extensie voor Azure CLI](https://github.com/Azure/azure-iot-cli-extension) om de dezelfde bewerking te voltooien. Het volgende voorbeeld maakt u een nieuwe IoT-apparaat met verificatie met een symmetrische sleutel en wijst een bovenliggende-apparaat: 

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID}
```

Zie voor meer informatie over Azure CLI-opdrachten voor het maken van het apparaat en het beheer van de bovenliggende/onderliggende de referentie-inhoud voor [az iot hub device-identity](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) opdrachten.

### <a name="connect-to-iot-hub-through-a-gateway"></a>Verbinding maken met IoT Hub via een gateway

Hetzelfde proces wordt gebruikt voor verificatie van de normale IoT-apparaten naar IoT Hub met symmetrische sleutels geldt ook voor downstream-apparaten. Het enige verschil is dat u een wijzer toevoegen aan de gateway-apparaat wilt voor het routeren van de verbinding of, in offlinescenario's, voor het afhandelen van de verificatie namens de IoT Hub. 

Voor verificatie met een symmetrische sleutel is er geen extra stappen die u moet uitvoeren op uw apparaat om te verifiëren met IoT Hub. U moet nog steeds de certificaten op locatie zodat uw downstream-apparaat verbinding met de gateway-apparaat maken kan, zoals beschreven in [een downstream-apparaat verbinden met Azure IoT Edge-gateway](how-to-connect-downstream-device.md).

Na het maken van een IoT-apparaat-id in de portal, kunt u de primaire of secundaire sleutels ophalen. Een van deze sleutels moet worden opgenomen in de verbindingsreeks die u in elke toepassing die communiceert met IoT Hub. Voor verificatie met een symmetrische sleutel, wordt met IoT Hub de volledig gevormde verbindingsreeks in de details ervan biedt voor uw gemak. U moet extra informatie over het gateway-apparaat toevoegen aan de verbindingsreeks. 

Symmetrische sleutel verbindingsreeksen voor downstream-apparaten moeten de volgende onderdelen: 

* De IoT-hub die het apparaat verbinding met maakt: `Hostname={iothub name}.azure-devices.net`
* De ID van het apparaat geregistreerd bij de hub: `DeviceID={device ID}`
* Het primaire of secundaire sleutel: `SharedAccessKey={key}`
* Het gatewayapparaat dat het apparaat verbinding via maakt. Geef de **hostnaam** waarde uit het IoT Edge-gatewayapparaat config.yaml bestand: `GatewayHostName={gateway hostname}`

Alles bij elkaar, een volledige verbindingsreeks ziet eruit zoals:

``` 
HostName=myiothub.azure-devices.net;DeviceId=myDownstreamDevice;SharedAccessKey=xxxyyyzzz;GatewayHostName=myGatewayDevice
```

Als u een bovenliggende/onderliggende relatie voor dit downstream apparaat tot stand gebracht, kunt u de verbindingsreeks vereenvoudigen door het aanroepen van de gateway rechtstreeks als de host verbinding. Bijvoorbeeld: 

```
HostName=myGatewayDevice;DeviceId=myDownstreamDevice;SharedAccessKey=xxxyyyzzz
```

## <a name="x509-authentication"></a>X.509-verificatie 

Er zijn twee manieren om een IoT-apparaat met behulp van X.509-certificaten te verifiëren. Welke manier u wilt verifiëren, de stappen voor uw apparaat verbinden met IoT Hub zijn hetzelfde. Kies zelf-ondertekend of CA-ondertekende certificaten voor verificatie, en vervolgens verder voor meer informatie over verbinding maken met IoT Hub. 

Zie voor meer informatie over hoe IoT Hub maakt gebruik van X.509-verificatie, de volgende artikelen: 
* [Apparaatverificatie met behulp van x.509-CA-certificaten](../iot-hub/iot-hub-x509ca-overview.md)
* [Conceptuele kennis van x.509-CA-certificaten in de branche IoT](../iot-hub/iot-hub-x509ca-concept.md)

### <a name="create-the-device-identity-with-x509-self-signed-certificates"></a>De apparaat-id maken met zelfondertekend X.509-certificaten

Voor de zelf-ondertekend x.509-verificatie, soms ook wel de vingerafdruk van verificatie, die u wilt maken van nieuwe certificaten om op uw IoT-apparaat te plaatsen. Deze certificaten hebben een vingerafdruk in dat u met IoT Hub voor verificatie deelt. 

De eenvoudigste manier om dit scenario testen is met dezelfde computer die u gebruikt voor het maken van certificaten in [configureren van een IoT Edge-apparaat om te fungeren als een transparante gateway](how-to-create-transparent-gateway.md). Deze machine moet al zijn ingesteld met de juiste hulpprogramma, de CA-basiscertificaat en tussenliggende CA-certificaat te maken van de certificaten voor IoT-apparaten. U kunt de laatste certificaten en de persoonlijke sleutels via kopiëren naar uw downstream-apparaat daarna. Na de stappen in de gateway-artikel, kunt u openssl instellen op uw computer en vervolgens gekloond van de IoT Edge-opslagplaats naar scripts voor het maken van access-certificaat. Vervolgens kunt u een werkmap die we noemen aangebracht  **\<WRKDIR >** voor het opslaan van de certificaten. De standaardcertificaten zijn bedoeld voor ontwikkelen en testen, zodat alleen de laatste 30 dagen. U moet een basis-CA-certificaat en een tussenliggende certificaat hebt gemaakt. 

1. Navigeer naar uw werkmap in een bash- of PowerShell-venster. 

2. Maak twee certificaten (primaire en secundaire) voor de downstream-apparaat. Geef de apparaatnaam van uw en klik vervolgens op het primaire of secundaire label. Deze informatie wordt gebruikt om de naam van de bestanden, zodat u kunt bijhouden van certificaten voor meerdere apparaten. 

   ```PowerShell
   New-CACertsDevice "<device name>-primary"
   New-CACertsDevice "<device name>-secondary"
   ```

   ```bash
   ./certGen.sh create_device_certificate "<device name>-primary"
   ./certGen.sh create_device_certificate "<device name>-secondary"
   ```

3. De SHA1-vingerafdruk (een vingerafdruk in de IoT Hub-interface genoemd) ophalen van elk certificaat dat een reeks 40 hexadecimale tekens is. Gebruik de volgende openssl-opdracht om te bekijken van het certificaat en de vingerafdruk vinden:

   ```PowerShell/bash
   openssl x509 -in <WORKDIR>/certs/iot-device-<device name>-primary.cert.pem -text -fingerprint | sed 's/[:]//g'
   ```

4. Navigeer naar uw IoT-hub in Azure portal en maak een nieuwe IoT-apparaat-id met de volgende waarden: 

   * Selecteer **zelfondertekend X.509** als verificatietype.
   * Plak de hexadecimale tekenreeksen die u hebt gekopieerd uit de primaire en secundaire certificaten van uw apparaat.
   * Selecteer **instellen van een apparaat bovenliggende** en kies de IoT Edge-gatewayapparaat dat via deze downstream apparaat verbinding. Een bovenliggende-apparaat is vereist voor X.509-verificatie van een downstream-apparaat. 

   ![Apparaat-ID maken met de zelf-ondertekend X.509-verificatie in de portal](./media/how-to-authenticate-downstream-device/x509-self-signed-portal.png)

5. Kopieer de volgende bestanden naar een willekeurige map in uw downstream-apparaat:

   * `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device name>*.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device id>*.cert.pfx`
   * `<WRKDIR>\certs\iot-device-<device name>*-full-chain.cert.pem`
   * `<WRKDIR>\private\iot-device-<device name>*.key.pem`

   U moet verwijzen naar deze bestanden in de toepassingen van de leaf-apparaten die verbinding met IoT Hub maken. Kunt u een service, zoals [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) of een functie, zoals [beveiligde kopie protocol](https://www.ssh.com/ssh/scp/) de certificaatbestanden te verplaatsen.

U kunt de [IoT-extensie voor Azure CLI](https://github.com/Azure/azure-iot-cli-extension) om uit te voeren van de bewerking voor het maken van hetzelfde apparaat. Het volgende voorbeeld maakt u een nieuwe IoT-apparaat met de zelf-ondertekend X.509-verificatie en wijst een bovenliggende-apparaat: 

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID} --am x509_thumbprint --ptp {primary thumbprint} --stp {secondary thumbprint}
```

Zie voor meer informatie over Azure CLI-opdrachten voor het maken van het apparaat, certificaat genereren en bovenliggende en onderliggende management, de referentie-inhoud voor [az iot hub device-identity](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) opdrachten.

### <a name="create-the-device-identity-with-x509-ca-signed-certificates"></a>Maken van het apparaat identiteit met X.509-CA ondertekende certificaten

Voor x.509-certificaat (certificeringsinstantie) ondertekende verificatie moet u een basis-CA-certificaat dat is geregistreerd in IoT-Hub die u gebruiken voor het ondertekenen van certificaten voor uw IoT-apparaat. Elk apparaat met een certificaat dat is van problemen door het basis-CA-certificaat of een van de tussenliggende certificaten worden toegestaan om te verifiëren. 

In deze sectie is gebaseerd op de instructies die zijn uiteengezet in de IoT Hub-artikel [instellen van de x.509-beveiliging in uw Azure-IoT-hub](../iot-hub/iot-hub-security-x509-get-started.md). Volg de stappen in deze sectie om te weten welke waarden u moet gebruiken voor het instellen van een downstream-apparaat dat is verbonden via een gateway. 

De eenvoudigste manier om dit scenario te testen op dezelfde computer die u gebruikt voor het maken van certificaten in gebruik is [configureren van een IoT Edge-apparaat om te fungeren als een transparante gateway](how-to-create-transparent-gateway.md). Deze machine moet al zijn ingesteld met de juiste hulpprogramma, de CA-basiscertificaat en tussenliggende CA-certificaat te maken van de certificaten voor IoT-apparaten. U kunt de laatste certificaten en de persoonlijke sleutels via kopiëren naar uw downstream-apparaat daarna. Na de stappen in de gateway-artikel, kunt u openssl instellen op uw computer en vervolgens gekloond van de IoT Edge-opslagplaats naar scripts voor het maken van access-certificaat. Vervolgens kunt u een werkmap die we noemen aangebracht  **\<WRKDIR >** voor het opslaan van de certificaten. De standaardcertificaten zijn bedoeld voor ontwikkelen en testen, zodat alleen de laatste 30 dagen. U moet een basis-CA-certificaat en een tussenliggende certificaat hebt gemaakt. 

1. Volg de instructies in de [registreren x.509-CA-certificaten voor uw IoT-hub](../iot-hub/iot-hub-security-x509-get-started.md#register-x509-ca-certificates-to-your-iot-hub) sectie van *instellen van de x.509-beveiliging in uw Azure-IoT-hub*. In deze sectie maakt uitvoeren u de volgende stappen: 

   1. Een basis-CA-certificaat uploaden. Als u de certificaten die u in het artikel transparante gateway hebt gemaakt, uploadt u  **\<WRKDIR > /certs/azure-iot-test-only.root.ca.cert.pem** als bestand van het basiscertificaat. 
   2. Controleren of u eigenaar dat de basis-CA-certificaat bent. U kunt controleren of bezit met hulpprogramma's voor het certificaat in \<WRKDIR >. 

      ```powershell
      New-CACertsVerificationCert "<verification code from Azure portal>"
      ```

      ```bash
      ./certGen.sh create_verification_certificate <verification code from Azure portal>"
      ```

2. Volg de instructies in de [maken van een X.509-apparaat voor uw IoT-hub](../iot-hub/iot-hub-security-x509-get-started.md#create-an-x509-device-for-your-iot-hub) sectie van *instellen van de x.509-beveiliging in uw Azure-IoT-hub*. In deze sectie maakt uitvoeren u de volgende stappen: 

   1. Een nieuw apparaat toevoegen. Geef een naam op in kleine letters voor **apparaat-ID**, en kies het verificatietype **x.509-CA ondertekend**. 
   2. Een bovenliggende-apparaat instellen. Selecteer voor downstream-apparaten, **instellen van een apparaat van de bovenliggende** en kies de IoT Edge-gatewayapparaat dat de verbinding met IoT Hub biedt. 

3. Maak een certificaatketen voor uw downstream-apparaat. Gebruik dezelfde CA-basiscertificaat dat u hebt geüpload naar IoT Hub om deze keten. Gebruik de dezelfde kleine apparaat-ID die u hebt opgegeven op uw apparaat-id in de portal.

   ```powershell
   New-CACertsDevice "<device id>"
   ```

   ```bash
   ./certGen.sh create_device_certificate "<device id>"
   ```

4. Kopieer de volgende bestanden naar een willekeurige map in uw downstream-apparaat: 

   * `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device id>*.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device id>*.cert.pfx`
   * `<WRKDIR>\certs\iot-device-<device id>*-full-chain.cert.pem`
   * `<WRKDIR>\private\iot-device-<device id>*.key.pem`

   U moet verwijzen naar deze bestanden in de toepassingen van de leaf-apparaten die verbinding met IoT Hub maken. Kunt u een service, zoals [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) of een functie, zoals [beveiligde kopie protocol](https://www.ssh.com/ssh/scp/) de certificaatbestanden te verplaatsen.

U kunt de [IoT-extensie voor Azure CLI](https://github.com/Azure/azure-iot-cli-extension) om uit te voeren van de bewerking voor het maken van hetzelfde apparaat. Het volgende voorbeeld maakt u een nieuwe IoT-apparaat met de ondertekende X.509-CA-verificatie en wijst een bovenliggende-apparaat: 

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID} --am x509_ca
```

Zie voor meer informatie over Azure CLI-opdrachten voor het maken van het apparaat en het beheer van de bovenliggende/onderliggende de referentie-inhoud voor [az iot hub device-identity](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) opdrachten.


### <a name="connect-to-iot-hub-through-a-gateway"></a>Verbinding maken met IoT Hub via een gateway

Elke Azure-IoT-SDK verwerkt iets anders X.509-verificatie. Echter hetzelfde proces wordt gebruikt voor verificatie van de normale IoT-apparaten naar IoT Hub met X.509-certificaten geldt ook voor downstream-apparaten. Het enige verschil is dat u een wijzer toevoegen aan de gateway-apparaat wilt voor het routeren van de verbinding of, in offlinescenario's, voor het afhandelen van de verificatie namens de IoT Hub. In het algemeen kunt u dezelfde stappen X.509-verificatie voor alle IoT Hub-apparaten, en vervolgens gewoon Vervang de waarde van **hostnaam** in de verbindingsreeks moet de hostnaam van uw gateway-apparaat. 

De volgende secties ziet enkele voorbeelden voor verschillende talen van de SDK. 

>[!IMPORTANT]
>De volgende voorbeelden laten zien hoe de IoT Hub SDK's certificaten gebruiken om apparaten te verifiëren. U moet alle geheimen zoals particuliere of SAS-sleutels in een beveiligde hardware-module (HSM) opslaan in een productie-implementatie. 

#### <a name="net"></a>.NET

Voor een voorbeeld van een C# programma verificatie bij IoT Hub met X.509-certificaten, Zie [instellen van de x.509-beveiliging in uw Azure-IoT-hub](../iot-hub/iot-hub-security-x509-get-started.md#authenticate-your-x509-device-with-the-x509-certificates). Enkele van de belangrijkste regels van het desbetreffende voorbeeld zijn hier opgenomen ter demonstratie van het verificatieproces uit.

Bij het melden van de hostnaam voor uw exemplaar DeviceClient, gebruik het IoT Edge-gatewayapparaat hostnaam. De hostnaam kan worden gevonden in de gateway-apparaat config.yaml bestand. 

Als u de testcertificaten geleverd door de IoT Edge git-opslagplaats, de sleutel voor de certificaten is **1234**.

```csharp
try
{
    var cert = new X509Certificate2(@"<absolute-path-to-your-device-pfx-file>", "1234");
    var auth = new DeviceAuthenticationWithX509Certificate("<device-id>", cert);
    var deviceClient = DeviceClient.Create("<gateway hostname>", auth, TransportType.Amqp_Tcp_Only);

    if (deviceClient == null)
    {
        Console.WriteLine("Failed to create DeviceClient!");
    }
    else
    {
        Console.WriteLine("Successfully created DeviceClient!");
        SendEvent(deviceClient).Wait();
    }

    Console.WriteLine("Exiting...\n");
}
catch (Exception ex)
{
    Console.WriteLine("Error in sample: {0}", ex.Message);
}
```

#### <a name="c"></a>C

Voor een voorbeeld van een programma C verificatie bij IoT Hub met X.509-certificaten, Zie de C IoT-SDK [iotedge_downstream_device_sample](https://github.com/Azure/azure-iot-sdk-c/tree/x509_edge_bugbash/iothub_client/samples/iotedge_downstream_device_sample) voorbeeld. Enkele van de belangrijkste regels van het desbetreffende voorbeeld zijn hier opgenomen ter demonstratie van het verificatieproces uit.

Bij het definiëren van de verbindingsreeks voor uw downstream-apparaat, gebruik het IoT Edge-gatewayapparaat hostnaam voor de **hostnaam** parameter. De hostnaam kan worden gevonden in de gateway-apparaat config.yaml bestand. 

```C
// If your downstream device uses X.509 authentication (self signed or X.509 CA) then
// resulting connection string should look like the following:
// "HostName=<gateway device hostname>;DeviceId=<device_id>;x509=true"
static const char* connectionString = "[Downstream device IoT Edge connection string]";

// Path to the Edge "owner" root CA certificate
static const char* edge_ca_cert_path = "[Path to root CA certificate]";

// When the downstream device uses X.509 authentication, a certificate and key 
// in PRM format must be provided.
static const char * x509_device_cert_path = "[Path to primary or secondary device cert]";
static const char * x509_device_key_path = "[Path to primary or secondary device key]";

int main(void)
{
    // Create the iothub handle here
    device_handle = IoTHubDeviceClient_CreateFromConnectionString(connectionString, protocol);

    // Provide the Azure IoT device client with the same root
    // X509 CA certificate that was used to set up the IoT Edge gateway runtime
    if (edge_ca_cert_path != NULL)
    {
        cert_string = obtain_edge_ca_certificate();
        (void)IoTHubDeviceClient_SetOption(device_handle, OPTION_TRUSTED_CERT, cert_string);
    }

    if ((x509_device_cert_path != NULL) && (x509_device_key_path != NULL))
    {
        const char *x509certificate = obtain_file_contents(x509_device_cert_path);
        const char *x509privatekey = obtain_file_contents(x509_device_key_path);
        if ((IoTHubDeviceClient_SetOption(device_handle, OPTION_X509_CERT, x509certificate) != IOTHUB_CLIENT_OK) ||
            (IoTHubDeviceClient_SetOption(device_handle, OPTION_X509_PRIVATE_KEY, x509privatekey) != IOTHUB_CLIENT_OK)
            )
        {
            printf("failure to set options for x509, aborting\r\n");
            exit(1);
        }
    }
}
```

#### <a name="nodejs"></a>Node.js

Zie voor een voorbeeld van een Node.js-programma verificatie bij IoT Hub met X.509-certificaten, de Node.js IoT-SDK [simple_sample_device_x509.js](https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device_x509.js) voorbeeld. Enkele van de belangrijkste regels van het desbetreffende voorbeeld zijn hier opgenomen ter demonstratie van het verificatieproces uit.

Bij het definiëren van de verbindingsreeks voor uw downstream-apparaat, gebruik het IoT Edge-gatewayapparaat hostnaam voor de **hostnaam** parameter. De hostnaam kan worden gevonden in de gateway-apparaat config.yaml bestand. 

Als u de testcertificaten geleverd door de IoT Edge git-opslagplaats, de sleutel voor de certificaten is **1234**.

```node
// String containing Hostname and Device Id in the following format:
//  "HostName=<gateway device hostname>;DeviceId=<device_id>;x509=true"
var connectionString = '<DEVICE CONNECTION STRING WITH x509=true>';
var certFile = '<PATH-TO-CERTIFICATE-FILE>';
var keyFile = '<PATH-TO-KEY-FILE>';
var passphrase = '<KEY PASSPHRASE IF ANY>';

// fromConnectionString must specify a transport constructor, coming from any transport package.
var client = Client.fromConnectionString(connectionString, Protocol);

var options = {
   cert : fs.readFileSync(certFile, 'utf-8').toString(),
   key : fs.readFileSync(keyFile, 'utf-8').toString(),
   passphrase: passphrase
 };

// Calling setOptions with the x509 certificate and key (and optionally, passphrase) will configure the client transport to use x509 when connecting to IoT Hub
client.setOptions(options);
```

#### <a name="python"></a>Python

Zie voor een voorbeeld van een Python-programma verificatie bij IoT Hub met X.509-certificaten, de Java IoT-SDK [iothub_client_sample_x509.py](https://github.com/Azure/azure-iot-sdk-python/blob/master/device/samples/iothub_client_sample_x509.py) voorbeeld. Enkele van de belangrijkste regels van het desbetreffende voorbeeld zijn hier opgenomen ter demonstratie van het verificatieproces uit.

Bij het definiëren van de verbindingsreeks voor uw downstream-apparaat, gebruik het IoT Edge-gatewayapparaat hostnaam voor de **hostnaam** parameter. De hostnaam kan worden gevonden in de gateway-apparaat config.yaml bestand. 

```python
# String containing Hostname, Device Id in the format:
# "HostName=<gateway device hostname>;DeviceId=<device_id>;x509=true"
CONNECTION_STRING = "[Device Connection String]"

X509_CERTIFICATE = (
    "-----BEGIN CERTIFICATE-----""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "...""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "XXXXXXXXXXXX""\n"
    "-----END CERTIFICATE-----"
)

X509_PRIVATEKEY = (
    "-----BEGIN RSA PRIVATE KEY-----""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "...""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    "-----END RSA PRIVATE KEY-----"
)

def iothub_client_init():
    # prepare iothub client
    client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

    # this brings in x509 privateKey and certificate
    client.set_option("x509certificate", X509_CERTIFICATE)
    client.set_option("x509privatekey", X509_PRIVATEKEY)

    return client
```

#### <a name="java"></a>Java

Voor een voorbeeld van een Java-programma, verificatie bij IoT Hub met X.509-certificaten, Zie de Java IoT-SDK [SendEventX509.java](https://github.com/Azure/azure-iot-sdk-python/blob/master/device/samples/iothub_client_sample_x509.py) voorbeeld. Enkele van de belangrijkste regels van het desbetreffende voorbeeld zijn hier opgenomen ter demonstratie van het verificatieproces uit.

Bij het definiëren van de verbindingsreeks voor uw downstream-apparaat, gebruik het IoT Edge-gatewayapparaat hostnaam voor de **hostnaam** parameter. De hostnaam kan worden gevonden in de gateway-apparaat config.yaml bestand. 

```java
//PEM encoded representation of the public key certificate
private static String publicKeyCertificateString =
    "-----BEGIN CERTIFICATE-----\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "-----END CERTIFICATE-----\n";

//PEM encoded representation of the private key
private static String privateKeyString =
    "-----BEGIN EC PRIVATE KEY-----\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "-----END EC PRIVATE KEY-----\n";

DeviceClient client = new DeviceClient(connectionString, protocol, publicKeyCertificateString, false, privateKeyString, false);
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel is voltooid, moet u een IoT Edge-apparaat werkt als een transparante gateway hebben en een downstream apparaat geregistreerd bij een IoT-hub. Vervolgens moet u uw downstream-apparaten voor het vertrouwen van de gateway-apparaat en berichten verzenden naar deze. Zie voor meer informatie, [een downstream-apparaat verbinden met Azure IoT Edge-gateway](how-to-connect-downstream-device.md).

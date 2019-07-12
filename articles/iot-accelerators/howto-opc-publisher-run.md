---
title: OPC Publisher - Azure uitvoeren | Microsoft Docs
description: OPC Publisher uitvoeren
author: dominicbetts
ms.author: dobett
ms.date: 06/10/2019
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 3b386171afc7916e5e803c39a9c7b3520752e6fd
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603756"
---
# <a name="run-opc-publisher"></a>OPC Publisher uitvoeren

In dit artikel wordt beschreven hoe u foutopsporing ad OPC Publisher worden uitgevoerd. Overwegingen voor prestaties en het geheugen ook oplost.

## <a name="command-line-options"></a>Opdrachtregelopties

Gebruik van de toepassing wordt weergegeven met de `--help` opdrachtregeloptie te gebruiken als volgt te werk:

```sh/cmd
Current directory is: /appdata
Log file is: <hostname>-publisher.log
Log level is: info

OPC Publisher V2.3.0
Informational version: V2.3.0+Branch.develop_hans_methodlog.Sha.0985e54f01a0b0d7f143b1248936022ea5d749f9

Usage: opcpublisher.exe <applicationname> [<IoT Hubconnectionstring>] [<options>]

OPC Edge Publisher to subscribe to configured OPC UA servers and send telemetry to Azure IoT Hub.
To exit the application, just press CTRL-C while it is running.

applicationname: the OPC UA application name to use, required
                  The application name is also used to register the publisher under this name in the
                  IoT Hub device registry.

IoT Hubconnectionstring: the IoT Hub owner connectionstring, optional

There are a couple of environment variables which can be used to control the application:
_HUB_CS: sets the IoT Hub owner connectionstring
_GW_LOGP: sets the filename of the log file to use
_TPC_SP: sets the path to store certificates of trusted stations
_GW_PNFP: sets the filename of the publishing configuration file

Command line arguments overrule environment variable settings.

Options:
      --pf, --publishfile=VALUE
                              the filename to configure the nodes to publish.
                                Default: '/appdata/publishednodes.json'
      --tc, --telemetryconfigfile=VALUE
                              the filename to configure the ingested telemetry
                                Default: ''
  -s, --site=VALUE           the site OPC Publisher is working in. if specified
                                this domain is appended (delimited by a ':' to
                                the 'ApplicationURI' property when telemetry is
                                sent to IoT Hub.
                                The value must follow the syntactical rules of a
                                DNS hostname.
                                Default: not set
      --ic, --iotcentral     publisher will send OPC UA data in IoTCentral
                                compatible format (DisplayName of a node is used
                                as key, this key is the Field name in IoTCentral)
                                . you need to ensure that all DisplayName's are
                                unique. (Auto enables fetch display name)
                                Default: False
      --sw, --sessionconnectwait=VALUE
                              specify the wait time in seconds publisher is
                                trying to connect to disconnected endpoints and
                                starts monitoring unmonitored items
                                Min: 10
                                Default: 10
      --mq, --monitoreditemqueuecapacity=VALUE
                              specify how many notifications of monitored items
                                can be stored in the internal queue, if the data
                                can not be sent quick enough to IoT Hub
                                Min: 1024
                                Default: 8192
      --di, --diagnosticsinterval=VALUE
                              shows publisher diagnostic info at the specified
                                interval in seconds (need log level info).
                                -1 disables remote diagnostic log and diagnostic
                                output
                                0 disables diagnostic output
                                Default: 0
      --ns, --noshutdown=VALUE
                              same as runforever.
                                Default: False
      --rf, --runforever     publisher can not be stopped by pressing a key on
                                the console, but will run forever.
                                Default: False
      --lf, --logfile=VALUE  the filename of the logfile to use.
                                Default: './<hostname>-publisher.log'
      --lt, --logflushtimespan=VALUE
                              the timespan in seconds when the logfile should be
                                flushed.
                                Default: 00:00:30 sec
      --ll, --loglevel=VALUE the loglevel to use (allowed: fatal, error, warn,
                                info, debug, verbose).
                                Default: info
        --ih, --IoT Hubprotocol=VALUE
                              the protocol to use for communication with IoT Hub (
                                allowed values: Amqp, Http1, Amqp_WebSocket_Only,
                                  Amqp_Tcp_Only, Mqtt, Mqtt_WebSocket_Only, Mqtt_
                                Tcp_Only) or IoT EdgeHub (allowed values: Mqtt_
                                Tcp_Only, Amqp_Tcp_Only).
                                Default for IoT Hub: Mqtt_WebSocket_Only
                                Default for IoT EdgeHub: Amqp_Tcp_Only
      --ms, --IoT Hubmessagesize=VALUE
                              the max size of a message which can be send to
                                IoT Hub. when telemetry of this size is available
                                it will be sent.
                                0 will enforce immediate send when telemetry is
                                available
                                Min: 0
                                Max: 262144
                                Default: 262144
      --si, --IoT Hubsendinterval=VALUE
                              the interval in seconds when telemetry should be
                                send to IoT Hub. If 0, then only the
                                IoT Hubmessagesize parameter controls when
                                telemetry is sent.
                                Default: '10'
      --dc, --deviceconnectionstring=VALUE
                              if publisher is not able to register itself with
                                IoT Hub, you can create a device with name <
                                applicationname> manually and pass in the
                                connectionstring of this device.
                                Default: none
  -c, --connectionstring=VALUE
                              the IoT Hub owner connectionstring.
                                Default: none
      --hb, --heartbeatinterval=VALUE
                              the publisher is using this as default value in
                                seconds for the heartbeat interval setting of
                                nodes without
                                a heartbeat interval setting.
                                Default: 0
      --sf, --skipfirstevent=VALUE
                              the publisher is using this as default value for
                                the skip first event setting of nodes without
                                a skip first event setting.
                                Default: False
      --pn, --portnum=VALUE  the server port of the publisher OPC server
                                endpoint.
                                Default: 62222
      --pa, --path=VALUE     the enpoint URL path part of the publisher OPC
                                server endpoint.
                                Default: '/UA/Publisher'
      --lr, --ldsreginterval=VALUE
                              the LDS(-ME) registration interval in ms. If 0,
                                then the registration is disabled.
                                Default: 0
      --ol, --opcmaxstringlen=VALUE
                              the max length of a string opc can transmit/
                                receive.
                                Default: 131072
      --ot, --operationtimeout=VALUE
                              the operation timeout of the publisher OPC UA
                                client in ms.
                                Default: 120000
      --oi, --opcsamplinginterval=VALUE
                              the publisher is using this as default value in
                                milliseconds to request the servers to sample
                                the nodes with this interval
                                this value might be revised by the OPC UA
                                servers to a supported sampling interval.
                                please check the OPC UA specification for
                                details how this is handled by the OPC UA stack.
                                a negative value will set the sampling interval
                                to the publishing interval of the subscription
                                this node is on.
                                0 will configure the OPC UA server to sample in
                                the highest possible resolution and should be
                                taken with care.
                                Default: 1000
      --op, --opcpublishinginterval=VALUE
                              the publisher is using this as default value in
                                milliseconds for the publishing interval setting
                                of the subscriptions established to the OPC UA
                                servers.
                                please check the OPC UA specification for
                                details how this is handled by the OPC UA stack.
                                a value less than or equal zero will let the
                                server revise the publishing interval.
                                Default: 0
      --ct, --createsessiontimeout=VALUE
                              specify the timeout in seconds used when creating
                                a session to an endpoint. On unsuccessful
                                connection attemps a backoff up to 5 times the
                                specified timeout value is used.
                                Min: 1
                                Default: 10
      --ki, --keepaliveinterval=VALUE
                              specify the interval in seconds the publisher is
                                sending keep alive messages to the OPC servers
                                on the endpoints it is connected to.
                                Min: 2
                                Default: 2
      --kt, --keepalivethreshold=VALUE
                              specify the number of keep alive packets a server
                                can miss, before the session is disconneced
                                Min: 1
                                Default: 5
      --aa, --autoaccept     the publisher trusts all servers it is
                                establishing a connection to.
                                Default: False
      --tm, --trustmyself=VALUE
                              same as trustowncert.
                                Default: False
      --to, --trustowncert   the publisher certificate is put into the trusted
                                certificate store automatically.
                                Default: False
      --fd, --fetchdisplayname=VALUE
                              same as fetchname.
                                Default: False
      --fn, --fetchname      enable to read the display name of a published
                                node from the server. this will increase the
                                runtime.
                                Default: False
      --ss, --suppressedopcstatuscodes=VALUE
                              specifies the OPC UA status codes for which no
                                events should be generated.
                                Default: BadNoCommunication,
                                BadWaitingForInitialData
      --at, --appcertstoretype=VALUE
                              the own application cert store type.
                                (allowed values: Directory, X509Store)
                                Default: 'Directory'
      --ap, --appcertstorepath=VALUE
                              the path where the own application cert should be
                                stored
                                Default (depends on store type):
                                X509Store: 'CurrentUser\UA_MachineDefault'
                                Directory: 'pki/own'
      --tp, --trustedcertstorepath=VALUE
                              the path of the trusted cert store
                                Default: 'pki/trusted'
      --rp, --rejectedcertstorepath=VALUE
                              the path of the rejected cert store
                                Default 'pki/rejected'
      --ip, --issuercertstorepath=VALUE
                              the path of the trusted issuer cert store
                                Default 'pki/issuer'
      --csr                  show data to create a certificate signing request
                                Default 'False'
      --ab, --applicationcertbase64=VALUE
                              update/set this applications certificate with the
                                certificate passed in as bas64 string
      --af, --applicationcertfile=VALUE
                              update/set this applications certificate with the
                                certificate file specified
      --pb, --privatekeybase64=VALUE
                              initial provisioning of the application
                                certificate (with a PEM or PFX fomat) requires a
                                private key passed in as base64 string
      --pk, --privatekeyfile=VALUE
                              initial provisioning of the application
                                certificate (with a PEM or PFX fomat) requires a
                                private key passed in as file
      --cp, --certpassword=VALUE
                              the optional password for the PEM or PFX or the
                                installed application certificate
      --tb, --addtrustedcertbase64=VALUE
                              adds the certificate to the applications trusted
                                cert store passed in as base64 string (multiple
                                strings supported)
      --tf, --addtrustedcertfile=VALUE
                              adds the certificate file(s) to the applications
                                trusted cert store passed in as base64 string (
                                multiple filenames supported)
      --ib, --addissuercertbase64=VALUE
                              adds the specified issuer certificate to the
                                applications trusted issuer cert store passed in
                                as base64 string (multiple strings supported)
      --if, --addissuercertfile=VALUE
                              adds the specified issuer certificate file(s) to
                                the applications trusted issuer cert store (
                                multiple filenames supported)
      --rb, --updatecrlbase64=VALUE
                              update the CRL passed in as base64 string to the
                                corresponding cert store (trusted or trusted
                                issuer)
      --uc, --updatecrlfile=VALUE
                              update the CRL passed in as file to the
                                corresponding cert store (trusted or trusted
                                issuer)
      --rc, --removecert=VALUE
                              remove cert(s) with the given thumbprint(s) (
                                multiple thumbprints supported)
      --dt, --devicecertstoretype=VALUE
                              the IoT Hub device cert store type.
                                (allowed values: Directory, X509Store)
                                Default: X509Store
      --dp, --devicecertstorepath=VALUE
                              the path of the iot device cert store
                                Default Default (depends on store type):
                                X509Store: 'My'
                                Directory: 'CertificateStores/IoT Hub'
  -i, --install              register OPC Publisher with IoT Hub and then exits.
                                Default:  False
  -h, --help                 show this message and exit
      --st, --opcstacktracemask=VALUE
                              ignored, only supported for backward comaptibility.
      --sd, --shopfloordomain=VALUE
                              same as site option, only there for backward
                                compatibility
                                The value must follow the syntactical rules of a
                                DNS hostname.
                                Default: not set
      --vc, --verboseconsole=VALUE
                              ignored, only supported for backward comaptibility.
      --as, --autotrustservercerts=VALUE
                              same as autoaccept, only supported for backward
                                cmpatibility.
                                Default: False
      --tt, --trustedcertstoretype=VALUE
                              ignored, only supported for backward compatibility.
                                the trusted cert store will always reside in a
                                directory.
      --rt, --rejectedcertstoretype=VALUE
                              ignored, only supported for backward compatibility.
                                the rejected cert store will always reside in a
                                directory.
      --it, --issuercertstoretype=VALUE
                              ignored, only supported for backward compatibility.
                                the trusted issuer cert store will always
                                reside in a directory.
```

U doorgaans opgeven de verbindingsreeks van de IoT Hub-eigenaar alleen op de eerste uitvoering van de toepassing. De verbindingsreeks is versleuteld en opgeslagen in het certificaatarchief van platform. De toepassing op hoger wordt uitgevoerd, wordt de verbindingsreeks uit het certificaatarchief gelezen. Als u de verbindingsreeks op elke uitvoering opgeeft, wordt het apparaat dat gemaakt voor de toepassing in de IoT Hub-apparaatregister verwijderd en opnieuw gemaakt.

## <a name="run-natively-on-windows"></a>Standaard worden uitgevoerd op Windows

Open de **opcpublisher.sln** project met Visual Studio, maak de oplossing en deze publiceren. U kunt de toepassing starten in de **doelmap** u gepubliceerd om te worden als volgt:

```cmd
dotnet opcpublisher.dll <applicationname> [<IoT Hubconnectionstring>] [options]
```

## <a name="use-a-self-built-container"></a>Een zelf gebouwde container gebruiken

Bouw uw eigen container en start als volgt:

```sh/cmd
docker run <your-container-name> <applicationname> [<IoT Hubconnectionstring>] [options]
```

## <a name="use-a-container-from-microsoft-container-registry"></a>Een container uit Microsoft Container Registry gebruiken

Er is een vooraf gedefinieerde container beschikbaar in de Microsoft-Containerregister. Start als volgt uit:

```sh/cmd
docker run mcr.microsoft.com/iotedge/opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

Controleer [Docker Hub](https://hub.docker.com/_/microsoft-iotedge-opc-publisher) om te zien van de ondersteunde besturingssystemen en de processorarchitectuur. Als uw besturingssysteem en CPU-architectuur wordt ondersteund, worden de juiste container in Docker automatisch geselecteerd.

## <a name="run-as-an-azure-iot-edge-module"></a>Uitvoeren als een Azure IoT Edge-module

OPC Publisher is gereed om te worden gebruikt als een [Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge) module. Wanneer u OPC Publisher als IoT Edge-module gebruikt, de enige ondersteunde transport protocollen zijn **Amqp_Tcp_Only** en **Mqtt_Tcp_Only**.

OPC Publisher module toevoegen aan uw IoT Edge-implementatie, gaat u naar uw IoT Hub-instellingen in Azure portal en voer de volgende stappen uit:

1. Ga naar **IoT Edge** en maak of Selecteer uw IoT Edge-apparaat.
1. Selecteer **Modules instellen**.
1. Selecteer **toevoegen** onder **implementatie Modules** en vervolgens **IoT Edge-Module**.
1. In de **naam** veld **publisher**.
1. In de **URI installatiekopie** veld `mcr.microsoft.com/iotedge/opc-publisher:<tag>`
1. U vindt de beschikbare labels op [Docker Hub](https://hub.docker.com/_/microsoft-iotedge-opc-publisher)
1. Plak de volgende JSON naar de **Container maken opties** veld:

    ```json
    {
        "Hostname": "publisher",
        "Cmd": [
            "--aa"
        ]
    }
    ```

    Deze configuratie configureert IoT Edge voor het starten van een container met de naam **publisher** met behulp van de OPC Publisher. De hostnaam van de container system is ingesteld op **publisher**. OPC Publisher is aangeroepen met de volgende opdrachtregelargument: `--aa`. OPC Publisher vertrouwensrelaties met deze optie worden de certificaten van deze verbinding met maakt de OPC UA-servers. U kunt alle OPC Publisher opdrachtregelopties gebruiken. De enige beperking is de grootte van de **Container maken opties** ondersteund door de IoT Edge.

1. Laat de overige instellingen ongewijzigd en selecteer **Opslaan**.
1. Als u de uitvoer van de OPC-uitgever lokaal met een andere IoT Edge-module worden verwerkt wilt, gaat u terug naar de **Modules instellen** pagina. Ga vervolgens naar de **Routes opgeven** tabblad en voeg een nieuwe route dat lijkt op de volgende JSON:

    ```json
    {
      "routes": {
        "processingModuleToIoT Hub": "FROM /messages/modules/processingModule/outputs/* INTO $upstream",
        "opcPublisherToProcessingModule": "FROM /messages/modules/publisher INTO BrokeredEndpoint(\"/modules/processingModule/inputs/input1\")"
      }
    }
    ```

1. Klik in de **Modules instellen** weergeeft, schakelt **volgende**, totdat u de laatste pagina van de configuratie.
1. Selecteer **indienen** voor het verzenden van uw configuratie op IoT Edge.
1. Wanneer u bent begonnen met IoT Edge op uw edge-apparaat en de docker-container **publisher** wordt uitgevoerd, kunt u de logboekuitvoer van OPC Publisher bekijken met behulp van `docker logs -f publisher` of door het controleren van het logboekbestand. In het vorige voorbeeld, het logboekbestand is hoger dan `d:\iiotegde\publisher-publisher.log`. U kunt ook de [hulpprogramma iot-edge-opc-uitgever-diagnostics](https://github.com/Azure-Samples/iot-edge-opc-publisher-diagnostics).

### <a name="make-the-configuration-files-accessible-on-the-host"></a>Controleer de configuratiebestanden toegankelijk is op de host

Als u wilt dat de IoT Edge module toegankelijk is in het bestandssysteem van de host configuratiebestanden, gebruikt u de volgende **Container maken opties**. Het volgende voorbeeld is van een implementatie met behulp van Linux-Containers voor Windows:

```json
{
    "Hostname": "publisher",
    "Cmd": [
        "--pf=./pn.json",
        "--aa"
    ],
    "HostConfig": {
        "Binds": [
            "d:/iiotedge:/appdata"
        ]
    }
}
```

Met deze opties OPC Publisher leest de knooppunten moet het publiceren van het bestand `./pn.json` en de werkmap van de container is ingesteld op `/appdata` bij het opstarten. Met deze instellingen OPC Publisher leest het bestand `/appdata/pn.json` uit de container om op te halen van de configuratie ervan. Zonder de `--pf` optie, OPC Publisher probeert te lezen van het configuratiebestand `./publishednodes.json`.

Het logboekbestand, met behulp van de standaardnaam `publisher-publisher.log`, worden geschreven naar `/appdata` en de `CertificateStores` directory ook in deze map wordt gemaakt.

Als u al deze bestanden in het bestandssysteem van de host, vereist de containerconfiguratie van de een bind mount-volume. De `d://iiotedge:/appdata` afhankelijk van de map toewijst `/appdata`, dit is de huidige werkmap bij het opstarten van de container, naar de hostmap `d://iiotedge`. Zonder deze optie worden er geen gegevens uit een bestand persistent gemaakt zodra de container wordt gestart.

Als u werkt met Windows-containers, en vervolgens de syntaxis van de `Binds` parameter verschilt. Bij het opstarten van de container, de werkmap is `c:\appdata`. Het configuratiebestand plaatsen in de map `d:\iiotedge`op de host, geeft u de toewijzing van het volgende in de `HostConfig` sectie:

```json
"HostConfig": {
    "Binds": [
        "d:/iiotedge:c:/appdata"
    ]
}
```

Als u Linux-containers op Linux, de syntaxis van uitvoert de `Binds` parameter verschilt het opnieuw. Bij het opstarten van de container, de werkmap is `/appdata`. Het configuratiebestand plaatsen in de map `/iiotedge` op de host, geeft u de toewijzing van het volgende in de `HostConfig` sectie:

```json
"HostConfig": {
    "Binds": [
        "/iiotedge:/appdata"
    ]
}
```

## <a name="considerations-when-using-a-container"></a>Overwegingen bij het gebruik van een container

De volgende secties worden een aantal zaken waarmee u rekening moet houden wanneer u een container:

### <a name="access-to-the-opc-publisher-opc-ua-server"></a>Toegang tot de OPC Publisher OPC UA-server

Standaard wordt de OPC Publisher OPC UA-server luistert op poort 62222. Als u wilt weergeven van deze binnenkomende poort in een container, gebruik de volgende opdracht:

```sh/cmd
docker run -p 62222:62222 mcr.microsoft.com/iotedge/opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="enable-intercontainer-name-resolution"></a>Intercontainer naamomzetting inschakelen

Een gebruiker maken om in te schakelen naamomzetting uit in de container naar andere containers, docker bridge netwerk definiëren en verbinden met dit netwerk met behulp van de container de `--network` optie. Ook toewijzen de container een naam met de `--name` optie als volgt:

```sh/cmd
docker network create -d bridge iot_edge
docker run --network iot_edge --name publisher mcr.microsoft.com/iotedge/opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

De container is nu bereikbaar is met de naam van de `publisher` door andere containers op hetzelfde netwerk.

### <a name="access-other-systems-from-within-the-container"></a>Toegang tot andere systemen uit in de container

Andere containers kunnen worden bereikt met behulp van de parameters die worden beschreven in de vorige sectie. Als het besturingssysteem waarop Docker wordt gehost is DNS ingeschakeld, werkt klikt u vervolgens toegang tot alle systemen die bekend zijn bij DNS.

In netwerken die gebruikmaken van de NetBIOS-naamomzetting, schakelt u de toegang tot andere systemen op basis van de container met de `--add-host` optie. Deze optie wordt een vermelding effectief toegevoegd aan het bestand van de container host:

```cmd/sh
docker run --add-host mydevbox:192.168.178.23  mcr.microsoft.com/iotedge/opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="assign-a-hostname"></a>Een hostnaam toewijzen

OPC Publisher maakt gebruik van de hostnaam van de machine waarop het wordt uitgevoerd voor het genereren van certificaten en het eindpunt. Docker kiest een willekeurige hostnaam als er een is ingesteld door de `-h` optie. Het volgende voorbeeld laat zien hoe het instellen van de interne hostnaam van de container `publisher`:

```sh/cmd
docker run -h publisher mcr.microsoft.com/iotedge/opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="use-bind-mounts-shared-filesystem"></a>Binding-koppelingen (gedeelde bestandssysteem) gebruiken

In plaats van het bestandssysteem van de container, kunt u besluiten de host bestandssysteem voor het opslaan van configuratiegegevens en -logboekbestanden. Gebruik voor het configureren van deze optie de `-v` optie van `docker run` in de modus voor het koppelen van binding.

## <a name="opc-ua-x509-certificates"></a>OPC UA X.509-certificaten

OPC UA maakt gebruik van X.509-certificaten voor het verifiëren van de OPC UA-client en server wanneer ze een verbinding tot stand brengen en voor het versleutelen van de communicatie ertussen. OPC Publisher maakt gebruik van certificaatarchieven onderhouden door de OPC UA-stack om alle certificaten te beheren. Bij het opstarten controleert OPC Publisher of er een certificaat voor zichzelf. Als er geen certificaat in het certificaatarchief en een is niet een is doorgegeven op de opdrachtregel, OPC Publisher maakt een zelfondertekend certificaat. Zie voor meer informatie de **InitApplicationSecurityAsync** methode in `OpcApplicationConfigurationSecurity.cs`.

Zelfondertekende certificaten bieden niet elke vorm van beveiliging, als ze niet zijn ondertekend door een vertrouwde Certificeringsinstantie.

OPC Publisher biedt opdrachtregelopties voor het:

- CSR-gegevens van de huidige toepassing authenticatiecertificaat van OPC Publisher worden opgehaald.
- OPC Publisher inrichten met een CA ondertekend certificaat.
- OPC Publisher inrichten met een nieuw sleutelpaar en die overeenkomt met de CA ondertekend certificaat.
- Certificaten toevoegen aan een vertrouwde peer of het certificaatarchief van vertrouwde uitgevers.
- Voeg een CRL.
- Een certificaat verwijderen uit de vertrouwde peer of het certificaatarchief van vertrouwde verleners.

Bij deze opties kunnen u met behulp van bestanden of base64-gecodeerde tekenreeksen parameters doorgeven.

Het standaardtype voor archief voor alle certificaatarchieven is het bestandssysteem, die u kunt wijzigen met behulp van opdrachtregelopties. Omdat de container geen permanente opslag in het bestandssysteem biedt, moet u een ander opslagtype te kiezen. Gebruik van de Docker `-v` optie om vast te leggen van het certificaat wordt opgeslagen in het bestandssysteem van de host of op een Docker-volume. Als u een Docker-volume, kunt u certificaten met behulp van base64-gecodeerde tekenreeksen doorgeven.

De runtime-omgeving is van invloed op hoe de certificaten zijn opgeslagen. Vermijd het maken van nieuwe certificaatarchieven telkens wanneer die u de toepassing uitvoert:

- Systeemeigen uitgevoerd op Windows, u kunt niet gebruiken een certificaatarchief van de toepassing van het type `Directory` omdat toegang tot de persoonlijke sleutel is mislukt. In dit geval gebruikt u de optie `--at X509Store`.
- Als u de docker-container Linux uitvoert, kunt u de certificaatarchieven naar het hostbestandsysteem toewijzen met de optie uitvoeren docker `-v <hostdirectory>:/appdata`. Deze optie wordt het certificaat blijvend wordt uitgevoerd.
- Die wordt uitgevoerd als Linux docker-container en u wilt een X509 gebruiken voor het toepassingscertificaat worden opgeslagen, gebruikt u de optie uitvoeren docker `-v x509certstores:/root/.dotnet/corefx/cryptography/x509stores` en de optie voor App `--at X509Store`

## <a name="performance-and-memory-considerations"></a>Aandachtspunten voor prestaties en geheugen

Deze sectie wordt beschreven opties voor het beheren van geheugen en -prestaties:

### <a name="command-line-parameters-to-control-performance-and-memory"></a>Opdrachtregelparameters besturingselement prestaties en geheugen

Wanneer u OPC Publisher uitvoert, moet u rekening houden met uw prestatievereisten en beschikbaar is op uw host de geheugenbronnen.

Geheugen en prestaties van elkaar afhankelijk zijn, en beide zijn afhankelijk van de configuratie van het aantal knooppunten die u configureert voor het publiceren van. Zorg ervoor dat de volgende parameters aan uw vereisten voldoen:

- Interval voor IoT Hub verzendt: `--si`
- Grootte van de IoT Hub-berichten (standaard `1`): `--ms`
- Bewaakte items in de wachtrij capaciteit: `--mq`

De `--mq` parameter bepaalt de bovengrens van de capaciteit van de interne wachtrij, die alle OPC waarde knooppunt wijzigingsmeldingen buffers. Als het verzenden van OPC Publisher kan geen snel berichten naar IoT Hub genoeg, deze wachtrij buffers de meldingen. De parameter Hiermee stelt u het aantal meldingen dat kan worden gebufferd. Als u het aantal items in deze wachtrij verhogen in uw testuitvoeringen ziet, klikt u vervolgens om te voorkomen dat verlies van berichten dient u eerst:

- Beperken van het interval voor het verzenden van IoT Hub
- Verhoog de grootte van de IoT Hub-berichten

De `--si` parameter zorgt ervoor dat OPC Publisher het verzenden van berichten naar IoT Hub met het opgegeven interval. OPC Publisher stuurt een bericht zodra de grootte van het bericht opgegeven door de `--ms` parameter is bereikt of zodra het interval dat is opgegeven door de `--si` parameter is bereikt. Schakel de optie bericht grootte gebruiken `--ms 0`. In dit geval OPC Publisher maakt gebruik van de grootste mogelijke IoT-Hub berichtgrootte van 256 kB aan batchgegevens.

De `--ms` parameter kunt u batch-berichten naar IoT Hub. Het protocol dat u bepaalt of de overhead van een bericht verzenden naar IoT Hub hoge vergeleken met de werkelijke tijd van het verzenden van de nettolading. Als uw scenario mogelijk voor latentie bij gegevens die zijn opgenomen door IoT Hub, configureert u OPC Publisher voor het gebruik van de grootste berichtgrootte van 256 kB.

Voordat u OPC Publisher in productiescenario's gebruikt, test u de prestaties en het geheugengebruik in productieomstandigheden. U kunt de `--di` parameter opgeven voor het interval in seconden, OPC Publisher diagnostische informatie schrijft.

### <a name="test-measurements"></a>Metingen testen

Het volgende voorbeeld van de diagnostische gegevens weergeven metingen met verschillende waarden voor `--si` en `--ms` parameters voor het publiceren van 500 knooppunten met een OPC-publicatie-interval van 1 seconde.  De test gebruikt een OPC Publisher foutopsporingsbuild werkt op Windows 10 systeemeigen voor 120 seconden. De IoT Hub-protocol is het standaard MQTT-protocol.

#### <a name="default-configuration---si-10---ms-262144"></a>Standaardconfiguratie (--si 10--ms 262144)

```log
==========================================================================
OpcPublisher status @ 26.10.2017 15:33:05 (started @ 26.10.2017 15:31:09)
---------------------------------
OPC sessions: 1
connected OPC sessions: 1
connected OPC subscriptions: 5
OPC monitored items: 500
---------------------------------
monitored items queue bounded capacity: 8192
monitored items queue current items: 0
monitored item notifications enqueued: 54363
monitored item notifications enqueue failure: 0
monitored item notifications dequeued: 54363
---------------------------------
messages sent to IoT Hub: 109
last successful msg sent @: 26.10.2017 15:33:04
bytes sent to IoT Hub: 12709429
avg msg size: 116600
msg send failures: 0
messages too large to sent to IoT Hub: 0
times we missed send interval: 0
---------------------------------
current working set in MB: 90
--si setting: 10
--ms setting: 262144
--ih setting: Mqtt
==========================================================================
```

De standaardconfiguratie worden gegevens verzonden naar IoT Hub elke 10 seconden, of wanneer 256 kB aan gegevens is beschikbaar voor IoT Hub om op te nemen. Deze configuratie voegt een gemiddelde latentie van ongeveer 10 seconden, maar laagste kans op verlies van gegevens is vanwege de grote berichtgrootte. De diagnostische uitvoer ziet er zijn geen verloren updates van de OPC-knooppunt: `monitored item notifications enqueue failure: 0`.

#### <a name="constant-send-interval---si-1---ms-0"></a>Interval voor constante verzenden (--si 1--ms 0)

```log
==========================================================================
OpcPublisher status @ 26.10.2017 15:35:59 (started @ 26.10.2017 15:34:03)
---------------------------------
OPC sessions: 1
connected OPC sessions: 1
connected OPC subscriptions: 5
OPC monitored items: 500
---------------------------------
monitored items queue bounded capacity: 8192
monitored items queue current items: 0
monitored item notifications enqueued: 54243
monitored item notifications enqueue failure: 0
monitored item notifications dequeued: 54243
---------------------------------
messages sent to IoT Hub: 109
last successful msg sent @: 26.10.2017 15:35:59
bytes sent to IoT Hub: 12683836
avg msg size: 116365
msg send failures: 0
messages too large to sent to IoT Hub: 0
times we missed send interval: 0
---------------------------------
current working set in MB: 90
--si setting: 1
--ms setting: 0
--ih setting: Mqtt
==========================================================================
```

Wanneer de grootte van het bericht is ingesteld op 0 vervolgens OPC Publisher intern batches gegevens met behulp van de grootste ondersteunde IoT-Hub berichtgrootte, 256 kB. De diagnostische uitvoer toont dat de gemiddelde berichtgrootte is 115,019 bytes. In deze configuratie OPC Publisher niet verliest een willekeurig knooppunt OPC waarde updates en vergeleken met de standaardwaarde is lagere latentie.

### <a name="send-each-opc-node-value-update---si-0---ms-0"></a>Elke update OPC knooppunt waarde verzenden (--si 0--ms 0)

```log
==========================================================================
OpcPublisher status @ 26.10.2017 15:39:33 (started @ 26.10.2017 15:37:37)
---------------------------------
OPC sessions: 1
connected OPC sessions: 1
connected OPC subscriptions: 5
OPC monitored items: 500
---------------------------------
monitored items queue bounded capacity: 8192
monitored items queue current items: 8184
monitored item notifications enqueued: 54232
monitored item notifications enqueue failure: 44624
monitored item notifications dequeued: 1424
---------------------------------
messages sent to IoT Hub: 1423
last successful msg sent @: 26.10.2017 15:39:33
bytes sent to IoT Hub: 333046
avg msg size: 234
msg send failures: 0
messages too large to sent to IoT Hub: 0
times we missed send interval: 0
---------------------------------
current working set in MB: 96
--si setting: 0
--ms setting: 0
--ih setting: Mqtt
==========================================================================
```

Deze configuratie wordt verzonden voor elke OPC-eindpuntwaarde het wijzigen van een bericht naar IoT Hub. Het weergeven van diagnostische gegevens van de gemiddelde berichtgrootte is 234 bytes klein. Het voordeel van deze configuratie is dat OPC Publisher eventuele latentie niet toevoegen. Het aantal verloren waarde updates van de OPC-knooppunt (`monitored item notifications enqueue failure: 44624`) is hoog, waardoor deze configuratie niet geschikt voor scenario's met grote hoeveelheden telemetrie worden gepubliceerd.

### <a name="maximum-batching---si-0---ms-262144"></a>Maximale batchverwerking (--0--ms 262144 si)

```log
==========================================================================
OpcPublisher status @ 26.10.2017 15:42:55 (started @ 26.10.2017 15:41:00)
---------------------------------
OPC sessions: 1
connected OPC sessions: 1
connected OPC subscriptions: 5
OPC monitored items: 500
---------------------------------
monitored items queue bounded capacity: 8192
monitored items queue current items: 0
monitored item notifications enqueued: 54137
monitored item notifications enqueue failure: 0
monitored item notifications dequeued: 54137
---------------------------------
messages sent to IoT Hub: 48
last successful msg sent @: 26.10.2017 15:42:55
bytes sent to IoT Hub: 12565544
avg msg size: 261782
msg send failures: 0
messages too large to sent to IoT Hub: 0
times we missed send interval: 0
---------------------------------
current working set in MB: 90
--si setting: 0
--ms setting: 262144
--ih setting: Mqtt
==========================================================================
```

Deze configuratie batches zoveel OPC knooppunt waarde updates mogelijk. De maximale grootte van de IoT Hub-bericht is 256 kB, dat hier is geconfigureerd. Er is geen interval verzenden aangevraagd, wat betekent dat de hoeveelheid gegevens voor IoT Hub om op te nemen bepaalt de latentie. Deze configuratie is de minimale waarschijnlijkheid van het verlies van alle waarden van de OPC-knooppunt en is geschikt voor het publiceren van een groot aantal knooppunten. Wanneer u deze configuratie gebruikt, zorg ervoor dat uw scenario geen voorwaarden waar hoge latentie is ingevoerd als de berichtgrootte van 256 kB niet is bereikt.

## <a name="debug-the-application"></a>Fouten in de toepassing opsporen

Om op te sporen van de toepassing, open de **opcpublisher.sln** oplossing bestand met Visual Studio en gebruik van de Visual Studio hulpmiddelen voor foutopsporing.

Als u nodig hebt voor toegang tot de OPC UA-server in de OPC-uitgever, zorg ervoor dat uw firewall toegang tot de geeft-de-server luistert op poort. De standaardpoort is: 62222.

## <a name="control-the-application-remotely"></a>De toepassing op afstand beheren

Configureren van de knooppunten om te publiceren, kan worden gedaan met behulp van IoT-Hub directe methoden.

OPC Publisher implementeert een paar extra IoT Hub rechtstreekse methodeaanroepen om te lezen:

- Algemene informatie.
- Diagnostische informatie over de OPC-sessies, abonnementen en bewaakte items.
- Diagnostische informatie over IoT Hub-berichten en gebeurtenissen.
- Het logboek voor opstarten.
- De laatste 100 regels van het logboek.
- Sluit de toepassing.

De volgende GitHub-opslagplaatsen bevatten de hulpprogramma's voor het [configureren van de knooppunten om te publiceren](https://github.com/Azure-Samples/iot-edge-opc-publisher-nodeconfiguration) en [lezen van de diagnostische gegevens](https://github.com/Azure-Samples/iot-edge-opc-publisher-diagnostics). Beide hulpprogramma's zijn ook beschikbaar als containers in Docker Hub.

## <a name="use-a-sample-opc-ua-server"></a>Een voorbeeld van een OPC UA-server gebruiken

Als u geen een echte OPC UA-server, kunt u de [voorbeeld OPC UA PLC](https://github.com/Azure-Samples/iot-edge-opc-plc) aan de slag. In dit voorbeeld PLC is ook beschikbaar op Docker Hub.

Deze implementeert een aantal tags, met willekeurige gegevens en tags met afwijkingen te genereren. U kunt het voorbeeld kunt uitbreiden, als u wilt meer tagwaarden simuleren.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd om uit te voeren van OPC Publisher, de aanbevolen volgende stappen zijn voor meer informatie over [OPC dubbele](overview-opc-twin.md) en [OPC kluis](overview-opc-vault.md).

---
title: Gebruikt de uitgever van Azure IoT Suite verbonden factory OPC | Microsoft Docs
description: Het maken en implementeren van de verbonden factory OPC publisher verwijzing implementatie.
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2017
ms.author: dobett
ms.openlocfilehash: 934f4deb3139c136c871ab0125ba45267e1d1b05
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="opc-publisher-for-azure-iot-edge"></a>OPC-Publisher voor Azure IoT-rand

Dit artikel wordt beschreven hoe u de implementatie van de verwijzing OPC-Publisher. De implementatie van de verwijzing wordt getoond hoe Azure IoT rand om te gebruiken:

- Verbinding maken met bestaande OPC UA-servers.
- JSON gecodeerde telemetrische gegevens van deze servers publiceren in OPC UA *Pub subitems* met gebruikmaking van een JSON-nettolading met Azure IoT Hub. U kunt een van de transportprotocollen die ondersteuning biedt voor Azure IoT Edge gebruiken.

Deze toepassing verwijzing bevat:

- Een OPC UA *client* voor de verbinding met bestaande OPC UA-servers in uw netwerk.
- Een OPC UA *server* luisteren op poort 62222 die u gebruiken kunt voor het beheren van wat er wordt gepubliceerd.

De toepassing wordt geïmplementeerd met .NET Core en kan worden uitgevoerd op de platformen die worden ondersteund door .NET Core.

De uitgever implementeert Pogingslogica vaststelt verbindingen met eindpunten. De uitgever verwacht eindpunten binnen een opgegeven aantal keep-alive aanvragen reageert. Deze Pogingslogica kunt de uitgever voor het detecteren van voorwaarden zoals een stroomstoring voor een OPC UA-server.

Voor elke afzonderlijke publicatie-interval voor een OPC UA-server maakt het een apart abonnement gedurende welke alle knooppunten met deze publicatie-interval worden bijgewerkt.

Om netwerkbelasting te verminderen, de uitgever ondersteunt batchverwerking van gegevens die naar IoT Hub worden verzonden. Een batch wordt verzonden naar IoT Hub, alleen wanneer de geconfigureerde pakketgrootte is bereikt.

Deze toepassing gebruikt het OPC-basisprincipes OPC UA verwijzing stack en daarom licentiebeperkingen toepassen. Ga naar http://opcfoundation.github.io/UA-.NETStandardLibrary/ voor OPC UA documentatie en de licentievoorwaarden.

U vindt de broncode OPC Publisher in de [OPC-Publisher voor Azure IoT rand](https://github.com/Azure/iot-edge-opc-publisher) GitHub-opslagplaats.

## <a name="prerequisites"></a>Vereisten

Als u wilt maken van de toepassing, moet u de [.NET Core SDK 1.1.](https://docs.microsoft.com/dotnet/core/sdk) voor uw besturingssysteem.

## <a name="build-the-application"></a>De toepassing bouwen

### <a name="as-native-windows-application"></a>Als de systeemeigen Windows-toepassing

Open de `OpcPublisher.sln` met Visual Studio 2017 project en bouw de oplossing te F7 raken.

### <a name="as-docker-container"></a>Als de Docker-container

Als u wilt maken van de toepassing als een Windows-Docker-container, gebruiken de `Dockerfile.Windows` configuratiebestand.

Als u wilt maken van de toepassing als een Linux-Docker-container, gebruiken de `Dockerfile` configuratiebestand.

Typ de volgende opdracht in een consolevenster vanuit de hoofdmap van de opslagplaats op uw ontwikkelcomputer:

`docker build -f <docker-configfile-to-use> -t <your-container-name> .`

De `-f` optie voor `docker build` is optioneel en de standaardwaarde is met de `Dockerfile` configuratiebestand.

Docker kunt u rechtstreeks vanuit een git-opslagplaats bouwen. U kunt een Linux-Docker-container met de volgende opdracht op te bouwen:

`docker build -t <your-container-name> .https://github.com/Azure/iot-edge-opc-publisher`

## <a name="configure-the-opc-ua-nodes-to-publish"></a>Configureer de knooppunten OPC UA publiceren

Om te configureren welke OPC UA-knooppunten moeten hebben hun waarden gepubliceerd naar Azure IoT Hub, door een opgemaakte JSON-configuratiebestand te maken. De standaardnaam voor dit configuratiebestand is `publishednodes.json`. De toepassing bijgewerkt en slaat dit configuratiebestand bij gebruik van het OPC UA servermethoden **PublishNode** of **UnpublishNode**.

De syntaxis van het configuratiebestand is als volgt:

```json
[
    {
        // example for an EnpointUrl is: opc.tcp://win10iot:51210/UA/SampleServer
        "EndpointUrl": "opc.tcp://<your_opcua_server>:<your_opcua_server_port>/<your_opcua_server_path>",
        "OpcNodes": [
            // Publisher will request the server at EndpointUrl to sample the node with the OPC sampling interval specified on command line (or the default value: OPC publishing interval)
            // and the subscription will publish the node value with the OPC publishing interval specified on command line (or the default value: server revised publishing interval).
            {
                // The identifier specifies the NamespaceUri and the node identifier in XML notation as specified in Part 6 of the OPC UA specification in the XML Mapping section.
                "ExpandedNodeId": "nsu=http://opcfoundation.org/UA/;i=2258"
            },
            // Publisher will request the server at EndpointUrl to sample the node with the OPC sampling interval specified on command line (or the default value: OPC publishing interval)
            // and the subscription will publish the node value with an OPC publishing interval of 4 seconds.
            // Publisher will use for each dinstinct publishing interval (of nodes on the same EndpointUrl) a separate subscription. All nodes without a publishing interval setting,
            // will be on the same subscription and the OPC UA stack will publish with the lowest sampling interval of a node.
            {
                "ExpandedNodeId": "nsu=http://opcfoundation.org/UA/;i=2258",
                "OpcPublishingInterval": 4000
            },
            // Publisher will request the server at EndpointUrl to sample the node with the given sampling interval of 1 second
            // and the subscription will publish the node value with the OPC publishing interval specified on command line (or the default value: server revised interval).
            // If the OPC publishing interval is set to a lower value, Publisher will adjust the OPC publishing interval of the subscription to the OPC sampling interval value.
            {
                "ExpandedNodeId": "nsu=http://opcfoundation.org/UA/;i=2258",
                // the OPC sampling interval to use for this node.
                "OpcSamplingInterval": 1000
            }
        ]
    },

    // the format below is only supported for backward compatibility. you need to ensure that the
    // OPC UA server on the configured EndpointUrl has the namespaceindex you expect with your configuration.
    // please use the ExpandedNodeId syntax instead.
    {
        "EndpointUrl": "opc.tcp://<your_opcua_server>:<your_opcua_server_port>/<your_opcua_server_path>",
        "NodeId": {
            "Identifier": "ns=0;i=2258"
        }
    }
    // please consult the OPC UA specification for details on how OPC monitored node sampling interval and OPC subscription publishing interval settings are handled by the OPC UA stack.
    // the publishing interval of the data to Azure IoT Hub is controlled by the command line settings (or the default: publish data to IoT Hub at least each 1 second).
]
```

## <a name="run-the-application"></a>De toepassing uitvoeren

### <a name="command-line-options"></a>Opdrachtregelopties

Als de volledige informatie over het gebruik van de toepassing wilt weergeven, gebruikt de `--help` opdrachtregeloptie te gebruiken. Het volgende voorbeeld ziet u de structuur van een opdracht:

```cmd/sh
OpcPublisher.exe <applicationname> [<IoT Hubconnectionstring>] [<options>]
```

`applicationname`is de naam van de toepassing OPC UA te gebruiken. Deze parameter is vereist. Naam van de toepassing wordt ook gebruikt voor het registreren van de uitgever in het register van IoT Hub apparaat.

`IoT Hubconnectionstring`de verbindingsreeks van de IoT Hub-eigenaar is. Deze parameter is optioneel.

De volgende opties worden ondersteund:

```cmd/sh
--pf, --publishfile=VALUE
  the filename to configure the nodes to publish.
    Default: './publishednodes.json'
--sd, --shopfloordomain=VALUE
  the domain of the shopfloor. if specified this
    domain is appended (delimited by a ':' to the '
    ApplicationURI' property when telemetry is
    sent to IoT Hub.
    The value must follow the syntactical rules of a
    DNS hostname.
    Default: not set
--sw, --sessionconnectwait=VALUE
  specify the wait time in seconds publisher is
    trying to connect to disconnected endpoints and
    starts monitoring unmonitored items
    Min: 10
    Default: 10
--vc, --verboseconsole=VALUE
  the output of publisher is shown on the console.
    Default: False
--ih, --IoT Hubprotocol=VALUE
  the protocol to use for communication with Azure
    IoT Hub (allowed values: Amqp, Http1, Amqp_
    WebSocket_Only, Amqp_Tcp_Only, Mqtt, Mqtt_
    WebSocket_Only, Mqtt_Tcp_Only).
    Default: Mqtt
--ms, --IoT Hubmessagesize=VALUE
  the max size of a message which can be send to
    IoT Hub. when telemetry of this size is available
    it will be sent.
    0 will enforce immediate send when telemetry is
    available
    Min: 0
    Max: 256 * 1024
    Default: 4096
--si, --IoT Hubsendinterval=VALUE
  the interval in seconds when telemetry should be
    send to IoT Hub. If 0, then only the
    IoT Hubmessagesize parameter controls when
    telemetry is sent.
    Default: '1'
--lf, --logfile=VALUE
  the filename of the logfile to use.
    Default: './Logs/<applicationname>.log.txt'
--pn, --portnum=VALUE
  the server port of the publisher OPC server
    endpoint.
    Default: 62222
--pa, --path=VALUE
  the endpoint URL path part of the publisher OPC
    server endpoint.
    Default: '/UA/Publisher'
--lr, --ldsreginterval=VALUE
  the LDS(-ME) registration interval in ms. If 0,
    then the registration is disabled.
    Default: 0
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
--st, --opcstacktracemask=VALUE
  the trace mask for the OPC stack. See github OPC .
    NET stack for definitions.
    To enable IoT Hub telemetry tracing set it to 711.
    Default: 285  (645)
--as, --autotrustservercerts=VALUE
  the publisher trusts all servers it is
    establishing a connection to.
    Default: False
--tm, --trustmyself=VALUE
  the publisher certificate is put into the trusted
    certificate store automatically.
    Default: True
--fd, --fetchdisplayname=VALUE
  enable to read the display name of a published
    node from the server. this will increase the
    runtime.
    Default: False
--at, --appcertstoretype=VALUE
  the own application cert store type.
    (allowed values: Directory, X509Store)
    Default: 'X509Store'
--ap, --appcertstorepath=VALUE
  the path where the own application cert should be
    stored
    Default (depends on store type):
    X509Store: 'CurrentUser\UA_MachineDefault'
    Directory: 'CertificateStores/own'
--tt, --trustedcertstoretype=VALUE
  the trusted cert store type.
    (allowed values: Directory, X509Store)
    Default: Directory
--tp, --trustedcertstorepath=VALUE
  the path of the trusted cert store
    Default (depends on store type):
    X509Store: 'CurrentUser\UA_MachineDefault'
    Directory: 'CertificateStores/trusted'
--rt, --rejectedcertstoretype=VALUE
  the rejected cert store type.
    (allowed values: Directory, X509Store)
    Default: Directory
--rp, --rejectedcertstorepath=VALUE
  the path of the rejected cert store
    Default (depends on store type):
    X509Store: 'CurrentUser\UA_MachineDefault'
    Directory: 'CertificateStores/rejected'
--it, --issuercertstoretype=VALUE
  the trusted issuer cert store type.
    (allowed values: Directory, X509Store)
    Default: Directory
--ip, --issuercertstorepath=VALUE
  the path of the trusted issuer cert store
    Default (depends on store type):
    X509Store: 'CurrentUser\UA_MachineDefault'
    Directory: 'CertificateStores/issuers'
--dt, --devicecertstoretype=VALUE
  the IoT Hub device cert store type.
    (allowed values: Directory, X509Store)
    Default: X509Store
--dp, --devicecertstorepath=VALUE
  the path of the iot device cert store
    Default Default (depends on store type):
    X509Store: 'My'
    Directory: 'CertificateStores/IoT Hub'
-h, --help
  show this message and exit
```

U kunt de volgende omgevingsvariabelen gebruiken om te bepalen van de toepassing:
- `_HUB_CS`: Hiermee stelt u de verbindingsreeks van de IoT Hub-eigenaar
- `_GW_LOGP`: Hiermee stelt u de bestandsnaam van het logboekbestand te gebruiken
- `_TPC_SP`: Hiermee stelt u het pad voor het opslaan van certificaten van vertrouwde stations
- `_GW_PNFP`: Hiermee stelt u de bestandsnaam van het configuratiebestand publiceren

Opdrachtregelargumenten vervangt omgevingsvariabele-instellingen.

U doorgaans opgeven de verbindingsreeks voor de eigenaar van IoT Hub alleen op de eerste begin van de toepassing. De verbindingsreeks is versleuteld en opgeslagen in het certificaatarchief van het platform.

De verbindingsreeks is op de volgende aanroepen lezen uit het certificaatarchief van het platform en opnieuw gebruikt. Als u de verbindingsreeks op elke begin opgeeft, wordt het apparaat in het register van IoT Hub apparaat verwijderd en opnieuw gemaakt telkens.

### <a name="native-on-windows"></a>Systeemeigen in Windows

Open de toepassing standaard uitgevoerd op Windows, de `OpcPublisher.sln` project met Visual Studio 2017, de oplossing bouwen en deze publiceren. U kunt de toepassing starten in de **doelmap** gepubliceerd naar met:

```cmd
dotnet OpcPublisher.dll <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="use-a-self-built-container"></a>Een zelf ingebouwde container gebruiken

Om de toepassing uitvoert in een container zelf ingebouwde, bouwen en vervolgens uw eigen container op te starten:

```cmd/sh
docker run <your-container-name> <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="use-a-container-from-hubdockercom"></a>Een container van hub.docker.com gebruiken

Er is een vooraf gedefinieerde container beschikbaar op DockerHub. Wilt starten, moet u de volgende opdracht uitvoeren:

```cmd/sh
docker run microsoft/iot-edge-opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="important-when-using-a-container"></a>Belangrijk wanneer u een container

#### <a name="access-to-the-publisher-opc-ua-server"></a>Toegang tot de uitgever OPC UA-server

De Publisher OPC UA-server standaard luistert op poort 62222. Als u wilt weergeven van deze binnenkomende poort in een container, moet u de `docker run` optie `-p`:

```cmd/sh
docker run -p 62222:62222 microsoft/iot-edge-opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

#### <a name="enable-intercontainer-name-resolution"></a>Intercontainer naamomzetting inschakelen

Om naamomzetting uit in de container naar andere containers, moet u het volgende doen:

- Een gebruiker gedefinieerde docker Brugnetwerk maken.
- Verbinding maken met de container met het netwerk via de `--network`optie.
- Toewijzen van de container een naam met de `--name` optie.

Het volgende voorbeeld ziet u deze configuratieopties:

```cmd/sh
docker network create -d bridge iot_edge
docker run --network iot_edge --name publisher microsoft/iot-edge-opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

De container kan nu worden bereikt door andere containers via het netwerk de naam van de `publisher`.

#### <a name="assign-a-hostname"></a>Toewijzen van een hostnaam

De hostnaam van de machine die wordt uitgevoerd op voor het genereren van het certificaat en een eindpunt wordt gebruikt. Docker kiest een willekeurige hostnaam, tenzij u een met de `-h` optie. Hier een voorbeeld om in te stellen van de interne hostnaam van de container `publisher`:

```cmd/sh
docker run -h publisher microsoft/iot-edge-opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

#### <a name="using-bind-mounts-shared-filesystem"></a>Met behulp van bind koppelt (gedeelde bestandssysteem)

In sommige scenario's die u wilt lezen van configuratie-informatie uit of schrijven van logboekbestanden naar de locaties op de host in plaats van het bestandssysteem van de container. Gebruiken voor het configureren van dit gedrag, de `-v` optie `docker run` in de modus van bind koppelen. Bijvoorbeeld:

```cmd/sh
-v //D/docker:/build/out/Logs
-v //D/docker:/build/out/CertificateStores
-v //D/docker:/shared
-v //D/docker:/root/.dotnet/corefx/cryptography/x509stores
```

#### <a name="store-for-x509-certificates"></a>Store voor X509 certificaten

Opslaan van X509 certificaten werkt niet met bind koppelingen, omdat de machtigingen van het pad naar de store hoeft te worden `rw` voor de eigenaar. In plaats daarvan moet u de `-v` optie `docker run` in de volume-modus.

## <a name="debug-the-application"></a>Fouten opsporen in de toepassing

### <a name="native-on-windows"></a>Systeemeigen in Windows

Open de `OpcPublisher.sln` project met Visual Studio 2017 en foutopsporing voor de app te raken F5 te starten.

### <a name="in-a-docker-container"></a>In een docker-container

Visual Studio 2017 ondersteunt foutopsporing toepassingen in een Docker-container via `docker-compose`. Echter is deze methode niet mogelijk om door te geven van opdrachtregelparameters.

Een optie die ondersteuning biedt voor VS2017 foutopsporing alternatief is om op te sporen via `ssh`. U kunt het configuratiebestand van de docker-build `Dockerfile.ssh` in de hoofdmap van de opslagplaats voor het maken van een container SSH ingeschakeld:

```cmd/sh
docker build -f .\Dockerfile.ssh -t publisherssh .
```

U kunt nu de container voor foutopsporing op de publisher starten:

```cmd/sh
docker run -it publisherssh
```

In de container, begint u de **ssh** daemon handmatig:

```cmd/sh
service ssh start
```

Op dit moment kunt u een **ssh** -sessie als gebruiker `root` met wachtwoord `Passw0rd`.

Als u wilt voorbereiden voor foutopsporing van de toepassing in de container, moet u de volgende aanvullende stappen uitvoeren:

1. Maak op de host-zijde, een `launch.json` bestand:

    ```json
    {
      "version": "0.2.0",
      "adapter": "<path>\\plink.exe",
      "adapterArgs": "root@localhost -pw Passw0rd -batch -T ~/vsdbg/vsdbg --interpreter=vscode",
      "languageMappings": {
        "C#": {
          "languageId": "3F5162F8-07C6-11D3-9053-00C04FA302A1",
          "extensions": [ "*" ]
        }
      },
      "exceptionCategoryMappings": {
        "CLR": "449EC4CC-30D2-4032-9256-EE18EB41B62B",
        "MDA": "6ECE07A9-0EDE-45C4-8296-818D8FC401D4"
      },
      "configurations": [
        {
          "name": ".NET Core Launch",
          "type": "coreclr",
          "cwd": "~/publisher",
          "program": "Opc.Ua.Publisher.dll",
          "args": "<put-the-publisher-command-line-options-here>",

          "request": "launch"
        }
      ]
    }
    ```

1. Het project bouwen en deze publiceren naar een map van uw keuze.

1. Gebruik een hulpprogramma zoals `WinSCP` de gepubliceerde bestanden kopiëren naar de map `/root/publisher` in de container. Als u kiest voor een andere map, werkt de `cdw` eigenschap in de `launch.json` bestand.

U kunt nu starten met de volgende opdracht in Visual Studio-opdrachtvenster foutopsporing:

```cmd
DebugAdapterHost.Launch /LaunchJson:"<path-to-the-launch.json-file-you-saved>"
```

## <a name="next-steps"></a>Volgende stappen

Een voorstel voor de volgende stap is te leren hoe u [Implementeer een gateway op Windows of Linux verbonden factory vooraf geconfigureerde oplossing voor](iot-suite-connected-factory-gateway-deployment.md).
---
title: Implementeer de gateway van uw verbonden Factory - Azure | Microsoft Docs
description: Klik hier voor meer informatie over het implementeren van een gateway op Windows of Linux voor verbindingen met de oplossingsverbetering voor verbonden Factory.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: dobett
ms.openlocfilehash: 0a2ccdc6b02e9d348517bee95306ffc88d9ccd98
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53599941"
---
# <a name="deploy-an-edge-gateway-for-the-connected-factory-solution-accelerator-on-windows-or-linux"></a>Een edge-gateway voor de verbonden Factory solution accelerator op Windows of Linux implementeren

U moet twee softwareonderdelen voor het implementeren van een edge-gateway voor de *verbonden Factory* oplossingsverbetering:

- De *OPC-Proxy* maakt verbinding met de verbonden Factory. De OPC-Proxy wordt vervolgens gewacht voor opdrachten en bedieningsberichten vanuit de geïntegreerde OPC-Browser die wordt uitgevoerd in de portal van de oplossing Connected Factory.

- De *OPC Publisher* maakt verbinding met bestaande on-premises OPC UA-servers en telemetrieberichten van ze doorstuurt naar verbonden Factory. U kunt verbinding maken met een OPC-klassieke apparaten met de [OPC-klassieke adapter voor OPC UA](https://github.com/OPCFoundation/UA-.NETStandard/blob/master/ComIOP/README.md).

Beide componenten zijn open-source en zijn beschikbaar als bron op GitHub en Docker-containers op DockerHub:

| GitHub | DockerHub |
| ------ | --------- |
| [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) | [OPC Publisher](https://hub.docker.com/r/microsoft/iot-edge-opc-publisher/)   |
| [OPC-Proxy](https://github.com/Azure/iot-edge-opc-proxy)         | [OPC-Proxy](https://hub.docker.com/r/microsoft/iot-edge-opc-proxy/) |

U hoeft geen een openbare IP-adres of binnenkomende poorten openen in de firewall van de gateway voor een onderdeel. Onderdelen van de OPC-Proxy en OPC Publisher gebruik alleen uitgaande poort 443.

De stappen in dit artikel ziet u hoe u een edge-gateway met behulp van Docker op Windows of Linux implementeren. De gateway zorgt voor de verbinding met de oplossingsverbetering voor verbonden Factory. U kunt ook de onderdelen zonder verbonden Factory gebruiken.

> [!NOTE]
> Beide onderdelen kunnen worden gebruikt als de modules in [Azure IoT Edge](https://github.com/Azure/iot-edge).

## <a name="choose-a-gateway-device"></a>Kies een gateway-apparaat

Als u nog een gateway-apparaat hebt, raadt Microsoft aan dat u een commerciële gateway bij een van hun partners kopen. Voor een lijst van de gatewayapparaten die compatibel zijn met de verbonden Factory-oplossing, gaat u naar de [Azure IoT-apparaatcatalogus](https://catalog.azureiotsuite.com/?q=opc). Volg de instructies die deel van het apparaat uitmaken voor het instellen van de gateway.

Als u een bestaande gatewayapparaat handmatig te configureren wilt, gebruikt u de volgende instructies.

## <a name="install-and-configure-docker"></a>Installeren en configureren van Docker

Installeer [Docker voor Windows](https://www.docker.com/docker-windows) op uw gateway op basis van een Windows-apparaat of gebruik een pakketbeheerder om docker te installeren op uw gatewayapparaat op basis van Linux-.

Selecteer een station op uw hostcomputer om te delen met Docker tijdens de installatie van Docker voor Windows. De volgende schermafbeelding ziet u delen van de **D** station op uw Windows-systeem. Delen van een station, kunt toegang tot het hoststation uit binnen een docker-container:

![Docker voor Windows installeren](./media/iot-accelerators-connected-factory-gateway-deployment/image1.png)

> [!NOTE]
> U kunt deze stap ook uitvoeren na de installatie van docker uit de **instellingen** dialoogvenster. Met de rechtermuisknop op de **Docker** pictogram in het Windows-systeemvak en kies **instellingen**. Als er belangrijke Windows-updates zijn geïmplementeerd op het systeem, zoals de Windows Fall Creators update, het delen ongedaan maken van de stations en deel ze opnieuw uit om te vernieuwen van de toegangsrechten.

Als u Linux gebruikt, is geen aanvullende configuratie vereist voor toegang tot het bestandssysteem.

Op Windows, maak een map op het station dat u hebt gedeeld met Docker, maak een map onder de basisbestandssysteem op Linux. In dit scenario verwijst naar deze map als `<SharedFolder>`.

Als u verwijst naar de `<SharedFolder>` in een Docker-opdracht, moet u de juiste syntaxis gebruiken voor uw besturingssysteem. Hier volgen twee voorbeelden, één voor Windows en één voor Linux:

- Als u met behulp van de map `D:\shared` op Windows als uw `<SharedFolder>`, de syntaxis van de opdracht Docker is `d:/shared`.

- Als u met behulp van de map `/shared` op Linux als uw `<SharedFolder>`, de syntaxis van de opdracht Docker is `/shared`.

Zie voor meer informatie de [volumes gebruiken](https://docs.docker.com/engine/admin/volumes/volumes/) docker-engine verwijzing.

## <a name="configure-the-opc-components"></a>Configureren van de OPC-onderdelen

Voordat u de OPC-onderdelen installeert, voert u de volgende stappen uit om voor te bereiden uw omgeving:

1. Voor het voltooien van de implementatie van de gateway, moet u de **iothubowner** verbindingsreeks van de IoT-Hub in uw verbonden Factory-implementatie. In de [Azure-portal](https://portal.azure.com/), gaat u naar uw IoT-Hub in de resourcegroep die is gemaakt tijdens de implementatie van de oplossing Connected Factory. Klik op **beleid voor gedeelde toegang** toegang krijgen tot de **iothubowner** verbindingsreeks:

    ![De IoT Hub-verbindingsreeks zoeken](./media/iot-accelerators-connected-factory-gateway-deployment/image2.png)

    Kopieer de **verbindingsreeks-primaire sleutel** waarde.

1. Als u wilt toestaan dat communicatie tussen de docker-containers, moet u een door de gebruiker gedefinieerde Brugnetwerk. Voor het maken van een Brugnetwerk voor uw containers, voer de volgende opdrachten achter de opdrachtprompt:

    ```cmd/sh
    docker network create -d bridge iot_edge
    ```

    Om te controleren of de **iot_edge** Brugnetwerk is gemaakt, voert u de volgende opdracht uit:

    ```cmd/sh
    docker network ls
    ```

    Uw **iot_edge** Brugnetwerk is opgenomen in de lijst met netwerken.

Als u wilt uitvoeren van de OPC-uitgever, voer de volgende opdracht achter de opdrachtprompt:

```cmd/sh
docker run --rm -it -v <SharedFolder>:/docker -v x509certstores:/root/.dotnet/corefx/cryptography/x509stores --network iot_edge --name publisher -h publisher -p 62222:62222 --add-host <OpcServerHostname>:<IpAddressOfOpcServerHostname> microsoft/iot-edge-opc-publisher:2.1.4 publisher "<IoTHubOwnerConnectionString>" --lf /docker/publisher.log.txt --as true --si 1 --ms 0 --tm true --vc true --di 30
```

- De [OPC Publisher GitHub](https://github.com/Azure/iot-edge-opc-publisher) en de [docker uitvoeren verwijzing](https://docs.docker.com/engine/reference/run/) vindt u meer informatie over:

  - De docker-opdrachtregelopties opgegeven vóór de containernaam van de (`microsoft/iot-edge-opc-publisher:2.1.4`).
  - De betekenis van de OPC Publisher opdrachtregelparameters opgegeven achter de containernaam (`microsoft/iot-edge-opc-publisher:2.1.4`).

- De `<IoTHubOwnerConnectionString>` is de **iothubowner** gedeelde toegang beleid connection string vanuit de Azure-portal. U hebt deze verbindingsreeks in de vorige stap hebt gekopieerd. Deze verbindingsreeks hoeft u alleen voor de eerste keer uitvoert OPC Publisher. Op het volgende wordt uitgevoerd, moet u het weglaten omdat het is een beveiligingsrisico.

- De `<SharedFolder>` u en de syntaxis wordt beschreven in de sectie [installeren en configureren van Docker](#install-and-configure-docker). OPC Publisher maakt gebruik van de `<SharedFolder>` aan:

    - Lezen en schrijven naar het configuratiebestand van OPC Publisher.
    - Schrijven naar het logboekbestand.
    - Beide deze bestanden beschikbaar zijn buiten de container te maken.

- OPC Publisher leest de configuratie van de **publishednodes.json** -bestand, dat wordt gelezen en geschreven naar de `<SharedFolder>/docker` map. Dit configuratiebestand definieert welke gegevens in de OPC UA-knooppunt van een bepaalde OPC UA-server die de OPC-uitgever moet u abonneren op. De volledige syntaxis van de **publishednodes.json** bestand wordt beschreven op de [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) pagina op GitHub. Wanneer u een gateway toevoegt, plaatst u een lege **publishednodes.json** in de map:

    ```json
    [
    ]
    ```

- Wanneer de OPC UA-server ontvangt een melding van OPC Publisher van een gegevenswijziging, worden de nieuwe waarde wordt verzonden naar IoT Hub. Afhankelijk van de batchverwerkingsindeling instellingen verzamelt de OPC-uitgever eerst de gegevens voordat deze de gegevens in een batch naar IoT Hub verzendt.

- Docker biedt geen ondersteuning voor NetBIOS-naamomzetting, alleen DNS-naamomzetting. Als u geen een DNS-server op het netwerk, kunt u de oplossing wordt weergegeven in het vorige voorbeeld van de opdrachtregel gebruiken. Maakt gebruik van het vorige voorbeeld van de opdrachtregel de `--add-host` parameter voor het toevoegen van een vermelding in het hosts-bestand van containers. Deze vermelding kunt opzoeken van de hostnaam voor de opgegeven `<OpcServerHostname>`, omzetten naar het opgegeven IP-adres `<IpAddressOfOpcServerHostname>`.

- OPC UA maakt gebruik van X.509-certificaten voor verificatie en versleuteling. Plaats het certificaat OPC Publisher op de OPC UA-server die u verbinding maakt, om te controleren of dat het OPC-uitgever vertrouwt. Het certificaatarchief van OPC Publisher bevindt zich in de `<SharedFolder>/CertificateStores` map. U vindt het OPC Publisher-certificaat in de `trusted/certs` map in de `CertificateStores` map.

  De stappen voor het configureren van de OPC UA-server, is afhankelijk van het apparaat dat u gebruikt. deze stappen worden doorgaans gedocumenteerd in de gebruikershandleiding van de OPC UA-server.

Voor het installeren van de OPC-Proxy, voer de volgende opdracht achter de opdrachtprompt:

```cmd/sh
docker run -it --rm -v <SharedFolder>:/mapped --network iot_edge --name proxy --add-host <OpcServerHostname>:<IpAddressOfOpcServerHostname> microsoft/iot-edge-opc-proxy:1.0.4 -i -c "<IoTHubOwnerConnectionString>" -D /mapped/cs.db
```

U hoeft alleen de installatie eenmaal wordt uitgevoerd op een systeem.

Gebruik de volgende opdracht om uit te voeren van de OPC-Proxy:

```cmd/sh
docker run -it --rm -v <SharedFolder>:/mapped --network iot_edge --name proxy --add-host <OpcServerHostname>:<IpAddressOfOpcServerHostname> microsoft/iot-edge-opc-proxy:1.0.4 -D /mapped/cs.db
```

OPC-Proxy wordt de verbindingsreeks opgeslagen tijdens de installatie. Op het volgende wordt uitgevoerd, moet u de verbindingsreeks weglaten omdat het is een beveiligingsrisico.

## <a name="enable-your-gateway"></a>Uw gateway inschakelen

Voer de volgende stappen voor het inschakelen van uw gateway in de oplossingsverbetering voor verbonden Factory:

1. Wanneer beide onderdelen worden uitgevoerd, bladert u naar de **verbinding maken met uw eigen OPC UA-Server** pagina in de portal van de oplossing Connected Factory. Deze pagina is alleen beschikbaar voor beheerders in de oplossing. Voer de uitgever eindpunt-URL (opc.tcp://publisher: 62222) en klikt u op **Connect**.

1. Een vertrouwensrelatie tussen de verbonden Factory-portal en OPC Publisher. Wanneer er een certificaatwaarschuwing weergegeven, klikt u op **doorgaan**. Vervolgens ziet u een foutbericht dat de OPC-uitgever de webclient UA niet vertrouwt. U kunt deze fout oplossen, Kopieer de **UA-webclient** certificaat van de `<SharedFolder>/CertificateStores/rejected/certs` map die u wilt de `<SharedFolder>/CertificateStores/trusted/certs` map op de gateway. U hoeft niet te starten van de gateway.

U kunt nu verbinding maken met de gateway vanuit de cloud, en u kunt OPC UA-servers toevoegen aan de oplossing.

## <a name="add-your-own-opc-ua-servers"></a>Uw eigen OPC UA-servers toevoegen

Uw eigen OPC UA-servers toevoegen aan de oplossingsverbetering voor verbonden Factory:

1. Blader naar de **verbinding maken met uw eigen OPC UA-server** pagina in de portal van de oplossing Connected Factory.

    1. Start de OPC UA-server die u wilt verbinden. Controleer of uw OPC UA-server kan worden bereikt vanaf OPC Publisher en de OPC-Proxy die worden uitgevoerd in de container. Zie de vorige opmerkingen over naamomzetting.
    1. Voer de eindpunt-URL van uw OPC UA-server (`opc.tcp://<host>:<port>`) en klikt u op **Connect**.
    1. Het installatieproces verbinding brengt een vertrouwensrelatie tussen de verbonden Factory-portal (OPC UA-client) en de OPC UA-server die u probeert om verbinding te maken. In het dashboard van Connected Factory die u krijgt een **certificaat van de server die u wilt verbinding kan niet worden geverifieerd** waarschuwing. Wanneer er een certificaatwaarschuwing weergegeven, klikt u op **doorgaan**.
    1. Tot het instellen van het moeilijker is de configuratie van het certificaat van de OPC UA-server waarmee u verbinding maken met. Voor pc's op basis van OPC UA-servers krijgt u mogelijk alleen een dialoogvenster met waarschuwing in het dashboard dat u kunt bevestigen. Raadpleeg de documentatie van uw OPC UA-server om te zoeken hoe deze taak wordt uitgevoerd voor ingesloten OPC UA-server-systemen. Als u wilt deze taak hebt voltooid, moet u het certificaat van de verbonden Factory-portal OPC UA-client. Een beheerder dit certificaat kunt downloaden op de **verbinding maken met uw eigen OPC UA-server** pagina:

        ![Oplossingsportal](./media/iot-accelerators-connected-factory-gateway-deployment/image4.png)

1. Bladeren in de structuur van de OPC UA-knooppunten van uw OPC UA-server, met de rechtermuisknop op de OPC-knooppunten die u wilt waarden naar verbonden Factory verzenden, en selecteer **publiceren**.

1. Telemetrie die nu stromen van het gateway-apparaat. U ziet de telemetrie in de **Factorylijst** weergave van de verbonden Factory-portal onder **nieuwe Factory**.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de architectuur van de oplossing Connected Factory accelerator [kennismaken met de verbonden Factory oplossing accelerator](iot-accelerators-connected-factory-sample-walkthrough.md).

Meer informatie over de [OPC Publisher referentie-implementatie](https://docs.microsoft.com/azure/iot-suite/iot-suite-connected-factory-publisher).
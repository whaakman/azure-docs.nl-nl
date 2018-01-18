---
title: De gateway van uw verbonden factory - Azure implementeren | Microsoft Docs
description: Het implementeren van een gateway op Windows of Linux voor verbindingen met de verbonden factory vooraf geconfigureerde oplossing.
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
ms.date: 01/17/2018
ms.author: dobett
ms.openlocfilehash: 4606cb676c3ab7c8c8511579f43d251ff7d2ae8a
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="deploy-an-edge-gateway-for-the-connected-factory-preconfigured-solution-on-windows-or-linux"></a>Een gateway aan de rand voor de verbonden factory vooraf geconfigureerde oplossing op Windows of Linux implementeren

U moet twee softwareonderdelen voor het implementeren van een edge-gateway voor de *verbonden factory* vooraf geconfigureerde oplossing:

- De *OPC Proxy* maakt een verbinding met verbonden factory. De Proxy OPC wordt er gewacht totdat de opdracht en controle berichten van de geïntegreerde OPC-Browser die wordt uitgevoerd in de oplossingsportal verbonden factory.

- De *OPC Publisher* maakt verbinding met bestaande lokale OPC UA-servers en telemetrieberichten van ze doorstuurt naar verbonden factory. U kunt verbinding maken met een OPC klassieke apparaten met de [OPC klassieke adapter voor OPC UA](https://github.com/OPCFoundation/UA-.NETStandard/blob/master/ComIOP/README.md).

Beide onderdelen zijn open source en zijn beschikbaar als bron op GitHub, en als Docker-containers op DockerHub:

| GitHub | DockerHub |
| ------ | --------- |
| [OPC-uitgever](https://github.com/Azure/iot-edge-opc-publisher) | [OPC-uitgever](https://hub.docker.com/r/microsoft/iot-edge-opc-publisher/)   |
| [OPC-Proxy](https://github.com/Azure/iot-edge-opc-proxy)         | [OPC-Proxy](https://hub.docker.com/r/microsoft/iot-edge-opc-proxy/) |

U hoeft niet een openbare IP-adres of open poorten voor inkomend verkeer in de firewall gateway voor beide onderdeel. De onderdelen OPC-Proxy en OPC Publisher gebruik alleen uitgaande poort 443.

De stappen in dit artikel ziet u het implementeren van een gateway aan de rand met behulp van Docker op Windows of Linux. De gateway maakt verbinding met de verbonden factory vooraf geconfigureerde oplossing. U kunt ook de onderdelen zonder verbonden factory gebruiken.

> [!NOTE]
> Beide onderdelen kunnen worden gebruikt als modules in [Azure IoT rand](https://github.com/Azure/iot-edge).

## <a name="choose-a-gateway-device"></a>Kies een gateway-apparaat

Als u een gateway-apparaat nog geen hebt, is het raadzaam om dat u een commerciële gateway koopt van een van de partners. Voor een lijst van de gatewayapparaten die compatibel zijn met de verbonden factory-oplossing, gaat u naar de [Azure IoT-apparaat catalogus](https://catalog.azureiotsuite.com/?q=opc). Volg de instructies die worden geleverd met het apparaat voor het instellen van de gateway.

Gebruik de volgende instructies ook handmatig configureren van een bestaande gateway-apparaat.

## <a name="install-and-configure-docker"></a>Installeren en configureren van Docker

Installeer [Docker voor Windows](https://www.docker.com/docker-windows) op uw apparaat op basis van de Windows-gateway of gebruik een Pakketbeheer docker installeren op uw apparaat op basis van Linux-gateway.

Selecteer een station op de hostcomputer te delen met Docker tijdens de installatie van Docker voor Windows. De volgende schermafbeelding ziet u delen van de **D** station op uw Windows-systeem dat toegang tot de host-station in een docker-container:

![Docker voor Windows installeren](./media/iot-suite-connected-factory-gateway-deployment/image1.png)

> [!NOTE]
> U kunt deze stap ook uitvoeren na de installatie van docker van de **instellingen** dialoogvenster. Met de rechtermuisknop op de **Docker** pictogram in het Windows-systeemvak en kies **instellingen**. Als u belangrijke Windows-updates zijn geïmplementeerd op het systeem, zoals het maken van de vallen Windows update, ongedaan maken van de stations en deze opnieuw als u wilt vernieuwen van de toegangsrechten delen.

Als u van Linux gebruikmaakt, wordt geen aanvullende configuratie vereist voor toegang tot het bestandssysteem.

Op Windows, maak een map op het station dat u hebt gedeeld met Docker, maak een map onder de hoofdmap bestandssysteem op Linux. In dit scenario verwijst naar deze map als `<SharedFolder>`.

Als u verwijst naar de `<SharedFolder>` in een Docker-opdracht, moet u de juiste syntaxis gebruiken voor uw besturingssysteem. Hier vindt u twee voorbeelden, één voor Windows en één voor Linux:

- Als u met behulp van de map `D:\shared` op Windows als uw `<SharedFolder>`, de syntaxis van de Docker-opdracht is `d:/shared`.

- Als u met behulp van de map `/shared` op Linux als uw `<SharedFolder>`, de syntaxis van de Docker-opdracht is `/shared`.

Zie voor meer informatie de [volumes gebruiken](https://docs.docker.com/engine/admin/volumes/volumes/) docker-engine-verwijzing.

## <a name="configure-the-opc-components"></a>De OPC-onderdelen configureren

Voordat u de OPC-onderdelen installeert, moet u de volgende stappen om uw omgeving voorbereiden uitvoeren:

1. Als u de gateway-implementatie, moet u de **iothubowner** verbindingsreeks van de IoT-Hub in uw implementatie verbonden factory. In de [Azure-portal](http://portal.azure.com/), gaat u naar uw IoT-Hub in de resourcegroep gemaakt tijdens de implementatie van de verbonden factory-oplossing. Klik op **gedeeld toegangsbeleid** voor toegang tot de **iothubowner** verbindingsreeks:

    ![De verbindingsreeks IoT Hub vinden](./media/iot-suite-connected-factory-gateway-deployment/image2.png)

    Kopieer de **Connection string-primaire sleutel** waarde.

1. Communicatie wilt toestaan tussen docker-containers, moet u een gebruiker gedefinieerde brug-netwerk. Voer de volgende opdrachten bij een opdrachtprompt voor het maken van een netwerk bridge voor uw containers:

    ```cmd/sh
    docker network create -d bridge iot_edge
    ```

    Om te controleren of de **iot_edge** Brugnetwerk is gemaakt, voert u de volgende opdracht:

    ```cmd/sh
    docker network ls
    ```

    Uw **iot_edge** Brugnetwerk is opgenomen in de lijst met netwerken.

Voor het uitvoeren van de uitgever OPC, voer de volgende opdracht achter de opdrachtprompt:

```cmd/sh
docker run --rm -it -v <SharedFolder>:/docker -v x509certstores:/root/.dotnet/corefx/cryptography/x509stores --network iot_edge --name publisher -h publisher -p 62222:62222 --add-host <OpcServerHostname>:<IpAddressOfOpcServerHostname> microsoft/iot-edge-opc-publisher:2.1.3 publisher "<IoTHubOwnerConnectionString>" --lf /docker/publisher.log.txt --as true --si 1 --ms 0 --tm true --vc true --di 30
```

- De [OPC Publisher GitHub](https://github.com/Azure/iot-edge-opc-publisher) en de [docker uitvoeren verwijzing](https://docs.docker.com/engine/reference/run/) vindt u meer informatie over:

  - De docker opdrachtregelopties opgegeven alvorens de containernaam (`microsoft/iot-edge-opc-publisher:2.1.3`).
  - De betekenis van de uitgever OPC opdrachtregelparameters opgegeven na de containernaam (`microsoft/iot-edge-opc-publisher:2.1.3`).

- De `<IoTHubOwnerConnectionString>` is de **iothubowner** gedeelde toegang beleid-verbindingsreeks uit de Azure-portal. U hebt deze verbindingsreeks in de vorige stap hebt gekopieerd. U hoeft alleen deze verbindingsreeks voor de eerste uitvoering van OPC-Publisher. Op latere uitvoeringen moet u het weglaten omdat dit een beveiligingsrisico inhouden.

- De `<SharedFolder>` u gebruikt en de syntaxis is beschreven in de sectie [installeren en configureren van Docker](#install-and-configure-docker). OPC Publisher gebruikt de `<SharedFolder>` schrijven naar het logboekbestand om te lezen en schrijven naar het configuratiebestand OPC-uitgever, en beide deze bestanden beschikbaar zijn buiten de container maken.

- OPC Publisher leest de configuratie van de **publishednodes.json** bestand, dat is gelezen uit en geschreven naar de `<SharedFolder>/docker` map. Dit configuratiebestand wordt gedefinieerd welke OPC UA knooppuntgegevens op een bepaalde OPC UA-server die moet u de uitgever OPC abonneren op. De volledige syntaxis van de **publishednodes.json** bestand zoals is beschreven in de [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) pagina op GitHub. Wanneer u een gateway toevoegt, plaatst u een lege **publishednodes.json** naar de map:

    ```json
    [
    ]
    ```

- Wanneer de OPC UA-server OPC-uitgever van gewijzigde gegevens meldt, worden de nieuwe waarde wordt verzonden naar IoT Hub. Afhankelijk van de batchen instellingen worden de gegevens eerst de uitgever van de OPC verzameld voordat de gegevens naar IoT Hub worden verzonden in een batch.

- Docker biedt geen ondersteuning voor NetBIOS-naamomzetting, alleen DNS-naamomzetting. Als u geen DNS-server op het netwerk hebt, kunt u de tijdelijke oplossing weergegeven in het vorige voorbeeld van de opdrachtregel. Het vorige voorbeeld van de opdrachtregel gebruikt de `--add-host` parameter een vermelding in het hosts-bestand van containers toe te voegen. Deze vermelding kan hostnaam zoekactie voor de opgegeven `<OpcServerHostname>`, het omzetten van het opgegeven IP-adres `<IpAddressOfOpcServerHostname>`.

- OPC UA maakt gebruik van X.509-certificaten voor verificatie en versleuteling. U moet het OPC-uitgeverscertificaat plaats op de OPC UA-server die u verbinding maakt, zodat het OPC uitgever vertrouwt. Het certificaatarchief OPC Publisher bevindt zich in de `<SharedFolder>/CertificateStores` map. U vindt het OPC Publisher-certificaat in de `trusted/certs` map in de `CertificateStores` map.

  De stappen voor het configureren van de OPC UA-server, is afhankelijk van het apparaat dat u gebruikt. deze stappen worden doorgaans gedocumenteerd in gebruikershandleiding voor de OPC UA-server.

Voer de volgende opdracht achter de opdrachtprompt voor de installatie van de OPC-Proxy:

```cmd/sh
docker run -it --rm -v <SharedFolder>:/mapped --network iot_edge --name proxy --add-host <OpcServerHostname>:<IpAddressOfOpcServerHostname> microsoft/iot-edge-opc-proxy:1.0.2 -i -c "<IoTHubOwnerConnectionString>" -D /mapped/cs.db
```

U hoeft de installatie eenmaal wordt uitgevoerd op een systeem.

Gebruik de volgende opdracht om uit te voeren van het OPC-Proxy:

```cmd/sh
docker run -it --rm -v <SharedFolder>:/mapped --network iot_edge --name proxy --add-host <OpcServerHostname>:<IpAddressOfOpcServerHostname> microsoft/iot-edge-opc-proxy:1.0.2 -D /mapped/cs.db
```

De verbindingsreeks OPC Proxy opgeslagen tijdens de installatie. Op latere uitvoeringen moet u de verbindingsreeks weglaten omdat dit een beveiligingsrisico inhouden.

## <a name="enable-your-gateway"></a>Uw gateway inschakelen

Voer de volgende stappen voor het inschakelen van uw gateway in de verbonden factory vooraf geconfigureerde oplossing:

1. Wanneer beide onderdelen worden uitgevoerd, bladert u naar de **verbinding maken met uw eigen OPC UA-Server** pagina in de oplossingsportal verbonden factory. Deze pagina is alleen beschikbaar voor beheerders in de oplossing. Voer de publisher eindpunt-URL (opc.tcp://publisher: 62222) en klik op **Connect**.

1. Een vertrouwensrelatie tussen de verbonden factory-portal en OPC Publisher maken. Wanneer er een certificaatwaarschuwing weergegeven, klikt u op **doorgaan**. Vervolgens ziet u dat de uitgever OPC de webclient UA niet vertrouwt. Los deze fout, kopieert u de **UA webclient** certificaat van de `<SharedFolder>/CertificateStores/rejected/certs` map voor de `<SharedFolder>/CertificateStores/trusted/certs` map op de gateway. U hoeft niet opnieuw starten van de gateway.

U kunt nu verbinding met de gateway vanuit de cloud en u bent klaar OPC UA servers toevoegen aan de oplossing.

## <a name="add-your-own-opc-ua-servers"></a>Uw eigen OPC UA-servers toevoegen

Vooraf geconfigureerde oplossing voor uw eigen OPC UA-servers toevoegen aan de verbonden factory:

1. Blader naar de **verbinding maken met uw eigen OPC UA-server** pagina in de oplossingsportal verbonden factory.

    1. Start de OPC UA-server die verbinding wilt maken. Zorg ervoor dat uw OPC UA-server kan worden bereikt vanaf het OPC-uitgever en OPC Proxy uitgevoerd in de container (Zie de vorige opmerkingen over naamomzetting).
    1. Voer de eindpunt-URL van uw OPC UA-server (`opc.tcp://<host>:<port>`) en klik op **Connect**.
    1. Als onderdeel van de installatie van de verbinding een vertrouwensrelatie tussen de verbonden factory-portal (OPC UA-client) en het OPC UA-server die u probeert verbinding tot stand is gebracht. In het dashboard verbonden factory krijgt u een **certificaat van de server die u wilt verbinden, kan niet worden geverifieerd** waarschuwing. Wanneer er een certificaatwaarschuwing weergegeven, klikt u op **doorgaan**.
    1. Moeilijker te setup is van het OPC UA-server die u probeert verbinding maken met de configuratie van het certificaat. Voor PC OPC UA-servers gebaseerde, krijgt u mogelijk slechts een waarschuwingsdialoogvenster in het dashboard die u kunt bevestigen. Raadpleeg de documentatie van uw OPC UA-server om te zoeken hoe deze taak wordt uitgevoerd voor ingesloten OPC UA-server-systemen. Als u wilt deze taak uitvoeren, moet u het certificaat van de verbonden factory-portal OPC UA-client. Een beheerder kan dit certificaat downloaden op de **verbinding maken met uw eigen OPC UA-server** pagina:

        ![Oplossingsportal](./media/iot-suite-connected-factory-gateway-deployment/image4.png)

1. Bladeren in de structuur van de knooppunten OPC UA van uw OPC UA-server, met de rechtermuisknop op de gewenste waarden verzenden naar verbonden factory en selecteer OPC-knooppunten **publiceren**.

1. Telemetrie loopt nu van het gateway-apparaat. Vindt u de telemetrie in de **Factory locaties** weergave van de portal verbonden factory onder **New Factory**.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de architectuur van de verbonden factory vooraf geconfigureerde oplossing, [verbonden factory oplossing walkthrough over vooraf geconfigureerde](https://docs.microsoft.com/azure/iot-suite/iot-suite-connected-factory-sample-walkthrough).

Meer informatie over de [OPC Publisher verwijzing implementatie](https://docs.microsoft.com/azure/iot-suite/iot-suite-connected-factory-publisher).